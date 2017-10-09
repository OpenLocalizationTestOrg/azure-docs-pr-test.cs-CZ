<span data-ttu-id="96dd5-101">Můžete ověřit, že bylo připojení úspěšně pomocí rutiny hello 'Get-AzureRmVirtualNetworkGatewayConnection' s nebo bez,-Debug ".</span><span class="sxs-lookup"><span data-stu-id="96dd5-101">You can verify that your connection succeeded by using hello 'Get-AzureRmVirtualNetworkGatewayConnection' cmdlet, with or without '-Debug'.</span></span> 

1. <span data-ttu-id="96dd5-102">Hello použijte následující příklad rutiny, konfigurace toomatch hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="96dd5-102">Use hello following cmdlet example, configuring hello values toomatch your own.</span></span> <span data-ttu-id="96dd5-103">Po zobrazení výzvy vyberte "A" v pořadí toorun "Vše".</span><span class="sxs-lookup"><span data-stu-id="96dd5-103">If prompted, select 'A' in order toorun 'All'.</span></span> <span data-ttu-id="96dd5-104">V příkladu hello '-Name' odkazuje toohello název hello připojení, které chcete tootest.</span><span class="sxs-lookup"><span data-stu-id="96dd5-104">In hello example, '-Name' refers toohello name of hello connection that you want tootest.</span></span>

  ```powershell
  Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG
  ```
2. <span data-ttu-id="96dd5-105">Po dokončení rutiny hello zobrazte hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="96dd5-105">After hello cmdlet has finished, view hello values.</span></span> <span data-ttu-id="96dd5-106">V příkladu hello níže ukazuje stav připojení hello jako "Připojena" a vy můžete zobrazit příchozí a Odchozí bajty.</span><span class="sxs-lookup"><span data-stu-id="96dd5-106">In hello example below, hello connection status shows as 'Connected' and you can see ingress and egress bytes.</span></span>
   
  ```
  "connectionStatus": "Connected",
  "ingressBytesTransferred": 33509044,
  "egressBytesTransferred": 4142431
  ```