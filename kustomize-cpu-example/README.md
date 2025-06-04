# vllm-cpu

This directory contains kustomization to deploy vLLM CPU-compatible image with a built-in `facebook/opt-125` model in a kubernetes cluster. Mostly taken from [docs](https://docs.vllm.ai/en/latest/deployment/k8s.html#using-kubernetes). This demo is useful for clusters where images can't be pulled externally. This kustomize repo also deploys a PVC for the corresponding kubernetes Deployment.

## Deploy vLLM-CPU image with pre-included facebook model
The code to build the image is presented in the Dockerfile. The Dockerfile is intended to substitute `Dockerfile.cpu` in the [official vLLM repo](https://github.com/vllm-project/vllm), see [docs](https://docs.vllm.ai/en/latest/getting_started/installation/cpu/index.html#set-up-using-docker) on how to build. 

Deployment steps briefly: 
1. Clone vLLM repo locally.
2. Substitute `Dockerfile.cpu` with the one from this repo. 
3. Build docker [image](https://docs.vllm.ai/en/latest/getting_started/installation/cpu/index.html#set-up-using-docker) and push to a repo from which your cluster can pull. 
4. Specify the pushed image in `base/kustomization.yaml`.
5. Apply manifest `kubectl apply -k base/`.

When deployed, the model is reachable inside the cluster with a following command:

```sh
# replace "example-prokube" if the service is deployed in another namespace
# model directory should include config.json file
curl -X POST "vllm-service.example-prokube.svc.cluster.local/v1/completions" \
     -H "Content-Type: application/json" \
     -d '{
           "prompt": "Once upon a time,",
           "max_tokens": 50,
           "model": "/models/models--facebook--opt-125m/snapshots/27dcfa74d334bc871f3234de431e71c6eeba5dd6"
         }'
```
