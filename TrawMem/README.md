# TrawMem

This repository contains the reference implementation of **TrawMem** from
the paper *Task-Oriented Role-Adaptive Workspaces*.

TrawMem builds persistent interaction threads and a sparse thread graph. For a
query, it routes to local threads and assembles a temporary workspace using
query-local Anchor, Bridge, and Context roles. Role assignments are discarded
after workspace construction and are never stored as permanent memory labels.

## Repository layout

- `main.py`: the `TrawMemSystem` interface and system factory
- `trawmem/`: thread construction, graph routing, role-aware workspace
  assembly, and answer generation
- `tests/`: focused tests for routing, workspace construction, prompts, and
  failure handling
- `test_locomo10.py`: LoCoMo evaluation adapter
- `test_memgallery.py`: MemGallery evaluation adapter
- `setup.py`: package metadata and optional dependency groups
- `LICENSE`: MIT License

The public Python package is `trawmem`. Dataset files, model weights, API
keys, caches, databases, and generated outputs are not included.

## Installation

```bash
python3 -m pip install -r requirements.txt
python3 -m pip install -e .
```

Set model and API settings with environment variables. You may optionally
create a local top-level `config.py`; it is intentionally not included in the
repository. Keep that file, datasets, model weights, caches, and generated
outputs outside version control. Never commit credentials.

## Minimal usage

```python
from main import TrawMemSystem

system = TrawMemSystem()
system.add_dialogue("Alice", "Bob and I will meet tomorrow at 2pm.")
system.finalize()
answer = system.ask("When will Alice and Bob meet?")
print(answer)
```

The first run may download the configured embedding model and requires a
working LLM endpoint. Set `OPENAI_API_KEY`, `LLM_MODEL`, and related settings
before creating the system.

## Tests

Run the full test suite with:

```bash
PYTHONPATH=. pytest -q
```

## Evaluation

Provide dataset paths from the caller. Do not place datasets, model weights,
or generated results in the repository.

LoCoMo:

```bash
PYTHONPATH=. python3 test_locomo10.py \
  --dataset /path/to/locomo10.json \
  --result-file /path/to/locomo10_results.json
```

MemGallery:

```bash
PYTHONPATH=. python3 test_memgallery.py \
  --benchmark memgallery \
  --dataset /path/to/memgallery \
  --result-file /path/to/memgallery_results.json
```

Use `--llm-judge` when semantic LLM judging is required. Both adapters expose
sharding options for distributed evaluation.

## Reproducibility scope

This repository contains source code, tests, package metadata, and the MIT
license. It does not contain paper files, private paths, datasets, model
weights, API keys, experiment outputs, caches, or generated figures.
