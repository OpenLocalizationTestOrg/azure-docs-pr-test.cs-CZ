---
title: "aaaCustom sledování schémata B2B monitorování - Azure Logic Apps | Microsoft Docs"
description: "Vytvořte vlastní sledování schémata toomonitor B2B zprávy z transakce ve vašem účtu integrace Azure."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8cf26a43d89f0414a2a8c5ef59d804235afeb5d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-tracking-toomonitor-your-complete-workflow-end-to-end"></a>Povolit sledování toomonitor dokončení pracovního postupu, klient server
Je integrované sledování, že můžete povolit pro různé části pracovního postupu business-to-business, jako je například sledování AS2 nebo X12 zprávy. Když vytvoříte pracovní postupy, které zahrnuje aplikace logiky, BizTalk Server, SQL Server nebo jinou vrstvu, můžete povolit vlastní sledování, který protokoluje události od hello začátku toohello konce pracovní postup. 

Toto téma obsahuje vlastní kód, který můžete použít v hello vrstvy mimo aplikaci logiky. 

## <a name="custom-tracking-schema"></a>Schéma vlastní sledování
````java

        {
            "sourceType": "",
            "source": {

            "workflow": {
                "systemId": ""
            },
            "runInstance": {
                "runId": ""
            },
            "operation": {
                "operationName": "",
                "repeatItemScopeName": "",
                "repeatItemIndex": "",
                "trackingId": "",
                "correlationId": "",
                "clientRequestId": ""
                }
            },
            "events": [
            {
                "eventLevel": "",
                "eventTime": "",
                "recordType": "",
                "record": {                
                }
            }
         ]
      }

````

| Vlastnost | Typ | Popis |
| --- | --- | --- |
| SourceType |   | Typ zdroje hello spustit. Povolené hodnoty jsou **Microsoft.Logic/workflows** a **vlastní**. (Povinné) |
| Zdroj |   | Pokud je typ zdroje hello **Microsoft.Logic/workflows**, informace o zdroji hello musí toofollow toto schéma. Pokud je typ zdroje hello **vlastní**, schéma hello je JToken. (Povinné) |
| ID systému | Řetězec | ID logiku aplikace systému. (Povinné) |
| identifikátor runId | Řetězec | Aplikace logiky spustit ID. (Povinné) |
| operationName | Řetězec | Název hello operace (například akce nebo aktivační událost). (Povinné) |
| repeatItemScopeName | Řetězec | Je-li akce hello je uvnitř opakujte název položky `foreach` / `until` smyčky. (Povinné) |
| repeatItemIndex | Integer | Jestli je akce hello uvnitř `foreach` / `until` smyčky. Určuje index opakovaných položky hello. (Povinné) |
| trackingId | Řetězec | ID sledování toocorrelate hello zprávy. (Volitelné) |
| correlationId | Řetězec | ID korelace, toocorrelate hello zprávy. (Volitelné) |
| clientRequestId | Řetězec | Klient jej můžete naplnit toocorrelate zprávy. (Volitelné) |
| eventLevel |   | Úroveň události hello. (Povinné) |
| eventTime |   | Čas události hello ve formátu RRRR-MM-DDTHH:MM:SS.00000Z UTC. (Povinné) |
| recordType |   | Typ záznamu sledovat hello. Povolená hodnota je **vlastní**. (Povinné) |
| záznam |   | Vlastní typ záznamu. Povolený formát je JToken. (Povinné) |

## <a name="b2b-protocol-tracking-schemas"></a>Schémata sledování protokol B2B
Informace o protokolu B2B sledování schémata najdete v tématu:
* [Schémata sledování AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [Schémata sledování X12](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Další kroky
* Další informace o [sledování zpráv B2B](logic-apps-monitor-b2b-message.md).   
* Další informace o [sledování zpráv B2B portálu Operations Management Suite hello](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
* Další informace o hello [Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md).
