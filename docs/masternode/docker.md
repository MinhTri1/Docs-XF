Alternatively to using our simple tool `tmn`, people or companies with existing infrastructure might want to directly run our docker images for more flexibility.

To achieve that, you just need to run our `XinFin Network/node` docker image.

This image runs the [XinFin Network go client](https://github.com/XinFin Network/XinFin Network) with some automation added on top.

## Tags

| Environment | Tag        |
| ----------- | ---------- |
| Testnet     | `:testnet` |
| Mainnet     | `:stable`  |

## Environment variables

| Variable        | Required | Default         | Example                                    | Description                                                                                                      |
| --------------- | -------- | --------------- | ------------------------------------------ | --------------------------------------------------------------------------------------------------------------- |
| `IDENTITY`      | False    | Random name     | `my-super-node`                            | The name of your masternode                                                                                     |
| `PASSWORD`      | False    | Random password | `S5G@fS3A8+*g-)ftYc`                       | The password used by the the client to localy encrypt its account                                               |
| `PRIVATE_KEY`   | False    | Random account  | `0xa3f5195...`                             | The private key of the account who will identify the node and receive txs fees                                  |
| `BOOTNODES`     | True     | None            | `enode://4d3...@1.2.3.4:30301,enode://...` | The comma separated list of bootnodes. Find them [here](https://docs.xinfin.org/general/networks/)           |
| `VERBOSITY`     | False    | 3               | `4`                                        | The level of logging (default one should be enough)                                                             |
| `NETWORK_ID`    | False    | 89              | `88`                                       | The network id of the join your node is joining. Find them [here](https://docs.xinfin.org/general/networks/) |
| `WS_SECRET`     | False    | None            | `d$M4J4\5gLAz%Zjn>%`                       | The password to send data to the stats website                                                                  |
| `NETSTATS_HOST` | False    | netstats-server | `https://stats.xinfin.org`              | The stats website to report to, regarding to your environment                                                   |
| `NETSTATS_PORT` | False    | 3000            | `443`                                      | The port used by the stats website (usually 443)                                                                |
| `ANNOUNCE_TXS`  | False    | None            | `True`                                     | Enable reporting transactions via RPC/WS                                                                        |

## Ports

| Exposed | Protocol | Description               |
| ------- | --------- | ------------------------- |
| `30303` | tcp       | XinFin Network client p2p port |
| `30303` | udp       | XinFin Network client p2p port |
| `8545`  | tcp       | Blockchain RPC            |
| `8546`  | tcp       | Blockchain WS             |

## Filesystem

| Path                  | Content   |
| --------------------- | --------- |
| `/XinFin Network/data`     | Chaindata |
| `/XinFin Network/keystore` | Accounts  |

## Examples
`docker run`

```bash
docker run -d --name masternode \
  -e IDENTITY=$MASTERNODE_NAME \
  -e PRIVATE_KEY=$MASTERNODE_PK \
  -e BOOTNODES=$BOOTNODES \
  -e NETSTATS_HOST=stats.xinfin.org \
  -e NETSTATS_PORT=443 \
  -e WS_SECRET=$STATS_WS_SECRET \
  -p 30303:30303 \
  -p 30303:30303/udp \
  -v chaindata:/XinFin Network/data \
  XinFin Network/node:stable
```

`docker-compose.yml`

```yml
version: "3.4"
services:
  masternode:
    image: XinFin Network/node:stable
    environment:
      IDENTITY: $MASTERNODE_NAME
      PRIVATE_KEY: $MASTERNODE_PK
      BOOTNODES: $BOOTNODES
      NETSTATS_HOST: stats.xinfin.org
      NETSTATS_PORT: 443
      WS_SECRET: $STATS_WS_SECRET
    ports:
      - 30303:30303
      - 30303:30303/udp
    volumes:
      - chaindata:/XinFin Network/data
```
