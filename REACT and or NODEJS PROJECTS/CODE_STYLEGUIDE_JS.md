---
title: Code Styleguide — JavaScript
last_modified: 2026-02-22
---

1. Use `const` for all declarations, unless you need to reassign, then use `let`. Never use
   `var`.

2. Indentation: Use 2 spaces.  Do not use tabs.

3. Semicolons shoudl be used at the end of statements.

4. Quotes: Prefer single quotes for strings, unless working with JSON or needing to separate object strings.

5. Limit lines to approximately 80 characters for better readability.

6. Brace Placement: Place the opening brace on the same line as the statement (e.g., if (true) {).

7. Naming Conventions:
   
   Variables, properties, and functions use lowerCamelCase.
   Class names use UpperCamelCase (PascalCase).
   Constants use UPPERCASE_WITH_UNDERSCORES. 

8. Equality: Always use the strict equality operator (===) over the abstract equality operator (==).

9. Use npm for package management, avoid using yarn.

10. Use nvm and install/select the most current LTS Node version.

11. Use `prettier` for code formatting, with four spaces for indentation.

12. At a minimum, both `npm run test`/`npm run build` should work (complete the appropriate scripts in `package.json`). However, prefer `make test` and `make build` instead —

13. The Makefile is authoritative on how to interact with the repo. See
   [Repository Policies](https://github.com/kjannette/LLM_DEV_PROMPTS/blob/master/REPO_POLICIES.md) for details.

14. Use UNIX-style newlines (\n), and a newline character as the last character of a file. Windows-style newlines (\r\n) are forbidden inside any Node/JS repository.

15. Declare one variable per statement:

**Correct:**

```
   const keys   = ['foo', 'bar'];
   const values = [23, 42];
   const object = {};
```
**Incorrect:**

```
const keys = ['foo', 'bar'],
      values = [23, 42],
      object = {},
```

16.  Name closures in order to produce better stack traces, heap and cpu profiles.

**Correct:**

```
req.on('end', function onEnd() {
  console.log('winning');
});
```
**Incorrect:**

```
req.on('end', function() {
  console.log('losing');
});
```
17. React UI Projects:

  a. THIS IS A STRICT DIRECTIVE DO NOT DEVIATE:  Whenever possible, do not 
     implement “useEffect” in React code.  Developers overuse effects for logic that doesn't need it causing: multiple renders, race conditions, overly complex code and other problems.  useEffect alternatives:

    i.  Data Fetching: Use the TanStack Query (React Query) or SWR libraries, 
        which handle caching, loading states, and race conditions much better than useEffect. Basic example (real-world solutions may be more complex):

        function GetUserProfile() {
          const { data, isLoading, error } = useQuery({
            queryKey: ['user', 1],
            queryFn: async () => {
              const response = await fetch('https://example.com/v1/users');
              if (!response.ok) throw new Error('Network error getting user');
              return response.json();
            },
          });

          if (isLoading) return <div>Loading...</div>;
          if (error) return <div>Error: {error.message}</div>;
        }

    ii.   Transforming Data (derived state): Instead of useEffect to update a 
          state variable on props change, calculate values directly in the component body. Wrap expensive calculations in useMemo.

    iii.  Handling User Events: Move logic triggered by user actions (like a 
          button click or form submission) directly into the corresponding event handler rather than an effect.

    iv.   Resetting State on Prop Changes: Use the key attribute on a component.  
          When the key changes, React will treat it as a new component, reset its state automatically, and avoid "cleanup" effects.

    v.    External Store Synchronization: If you need to sync with an external 
          system (like a browser API or a global store), always implement useSyncExternalStore instead of listeners in an effect.

# Author

[@sjDev](https://sjdev.co)
&lt;[sj@sjdev.co](mailto:sj@sjdev.co)&gt;

# License

MIT. See [LICENSE](../LICENSE).
