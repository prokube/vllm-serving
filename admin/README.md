Admin info:
* Deploy the manifest to allow traffic to `/vllm-serving/` endpoint without dex auth
* Corresponding istio jwt and oauth2 authorization policies should have an ignore rule for `/vllm-serving/*` endpoint
