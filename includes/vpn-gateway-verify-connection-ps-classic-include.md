<span data-ttu-id="f8ab6-101">Můžete ověřit, že bylo připojení úspěšně pomocí rutiny hello 'Get-AzureVNetConnection'.</span><span class="sxs-lookup"><span data-stu-id="f8ab6-101">You can verify that your connection succeeded by using hello 'Get-AzureVNetConnection' cmdlet.</span></span>

1. <span data-ttu-id="f8ab6-102">Hello použijte následující příklad rutiny, konfigurace toomatch hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="f8ab6-102">Use hello following cmdlet example, configuring hello values toomatch your own.</span></span> <span data-ttu-id="f8ab6-103">Název Hello hello virtuální sítě musí být v uvozovkách, pokud obsahuje mezery.</span><span class="sxs-lookup"><span data-stu-id="f8ab6-103">hello name of hello virtual network must be in quotes if it contains spaces.</span></span>

  ```powershell
  Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
  ```
2. <span data-ttu-id="f8ab6-104">Po dokončení rutiny hello zobrazte hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f8ab6-104">After hello cmdlet has finished, view hello values.</span></span> <span data-ttu-id="f8ab6-105">V příkladu hello níže zobrazuje stav připojení hello jako "Připojena" a vidíte příchozí a Odchozí bajty.</span><span class="sxs-lookup"><span data-stu-id="f8ab6-105">In hello example below, hello Connectivity State shows as 'Connected' and you can see ingress and egress bytes.</span></span>

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : hello connectivity state for hello local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal