### <a name="toomodify-hello-local-network-gateway-gatewayipaddress"></a><span data-ttu-id="1f4a1-101">Brána místní sítě hello toomodify 'gatewayIpAddress.</span><span class="sxs-lookup"><span data-stu-id="1f4a1-101">toomodify hello local network gateway 'gatewayIpAddress'</span></span>

<span data-ttu-id="1f4a1-102">Pokud hello zařízení VPN, které chcete tooconnect toohas změnit jeho veřejnou IP adresu, musíte toomodify hello místní síťové brány tooreflect, který změnit.</span><span class="sxs-lookup"><span data-stu-id="1f4a1-102">If hello VPN device that you want tooconnect toohas changed its public IP address, you need toomodify hello local network gateway tooreflect that change.</span></span> <span data-ttu-id="1f4a1-103">IP adresa brány Hello lze změnit bez předchozího odstranění stávajícího připojení brány sítě VPN (pokud nějaký máte).</span><span class="sxs-lookup"><span data-stu-id="1f4a1-103">hello gateway IP address can be changed without removing an existing VPN gateway connection (if you have one).</span></span> <span data-ttu-id="1f4a1-104">toomodify hello brány IP adres, nahraďte hello hodnoty 'Site2' a 'TestRG1' s vlastními pomocí hello [aktualizace brány místní sítě az](https://docs.microsoft.com/cli/azure/network/local-gateway#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1f4a1-104">toomodify hello gateway IP address, replace hello values 'Site2' and 'TestRG1' with your own using hello [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway#update) command.</span></span>

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

<span data-ttu-id="1f4a1-105">Ověřte správnost hello IP adresu ve výstupu hello:</span><span class="sxs-lookup"><span data-stu-id="1f4a1-105">Verify that hello IP address is correct in hello output:</span></span>

```
"gatewayIpAddress": "23.99.222.170",
```