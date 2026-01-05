# Criterion: API documentation

## Architecture Decision Record

### Status

**Status:** Accepted 

**Date:** [2026-01-05]

### Context

The project requires clear and complete API documentation so that system behavior and endpoints can be understood, tested, and evaluated without inspecting the source code.
AutoChess Classic is built as an API-only backend consisting of multiple Spring Boot microservices. This introduces the following forces and constraints:
- No traditional frontend exists; APIs are the primary interface
- Each microservice exposes its own endpoints and responsibilities
- Documentation must remain accurate as APIs evolve
- The committee must be able to test the system easily
- Manual documentation would be hard to maintain

### Decision

Swagger / OpenAPI (springdoc-openapi) is used to automatically generate and expose API documentation for each microservice.
Every service provides:
- A dedicated Swagger UI
- Full endpoint descriptions
- Request/response schemas
- Validation rules
Swagger is also used as the primary interaction tool for testing and demonstrating the system during development and defense.

### Alternatives Considered

| Alternative                   | Pros                    | Cons                         | Why Not Chosen               |
| ----------------------------- | ----------------------- | -----------------------------| -----------------------------|
| Manual Markdown documentation | Simple, no dependencies | Not testable                 | Not reliable for the project |
| Postman only                  | Good for testing        | Not documenting, no schemas  | Insufficient for evaluation  |
| Custom API documentation UI   | Great way to present    | Requires frontend            | Too much work, time limit    |


### Consequences

**Positive:**
- Always up-to-date documentation generated from code
- Clear contract between services and clients
- Easy manual testing via browser

**Negative:**
- Swagger UI exposes many technical details, can be a mess, create difficulties 

**Neutral:**
- Documentation is for the developer, not the end-user

## Implementation Details

### API Documentation Strategy

- Each microservice includes springdoc-openapi
- Swagger UI is enabled per service (e.g. /swagger-ui.html)
- Endpoints are documented using annotations:
@Operation
@ApiResponse
@Schema
- DTO validation annotations (@NotNull, @Size, etc.) are reflected automatically

### Key Implementation Decisions

| Decision                   | Rationale                           |
| -------------------------- | ----------------------------------- |
| Swagger/OpenAPI            | Industry standard, widely supported |
| One Swagger UI per service | Matches microservice boundaries     |
| Annotation-based docs      | Keeps documentation close to code   |
| Swagger as test interface  | No frontend required                |


### Code Examples
The following snippet: 
- Authenticates a user using email and password credentials.
- Returns JWT access and refresh tokens upon successful authentication.
- The endpoint is automatically documented using Swagger/OpenAPI annotations.

```Java
    @Operation(summary = "User login",
            description = "Authenticates a user and returns JWT tokens.")
    @PostMapping("/login")
    public ResponseEntity<AuthResponse> login(@Valid @RequestBody LoginRequest req) {
        return ResponseEntity.ok(auth.login(req));
    }
```


Next snippet is the security configuration that is used across all backend services.

It enforces stateless JWT-based authentication and allows access to Swagger/OpenAPI and Actuator endpoints.

``` Java
@Bean
SecurityFilterChain securityFilterChain(
        HttpSecurity http,
        JwtAuthenticationFilter jwtAuthenticationFilter
) throws Exception {

    http
        .csrf(csrf -> csrf.disable())
        .sessionManagement(sm ->
            sm.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
        )
        .authorizeHttpRequests(auth -> auth
            .requestMatchers(
                "/api/auth/**",
                "/actuator/**",
                "/v3/api-docs/**",
                "/swagger-ui/**",
                "/swagger-ui.html"
            ).permitAll()
            .anyRequest().authenticated()
        )
        .addFilterBefore(
            jwtAuthenticationFilter,
            UsernamePasswordAuthenticationFilter.class
        );

    return http.build();
}
```

### Diagrams

[API-Overview-Diagram](../../assets/diagrams/API-Overview-Diagram.png)


## Requirements Checklist

| # | Requirement                      | Status | Evidence/Notes                        |
| - | -------------------------------- | ------ | ------------------------------------- |
| 1 | API endpoints documented         | ✅      | Swagger UI available for all services |
| 2 | Request/response schemas defined | ✅      | DTOs reflected in OpenAPI specs       |
| 3 | Authentication documented        | ✅      | JWT requirements visible in Swagger   |
| 4 | Interactive API testing          | ✅      | Endpoints callable via Swagger UI     |


**Legend:**
- ✅ Fully implemented
- ⚠️ Partially implemented
- ❌ Not implemented

## Known Limitations

| Limitation              | Impact                     | Potential Solution       |
| ----------------------- | -------------------------- | ------------------------ |
| Public Swagger exposure | Potential security risk    | Restrict in production   |
| No front-end            | Not testable on UI         | Add front-end            |


## References

- Swagger / OpenAPI Documentation: (https://swagger.io/docs/)
- Springdoc OpenAPI: (https://springdoc.org/)
- Spring Boot REST Documentation: (https://docs.spring.io/spring-boot/docs/current/reference/html/web.html)
