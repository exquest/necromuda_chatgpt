Necromunda-Inspired Skirmish Wargame Simulator

Project Description

This project is a web-based tabletop skirmish wargame simulator inspired by Necromunda, where players control gangs of fighters in turn-based combat. The goal is to create a full game engine with modules for gang creation, persistent campaign tracking, tactical combat resolution, equipment management, and territory control. The simulator will run on a MacBook Pro (macOS) and be accessible via a web browser, powered by Python on a Conda environment. The backend will use either Flask or Django (to be decided), providing a RESTful API or web interface for game interactions. The AI coding assistant (GPT Codex) will help develop the project by generating modular, testable Python code and offering guidance on game logic and rules. Future phases may introduce AI-controlled gangs for solo play. All development should emphasize maintainability and clarity, following the guidelines below.

Development Environment Setup

Ensure the development environment is reproducible and well-documented, using explicit commands for setup ￼. Use Conda for Python environment management and pip for package installation:
	•	Python & Conda: Use a compatible Python 3 version (e.g. 3.10+). Create a Conda environment and activate it:

conda create -n necro_sim python=3.10 -y
conda activate necro_sim


	•	Dependencies: Maintain a requirements.txt file for Python dependencies. Install packages with pip:

pip install -r requirements.txt

Include either Flask or Django (but not both) in this file, depending on the chosen framework. For example:
	•	If using Flask: Flask (and possibly Flask-SQLAlchemy for database, etc.)
	•	If using Django: Django (and possibly djangorestframework for API support).
Also list any other libraries (e.g., SQLAlchemy or Django ORM (built-in), pytest for testing, black for code formatting, etc.). Use pinned versions for reproducibility.

	•	Initial Project Structure: After installing dependencies, set up the project structure. For Flask, create a file like app.py or wsgi.py and organize code into modules (e.g. gangs.py, combat.py, etc.) or Blueprints. For Django, use the django-admin startproject and startapp commands to create a project and apps (e.g. an app for gang management, one for combat).
	•	Running the Application: Provide clear commands to run the development server:
	•	Flask: Set the Flask app environment variable and run:

export FLASK_APP=app.py 
flask run

(Adjust FLASK_APP to your main application entry-point as needed.)

	•	Django: Apply migrations and start the server:

python manage.py migrate
python manage.py runserver


	•	Database Setup: Use a lightweight database for development (SQLite by default). For Flask, configure SQLAlchemy (e.g. database URI in an environment variable or config file). For Django, the default SQLite settings suffice for dev. Ensure database initialization (and migrations for Django) are documented.
	•	Environment Variables: Use a .env file or environment variables for configuration (like database URLs, secret keys, etc.). Do not hard-code secrets. Document any required env vars in the setup instructions.
	•	Dev Tools: It’s recommended to use development tools for code quality: e.g., black for formatting, flake8 for linting, isort for import sorting. These can be installed as dev dependencies and run via pre-commit hooks or CI. For example, run black . and flake8 to format code and check style before commits.

Coding Style

All code should follow Python best practices for readability and consistency. We adhere to PEP 8 style guidelines (4-space indentation, descriptive naming, 79 character line length, etc.) since PEP 8 is widely adopted in the Python community ￼. Key style rules and conventions include:
	•	General Formatting: Use 4 spaces for indentation (no tabs). Limit line length to ~79 characters for code, 72 for comments/docstrings. Include comments and docstrings where appropriate to explain complex logic. Write docstrings for all public modules, classes, and functions to describe their purpose and usage.
	•	Naming Conventions: Use lower_case_with_underscores for function and variable names, CamelCase for class names, and UPPER_CASE for constants. Choose clear, meaningful names that reflect their intent (e.g., calculate_initiative_order() rather than calcOrd). Avoid single-letter names except for simple loop indices. Follow Python conventions for special methods and internal variables (e.g., use self as the first method argument in classes).
	•	Imports: Organize imports into groups: standard library, third-party, then local modules. Avoid wildcard imports (never use from module import * as it pollutes namespaces). Import only what is needed.
	•	Code Structure: Write modular, single-responsibility functions and classes. Each module (file) should encapsulate a distinct aspect of the game (gang management, combat resolution, etc.). Keep functions and methods focused; if a function exceeds ~50 lines or tries to do too much, consider refactoring it into smaller pieces. Use classes to model game entities (e.g., Gang, Fighter, Weapon) and to encapsulate related behavior, but prefer composition over deep inheritance to keep designs flexible.
	•	Pythonic Practices: Use Python’s idiomatic constructs. Prefer list/dict comprehensions and generators where they enhance clarity. Use f-strings for string formatting (e.g., f"{fighter.name} wins!"). Leverage dataclasses (@dataclass) for simple data-holding classes like configuration objects or DTOs. Handle errors with exceptions and use context managers (with statements) for resource management (files, transactions) rather than manual open/close.
	•	Type Annotations: Whenever possible, include type hints for function signatures and class methods. This improves readability and helps with static analysis or auto-complete in editors. Run tools like mypy (optional) to catch type inconsistencies.
	•	Positive Practices: Always write code that is self-documenting – use clear naming and structure so that logic can be followed without excessive comments. Always test code changes locally (and ensure tests pass) before committing. Always keep the code style consistent (it helps to run automated formatters/linters as mentioned).
	•	Prohibited/Negative Practices: Never commit code with failing tests or linter errors. Never use hacks or shortcuts that make code unclear (e.g., obscure one-liners) just to save lines. Never ignore or suppress exceptions without logging or handling them; catching exceptions should be purposeful. Avoid using global variables for mutable state – instead, encapsulate state in classes or use databases for persistence. Do not repeat code (follow DRY principle) – refactor common logic into helper functions or base classes. By defining these “never” rules alongside the “always” rules, we set clear boundaries for coding style ￼.

Testing

We will practice Test-Driven Development (TDD) as much as possible and maintain a robust test suite. All features should come with corresponding tests to verify correctness and prevent regressions. Guidelines for testing:
	•	Testing Framework: Use pytest (preferred for its simplicity and powerful features) for writing and running tests. For Django projects, you may use Django’s test runner (or pytest-django which integrates pytest with Django). Write tests in a tests/ directory or within each app directory (e.g., gangs/tests/test_models.py, combat/tests/test_combat.py).
	•	Unit Tests: Write unit tests for each module and component of the game engine. For example, test the gang creation logic (adding fighters, calculating gang rating), combat resolution (dice roll outcomes, hit/miss calculations, damage application), and any utility functions. Each rule or mechanic should have tests covering normal cases and edge cases. Use descriptive test function names (e.g., test_combat_hit_chance_calculation()).
	•	Integration Tests: Develop integration tests or functional tests for larger scenarios, such as running a full round of combat or a campaign cycle. If using Flask, use Flask’s test client to simulate API calls to endpoints (or for Django, use the test client to simulate web requests) to verify end-to-end behavior (e.g., creating a gang via an API endpoint and then starting a battle). These tests ensure that different parts of the system work together correctly.
	•	Test Data and Determinism: When testing random or probabilistic outcomes (like dice rolls), control randomness by seeding the random number generator or by injecting a dice-rolling service that can be stubbed or mocked. This ensures tests are deterministic. Use fixtures for setting up common objects (e.g., a sample gang with members) to avoid duplication. Leverage pytest fixtures and parametrize tests for different scenarios (e.g., multiple weapon types or skill levels).
	•	Running Tests: Provide commands to run tests easily. For example, a command in the Makefile or README:

pytest

or for Django:

python manage.py test

Always run the full test suite (and linting) before committing code to ensure nothing is broken ￼. Consider setting up a Continuous Integration workflow (like GitHub Actions) to run tests on each pull request or push to main.

	•	Coverage: Aim for a high coverage (e.g., > 80%) of critical modules. While 100% coverage is not mandatory, any fix or feature should include tests. Pay particular attention to complex game logic and math (e.g., injury calculations, experience gain) – these should be thoroughly tested.
	•	Bug Fixes: When bugs are found, write a test that reproduces the bug, then fix the bug and ensure the test passes (this prevents regressions in the future).

Project Architecture

The project’s architecture should be modular, scalable, and organized following web framework conventions (MVC in Django, Blueprint/Blueprint-like structure in Flask) while cleanly separating game logic from framework-specific code. Key architectural guidelines and components:
	•	Modules / Apps: Divide the system into logical modules, each handling a specific domain of the game:
	•	Gang Management: Handles gang creation, customization, roster management, recruitment, and advancements. Likely includes models for Gang and Fighter, with attributes like stats, skills, equipment list, and methods to calculate gang rating or handle adding/removing fighters.
	•	Campaign & Persistence: Manages the campaign between battles – tracking gang progression, injuries, experience, income, and territory control. This could include models like Territory or Resource, and logic for post-battle sequence (income generation, injury recovery, advancements). Persist campaign data in a database so it carries over between sessions.
	•	Tactical Combat Engine: The core turn-based combat simulator. This should be an engine that can run a scenario given participating gangs and terrain setup. It will manage turn order (initiative), movement, attacks, dice rolls, and rule enforcement (e.g., cover affects hit chances, weapon stats determine damage). This might be implemented as a set of classes (e.g., Battle or Scenario class coordinating multiple Fighter instances, maybe a TurnManager or state machine for phases of a turn).
	•	Equipment & Inventory: Handles items such as weapons, armor, and gear. Define data structures or models for Equipment, with properties like cost, rarity, stats (damage, range, etc.). This module interacts with both gang management (for buying/selling gear) and combat (to apply weapon effects). Make it flexible to add new items easily.
	•	Territory & Resources: Manages territories controlled by gangs and resource generation. This ties into the campaign as gangs can earn income or benefits from territories. It might include simple economic simulation (earning credits, spending on gear or upgrades).
	•	AI (Future Module): Although AI-controlled gangs are a future phase, keep the design open for plugging in AI logic. For example, define an interface or abstract class for a “Player Controller” that could be human (driven by UI inputs) or AI (driven by an algorithm). The combat engine then can use this interface to get actions for each gang on their turn. This sets the stage for adding AI opponents without redesigning the combat system.
	•	Separation of Concerns: Keep framework-specific code (routes, views, serializers) separate from game logic. For instance, in a Flask app, use Blueprints or route handlers that call into the game engine modules. In Django, use views and possibly Django Rest Framework serializers to expose functionality, but the heavy logic should reside in plain Python classes/functions outside of views. This makes the core game engine framework-agnostic and easier to test.
	•	Models and Database: Use a relational database (SQLite for dev, with potential to switch to PostgreSQL in production) to store persistent data: gangs, fighters, items, territories, battle outcomes, etc. Design the schema (or Django models) to reflect the game world (tables for gangs, fighters, etc., with relationships). Encapsulate database access through an ORM (SQLAlchemy or Django ORM) for convenience and to avoid raw SQL. Ensure that the domain models (Gang, Fighter, etc.) have methods for game logic (e.g., fighter.attack(target) might roll attack dice and return a result), but heavy computations can also be in separate service classes if needed.
	•	Services/Utilities: Consider creating service classes or utility modules for complex operations that don’t naturally belong on a single model. For example, a CombatService to resolve an entire fight round or a CampaignManager to handle post-battle updates. This prevents bloating models or views with too much logic.
	•	API Design: If building an API for the front-end, design endpoints that align with game actions (e.g., an endpoint to create a gang, an endpoint to start a battle, an endpoint to apply an action in combat like “fighter moves” or “fighter attacks”). Use JSON to send/receive game state. Keep the API stateless where possible; the server should hold the authoritative game state in memory or DB, especially for a multi-turn battle.
	•	Real-time or Turn Processing: Given this is turn-based, real-time updates are less critical, but if multiplayer or asynchronous play is considered, plan how the state is updated and shared. Possibly use WebSockets (Flask-SocketIO or Django Channels) for real-time updates in future, but for now, sequential turn submission via normal requests is fine.
	•	Extensibility: The architecture should make it easy to add new content or rules. For example, if a new weapon type or skill is introduced, the code should be structured such that adding it doesn’t require massive changes. This might mean using data-driven approaches (e.g., define weapons in JSON or a database, and have combat logic reference those stats) or plugin-like modules for rules.
	•	Performance Considerations: The scale (small number of models and dice rolls) means performance is not a huge concern, but keep algorithms efficient. Combat resolution might loop through fighters and such—this is fine given the scale is tens of fighters, but avoid unnecessary database hits in loops (prefetch data as needed). Use caching for any repeated lookups (e.g., rule lookups or terrain effects) if necessary. Keep the server logic authoritative and trust it over any client logic to prevent cheating if multiplayer.
	•	Documentation: Maintain clear documentation of module responsibilities and data models (could be in code docstrings or separate markdown files). A new contributor (or the AI assistant itself) should be able to understand the project structure quickly.

Security

Even though this is a game simulator, standard web application security practices must be followed:
	•	Input Validation: Validate and sanitize any input that comes from users or external sources. This includes web form data or API payloads (e.g., gang names, or custom scenario parameters). For Flask, use something like WTForms or manual validation; for Django, use forms or serializers (which provide validation). This prevents malicious inputs or game-breaking values (e.g., negative credits, overly large numbers).
	•	Authentication & Authorization: In the initial phase, the simulator might be single-player or local, but if multi-user access or remote play is added, implement proper authentication (e.g., user accounts, login system) and authorization checks (one user’s gang data should not be accessible or modifiable by another user without permission). Leverage framework features: Django’s auth system or Flask extensions if needed.
	•	Data Protection: Store sensitive data (if any) securely. Although game data isn’t highly sensitive, if the app is deployed, use environment variables or config files for secrets (like Django SECRET_KEY, database passwords) and never commit those to source control. Ensure cookies (if used) are HTTPOnly and use secure flags in production, and enable protections like Django’s CSRF middleware or Flask’s CSRF tokens for forms.
	•	Secure Code Practices: Avoid common vulnerabilities. For example, prevent SQL injection by using ORM parameter binding (never string format SQL queries). Guard against XSS by properly escaping any user-generated content in templates (Django autoescapes by default; with Flask+jinja, ensure autoescape on). Use SSL (HTTPS) if transmitting any sensitive information. For any file I/O (if players can upload or if you save campaign files), handle paths carefully to avoid path traversal vulnerabilities.
	•	Game Logic Abuse: Since this is a simulator, if any user inputs can affect game outcomes (e.g., a user could submit a manipulated API call to give themselves extra money or skip a turn), the server should validate all game actions. Do not trust the client to enforce rules. The server-side engine should enforce game rules consistently (for example, a fighter cannot move more than their allowance, or cannot shoot if they already did, etc.). If an invalid action is detected, reject it and possibly log it.
	•	Logging and Monitoring: Include logging of important events (e.g., game errors, security events). Do not log sensitive info (like passwords). Use logging for debugging but sanitize any user input in logs. This helps in auditing and debugging issues.
	•	Dependency Security: Keep an eye on dependency vulnerabilities. Use recent versions of frameworks and libraries (Flask, Django, etc.) that receive security updates. Periodically review pip install for any flagged vulnerabilities (tools like pip-audit can help). Pin versions in requirements.txt so that the environment is consistent, and update with caution, testing after upgrades.
	•	Compliance and IP: Since the game is “inspired by Necromunda,” be mindful of intellectual property. Do not copy any copyrighted rule text or assets from the official game. Implement mechanics originally and use generic terms if needed to avoid trademark issues. This isn’t a traditional “security” concern, but it is a legal safety measure for the project’s longevity.

Git Workflow

Maintain a clean and collaborative Git workflow to manage the project’s source code. This ensures that the AI assistant and any human collaborators can work together smoothly and safely:
	•	Repository Structure: Keep this AGENTS.md file at the root of the repository (or in a dedicated agents/ folder if using multiple agent config files) so that AI development tools (like GitHub Copilot or GPT Codex) can easily find and follow it. Also include a human-facing README.md for general project info (audience: developers and players).
	•	Branching Strategy: Use a feature-branch workflow. For each new feature or module (e.g., feature/gang-management-module or feature/combat-engine), create a separate Git branch. Do not commit directly to main (or master). The main branch should contain only tested, stable code. Merge feature branches via pull requests (PRs) after review.
	•	Commits: Write descriptive commit messages for every change. A good format is to start with a short summary (e.g., “Add Gang model and creation logic”), optionally followed by details if necessary. If fixing a bug, reference the issue or describe the bug in the commit message. Commit often, whenever a logical piece of work is completed, to make it easier to track changes and revert if needed.
	•	Pull Requests and Code Review: Even if working solo with the AI assistant, treat pull requests as a way to review and validate changes. When a feature is ready, open a PR to merge into main and have the AI assistant (or another developer) review the code for any issues or improvements. Ensure all checks pass (tests, linters) before merging. Use PR templates if available to remind of checklist items (tests written, documentation updated, etc.).
	•	Continuous Integration (CI): Set up CI workflows (e.g., GitHub Actions or similar) to run automated tests and linters on each push or PR. This helps catch issues early. For example, a CI pipeline can create the Conda environment, install dependencies, then run pytest and report status. Only merge PRs when CI is green.
	•	Git Hooks: Implement client-side git hooks (using pre-commit framework or custom scripts) to automate checks. For instance, a pre-commit hook can run black and flake8, and a pre-push hook can run tests. This ensures the code meets standards before it even gets to GitHub.
	•	Releases & Tags: As the project progresses, use git tags or releases to mark notable versions (v0.1, v1.0, etc.). This is useful for maintaining a changelog and for any future packaging or distribution of the simulator. Tags can also help coordinate with the AI assistant by providing stable reference points for the codebase.
	•	Collaboration with AI: Clearly annotate in commit messages or PR descriptions when changes are AI-generated or AI-assisted. This helps maintain transparency. Also, keep conversations or prompts that led to significant code (if possible) for future reference (maybe in the commit message or separate docs).
	•	Rollback Plan: In case the AI assistant’s contribution introduces issues, have a strategy to revert commits. Test each AI-generated feature in a safe environment before relying on it. Use git’s branching to experiment, and only merge to main when confident.
	•	No Secrets in Git: As mentioned in Security, never commit sensitive information. Use a .gitignore to exclude files like .env or credentials. If a secret accidentally gets committed, purge it from history and rotate it if applicable.

By following the above configuration and guidelines, the AI coding agent and human developers will work in tandem efficiently. The structure of this AGENTS.md is based on proven practices for AI agent configuration ￼ ￼, ensuring that the development process for the game simulator remains organized, secure, and aligned with project goals.
