# Aliyun CPU Deployment

This layout keeps all model inference off the 2-vCPU/2-GiB host:

- Nginx serves the static HIS pages and terminates TLS.
- FastAPI backend runs on `127.0.0.1:8000`.
- GPT-5.5 proxy runs on `127.0.0.1:8001` and calls an external API.
- Qwen3 ASR bridge runs on `127.0.0.1:8010` and calls the realtime ASR API.
- Diart runs on Modal and is reached through the backend proxy.

Clone the repository to `/opt/his-agent`, then run `deploy/aliyun/install.sh` as
root. Secret `.env` files are intentionally not part of the repository.

The supplied Nginx configuration uses same-origin routes:

- `/api/*` -> backend
- `/health` and `/ws` -> ASR
- `/diarization/*` and `/ws/diarization` -> backend -> Modal

For a temporary TLS hostname, `8-210-44-40.sslip.io` resolves to the current
server IP. Replace it with the final reviewed domain in the Nginx server block,
then issue a new certificate.
