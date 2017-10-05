---
title: "Automatizace Azure Cosmos DB - Management pomocí prostředí Powershell | Microsoft Docs"
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
ms.openlocfilehash: 25c543528119410dff0684845a713dcb0d6151d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a><span data-ttu-id="81c8f-103">Vytvoření účtu Azure Cosmos DB pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="81c8f-103">Create an Azure Cosmos DB account using PowerShell</span></span>

<span data-ttu-id="81c8f-104">Následující příručka popisuje příkazy, které automatizují správu svých účtů databáze Azure Cosmos DB pomocí Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="81c8f-104">The following guide describes commands to automate management of your Azure Cosmos DB database accounts using Azure Powershell.</span></span> <span data-ttu-id="81c8f-105">Zahrnuje také příkazy pro správu klíčů k účtu a priorit převzetí služeb při selhání v [účty databáze více oblast][scaling-globally].</span><span class="sxs-lookup"><span data-stu-id="81c8f-105">It also includes commands to manage account keys and failover priorities in [multi-region database accounts][scaling-globally].</span></span> <span data-ttu-id="81c8f-106">Aktualizace databázového účtu můžete upravit zásady konzistence a přidat nebo odebrat oblasti.</span><span class="sxs-lookup"><span data-stu-id="81c8f-106">Updating your database account allows you to modify consistency policies and add/remove regions.</span></span> <span data-ttu-id="81c8f-107">Pro různé platformy správy účtu Azure Cosmos DB, můžete použít buď [rozhraní příkazového řádku Azure](cli-samples.md), [rozhraní API REST zprostředkovatele prostředků][rp-rest-api], nebo [portálu Azure ](create-documentdb-dotnet.md#create-account).</span><span class="sxs-lookup"><span data-stu-id="81c8f-107">For cross-platform management of your Azure Cosmos DB account, you can use either [Azure CLI](cli-samples.md), the [Resource Provider REST API][rp-rest-api], or the [Azure portal](create-documentdb-dotnet.md#create-account).</span></span>

## <a name="getting-started"></a><span data-ttu-id="81c8f-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="81c8f-108">Getting Started</span></span>

<span data-ttu-id="81c8f-109">Postupujte podle pokynů v [postup instalace a konfigurace prostředí Azure PowerShell] [ powershell-install-configure] k instalaci a přihlaste se k účtu Azure Resource Manager v prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="81c8f-109">Follow the instructions in [How to install and configure Azure PowerShell][powershell-install-configure] to install and log in to your Azure Resource Manager account in Powershell.</span></span>

### <a name="notes"></a><span data-ttu-id="81c8f-110">Poznámky</span><span class="sxs-lookup"><span data-stu-id="81c8f-110">Notes</span></span>

* <span data-ttu-id="81c8f-111">Pokud chcete spustit následující příkazy bez nutnosti potvrzení uživatelem, připojte `-Force` příznak k příkazu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-111">If you would like to execute the following commands without requiring user confirmation, append the `-Force` flag to the command.</span></span>
* <span data-ttu-id="81c8f-112">Následující příkazy jsou synchronní.</span><span class="sxs-lookup"><span data-stu-id="81c8f-112">All the following commands are synchronous.</span></span>

## <span data-ttu-id="81c8f-113"><a id="create-documentdb-account-powershell"></a>Vytvoření účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="81c8f-113"><a id="create-documentdb-account-powershell"></a> Create an Azure Cosmos DB Account</span></span>

<span data-ttu-id="81c8f-114">Tento příkaz umožňuje vytvoření účtu Azure Cosmos DB databáze.</span><span class="sxs-lookup"><span data-stu-id="81c8f-114">This command allows you to create an Azure Cosmos DB database account.</span></span> <span data-ttu-id="81c8f-115">Konfigurace nového účtu databáze jako buď jedné oblasti nebo [více oblast] [ scaling-globally] s určitým [konzistence zásad](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="81c8f-115">Configure your new database account as either single-region or [multi-region][scaling-globally] with a certain [consistency policy](consistency-levels.md).</span></span>

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* <span data-ttu-id="81c8f-116">`<write-region-location>`Název umístění oblasti zápisu databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-116">`<write-region-location>` The location name of the write region of the database account.</span></span> <span data-ttu-id="81c8f-117">Toto umístění je potřeba mít hodnotu převzetí služeb při selhání s prioritou 0.</span><span class="sxs-lookup"><span data-stu-id="81c8f-117">This location is required to have a failover priority value of 0.</span></span> <span data-ttu-id="81c8f-118">Musí mít přesně jeden zápis oblast na databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-118">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="81c8f-119">`<read-region-location>`Název umístění oblasti čtení databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-119">`<read-region-location>` The location name of the read region of the database account.</span></span> <span data-ttu-id="81c8f-120">Toto umístění je potřeba mít hodnotu převzetí služeb při selhání s prioritou větší než 0.</span><span class="sxs-lookup"><span data-stu-id="81c8f-120">This location is required to have a failover priority value of greater than 0.</span></span> <span data-ttu-id="81c8f-121">Může existovat více než jeden pro čtení oblasti za databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-121">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="81c8f-122">`<ip-range-filter>`Určuje sadu IP adresy nebo rozsahy IP adres ve formátu CIDR zahrnuty jako seznam povolených IP adresy klienta pro účet danou databázi.</span><span class="sxs-lookup"><span data-stu-id="81c8f-122">`<ip-range-filter>` Specifies the set of IP addresses or IP address ranges in CIDR form to be included as the allowed list of client IPs for a given database account.</span></span> <span data-ttu-id="81c8f-123">IP adresy a rozsahy musí být oddělené čárkami a nesmí obsahovat žádné mezery.</span><span class="sxs-lookup"><span data-stu-id="81c8f-123">IP addresses/ranges must be comma separated and must not contain any spaces.</span></span> <span data-ttu-id="81c8f-124">Další informace najdete v tématu [podpora brány Firewall databáze Azure Cosmos](firewall-support.md)</span><span class="sxs-lookup"><span data-stu-id="81c8f-124">For more information, see [Azure Cosmos DB Firewall Support](firewall-support.md)</span></span>
* <span data-ttu-id="81c8f-125">`<default-consistency-level>`Výchozí úroveň konzistence účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-125">`<default-consistency-level>` The default consistency level of the Azure Cosmos DB account.</span></span> <span data-ttu-id="81c8f-126">Další informace najdete v tématu [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="81c8f-126">For more information, see [Consistency Levels in Azure Cosmos DB](consistency-levels.md).</span></span>
* <span data-ttu-id="81c8f-127">`<max-interval>`Při použití s konzistence typu s ohraničenou Prošlostí, tato hodnota představuje čas množství typu prošlostí (v sekundách) tolerovat.</span><span class="sxs-lookup"><span data-stu-id="81c8f-127">`<max-interval>` When used with Bounded Staleness consistency, this value represents the time amount of staleness (in seconds) tolerated.</span></span> <span data-ttu-id="81c8f-128">Přijatém rozsahu pro tuto hodnotu je 1 až 100.</span><span class="sxs-lookup"><span data-stu-id="81c8f-128">Accepted range for this value is 1 - 100.</span></span>
* <span data-ttu-id="81c8f-129">`<max-staleness-prefix>`Při použití s konzistence typu s ohraničenou Prošlostí, tato hodnota představuje počet zastaralých požadavků tolerovat.</span><span class="sxs-lookup"><span data-stu-id="81c8f-129">`<max-staleness-prefix>` When used with Bounded Staleness consistency, this value represents the number of stale requests tolerated.</span></span> <span data-ttu-id="81c8f-130">Přijatém rozsahu pro tuto hodnotu je 1 – 2 147 483 647.</span><span class="sxs-lookup"><span data-stu-id="81c8f-130">Accepted range for this value is 1 – 2,147,483,647.</span></span>
* <span data-ttu-id="81c8f-131">`<resource-group-name>`Název [skupiny prostředků Azure] [ azure-resource-groups] , ke které se nový účet Azure Cosmos DB databáze patří do.</span><span class="sxs-lookup"><span data-stu-id="81c8f-131">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="81c8f-132">`<resource-group-location>`Umístění skupiny prostředků Azure, do které patří nový databázový účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-132">`<resource-group-location>` The location of the Azure Resource Group to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="81c8f-133">`<database-account-name>`Název účtu Azure Cosmos DB databázi vytvořit.</span><span class="sxs-lookup"><span data-stu-id="81c8f-133">`<database-account-name>` The name of the Azure Cosmos DB database account to be created.</span></span> <span data-ttu-id="81c8f-134">Lze použít pouze malá písmena, čísla, '-' znak a musí být v rozmezí 3 až 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="81c8f-134">It can only use lowercase letters, numbers, the '-' character, and must be between 3 and 50 characters.</span></span>

<span data-ttu-id="81c8f-135">Příklad:</span><span class="sxs-lookup"><span data-stu-id="81c8f-135">Example:</span></span> 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a><span data-ttu-id="81c8f-136">Poznámky</span><span class="sxs-lookup"><span data-stu-id="81c8f-136">Notes</span></span>
* <span data-ttu-id="81c8f-137">V předchozím příkladu vytvoří databázový účet se dvěma oblastmi.</span><span class="sxs-lookup"><span data-stu-id="81c8f-137">The preceding example creates a database account with two regions.</span></span> <span data-ttu-id="81c8f-138">Je také možné vytvoření databázového účtu pomocí jedné oblasti (který je určený jako oblasti zápisu a mít hodnotu převzetí služeb při selhání s prioritou 0) nebo víc než dvou oblastech.</span><span class="sxs-lookup"><span data-stu-id="81c8f-138">It is also possible to create a database account with either one region (which is designated as the write region and have a failover priority value of 0) or more than two regions.</span></span> <span data-ttu-id="81c8f-139">Další informace najdete v tématu [účty databáze více oblast][scaling-globally].</span><span class="sxs-lookup"><span data-stu-id="81c8f-139">For more information, see [multi-region database accounts][scaling-globally].</span></span>
* <span data-ttu-id="81c8f-140">Umístění musí být oblasti, ve kterých je Azure Cosmos DB všeobecně dostupná.</span><span class="sxs-lookup"><span data-stu-id="81c8f-140">The locations must be regions in which Azure Cosmos DB is generally available.</span></span> <span data-ttu-id="81c8f-141">Aktuální seznam oblastí je uvedený na [oblasti Azure stránky](https://azure.microsoft.com/regions/#services).</span><span class="sxs-lookup"><span data-stu-id="81c8f-141">The current list of regions is provided on the [Azure Regions page](https://azure.microsoft.com/regions/#services).</span></span>

## <span data-ttu-id="81c8f-142"><a id="update-documentdb-account-powershell"></a>Aktualizace účtu databáze DocumentDB</span><span class="sxs-lookup"><span data-stu-id="81c8f-142"><a id="update-documentdb-account-powershell"></a> Update a DocumentDB Database Account</span></span>

<span data-ttu-id="81c8f-143">Tento příkaz umožňuje aktualizovat vlastnostech účtu databáze Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-143">This command allows you to update your Azure Cosmos DB database account properties.</span></span> <span data-ttu-id="81c8f-144">To zahrnuje konzistence zásad a které databázový účet existuje v umístění.</span><span class="sxs-lookup"><span data-stu-id="81c8f-144">This includes the consistency policy and the locations which the database account exists in.</span></span>

> [!NOTE]
> <span data-ttu-id="81c8f-145">Tento příkaz umožňuje přidávat a odebírat oblasti, ale neumožňuje změnit priorit převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="81c8f-145">This command allows you to add and remove regions but does not allow you to modify failover priorities.</span></span> <span data-ttu-id="81c8f-146">Chcete-li upravit priorit převzetí služeb při selhání, přečtěte si téma [pod](#modify-failover-priority-powershell).</span><span class="sxs-lookup"><span data-stu-id="81c8f-146">To modify failover priorities, see [below](#modify-failover-priority-powershell).</span></span>

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* <span data-ttu-id="81c8f-147">`<write-region-location>`Název umístění oblasti zápisu databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-147">`<write-region-location>` The location name of the write region of the database account.</span></span> <span data-ttu-id="81c8f-148">Toto umístění je potřeba mít hodnotu převzetí služeb při selhání s prioritou 0.</span><span class="sxs-lookup"><span data-stu-id="81c8f-148">This location is required to have a failover priority value of 0.</span></span> <span data-ttu-id="81c8f-149">Musí mít přesně jeden zápis oblast na databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-149">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="81c8f-150">`<read-region-location>`Název umístění oblasti čtení databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-150">`<read-region-location>` The location name of the read region of the database account.</span></span> <span data-ttu-id="81c8f-151">Toto umístění je potřeba mít hodnotu převzetí služeb při selhání s prioritou větší než 0.</span><span class="sxs-lookup"><span data-stu-id="81c8f-151">This location is required to have a failover priority value of greater than 0.</span></span> <span data-ttu-id="81c8f-152">Může existovat více než jeden pro čtení oblasti za databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-152">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="81c8f-153">`<default-consistency-level>`Výchozí úroveň konzistence účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-153">`<default-consistency-level>` The default consistency level of the Azure Cosmos DB account.</span></span> <span data-ttu-id="81c8f-154">Další informace najdete v tématu [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="81c8f-154">For more information, see [Consistency Levels in Azure Cosmos DB](consistency-levels.md).</span></span>
* <span data-ttu-id="81c8f-155">`<ip-range-filter>`Určuje sadu IP adresy nebo rozsahy IP adres ve formátu CIDR zahrnuty jako seznam povolených IP adresy klienta pro účet danou databázi.</span><span class="sxs-lookup"><span data-stu-id="81c8f-155">`<ip-range-filter>` Specifies the set of IP addresses or IP address ranges in CIDR form to be included as the allowed list of client IPs for a given database account.</span></span> <span data-ttu-id="81c8f-156">IP adresy a rozsahy musí být oddělené čárkami a nesmí obsahovat žádné mezery.</span><span class="sxs-lookup"><span data-stu-id="81c8f-156">IP addresses/ranges must be comma separated and must not contain any spaces.</span></span> <span data-ttu-id="81c8f-157">Další informace najdete v tématu [podpora brány Firewall databáze Azure Cosmos](firewall-support.md)</span><span class="sxs-lookup"><span data-stu-id="81c8f-157">For more information, see [Azure Cosmos DB Firewall Support](firewall-support.md)</span></span>
* <span data-ttu-id="81c8f-158">`<max-interval>`Při použití s konzistence typu s ohraničenou Prošlostí, tato hodnota představuje čas množství typu prošlostí (v sekundách) tolerovat.</span><span class="sxs-lookup"><span data-stu-id="81c8f-158">`<max-interval>` When used with Bounded Staleness consistency, this value represents the time amount of staleness (in seconds) tolerated.</span></span> <span data-ttu-id="81c8f-159">Přijatém rozsahu pro tuto hodnotu je 1 až 100.</span><span class="sxs-lookup"><span data-stu-id="81c8f-159">Accepted range for this value is 1 - 100.</span></span>
* <span data-ttu-id="81c8f-160">`<max-staleness-prefix>`Při použití s konzistence typu s ohraničenou Prošlostí, tato hodnota představuje počet zastaralých požadavků tolerovat.</span><span class="sxs-lookup"><span data-stu-id="81c8f-160">`<max-staleness-prefix>` When used with Bounded Staleness consistency, this value represents the number of stale requests tolerated.</span></span> <span data-ttu-id="81c8f-161">Přijatém rozsahu pro tuto hodnotu je 1 – 2 147 483 647.</span><span class="sxs-lookup"><span data-stu-id="81c8f-161">Accepted range for this value is 1 – 2,147,483,647.</span></span>
* <span data-ttu-id="81c8f-162">`<resource-group-name>`Název [skupiny prostředků Azure] [ azure-resource-groups] , ke které se nový účet Azure Cosmos DB databáze patří do.</span><span class="sxs-lookup"><span data-stu-id="81c8f-162">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="81c8f-163">`<resource-group-location>`Umístění skupiny prostředků Azure, do které patří nový databázový účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-163">`<resource-group-location>` The location of the Azure Resource Group to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="81c8f-164">`<database-account-name>`Název účtu Azure Cosmos DB databáze aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="81c8f-164">`<database-account-name>` The name of the Azure Cosmos DB database account to be updated.</span></span>

<span data-ttu-id="81c8f-165">Příklad:</span><span class="sxs-lookup"><span data-stu-id="81c8f-165">Example:</span></span> 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <span data-ttu-id="81c8f-166"><a id="delete-documentdb-account-powershell"></a>Odstranění účtu databáze DocumentDB</span><span class="sxs-lookup"><span data-stu-id="81c8f-166"><a id="delete-documentdb-account-powershell"></a> Delete a DocumentDB Database Account</span></span>

<span data-ttu-id="81c8f-167">Tento příkaz umožňuje odstranit existující databáze účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-167">This command allows you to delete an existing Azure Cosmos DB database account.</span></span>

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* <span data-ttu-id="81c8f-168">`<resource-group-name>`Název [skupiny prostředků Azure] [ azure-resource-groups] , ke které se nový účet Azure Cosmos DB databáze patří do.</span><span class="sxs-lookup"><span data-stu-id="81c8f-168">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="81c8f-169">`<database-account-name>`Název účtu databáze Azure Cosmos DB k odstranění.</span><span class="sxs-lookup"><span data-stu-id="81c8f-169">`<database-account-name>` The name of the Azure Cosmos DB database account to be deleted.</span></span>

<span data-ttu-id="81c8f-170">Příklad:</span><span class="sxs-lookup"><span data-stu-id="81c8f-170">Example:</span></span>

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="81c8f-171"><a id="get-documentdb-properties-powershell"></a>Získat vlastnosti účtu databáze DocumentDB</span><span class="sxs-lookup"><span data-stu-id="81c8f-171"><a id="get-documentdb-properties-powershell"></a> Get Properties of a DocumentDB Database Account</span></span>

<span data-ttu-id="81c8f-172">Tento příkaz umožňuje získat vlastnosti existující databáze účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-172">This command allows you to get the properties of an existing Azure Cosmos DB database account.</span></span>

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="81c8f-173">`<resource-group-name>`Název [skupiny prostředků Azure] [ azure-resource-groups] , ke které se nový účet Azure Cosmos DB databáze patří do.</span><span class="sxs-lookup"><span data-stu-id="81c8f-173">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="81c8f-174">`<database-account-name>`Název databázového účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-174">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>

<span data-ttu-id="81c8f-175">Příklad:</span><span class="sxs-lookup"><span data-stu-id="81c8f-175">Example:</span></span>

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="81c8f-176"><a id="update-tags-powershell"></a>Značky aktualizace účtu databáze Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="81c8f-176"><a id="update-tags-powershell"></a> Update Tags of an Azure Cosmos DB Database Account</span></span>

<span data-ttu-id="81c8f-177">Následující příklad popisuje, jak nastavit [značky prostředku Azure] [ azure-resource-tags] pro vaši Azure DB Cosmos databáze účtu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-177">The following example describes how to set [Azure resource tags][azure-resource-tags] for your Azure Cosmos DB database account.</span></span>

> [!NOTE]
> <span data-ttu-id="81c8f-178">Tento příkaz je možné kombinovat s příkazy vytvoření nebo aktualizace připojením `-Tags` příznak s odpovídající parametr.</span><span class="sxs-lookup"><span data-stu-id="81c8f-178">This command can be combined with the create or update commands by appending the `-Tags` flag with the corresponding parameter.</span></span>

<span data-ttu-id="81c8f-179">Příklad:</span><span class="sxs-lookup"><span data-stu-id="81c8f-179">Example:</span></span>

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <span data-ttu-id="81c8f-180"><a id="list-account-keys-powershell"></a>Klíče účtu seznamu</span><span class="sxs-lookup"><span data-stu-id="81c8f-180"><a id="list-account-keys-powershell"></a> List Account Keys</span></span>

<span data-ttu-id="81c8f-181">Když vytvoříte účet Azure Cosmos DB, generuje tato služba dva hlavní přístupové klíče, které lze použít pro ověření při přístupu k účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-181">When you create an Azure Cosmos DB account, the service generates two master access keys that can be used for authentication when the Azure Cosmos DB account is accessed.</span></span> <span data-ttu-id="81c8f-182">Poskytnutím dvou přístupových klíčů Azure Cosmos DB umožňuje znovu vygenerovat klíče bez přerušení ke svému účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-182">By providing two access keys, Azure Cosmos DB enables you to regenerate the keys with no interruption to your Azure Cosmos DB account.</span></span> <span data-ttu-id="81c8f-183">Jen pro čtení klíče pro ověřování operacím jen pro čtení jsou také k dispozici.</span><span class="sxs-lookup"><span data-stu-id="81c8f-183">Read-only keys for authenticating read-only operations are also available.</span></span> <span data-ttu-id="81c8f-184">(Primární i sekundární) existují dva klíče pro čtení a zápis (primární i sekundární) a dva klíče jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="81c8f-184">There are two read-write keys (primary and secondary) and two read-only keys (primary and secondary).</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="81c8f-185">`<resource-group-name>`Název [skupiny prostředků Azure] [ azure-resource-groups] , ke které se nový účet Azure Cosmos DB databáze patří do.</span><span class="sxs-lookup"><span data-stu-id="81c8f-185">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="81c8f-186">`<database-account-name>`Název databázového účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-186">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>

<span data-ttu-id="81c8f-187">Příklad:</span><span class="sxs-lookup"><span data-stu-id="81c8f-187">Example:</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="81c8f-188"><a id="list-connection-strings-powershell"></a>Seznamu připojovacích řetězců</span><span class="sxs-lookup"><span data-stu-id="81c8f-188"><a id="list-connection-strings-powershell"></a> List Connection Strings</span></span>

<span data-ttu-id="81c8f-189">Pro účty MongoDB mohou být načteny připojovacího řetězce pro připojení k účtu databázi MongoDB aplikace, pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-189">For MongoDB accounts, the connection string to connect your MongoDB app to the database account can be retrieved using the following command.</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* <span data-ttu-id="81c8f-190">`<resource-group-name>`Název [skupiny prostředků Azure] [ azure-resource-groups] , ke které se nový účet Azure Cosmos DB databáze patří do.</span><span class="sxs-lookup"><span data-stu-id="81c8f-190">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="81c8f-191">`<database-account-name>`Název databázového účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-191">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>

<span data-ttu-id="81c8f-192">Příklad:</span><span class="sxs-lookup"><span data-stu-id="81c8f-192">Example:</span></span>

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <span data-ttu-id="81c8f-193"><a id="regenerate-account-key-powershell"></a>Znovu vygenerovat klíč účtu</span><span class="sxs-lookup"><span data-stu-id="81c8f-193"><a id="regenerate-account-key-powershell"></a> Regenerate Account Key</span></span>

<span data-ttu-id="81c8f-194">Měli byste změnit přístupové klíče k účtu Azure Cosmos DB pravidelně pro zvýšení zabezpečení připojení.</span><span class="sxs-lookup"><span data-stu-id="81c8f-194">You should change the access keys to your Azure Cosmos DB account periodically to help keep your connections more secure.</span></span> <span data-ttu-id="81c8f-195">Abyste mohli udržovat připojení k účtu Azure Cosmos DB používat jeden přístupový klíč, zatímco si znovu vygenerujete druhý přístupový klíč jsou přiřazeny dva přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="81c8f-195">Two access keys are assigned to enable you to maintain connections to the Azure Cosmos DB account using one access key while you regenerate the other access key.</span></span>

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* <span data-ttu-id="81c8f-196">`<resource-group-name>`Název [skupiny prostředků Azure] [ azure-resource-groups] , ke které se nový účet Azure Cosmos DB databáze patří do.</span><span class="sxs-lookup"><span data-stu-id="81c8f-196">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="81c8f-197">`<database-account-name>`Název databázového účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-197">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>
* <span data-ttu-id="81c8f-198">`<key-kind>`Jedním ze čtyř typů klíčů: ["Primární" | " Sekundární "|" PrimaryReadonly "|" SecondaryReadonly"], které chcete znovu vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="81c8f-198">`<key-kind>` One of the four types of keys: ["Primary"|"Secondary"|"PrimaryReadonly"|"SecondaryReadonly"] that you would like to regenerate.</span></span>

<span data-ttu-id="81c8f-199">Příklad:</span><span class="sxs-lookup"><span data-stu-id="81c8f-199">Example:</span></span>

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <span data-ttu-id="81c8f-200"><a id="modify-failover-priority-powershell"></a>Upravte prioritu převzetí služeb při selhání databáze účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="81c8f-200"><a id="modify-failover-priority-powershell"></a> Modify Failover Priority of an Azure Cosmos DB Database Account</span></span>

<span data-ttu-id="81c8f-201">Pro účty určené pro více oblastí databáze můžete změnit prioritu převzetí služeb při selhání různých oblastech, které v Azure Cosmos DB databázový účet existuje.</span><span class="sxs-lookup"><span data-stu-id="81c8f-201">For multi-region database accounts, you can change the failover priority of the various regions which the Azure Cosmos DB database account exists in.</span></span> <span data-ttu-id="81c8f-202">Další informace o převzetí služeb při selhání ve vašem účtu Azure Cosmos DB databáze najdete v tématu [distribuci dat globálně pomocí Azure Cosmos DB][distribute-data-globally].</span><span class="sxs-lookup"><span data-stu-id="81c8f-202">For more information on failover in your Azure Cosmos DB database account, see [Distribute data globally with Azure Cosmos DB][distribute-data-globally].</span></span>

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* <span data-ttu-id="81c8f-203">`<write-region-location>`Název umístění oblasti zápisu databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-203">`<write-region-location>` The location name of the write region of the database account.</span></span> <span data-ttu-id="81c8f-204">Toto umístění je potřeba mít hodnotu převzetí služeb při selhání s prioritou 0.</span><span class="sxs-lookup"><span data-stu-id="81c8f-204">This location is required to have a failover priority value of 0.</span></span> <span data-ttu-id="81c8f-205">Musí mít přesně jeden zápis oblast na databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-205">There must be exactly one write region per database account.</span></span>
* <span data-ttu-id="81c8f-206">`<read-region-location>`Název umístění oblasti čtení databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-206">`<read-region-location>` The location name of the read region of the database account.</span></span> <span data-ttu-id="81c8f-207">Toto umístění je potřeba mít hodnotu převzetí služeb při selhání s prioritou větší než 0.</span><span class="sxs-lookup"><span data-stu-id="81c8f-207">This location is required to have a failover priority value of greater than 0.</span></span> <span data-ttu-id="81c8f-208">Může existovat více než jeden pro čtení oblasti za databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="81c8f-208">There can be more than one read regions per database account.</span></span>
* <span data-ttu-id="81c8f-209">`<resource-group-name>`Název [skupiny prostředků Azure] [ azure-resource-groups] , ke které se nový účet Azure Cosmos DB databáze patří do.</span><span class="sxs-lookup"><span data-stu-id="81c8f-209">`<resource-group-name>` The name of the [Azure Resource Group][azure-resource-groups] to which the new Azure Cosmos DB database account belongs to.</span></span>
* <span data-ttu-id="81c8f-210">`<database-account-name>`Název databázového účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="81c8f-210">`<database-account-name>` The name of the Azure Cosmos DB database account.</span></span>

<span data-ttu-id="81c8f-211">Příklad:</span><span class="sxs-lookup"><span data-stu-id="81c8f-211">Example:</span></span>

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a><span data-ttu-id="81c8f-212">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81c8f-212">Next steps</span></span>

* <span data-ttu-id="81c8f-213">Připojení pomocí rozhraní .NET, najdete v části [připojit a zadávat dotazy pomocí .NET](create-documentdb-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="81c8f-213">To connect using .NET, see [Connect and query with .NET](create-documentdb-dotnet.md).</span></span>
* <span data-ttu-id="81c8f-214">Připojení pomocí .NET Core, najdete v části [připojit a zadávat dotazy pomocí .NET Core](create-documentdb-dotnet-core.md).</span><span class="sxs-lookup"><span data-stu-id="81c8f-214">To connect using .NET Core, see [Connect and query with .NET Core](create-documentdb-dotnet-core.md).</span></span>
* <span data-ttu-id="81c8f-215">Připojení pomocí Node.js, najdete v části [připojit a zadávat dotazy s Node.js a aplikace pro práci s MongoDB](create-mongodb-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="81c8f-215">To connect using Node.js, see [Connect and query with Node.js and a MongoDB app](create-mongodb-nodejs.md).</span></span>

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/documentdbresourceprovider/
