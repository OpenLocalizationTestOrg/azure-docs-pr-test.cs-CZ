---
title: "Správa účtů Azure Media Services pomocí prostředí PowerShell"
description: "Zjistěte, jak chcete spravovat účty pro Azure Media Services pomocí rutin prostředí PowerShell."
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
ms.openlocfilehash: 3d999d9e27844bc0164cc3572522b9ec022118a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-media-services-accounts-with-powershell"></a>Správa účtů Azure Media Services pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](media-services-portal-create-account.md)
> * [PowerShell](media-services-manage-with-powershell.md)
> * [REST](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> Abyste mohli vytvořit účet Azure Media Services, musí mít účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Bezplatná zkušební verze Azure</a>.
> 
> 

## <a name="overview"></a>Přehled
Tento článek obsahuje seznam rutin prostředí Azure PowerShell pro Azure Media Services (AMS) v rámci Azure Resource Manager. Rutiny v existovat **Microsoft.Azure.Commands.Media** oboru názvů.

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

Určuje název skupiny prostředků, do které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-AccountName &lt;řetězec&gt;**

Určuje název mediální služby.

| Aliasy | Name (Název) |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |1 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

**-Umístění &lt;řetězec&gt;**

Určuje umístění prostředků mediální služby.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |2 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-StorageAccountId &lt;řetězec&gt;**

Určuje primárního úložiště účet, který přidružené mediální služby.

* Nový účet úložiště jsou (vytvořený pomocí rozhraní API Správce prostředků) podporovaná jenom.
* Účet úložiště, musí existovat a má stejné umístění s mediální služby.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |3 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Název sady parametrů |StorageAccountIdParamSet |
| Přijímat zástupné znaky? |False |

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Určuje účty úložiště, které přidružené mediální služby.

* Nový účet úložiště jsou (vytvořený pomocí rozhraní API Správce prostředků) podporovaná jenom.
* Účet úložiště, musí existovat a má stejné umístění s mediální služby.
* Pouze jeden účet úložiště lze zadat jako primární.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |3 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Název sady parametrů |StorageAccountsParamSet |
| Přijímat zástupné znaky? |False |

**-Značky &lt;zatřiďovací tabulky&gt;**

Určuje tabulku hash značek, které jsou přidruženy mediální služby.

* Příklad: @{"tag1"="hodnota1";" tag2 "=: value2"}

| Aliasy | None |
| --- | --- |
| Vyžaduje? |False |
| Pozice? |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.

### <a name="inputs"></a>Vstupy
Vstupní typ je typ objektů, které můžete rutině předat kanálem.

### <a name="outputs"></a>Výstupy
Výstupní typ je typ objektů, které rutina generuje.

## <a name="set-azurermmediaservice"></a>Set-AzureRmMediaService
Aktualizace služby media service.

### <a name="syntax"></a>Syntaxe
    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Parametry
**-ResourceGroupName &lt;řetězec&gt;**

Určuje název skupiny prostředků, do které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-AccountName &lt;řetězec&gt;**

Určuje název mediální služby.

| Aliasy | Name (Název) |
| --- | --- |
| Vyžaduje? |True |
| Pozice? |1 |
| Výchozí hodnota |Žádný |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Určuje účty úložiště, které přidružené mediální služby.

* Nový účet úložiště jsou (vytvořený pomocí rozhraní API Správce prostředků) podporovaná jenom.
* Účet úložiště, musí existovat a má stejné umístění s mediální služby.
* Pouze jeden účet úložiště lze zadat jako primární.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |False |
| Pozice? |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Název sady parametrů |StorageAccountsParamSet |
| Přijímat zástupné znaky? |False |

**-Značky &lt;zatřiďovací tabulky&gt;**

Určuje tabulku hash značek, které jsou spojeny s touto službou média.

* Značky, které jsou přidruženy mediální služby se nahradí hodnotu zadanou pomocí zákazníka.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |False |
| Pozice? |S názvem |
| Výchozí hodnota |Žádný |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.

### <a name="inputs"></a>Vstupy
Vstupní typ je typ objektů, které můžete rutině předat kanálem.

### <a name="outputs"></a>Výstupy
Výstupní typ je typ objektů, které rutina generuje.

## <a name="remove-azurermmediaservice"></a>Remove-AzureRmMediaService
Odebere služby media service.

### <a name="syntax"></a>Syntaxe
    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry
**-ResourceGroupName &lt;řetězec&gt;**

Určuje název skupiny prostředků, do které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-AccountName &lt;řetězec&gt;**

Určuje název mediální služby.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |2 |
| Výchozí hodnota |Žádný |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.

### <a name="inputs"></a>Vstupy
Vstupní typ je typ objektů, které můžete rutině předat kanálem.

### <a name="outputs"></a>Výstupy
Výstupní typ je typ objektů, které rutina generuje.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService
Získá všechny služby media services ve skupině prostředků nebo služby media service se zadaným názvem.

### <a name="syntax"></a>Syntaxe
ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>]    

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry
**-ResourceGroupName &lt;řetězec&gt;**

Určuje název skupiny prostředků, do které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Název sady parametrů |ResourceGroupParameterSet AccountNameParameterSet |

Přijímat zástupné znaky?   False

**-AccountName &lt;řetězec&gt;**

Určuje název mediální služby.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |1 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Název sady parametrů |AccountNameParameterSet |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.

### <a name="inputs"></a>Vstupy
Vstupní typ je typ objektů, které můžete rutině předat kanálem.

### <a name="outputs"></a>Výstupy
Výstupní typ je typ objektů, které rutina generuje.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys
Získá klíče služby media service.

### <a name="syntax"></a>Syntaxe
    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry
**-ResourceGroupName &lt;řetězec&gt;**

Určuje název skupiny prostředků, do které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-AccountName &lt;řetězec&gt;**

Určuje název mediální služby.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |1 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.

### <a name="inputs"></a>Vstupy
Vstupní typ je typ objektů, které můžete rutině předat kanálem.

### <a name="outputs"></a>Výstupy
Výstupní typ je typ objektů, které rutina generuje.

## <a name="set-azurermmediaservicekey"></a>Set-AzureRmMediaServiceKey
Regeneruje primární nebo sekundární klíč služby média.

### <a name="syntax"></a>Syntaxe
    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Parametry
**-ResourceGroupName &lt;řetězec&gt;**

Určuje název skupiny prostředků, do které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-AccountName &lt;řetězec&gt;**

Určuje název mediální služby.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |1 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**Typ_klíče - &lt;Typ_klíče.&gt;**

Určuje typ klíče mediální služby.

* Primární nebo sekundární

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |2 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.

### <a name="inputs"></a>Vstupy
Vstupní typ je typ objektů, které můžete rutině předat kanálem.

### <a name="outputs"></a>Výstupy
Výstupní typ je typ objektů, které rutina generuje.

## <a name="sync-azurermmediaservicestoragekeys"></a>Sync-AzureRmMediaServiceStorageKeys
Synchronizuje klíče účtu úložiště pro účet úložiště, který je přidružený k službě média.

### <a name="syntax"></a>Syntaxe
    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametry
**-ResourceGroupName &lt;řetězec&gt;**

Určuje název skupiny prostředků, do které patří tato služba média.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |0 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-AccountName &lt;řetězec&gt;**

Určuje název mediální služby.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |1 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**-StorageAccountId &lt;řetězec&gt;**

Určuje účet úložiště, který je přidružený k službě média.

| Aliasy | ID |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice? |2 |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |true(ByPropertyName) |
| Přijímat zástupné znaky? |False |

**&lt;CommandParameters&gt;**

Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.

### <a name="inputs"></a>Vstupy
Vstupní typ je typ objektů, které můžete rutině předat kanálem.

### <a name="outputs"></a>Výstupy
Výstupní typ je typ objektů, které rutina generuje.

## <a name="next-step"></a>Další krok
Podívejte se na kurzů ke službě Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

