Experiments: Intermittent Numeric Separator Errors (Qwen3-VL + vLLM)

Experiment| Objective| Rationale| Caveats
1. Fixed Seed| Improve reproducibility and understand whether sampling contributes to variability.| Removes one source of randomness during generation.| Doesn't improve OCR accuracy. Under continuous batching, outputs may still vary.
2. Numeric-specific Prompt| Instruct the model to preserve numeric formatting exactly as printed.| Reduces the chance of the model normalizing or altering separators.| May not overcome genuine visual ambiguity.
3. Validation + Targeted Higher-DPI Retry| Validate numeric fields against expected formats. If validation fails, rerender only the affected page/crop at higher DPI and re-extract that field.| Addresses likely OCR/perception issues while avoiding full document reprocessing.| Higher DPI invalidates KV cache, but retrying only one page/crop minimizes overhead.
4. Separate Numeric & Text Extraction (Optional)| Split extraction into numeric and textual passes, using different prompts and/or decoding strategies (e.g., stricter prompt/lower temperature for numeric fields).| Numeric fields require exact copying, while long clauses benefit from natural language generation. Different strategies can optimize each independently while reusing the same document context/KV cache.| Minimal benefit if both passes use identical prompts and decoding settings.
5. Targeted Post-processing (If Business Rules Permit)| Normalize malformed numeric values only when the expected format is unambiguous.| Lightweight safeguard for known formatting patterns.| Avoid blanket separator replacement if both "," and "." are valid in the source documents.

Notes

- Separator errors ("1,123.45" → "1.123.45") are more likely due to visual ambiguity, sampling, or prompt fidelity than long-context drift.
- Repetition loops or truncated JSON are a separate prompt-design issue, typically caused by overly complex or conflicting instructions (e.g., extract → re-extract → verify in the same prompt), and should be addressed independently.
