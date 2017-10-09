### <a name="tooview-local-network-gateways"></a><span data-ttu-id="70b36-101">tooview brány místní sítě</span><span class="sxs-lookup"><span data-stu-id="70b36-101">tooview local network gateways</span></span>

<span data-ttu-id="70b36-102">tooview seznam hello brány místní sítě, použijte hello [seznam brány místní sítě az](https://docs.microsoft.com/cli/azure/network/local-gateway#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="70b36-102">tooview a list of hello local network gateways, use hello [az network local-gateway list](https://docs.microsoft.com/cli/azure/network/local-gateway#list) command.</span></span>

```azurecli
az network local-gateway list --resource-group TestRG1
```

[!INCLUDE [modify-prefix](vpn-gateway-modify-ip-prefix-cli-include.md)]

[!INCLUDE [modify-gateway-IP](vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

### <a name="tooverify-hello-shared-key-values"></a><span data-ttu-id="70b36-103">tooverify hello sdílené hodnoty klíče</span><span class="sxs-lookup"><span data-stu-id="70b36-103">tooverify hello shared key values</span></span>

<span data-ttu-id="70b36-104">Ověřte, zda hodnota klíče hello sdílené hello stejnou hodnotou, kterou jste použili pro konfiguraci zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="70b36-104">Verify that hello shared key value is hello same value that you used for your VPN device configuration.</span></span> <span data-ttu-id="70b36-105">Pokud není, spusťte připojení hello znovu s použitím hello hodnotu z hello zařízení nebo aktualizujte hello zařízení s hodnotou hello z návratové hello.</span><span class="sxs-lookup"><span data-stu-id="70b36-105">If it is not, either run hello connection again using hello value from hello device, or update hello device with hello value from hello return.</span></span> <span data-ttu-id="70b36-106">Hello hodnoty se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="70b36-106">hello values must match.</span></span> <span data-ttu-id="70b36-107">tooview hello sdílený klíč, používání hello [az sítě seznam připojení vpn](https://docs.microsoft.com/cli/azure/network/vpn-connection#list).</span><span class="sxs-lookup"><span data-stu-id="70b36-107">tooview hello shared key, use hello [az network vpn-connection-list](https://docs.microsoft.com/cli/azure/network/vpn-connection#list).</span></span>

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 --resource-group TestRG1
```
### <a name="tooview-hello-vpn-gateway-public-ip-address"></a><span data-ttu-id="70b36-108">Brána sítě VPN hello tooview veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="70b36-108">tooview hello VPN gateway Public IP address</span></span>

<span data-ttu-id="70b36-109">toofind hello veřejnou IP adresu brány virtuální sítě, použijte hello [seznam veřejné ip sítě az](https://docs.microsoft.com/cli/azure/network/public-ip#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="70b36-109">toofind hello public IP address of your virtual network gateway, use hello [az network public-ip list](https://docs.microsoft.com/cli/azure/network/public-ip#list) command.</span></span> <span data-ttu-id="70b36-110">Pro snadné čtení výstup hello v tomto příkladu je formátovaný toodisplay hello seznam veřejné IP adresy ve formátu tabulky.</span><span class="sxs-lookup"><span data-stu-id="70b36-110">For easy reading, hello output for this example is formatted toodisplay hello list of public IPs in table format.</span></span>

```azurecli
az network public-ip list --resource-group TestRG1 --output table
```
