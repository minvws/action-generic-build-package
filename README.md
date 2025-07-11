# Generic build package

- This pipeline is designed to export a source build package for an application.
- The pipeline is designed to be as generic as possible, so they can be easily reused in any project.
- This repository is a part of the generic GitHub Actions pipeline collection that can be used in any project.

## Usage

Here are basic examples of how you can integrate it in your project.

<details>
  <summary>Example workflow</summary>

This workflow is executed automatically on push of tags.

In the code below you need to change the `working_directory` and `package_file_name` and `include_paths` according to the requirements of the project.
See the [configuration section](#configuration).

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
      # Using the action
      - name: Create source package
        uses: minvws/action-generic-build-package/.github/actions/build-package@main
        with:
          working_directory: "."
          include_paths: "app static tools app.conf.example HOSTING_CHANGELOG.md"
          package_file_name: "nl-irealisatie-project-name"
```

</details>

<details>
  <summary>Example workflow self checkout</summary>

This workflow is executed automatically on the push of tags. The workflow will check out the repo and the action won't.

In the code below you need to replace the `<package_file_name>` and `<working_directory>`. See the [configuration section](#configuration).

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
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Extract version from tag
        shell: bash
        run: echo "RELEASE_VERSION=${GITHUB_REF#refs/*/}" >> $GITHUB_ENV

      # Using the action
      - name: Build src package
        uses: minvws/action-generic-build-package/.github/actions/build-package@main
        with:
          checkout_repository: "false"
          include_paths: "app static tools app.conf.example HOSTING_CHANGELOG.md"
          package_file_name: "nl-irealisatie-project-name"
```

</details>

### Configuration

The action has inputs. The inputs are:

- include_paths: A space seperated list with files and directories to include in the package relative to the working_directory.
- package_file_name: Name of the package (without extension), for example: `nl-example-package`.
- working_directory: The base directory containing the source code, only required if it does not need to be the root folder.
- version_json_path: The location where version.json needs to be stored. For example `public/version.json`. Default: `version.json`.
- checkout_repository: Boolean value inside string to enable or disable checkout repository
  in the action. For example `'true'` or `'false'`. Default `'true'`.

### Result

This action will create a `.tar.gz` file containing the source files specified by `include_paths` relative to the base working directory specified by `working_directory`.
The source package also contains an automatically generated `version.json` in the root of the package.
The default location of the source package can be changed by using `version_json_path` input parameter of the github action.

The source package will be available as an artifact and will look like: `<package_file_name>_<tag_version>.tar.gz`. For example `nl-example-package_v0.0.1.tar.gz`.
The uploaded artifact will have a limited lifetime depending on what is currently configured.

## Contributing

If you want to contribute a new pipeline, please check the reusable workflow guidelines in the
[GitHub documentation](https://docs.github.com/en/actions/using-workflows/reusing-workflows#creating-a-reusable-workflow).
