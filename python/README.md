# kloudlite

Download models and datasets from Hugging Face or from the Kloudlite hub with one call. The
result is a local path you hand straight to `transformers`, `diffusers` or `datasets`.

```sh
pip install kloudlite
```

## Quick start

```python
from kloudlite import hub
from transformers import AutoModelForCausalLM, AutoTokenizer

path = hub.download("acme/my-model", provider="kloudlite")   # a model on the Kloudlite hub
model = AutoModelForCausalLM.from_pretrained(path)
tokenizer = AutoTokenizer.from_pretrained(path)
```

Leave out `provider` and the same call reads from Hugging Face:

```python
path = hub.download("openai-community/gpt2")
```

## Providers

| `provider` | Reads from | Token | Cache |
|---|---|---|---|
| `"huggingface"` (default) | huggingface.co, or your `HF_ENDPOINT` | your Hugging Face token, if any | the usual Hugging Face cache |
| `"kloudlite"` | `$KL_HUB_URL` | `$KL_HUB_TOKEN`, else the workspace token | `~/.cache/kloudlite/hub` |

The Hugging Face provider passes nothing of its own, so your Hugging Face token, mirror and cache
work as before.

The Kloudlite provider never sends your Hugging Face token. It keeps a separate cache because
Hugging Face stores files by repo name only, and the same name can exist on both hubs.

Inside a Kloudlite workspace, `KL_HUB_URL` and the token are already set. Elsewhere, set
`KL_HUB_URL` and `KL_HUB_TOKEN` yourself.

## API

| Function | Returns |
|---|---|
| `hub.download(repo_id, provider="huggingface", **kw)` | local path of the whole repo |
| `hub.download_file(repo_id, filename, provider="huggingface", **kw)` | local path of one file |
| `hub.api(provider="huggingface")` | an `HfApi` bound to the provider, for uploads, repo creation and the rest |

Extra keyword arguments go straight to `huggingface_hub` (`snapshot_download`,
`hf_hub_download`), for example `revision="v1"`, `repo_type="dataset"` or `allow_patterns=["*.json"]`.

```python
# A dataset
path = hub.download("acme/reviews", provider="kloudlite", repo_type="dataset")

# One file
cfg = hub.download_file("acme/my-model", "config.json", provider="kloudlite")

# Upload a folder
api = hub.api("kloudlite")
api.create_repo("acme/my-model", exist_ok=True)
api.upload_folder(repo_id="acme/my-model", folder_path="./out")
```

## Requirements

- Python 3.9 or newer
- `huggingface_hub`, installed with the package

## Development

```sh
cd python
python -m unittest discover -s tests
```

Source: https://github.com/kloudlite/kloudlite-sdk
