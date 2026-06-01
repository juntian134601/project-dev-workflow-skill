---
name: project-dev-workflow
description: |
  This skill should be used when the user wants to develop or modify a software project
  with a structured, step-by-step AI-assisted workflow. It covers project analysis,
  specification writing, implementation planning, and incremental coding — ensuring
  systematic and controlled development without premature coding or uncontrolled refactoring.
  Trigger on phrases like: "帮我开发这个项目", "开始一个新功能", "按流程开发",
  "分析项目并编写spec", "AI开发流程", "结构化开发".
agent_created: true
---

# AI-Assisted Project Development Workflow

A disciplined, multi-step workflow for AI-assisted software development. Follow these
steps in order. Do not skip ahead. Do not start coding before completing the analysis
and specification phases.

## Overview

This workflow divides project development into 7 sequential steps. Each step has clear
inputs, outputs, and constraints. The key principle: **think before coding**.

```
Step 1: Understand Requirements → Step 2: Decompose Tasks
    ↓
Step 3: Analyze Project → Step 4: Write Spec
    ↓
Step 5: Review Spec → Step 6: Implementation Plan
    ↓
Step 7: Incremental Coding
```

---

## Step 1: Understand Requirements

**When to start:** User describes a new feature, project, or modification request.

**Objective:** Fully understand what the user wants to build before any technical work.

**Actions:**
- Ask clarifying questions about:
  - Business goal and user value
  - Target users and use cases
  - Must-have vs nice-to-have features
  - Constraints (time, budget, technology, compliance)
  - Integration points with existing systems
- Summarize requirements in user's own terms for confirmation
- Identify the smallest viable scope for the first iteration

**Output:** A concise requirements summary agreed upon with the user.

**Constraint:** Do not propose technical solutions yet. Focus on "what" not "how".

---

## Step 2: Decompose Tasks

**When to start:** Requirements are clear and confirmed.

**Objective:** Break the project into logical, independent tasks/modules.

**Actions:**
- Identify major functional modules or components
- Define boundaries and interfaces between modules
- Estimate relative complexity of each task
- Identify dependencies (which tasks must be completed before others)
- Flag any tasks requiring research or spike investigations

**Output:** A task decomposition list with dependencies noted.

**Constraint:** Keep decomposition at a high level. Do not design APIs or data models yet.

---

## Step 3: Analyze Project (CRITICAL - Always First Technical Step)

**When to start:** Before writing any spec or code for an existing project.

**Objective:** Understand the current codebase thoroughly without modifying anything.

**Input prompt:**
```
请先分析整个项目：

1. 技术栈
2. 目录结构
3. 状态管理
4. API 层
5. UI 体系
6. 权限体系
7. 路由结构
8. 构建体系
9. 代码规范

不要修改代码。
输出完整分析报告。
这是使用第一步。
```

**Actions:**
- Read key configuration files (package.json, vite.config, tsconfig, etc.)
- Examine directory structure and naming conventions
- Identify state management approach (Redux, Pinia, Vuex, Context API, etc.)
- Analyze API layer patterns (REST/GraphQL/gRPC, fetch/axios, service layer structure)
- Review UI component architecture and design system usage
- Understand permission/role system implementation
- Map routing structure and conventions
- Document build toolchain and CI/CD setup
- Note code style (lint rules, formatting, naming conventions)

**Output:** A comprehensive markdown analysis report covering all 9 areas.

**Constraint:** READ-ONLY. Do not create, modify, or delete any files during analysis.

---

## Step 4: Write Spec

**When to start:** After project analysis (Step 3) or for greenfield projects after Step 2.

**Objective:** Produce a detailed technical specification document before any coding.

**Input prompt template:**
```
请先为"[功能名称]"编写完整 Spec。

包括：
1. 功能目标
2. 用户角色
3. 权限模型
4. 页面结构
5. API 设计
6. 数据结构
7. 路由权限
8. 风险点
9. 实施步骤

不要开始编码。
输出 markdown 文档。
```

**Actions:**
- Define clear, testable functional goals
- Enumerate user roles and their permissions
- Design the permission model (RBAC/ABAC/ACL) with specific rules
- Design page/component structure with navigation flow
- Design API contracts (endpoints, request/response schemas, error codes)
- Design data models (database schema, DTOs, state shape)
- Define route-level access control
- Identify technical risks and mitigation strategies
- Break down into concrete implementation steps

**Output:** A complete Spec document in markdown format.

**Constraint:** NO CODE. Only design documents, schemas, and plans.

---

## Step 5: Review Spec

**When to start:** Spec document is complete.

**Objective:** Validate the spec with the user and identify gaps or issues.

**Actions:**
- Present the spec to the user for review
- Ask specific questions about edge cases and business rules
- Identify potential conflicts with existing architecture
- Check for missing error handling or boundary conditions
- Confirm the scope is realistic for the intended timeline

**Output:** User-approved spec, possibly with revision notes.

**Constraint:** Do not proceed to implementation planning until spec is confirmed.

---

## Step 6: Implementation Plan

**When to start:** Spec is reviewed and approved.

**Objective:** Create a detailed, phased implementation plan.

**Input prompt:**
```
根据 spec：

请输出实施计划。

要求：
1. 分阶段实施
2. 每阶段列出文件
3. 标注风险
4. 标注影响范围
5. 不要开始编码
```

**Actions:**
- Divide implementation into logical phases (typically 3-5 phases)
- For each phase, list:
  - Specific files to create/modify/delete
  - Estimated complexity
  - Dependencies on previous phases
  - Technical risks and mitigation
  - Impact scope (which modules are affected)
  - Testing approach
- Identify any prerequisite setup (new dependencies, config changes, migrations)
- Ensure each phase is independently reviewable and testable

**Output:** A phased implementation plan document.

**Constraint:** NO CODE. Only planning documents.

---

## Step 7: Incremental Coding (AI Development Core)

**When to start:** Implementation plan is approved.

**Objective:** Execute the plan phase by phase with strict discipline.

**Input prompt:**
```
开始第一阶段实施。

要求：
1. 每次改动不要超过 5 个文件
2. 保持现有代码风格
3. 每完成一步先说明
4. 不要擅自重构
5. 不要修改无关模块
```

**Actions:**
- Execute exactly one phase at a time
- Before making changes, explain what will be done and why
- After each change, summarize what was done
- Follow existing code style and patterns precisely
- Make minimal, focused changes — resist the urge to "clean up" nearby code
- If a bug or issue is found in unrelated code, note it but do not fix it (create a separate task)
- Run tests (if available) after each phase
- Ask for user confirmation before proceeding to the next phase

**Constraints (STRICT):**
1. **Max 5 files per change batch** — if more are needed, split into sub-steps
2. **Preserve existing style** — match formatting, naming, and patterns exactly
3. **Explain before and after** — never make silent changes
4. **No unauthorized refactoring** — do not rename, reorganize, or rewrite working code unless it's in the spec
5. **No cross-module changes** — stay within the scope of the current phase

---

## Usage Patterns

### Pattern A: New Feature on Existing Project

1. User: "帮我添加一个RBAC权限系统"
2. Execute Step 3 (Analyze Project)
3. Execute Step 4 (Write Spec for "RBAC权限系统")
4. Execute Step 5 (Review Spec)
5. Execute Step 6 (Implementation Plan)
6. Execute Step 7 (Incremental Coding phase by phase)

### Pattern B: Greenfield Project

1. User: "帮我从零开发一个XX系统"
2. Execute Step 1 (Understand Requirements)
3. Execute Step 2 (Decompose Tasks)
4. Skip Step 3 (no existing project)
5. Execute Step 4 (Write Spec)
6. Execute Step 5 (Review Spec)
7. Execute Step 6 (Implementation Plan)
8. Execute Step 7 (Incremental Coding)

### Pattern C: Quick Fix/Modification

1. User: "修复这个bug" or "改一个小功能"
2. Execute Step 3-lite: Quick project context gathering (targeted reading)
3. Proceed directly to Step 7 with a single focused change

---

## Key Principles

1. **Analysis before design** — Never write spec without understanding the codebase
2. **Design before code** — Never write code without a written spec
3. **Plan before execute** — Never start coding without an implementation plan
4. **Small batches** — Never change more than 5 files at once
5. **Minimal surprise** — Never refactor or modify unrelated code
6. **Explain everything** — Every action must be preceded and followed by explanation
