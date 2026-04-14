# You are a assistant that implements what the user wants. However, in doing so, you must follow the following rules:


# Coding Style & Best Practices

## Type Hinting
- Use type hints for all new function signatures.
- Prefer `typing` module types when needed: `Optional`, `Dict`, `List`, `Callable`, `Tuple`, and similar.

```python
def process_players(self, raw_players: List[Dict], queue_id: str = "") -> List[Player]:
```

## Threading
- Never block the main thread with API calls or network I/O.
- Use background threads for slow work.
- Use `self.after(0, callback)` to safely update UI from background threads.
- Use `ThreadPoolExecutor` for bounded parallel operations where that pattern already exists.
- Remember API limitations and cooldowns when designing background tasks. Don't create a thread that will spam an endpoint.

## Constants & Configuration
- Never hardcode values directly in code. Create named constants and place them in a central file. 

```python
# BAD
time.sleep(5)

# GOOD (in config.py)
REFRESH_COOLDOWN_SECONDS = 5
time.sleep(REFRESH_COOLDOWN_SECONDS)
```

## Code Organization
- Keep files focused. If a file grows too large, consider extracting cohesive helpers or components.
- Reuse existing classes if possible.
- Services should not import UI code.
- UI may use services and models but should not call raw API endpoints directly.
- Prioritize cohesion and decoupling over strict layering when it makes the code cleaner and more maintainable.
- Can the human easily read and understand the code? If not, consider refactoring for clarity.
- follow standard project structure and naming conventions to make it easy for other developers to navigate the codebase.

## Error Handling
- Wrap API calls in try/except blocks.
- Try to make descriptive log messages when exceptions occur, but only when it is enabled in config. Ask yourself what would I want to know if I were debugging this issue?
- Never let exceptions crash the main thread; surface graceful error states in the UI.

## Docstrings
- Public functions and classes should have docstrings.
- Keep simple docstrings short; use Args/Returns for more complex functions.

```python
def get_cooldown_remaining(self) -> float:
    """Get remaining cooldown time in seconds."""
```

## 2. Security & Privacy
- Treat the client as a potentially hostile environment. 
- Always validate and sanitize user input before using it in API calls or database queries.
- Never log sensitive information such as API keys, access tokens, or personally identifiable information.
- Do not expose sensitive information in error messages or stack traces that could be seen by end users.
- Use secure storage mechanisms for any sensitive data that must be stored, such as encrypted files or secure vaults.
- Regularly review and update dependencies to patch known security vulnerabilities.
- Follow the principle of least privilege when granting permissions to services and APIs. Only give access to what is necessary for the functionality.
- Be mindful of the CIA triad (Confidentiality, Integrity, Availability) when designing features and handling data. Ensure that data is protected from unauthorized access, maintain data integrity, and design for high availability to prevent service disruptions.

## 3. Performance & Efficiency
- Avoid unnecessary API calls. Cache results when appropriate and implement cooldowns to prevent spamming endpoints.
- Use efficient data structures and algorithms to minimize memory usage and improve performance.
- Profile and optimize code that is identified as a bottleneck.
- Be realistic about performance expectations. Some operations, like loading match details, will inherently take time due to external factors. Focus on providing good feedback to the user during these operations rather than trying to make them instantaneous.
- Use asynchronous programming techniques where appropriate to keep the UI responsive during long-running operations.

## 4. User Experience
- Provide clear feedback to the user during loading states, errors, and successful operations.
- Design the UI to be intuitive and easy to navigate. Follow common design patterns and conventions.
- Ensure that error messages are user-friendly and provide actionable information when possible.
- Consider edge cases and how the application should behave in unexpected scenarios (e.g., network failures, API rate limits, invalid input).

## 5. Testing
- Write unit tests for critical functions and components to ensure they work as expected.
- Create a debug mode that can be enabled in config to provide additional logging and information for troubleshooting issues.
- Run test every time you make significant changes to the codebase to catch regressions early.
- Make test as reflective of real-world usage as possible, including edge cases and error conditions.

6. AGENT-Specific Guidelines
- To reduce token consumption and making it hard to understand, create a markdown file that contains the context and features of the codebase. This file should be updated as the codebase evolves and should be used as a reference for the assistant when implementing features or making changes to the codebase.

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

#### Tech Stack
- Use the best tools and languages for the job. For example, Python is a good choice for backend services and scripting, while JavaScript/TypeScript is ideal for frontend development. Use libraries and frameworks that are well-maintained and widely adopted in the community.