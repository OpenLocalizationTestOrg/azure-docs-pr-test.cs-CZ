### <a name="noconnection"></a>toomodify místní síťové brány IP předpon adres – žádné připojení brány

Pokud nemáte připojení brány a mají tooadd nebo odebrat předpony IP adres, můžete použít hello stejný příkaz, že používáte bránu místní sítě hello toocreate, [az brány místní-vytvořit](https://docs.microsoft.com/cli/azure/network/local-gateway#create). Tento příkaz tooupdate hello brány IP adresu můžete použít také pro zařízení VPN hello. aktuální nastavení hello toooverwrite použít hello stávající název brány místní sítě. Pokud použijete jiný název, můžete vytvořit novou bránu místní sítě, místo přepsání hello existující.

Musíte zadat pokaždé, když provedete změny, hello celý seznam předpon, ne jenom hello předpony, které chcete toochange. Zadejte pouze hello předpon, které chcete tookeep. V tomto případě 10.0.0.0/24 a 20.0.0.0/24.

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --connection-name TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

### <a name="withconnection"></a>toomodify předpon brány místní sítě IP adresy - existující připojení brány

Chcete-li mít připojení brány a tooadd nebo odebrat předpony IP adres, můžete aktualizovat pomocí předpony hello [aktualizace brány místní sítě az](https://docs.microsoft.com/cli/azure/network/local-gateway#update). Způsobí to určitý výpadek připojení VPN. Při změnách hello IP adres předpony, nepotřebujete toodelete hello VPN gateway.

Musíte zadat pokaždé, když provedete změny, hello celý seznam předpon, ne jenom hello předpony, které chcete toochange. V tomto příkladu jsou již přítomny předpony 10.0.0.0/24 a 20.0.0.0/24. Nemůžeme přidat hello předpony 30.0.0.0/24 a 40.0.0.0/24 a určit všechny 4 předpon hello při aktualizaci.

```azurecli
az network local-gateway update --local-address-prefixes 10.0.0.0/24 20.0.0.0/24 30.0.0.0/24 40.0.0.0/24 --name VNet1toSite2 --connection-name TestRG1
```