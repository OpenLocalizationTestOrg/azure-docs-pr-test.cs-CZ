---
title: "aaaConfigure naslouchací proces ILB pro Always On skupiny dostupnosti v Azure | Microsoft Docs"
description: "Tento kurz používá prostředky, které jsou vytvořené pomocí modelu nasazení classic hello a vytvoří Always On naslouchací proces skupiny dostupnosti v Azure, která používá interní nástroj."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 291288a0-740b-4cfa-af62-053218beba77
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: 2ce9b64fea491c945b58f7641e41fd39d90b078a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a><span data-ttu-id="266d7-103">Konfigurace naslouchací proces ILB pro skupiny dostupnosti Always On v Azure</span><span class="sxs-lookup"><span data-stu-id="266d7-103">Configure an ILB listener for Always On availability groups in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="266d7-104">Interní naslouchací proces</span><span class="sxs-lookup"><span data-stu-id="266d7-104">Internal listener</span></span>](../classic/ps-sql-int-listener.md)
> * [<span data-ttu-id="266d7-105">Externí naslouchací proces</span><span class="sxs-lookup"><span data-stu-id="266d7-105">External listener</span></span>](../classic/ps-sql-ext-listener.md)
>
>

## <a name="overview"></a><span data-ttu-id="266d7-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="266d7-106">Overview</span></span>

> [!IMPORTANT]
> <span data-ttu-id="266d7-107">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="266d7-107">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="266d7-108">Tento článek se zabývá hello pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="266d7-108">This article covers hello use of hello classic deployment model.</span></span> <span data-ttu-id="266d7-109">Doporučujeme vám, že většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="266d7-109">We recommend that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="266d7-110">najdete v části tooconfigure naslouchací proces pro skupiny dostupnosti Always On v modelu Resource Manager hello [konfigurace pro vyrovnávání zatížení pro skupinu dostupnosti Always On v Azure](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="266d7-110">tooconfigure a listener for an Always On availability group in hello Resource Manager model, see [Configure a load balancer for an Always On availability group in Azure](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span></span>

<span data-ttu-id="266d7-111">Skupině dostupnosti může obsahovat repliky pouze místní nebo jenom Azure, které jsou, nebo které span místní a Azure pro hybridní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="266d7-111">Your availability group can contain replicas that are on-premises only or Azure only, or that span both on-premises and Azure for hybrid configurations.</span></span> <span data-ttu-id="266d7-112">Azure repliky mohou být uloženy v rámci hello stejné oblasti nebo v několika oblastech, které používají více virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="266d7-112">Azure replicas can reside within hello same region or across multiple regions that use multiple virtual networks.</span></span> <span data-ttu-id="266d7-113">Hello postupy v tomto článku předpokládá, že máte již [nakonfigurovat skupinu dostupnosti](../classic/portal-sql-alwayson-availability-groups.md) , ale zatím nenakonfigurovali naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="266d7-113">hello procedures in this article assume that you have already [configured an availability group](../classic/portal-sql-alwayson-availability-groups.md) but have not yet configured a listener.</span></span>

## <a name="guidelines-and-limitations-for-internal-listeners"></a><span data-ttu-id="266d7-114">Pokyny a omezení pro interní naslouchací procesy</span><span class="sxs-lookup"><span data-stu-id="266d7-114">Guidelines and limitations for internal listeners</span></span>
<span data-ttu-id="266d7-115">použití Hello k interní pro vyrovnávání zatížení (ILB) s naslouchací proces skupiny dostupnosti v Azure je subjektu toohello následující pokyny:</span><span class="sxs-lookup"><span data-stu-id="266d7-115">hello use of an internal load balancer (ILB) with an availability group listener in Azure is subject toohello following guidelines:</span></span>

* <span data-ttu-id="266d7-116">naslouchací proces skupiny dostupnosti Hello je podporován v systému Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="266d7-116">hello availability group listener is supported on Windows Server 2008 R2, Windows Server 2012, and Windows Server 2012 R2.</span></span>
* <span data-ttu-id="266d7-117">Pouze jeden naslouchací proces skupiny dostupnosti interní je podporována pro jednotlivých cloudových služeb, protože je hello naslouchací proces nakonfigurovat toohello ILB a je pouze jeden ILB pro jednotlivých cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="266d7-117">Only one internal availability group listener is supported for each cloud service, because hello listener is configured toohello ILB, and there is only one ILB for each cloud service.</span></span> <span data-ttu-id="266d7-118">Je však možné toocreate více externí naslouchací procesy.</span><span class="sxs-lookup"><span data-stu-id="266d7-118">However, it is possible toocreate multiple external listeners.</span></span> <span data-ttu-id="266d7-119">Další informace najdete v tématu [konfigurace o externí naslouchací proces pro skupiny dostupnosti Always On v Azure](../classic/ps-sql-ext-listener.md).</span><span class="sxs-lookup"><span data-stu-id="266d7-119">For more information, see [Configure an external listener for Always On availability groups in Azure](../classic/ps-sql-ext-listener.md).</span></span>

## <a name="determine-hello-accessibility-of-hello-listener"></a><span data-ttu-id="266d7-120">Určení hello usnadnění hello naslouchacího procesu</span><span class="sxs-lookup"><span data-stu-id="266d7-120">Determine hello accessibility of hello listener</span></span>
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

<span data-ttu-id="266d7-121">Tento článek se zaměřuje na tvorbu naslouchací proces, který používá ILB.</span><span class="sxs-lookup"><span data-stu-id="266d7-121">This article focuses on creating a listener that uses an ILB.</span></span> <span data-ttu-id="266d7-122">Pokud potřebujete naslouchací proces veřejných nebo externí, naleznete v části hello verze tohoto článku, který popisuje nastavení nahoru [externí naslouchací proces](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="266d7-122">If you need an public or external listener, see hello version of this article that discusses setting up an [external listener](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a><span data-ttu-id="266d7-123">Vytvoření koncové body s vyrovnáváním zatížení virtuálních počítačů s přímý server návratový</span><span class="sxs-lookup"><span data-stu-id="266d7-123">Create load-balanced VM endpoints with direct server return</span></span>
<span data-ttu-id="266d7-124">Nejprve vytvoříte ILB spuštěním skriptu hello později v této části.</span><span class="sxs-lookup"><span data-stu-id="266d7-124">You first create an ILB by running hello script later in this section.</span></span>

<span data-ttu-id="266d7-125">Vytvořte koncový bod Vyrovnávání zatížení sítě pro každý virtuální počítač, který je hostitelem Azure repliky.</span><span class="sxs-lookup"><span data-stu-id="266d7-125">Create a load-balanced endpoint for each VM that hosts an Azure replica.</span></span> <span data-ttu-id="266d7-126">Pokud máte repliky v několika oblastech, každou repliku pro danou oblast musí být ve stejné cloudové služby v hello hello stejné virtuální síti Azure.</span><span class="sxs-lookup"><span data-stu-id="266d7-126">If you have replicas in multiple regions, each replica for that region must be in hello same cloud service in hello same Azure virtual network.</span></span> <span data-ttu-id="266d7-127">Vytváření replik skupin, které jsou rozmístěny v několika oblastmi Azure dostupnosti vyžaduje konfiguraci více virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="266d7-127">Creating availability group replicas that span multiple Azure regions requires configuring multiple virtual networks.</span></span> <span data-ttu-id="266d7-128">Další informace o konfiguraci křížové připojení k virtuální síti, najdete v části [nakonfigurovat virtuální síť připojení k síti toovirtual](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span><span class="sxs-lookup"><span data-stu-id="266d7-128">For more information on configuring cross virtual network connectivity, see [Configure virtual network toovirtual network connectivity](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span></span>

1. <span data-ttu-id="266d7-129">V hello portálu Azure přejděte tooeach virtuálního počítače, který je hostitelem repliky tooview hello podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="266d7-129">In hello Azure portal, go tooeach VM that hosts a replica tooview hello details.</span></span>

2. <span data-ttu-id="266d7-130">Klikněte na tlačítko hello **koncové body** kartu pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="266d7-130">Click hello **Endpoints** tab for each VM.</span></span>

3. <span data-ttu-id="266d7-131">Ověřte, že hello **název** a **veřejný Port** koncového bodu hello naslouchací proces, který chcete toouse již nejsou používány.</span><span class="sxs-lookup"><span data-stu-id="266d7-131">Verify that hello **Name** and **Public Port** of hello listener endpoint that you want toouse are not already in use.</span></span> <span data-ttu-id="266d7-132">V příkladu hello v této části, je název hello *MyEndpoint*, a hello port je *1433*.</span><span class="sxs-lookup"><span data-stu-id="266d7-132">In hello example in this section, hello name is *MyEndpoint*, and hello port is *1433*.</span></span>

4. <span data-ttu-id="266d7-133">Na místního klienta, stáhněte a nainstalujte nejnovější hello [modulu PowerShell](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="266d7-133">On your local client, download and install hello latest [PowerShell module](https://azure.microsoft.com/downloads/).</span></span>

5. <span data-ttu-id="266d7-134">Spusťte prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="266d7-134">Start Azure PowerShell.</span></span>  
    <span data-ttu-id="266d7-135">Otevře novou relaci prostředí PowerShell s hello Azure pro správu moduly zavedené.</span><span class="sxs-lookup"><span data-stu-id="266d7-135">A new PowerShell session opens, with hello Azure administrative modules loaded.</span></span>

6. <span data-ttu-id="266d7-136">Spusťte `Get-AzurePublishSettingsFile`.</span><span class="sxs-lookup"><span data-stu-id="266d7-136">Run `Get-AzurePublishSettingsFile`.</span></span> <span data-ttu-id="266d7-137">Tato rutina přesměruje tooa prohlížeče toodownload k publikování nastavení souboru tooa místnímu adresáři.</span><span class="sxs-lookup"><span data-stu-id="266d7-137">This cmdlet directs you tooa browser toodownload a publish settings file tooa local directory.</span></span> <span data-ttu-id="266d7-138">Pro vaše předplatné Azure, může být vyzvání k zadání pověření přihlášení.</span><span class="sxs-lookup"><span data-stu-id="266d7-138">You might be prompted for your sign-in credentials for your Azure subscription.</span></span>

7. <span data-ttu-id="266d7-139">Spusťte následující hello `Import-AzurePublishSettingsFile` příkaz s hello cestu hello publikovat nastavení souboru, který jste stáhli:</span><span class="sxs-lookup"><span data-stu-id="266d7-139">Run hello following `Import-AzurePublishSettingsFile` command with hello path of hello publish settings file that you downloaded:</span></span>

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    <span data-ttu-id="266d7-140">Po publikování hello importu souboru nastavení, můžete spravovat vaše předplatné Azure v relaci prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="266d7-140">After hello publish settings file is imported, you can manage your Azure subscription in hello PowerShell session.</span></span>

8. <span data-ttu-id="266d7-141">Pro *ILB*, přiřadit statickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="266d7-141">For *ILB*, assign a static IP address.</span></span> <span data-ttu-id="266d7-142">Zkontrolujte aktuální konfiguraci virtuální sítě hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="266d7-142">Examine hello current virtual network configuration by running hello following command:</span></span>

        (Get-AzureVNetConfig).XMLConfiguration
9. <span data-ttu-id="266d7-143">Poznámka: hello *podsíť* název pro podsíť hello, který obsahuje hello virtuálních počítačů, které jsou hostiteli hello repliky.</span><span class="sxs-lookup"><span data-stu-id="266d7-143">Note hello *Subnet* name for hello subnet that contains hello VMs that host hello replicas.</span></span> <span data-ttu-id="266d7-144">Tento název se používá v parametru hello $SubnetName ve skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="266d7-144">This name is used in hello $SubnetName parameter in hello script.</span></span>

10. <span data-ttu-id="266d7-145">Poznámka: hello *VirtualNetworkSite* název a text hello, od *AddressPrefix* hello podsítě, která obsahuje hello virtuálních počítačů, které jsou hostiteli hello repliky.</span><span class="sxs-lookup"><span data-stu-id="266d7-145">Note hello *VirtualNetworkSite* name and hello starting *AddressPrefix* for hello subnet that contains hello VMs that host hello replicas.</span></span> <span data-ttu-id="266d7-146">Hledat dostupnou adresu IP pomocí předání obě hodnoty toohello `Test-AzureStaticVNetIP` příkazu a kontrolou hello *AvailableAddresses*.</span><span class="sxs-lookup"><span data-stu-id="266d7-146">Look for an available IP address by passing both values toohello `Test-AzureStaticVNetIP` command and by examining hello *AvailableAddresses*.</span></span> <span data-ttu-id="266d7-147">Například, pokud hello virtuální sítě se jmenuje *MyVNet* a má rozsah adres podsítě, která se spouští v *172.16.0.128*, hello následující příkaz, by seznam dostupných adres:</span><span class="sxs-lookup"><span data-stu-id="266d7-147">For example, if hello virtual network is named *MyVNet* and has a subnet address range that starts at *172.16.0.128*, hello following command would list available addresses:</span></span>

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
11. <span data-ttu-id="266d7-148">Vyberte jednu z dostupných adres hello a použít ho v parametru hello $ILBStaticIP hello skriptu v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="266d7-148">Select one of hello available addresses, and use it in hello $ILBStaticIP parameter of hello script in hello next step.</span></span>

12. <span data-ttu-id="266d7-149">Zkopírujte hello následující prostředí PowerShell skriptu tooa textovém editoru a nastavte toosuit hello hodnoty proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="266d7-149">Copy hello following PowerShell script tooa text editor, and set hello variable values toosuit your environment.</span></span> <span data-ttu-id="266d7-150">Výchozí hodnoty jsou uvedeny některé parametry.</span><span class="sxs-lookup"><span data-stu-id="266d7-150">Defaults have been provided for some parameters.</span></span>  

    <span data-ttu-id="266d7-151">Existující nasazení, které používají skupiny vztahů nelze přidat ILB.</span><span class="sxs-lookup"><span data-stu-id="266d7-151">Existing deployments that use affinity groups cannot add an ILB.</span></span> <span data-ttu-id="266d7-152">Další informace o požadavcích na ILB najdete v tématu [přehled nástroje pro vyrovnávání zatížení pro vnitřní](../../../load-balancer/load-balancer-internal-overview.md).</span><span class="sxs-lookup"><span data-stu-id="266d7-152">For more information about ILB requirements, see [Internal load balancer overview](../../../load-balancer/load-balancer-internal-overview.md).</span></span>

    <span data-ttu-id="266d7-153">Navíc pokud vaší skupiny dostupnosti zahrnuje oblasti Azure, je nutné spustit skript hello jednou v každé datové centrum pro hello Cloudová služba a uzly, které jsou umístěny ve stejné datové centrum.</span><span class="sxs-lookup"><span data-stu-id="266d7-153">Also, if your availability group spans Azure regions, you must run hello script once in each datacenter for hello cloud service and nodes that reside in that datacenter.</span></span>

        # Define variables
        $ServiceName = "<MyCloudService>" # hello name of hello cloud service that contains hello availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in hello same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that hello replicas use in hello virtual network
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for hello ILB in hello subnet
        $ILBName = "AGListenerLB" # customize hello ILB name or use this default value

        # Create hello ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load-balanced endpoint for each node in $AGNodes by using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

13. <span data-ttu-id="266d7-154">Po nastavení proměnné hello kopie hello skript z hello textového editoru tooyour prostředí PowerShell relace toorun ho.</span><span class="sxs-lookup"><span data-stu-id="266d7-154">After you have set hello variables, copy hello script from hello text editor tooyour PowerShell session toorun it.</span></span> <span data-ttu-id="266d7-155">Pokud stále zobrazuje hello řádku  **>>** , stiskněte klávesu Enter znovu toomake zda hello skript spuštěn.</span><span class="sxs-lookup"><span data-stu-id="266d7-155">If hello prompt still shows **>>**, press Enter again toomake sure hello script starts running.</span></span>

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a><span data-ttu-id="266d7-156">Ověřte, zda KB2854082 nainstalována v případě potřeby</span><span class="sxs-lookup"><span data-stu-id="266d7-156">Verify that KB2854082 is installed if necessary</span></span>
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-hello-firewall-ports-in-availability-group-nodes"></a><span data-ttu-id="266d7-157">Otevřete porty brány firewall hello uzly skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="266d7-157">Open hello firewall ports in availability group nodes</span></span>
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-hello-availability-group-listener"></a><span data-ttu-id="266d7-158">Vytvořte naslouchací proces skupiny dostupnosti hello</span><span class="sxs-lookup"><span data-stu-id="266d7-158">Create hello availability group listener</span></span>

<span data-ttu-id="266d7-159">Vytvořte naslouchací proces skupiny dostupnosti hello ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="266d7-159">Create hello availability group listener in two steps.</span></span> <span data-ttu-id="266d7-160">Nejprve vytvořte prostředek clusteru hello klientský přístup k bodu a nakonfigurovat závislosti.</span><span class="sxs-lookup"><span data-stu-id="266d7-160">First, create hello client access point cluster resource and configure  dependencies.</span></span> <span data-ttu-id="266d7-161">Druhý nakonfigurujte prostředky clusteru hello v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="266d7-161">Second, configure hello cluster resources in PowerShell.</span></span>

### <a name="create-hello-client-access-point-and-configure-hello-cluster-dependencies"></a><span data-ttu-id="266d7-162">Vytvořit hello klientský přístupový bod a nakonfigurovat závislosti clusteru hello</span><span class="sxs-lookup"><span data-stu-id="266d7-162">Create hello client access point and configure hello cluster dependencies</span></span>
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-hello-cluster-resources-in-powershell"></a><span data-ttu-id="266d7-163">Konfigurace prostředků clusteru hello v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="266d7-163">Configure hello cluster resources in PowerShell</span></span>
1. <span data-ttu-id="266d7-164">Pro ILB je nutné použít IP adresu hello hello ILB, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="266d7-164">For ILB, you must use hello IP address of hello ILB that was created earlier.</span></span> <span data-ttu-id="266d7-165">tooobtain této IP adres v prostředí PowerShell, použijte hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="266d7-165">tooobtain this IP address in PowerShell, use hello following script:</span></span>

        # Define variables
        $ServiceName="<MyServiceName>" # hello name of hello cloud service that contains hello AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

2. <span data-ttu-id="266d7-166">Na jednom ze hello virtuální počítače zkopírujte hello skript prostředí PowerShell pro váš operační systém tooa textovém editoru a nastavte hello proměnné toohello hodnoty, které jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="266d7-166">On one of hello VMs, copy hello PowerShell script for your operating system tooa text editor, and then set hello variables toohello values you noted earlier.</span></span>

    <span data-ttu-id="266d7-167">Pro Windows Server 2012 nebo novější použijte hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="266d7-167">For Windows Server 2012 or later, use hello following script:</span></span>

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP address resource name
        $ILBIP = “<X.X.X.X>” # hello IP address of hello ILB

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

    <span data-ttu-id="266d7-168">Pro Windows Server 2008 R2 použijte hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="266d7-168">For Windows Server 2008 R2, use hello following script:</span></span>

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP address resource name
        $ILBIP = “<X.X.X.X>” # hello IP address of hello ILB

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

3. <span data-ttu-id="266d7-169">Až budete mít sadu hello proměnné, otevřete okno prostředí Windows PowerShell se zvýšenými oprávněními, vložte hello skript z hello textového editoru do vaší toorun relace prostředí PowerShell ji.</span><span class="sxs-lookup"><span data-stu-id="266d7-169">After you have set hello variables, open an elevated Windows PowerShell window, paste hello script from hello text editor into your PowerShell session toorun it.</span></span> <span data-ttu-id="266d7-170">Pokud stále zobrazuje hello řádku  **>>** , stiskněte klávesu Enter znovu toomake jistotu, že hello skript spuštěn.</span><span class="sxs-lookup"><span data-stu-id="266d7-170">If hello prompt still shows **>>**, Press Enter again toomake sure that hello script starts running.</span></span>

4. <span data-ttu-id="266d7-171">Opakujte hello předcházející kroky pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="266d7-171">Repeat hello preceding steps for each VM.</span></span>  
    <span data-ttu-id="266d7-172">Tento skript nakonfiguruje prostředek hello IP adresy s IP adresou hello hello cloudové služby a nastaví dalších parametrů, jako je port testu hello.</span><span class="sxs-lookup"><span data-stu-id="266d7-172">This script configures hello IP address resource with hello IP address of hello cloud service and sets other parameters, such as hello probe port.</span></span> <span data-ttu-id="266d7-173">Pokud prostředek hello IP adresy je uvést do režimu online, může reagovat toohello dotazování na port testu hello z hello Vyrovnávání zatížení sítě koncový bod, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="266d7-173">When hello IP address resource is brought online, it can respond toohello polling on hello probe port from hello load-balanced endpoint that you created earlier.</span></span>

## <a name="bring-hello-listener-online"></a><span data-ttu-id="266d7-174">Přepněte naslouchací proces hello online</span><span class="sxs-lookup"><span data-stu-id="266d7-174">Bring hello listener online</span></span>
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a><span data-ttu-id="266d7-175">Položky následnou akci</span><span class="sxs-lookup"><span data-stu-id="266d7-175">Follow-up items</span></span>
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-hello-availability-group-listener-within-hello-same-virtual-network"></a><span data-ttu-id="266d7-176">Naslouchací proces skupiny dostupnosti testu hello (uvnitř hello stejné virtuální sítě)</span><span class="sxs-lookup"><span data-stu-id="266d7-176">Test hello availability group listener (within hello same virtual network)</span></span>
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a><span data-ttu-id="266d7-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="266d7-177">Next steps</span></span>
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]
