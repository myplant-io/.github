# Github Template Repository

Shared workflow templates (for github actions).

:warning: This is a public repo. :warning:  
To learn why this is the case, have a look at the following issue:
https://github.com/github/roadmap/issues/98

## Auto update Github Actions

This repository provides an auto-update.yml-action that installs/updates common workflows. Dependabot automatically creates pull requests if there are changes in this repository.

Auto-update features:

- Install generalized workflows based on `workflow-config.env` (through Dependabot or `dispatch_workflow`)
- Updates target repository's `Readme.md` with CI/CD section
- Allows dependabot to create pull request with changes in generalized workflows, such that all repositories in myPlant share the same workflows (also in the future)
- \[TODO\] Support for custom workflow files that implements myPlant's release flow

## Installation

Install the common-auto-update.yml into your repository (Use a feature branch). Example in cli (requires git, gh, curl), but also works on [github.com/myplant-io](https://github.com/myplant-io)

1. Create feature branch

```
git branch feat/install-auto-update develop
git checkout feat/install-auto-update
git push --set-upstream origin feat/install-auto-update
```

2. Create `workflow-config.env` file in the `{{TARGET_REPOSITORY}}/.github/` folder

yarn:

```
curl https://raw.githubusercontent.com/myplant-io/.github/master/env-templates/yarn-workflow-config.env --output ./.github/workflow-config.env
```

yarn2:

```
curl https://raw.githubusercontent.com/myplant-io/.github/master/env-templates/yarn2-workflow-config.env --output ./.github/workflow-config.env
```

gradle:

```
curl https://raw.githubusercontent.com/myplant-io/.github/master/env-templates/gradle-workflow-config.env --output ./.github/workflow-config.env
```

3. Install common-auto-update.yml through `{{TARGET_REPOSITORY}}/new/feat/install-auto-update/?filename=.github%2Fworkflows%2Fauto-update.yml&workflow_template=workflow-templates%2Fcommon-auto-update`

```
python -m webbrowser "$(gh repo view --json url --template '{{.url}}')/new/feat/install-auto-update/?filename=.github%2Fworkflows%2Fauto-update.yml&workflow_template=workflow-templates%2Fcommon-auto-update"
```

4. Dispatch auto-update workflow (workflow_run) in the `feat/install-auto-update` branch

```
gh workflow run auto-update.yml -r feat/install-auto-update
```

5. Create PR `feat/install-auto-update` -> `develop` and review changes.

```
gh pr create --base develop
```

## GitHub dependabot configuration

It is recommended to enable a dependabot for checking on new version of the
actions used.

A sample configuration _dependabot.yml_ could look like this:

```
version: 2
registries:
  maven-myplant-nexus:
    type: maven-repository
    url: https://nexus-dev.myplant.io/repository/maven-public
    username: ${{secrets.NEXUS_USER_RO}}
    password: ${{secrets.NEXUS_PW_RO}}
updates:
  - package-ecosystem: "gradle"
    directory: "/"
    registries:
    - maven-myplant-nexus
    schedule:
      interval: "daily"
    open-pull-requests-limit: 5
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "daily"
```

## Noteworthy

Next to the workflow files itself (e.g. _.github/gradle-deploy.yml_) you will
need a _.github/workflow-config.env_ file for the actions to work. See [env-templates](https://github.com/myplant-io/.github/tree/master/env-templates/)

Here is an example workflow configuration file:

```
PROJECT_TYPE=gradle
COMPONENT_NAME=auto
DEPLOYMENT_REPO=myplant-io/deployment
DOCKER_TAG_PREFIX=auto
GRADLE_BUILD_TASK=build test
GRADLE_DEPENDENCY_CHECK_TASK=cyclonedx dependencyCheckAnalyze
GRADLE_DEPLOY_TASK=dockerPushAndDeleteLocal -x test
GRADLE_PUBLISH_SHA=false
GRADLE_PUBLISH_TASK=none
GRADLE_SONARQUBE_TASK=test sonarqube
GRADLE_TEST_TASK=test
PUSH_DEPLOY_TARGET=["staging-io/${COMPONENT_NAME}.yaml"]
PRE_RELEASE_DEPLOY_TARGET=none
RELEASE_DEPLOY_TARGET=["production-io/${COMPONENT_NAME}.yaml"]
VERBOSE_ARTIFACT_UPLOAD=false
```

**Hints:**
The files can slightly vary between build tools (the above one is for gradle),
so please make sure that it contains everything that is needed.

- Most docker deployments actions (docker-deploy.yaml, yarn/yarn2-deploy) actions
  require a DOCKER_REPOSITORY variable.
- Yarn/Yarn2 dependency checks need a SECURITY_LEVEL defined.
