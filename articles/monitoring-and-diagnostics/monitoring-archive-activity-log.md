---
title: "aaaArchive hello protokol činnosti Azure | Microsoft Docs"
description: "Zjistěte, jak tooarchive si Azure aktivitu protokolu pro dlouhodobé uchovávání v účtu úložiště."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: johnkem
ms.openlocfilehash: 58c6d3a3a31398287f66f76999d48f2942ab5109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="archive-hello-azure-activity-log"></a>Archiv hello protokol činnosti Azure
V tomto článku jsme ukazují, jak můžete použít hello portálu Azure, rutiny prostředí PowerShell nebo rozhraní příkazového řádku a platformy tooarchive vaše [ **protokol činnosti Azure** ](monitoring-overview-activity-logs.md) v účtu úložiště. Tato možnost je užitečná, pokud byste chtěli tooretain delší než 90 dní (s plnou kontrolu nad zásady uchovávání informací hello) pro aktivitu protokolu auditování, statické analýzy, nebo zálohování. Pokud potřebujete tooretain události za 90 dní nebo méně. můžete nemusí tooset archivace tooa účet úložiště, protože aktivity protokolu události se zachovají v hello platformy Azure za 90 dnů bez povolení archivace.

## <a name="prerequisites"></a>Požadavky
Než začnete, je třeba příliš[vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) toowhich archivujete protokolu aktivit. Důrazně doporučujeme stávající účet úložiště, který má jiné, -monitoring data v ní uloženy, takže může lépe řídit přístup k datům toomonitoring nepoužívejte. Ale pokud jsou také archivace diagnostické protokoly a účet úložiště tooa metriky, může být smysl toouse daný účet úložiště pro vaše aktivity protokolu také tookeep všechna monitorování data v centrálním umístění. Hello účet úložiště, které používáte, musí být účet úložiště obecné účely, nikoli účet úložiště objektů blob. účet úložiště Hello nemá toobe v hello stejnému předplatnému jako hello předplatné emitování protokoly tak dlouho, dokud hello uživatel, který konfiguruje nastavení hello má příslušné předplatné tooboth přístupu RBAC.

## <a name="log-profile"></a>Profil protokolu
tooarchive hello protokol aktivit pomocí metod hello níže, nastavíte hello **profil protokolu** pro předplatné. Hello profil protokolu definuje hello typ události, které jsou uložené nebo prostřednictvím datového proudu a hello výstupy – úložiště účet nebo události rozbočovače. Definuje také zásady uchovávání informací hello (počet dní tooretain) pro události, které jsou uložené v účtu úložiště. Pokud zásady uchovávání informací hello nastavena toozero, události jsou uloženy po neomezenou dobu. Jinak to je možné nastavit tooany hodnotu od 1 do 2147483647. Zásady uchovávání informací jsou použité za den, tak při hello Konec dne (UTC), zaprotokoluje hello dni, který je teď nad rámec zásad uchovávání informací hello budou odstraněny. Například pokud jste měli zásady uchovávání informací jeden den, od začátku hello dnešní den hello hello protokoly z hello den před včerejšek by odstraněn. [Další informace o protokolu profily zde](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile). 

## <a name="archive-hello-activity-log-using-hello-portal"></a>Archiv hello protokol aktivit portálu hello
1. Hello portálu, klikněte na tlačítko hello **protokol aktivit** na hello levé navigační odkaz. Pokud nevidíte odkaz pro hello protokol aktivit, klikněte na tlačítko hello **více služeb** nejprve odkaz.
   
    ![Přejděte tooActivity protokolu okno](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. V horní části hello hello okna, klikněte na tlačítko **exportovat**.
   
    ![Klikněte na tlačítko pro Export hello](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. V okně hello, který se zobrazí, zaškrtněte políčko hello pro **exportovat účet úložiště tooa** a vyberte účet úložiště.
   
    ![Nastavit účet úložiště](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Pomocí posuvníku hello nebo textové pole, zadejte počet dní, pro které by měly být udržovány aktivity protokolu události v účtu úložiště. Pokud dáváte přednost toohave data uchovávané v účtu úložiště hello bez omezení, nastavte toto číslo toozero.
5. Klikněte na **Uložit**.

## <a name="archive-hello-activity-log-via-powershell"></a>Archiv hello protokol aktivit pomocí prostředí PowerShell
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Vlastnost | Požaduje se | Popis |
| --- | --- | --- |
| StorageAccountId |Ne |ID prostředku hello účet úložiště toowhich protokoly aktivity má být uložen. |
| Umístění |Ano |Seznam oddělený čárkami oblastí, pro které byste chtěli toocollect aktivity protokolu události. Můžete zobrazit seznam všech oblastech [této stránce](https://azure.microsoft.com/en-us/regions) nebo pomocí [hello REST API pro správu Azure](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| retentionInDays |Ano |Počet dní pro události, které by měl být zachován, od 1 do 2147483647. Hodnota nula ukládá protokoly hello po neomezenou dobu (navždy). |
| Kategorie |Ano |Seznam oddělený čárkami kategorií událostí, které by měl být shromážděny. Možné hodnoty jsou zápisu, odstranění a akce. |

## <a name="archive-hello-activity-log-via-cli"></a>Archiv hello protokol aktivit prostřednictvím rozhraní příkazového řádku
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Vlastnost | Požaduje se | Popis |
| --- | --- | --- |
| jméno |Ano |Název vašeho profilu protokolu. |
| storageId |Ne |ID prostředku hello účet úložiště toowhich protokoly aktivity má být uložen. |
| Umístění |Ano |Seznam oddělený čárkami oblastí, pro které byste chtěli toocollect aktivity protokolu události. Můžete zobrazit seznam všech oblastech [této stránce](https://azure.microsoft.com/en-us/regions) nebo pomocí [hello REST API pro správu Azure](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| retentionInDays |Ano |Počet dní pro události, které by měl být zachován, od 1 do 2147483647. Hodnota nula budou ukládat protokoly hello po neomezenou dobu (navždy). |
| Kategorie |Ano |Seznam oddělený čárkami kategorií událostí, které by měl být shromážděny. Možné hodnoty jsou zápisu, odstranění a akce. |

## <a name="storage-schema-of-hello-activity-log"></a>Schéma úložiště hello protokol aktivit
Jakmile jste nastavili archivace, kontejner úložiště bude vytvořen v účtu úložiště hello Jakmile dojde k aktivity protokolu události. Hello objektů BLOB v kontejneru hello podle hello stejný formát napříč hello protokol aktivit a diagnostické protokoly. Struktura Hello tyto objekty BLOB je:

> Statistika provozní protokoly/name = výchozí/resourceId = v odběry / {ID předplatného} / y = {čtyřciferné číselné year} / m = {dvoumístné číslo měsíce} / d = {letopočty numerický den} / h = {hour}/m=00/PT1H.json letopočty 24 hodin
> 
> 

Například může být název objektu blob:

> insights-Operational-Logs/Name=default/resourceId=/Subscriptions/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON
> 
> 

Každý objekt blob PT1H.json obsahuje objekt blob JSON událostí, které nastaly během hodiny hello zadaný v adrese URL objektu blob hello (například h = 12). Během hello přítomen hodinu události jsou připojením toohello PT1H.json souboru, kdy k nim dojde. Hello minutu hodnotu (m = 00) je vždy 00, protože aktivity protokolu události jsou rozdělená do jednotlivých objektů blob za hodinu.

V souboru PT1H.json hello se ukládají všechny události v poli záznamů"hello", následující tento formát:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
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
| category |Kategorie hello akce, např. Zápis, čtení, akce. |
| resultType |Typ výsledku hello Hello např. Úspěch, chyba, spuštění |
| resultSignature |Závisí na typu prostředku hello. |
| durationMs |Doba trvání operace hello v milisekundách |
| callerIpAddress |IP adresa hello uživatele, který se má provést operaci hello, deklarace nebo SPN deklarace identity na základě dostupnosti. |
| correlationId |Obvykle GUID ve formátu řetězce hello. Události, které sdílejí correlationId patří toohello stejné uber akce. |
| identity |Objekt blob JSON s popisem hello autorizace a deklarace identity. |
| Autorizace |Objekt BLOB RBAC vlastností události hello. Obvykle zahrnuje vlastnosti "action", "role" a "obor" hello. |
| úroveň |Úroveň události hello. Jeden z hello následující hodnoty: "Kritická", "Chyba", "Upozornění", "Informační" a "Podrobné" |
| location |Oblast, ve které hello umístění k chybě (nebo globální). |
| properties |Sada `<Key, Value>` páry (tj. slovník) popisující hello podrobnosti události hello. |

> [!NOTE]
> Hello vlastnosti a využití tyto vlastnosti se může lišit v závislosti na hello prostředků.
> 
> 

## <a name="next-steps"></a>Další kroky
* [Stáhnout objekty BLOB pro analýzu](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
* [Stream hello protokol aktivit tooEvent rozbočovače](monitoring-stream-activity-logs-event-hubs.md)
* [Další informace o hello protokol aktivit](monitoring-overview-activity-logs.md)

