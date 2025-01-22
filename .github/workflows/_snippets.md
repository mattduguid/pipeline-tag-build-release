
# useful code snippets for workflows

## table of contents
- [cache dependencies with the cache action](#cache-dependencies-with-the-cache-action)
- [pass artifact data between jobs](#pass-artifact-data-between-jobs)
- [enable step debug logging in a workflow](#enable-step-debug-logging-in-a-workflow)
- [use variables and secrets and environments](#use-variables-and-secrets-and-environments)
- [conditionals](#conditionals)
- [specials](#specials)
- [github script](#github-script)
- [message logging](message-logging)

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

## use variables and secrets and environments

```yaml
# secret as input
- uses: azure/docker-login@v1
      with:
        login-server: ${{env.IMAGE_REGISTRY_URL}}
        username: ${{ var.GITHUB_USER }}
        password: ${{ secrets.GITHUB_TOKEN }}

# secret as input or env var
steps:
  - name: Hello world action
    with: # Set the secret as an input
      super_secret: ${{ secrets.SuperSecret }}
    env: # Or as an environment variable
      super_secret: ${{ secrets.SuperSecret }} 
```

## conditionals

```yaml
# pull request labels
if: contains(github.event.pull_request.labels.*.name, 'spin up environment')
```

## specials

```yaml
# treat everything inside raw/emdraw as not to be interpreted by a template engine
github-token: {% raw %}${{secrets.GITHUB_TOKEN}}{% endraw %}
```

## github script

```yaml
name: GitHub Script examples
on:
  issues:
    types: [opened]
jobs:
  comment:
    runs-on: ubuntu-latest
    steps:
    - name: Comment on new issue
      uses: actions/github-script@0.8.0
      with:
        github-token: {% raw %}${{secrets.GITHUB_TOKEN}}{% endraw %}
        script: |
            github.issues.createComment({
            issue_number: context.issue.number,
            owner: context.repo.owner,
            repo: context.repo.repo,
            body: "🎉 You've created this issue comment using GitHub Script!!!"
            })
    - name: Add issue to project board
      if: contains(github.event.issue.labels.*.name, 'bug')
      uses: actions/github-script@0.8.0
      with:
        github-token: {% raw %}${{secrets.GITHUB_TOKEN}}{% endraw %}
        script: |
            github.projects.createCard({
            column_id: {{columnID}},
            content_id: context.payload.issue.id,
            content_type: "Issue"
            });
```

## message logging

```yaml
- name: workflow commands logging messages
  run: |
    echo "::debug::This is a debug message"
    echo "This is an info message"
    echo "::error::This is an error message"
    echo "::warning::This is a warning message"
    echo "::error file=app.js,line=10,col=15::Something went wrong"
    echo "multiline message%0This text spans%0Aacross multiple lines"
```
