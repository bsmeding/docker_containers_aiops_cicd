# AIOps CI/CD Docker Images

Prebuilt Docker images for testing AI-assisted operational workflows in CI/CD pipelines.

These images focus on evaluation, regression testing, schema validation, tool-call testing, and replaying operational scenarios. They are not intended to be GPU or model-training images.

## Supported Base Images

Each image is built for a specific OS to match production environments or CI runner needs:

- `ubuntu2004` -> Ubuntu 20.04
- `ubuntu2204` -> Ubuntu 22.04
- `ubuntu2404` -> Ubuntu 24.04
- `ubuntu2604` -> Ubuntu 26.04
- `debian11` -> Debian Bullseye
- `debian12` -> Debian Bookworm
- `debian13` -> Debian Trixie
- `rockylinux8` -> Rocky Linux 8
- `rockylinux9` -> Rocky Linux 9
- `rockylinux10` -> Rocky Linux 10
- `alpine3.20` -> Alpine 3.20
- `alpine3.21` -> Alpine 3.21
- `alpine3.22` -> Alpine 3.22
- `alpine3.23` -> Alpine 3.23

Default tags point to the newest supported version for each distro family:

- `ubuntu` -> Ubuntu 26.04
- `debian` -> Debian Trixie
- `rockylinux` -> Rocky Linux 10
- `alpine3` -> Alpine 3.23

Each image is tagged as:

```text
bsmeding/aiops_cicd_<tag>:latest
```

## Included Software

Each image includes:

- Python 3 with a virtual environment at `/opt/venv`
- LLM clients and routers: OpenAI, Anthropic, LiteLLM
- Evaluation tooling: DeepEval and Ragas
- Agent/workflow tooling: LangChain, LangGraph, LangSmith client
- Test tooling: pytest, pytest-asyncio, pytest-cov, ruff, mypy
- Schema and contract tooling: Pydantic, JSON Schema, YAML, Jinja2
- Replay/mocking tools: responses, respx, vcrpy, freezegun, faker
- Data tools: pandas, numpy, duckdb
- Observability/API clients: OpenTelemetry SDK, Prometheus API client, pynautobot, pynetbox

## Use Cases

- Run prompt and agent regression tests
- Validate structured LLM output against JSON schemas
- Test tool-calling behavior before deployment
- Replay incident, ticket, or log examples as CI tests
- Test AI-assisted NetOps workflows against mocked Nautobot/NetBox APIs
- Score retrieval-augmented generation and operational recommendations

## GitHub Actions Example

```yaml
jobs:
  eval:
    runs-on: ubuntu-latest
    container:
      image: bsmeding/aiops_cicd_ubuntu:latest
    steps:
      - uses: actions/checkout@v5
      - run: pytest tests/aiops/
      - run: pytest tests/evals/
```

## Notes

Heavy local ML stacks such as CUDA, PyTorch, TensorFlow, and Transformers are intentionally not installed by default. This keeps the images suitable for fast CI jobs that call hosted models or test agent behavior.

promptfoo is useful for prompt regression testing, but it is not installed in the base image because reliable Node.js versions are not available across every supported older distro tag. Install promptfoo in a derived image or in the pipeline step when needed.

Third-party software installed in the image retains its own license terms.

## License

Apache-2.0

## Maintainer

[bsmeding](https://github.com/bsmeding) - Built for reproducible AIOps evaluation pipelines.
