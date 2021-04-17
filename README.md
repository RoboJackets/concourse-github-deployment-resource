# concourse-github-deployment-resource
[![GitHub license](https://img.shields.io/github/license/RoboJackets/concourse-github-deployment-resource)](https://github.com/RoboJackets/concourse-github-deployment-resource/blob/main/LICENSE) [![CI](https://concourse.robojackets.org/api/v1/teams/information-technology/pipelines/github-deployment/jobs/build-main/badge)](https://concourse.robojackets.org/teams/information-technology/pipelines/github-deployment)

Concourse resource for GitHub deployments

## Source configuration

- `repository_url` (required) - location of the repository
- `commit` (required) - commit that is being built
- `token` (required) - GitHub App token to use to authenticate
- `resource_name` (required) - name of the resource within Concourse
- `environment_urls` (optional) - map of environment names to URLs, to be passed to GitHub
- `debug` (optional) - whether to enable debug logging; must be set to boolean true if present

GitHub endpoint information and the URL to the Concourse job log will be derived from the environment.

## Behavior
Do not `get` this resource manually, it will not work.

### `check`
Returns an empty list.

### `in`
Writes the requested version out to disk for future `put`s to update. Intended only for implicit `get`s after `put`s.

### `out`

#### First `put`
Creates a new deployment. Specify `environment` in `params`. Must be called with no `inputs`.

#### Subsequent `put`s
Creates a deployment status for a previously-created deployment. Specify `state` in `params`. Refer to [the GitHub deployment status API documentation](https://docs.github.com/en/rest/reference/repos#create-a-deployment-status) for possible values.
