---
title: "aaaManage Azure Search pomocí skriptů prostředí Powershell | Microsoft Docs"
description: "Správa služby Azure Search pomocí skriptů prostředí PowerShell. Vytvořit nebo aktualizovat službu Azure Search a spravovat klíče správce Azure Search"
services: search
documentationcenter: 
author: seansaleh
manager: mblythe
editor: 
tags: azure-resource-manager
ms.assetid: 9b3dc1f2-3619-4235-ba1f-d2d6f5c45dd5
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: powershell
ms.date: 08/15/2016
ms.author: seasa
ms.openlocfilehash: fc7fb4b025340c77717601e0aaee938be3e9230f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-azure-search-service-with-powershell"></a>Správa služby Azure Search pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> 
> 

Toto téma popisuje tooperform příkazy prostředí PowerShell hello řadu hello úlohy správy pro služby vyhledávání systému Azure. Jsme provede procesem vytváření vyhledávací službu, je škálování a správu jeho klíče rozhraní API.
Tyto příkazy paralelní možnosti správy hello k dispozici v hello [REST API správy služby Azure Search](http://msdn.microsoft.com/library/dn832684.aspx).

## <a name="prerequisites"></a>Požadavky
* Pokud nemáte Azure PowerShell 1.0 nebo novější. Pokyny najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).
* Musíte být přihlášení tooyour předplatného Azure v prostředí PowerShell, jak je popsáno níže.

Nejdřív musíte tooAzure přihlášení pomocí tohoto příkazu:

    Login-AzureRmAccount

Zadejte hello e-mailovou adresu účtu Azure a jeho heslo v dialogovém okně pro přihlášení hello Microsoft Azure.

Případně můžete [přihlášení interaktivně pomocí objektu služby](../azure-resource-manager/resource-group-authenticate-service-principal.md).

Pokud máte víc předplatných Azure, musíte tooset vašeho předplatného Azure. Seznam odběrů aktuální toosee tento příkaz spustit.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

toospecify hello předplatného, spusťte následující příkaz hello. V následujícím příkladu hello, je název odběru hello `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-toohelp-you-get-started"></a>Toohelp příkazy, které začít pracovat
    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register hello ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search"

    # Create a new search service
    # This command will return once hello service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1

    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # View your resource
    $resource

    # Get hello primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate hello secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access tooyour indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key

    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until hello operation is finished
    # It can take 15 minutes or more tooprovision hello additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource

    # Delete your service
    # Deleting your service will delete all indexes and data in hello service
    $resource | Remove-AzureRmResource

## <a name="next-steps"></a>Další kroky
Je teď vytvořená služby, můžete provést další kroky hello: sestavení [index](search-what-is-an-index.md), [dotazovat index](search-query-overview.md)a nakonec vytvořit a spravovat vlastní vyhledávací aplikaci, která používá službu Azure Search.

* [Vytvoření indexu Azure Search v hello portálu Azure](search-create-index-portal.md)
* [Dotazování indexu Azure Search pomocí Průzkumník služby Search v hello portálu Azure](search-explorer.md)
* [Instalační program indexer tooload data z jiných služeb](search-indexer-overview.md)
* [Jak toouse Azure hledání v rozhraní .NET](search-howto-dotnet-sdk.md)
* [Analýza provozu Azure Search](search-traffic-analytics.md)

