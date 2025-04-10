# vllm-serving
This example demonstrates how to deploy a vLLM model in kubernetes cluster.


## Deploy the Model
You would need a valid kube config and a KUBECONFIG env var set.  
The deployment can then be initiated from this directory as follows:
```sh
kubectl apply -f qwen-vllm.yaml
kubectl apply -f auth.yaml
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