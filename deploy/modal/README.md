# Modal T4 Diart Deployment

The deployment uses a scale-to-zero T4, a persistent model-cache volume, and a
named Hugging Face secret. WebSocket support is provided by Modal's ASGI web
function.

```bash
python -m pip install modal
modal setup
modal secret create his-agent-huggingface HF_TOKEN=YOUR_TOKEN
modal run deploy/modal/diart_app.py::populate_model_cache
modal deploy deploy/modal/diart_app.py
```

Copy the deployed `diart_web` HTTPS URL into the Aliyun backend environment:

```env
DIARIZATION_INTERNAL_URL=https://YOUR-MODAL-URL.modal.run
```

Then restart `his-agent-backend`. Keep `min_containers=0` for low cost. Before a
scheduled live review, temporarily set `min_containers=1` if eliminating the
GPU cold start is worth the additional T4 runtime cost.
