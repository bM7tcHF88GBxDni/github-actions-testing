### Github Action

Some research notes (https://docs.github.com/en/actions/using-workflows):

- configure a workflow to be triggered when an event occurs in your repository e.g a pull request being opened or an issue being created
- workflows contain jobs that can run in order or in parallel

  - each job will run inside its own VM or container, this is referred to as a runner. one runner can do one job
  - jobs contain steps to
    - run scripts
    - run actions (reusable extension that can simplify our workflow)

![](https://docs.github.com/assets/cb-25628/images/help/images/overview-actions-simple.png)

- workflows are defined by a YAML file in your repository
- they can be triggered by an event in the repository, manually, or a schedule. Events that trigger workflows: https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows
- workflows can be reused within workflows
- can have one workflow to build and test pull requests, another workflow to deploy every time a release is created

Actions

- custom app for the GHA platform that does a frequently repeated task. Use an action to help reduce the amount repetitive code you write in your workflow files

### Writing Workflows: https://docs.github.com/en/actions/using-workflows

Create directory in root:
`./github/workflows/`

Write workflows in .yml extension. For example:
`gha-testing.yml`

Writing yml files: https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions

```yml
name: learn-github-actions #comments
on: [push] #triggers workflow on push or PR merge to any branch of this repo
jobs: #each runner can have one job
  check-bats-version: #job name, below key:values are job properties
    runs-on: ubuntu-latest #os
    steps: #each item below is a script or action
      - uses: actions/checkout@v2 #clones repository into runner/container/VM
      - uses: actions/setup-node@v2 #action that setups up node and npm
        with:
          node-version: "14"
      - run: npm install -g bats #scripts to get project dependencies installed
      - run: bats -v #in a project, this would be like npm start. this example is like `node -v` and just returns the bats dependency version installed
```
