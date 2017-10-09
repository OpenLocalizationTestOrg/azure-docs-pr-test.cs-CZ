### <a name="toomodify-hello-local-network-gateway-gatewayipaddress"></a>Brána místní sítě hello toomodify 'gatewayIpAddress.

Pokud hello zařízení VPN, které chcete tooconnect toohas změnit jeho veřejnou IP adresu, musíte toomodify hello místní síťové brány tooreflect, který změnit. IP adresa brány Hello lze změnit bez předchozího odstranění stávajícího připojení brány sítě VPN (pokud nějaký máte). toomodify hello brány IP adres, nahraďte hello hodnoty 'Site2' a 'TestRG1' s vlastními pomocí hello [aktualizace brány místní sítě az](https://docs.microsoft.com/cli/azure/network/local-gateway#update) příkaz.

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

Ověřte správnost hello IP adresu ve výstupu hello:

```
"gatewayIpAddress": "23.99.222.170",
```