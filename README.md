# Procedural Planet Generation Documentation

This folder contains the documentation site for the Procedural Planet Generation (PPG) Unreal Engine plugin.

## Local Preview

Install the documentation dependencies:

```bash
pip install -r requirements.txt
```

Run a local preview server:

```bash
mkdocs serve
```

Build the static site:

```bash
mkdocs build --strict
```

The project is intentionally kept close to the standard Material for MkDocs layout, so it can also be tested with Zensical as its MkDocs compatibility improves.

## Images

Prepared screenshot placeholders are written as callout blocks throughout the docs. Add screenshots under:

```text
docs/assets/images/
```

Then replace the callout with a normal Markdown image reference.

