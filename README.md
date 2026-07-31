[![StepSecurity Maintained Action](https://raw.githubusercontent.com/step-security/maintained-actions-assets/main/assets/maintained-action-banner.png)](https://docs.stepsecurity.io/actions/stepsecurity-maintained-actions)

# Setup Helm

Install a specific version of helm binary on the runner.

## Example

Acceptable values are latest or any semantic version string like v3.5.0 Use this action in workflow to define which version of helm will be used. v2+ of this action only support Helm3.

```yaml
- uses: step-security/setup-helm@v5
  with:
     version: '<version>' # default is latest (stable)
  id: install
```

Alternatively, the version can be read from a [`.tool-versions`](https://asdf-vm.com/manage/configuration.html) file (the format used by [asdf](https://asdf-vm.com/) and [mise](https://mise.jdx.dev/)) via the `version-file` input:

```yaml
- uses: step-security/setup-helm@v5
  with:
     version-file: .tool-versions
  id: install
```

The action reads the version declared for the `helm` tool, for example:

```
helm 3.18.4
```

If both `version` and `version-file` are set, an explicitly requested `version` takes precedence and `version-file` is ignored (a warning is emitted). Because `version` defaults to `latest`, `version-file` is only ignored when you set `version` to a specific value other than `latest`; if `version` is left at its default, the version from `version-file` is used.

> [!NOTE]
> If something goes wrong with fetching the latest version the action will use the hardcoded default version (currently v3.18.4). If you rely on a certain version higher than the default, you should explicitly use that version instead of latest.

The cached helm binary path is prepended to the PATH environment variable as well as stored in the helm-path output variable.
Refer to the action metadata file for details about all the inputs https://github.com/step-security/setup-helm/blob/main/action.yml
