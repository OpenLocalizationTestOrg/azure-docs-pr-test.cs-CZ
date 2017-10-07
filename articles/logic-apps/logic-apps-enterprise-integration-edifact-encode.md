---
title: "aaaEncode EDIFACT zprávy – Azure Logic Apps | Microsoft Docs"
description: "Ověření EDI a vygenerování XML pomocí kodéru zpráv EDIFACT v hello Enterprise integrační balíček pro Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 974ac339-d97a-4715-bc92-62d02281e900
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: b3799dbd2492adef597022d017cf28f5ceb1085c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Kódování zprávy EDIFACT pro Azure Logic Apps s hello Enterprise integračního balíčku

Hello EDIFACT kódování zpráv konektor může ověřit EDI a vlastnosti specifické pro partnery, Generovat dokument XML pro každou sadu transakce a vyžádat technických potvrzení, funkční potvrzení nebo obojí.
toouse tohoto konektoru, je nutné přidat tooan konektor hello existující aktivační události v aplikaci logiky.

## <a name="before-you-start"></a>Než začnete

Tady je hello položky, které budete potřebovat:

* Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
* [Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure. Musíte mít konektoru integrace účet toouse hello EDIFACT kódování zprávy. 
* Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace
* [Smlouvy EDIFACT](logic-apps-enterprise-integration-edifact.md) , již je definována v účtu integrace

## <a name="encode-edifact-messages"></a>Kódování zprávy EDIFACT

1. [Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).

2. Hello EDIFACT kódování zpráv konektor nemá, aktivační události, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku. V hello návrhář aplikace logiky přidejte aktivační události a poté přidejte logiku aplikace tooyour akce.

3.  Hello vyhledávacího pole zadejte "EDIFACT" jako filtr. Vyberte buď **zakódovat zprávu EDIFACT název smlouvy** nebo **kódovat tooEDIFACT zprávu identity**.
   
    ![hledání EDIFACT](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. Pokud jste nevytvořili dříve všechna připojení tooyour integrace účet, se zobrazí výzva toocreate nyní toto připojení. Název připojení a vyberte účet hello integrace, které chcete tooconnect.

    ![Vytvoření připojení účtu integrace](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    Vyžadují se vlastnosti s hvězdičkou.

    | Vlastnost | Podrobnosti |
    | --- | --- |
    | Název připojení * |Zadejte libovolný název pro připojení. |
    | Integrace účet * |Zadejte název pro váš účet integrace. Ujistěte se, že integrace účet a logiku aplikace jsou v hello stejného umístění Azure. |

5.  Když jste hotovi, podrobné informace o připojení by měla vypadat podobně jako příklad toothis. toofinish vytváření připojení, zvolte **vytvořit**.

    ![Podrobnosti připojení účtu integrace](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    Připojení je nyní vytvořena.

    ![Integrace připojení účet vytvořili](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a>Kódování zprávy EDIFACT podle název smlouvy

Pokud jste zvolili tooencode EDIFACT zprávy pomocí název smlouvy, otevřete hello **název EDIFACT smlouvy** seznamu, zadejte nebo vyberte název smlouvy EDIFACT. Zadejte tooencode zprávy XML hello.

![Zadejte název smlouvy EDIFACT a tooencode zprávy XML](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a>Kódování zprávy EDIFACT podle identity

Pokud si zvolíte tooencode EDIFACT zprávy pomocí identity, zadejte identifikátor odesílatele hello, kvalifikátor odesílatel, příjemce identifikátor a příjemce kvalifikátor jako nakonfigurovaný v vaší smlouvy EDIFACT. Vyberte tooencode zprávy XML hello.

![Zadejte identit pro odesílatele a příjemce, vyberte tooencode zprávy XML](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a>Kódování EDIFACT podrobnosti

konektor kódování EDIFACT Hello provádí tyto úlohy: 

* Hello smlouvu můžete vyřešit odpovídající hello odesílatele kvalifikátor & identifikátor a kvalifikátor příjemce a identifikátor
* Serializuje hello EDI výměnu, převodu kódu XML zprávy EDI transakce sady v hello výměnu.
* Použije sadu transakce hlavičku a přípojného segmenty
* Generuje představuje počet řízení výměnu, číslem skupiny řízení a číslem transakce sadu ovládacího prvku pro každý odchozí výměnu
* Nahradí oddělovače v datové části hello
* Ověří EDI a vlastnosti specifické pro partnery
  * Ověření schématu elementů hello transakce sady dat pro schéma zpráva hello.
  * Ověření EDI provádět transakce set datové prvky.
  * Rozšířené ověření provádět transakce set datové prvky.
* Vygeneruje dokument XML pro každou sadu transakce.
* Požádá o potvrzení o Technical nebo funkčnosti (Pokud je nakonfigurovaná).
  * Jako potvrzení o technické hello CONTRL zpráva znamená přijetí výměnu.
  * Jako potvrzení o funkční hello CONTRL zpráva znamená přijetí nebo odmítnutí výměnu hello přijata, skupiny nebo zprávy, se seznamem chyby nebo nepodporované funkce

## <a name="view-swagger-file"></a>Soubor Swagger zobrazení
Podrobnosti Swagger hello tooview hello EDIFACT konektor, najdete v tématu [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Další kroky
[Další informace o hello Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

