1. <span data-ttu-id="5d912-101">Ve Správci clusteru převzetí služeb při selhání rozbalte **role**a pak zvýraznit vaší skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="5d912-101">In Failover Cluster Manager, expand **Roles**, and then highlight your availability group.</span></span>  

2. <span data-ttu-id="5d912-102">Na **prostředky** , klikněte pravým tlačítkem na název naslouchacího procesu a pak klikněte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="5d912-102">On the **Resources** tab, right-click the listener name, and then click **Properties**.</span></span>

3. <span data-ttu-id="5d912-103">Klikněte **závislosti** kartě.</span><span class="sxs-lookup"><span data-stu-id="5d912-103">Click the **Dependencies** tab.</span></span> <span data-ttu-id="5d912-104">Pokud nejsou uvedeny více prostředků, ověřte, že IP adresy OR, not a závislosti.</span><span class="sxs-lookup"><span data-stu-id="5d912-104">If multiple resources are listed, verify that the IP addresses have OR, not AND, dependencies.</span></span>  

4. <span data-ttu-id="5d912-105">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d912-105">Click **OK**.</span></span>

5. <span data-ttu-id="5d912-106">Klikněte pravým tlačítkem na název naslouchacího procesu a pak klikněte na **přepnout do režimu Online**.</span><span class="sxs-lookup"><span data-stu-id="5d912-106">Right-click the listener name, and then click **Bring Online**.</span></span>

6. <span data-ttu-id="5d912-107">Po online na naslouchací proces **prostředky** , klikněte pravým tlačítkem na skupinu dostupnosti a pak klikněte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="5d912-107">After the listener is online, on the **Resources** tab, right-click the availability group, and then click **Properties**.</span></span>
   
    ![Konfigurace prostředek skupiny dostupnosti](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. <span data-ttu-id="5d912-109">Závislost na prostředku názvu naslouchací proces (ne IP adresy prostředky názvem) a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d912-109">Create a dependency on the listener name resource (not the IP address resources name), and then click **OK**.</span></span>
   
    ![Přidat závislost na název naslouchacího procesu](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. <span data-ttu-id="5d912-111">Spusťte SQL Server Management Studio a připojte se k primární replice.</span><span class="sxs-lookup"><span data-stu-id="5d912-111">Start SQL Server Management Studio, and then connect to the primary replica.</span></span>

9. <span data-ttu-id="5d912-112">Přejděte na **AlwaysOn vysokou dostupnost** > **skupiny dostupnosti** > **\<AvailabilityGroupName\>**   >  **Naslouchací procesy skupiny dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="5d912-112">Go to **AlwaysOn High Availability** > **Availability Groups** > **\<AvailabilityGroupName\>** > **Availability Group Listeners**.</span></span>  
    <span data-ttu-id="5d912-113">Název naslouchacího procesu, který jste vytvořili ve Správci clusteru převzetí služeb při selhání by měl být zobrazen.</span><span class="sxs-lookup"><span data-stu-id="5d912-113">The listener name that you created in Failover Cluster Manager should be displayed.</span></span>

10. <span data-ttu-id="5d912-114">Klikněte pravým tlačítkem na název naslouchacího procesu a pak klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="5d912-114">Right-click the listener name, and then click **Properties**.</span></span>

11. <span data-ttu-id="5d912-115">V **Port** pole, zadejte číslo portu pro naslouchací proces skupiny dostupnosti pomocí $EndpointPort dříve používaného (v tomto kurzu 1433 se výchozí nastavení) a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d912-115">In the **Port** box, specify the port number for the availability group listener by using the $EndpointPort that you used earlier (in this tutorial, 1433 was the default), and then click **OK**.</span></span>

