# kube-ollama

Ollama manifests to deploy in a Kubernetes cluster

## Steps

1. Create [kind](https://github.com/kubernetes-sigs/kind) cluster, make sure there enough cpu and memory in respective vm/docker desktop/podman desktop resource settings. In this example I am using multi node cluster, you can single node cluster if wanted. It will take sometime to get the deployment ready.

    ```bash
    kind create cluster --config kind-config.yaml
    ```

1. Deploy ollama and expose the service

    ```bash
    kubectl create -f deploy/deployment.yaml
    ```

1. Port-forward the service for interacting with ollama running in cluster.

    Note: If there is a port conflicct use a different port than 8000

    ```bash
    kubectl -n ollama  port-forward svc/ollama 8000
    ```

1. Download a model

    ```bash
    curl http://localhost:8000/api/pull -d '{
    "model": "llama3.2"
    }'
    ```

1. See the downloaded model

    ```bash
    curl http://localhost:8000/api/tags
    ```

1. Try chatting with the model

    ```bash
    curl http://localhost:8000/api/generate -d '{
    "model": "llama3.2",
    "prompt": "What is capital of India?"
    }'
    ```

Now you have installed and configured ollama in a kubernetes cluster.

## References

- [ollama api](https://github.com/ollama/ollama/blob/main/docs/api.md)
