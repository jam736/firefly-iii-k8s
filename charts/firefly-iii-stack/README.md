# firefly-iii-stack

![Version: 0.8.5](https://img.shields.io/badge/Version-0.8.5-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square)

Installs Firefly III stack (db, app, importer)
**Homepage:** <https://github.com/firefly-iii/kubernetes>
## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| morremeyer | <firefly-iii@mor.re> |  |
## Source Code

* <https://github.com/firefly-iii/kubernetes>
## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://firefly-iii.github.io/kubernetes/ | firefly-db | 0.2.7 |
| https://firefly-iii.github.io/kubernetes/ | firefly-iii | 1.9.4 |
| https://firefly-iii.github.io/kubernetes/ | importer | 1.4.2 |

## Upgrading

When a release introduces breaking changes, this section outlines the manual actions that need to be taken.

### From 0.4.0 to 0.5.0

No breaking changes in this release, but you can now use the helm repository directly from https://firefly-iii.github.io/kubernetes/.

### From 0.1.0 to 0.2.0

The **firefely** wrapper chart has been renamed to **firefly-iii-stack**. All charts are now contained directly in the `charts` directory, following best practices for chart repositories.

### From 0.0.5 to 0.1.0

The `firefly-iii` chart has been overhauled and contains breaking changes. Please check out [the charts upgrade notes](charts/firefly-iii/README.md#from-004-to-100).

### From 0.0.4 to 0.0.5

The `firefly-csv` chart has been removed as the CSV importer has been integrated into the importer. Please check out the new [`importer` chart](charts/importer/README.md).

### From 0.0.3 to 0.0.4

The storage class and access modes have been changed to match more setups without the need for configuration. If you want to keep the old settings, set the following values:

```yaml
firefly-iii:
  storage:
    class: nfs-client
    accessModes: ReadWriteMany

firefly-csv:
  storage:
    class: nfs-client
    accessModes: ReadWriteMany

firefly-db:
  storage:
    class: nfs-client
    accessModes: ReadWriteMany
```

### From 0.0.2 to 0.0.3

The `firefly-iii` and `firefly-csv` charts have been updated and now support configuring ingress annotations.

To keep the old annotations, add the following values:

```yaml
firefly-iii:
  ingress:
    annotations:
      kubernetes.io/ingress.class: "nginx"
      nginx.ingress.kubernetes.io/proxy-buffer-size: "16k"

firefly-csv:
  ingress:
    annotations:
      kubernetes.io/ingress.class: "nginx"
      nginx.org/client-max-body-size: "0"
      nginx.ingress.kubernetes.io/proxy-buffer-size: "16k"
```

## Development and testing

### Dependency chart overrides

Unfortunately, helm does not yet have a neat mechanism to dynamically override chart dependencies for local development/testing.

Therefore, you'll need to override the dependencies manually for local testing and development. To do so, you need to update the `Chart.yaml` file.

For every dependency, replace `repository: https://firefly-iii.github.io/kubernetes/` with `repository: file://../${dependency_chart_name}`.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| firefly-db.configs.PGPASSWORD | string | `"YOURDBPASSWORD"` |  |
| firefly-db.enabled | bool | `true` |  |
| firefly-iii.enabled | bool | `true` | Set to false to not deploy Firefly III |
| importer.enabled | bool | `true` | Set to false to not deploy the importer |
