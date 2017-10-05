---
title: "Konfigurace naslouchací proces ILB pro skupiny dostupnosti Always On v Azure | Microsoft Docs"
description: "Tento kurz používá prostředky, které jsou vytvořené pomocí modelu nasazení classic a vytvoří Always On naslouchací proces skupiny dostupnosti v Azure, která používá interní nástroj."
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
ms.openlocfilehash: fea70b389b1f1d6af963e3f14fdc48e8d857dd53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a><span data-ttu-id="59654-103">Konfigurace naslouchací proces ILB pro skupiny dostupnosti Always On v Azure</span><span class="sxs-lookup"><span data-stu-id="59654-103">Configure an ILB listener for Always On availability groups in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="59654-104">Interní naslouchací proces</span><span class="sxs-lookup"><span data-stu-id="59654-104">Internal listener</span></span>](../classic/ps-sql-int-listener.md)
> * [<span data-ttu-id="59654-105">Externí naslouchací proces</span><span class="sxs-lookup"><span data-stu-id="59654-105">External listener</span></span>](../classic/ps-sql-ext-listener.md)
>
>

## <a name="overview"></a><span data-ttu-id="59654-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="59654-106">Overview</span></span>

> [!IMPORTANT]
> <span data-ttu-id="59654-107">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="59654-107">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="59654-108">Tento článek popisuje použití modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="59654-108">This article covers the use of the classic deployment model.</span></span> <span data-ttu-id="59654-109">Doporučujeme vám, že většina nových nasazení používala model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="59654-109">We recommend that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="59654-110">Ke konfiguraci naslouchacího procesu pro skupiny dostupnosti Always On v modelu Resource Manager, najdete v části [konfigurace pro vyrovnávání zatížení pro skupinu dostupnosti Always On v Azure](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="59654-110">To configure a listener for an Always On availability group in the Resource Manager model, see [Configure a load balancer for an Always On availability group in Azure](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span></span>

<span data-ttu-id="59654-111">Skupině dostupnosti může obsahovat repliky pouze místní nebo jenom Azure, které jsou, nebo které span místní a Azure pro hybridní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="59654-111">Your availability group can contain replicas that are on-premises only or Azure only, or that span both on-premises and Azure for hybrid configurations.</span></span> <span data-ttu-id="59654-112">Azure repliky mohou být uloženy ve stejné oblasti nebo v několika oblastech, které používají více virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="59654-112">Azure replicas can reside within the same region or across multiple regions that use multiple virtual networks.</span></span> <span data-ttu-id="59654-113">Postupy v tomto článku předpokládá, že máte již [nakonfigurovat skupinu dostupnosti](../classic/portal-sql-alwayson-availability-groups.md) , ale zatím nenakonfigurovali naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="59654-113">The procedures in this article assume that you have already [configured an availability group](../classic/portal-sql-alwayson-availability-groups.md) but have not yet configured a listener.</span></span>

## <a name="guidelines-and-limitations-for-internal-listeners"></a><span data-ttu-id="59654-114">Pokyny a omezení pro interní naslouchací procesy</span><span class="sxs-lookup"><span data-stu-id="59654-114">Guidelines and limitations for internal listeners</span></span>
<span data-ttu-id="59654-115">Použití k interní pro vyrovnávání zatížení (ILB) s naslouchací proces skupiny dostupnosti v Azure se vztahují následující pokyny:</span><span class="sxs-lookup"><span data-stu-id="59654-115">The use of an internal load balancer (ILB) with an availability group listener in Azure is subject to the following guidelines:</span></span>

* <span data-ttu-id="59654-116">Naslouchací proces skupiny dostupnosti je podporován v systému Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="59654-116">The availability group listener is supported on Windows Server 2008 R2, Windows Server 2012, and Windows Server 2012 R2.</span></span>
* <span data-ttu-id="59654-117">Pouze jeden naslouchací proces skupiny dostupnosti interní je podporována pro jednotlivých cloudových služeb, protože je nakonfigurován naslouchací proces pro ILB, a je pouze jeden ILB u každé cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="59654-117">Only one internal availability group listener is supported for each cloud service, because the listener is configured to the ILB, and there is only one ILB for each cloud service.</span></span> <span data-ttu-id="59654-118">Nicméně je možné vytvořit více externí naslouchací procesy.</span><span class="sxs-lookup"><span data-stu-id="59654-118">However, it is possible to create multiple external listeners.</span></span> <span data-ttu-id="59654-119">Další informace najdete v tématu [konfigurace o externí naslouchací proces pro skupiny dostupnosti Always On v Azure](../classic/ps-sql-ext-listener.md).</span><span class="sxs-lookup"><span data-stu-id="59654-119">For more information, see [Configure an external listener for Always On availability groups in Azure](../classic/ps-sql-ext-listener.md).</span></span>

## <a name="determine-the-accessibility-of-the-listener"></a><span data-ttu-id="59654-120">Určení usnadnění naslouchacího procesu</span><span class="sxs-lookup"><span data-stu-id="59654-120">Determine the accessibility of the listener</span></span>
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

<span data-ttu-id="59654-121">Tento článek se zaměřuje na tvorbu naslouchací proces, který používá ILB.</span><span class="sxs-lookup"><span data-stu-id="59654-121">This article focuses on creating a listener that uses an ILB.</span></span> <span data-ttu-id="59654-122">Pokud potřebujete naslouchací proces veřejných nebo externí, přečtěte si verzi tohoto článku, který popisuje nastavení, až [externí naslouchací proces](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59654-122">If you need an public or external listener, see the version of this article that discusses setting up an [external listener](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a><span data-ttu-id="59654-123">Vytvoření koncové body s vyrovnáváním zatížení virtuálních počítačů s přímý server návratový</span><span class="sxs-lookup"><span data-stu-id="59654-123">Create load-balanced VM endpoints with direct server return</span></span>
<span data-ttu-id="59654-124">Nejprve vytvoříte ILB později spuštěním skriptu v této části.</span><span class="sxs-lookup"><span data-stu-id="59654-124">You first create an ILB by running the script later in this section.</span></span>

<span data-ttu-id="59654-125">Vytvořte koncový bod Vyrovnávání zatížení sítě pro každý virtuální počítač, který je hostitelem Azure repliky.</span><span class="sxs-lookup"><span data-stu-id="59654-125">Create a load-balanced endpoint for each VM that hosts an Azure replica.</span></span> <span data-ttu-id="59654-126">Pokud máte repliky v několika oblastech, každá replika pro danou oblast musí být v rámci stejné cloudové služby ve stejné virtuální síti Azure.</span><span class="sxs-lookup"><span data-stu-id="59654-126">If you have replicas in multiple regions, each replica for that region must be in the same cloud service in the same Azure virtual network.</span></span> <span data-ttu-id="59654-127">Vytváření replik skupin, které jsou rozmístěny v několika oblastmi Azure dostupnosti vyžaduje konfiguraci více virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="59654-127">Creating availability group replicas that span multiple Azure regions requires configuring multiple virtual networks.</span></span> <span data-ttu-id="59654-128">Další informace o konfiguraci křížové připojení k virtuální síti, najdete v části [konfigurace virtuální sítě pro připojení k virtuální síti](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span><span class="sxs-lookup"><span data-stu-id="59654-128">For more information on configuring cross virtual network connectivity, see [Configure virtual network to virtual network connectivity](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span></span>

1. <span data-ttu-id="59654-129">Na portálu Azure přejděte na každý virtuální počítač, který je hostitelem repliky zobrazíte podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="59654-129">In the Azure portal, go to each VM that hosts a replica to view the details.</span></span>

2. <span data-ttu-id="59654-130">Klikněte **koncové body** kartu pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="59654-130">Click the **Endpoints** tab for each VM.</span></span>

3. <span data-ttu-id="59654-131">Ověřte, zda **název** a **veřejný Port** naslouchacího procesu koncový bod, který chcete použít nejsou již používán.</span><span class="sxs-lookup"><span data-stu-id="59654-131">Verify that the **Name** and **Public Port** of the listener endpoint that you want to use are not already in use.</span></span> <span data-ttu-id="59654-132">V tomto příkladu v této části, je název *MyEndpoint*, a port, který je *1433*.</span><span class="sxs-lookup"><span data-stu-id="59654-132">In the example in this section, the name is *MyEndpoint*, and the port is *1433*.</span></span>

4. <span data-ttu-id="59654-133">Na místního klienta, stáhněte a nainstalujte nejnovější [modulu PowerShell](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="59654-133">On your local client, download and install the latest [PowerShell module](https://azure.microsoft.com/downloads/).</span></span>

5. <span data-ttu-id="59654-134">Spusťte prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="59654-134">Start Azure PowerShell.</span></span>  
    <span data-ttu-id="59654-135">Novou relaci prostředí PowerShell otevře s Azure pro správu moduly načíst.</span><span class="sxs-lookup"><span data-stu-id="59654-135">A new PowerShell session opens, with the Azure administrative modules loaded.</span></span>

6. <span data-ttu-id="59654-136">Spusťte `Get-AzurePublishSettingsFile`.</span><span class="sxs-lookup"><span data-stu-id="59654-136">Run `Get-AzurePublishSettingsFile`.</span></span> <span data-ttu-id="59654-137">Tato rutina vás přesměruje do prohlížeče a stáhněte soubor nastavení publikování do místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="59654-137">This cmdlet directs you to a browser to download a publish settings file to a local directory.</span></span> <span data-ttu-id="59654-138">Pro vaše předplatné Azure, může být vyzvání k zadání pověření přihlášení.</span><span class="sxs-lookup"><span data-stu-id="59654-138">You might be prompted for your sign-in credentials for your Azure subscription.</span></span>

7. <span data-ttu-id="59654-139">Spusťte následující `Import-AzurePublishSettingsFile` příkazu s cestou soubor nastavení publikování, který jste stáhli:</span><span class="sxs-lookup"><span data-stu-id="59654-139">Run the following `Import-AzurePublishSettingsFile` command with the path of the publish settings file that you downloaded:</span></span>

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    <span data-ttu-id="59654-140">Po importu se soubor nastavení publikování, můžete spravovat vaše předplatné Azure v relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="59654-140">After the publish settings file is imported, you can manage your Azure subscription in the PowerShell session.</span></span>

8. <span data-ttu-id="59654-141">Pro *ILB*, přiřadit statickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="59654-141">For *ILB*, assign a static IP address.</span></span> <span data-ttu-id="59654-142">Zkontrolujte aktuální konfiguraci virtuální sítě tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="59654-142">Examine the current virtual network configuration by running the following command:</span></span>

        (Get-AzureVNetConfig).XMLConfiguration
9. <span data-ttu-id="59654-143">Poznámka: *podsíť* název pro podsíť, která obsahuje virtuální počítače tohoto hostitele repliky.</span><span class="sxs-lookup"><span data-stu-id="59654-143">Note the *Subnet* name for the subnet that contains the VMs that host the replicas.</span></span> <span data-ttu-id="59654-144">Tento název se používá v parametru $SubnetName ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="59654-144">This name is used in the $SubnetName parameter in the script.</span></span>

10. <span data-ttu-id="59654-145">Poznámka: *VirtualNetworkSite* název a počáteční *AddressPrefix* pro podsíť, která obsahuje virtuální počítače, které hostitele repliky.</span><span class="sxs-lookup"><span data-stu-id="59654-145">Note the *VirtualNetworkSite* name and the starting *AddressPrefix* for the subnet that contains the VMs that host the replicas.</span></span> <span data-ttu-id="59654-146">Hledat dostupnou adresu IP pomocí předání obě hodnoty `Test-AzureStaticVNetIP` příkazů a nástrojem prozkoumání *AvailableAddresses*.</span><span class="sxs-lookup"><span data-stu-id="59654-146">Look for an available IP address by passing both values to the `Test-AzureStaticVNetIP` command and by examining the *AvailableAddresses*.</span></span> <span data-ttu-id="59654-147">Například pokud je název virtuální sítě *MyVNet* a má rozsah adres podsítě, která se spouští v *172.16.0.128*, následující příkaz by seznam dostupných adres:</span><span class="sxs-lookup"><span data-stu-id="59654-147">For example, if the virtual network is named *MyVNet* and has a subnet address range that starts at *172.16.0.128*, the following command would list available addresses:</span></span>

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
11. <span data-ttu-id="59654-148">Vyberte jednu z dostupných adres a použít ho v parametru $ILBStaticIP skript v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="59654-148">Select one of the available addresses, and use it in the $ILBStaticIP parameter of the script in the next step.</span></span>

12. <span data-ttu-id="59654-149">Zkopírujte následující skript PowerShell do textového editoru a nastavte hodnoty proměnné tak, aby odpovídaly vašemu prostředí.</span><span class="sxs-lookup"><span data-stu-id="59654-149">Copy the following PowerShell script to a text editor, and set the variable values to suit your environment.</span></span> <span data-ttu-id="59654-150">Výchozí hodnoty jsou uvedeny některé parametry.</span><span class="sxs-lookup"><span data-stu-id="59654-150">Defaults have been provided for some parameters.</span></span>  

    <span data-ttu-id="59654-151">Existující nasazení, které používají skupiny vztahů nelze přidat ILB.</span><span class="sxs-lookup"><span data-stu-id="59654-151">Existing deployments that use affinity groups cannot add an ILB.</span></span> <span data-ttu-id="59654-152">Další informace o požadavcích na ILB najdete v tématu [přehled nástroje pro vyrovnávání zatížení pro vnitřní](../../../load-balancer/load-balancer-internal-overview.md).</span><span class="sxs-lookup"><span data-stu-id="59654-152">For more information about ILB requirements, see [Internal load balancer overview](../../../load-balancer/load-balancer-internal-overview.md).</span></span>

    <span data-ttu-id="59654-153">Navíc pokud vaší skupiny dostupnosti zahrnuje oblasti Azure, musíte spustit skript jednou v každé datové centrum pro cloudové služby a uzly, které jsou umístěny ve stejné datové centrum.</span><span class="sxs-lookup"><span data-stu-id="59654-153">Also, if your availability group spans Azure regions, you must run the script once in each datacenter for the cloud service and nodes that reside in that datacenter.</span></span>

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the virtual network
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load-balanced endpoint for each node in $AGNodes by using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

13. <span data-ttu-id="59654-154">Po nastavení proměnné, zkopírujte do relace prostředí PowerShell ji spustit skript z textového editoru.</span><span class="sxs-lookup"><span data-stu-id="59654-154">After you have set the variables, copy the script from the text editor to your PowerShell session to run it.</span></span> <span data-ttu-id="59654-155">Pokud stále zobrazuje řádku  **>>** , stisknutím klávesy Enter zajistěte, aby je skript spuštěn.</span><span class="sxs-lookup"><span data-stu-id="59654-155">If the prompt still shows **>>**, press Enter again to make sure the script starts running.</span></span>

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a><span data-ttu-id="59654-156">Ověřte, zda KB2854082 nainstalována v případě potřeby</span><span class="sxs-lookup"><span data-stu-id="59654-156">Verify that KB2854082 is installed if necessary</span></span>
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a><span data-ttu-id="59654-157">Otevřít porty brány firewall v uzlech skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="59654-157">Open the firewall ports in availability group nodes</span></span>
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a><span data-ttu-id="59654-158">Vytvoření naslouchacího procesu skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="59654-158">Create the availability group listener</span></span>

<span data-ttu-id="59654-159">Vytvořte naslouchací proces skupiny dostupnosti ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="59654-159">Create the availability group listener in two steps.</span></span> <span data-ttu-id="59654-160">Nejprve vytvořte prostředek clusteru bodu přístupu klienta a nakonfigurovat závislosti.</span><span class="sxs-lookup"><span data-stu-id="59654-160">First, create the client access point cluster resource and configure  dependencies.</span></span> <span data-ttu-id="59654-161">Druhý nakonfigurujte prostředky clusteru v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="59654-161">Second, configure the cluster resources in PowerShell.</span></span>

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a><span data-ttu-id="59654-162">Vytvořit klientský přístupový bod a nakonfigurovat závislosti clusteru</span><span class="sxs-lookup"><span data-stu-id="59654-162">Create the client access point and configure the cluster dependencies</span></span>
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a><span data-ttu-id="59654-163">Konfigurace prostředků clusteru v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="59654-163">Configure the cluster resources in PowerShell</span></span>
1. <span data-ttu-id="59654-164">Pro ILB musíte použít IP adresu ILB, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="59654-164">For ILB, you must use the IP address of the ILB that was created earlier.</span></span> <span data-ttu-id="59654-165">Chcete-li získat tuto IP adresu v prostředí PowerShell, použijte následující skript:</span><span class="sxs-lookup"><span data-stu-id="59654-165">To obtain this IP address in PowerShell, use the following script:</span></span>

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

2. <span data-ttu-id="59654-166">Na jednom z virtuálních počítačů zkopírujte skript prostředí PowerShell pro váš operační systém do textového editoru a nastavte proměnné na hodnoty, které jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="59654-166">On one of the VMs, copy the PowerShell script for your operating system to a text editor, and then set the variables to the values you noted earlier.</span></span>

    <span data-ttu-id="59654-167">Pro Windows Server 2012 nebo novější použijte tento skript:</span><span class="sxs-lookup"><span data-stu-id="59654-167">For Windows Server 2012 or later, use the following script:</span></span>

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = “<X.X.X.X>” # the IP address of the ILB

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

    <span data-ttu-id="59654-168">Pro Windows Server 2008 R2 použijte následující skript:</span><span class="sxs-lookup"><span data-stu-id="59654-168">For Windows Server 2008 R2, use the following script:</span></span>

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = “<X.X.X.X>” # the IP address of the ILB

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

3. <span data-ttu-id="59654-169">Po nastavení proměnné, otevřete okno prostředí Windows PowerShell se zvýšenými oprávněními, vložte do relace prostředí PowerShell ji spustit skript z textového editoru.</span><span class="sxs-lookup"><span data-stu-id="59654-169">After you have set the variables, open an elevated Windows PowerShell window, paste the script from the text editor into your PowerShell session to run it.</span></span> <span data-ttu-id="59654-170">Pokud stále zobrazuje řádku  **>>** , stiskněte klávesu Enter znovu, abyste měli jistotu, že je skript spuštěn.</span><span class="sxs-lookup"><span data-stu-id="59654-170">If the prompt still shows **>>**, Press Enter again to make sure that the script starts running.</span></span>

4. <span data-ttu-id="59654-171">Opakujte předchozí kroky pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="59654-171">Repeat the preceding steps for each VM.</span></span>  
    <span data-ttu-id="59654-172">Tento skript nakonfiguruje prostředek IP adresy s IP adresou cloudové služby a nastaví dalších parametrů, jako je port testu.</span><span class="sxs-lookup"><span data-stu-id="59654-172">This script configures the IP address resource with the IP address of the cloud service and sets other parameters, such as the probe port.</span></span> <span data-ttu-id="59654-173">Pokud prostředek IP adresy je uvést do režimu online, může reagovat na dotazování na port testu z koncového bodu Vyrovnávání zatížení sítě, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="59654-173">When the IP address resource is brought online, it can respond to the polling on the probe port from the load-balanced endpoint that you created earlier.</span></span>

## <a name="bring-the-listener-online"></a><span data-ttu-id="59654-174">Přepněte naslouchací proces online</span><span class="sxs-lookup"><span data-stu-id="59654-174">Bring the listener online</span></span>
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a><span data-ttu-id="59654-175">Položky následnou akci</span><span class="sxs-lookup"><span data-stu-id="59654-175">Follow-up items</span></span>
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-virtual-network"></a><span data-ttu-id="59654-176">Testování naslouchacího procesu skupiny dostupnosti (v rámci stejné virtuální síti)</span><span class="sxs-lookup"><span data-stu-id="59654-176">Test the availability group listener (within the same virtual network)</span></span>
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a><span data-ttu-id="59654-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59654-177">Next steps</span></span>
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]
