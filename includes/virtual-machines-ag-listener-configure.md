<span data-ttu-id="2798f-101">naslouchací proces skupiny dostupnosti Hello je IP adresy a síťového názvu objektu který hello skupiny dostupnosti naslouchá na serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2798f-101">hello availability group listener is an IP address and network name that hello SQL Server availability group listens on.</span></span> <span data-ttu-id="2798f-102">toocreate hello naslouchací proces skupiny dostupnosti, hello následující:</span><span class="sxs-lookup"><span data-stu-id="2798f-102">toocreate hello availability group listener, do hello following:</span></span>

1. <span data-ttu-id="2798f-103"><a name="getnet"></a>Získáte název hello hello clusteru síťovému prostředku.</span><span class="sxs-lookup"><span data-stu-id="2798f-103"><a name="getnet"></a>Get hello name of hello cluster network resource.</span></span>

    <span data-ttu-id="2798f-104">a.</span><span class="sxs-lookup"><span data-stu-id="2798f-104">a.</span></span> <span data-ttu-id="2798f-105">Pomocí protokolu RDP tooconnect toohello Azure virtuální počítač, který je hostitelem primární repliky hello.</span><span class="sxs-lookup"><span data-stu-id="2798f-105">Use RDP tooconnect toohello Azure virtual machine that hosts hello primary replica.</span></span> 

    <span data-ttu-id="2798f-106">b.</span><span class="sxs-lookup"><span data-stu-id="2798f-106">b.</span></span> <span data-ttu-id="2798f-107">Otevřete Správce clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="2798f-107">Open Failover Cluster Manager.</span></span>

    <span data-ttu-id="2798f-108">c.</span><span class="sxs-lookup"><span data-stu-id="2798f-108">c.</span></span> <span data-ttu-id="2798f-109">Vyberte hello **sítě** uzel a název sítě s clustery Poznámka hello.</span><span class="sxs-lookup"><span data-stu-id="2798f-109">Select hello **Networks** node, and note hello cluster network name.</span></span> <span data-ttu-id="2798f-110">Tento název použít v hello `$ClusterNetworkName` proměnné v hello skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2798f-110">Use this name in hello `$ClusterNetworkName` variable in hello PowerShell script.</span></span> <span data-ttu-id="2798f-111">Následující obrázek hello clusteru síťový název je v hello **clusteru sítě 1**:</span><span class="sxs-lookup"><span data-stu-id="2798f-111">In hello following image hello cluster network name is **Cluster Network 1**:</span></span>

   ![Název sítě s clustery](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

2. <span data-ttu-id="2798f-113"><a name="addcap"></a>Přidejte hello klientský přístupový bod.</span><span class="sxs-lookup"><span data-stu-id="2798f-113"><a name="addcap"></a>Add hello client access point.</span></span>  
    <span data-ttu-id="2798f-114">Hello klientský přístupový bod je název sítě hello, že aplikace používat tooconnect toohello databází ve skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="2798f-114">hello client access point is hello network name that applications use tooconnect toohello databases in an availability group.</span></span> <span data-ttu-id="2798f-115">Vytvoření hello klientský přístupový bod, ve Správci clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="2798f-115">Create hello client access point in Failover Cluster Manager.</span></span>

    <span data-ttu-id="2798f-116">a.</span><span class="sxs-lookup"><span data-stu-id="2798f-116">a.</span></span> <span data-ttu-id="2798f-117">Rozbalte název clusteru hello a pak klikněte na tlačítko **role**.</span><span class="sxs-lookup"><span data-stu-id="2798f-117">Expand hello cluster name, and then click **Roles**.</span></span>

    <span data-ttu-id="2798f-118">b.</span><span class="sxs-lookup"><span data-stu-id="2798f-118">b.</span></span> <span data-ttu-id="2798f-119">V hello **role** podokně, klikněte pravým tlačítkem na skupinu dostupnosti hello názvem a pak vyberte **přidat prostředek** > **klientský přístupový bod**.</span><span class="sxs-lookup"><span data-stu-id="2798f-119">In hello **Roles** pane, right-click hello availability group name, and then select **Add Resource** > **Client Access Point**.</span></span>

   ![Klientský přístupový bod](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    <span data-ttu-id="2798f-121">c.</span><span class="sxs-lookup"><span data-stu-id="2798f-121">c.</span></span> <span data-ttu-id="2798f-122">V hello **název** pole, vytvořte název pro tento nový naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="2798f-122">In hello **Name** box, create a name for this new listener.</span></span> 
   <span data-ttu-id="2798f-123">Hello název nové naslouchací proces hello je název sítě hello, že aplikace používat tooconnect toodatabases ve skupině dostupnosti systému SQL Server hello.</span><span class="sxs-lookup"><span data-stu-id="2798f-123">hello name for hello new listener is hello network name that applications use tooconnect toodatabases in hello SQL Server availability group.</span></span>
   
    <span data-ttu-id="2798f-124">d.</span><span class="sxs-lookup"><span data-stu-id="2798f-124">d.</span></span> <span data-ttu-id="2798f-125">Klikněte na tlačítko toofinish vytváření hello naslouchací proces, **Další** dvakrát a potom klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="2798f-125">toofinish creating hello listener, click **Next** twice, and then click **Finish**.</span></span> <span data-ttu-id="2798f-126">Nelze zobrazit naslouchací proces hello nebo prostředek do online režimu v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="2798f-126">Do not bring hello listener or resource online at this point.</span></span>

3. <span data-ttu-id="2798f-127"><a name="congroup"></a>Nakonfigurujte hello IP prostředku pro skupinu dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="2798f-127"><a name="congroup"></a>Configure hello IP resource for hello availability group.</span></span>

    <span data-ttu-id="2798f-128">a.</span><span class="sxs-lookup"><span data-stu-id="2798f-128">a.</span></span> <span data-ttu-id="2798f-129">Klikněte na tlačítko hello **prostředky** kartě a potom rozbalte hello klientský přístupový bod jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2798f-129">Click hello **Resources** tab, and then expand hello client access point you created.</span></span>  
    <span data-ttu-id="2798f-130">Hello klientský přístupový bod je offline.</span><span class="sxs-lookup"><span data-stu-id="2798f-130">hello client access point is offline.</span></span>

   ![Klientský přístupový bod](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    <span data-ttu-id="2798f-132">b.</span><span class="sxs-lookup"><span data-stu-id="2798f-132">b.</span></span> <span data-ttu-id="2798f-133">Klikněte pravým tlačítkem na prostředek hello IP a potom klikněte na položku Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2798f-133">Right-click hello IP resource, and then click properties.</span></span> <span data-ttu-id="2798f-134">Poznamenejte si název hello hello IP adresy a použít ho v hello `$IPResourceName` proměnné v hello skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2798f-134">Note hello name of hello IP address, and use it in hello `$IPResourceName` variable in hello PowerShell script.</span></span>

    <span data-ttu-id="2798f-135">c.</span><span class="sxs-lookup"><span data-stu-id="2798f-135">c.</span></span> <span data-ttu-id="2798f-136">V části **IP adresu**, klikněte na tlačítko **statickou IP adresu**.</span><span class="sxs-lookup"><span data-stu-id="2798f-136">Under **IP Address**, click **Static IP Address**.</span></span> <span data-ttu-id="2798f-137">Nastavit hello IP adresu jako hello stejná adresa jste použili při nastavování hello adresa služby Vyrovnávání zatížení pro hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2798f-137">Set hello IP address as hello same address that you used when you set hello load balancer address on hello Azure portal.</span></span>

   ![Prostředek IP](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

4. <span data-ttu-id="2798f-139"><a name = "dependencyGroup"></a>Vytvořte prostředek skupiny dostupnosti systému SQL Server hello závislost na hello klientský přístupový bod.</span><span class="sxs-lookup"><span data-stu-id="2798f-139"><a name = "dependencyGroup"></a>Make hello SQL Server availability group resource dependent on hello client access point.</span></span>

    <span data-ttu-id="2798f-140">a.</span><span class="sxs-lookup"><span data-stu-id="2798f-140">a.</span></span> <span data-ttu-id="2798f-141">Ve Správci clusteru převzetí služeb při selhání, klikněte na tlačítko **role**a potom klikněte na vaší skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="2798f-141">In Failover Cluster Manager, click **Roles**, and then click your availability group.</span></span>

    <span data-ttu-id="2798f-142">b.</span><span class="sxs-lookup"><span data-stu-id="2798f-142">b.</span></span> <span data-ttu-id="2798f-143">Na hello **prostředky** v části **další prostředky**, klikněte pravým tlačítkem na skupinu prostředků hello dostupnosti a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="2798f-143">On hello **Resources** tab, under **Other Resources**, right-click hello availability resource group, and then click **Properties**.</span></span> 

    <span data-ttu-id="2798f-144">c.</span><span class="sxs-lookup"><span data-stu-id="2798f-144">c.</span></span> <span data-ttu-id="2798f-145">Na kartě hello závislosti přidejte název hello hello klientský přístup k bodu (hello naslouchací proces) prostředku.</span><span class="sxs-lookup"><span data-stu-id="2798f-145">On hello dependencies tab, add hello name of hello client access point (hello listener) resource.</span></span>

   ![Prostředek IP](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    <span data-ttu-id="2798f-147">d.</span><span class="sxs-lookup"><span data-stu-id="2798f-147">d.</span></span> <span data-ttu-id="2798f-148">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2798f-148">Click **OK**.</span></span>

5. <span data-ttu-id="2798f-149"><a name="listname"></a>Usnadnění přístupu klienta hello bodu prostředků, které jsou závislé na hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2798f-149"><a name="listname"></a>Make hello client access point resource dependent on hello IP address.</span></span>

    <span data-ttu-id="2798f-150">a.</span><span class="sxs-lookup"><span data-stu-id="2798f-150">a.</span></span> <span data-ttu-id="2798f-151">Ve Správci clusteru převzetí služeb při selhání, klikněte na tlačítko **role**a potom klikněte na vaší skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="2798f-151">In Failover Cluster Manager, click **Roles**, and then click your availability group.</span></span> 

    <span data-ttu-id="2798f-152">b.</span><span class="sxs-lookup"><span data-stu-id="2798f-152">b.</span></span> <span data-ttu-id="2798f-153">Na hello **prostředky** kartě, klikněte pravým tlačítkem na hello klientský přístup k bodu prostředek pod **název serveru**a potom klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="2798f-153">On hello **Resources** tab, right-click hello client access point resource under **Server Name**, and then click **Properties**.</span></span> 

   ![Prostředek IP](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    <span data-ttu-id="2798f-155">c.</span><span class="sxs-lookup"><span data-stu-id="2798f-155">c.</span></span> <span data-ttu-id="2798f-156">Klikněte na tlačítko hello **závislosti** kartě. Ověřte, zda je adresa IP hello závislost.</span><span class="sxs-lookup"><span data-stu-id="2798f-156">Click hello **Dependencies** tab. Verify that hello IP address is a dependency.</span></span> <span data-ttu-id="2798f-157">Pokud není, nastavte závislost na IP adresu hello.</span><span class="sxs-lookup"><span data-stu-id="2798f-157">If it is not, set a dependency on hello IP address.</span></span> <span data-ttu-id="2798f-158">Pokud uvedený více prostředků, ověřte, že hello IP adres nebo Ne a závislosti.</span><span class="sxs-lookup"><span data-stu-id="2798f-158">If there are multiple resources listed, verify that hello IP addresses have OR, not AND, dependencies.</span></span> <span data-ttu-id="2798f-159">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2798f-159">Click **OK**.</span></span> 

   ![Prostředek IP](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    <span data-ttu-id="2798f-161">d.</span><span class="sxs-lookup"><span data-stu-id="2798f-161">d.</span></span> <span data-ttu-id="2798f-162">Klikněte pravým tlačítkem na název naslouchacího procesu hello a pak klikněte na **přepnout do režimu Online**.</span><span class="sxs-lookup"><span data-stu-id="2798f-162">Right-click hello listener name, and then click **Bring Online**.</span></span> 

    >[!TIP]
    ><span data-ttu-id="2798f-163">Můžete ověřit, že hello, které jsou správně nakonfigurovány závislosti.</span><span class="sxs-lookup"><span data-stu-id="2798f-163">You can validate that hello dependencies are correctly configured.</span></span> <span data-ttu-id="2798f-164">Ve Správci clusteru převzetí služeb při selhání přejděte tooRoles, klikněte pravým tlačítkem na skupinu dostupnosti hello, klikněte na **další akce**a potom klikněte na **zobrazit sestavu závislostí**.</span><span class="sxs-lookup"><span data-stu-id="2798f-164">In Failover Cluster Manager, go tooRoles, right-click hello availability group, click **More Actions**, and then click  **Show Dependency Report**.</span></span> <span data-ttu-id="2798f-165">Pokud jsou správně nakonfigurovány závislosti hello, skupina dostupnosti hello je závislá na název sítě hello a hello síťový název je závislá na IP adresu hello.</span><span class="sxs-lookup"><span data-stu-id="2798f-165">When hello dependencies are correctly configured, hello availability group is dependent on hello network name, and hello network name is dependent on hello IP address.</span></span> 


6. <span data-ttu-id="2798f-166"><a name="setparam"></a>Nastavení parametrů clusteru hello v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2798f-166"><a name="setparam"></a>Set hello cluster parameters in PowerShell.</span></span>
    
    <span data-ttu-id="2798f-167">a.</span><span class="sxs-lookup"><span data-stu-id="2798f-167">a.</span></span> <span data-ttu-id="2798f-168">Zkopírujte hello následující tooone skript prostředí PowerShell vaše instancí systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2798f-168">Copy hello following PowerShell script tooone of your SQL Server instances.</span></span> <span data-ttu-id="2798f-169">Aktualizujte hello proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="2798f-169">Update hello variables for your environment.</span></span>     
    
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
    $IPResourceName = "<IPResourceName>" # hello IP Address resource name
    $ILBIP = “<n.n.n.n>” # hello IP Address of hello Internal Load Balancer (ILB). This is hello static IP address for hello load balancer you configured in hello Azure portal.
    [int]$ProbePort = <nnnnn>
    
    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```

    <span data-ttu-id="2798f-170">b.</span><span class="sxs-lookup"><span data-stu-id="2798f-170">b.</span></span> <span data-ttu-id="2798f-171">Nastavení parametrů clusteru hello spuštěním hello skript prostředí PowerShell na jednom z uzlů clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="2798f-171">Set hello cluster parameters by running hello PowerShell script on one of hello cluster nodes.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="2798f-172">Pokud vaše instance systému SQL Server v oblastech, musíte skript prostředí PowerShell hello toorun dvakrát.</span><span class="sxs-lookup"><span data-stu-id="2798f-172">If your SQL Server instances are in separate regions, you need toorun hello PowerShell script twice.</span></span> <span data-ttu-id="2798f-173">Hello poprvé, použijte hello `$ILBIP` a `$ProbePort` z první oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="2798f-173">hello first time, use hello `$ILBIP` and `$ProbePort` from hello first region.</span></span> <span data-ttu-id="2798f-174">Hello podruhé, použijte hello `$ILBIP` a `$ProbePort` z druhé oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="2798f-174">hello second time, use hello `$ILBIP` and `$ProbePort` from hello second region.</span></span> <span data-ttu-id="2798f-175">Název sítě Hello clusteru a název prostředku IP clusteru hello hello jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="2798f-175">hello cluster network name and hello cluster IP resource name are hello same.</span></span> 
