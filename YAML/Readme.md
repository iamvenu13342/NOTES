<h1>YAML</h1>

YAML (YAML Ain't Markup Language) is a human-readable data serialization format often used for configuration files. It is designed to be easy to read and write. Here are comprehensive notes on YAML covering its key features, syntax, and usage:

### Basics of YAML

- **YAML is case-sensitive.**
- Files typically use the `.yaml` or `.yml` extension.
- YAML documents start with `---` (optional) and end with `...` (optional).

### Structure

- **Indentation:** YAML uses spaces for indentation. Tabs are not allowed.
- **Lines:** Each entry is separated by a newline.
- **Comments:** Lines starting with `#` are comments.

### Data Types

1. **Scalars:**
   - Strings:
     ```yaml
     single_line: "Hello, World!"
     multiline: |
       This is a
       multiline string.
     ```
   - Numbers (integers and floats):
     ```yaml
     integer: 42
     float: 3.14
     ```
   - Booleans:
     ```yaml
     boolean_true: true
     boolean_false: false
     ```
   - Null:
     ```yaml
     null_value: null
     ```

2. **Sequences (lists):**
   ```yaml
   fruits:
     - Apple
     - Banana
     - Orange
   ```

3. **Mappings (dictionaries):**
   ```yaml
   person:
     name: John Doe
     age: 30
     address:
       city: New York
       state: NY
   ```

### Advanced Features

- **Anchors and Aliases:** Reuse values within the YAML document.
  ```yaml
  default: &default
    name: John Doe
    age: 30

  user1:
    <<: *default
    age: 25  # Override age
  ```

- **Merge keys:**
  ```yaml
  defaults: &defaults
    adapter: postgres
    host: localhost

  development:
    <<: *defaults
    database: dev_db

  production:
    <<: *defaults
    database: prod_db
  ```

- **Strings:**
  - Plain: No quotes.
    ```yaml
    key: value
    ```
  - Single-quoted: Preserve formatting and escape sequences are not processed.
    ```yaml
    key: 'value'
    ```
  - Double-quoted: Supports escape sequences.
    ```yaml
    key: "value\nnewline"
    ```

### Special Notations

- **Dates and Times:**
  ```yaml
  date: 2023-01-01
  datetime: 2023-01-01T13:00:00Z
  ```

- **Explicit Type Tags:** Use `!!` to specify the type.
  ```yaml
  canonical: !!str 2023-01-01
  ```

- **Folded Blocks:** Use `>` for folded text (preserves newlines).
  ```yaml
  folded: >
    This is a folded block
    with multiple lines.
  ```

### Examples

1. **Simple Configuration:**
   ```yaml
   server:
     port: 8080
     host: localhost

   database:
     user: admin
     password: secret
     name: mydb
   ```

2. **Complex Configuration:**
   ```yaml
   services:
     web:
       image: nginx
       ports:
         - "80:80"
     db:
       image: postgres
       environment:
         POSTGRES_DB: mydb
         POSTGRES_USER: admin
         POSTGRES_PASSWORD: secret
   ```

### Best Practices

- Use spaces for indentation (2 or 4 spaces are common).
- Keep the structure simple and consistent.
- Use comments to explain complex configurations.
- Validate YAML files to ensure correctness (many tools and libraries are available for this).

### Tools and Libraries

- **Online Validators:** Websites that check YAML syntax.
- **Libraries:** PyYAML (Python), js-yaml (JavaScript), ruamel.yaml (Python), etc.

### Conclusion

YAML is a versatile and readable format suitable for a variety of applications, particularly configurations. Understanding its syntax and features helps in creating and managing structured data effectively.
