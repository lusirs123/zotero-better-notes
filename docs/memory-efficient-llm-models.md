# Memory-Efficient LLM Models for Zotero-GPT Integration

This guide helps you choose and configure lightweight Large Language Models (LLMs) that work well with the [Zotero-GPT](https://github.com/MuiseDestiny/zotero-gpt) integration while requiring minimal VRAM (GPU memory).

## Overview

Better Notes integrates with Zotero-GPT to provide AI-powered note-taking assistance. If you have limited GPU resources or want to run models locally, this guide recommends memory-efficient options.

## Recommended Models by VRAM Capacity

### Ultra-Low VRAM (2-4GB)

Perfect for integrated GPUs or entry-level graphics cards:

1. **TinyLlama-1.1B**
   - **VRAM Required**: ~2GB
   - **Parameters**: 1.1 billion
   - **Strengths**: Very fast inference, low memory footprint
   - **Use Cases**: Simple text completion, basic summarization
   - **Quantization**: Works well with 4-bit quantization

2. **Phi-2 (2.7B)**
   - **VRAM Required**: ~3GB
   - **Parameters**: 2.7 billion
   - **Strengths**: Strong reasoning for its size, code-capable
   - **Use Cases**: Note summarization, citation formatting, simple research assistance
   - **Quantization**: Excellent with 4-bit GPTQ or GGUF

3. **StableLM-2-1.6B**
   - **VRAM Required**: ~2.5GB
   - **Parameters**: 1.6 billion
   - **Strengths**: Good instruction following
   - **Use Cases**: Basic note editing, formatting assistance

### Low VRAM (4-8GB)

Suitable for mid-range GPUs (GTX 1060, RTX 3050, etc.):

1. **Mistral-7B-Instruct (Quantized)**
   - **VRAM Required**: ~4-6GB (4-bit quantization)
   - **Parameters**: 7 billion
   - **Strengths**: Excellent performance-to-size ratio, strong reasoning
   - **Use Cases**: Research summarization, literature review, citation analysis
   - **Quantization**: 4-bit GPTQ or GGUF Q4_K_M

2. **Llama-2-7B-Chat (Quantized)**
   - **VRAM Required**: ~5-7GB (4-bit quantization)
   - **Parameters**: 7 billion
   - **Strengths**: Well-rounded capabilities, good safety alignment
   - **Use Cases**: General note-taking, research assistance, academic writing

3. **Phi-3-Mini-4K-Instruct**
   - **VRAM Required**: ~4GB
   - **Parameters**: 3.8 billion
   - **Strengths**: Strong performance for size, 4K context
   - **Use Cases**: Note summarization, research extraction, academic tasks

### Medium VRAM (8-12GB)

For dedicated GPUs (RTX 3060, RTX 4060, etc.):

1. **Mistral-7B (FP16)**
   - **VRAM Required**: ~14GB (full precision), ~8GB (8-bit)
   - **Parameters**: 7 billion
   - **Strengths**: State-of-the-art performance for 7B size
   - **Use Cases**: Advanced research assistance, literature synthesis

2. **Llama-2-13B (Quantized)**
   - **VRAM Required**: ~8-10GB (4-bit quantization)
   - **Parameters**: 13 billion
   - **Strengths**: Strong capabilities across domains
   - **Use Cases**: Complex research tasks, detailed note generation

## Cloud-Based Alternatives (No Local VRAM Required)

If you don't have a GPU or prefer not to run models locally:

1. **OpenAI GPT-3.5-Turbo**
   - **Cost**: Pay-per-use (~$0.002/1K tokens)
   - **Strengths**: Fast, reliable, no setup required
   - **Use Cases**: All note-taking tasks

2. **OpenAI GPT-4**
   - **Cost**: Higher pay-per-use (~$0.03/1K tokens)
   - **Strengths**: Most capable, best reasoning
   - **Use Cases**: Complex research synthesis, detailed analysis

3. **Anthropic Claude**
   - **Cost**: Pay-per-use
   - **Strengths**: Long context (100K+ tokens), strong safety
   - **Use Cases**: Analyzing large documents, comprehensive literature reviews

4. **Google Gemini**
   - **Free Tier**: Available
   - **Strengths**: Good performance, multimodal capabilities
   - **Use Cases**: General note-taking and research assistance

## Quantization Techniques

To reduce VRAM requirements, use quantization:

- **4-bit (GPTQ/GGUF)**: Reduces VRAM by ~75% with minimal quality loss
- **8-bit**: Reduces VRAM by ~50% with negligible quality loss
- **AWQ**: Advanced quantization with better quality preservation

### Recommended Tools for Quantized Models

- **llama.cpp**: Run GGUF quantized models (CPU or GPU)
- **text-generation-webui**: User-friendly interface for various models
- **Ollama**: Simple local LLM management and inference

## Configuration Tips for Zotero-GPT

1. **Context Length**:
   - Limit to 2048-4096 tokens for efficiency
   - Longer contexts require more VRAM

2. **Batch Size**:
   - Use batch size of 1 for minimal memory usage
   - Increase only if you have extra VRAM

3. **Temperature Settings**:
   - Lower (0.3-0.5) for factual tasks like citation formatting
   - Higher (0.7-0.9) for creative note generation

4. **Max Tokens**:
   - Limit response length to 512-1024 tokens to save memory
   - Adjust based on your note-taking needs

## Installation Guide for Local Models

### Using Ollama (Recommended for Beginners)

```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Pull a lightweight model
ollama pull phi-2
ollama pull mistral:7b-instruct-v0.2-q4_K_M

# Run the model
ollama run phi-2
```

### Using llama.cpp

```bash
# Clone repository
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp

# Build with GPU support (CUDA)
make LLAMA_CUBLAS=1

# Download a GGUF model and run
./main -m models/mistral-7b-instruct-v0.2.Q4_K_M.gguf \
  -n 512 -c 2048 --temp 0.7
```

## Performance Benchmarks

| Model          | VRAM | Speed (tokens/s) | Quality Score |
| -------------- | ---- | ---------------- | ------------- |
| TinyLlama-1.1B | 2GB  | 100-150          | 6/10          |
| Phi-2          | 3GB  | 80-120           | 7.5/10        |
| Mistral-7B-Q4  | 5GB  | 40-60            | 8.5/10        |
| Llama-2-7B-Q4  | 6GB  | 35-55            | 8/10          |
| Llama-2-13B-Q4 | 9GB  | 25-40            | 8.8/10        |

_Note: Speeds vary based on hardware. Quality scores are subjective estimates for academic tasks._

## Choosing the Right Model

Consider these factors:

1. **Available VRAM**: Start with models that fit comfortably within your limits
2. **Task Complexity**: Simple tasks (formatting) vs. complex tasks (synthesis)
3. **Speed Requirements**: Faster models may sacrifice quality
4. **Context Length**: Longer documents need models with larger context windows
5. **Privacy Concerns**: Local models keep your research notes private

## Common Issues and Solutions

### Out of Memory Errors

- Use more aggressive quantization (4-bit instead of 8-bit)
- Reduce context length
- Close other GPU applications
- Use CPU offloading for layers that don't fit in VRAM

### Slow Inference

- Use quantized models
- Reduce batch size to 1
- Enable Flash Attention if available
- Consider using smaller models

### Poor Quality Outputs

- Try less aggressive quantization
- Increase temperature for creative tasks
- Use a larger model if VRAM allows
- Fine-tune prompts for better results

## Integration with Zotero-GPT

Once you've chosen and set up a model, configure Zotero-GPT:

1. Open Zotero preferences
2. Navigate to Zotero-GPT settings
3. Configure the API endpoint (local or cloud)
4. Test the connection with a simple note
5. Adjust parameters based on performance

## Recommended Workflow

For most users with 4-8GB VRAM:

1. **Start with**: Mistral-7B-Instruct (4-bit quantized)
2. **Install via**: Ollama (easiest) or text-generation-webui
3. **Configure**: 2048 token context, temperature 0.6
4. **Test**: Generate a note summary from a paper
5. **Adjust**: Based on speed and quality needs

## Additional Resources

- [Zotero-GPT Documentation](https://github.com/MuiseDestiny/zotero-gpt)
- [Ollama Model Library](https://ollama.com/library)
- [Hugging Face Model Hub](https://huggingface.co/models)
- [llama.cpp Repository](https://github.com/ggerganov/llama.cpp)

## Conclusion

You don't need high-end hardware to use AI-powered features with Better Notes. Start with a model that fits your VRAM budget, and experiment with different options to find the best balance of speed, quality, and resource usage for your research workflow.

For questions or issues, please visit the [Better Notes discussions](https://github.com/windingwind/zotero-better-notes/discussions) or [Zotero-GPT issues](https://github.com/MuiseDestiny/zotero-gpt/issues).
