# Agent configuration

All agents use the same method of configuration which is a multi-layered config approach which allows for easy overriding of default configs. Each layer overrides overlapping values in the previous.

### Config layers

1. Base configuration which takes the form `rust/config/<env_name>/<config_name>.json` all of these are loaded automatically.
2. Config files passed in via the `CONFIG_FILES` env var are loaded next; this env var should be a comma separated list of paths to json files that should be loaded in order from first to last.
3. `HYP_BASE_` prefixed env vars will be read next.
4. `HYP_<AGENT>_` prefixed env vars will be read last and ONLY for the current agent. I.e. `RELAYER`, `VALIDATOR`, and `SCRAPER` will only read their respective prefixes.

### Using environment variables

The config file format is the preferred way to set things which are not secret and do not need to change for each run as it is the easiest format to inspect and edit. Every config value in the file can be set as an env var if you use the correct name, however, there are a few env vars that cannot be set in the config such as `CONFIG_FILES`.

`HYP_BASE_` and `HYP_<AGENT>_` are equivalent prefixes with the only difference being which order they are loaded in and can refer to all of the config values in the config files.

The env name will be one of those two prefixes, and then the underscore-separated path of the uppercased path components to the config value.

For example:

* `{ "db": "/path/to/dir" }` can be set with `HYP_BASE_DB="/path/to/dir"` or `HYP_RELAYER_DB="/path/to/dir"`
* `{ "chains": { "ethereum": { "name": "ethereum" } } }` (abbreviated as `chains.ethereum.name` or `chains.<chain_name>.name`) can be set with `HYP_BASE_CHAINS_ETHEREUM_NAME="ethereum"` or `HYP_VALIDATOR_CHAINS_ETHEREUM_NAME="ethereum"` or `HYP_RELAYER_CHAINS_ETHEREUM_NAME="ethereum"` ...
* `{ "chains": { "avalanche": { "connection": { "url": "https://some-url.com" } } } }` (abbreviated as `chains.avalanche.connection.url` or `chains.<chain_name>.connection.url`) can be set with `HYP_BASE_CHAINS_AVALANCHE_CONNECTION_URL="https://some-url.com"` or `HYP_BASE_VALIDATOR_AVALANCHE_CONNECTION_URL="https://some-url.com"` and so on ...

### Config files with docker

Running with the agent in docker adds an extra layer of complexity because the config files need to be accessible from within the docker container. The base configs that can be found in [the repo](https://github.com/hyperlane-xyz/hyperlane-monorepo/tree/main/rust/config) are already part of the provided docker image and will all be loaded by default.

Mounting a single config file can be done with the flag `--mount type=bind,source=$LOCAL_CONFIG_PATH,target=/config/$CONFIG_NAME,readonly` and then adding the config file to `CONFIG_FILES` so it is loaded.

Mounting an entire directory can be done with the flag `--mount source=$LOCAL_CONFIG_DIR_PATH,target=/config,readonly` then adding the individual config files to `CONFIG_FILES` that you want so they are loaded.

Full example, suppose you have a config file `ethereum.json` and want to run the validator with it. `docker run -it --mount type=bind,source=/home/workspace/ethereum.json,target=/config/ethereum.json,readonly -e CONFIG_FILES=/config/ethereum.json gcr.io/abacus-labs-dev/hyperlane-agent:44361db-20230209-031017 ./validator`



