# Contributing

## Adding a new example

1. Create a new directory at the repo root named after your example (use `kebab-case`).
2. Inside it, create a working `meltano.yml` and a `README.md` following the template below.
3. Add a row for your example to the table in the root [README.md](./README.md).

## Directory structure

```
my-example/
  meltano.yml        # required — the Meltano project file
  README.md          # required — see template below
  .gitignore         # recommended — at minimum ignore .meltano/
  data/              # optional — sample input files
  scripts/           # optional — helper scripts
```

A minimal `.gitignore` for any Meltano project:

```
.meltano/
*.db
```

## README template

Copy this into your example's `README.md` and fill it in:

```markdown
# <example-name>

One sentence describing what this example does.

## What it demonstrates

- Key concept or feature #1
- Key concept or feature #2

## Prerequisites

- [Meltano installed](https://docs.meltano.com/getting-started/installation)
- Any other tools or accounts needed

## How to run

    cd <example-name>
    meltano install
    meltano invoke <plugin>:<command>

Add any additional steps specific to this example.

## Expected output

    <paste what the user should see>

## Key config notes

Explain any non-obvious configuration choices in meltano.yml.
```

## Guidelines

- **Keep examples minimal.** Each example should illustrate one concept clearly. Avoid combining multiple unrelated features.
- **Prefer working defaults.** Examples should run successfully out of the box with no required configuration edits.
- **Avoid committing secrets.** Use environment variables and document them in the README rather than hardcoding credentials.
- **Test before submitting.** Run through your own How to run steps in a clean environment.
