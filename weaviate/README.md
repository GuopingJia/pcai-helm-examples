# Weaviate

[Weaviate](https://weaviate.io) is a decentralised vector search engine.

Please visit the [official docs](https://weaviate.io/developers/weaviate/installation/kubernetes)
to learn how to install Weaviate on K8s using helm.

## Install

```bash
helm repo add weaviate https://weaviate.github.io/weaviate-helm
helm install my-weaviate weaviate/weaviate
```

# Notes

The original Weaviate helm chart has been downloaded from [here](https://github.com/weaviate/weaviate-helm/releases/download/v17.4.2/weaviate.tgz) 
and refers to v17.4.2 (Weaviate version 1.28.4). We've modified the existing values.yaml and added additional manifest files required for HPE PCAI.
