# 🐙 Octavia CLI

Octavia CLI is a tool to manage Airbyte configuration in YAML.
It has the following features:
* Scaffolding of a readable directory architecture that will host the YAML configs.
* Auto-generation of YAML config file that matches the resources' schemas.
* Manage Airbyte resources with YAML config files.
* Safe resources update through diff display and validation.
* Simple secret management to avoid versioning credentials.

## Disclaimer
The project is in **alpha** version. 
Readers can refer to our [opened GitHub issues](https://github.com/airbytehq/airbyte/issues?q=is%3Aopen+is%3Aissue+label%3Aarea%2Foctavia-cli) to check the ongoing work on this project.

# Install

## 1. Install and run Docker
We are packaging this CLI as a Docker image to avoid dependency hell, **[please install and run Docker if you are not](https://docs.docker.com/get-docker/)**. 

## 2.a If you are using ZSH / Bash:
```bash
curl -o- https://raw.githubusercontent.com/airbytehq/airbyte/master/octavia-cli/install.sh | bash
```

This script:
1. Pulls the [octavia-cli image](https://hub.docker.com/r/airbyte/octavia-cli/tags) from our Docker registry.
2. Creates an `octavia` alias in your profile.
3. Creates a `~/.octavia` file whose values are mapped to the octavia container's environment variables.

## 2.b If you want to directly run the CLI without alias in your current directory:
```bash
mkdir my_octavia_project_directory # Create your octavia project directory where YAML configurations will be stored.
docker run -i --rm -v ./my_octavia_project_directory:/home/octavia-project --network host -e AIRBYTE_URL="http://localhost:8000" airbyte/octavia-cli:dev
```

# Commands reference

## `octavia` command flags
| **Flag**         | **Description**            | **Env Variable**       | **Default**                                            |
|------------------|----------------------------|------------------------|--------------------------------------------------------|
| `--airbyte-url`  | Your Airbyte instance URL. | `AIRBYTE_URL`          | `http://localhost:8000`                                |
| `--workspace-id` | Your Airbyte workspace id. | `AIRBYTE_WORKSPACE_ID` | The first workspace id found on your Airbyte instance. |

## `octavia` subcommands

| **Command**                             | **Usage**                                                                           |
|-----------------------------------------|-------------------------------------------------------------------------------------|
| **`octavia init`**                        | Initialize required directories for the project.                                  |
| **`octavia list connectors sources`**     | List all sources connectors available on the remote Airbyte instance.             |
| **`octavia list connectors destination`** | List all destinations connectors available on the remote Airbyte instance.        |
| **`octavia list workspace sources`**      | List existing sources in current the Airbyte workspace.                           |
| **`octavia list workspace destinations`** | List existing destinations in the current Airbyte workspace.                      |
| **`octavia list workspace connections`**  | List existing connections in the current Airbyte workspace.                       |
| **`octavia generate source`**             | Generate a local YAML configuration for a new source.                             |
| **`octavia generate destination`**        | Generate a local YAML configuration for a new destination.                        |
| **`octavia generate connection`**         | Generate a local YAML configuration for a new connection.                         |
| **`octavia apply`**                       | Create or update Airbyte remote resources according to local YAML configurations. |

### `octavia init`
The `octavia init` commands scaffolds the local directory architecture which is required for running `octavia generate` and `octavia apply` commands.

#### Example
```bash
$ mkdir my_octavia_project && cd my_octavia_project
$ octavia init
🐙 - Octavia is targetting your Airbyte instance running at http://localhost:8000 on workspace e1f46f7d-5354-4200-aed6-7816015ca54b.
🐙 - Project is not yet initialized.
🔨 - Initializing the project.
✅ - Created the following directories: sources, destinations, connections.
$ ls
connections  destinations sources
```

### `octavia list connectors sources`
List all the source connectors currently available on your Airbyte instance.

#### Example
```bash
$ octavia list connectors sources
🐙 - Octavia is targetting your Airbyte instance running at http://localhost:8000 on workspace e1f46f7d-5354-4200-aed6-7816015ca54b.
NAME                            DOCKER REPOSITORY                              DOCKER IMAGE TAG  SOURCE DEFINITION ID
Airtable                        airbyte/source-airtable                        0.1.1             14c6e7ea-97ed-4f5e-a7b5-25e9a80b8212
AWS CloudTrail                  airbyte/source-aws-cloudtrail                  0.1.4             6ff047c0-f5d5-4ce5-8c81-204a830fa7e1
Amazon Ads                      airbyte/source-amazon-ads                      0.1.3             c6b0a29e-1da9-4512-9002-7bfd0cba2246
Amazon Seller Partner           airbyte/source-amazon-seller-partner           0.2.15            e55879a8-0ef8-4557-abcf-ab34c53ec460
```

### `octavia list connectors destinations`
List all the destinations connectors currently available on your Airbyte instance.

#### Example
```bash
$ octavia list connectors destinations
🐙 - Octavia is targetting your Airbyte instance running at http://localhost:8000 on workspace e1f46f7d-5354-4200-aed6-7816015ca54b.
NAME                                  DOCKER REPOSITORY                                 DOCKER IMAGE TAG  DESTINATION DEFINITION ID
Azure Blob Storage                    airbyte/destination-azure-blob-storage            0.1.3             b4c5d105-31fd-4817-96b6-cb923bfc04cb
Amazon SQS                            airbyte/destination-amazon-sqs                    0.1.0             0eeee7fb-518f-4045-bacc-9619e31c43ea
BigQuery                              airbyte/destination-bigquery                      0.6.11            22f6c74f-5699-40ff-833c-4a879ea40133
BigQuery (denormalized typed struct)  airbyte/destination-bigquery-denormalized         0.2.10            079d5540-f236-4294-ba7c-ade8fd918496
```

### `octavia list connectors destinations`
List all the destinations connectors currently available on your Airbyte instance.

#### Example
```bash
$ octavia list connectors destinations
🐙 - Octavia is targetting your Airbyte instance running at http://localhost:8000 on workspace e1f46f7d-5354-4200-aed6-7816015ca54b.
NAME                                  DOCKER REPOSITORY                                 DOCKER IMAGE TAG  DESTINATION DEFINITION ID
Azure Blob Storage                    airbyte/destination-azure-blob-storage            0.1.3             b4c5d105-31fd-4817-96b6-cb923bfc04cb
Amazon SQS                            airbyte/destination-amazon-sqs                    0.1.0             0eeee7fb-518f-4045-bacc-9619e31c43ea
BigQuery                              airbyte/destination-bigquery                      0.6.11            22f6c74f-5699-40ff-833c-4a879ea40133
BigQuery (denormalized typed struct)  airbyte/destination-bigquery-denormalized         0.2.10            079d5540-f236-4294-ba7c-ade8fd918496
```

### `octavia list workspace sources`
List all the sources existing on your targeted Airbyte instance.

#### Example
```bash
$ octavia list workspace sources
NAME     SOURCE NAME  SOURCE ID
weather  OpenWeather  c4aa8550-2122-4a33-9a21-adbfaa638544
```

### `octavia list workspace destinations`
List all the destinations existing on your targeted Airbyte instance.

#### Example
```bash
$ octavia list workspace destinations
NAME   DESTINATION NAME  DESTINATION ID
my_db  Postgres          c0c977c2-48e7-46fe-9f57-576285c26d42
```

### `octavia list workspace connections`
List all the connections existing on your targeted Airbyte instance.

#### Example
```bash
$ octavia list workspace connections
NAME           CONNECTION ID                         STATUS  SOURCE ID                             DESTINATION ID
weather_to_pg  a4491317-153e-436f-b646-0b39338f9aab  active  c4aa8550-2122-4a33-9a21-adbfaa638544  c0c977c2-48e7-46fe-9f57-576285c26d42
```

### `octavia generate source <DEFINITION_ID> <SOURCE_NAME>`
Generate a YAML configuration for a source.
The YAML file will be stored at `./sources/<resource_name>/configuration.yaml`.

| **Argument**    | **Description**                                                                             |
|-----------------|---------------------------------------------------------------------------------------------|
| `DEFINITION_ID` | The source connector definition id. Can be retrieved using octavia list connectors sources. |
| `SOURCE_NAME`   | The name you want to give to this source in Airbyte.                                          |

#### Example
```bash
$ octavia generate source d8540a80-6120-485d-b7d6-272bca477d9b weather
✅ - Created the source template for weather in ./sources/weather/configuration.yaml.
```

### `octavia generate destination <DEFINITION_ID> <DESTINATION_NAME>`
Generate a YAML configuration for a destination.
The YAML file will be stored at `./destinations/<destination_name>/configuration.yaml`.

| **Argument**       | **Description**                                                                                       |
|--------------------|-------------------------------------------------------------------------------------------------------|
| `DEFINITION_ID`    | The destination connector definition id. Can be retrieved using octavia list connectors destinations. |
| `DESTINATION_NAME` | The name you want to give to this destination in Airbyte.                                             |

#### Example
```bash
$ octavia generate destination 25c5221d-dce2-4163-ade9-739ef790f503 my_db
✅ - Created the destination template for my_db in ./destinations/my_db/configuration.yaml.
```

### `octavia generate connection --source <path-to-source-configuration.yaml> --destination <path-to-destination-configuration.yaml> <CONNECTION_NAME>`
Generate a YAML configuration for a connection.
The YAML file will be stored at `./connections/<connection_name>/configuration.yaml`.

| **Option**      | **Required** | **Description**                                                                            |
|-----------------|--------------|--------------------------------------------------------------------------------------------|
| `--source`      | Yes          | Path to the YAML configuration file of the source you want to create a connection from.    |
| `--destination` | Yes          | Path to the YAML configuration file of the destination you want to create a connection to. |

| **Argument**      | **Description**                                          |
|-------------------|----------------------------------------------------------|
| `CONNECTION_NAME` | The name you want to give to this connection in Airbyte. |

#### Example
```bash
$ octavia generate connection --source sources/weather/configuration.yaml --destination destinations/my_db/configuration.yaml weather_to_pg
✅ - Created the connection template for weather_to_pg in ./connections/weather_to_pg/configuration.yaml.
```

### `octavia apply`
TODO

| **Option**      | **Required** | **Description**                                                                            |
|-----------------|--------------|--------------------------------------------------------------------------------------------|
| `--file`        | No           | Path to the YAML configuration files you want to create or update.                          |
| `--force`       | No           | Does not display the update diff and updates without user prompt.                           |


#### Example
```bash
TODO
```


# Secret management
Sources and destinations configurations have credential fields that you **do not want to store as plain text and version on Git**.
`octavia` offers secret management through environment variables expansion:
```yaml
configuration:
  password: ${MY_PASSWORD}
```
If you have set a  `MY_PASSWORD` environment variable, `octavia apply` will load its value into the `password` field. 


# Developing locally
1. Install Python 3.8.12. We suggest doing it through `pyenv`
2. Create a virtualenv: `python -m venv .venv`
3. Activate the virtualenv: `source .venv/bin/activate`
4. Install dev dependencies: `pip install -e .\[tests\]`
5. Install `pre-commit` hooks: `pre-commit install`
6. Run the unittest suite: `pytest --cov=octavia_cli`
7. Iterate: please check the [Contributing](#contributing) for instructions on contributing.

## Build
Build the project locally (from the root of the repo):
```bash
SUB_BUILD=OCTAVIA_CLI ./gradlew build # from the root directory of the repo
```
# Contributing
1. Please sign up to [Airbyte's Slack workspace](https://slack.airbyte.io/) and join the `#octavia-cli`. We'll sync up community efforts in this channel.
2. Read the [execution plan](https://docs.google.com/spreadsheets/d/1weB9nf0Zx3IR_QvpkxtjBAzyfGb7B0PWpsVt6iMB5Us/edit#gid=0) and find a task you'd like to work on.
3. Open a PR, make sure to test your code thoroughly. 


# Changelog

| Version | Date       | Description      | PR                                                       |
|---------|------------|------------------|----------------------------------------------------------|
| 0.1.0   | 2022-03-15 | Alpha release    | [EPIC](https://github.com/airbytehq/airbyte/issues/10704)|
