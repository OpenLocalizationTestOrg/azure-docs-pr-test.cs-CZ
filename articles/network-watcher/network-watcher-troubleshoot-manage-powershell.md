---
title: "aaaTroubleshoot brány virtuální sítě Azure a připojení – prostředí PowerShell | Microsoft Docs"
description: "Tato stránka vysvětluje, jak řešit toouse hello sledovací proces sítě Azure rutiny prostředí PowerShell"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: b7dbe6e44e0fa1ea0481223a84098d7a57c068a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a><span data-ttu-id="1ccbc-103">Řešení potíží s brány virtuální sítě a připojení pomocí prostředí PowerShell sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="1ccbc-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="1ccbc-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1ccbc-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="1ccbc-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1ccbc-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="1ccbc-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="1ccbc-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="1ccbc-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="1ccbc-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="1ccbc-108">REST API</span><span class="sxs-lookup"><span data-stu-id="1ccbc-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="1ccbc-109">Sledovací proces sítě obsahuje řadu funkcí, protože se týká toounderstanding síťovým prostředkům v Azure.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-109">Network Watcher provides many capabilities as it relates toounderstanding your network resources in Azure.</span></span> <span data-ttu-id="1ccbc-110">Jeden z těchto funkcí je prostředek řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="1ccbc-111">Řešení potíží s prostředků je možné volat prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-111">Resource troubleshooting can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="1ccbc-112">Při volání, sledovací proces sítě kontroluje stav hello bránu virtuální sítě nebo připojení a vrátí nalezených výsledcích.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-112">When called, Network Watcher inspects hello health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="1ccbc-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1ccbc-113">Before you begin</span></span>

<span data-ttu-id="1ccbc-114">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

<span data-ttu-id="1ccbc-115">Seznam podporovaných brány typy návštěvě [typy podporované brány](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="1ccbc-115">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="1ccbc-116">Přehled</span><span class="sxs-lookup"><span data-stu-id="1ccbc-116">Overview</span></span>

<span data-ttu-id="1ccbc-117">Řešení potíží s prostředků poskytuje hello možnosti řešení potíží, které nastat u brány virtuální sítě a připojení.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-117">Resource troubleshooting provides hello ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="1ccbc-118">Když je žádosti tooresource řešení potíží s protokoly Probíhá dotaz a prověřovány.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-118">When a request is made tooresource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="1ccbc-119">Po dokončení kontroly hello výsledky se vrátí.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-119">When inspection is complete, hello results are returned.</span></span> <span data-ttu-id="1ccbc-120">Požadavky na zdroje, řešení problémů s požadavky jsou dlouho spuštěný, což může trvat několik minut tooreturn výsledku.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-120">Resource troubleshooting requests are long running requests, which could take multiple minutes tooreturn a result.</span></span> <span data-ttu-id="1ccbc-121">Hello protokoly z řešení potíží se ukládají v kontejneru v účtu úložiště, který je zadán.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-121">hello logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="1ccbc-122">Načtení sledovací proces sítě</span><span class="sxs-lookup"><span data-stu-id="1ccbc-122">Retrieve Network Watcher</span></span>

<span data-ttu-id="1ccbc-123">prvním krokem Hello je tooretrieve hello sledovací proces sítě instance.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-123">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="1ccbc-124">Hello `$networkWatcher` proměnné je předán toohello `Start-AzureRmNetworkWatcherResourceTroubleshooting` rutiny v kroku 4.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-124">hello `$networkWatcher` variable is passed toohello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet in step 4.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="retrieve-a-virtual-network-gateway-connection"></a><span data-ttu-id="1ccbc-125">Načtení připojení brány virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="1ccbc-125">Retrieve a Virtual Network Gateway Connection</span></span>

<span data-ttu-id="1ccbc-126">V tomto příkladu je se řešení potíží s prostředků spustili na připojení.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-126">In this example, resource troubleshooting is being ran on a Connection.</span></span> <span data-ttu-id="1ccbc-127">Můžete také předat ji bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-127">You can also pass it a Virtual Network Gateway.</span></span>

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "2to3" -ResourceGroupName "testrg"
```

## <a name="create-a-storage-account"></a><span data-ttu-id="1ccbc-128">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="1ccbc-128">Create a storage account</span></span>

<span data-ttu-id="1ccbc-129">Řešení potíží s prostředků vrátí data o stavu hello hello prostředku, uloží zároveň protokoly tooa úložiště účet toobe zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-129">Resource troubleshooting returns data about hello health of hello resource, it also saves logs tooa storage account toobe reviewed.</span></span> <span data-ttu-id="1ccbc-130">V tomto kroku vytvoříme účet úložiště, pokud existuje stávající účet úložiště můžete ji použít.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-130">In this step, we create a storage account, if an existing storage account exists you can use it.</span></span>

```powershell
$sa = New-AzureRmStorageAccount -Name "contosoexamplesa" -SKU "Standard_LRS" -ResourceGroupName "testrg" -Location "WestCentralUS"
Set-AzureRmCurrentStorageAccount -ResourceGroupName $sa.ResourceGroupName -Name $sa.StorageAccountName
$sc = New-AzureStorageContainer -Name logs
```

## <a name="run-network-watcher-resource-troubleshooting"></a><span data-ttu-id="1ccbc-131">Spustit Poradce při potížích sledovací proces sítě prostředků</span><span class="sxs-lookup"><span data-stu-id="1ccbc-131">Run Network Watcher resource troubleshooting</span></span>

<span data-ttu-id="1ccbc-132">Řešení potíží s prostředky s hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` rutiny.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-132">You troubleshoot resources with hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet.</span></span> <span data-ttu-id="1ccbc-133">Jsme předat objekt hello rutiny hello sledovací proces sítě, hello Id hello připojení nebo brány virtuální sítě, id účtu úložiště hello a hello cesta toostore hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-133">We pass hello cmdlet hello Network Watcher object, hello Id of hello Connection or Virtual Network Gateway, hello storage account id, and hello path toostore hello results.</span></span>

> [!NOTE]
> <span data-ttu-id="1ccbc-134">Hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` rutina dlouho spuštěný a může trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-134">hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet is long running and may take a few minutes toocomplete.</span></span>

```powershell
Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath "$($sa.PrimaryEndpoints.Blob)$($sc.name)"
```

<span data-ttu-id="1ccbc-135">Po spuštění rutiny hello sledovací proces sítě zkontroluje stav hello tooverify hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-135">Once you run hello cmdlet, Network Watcher reviews hello resource tooverify hello health.</span></span> <span data-ttu-id="1ccbc-136">Se vrátí výsledky hello toohello prostředí a ukládá protokoly hello výsledků v zadaný účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-136">It returns hello results toohello shell and stores logs of hello results in hello storage account specified.</span></span>

## <a name="understanding-hello-results"></a><span data-ttu-id="1ccbc-137">Seznámení s výsledky hello</span><span class="sxs-lookup"><span data-stu-id="1ccbc-137">Understanding hello results</span></span>

<span data-ttu-id="1ccbc-138">text Hello akce obsahuje obecné pokyny k jak tooresolve hello problém.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-138">hello action text provides general guidance on how tooresolve hello issue.</span></span> <span data-ttu-id="1ccbc-139">Pokud může být akce hello problému, je k dispozici odkaz s další pokyny.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-139">If an action can be taken for hello issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="1ccbc-140">V případě hello tam, kde není žádná další pokyny, hello odpovědi poskytuje hello url tooopen případu podpory.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-140">In hello case where there is no additional guidance, hello response provides hello url tooopen a support case.</span></span>  <span data-ttu-id="1ccbc-141">Další informace o vlastnostech hello hello odpovědi a co je součástí najdete v článku [řešení sledovací proces sítě – přehled](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="1ccbc-141">For more information about hello properties of hello response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="1ccbc-142">Pokyny ke stahování souborů z účty azure storage, najdete v části příliš[Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="1ccbc-142">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="1ccbc-143">Jiný nástroj, který je možné je Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-143">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="1ccbc-144">Další informace o Storage Explorer naleznete zde na hello následující odkaz: [Storage Explorer](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="1ccbc-144">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ccbc-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ccbc-145">Next steps</span></span>

<span data-ttu-id="1ccbc-146">Pokud nastavení bylo změněno tohoto připojení VPN zastavit, přečtěte si téma [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které může být nejistá.</span><span class="sxs-lookup"><span data-stu-id="1ccbc-146">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that may be in question.</span></span>
