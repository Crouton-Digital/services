___
**ENDPOINTS**
___
**RPC:**
```https
https://rpc-celestia.t.crouton.digital
```
**API:**
```https
https://api-celestia.t.crouton.digital
```
**gRPC:**
```
grpc-celestia.t.crouton.digital:9390
```
**gRPC-web:**
```
grpc-web-celestia.t.crouton.digital:9391
```

**Peer:**
```
c13acb30480216ade8e182f7adc9206842d849b7@peer-celestia.t.crouton.digital:26956
```
___
**RECOVERY**
___

#### STATE SYNC

```bash
sudo systemctl stop celestia-appd ; \
celestia-appd tendermint unsafe-reset-all --home ~/.celestia-app/ --keep-addr-book
```
```bash
SNAP_RPC="https://rpc-celestia.t.crouton.digital:443"
```
```bash
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 100)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)
```
```bash
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
```
```bash
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.celestia-app/config/config.toml
```
```bash
peers="c13acb30480216ade8e182f7adc9206842d849b7@peer-celestia.t.crouton.digital:26956"
sed -i.bak -e  "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.celestia-app/config/config.toml
```
```bash
sudo systemctl restart celestia-appd && sudo journalctl -u celestia-appd -f -o cat
```
#### SNAPSHOT
```
Soon
```
