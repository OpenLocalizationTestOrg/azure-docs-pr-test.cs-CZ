---
title: "Spravovat mezipaměť Redis systému Azure pomocí Azure Powershellu | Microsoft Docs"
description: "Zjistěte, jak k provádění úloh správy pro Azure Redis Cache pomocí Azure PowerShell."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 1136efe5-1e33-4d91-bb49-c8e2a6dca475
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: sdanie
ms.openlocfilehash: 0a5c95eab3fd01f611fc049e80c5c506857e0b81
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="manage-azure-redis-cache-with-azure-powershell"></a><span data-ttu-id="3973f-103">Spravovat mezipaměť Redis systému Azure pomocí Azure Powershellu</span><span class="sxs-lookup"><span data-stu-id="3973f-103">Manage Azure Redis Cache with Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3973f-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3973f-104">PowerShell</span></span>](cache-howto-manage-redis-cache-powershell.md)
> * [<span data-ttu-id="3973f-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3973f-105">Azure CLI</span></span>](cache-manage-cli.md)
> 
> 

<span data-ttu-id="3973f-106">Toto téma ukazuje, jak provádět běžné úkoly, jako vytvořit, aktualizujete a škálovat vaše instance služby Azure Redis Cache, postup opětovné vygenerování přístupových klíčů a k zobrazení informací o své mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-106">This topic shows you how to perform common tasks such as create, update, and scale your Azure Redis Cache instances, how to regenerate access keys, and how to view information about your caches.</span></span> <span data-ttu-id="3973f-107">Úplný seznam rutin Powershellu pro Azure Redis Cache najdete v tématu [rutiny Azure Redis Cache](https://msdn.microsoft.com/library/azure/mt634513.aspx).</span><span class="sxs-lookup"><span data-stu-id="3973f-107">For a complete list of Azure Redis Cache PowerShell cmdlets, see [Azure Redis Cache cmdlets](https://msdn.microsoft.com/library/azure/mt634513.aspx).</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="3973f-108">Další informace o modelu nasazení classic najdete v tématu [Azure Resource Manager oproti nasazení classic: pochopení modely nasazení a stav svých prostředků](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).</span><span class="sxs-lookup"><span data-stu-id="3973f-108">For more information about the classic deployment model, see [Azure Resource Manager vs. classic deployment: Understand deployment models and the state of your resources](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3973f-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3973f-109">Prerequisites</span></span>
<span data-ttu-id="3973f-110">Pokud jste již Azure PowerShell nainstalovali, musíte mít prostředí Azure PowerShell verze 1.0.0 nebo později.</span><span class="sxs-lookup"><span data-stu-id="3973f-110">If you have already installed Azure PowerShell, you must have Azure PowerShell version 1.0.0 or later.</span></span> <span data-ttu-id="3973f-111">Můžete zkontrolovat verzi prostředí Azure PowerShell, který jste nainstalovali pomocí tohoto příkazu na příkazovém řádku prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3973f-111">You can check the version of Azure PowerShell that you have installed with this command at the Azure PowerShell command prompt.</span></span>

    Get-Module azure | format-table version


<span data-ttu-id="3973f-112">Nejdřív musíte být přihlášení do Azure pomocí tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="3973f-112">First, you must log in to Azure with this command.</span></span>

    Login-AzureRmAccount

<span data-ttu-id="3973f-113">Zadejte e-mailovou adresu účtu Azure a jeho heslo v dialogovém okně sign-in Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="3973f-113">Specify the email address of your Azure account and its password in the Microsoft Azure sign-in dialog.</span></span>

<span data-ttu-id="3973f-114">Dále pokud máte víc předplatných Azure, budete muset nastavit vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="3973f-114">Next, if you have multiple Azure subscriptions, you need to set your Azure subscription.</span></span> <span data-ttu-id="3973f-115">Chcete-li zobrazit seznam aktuální předplatných, spusťte tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="3973f-115">To see a list of your current subscriptions, run this command.</span></span>

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

<span data-ttu-id="3973f-116">Pro určení předplatného, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="3973f-116">To specify the subscription, run the following command.</span></span> <span data-ttu-id="3973f-117">V následujícím příkladu je název odběru `ContosoSubscription`.</span><span class="sxs-lookup"><span data-stu-id="3973f-117">In the following example, the subscription name is `ContosoSubscription`.</span></span>

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

<span data-ttu-id="3973f-118">Než budete moct použít prostředí Windows PowerShell s Azure Resource Managerem, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="3973f-118">Before you can use Windows PowerShell with Azure Resource Manager, you need the following:</span></span>

* <span data-ttu-id="3973f-119">Prostředí Windows PowerShell, verze 3.0 nebo 4.0.</span><span class="sxs-lookup"><span data-stu-id="3973f-119">Windows PowerShell, Version 3.0 or 4.0.</span></span> <span data-ttu-id="3973f-120">Chcete-li najít verzi prostředí Windows PowerShell, zadejte:`$PSVersionTable` a ověřte hodnotu `PSVersion` je 3.0 nebo 4.0.</span><span class="sxs-lookup"><span data-stu-id="3973f-120">To find the version of Windows PowerShell, type:`$PSVersionTable` and verify the value of `PSVersion` is 3.0 or 4.0.</span></span> <span data-ttu-id="3973f-121">K instalaci kompatibilní verze, najdete v části [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) nebo [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).</span><span class="sxs-lookup"><span data-stu-id="3973f-121">To install a compatible version, see [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) or [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).</span></span>

<span data-ttu-id="3973f-122">Podrobnou nápovědu pro všechny rutiny, které se zobrazí v tomto kurzu získáte pomocí rutiny Get-Help.</span><span class="sxs-lookup"><span data-stu-id="3973f-122">To get detailed help for any cmdlet you see in this tutorial, use the Get-Help cmdlet.</span></span>

    Get-Help <cmdlet-name> -Detailed

<span data-ttu-id="3973f-123">Například pro získání nápovědy pro `New-AzureRmRedisCache` rutiny, zadejte:</span><span class="sxs-lookup"><span data-stu-id="3973f-123">For example, to get help for the `New-AzureRmRedisCache` cmdlet, type:</span></span>

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-other-clouds"></a><span data-ttu-id="3973f-124">Jak se připojit k ostatních cloudů</span><span class="sxs-lookup"><span data-stu-id="3973f-124">How to connect to other clouds</span></span>
<span data-ttu-id="3973f-125">Ve výchozím nastavení Azure je prostředí `AzureCloud`, který představuje instanci globální cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="3973f-125">By default the Azure environment is `AzureCloud`, which represents the global Azure cloud instance.</span></span> <span data-ttu-id="3973f-126">Chcete-li se připojit k jiné instanci, použijte `Add-AzureRmAccount` s `-Environment` nebo -`EnvironmentName` přepínač příkazového řádku s názvem prostředí nebo požadované prostředí.</span><span class="sxs-lookup"><span data-stu-id="3973f-126">To connect to a different instance, use the `Add-AzureRmAccount` command with the `-Environment` or -`EnvironmentName` command line switch with the desired environment or environment name.</span></span>

<span data-ttu-id="3973f-127">Chcete-li zobrazit seznam dostupných prostředí, spusťte `Get-AzureRmEnvironment` rutiny.</span><span class="sxs-lookup"><span data-stu-id="3973f-127">To see the list of available environments, run the `Get-AzureRmEnvironment` cmdlet.</span></span>

### <a name="to-connect-to-the-azure-government-cloud"></a><span data-ttu-id="3973f-128">Pro připojení k Azure Government cloudu</span><span class="sxs-lookup"><span data-stu-id="3973f-128">To connect to the Azure Government Cloud</span></span>
<span data-ttu-id="3973f-129">Pokud chcete připojit ke cloudu Azure Government, použijte jednu z následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="3973f-129">To connect to the Azure Government Cloud, use one of the following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

<span data-ttu-id="3973f-130">nebo</span><span class="sxs-lookup"><span data-stu-id="3973f-130">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

<span data-ttu-id="3973f-131">K vytvoření mezipaměti se v cloudu Azure Government, použijte jednu z následujících umístění.</span><span class="sxs-lookup"><span data-stu-id="3973f-131">To create a cache in the Azure Government Cloud, use one of the following locations.</span></span>

* <span data-ttu-id="3973f-132">Vláda USA Virginia</span><span class="sxs-lookup"><span data-stu-id="3973f-132">USGov Virginia</span></span>
* <span data-ttu-id="3973f-133">Iowa vláda USA</span><span class="sxs-lookup"><span data-stu-id="3973f-133">USGov Iowa</span></span>

<span data-ttu-id="3973f-134">Další informace o Cloud vlády Azure najdete v tématu [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) a [Průvodce pro vývojáře k Microsoft Azure Government](../azure-government-developer-guide.md).</span><span class="sxs-lookup"><span data-stu-id="3973f-134">For more information about the Azure Government Cloud, see [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) and [Microsoft Azure Government Developer Guide](../azure-government-developer-guide.md).</span></span>

### <a name="to-connect-to-the-azure-china-cloud"></a><span data-ttu-id="3973f-135">Pro připojení k Azure Cloud Čína</span><span class="sxs-lookup"><span data-stu-id="3973f-135">To connect to the Azure China Cloud</span></span>
<span data-ttu-id="3973f-136">Pro připojení k Azure Cloud Čína, použijte jednu z následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="3973f-136">To connect to the Azure China Cloud, use one of the following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

<span data-ttu-id="3973f-137">nebo</span><span class="sxs-lookup"><span data-stu-id="3973f-137">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

<span data-ttu-id="3973f-138">K vytvoření mezipaměti se v Číně cloudu Azure, použijte jednu z následujících umístění.</span><span class="sxs-lookup"><span data-stu-id="3973f-138">To create a cache in the Azure China Cloud, use one of the following locations.</span></span>

* <span data-ttu-id="3973f-139">Čína – východ</span><span class="sxs-lookup"><span data-stu-id="3973f-139">China East</span></span>
* <span data-ttu-id="3973f-140">Čína – sever</span><span class="sxs-lookup"><span data-stu-id="3973f-140">China North</span></span>

<span data-ttu-id="3973f-141">Další informace o cloudu Číně Azure najdete v tématu [AzureChinaCloud pro Azure provozované v Číně společností 21Vianet](http://www.windowsazure.cn/).</span><span class="sxs-lookup"><span data-stu-id="3973f-141">For more information about the Azure China Cloud, see [AzureChinaCloud for Azure operated by 21Vianet in China](http://www.windowsazure.cn/).</span></span>

### <a name="to-connect-to-microsoft-azure-germany"></a><span data-ttu-id="3973f-142">Pro připojení k Microsoft Azure v Německu</span><span class="sxs-lookup"><span data-stu-id="3973f-142">To connect to Microsoft Azure Germany</span></span>
<span data-ttu-id="3973f-143">Pro připojení k Microsoft Azure v Německu, použijte jednu z následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="3973f-143">To connect to Microsoft Azure Germany, use one of the following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureGermanCloud


<span data-ttu-id="3973f-144">nebo</span><span class="sxs-lookup"><span data-stu-id="3973f-144">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

<span data-ttu-id="3973f-145">K vytvoření mezipaměti se v Microsoft Azure v Německu, použijte jednu z následujících umístění.</span><span class="sxs-lookup"><span data-stu-id="3973f-145">To create a cache in Microsoft Azure Germany, use one of the following locations.</span></span>

* <span data-ttu-id="3973f-146">Německo – střed</span><span class="sxs-lookup"><span data-stu-id="3973f-146">Germany Central</span></span>
* <span data-ttu-id="3973f-147">Německo – severovýchod</span><span class="sxs-lookup"><span data-stu-id="3973f-147">Germany Northeast</span></span>

<span data-ttu-id="3973f-148">Další informace o Microsoft Azure v Německu najdete v tématu [Microsoft Azure v Německu](https://azure.microsoft.com/overview/clouds/germany/).</span><span class="sxs-lookup"><span data-stu-id="3973f-148">For more information about Microsoft Azure Germany, see [Microsoft Azure Germany](https://azure.microsoft.com/overview/clouds/germany/).</span></span>

### <a name="properties-used-for-azure-redis-cache-powershell"></a><span data-ttu-id="3973f-149">Vlastnosti používané pro Azure Redis Cache prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3973f-149">Properties used for Azure Redis Cache PowerShell</span></span>
<span data-ttu-id="3973f-150">Následující tabulka obsahuje vlastnosti a popisy pro běžně používané parametry při vytváření a správě vaší instance služby Azure Redis Cache pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3973f-150">The following table contains properties and descriptions for commonly used parameters when creating and managing your Azure Redis Cache instances using Azure PowerShell.</span></span>

| <span data-ttu-id="3973f-151">Parametr</span><span class="sxs-lookup"><span data-stu-id="3973f-151">Parameter</span></span> | <span data-ttu-id="3973f-152">Popis</span><span class="sxs-lookup"><span data-stu-id="3973f-152">Description</span></span> | <span data-ttu-id="3973f-153">Výchozí</span><span class="sxs-lookup"><span data-stu-id="3973f-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3973f-154">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="3973f-154">Name</span></span> |<span data-ttu-id="3973f-155">Název mezipaměti</span><span class="sxs-lookup"><span data-stu-id="3973f-155">Name of the cache</span></span> | |
| <span data-ttu-id="3973f-156">Umístění</span><span class="sxs-lookup"><span data-stu-id="3973f-156">Location</span></span> |<span data-ttu-id="3973f-157">Umístění mezipaměti</span><span class="sxs-lookup"><span data-stu-id="3973f-157">Location of the cache</span></span> | |
| <span data-ttu-id="3973f-158">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="3973f-158">ResourceGroupName</span></span> |<span data-ttu-id="3973f-159">Název skupiny prostředků, ve kterém k vytvoření mezipaměti</span><span class="sxs-lookup"><span data-stu-id="3973f-159">Resource group name in which to create the cache</span></span> | |
| <span data-ttu-id="3973f-160">Velikost</span><span class="sxs-lookup"><span data-stu-id="3973f-160">Size</span></span> |<span data-ttu-id="3973f-161">Velikost mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-161">The size of the cache.</span></span> <span data-ttu-id="3973f-162">Platné hodnoty jsou: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB</span><span class="sxs-lookup"><span data-stu-id="3973f-162">Valid values are: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB</span></span> |<span data-ttu-id="3973f-163">1GB</span><span class="sxs-lookup"><span data-stu-id="3973f-163">1GB</span></span> |
| <span data-ttu-id="3973f-164">ShardCount</span><span class="sxs-lookup"><span data-stu-id="3973f-164">ShardCount</span></span> |<span data-ttu-id="3973f-165">Počet horizontálních oddílů vytvořit při vytváření cache ve verzi premium s povoleným clusteringem.</span><span class="sxs-lookup"><span data-stu-id="3973f-165">The number of shards to create when creating a premium cache with clustering enabled.</span></span> <span data-ttu-id="3973f-166">Platné hodnoty jsou: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10</span><span class="sxs-lookup"><span data-stu-id="3973f-166">Valid values are: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10</span></span> | |
| <span data-ttu-id="3973f-167">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="3973f-167">SKU</span></span> |<span data-ttu-id="3973f-168">Určuje skladová položka mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-168">Specifies the SKU of the cache.</span></span> <span data-ttu-id="3973f-169">Platné hodnoty jsou: Basic, Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="3973f-169">Valid values are: Basic, Standard, Premium</span></span> |<span data-ttu-id="3973f-170">Standard</span><span class="sxs-lookup"><span data-stu-id="3973f-170">Standard</span></span> |
| <span data-ttu-id="3973f-171">RedisConfiguration</span><span class="sxs-lookup"><span data-stu-id="3973f-171">RedisConfiguration</span></span> |<span data-ttu-id="3973f-172">Určuje nastavení konfigurace Redis.</span><span class="sxs-lookup"><span data-stu-id="3973f-172">Specifies Redis configuration settings.</span></span> <span data-ttu-id="3973f-173">Podrobnosti o jednotlivých nastaveních najdete v tématu následující [RedisConfiguration vlastnosti](#redisconfiguration-properties) tabulky.</span><span class="sxs-lookup"><span data-stu-id="3973f-173">For details on each setting, see the following [RedisConfiguration properties](#redisconfiguration-properties) table.</span></span> | |
| <span data-ttu-id="3973f-174">EnableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="3973f-174">EnableNonSslPort</span></span> |<span data-ttu-id="3973f-175">Určuje, zda je povoleno port bez SSL.</span><span class="sxs-lookup"><span data-stu-id="3973f-175">Indicates whether the non-SSL port is enabled.</span></span> |<span data-ttu-id="3973f-176">False</span><span class="sxs-lookup"><span data-stu-id="3973f-176">False</span></span> |
| <span data-ttu-id="3973f-177">MaxMemoryPolicy</span><span class="sxs-lookup"><span data-stu-id="3973f-177">MaxMemoryPolicy</span></span> |<span data-ttu-id="3973f-178">Tento parametr se již nepoužívá – místo toho použijte RedisConfiguration.</span><span class="sxs-lookup"><span data-stu-id="3973f-178">This parameter has been deprecated - use RedisConfiguration instead.</span></span> | |
| <span data-ttu-id="3973f-179">StaticIP</span><span class="sxs-lookup"><span data-stu-id="3973f-179">StaticIP</span></span> |<span data-ttu-id="3973f-180">Při hostování vaší mezipaměti ve virtuální síti, určuje jedinečnou IP adresu v mezipaměti v podsíti.</span><span class="sxs-lookup"><span data-stu-id="3973f-180">When hosting your cache in a VNET, specifies a unique IP address in the subnet for the cache.</span></span> <span data-ttu-id="3973f-181">Pokud není zadaná, jeden z podsítě vybrali za vás.</span><span class="sxs-lookup"><span data-stu-id="3973f-181">If not provided, one is chosen for you from the subnet.</span></span> | |
| <span data-ttu-id="3973f-182">Podsíť</span><span class="sxs-lookup"><span data-stu-id="3973f-182">Subnet</span></span> |<span data-ttu-id="3973f-183">Při hostování vaší mezipaměti ve virtuální síti, určuje název podsítě, ve které chcete nasadit do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-183">When hosting your cache in a VNET, specifies the name of the subnet in which to deploy the cache.</span></span> | |
| <span data-ttu-id="3973f-184">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="3973f-184">VirtualNetwork</span></span> |<span data-ttu-id="3973f-185">Při hostování vaší mezipaměti ve virtuální síti, určuje ID prostředku sítě vnet, ve které chcete nasadit do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-185">When hosting your cache in a VNET, specifies the resource ID of the VNET in which to deploy the cache.</span></span> | |
| <span data-ttu-id="3973f-186">Typ_klíče.</span><span class="sxs-lookup"><span data-stu-id="3973f-186">KeyType</span></span> |<span data-ttu-id="3973f-187">Určuje, které přístupový klíč se znovu vygenerovat při obnovování přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="3973f-187">Specifies which access key to regenerate when renewing access keys.</span></span> <span data-ttu-id="3973f-188">Platné hodnoty jsou: primární, sekundární</span><span class="sxs-lookup"><span data-stu-id="3973f-188">Valid values are: Primary, Secondary</span></span> | |

### <a name="redisconfiguration-properties"></a><span data-ttu-id="3973f-189">Vlastnosti RedisConfiguration</span><span class="sxs-lookup"><span data-stu-id="3973f-189">RedisConfiguration properties</span></span>
| <span data-ttu-id="3973f-190">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3973f-190">Property</span></span> | <span data-ttu-id="3973f-191">Popis</span><span class="sxs-lookup"><span data-stu-id="3973f-191">Description</span></span> | <span data-ttu-id="3973f-192">Cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="3973f-192">Pricing tiers</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3973f-193">Povolit zálohování RDB</span><span class="sxs-lookup"><span data-stu-id="3973f-193">rdb-backup-enabled</span></span> |<span data-ttu-id="3973f-194">Jestli [trvalosti dat Redis](cache-how-to-premium-persistence.md) je povoleno</span><span class="sxs-lookup"><span data-stu-id="3973f-194">Whether [Redis data persistence](cache-how-to-premium-persistence.md) is enabled</span></span> |<span data-ttu-id="3973f-195">Pouze Premium</span><span class="sxs-lookup"><span data-stu-id="3973f-195">Premium only</span></span> |
| <span data-ttu-id="3973f-196">RDB úložiště připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="3973f-196">rdb-storage-connection-string</span></span> |<span data-ttu-id="3973f-197">Připojovací řetězec k účtu úložiště pro [trvalosti dat Redis](cache-how-to-premium-persistence.md)</span><span class="sxs-lookup"><span data-stu-id="3973f-197">The connection string to the storage account for [Redis data persistence](cache-how-to-premium-persistence.md)</span></span> |<span data-ttu-id="3973f-198">Pouze Premium</span><span class="sxs-lookup"><span data-stu-id="3973f-198">Premium only</span></span> |
| <span data-ttu-id="3973f-199">četnost záloh RDB</span><span class="sxs-lookup"><span data-stu-id="3973f-199">rdb-backup-frequency</span></span> |<span data-ttu-id="3973f-200">Četnost záloh pro [trvalosti dat Redis](cache-how-to-premium-persistence.md)</span><span class="sxs-lookup"><span data-stu-id="3973f-200">The backup frequency for [Redis data persistence](cache-how-to-premium-persistence.md)</span></span> |<span data-ttu-id="3973f-201">Pouze Premium</span><span class="sxs-lookup"><span data-stu-id="3973f-201">Premium only</span></span> |
| <span data-ttu-id="3973f-202">vyhrazené maxmemory</span><span class="sxs-lookup"><span data-stu-id="3973f-202">maxmemory-reserved</span></span> |<span data-ttu-id="3973f-203">Nakonfiguruje [paměti vyhrazené](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) pro procesy bez ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="3973f-203">Configures the [memory reserved](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) for non-cache processes</span></span> |<span data-ttu-id="3973f-204">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="3973f-204">Standard and Premium</span></span> |
| <span data-ttu-id="3973f-205">maxmemory zásady</span><span class="sxs-lookup"><span data-stu-id="3973f-205">maxmemory-policy</span></span> |<span data-ttu-id="3973f-206">Nakonfiguruje [zásady vyřazení](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) mezipaměti</span><span class="sxs-lookup"><span data-stu-id="3973f-206">Configures the [eviction policy](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) for the cache</span></span> |<span data-ttu-id="3973f-207">Všechny cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="3973f-207">All pricing tiers</span></span> |
| <span data-ttu-id="3973f-208">oznámení události keyspace</span><span class="sxs-lookup"><span data-stu-id="3973f-208">notify-keyspace-events</span></span> |<span data-ttu-id="3973f-209">Nakonfiguruje [oznámení keyspace](cache-configure.md#keyspace-notifications-advanced-settings)</span><span class="sxs-lookup"><span data-stu-id="3973f-209">Configures [keyspace notifications](cache-configure.md#keyspace-notifications-advanced-settings)</span></span> |<span data-ttu-id="3973f-210">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="3973f-210">Standard and Premium</span></span> |
| <span data-ttu-id="3973f-211">Hodnota hash-max-ziplist – položky</span><span class="sxs-lookup"><span data-stu-id="3973f-211">hash-max-ziplist-entries</span></span> |<span data-ttu-id="3973f-212">Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy</span><span class="sxs-lookup"><span data-stu-id="3973f-212">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="3973f-213">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="3973f-213">Standard and Premium</span></span> |
| <span data-ttu-id="3973f-214">max-ziplist hodnota hash</span><span class="sxs-lookup"><span data-stu-id="3973f-214">hash-max-ziplist-value</span></span> |<span data-ttu-id="3973f-215">Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy</span><span class="sxs-lookup"><span data-stu-id="3973f-215">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="3973f-216">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="3973f-216">Standard and Premium</span></span> |
| <span data-ttu-id="3973f-217">set-max-intset – položky</span><span class="sxs-lookup"><span data-stu-id="3973f-217">set-max-intset-entries</span></span> |<span data-ttu-id="3973f-218">Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy</span><span class="sxs-lookup"><span data-stu-id="3973f-218">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="3973f-219">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="3973f-219">Standard and Premium</span></span> |
| <span data-ttu-id="3973f-220">zset-max-ziplist – položky</span><span class="sxs-lookup"><span data-stu-id="3973f-220">zset-max-ziplist-entries</span></span> |<span data-ttu-id="3973f-221">Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy</span><span class="sxs-lookup"><span data-stu-id="3973f-221">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="3973f-222">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="3973f-222">Standard and Premium</span></span> |
| <span data-ttu-id="3973f-223">zset-max-ziplist – hodnota</span><span class="sxs-lookup"><span data-stu-id="3973f-223">zset-max-ziplist-value</span></span> |<span data-ttu-id="3973f-224">Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy</span><span class="sxs-lookup"><span data-stu-id="3973f-224">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="3973f-225">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="3973f-225">Standard and Premium</span></span> |
| <span data-ttu-id="3973f-226">databáze</span><span class="sxs-lookup"><span data-stu-id="3973f-226">databases</span></span> |<span data-ttu-id="3973f-227">Konfiguruje počet databází.</span><span class="sxs-lookup"><span data-stu-id="3973f-227">Configures the number of databases.</span></span> <span data-ttu-id="3973f-228">Tuto vlastnost lze nastavit pouze při vytváření mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-228">This property can be configured only at cache creation.</span></span> |<span data-ttu-id="3973f-229">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="3973f-229">Standard and Premium</span></span> |

## <a name="to-create-a-redis-cache"></a><span data-ttu-id="3973f-230">K vytvoření mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="3973f-230">To create a Redis Cache</span></span>
<span data-ttu-id="3973f-231">Nové instance služby Azure Redis Cache lze vytvořit pomocí [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3973f-231">New Azure Redis Cache instances are created using the [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3973f-232">Při prvním vytvoření mezipaměti Redis v předplatném pomocí portálu Azure portálu zaregistruje `Microsoft.Cache` obor názvů pro toto předplatné.</span><span class="sxs-lookup"><span data-stu-id="3973f-232">The first time you create a Redis cache in a subscription using the Azure portal, the portal registers the `Microsoft.Cache` namespace for that subscription.</span></span> <span data-ttu-id="3973f-233">Pokud se pokusíte vytvořit první mezipaměti Redis v předplatném pomocí prostředí PowerShell, je nutné nejprve zaregistrovat tento obor názvů pomocí následujícího příkazu; jinak rutin, jako `New-AzureRmRedisCache` a `Get-AzureRmRedisCache` nezdaří.</span><span class="sxs-lookup"><span data-stu-id="3973f-233">If you attempt to create the first Redis cache in a subscription using PowerShell, you must first register that namespace using the following command; otherwise cmdlets such as `New-AzureRmRedisCache` and `Get-AzureRmRedisCache` fail.</span></span>
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

<span data-ttu-id="3973f-234">Chcete-li zobrazit seznam dostupných parametrů a jejich popisy pro `New-AzureRmRedisCache`, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="3973f-234">To see a list of available parameters and their descriptions for `New-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help New-AzureRmRedisCache -detailed

    NAME
        New-AzureRmRedisCache

    SYNOPSIS
        Creates a new redis cache.


    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.


    PARAMETERS
        -Name <String>
            Name of the redis cache to create.

        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.

        -Location <String>
            Location in which to create the redis cache.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="3973f-235">K vytvoření mezipaměti se výchozí parametry, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="3973f-235">To create a cache with default parameters, run the following command.</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

<span data-ttu-id="3973f-236">`ResourceGroupName`, `Name`, a `Location` jsou požadované parametry, ale zbývající jsou volitelné a mají výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3973f-236">`ResourceGroupName`, `Name`, and `Location` are required parameters, but the rest are optional and have default values.</span></span> <span data-ttu-id="3973f-237">Předchozí příkaz vytvoří instanci standardní SKU Azure Redis Cache s zadaný název, umístění a skupinu prostředků, který je 1 GB velikost port bez SSL zakázána.</span><span class="sxs-lookup"><span data-stu-id="3973f-237">Running the previous command creates a Standard SKU Azure Redis Cache instance with the specified name, location, and resource group, that is 1 GB in size with the non-SSL port disabled.</span></span>

<span data-ttu-id="3973f-238">Chcete-li vytvořit cache ve verzi premium, zadejte velikost P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), nebo P4 (53 GB - 530 GB).</span><span class="sxs-lookup"><span data-stu-id="3973f-238">To create a premium cache, specify a size of P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), or P4 (53 GB - 530 GB).</span></span> <span data-ttu-id="3973f-239">Pokud chcete povolit, clustering, zadejte počet horizontálních pomocí `ShardCount` parametr.</span><span class="sxs-lookup"><span data-stu-id="3973f-239">To enable clustering, specify a shard count using the `ShardCount` parameter.</span></span> <span data-ttu-id="3973f-240">Následující příklad vytvoří mezipaměť premium P1 s 3 horizontálními oddíly.</span><span class="sxs-lookup"><span data-stu-id="3973f-240">The following example creates a P1 premium cache with 3 shards.</span></span> <span data-ttu-id="3973f-241">Mezipaměť premium P1 je 6 GB velikost a vzhledem k tomu, že jsme zadali tři horizontálních oddílů je celková velikost 18 GB (3 × 6 GB).</span><span class="sxs-lookup"><span data-stu-id="3973f-241">A P1 premium cache is 6 GB in size, and since we specified three shards the total size is 18 GB (3 x 6 GB).</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

<span data-ttu-id="3973f-242">Zadat hodnoty `RedisConfiguration` parametr, uzavřete hodnoty uvnitř `{}` jako klíč/hodnota, jako páry `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`.</span><span class="sxs-lookup"><span data-stu-id="3973f-242">To specify values for the `RedisConfiguration` parameter, enclose the values inside `{}` as key/value pairs like `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`.</span></span> <span data-ttu-id="3973f-243">Následující příklad vytvoří standardní 1 GB mezipaměti s `allkeys-random` maxmemory zásady a keyspace oznámení nakonfigurované s `KEA`.</span><span class="sxs-lookup"><span data-stu-id="3973f-243">The following example creates a standard 1 GB cache with `allkeys-random` maxmemory policy and keyspace notifications configured with `KEA`.</span></span> <span data-ttu-id="3973f-244">Další informace najdete v tématu [oznámení Keyspace (rozšířené nastavení)](cache-configure.md#keyspace-notifications-advanced-settings) a [paměti zásady](cache-configure.md#memory-policies).</span><span class="sxs-lookup"><span data-stu-id="3973f-244">For more information, see [Keyspace notifications (advanced settings)](cache-configure.md#keyspace-notifications-advanced-settings) and [Memory policies](cache-configure.md#memory-policies).</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="to-configure-the-databases-setting-during-cache-creation"></a><span data-ttu-id="3973f-245">Chcete-li konfigurovat nastavení během vytváření mezipaměti databáze</span><span class="sxs-lookup"><span data-stu-id="3973f-245">To configure the databases setting during cache creation</span></span>
<span data-ttu-id="3973f-246">`databases` Nastavení se dá nakonfigurovat jenom během vytváření mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-246">The `databases` setting can be configured only during cache creation.</span></span> <span data-ttu-id="3973f-247">Následující příklad vytvoří premium P3 (26 GB) mezipaměti s 48 databází pomocí [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3973f-247">The following example creates a premium P3 (26 GB) cache with 48 databases using the [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

<span data-ttu-id="3973f-248">Další informace o `databases` vlastnost, najdete v části [konfigurace serveru výchozí Azure Redis Cache](cache-configure.md#default-redis-server-configuration).</span><span class="sxs-lookup"><span data-stu-id="3973f-248">For more information on the `databases` property, see [Default Azure Redis Cache server configuration](cache-configure.md#default-redis-server-configuration).</span></span> <span data-ttu-id="3973f-249">Další informace o vytvoření mezipaměti pomocí [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) rutiny, najdete v předchozí [k vytvoření mezipaměti Redis](#to-create-a-redis-cache) části.</span><span class="sxs-lookup"><span data-stu-id="3973f-249">For more information on creating a cache using the [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet, see the previous [To create a Redis Cache](#to-create-a-redis-cache) section.</span></span>

## <a name="to-update-a-redis-cache"></a><span data-ttu-id="3973f-250">Aktualizace mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="3973f-250">To update a Redis cache</span></span>
<span data-ttu-id="3973f-251">Instance služby Azure Redis Cache jsou aktualizovány pomocí [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3973f-251">Azure Redis Cache instances are updated using the [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet.</span></span>

<span data-ttu-id="3973f-252">Chcete-li zobrazit seznam dostupných parametrů a jejich popisy pro `Set-AzureRmRedisCache`, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="3973f-252">To see a list of available parameters and their descriptions for `Set-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed

    NAME
        Set-AzureRmRedisCache

    SYNOPSIS
        Set redis cache updatable parameters.

    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.

    PARAMETERS
        -Name <String>
            Name of the redis cache to update.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="3973f-253">`Set-AzureRmRedisCache` Rutinu můžete použít k aktualizaci vlastností, jako `Size`, `Sku`, `EnableNonSslPort`a `RedisConfiguration` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3973f-253">The `Set-AzureRmRedisCache` cmdlet can be used to update properties such as `Size`, `Sku`, `EnableNonSslPort`, and the `RedisConfiguration` values.</span></span> 

<span data-ttu-id="3973f-254">Příkaz aktualizuje maxmemory zásady pro Redis Cache s názvem myCache.</span><span class="sxs-lookup"><span data-stu-id="3973f-254">The following command updates the maxmemory-policy for the Redis Cache named myCache.</span></span>

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="to-scale-a-redis-cache"></a><span data-ttu-id="3973f-255">Škálování mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="3973f-255">To scale a Redis cache</span></span>
<span data-ttu-id="3973f-256">`Set-AzureRmRedisCache`umožňuje škálovat mezipamětí Azure Redis instance, kdy `Size`, `Sku`, nebo `ShardCount` jsou upraveny vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3973f-256">`Set-AzureRmRedisCache` can be used to scale an Azure Redis cache instance when the `Size`, `Sku`, or `ShardCount` properties are modified.</span></span> 

> [!NOTE]
> <span data-ttu-id="3973f-257">Změna velikosti mezipaměti pomocí prostředí PowerShell je za stejné omezení a pokyny jako škálování mezipaměti z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3973f-257">Scaling a cache using PowerShell is subject to the same limits and guidelines as scaling a cache from the Azure portal.</span></span> <span data-ttu-id="3973f-258">Je možné škálovat na jinou cenovou úroveň, s následujícími omezeními.</span><span class="sxs-lookup"><span data-stu-id="3973f-258">You can scale to a different pricing tier with the following restrictions.</span></span>
> 
> * <span data-ttu-id="3973f-259">Z používat vyšší cenová úroveň je nelze škálovat na nižší cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="3973f-259">You can't scale from a higher pricing tier to a lower pricing tier.</span></span>
> * <span data-ttu-id="3973f-260">Nelze škálovat od **Premium** dolů do mezipaměti **standardní** nebo **základní** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-260">You can't scale from a **Premium** cache down to a **Standard** or a **Basic** cache.</span></span>
> * <span data-ttu-id="3973f-261">Nelze škálovat od **standardní** dolů do mezipaměti **základní** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-261">You can't scale from a **Standard** cache down to a **Basic** cache.</span></span>
> * <span data-ttu-id="3973f-262">Je možné škálovat od **základní** mezipaměti tak, aby **standardní** mezipaměti, ale nemůže změnit velikost ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="3973f-262">You can scale from a **Basic** cache to a **Standard** cache but you can't change the size at the same time.</span></span> <span data-ttu-id="3973f-263">Pokud potřebujete jinou velikost, můžete provést následující operaci škálování na požadovanou velikost.</span><span class="sxs-lookup"><span data-stu-id="3973f-263">If you need a different size, you can do a subsequent scaling operation to the desired size.</span></span>
> * <span data-ttu-id="3973f-264">Nelze škálovat od **základní** přímo do mezipaměti **Premium** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-264">You can't scale from a **Basic** cache directly to a **Premium** cache.</span></span> <span data-ttu-id="3973f-265">Musí škálování z **základní** k **standardní** v rámci jedné operace škálování a potom z **standardní** k **Premium** v následných operaci škálování.</span><span class="sxs-lookup"><span data-stu-id="3973f-265">You must scale from **Basic** to **Standard** in one scaling operation, and then from **Standard** to **Premium** in a subsequent scaling operation.</span></span>
> * <span data-ttu-id="3973f-266">Nelze škálovat z větší velikost dolů na **C0 (250 MB)** velikost.</span><span class="sxs-lookup"><span data-stu-id="3973f-266">You can't scale from a larger size down to the **C0 (250 MB)** size.</span></span>
> 
> <span data-ttu-id="3973f-267">Další informace najdete v tématu [postup škálování Azure Redis Cache](cache-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="3973f-267">For more information, see [How to Scale Azure Redis Cache](cache-how-to-scale.md).</span></span>
> 
> 

<span data-ttu-id="3973f-268">Následující příklad ukazuje postup škálování mezipaměti s názvem `myCache` do mezipaměti 2,5 GB.</span><span class="sxs-lookup"><span data-stu-id="3973f-268">The following example shows how to scale a cache named `myCache` to a 2.5 GB cache.</span></span> <span data-ttu-id="3973f-269">Všimněte si, že tento příkaz lze použít pro základní nebo standardní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-269">Note that this command works for both a Basic or a Standard cache.</span></span>

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

<span data-ttu-id="3973f-270">Po vydání tento příkaz, je vrácen stav mezipaměti (podobně jako volání `Get-AzureRmRedisCache`).</span><span class="sxs-lookup"><span data-stu-id="3973f-270">After this command is issued, the status of the cache is returned (similar to calling `Get-AzureRmRedisCache`).</span></span> <span data-ttu-id="3973f-271">Všimněte si, že `ProvisioningState` je `Scaling`.</span><span class="sxs-lookup"><span data-stu-id="3973f-271">Note that the `ProvisioningState` is `Scaling`.</span></span>

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB


    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

<span data-ttu-id="3973f-272">Po dokončení operace škálování `ProvisioningState` změny `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="3973f-272">When the scaling operation is complete, the `ProvisioningState` changes to `Succeeded`.</span></span> <span data-ttu-id="3973f-273">Pokud potřebujete provést následné škálování operace, jako je například změna z Basic na Standard a pak změny velikosti, musíte počkat, až po dokončení předchozí operace nebo obdržíte chybu podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="3973f-273">If you need to make a subsequent scaling operation, such as changing from Basic to Standard and then changing the size, you must wait until the previous operation is complete or you receive an error similar to the following.</span></span>

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a><span data-ttu-id="3973f-274">Chcete-li získat informace o mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="3973f-274">To get information about a Redis cache</span></span>
<span data-ttu-id="3973f-275">Můžete načíst informace o mezipaměti pomocí [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3973f-275">You can retrieve information about a cache using the [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) cmdlet.</span></span>

<span data-ttu-id="3973f-276">Chcete-li zobrazit seznam dostupných parametrů a jejich popisy pro `Get-AzureRmRedisCache`, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="3973f-276">To see a list of available parameters and their descriptions for `Get-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed

    NAME
        Get-AzureRmRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.

    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.

        If no parameters are given than it will return details about all caches the current subscription.

    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="3973f-277">K vrácení informací o všechny mezipaměti v aktuálním předplatném, spusťte `Get-AzureRmRedisCache` bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="3973f-277">To return information about all caches in the current subscription, run `Get-AzureRmRedisCache` without any parameters.</span></span>

    Get-AzureRmRedisCache

<span data-ttu-id="3973f-278">K vrácení informací o všechny mezipaměti v určité skupiny zdrojů, spusťte `Get-AzureRmRedisCache` s `ResourceGroupName` parametr.</span><span class="sxs-lookup"><span data-stu-id="3973f-278">To return information about all caches in a specific resource group, run `Get-AzureRmRedisCache` with the `ResourceGroupName` parameter.</span></span>

    Get-AzureRmRedisCache -ResourceGroupName myGroup

<span data-ttu-id="3973f-279">Chcete-li vrátit informace o konkrétní mezipaměti, spusťte `Get-AzureRmRedisCache` s `Name` parametr, který obsahuje název mezipaměti a `ResourceGroupName` parametr s skupinu prostředků obsahující mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="3973f-279">To return information about a specific cache, run `Get-AzureRmRedisCache` with the `Name` parameter containing the name of the cache, and the `ResourceGroupName` parameter with the resource group containing that cache.</span></span>

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a><span data-ttu-id="3973f-280">Chcete-li získat přístupové klíče pro Redis cache</span><span class="sxs-lookup"><span data-stu-id="3973f-280">To retrieve the access keys for a Redis cache</span></span>
<span data-ttu-id="3973f-281">Chcete-li získat přístupové klíče pro mezipaměť, můžete použít [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3973f-281">To retrieve the access keys for your cache, you can use the [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) cmdlet.</span></span>

<span data-ttu-id="3973f-282">Chcete-li zobrazit seznam dostupných parametrů a jejich popisy pro `Get-AzureRmRedisCacheKey`, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="3973f-282">To see a list of available parameters and their descriptions for `Get-AzureRmRedisCacheKey`, run the following command.</span></span>

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets the accesskeys for the specified redis cache.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="3973f-283">Chcete-li získat klíče pro mezipaměť, zavolejte `Get-AzureRmRedisCacheKey` rutiny a předat názvu vaší mezipaměti název skupiny prostředků, který obsahuje mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-283">To retrieve the keys for your cache, call the `Get-AzureRmRedisCacheKey` cmdlet and pass in the name of your cache the name of the resource group that contains the cache.</span></span>

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a><span data-ttu-id="3973f-284">Chcete-li znovu vygenerovat přístupové klíče pro vaše mezipaměť Redis</span><span class="sxs-lookup"><span data-stu-id="3973f-284">To regenerate access keys for your Redis cache</span></span>
<span data-ttu-id="3973f-285">Chcete-li znovu vygenerovat přístupové klíče pro mezipaměť, můžete použít [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3973f-285">To regenerate the access keys for your cache, you can use the [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) cmdlet.</span></span>

<span data-ttu-id="3973f-286">Chcete-li zobrazit seznam dostupných parametrů a jejich popisy pro `New-AzureRmRedisCacheKey`, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="3973f-286">To see a list of available parameters and their descriptions for `New-AzureRmRedisCacheKey`, run the following command.</span></span>

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates the access key of a redis cache.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="3973f-287">Chcete-li znovu vygenerovat primární nebo sekundární klíč pro mezipaměť, volejte `New-AzureRmRedisCacheKey` rutiny a předejte název skupiny prostředků a zadejte buď `Primary` nebo `Secondary` pro `KeyType` parametr.</span><span class="sxs-lookup"><span data-stu-id="3973f-287">To regenerate the primary or secondary key for your cache, call the `New-AzureRmRedisCacheKey` cmdlet and pass in the name, resource group, and specify either `Primary` or `Secondary` for the `KeyType` parameter.</span></span> <span data-ttu-id="3973f-288">V následujícím příkladu je znovu vygenerovat sekundární přístupový klíč pro mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="3973f-288">In the following example, the secondary access key for a cache is regenerated.</span></span>

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a><span data-ttu-id="3973f-289">Chcete-li odstranit mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="3973f-289">To delete a Redis cache</span></span>
<span data-ttu-id="3973f-290">Pokud chcete odstranit mezipaměti Redis, použijte [odebrat AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3973f-290">To delete a Redis cache, use the [Remove-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) cmdlet.</span></span>

<span data-ttu-id="3973f-291">Chcete-li zobrazit seznam dostupných parametrů a jejich popisy pro `Remove-AzureRmRedisCache`, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="3973f-291">To see a list of available parameters and their descriptions for `Remove-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove redis cache if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.

    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.

        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.

        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="3973f-292">V následujícím příkladu, s názvem mezipaměti `myCache` se odebere.</span><span class="sxs-lookup"><span data-stu-id="3973f-292">In the following example, the cache named `myCache` is removed.</span></span>

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a><span data-ttu-id="3973f-293">Chcete-li importovat mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="3973f-293">To import a Redis cache</span></span>
<span data-ttu-id="3973f-294">Data můžete importovat do instance Azure Redis Cache pomocí `Import-AzureRmRedisCache` rutiny.</span><span class="sxs-lookup"><span data-stu-id="3973f-294">You can import data into an Azure Redis Cache instance using the `Import-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3973f-295">Import a Export je k dispozici pouze [úroveň premium](cache-premium-tier-intro.md) ukládá do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-295">Import/Export is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="3973f-296">Další informace o importu a exportu najdete v tématu [importu a exportu dat ve službě Azure Redis Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="3973f-296">For more information about Import/Export, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

<span data-ttu-id="3973f-297">Chcete-li zobrazit seznam dostupných parametrů a jejich popisy pro `Import-AzureRmRedisCache`, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="3973f-297">To see a list of available parameters and their descriptions for `Import-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs to Azure Redis Cache.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="3973f-298">Následující příkaz importuje data z objektu blob určeného identifikátor uri SAS do Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="3973f-298">The following command imports data from the blob specified by the SAS uri into Azure Redis Cache.</span></span>

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a><span data-ttu-id="3973f-299">Chcete-li exportovat mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="3973f-299">To export a Redis cache</span></span>
<span data-ttu-id="3973f-300">Data můžete exportovat z instance Azure Redis Cache pomocí `Export-AzureRmRedisCache` rutiny.</span><span class="sxs-lookup"><span data-stu-id="3973f-300">You can export data from an Azure Redis Cache instance using the `Export-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3973f-301">Import a Export je k dispozici pouze [úroveň premium](cache-premium-tier-intro.md) ukládá do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-301">Import/Export is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="3973f-302">Další informace o importu a exportu najdete v tématu [importu a exportu dat ve službě Azure Redis Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="3973f-302">For more information about Import/Export, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

<span data-ttu-id="3973f-303">Chcete-li zobrazit seznam dostupných parametrů a jejich popisy pro `Export-AzureRmRedisCache`, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="3973f-303">To see a list of available parameters and their descriptions for `Export-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Prefix <String>
            Prefix to use for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="3973f-304">Tento příkaz exportuje data z instance Azure Redis Cache do kontejneru určeného identifikátor uri SAS.</span><span class="sxs-lookup"><span data-stu-id="3973f-304">The following command exports data from an Azure Redis Cache instance into the container specified by the SAS uri.</span></span>

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a><span data-ttu-id="3973f-305">Restartování mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="3973f-305">To reboot a Redis cache</span></span>
<span data-ttu-id="3973f-306">Restartování počítače pomocí instance Azure Redis Cache `Reset-AzureRmRedisCache` rutiny.</span><span class="sxs-lookup"><span data-stu-id="3973f-306">You can reboot your Azure Redis Cache instance using the `Reset-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3973f-307">Restart je k dispozici pouze [úroveň premium](cache-premium-tier-intro.md) ukládá do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-307">Reboot is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="3973f-308">Další informace o restartování mezipaměti najdete v tématu [mezipaměti správy – restartovat](cache-administration.md#reboot).</span><span class="sxs-lookup"><span data-stu-id="3973f-308">For more information about rebooting your cache, see [Cache administration - reboot](cache-administration.md#reboot).</span></span>
> 
> 

<span data-ttu-id="3973f-309">Chcete-li zobrazit seznam dostupných parametrů a jejich popisy pro `Reset-AzureRmRedisCache`, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="3973f-309">To see a list of available parameters and their descriptions for `Reset-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.

        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="3973f-310">Následující příkaz restartuje oba uzly zadané mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3973f-310">The following command reboots both nodes of the specified cache.</span></span>

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a><span data-ttu-id="3973f-311">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3973f-311">Next steps</span></span>
<span data-ttu-id="3973f-312">Další informace o používání prostředí Windows PowerShell s Azure, najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="3973f-312">To learn more about using Windows PowerShell with Azure, see the following resources:</span></span>

* [<span data-ttu-id="3973f-313">Azure Redis Cache rutiny dokumentaci na webu MSDN</span><span class="sxs-lookup"><span data-stu-id="3973f-313">Azure Redis Cache cmdlet documentation on MSDN</span></span>](https://msdn.microsoft.com/library/azure/mt634513.aspx)
* <span data-ttu-id="3973f-314">[Rutiny Azure Resource Manager](http://go.microsoft.com/fwlink/?LinkID=394765): Naučte se používat rutiny v modulu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="3973f-314">[Azure Resource Manager Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): Learn to use the cmdlets in the Azure Resource Manager module.</span></span>
* <span data-ttu-id="3973f-315">[Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-template-deploy-portal.md): Naučte se vytvářet a spravovat skupiny prostředků na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3973f-315">[Using Resource groups to manage your Azure resources](../azure-resource-manager/resource-group-template-deploy-portal.md): Learn how to create and manage resource groups in the Azure portal.</span></span>
* <span data-ttu-id="3973f-316">[Azure blog](http://blogs.msdn.com/windowsazure): Další informace o nových funkcích v Azure.</span><span class="sxs-lookup"><span data-stu-id="3973f-316">[Azure blog](http://blogs.msdn.com/windowsazure): Learn about new features in Azure.</span></span>
* <span data-ttu-id="3973f-317">[Blogu o prostředí Windows PowerShell](http://blogs.msdn.com/powershell): Další informace o nové funkce v prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3973f-317">[Windows PowerShell blog](http://blogs.msdn.com/powershell): Learn about new features in Windows PowerShell.</span></span>
* <span data-ttu-id="3973f-318">["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): získat reálného tipy a triky od komunity prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3973f-318">["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): Get real-world tips and tricks from the Windows PowerShell community.</span></span>

