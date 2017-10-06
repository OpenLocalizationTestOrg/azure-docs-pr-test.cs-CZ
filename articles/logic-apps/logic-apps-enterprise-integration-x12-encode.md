---
title: "zprávy aaaEncode X12 – Azure Logic Apps | Microsoft Docs"
description: "Ověření EDI a převést kódováním XML zprávy s X12 zprávy kodér v hello Enterprise integrační balíček pro Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: a01e9ca9-816b-479e-ab11-4a984f10f62d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 785dbd2c7c82463154732921e07e3586d2307840
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encode-x12-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Kódování X12 zprávy pro Azure Logic Apps s hello Enterprise integračního balíčku

Hello kódovat X12 zpráva konektor může ověřit EDI a vlastnosti specifické pro partnery, převést kódováním XML zprávy EDI transakce sady v hello výměnu a vyžádat technických potvrzení, funkční potvrzení nebo obojí.
toouse tohoto konektoru, je nutné přidat tooan konektor hello existující aktivační události v aplikaci logiky.

## <a name="before-you-start"></a>Než začnete

Tady je hello položky, které budete potřebovat:

* Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
* [Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure. Musíte mít konektoru integrace účet toouse hello kódovat X12 zprávy.
* Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace
* [X12 smlouvy](logic-apps-enterprise-integration-x12.md) , již je definována v účtu integrace

## <a name="encode-x12-messages"></a>Kódování X12 zprávy

1. [Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).

2. Hello kódovat X12 zpráva konektor nemá aktivačních událostí, proto musíte přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku. V hello návrhář aplikace logiky přidejte aktivační události a poté přidejte logiku aplikace tooyour akce.

3.  Hello vyhledávacího pole zadejte "x12" filtru. Vyberte buď **X12-kódování zpráv tooX12 podle názvu smlouvy** nebo **X12-kódování zpráv tooX12 podle identity**.
   
    ![Vyhledejte "x12"](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. Pokud jste nevytvořili dříve všechna připojení tooyour integrace účet, se zobrazí výzva toocreate nyní toto připojení. Název připojení a vyberte účet hello integrace, které chcete tooconnect. 
   
    ![účet připojení integrace](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    Vyžadují se vlastnosti s hvězdičkou.

    | Vlastnost | Podrobnosti |
    | --- | --- |
    | Název připojení * |Zadejte libovolný název pro připojení. |
    | Integrace účet * |Zadejte název pro váš účet integrace. Ujistěte se, že integrace účet a logiku aplikace jsou v hello stejného umístění Azure. |

5.  Když jste hotovi, podrobné informace o připojení by měla vypadat podobně jako příklad toothis. toofinish vytváření připojení, zvolte **vytvořit**.

    ![Integrace připojení účet vytvořili](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    Připojení je nyní vytvořena.

    ![Podrobnosti připojení účtu integrace](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a>Kódování X12 zprávy podle název smlouvy

Pokud jste zvolili tooencode X12 zprávy název smlouvy, otevřete hello **název X12 smlouvy** seznamu, zadejte nebo vyberte vaše stávající X12 smlouvy. Zadejte tooencode zprávy XML hello.

![Zadejte X12 smlouvy, název a XML zprávy tooencode](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a>Kódování X12 zprávy podle identity

Pokud si zvolíte tooencode X12 zprávy pomocí identity, zadejte identifikátor odesílatele hello, kvalifikátor odesílatele, identifikátor příjemce a příjemce kvalifikátor jako nakonfigurovaný v vaší X12 smlouvy. Vyberte tooencode zprávy XML hello.
   
![Zadejte identit pro odesílatele a příjemce, vyberte tooencode zprávy XML](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a>X12 kódování podrobnosti

konektor kódovat Hello X12 provádí tyto úlohy:

* Smlouva řešení pomocí odpovídajících vlastností kontextu odesílatele a příjemce.
* Serializuje hello EDI výměnu, převodu kódu XML zprávy EDI transakce sady v hello výměnu.
* Použije sadu transakce hlavičku a přípojného segmenty
* Generuje představuje počet řízení výměnu, číslem skupiny řízení a číslem transakce sadu ovládacího prvku pro každý odchozí výměnu
* Nahradí oddělovače v datové části hello
* Ověří EDI a vlastnosti specifické pro partnery
  * Ověření schématu hello data transakce sady elementů na uvítací zprávu schématu
  * Ověření EDI provádět transakce set datové prvky.
  * Rozšířené ověření provádět transakce set datové prvky.
* Požádá o potvrzení o Technical nebo funkčnosti (Pokud je nakonfigurovaná).
  * Potvrzení o technické generuje v důsledku hlavičky ověření. Hello technické potvrzení sestavy hello stav zpracování hello výměnu záhlaví a přípojného podle hello adresa příjemce
  * Potvrzení o funkční generuje v důsledku ověření obsahu. Hello funkční potvrzení sestavy každou při došlo k chybě zpracování hello přijatých dokumentu

## <a name="view-hello-swagger"></a>Zobrazení hello swagger
V tématu hello [swagger podrobnosti](/connectors/x12/). 

## <a name="next-steps"></a>Další kroky
[Další informace o hello Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

