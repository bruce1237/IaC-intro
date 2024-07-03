# [E1 write your first workflow](https://www.youtube.com/watch?v=-hVG9z0fCac&list=PLArH6NjfKsUhvGHrpag7SuPumMzQRhUKY)

# structure

the workflow is triggered by `Event`: pull-request, push, Issues....

and each `Event` will trigger at least one workflow,

each `workflow` containers `jobs`

`jobs` container `steps` 

and `steps` has to run in sequence

and `jobs` can be run in parallel

each job is associate with a `runner`

`runners` has two type: `github hosted` runner and `self-hosted` runners



hello-world workflow
```yml
# name of the workflow
name: hello world workflow

# trigger
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  workflow_dispatch: # give a manual button to run the workflow

# define jobs
jobs:
  hello: # job name
    runs-on: ubuntu-latest  # runner OS
    steps: # define steps
      - uses: actions/checkout@v4   # step 1 copy the repo into the runner
      - name: hello world  # name of step two
        run: echo "Hello World"  # run: define a shell command
        shell: bash  # define which shell the command going to run

  goodbye:
    runs-on: ubuntu-latest
    needs: ['hello', 'job2']  # this has to wait for the hello job to complete successfully
    if: condition == condition # only if the condition is true, the job will be run
    steps:
      - name: goodbye
        run: echo "Good-Bye"
        shell: bash
```

issue.yml
```yml
name: create a comment on new issues

on:
  issues:
    types:
      - opened

jobs:
  comment-with-action:
    runs-on: ubuntu-latest
    steps:
      - name: "dump github context"
        run: echo '${{ toJSON(github.event) }}' | jq
        shell: bash
    
      - name: Create comment
        uses: peter-evans/create-or-update-comment@v3
        with:
          issue-number: ${{ github.event.issue.number }}
          body: |
            This is a multi-line test comment
              - With GitHub **Markdown** :sparkles:
              - Created by [create-or-update-comment][1]

              [1]: https://github.com/peter-evans/create-or-update-comment
          reactions: '+1'          

  comment-with-api:
    runs-on: ubuntu-latest
    steps:
      - name: create comment with API
        run: |
          gh api -X POST \
            http://api.github.com/repo/${REPOSITORY}/issues/${ISSUE_NUMBER}/comments \
            -f body='
            comment from 
            API
            multiple lines
            '
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          ORGANIZATION: ${{ github.event.user.login }}
          REPOSITORY: ${{ github.event.repository.full_name }}
          ISSUE_NUMBER: ${{ github.event.issue.number}}

```
