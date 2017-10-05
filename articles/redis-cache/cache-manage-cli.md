---
title: "Správa Azure Redis Cache pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak nainstalovat rozhraní příkazového řádku Azure na jakékoli platformě, jak používat pro připojení k účtu Azure a jak vytvořit a spravovat mezipaměti Redis z příkazového řádku Azure."
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
ms.openlocfilehash: ba078a870a3998568170cc197bd6698b97b7fadb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a><span data-ttu-id="8315d-103">Jak vytvořit a spravovat Azure Redis Cache pomocí rozhraní příkazového řádku Azure (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="8315d-103">How to create and manage Azure Redis Cache using the Azure Command-Line Interface (Azure CLI)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8315d-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8315d-104">PowerShell</span></span>](cache-howto-manage-redis-cache-powershell.md)
> * [<span data-ttu-id="8315d-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8315d-105">Azure CLI</span></span>](cache-manage-cli.md)
>
>

<span data-ttu-id="8315d-106">Rozhraní příkazového řádku Azure je skvělým způsobem, jak spravovat infrastrukturu Azure z libovolné platformě.</span><span class="sxs-lookup"><span data-stu-id="8315d-106">The Azure CLI is a great way to manage your Azure infrastructure from any platform.</span></span> <span data-ttu-id="8315d-107">Tento článek ukazuje, jak vytvořit a spravovat vaše instance služby Azure Redis Cache pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="8315d-107">This article shows you how to create and manage your Azure Redis Cache instances using the Azure CLI.</span></span>

> [!NOTE]
> <span data-ttu-id="8315d-108">Tento článek se týká předchozí verze rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="8315d-108">This article applies to a previous version of Azure CLI.</span></span> <span data-ttu-id="8315d-109">Nejnovější ukázkové skripty Azure CLI 2.0, naleznete v části [ukázky rozhraní příkazového řádku služby Azure Redis cache](cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8315d-109">For the latest Azure CLI 2.0 sample scripts, see [Azure CLI Redis cache samples](cli-samples.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="8315d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8315d-110">Prerequisites</span></span>
<span data-ttu-id="8315d-111">Chcete-li vytvořit a spravovat instance služby Azure Redis Cache pomocí rozhraní příkazového řádku Azure, musíte provést následující kroky.</span><span class="sxs-lookup"><span data-stu-id="8315d-111">To create and manage Azure Redis Cache instances using Azure CLI, you must complete the following steps.</span></span>

* <span data-ttu-id="8315d-112">Musí mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="8315d-112">You must have an Azure account.</span></span> <span data-ttu-id="8315d-113">Pokud nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) za pár chvil.</span><span class="sxs-lookup"><span data-stu-id="8315d-113">If you don't have one, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a few moments.</span></span>
* <span data-ttu-id="8315d-114">[Instalace rozhraní příkazového řádku Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="8315d-114">[Install the Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="8315d-115">Připojit vaše instalace rozhraní příkazového řádku Azure osobní účet Azure, nebo s pracovní nebo školní účet Azure a přihlaste se pomocí rozhraní příkazového řádku Azure `azure login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8315d-115">Connect your Azure CLI installation with a personal Azure account, or with a work or school Azure account, and log in from the Azure CLI using the `azure login` command.</span></span> <span data-ttu-id="8315d-116">Porozumět rozdílům a zvolte najdete v tématu [připojení k předplatnému Azure z rozhraní příkazového řádku (Azure CLI)](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="8315d-116">To understand the differences and choose, see [Connect to an Azure subscription from the Azure Command-Line Interface (Azure CLI)](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="8315d-117">Před s některým z následujících příkazů, přepínač příkazového řádku Azure CLI do režimu Resource Manager tak, že spustíte `azure config mode arm` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8315d-117">Before running any of the following commands, switch the Azure CLI into Resource Manager mode by running the `azure config mode arm` command.</span></span> <span data-ttu-id="8315d-118">Další informace najdete v tématu [používat rozhraní příkazového řádku Azure ke správě prostředků Azure a skupiny prostředků](../xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="8315d-118">For more information, see [Use the Azure CLI to manage Azure resources and resource groups](../xplat-cli-azure-resource-manager.md).</span></span>

## <a name="redis-cache-properties"></a><span data-ttu-id="8315d-119">Vlastnosti mezipaměti redis</span><span class="sxs-lookup"><span data-stu-id="8315d-119">Redis Cache properties</span></span>
<span data-ttu-id="8315d-120">Následující vlastnosti se používají při vytváření nebo aktualizaci instance služby Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="8315d-120">The following properties are used when creating and updating Redis Cache instances.</span></span>

| <span data-ttu-id="8315d-121">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="8315d-121">Property</span></span> | <span data-ttu-id="8315d-122">Přepínač</span><span class="sxs-lookup"><span data-stu-id="8315d-122">Switch</span></span> | <span data-ttu-id="8315d-123">Popis</span><span class="sxs-lookup"><span data-stu-id="8315d-123">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8315d-124">jméno</span><span class="sxs-lookup"><span data-stu-id="8315d-124">name</span></span> |<span data-ttu-id="8315d-125">-n, – název</span><span class="sxs-lookup"><span data-stu-id="8315d-125">-n, --name</span></span> |<span data-ttu-id="8315d-126">Název mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="8315d-126">Name of the Redis Cache.</span></span> |
| <span data-ttu-id="8315d-127">skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="8315d-127">resource group</span></span> |<span data-ttu-id="8315d-128">-g, – skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="8315d-128">-g, --resource-group</span></span> |<span data-ttu-id="8315d-129">Název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="8315d-129">Name of the Resource Group.</span></span> |
| <span data-ttu-id="8315d-130">location</span><span class="sxs-lookup"><span data-stu-id="8315d-130">location</span></span> |<span data-ttu-id="8315d-131">-l, – umístění</span><span class="sxs-lookup"><span data-stu-id="8315d-131">-l, --location</span></span> |<span data-ttu-id="8315d-132">Umístění pro vytvoření mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="8315d-132">Location to create cache.</span></span> |
| <span data-ttu-id="8315d-133">Velikost</span><span class="sxs-lookup"><span data-stu-id="8315d-133">size</span></span> |<span data-ttu-id="8315d-134">-z, – velikost</span><span class="sxs-lookup"><span data-stu-id="8315d-134">-z, --size</span></span> |<span data-ttu-id="8315d-135">Velikost mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="8315d-135">Size of the Redis Cache.</span></span> <span data-ttu-id="8315d-136">Platné hodnoty: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]</span><span class="sxs-lookup"><span data-stu-id="8315d-136">Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]</span></span> |
| <span data-ttu-id="8315d-137">SKU</span><span class="sxs-lookup"><span data-stu-id="8315d-137">sku</span></span> |<span data-ttu-id="8315d-138">-x, – sku</span><span class="sxs-lookup"><span data-stu-id="8315d-138">-x, --sku</span></span> |<span data-ttu-id="8315d-139">Redis SKU.</span><span class="sxs-lookup"><span data-stu-id="8315d-139">Redis SKU.</span></span> <span data-ttu-id="8315d-140">By měl být jeden z: [Basic, Standard, Premium]</span><span class="sxs-lookup"><span data-stu-id="8315d-140">Should be one of : [Basic, Standard, Premium]</span></span> |
| <span data-ttu-id="8315d-141">EnableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="8315d-141">EnableNonSslPort</span></span> |<span data-ttu-id="8315d-142">-e, – enable bez--port ssl</span><span class="sxs-lookup"><span data-stu-id="8315d-142">-e, --enable-non-ssl-port</span></span> |<span data-ttu-id="8315d-143">Vlastnost EnableNonSslPort Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="8315d-143">EnableNonSslPort property of the Redis Cache.</span></span> <span data-ttu-id="8315d-144">Přidejte tento příznak, pokud chcete povolit Port bez SSL pro mezipaměť</span><span class="sxs-lookup"><span data-stu-id="8315d-144">Add this flag if you want to enable the Non SSL Port for your cache</span></span> |
| <span data-ttu-id="8315d-145">Konfigurace redis</span><span class="sxs-lookup"><span data-stu-id="8315d-145">Redis Configuration</span></span> |<span data-ttu-id="8315d-146">-c, – konfigurací redis</span><span class="sxs-lookup"><span data-stu-id="8315d-146">-c, --redis-configuration</span></span> |<span data-ttu-id="8315d-147">Redis konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8315d-147">Redis Configuration.</span></span> <span data-ttu-id="8315d-148">Zadejte řetězec formátu JSON konfigurace klíčů a hodnot v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="8315d-148">Enter a JSON formatted string of configuration keys and values here.</span></span> <span data-ttu-id="8315d-149">Formát: "{" ":""," ":" "}"</span><span class="sxs-lookup"><span data-stu-id="8315d-149">Format:"{"":"","":""}"</span></span> |
| <span data-ttu-id="8315d-150">Konfigurace redis</span><span class="sxs-lookup"><span data-stu-id="8315d-150">Redis Configuration</span></span> |<span data-ttu-id="8315d-151">-f, – redis. konfigurační soubor</span><span class="sxs-lookup"><span data-stu-id="8315d-151">-f, --redis-configuration-file</span></span> |<span data-ttu-id="8315d-152">Redis konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8315d-152">Redis Configuration.</span></span> <span data-ttu-id="8315d-153">Zadejte cestu k souboru obsahující konfigurační klíče a hodnoty v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="8315d-153">Enter the path of a file containing configuration keys and values here.</span></span> <span data-ttu-id="8315d-154">Formát vstupního souboru: {"": "","": ""}</span><span class="sxs-lookup"><span data-stu-id="8315d-154">Format for the file entry: {"":"","":""}</span></span> |
| <span data-ttu-id="8315d-155">Počet horizontálních</span><span class="sxs-lookup"><span data-stu-id="8315d-155">Shard Count</span></span> |<span data-ttu-id="8315d-156">-r, – počet horizontálních</span><span class="sxs-lookup"><span data-stu-id="8315d-156">-r, --shard-count</span></span> |<span data-ttu-id="8315d-157">Počet horizontálních oddílů vytvořit na clusteru Cache ve verzi Premium s clusteringem.</span><span class="sxs-lookup"><span data-stu-id="8315d-157">Number of Shards to create on a Premium Cluster Cache with clustering.</span></span> |
| <span data-ttu-id="8315d-158">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="8315d-158">Virtual Network</span></span> |<span data-ttu-id="8315d-159">-v, – virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="8315d-159">-v, --virtual-network</span></span> |<span data-ttu-id="8315d-160">Při hostování vaší mezipaměti ve virtuální síti, určuje přesné ARM ID prostředku virtuální sítě pro nasazení mezipaměti redis v.</span><span class="sxs-lookup"><span data-stu-id="8315d-160">When hosting your cache in a VNET, specifies the exact ARM resource ID of the virtual network to deploy the redis cache in.</span></span> <span data-ttu-id="8315d-161">Příklad formátu: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span><span class="sxs-lookup"><span data-stu-id="8315d-161">Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span></span> |
| <span data-ttu-id="8315d-162">Typ klíče</span><span class="sxs-lookup"><span data-stu-id="8315d-162">key type</span></span> |<span data-ttu-id="8315d-163">-t, – typ klíče</span><span class="sxs-lookup"><span data-stu-id="8315d-163">-t, --key-type</span></span> |<span data-ttu-id="8315d-164">Typ klíče pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="8315d-164">Type of key to renew.</span></span> <span data-ttu-id="8315d-165">Platné hodnoty: [primární, sekundární]</span><span class="sxs-lookup"><span data-stu-id="8315d-165">Valid values: [Primary, Secondary]</span></span> |
| <span data-ttu-id="8315d-166">StaticIP</span><span class="sxs-lookup"><span data-stu-id="8315d-166">StaticIP</span></span> |<span data-ttu-id="8315d-167">-p, – statické ip < statické ip ></span><span class="sxs-lookup"><span data-stu-id="8315d-167">-p, --static-ip <static-ip></span></span> |<span data-ttu-id="8315d-168">Při hostování vaší mezipaměti ve virtuální síti, určuje jedinečnou IP adresu v mezipaměti v podsíti.</span><span class="sxs-lookup"><span data-stu-id="8315d-168">When hosting your cache in a VNET, specifies a unique IP address in the subnet for the cache.</span></span> <span data-ttu-id="8315d-169">Pokud není zadaná, jeden z podsítě vybrali za vás.</span><span class="sxs-lookup"><span data-stu-id="8315d-169">If not provided, one is chosen for you from the subnet.</span></span> |
| <span data-ttu-id="8315d-170">Podsíť</span><span class="sxs-lookup"><span data-stu-id="8315d-170">Subnet</span></span> |<span data-ttu-id="8315d-171">t, – podsítě<subnet></span><span class="sxs-lookup"><span data-stu-id="8315d-171">t, --subnet <subnet></span></span> |<span data-ttu-id="8315d-172">Při hostování vaší mezipaměti ve virtuální síti, určuje název podsítě, ve které chcete nasadit do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="8315d-172">When hosting your cache in a VNET, specifies the name of the subnet in which to deploy the cache.</span></span> |
| <span data-ttu-id="8315d-173">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="8315d-173">VirtualNetwork</span></span> |<span data-ttu-id="8315d-174">-v, – virtuální sítě < virtuálních sítí ></span><span class="sxs-lookup"><span data-stu-id="8315d-174">-v, --virtual-network <virtual-network></span></span> |<span data-ttu-id="8315d-175">Při hostování vaší mezipaměti ve virtuální síti, určuje přesné ARM ID prostředku virtuální sítě pro nasazení mezipaměti redis v.</span><span class="sxs-lookup"><span data-stu-id="8315d-175">When hosting your cache in a VNET, specifies the exact ARM resource ID of the virtual network to deploy the redis cache in.</span></span> <span data-ttu-id="8315d-176">Příklad formátu: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span><span class="sxs-lookup"><span data-stu-id="8315d-176">Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span></span> |
| <span data-ttu-id="8315d-177">Předplatné</span><span class="sxs-lookup"><span data-stu-id="8315d-177">Subscription</span></span> |<span data-ttu-id="8315d-178">-s, – předplatné</span><span class="sxs-lookup"><span data-stu-id="8315d-178">-s, --subscription</span></span> |<span data-ttu-id="8315d-179">Identifikátor předplatného.</span><span class="sxs-lookup"><span data-stu-id="8315d-179">The subscription identifier.</span></span> |

## <a name="see-all-redis-cache-commands"></a><span data-ttu-id="8315d-180">Zobrazit všechny příkazy pro Redis Cache</span><span class="sxs-lookup"><span data-stu-id="8315d-180">See all Redis Cache commands</span></span>
<span data-ttu-id="8315d-181">Pokud chcete zobrazit všechny příkazy Redis Cache a jejich parametrů, použijte `azure rediscache -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8315d-181">To see all Redis Cache commands and their parameters, use the `azure rediscache -h` command.</span></span>

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
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
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a><span data-ttu-id="8315d-182">Vytvoření Redis Cache</span><span class="sxs-lookup"><span data-stu-id="8315d-182">Create a Redis Cache</span></span>
<span data-ttu-id="8315d-183">Chcete-li vytvořit Redis Cache, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8315d-183">To create a Redis Cache, use the following command:</span></span>

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

<span data-ttu-id="8315d-184">Další informace o tomto příkazu, spusťte `azure rediscache create -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8315d-184">For more information about this command, run the `azure rediscache create -h` command.</span></span>

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
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a><span data-ttu-id="8315d-185">Odstraňte existující Redis Cache</span><span class="sxs-lookup"><span data-stu-id="8315d-185">Delete an existing Redis Cache</span></span>
<span data-ttu-id="8315d-186">Pokud chcete odstranit Redis Cache, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8315d-186">To delete a Redis Cache, use the following command:</span></span>

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

<span data-ttu-id="8315d-187">Další informace o tomto příkazu, spusťte `azure rediscache delete -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8315d-187">For more information about this command, run the `azure rediscache delete -h` command.</span></span>

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
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a><span data-ttu-id="8315d-188">Zobrazí seznam všech mezipaměti Redis v rámci předplatného nebo skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="8315d-188">List all Redis Caches within your Subscription or Resource Group</span></span>
<span data-ttu-id="8315d-189">K zobrazení seznamu všech mezipaměti Redis v rámci předplatného nebo skupinu prostředků, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8315d-189">To list all Redis Caches within your Subscription or Resource Group, use the following command:</span></span>

    azure rediscache list [options]

<span data-ttu-id="8315d-190">Další informace o tomto příkazu, spusťte `azure rediscache list -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8315d-190">For more information about this command, run the `azure rediscache list -h` command.</span></span>

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
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a><span data-ttu-id="8315d-191">Zobrazit vlastnosti existující Redis Cache</span><span class="sxs-lookup"><span data-stu-id="8315d-191">Show properties of an existing Redis Cache</span></span>
<span data-ttu-id="8315d-192">Chcete-li zobrazit vlastnosti existující Redis Cache, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8315d-192">To show properties of an existing Redis Cache, use the following command:</span></span>

    azure rediscache show [--name <name> --resource-group <resource-group>]

<span data-ttu-id="8315d-193">Další informace o tomto příkazu, spusťte `azure rediscache show -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8315d-193">For more information about this command, run the `azure rediscache show -h` command.</span></span>

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
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>

## <a name="change-settings-of-an-existing-redis-cache"></a><span data-ttu-id="8315d-194">Změňte nastavení existující Redis Cache</span><span class="sxs-lookup"><span data-stu-id="8315d-194">Change settings of an existing Redis Cache</span></span>
<span data-ttu-id="8315d-195">Chcete-li změnit nastavení existujícího Redis Cache, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8315d-195">To change settings of an existing Redis Cache, use the following command:</span></span>

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

<span data-ttu-id="8315d-196">Další informace o tomto příkazu, spusťte `azure rediscache set -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8315d-196">For more information about this command, run the `azure rediscache set -h` command.</span></span>

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
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a><span data-ttu-id="8315d-197">Obnovit ověřovací klíč pro existující Redis Cache</span><span class="sxs-lookup"><span data-stu-id="8315d-197">Renew the authentication key for an existing Redis Cache</span></span>
<span data-ttu-id="8315d-198">Chcete-li obnovit ověřovací klíč pro existující Redis Cache, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8315d-198">To renew the authentication key for an existing Redis Cache, use the following command:</span></span>

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

<span data-ttu-id="8315d-199">Zadejte `Primary` nebo `Secondary` pro `key-type`.</span><span class="sxs-lookup"><span data-stu-id="8315d-199">Specify `Primary` or `Secondary` for `key-type`.</span></span>

<span data-ttu-id="8315d-200">Další informace o tomto příkazu, spusťte `azure rediscache renew-key -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8315d-200">For more information about this command, run the `azure rediscache renew-key -h` command.</span></span>

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a><span data-ttu-id="8315d-201">Seznam primární a sekundární klíče existující mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="8315d-201">List Primary and Secondary keys of an existing Redis Cache</span></span>
<span data-ttu-id="8315d-202">Do seznamu primární a sekundární klíče existující Redis Cache použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8315d-202">To list Primary and Secondary keys of an existing Redis Cache, use the following command:</span></span>

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

<span data-ttu-id="8315d-203">Další informace o tomto příkazu, spusťte `azure rediscache list-keys -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8315d-203">For more information about this command, run the `azure rediscache list-keys -h` command.</span></span>

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
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
