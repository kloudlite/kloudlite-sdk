# kloudlite (Python)

```python
from kloudlite import hub
path = hub.download("acme/my-model", provider="kloudlite")
```

Hugging Face is the default provider. `kloudlite` reads `KL_HUB_URL` and the workspace token.
Install: `pip install kloudlite` (before the first PyPI release: `pip install "kloudlite @ git+https://github.com/kloudlite/kloudlite-sdk#subdirectory=python"`).
Tests: `python -m unittest discover -s tests` (needs `huggingface_hub`).
