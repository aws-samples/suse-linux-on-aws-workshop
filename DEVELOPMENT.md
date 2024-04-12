# Development

The GitHub repository https://github.com/aws-samples/suse-linux-on-aws-workshop is the single source of truth.
All development takes place there and when a release is ready, it has to be pushed to AWS workshop studio.
After publication in AWS workshop studio, the content on https://catalog.workshops.aws/suse-linux-on-aws will be aligned with the GitHub repository.

Never update the workshop directly in AWS workshop studio, this will cause both repositories to drift.
Always create Pull Requests in GitHub against the main branch.

## Update workshop in AWS workshop studio

Use your Fork and ensure that you have three remotes configured:
- origin: `your fork`
- upstream: `aws-samples/suse-linux-on-aws-workshop`
- ws: `codecommit://suse-linux-on-aws`

Example:
```
origin	git@github.com:wombelix/fork_aws-samples_suse-linux-on-aws-workshop.git (fetch)
origin	git@github.com:wombelix/fork_aws-samples_suse-linux-on-aws-workshop.git (push)
upstream	git@github.com:aws-samples/suse-linux-on-aws-workshop.git (fetch)
upstream	git@github.com:aws-samples/suse-linux-on-aws-workshop.git (push)
ws	codecommit://suse-linux-on-aws (fetch)
ws	codecommit://suse-linux-on-aws (push)
```

Ensure you are in the `main` branch and have the latest changes from `upstream`:
```
git checkout main
git pull upstream main
```

Get and apply the repository credentials from AWS workshop studio.
Afterwards push from the `main` to the `mainline` branch.
```
git push ws main:mainline
```

Navigate to the workshop in AWS workshop studio and verify that the pushed changes build successful.
Publish the latest build and Pin the Build ID.
Open https://catalog.workshops.aws/suse-linux-on-aws and confirm that the correct version is published.