# CHAINBASE NODE OPERATOR SETUP

## SPEC
![SPECIFICATION](assets/spec.png)


## SETUP OPERATOR STEP BY STEP
1. Install Dependency
   ```bash
    sudo apt update & sudo apt upgrade -y
    sudo apt install ca-certificates zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev curl git wget make jq build-essential pkg-config lsb-release libssl-dev libreadline-dev libffi-dev gcc screen unzip lz4 -y
   ```
2. Install Docker (if you don't have it) -> check it first using `docker-compose --version` or `docker compose --version`
   ```bash
    sudo apt install docker.io
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    docker-compose --version
   ```
3. Install Go (if you don't have it) -> check it first using `go version`
   ```bash
    sudo rm -rf /usr/local/go
    curl -L https://go.dev/dl/go1.22.4.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
    echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile
    echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> $HOME/.bash_profile
    source .bash_profile
    go version
   ```
4. Install Eigen Layer CLI
   ```bash
    curl -sSfL https://raw.githubusercontent.com/layr-labs/eigenlayer-cli/master/scripts/install.sh | sh -s
    export PATH=$PATH:~/bin
    eigenlayer --version
   ```
5. Clone Chainbase AVS repo
   ```bash
    git clone https://github.com/chainbase-labs/chainbase-avs-setup

    cd chainbase-avs-setup/holesky
   ```
6. Create / Import EigenLayer Wallet
   - To Create Wallet Use
   ```bash
   eigenlayer operator keys create --key-type ecdsa opr
   ```
     - Enter Password
     - Back Up The Wallet Private Key (file : `/root/.eigenlayer/operator_keys/opr.ecdsa.key.json`)
     - Press `ctrl+c+enter` to exit from create wallet process
   - To Import Wallet Use
   ```
   eigenlayer operator keys import --key-type ecdsa opr PRIVATEKEY
   ```
   - To Export EVM WALLET
   ```bash
   eigenlayer operator keys export -k ecdsa -p /root/.eigenlayer/operator_keys/opr.ecdsa.key.json
   ```
7. Configure and register Operator **(NEED 1 HOLESKY ETH FOR OPERATOR REGISTRATION FEE)**
   ```bash
   eigenlayer operator config create
   ```
   fill up the prompt with : 
   - operator address: Your Eigenlayer ETH address
   - earnings address: press Enter
   - ETH rpc url: https://ethereum-holesky-rpc.publicnode.com
   - network: holesky
   - signer type: local_keystore
   - ecdsa key path:: /root/.eigenlayer/operator_keys/opr.ecdsa.key.json
8. Configure metadata.json
   ```bash
   nano metadata.json
   ```
   fill up the details like this example, remember this just example, change something that need to change
   ```json
    {
        "name": "YOUR MONIKER / NAME",
        "website": "WEBSITE URL / GITHUB URL",
        "description": "ANY DESCRIPTION",
        "logo": "ANY PNG WITH MAX SIZE 1 MB",
        "twitter": "YOUR TWITTER URL"
    }
   ```
   now copy all data inside metadata.json and save with `ctrl+x` press `y` and press `enter`. and follow this step :
   - create github repository.
   - create file `metadata.json` and paste metadata.json from step before.
   - optional you can also upload your logo here and change your metadata logo with your logo url from github
9.  Configure operator.yml
    ```bash
    nano operator.yml
    ```
    - find for `metadata_url:`.
    - fill up with your metadata.json raw url from github.
    - save with `ctrl+x` press `y` and press `enter`.
10. Run The Operator
    ```bash
    eigenlayer operator register operator.yaml
    ```
11. Check The Operator Status
    ```bash
    eigenlayer operator status operator.yaml
    ```
12. DONE