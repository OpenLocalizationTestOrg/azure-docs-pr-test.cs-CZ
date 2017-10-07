---
title: "aaaAzure Cosmos automatizace DB - Management pomocí prostředí Powershell | Microsoft Docs"
description: "Použití Azure Powershell spravovat své účty Azure Cosmos DB."
services: cosmos-db
author: dmakwana
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: dimakwan
ms.openlocfilehash: 3239fb815918a0e47bff69fcd1ab6562519e429b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a><span data-ttu-id="d6836-103">Vytvoření účtu Azure Cosmos DB pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d6836-103">Create an Azure Cosmos DB account using PowerShell</span></span>

<span data-ttu-id="d6836-104">Hello následující příručka popisuje správu tooautomate příkazy účty Azure Cosmos DB databáze pomocí prostředí Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="d6836-104">hello following guide describes commands tooautomate management of your Azure Cosmos DB database accounts using Azure Powershell.</span></span> <span data-ttu-id="d6836-105">Zahrnuje také klíče účtu toomanage příkazy a priorit převzetí služeb při selhání v [účty databáze více oblast][scaling-globally].</span><span class="sxs-lookup"><span data-stu-id="d6836-105">It also includes commands toomanage account keys and failover priorities in [multi-region database accounts][scaling-globally].</span></span> <span data-ttu-id="d6836-106">Aktualizace databázového účtu vám umožní toomodify konzistence zásady a oblastí, přidat nebo odebrat.</span><span class="sxs-lookup"><span data-stu-id="d6836-106">Updating your database account allows you toomodify consistency policies and add/remove regions.</span></span> <span data-ttu-id="d6836-107">Pro různé platformy správy účtu Azure Cosmos DB, můžete použít buď [rozhraní příkazového řádku Azure](cli-samples.md), hello [rozhraní API REST zprostředkovatele prostředků][rp-rest-api], nebo hello [Azure portál](create-documentdb-dotnet.md#create-account).</span><span class="sxs-lookup"><span data-stu-id="d6836-107">For cross-platform management of your Azure Cosmos DB account, you can use either [Azure CLI](cli-samples.md), hello [Resource Provider REST API][rp-rest-api], or hello [Azure portal](create-documentdb-dotnet.md#create-account).</span></span>

## <a name="getting-started"></a><span data-ttu-id="d6836-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="d6836-108">Getting Started</span></span>

<span data-ttu-id="d6836-109">Postupujte podle pokynů hello v [jak tooinstall a konfigurace prostředí Azure PowerShell] [ powershell-install-configure] tooinstall a přihlaste se tooyour Azure Resource Manager účet v prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="d6836-109">Follow hello instructions in [How tooinstall and configure Azure PowerShell][powershell-install-configure] tooinstall and log in tooyour Azure Resource Manager account in Powershell.</span></span>

### <a name="notes"></a><span data-ttu-id="d6836-110">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d6836-110">Notes</span></span>

* <span data-ttu-id="d6836-111">Pokud chcete tooexecute hello následující příkazy bez nutnosti potvrzení uživatelem, připojit hello `-Force` příznak toohello příkaz.</span><span class="sxs-lookup"><span data-stu-id="d6836-111">If you would like tooexecute hello following commands without requiring user confirmation, append hello `-Force` flag toohello command.</span></span>
* <span data-ttu-id="d6836-112">Všechny hello následující příkazy jsou synchronní.</span><span class="sxs-lookup"><span data-stu-id="d6836-112">All hello following commands are synchronous.</span></span>

## <span data-ttu-id="d6836-113"><a id="create-documentdb-account-powershell"></a>Vytvoření účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d6836-113"><a id="create-documentdb-account-powershell"></a> Create an Azure Cosmos DB Account</span></span>

<span data-ttu-id="d6836-114">Tento příkaz umožňuje toocreate účet Azure Cosmos DB databáze.</span><span class="sxs-lookup"><span data-stu-id="d6836-114">This command allows you toocreate an Azure Cosmos DB database account.</span></span> <span data-ttu-id="d6836-115">Konfigurace nového účtu databáze jako buď jedné oblasti nebo [více oblast] [ scaling-globally] s určitým [konzistence zásad](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="d6836-115">Configure your new database account as either single-region or [multi-region][scaling-globally] with a certain [consistency policy](consistency-levels.md).</span></span>

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* <span data-ttu-id="d6836-116">`<write-region-location>`název umístění Hello hello zápisu oblast hello databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="d6836-116">`<write-region-location>` hello location name of hello write region of hello database account.</span></span> <span data-ttu-id="d6836-117">Toto umístění je požadovaná toohave hodnotu převzetí služeb při selhání s prioritou 0.</span><span class="sxs-lookup"><span data-stu-id="d6836-117">This location is required toohave a failover priority value of 0.</span></span> <span data-ttu-id="d6836-118">Musí mít přesně jeden zápis oblast na databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="d6836-118">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="d6836-119">`<read-region-location>`název umístění Hello hello přečíst oblast hello databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="d6836-119">`<read-region-location>` hello location name of hello read region of hello database account.</span></span> <span data-ttu-id="d6836-120">Toto umístění je požadováno toohave hodnotu převzetí služeb při selhání s prioritou větší než 0.</span><span class="sxs-lookup"><span data-stu-id="d6836-120">This location is required toohave a failover priority value of greater than 0.</span></span> <span data-ttu-id="d6836-121">Může existovat více než jeden pro čtení oblasti za databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="d6836-121">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="d6836-122">`<ip-range-filter>`Určuje sadu hello IP adresy nebo rozsahy IP adres v toobe formuláře CIDR zahrnuty jako hello klienta IP adres pro danou databázi účet seznamu povolených aplikací.</span><span class="sxs-lookup"><span data-stu-id="d6836-122">`<ip-range-filter>` Specifies hello set of IP addresses or IP address ranges in CIDR form toobe included as hello allowed list of client IPs for a given database account.</span></span> <span data-ttu-id="d6836-123">IP adresy a rozsahy musí být oddělené čárkami a nesmí obsahovat žádné mezery.</span><span class="sxs-lookup"><span data-stu-id="d6836-123">IP addresses/ranges must be comma separated and must not contain any spaces.</span></span> <span data-ttu-id="d6836-124">Další informace najdete v tématu [podpora brány Firewall databáze Azure Cosmos](firewall-support.md)</span><span class="sxs-lookup"><span data-stu-id="d6836-124">For more information, see [Azure Cosmos DB Firewall Support](firewall-support.md)</span></span>
* <span data-ttu-id="d6836-125">`<default-consistency-level>`Hello výchozí konzistence úroveň hello účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d6836-125">`<default-consistency-level>` hello default consistency level of hello Azure Cosmos DB account.</span></span> <span data-ttu-id="d6836-126">Další informace najdete v tématu [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="d6836-126">For more information, see [Consistency Levels in Azure Cosmos DB](consistency-levels.md).</span></span>
* <span data-ttu-id="d6836-127">`<max-interval>`Při použití s konzistence typu s ohraničenou Prošlostí, tato hodnota představuje hello čas množství typu prošlostí (v sekundách) tolerovat.</span><span class="sxs-lookup"><span data-stu-id="d6836-127">`<max-interval>` When used with Bounded Staleness consistency, this value represents hello time amount of staleness (in seconds) tolerated.</span></span> <span data-ttu-id="d6836-128">Přijatém rozsahu pro tuto hodnotu je 1 až 100.</span><span class="sxs-lookup"><span data-stu-id="d6836-128">Accepted range for this value is 1 - 100.</span></span>
* <span data-ttu-id="d6836-129">`<max-staleness-prefix>`Při použití s konzistence typu s ohraničenou Prošlostí, tato hodnota představuje hello počet zastaralých požadavků tolerovat.</span><span class="sxs-lookup"><span data-stu-id="d6836-129">`<max-staleness-prefix>` When used with Bounded Staleness consistency, this value represents hello number of stale requests tolerated.</span></span> <span data-ttu-id="d6836-130">Přijatém rozsahu pro tuto hodnotu je 1 – 2 147 483 647.</span><span class="sxs-lookup"><span data-stu-id="d6836-130">Accepted range for this value is 1 – 2,147,483,647.</span></span>
* <span data-ttu-id="d6836-131">`<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.</span><span class="sxs-lookup"><span data-stu-id="d6836-131">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="d6836-132">`<resource-group-location>`umístění Hello hello skupiny prostředků Azure toowhich hello nový Azure Cosmos DB databáze účet patří do.</span><span class="sxs-lookup"><span data-stu-id="d6836-132">`<resource-group-location>` hello location of hello Azure Resource Group toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="d6836-133">`<database-account-name>`Název Hello hello Azure Cosmos DB databáze účet toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="d6836-133">`<database-account-name>` hello name of hello Azure Cosmos DB database account toobe created.</span></span> <span data-ttu-id="d6836-134">Může použít jenom malá písmena, číslice, hello '-' znak a musí být v rozmezí 3 až 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="d6836-134">It can only use lowercase letters, numbers, hello '-' character, and must be between 3 and 50 characters.</span></span>

<span data-ttu-id="d6836-135">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d6836-135">Example:</span></span> 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a><span data-ttu-id="d6836-136">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d6836-136">Notes</span></span>
* <span data-ttu-id="d6836-137">Hello předchozí příklad vytvoří databázový účet se dvěma oblastmi.</span><span class="sxs-lookup"><span data-stu-id="d6836-137">hello preceding example creates a database account with two regions.</span></span> <span data-ttu-id="d6836-138">Je také možné toocreate databázový účet se jedné oblasti (který je určený jako hello zápisu oblasti a mají hodnotu převzetí služeb při selhání s prioritou 0) nebo víc než dvou oblastech.</span><span class="sxs-lookup"><span data-stu-id="d6836-138">It is also possible toocreate a database account with either one region (which is designated as hello write region and have a failover priority value of 0) or more than two regions.</span></span> <span data-ttu-id="d6836-139">Další informace najdete v tématu [účty databáze více oblast][scaling-globally].</span><span class="sxs-lookup"><span data-stu-id="d6836-139">For more information, see [multi-region database accounts][scaling-globally].</span></span>
* <span data-ttu-id="d6836-140">Hello umístění musí být oblasti, ve kterých je Azure Cosmos DB všeobecně dostupná.</span><span class="sxs-lookup"><span data-stu-id="d6836-140">hello locations must be regions in which Azure Cosmos DB is generally available.</span></span> <span data-ttu-id="d6836-141">aktuální seznam oblastí Hello je uvedený na hello [oblasti Azure stránky](https://azure.microsoft.com/regions/#services).</span><span class="sxs-lookup"><span data-stu-id="d6836-141">hello current list of regions is provided on hello [Azure Regions page](https://azure.microsoft.com/regions/#services).</span></span>

## <span data-ttu-id="d6836-142"><a id="update-documentdb-account-powershell"></a>Aktualizace účtu databáze DocumentDB</span><span class="sxs-lookup"><span data-stu-id="d6836-142"><a id="update-documentdb-account-powershell"></a> Update a DocumentDB Database Account</span></span>

<span data-ttu-id="d6836-143">Tento příkaz vám umožní tooupdate vlastnostech účtu databáze Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d6836-143">This command allows you tooupdate your Azure Cosmos DB database account properties.</span></span> <span data-ttu-id="d6836-144">To zahrnuje hello konzistence zásad a hello umístění, které hello databázový účet existuje v.</span><span class="sxs-lookup"><span data-stu-id="d6836-144">This includes hello consistency policy and hello locations which hello database account exists in.</span></span>

> [!NOTE]
> <span data-ttu-id="d6836-145">Tento příkaz vám umožní tooadd a odebrat oblasti, ale neumožňuje toomodify priorit převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="d6836-145">This command allows you tooadd and remove regions but does not allow you toomodify failover priorities.</span></span> <span data-ttu-id="d6836-146">toomodify priorit převzetí služeb při selhání, najdete v části [pod](#modify-failover-priority-powershell).</span><span class="sxs-lookup"><span data-stu-id="d6836-146">toomodify failover priorities, see [below](#modify-failover-priority-powershell).</span></span>

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* <span data-ttu-id="d6836-147">`<write-region-location>`název umístění Hello hello zápisu oblast hello databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="d6836-147">`<write-region-location>` hello location name of hello write region of hello database account.</span></span> <span data-ttu-id="d6836-148">Toto umístění je požadovaná toohave hodnotu převzetí služeb při selhání s prioritou 0.</span><span class="sxs-lookup"><span data-stu-id="d6836-148">This location is required toohave a failover priority value of 0.</span></span> <span data-ttu-id="d6836-149">Musí mít přesně jeden zápis oblast na databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="d6836-149">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="d6836-150">`<read-region-location>`název umístění Hello hello přečíst oblast hello databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="d6836-150">`<read-region-location>` hello location name of hello read region of hello database account.</span></span> <span data-ttu-id="d6836-151">Toto umístění je požadováno toohave hodnotu převzetí služeb při selhání s prioritou větší než 0.</span><span class="sxs-lookup"><span data-stu-id="d6836-151">This location is required toohave a failover priority value of greater than 0.</span></span> <span data-ttu-id="d6836-152">Může existovat více než jeden pro čtení oblasti za databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="d6836-152">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="d6836-153">`<default-consistency-level>`Hello výchozí konzistence úroveň hello účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d6836-153">`<default-consistency-level>` hello default consistency level of hello Azure Cosmos DB account.</span></span> <span data-ttu-id="d6836-154">Další informace najdete v tématu [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="d6836-154">For more information, see [Consistency Levels in Azure Cosmos DB](consistency-levels.md).</span></span>
* <span data-ttu-id="d6836-155">`<ip-range-filter>`Určuje sadu hello IP adresy nebo rozsahy IP adres v toobe formuláře CIDR zahrnuty jako hello klienta IP adres pro danou databázi účet seznamu povolených aplikací.</span><span class="sxs-lookup"><span data-stu-id="d6836-155">`<ip-range-filter>` Specifies hello set of IP addresses or IP address ranges in CIDR form toobe included as hello allowed list of client IPs for a given database account.</span></span> <span data-ttu-id="d6836-156">IP adresy a rozsahy musí být oddělené čárkami a nesmí obsahovat žádné mezery.</span><span class="sxs-lookup"><span data-stu-id="d6836-156">IP addresses/ranges must be comma separated and must not contain any spaces.</span></span> <span data-ttu-id="d6836-157">Další informace najdete v tématu [podpora brány Firewall databáze Azure Cosmos](firewall-support.md)</span><span class="sxs-lookup"><span data-stu-id="d6836-157">For more information, see [Azure Cosmos DB Firewall Support](firewall-support.md)</span></span>
* <span data-ttu-id="d6836-158">`<max-interval>`Při použití s konzistence typu s ohraničenou Prošlostí, tato hodnota představuje hello čas množství typu prošlostí (v sekundách) tolerovat.</span><span class="sxs-lookup"><span data-stu-id="d6836-158">`<max-interval>` When used with Bounded Staleness consistency, this value represents hello time amount of staleness (in seconds) tolerated.</span></span> <span data-ttu-id="d6836-159">Přijatém rozsahu pro tuto hodnotu je 1 až 100.</span><span class="sxs-lookup"><span data-stu-id="d6836-159">Accepted range for this value is 1 - 100.</span></span>
* <span data-ttu-id="d6836-160">`<max-staleness-prefix>`Při použití s konzistence typu s ohraničenou Prošlostí, tato hodnota představuje hello počet zastaralých požadavků tolerovat.</span><span class="sxs-lookup"><span data-stu-id="d6836-160">`<max-staleness-prefix>` When used with Bounded Staleness consistency, this value represents hello number of stale requests tolerated.</span></span> <span data-ttu-id="d6836-161">Přijatém rozsahu pro tuto hodnotu je 1 – 2 147 483 647.</span><span class="sxs-lookup"><span data-stu-id="d6836-161">Accepted range for this value is 1 – 2,147,483,647.</span></span>
* <span data-ttu-id="d6836-162">`<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.</span><span class="sxs-lookup"><span data-stu-id="d6836-162">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="d6836-163">`<resource-group-location>`umístění Hello hello skupiny prostředků Azure toowhich hello nový Azure Cosmos DB databáze účet patří do.</span><span class="sxs-lookup"><span data-stu-id="d6836-163">`<resource-group-location>` hello location of hello Azure Resource Group toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="d6836-164">`<database-account-name>`Název Hello hello Azure Cosmos DB databáze účet toobe aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="d6836-164">`<database-account-name>` hello name of hello Azure Cosmos DB database account toobe updated.</span></span>

<span data-ttu-id="d6836-165">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d6836-165">Example:</span></span> 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <span data-ttu-id="d6836-166"><a id="delete-documentdb-account-powershell"></a>Odstranění účtu databáze DocumentDB</span><span class="sxs-lookup"><span data-stu-id="d6836-166"><a id="delete-documentdb-account-powershell"></a> Delete a DocumentDB Database Account</span></span>

<span data-ttu-id="d6836-167">Tento příkaz umožňuje toodelete existující databáze účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d6836-167">This command allows you toodelete an existing Azure Cosmos DB database account.</span></span>

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* <span data-ttu-id="d6836-168">`<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.</span><span class="sxs-lookup"><span data-stu-id="d6836-168">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="d6836-169">`<database-account-name>`Název Hello hello Azure Cosmos DB databáze účet toobe odstranit.</span><span class="sxs-lookup"><span data-stu-id="d6836-169">`<database-account-name>` hello name of hello Azure Cosmos DB database account toobe deleted.</span></span>

<span data-ttu-id="d6836-170">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d6836-170">Example:</span></span>

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="d6836-171"><a id="get-documentdb-properties-powershell"></a>Získat vlastnosti účtu databáze DocumentDB</span><span class="sxs-lookup"><span data-stu-id="d6836-171"><a id="get-documentdb-properties-powershell"></a> Get Properties of a DocumentDB Database Account</span></span>

<span data-ttu-id="d6836-172">Tento příkaz umožňuje tooget hello vlastnosti existující databáze účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d6836-172">This command allows you tooget hello properties of an existing Azure Cosmos DB database account.</span></span>

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="d6836-173">`<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.</span><span class="sxs-lookup"><span data-stu-id="d6836-173">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="d6836-174">`<database-account-name>`Název Hello hello databázový účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d6836-174">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>

<span data-ttu-id="d6836-175">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d6836-175">Example:</span></span>

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="d6836-176"><a id="update-tags-powershell"></a>Značky aktualizace účtu databáze Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d6836-176"><a id="update-tags-powershell"></a> Update Tags of an Azure Cosmos DB Database Account</span></span>

<span data-ttu-id="d6836-177">Hello následující příklad popisuje jak tooset [značky prostředku Azure] [ azure-resource-tags] pro vaši Azure DB Cosmos databáze účtu.</span><span class="sxs-lookup"><span data-stu-id="d6836-177">hello following example describes how tooset [Azure resource tags][azure-resource-tags] for your Azure Cosmos DB database account.</span></span>

> [!NOTE]
> <span data-ttu-id="d6836-178">Tento příkaz je možné kombinovat s hello vytvořit nebo aktualizovat příkazy připojením hello `-Tags` příznak s hello odpovídající parametr.</span><span class="sxs-lookup"><span data-stu-id="d6836-178">This command can be combined with hello create or update commands by appending hello `-Tags` flag with hello corresponding parameter.</span></span>

<span data-ttu-id="d6836-179">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d6836-179">Example:</span></span>

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <span data-ttu-id="d6836-180"><a id="list-account-keys-powershell"></a>Klíče účtu seznamu</span><span class="sxs-lookup"><span data-stu-id="d6836-180"><a id="list-account-keys-powershell"></a> List Account Keys</span></span>

<span data-ttu-id="d6836-181">Při vytváření účtu Azure Cosmos DB hello služba generuje dva hlavní přístupové klíče, které lze použít pro ověření při přístupu k účtu Azure Cosmos DB hello.</span><span class="sxs-lookup"><span data-stu-id="d6836-181">When you create an Azure Cosmos DB account, hello service generates two master access keys that can be used for authentication when hello Azure Cosmos DB account is accessed.</span></span> <span data-ttu-id="d6836-182">Poskytnutím dvou přístupových klíčů Azure Cosmos DB vám umožní tooregenerate hello klíče bez přerušení tooyour účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d6836-182">By providing two access keys, Azure Cosmos DB enables you tooregenerate hello keys with no interruption tooyour Azure Cosmos DB account.</span></span> <span data-ttu-id="d6836-183">Jen pro čtení klíče pro ověřování operacím jen pro čtení jsou také k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d6836-183">Read-only keys for authenticating read-only operations are also available.</span></span> <span data-ttu-id="d6836-184">(Primární i sekundární) existují dva klíče pro čtení a zápis (primární i sekundární) a dva klíče jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="d6836-184">There are two read-write keys (primary and secondary) and two read-only keys (primary and secondary).</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="d6836-185">`<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.</span><span class="sxs-lookup"><span data-stu-id="d6836-185">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="d6836-186">`<database-account-name>`Název Hello hello databázový účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d6836-186">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>

<span data-ttu-id="d6836-187">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d6836-187">Example:</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="d6836-188"><a id="list-connection-strings-powershell"></a>Seznamu připojovacích řetězců</span><span class="sxs-lookup"><span data-stu-id="d6836-188"><a id="list-connection-strings-powershell"></a> List Connection Strings</span></span>

<span data-ttu-id="d6836-189">Pro účty MongoDB hello tooconnect řetězec připojení, které MongoDB aplikace toohello databázový účet se dá načíst pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="d6836-189">For MongoDB accounts, hello connection string tooconnect your MongoDB app toohello database account can be retrieved using hello following command.</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="d6836-190">`<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.</span><span class="sxs-lookup"><span data-stu-id="d6836-190">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="d6836-191">`<database-account-name>`Název Hello hello databázový účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d6836-191">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>

<span data-ttu-id="d6836-192">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d6836-192">Example:</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="d6836-193"><a id="regenerate-account-key-powershell"></a>Znovu vygenerovat klíč účtu</span><span class="sxs-lookup"><span data-stu-id="d6836-193"><a id="regenerate-account-key-powershell"></a> Regenerate Account Key</span></span>

<span data-ttu-id="d6836-194">Měli byste změnit účet Azure Cosmos DB hello přístupu k klíče tooyour pravidelně toohelp připojení lépe zabezpečit.</span><span class="sxs-lookup"><span data-stu-id="d6836-194">You should change hello access keys tooyour Azure Cosmos DB account periodically toohelp keep your connections more secure.</span></span> <span data-ttu-id="d6836-195">Dva přístupové klíče se přiřazují tooenable hello je toomaintain připojení toohello účet Azure Cosmos DB používat jeden přístupový klíč, zatímco si znovu vygenerujete druhý přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="d6836-195">Two access keys are assigned tooenable you toomaintain connections toohello Azure Cosmos DB account using one access key while you regenerate hello other access key.</span></span>

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* <span data-ttu-id="d6836-196">`<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.</span><span class="sxs-lookup"><span data-stu-id="d6836-196">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="d6836-197">`<database-account-name>`Název Hello hello databázový účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d6836-197">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>
* <span data-ttu-id="d6836-198">`<key-kind>`Jeden z typů hello čtyři klíčů: ["Primární" | " Sekundární "|" PrimaryReadonly "|" SecondaryReadonly"], které chcete tooregenerate.</span><span class="sxs-lookup"><span data-stu-id="d6836-198">`<key-kind>` One of hello four types of keys: ["Primary"|"Secondary"|"PrimaryReadonly"|"SecondaryReadonly"] that you would like tooregenerate.</span></span>

<span data-ttu-id="d6836-199">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d6836-199">Example:</span></span>

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <span data-ttu-id="d6836-200"><a id="modify-failover-priority-powershell"></a>Upravte prioritu převzetí služeb při selhání databáze účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d6836-200"><a id="modify-failover-priority-powershell"></a> Modify Failover Priority of an Azure Cosmos DB Database Account</span></span>

<span data-ttu-id="d6836-201">Pro účty databáze více oblast můžete změnit prioritu převzetí služeb při selhání hello hello v různých oblastech, které hello Azure Cosmos DB databázový účet existuje.</span><span class="sxs-lookup"><span data-stu-id="d6836-201">For multi-region database accounts, you can change hello failover priority of hello various regions which hello Azure Cosmos DB database account exists in.</span></span> <span data-ttu-id="d6836-202">Další informace o převzetí služeb při selhání ve vašem účtu Azure Cosmos DB databáze najdete v tématu [distribuci dat globálně pomocí Azure Cosmos DB][distribute-data-globally].</span><span class="sxs-lookup"><span data-stu-id="d6836-202">For more information on failover in your Azure Cosmos DB database account, see [Distribute data globally with Azure Cosmos DB][distribute-data-globally].</span></span>

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* <span data-ttu-id="d6836-203">`<write-region-location>`název umístění Hello hello zápisu oblast hello databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="d6836-203">`<write-region-location>` hello location name of hello write region of hello database account.</span></span> <span data-ttu-id="d6836-204">Toto umístění je požadovaná toohave hodnotu převzetí služeb při selhání s prioritou 0.</span><span class="sxs-lookup"><span data-stu-id="d6836-204">This location is required toohave a failover priority value of 0.</span></span> <span data-ttu-id="d6836-205">Musí mít přesně jeden zápis oblast na databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="d6836-205">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="d6836-206">`<read-region-location>`název umístění Hello hello přečíst oblast hello databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="d6836-206">`<read-region-location>` hello location name of hello read region of hello database account.</span></span> <span data-ttu-id="d6836-207">Toto umístění je požadováno toohave hodnotu převzetí služeb při selhání s prioritou větší než 0.</span><span class="sxs-lookup"><span data-stu-id="d6836-207">This location is required toohave a failover priority value of greater than 0.</span></span> <span data-ttu-id="d6836-208">Může existovat více než jeden pro čtení oblasti za databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="d6836-208">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="d6836-209">`<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.</span><span class="sxs-lookup"><span data-stu-id="d6836-209">`<resource-group-name>` hello name of hello [Azure Resource Group][azure-resource-groups] toowhich hello new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="d6836-210">`<database-account-name>`Název Hello hello databázový účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d6836-210">`<database-account-name>` hello name of hello Azure Cosmos DB database account.</span></span>

<span data-ttu-id="d6836-211">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d6836-211">Example:</span></span>

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a><span data-ttu-id="d6836-212">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d6836-212">Next steps</span></span>

* <span data-ttu-id="d6836-213">tooconnect pomocí rozhraní .NET, najdete v části [připojit a zadávat dotazy pomocí .NET](create-documentdb-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="d6836-213">tooconnect using .NET, see [Connect and query with .NET](create-documentdb-dotnet.md).</span></span>
* <span data-ttu-id="d6836-214">tooconnect pomocí .NET Core, najdete v části [připojit a zadávat dotazy pomocí .NET Core](create-documentdb-dotnet-core.md).</span><span class="sxs-lookup"><span data-stu-id="d6836-214">tooconnect using .NET Core, see [Connect and query with .NET Core](create-documentdb-dotnet-core.md).</span></span>
* <span data-ttu-id="d6836-215">tooconnect pomocí Node.js, najdete v části [připojit a zadávat dotazy s Node.js a aplikace pro práci s MongoDB](create-mongodb-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="d6836-215">tooconnect using Node.js, see [Connect and query with Node.js and a MongoDB app](create-mongodb-nodejs.md).</span></span>

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/documentdbresourceprovider/
