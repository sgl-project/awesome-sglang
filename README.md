# Awesome SGLang

[SGLang](https://github.com/sgl-project/sglang) is a fast serving framework for large language models and vision language models. It makes your interaction with models faster and more controllable by co-designing the backend runtime and frontend language. The core features include:

* **Fast Backend Runtime**: Provides efficient serving with RadixAttention for prefix caching, zero-overhead CPU scheduler, prefill-decode disaggregation, speculative decoding, continuous batching, paged attention, tensor/pipeline/expert/data parallelism, structured outputs, chunked prefill, quantization (FP4/FP8/INT4/AWQ/GPTQ), and multi-lora batching.
* **Flexible Frontend Language**: Offers an intuitive interface for programming LLM applications, including chained generation calls, advanced prompting, control flow, multi-modal inputs, parallelism, and external interactions.
* **Extensive Model Support**: Supports a wide range of generative models (Llama, Qwen, DeepSeek, Kimi, GPT, Gemma, Mistral, etc.), embedding models (e5-mistral, gte, mcdse) and reward models (Skywork), with easy extensibility for integrating new models.
* **Active Community**: SGLang is open-source and backed by an active community with wide industry adoption.

If you are looking to:

* **Run open-source models with SGLang**, please check out [Installation](sglang-cookbook/installation/) and [Send your first request](sglang-cookbook/send-your-first-request.md)
* **Build applications with SGLang**, please start with [Model Recipes](sglang-cookbook/model-recipes/) and [API](sglang-cookbook/api/).
* **Deploy SGLang to production environment**, please check out [Production Deployment](k8s/).
* **Contribute to SGLang**, please check out [Developer Guide](developer-guide/).

For information about SGLang, see:

* [LMSYS blog](https://lmsys.org/blog/) for features and updates
* [Roadmap](https://github.com/sgl-project/sglang/issues/7736)
* [Releases](https://github.com/sgl-project/sglang/releases)
* [sgl-learning-materials](https://github.com/sgl-project/sgl-learning-materials) for more blogs, slides, and videos

Please join our Slack Channel [https://slack.sglang.ai](https://slack.sglang.ai/) for questions. For enterprises interested in adopting or deploying SGLang at scale, including technical consulting, sponsorship opportunities, or partnership inquiries, please contact us at [contact@sglang.ai](mailto:contact@sglang.ai).
