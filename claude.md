# Django Framework

## Project Overview

Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design. This is the **core Django framework repository**, not a Django application.

- **Python Version**: Requires Python >= 3.12
- **Main Dependencies**: asgiref>=3.9.1, sqlparse>=0.5.0
- **Documentation**: https://docs.djangoproject.com/
- **Issue Tracker**: https://code.djangoproject.com/ (Trac, not GitHub Issues)

## Important Contribution Requirements

**CRITICAL**: Django uses Trac for issue tracking, not GitHub Issues. Non-trivial pull requests (anything more than fixing a typo) **must** have an associated Trac ticket or they will be closed.

- File a ticket: https://code.djangoproject.com/newticket
- Always reference the ticket number in commits and PRs
- Follow the contribution guidelines: https://docs.djangoproject.com/en/dev/internals/contributing/

## Project Structure

```
django/
├── django/              # Main Django package
│   ├── contrib/        # Optional "contrib" packages (admin, auth, etc.)
│   ├── core/          # Core functionality (management, mail, cache, etc.)
│   ├── db/            # Database backends and ORM
│   ├── forms/         # Form handling
│   ├── http/          # HTTP request/response
│   ├── template/      # Template engine
│   ├── urls/          # URL routing
│   └── views/         # View classes
├── tests/             # Django's comprehensive test suite
├── docs/              # Documentation (reStructuredText)
└── extras/            # Extra contributed files
```

## Code Style Guidelines

### Python Style

- **Formatter**: Use Black (line length: 88 characters for code)
- **Linter**: Use flake8 (configured in `.flake8`)
- **Import Sorter**: Use isort with Black profile
- **Docstrings/Comments**: Max 79 characters
- **Pre-commit**: Run `pre-commit install` to enable automated checks

### Key Conventions

1. **Naming**:
   - Variables/functions/methods: `snake_case`
   - Classes: `InitialCaps`
   - Model fields: `lowercase_with_underscores`
   - Constants: `ALL_CAPS`

2. **String Formatting**:
   - Prefer f-strings for simple variable interpolation
   - Use `str.format()` or `%` for translatable strings
   - f-strings should only contain simple variable/property access
   - Avoid complex expressions in f-strings

3. **Imports** (automatically handled by isort):
   - Group order: future → stdlib → third-party → Django → local → try/except
   - Sort alphabetically within each group
   - Use absolute imports for Django components
   - Use relative imports (`.foo`) for local components only

4. **Model Conventions**:
   - Field definitions first
   - Then custom manager attributes
   - Then `class Meta`
   - Then `__str__()` and magic methods
   - Then `save()`, `get_absolute_url()`
   - Then custom methods

5. **View Conventions**:
   - First parameter must be named `request`

6. **Settings Usage**:
   - Never access `django.conf.settings` at module level
   - Use lazy evaluation to avoid premature configuration

### Template Style

- `{% extends %}` must be first non-comment line
- Exactly one space inside `{{ }}` and `{% %}`
- Put block names in `{% endblock %}` tags
- Alphabetize template tag libraries in `{% load %}`

## Testing

### Running Tests

```bash
# Quick start - run all tests
cd tests
python -m pip install -e ..
python -m pip install -r requirements/py3.txt
./runtests.py

# Run specific test module
./runtests.py <test_module>

# Using tox (recommended for PR validation)
pip install tox
tox
```

### Test Conventions

- Use `assertRaisesMessage()` / `assertWarnsMessage()` instead of `assertRaises()` / `assertWarns()`
- Use `assertIs(..., True/False)` for booleans, not `assertTrue()` / `assertFalse()`
- Test docstrings should state expected behavior without preambles ("Tests that...")
- Include ticket numbers at end of docstring when relevant: `(#12345)`

## Making Changes for Small Improvements

### What Makes a Good Small Improvement

1. **Bug Fixes**: Clear, reproducible bugs with minimal scope
2. **Documentation Improvements**: Typos, clarifications, examples
3. **Code Quality**: Remove unused imports, fix style inconsistencies
4. **Test Coverage**: Add missing tests for existing functionality
5. **Performance**: Small, measurable optimizations
6. **Deprecation Cleanups**: Remove code marked for removal

### Before Making Changes

1. **Search for existing tickets**: Check if issue already reported
2. **Read surrounding code**: Understand context and patterns
3. **Check deprecation policy**: Don't remove deprecated features prematurely
4. **Run relevant tests**: Ensure changes don't break existing functionality

### Code Quality Checks

```bash
# Format code
black .

# Sort imports
isort .

# Check style
flake8

# Or use pre-commit for all checks
pre-commit run --all-files
```

### Anti-Patterns to Avoid

- **Over-engineering**: Don't add unnecessary abstractions or configurability
- **Scope creep**: Fix only what's needed, don't refactor surrounding code
- **Breaking changes**: Maintain backwards compatibility
- **Premature optimization**: Only optimize with benchmarks
- **Unnecessary comments**: Code should be self-documenting
- **Adding your name**: Contributors are tracked in AUTHORS file only

### Common Patterns in Django Codebase

1. **Lazy evaluation**: Use `LazyObject`, `lazy()`, or lambda for deferred execution
2. **Settings access**: Always use lazy evaluation, never at module level
3. **Internationalization**: Mark all user-facing strings with translation functions
4. **Database independence**: Code must work with all supported databases
5. **Backwards compatibility**: Use deprecation warnings before removing features

## File Locations for Common Tasks

- **Admin UI**: `django/contrib/admin/`
- **Authentication**: `django/contrib/auth/`
- **Database backends**: `django/db/backends/`
- **Forms**: `django/forms/`
- **Migrations**: `django/db/migrations/`
- **ORM**: `django/db/models/`
- **Template engine**: `django/template/`
- **URL routing**: `django/urls/`
- **Views**: `django/views/`
- **Middleware**: `django/middleware/`
- **Management commands**: `django/core/management/`

## Documentation

- All docs use reStructuredText (.txt files in `docs/`)
- Build docs: `cd docs && make html`
- Spelling check: `tox -e docs`
- Max line length: 79 characters

## Git Workflow

1. Create feature branch from main
2. Make changes following style guide
3. Write/update tests
4. Run test suite: `./runtests.py`
5. Run code quality checks: `pre-commit run --all-files`
6. Commit with clear messages referencing Trac ticket
7. Push to your fork
8. Create PR referencing Trac ticket number

## Need Help?

- Discord: https://chat.djangoproject.com
- Forum: https://forum.djangoproject.com/
- Contributing Docs: https://docs.djangoproject.com/en/dev/internals/contributing/

## Quick Reference

```bash
# Install development environment
python -m pip install -e .
python -m pip install -r tests/requirements/py3.txt
pre-commit install

# Run tests
cd tests && ./runtests.py

# Run specific app tests
./runtests.py admin_views

# Format and check code
black .
isort .
flake8

# Build documentation
cd docs && make html
```

## Notes for AI Assistants

When proposing improvements:
- Respect the existing code style and patterns
- Keep changes minimal and focused
- Don't add features without a Trac ticket
- Don't refactor code unrelated to the fix
- Always run tests before proposing changes
- Consider backwards compatibility
- Check for similar patterns elsewhere in the codebase
- Remember: simplicity over cleverness
