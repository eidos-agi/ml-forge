---
id: "GUARD-001"
type: "guardrail"
title: "No software — skills and templates only"
status: "active"
date: "2026-03-24"
---

## Rule
This repo must never become a Python package or installable tool. No pyproject.toml, no CLI, no MCP server. Skills and templates only.

## Why
Consistency across the forge ecosystem. Skills evolve with the agent. Software requires maintenance, versioning, and dependency management. The forge pattern keeps knowledge portable.

## Violation Examples
- Adding a `pyproject.toml` with `[project]` metadata
- Creating a `__main__.py` or CLI entry point
- Publishing to PyPI
