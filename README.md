# Github Template Repository

Include workflow templates (for github actions).

** WARNING ** This is a public repo. For more info see here:  
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

The files can slightly vary between build tools (the above one is for gradle),
so please make sure that it contains everything that is needed.

**Hint:**
Common docker deployment (like the ones we use in python) actions require a
DOCKER_REGISTRY variable. Yarn/Yarn2 dependency checks need a SECURITY_LEVEL
defined.
