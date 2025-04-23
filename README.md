# GitHub Template Repository

Shared workflow templates (for GitHub actions).

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

Here is an example workflow configuration file (useable for java microservices built with gradle):

```
COMPONENT_NAME=auto
DEPLOYMENT_REPO=myplant-io/deployment
DEPLOYMENT_METHOD=helm
DOCKER_REPOSITORY=auto
DOCKER_TAG_PREFIX=auto
GRADLE_BUILD_TASK=build test
GRADLE_DEPENDENCY_CHECK_TASK=assemble cyclonedx
GRADLE_DEPLOY_TASK=dockerPushAndDeleteLocal -x test
GRADLE_PUBLISH_SHA=false
GRADLE_PUBLISH_TASK=none
GRADLE_SONARQUBE_TASK=test sonar
GRADLE_TEST_TASK=test
PUSH_DEPLOY_TARGET=["staging-alpha-io/${COMPONENT_NAME}.yaml"]
PRE_RELEASE_DEPLOY_TARGET=["staging-beta-io/${COMPONENT_NAME}.yaml"]
RELEASE_DEPLOY_TARGET=["production-io/${COMPONENT_NAME}.yaml"]
SONAR_PREFIX=io.myplant
VERBOSE_ARTIFACT_UPLOAD=false
VERBOSE_LOGGING=false
```

Another example workflow configuration file (useable for python microservices) could look like this:

```
COMPONENT_NAME=auto
DEPLOYMENT_REPO=myplant-io/deployment
DEPLOYMENT_METHOD=legacy
DOCKER_REPOSITORY=myplant-io
#DOCKER_REPOSITORY=auto
#DOCKER_REPOSITORY=none
DOCKER_TAG_PREFIX=auto
PUBLISH_SHA=false
PUBLISH_TARGET=["myplant_nexus", "myplant_nexus_external"]
#PUBLISH_TARGET=none
PUSH_DEPLOY_TARGET=["staging-alpha-io/${COMPONENT_NAME}.yaml"]
PRE_RELEASE_DEPLOY_TARGET=none
RELEASE_DEPLOY_TARGET=["production-io/${COMPONENT_NAME}.yaml"]
SONAR_PREFIX=io.myplant
VERBOSE_LOGGING=false
```

**Hints:**
The files can slightly vary between build tools (the above one is for gradle),
so please make sure that it contains everything that is needed.

The following might also help during issue resolution:

- Most docker deployment actions (docker-deploy.yaml, yarn/yarn2-deploy) actions
  require a DOCKER_REPOSITORY variable.
- Yarn/Yarn2 projects will search for a node version in a .nvmrc file that is
  located in the project's root folder.
- Yarn/Yarn2 dependency checks need a SECURITY_LEVEL defined.
