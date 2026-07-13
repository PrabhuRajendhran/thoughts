## Architectural & Memory Rules

* Native FP8 Supported: Your NVIDIA L4 GPUs (Ada Lovelace) natively support both E4M3 and E5M2 formats.
* E4M3 Recommended: This specific 8-bit format preserves text structure and layout accuracy for complex document extractions with less than 0.5% degradation.
* VRAM Halved: Storing your KV cache in FP8 E4M3 cuts token memory usage perfectly in half compared to native BF16.

## Image Processing & Model Math

* The 100 DPI Sweet Spot: Converting a standard page at 100 DPI outputs an image size of roughly 850×1100 pixels.
* The New 32x32 Grid: Qwen3 and Qwen3.5 architectures use a 32-pixel layout (updated from Qwen2's 28-pixel layout).
* The Token Impact: Your 100 DPI pages must be resized to 832×1088 to snap to this 32-pixel grid, resulting in exactly 884 tokens per page.
* The 22-Page Total: Your full document block equals 19,448 tokens. In FP8, this context costs ~2.08 GB of VRAM, fitting safely inside your 24 GB L4 GPU bucket. If you run native FP16/BF16, this doubles to 4.15 GB, leaving near-zero breathing room.

## Scaling & Infrastructure Choices

* Chunked Prefill is Mandatory: Enabling --enable-chunked-prefill with a chunk size of --max-num-batched-tokens 4096 lets the GPU digest 4 full pages at a time. This keeps your server responsive without lowering accuracy.
* Hardware Limitations: Your small G6 nodes (g6.xlarge/4xlarge) are completely isolated. They do not have NVLink or EFA support, so you cannot split up or share the model's processing lanes between instances.
* ALB & Dynamic Scheduling Limitations: vLLM's internal scheduling will prevent a GPU crash, but it cannot hold 100 simultaneous requests. Because your 2 instances take 12–15 minutes to clear a 100-document burst, requests waiting at the back of the line will trigger an AWS ALB 60-second gateway timeout error.
* The Production Queue Fix: To handle a burst where 100 documents arrive at the exact same time, you must decouple your upload system by using an external message queue like AWS SQS or Redis to feed your GPUs 1 or 2 files at a time.

Would you like to focus on setting up the AWS SQS buffer queue architecture or adjusting the vLLM/ALB timeout properties first?

