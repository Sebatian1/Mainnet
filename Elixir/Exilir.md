### System Update:
```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt install curl tar wget clang pkg-config protobuf-compiler libssl-dev jq build-essential protobuf-compiler bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
```
### Install Docker:
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
rm $HOME/get-docker.sh
```
### Creat validator env:
```
mkdir $HOME/elixir
nano $HOME/elixir/validator.env
```
Fill in the information for **STRATEGY_EXECUTOR_DISPLAY_NAME, STRATEGY_EXECUTOR_BENEFICIARY, and SIGNER_PRIVATE_KEY.**
#### Example:
```
# Valid environments are "prod" and "testnet"
ENV=prod

# Allowed characters A-Z, a-z, 0-9, _, -, and space
STRATEGY_EXECUTOR_DISPLAY_NAME=

# The Ethereum address to receive ELX rewards for this validator
STRATEGY_EXECUTOR_BENEFICIARY=

# A private key used only for this validator. Note: Does not begin with "0x"
SIGNER_PRIVATE_KEY=
```
### Run elixir node:
```
docker run -d \
  --env-file $HOME/elixir/validator.env \
  --name elixir \
  --restart unless-stopped \
  -p 17690:17690 \
  elixirprotocol/validator
```
### Upgrading your validator:
```
docker stop elixir
docker rm elixir
docker pull elixirprotocol/validator
```
```
docker run -d \
  --env-file $HOME/elixir/validator.env \
  --name elixir \
  --restart unless-stopped \
  -p 17690:17690 \
  elixirprotocol/validator
```
### Remove elixir node:
```
docker stop elixir
docker rm elixir
rm -rf $HOME/elixir
```
