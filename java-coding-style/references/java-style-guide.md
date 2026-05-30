# Java Style Guide

Use this reference after inspecting the target repository. These are defaults for the user's Java work, not a reason to fight established project conventions.

## Priority Order

1. Current user instruction in the active conversation.
2. Existing repository conventions and framework choices.
3. Rules in `User Overrides`.
4. Defaults in this guide.

If a rule cannot be applied cleanly, keep the repository consistent and mention the reason.

## User Overrides

Add confirmed personal preferences here as the user gives feedback.

- No confirmed overrides yet.

## Project Discovery Checklist

Before making a style-sensitive change, inspect:

- Build system and Java version: `pom.xml`, `build.gradle`, `.java-version`, toolchains.
- Format and static checks: Checkstyle, Spotless, PMD, Error Prone, IDE formatter files.
- Framework style: Spring MVC/WebFlux, MyBatis/MyBatis-Plus, JPA, MapStruct, Lombok, validation, logging.
- Existing layer patterns: controller/service/repository/mapper/entity/DTO/converter naming and package layout.
- Test conventions: JUnit version, Mockito style, Spring test slices, fixture builders, naming.

## Naming

- Use names from the business domain over technical filler.
- Prefer precise verbs: `createOrder`, `cancelOrder`, `calculatePayableAmount`, `findActiveUsers`.
- Avoid vague suffixes unless they are established locally: `Manager`, `Processor`, `Helper`, `Util`, `Data`, `Info`.
- Boolean names should read as predicates: `isEnabled`, `hasPermission`, `canRetry`, `shouldNotify`.
- Collection names should be plural or purpose-specific: `orders`, `enabledUserIds`, `ordersById`.
- Constants use `UPPER_SNAKE_CASE`; classes use `UpperCamelCase`; methods, fields, and variables use `lowerCamelCase`.

## Method Shape

- Keep methods small enough to understand without scrolling through unrelated concerns.
- Prefer guard clauses for preconditions, empty input, unsupported state, and authorization failures.
- Keep the main business path visually obvious.
- Extract helper methods only when the helper has a clear name and reusable concept, or when extraction removes meaningful branching noise.
- Avoid boolean parameters that change behavior substantially. Prefer separate methods, an enum, or a small command/request object.
- Do not introduce a new abstraction for a single call site unless it matches an existing project pattern or protects a real boundary.

## Data And Null Handling

- Treat external inputs as untrusted at boundaries: controller request bodies, RPC messages, queue payloads, file rows, and database records from legacy tables.
- Return empty collections instead of `null`.
- Use `Optional` mainly as a return type for maybe-present results. Avoid `Optional` fields, parameters, DTO properties, and serialization surfaces unless the project already does this.
- Prefer immutable local values where it improves clarity. Do not add `final` to every local variable unless the repository already follows that style.
- Make default values explicit and close to the boundary where they are chosen.

## Collections And Streams

- Use streams for linear transformations that stay readable in one expression.
- Use loops when the logic includes complex branching, multiple side effects, checked exceptions, early exits, or debug-friendly intermediate state.
- Prefer `List.of`, `Set.of`, `Map.of`, `Collections.emptyList`, or project helpers where supported by the Java version.
- Avoid repeated linear scans in hot or obviously large paths; build a map/set once with a name that explains the lookup.

## Exceptions

- Throw domain-specific or project-standard exceptions at service boundaries.
- Include useful context in exception messages, but never secrets, tokens, passwords, or full personal data.
- Do not catch `Exception` broadly unless translating at a boundary, cleaning up resources, or adding context before rethrowing.
- Avoid returning magic values to represent failures when the project has an exception/result convention.

## Logging

- Use the repository's logger style. In Lombok projects, prefer the established annotation such as `@Slf4j`.
- Log state transitions and important external calls with stable identifiers.
- Avoid logs inside tight loops unless sampled or clearly needed.
- Do not log sensitive values. Prefer IDs, counts, statuses, and sanitized descriptions.
- Avoid both "log and throw" and swallowed exceptions unless the project explicitly expects it.

## Spring Boot

- Keep controllers focused on HTTP concerns: request binding, validation annotations, authentication context extraction, and response mapping.
- Keep business rules in services. Keep persistence query construction in repositories/mappers or query services.
- Prefer constructor injection. If Lombok is already used, `@RequiredArgsConstructor` is acceptable.
- Put `@Transactional` on service methods that define a consistency boundary. Use `readOnly = true` for read-only service operations when the project uses it.
- Validate request DTOs with Bean Validation where available; keep cross-field and business validation in service/domain code.
- Keep API DTOs separate from persistence entities unless the existing project deliberately exposes entities.
- Use central exception handling if the project has `@ControllerAdvice` or a standard error response model.

## Persistence

- Avoid N+1 queries. When loading related data, check existing fetch/join/batch conventions.
- Use pagination for list endpoints that can grow.
- Keep query criteria explicit and named; avoid embedding business rules only in mapper XML or annotations when service readability suffers.
- Do not use `select *` in handwritten SQL unless the project standard does.
- For updates/deletes, make the criteria safe and obvious. Include tenant/user scope when the domain requires it.
- For MyBatis/MyBatis-Plus, follow the existing wrapper/lambda/query object style; keep mapper names aligned with entity or aggregate names.

## DTOs And Mapping

- Use request/response DTOs with names tied to use cases: `CreateOrderRequest`, `OrderDetailResponse`.
- Keep conversion logic in a mapper/converter/assembler when mapping is reused or non-trivial.
- Inline mapping is acceptable for tiny, local transformations if the repository commonly does that.
- Do not leak internal persistence fields through API responses unless the contract requires them.

## Tests

- Match the project's test stack and naming.
- Prefer one behavior per test. Names should describe the condition and expected outcome.
- Use arrange/act/assert spacing or comments only when it improves readability.
- Test meaningful branches: success, validation failure, permission/state failure, empty result, and important exception translation.
- Avoid over-mocking simple value objects or pure functions.
- Prefer realistic fixture builders or local helper methods over large copy-pasted setup blocks.
- For bug fixes, add a regression test that fails without the fix when practical.

## Review Checklist

Use this list before finishing:

- Does the change match nearby naming, annotations, formatting, and package layout?
- Is the business rule located at the right layer?
- Are null, empty, and invalid states handled deliberately?
- Are exceptions and logs useful without exposing sensitive data?
- Are persistence queries safe for growth, tenancy, and N+1 behavior?
- Are tests focused on behavior and aligned with the existing test framework?
- Did verification run, or is the reason for not running it clear?
