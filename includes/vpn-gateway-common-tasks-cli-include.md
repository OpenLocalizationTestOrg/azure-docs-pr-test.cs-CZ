### <a name="tooview-local-network-gateways"></a>tooview brány místní sítě

tooview seznam hello brány místní sítě, použijte hello [seznam brány místní sítě az](https://docs.microsoft.com/cli/azure/network/local-gateway#list) příkaz.

```azurecli
az network local-gateway list --resource-group TestRG1
```

[!INCLUDE [modify-prefix](vpn-gateway-modify-ip-prefix-cli-include.md)]

[!INCLUDE [modify-gateway-IP](vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

### <a name="tooverify-hello-shared-key-values"></a>tooverify hello sdílené hodnoty klíče

Ověřte, zda hodnota klíče hello sdílené hello stejnou hodnotou, kterou jste použili pro konfiguraci zařízení VPN. Pokud není, spusťte připojení hello znovu s použitím hello hodnotu z hello zařízení nebo aktualizujte hello zařízení s hodnotou hello z návratové hello. Hello hodnoty se musí shodovat. tooview hello sdílený klíč, používání hello [az sítě seznam připojení vpn](https://docs.microsoft.com/cli/azure/network/vpn-connection#list).

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 --resource-group TestRG1
```
### <a name="tooview-hello-vpn-gateway-public-ip-address"></a>Brána sítě VPN hello tooview veřejnou IP adresu

toofind hello veřejnou IP adresu brány virtuální sítě, použijte hello [seznam veřejné ip sítě az](https://docs.microsoft.com/cli/azure/network/public-ip#list) příkaz. Pro snadné čtení výstup hello v tomto příkladu je formátovaný toodisplay hello seznam veřejné IP adresy ve formátu tabulky.

```azurecli
az network public-ip list --resource-group TestRG1 --output table
```
