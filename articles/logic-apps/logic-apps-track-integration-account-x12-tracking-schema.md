---
title: "schémata sledování aaaX12 pro monitorování B2B - Azure Logic Apps | Microsoft Docs"
description: "Použijte X12 sledování zpráv toomonitor B2B schémata z transakcí ve vašem účtu integrace Azure."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: a5413f80-eaad-4bcf-b371-2ad0ef629c3d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ed1b338730214dcae12c367ebff025d7122328fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="start-or-enable-tracking-of-x12-messages-toomonitor-success-errors-and-message-properties"></a>Počáteční nebo povolit sledování X12 zprávy toomonitor úspěch, chyb a vlastnosti zprávy
Můžete použít tyto X12 sledování schémata ve vašem účtu toohelp integrace se službou Azure sledování transakcí business-to-business (B2B):

* X12 transakce nastavení sledování schématu
* X12 transakce nastavit potvrzení sledování schématu
* X12 interchange sledování schématu
* X12 interchange potvrzení sledování schématu
* X12 funkční skupiny sledování schématu
* X12 funkční skupiny potvrzení sledování schématu

## <a name="x12-transaction-set-tracking-schema"></a>X12 transakce nastavení sledování schématu
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "transactionSetControlNumber": "",
                "CorrelationMessageId": "",
                "messageType": "",
                "isMessageFailed": "",
                "isTechnicalAcknowledgmentExpected": "",
                "isFunctionalAcknowledgmentExpected": "",
                "needAk2LoopForValidMessages":  "",
                "segmentsCount": ""
            }
    }
````

| Vlastnost | Typ | Popis |
| --- | --- | --- |
| senderPartnerName | Řetězec | X12 zpráva partnera název odesílatele. (Volitelné) |
| receiverPartnerName | Řetězec | X12 zpráva Název partnera společnosti příjemce. (Volitelné) |
| senderQualifier | Řetězec | Odešlete kvalifikátor partnera. (Povinné) |
| senderIdentifier | Řetězec | Odešlete identifikátor partnera. (Povinné) |
| receiverQualifier | Řetězec | Zobrazí kvalifikátor partnera. (Povinné) |
| receiverIdentifier | Řetězec | Zobrazí identifikátor partnera. (Povinné) |
| agreementName | Řetězec | Název hello X12 smlouvy toowhich hello zpráv se překládají. (Volitelné) |
| Směr | výčet | Směr toku zpráv hello, přijímat nebo odesílat. (Povinné) |
| interchangeControlNumber | Řetězec | Interchange číslo ovládacího prvku. (Volitelné) |
| functionalGroupControlNumber | Řetězec | Funkční řízení číslo. (Volitelné) |
| transactionSetControlNumber | Řetězec | Transakce nastavit počet ovládacího prvku. (Volitelné) |
| correlationMessageId | Řetězec | ID korelace zprávy. Kombinace {AgreementName} {*GroupControlNumber*} {TransactionSetControlNumber}. (Volitelné) |
| Typ zprávy | Řetězec | Transakce nastavit nebo typ dokumentu. (Volitelné) |
| isMessageFailed | Logická hodnota | Jestli hello X12 se nezdařilo. (Povinné) |
| isTechnicalAcknowledgmentExpected | Logická hodnota | Jestli je nakonfigurovaný technické potvrzení hello hello X12 smlouvy. (Povinné) |
| isFunctionalAcknowledgmentExpected | Logická hodnota | Jestli je nakonfigurovaný funkční potvrzení hello hello X12 smlouvy. (Povinné) |
| needAk2LoopForValidMessages | Logická hodnota | Jestli je vyžadován platný zprávy hello AK2 smyčky. (Povinné) |
| segmentsCount | Integer | Nastavit počet segmentů v hello X12 transakci. (Volitelné) |

## <a name="x12-transaction-set-acknowledgement-tracking-schema"></a>X12 transakce nastavit potvrzení sledování schématu
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "isaSegment": "",
                "gsSegment": "",
                "respondingfunctionalGroupControlNumber": "",
                "respondingFunctionalGroupId": "",
                "respondingtransactionSetControlNumber": "",
                "respondingTransactionSetId": "",
                "statusCode": "",
                "processingStatus": "",
                "CorrelationMessageId": ""
                "isMessageFailed": "",
                "ak2Segment": "",
                "ak3Segment": "",
                "ak5Segment": ""
            }
    }
````

| Vlastnost | Typ | Popis |
| --- | --- | --- |
| senderPartnerName | Řetězec | X12 zpráva partnera název odesílatele. (Volitelné) |
| receiverPartnerName | Řetězec | X12 zpráva Název partnera společnosti příjemce. (Volitelné) |
| senderQualifier | Řetězec | Odešlete kvalifikátor partnera. (Povinné) |
| senderIdentifier | Řetězec | Odešlete identifikátor partnera. (Povinné) |
| receiverQualifier | Řetězec | Zobrazí kvalifikátor partnera. (Povinné) |
| receiverIdentifier | Řetězec | Zobrazí identifikátor partnera. (Povinné) |
| agreementName | Řetězec | Název hello X12 smlouvy toowhich hello zpráv se překládají. (Volitelné) |
| Směr | výčet | Směr toku zpráv hello, přijímat nebo odesílat. (Povinné) |
| interchangeControlNumber | Řetězec | Interchange řízení počet potvrzení funkční hello. Hodnota naplní pouze pro straně odesílání hello kde hello zprávy odeslané toopartner přijato potvrzení funkční. (Volitelné) |
| functionalGroupControlNumber | Řetězec | Funkční skupiny řízení počet potvrzení funkční hello. Hodnota naplní pouze pro straně odesílání hello kde hello zprávy odeslané toopartner přijato potvrzení funkční. (Volitelné) |
| isaSegment | Řetězec | Segment ISA hello zprávy. Hodnota naplní pouze pro straně odesílání hello kde hello zprávy odeslané toopartner přijato potvrzení funkční. (Volitelné) |
| gsSegment | Řetězec | Segment GS hello zprávy. Hodnota naplní pouze pro straně odesílání hello kde hello zprávy odeslané toopartner přijato potvrzení funkční. (Volitelné) |
| respondingfunctionalGroupControlNumber | Řetězec | Neodpovídá výměnu řízení číslo. (Volitelné) |
| respondingFunctionalGroupId | Řetězec | Neodpovídá ID funkční skupiny, která mapuje tooAK101 v hello potvrzení. (Volitelné) |
| respondingtransactionSetControlNumber | Řetězec | Odpovídá transakce nastavit počet ovládacího prvku. (Volitelné) |
| respondingTransactionSetId | Řetězec | Odpovídá transakce nastavit ID, který mapuje tooAK201 v hello potvrzení. (Volitelné) |
| statusCode | Logická hodnota | Transakce nastavit potvrzení stavový kód. (Povinné) |
| segmentsCount | výčet | Potvrzení stavový kód. Povolené hodnoty jsou **platných**, **zamítnutý**, a **AcceptedWithErrors**. (Povinné) |
| StavZpracování | výčet | Stav zpracování hello potvrzení. Povolené hodnoty jsou **přijaté**, **generované**, a **odeslané**. (Povinné) |
| correlationMessageId | Řetězec | ID korelace zprávy. Kombinace {AgreementName} {*GroupControlNumber*} {TransactionSetControlNumber}. (Volitelné) |
| isMessageFailed | Logická hodnota | Jestli hello X12 se nezdařilo. (Povinné) |
| ak2Segment | Řetězec | Přijato potvrzení pro sadu transakce v rámci hello funkční skupiny. (Volitelné) |
| ak3Segment | Řetězec | Sestavy chyb v datového segmentu. (Volitelné) |
| ak5Segment | Řetězec | Určí, zda transakce hello nastavit identifikované v segmentu hello AK2 přijetí nebo zamítnutí a proto. (Volitelné) |

## <a name="x12-interchange-tracking-schema"></a>X12 interchange sledování schématu
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "isaSegment": "",
                "isTechnicalAcknowledgmentExpected": "",
                "isMessageFailed": "",
                "isa09": "",
                "isa10": "",
                "isa11": "",
                "isa12": "",
                "isa14": "",
                "isa15": "",
                "isa16": ""
            }
    }
````

| Vlastnost | Typ | Popis |
| --- | --- | --- |
| senderPartnerName | Řetězec | X12 zpráva partnera název odesílatele. (Volitelné) |
| receiverPartnerName | Řetězec | X12 zpráva Název partnera společnosti příjemce. (Volitelné) |
| senderQualifier | Řetězec | Odešlete kvalifikátor partnera. (Povinné) |
| senderIdentifier | Řetězec | Odešlete identifikátor partnera. (Povinné) |
| receiverQualifier | Řetězec | Zobrazí kvalifikátor partnera. (Povinné) |
| receiverIdentifier | Řetězec | Zobrazí identifikátor partnera. (Povinné) |
| agreementName | Řetězec | Název hello X12 smlouvy toowhich hello zpráv se překládají. (Volitelné) |
| Směr | výčet | Směr toku zpráv hello, přijímat nebo odesílat. (Povinné) |
| interchangeControlNumber | Řetězec | Interchange číslo ovládacího prvku. (Volitelné) |
| isaSegment | Řetězec | Segment ISA zprávy. (Volitelné) |
| isTechnicalAcknowledgmentExpected | Logická hodnota | Jestli je nakonfigurovaný technické potvrzení hello hello X12 smlouvy. (Povinné) |
| isMessageFailed | Logická hodnota | Jestli hello X12 se nezdařilo. (Povinné) |
| isa09 | Řetězec | X12 dokumentu výměnu datum. (Volitelné) |
| isa10 | Řetězec | X12 dokumentů výměnu čas. (Volitelné) |
| isa11 | Řetězec | X12 interchange řízení identifikátor standardů. (Volitelné) |
| isa12 | Řetězec | X12 interchange řízení číslo verze. (Volitelné) |
| isa14 | Řetězec | X12 vyžádání potvrzení. (Volitelné) |
| isa15 | Řetězec | Indikátor pro testovací nebo produkčního prostředí. (Volitelné) |
| isa16 | Řetězec | Element oddělovače. (Volitelné) |

## <a name="x12-interchange-acknowledgement-tracking-schema"></a>X12 interchange potvrzení sledování schématu
````java
    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "isaSegment": "",
                "respondingInterchangeControlNumber": "",
                "isMessageFailed": "",
                "statusCode": "",
                "processingStatus": "",
                "ta102": "",
                "ta103": "",
                "ta105": ""
            }
    }
````

| Vlastnost | Typ | Popis |
| --- | --- | --- |
| senderPartnerName | Řetězec | X12 zpráva partnera název odesílatele. (Volitelné) |
| receiverPartnerName | Řetězec | X12 zpráva Název partnera společnosti příjemce. (Volitelné) |
| senderQualifier | Řetězec | Odešlete kvalifikátor partnera. (Povinné) |
| senderIdentifier | Řetězec | Odešlete identifikátor partnera. (Povinné) |
| receiverQualifier | Řetězec | Zobrazí kvalifikátor partnera. (Povinné) |
| receiverIdentifier | Řetězec | Zobrazí identifikátor partnera. (Povinné) |
| agreementName | Řetězec | Název hello X12 smlouvy toowhich hello zpráv se překládají. (Volitelné) |
| Směr | výčet | Směr toku zpráv hello, přijímat nebo odesílat. (Povinné) |
| interchangeControlNumber | Řetězec | Interchange řízení počet hello technické potvrzení, které se získaly od partnerů. (Volitelné) |
| isaSegment | Řetězec | Segment ISA pro hello technické potvrzení, které se získaly od partnerů. (Volitelné) |
| respondingInterchangeControlNumber |Řetězec | Interchange číslo ovládací prvek pro hello technické potvrzení, které se získaly od partnerů. (Volitelné) |
| isMessageFailed | Logická hodnota | Jestli hello X12 se nezdařilo. (Povinné) |
| statusCode | výčet | Interchange potvrzení stavový kód. Povolené hodnoty jsou **platných**, **zamítnutý**, a **AcceptedWithErrors**. (Povinné) |
| StavZpracování | výčet | Stav potvrzení. Povolené hodnoty jsou **přijaté**, **generované**, a **odeslané**. (Povinné) |
| TA102 | Řetězec | Interchange datum. (Volitelné) |
| ta103 | Řetězec | Interchange čas. (Volitelné) |
| ta105 | Řetězec | Interchange Poznámka kódu. (Volitelné) |

## <a name="x12-functional-group-tracking-schema"></a>X12 funkční skupiny sledování schématu
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "gsSegment": "",
                "isTechnicalAcknowledgmentExpected": "",
                "isFunctionalAcknowledgmentExpected": "",
                "isMessageFailed": "",
                "gs01": "",
                "gs02": "",
                "gs03": "",
                "gs04": "",
                "gs05": "",
                "gs07": "",
                "gs08": ""
            }
    }
````

| Vlastnost | Typ | Popis |
| --- | --- | --- |
| senderPartnerName | Řetězec | X12 zpráva partnera název odesílatele. (Volitelné) |
| receiverPartnerName | Řetězec | X12 zpráva Název partnera společnosti příjemce. (Volitelné) |
| senderQualifier | Řetězec | Odešlete kvalifikátor partnera. (Povinné) |
| senderIdentifier | Řetězec | Odešlete identifikátor partnera. (Povinné) |
| receiverQualifier | Řetězec | Zobrazí kvalifikátor partnera. (Povinné) |
| receiverIdentifier | Řetězec | Zobrazí identifikátor partnera. (Povinné) |
| agreementName | Řetězec | Název hello X12 smlouvy toowhich hello zpráv se překládají. (Volitelné) |
| Směr | výčet | Směr toku zpráv hello, přijímat nebo odesílat. (Povinné) |
| interchangeControlNumber | Řetězec | Interchange číslo ovládacího prvku. (Volitelné) |
| functionalGroupControlNumber | Řetězec | Funkční řízení číslo. (Volitelné) |
| gsSegment | Řetězec | Zpráva GS segmentu. (Volitelné) |
| isTechnicalAcknowledgmentExpected | Logická hodnota | Jestli je nakonfigurovaný technické potvrzení hello hello X12 smlouvy. (Povinné) |
| isFunctionalAcknowledgmentExpected | Logická hodnota | Jestli je nakonfigurovaný funkční potvrzení hello hello X12 smlouvy. (Povinné) |
| isMessageFailed | Logická hodnota | Jestli hello X12 se nezdařilo. (Povinné)|
| gs01 | Řetězec | Funkční identifikačního kódu. (Volitelné) |
| gs02 | Řetězec | Kód aplikace odesílatele. (Volitelné) |
| gs03 | Řetězec | Kód aplikace příjemce. (Volitelné) |
| gs04 | Řetězec | Datum funkční skupiny. (Volitelné) |
| gs05 | Řetězec | Čas funkční skupiny. (Volitelné) |
| gs07 | Řetězec | Příslušné agentury kód. (Volitelné) |
| gs08 | Řetězec | Identifikátor verze nebo verze nebo odvětví kód. (Volitelné) |

## <a name="x12-functional-group-acknowledgement-tracking-schema"></a>X12 funkční skupiny potvrzení sledování schématu
````java
    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "isaSegment": "",
                "gsSegment": "",
                "respondingfunctionalGroupControlNumber": "",
                "respondingFunctionalGroupId": "",
                "isMessageFailed": "",
                "statusCode": "",
                "processingStatus": "",
                "ak903": "",
                "ak904": "",
                "ak9Segment": ""
            }
    }
````

| Vlastnost | Typ | Popis |
| --- | --- | --- |
| senderPartnerName | Řetězec | X12 zpráva partnera název odesílatele. (Volitelné) |
| receiverPartnerName | Řetězec | X12 zpráva Název partnera společnosti příjemce. (Volitelné) |
| senderQualifier | Řetězec | Odešlete kvalifikátor partnera. (Povinné) |
| senderIdentifier | Řetězec | Odešlete identifikátor partnera. (Povinné) |
| receiverQualifier | Řetězec | Zobrazí kvalifikátor partnera. (Povinné) |
| receiverIdentifier | Řetězec | Zobrazí identifikátor partnera. (Povinné) |
| agreementName | Řetězec | Název hello X12 smlouvy toowhich hello zpráv se překládají. (Volitelné) |
| Směr | výčet | Směr toku zpráv hello, přijímat nebo odesílat. (Povinné) |
| interchangeControlNumber | Řetězec | Výměnu řízení číslo, které naplňuje pro straně odesílání hello při přijetí technické potvrzení od partnerů. (Volitelné) |
| functionalGroupControlNumber | Řetězec | Po přijetí technické potvrzení od partnerů, funkční skupiny řízení počet technické potvrzení hello, který naplní pro hello odeslat straně. (Volitelné) |
| isaSegment | Řetězec | Stejné jako výměnu řídit číslo, ale vyplněná pouze v určitých případech. (Volitelné) |
| gsSegment | Řetězec | Stejná jako skupina funkčnosti řídit číslo, ale vyplněná pouze v určitých případech. (Volitelné) |
| respondingfunctionalGroupControlNumber | Řetězec | Ovládací prvek počet hello původní funkční skupiny. (Volitelné) |
| respondingFunctionalGroupId | Řetězec | Mapuje tooAK101 v ID hello potvrzení funkční skupiny. (Volitelné) |
| isMessageFailed | Logická hodnota | Jestli hello X12 se nezdařilo. (Povinné) |
| statusCode | výčet | Potvrzení stavový kód. Povolené hodnoty jsou **platných**, **zamítnutý**, a **AcceptedWithErrors**. (Povinné) |
| StavZpracování | výčet | Stav zpracování hello potvrzení. Povolené hodnoty jsou **přijaté**, **generované**, a **odeslané**. (Povinné) |
| ak903 | Řetězec | Počet přijatých sad transakce. (Volitelné) |
| ak904 | Řetězec | Počet transakcí sad akceptovány v hello identifikovat funkční skupiny. (Volitelné) |
| ak9Segment | Řetězec | Zda je funkční skupiny hello identifikovat v segmentu hello AK1 přijetí nebo odmítnutí a proč. (Volitelné) |

## <a name="next-steps"></a>Další kroky
* Další informace o [sledování zpráv B2B](logic-apps-monitor-b2b-message.md).
* Další informace o [schémata sledování AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md).
* Další informace o [B2B vlastní sledování schémata](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md).
* Další informace o [sledování zpráv B2B portálu Operations Management Suite hello](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
* Další informace o hello [Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md).  
