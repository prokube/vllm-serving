# vllm-serving

This example demonstrates how to deploy a vLLM model in a kubernetes cluster with istio service mesh. Inspired by: https://docs.vllm.ai/en/latest/deployment/frameworks/helm.html

## Deploy the Model

With helm:

1. Modify `values.yaml` according to your needs.
1. Deploy chart (example for `example-prokube` namespace):
   ```sh
   helm install my-vllm  . \
         -f values.yaml \
         --namespace example-prokube \
         --create-namespace
   ```
1. If you want to deploy huggingface models from gated repos, specify a hugginface API token on helm chart install (optional):
   ```sh
     helm install my-vllm  . \
         -f values.yaml \
         --namespace example-prokube \
         --create-namespace \
         --set HF_API_TOKEN="YOUR_TOKEN"
   ```

Namespace is required, otherwise chart is deployed in `default` namespace.

## Sending Requests

Example request is provided below.

```sh
 curl https://<your host>/vllm-serving/qwen-inf-serv/v1/chat/completions \
    -H "Content-Type: application/json" \
    -H "x-api-key: 91cEhCn37qnBXZES" \
    -d '{
          "model": "Qwen/Qwen2.5-1.5B-Instruct",
          "messages": [{"role": "user", "content": "Hello!"}]
        }'
```

_Notes_:

- `qwen-inf-serv` in the address should correspond to `endpointPath` in values.yaml
- "x-api-key: 91cEhCn37qnBXZES" header is required by EnvoyFilter (see admin/envoyfilter.yaml)