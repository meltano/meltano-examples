# meltano-examples

Examples of the many ways Meltano can be used to solve data engineering use cases.

Each example is a self-contained Meltano project you can clone and run locally.

## Examples

| Example | Category | What it demonstrates |
|---------|----------|----------------------|
| [hello-world](./hello-world/) | Utilities | Inline custom plugin with no hub lookup or pip install |

## Getting started

**Prerequisites:** [Meltano installed](https://docs.meltano.com/getting-started/installation) (pipx install meltano recommended).

Each example follows the same pattern:

```bash
cd <example-name>
meltano install
meltano invoke <plugin>:<command>
```

Some examples may have additional steps — see the individual README for details.

## Categories

As this repo grows, examples will span several themes:

- **Utilities** — custom and inline utility plugins
- **ELT Pipelines** — extractors, loaders, and `meltano run` pipelines
- **Transforms** — dbt integration and SQL transforms
- **Orchestration** — scheduling and Airflow/Dagster integration
- **Custom Plugins** — building and packaging your own Meltano plugins

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for how to add a new example.
