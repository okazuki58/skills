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

**CRITICAL**: When conducting specification interviews, you MUST use the AskUserQuestionTool to ask questions one at a time. Do NOT list multiple questions in a single message.

Read `references/spec_template.md` and conduct thorough interview covering:
- Technical implementation
- UI/UX considerations
- Performance and security
- Edge cases and tradeoffs

**Interview Process**:
1. Read the spec_template.md to understand what information is needed
2. Use AskUserQuestionTool to ask ONE focused question at a time
3. Wait for the user's response
4. Based on their answer, ask the next most relevant question
5. Continue until all critical aspects are covered
6. Avoid obvious questions - focus on non-trivial design decisions

**Example Question Flow**:
- "How should cart data be persisted? (Redis sessions vs Database vs Hybrid approach)"
- [Wait for answer]
- "What should happen when a product in the cart goes out of stock?"
- [Wait for answer]
- "How long should cart data be retained? (Session-based, 30 days, 90 days?)"

### 2. Generate Documentation & GitHub Issue

Create specification document using the template structure, including decisions and rationale.

Then generate GitHub issue from the specification with:
- Acceptance criteria
- Test scenarios
- Implementation files needed
- Technical approach and tradeoffs

### 3. Follow TDD

Read `references/tdd_guide.md` and guide through Red-Green-Refactor cycle.

## Key Principles

- **Specificity over ambiguity**: Concrete requirements only
- **Document the "why"**: Rationale is crucial
- **Test-first approach**: Write tests before implementation
- **Language agnostic**: Works with any tech stack
- **Interactive interviews**: Use AskUserQuestionTool for one question at a time

## References

- `references/spec_template.md` - Specification interview template
- `references/tdd_guide.md` - TDD workflow guide
