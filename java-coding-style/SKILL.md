---
name: Java开发规范
description: 在编写、重构、评审或测试 Java 代码时，需遵循Java编码规范，尤其针对 Spring Boot/Spring Cloud 项目、服务层 / 控制层 / 数据访问层、数据传输对象、参数校验、异常处理、日志打印、数据持久化及单元测试场景。当要求按照Java开发规范编码、减少代码返工、调整实现细节或遵循 Java 项目规范时，均需执行该要求。
allowed-tools: 
  - Read
  - Grep
  - Glob
---
# Java开发规范

以下是我的Java开发规范。在用Java做需求开发时，必须始终遵循这些约定。

## 速查手册


| 开发规范类型 | 适用场景                           | 参考手册                   |
| ------------ | ---------------------------------- | -------------------------- |
| maven项目    | 新建Java项目,模块,包               | reference/maven-project.md |
| 命名风格     | 项目/模块/包/类/方法/变量/常量命名 |                            |
| 接口设计     | 新建http,rpc接口                   |                            |
| rocketmq使用 |                                    |                            |
| kafka使用    |                                    |                            |

## Core Rule

Prefer the target repository's existing conventions first, the user's explicit request second, and this skill's defaults third. When those conflict, explain the tradeoff briefly and choose the least surprising option for the repository.

## Workflow

1. Inspect nearby Java files before editing: naming, package layout, annotations, Lombok usage, error handling, logging, test style, mapper/repository style, and formatting tools.
2. Read `references/java-style-guide.md` when implementing non-trivial Java changes, refactors, reviews, tests, or Spring features.
3. Keep changes narrow. Avoid unrelated rewrites, broad abstractions, and cosmetic churn outside the touched behavior.
4. Implement production code and tests in the same local style. Prefer fixing the design at the layer boundary over adding patches deep inside helpers.
5. Run the project's normal verification command when practical. If no clear command exists, inspect build files and run the smallest relevant test set.

## Default Implementation Preferences

- Use clear, domain-oriented names; avoid vague names such as `data`, `info`, `handler`, `process`, or `doSomething` unless the surrounding code already standardizes them.
- Keep controllers thin, services business-focused, repositories/mappers persistence-focused, and converters/assemblers explicit.
- Prefer guard clauses for invalid or empty cases; keep the happy path easy to read.
- Make null handling deliberate. Return empty collections instead of `null`; validate API inputs at the boundary; avoid passing nullable values through several layers without naming that contract.
- Use streams for simple mapping/filtering/collecting. Use loops when branching, mutation, exception handling, or debugging clarity matters.
- Use constructor injection in Spring code. Use Lombok only when the project already uses it.
- Put transactions on service methods that define business consistency, not on controllers or low-level utility methods.
- Log actionable context without secrets or noisy stack traces. Avoid swallowing exceptions.
- In tests, prefer behavior-focused cases with readable arrange/act/assert structure and realistic fixtures.

## Updating This Skill

When the user corrects a style choice, add the new rule to `references/java-style-guide.md` under `User Overrides`. Keep overrides specific and example-driven so future agents can apply them without guessing.

