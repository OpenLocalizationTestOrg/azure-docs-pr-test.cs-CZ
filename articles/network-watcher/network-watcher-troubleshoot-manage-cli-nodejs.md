---
title: "aaaTroubleshoot brány virtuální sítě Azure a připojení - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak toouse hello sledovací proces sítě Azure vyřešit potíže s Azure CLI 1.0"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: a0511689d773f9c7216b0fe5470e27f754eb600b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-10"></a><span data-ttu-id="f5396-103">Řešení potíží s brány virtuální sítě a připojení pomocí Azure sítě sledovacích procesů Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f5396-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="f5396-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f5396-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="f5396-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f5396-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="f5396-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f5396-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="f5396-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f5396-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="f5396-108">REST API</span><span class="sxs-lookup"><span data-stu-id="f5396-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="f5396-109">Sledovací proces sítě obsahuje řadu funkcí, protože se týká toounderstanding síťovým prostředkům v Azure.</span><span class="sxs-lookup"><span data-stu-id="f5396-109">Network Watcher provides many capabilities as it relates toounderstanding your network resources in Azure.</span></span> <span data-ttu-id="f5396-110">Jeden z těchto funkcí je prostředek řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="f5396-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="f5396-111">Řešení potíží s prostředků je možné volat prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="f5396-111">Resource troubleshooting can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="f5396-112">Při volání, sledovací proces sítě kontroluje stav hello bránu virtuální sítě nebo připojení a vrátí nalezených výsledcích.</span><span class="sxs-lookup"><span data-stu-id="f5396-112">When called, Network Watcher inspects hello health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

<span data-ttu-id="f5396-113">Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="f5396-113">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="f5396-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f5396-114">Before you begin</span></span>

<span data-ttu-id="f5396-115">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="f5396-115">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

<span data-ttu-id="f5396-116">Seznam podporovaných brány typy návštěvě [typy podporované brány](/network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="f5396-116">For a list of supported gateway types visit, [Supported Gateway types](/network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="f5396-117">Přehled</span><span class="sxs-lookup"><span data-stu-id="f5396-117">Overview</span></span>

<span data-ttu-id="f5396-118">Řešení potíží s prostředků poskytuje hello možnosti řešení potíží, které nastat u brány virtuální sítě a připojení.</span><span class="sxs-lookup"><span data-stu-id="f5396-118">Resource troubleshooting provides hello ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="f5396-119">Když je žádosti tooresource řešení potíží s protokoly Probíhá dotaz a prověřovány.</span><span class="sxs-lookup"><span data-stu-id="f5396-119">When a request is made tooresource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="f5396-120">Po dokončení kontroly hello výsledky se vrátí.</span><span class="sxs-lookup"><span data-stu-id="f5396-120">When inspection is complete, hello results are returned.</span></span> <span data-ttu-id="f5396-121">Požadavky na zdroje, řešení problémů s požadavky jsou dlouho spuštěný, což může trvat několik minut tooreturn výsledku.</span><span class="sxs-lookup"><span data-stu-id="f5396-121">Resource troubleshooting requests are long running requests, which could take multiple minutes tooreturn a result.</span></span> <span data-ttu-id="f5396-122">Hello protokoly z řešení potíží se ukládají v kontejneru v účtu úložiště, který je zadán.</span><span class="sxs-lookup"><span data-stu-id="f5396-122">hello logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="retrieve-a-virtual-network-gateway-connection"></a><span data-ttu-id="f5396-123">Načtení připojení brány virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="f5396-123">Retrieve a Virtual Network Gateway Connection</span></span>

<span data-ttu-id="f5396-124">V tomto příkladu je se řešení potíží s prostředků spustili na připojení.</span><span class="sxs-lookup"><span data-stu-id="f5396-124">In this example, resource troubleshooting is being ran on a Connection.</span></span> <span data-ttu-id="f5396-125">Můžete také předat ji bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="f5396-125">You can also pass it a Virtual Network Gateway.</span></span> <span data-ttu-id="f5396-126">Hello následující rutiny uvádí připojení vpn hello ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="f5396-126">hello following cmdlet lists hello vpn-connections in a resource group.</span></span>

```azurecli
azure network vpn-connection list -g resourceGroupName
```

<span data-ttu-id="f5396-127">Můžete také spustit příkaz toosee hello hello připojení v předplatném.</span><span class="sxs-lookup"><span data-stu-id="f5396-127">You can also run hello command toosee hello connections in a subscription.</span></span>

```azurecli
azure network vpn-connection list -s subscription
```

<span data-ttu-id="f5396-128">Jakmile máte hello název hello připojení, můžete spustit tento příkaz tooget jeho Id prostředku:</span><span class="sxs-lookup"><span data-stu-id="f5396-128">Once you have hello name of hello connection, you can run this command tooget its resource Id:</span></span>

```azurecli
azure network vpn-connection show -g resourceGroupName -n connectionName
```

## <a name="create-a-storage-account"></a><span data-ttu-id="f5396-129">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="f5396-129">Create a storage account</span></span>

<span data-ttu-id="f5396-130">Řešení potíží s prostředků vrátí data o stavu hello hello prostředku, uloží zároveň protokoly tooa úložiště účet toobe zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="f5396-130">Resource troubleshooting returns data about hello health of hello resource, it also saves logs tooa storage account toobe reviewed.</span></span> <span data-ttu-id="f5396-131">V tomto kroku vytvoříme účet úložiště, pokud existuje stávající účet úložiště můžete ji použít.</span><span class="sxs-lookup"><span data-stu-id="f5396-131">In this step, we create a storage account, if an existing storage account exists you can use it.</span></span>

1. <span data-ttu-id="f5396-132">Vytvořit účet úložiště hello</span><span class="sxs-lookup"><span data-stu-id="f5396-132">Create hello storage account</span></span>

    ```azurecli
    azure storage account create -n storageAccountName -l location -g resourceGroupName
    ```

1. <span data-ttu-id="f5396-133">Získání klíče účtu úložiště hello</span><span class="sxs-lookup"><span data-stu-id="f5396-133">Get hello storage account keys</span></span>

    ```azurecli
    azure storage account keys list storageAccountName -g resourcegroupName
    ```

1. <span data-ttu-id="f5396-134">Vytvoření kontejneru hello</span><span class="sxs-lookup"><span data-stu-id="f5396-134">Create hello container</span></span>

    ```azurecli
    azure storage container create --account-name storageAccountName -g resourcegroupName --account-key {storageAccountKey} --container logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a><span data-ttu-id="f5396-135">Spustit Poradce při potížích sledovací proces sítě prostředků</span><span class="sxs-lookup"><span data-stu-id="f5396-135">Run Network Watcher resource troubleshooting</span></span>

<span data-ttu-id="f5396-136">Řešení potíží s prostředky s hello `network watcher troubleshoot` rutiny.</span><span class="sxs-lookup"><span data-stu-id="f5396-136">You troubleshoot resources with hello `network watcher troubleshoot` cmdlet.</span></span> <span data-ttu-id="f5396-137">Jsme předat skupinu prostředků hello hello rutiny, hello název hello sledovací proces sítě, hello Id připojení hello hello Id účtu úložiště hello a hello cesta toohello blob toostore hello řešení potíží s výsledky v.</span><span class="sxs-lookup"><span data-stu-id="f5396-137">We pass hello cmdlet hello resource group, hello name of hello Network Watcher, hello Id of hello connection, hello Id of hello storage account, and hello path toohello blob toostore hello troubleshoot results in.</span></span>

```azurecli
azure network watcher troubleshoot -g resourceGroupName -n networkWatcherName -t connectionId -i storageId -p storagePath
```

<span data-ttu-id="f5396-138">Po spuštění rutiny hello sledovací proces sítě zkontroluje stav hello tooverify hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="f5396-138">Once you run hello cmdlet, Network Watcher reviews hello resource tooverify hello health.</span></span> <span data-ttu-id="f5396-139">Se vrátí výsledky hello toohello prostředí a ukládá protokoly hello výsledků v zadaný účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="f5396-139">It returns hello results toohello shell and stores logs of hello results in hello storage account specified.</span></span>

## <a name="understanding-hello-results"></a><span data-ttu-id="f5396-140">Seznámení s výsledky hello</span><span class="sxs-lookup"><span data-stu-id="f5396-140">Understanding hello results</span></span>

<span data-ttu-id="f5396-141">text Hello akce obsahuje obecné pokyny k jak tooresolve hello problém.</span><span class="sxs-lookup"><span data-stu-id="f5396-141">hello action text provides general guidance on how tooresolve hello issue.</span></span> <span data-ttu-id="f5396-142">Pokud může být akce hello problému, je k dispozici odkaz s další pokyny.</span><span class="sxs-lookup"><span data-stu-id="f5396-142">If an action can be taken for hello issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="f5396-143">V případě hello tam, kde není žádná další pokyny, hello odpovědi poskytuje hello url tooopen případu podpory.</span><span class="sxs-lookup"><span data-stu-id="f5396-143">In hello case where there is no additional guidance, hello response provides hello url tooopen a support case.</span></span>  <span data-ttu-id="f5396-144">Další informace o vlastnostech hello hello odpovědi a co je součástí najdete v článku [řešení sledovací proces sítě – přehled](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="f5396-144">For more information about hello properties of hello response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="f5396-145">Pokyny ke stahování souborů z účty azure storage, najdete v části příliš[Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="f5396-145">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="f5396-146">Jiný nástroj, který je možné je Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="f5396-146">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="f5396-147">Další informace o Storage Explorer naleznete zde na hello následující odkaz: [Storage Explorer](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="f5396-147">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f5396-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5396-148">Next steps</span></span>

<span data-ttu-id="f5396-149">Pokud nastavení bylo změněno tohoto připojení VPN zastavit, přečtěte si téma [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které může být nejistá.</span><span class="sxs-lookup"><span data-stu-id="f5396-149">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that may be in question.</span></span>
