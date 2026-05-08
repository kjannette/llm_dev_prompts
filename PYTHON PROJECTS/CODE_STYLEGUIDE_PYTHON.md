---
title: Code Styleguide — Python
last_modified: 2026-02-22
---

1. Standard Project Layout.  Use the src/ layout to prevent accidental imports from the root and ensure your project behaves like an installed package:

my_project/
├── pyproject.toml      # Modern tool & dependency configuration
├── README.md           # Instructions for humans
├── LICENSE             # Usage rights
├── .gitignore          # Exclude venv/, __pycache__/, .env
├── src/
│   └── my_project/     # Main package
│       ├── __init__.py
│       ├── main.py     # Minimal entry point logic
│       └── core.py     # Business logic
├── tests/              # Mirror src structure for testing
└── docs/               # Technical documentation

2. Put all code in functions. If you are writing a script, put the script in a
   function called `main` and call `main()` at the end of the script using the
   standard invocation:

    ```python
    if __name__ == "__main__":
        main()
    ```

# Author

[@sjdev](https://sjdev.co)
&lt;[sj@sjdev.co](mailto:sj@sjdev.berlin)&gt;

# License

MIT. See [LICENSE](../LICENSE).
