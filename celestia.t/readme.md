## Endpoints:

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
grpc-celestia.t.crouton.digital:10090
```
**gRPC-web:**
```
grpc-web-celestia.t.crouton.digital:10091
```

**Peer:**
```
28a8540126219298f40986d686a63fce401f9a14@peer-celestia.t.crouton.digital:27656
```
___
## Recovery

```python
STATE-SYNC
```
```
sudo systemctl stop celestia-appd \
celestia-appd tendermint unsafe-reset-all --home ~/.celestia-app/ --keep-addr-book
```
```
SNAP_RPC="https://rpc-celestia.t.crouton.digital:443"
```
```
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 100)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)
```
```
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.celestia-app/config/config.toml
```
```
sudo systemctl restart celestia-appd && sudo journalctl -u celestia-appd -f -o cat
```
### Snapshot
```
Soon
```
