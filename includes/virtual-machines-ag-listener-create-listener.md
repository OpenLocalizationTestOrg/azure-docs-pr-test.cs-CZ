<span data-ttu-id="1b31d-101">V tomto kroku vytvoříte ručně hello naslouchací proces skupiny dostupnosti ve Správci clusteru převzetí služeb při selhání a SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="1b31d-101">In this step, you manually create hello availability group listener in Failover Cluster Manager and SQL Server Management Studio.</span></span>

1. <span data-ttu-id="1b31d-102">Otevřete Správce clusteru převzetí služeb při selhání z hello uzlu, který je hostitelem primární repliky hello.</span><span class="sxs-lookup"><span data-stu-id="1b31d-102">Open Failover Cluster Manager from hello node that hosts hello primary replica.</span></span>

2. <span data-ttu-id="1b31d-103">Vyberte hello **sítě** uzel a potom název sítě s clustery Poznámka hello.</span><span class="sxs-lookup"><span data-stu-id="1b31d-103">Select hello **Networks** node, and then note hello cluster network name.</span></span> <span data-ttu-id="1b31d-104">Tento název se používá v proměnné hello $ClusterNetworkName v hello skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1b31d-104">This name is used in hello $ClusterNetworkName variable in hello PowerShell script.</span></span>

3. <span data-ttu-id="1b31d-105">Rozbalte název clusteru hello a pak klikněte na tlačítko **role**.</span><span class="sxs-lookup"><span data-stu-id="1b31d-105">Expand hello cluster name, and then click **Roles**.</span></span>

4. <span data-ttu-id="1b31d-106">V hello **role** podokně, klikněte pravým tlačítkem na skupinu dostupnosti hello názvem a pak vyberte **přidat prostředek** > **klientský přístupový bod**.</span><span class="sxs-lookup"><span data-stu-id="1b31d-106">In hello **Roles** pane, right-click hello availability group name, and then select **Add Resource** > **Client Access Point**.</span></span>
   
    ![Přidat klientský přístupový bod pro skupinu dostupnosti](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. <span data-ttu-id="1b31d-108">V hello **název** pole, vytvořte název pro tento nový naslouchací proces, klikněte na **Další** dvakrát a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="1b31d-108">In hello **Name** box, create a name for this new listener, click **Next** twice, and then click **Finish**.</span></span>  
    <span data-ttu-id="1b31d-109">Nelze zobrazit naslouchací proces hello nebo prostředek do online režimu v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="1b31d-109">Do not bring hello listener or resource online at this point.</span></span>

6. <span data-ttu-id="1b31d-110">Klikněte na tlačítko hello **prostředky** kartě a potom rozbalte hello klientský přístupový bod, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="1b31d-110">Click hello **Resources** tab, and then expand hello client access point you just created.</span></span> 
    <span data-ttu-id="1b31d-111">Zobrazí se Hello IP adresu prostředku pro každou síť s clustery v clusteru.</span><span class="sxs-lookup"><span data-stu-id="1b31d-111">hello IP address resource for each cluster network in your cluster is displayed.</span></span> <span data-ttu-id="1b31d-112">Pokud je to řešení Azure, se zobrazí pouze jeden prostředek IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1b31d-112">If this is an Azure-only solution, only one IP address resource is displayed.</span></span>

7. <span data-ttu-id="1b31d-113">Proveďte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="1b31d-113">Do either of hello following:</span></span>
   
   * <span data-ttu-id="1b31d-114">tooconfigure hybridní řešení:</span><span class="sxs-lookup"><span data-stu-id="1b31d-114">tooconfigure a hybrid solution:</span></span>
     
        <span data-ttu-id="1b31d-115">a.</span><span class="sxs-lookup"><span data-stu-id="1b31d-115">a.</span></span> <span data-ttu-id="1b31d-116">Klikněte pravým tlačítkem na prostředek hello IP adresy, která odpovídá tooyour místní podsítě a potom vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="1b31d-116">Right-click hello IP address resource that corresponds tooyour on-premises subnet, and then select **Properties**.</span></span> <span data-ttu-id="1b31d-117">Poznamenejte si název hello IP adresy a síťového názvu.</span><span class="sxs-lookup"><span data-stu-id="1b31d-117">Note hello IP address name and network name.</span></span>
   
        <span data-ttu-id="1b31d-118">b.</span><span class="sxs-lookup"><span data-stu-id="1b31d-118">b.</span></span> <span data-ttu-id="1b31d-119">Vyberte **statickou IP adresu**, přiřadit nepoužívané IP adresu a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b31d-119">Select **Static IP Address**, assign an unused IP address, and then click **OK**.</span></span>
 
   * <span data-ttu-id="1b31d-120">tooconfigure řešení služby Azure:</span><span class="sxs-lookup"><span data-stu-id="1b31d-120">tooconfigure an Azure-only solution:</span></span>

        <span data-ttu-id="1b31d-121">a.</span><span class="sxs-lookup"><span data-stu-id="1b31d-121">a.</span></span> <span data-ttu-id="1b31d-122">Klikněte pravým tlačítkem na prostředek hello IP adresy, která odpovídá tooyour podsíti Azure a potom vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="1b31d-122">Right-click hello IP address resource that corresponds tooyour Azure subnet, and then select **Properties**.</span></span>
       
       > [!NOTE]
       > <span data-ttu-id="1b31d-123">Pokud toocome online naslouchací proces hello později selže z důvodu konfliktních IP adresa vybraná protokolem DHCP, můžete nakonfigurovat platnou statickou IP adresu v tomto okně Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1b31d-123">If hello listener later fails toocome online because of a conflicting IP address selected by DHCP, you can configure a valid static IP address in this properties window.</span></span>
       > 
       > 

       <span data-ttu-id="1b31d-124">b.</span><span class="sxs-lookup"><span data-stu-id="1b31d-124">b.</span></span> <span data-ttu-id="1b31d-125">V hello stejné **IP adresu** vlastnosti – okno, změna hello **název IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="1b31d-125">In hello same **IP Address** properties window, change hello **IP Address Name**.</span></span>  
        <span data-ttu-id="1b31d-126">Tento název se používá v hello $IPResourceName proměnná hello skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1b31d-126">This name is used in hello $IPResourceName variable of hello PowerShell script.</span></span> <span data-ttu-id="1b31d-127">Pokud vaše řešení zahrnuje více virtuálních sítí Azure, můžete tento krok opakujte pro každý prostředek IP.</span><span class="sxs-lookup"><span data-stu-id="1b31d-127">If your solution spans multiple Azure virtual networks, repeat this step for each IP resource.</span></span>

