---
title: "aaaManage Azure Redis Cache pomocí Azure Powershellu | Microsoft Docs"
description: "Zjistěte, jak tooperform úlohy správy pro Azure Redis Cache pomocí Azure PowerShell."
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
ms.openlocfilehash: 1d526ce65c4bc05345cd6c3ff370211ed562cab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-redis-cache-with-azure-powershell"></a><span data-ttu-id="dc0d2-103">Spravovat mezipaměť Redis systému Azure pomocí Azure Powershellu</span><span class="sxs-lookup"><span data-stu-id="dc0d2-103">Manage Azure Redis Cache with Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dc0d2-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="dc0d2-104">PowerShell</span></span>](cache-howto-manage-redis-cache-powershell.md)
> * [<span data-ttu-id="dc0d2-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="dc0d2-105">Azure CLI</span></span>](cache-manage-cli.md)
> 
> 

<span data-ttu-id="dc0d2-106">Toto téma ukazuje, jak tooperform běžné úkoly, jako je vytváření, aktualizace a škálovat vaše instance služby Azure Redis Cache, jak tooregenerate přístupových klíčů a jak tooview informace o své mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-106">This topic shows you how tooperform common tasks such as create, update, and scale your Azure Redis Cache instances, how tooregenerate access keys, and how tooview information about your caches.</span></span> <span data-ttu-id="dc0d2-107">Úplný seznam rutin Powershellu pro Azure Redis Cache najdete v tématu [rutiny Azure Redis Cache](https://msdn.microsoft.com/library/azure/mt634513.aspx).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-107">For a complete list of Azure Redis Cache PowerShell cmdlets, see [Azure Redis Cache cmdlets](https://msdn.microsoft.com/library/azure/mt634513.aspx).</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="dc0d2-108">Další informace o modelu nasazení classic hello najdete v tématu [Azure Resource Manager oproti nasazení classic: pochopení modely nasazení a hello stav svých prostředků](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-108">For more information about hello classic deployment model, see [Azure Resource Manager vs. classic deployment: Understand deployment models and hello state of your resources](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dc0d2-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dc0d2-109">Prerequisites</span></span>
<span data-ttu-id="dc0d2-110">Pokud jste již Azure PowerShell nainstalovali, musíte mít prostředí Azure PowerShell verze 1.0.0 nebo později.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-110">If you have already installed Azure PowerShell, you must have Azure PowerShell version 1.0.0 or later.</span></span> <span data-ttu-id="dc0d2-111">Můžete zkontrolovat hello verzi prostředí Azure PowerShell, který jste nainstalovali pomocí tohoto příkazu na příkazovém řádku prostředí Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-111">You can check hello version of Azure PowerShell that you have installed with this command at hello Azure PowerShell command prompt.</span></span>

    Get-Module azure | format-table version


<span data-ttu-id="dc0d2-112">Nejdřív musíte být přihlášení tooAzure pomocí tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-112">First, you must log in tooAzure with this command.</span></span>

    Login-AzureRmAccount

<span data-ttu-id="dc0d2-113">Zadejte hello e-mailovou adresu účtu Azure a jeho heslo v dialogovém okně hello přihlášení Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-113">Specify hello email address of your Azure account and its password in hello Microsoft Azure sign-in dialog.</span></span>

<span data-ttu-id="dc0d2-114">Dále pokud máte víc předplatných Azure, musíte tooset vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-114">Next, if you have multiple Azure subscriptions, you need tooset your Azure subscription.</span></span> <span data-ttu-id="dc0d2-115">Seznam odběrů aktuální toosee tento příkaz spustit.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-115">toosee a list of your current subscriptions, run this command.</span></span>

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

<span data-ttu-id="dc0d2-116">toospecify hello předplatného, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-116">toospecify hello subscription, run hello following command.</span></span> <span data-ttu-id="dc0d2-117">V následujícím příkladu hello, je název odběru hello `ContosoSubscription`.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-117">In hello following example, hello subscription name is `ContosoSubscription`.</span></span>

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

<span data-ttu-id="dc0d2-118">Než budete moct použít prostředí Windows PowerShell s Azure Resource Manager, je třeba hello následující:</span><span class="sxs-lookup"><span data-stu-id="dc0d2-118">Before you can use Windows PowerShell with Azure Resource Manager, you need hello following:</span></span>

* <span data-ttu-id="dc0d2-119">Prostředí Windows PowerShell, verze 3.0 nebo 4.0.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-119">Windows PowerShell, Version 3.0 or 4.0.</span></span> <span data-ttu-id="dc0d2-120">toofind hello verzi Windows PowerShell, zadejte:`$PSVersionTable` a ověřte hodnotu hello `PSVersion` je 3.0 nebo 4.0.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-120">toofind hello version of Windows PowerShell, type:`$PSVersionTable` and verify hello value of `PSVersion` is 3.0 or 4.0.</span></span> <span data-ttu-id="dc0d2-121">tooinstall kompatibilní verze, najdete v části [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) nebo [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-121">tooinstall a compatible version, see [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) or [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).</span></span>

<span data-ttu-id="dc0d2-122">tooget podrobnou nápovědu pro všechny rutiny, které se zobrazí v tomto kurzu, použijte rutinu Get-Help hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-122">tooget detailed help for any cmdlet you see in this tutorial, use hello Get-Help cmdlet.</span></span>

    Get-Help <cmdlet-name> -Detailed

<span data-ttu-id="dc0d2-123">Například tooget nápovědu pro hello `New-AzureRmRedisCache` rutiny, zadejte:</span><span class="sxs-lookup"><span data-stu-id="dc0d2-123">For example, tooget help for hello `New-AzureRmRedisCache` cmdlet, type:</span></span>

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-tooconnect-tooother-clouds"></a><span data-ttu-id="dc0d2-124">Jak tooconnect tooother cloudy</span><span class="sxs-lookup"><span data-stu-id="dc0d2-124">How tooconnect tooother clouds</span></span>
<span data-ttu-id="dc0d2-125">Ve výchozím nastavení hello Azure je prostředí `AzureCloud`, což představuje hello instance globální cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-125">By default hello Azure environment is `AzureCloud`, which represents hello global Azure cloud instance.</span></span> <span data-ttu-id="dc0d2-126">tooconnect tooa jinou instanci, použijte hello `Add-AzureRmAccount` s hello `-Environment` nebo -`EnvironmentName` přepínač příkazového řádku s názvem prostředí nebo hello požadované prostředí.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-126">tooconnect tooa different instance, use hello `Add-AzureRmAccount` command with hello `-Environment` or -`EnvironmentName` command line switch with hello desired environment or environment name.</span></span>

<span data-ttu-id="dc0d2-127">toosee hello seznam dostupných prostředí, spusťte hello `Get-AzureRmEnvironment` rutiny.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-127">toosee hello list of available environments, run hello `Get-AzureRmEnvironment` cmdlet.</span></span>

### <a name="tooconnect-toohello-azure-government-cloud"></a><span data-ttu-id="dc0d2-128">tooconnect toohello Cloud vlády Azure</span><span class="sxs-lookup"><span data-stu-id="dc0d2-128">tooconnect toohello Azure Government Cloud</span></span>
<span data-ttu-id="dc0d2-129">tooconnect toohello Azure Cloud vlády, použijte jednu z následujících příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-129">tooconnect toohello Azure Government Cloud, use one of hello following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

<span data-ttu-id="dc0d2-130">nebo</span><span class="sxs-lookup"><span data-stu-id="dc0d2-130">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

<span data-ttu-id="dc0d2-131">toocreate mezipaměti hello Azure Cloud vlády, použijte jednu z následujících umístění hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-131">toocreate a cache in hello Azure Government Cloud, use one of hello following locations.</span></span>

* <span data-ttu-id="dc0d2-132">Vláda USA Virginia</span><span class="sxs-lookup"><span data-stu-id="dc0d2-132">USGov Virginia</span></span>
* <span data-ttu-id="dc0d2-133">Iowa vláda USA</span><span class="sxs-lookup"><span data-stu-id="dc0d2-133">USGov Iowa</span></span>

<span data-ttu-id="dc0d2-134">Další informace o hello Cloud vlády Azure najdete v tématu [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) a [Průvodce pro vývojáře k Microsoft Azure Government](../azure-government-developer-guide.md).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-134">For more information about hello Azure Government Cloud, see [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) and [Microsoft Azure Government Developer Guide](../azure-government-developer-guide.md).</span></span>

### <a name="tooconnect-toohello-azure-china-cloud"></a><span data-ttu-id="dc0d2-135">tooconnect toohello Číně cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="dc0d2-135">tooconnect toohello Azure China Cloud</span></span>
<span data-ttu-id="dc0d2-136">tooconnect toohello Číně cloudu Azure, použijte jednu z následujících příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-136">tooconnect toohello Azure China Cloud, use one of hello following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

<span data-ttu-id="dc0d2-137">nebo</span><span class="sxs-lookup"><span data-stu-id="dc0d2-137">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

<span data-ttu-id="dc0d2-138">toocreate mezipaměti hello Číně cloudu Azure, použijte jednu z následujících umístění hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-138">toocreate a cache in hello Azure China Cloud, use one of hello following locations.</span></span>

* <span data-ttu-id="dc0d2-139">Čína – východ</span><span class="sxs-lookup"><span data-stu-id="dc0d2-139">China East</span></span>
* <span data-ttu-id="dc0d2-140">Čína – sever</span><span class="sxs-lookup"><span data-stu-id="dc0d2-140">China North</span></span>

<span data-ttu-id="dc0d2-141">Další informace o hello Číně cloudu Azure najdete v tématu [AzureChinaCloud pro Azure provozované v Číně společností 21Vianet](http://www.windowsazure.cn/).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-141">For more information about hello Azure China Cloud, see [AzureChinaCloud for Azure operated by 21Vianet in China](http://www.windowsazure.cn/).</span></span>

### <a name="tooconnect-toomicrosoft-azure-germany"></a><span data-ttu-id="dc0d2-142">tooconnect tooMicrosoft Azure v Německu</span><span class="sxs-lookup"><span data-stu-id="dc0d2-142">tooconnect tooMicrosoft Azure Germany</span></span>
<span data-ttu-id="dc0d2-143">tooconnect tooMicrosoft Azure v Německu, použijte jednu z následujících příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-143">tooconnect tooMicrosoft Azure Germany, use one of hello following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureGermanCloud


<span data-ttu-id="dc0d2-144">nebo</span><span class="sxs-lookup"><span data-stu-id="dc0d2-144">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

<span data-ttu-id="dc0d2-145">toocreate mezipaměti v Microsoft Azure v Německu, použijte jednu z následujících umístění hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-145">toocreate a cache in Microsoft Azure Germany, use one of hello following locations.</span></span>

* <span data-ttu-id="dc0d2-146">Německo – střed</span><span class="sxs-lookup"><span data-stu-id="dc0d2-146">Germany Central</span></span>
* <span data-ttu-id="dc0d2-147">Německo – severovýchod</span><span class="sxs-lookup"><span data-stu-id="dc0d2-147">Germany Northeast</span></span>

<span data-ttu-id="dc0d2-148">Další informace o Microsoft Azure v Německu najdete v tématu [Microsoft Azure v Německu](https://azure.microsoft.com/overview/clouds/germany/).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-148">For more information about Microsoft Azure Germany, see [Microsoft Azure Germany](https://azure.microsoft.com/overview/clouds/germany/).</span></span>

### <a name="properties-used-for-azure-redis-cache-powershell"></a><span data-ttu-id="dc0d2-149">Vlastnosti používané pro Azure Redis Cache prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="dc0d2-149">Properties used for Azure Redis Cache PowerShell</span></span>
<span data-ttu-id="dc0d2-150">Hello následující tabulka obsahuje vlastnosti a popisy pro běžně používané parametry při vytváření a správě vaší instance služby Azure Redis Cache pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-150">hello following table contains properties and descriptions for commonly used parameters when creating and managing your Azure Redis Cache instances using Azure PowerShell.</span></span>

| <span data-ttu-id="dc0d2-151">Parametr</span><span class="sxs-lookup"><span data-stu-id="dc0d2-151">Parameter</span></span> | <span data-ttu-id="dc0d2-152">Popis</span><span class="sxs-lookup"><span data-stu-id="dc0d2-152">Description</span></span> | <span data-ttu-id="dc0d2-153">Výchozí</span><span class="sxs-lookup"><span data-stu-id="dc0d2-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dc0d2-154">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="dc0d2-154">Name</span></span> |<span data-ttu-id="dc0d2-155">Název mezipaměti hello</span><span class="sxs-lookup"><span data-stu-id="dc0d2-155">Name of hello cache</span></span> | |
| <span data-ttu-id="dc0d2-156">Umístění</span><span class="sxs-lookup"><span data-stu-id="dc0d2-156">Location</span></span> |<span data-ttu-id="dc0d2-157">Umístění mezipaměti hello</span><span class="sxs-lookup"><span data-stu-id="dc0d2-157">Location of hello cache</span></span> | |
| <span data-ttu-id="dc0d2-158">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="dc0d2-158">ResourceGroupName</span></span> |<span data-ttu-id="dc0d2-159">Název skupiny prostředků v mezipaměti které toocreate hello</span><span class="sxs-lookup"><span data-stu-id="dc0d2-159">Resource group name in which toocreate hello cache</span></span> | |
| <span data-ttu-id="dc0d2-160">Velikost</span><span class="sxs-lookup"><span data-stu-id="dc0d2-160">Size</span></span> |<span data-ttu-id="dc0d2-161">Hello velikost mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-161">hello size of hello cache.</span></span> <span data-ttu-id="dc0d2-162">Platné hodnoty jsou: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB</span><span class="sxs-lookup"><span data-stu-id="dc0d2-162">Valid values are: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB</span></span> |<span data-ttu-id="dc0d2-163">1GB</span><span class="sxs-lookup"><span data-stu-id="dc0d2-163">1GB</span></span> |
| <span data-ttu-id="dc0d2-164">ShardCount</span><span class="sxs-lookup"><span data-stu-id="dc0d2-164">ShardCount</span></span> |<span data-ttu-id="dc0d2-165">Hello počet horizontálních oddílů toocreate při vytváření cache ve verzi premium s povoleným clusteringem.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-165">hello number of shards toocreate when creating a premium cache with clustering enabled.</span></span> <span data-ttu-id="dc0d2-166">Platné hodnoty jsou: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10</span><span class="sxs-lookup"><span data-stu-id="dc0d2-166">Valid values are: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10</span></span> | |
| <span data-ttu-id="dc0d2-167">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="dc0d2-167">SKU</span></span> |<span data-ttu-id="dc0d2-168">Určuje hello SKU hello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-168">Specifies hello SKU of hello cache.</span></span> <span data-ttu-id="dc0d2-169">Platné hodnoty jsou: Basic, Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="dc0d2-169">Valid values are: Basic, Standard, Premium</span></span> |<span data-ttu-id="dc0d2-170">Standard</span><span class="sxs-lookup"><span data-stu-id="dc0d2-170">Standard</span></span> |
| <span data-ttu-id="dc0d2-171">RedisConfiguration</span><span class="sxs-lookup"><span data-stu-id="dc0d2-171">RedisConfiguration</span></span> |<span data-ttu-id="dc0d2-172">Určuje nastavení konfigurace Redis.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-172">Specifies Redis configuration settings.</span></span> <span data-ttu-id="dc0d2-173">Podrobnosti o jednotlivých nastaveních najdete v tématu hello následující [RedisConfiguration vlastnosti](#redisconfiguration-properties) tabulky.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-173">For details on each setting, see hello following [RedisConfiguration properties](#redisconfiguration-properties) table.</span></span> | |
| <span data-ttu-id="dc0d2-174">EnableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="dc0d2-174">EnableNonSslPort</span></span> |<span data-ttu-id="dc0d2-175">Určuje, zda je povoleno port bez SSL hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-175">Indicates whether hello non-SSL port is enabled.</span></span> |<span data-ttu-id="dc0d2-176">False</span><span class="sxs-lookup"><span data-stu-id="dc0d2-176">False</span></span> |
| <span data-ttu-id="dc0d2-177">MaxMemoryPolicy</span><span class="sxs-lookup"><span data-stu-id="dc0d2-177">MaxMemoryPolicy</span></span> |<span data-ttu-id="dc0d2-178">Tento parametr se již nepoužívá – místo toho použijte RedisConfiguration.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-178">This parameter has been deprecated - use RedisConfiguration instead.</span></span> | |
| <span data-ttu-id="dc0d2-179">StaticIP</span><span class="sxs-lookup"><span data-stu-id="dc0d2-179">StaticIP</span></span> |<span data-ttu-id="dc0d2-180">Při hostování vaší mezipaměti ve virtuální síti, určuje jedinečnou IP adresu v podsíti hello hello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-180">When hosting your cache in a VNET, specifies a unique IP address in hello subnet for hello cache.</span></span> <span data-ttu-id="dc0d2-181">Pokud není zadaná, jeden z podsítě hello vybrali za vás.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-181">If not provided, one is chosen for you from hello subnet.</span></span> | |
| <span data-ttu-id="dc0d2-182">Podsíť</span><span class="sxs-lookup"><span data-stu-id="dc0d2-182">Subnet</span></span> |<span data-ttu-id="dc0d2-183">Při hostování vaší mezipaměti ve virtuální síti, určuje název hello hello podsítě v mezipaměti které toodeploy hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-183">When hosting your cache in a VNET, specifies hello name of hello subnet in which toodeploy hello cache.</span></span> | |
| <span data-ttu-id="dc0d2-184">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="dc0d2-184">VirtualNetwork</span></span> |<span data-ttu-id="dc0d2-185">Při hostování vaší mezipaměti ve virtuální síti, určuje ID prostředku hello hello virtuální síť, ve které toodeploy hello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-185">When hosting your cache in a VNET, specifies hello resource ID of hello VNET in which toodeploy hello cache.</span></span> | |
| <span data-ttu-id="dc0d2-186">Typ_klíče.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-186">KeyType</span></span> |<span data-ttu-id="dc0d2-187">Určuje, které přístupový klíč tooregenerate při obnovování přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-187">Specifies which access key tooregenerate when renewing access keys.</span></span> <span data-ttu-id="dc0d2-188">Platné hodnoty jsou: primární, sekundární</span><span class="sxs-lookup"><span data-stu-id="dc0d2-188">Valid values are: Primary, Secondary</span></span> | |

### <a name="redisconfiguration-properties"></a><span data-ttu-id="dc0d2-189">Vlastnosti RedisConfiguration</span><span class="sxs-lookup"><span data-stu-id="dc0d2-189">RedisConfiguration properties</span></span>
| <span data-ttu-id="dc0d2-190">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="dc0d2-190">Property</span></span> | <span data-ttu-id="dc0d2-191">Popis</span><span class="sxs-lookup"><span data-stu-id="dc0d2-191">Description</span></span> | <span data-ttu-id="dc0d2-192">Cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="dc0d2-192">Pricing tiers</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dc0d2-193">Povolit zálohování RDB</span><span class="sxs-lookup"><span data-stu-id="dc0d2-193">rdb-backup-enabled</span></span> |<span data-ttu-id="dc0d2-194">Jestli [trvalosti dat Redis](cache-how-to-premium-persistence.md) je povoleno</span><span class="sxs-lookup"><span data-stu-id="dc0d2-194">Whether [Redis data persistence](cache-how-to-premium-persistence.md) is enabled</span></span> |<span data-ttu-id="dc0d2-195">Pouze Premium</span><span class="sxs-lookup"><span data-stu-id="dc0d2-195">Premium only</span></span> |
| <span data-ttu-id="dc0d2-196">RDB úložiště připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="dc0d2-196">rdb-storage-connection-string</span></span> |<span data-ttu-id="dc0d2-197">Hello připojovací řetězec toohello účet úložiště pro [trvalosti dat Redis](cache-how-to-premium-persistence.md)</span><span class="sxs-lookup"><span data-stu-id="dc0d2-197">hello connection string toohello storage account for [Redis data persistence](cache-how-to-premium-persistence.md)</span></span> |<span data-ttu-id="dc0d2-198">Pouze Premium</span><span class="sxs-lookup"><span data-stu-id="dc0d2-198">Premium only</span></span> |
| <span data-ttu-id="dc0d2-199">četnost záloh RDB</span><span class="sxs-lookup"><span data-stu-id="dc0d2-199">rdb-backup-frequency</span></span> |<span data-ttu-id="dc0d2-200">Hello četnost záloh pro [trvalosti dat Redis](cache-how-to-premium-persistence.md)</span><span class="sxs-lookup"><span data-stu-id="dc0d2-200">hello backup frequency for [Redis data persistence](cache-how-to-premium-persistence.md)</span></span> |<span data-ttu-id="dc0d2-201">Pouze Premium</span><span class="sxs-lookup"><span data-stu-id="dc0d2-201">Premium only</span></span> |
| <span data-ttu-id="dc0d2-202">vyhrazené maxmemory</span><span class="sxs-lookup"><span data-stu-id="dc0d2-202">maxmemory-reserved</span></span> |<span data-ttu-id="dc0d2-203">Nakonfiguruje hello [paměti vyhrazené](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) pro procesy bez ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="dc0d2-203">Configures hello [memory reserved](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) for non-cache processes</span></span> |<span data-ttu-id="dc0d2-204">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="dc0d2-204">Standard and Premium</span></span> |
| <span data-ttu-id="dc0d2-205">maxmemory zásady</span><span class="sxs-lookup"><span data-stu-id="dc0d2-205">maxmemory-policy</span></span> |<span data-ttu-id="dc0d2-206">Nakonfiguruje hello [zásady vyřazení](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) pro mezipaměť hello</span><span class="sxs-lookup"><span data-stu-id="dc0d2-206">Configures hello [eviction policy](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) for hello cache</span></span> |<span data-ttu-id="dc0d2-207">Všechny cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="dc0d2-207">All pricing tiers</span></span> |
| <span data-ttu-id="dc0d2-208">oznámení události keyspace</span><span class="sxs-lookup"><span data-stu-id="dc0d2-208">notify-keyspace-events</span></span> |<span data-ttu-id="dc0d2-209">Nakonfiguruje [oznámení keyspace](cache-configure.md#keyspace-notifications-advanced-settings)</span><span class="sxs-lookup"><span data-stu-id="dc0d2-209">Configures [keyspace notifications](cache-configure.md#keyspace-notifications-advanced-settings)</span></span> |<span data-ttu-id="dc0d2-210">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="dc0d2-210">Standard and Premium</span></span> |
| <span data-ttu-id="dc0d2-211">Hodnota hash-max-ziplist – položky</span><span class="sxs-lookup"><span data-stu-id="dc0d2-211">hash-max-ziplist-entries</span></span> |<span data-ttu-id="dc0d2-212">Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy</span><span class="sxs-lookup"><span data-stu-id="dc0d2-212">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="dc0d2-213">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="dc0d2-213">Standard and Premium</span></span> |
| <span data-ttu-id="dc0d2-214">max-ziplist hodnota hash</span><span class="sxs-lookup"><span data-stu-id="dc0d2-214">hash-max-ziplist-value</span></span> |<span data-ttu-id="dc0d2-215">Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy</span><span class="sxs-lookup"><span data-stu-id="dc0d2-215">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="dc0d2-216">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="dc0d2-216">Standard and Premium</span></span> |
| <span data-ttu-id="dc0d2-217">set-max-intset – položky</span><span class="sxs-lookup"><span data-stu-id="dc0d2-217">set-max-intset-entries</span></span> |<span data-ttu-id="dc0d2-218">Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy</span><span class="sxs-lookup"><span data-stu-id="dc0d2-218">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="dc0d2-219">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="dc0d2-219">Standard and Premium</span></span> |
| <span data-ttu-id="dc0d2-220">zset-max-ziplist – položky</span><span class="sxs-lookup"><span data-stu-id="dc0d2-220">zset-max-ziplist-entries</span></span> |<span data-ttu-id="dc0d2-221">Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy</span><span class="sxs-lookup"><span data-stu-id="dc0d2-221">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="dc0d2-222">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="dc0d2-222">Standard and Premium</span></span> |
| <span data-ttu-id="dc0d2-223">zset-max-ziplist – hodnota</span><span class="sxs-lookup"><span data-stu-id="dc0d2-223">zset-max-ziplist-value</span></span> |<span data-ttu-id="dc0d2-224">Nakonfiguruje [optimalizace paměti](http://redis.io/topics/memory-optimization) pro malé agregační datové typy</span><span class="sxs-lookup"><span data-stu-id="dc0d2-224">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="dc0d2-225">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="dc0d2-225">Standard and Premium</span></span> |
| <span data-ttu-id="dc0d2-226">databáze</span><span class="sxs-lookup"><span data-stu-id="dc0d2-226">databases</span></span> |<span data-ttu-id="dc0d2-227">Nakonfiguruje hello počet databází.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-227">Configures hello number of databases.</span></span> <span data-ttu-id="dc0d2-228">Tuto vlastnost lze nastavit pouze při vytváření mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-228">This property can be configured only at cache creation.</span></span> |<span data-ttu-id="dc0d2-229">Standard a Premium</span><span class="sxs-lookup"><span data-stu-id="dc0d2-229">Standard and Premium</span></span> |

## <a name="toocreate-a-redis-cache"></a><span data-ttu-id="dc0d2-230">toocreate Redis Cache</span><span class="sxs-lookup"><span data-stu-id="dc0d2-230">toocreate a Redis Cache</span></span>
<span data-ttu-id="dc0d2-231">Nové instance služby Azure Redis Cache lze vytvořit pomocí hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-231">New Azure Redis Cache instances are created using hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dc0d2-232">Hello prvním vytvoření mezipaměti Redis v předplatném pomocí hello portál Azure, portál hello zaregistruje hello `Microsoft.Cache` obor názvů pro toto předplatné.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-232">hello first time you create a Redis cache in a subscription using hello Azure portal, hello portal registers hello `Microsoft.Cache` namespace for that subscription.</span></span> <span data-ttu-id="dc0d2-233">Pokud se pokusíte toocreate hello nejprve Redis cache v předplatném pomocí prostředí PowerShell, nejprve je nutné zaregistrovat tento obor názvů pomocí hello následující příkaz; jinak rutin, jako `New-AzureRmRedisCache` a `Get-AzureRmRedisCache` nezdaří.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-233">If you attempt toocreate hello first Redis cache in a subscription using PowerShell, you must first register that namespace using hello following command; otherwise cmdlets such as `New-AzureRmRedisCache` and `Get-AzureRmRedisCache` fail.</span></span>
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

<span data-ttu-id="dc0d2-234">toosee seznam dostupných parametrů a jejich popisy pro `New-AzureRmRedisCache`spusťte hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-234">toosee a list of available parameters and their descriptions for `New-AzureRmRedisCache`, run hello following command.</span></span>

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
        hello New-AzureRmRedisCache cmdlet creates a new redis cache.


    PARAMETERS
        -Name <String>
            Name of hello redis cache toocreate.

        -ResourceGroupName <String>
            Name of resource group in which toocreate hello redis cache.

        -Location <String>
            Location in which toocreate hello redis cache.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of hello redis cache. hello default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. hello default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            hello 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting tooset
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, hello default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            hello number of shards toocreate on a Premium Cluster Cache.

        -VirtualNetwork <String>
            hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="dc0d2-235">toocreate mezipaměti s výchozími parametry, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-235">toocreate a cache with default parameters, run hello following command.</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

<span data-ttu-id="dc0d2-236">`ResourceGroupName`, `Name`, a `Location` jsou požadované parametry, ale hello rest jsou volitelné a mají výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-236">`ResourceGroupName`, `Name`, and `Location` are required parameters, but hello rest are optional and have default values.</span></span> <span data-ttu-id="dc0d2-237">Předchozí příkaz hello vytvoří instanci standardní SKU Azure Redis Cache s hello zadaný název, umístění a skupinu prostředků, který je 1 GB velikost port bez SSL hello zakázána.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-237">Running hello previous command creates a Standard SKU Azure Redis Cache instance with hello specified name, location, and resource group, that is 1 GB in size with hello non-SSL port disabled.</span></span>

<span data-ttu-id="dc0d2-238">velikost P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), zadejte toocreate cache ve verzi premium P3 (26 GB - 260 GB), nebo P4 (53 GB - 530 GB).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-238">toocreate a premium cache, specify a size of P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), or P4 (53 GB - 530 GB).</span></span> <span data-ttu-id="dc0d2-239">tooenable clustering, zadat počet horizontálních pomocí hello `ShardCount` parametr.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-239">tooenable clustering, specify a shard count using hello `ShardCount` parameter.</span></span> <span data-ttu-id="dc0d2-240">Hello následující příklad vytvoří mezipaměť premium P1 s 3 horizontálními oddíly.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-240">hello following example creates a P1 premium cache with 3 shards.</span></span> <span data-ttu-id="dc0d2-241">Mezipaměť premium P1 je 6 GB velikost a vzhledem k tomu, že jsme zadali, že tři horizontálních oddílů hello celková velikost je 18 GB (3 × 6 GB).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-241">A P1 premium cache is 6 GB in size, and since we specified three shards hello total size is 18 GB (3 x 6 GB).</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

<span data-ttu-id="dc0d2-242">toospecify hodnoty pro hello `RedisConfiguration` parametr, uzavřete hello hodnoty uvnitř `{}` jako klíč/hodnota, jako páry `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-242">toospecify values for hello `RedisConfiguration` parameter, enclose hello values inside `{}` as key/value pairs like `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`.</span></span> <span data-ttu-id="dc0d2-243">Hello následující příklad vytvoří standardní 1 GB mezipaměti s `allkeys-random` maxmemory zásady a keyspace oznámení nakonfigurované s `KEA`.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-243">hello following example creates a standard 1 GB cache with `allkeys-random` maxmemory policy and keyspace notifications configured with `KEA`.</span></span> <span data-ttu-id="dc0d2-244">Další informace najdete v tématu [oznámení Keyspace (rozšířené nastavení)](cache-configure.md#keyspace-notifications-advanced-settings) a [paměti zásady](cache-configure.md#memory-policies).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-244">For more information, see [Keyspace notifications (advanced settings)](cache-configure.md#keyspace-notifications-advanced-settings) and [Memory policies](cache-configure.md#memory-policies).</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="tooconfigure-hello-databases-setting-during-cache-creation"></a><span data-ttu-id="dc0d2-245">databáze hello tooconfigure nastavení v průběhu vytvoření mezipaměti</span><span class="sxs-lookup"><span data-stu-id="dc0d2-245">tooconfigure hello databases setting during cache creation</span></span>
<span data-ttu-id="dc0d2-246">Hello `databases` nastavení se dá nakonfigurovat jenom během vytváření mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-246">hello `databases` setting can be configured only during cache creation.</span></span> <span data-ttu-id="dc0d2-247">Hello následující příklad vytvoří premium P3 (26 GB) mezipaměti s 48 databází pomocí hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-247">hello following example creates a premium P3 (26 GB) cache with 48 databases using hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

<span data-ttu-id="dc0d2-248">Další informace o hello `databases` vlastnost, najdete v části [konfigurace serveru výchozí Azure Redis Cache](cache-configure.md#default-redis-server-configuration).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-248">For more information on hello `databases` property, see [Default Azure Redis Cache server configuration](cache-configure.md#default-redis-server-configuration).</span></span> <span data-ttu-id="dc0d2-249">Další informace o vytvoření mezipaměti pomocí hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) rutiny, najdete v části hello předchozí [toocreate Redis Cache](#to-create-a-redis-cache) části.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-249">For more information on creating a cache using hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet, see hello previous [toocreate a Redis Cache](#to-create-a-redis-cache) section.</span></span>

## <a name="tooupdate-a-redis-cache"></a><span data-ttu-id="dc0d2-250">tooupdate mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="dc0d2-250">tooupdate a Redis cache</span></span>
<span data-ttu-id="dc0d2-251">Instance služby Azure Redis Cache jsou aktualizovány pomocí hello [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-251">Azure Redis Cache instances are updated using hello [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet.</span></span>

<span data-ttu-id="dc0d2-252">toosee seznam dostupných parametrů a jejich popisy pro `Set-AzureRmRedisCache`spusťte hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-252">toosee a list of available parameters and their descriptions for `Set-AzureRmRedisCache`, run hello following command.</span></span>

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
        hello Set-AzureRmRedisCache cmdlet sets redis cache parameters.

    PARAMETERS
        -Name <String>
            Name of hello redis cache tooupdate.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        -Size <String>
            Size of hello redis cache. hello default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. hello default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            hello 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting tooset
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. hello default value is null and no change will be made toothe
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            hello number of shards toocreate on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="dc0d2-253">Hello `Set-AzureRmRedisCache` rutiny lze použít tooupdate vlastnosti, jako `Size`, `Sku`, `EnableNonSslPort`a hello `RedisConfiguration` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-253">hello `Set-AzureRmRedisCache` cmdlet can be used tooupdate properties such as `Size`, `Sku`, `EnableNonSslPort`, and hello `RedisConfiguration` values.</span></span> 

<span data-ttu-id="dc0d2-254">Hello následující příkaz, že aktualizace hello maxmemory zásady pro hello Redis Cache s názvem myCache.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-254">hello following command updates hello maxmemory-policy for hello Redis Cache named myCache.</span></span>

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="tooscale-a-redis-cache"></a><span data-ttu-id="dc0d2-255">tooscale mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="dc0d2-255">tooscale a Redis cache</span></span>
<span data-ttu-id="dc0d2-256">`Set-AzureRmRedisCache`může být použité tooscale instance mezipaměti Azure Redis při hello `Size`, `Sku`, nebo `ShardCount` jsou upraveny vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-256">`Set-AzureRmRedisCache` can be used tooscale an Azure Redis cache instance when hello `Size`, `Sku`, or `ShardCount` properties are modified.</span></span> 

> [!NOTE]
> <span data-ttu-id="dc0d2-257">Škálování mezipaměti pomocí prostředí PowerShell je subjektu toohello stejné omezení a pokyny jako škálování mezipaměti z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-257">Scaling a cache using PowerShell is subject toohello same limits and guidelines as scaling a cache from hello Azure portal.</span></span> <span data-ttu-id="dc0d2-258">Je možné škálovat tooa jinou cenovou úroveň s hello následující omezení.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-258">You can scale tooa different pricing tier with hello following restrictions.</span></span>
> 
> * <span data-ttu-id="dc0d2-259">Nelze škálovat z vyšší cenová úroveň tooa nižší cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-259">You can't scale from a higher pricing tier tooa lower pricing tier.</span></span>
> * <span data-ttu-id="dc0d2-260">Nelze škálovat od **Premium** mezipaměti dolů tooa **standardní** nebo **základní** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-260">You can't scale from a **Premium** cache down tooa **Standard** or a **Basic** cache.</span></span>
> * <span data-ttu-id="dc0d2-261">Nelze škálovat od **standardní** mezipaměti dolů tooa **základní** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-261">You can't scale from a **Standard** cache down tooa **Basic** cache.</span></span>
> * <span data-ttu-id="dc0d2-262">Je možné škálovat od **základní** mezipaměti tooa **standardní** mezipaměti, ale nemůže změnit velikost hello v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-262">You can scale from a **Basic** cache tooa **Standard** cache but you can't change hello size at hello same time.</span></span> <span data-ttu-id="dc0d2-263">Pokud potřebujete jinou velikost, můžete provést následné velikost toohello potřeby operace škálování.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-263">If you need a different size, you can do a subsequent scaling operation toohello desired size.</span></span>
> * <span data-ttu-id="dc0d2-264">Nelze škálovat od **základní** mezipaměti přímo tooa **Premium** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-264">You can't scale from a **Basic** cache directly tooa **Premium** cache.</span></span> <span data-ttu-id="dc0d2-265">Musí škálování z **základní** příliš**standardní** v rámci jedné operace škálování a potom z **standardní** příliš**Premium** v následných škálování operace.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-265">You must scale from **Basic** too**Standard** in one scaling operation, and then from **Standard** too**Premium** in a subsequent scaling operation.</span></span>
> * <span data-ttu-id="dc0d2-266">Nelze škálovat z větší velikost dolů toohello **C0 (250 MB)** velikost.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-266">You can't scale from a larger size down toohello **C0 (250 MB)** size.</span></span>
> 
> <span data-ttu-id="dc0d2-267">Další informace najdete v tématu [jak tooScale Azure mezipaměti Redis](cache-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-267">For more information, see [How tooScale Azure Redis Cache](cache-how-to-scale.md).</span></span>
> 
> 

<span data-ttu-id="dc0d2-268">Hello následující příklad ukazuje, jak tooscale mezipaměti s názvem `myCache` tooa 2,5 GB mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-268">hello following example shows how tooscale a cache named `myCache` tooa 2.5 GB cache.</span></span> <span data-ttu-id="dc0d2-269">Všimněte si, že tento příkaz lze použít pro základní nebo standardní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-269">Note that this command works for both a Basic or a Standard cache.</span></span>

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

<span data-ttu-id="dc0d2-270">Po vydání tento příkaz, je vrácen stav hello hello mezipaměti (podobně jako toocalling `Get-AzureRmRedisCache`).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-270">After this command is issued, hello status of hello cache is returned (similar toocalling `Get-AzureRmRedisCache`).</span></span> <span data-ttu-id="dc0d2-271">Všimněte si, že hello `ProvisioningState` je `Scaling`.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-271">Note that hello `ProvisioningState` is `Scaling`.</span></span>

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

<span data-ttu-id="dc0d2-272">Po dokončení operace škálování hello hello `ProvisioningState` změní příliš`Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-272">When hello scaling operation is complete, hello `ProvisioningState` changes too`Succeeded`.</span></span> <span data-ttu-id="dc0d2-273">Pokud potřebujete toomake následné škálování operace, jako je například změna ze základní tooStandard a pak změníte velikost hello musíte počkat, dokud dokončení předchozí operace hello nebo se zobrazí následující toohello podobné k chybě.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-273">If you need toomake a subsequent scaling operation, such as changing from Basic tooStandard and then changing hello size, you must wait until hello previous operation is complete or you receive an error similar toohello following.</span></span>

    Set-AzureRmRedisCache : Conflict: hello resource '...' is not in a stable state, and is currently unable tooaccept hello update request.

## <a name="tooget-information-about-a-redis-cache"></a><span data-ttu-id="dc0d2-274">tooget informace o mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="dc0d2-274">tooget information about a Redis cache</span></span>
<span data-ttu-id="dc0d2-275">Můžete načíst informace o mezipaměti pomocí hello [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-275">You can retrieve information about a cache using hello [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) cmdlet.</span></span>

<span data-ttu-id="dc0d2-276">toosee seznam dostupných parametrů a jejich popisy pro `Get-AzureRmRedisCache`spusťte hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-276">toosee a list of available parameters and their descriptions for `Get-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed

    NAME
        Get-AzureRmRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in hello specified resource group or all caches in hello current
        subscription.

    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        hello Get-AzureRmRedisCache cmdlet gets hello details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in hello specified resource group.

        If no parameters are given than it will return details about all caches hello current subscription.

    PARAMETERS
        -Name <String>
            hello name of hello cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns hello details for hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns hello details of hello cache specified by Name. If only hello ResourceGroup
            parameter is provided, then details for all caches in hello resource group are returned.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="dc0d2-277">Spustit tooreturn informace o všech mezipaměti v aktuálním předplatném hello, `Get-AzureRmRedisCache` bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-277">tooreturn information about all caches in hello current subscription, run `Get-AzureRmRedisCache` without any parameters.</span></span>

    Get-AzureRmRedisCache

<span data-ttu-id="dc0d2-278">Spustit tooreturn informace o všech mezipamětí ve skupině prostředků konkrétní `Get-AzureRmRedisCache` s hello `ResourceGroupName` parametr.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-278">tooreturn information about all caches in a specific resource group, run `Get-AzureRmRedisCache` with hello `ResourceGroupName` parameter.</span></span>

    Get-AzureRmRedisCache -ResourceGroupName myGroup

<span data-ttu-id="dc0d2-279">Spustit tooreturn informace o konkrétní mezipaměti, `Get-AzureRmRedisCache` s hello `Name` parametr, který obsahuje název hello hello mezipaměti a hello `ResourceGroupName` parametr s hello skupinu prostředků obsahující mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-279">tooreturn information about a specific cache, run `Get-AzureRmRedisCache` with hello `Name` parameter containing hello name of hello cache, and hello `ResourceGroupName` parameter with hello resource group containing that cache.</span></span>

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

## <a name="tooretrieve-hello-access-keys-for-a-redis-cache"></a><span data-ttu-id="dc0d2-280">tooretrieve hello přístupové klíče pro Redis cache</span><span class="sxs-lookup"><span data-stu-id="dc0d2-280">tooretrieve hello access keys for a Redis cache</span></span>
<span data-ttu-id="dc0d2-281">tooretrieve hello přístupové klíče pro mezipaměť, můžete použít hello [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-281">tooretrieve hello access keys for your cache, you can use hello [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) cmdlet.</span></span>

<span data-ttu-id="dc0d2-282">toosee seznam dostupných parametrů a jejich popisy pro `Get-AzureRmRedisCacheKey`spusťte hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-282">toosee a list of available parameters and their descriptions for `Get-AzureRmRedisCacheKey`, run hello following command.</span></span>

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets hello accesskeys for hello specified redis cache.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        hello Get-AzureRmRedisCacheKey cmdlet gets hello access keys for hello specified cache.

    PARAMETERS
        -Name <String>
            Name of hello redis cache.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="dc0d2-283">tooretrieve hello klíče pro mezipaměť, volání hello `Get-AzureRmRedisCacheKey` rutiny a předejte jí hello název mezipaměti hello název skupiny prostředků hello, který obsahuje hello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-283">tooretrieve hello keys for your cache, call hello `Get-AzureRmRedisCacheKey` cmdlet and pass in hello name of your cache hello name of hello resource group that contains hello cache.</span></span>

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="tooregenerate-access-keys-for-your-redis-cache"></a><span data-ttu-id="dc0d2-284">tooregenerate přístupové klíče pro vaše mezipaměť Redis</span><span class="sxs-lookup"><span data-stu-id="dc0d2-284">tooregenerate access keys for your Redis cache</span></span>
<span data-ttu-id="dc0d2-285">tooregenerate hello přístupové klíče pro mezipaměť, můžete použít hello [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-285">tooregenerate hello access keys for your cache, you can use hello [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) cmdlet.</span></span>

<span data-ttu-id="dc0d2-286">toosee seznam dostupných parametrů a jejich popisy pro `New-AzureRmRedisCacheKey`spusťte hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-286">toosee a list of available parameters and their descriptions for `New-AzureRmRedisCacheKey`, run hello following command.</span></span>

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates hello access key of a redis cache.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        hello New-AzureRmRedisCacheKey cmdlet regenerate hello access key of a redis cache.

    PARAMETERS
        -Name <String>
            Name of hello redis cache.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        -KeyType <String>
            Specifies whether tooregenerate hello primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When hello Force parameter is provided, hello specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="dc0d2-287">tooregenerate hello primární nebo sekundární klíč pro mezipaměť, volání hello `New-AzureRmRedisCacheKey` rutiny a předejte jí hello name, skupinu prostředků a zadejte buď `Primary` nebo `Secondary` pro hello `KeyType` parametr.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-287">tooregenerate hello primary or secondary key for your cache, call hello `New-AzureRmRedisCacheKey` cmdlet and pass in hello name, resource group, and specify either `Primary` or `Secondary` for hello `KeyType` parameter.</span></span> <span data-ttu-id="dc0d2-288">V následujícím příkladu hello hello sekundární přístupový klíč pro mezipaměť je znovu vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-288">In hello following example, hello secondary access key for a cache is regenerated.</span></span>

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want tooregenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="toodelete-a-redis-cache"></a><span data-ttu-id="dc0d2-289">toodelete mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="dc0d2-289">toodelete a Redis cache</span></span>
<span data-ttu-id="dc0d2-290">toodelete mezipaměti Redis, použijte hello [odebrat AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-290">toodelete a Redis cache, use hello [Remove-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) cmdlet.</span></span>

<span data-ttu-id="dc0d2-291">toosee seznam dostupných parametrů a jejich popisy pro `Remove-AzureRmRedisCache`spusťte hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-291">toosee a list of available parameters and their descriptions for `Remove-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove redis cache if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        hello Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.

    PARAMETERS
        -Name <String>
            Name of hello redis cache tooremove.

        -ResourceGroupName <String>
            Name of hello resource group of hello cache tooremove.

        -Force
            When hello Force parameter is provided, hello cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzureRmRedisCache removes hello cache and does not return any value. If hello PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating hello success of hello operatio

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="dc0d2-292">V následujícím příkladu hello, hello mezipaměti s názvem `myCache` se odebere.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-292">In hello following example, hello cache named `myCache` is removed.</span></span>

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want tooremove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="tooimport-a-redis-cache"></a><span data-ttu-id="dc0d2-293">tooimport mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="dc0d2-293">tooimport a Redis cache</span></span>
<span data-ttu-id="dc0d2-294">Data můžete importovat do instanci služby Azure Redis Cache pomocí hello `Import-AzureRmRedisCache` rutiny.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-294">You can import data into an Azure Redis Cache instance using hello `Import-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dc0d2-295">Import a Export je k dispozici pouze [úroveň premium](cache-premium-tier-intro.md) ukládá do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-295">Import/Export is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="dc0d2-296">Další informace o importu a exportu najdete v tématu [importu a exportu dat ve službě Azure Redis Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-296">For more information about Import/Export, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

<span data-ttu-id="dc0d2-297">toosee seznam dostupných parametrů a jejich popisy pro `Import-AzureRmRedisCache`spusťte hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-297">toosee a list of available parameters and their descriptions for `Import-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs tooAzure Redis Cache.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Import-AzureRmRedisCache cmdlet imports data from hello specified blobs into Azure Redis Cache.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into hello cache.

        -Format <String>
            Format for hello blob.  Currently "rdb" is hello only supported, with other formats expected in hello future.

        -Force
            When hello Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If hello PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating hello success of the
            operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="dc0d2-298">Hello následující příkaz importuje data z objektu blob hello určeného identifikátorem hello SAS uri do Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-298">hello following command imports data from hello blob specified by hello SAS uri into Azure Redis Cache.</span></span>

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="tooexport-a-redis-cache"></a><span data-ttu-id="dc0d2-299">tooexport mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="dc0d2-299">tooexport a Redis cache</span></span>
<span data-ttu-id="dc0d2-300">Data můžete exportovat z instance Azure Redis Cache pomocí hello `Export-AzureRmRedisCache` rutiny.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-300">You can export data from an Azure Redis Cache instance using hello `Export-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dc0d2-301">Import a Export je k dispozici pouze [úroveň premium](cache-premium-tier-intro.md) ukládá do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-301">Import/Export is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="dc0d2-302">Další informace o importu a exportu najdete v tématu [importu a exportu dat ve službě Azure Redis Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-302">For more information about Import/Export, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

<span data-ttu-id="dc0d2-303">toosee seznam dostupných parametrů a jejich popisy pro `Export-AzureRmRedisCache`spusťte hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-303">toosee a list of available parameters and their descriptions for `Export-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Redis Cache tooa specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache tooa specified container.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -Prefix <String>
            Prefix toouse for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for hello blob.  Currently "rdb" is hello only supported, with other formats expected in hello future.

        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If hello PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating hello success of hello operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="dc0d2-304">Hello následující příkaz exportuje data z instance Azure Redis Cache do kontejneru hello určeného identifikátor uri SAS hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-304">hello following command exports data from an Azure Redis Cache instance into hello container specified by hello SAS uri.</span></span>

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="tooreboot-a-redis-cache"></a><span data-ttu-id="dc0d2-305">tooreboot mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="dc0d2-305">tooreboot a Redis cache</span></span>
<span data-ttu-id="dc0d2-306">Restartujete instance služby Azure Redis Cache pomocí hello `Reset-AzureRmRedisCache` rutiny.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-306">You can reboot your Azure Redis Cache instance using hello `Reset-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dc0d2-307">Restart je k dispozici pouze [úroveň premium](cache-premium-tier-intro.md) ukládá do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-307">Reboot is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="dc0d2-308">Další informace o restartování mezipaměti najdete v tématu [mezipaměti správy – restartovat](cache-administration.md#reboot).</span><span class="sxs-lookup"><span data-stu-id="dc0d2-308">For more information about rebooting your cache, see [Cache administration - reboot](cache-administration.md#reboot).</span></span>
> 
> 

<span data-ttu-id="dc0d2-309">toosee seznam dostupných parametrů a jejich popisy pro `Reset-AzureRmRedisCache`spusťte hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-309">toosee a list of available parameters and their descriptions for `Reset-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Reset-AzureRmRedisCache cmdlet reboots hello specified node(s) of an Azure Redis Cache instance.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -RebootType <String>
            Which node tooreboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard tooreboot when rebooting a premium cache with clustering enabled.

        -Force
            When hello Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If hello PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating hello success of hello operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="dc0d2-310">Hello následující příkaz restartuje obou uzlech hello zadaný mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-310">hello following command reboots both nodes of hello specified cache.</span></span>

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a><span data-ttu-id="dc0d2-311">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dc0d2-311">Next steps</span></span>
<span data-ttu-id="dc0d2-312">toolearn Další informace o použití prostředí Windows PowerShell s Azure, najdete v části hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="dc0d2-312">toolearn more about using Windows PowerShell with Azure, see hello following resources:</span></span>

* [<span data-ttu-id="dc0d2-313">Azure Redis Cache rutiny dokumentaci na webu MSDN</span><span class="sxs-lookup"><span data-stu-id="dc0d2-313">Azure Redis Cache cmdlet documentation on MSDN</span></span>](https://msdn.microsoft.com/library/azure/mt634513.aspx)
* <span data-ttu-id="dc0d2-314">[Rutiny Azure Resource Manager](http://go.microsoft.com/fwlink/?LinkID=394765): Další toouse hello rutiny v modulu Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-314">[Azure Resource Manager Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): Learn toouse hello cmdlets in hello Azure Resource Manager module.</span></span>
* <span data-ttu-id="dc0d2-315">[Použití prostředků skupiny prostředků Azure toomanage](../azure-resource-manager/resource-group-template-deploy-portal.md): Zjistěte, jak toocreate a spravovat skupiny prostředků v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-315">[Using Resource groups toomanage your Azure resources](../azure-resource-manager/resource-group-template-deploy-portal.md): Learn how toocreate and manage resource groups in hello Azure portal.</span></span>
* <span data-ttu-id="dc0d2-316">[Azure blog](http://blogs.msdn.com/windowsazure): Další informace o nových funkcích v Azure.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-316">[Azure blog](http://blogs.msdn.com/windowsazure): Learn about new features in Azure.</span></span>
* <span data-ttu-id="dc0d2-317">[Blogu o prostředí Windows PowerShell](http://blogs.msdn.com/powershell): Další informace o nové funkce v prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-317">[Windows PowerShell blog](http://blogs.msdn.com/powershell): Learn about new features in Windows PowerShell.</span></span>
* <span data-ttu-id="dc0d2-318">["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): získání reálného tipy a triky z hello komunity prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dc0d2-318">["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): Get real-world tips and tricks from hello Windows PowerShell community.</span></span>

