---
title: "zprávy aaaDecode X12 – Azure Logic Apps | Microsoft Docs"
description: "Ověření EDI a generování potvrzení s hello X12 zpráva decoder v hello Enterprise integrační balíček pro Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4fd48d2d-2008-4080-b6a1-8ae183b48131
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 1ffececca1ff835b319b64c85f86c421395833c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="decode-x12-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Dekódovat X12 zprávy pro Azure Logic Apps s hello Enterprise integračního balíčku

S hello dekódování X12 zpráva konektor můžete ověřit hello obálky proti smlouvy s obchodním partnerem, ověření EDI a vlastnosti specifické pro partnery, rozdělení mimoúrovňové křižovatky do nastaví transakce nebo zachovat celý mimoúrovňové křižovatky a generovat potvrzování pro zpracování transakcí. toouse tohoto konektoru, je nutné přidat tooan konektor hello existující aktivační události v aplikaci logiky.

## <a name="before-you-start"></a>Než začnete

Tady je hello položky, které budete potřebovat:

* Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
* [Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure. Musíte mít konektoru integrace účet toouse hello dekódování X12 zprávy.
* Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace
* [X12 smlouvy](logic-apps-enterprise-integration-x12.md) , již je definována v účtu integrace

## <a name="decode-x12-messages"></a>Dekódovat X12 zprávy

1. [Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).

2. Hello dekódování X12 zpráva konektor nemá, aktivační události, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku. V hello návrhář aplikace logiky přidejte aktivační události a poté přidejte logiku aplikace tooyour akce.

3.  Hello vyhledávacího pole zadejte "x12" filtru. Vyberte **X12-dekódovat X12 zpráva**.
   
    ![Vyhledejte "x12"](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. Pokud jste nevytvořili dříve všechna připojení tooyour integrace účet, se zobrazí výzva toocreate nyní toto připojení. Název připojení a vyberte účet hello integrace, které chcete tooconnect. 

    ![Zadejte podrobnosti připojení účtu integrace](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    Vyžadují se vlastnosti s hvězdičkou.

    | Vlastnost | Podrobnosti |
    | --- | --- |
    | Název připojení * |Zadejte libovolný název pro připojení. |
    | Integrace účet * |Zadejte název pro váš účet integrace. Ujistěte se, že integrace účet a logiku aplikace jsou v hello stejného umístění Azure. |

5.  Když jste hotovi, podrobné informace o připojení by měla vypadat podobně jako příklad toothis. toofinish vytváření připojení, zvolte **vytvořit**.
   
    ![Podrobnosti připojení účtu integrace](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. Po vytvoření připojení, jak je znázorněno v tomto příkladu, vyberte hello X12 plochý soubor zprávy toodecode.

    ![Integrace připojení účet vytvořili](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    Například:

    ![Vyberte X12 s plochou zpráva souboru pro dekódování](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

## <a name="x12-decode-details"></a>X12 dekódovat podrobnosti

konektor dekódování Hello X12 provádí tyto úlohy:

* Ověří hello obálky proti obchodování smlouvu pro partnery
* Ověří EDI a vlastnosti specifické pro partnery
  * Ověření strukturální EDI a rozšířeného schématu ověření
  * Ověření struktury hello obálky výměnu hello.
  * Ověření schématu hello obálky pro schéma řízení hello.
  * Ověření schématu elementů hello transakce sady dat pro schéma zpráva hello.
  * Ověření EDI provádět transakce set datové prvky. 
* Ověřuje, že hello výměnu, skupiny a transakce sadu řízení čísla nejsou duplicitní
  * Zkontroluje hello výměnu řízení číslo proti dříve přijaté mimoúrovňové křižovatky.
  * Zkontroluje hello skupiny řízení číslo pro jiné skupiny řízení čísel ve hello výměnu.
  * Ověří, že transakce hello nastavit počet ovládacího prvku na jiné transakci sadu řízení čísla v této skupině.
* Rozdělí hello výměnu do nastaví transakce, nebo zachovává celý výměnu hello:
  * Rozdělení výměnu jako sady transakce – pozastavení sady transakce Chyba: výměnu rozdělení do transakce nastaví a analyzuje každá sada transakce. 
  Akce dekódování Hello X12 výstupy pouze transakce sady, které nesplní ověření příliš`badMessages`a výstupy hello zbývající transakce se nastaví příliš`goodMessages`.
  * Rozdělení výměnu jako sady transakce – pozastavení výměnu o chybě: výměnu rozdělení do transakce nastaví a analyzuje každá sada transakce. 
  Pokud jeden nebo více transakcí nastaví hello výměnu selhání ověřování, hello X12 dekódování akce vypíše všechny transakce hello nastaví v tomto výměnu příliš`badMessages`.
  * Zachovat výměnu – pozastavení sady transakce o chybě: proces a zachovat hello výměnu hello celý dávkové výměnu. 
  Akce dekódování Hello X12 výstupy pouze transakce sady, které nesplní ověření příliš`badMessages`a výstupy hello zbývající transakce se nastaví příliš`goodMessages`.
  * Zachovat výměnu – pozastavení výměnu o chybě: proces a zachovat hello výměnu hello celý dávkové výměnu. 
  Pokud jeden nebo více transakcí nastaví hello výměnu selhání ověřování, hello X12 dekódování akce vypíše všechny transakce hello nastaví v tomto výměnu příliš`badMessages`. 
* Generuje Technical nebo funkčnosti potvrzení (Pokud je nakonfigurovaná).
  * Potvrzení o technické generuje v důsledku hlavičky ověření. Hello technické potvrzení sestavy hello stav zpracování hello výměnu záhlaví a přípojného podle hello adresu příjemce.
  * Potvrzení o funkční generuje v důsledku ověření obsahu. Hello funkční potvrzení sestavy každou při došlo k chybě zpracování hello přijatých dokumentu

## <a name="view-hello-swagger"></a>Zobrazení hello swagger
V tématu hello [swagger podrobnosti](/connectors/x12/). 

## <a name="next-steps"></a>Další kroky
[Další informace o hello Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

