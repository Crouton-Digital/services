___
**ENDPOINTS**
___
**RPC:**
```https
https://rpc-haqq.m.crouton.digital
```
**API:**
```https
https://api-haqq.m.crouton.digital
```
**gRPC:**
```
grpc-haqq.m.crouton.digital:9190
```
**gRPC-web:**
```
grpc-web-haqq.m.crouton.digital:9191
```

**Peer:**
```
fc49c7e9a08e12a3324fad5790605e0639f9d462@peer-haqq.m.crouton.digital:26756
```
___
**RECOVERY**
___

#### STATE SYNC

```bash
sudo systemctl stop haqqd ; \
haqqd tendermint unsafe-reset-all --home ~/.haqqd/ --keep-addr-book
```
```bash
SNAP_RPC="https://rpc-haqq.m.crouton.digital:443"
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
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.haqqd/config/config.toml
```
```bash
peers="fc49c7e9a08e12a3324fad5790605e0639f9d462@peer-haqq.m.crouton.digital:26756"
sed -i.bak -e  "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.haqqd/config/config.toml
```
```bash
sudo systemctl restart haqqd && sudo journalctl -u haqqd -f -o cat
```
#### SNAPSHOT
```
Soon
```
