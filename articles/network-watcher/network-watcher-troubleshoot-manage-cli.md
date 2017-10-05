---
title: "Řešení potíží s brány virtuální sítě Azure a připojení - rozhraní příkazového řádku Azure 2.0 | Microsoft Docs"
description: "Tato stránka vysvětluje, jak používat Azure sledovací proces sítě řešení potíží s Azure CLI 2.0"
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
ms.openlocfilehash: e1d56317b10fa738a3d7089f6c4f357159fe2c2b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-20"></a><span data-ttu-id="f6b8d-103">Řešení potíží s brány virtuální sítě a připojení pomocí Azure sítě sledovacích procesů Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f6b8d-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="f6b8d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f6b8d-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="f6b8d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f6b8d-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="f6b8d-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f6b8d-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="f6b8d-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f6b8d-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="f6b8d-108">REST API</span><span class="sxs-lookup"><span data-stu-id="f6b8d-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="f6b8d-109">Sledovací proces sítě obsahuje řadu funkcí ve vztahu k pochopení síťovým prostředkům v Azure.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-109">Network Watcher provides many capabilities as it relates to understanding your network resources in Azure.</span></span> <span data-ttu-id="f6b8d-110">Jeden z těchto funkcí je prostředek řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="f6b8d-111">Řešení potíží s prostředků je možné volat prostřednictvím portálu, prostředí PowerShell, rozhraní příkazového řádku nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-111">Resource troubleshooting can be called through the portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="f6b8d-112">Při volání, sledovací proces sítě kontroluje stav bránu virtuální sítě nebo připojení a vrátí nalezených výsledcích.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-112">When called, Network Watcher inspects the health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

<span data-ttu-id="f6b8d-113">Tento článek používá naší nové generace rozhraní příkazového řádku pro model nasazení prostředků management, Azure CLI 2.0, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-113">This article uses our next generation CLI for the resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="f6b8d-114">Chcete-li provést kroky v tomto článku, je potřeba [nainstalovat rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="f6b8d-114">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f6b8d-115">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f6b8d-115">Before you begin</span></span>

<span data-ttu-id="f6b8d-116">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-116">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

<span data-ttu-id="f6b8d-117">Seznam podporovaných brány typy návštěvě [typy podporované brány](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="f6b8d-117">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="f6b8d-118">Přehled</span><span class="sxs-lookup"><span data-stu-id="f6b8d-118">Overview</span></span>

<span data-ttu-id="f6b8d-119">Řešení potíží s prostředků nabízí možnost odstraňování problémů, které nastat u brány virtuální sítě a připojení.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-119">Resource troubleshooting provides the ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="f6b8d-120">Po odeslání žádosti k prostředku, řešení potíží s protokoly Probíhá dotaz a prověřovány.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-120">When a request is made to resource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="f6b8d-121">Po dokončení kontroly budou vráceny výsledky.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-121">When inspection is complete, the results are returned.</span></span> <span data-ttu-id="f6b8d-122">Prostředek, který řešení problémů s požadavky jsou dlouho spuštěný žádosti, což může trvat několik minut vrácení výsledku.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-122">Resource troubleshooting requests are long running requests, which could take multiple minutes to return a result.</span></span> <span data-ttu-id="f6b8d-123">Protokoly z řešení potíží se ukládají v kontejneru v účtu úložiště, který je zadán.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-123">The logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="retrieve-a-virtual-network-gateway-connection"></a><span data-ttu-id="f6b8d-124">Načtení připojení brány virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="f6b8d-124">Retrieve a Virtual Network Gateway Connection</span></span>

<span data-ttu-id="f6b8d-125">V tomto příkladu je se řešení potíží s prostředků spustili na připojení.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-125">In this example, resource troubleshooting is being ran on a Connection.</span></span> <span data-ttu-id="f6b8d-126">Můžete také předat ji bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-126">You can also pass it a Virtual Network Gateway.</span></span> <span data-ttu-id="f6b8d-127">Následující rutiny uvádí připojení vpn ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-127">The following cmdlet lists the vpn-connections in a resource group.</span></span>

```azurecli
az network vpn-connection list --resource-group resourceGroupName
```

<span data-ttu-id="f6b8d-128">Jakmile máte název připojení, můžete spustit tento příkaz získat jeho Id prostředku:</span><span class="sxs-lookup"><span data-stu-id="f6b8d-128">Once you have the name of the connection, you can run this command to get its resource Id:</span></span>

```azurecli
az network vpn-connection show --resource-group resourceGroupName --ids vpnConnectionIds
```

## <a name="create-a-storage-account"></a><span data-ttu-id="f6b8d-129">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="f6b8d-129">Create a storage account</span></span>

<span data-ttu-id="f6b8d-130">Řešení potíží s prostředků vrátí data o stavu prostředku, také ukládá protokoly na účet úložiště mají být zkontrolovány.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-130">Resource troubleshooting returns data about the health of the resource, it also saves logs to a storage account to be reviewed.</span></span> <span data-ttu-id="f6b8d-131">V tomto kroku vytvoříme účet úložiště, pokud existuje stávající účet úložiště můžete ji použít.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-131">In this step, we create a storage account, if an existing storage account exists you can use it.</span></span>

1. <span data-ttu-id="f6b8d-132">Vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="f6b8d-132">Create the storage account</span></span>

    ```azurecli
    az storage account create --name storageAccountName --location westcentralus --resource-group resourceGroupName --sku Standard_LRS
    ```

1. <span data-ttu-id="f6b8d-133">Získání klíče účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="f6b8d-133">Get the storage account keys</span></span>

    ```azurecli
    az storage account keys list --resource-group resourcegroupName --account-name storageAccountName
    ```

1. <span data-ttu-id="f6b8d-134">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="f6b8d-134">Create the container</span></span>

    ```azurecli
    az storage container create --account-name storageAccountName --account-key {storageAccountKey} --name logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a><span data-ttu-id="f6b8d-135">Spustit Poradce při potížích sledovací proces sítě prostředků</span><span class="sxs-lookup"><span data-stu-id="f6b8d-135">Run Network Watcher resource troubleshooting</span></span>

<span data-ttu-id="f6b8d-136">Řešení potíží s prostředky s `az network watcher troubleshooting` rutiny.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-136">You troubleshoot resources with the `az network watcher troubleshooting` cmdlet.</span></span> <span data-ttu-id="f6b8d-137">Jsme předat rutinu skupinu prostředků, název sledovací proces sítě, Id připojení, Id účtu úložiště a výsledkem cesta k objektu blob do úložiště řešení.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-137">We pass the cmdlet the resource group, the name of the Network Watcher, the Id of the connection, the Id of the storage account, and the path to the blob to store the troubleshoot results in.</span></span>

```azurecli
az network watcher troubleshooting start --resource-group resourceGroupName --resource resourceName --resource-type {vnetGateway/vpnConnection} --storage-account storageAccountName  --storage-path https://{storageAccountName}.blob.core.windows.net/{containerName}
```

<span data-ttu-id="f6b8d-138">Po spuštění rutiny sledovací proces sítě zkontroluje prostředek ověření stavu.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-138">Once you run the cmdlet, Network Watcher reviews the resource to verify the health.</span></span> <span data-ttu-id="f6b8d-139">Se vrátí výsledky do prostředí a ukládá protokoly výsledky v zadaný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-139">It returns the results to the shell and stores logs of the results in the storage account specified.</span></span>

## <a name="understanding-the-results"></a><span data-ttu-id="f6b8d-140">Seznámení s výsledky</span><span class="sxs-lookup"><span data-stu-id="f6b8d-140">Understanding the results</span></span>

<span data-ttu-id="f6b8d-141">Text akce obsahuje obecné pokyny k vyřešení problému.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-141">The action text provides general guidance on how to resolve the issue.</span></span> <span data-ttu-id="f6b8d-142">Pokud může být akce pro tento problém, je k dispozici odkaz s další pokyny.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-142">If an action can be taken for the issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="f6b8d-143">V případě, že tam, kde není žádná další pokyny, odpověď obsahuje adresu url pro otevření případu podpory.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-143">In the case where there is no additional guidance, the response provides the url to open a support case.</span></span>  <span data-ttu-id="f6b8d-144">Další informace o vlastnostech odpovědi a co je součástí najdete v článku [řešení sledovací proces sítě – přehled](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="f6b8d-144">For more information about the properties of the response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="f6b8d-145">Pokyny ke stahování souborů z účty azure storage, najdete v části [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="f6b8d-145">For instructions on downloading files from azure storage accounts, refer to [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="f6b8d-146">Jiný nástroj, který je možné je Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-146">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="f6b8d-147">Další informace o Storage Explorer naleznete zde na následující odkaz: [Storage Explorer](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="f6b8d-147">More information about Storage Explorer can be found here at the following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6b8d-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f6b8d-148">Next steps</span></span>

<span data-ttu-id="f6b8d-149">Pokud nastavení bylo změněno tohoto připojení VPN zastavit, přečtěte si téma [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) sledovat pravidla zabezpečení sítě skupiny a zabezpečení, které může být nejistá.</span><span class="sxs-lookup"><span data-stu-id="f6b8d-149">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that may be in question.</span></span>
