<span data-ttu-id="5665e-101">V tomto kroku vytvoříte ručně naslouchacího procesu skupiny dostupnosti ve Správci clusteru převzetí služeb při selhání a SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="5665e-101">In this step, you manually create the availability group listener in Failover Cluster Manager and SQL Server Management Studio.</span></span>

1. <span data-ttu-id="5665e-102">Otevřete Správce clusteru převzetí služeb při selhání z uzlu, který je hostitelem primární repliky.</span><span class="sxs-lookup"><span data-stu-id="5665e-102">Open Failover Cluster Manager from the node that hosts the primary replica.</span></span>

2. <span data-ttu-id="5665e-103">Vyberte **sítě** uzel a Poznámka: název sítě s clustery.</span><span class="sxs-lookup"><span data-stu-id="5665e-103">Select the **Networks** node, and then note the cluster network name.</span></span> <span data-ttu-id="5665e-104">Tento název se používá v $ClusterNetworkName proměnné ve skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5665e-104">This name is used in the $ClusterNetworkName variable in the PowerShell script.</span></span>

3. <span data-ttu-id="5665e-105">Rozbalte název clusteru a pak klikněte na tlačítko **role**.</span><span class="sxs-lookup"><span data-stu-id="5665e-105">Expand the cluster name, and then click **Roles**.</span></span>

4. <span data-ttu-id="5665e-106">V **role** podokně klikněte pravým tlačítkem na název skupiny dostupnosti a potom vyberte **přidat prostředek** > **klientský přístupový bod**.</span><span class="sxs-lookup"><span data-stu-id="5665e-106">In the **Roles** pane, right-click the availability group name, and then select **Add Resource** > **Client Access Point**.</span></span>
   
    ![Přidat klientský přístupový bod pro skupinu dostupnosti](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. <span data-ttu-id="5665e-108">V **název** pole, vytvořte název pro tento nový naslouchací proces, klikněte na **Další** dvakrát a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="5665e-108">In the **Name** box, create a name for this new listener, click **Next** twice, and then click **Finish**.</span></span>  
    <span data-ttu-id="5665e-109">Nelze zobrazit naslouchací proces nebo prostředek do online režimu v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="5665e-109">Do not bring the listener or resource online at this point.</span></span>

6. <span data-ttu-id="5665e-110">Klikněte **prostředky** kartě a potom rozbalte klientský přístupový bod, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="5665e-110">Click the **Resources** tab, and then expand the client access point you just created.</span></span> 
    <span data-ttu-id="5665e-111">Zobrazí se prostředek IP adresy pro každou síť clusteru v clusteru.</span><span class="sxs-lookup"><span data-stu-id="5665e-111">The IP address resource for each cluster network in your cluster is displayed.</span></span> <span data-ttu-id="5665e-112">Pokud je to řešení Azure, se zobrazí pouze jeden prostředek IP adresy.</span><span class="sxs-lookup"><span data-stu-id="5665e-112">If this is an Azure-only solution, only one IP address resource is displayed.</span></span>

7. <span data-ttu-id="5665e-113">Proveďte jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="5665e-113">Do either of the following:</span></span>
   
   * <span data-ttu-id="5665e-114">Ke konfiguraci hybridního řešení:</span><span class="sxs-lookup"><span data-stu-id="5665e-114">To configure a hybrid solution:</span></span>
     
        <span data-ttu-id="5665e-115">a.</span><span class="sxs-lookup"><span data-stu-id="5665e-115">a.</span></span> <span data-ttu-id="5665e-116">Klikněte pravým tlačítkem na prostředek IP adresy, která odpovídá místní podsíť a potom vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="5665e-116">Right-click the IP address resource that corresponds to your on-premises subnet, and then select **Properties**.</span></span> <span data-ttu-id="5665e-117">Poznamenejte si název IP adresy a síťového názvu.</span><span class="sxs-lookup"><span data-stu-id="5665e-117">Note the IP address name and network name.</span></span>
   
        <span data-ttu-id="5665e-118">b.</span><span class="sxs-lookup"><span data-stu-id="5665e-118">b.</span></span> <span data-ttu-id="5665e-119">Vyberte **statickou IP adresu**, přiřadit nepoužívané IP adresu a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="5665e-119">Select **Static IP Address**, assign an unused IP address, and then click **OK**.</span></span>
 
   * <span data-ttu-id="5665e-120">Konfigurace řešení služby Azure:</span><span class="sxs-lookup"><span data-stu-id="5665e-120">To configure an Azure-only solution:</span></span>

        <span data-ttu-id="5665e-121">a.</span><span class="sxs-lookup"><span data-stu-id="5665e-121">a.</span></span> <span data-ttu-id="5665e-122">Klikněte pravým tlačítkem na IP adresu prostředek, který odpovídá podsíti služby Azure a pak vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="5665e-122">Right-click the IP address resource that corresponds to your Azure subnet, and then select **Properties**.</span></span>
       
       > [!NOTE]
       > <span data-ttu-id="5665e-123">Pokud naslouchací proces později selže do režimu online z důvodu konfliktních IP adresa vybraná protokolem DHCP, můžete nakonfigurovat platnou statickou IP adresu v tomto okně Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5665e-123">If the listener later fails to come online because of a conflicting IP address selected by DHCP, you can configure a valid static IP address in this properties window.</span></span>
       > 
       > 

       <span data-ttu-id="5665e-124">b.</span><span class="sxs-lookup"><span data-stu-id="5665e-124">b.</span></span> <span data-ttu-id="5665e-125">Ve stejném **IP adresu** vlastnosti – okno, změny **název IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="5665e-125">In the same **IP Address** properties window, change the **IP Address Name**.</span></span>  
        <span data-ttu-id="5665e-126">Tento název se používá v proměnné $IPResourceName skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5665e-126">This name is used in the $IPResourceName variable of the PowerShell script.</span></span> <span data-ttu-id="5665e-127">Pokud vaše řešení zahrnuje více virtuálních sítí Azure, můžete tento krok opakujte pro každý prostředek IP.</span><span class="sxs-lookup"><span data-stu-id="5665e-127">If your solution spans multiple Azure virtual networks, repeat this step for each IP resource.</span></span>

