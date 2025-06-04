Admin info:
* Deploy the manifests to allow traffic to `/vllm-serving/` endpoint without dex auth (if dex is enabled in cluster)
* If using dex, corresponding istio jwt and oauth2 authorization policies should have an ignore rule for `/vllm-serving/*` endpoint
