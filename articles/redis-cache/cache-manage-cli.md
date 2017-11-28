---
title: "aaaManage Azure Redis Cache pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall hello rozhraní příkazového řádku Azure na jakékoli platformě, jak toouse ho tooconnect tooyour účet Azure a jak toocreate a správě mezipaměti Redis z hello rozhraní příkazového řádku Azure."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 964ff245-859d-4bc1-bccf-62e4b3c1169f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: sdanie
ms.openlocfilehash: e8ee30090133e6b4edea93dcd13fd171e75989bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-azure-redis-cache-using-hello-azure-command-line-interface-azure-cli"></a><span data-ttu-id="c78b2-103">Jak toocreate a spravovat Azure Redis Cache pomocí hello rozhraní příkazového řádku Azure (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="c78b2-103">How toocreate and manage Azure Redis Cache using hello Azure Command-Line Interface (Azure CLI)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c78b2-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c78b2-104">PowerShell</span></span>](cache-howto-manage-redis-cache-powershell.md)
> * [<span data-ttu-id="c78b2-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c78b2-105">Azure CLI</span></span>](cache-manage-cli.md)
>
>

<span data-ttu-id="c78b2-106">Hello rozhraní příkazového řádku Azure je skvělým způsobem toomanage infrastruktury Azure z libovolné platformě.</span><span class="sxs-lookup"><span data-stu-id="c78b2-106">hello Azure CLI is a great way toomanage your Azure infrastructure from any platform.</span></span> <span data-ttu-id="c78b2-107">Tento článek ukazuje, jak toocreate a správě vaší instancí Azure Redis Cache pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="c78b2-107">This article shows you how toocreate and manage your Azure Redis Cache instances using hello Azure CLI.</span></span>

> [!NOTE]
> <span data-ttu-id="c78b2-108">Tento článek se týká tooa předchozí verzi rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="c78b2-108">This article applies tooa previous version of Azure CLI.</span></span> <span data-ttu-id="c78b2-109">Hello nejnovější Azure CLI 2.0 ukázkové skripty, najdete v části [ukázky rozhraní příkazového řádku služby Azure Redis cache](cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c78b2-109">For hello latest Azure CLI 2.0 sample scripts, see [Azure CLI Redis cache samples](cli-samples.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="c78b2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c78b2-110">Prerequisites</span></span>
<span data-ttu-id="c78b2-111">toocreate a spravovat instance služby Azure Redis Cache pomocí rozhraní příkazového řádku Azure, je třeba provést následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="c78b2-111">toocreate and manage Azure Redis Cache instances using Azure CLI, you must complete hello following steps.</span></span>

* <span data-ttu-id="c78b2-112">Musí mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="c78b2-112">You must have an Azure account.</span></span> <span data-ttu-id="c78b2-113">Pokud nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) za pár chvil.</span><span class="sxs-lookup"><span data-stu-id="c78b2-113">If you don't have one, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a few moments.</span></span>
* <span data-ttu-id="c78b2-114">[Instalace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="c78b2-114">[Install hello Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="c78b2-115">Připojit vaše instalace rozhraní příkazového řádku Azure osobní účet Azure, nebo s pracovní nebo školní účet Azure a přihlásit z hello rozhraní příkazového řádku Azure pomocí hello `azure login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c78b2-115">Connect your Azure CLI installation with a personal Azure account, or with a work or school Azure account, and log in from hello Azure CLI using hello `azure login` command.</span></span> <span data-ttu-id="c78b2-116">toounderstand hello rozdíly a vybrat, najdete v části [připojit tooan předplatného Azure z rozhraní příkazového řádku Azure (Azure CLI) hello](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="c78b2-116">toounderstand hello differences and choose, see [Connect tooan Azure subscription from hello Azure Command-Line Interface (Azure CLI)](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="c78b2-117">Před spuštěním, žádný z následujících příkazů hello, přepnout do režimu Resource Manager hello rozhraní příkazového řádku Azure spuštěním hello `azure config mode arm` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c78b2-117">Before running any of hello following commands, switch hello Azure CLI into Resource Manager mode by running hello `azure config mode arm` command.</span></span> <span data-ttu-id="c78b2-118">Další informace najdete v tématu [pomocí rozhraní příkazového řádku Azure toomanage hello Azure skupiny prostředků a prostředky](../xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c78b2-118">For more information, see [Use hello Azure CLI toomanage Azure resources and resource groups](../xplat-cli-azure-resource-manager.md).</span></span>

## <a name="redis-cache-properties"></a><span data-ttu-id="c78b2-119">Vlastnosti mezipaměti redis</span><span class="sxs-lookup"><span data-stu-id="c78b2-119">Redis Cache properties</span></span>
<span data-ttu-id="c78b2-120">Hello následující vlastnosti se používají při vytváření nebo aktualizaci instance služby Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="c78b2-120">hello following properties are used when creating and updating Redis Cache instances.</span></span>

| <span data-ttu-id="c78b2-121">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c78b2-121">Property</span></span> | <span data-ttu-id="c78b2-122">Přepínač</span><span class="sxs-lookup"><span data-stu-id="c78b2-122">Switch</span></span> | <span data-ttu-id="c78b2-123">Popis</span><span class="sxs-lookup"><span data-stu-id="c78b2-123">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c78b2-124">jméno</span><span class="sxs-lookup"><span data-stu-id="c78b2-124">name</span></span> |<span data-ttu-id="c78b2-125">-n, – název</span><span class="sxs-lookup"><span data-stu-id="c78b2-125">-n, --name</span></span> |<span data-ttu-id="c78b2-126">Název hello Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="c78b2-126">Name of hello Redis Cache.</span></span> |
| <span data-ttu-id="c78b2-127">skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="c78b2-127">resource group</span></span> |<span data-ttu-id="c78b2-128">-g, – skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="c78b2-128">-g, --resource-group</span></span> |<span data-ttu-id="c78b2-129">Název skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="c78b2-129">Name of hello Resource Group.</span></span> |
| <span data-ttu-id="c78b2-130">location</span><span class="sxs-lookup"><span data-stu-id="c78b2-130">location</span></span> |<span data-ttu-id="c78b2-131">-l, – umístění</span><span class="sxs-lookup"><span data-stu-id="c78b2-131">-l, --location</span></span> |<span data-ttu-id="c78b2-132">Mezipaměť toocreate umístění.</span><span class="sxs-lookup"><span data-stu-id="c78b2-132">Location toocreate cache.</span></span> |
| <span data-ttu-id="c78b2-133">Velikost</span><span class="sxs-lookup"><span data-stu-id="c78b2-133">size</span></span> |<span data-ttu-id="c78b2-134">-z, – velikost</span><span class="sxs-lookup"><span data-stu-id="c78b2-134">-z, --size</span></span> |<span data-ttu-id="c78b2-135">Velikost hello Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="c78b2-135">Size of hello Redis Cache.</span></span> <span data-ttu-id="c78b2-136">Platné hodnoty: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]</span><span class="sxs-lookup"><span data-stu-id="c78b2-136">Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]</span></span> |
| <span data-ttu-id="c78b2-137">SKU</span><span class="sxs-lookup"><span data-stu-id="c78b2-137">sku</span></span> |<span data-ttu-id="c78b2-138">-x, – sku</span><span class="sxs-lookup"><span data-stu-id="c78b2-138">-x, --sku</span></span> |<span data-ttu-id="c78b2-139">Redis SKU.</span><span class="sxs-lookup"><span data-stu-id="c78b2-139">Redis SKU.</span></span> <span data-ttu-id="c78b2-140">By měl být jeden z: [Basic, Standard, Premium]</span><span class="sxs-lookup"><span data-stu-id="c78b2-140">Should be one of : [Basic, Standard, Premium]</span></span> |
| <span data-ttu-id="c78b2-141">EnableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="c78b2-141">EnableNonSslPort</span></span> |<span data-ttu-id="c78b2-142">-e, – enable bez--port ssl</span><span class="sxs-lookup"><span data-stu-id="c78b2-142">-e, --enable-non-ssl-port</span></span> |<span data-ttu-id="c78b2-143">Vlastnost EnableNonSslPort hello Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="c78b2-143">EnableNonSslPort property of hello Redis Cache.</span></span> <span data-ttu-id="c78b2-144">Pokud chcete Port tooenable hello bez SSL pro mezipaměť přidat tento příznak</span><span class="sxs-lookup"><span data-stu-id="c78b2-144">Add this flag if you want tooenable hello Non SSL Port for your cache</span></span> |
| <span data-ttu-id="c78b2-145">Konfigurace redis</span><span class="sxs-lookup"><span data-stu-id="c78b2-145">Redis Configuration</span></span> |<span data-ttu-id="c78b2-146">-c, – konfigurací redis</span><span class="sxs-lookup"><span data-stu-id="c78b2-146">-c, --redis-configuration</span></span> |<span data-ttu-id="c78b2-147">Redis konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c78b2-147">Redis Configuration.</span></span> <span data-ttu-id="c78b2-148">Zadejte řetězec formátu JSON konfigurace klíčů a hodnot v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="c78b2-148">Enter a JSON formatted string of configuration keys and values here.</span></span> <span data-ttu-id="c78b2-149">Formát: "{" ":""," ":" "}"</span><span class="sxs-lookup"><span data-stu-id="c78b2-149">Format:"{"":"","":""}"</span></span> |
| <span data-ttu-id="c78b2-150">Konfigurace redis</span><span class="sxs-lookup"><span data-stu-id="c78b2-150">Redis Configuration</span></span> |<span data-ttu-id="c78b2-151">-f, – redis. konfigurační soubor</span><span class="sxs-lookup"><span data-stu-id="c78b2-151">-f, --redis-configuration-file</span></span> |<span data-ttu-id="c78b2-152">Redis konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c78b2-152">Redis Configuration.</span></span> <span data-ttu-id="c78b2-153">Zadejte hello cestu k souboru obsahující konfigurační klíče a hodnoty v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="c78b2-153">Enter hello path of a file containing configuration keys and values here.</span></span> <span data-ttu-id="c78b2-154">Formát pro vstupní soubor hello: {"": "","": ""}</span><span class="sxs-lookup"><span data-stu-id="c78b2-154">Format for hello file entry: {"":"","":""}</span></span> |
| <span data-ttu-id="c78b2-155">Počet horizontálních</span><span class="sxs-lookup"><span data-stu-id="c78b2-155">Shard Count</span></span> |<span data-ttu-id="c78b2-156">-r, – počet horizontálních</span><span class="sxs-lookup"><span data-stu-id="c78b2-156">-r, --shard-count</span></span> |<span data-ttu-id="c78b2-157">Počet toocreate horizontálních oddílů v clusteru Cache ve verzi Premium s clusteringem.</span><span class="sxs-lookup"><span data-stu-id="c78b2-157">Number of Shards toocreate on a Premium Cluster Cache with clustering.</span></span> |
| <span data-ttu-id="c78b2-158">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="c78b2-158">Virtual Network</span></span> |<span data-ttu-id="c78b2-159">-v, – virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="c78b2-159">-v, --virtual-network</span></span> |<span data-ttu-id="c78b2-160">Při hostování vaší mezipaměti ve virtuální síti, určuje hello přesný ARM ID prostředku hello virtuální sítě toodeploy hello redis v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="c78b2-160">When hosting your cache in a VNET, specifies hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in.</span></span> <span data-ttu-id="c78b2-161">Příklad formátu: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span><span class="sxs-lookup"><span data-stu-id="c78b2-161">Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span></span> |
| <span data-ttu-id="c78b2-162">Typ klíče</span><span class="sxs-lookup"><span data-stu-id="c78b2-162">key type</span></span> |<span data-ttu-id="c78b2-163">-t, – typ klíče</span><span class="sxs-lookup"><span data-stu-id="c78b2-163">-t, --key-type</span></span> |<span data-ttu-id="c78b2-164">Typ klíče toorenew.</span><span class="sxs-lookup"><span data-stu-id="c78b2-164">Type of key toorenew.</span></span> <span data-ttu-id="c78b2-165">Platné hodnoty: [primární, sekundární]</span><span class="sxs-lookup"><span data-stu-id="c78b2-165">Valid values: [Primary, Secondary]</span></span> |
| <span data-ttu-id="c78b2-166">StaticIP</span><span class="sxs-lookup"><span data-stu-id="c78b2-166">StaticIP</span></span> |<span data-ttu-id="c78b2-167">-p, – statické ip < statické ip ></span><span class="sxs-lookup"><span data-stu-id="c78b2-167">-p, --static-ip <static-ip></span></span> |<span data-ttu-id="c78b2-168">Při hostování vaší mezipaměti ve virtuální síti, určuje jedinečnou IP adresu v podsíti hello hello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="c78b2-168">When hosting your cache in a VNET, specifies a unique IP address in hello subnet for hello cache.</span></span> <span data-ttu-id="c78b2-169">Pokud není zadaná, jeden z podsítě hello vybrali za vás.</span><span class="sxs-lookup"><span data-stu-id="c78b2-169">If not provided, one is chosen for you from hello subnet.</span></span> |
| <span data-ttu-id="c78b2-170">Podsíť</span><span class="sxs-lookup"><span data-stu-id="c78b2-170">Subnet</span></span> |<span data-ttu-id="c78b2-171">t, – podsítě<subnet></span><span class="sxs-lookup"><span data-stu-id="c78b2-171">t, --subnet <subnet></span></span> |<span data-ttu-id="c78b2-172">Při hostování vaší mezipaměti ve virtuální síti, určuje název hello hello podsítě v mezipaměti které toodeploy hello.</span><span class="sxs-lookup"><span data-stu-id="c78b2-172">When hosting your cache in a VNET, specifies hello name of hello subnet in which toodeploy hello cache.</span></span> |
| <span data-ttu-id="c78b2-173">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="c78b2-173">VirtualNetwork</span></span> |<span data-ttu-id="c78b2-174">-v, – virtuální sítě < virtuálních sítí ></span><span class="sxs-lookup"><span data-stu-id="c78b2-174">-v, --virtual-network <virtual-network></span></span> |<span data-ttu-id="c78b2-175">Při hostování vaší mezipaměti ve virtuální síti, určuje hello přesný ARM ID prostředku hello virtuální sítě toodeploy hello redis v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="c78b2-175">When hosting your cache in a VNET, specifies hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in.</span></span> <span data-ttu-id="c78b2-176">Příklad formátu: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span><span class="sxs-lookup"><span data-stu-id="c78b2-176">Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span></span> |
| <span data-ttu-id="c78b2-177">Předplatné</span><span class="sxs-lookup"><span data-stu-id="c78b2-177">Subscription</span></span> |<span data-ttu-id="c78b2-178">-s, – předplatné</span><span class="sxs-lookup"><span data-stu-id="c78b2-178">-s, --subscription</span></span> |<span data-ttu-id="c78b2-179">identifikátor předplatného Hello.</span><span class="sxs-lookup"><span data-stu-id="c78b2-179">hello subscription identifier.</span></span> |

## <a name="see-all-redis-cache-commands"></a><span data-ttu-id="c78b2-180">Zobrazit všechny příkazy pro Redis Cache</span><span class="sxs-lookup"><span data-stu-id="c78b2-180">See all Redis Cache commands</span></span>
<span data-ttu-id="c78b2-181">toosee všechny příkazy Redis Cache a jejich parametrů, použijte hello `azure rediscache -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c78b2-181">toosee all Redis Cache commands and their parameters, use hello `azure rediscache -h` command.</span></span>

    C:\>azure rediscache -h
    help:    Commands toomanage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew hello authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a><span data-ttu-id="c78b2-182">Vytvoření Redis Cache</span><span class="sxs-lookup"><span data-stu-id="c78b2-182">Create a Redis Cache</span></span>
<span data-ttu-id="c78b2-183">toocreate Redis Cache, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c78b2-183">toocreate a Redis Cache, use hello following command:</span></span>

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

<span data-ttu-id="c78b2-184">Další informace o tomto příkazu Spustit hello `azure rediscache create -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c78b2-184">For more information about this command, run hello `azure rediscache create -h` command.</span></span>

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of hello Resource Group
    help:      -l, --location <location>                                Location toocreate cache.
    help:      -z, --size <size>                                        Size of hello Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of hello Redis Cache. Add this flag if you want tooenable hello Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter hello path of a file containing configuration keys and values here. Format for hello file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards toocreate on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a><span data-ttu-id="c78b2-185">Odstraňte existující Redis Cache</span><span class="sxs-lookup"><span data-stu-id="c78b2-185">Delete an existing Redis Cache</span></span>
<span data-ttu-id="c78b2-186">toodelete Redis Cache, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c78b2-186">toodelete a Redis Cache, use hello following command:</span></span>

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

<span data-ttu-id="c78b2-187">Další informace o tomto příkazu Spustit hello `azure rediscache delete -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c78b2-187">For more information about this command, run hello `azure rediscache delete -h` command.</span></span>

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group under which hello cache exists
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a><span data-ttu-id="c78b2-188">Zobrazí seznam všech mezipaměti Redis v rámci předplatného nebo skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="c78b2-188">List all Redis Caches within your Subscription or Resource Group</span></span>
<span data-ttu-id="c78b2-189">použít všechny mezipaměti Redis v rámci předplatného nebo skupinu prostředků, toolist hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c78b2-189">toolist all Redis Caches within your Subscription or Resource Group, use hello following command:</span></span>

    azure rediscache list [options]

<span data-ttu-id="c78b2-190">Další informace o tomto příkazu Spustit hello `azure rediscache list -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c78b2-190">For more information about this command, run hello `azure rediscache list -h` command.</span></span>

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a><span data-ttu-id="c78b2-191">Zobrazit vlastnosti existující Redis Cache</span><span class="sxs-lookup"><span data-stu-id="c78b2-191">Show properties of an existing Redis Cache</span></span>
<span data-ttu-id="c78b2-192">tooshow vlastnosti existující Redis Cache, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c78b2-192">tooshow properties of an existing Redis Cache, use hello following command:</span></span>

    azure rediscache show [--name <name> --resource-group <resource-group>]

<span data-ttu-id="c78b2-193">Další informace o tomto příkazu Spustit hello `azure rediscache show -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c78b2-193">For more information about this command, run hello `azure rediscache show -h` command.</span></span>

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>

## <a name="change-settings-of-an-existing-redis-cache"></a><span data-ttu-id="c78b2-194">Změňte nastavení existující Redis Cache</span><span class="sxs-lookup"><span data-stu-id="c78b2-194">Change settings of an existing Redis Cache</span></span>
<span data-ttu-id="c78b2-195">nastavení toochange existující Redis Cache, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c78b2-195">toochange settings of an existing Redis Cache, use hello following command:</span></span>

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

<span data-ttu-id="c78b2-196">Další informace o tomto příkazu Spustit hello `azure rediscache set -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c78b2-196">For more information about this command, run hello `azure rediscache set -h` command.</span></span>

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of hello Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter hello path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-hello-authentication-key-for-an-existing-redis-cache"></a><span data-ttu-id="c78b2-197">Obnovit hello ověřovací klíč pro existující Redis Cache</span><span class="sxs-lookup"><span data-stu-id="c78b2-197">Renew hello authentication key for an existing Redis Cache</span></span>
<span data-ttu-id="c78b2-198">toorenew hello ověřovací klíč pro existující Redis Cache, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c78b2-198">toorenew hello authentication key for an existing Redis Cache, use hello following command:</span></span>

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

<span data-ttu-id="c78b2-199">Zadejte `Primary` nebo `Secondary` pro `key-type`.</span><span class="sxs-lookup"><span data-stu-id="c78b2-199">Specify `Primary` or `Secondary` for `key-type`.</span></span>

<span data-ttu-id="c78b2-200">Další informace o tomto příkazu Spustit hello `azure rediscache renew-key -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c78b2-200">For more information about this command, run hello `azure rediscache renew-key -h` command.</span></span>

    C:\>azure rediscache renew-key -h
    help:    Renew hello authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key toorenew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a><span data-ttu-id="c78b2-201">Seznam primární a sekundární klíče existující mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="c78b2-201">List Primary and Secondary keys of an existing Redis Cache</span></span>
<span data-ttu-id="c78b2-202">toolist primární a sekundární klíče existující Redis Cache, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c78b2-202">toolist Primary and Secondary keys of an existing Redis Cache, use hello following command:</span></span>

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

<span data-ttu-id="c78b2-203">Další informace o tomto příkazu Spustit hello `azure rediscache list-keys -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c78b2-203">For more information about this command, run hello `azure rediscache list-keys -h` command.</span></span>

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
