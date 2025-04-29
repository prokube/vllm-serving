# vllm-serving
This example demonstrates how to deploy a vLLM model in kubernetes cluster.


## Deploy the Model
You would need a valid kube config and a KUBECONFIG env var set.  
The deployment can then be initiated from this directory as follows:
```sh
kubectl apply -f examples/qwen-vllm.yaml
```

With helm:
1. If you want to deploy huggingface models, specify an API token first and convert it to base64:
    ```sh
    echo "YOUR_TOKEN" | tr -d "\n" | base64
    ```
2. Copy the command output and run helm:
    ```sh
    helm install my-vllm . -f values.yaml  --set HF_API_TOKEN="YOUR_COMMAND_OUTPUT"
    ```

## Sending Requests
Note: `qwen-inf-serv` in the address corresponds to `endpointPath` in values.yaml. (it should correspond to endpoint defined in VirtualService resource)
```sh
 curl https://<your host>/vllm-serving/qwen-inf-serv/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{
          "model": "qwen-2.5-1.5b",
          "messages": [{"role": "user", "content": "Hello!"}]
        }'