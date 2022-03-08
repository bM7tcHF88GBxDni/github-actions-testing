# GitHub Actions R&D

I lead the implementation of a CI/CD pipeline in our School of Code final 4 week project:

- Continuous Testing implemented when a Pull Request is opened to merge into dev or main
- Implemented unit tests (Jest) and integration tests (React Testing Library and SuperTest)
- API tests use test environment variables automatically which means tests run on a database setup specifically for testing
  - testing database is destroyed and rebuilt before every test runs

This repository contains independent research and testing GitHub Actions that I also used to upskill the team and help a few other students at School of Code.

<details>
<summary>
CI/CD/CD recap:
</summary>
<hr/>

### Continuous Integration (CI)

In this phase changes from a developer are merged and validated. The goal of CI is to quickly validate these pushed code changes.

The intended outcome is to identify any problems in the code and automatically notify the developer. This helps ensure that the code base is not broken any longer than necessary. The CI process detects when code changes are made, and runs any associated build processes to prove the code changes are buildable. It can also run targeted testing.

### Continuous Delivery

Continuous Delivery refers to the chain of processes (the pipeline) that automatically gets code changes and runs them through build, test, packaging, and/or related operations to produce a deployable release. Typically, it does this without much or any human intervention

#### Continuous Testing

Continuous Testing refers to the practice of running automated tests or other types of analysis, of broadening scope as code goes through the Continuous Delivery pipeline. These include: unit testing, integration testing, functional testing, acceptance testing (performance, scalability, stress, and capacity).

### Continuous Deployment

Continuous Deployment refers to being able to take a release of code that has come out of the delivery pipeline and automatically make it available for end users.

Just because Continuous Deployment can be done doesn’t mean that every set of deliverables coming out of a pipeline is always deployed or that new functionality is turned on. It means that, via the pipeline, every set of deliverables is proven to be deployable through mechanisms such as Continuous Testing.

<hr/>
</details>

## GitHub Actions Workflow (https://docs.github.com/en/actions/using-workflows):

![](https://docs.github.com/assets/cb-25628/images/help/images/overview-actions-simple.png)

- configure a `workflow` to be triggered when an `event` occurs in your repository e.g a pull request being opened or an issue being created
- `workflows` contain `jobs` that can run in order (a job is dependent on previous job) or in parallel
  - each job will run inside its own Virtual Machine (VM) or container, this is referred to as a `runner`. one `runner` can do multiple `jobs`
  - `jobs` contain `steps` to
    - run `scripts`
    - run `actions` (reusable extension that can simplify our workflow)
- `workflows` are defined by a YAML file in your repository: `.github\workflows\example-workflow-file.yml`
- `workflows` can be triggered by an `event` in the repository, manually, or a schedule. `Events` that trigger `workflows`: https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows
- workflows can be reused within workflows
- can have one `workflow` to build and test pull requests, another `workflow` to deploy every time a release is created

### Actions and Marketplace: https://github.com/marketplace?type=actions

- custom app/plugin for the GHA platform that does a frequently repeated task. Use an `action` to help reduce the amount repetitive code you write in your `workflow` files

## Writing Workflows: https://docs.github.com/en/actions/using-workflows

Visualising the `learn-github-actions` workflow we are going to start with:
![](https://docs.github.com/assets/cb-33984/images/help/images/overview-actions-event.png)

Create directory in root:
`./github/workflows/`

Write workflows in .yml extension. For example:
`learn-github-actions.yml`

Writing yml files: https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions

```yml
name: learn-github-actions
on: [push] #triggers workflow on push or PR merge to any branch of this repo
jobs: #each runner can have one job
  check-bats-version: #job name, below key:values are job properties
    runs-on: ubuntu-latest #os
    steps: #each item below is a script or action
      - uses: actions/checkout@v2 #clones repository into runner/container/VM
      - uses: actions/setup-node@v2 #action that setups up node and npm
        with:
          node-version: "14"
      - run: npm install -g bats #scripts to get project dependencies installed, this would just be npm install as the project should have all dependencies in package.json
      - run: bats -v #in a project, this would be like npm start. this example is like `node -v` and just returns the bats dependency version installed
```

## Viewing the workflow's activity: https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#viewing-the-workflows-activity

- Actions tab in repository
- Left sidebar, select a workflow
- Under Jobs or in the visualization graph, select a job
- Click on each step to view the results of that step
