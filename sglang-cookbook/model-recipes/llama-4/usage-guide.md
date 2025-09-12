# Usage Guide

### <mark style="background-color:green;">Serving with 1 x 4 x H200</mark>

{% stepper %}
{% step %}
#### Install SGLang


{% endstep %}

{% step %}
#### Serve the model (text only)

{% code overflow="wrap" %}
```bash
python3 -m sglang.launch_server \  
  --model-path meta-llama/Llama-4-Scout-17B-16E-Instruct \  
  --host 0.0.0.0 \  
  --port 30000
```
{% endcode %}


{% endstep %}

{% step %}
#### Benchmark
{% endstep %}
{% endstepper %}
