# LiteLLM

- https://github.com/BerriAI/litellm
- https://docs.litellm.ai/docs/proxy/docker_quick_start

```sh
podman run \
    -v $(pwd)/litellm_config.yaml:/app/config.yaml \
    -p 4000:4000 \
    -e GITHUB_API_KEY=*** ¥
    ghcr.io/berriai/litellm:main-latest \
    --config /app/config.yaml --detailed_debug

# RUNNING on http://0.0.0.0:4000
```
