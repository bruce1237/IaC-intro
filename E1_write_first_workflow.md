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
    steps:
      - name: goodbye
        run: echo "Good-Bye"
        shell: bash
```
