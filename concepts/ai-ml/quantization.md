---
type: Concept
title: 'Quantization'
description: 'Storing and computing a model''s weights (and sometimes activations) in lower-precision numbers - e.g. 8-bit or 4-bit integers instead of 16/32-bit floats - to cut memory and speed up inference at a small cost in accuracy.'
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 7 Numerical Representations p. 325 and Quantization p. 328; ch. 9 Model Optimization, p. 426'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 8 Model quantization, pp. 303-316 (GGUF and llama.cpp p. 309; GPTQ and EXL2 p. 311)'
- title: 'Tunstall, L., von Werra, L. and Wolf, T. (2022). Natural Language Processing with Transformers, revised edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098136796'
  kind: book
  locator: 'ch. 8 Making Models Faster with Quantization, pp. 230-243'
- title: 'Jacob, B. et al. (2018). Quantization and Training of Neural Networks for Efficient Integer-Arithmetic-Only Inference. CVPR 2018'
  resource: 'https://arxiv.org/abs/1712.05877'
  kind: paper
- title: 'Dettmers, T. et al. (2023). QLoRA: Efficient Finetuning of Quantized LLMs. NeurIPS 2023'
  resource: 'https://arxiv.org/abs/2305.14314'
  kind: paper
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Quantization

**Definition.** Every parameter is a number, and its format sets the memory it needs: FP32 uses 4 bytes, FP16/BF16 2 bytes, INT8 1 byte, 4-bit formats half a byte. Quantization maps the original values onto the smaller format with a scale (and sometimes zero point). Post-training quantization (PTQ) converts a trained model directly and is the common choice for LLMs (formats and tools such as GGUF/llama.cpp, GPTQ, EXL2, AWQ); quantization-aware training simulates low precision during training for better accuracy. Huyen places it among model optimisations for inference, and in fine-tuning it enables QLoRA, which trains LoRA adapters on top of a 4-bit base model.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Quantization is why a 7-8B model can run on a laptop: about 16 GB of weights at FP16 becomes about 4-5 GB at 4 bits. It trades accuracy for memory and speed, so the effect must be measured on your own evaluation set, not assumed; very low bit widths degrade reasoning and long-context quality first. It combines with the other inference levers (KV cache, batching, speculative decoding).

## Example

Running a Llama-class 8B model locally in a 4-bit GGUF file instead of the original BF16 checkpoint: roughly a quarter of the memory, faster tokens, a small drop on benchmarks.

## Broader concepts

- [Inference throughput/latency/cost](/concepts/ai-ml/inference-tradeoffs.md)

## Related concepts

- [Low-rank adaptation (LoRA)](/concepts/ai-ml/lora.md)
- [Distillation](/concepts/ai-ml/distillation.md)

## Sources

1. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 7 Numerical Representations p. 325 and Quantization p. 328; ch. 9 Model Optimization, p. 426. Number formats, memory maths and quantization for both fine-tuning and inference.
2. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 8 Model quantization, pp. 303-316 (GGUF and llama.cpp p. 309; GPTQ and EXL2 p. 311). Practical comparison of LLM quantization formats and tools.
3. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 8 Making Models Faster with Quantization, pp. 230-243. Quantizing a transformer and benchmarking speed against accuracy.
4. Jacob, B. et al. (2018). *Quantization and Training of Neural Networks for Efficient Integer-Arithmetic-Only Inference*. CVPR 2018. <https://arxiv.org/abs/1712.05877> — Integer-only quantization and quantization-aware training.
5. Dettmers, T. et al. (2023). *QLoRA: Efficient Finetuning of Quantized LLMs*. NeurIPS 2023. <https://arxiv.org/abs/2305.14314> — QLoRA: fine-tuning on a 4-bit base model.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
