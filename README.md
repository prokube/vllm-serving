# vllm-serving
This example demonstrates how to deploy a vLLM model in a kubernetes cluster. A prerequisite is an existing namespace where you want to deploy.

## Deploy the Model
With helm:
1. If you want to deploy huggingface models, specify a hugginface API token on helm chart install (optional):
    ```sh
    helm install my-vllm . -f values.yaml  --set HF_API_TOKEN="YOUR_TOKEN" --namespace example-prokube
    ```
    Namespace is required, otherwise chart is deployed in `default` namespace.

## Sending Requests
Example request is provided below. *Note*: `qwen-inf-serv` in the address should correspond to `endpointPath` in values.yaml.
```sh
 curl https://<your host>/vllm-serving/qwen-inf-serv/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{
          "model": "Qwen/Qwen2.5-1.5B-Instruct",
          "messages": [{"role": "user", "content": "Hello!"}]
        }'