---
title: "aaaArchive diagnostických protokolů Azure | Microsoft Docs"
description: "Zjistěte, jak tooarchive vaše Azure diagnostické protokoly pro dlouhodobé uchovávání v účtu úložiště."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 3a55c73f-2ef3-45f3-8956-bcf9c0cb7e05
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: bc9edbd3a649023a728b7fe77130dba2b6e6370d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="archive-azure-diagnostic-logs"></a>Archivovat Azure diagnostických protokolů
V tomto článku jsme ukazují, jak můžete použít hello portál Azure, rutiny prostředí PowerShell, rozhraní příkazového řádku, nebo REST API tooarchive vaše [Azure diagnostické protokoly](monitoring-overview-of-diagnostic-logs.md) v účtu úložiště. Tato možnost je užitečná, pokud byste chtěli tooretain vaše diagnostické protokoly zásadami volitelné uchování pro audit, statické analýzy nebo zálohování. účet úložiště Hello nemá toobe v hello stejného předplatného jako prostředek hello emitování protokoly tak dlouho, dokud hello uživatel, který konfiguruje nastavení hello má příslušné předplatné tooboth přístupu RBAC.

## <a name="prerequisites"></a>Požadavky
Než začnete, je třeba příliš[vytvořit účet úložiště](../storage/storage-create-storage-account.md) toowhich archivujete diagnostické protokoly. Důrazně doporučujeme stávající účet úložiště, který má jiné, -monitoring data v ní uloženy, takže může lépe řídit přístup k datům toomonitoring nepoužívejte. Ale pokud jsou také archivaci protokolu aktivit a účet úložiště diagnostiky metriky tooa, může být smysl toouse daný účet úložiště pro vaše diagnostické protokoly a tookeep všechna monitorování data v centrálním umístění. Hello účet úložiště, které používáte, musí být účet úložiště obecné účely, nikoli účet úložiště objektů blob.

## <a name="diagnostic-settings"></a>Nastavení diagnostiky
tooarchive vaše diagnostické protokoly, pomocí metod hello níže, nastavíte **nastavení diagnostiky** pro určitý prostředek. Nastavení diagnostiky pro prostředek definuje kategorie hello protokolů a metriky data odeslána tooa cílového (účet úložiště, obor názvů služby Event Hubs nebo analýzy protokolů). Definuje také zásady uchovávání informací hello (počet dní tooretain) pro události pro každé kategorie protokolu a metriky data uložená v účtu úložiště. Pokud zásady uchovávání informací nastavena toozero, události pro tuto kategorii protokolu se ukládají po neomezenou dobu (které je toosay, trvale). Zásady uchovávání informací v opačném případě může být libovolný počet dnů od 1 do 2147483647. [Další informace o nastavení pro diagnostiku zde](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings). Zásady uchovávání informací jsou použité za den, tak při hello Konec dne (UTC), zaprotokoluje hello dni, který je teď nad rámec zásad uchovávání informací hello budou odstraněny. Například pokud jste měli zásady uchovávání informací jeden den, od začátku hello dnešní den hello hello protokoly z hello den před včerejšek by odstraněn

## <a name="archive-diagnostic-logs-using-hello-portal"></a>Diagnostické protokoly archivu pomocí portálu hello
1. Hello portálu, přejděte tooAzure monitorování a klikněte na **nastavení diagnostiky**

    ![Monitorování části monitorování Azure](media/monitoring-archive-diagnostic-logs/diagnostic-settings-blade.png)

2. Volitelně můžete filtrovat seznam hello skupinu prostředků nebo typ prostředku, a potom klikněte na hello prostředků, pro kterou chcete tooset nastavení diagnostiky.

3. Pokud neexistuje žádné nastavení na hello prostředku, který jste zvolili, jste výzvami toocreate nastavení. Klikněte na tlačítko "Zapnout diagnostiky."

   ![Přidat nastavení diagnostiky - žádná existující nastavení](media/monitoring-archive-diagnostic-logs/diagnostic-settings-none.png)

   Pokud máte stávající nastavení na hello prostředku, zobrazí se seznam nastavení, které jsou již nakonfigurována na tomto prostředku. Klikněte na tlačítko "Přidat nastavení diagnostiky."

   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. Zadejte název nastavení a zaškrtněte políčko hello pro **exportovat tooStorage účet**, pak vyberte účet úložiště. Volitelně můžete nastavit počet dní tooretain tyto protokoly s použitím hello **uchovávání dat (dny)** posuvníky. Uchování nulový počet dnů po neomezenou dobu ukládá protokoly hello.
   
   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-archive-diagnostic-logs/diagnostic-settings-configure.png)
    
4. Klikněte na **Uložit**.

Po chvíli se hello nové nastavení se zobrazí v seznamu nastavení pro tento prostředek a diagnostické protokoly jsou archivovaný toothat úložiště účet, jakmile se vygeneruje nová data události.

## <a name="archive-diagnostic-logs-via-azure-powershell"></a>Archiv diagnostických protokolů pomocí Azure PowerShell
```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Vlastnost | Požaduje se | Popis |
| --- | --- | --- |
| ID prostředku |Ano |ID prostředku hello prostředku, na kterém chcete tooset nastavení diagnostiky. |
| StorageAccountId |Ne |ID prostředku hello účet úložiště toowhich diagnostických protokolů má být uložen. |
| Kategorie |Ne |Textový soubor s oddělovači seznam kategorií tooenable protokolu. |
| Povoleno |Ano |Logická hodnota, která určuje, zda diagnostiky jsou zapnutá nebo vypnutá u tohoto prostředku. |
| RetentionEnabled |Ne |Logická hodnota, která udává, pokud jsou zásady uchovávání informací na tento prostředek povolené. |
| retentionInDays |Ne |Počet dní, pro které by měl být zachován události od 1 do 2147483647. Hodnota nula ukládá protokoly hello po neomezenou dobu. |

## <a name="archive-diagnostic-logs-via-hello-cross-platform-cli"></a>Diagnostické protokoly archivu prostřednictvím hello napříč platformami rozhraní příkazového řádku
```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Vlastnost | Požaduje se | Popis |
| --- | --- | --- |
| resourceId |Ano |ID prostředku hello prostředku, na kterém chcete tooset nastavení diagnostiky. |
| storageId |Ne |ID prostředku hello účet úložiště toowhich diagnostických protokolů má být uložen. |
| Kategorie |Ne |Textový soubor s oddělovači seznam kategorií tooenable protokolu. |
| povoleno |Ano |Logická hodnota, která určuje, zda diagnostiky jsou zapnutá nebo vypnutá u tohoto prostředku. |

## <a name="archive-diagnostic-logs-via-hello-rest-api"></a>Diagnostické protokoly archivu prostřednictvím hello REST API
[Najdete v tomto dokumentu](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) informace o tom, jak můžete nastavit pomocí hello REST API služby Azure monitorování diagnostické nastavení.

## <a name="schema-of-diagnostic-logs-in-hello-storage-account"></a>Schéma diagnostických protokolů v účtu úložiště hello
Jakmile jste nastavili archivace, kontejner úložiště se vytvoří v účtu úložiště hello, jakmile dojde k události v jedné z hello protokolu kategorií, které jste povolili. Hello objektů BLOB v kontejneru hello podle hello stejný formát napříč diagnostické protokoly a hello protokol aktivit. Struktura Hello tyto objekty BLOB je:

> insights - logs-{název kategorie protokolu} / resourceId = v odběry / {ID předplatného} /RESOURCEGROUPS/ {název skupiny prostředků} /PROVIDERS/ {název zprostředkovatele prostředků} / {resource type} nebo {název prostředku} / y = {čtyřciferné číselné year} / m = {dvoumístné číslo měsíce} / d = {letopočty numerický den} / h = {hour}/m=00/PT1H.json letopočty 24 hodin
> 
> 

Nebo více jednoduše

> insights - logs-{název kategorie protokolu} / resourceId = / {Id prostředku} / y = {čtyřciferné číselné year} / m = {dvoumístné číslo měsíce} / d = {letopočty numerický den} / h = {hour}/m=00/PT1H.json letopočty 24 hodin
> 
> 

Například může být název objektu blob:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
> 
> 

Každý objekt blob PT1H.json obsahuje objekt blob JSON událostí, které nastaly během hodiny hello zadaný v adrese URL objektu blob hello (například h = 12). Během hello přítomen hodinu události jsou připojením toohello PT1H.json souboru, kdy k nim dojde. Hello minutu hodnotu (m = 00) je vždy 00, protože protokolů diagnostiky události jsou rozdělená do jednotlivých objektů blob za hodinu.

V souboru PT1H.json hello se ukládají všechny události v poli záznamů"hello", následující tento formát:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Název elementu | Popis |
| --- | --- |
| time |Časové razítko při hello událostí vygenerovalo hello zpracování služby Azure hello žádost odpovídající hello událost. |
| resourceId |ID prostředku hello dopad prostředků. |
| operationName |Název operace hello. |
| category |Kategorie protokolu události hello. |
| properties |Sada `<Key, Value>` páry (tj. slovník) popisující hello podrobnosti události hello. |

> [!NOTE]
> Hello vlastnosti a využití tyto vlastnosti se může lišit v závislosti na hello prostředků.
> 
> 

## <a name="next-steps"></a>Další kroky
* [Stáhnout objekty BLOB pro analýzu](../storage/storage-dotnet-how-to-use-blobs.md)
* [Datový proud diagnostické protokoly obor názvů tooan Event Hubs](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Další informace o diagnostických protokolů](monitoring-overview-of-diagnostic-logs.md)
