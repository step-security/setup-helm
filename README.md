[![StepSecurity Maintained Action](https://raw.githubusercontent.com/step-security/maintained-actions-assets/main/assets/maintained-action-banner.png)](https://docs.stepsecurity.io/actions/stepsecurity-maintained-actions)

# Setup Helm

Install a specific version of helm binary on the runner.

## Example

Acceptable values are `latest` or any semantic version string such as `v4.3.1`. The `latest` value follows Helm's current stable release and can change major versions.

```yaml
- uses: step-security/setup-helm@v5
  with:
     version: '<version>' # default is latest (stable)
  id: install
```

Alternatively, the version can be read from a [`.tool-versions`](https://asdf-vm.com/manage/configuration.html) file (the format used by [asdf](https://asdf-vm.com/) and [mise](https://mise.jdx.dev/)) via the `version-file` input:

```yaml
- uses: step-security/setup-helm@v5.0.0
  with:
     version-file: .tool-versions
  id: install
```

The action reads the version declared for the `helm` tool, for example:

```
helm 4.3.1
```

If both `version` and `version-file` are set, an explicitly requested `version` takes precedence and `version-file` is ignored (a warning is emitted). Because `version` defaults to `latest`, `version-file` is only ignored when you set `version` to a specific value other than `latest`; if `version` is left at its default, the version from `version-file` is used.

> [!NOTE]
> If fetching the latest version fails, the action retries a few times and then uses the built-in fallback version (currently v4.3.1). This is a last-known stable version and can become a major version behind what `latest` resolves to. To fail the step with the underlying error instead, set `latest-fallback` to `'false'`:
>
> ```yaml
> - uses: step-security/setup-helm@v5
>   with:
>      version: 'latest'
>      latest-fallback: 'false'
> ```
>
> If you rely on a certain version, you should explicitly request that version instead of `latest`.

Freshly downloaded Helm archives are verified against the SHA-256 checksum published beside the archive before extraction. With the default `checksum-policy: auto`, the action verifies a matching checksum when one is available as a bare 64-character hexadecimal digest at `<archive-url>.sha256`; when the checksum is not found, the action warns and continues downloading. Existing tool-cache entries continue to be reused without downloading or re-verifying the archive.

Set `checksum-policy` to control this behavior:

- `auto` (default): verify checksums when available and warn when they are absent.
- `required`: require a valid matching checksum from every download location.
- `if-available`: explicitly select the same verify-or-warn behavior as `auto`.
- `disabled`: skip checksum verification and emit a warning.

Malformed or mismatched checksums always fail unless verification is explicitly `disabled`. In `if-available` and `auto` modes, only a checksum response of HTTP 404 or 410 is treated as absent; other download errors fail closed.

Because each checksum is retrieved from the same mirror as its archive, verification detects corruption or an incomplete mirror sync; it does not protect against a mirror that has been compromised together with its checksum files.

An optional fallback mirror can be used when version resolution, archive download, or checksum verification fails against the primary location:

```yaml
- uses: step-security/setup-helm@v5
  with:
     version: 'v4.3.1'
     downloadBaseURL: 'https://primary.example/helm'
     downloadBaseURLFallback: 'https://fallback.example/helm'
```

The fallback archive follows the same checksum policy as the primary mirror. This capability follows the use case proposed by @mvtavares in [#227](https://github.com/Azure/setup-helm/pull/227).

The cached helm binary path is prepended to the PATH environment variable as well as stored in the helm-path output variable.
Refer to the action metadata file for details about all the inputs https://github.com/Azure/setup-helm/blob/master/action.yml

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
