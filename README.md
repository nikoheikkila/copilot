# Modern Software Engineering with GitHub Copilot

Custom Copilot agents, instructions, and prompt files.

This repository contains specialized GitHub Copilot agents that guide you through the Test-Driven Development (TDD) cycle. These agents follow the Red-Green-Refactor workflow and are designed to work with Python projects using PyTest and Gherkin/BDD feature files.

## What are GitHub Copilot Agents?

GitHub Copilot agents are AI assistants that provide specialized guidance for specific development tasks. They live in the `.github/agents/` directory of your repository and can be invoked directly in VS Code through GitHub Copilot Chat.

## Available Agents

### 1. [TDD-Red](./.github/agents/tdd_red.agent.md)

**Purpose**: Write failing tests that describe desired behavior before implementation exists.

**When to use**: Start here when beginning a new feature. This agent helps you translate Gherkin (BDD) feature files into PyTest tests.

**How to use**:

1. Open VS Code and ensure you have GitHub Copilot and GitHub Copilot Chat extensions installed
2. Create or open a Gherkin feature file (e.g., `features/authentication.feature`)
3. Open the Copilot Chat panel (Ctrl/Cmd + Shift + I)
4. Write your prompt
5. Example: `Write failing tests for the authentication feature`

**What it does**:

- Reads your Gherkin feature files from the `features/` directory
- Translates scenarios into PyTest tests following Given-When-Then structure
- Ensures tests fail for the right reason (missing implementation, not syntax errors)
- Uses `assertpy` library for readable assertions
- Creates isolated, independent tests with proper fixtures

**Key principles**:

- Write one test at a time
- Focus on specific behavior from feature requirements
- Use descriptive test names like `test_user_can_sign_in_with_valid_credentials()`
- Prefer real objects; only fake external dependencies (databases, APIs, file systems)

### 2. [TDD-Green](./.github/agents/tdd_green.agent.md)

**Purpose**: Implement minimal code to make failing tests pass without over-engineering.

**When to use**: After you have failing tests from the Red phase. This agent helps you write just enough code to make tests pass.

**How to use**:

1. Ensure you have failing tests in your `tests/` directory
2. Open the Copilot Chat panel
3. Write your prompt
4. Example: `Make unit tests pass in this file`

**What it does**:

- Writes minimal implementation code to satisfy test requirements
- Never modifies existing tests
- Uses simple, straightforward solutions (no premature optimization)
- Follows the Transformation Priority Premise (starts with constants, then conditionals, then loops)
- Ensures tests pass quickly without adding unnecessary complexity

**Key principles**:

- Fake it till you make it (start with hard-coded returns, generalize later)
- Ignore code smells temporarily (duplication is acceptable)
- Use the simplest language features (basic if/else, for/while)
- No logging, comments, type hints, or extra validation
- Run tests after each small change

### 3. [TDD-Refactor](./.github/agents/tdd_refactor.agent.md)

**Purpose**: Improve code quality, apply best practices, and enhance design while maintaining green tests.

**When to use**: After all tests are passing from the Green phase. This agent helps you clean up the code without breaking functionality.

**How to use**:

1. Ensure all tests are passing (run `task test`)
2. Open the Copilot Chat panel
3. Write your prompt
4. Example: `Improve the authentication code design`

**What it does**:

- Removes code duplication following the "Rule of Three"
- Applies SOLID principles
- Improves readability with intention-revealing names
- Keeps classes under 100 lines and methods under 10 lines
- Uses Pydantic models for data structures
- Maintains test coverage and ensures tests stay green
- Runs linters and static analysis

**Key principles**:

- Make small, incremental changes
- Run tests after every change
- Never modify test code during refactoring
- Use proven refactoring patterns (Extract Method, Extract Class, Rename, etc.)
- Document WHY, not WHAT
- Revert if tests break or complexity increases

### Complete TDD Workflow

Here's how to use all three agents together:

```text
1. RED Phase
   └─> Write tests for login feature
       ├─> Creates failing PyTest tests from Gherkin scenarios
       └─> Verify tests fail: `task test`
       └─> Handoff to the TDD-Green agent

2. GREEN Phase
   └─> Implement code to pass login tests
       ├─> Writes minimal implementation
       └─> Verify tests pass: `task test`
       └─> Handoff to the TDD-Refactor agent

3. REFACTOR Phase
   └─> Refactor the login code
       ├─> Improves design and readability
       └─> Verify tests still pass: `task test`
       └─> Handoff to the TDD-Red agent

4. Repeat RED Phase
   ...
```

### Prerequisites

**VS Code** with the following extensions:

- GitHub Copilot
- GitHub Copilot Chat

**Python** project with:

- PyTest test runner
- `assertpy` library for assertions
- Gherkin feature files (optional, but recommended)
- Task runner configured (for `task test` command)

### Tips for Best Results

1. **Be specific in your prompts**: Instead of "write tests", say "write tests for user authentication with valid credentials".
2. **Work one phase at a time**: Don't skip ahead; the agents are designed to work sequentially.
3. **Reference files**: Mention specific feature files or test files in your prompts with <kbd>#</kbd>.
4. **Review agent output**: These agents provide guidance, but you should review and validate all generated code. Do not blindly accept suggestions and accumulate comprehension debt.
5. **Keep tests isolated**: Each test should be independent and not rely on shared state.
6. **Run tests frequently**: Verify your tests after each change with `task test`.
7. **Do not let agents to use Git**: Forbid agents to make commits, branches, or any other Git operations. You are still and always will be the navigator while the agents are simple drivers.
