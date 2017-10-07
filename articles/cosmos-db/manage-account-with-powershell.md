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
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a>Vytvoření účtu Azure Cosmos DB pomocí prostředí PowerShell

Hello následující příručka popisuje správu tooautomate příkazy účty Azure Cosmos DB databáze pomocí prostředí Azure Powershell. Zahrnuje také klíče účtu toomanage příkazy a priorit převzetí služeb při selhání v [účty databáze více oblast][scaling-globally]. Aktualizace databázového účtu vám umožní toomodify konzistence zásady a oblastí, přidat nebo odebrat. Pro různé platformy správy účtu Azure Cosmos DB, můžete použít buď [rozhraní příkazového řádku Azure](cli-samples.md), hello [rozhraní API REST zprostředkovatele prostředků][rp-rest-api], nebo hello [Azure portál](create-documentdb-dotnet.md#create-account).

## <a name="getting-started"></a>Začínáme

Postupujte podle pokynů hello v [jak tooinstall a konfigurace prostředí Azure PowerShell] [ powershell-install-configure] tooinstall a přihlaste se tooyour Azure Resource Manager účet v prostředí Powershell.

### <a name="notes"></a>Poznámky

* Pokud chcete tooexecute hello následující příkazy bez nutnosti potvrzení uživatelem, připojit hello `-Force` příznak toohello příkaz.
* Všechny hello následující příkazy jsou synchronní.

## <a id="create-documentdb-account-powershell"></a>Vytvoření účtu Azure Cosmos DB

Tento příkaz umožňuje toocreate účet Azure Cosmos DB databáze. Konfigurace nového účtu databáze jako buď jedné oblasti nebo [více oblast] [ scaling-globally] s určitým [konzistence zásad](consistency-levels.md).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>`název umístění Hello hello zápisu oblast hello databázového účtu. Toto umístění je požadovaná toohave hodnotu převzetí služeb při selhání s prioritou 0. Musí mít přesně jeden zápis oblast na databázového účtu.
* `<read-region-location>`název umístění Hello hello přečíst oblast hello databázového účtu. Toto umístění je požadováno toohave hodnotu převzetí služeb při selhání s prioritou větší než 0. Může existovat více než jeden pro čtení oblasti za databázového účtu.
* `<ip-range-filter>`Určuje sadu hello IP adresy nebo rozsahy IP adres v toobe formuláře CIDR zahrnuty jako hello klienta IP adres pro danou databázi účet seznamu povolených aplikací. IP adresy a rozsahy musí být oddělené čárkami a nesmí obsahovat žádné mezery. Další informace najdete v tématu [podpora brány Firewall databáze Azure Cosmos](firewall-support.md)
* `<default-consistency-level>`Hello výchozí konzistence úroveň hello účet Azure Cosmos DB. Další informace najdete v tématu [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).
* `<max-interval>`Při použití s konzistence typu s ohraničenou Prošlostí, tato hodnota představuje hello čas množství typu prošlostí (v sekundách) tolerovat. Přijatém rozsahu pro tuto hodnotu je 1 až 100.
* `<max-staleness-prefix>`Při použití s konzistence typu s ohraničenou Prošlostí, tato hodnota představuje hello počet zastaralých požadavků tolerovat. Přijatém rozsahu pro tuto hodnotu je 1 – 2 147 483 647.
* `<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.
* `<resource-group-location>`umístění Hello hello skupiny prostředků Azure toowhich hello nový Azure Cosmos DB databáze účet patří do.
* `<database-account-name>`Název Hello hello Azure Cosmos DB databáze účet toobe vytvořili. Může použít jenom malá písmena, číslice, hello '-' znak a musí být v rozmezí 3 až 50 znaků.

Příklad: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a>Poznámky
* Hello předchozí příklad vytvoří databázový účet se dvěma oblastmi. Je také možné toocreate databázový účet se jedné oblasti (který je určený jako hello zápisu oblasti a mají hodnotu převzetí služeb při selhání s prioritou 0) nebo víc než dvou oblastech. Další informace najdete v tématu [účty databáze více oblast][scaling-globally].
* Hello umístění musí být oblasti, ve kterých je Azure Cosmos DB všeobecně dostupná. aktuální seznam oblastí Hello je uvedený na hello [oblasti Azure stránky](https://azure.microsoft.com/regions/#services).

## <a id="update-documentdb-account-powershell"></a>Aktualizace účtu databáze DocumentDB

Tento příkaz vám umožní tooupdate vlastnostech účtu databáze Azure Cosmos DB. To zahrnuje hello konzistence zásad a hello umístění, které hello databázový účet existuje v.

> [!NOTE]
> Tento příkaz vám umožní tooadd a odebrat oblasti, ale neumožňuje toomodify priorit převzetí služeb při selhání. toomodify priorit převzetí služeb při selhání, najdete v části [pod](#modify-failover-priority-powershell).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>`název umístění Hello hello zápisu oblast hello databázového účtu. Toto umístění je požadovaná toohave hodnotu převzetí služeb při selhání s prioritou 0. Musí mít přesně jeden zápis oblast na databázového účtu.
* `<read-region-location>`název umístění Hello hello přečíst oblast hello databázového účtu. Toto umístění je požadováno toohave hodnotu převzetí služeb při selhání s prioritou větší než 0. Může existovat více než jeden pro čtení oblasti za databázového účtu.
* `<default-consistency-level>`Hello výchozí konzistence úroveň hello účet Azure Cosmos DB. Další informace najdete v tématu [úrovně konzistence v Azure Cosmos DB](consistency-levels.md).
* `<ip-range-filter>`Určuje sadu hello IP adresy nebo rozsahy IP adres v toobe formuláře CIDR zahrnuty jako hello klienta IP adres pro danou databázi účet seznamu povolených aplikací. IP adresy a rozsahy musí být oddělené čárkami a nesmí obsahovat žádné mezery. Další informace najdete v tématu [podpora brány Firewall databáze Azure Cosmos](firewall-support.md)
* `<max-interval>`Při použití s konzistence typu s ohraničenou Prošlostí, tato hodnota představuje hello čas množství typu prošlostí (v sekundách) tolerovat. Přijatém rozsahu pro tuto hodnotu je 1 až 100.
* `<max-staleness-prefix>`Při použití s konzistence typu s ohraničenou Prošlostí, tato hodnota představuje hello počet zastaralých požadavků tolerovat. Přijatém rozsahu pro tuto hodnotu je 1 – 2 147 483 647.
* `<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.
* `<resource-group-location>`umístění Hello hello skupiny prostředků Azure toowhich hello nový Azure Cosmos DB databáze účet patří do.
* `<database-account-name>`Název Hello hello Azure Cosmos DB databáze účet toobe aktualizovat.

Příklad: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <a id="delete-documentdb-account-powershell"></a>Odstranění účtu databáze DocumentDB

Tento příkaz umožňuje toodelete existující databáze účet Azure Cosmos DB.

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* `<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.
* `<database-account-name>`Název Hello hello Azure Cosmos DB databáze účet toobe odstranit.

Příklad:

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="get-documentdb-properties-powershell"></a>Získat vlastnosti účtu databáze DocumentDB

Tento příkaz umožňuje tooget hello vlastnosti existující databáze účet Azure Cosmos DB.

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.
* `<database-account-name>`Název Hello hello databázový účet Azure Cosmos DB.

Příklad:

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="update-tags-powershell"></a>Značky aktualizace účtu databáze Azure Cosmos DB

Hello následující příklad popisuje jak tooset [značky prostředku Azure] [ azure-resource-tags] pro vaši Azure DB Cosmos databáze účtu.

> [!NOTE]
> Tento příkaz je možné kombinovat s hello vytvořit nebo aktualizovat příkazy připojením hello `-Tags` příznak s hello odpovídající parametr.

Příklad:

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <a id="list-account-keys-powershell"></a>Klíče účtu seznamu

Při vytváření účtu Azure Cosmos DB hello služba generuje dva hlavní přístupové klíče, které lze použít pro ověření při přístupu k účtu Azure Cosmos DB hello. Poskytnutím dvou přístupových klíčů Azure Cosmos DB vám umožní tooregenerate hello klíče bez přerušení tooyour účet Azure Cosmos DB. Jen pro čtení klíče pro ověřování operacím jen pro čtení jsou také k dispozici. (Primární i sekundární) existují dva klíče pro čtení a zápis (primární i sekundární) a dva klíče jen pro čtení.

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.
* `<database-account-name>`Název Hello hello databázový účet Azure Cosmos DB.

Příklad:

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="list-connection-strings-powershell"></a>Seznamu připojovacích řetězců

Pro účty MongoDB hello tooconnect řetězec připojení, které MongoDB aplikace toohello databázový účet se dá načíst pomocí hello následující příkaz.

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.
* `<database-account-name>`Název Hello hello databázový účet Azure Cosmos DB.

Příklad:

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="regenerate-account-key-powershell"></a>Znovu vygenerovat klíč účtu

Měli byste změnit účet Azure Cosmos DB hello přístupu k klíče tooyour pravidelně toohelp připojení lépe zabezpečit. Dva přístupové klíče se přiřazují tooenable hello je toomaintain připojení toohello účet Azure Cosmos DB používat jeden přístupový klíč, zatímco si znovu vygenerujete druhý přístupový klíč.

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* `<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.
* `<database-account-name>`Název Hello hello databázový účet Azure Cosmos DB.
* `<key-kind>`Jeden z typů hello čtyři klíčů: ["Primární" | " Sekundární "|" PrimaryReadonly "|" SecondaryReadonly"], které chcete tooregenerate.

Příklad:

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <a id="modify-failover-priority-powershell"></a>Upravte prioritu převzetí služeb při selhání databáze účtu Azure Cosmos DB

Pro účty databáze více oblast můžete změnit prioritu převzetí služeb při selhání hello hello v různých oblastech, které hello Azure Cosmos DB databázový účet existuje. Další informace o převzetí služeb při selhání ve vašem účtu Azure Cosmos DB databáze najdete v tématu [distribuci dat globálně pomocí Azure Cosmos DB][distribute-data-globally].

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* `<write-region-location>`název umístění Hello hello zápisu oblast hello databázového účtu. Toto umístění je požadovaná toohave hodnotu převzetí služeb při selhání s prioritou 0. Musí mít přesně jeden zápis oblast na databázového účtu.
* `<read-region-location>`název umístění Hello hello přečíst oblast hello databázového účtu. Toto umístění je požadováno toohave hodnotu převzetí služeb při selhání s prioritou větší než 0. Může existovat více než jeden pro čtení oblasti za databázového účtu.
* `<resource-group-name>`Název Hello hello [skupiny prostředků Azure] [ azure-resource-groups] toowhich hello nový Azure Cosmos DB databáze účet patří do.
* `<database-account-name>`Název Hello hello databázový účet Azure Cosmos DB.

Příklad:

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a>Další kroky

* tooconnect pomocí rozhraní .NET, najdete v části [připojit a zadávat dotazy pomocí .NET](create-documentdb-dotnet.md).
* tooconnect pomocí .NET Core, najdete v části [připojit a zadávat dotazy pomocí .NET Core](create-documentdb-dotnet-core.md).
* tooconnect pomocí Node.js, najdete v části [připojit a zadávat dotazy s Node.js a aplikace pro práci s MongoDB](create-mongodb-nodejs.md).

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/documentdbresourceprovider/
