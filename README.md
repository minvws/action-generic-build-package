# Generic build package GitHub Action

This repository provides a reusable GitHub Action for creating a source build package for an application and uploading it as a CI artifact.

## Usage

To use the action, add it to a workflow in your repository:

Here are basic examples of how you can integrate it in your project.

<details>
  <summary>Example workflow</summary>

This workflow is executed automatically on push of tags.

```yml
name: Build release package

on:
    push:
        tags:
            - v*

jobs:
    build-src-package:
        runs-on: ubuntu-latest
        steps:
            - name: Create package
              uses: minvws/action-generic-build-package@v1
              with:
                  working_directory: "."
                  include_paths: "app static tools app.conf.example HOSTING_CHANGELOG.md"
                  package_file_name: "nl-irealisatie-project-name"
```

Change the `working_directory`, `package_file_name` and `include_paths` according to the requirements of the project. See the [configuration section](#configuration).

Note that the workflow doesn't use a checkout step. This action already checks out the repository by default. You can use the `checkout_repository` option to disable this behavior.

### Configuration

The action has the following inputs:

- `include_paths`: A space-separated list with files and directories to include in the package relative to the working_directory.
- `package_file_name`: Name of the package (without extension), for example: `nl-example-package`.
- `working_directory`: The base directory containing the source code, only required if it does not need to be the root folder.
- `version_json_path`: The location where version.json needs to be stored. For example `public/version.json`. Default: `version.json`.
- `checkout_repository`: Boolean value inside string to enable or disable checkout repository
  in the action. For example `"true"` or `"false"`. Default `"true"`.

### Result

This action will create a `.tar.gz` file containing the source files specified by `include_paths` relative to the base working directory specified by `working_directory`.
The source package also contains an automatically generated `version.json` in the root of the package.
The default location of the source package can be changed by using `version_json_path` input parameter of the github action.

The source package will be available as an artifact and will look like: `<package_file_name>_<tag_version>.tar.gz`. For example `nl-example-package_v0.0.1.tar.gz`.
The uploaded artifact will have a limited lifetime depending on what is currently configured.

## Contributing

If you want to contribute a new pipeline, please check the reusable workflow guidelines in the
[GitHub documentation](https://docs.github.com/en/actions/using-workflows/reusing-workflows#creating-a-reusable-workflow).

## License

This repository is released under the EUPL 1.2 license. See [LICENSE](./LICENSE) for details.

## Part of iCore

This package is part of the iCore project.
