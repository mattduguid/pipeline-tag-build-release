
# useful code snippets for workflows

## table of contents
- [cache dependencies with the cache action](#cache-dependencies-with-the-cache-action)
- [pass artifact data between jobs](#pass-artifact-data-between-jobs)
- [enable step debug logging in a workflow](#enable-step-debug-logging-in-a-workflow)
- [use secrets](#use-secrets)

## cache dependencies with the cache action

```yaml
steps:
  - uses: actions/checkout@v2

  - name: Cache NPM dependencies
    uses: actions/cache@v2
    with:
      path: ~/.npm
      key: ${{ runner.os }}-npm-cache-${{ hashFiles('**/package-lock.json') }}
      restore-keys: |
        ${{ runner.os }}-npm-cache-
```

## pass artifact data between jobs

```yaml
name: Share data between jobs
on: push
jobs:
  job_1:
    name: Upload File
    runs-on: ubuntu-latest
    steps:
      - run: echo "Hello World" > file.txt
      - uses: actions/upload-artifact@v2
        with:
          name: file
          path: file.txt

  job_2:
    name: Download File
    runs-on: ubuntu-latest
    needs: job_1
    steps:
      - uses: actions/download-artifact@v2
        with:
          name: file
      - run: cat file.txt
```

## enable step debug logging in a workflow

- To enable runner diagnostic logging, set the ACTIONS_RUNNER_DEBUG secret in the repository that contains the workflow to true.
- To enable step diagnostic logging, set the ACTIONS_STEP_DEBUG secret in the repository that contains the workflow to true.

## use secrets

```yaml
steps:
      - name: "Login via Azure CLI"
        uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}
```
