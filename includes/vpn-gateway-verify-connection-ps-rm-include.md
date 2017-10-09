Můžete ověřit, že bylo připojení úspěšně pomocí rutiny hello 'Get-AzureRmVirtualNetworkGatewayConnection' s nebo bez,-Debug ". 

1. Hello použijte následující příklad rutiny, konfigurace toomatch hello hodnoty vlastními. Po zobrazení výzvy vyberte "A" v pořadí toorun "Vše". V příkladu hello '-Name' odkazuje toohello název hello připojení, které chcete tootest.

  ```powershell
  Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG
  ```
2. Po dokončení rutiny hello zobrazte hello hodnoty. V příkladu hello níže ukazuje stav připojení hello jako "Připojena" a vy můžete zobrazit příchozí a Odchozí bajty.
   
  ```
  "connectionStatus": "Connected",
  "ingressBytesTransferred": 33509044,
  "egressBytesTransferred": 4142431
  ```