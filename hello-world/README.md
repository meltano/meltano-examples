# hello-world

Demonstrates how to define a fully inline custom utility plugin — no Meltano Hub lookup, no `pip install`, no virtual environment required.

## What it demonstrates

- Defining a plugin entirely inside `meltano.yml` using `executable` and `commands`
- Using a system binary (`python3`) as the plugin executable
- Running a custom command with `meltano invoke`

## Prerequisites

- [Meltano installed](https://docs.meltano.com/getting-started/installation)
- `python3` available on your system PATH

## How to run

```bash
cd hello-world
meltano invoke hello-world:say-hello
```

No `meltano install` step is needed — this plugin uses the system `python3` binary directly.

## Expected output

```
hello world
```

## Key config notes

The plugin is defined entirely in `meltano.yml` with no external dependencies:

```yaml
plugins:
  utilities:
  - name: hello-world
    namespace: hello_world
    executable: python3
    commands:
      say-hello:
        args: -c "print('hello world')"
        description: Prints hello world to stdout
```

The `executable` field points to any binary on your PATH, and `commands` lets you attach named argument sets to it. This pattern is useful for wrapping simple scripts or system tools without creating a full plugin package.
