<img src="https://opensearch.org/assets/brand/SVG/Logo/opensearch_logo_default.svg" height="64px"/>

- [OpenSearch Project Helm-Charts](#opensearch-project-helm-charts)
- [Status](#status)
- [Version and Branching](#version-and-branching)
- [Installation](#installation)
- [Change Logs](#change-logs)
- [Contributing](#contributing)
- [Getting Help](#getting-help)
- [Code of Conduct](#code-of-conduct)
- [Security](#security)
- [License](#license)

## OpenSearch Project Helm Charts

A community repository for Helm Charts of OpenSearch Project.

## Status

[![Lint and Test Charts](https://github.com/opensearch-project/helm-charts/actions/workflows/lint-test.yaml/badge.svg)](https://github.com/opensearch-project/helm-charts/actions/workflows/lint-test.yaml)
 [![Release Charts](https://github.com/opensearch-project/helm-charts/actions/workflows/release.yaml/badge.svg)](https://github.com/opensearch-project/helm-charts/actions/workflows/release.yaml)

## Version and Branching
As of now, this helm-charts repository maintains these branches:
* _main_ (Version is 3.x.x for both `version` and `appVersion` in `Chart.yaml`)
* _2.x_ (Version is 2.x.x for both `version` and `appVersion` in `Chart.yaml`)
* _1.x_ (Version is 1.x.x for both `version` and `appVersion` in `Chart.yaml`)
* _gh-pages_ (Reserved branch for publishing helm-charts through github pages)
<br>

Contributors should choose the corresponding branch(es) when commiting their change(s):
* If you have a change for a specific version, only open PR to specific branch
* If you have a change for all available versions, first open a PR on `main`, then open a backport PR with `[backport 1.x]` in the title, with label `backport 1.x`, etc.
* No changes should be commited to `gh-pages` by any contributor, as this branch should be only changed by github actions `chart-releaser`

## Kubernetes Version Support
* This helm-chart repository is tested with kubernetes version 1.19 and above

## Installation

To install the OpenSearch Helm charts, execute the following commands:

```shell
helm repo add opensearch https://opensearch-project.github.io/helm-charts/
helm repo update
```

Once the charts repository reference is added, you can run the following command to see the charts.

```shell
helm search repo opensearch
```

You can now deploy charts with this command.

```shell
helm install my-deployment opensearch/<chart name>
```

Please see the `README.md` in the [OpenSearch](charts/opensearch) and [OpenSearch Dashboards](charts/opensearch-dashboards) directories for installation instructions.


### Breaking Change 
For OpenSearch Chart version 2.18.0 and App Version OpenSearch version 2.12.0 and above require a custom strong password to be provided in order to setup demo admin user during the first time cluster installation, without this password the cluster would not spin up, unless demo config install is disabled or the cluster security settings are handled by the user.

Note: This won’t impact users who have already installed the cluster and created the security index. It only affects users starting a brand new cluster with OpenSearch version 2.12.0 and above.

The custom admin password can be supplied by adding the environment variable `OPENSEARCH_INITIAL_ADMIN_PASSWORD` in the `value.yml` as:
```
extraEnvs:
  - name: OPENSEARCH_INITIAL_ADMIN_PASSWORD
    value: <strong-password>
```
or during the installation example as `helm install opensearch opensearch/opensearch --set extraEnvs[0].name=OPENSEARCH_INITIAL_ADMIN_PASSWORD,extraEnvs[0].value=$OPENSEARCH_INITIAL_ADMIN_PASSWORD`


### Notes About Default Installation

By default, on startup, the `install_demo_configuration.sh` is run via the `opensearch-docker-entrypoint.sh` script if `DISABLE_INSTALL_DEMO_CONFIG` is not `true`.

If custom certificates are used and `allow_unsafe_democertificates` is set to `false` in the configuration, this can prevent pods from starting with the following error: `Demo certificates found but plugins.security.allow_unsafe_democertificates is set to false.`

This can be solved by adding an environment variable in the `value.yml`:
```
extraEnvs:
  - name: DISABLE_INSTALL_DEMO_CONFIG
    value: "true"
```

## Change Logs

Please review the [OpenSearch](charts/opensearch/CHANGELOG.md) and the
[OpenSearch Dashboards](charts/opensearch-dashboards/CHANGELOG.md) change logs for the latest
release details.

## Contributing

See [developer guide](DEVELOPER_GUIDE.md) and [how to contribute to this project](CONTRIBUTING.md).

## Getting Help

If you find a bug, or have a feature request, please don't hesitate to open an issue in this repository.

For more information, see [project website](https://opensearch.org/) and [documentation](https://opensearch.org/docs). If you need help and are unsure where to open an issue, try [forums](https://discuss.opendistrocommunity.dev/).

## Code of Conduct

This project has adopted the [Amazon Open Source Code of Conduct](CODE_OF_CONDUCT.md). For more information see the [Code of Conduct FAQ](https://aws.github.io/code-of-conduct-faq), or contact [opensource-codeofconduct@amazon.com](mailto:opensource-codeofconduct@amazon.com) with any additional questions or comments.

## Security

If you discover a potential security issue in this project we ask that you notify AWS/Amazon Security via our [vulnerability reporting page](http://aws.amazon.com/security/vulnerability-reporting/). Please do **not** create a public GitHub issue.

## License

This project is licensed under the [Apache v2.0 License](LICENSE.txt).
