# GitHub Copilot Instructions

## Core Principle: Ensure High Quality

All code, documentation, and contributions to this repository must meet high quality standards. Quality is non-negotiable and should be considered in every decision.

## Code Quality Standards

### General Principles
- Write clean, readable, and maintainable code
- Follow language-specific best practices and idioms
- Prefer simplicity over cleverness
- Use meaningful and descriptive names for variables, functions, and classes
- Keep functions focused and single-purpose
- Avoid code duplication (DRY principle)

### Code Review Checklist
Before suggesting or committing code, verify:
- ✅ Code is properly formatted and follows project style guidelines
- ✅ No hardcoded values (use constants or configuration)
- ✅ Error handling is comprehensive and appropriate
- ✅ Edge cases are considered and handled
- ✅ Performance implications are understood
- ✅ Security best practices are followed
- ✅ Code is testable and includes appropriate tests

## Testing Requirements

### Test Coverage
- All new features must include unit tests
- Bug fixes must include regression tests
- Aim for meaningful test coverage, not just percentage metrics
- Test both happy paths and error conditions

### Test Quality
- Tests should be clear, focused, and independent
- Use descriptive test names that explain what is being tested
- Avoid brittle tests that break with minor refactoring
- Mock external dependencies appropriately

## Documentation Standards

### Code Documentation
- Document public APIs, complex algorithms, and non-obvious logic
- Use clear, concise comments that explain "why", not "what"
- Keep documentation up-to-date with code changes
- Include usage examples for public interfaces

### Project Documentation
- README should be clear and complete
- Document setup, build, and deployment procedures
- Maintain a changelog for significant changes
- Document architectural decisions and trade-offs

## Security Considerations

- Never commit secrets, credentials, or sensitive data
- Validate and sanitize all external inputs
- Follow principle of least privilege
- Keep dependencies up-to-date and scan for vulnerabilities
- Use secure coding practices appropriate for the language

## Performance

- Consider performance implications of implementation choices
- Profile before optimizing (avoid premature optimization)
- Document performance requirements and benchmarks
- Test performance for critical paths

## Error Handling

- Handle errors gracefully with clear, actionable messages
- Log errors with sufficient context for debugging
- Don't swallow exceptions without proper handling
- Provide meaningful feedback to users

## Version Control

- Write clear, descriptive commit messages
- Keep commits atomic and focused
- Reference related issues in commit messages
- Review changes before committing

## Before Submitting

Every contribution should:
1. Build successfully without warnings
2. Pass all existing tests
3. Include tests for new functionality
4. Be properly documented
5. Follow the repository's coding standards
6. Be reviewed for security implications
7. Have clear, descriptive commit messages

---

**Remember**: High quality code is not just about functionality—it's about maintainability, reliability, security, and user experience. Take the time to do it right.
