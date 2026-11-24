<div align="center">

<img src="https://raw.githubusercontent.com/Studio1HQ/Studio1HQ/main/assets/banners/llm-deployment-jozu-hub.webp" alt="On-Prem LLM Deployment with Jozu Hub: Package a Docker model as a ModelKit and run it on local Kubernetes" width="100%" />

<br />
<br />

# On-Prem LLM Deployment with Jozu Hub

### Package a Docker model as a ModelKit and run it on local Kubernetes

A notebook that extracts a model from Docker Model Runner, packages it as a KitOps ModelKit, pushes it to Jozu Hub and deploys it to Minikube as a llama.cpp pod.

<br />

[![Built by Studio1](https://img.shields.io/badge/Built_by_Studio1-f97316?style=for-the-badge&logo=data%3Aimage%2Fsvg%2Bxml%3Bbase64%2CPHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxNjAgMTYwIiBmaWxsPSJub25lIj48cGF0aCBmaWxsLXJ1bGU9ImV2ZW5vZGQiIGNsaXAtcnVsZT0iZXZlbm9kZCIgZD0iTTAgMzVDMCAxNi42ODA3IDE0LjA3NDQgMS42NDg0NCAzMiAwLjEyNjk1M1YxNDFINTEuMDUyM0M1Mi40MzA4IDEzNy4zNDQgNTQuNzQ0NiAxMzMuNjg5IDU3Ljk5MzggMTMwLjAzM0M2MS4xNDQ2IDEyNi41MDggNjQuNjg5MiAxMjMuNDQgNjguNjI3NyAxMjAuODI5QzcyLjU2NjIgMTE4LjM0OSA3Ni4zNTY5IDExNi43ODIgODAgMTE2LjEyOVY4Ni43NTQ0Qzc1LjQ3MDggODcuOTI5MiA3MS4zODQ2IDg5LjY5MTkgNjcuNzQxNSA5Mi4wNDE1QzY0IDk0LjUyMjUgNjAuNzUwOCA5Ny40NiA1Ny45OTM4IDEwMC44NTRDNTYuNTM0NSAxMDIuNzIgNTUuMjU0NSAxMDQuNjU5IDU0LjE1MzkgMTA2LjY3MVYwSDEyNUMxNDQuMzMgMCAxNjAgMTUuNjY5OSAxNjAgMzVWMTI1QzE2MCAxNDQuMzMgMTQ0LjMzIDE2MCAxMjUgMTYwSDEyNFYxOEgxMDQuOTQ4QzEwMy41NjkgMjEuNjgxNiAxMDEuMjU1IDI1LjM2MjggOTguMDA2MiAyOS4wNDQ0Qzk0Ljg1NTQgMzIuNTk0MiA5MS4zMTA4IDM1LjY4NDEgODcuMzcyMyAzOC4zMTRDODMuNDMzOCA0MC44MTIgNzkuNjQzMSA0Mi4zODk2IDc2IDQzLjA0NzRWNzIuNjMwNEM4MC41MjkyIDcxLjQ0NzMgODQuNjE1NCA2OS42NzI0IDg4LjI1ODUgNjcuMzA1N0M5MiA2NC44MDc2IDk1LjI0OTIgNjEuODQ5MSA5OC4wMDYyIDU4LjQzMDdDOTkuNDY1NSA1Ni41NTEzIDEwMC43NDUgNTQuNTk4NiAxMDEuODQ2IDUyLjU3MjNWMTYwSDM1QzE1LjY3IDE2MCAwIDE0NC4zMyAwIDEyNVYzNVoiIGZpbGw9IiNmZmZmZmYiLz48L3N2Zz4K)](https://studio1hq.com)
[![Built with Jozu](https://img.shields.io/badge/Built_with_Jozu-141414?style=for-the-badge)](https://jozu.com)

[Jozu docs](https://jozu.com/docs) &nbsp;·&nbsp; [Jozu](https://jozu.com) &nbsp;·&nbsp; [KitOps](https://kitops.org) &nbsp;·&nbsp; [More examples](https://github.com/Studio1HQ) &nbsp;·&nbsp; [studio1hq.com](https://studio1hq.com)

</div>

<br />

## Overview

This repo accompanies the blog post "Automate On-Prem LLM Deployment With Jozu Hub". A single notebook (`main.ipynb`) holds the commands from the article.

You pull the `ai/smollm2` model with Docker Model Runner, copy its GGUF file out, package it as a [KitOps](https://kitops.org) ModelKit, push it to [Jozu Hub](https://jozu.com), and deploy it to a local Minikube cluster. You then call the running model with the OpenAI Python SDK.

## Features

- **Docker model extraction**: `docker model pull ai/smollm2`, then `docker cp` to copy the GGUF bundle out of the model runner container.
- **Kitfile from Python**: The `kitops` library builds the Kitfile (package `smollm2`, model path `all_models/smollm2/model/model.gguf`, Apache 2.0).
- **Registry push**: `ModelKitManager.pack_and_push_modelkit` publishes to Jozu Hub.
- **Local Kubernetes**: Minikube and `kubectl` run the pod named `smollm2-llama-cpp`.
- **OpenAI-compatible client**: The `openai` SDK talks to the pod through `kubectl port-forward` on port 8000.

## Tech stack

| Layer | Tool |
| --- | --- |
| Model | `ai/smollm2` from [Docker Model Runner](https://docs.docker.com/ai/model-runner/) |
| Packaging | [KitOps](https://kitops.org) CLI and `kitops` Python library |
| Registry | [Jozu Hub](https://jozu.ml) |
| Orchestration | [Minikube](https://minikube.sigs.k8s.io) and `kubectl` |
| Client | [OpenAI Python SDK](https://github.com/openai/openai-python) |
| Runtime | Python 3.11+, Docker, Jupyter |

## Getting started

### Prerequisites

- Python 3.11 or later and Jupyter
- Docker
- A Jozu Hub account at [jozu.ml](https://jozu.ml)
- Homebrew on Linux or macOS (the notebook installs it if missing)
- Minikube and `kubectl` (the notebook installs Minikube with Homebrew; `kubectl` comes with it)

### Installation

```bash
git clone https://github.com/Studio1HQ/llm-deployment-jozu-hub.git
cd llm-deployment-jozu-hub
brew tap kitops-ml/kitops
brew install kitops
brew install minikube
pip install openai kitops
```

### Environment variables

No environment variables. The notebook authenticates with the email and password of your Jozu Hub account and uses placeholders you replace.

| Placeholder | Description |
| --- | --- |
| `email-prefix-here` | The part of your Jozu Hub email before the `@` (for `jack123@gmail.com` use `jack123`). Keep tags lowercase. |
| `full-email-here`, `password-here` | Jozu Hub credentials passed to `UserCredentials` in the notebook. |

### Run

Log in to Jozu Hub in a terminal (you are prompted for email and password), then open the notebook:

```bash
kit login jozu.ml
jupyter notebook main.ipynb
```


## How it works

1. Install Docker (Ubuntu steps are in the notebook), Docker Model Runner, KitOps, Minikube and `pip install openai kitops`.
2. Start the cluster with `minikube start` and log in with `kit login jozu.ml`.
3. Pull and run the model: `docker model pull ai/smollm2`, then `docker model run ai/smollm2` so it is loaded.
4. Find the model runner container with `docker ps`, inspect the model with `docker model inspect ai/smollm2`, and extract the files:
   ```bash
   docker cp <model-runner-container-id>:/models/bundles/sha256 ./all_models
   ```
   The internal path was verified with Docker Engine 27.2.0 and Docker Model Runner v1.0.9 and may change.
5. Build the Kitfile and push the ModelKit with `ModelKitManager`.
6. Deploy and test:
   ```bash
   kubectl apply -f jozu-deploy.yaml
   kubectl logs smollm2-llama-cpp
   kubectl port-forward pod/smollm2-llama-cpp 8000:8000
   ```
   The pod manifest `jozu-deploy.yaml` is created as part of the article and is not committed in this repo.
7. Call `http://127.0.0.1:8000/` with `OpenAI(base_url=..., api_key="sk-no-key-required")` and `model="smollm2"`.

## Resources

- [Jozu documentation](https://jozu.com/docs): Jozu Hub and Rapid Inference Container guides.
- [Jozu](https://jozu.com): Product site.
- [KitOps documentation](https://kitops.org/docs/overview/): ModelKit and Kitfile reference.

- [Docker Model Runner](https://docs.docker.com/ai/model-runner/): Source of the model used in the notebook.

<br />

---

<div align="center">

<a href="https://studio1hq.com">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Studio1HQ/website/main/brand/studio1-logo-on-dark.svg" />
    <img src="https://raw.githubusercontent.com/Studio1HQ/website/main/brand/studio1-logo-on-light.svg" alt="Studio1" width="160" />
  </picture>
</a>

Built by **[Studio1](https://studio1hq.com)**, the technical content and DevRel partner for devtool teams. Made with [Jozu](https://jozu.com).<br />
Browse more open-source examples at [github.com/Studio1HQ](https://github.com/Studio1HQ).

[![Website](https://img.shields.io/badge/studio1hq.com-0b0807?style=for-the-badge&logo=googlechrome&logoColor=white)](https://studio1hq.com)
[![Book a call](https://img.shields.io/badge/Book_a_strategy_call-f97316?style=for-the-badge&logo=googlecalendar&logoColor=white)](https://cal.com/studio1/collab)
[![X](https://img.shields.io/badge/%40Studio1HQ-141414?style=for-the-badge&logo=x&logoColor=white)](https://x.com/Studio1HQ)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0a66c2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/company/studio1hq/)

<sub><a href="https://studio1hq.com">studio1hq.com</a> · Technical content, docs, video and DevRel for devtools</sub>

</div>
