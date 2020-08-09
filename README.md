# concourse-github-deployment-resource
[![GitHub license](https://img.shields.io/github/license/RoboJackets/concourse-github-deployment-resource)](https://github.com/RoboJackets/concourse-github-deployment-resource/blob/main/LICENSE) [![CI](https://concourse.robojackets.org/api/v1/teams/information-technology/pipelines/github-deployment/jobs/build-main/badge)](https://concourse.robojackets.org/teams/information-technology/pipelines/github-deployment)

Concourse resource for GitHub deployments

## Source configuration

- `repository` (required) - the resource where your source code is provided
- `pull_request` (optional) - the resource where pull requests are provided
- `token` (required) - GitHub App token to use to authenticate
- `resource_name` (required) - the name of the resource within Concourse
- `debug` (optional) - whether to enable debug logging; must be set to boolean true if present
- `environment_urls` (optional) - map of environment names to URLs, to be passed to GitHub

GitHub endpoint information, commit SHA, and the URL to the Concourse job log will be derived from the environment. `repository` will be used first, then `pull_request` if it is not available.

## Behavior
Do not `get` this resource manually, it will not work.

### `check`
Returns an empty list.

### `in`
Writes the requested version out to disk for future `put`s to update. Intended only for implicit `get`s after `put`s.

### `out`
You may want to [manually configure inputs](https://concourse-ci.org/jobs.html#schema.step.put-step.inputs) for better performance if you have large resources in your pipeline.

#### First `put`
Creates a new deployment. Specify `environment` in `params`.

#### Subsequent `put`s
Creates a deployment status for a previously-created deployment. Specify `state` in `params`. Refer to [the GitHub deployment status API documentation](https://docs.github.com/en/rest/reference/repos#create-a-deployment-status) for possible values.
