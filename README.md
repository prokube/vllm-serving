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
Note: `qwen-inf-serv` endpoint is defined in VirtualService resource.
```sh
 curl https://<your host>/qwen-inf-serv/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{
        "model": "Qwen/Qwen2.5-1.5B-Instruct",
        "guided_choice": ["positive", "negative"],
        "messages": [
            {"role": "user", "content": "Classify this sentiment: vLLM is wonderful!"}
        ]
    }' \
```