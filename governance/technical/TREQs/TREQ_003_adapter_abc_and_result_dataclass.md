---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_003
type: TREQ
title: "Adapter ABC & AdapterResult Dataclass"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.1.0 — Bootstrap & Session Gateway"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_002
  cross_plane:
    architecture_adr: ADR_001
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_003 — Adapter ABC & AdapterResult

## 1. Intent & Scope
Every adapter interface declared as Python ABC. All methods
return AdapterResult dataclass.

## 2. Technical Constraints
```python
from abc import ABC, abstractmethod
from dataclasses import dataclass
from typing import Any

@dataclass
class AdapterResult:
    success: bool
    data: Any | None = None
    error: str | None = None

class LLMAdapter(ABC):
    @abstractmethod
    def invoke_llm(self, prompt: str, context: str) -> AdapterResult: ...

class GitAdapter(ABC):
    @abstractmethod
    def fetch_code_delta(self, repo: str, branch: str) -> AdapterResult: ...
    @abstractmethod
    def commit(self, message: str, files: list) -> AdapterResult: ...
    @abstractmethod
    def branch(self, name: str) -> AdapterResult: ...
    @abstractmethod
    def create_remote_repo(self, name: str, visibility: str, org: str) -> AdapterResult: ...

class FilesystemAdapter(ABC):
    @abstractmethod
    def write_file(self, path: str, content: str) -> AdapterResult: ...
    @abstractmethod
    def read_file(self, path: str) -> AdapterResult: ...
    @abstractmethod
    def archive_asset(self, source: str, destination: str) -> AdapterResult: ...
    @abstractmethod
    def secure_delete(self, path: str) -> AdapterResult: ...

class RegistryAdapter(ABC):
    @abstractmethod
    def read_registry(self) -> AdapterResult: ...
    @abstractmethod
    def write_registry(self, content: str) -> AdapterResult: ...
    @abstractmethod
    def read_profile(self, operator_id: str) -> AdapterResult: ...
    @abstractmethod
    def write_profile(self, operator_id: str, content: str) -> AdapterResult: ...
    @abstractmethod
    def read_context(self, project_id: str) -> AdapterResult: ...
    @abstractmethod
    def write_context(self, project_id: str, content: str) -> AdapterResult: ...
```

## 3. Verification & QA Criteria
* All production adapters verified as ABC subclasses.
* AdapterResult returned by 100% of adapter methods.
* No raw exceptions propagate beyond adapter boundary.
