# Architecture Diagrams

This directory contains shared Mermaid diagrams for the GenAI Email & Report Drafting System architecture.

## Diagram Files

### `system-architecture.mmd`

**Purpose:** Detailed system architecture diagram with component breakdown  
**Used in:** README.md (main project overview)  
**Features:**

- Full N-Tier architecture flow
- Frontend components subgraph (Login, Email Generator, Report Generator, History, Admin)
- Backend services subgraph (Authentication, Document Generation, Prompt Engine, Admin API)
- Styled with color-coded layers

### `system-architecture-simple.mmd`

**Purpose:** Simplified architecture overview  
**Used in:** docs/04_technical.md (technical documentation)  
**Features:**

- Basic N-Tier flow without subgraphs
- Clean, simple representation
- Focus on layer interactions

### `system-architecture-basic.mmd`

**Purpose:** Basic architecture diagram with styling  
**Used in:** docs/05_architecture_plan.md (architecture planning document)  
**Features:**

- N-Tier architecture with styled layers
- Uses `--` for connections (different from main diagram)
- Color-coded layers matching project theme

## Usage

**Current Approach:** Diagrams are inlined in markdown files for compatibility with standard Mermaid renderers.

**Source Files:** The `.mmd` files in this directory serve as the **single source of truth** for diagram definitions. When updating diagrams:

1. Edit the `.mmd` file in this directory
2. Copy the content to all markdown files that reference it
3. This ensures consistency across all documentation

**Files to Update When Modifying Diagrams:**

- `system-architecture.mmd` → Update in: `README.md`
- `system-architecture-simple.mmd` → Update in: `docs/04_technical.md`
- `system-architecture-basic.mmd` → Update in: `docs/05_architecture_plan.md`
- `system-architecture-requirements.mmd` → Update in: `docs/02_requirements.md`

## Benefits

- **Single Source of Truth**: `.mmd` files in this directory are the master copies
- **Consistency**: All documentation uses diagrams from the same source files
- **Maintainability**: Update the `.mmd` file, then copy to markdown files
- **Compatibility**: Works with all standard Mermaid renderers (GitHub, VS Code, etc.)

## Note

Standard Mermaid renderers don't support `@include` directives. Diagrams are inlined in markdown files for maximum compatibility. The `.mmd` files serve as master copies that should be kept in sync with the inlined versions.
