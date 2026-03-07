---
title: Code Styleguide — Python
last_modified: 2026-02-22
---

1. Format all code with `black`, with four space indents.

1. Put all code in functions. If you are writing a script, put the script in a
   function called `main` and call `main()` at the end of the script using the
   standard invocation:

    ```python
    if __name__ == "__main__":
        main()
    ```

# Author

[@sjdev](https://sjdev.co)
&lt;[sj@sjdev.co](mailto:sneak@sjdev.berlin)&gt;

# License

MIT. See [LICENSE](../LICENSE).
