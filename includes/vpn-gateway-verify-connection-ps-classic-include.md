<span data-ttu-id="eb2b1-101">Můžete ověřit, že bylo připojení úspěšně pomocí rutiny 'Get-AzureVNetConnection'.</span><span class="sxs-lookup"><span data-stu-id="eb2b1-101">You can verify that your connection succeeded by using the 'Get-AzureVNetConnection' cmdlet.</span></span>

1. <span data-ttu-id="eb2b1-102">Použijte následující příklad rutiny a nakonfigurujte hodnoty tak, aby odpovídaly vašemu prostředí.</span><span class="sxs-lookup"><span data-stu-id="eb2b1-102">Use the following cmdlet example, configuring the values to match your own.</span></span> <span data-ttu-id="eb2b1-103">Název virtuální sítě musí být v uvozovkách, pokud obsahuje mezery.</span><span class="sxs-lookup"><span data-stu-id="eb2b1-103">The name of the virtual network must be in quotes if it contains spaces.</span></span>

  ```powershell
  Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
  ```
2. <span data-ttu-id="eb2b1-104">Po dokončení zpracování rutiny si prohlédněte hodnoty.</span><span class="sxs-lookup"><span data-stu-id="eb2b1-104">After the cmdlet has finished, view the values.</span></span> <span data-ttu-id="eb2b1-105">V následujícím příkladu zobrazuje stav připojení jako "Připojena" a vy vidíte příchozí a Odchozí bajty.</span><span class="sxs-lookup"><span data-stu-id="eb2b1-105">In the example below, the Connectivity State shows as 'Connected' and you can see ingress and egress bytes.</span></span>

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : The connectivity state for the local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal