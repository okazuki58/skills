---
name: spec-to-code
description: Guide for specification-driven development workflow. Use when users want to (1) conduct specification interviews to clarify requirements, (2) follow test-driven development (TDD) practices, (3) generate GitHub issues from specifications, or (4) establish a structured development workflow from requirements to implementation. Language and framework agnostic.
---

# Spec-to-Code Development Workflow

This skill guides you through a comprehensive development workflow from specification to implementation.

## Workflow Overview

```
1. Specification Interview → 2. Documentation → 3. GitHub Issue → 4. TDD → 5. Implementation
```

## When to Use

- Starting new features or projects
- Requirements are unclear
- Following TDD practices
- Creating structured GitHub issues

## Usage Instructions

### 1. Specification Interview

Read `references/spec_template.md` and conduct thorough interview covering:
- Technical implementation
- UI/UX considerations
- Performance and security
- Edge cases and tradeoffs

### 2. Generate Documentation

Create specification document using the template structure, including decisions and rationale.

### 3. Create GitHub Issue

Read `references/issue_template.md` and generate complete issue with acceptance criteria and test scenarios.

### 4. Follow TDD

Read `references/tdd_guide.md` and guide through Red-Green-Refactor cycle.

## Key Principles

- **Specificity over ambiguity**: Concrete requirements only
- **Document the "why"**: Rationale is crucial
- **Test-first approach**: Write tests before implementation
- **Language agnostic**: Works with any tech stack

## References

- `references/spec_template.md` - Specification interview template
- `references/tdd_guide.md` - TDD workflow guide  
- `references/issue_template.md` - GitHub issue template
