<!--- release-section --->
<!--- release-section will be updated automatically through the auto-update gh action --->

# Continues Integration / Continues Delivery

This section describes how to release using myPlant's automated CI/CD workflow. More extensive explanation can be found in [[Confluence] How to release a (micro)service using Github](https://innio.atlassian.net/wiki/spaces/JHJAL/pages/2146009473/How+to+release+a+micro+service+using+Github)

### Deploy to `alpha`

Changes to `develop` will be deployed to `staging-alpha` on every push

---

### Deploy release candidate:

Stage (create or update) release branch:  
[workflow_dispatch: stage-release-branch.yml]({{repo_url}}/actions/workflows/stage-release-branch.yml)

```
gh workflow run stage-release-branch.yml -r develop
```

Then, gh actions will create a pre-release automatically  
Then, gh actions will deploy the pre-release to `staging-beta` automatically

---

### Deploy release:

**[Requires release candidate]** If release candidate is acceptable:  
[workflow_dispatch: publish-release.yml]({{repo_url}}/actions/workflows/publish-release.yml)

```
gh workflow run publish-release.yml -r release/v{TAG}
```

Then, gh actions will deploy release to production automatically  
Then, gh actions will delete release branch

<!--- release-section-end --->
