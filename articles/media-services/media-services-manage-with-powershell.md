---
title: "aaaManage Azure Media Services účty pomocí prostředí PowerShell"
description: "Zjistěte, jak účty toomanage Azure Media Services pomocí rutin prostředí PowerShell."
author: Juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 17a10c25-d94f-421c-b6bc-ae0958e2ac96
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: juliako
ms.openlocfilehash: e8f97bb2393343e45fabf9c437b4fc09f2525dc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-media-services-accounts-with-powershell"></a>Správa účtů Azure Media Services pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](media-services-portal-create-account.md)
> * [PowerShell](media-services-manage-with-powershell.md)
> * [REST](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> toobe možné toocreate účtu Azure Media Services, musí mít účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Bezplatná zkušební verze Azure</a>.
> 
> 

## <a name="overview"></a>Přehled
Tento článek obsahuje seznam rutin prostředí Azure PowerShell hello pro Azure Media Services (AMS) v Azure Resource Manager framework hello. rutiny Hello existovat v hello **Microsoft.Azure.Commands.Media** oboru názvů.

## <a name="versions"></a>Verze
**ApiVersion**: "2015-10-01"

## <a name="new-azurermmediaservice"></a>New-AzureRmMediaService
Vytvoří služby media service.

### <a name="syntax"></a>Syntaxe
Sada parametrů: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Sada parametrů: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Parametry
**-ResourceGroupName &lt;řetězec&gt;**

Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-AccountName &lt;řetězec&gt;**

Určuje název hello hello mediální služby.

| Aliasy | Name (Název) |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |1 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

**-Umístění &lt;řetězec&gt;**

Určuje umístění prostředků hello hello mediální služby.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |2 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-StorageAccountId &lt;řetězec&gt;**

Určuje primárního úložiště účet, který přidružené hello mediální služby.

* Nový účet úložiště jsou (vytvořený pomocí rozhraní API Správce prostředků hello) podporovaná jenom.
* Hello účet úložiště, musí existovat a má hello stejného umístění, hello mediální služby.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |3 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Název sady parametrů |StorageAccountIdParamSet |
| Přijímat zástupné znaky? |False |

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Určuje účty úložiště, které přísluší službě hello média.

* Nový účet úložiště jsou (vytvořený pomocí rozhraní API Správce prostředků hello) podporovaná jenom.
* Hello účet úložiště, musí existovat a má hello stejného umístění, hello mediální služby.
* Pouze jeden účet úložiště lze zadat jako primární.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |3 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Název sady parametrů |StorageAccountsParamSet |
| Přijímat zástupné znaky? |False |

**-Značky &lt;zatřiďovací tabulky&gt;**

Určuje tabulku hash hello značky, které jsou přidruženy hello mediální služby.

* Příklad: @{"tag1"="hodnota1";" tag2 "=: value2"}

| Aliasy | None |
| --- | --- |
| Povinné? |False |
| Pozice? |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Vstupy
Hello vstupní typ je typ hello hello objekty toohello rutiny můžete prostřednictvím kanálu.

### <a name="outputs"></a>Výstupy
Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.

## <a name="set-azurermmediaservice"></a>Set-AzureRmMediaService
Aktualizace služby media service.

### <a name="syntax"></a>Syntaxe
    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Parametry
**-ResourceGroupName &lt;řetězec&gt;**

Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-AccountName &lt;řetězec&gt;**

Určuje název hello hello mediální služby.

| Aliasy | Name (Název) |
| --- | --- |
| Povinné? |True |
| Pozice? |1 |
| Výchozí hodnota |Žádný |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Určuje účty úložiště, které přísluší službě hello média.

* Nový účet úložiště jsou (vytvořený pomocí rozhraní API Správce prostředků hello) podporovaná jenom.
* Hello účet úložiště, musí existovat a má hello stejného umístění, hello mediální služby.
* Pouze jeden účet úložiště lze zadat jako primární.

| Aliasy | None |
| --- | --- |
| Povinné? |False |
| Pozice? |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Název sady parametrů |StorageAccountsParamSet |
| Přijímat zástupné znaky? |False |

**-Značky &lt;zatřiďovací tabulky&gt;**

Určuje tabulku hash hello značky, které jsou spojeny s touto službou média.

* Hello značky, které jsou přidruženy hello mediální služby se nahradí hodnotu zadanou pomocí hello zákazníka.

| Aliasy | None |
| --- | --- |
| Povinné? |False |
| Pozice? |S názvem |
| Výchozí hodnota |Žádný |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Vstupy
Hello vstupní typ je typ hello hello objekty toohello rutiny můžete prostřednictvím kanálu.

### <a name="outputs"></a>Výstupy
Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.

## <a name="remove-azurermmediaservice"></a>Remove-AzureRmMediaService
Odebere služby media service.

### <a name="syntax"></a>Syntaxe
    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry
**-ResourceGroupName &lt;řetězec&gt;**

Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-AccountName &lt;řetězec&gt;**

Určuje název hello hello mediální služby.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |2 |
| Výchozí hodnota |Žádný |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Vstupy
Hello vstupní typ je typ hello hello objekty toohello rutiny můžete prostřednictvím kanálu.

### <a name="outputs"></a>Výstupy
Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService
Získá všechny služby media services ve skupině prostředků nebo služby media service se zadaným názvem.

### <a name="syntax"></a>Syntaxe
ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>]    

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry
**-ResourceGroupName &lt;řetězec&gt;**

Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Název sady parametrů |ResourceGroupParameterSet AccountNameParameterSet |

Přijímat zástupné znaky?   False

**-AccountName &lt;řetězec&gt;**

Určuje název hello hello mediální služby.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |1 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Název sady parametrů |AccountNameParameterSet |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Vstupy
Hello vstupní typ je typ hello hello objekty toohello rutiny můžete prostřednictvím kanálu.

### <a name="outputs"></a>Výstupy
Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys
Získá klíče služby media service.

### <a name="syntax"></a>Syntaxe
    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry
**-ResourceGroupName &lt;řetězec&gt;**

Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-AccountName &lt;řetězec&gt;**

Určuje název hello hello mediální služby.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |1 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Vstupy
Hello vstupní typ je typ hello hello objekty toohello rutiny můžete prostřednictvím kanálu.

### <a name="outputs"></a>Výstupy
Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.

## <a name="set-azurermmediaservicekey"></a>Set-AzureRmMediaServiceKey
Regeneruje primární nebo sekundární klíč služby média.

### <a name="syntax"></a>Syntaxe
    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Parametry
**-ResourceGroupName &lt;řetězec&gt;**

Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-AccountName &lt;řetězec&gt;**

Určuje název hello hello mediální služby.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |1 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**Typ_klíče - &lt;Typ_klíče.&gt;**

Určuje typ klíče hello hello média služby.

* Primární nebo sekundární

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |2 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Vstupy
Hello vstupní typ je typ hello hello objekty toothe rutiny můžete prostřednictvím kanálu.

### <a name="outputs"></a>Výstupy
Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.

## <a name="sync-azurermmediaservicestoragekeys"></a>Sync-AzureRmMediaServiceStorageKeys
Synchronizuje klíče účtu úložiště pro účet úložiště přidružené hello mediální služby.

### <a name="syntax"></a>Syntaxe
    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry
**-ResourceGroupName &lt;řetězec&gt;**

Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-AccountName &lt;řetězec&gt;**

Určuje název hello hello mediální služby.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |1 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-StorageAccountId &lt;řetězec&gt;**

Určuje účet úložiště hello přidružený hello mediální služby.

| Aliasy | ID |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice? |2 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.

### <a name="inputs"></a>Vstupy
Hello vstupní typ je typ hello hello objekty toohello rutiny můžete prostřednictvím kanálu.

### <a name="outputs"></a>Výstupy
Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.

## <a name="next-step"></a>Další krok
Podívejte se na kurzů ke službě Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

