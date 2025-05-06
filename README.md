# lab-ollama-openhands

## WSL NVIDIA Container Toolkit

https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installing-the-nvidia-container-toolkit

dockerの設定はDocker Desktop側に入れて再起動。
docker再起動でだめだったのでWSL更新のうえWindowsごと再起動した。

[Running a Sample Workload — NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/sample-workload.html) の通り、WSLからGPUが見えるようになったのでOK

```sh
~/github.com/lab-ollama-openhunds (main*) » sudo docker run --rm --runtime=nvidia --gpus all ubuntu nvidia-smi
[sudo] password for tk3fftk: 
Sat Mar 22 09:12:46 2025       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 570.133.07             Driver Version: 572.83         CUDA Version: 12.8     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 3060 Ti     On  |   00000000:01:00.0  On |                  N/A |
|  0%   48C    P8             16W /  200W |    1337MiB /   8192MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
                                                                                         
+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI              PID   Type   Process name                        GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+
-------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------
```

## OpenHands

- docker containerが起動できない的なエラーが発生したので `privileged: true` を渡してみた
- custom modelの指定は `ollama/deepseek-r1:7b` のように `ollama/<model_name>` みたいにする必要あり
- Base URLはvalidationなどは特に入ってないので注意 (最初にスペースが入っていても通ってしまうが、スキーマエラーになる)

## Ollama

- Modelを読み込んでおく (brunoでやったが、curlだとこうなるらしい)

```sh
curl --request POST \
  --url http://localhost:11434/api/pull \
  --header 'content-type: application/json' \
  --data '{
  "model": "deepseek-r1:7b"
}'
```

## Open WebUI

https://github.com/open-webui/open-webui

```sh
docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

## References

- [OpenHandsをOllamaをwsl2を使って完全ローカルで動かしてみる。 - 地平線まで行ってくる。](https://bwgift.hatenadiary.jp/entry/2025/02/24/233957)
- [SRE こそ OpenHands 使ってみな 飛ぶぞ](https://zenn.dev/ryoyoshii/articles/c810d2fa9f7769)

