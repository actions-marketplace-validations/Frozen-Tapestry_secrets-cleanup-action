# project-secrets-cleanup

A small, defensive GitHub **composite action** that removes a secrets directory created during CI runs.

This action is intended for cleanup at the end of a workflow, ensuring temporary secret material does not persist in the workspace.

## Description

`project-secrets-cleanup` deletes a specified directory using strict safety checks to prevent accidental removal of dangerous paths.

Built-in guards refuse to delete:

- Empty paths
- Absolute paths (`/…`)
- Paths containing `..`
- The current directory (`.` or `./`)

If the directory does not exist, the action exits successfully.

## Inputs

| Name | Required | Default | Description |
|-----|----------|---------|-------------|
| `dir` | No | `.ci-secrets` | Relative directory path to remove |

## Usage

### Basic example

```yaml
- name: Cleanup CI secrets
  uses: frozen-tapestry/project-secrets-cleanup@v1
````

This removes the default `.ci-secrets` directory.

### Custom directory

```yaml
- name: Cleanup secrets directory
  uses: frozen-tapestry/project-secrets-cleanup@v1
  with:
    dir: .github/secrets
```

## Behavior

* Uses `bash` with `-euo pipefail`
* Fails fast if the path is unsafe
* Runs `rm -r -- <dir>` only after validation
* Safe to run even if the directory does not exist

## When to use

* Cleaning up decrypted secrets
* Removing temporary credentials
* Post-job or post-step cleanup in CI pipelines

## License

See [LICENSE](LICENSE).