# Github Template Repository

Shared workflow templates (for github actions).

:warning: This is a public repo. :warning:  
To learn why this is the case, have a look at the following issue:
https://github.com/github/roadmap/issues/98

## Noteworthy

Next to the workflow files itself (e.g. _.github/gradle-deploy.yml_) you will
need a _.github/workflow-config.env_ file for the actions to work.

Here is an example workflow configuration file:

```
COMPONENT_NAME=auto
DEPLOYMENT_REPO=myplant-io/deployment
DOCKER_TAG_PREFIX=auto
GRADLE_BUILD_TASK=build test
GRADLE_DEPENDENCY_CHECK_TASK=cyclonedx dependencyCheckAnalyze
GRADLE_DEPLOY_TASK=dockerPushAndDeleteLocal -x test
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
  require a DOCKER_REGISTRY variable.
- Yarn/Yarn2 dependency checks need a SECURITY_LEVEL defined.
