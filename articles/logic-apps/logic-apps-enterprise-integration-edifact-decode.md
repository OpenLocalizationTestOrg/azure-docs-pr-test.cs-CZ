---
title: "aaaDecode EDIFACT zprávy – Azure Logic Apps | Microsoft Docs"
description: "Ověření EDI a generování potvrzení s decoder zpráva hello EDIFACT v hello Enterprise integrační balíček pro Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 0e61501d-21a2-4419-8c6c-88724d346e81
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 94faebdec4e4ffc8ad76ad1609495ddf9f002250
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Dekódovat EDIFACT zprávy pro Azure Logic Apps s hello Enterprise integračního balíčku

Hello dekódovat EDIFACT zpráva konektor může ověřit EDI a vlastnosti specifické pro partnery, rozdělení mimoúrovňové křižovatky do nastaví transakce nebo zachovat celý mimoúrovňové křižovatky a generovat potvrzování pro zpracování transakcí. toouse tohoto konektoru, je nutné přidat tooan konektor hello existující aktivační události v aplikaci logiky.

## <a name="before-you-start"></a>Než začnete

Tady je hello položky, které budete potřebovat:

* Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
* [Integrace účet](logic-apps-enterprise-integration-create-integration-account.md) který již má definovaný a přidružené k předplatnému Azure. Musíte mít konektoru integrace účet toouse hello dekódovat EDIFACT zprávy. 
* Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace
* [Smlouvy EDIFACT](logic-apps-enterprise-integration-edifact.md) , již je definována v účtu integrace

## <a name="decode-edifact-messages"></a>Dekódovat EDIFACT zprávy

1. [Vytvoření aplikace logiky](logic-apps-create-a-logic-app.md).

2. Hello dekódovat EDIFACT zpráva konektor nemá aktivačních událostí, je nutné přidat aktivační událost pro spuštění aplikace logiky, jako je aktivační událost požadavku. V hello návrhář aplikace logiky přidejte aktivační události a poté přidejte logiku aplikace tooyour akce.

3. Hello vyhledávacího pole zadejte "EDIFACT" jako filtr. Vyberte **dekódovat EDIFACT zpráva**.
   
    ![hledání EDIFACT](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. Pokud jste nevytvořili dříve všechna připojení tooyour integrace účet, se zobrazí výzva toocreate nyní toto připojení. Název připojení a vyberte účet hello integrace, které chcete tooconnect.
   
    ![Vytvoření účtu integrace](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    Vyžadují se vlastnosti s hvězdičkou.

    | Vlastnost | Podrobnosti |
    | --- | --- |
    | Název připojení * |Zadejte libovolný název pro připojení. |
    | Integrace účet * |Zadejte název pro váš účet integrace. Ujistěte se, že integrace účet a logiku aplikace jsou v hello stejného umístění Azure. |

4. Po dokončení toofinish vytváření připojení, zvolte **vytvořit**. Podrobné informace o připojení by měl vypadat podobně jako příklad toothis:

    ![Podrobnosti účtu integrace](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. Po vytvoření připojení, jak je znázorněno v tomto příkladu, vyberte hello EDIFACT plochý soubor zprávy toodecode.

    ![Integrace připojení účet vytvořili](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    Například:

    ![Vyberte zprávu plochý soubor EDIFACT pro dekódování](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a>Podrobnosti decoder EDIFACT

konektor dekódovat EDIFACT Hello provádí tyto úlohy: 

* Ověří hello obálky proti obchodování smlouvu pro partnery.
* Přeloží hello smlouvy porovnáním hello odesílatele kvalifikátor & identifikátor a příjemce kvalifikátor & identifikátor.
* Rozdělí výměnu do více transakcí, když hello výměnu má více než jednu transakci na základě smlouvy hello získat konfiguraci nastavení.
* Provede zpětný překlad hello výměnu.
* Ověří EDI a vlastnosti specifické pro partnery, včetně:
  * Ověření struktury hello výměnu obálky
  * Ověření schématu hello obálky pro schéma řízení hello
  * Ověření schématu elementů hello transakce sady dat pro schéma zprávy hello
  * Ověření EDI provádět transakce set datové prvky.
* Ověří, že hello výměnu, skupiny a transakce sadu řízení čísla není duplicitní položky (Pokud je nakonfigurováno) 
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
* Generuje Technical (řízení) nebo funkční potvrzení (Pokud je nakonfigurovaná).
  * Technické potvrzení nebo hello CONTRL ACK hlásí výsledky hello syntaktické kontrolu výměnu hello dokončení přijata.
  * Potvrzení o funkční uznává přijmout nebo odmítnout přijaté výměnu nebo skupinu

## <a name="view-swagger-file"></a>Soubor Swagger zobrazení
Podrobnosti Swagger hello tooview hello EDIFACT konektor, najdete v tématu [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Další kroky
[Další informace o hello Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku") 

