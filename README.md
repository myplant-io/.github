# Github Template Repository

Shared workflow templates (for github actions).

:warning: This is a public repo. :warning:  
To learn why this is the case, have a look at the following issue:
https://github.com/github/roadmap/issues/98

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
need a _.github/workflow-config.env_ file for the actions to work.

Here is an example workflow configuration file:

```
COMPONENT_NAME=auto
DEPLOYMENT_REPO=myplant-io/deployment
DOCKER_REPOSITORY=myplant-io
DOCKER_TAG_PREFIX=auto
GRADLE_BUILD_TASK=build test
GRADLE_DEPENDENCY_CHECK_TASK=assemble cyclonedx dependencyCheckAnalyze
GRADLE_DEPLOY_TASK=dockerPushAndDeleteLocal -x test
GRADLE_PUBLISH_SHA=false
GRADLE_PUBLISH_TASK=none
GRADLE_SONARQUBE_TASK=test sonarqube
GRADLE_TEST_TASK=test
PUSH_DEPLOY_TARGET=["staging-io/${COMPONENT_NAME}.yaml"]
PRE_RELEASE_DEPLOY_TARGET=none
RELEASE_DEPLOY_TARGET=["production-io/${COMPONENT_NAME}.yaml"]
VERBOSE_ARTIFACT_UPLOAD=false
VERBOSE_LOGGING=false
```

**Hints:**
The files can slightly vary between build tools (the above one is for gradle),
so please make sure that it contains everything that is needed.

Prefect would need the following:
```
COMPONENT_NAME: auto
DOCKER_TAG_PREFIX: auto
STAGING_PREFECT_SERVER_URL: 'https://prefect-dev.staging.myplant.io'
PROD_PREFECT_SERVER_URL: 'https://prefect-dev.myplant.io'
PROJECT_NAME: 'prod-to-staging-dump'
```

- Most docker deployments actions (docker-deploy.yaml, yarn/yarn2-deploy) actions
  require a DOCKER_REPOSITORY variable.
- Yarn/Yarn2 dependency checks need a SECURITY_LEVEL defined.
