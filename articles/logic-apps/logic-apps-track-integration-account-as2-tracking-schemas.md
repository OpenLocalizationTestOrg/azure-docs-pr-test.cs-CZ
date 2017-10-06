---
title: "schémata sledování aaaAS2 pro monitorování B2B - Azure Logic Apps | Microsoft Docs"
description: "Použít AS2 sledování zpráv toomonitor B2B schémata z transakcí ve vašem účtu integrace Azure."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f169c411-1bd7-4554-80c1-84351247bf94
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fe3c5845e2e80160d6857d8c308d836e88af7331
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="start-or-enable-tracking-of-as2-messages-and-mdns-toomonitor-success-errors-and-message-properties"></a>Spuštění nebo povolení sledování AS2 zprávy a MDNs toomonitor úspěch, chyb a vlastnosti zprávy
Můžete použít tyto schémata sledování AS2 ve vašem účtu toohelp integrace se službou Azure sledování transakcí business-to-business (B2B):

* Schéma sledování AS2 zpráv
* Schéma sledování AS2 MDN

## <a name="as2-message-tracking-schema"></a>Schéma sledování AS2 zpráv
````java

    {
       "agreementProperties": {  
            "senderPartnerName": "",  
            "receiverPartnerName": "",  
            "as2To": "",  
            "as2From": "",  
            "agreementName": ""  
        },  
        "messageProperties": {
            "direction": "",
            "messageId": "",
            "dispositionType": "",
            "fileName": "",
            "isMessageFailed": "",
            "isMessageSigned": "",
            "isMessageEncrypted": "",
            "isMessageCompressed": "",
            "correlationMessageId": "",
            "incomingHeaders": {
            },
            "outgoingHeaders": {
            },
        "isNrrEnabled": "",
        "isMdnExpected": "",
        "mdnType": ""
        }
    }
````

| Vlastnost | Typ | Popis |
| --- | --- | --- |
| senderPartnerName | Řetězec | Název odesílatele zprávy AS2 partnera. (Volitelné) |
| receiverPartnerName | Řetězec | Příjemce zprávu AS2 název partnera. (Volitelné) |
| as2To | Řetězec | Příjemce zprávu AS2 název z hlavičky hello hello AS2 zprávy. (Povinné) |
| as2From | Řetězec | Název odesílatele zprávy AS2 z hlavičky hello hello AS2 zprávy. (Povinné) |
| agreementName | Řetězec | Název hello AS2 smlouvy toowhich hello zpráv se překládají. (Volitelné) |
| Směr | Řetězec | Směr toku zpráv hello, přijímat nebo odesílat. (Povinné) |
| messageId | Řetězec | ID zprávy AS2, z hlavičky hello hello AS2 zprávy (volitelné) |
| dispositionType |Řetězec | Hodnota typu dispozice zpráva dispozice oznámení (MDN). (Volitelné) |
| fileName | Řetězec | Název souboru z hlavičky hello hello AS2 zprávy. (Volitelné) |
| isMessageFailed |Logická hodnota | Tom, zda text hello AS2 zpráv se nezdařilo. (Povinné) |
| isMessageSigned | Logická hodnota | Jestli hello AS2 zpráva byla podepsána. (Povinné) |
| isMessageEncrypted | Logická hodnota | Jestli hello AS2 zpráva byla zašifrována. (Povinné) |
| isMessageCompressed |Logická hodnota | Jestli byla komprimována hello AS2 zprávy. (Povinné) |
| correlationMessageId | Řetězec | ID zprávy AS2, toocorrelate zprávy s MDNs. (Volitelné) |
| incomingHeaders |Slovník JToken | Podrobnosti záhlaví zprávy příchozí AS2. (Volitelné) |
| outgoingHeaders |Slovník JToken | Odchozí AS2 podrobnosti záhlaví zprávy. (Volitelné) |
| isNrrEnabled | Logická hodnota | Použijte výchozí hodnotu, pokud hodnota hello není znám. (Povinné) |
| isMdnExpected | Logická hodnota | Použijte výchozí hodnotu, pokud hodnota hello není znám. (Povinné) |
| mdnType | výčet | Povolené hodnoty jsou **NotConfigured**, **synchronizace**, a **asynchronní**. (Povinné) |

## <a name="as2-mdn-tracking-schema"></a>Schéma sledování AS2 MDN
````java

    {
        "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "as2To": "",
                "as2From": "",
                "agreementName": "g"
            },
            "messageProperties": {
                "direction": "",
                "messageId": "",
                "originalMessageId": "",
                "dispositionType": "",
                "isMessageFailed": "",
                "isMessageSigned": "",
                "isNrrEnabled": "",
                "statusCode": "",
                "micVerificationStatus": "",
                "correlationMessageId": "",
                "incomingHeaders": {
                },
                "outgoingHeaders": {
                }
            }
    }
````

| Vlastnost | Typ | Popis |
| --- | --- | --- |
| senderPartnerName | Řetězec | Název odesílatele zprávy AS2 partnera. (Volitelné) |
| receiverPartnerName | Řetězec | Příjemce zprávu AS2 název partnera. (Volitelné) |
| as2To | Řetězec | Název partnera, který přijme zprávu hello AS2. (Povinné) |
| as2From | Řetězec | Název partnera, který odešle zprávu hello AS2. (Povinné) |
| agreementName | Řetězec | Název hello AS2 smlouvy toowhich hello zpráv se překládají. (Volitelné) |
| Směr |Řetězec | Směr toku zpráv hello, přijímat nebo odesílat. (Povinné) |
| messageId | Řetězec | ID AS2 zprávy. (Volitelné) |
| OriginalMessageId |Řetězec | ID AS2 původní zprávy. (Volitelné) |
| dispositionType | Řetězec | Hodnota typu MDN dispozice. (Volitelné) |
| isMessageFailed |Logická hodnota | Tom, zda text hello AS2 zpráv se nezdařilo. (Povinné) |
| isMessageSigned |Logická hodnota | Jestli hello AS2 zpráva byla podepsána. (Povinné) |
| isNrrEnabled | Logická hodnota | Použijte výchozí hodnotu, pokud hodnota hello není znám. (Povinné) |
| statusCode | výčet | Povolené hodnoty jsou **platných**, **zamítnutý**, a **AcceptedWithErrors**. (Povinné) |
| micVerificationStatus | výčet | Povolené hodnoty jsou **NotApplicable**, **úspěšné**, a **se nezdařilo**. (Povinné) |
| correlationMessageId | Řetězec | ID korelace. Hello původní messaged ID (ID zprávy hello zprávy, pro který je nakonfigurovaný MDN hello). (Volitelné) |
| incomingHeaders | Slovník JToken | Určuje podrobnosti záhlaví příchozí zprávy. (Volitelné) |
| outgoingHeaders |Slovník JToken | Určuje odchozí podrobnosti záhlaví zprávy. (Volitelné) |

## <a name="next-steps"></a>Další kroky
* Další informace o hello [Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md).    
* Další informace o [sledování zpráv B2B](logic-apps-monitor-b2b-message.md).   
* Další informace o [B2B vlastní sledování schémata](logic-apps-track-integration-account-custom-tracking-schema.md).   
* Další informace o [X12 sledování schémata](logic-apps-track-integration-account-x12-tracking-schema.md).   
* Další informace o [sledování zpráv B2B portálu Operations Management Suite hello](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
