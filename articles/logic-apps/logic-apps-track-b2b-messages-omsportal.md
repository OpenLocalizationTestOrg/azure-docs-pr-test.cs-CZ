---
title: "aaaTrack B2B zprávy v Operations Management Suite - Azure Logic Apps | Microsoft Docs"
description: "Sledování komunikace B2B pro integraci účet a logiku aplikace v Operations Management Suite (OMS) s Azure Log Analytics"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: f385a72008b19408bb45d61c440df0505b688175
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="track-b2b-communication-in-hello-microsoft-operations-management-suite-oms"></a>Sledování komunikace B2B ve hello Microsoft Operations Management Suite (OMS)

Po nastavení komunikace B2B mezi dvěma systémem obchodních procesů nebo aplikací prostřednictvím účtu integrace tyto entity můžou vyměňovat zprávy mezi sebou. toocheck zda tyto zprávy jsou zpracovány správně, můžete sledovat AS2, X12, a EDIFACT zprávy s [Azure Log Analytics](../log-analytics/log-analytics-overview.md) v hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Například můžete použít tyto možnosti sledování založené na webu pro sledování zpráv:

* Počet zpráv a stav
* Potvrzení stavu
* Korelovat zprávy s potvrzování
* Podrobné informace o chybě popisy chyb
* Možnosti hledání

## <a name="requirements"></a>Požadavky

* Aplikace logiky, který je nastavený s protokolování diagnostiky. Další informace [jak toocreate aplikace logiky](logic-apps-create-a-logic-app.md) a [jak tooset protokolování pro tuto aplikaci logiky](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Integrace účet, který je nastavený s sledování a protokolování. Další informace [jak toocreate účet integrace](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) a [jak tooset sledování a protokolování pro tento účet](../logic-apps/logic-apps-monitor-b2b-message.md).

* Pokud jste to ještě neudělali, [publikování diagnostických dat tooLog Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) v OMS.

> [!NOTE]
> Po splnění požadavků na předchozí hello, měli byste mít pracovního prostoru v hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Měli byste použít hello stejný pracovní prostor OMS pro sledování vaší komunikace B2B v OMS. 
>  
> Pokud nemáte pracovním prostorem OMS, přečtěte si [jak toocreate pracovním prostorem OMS](../log-analytics/log-analytics-get-started.md).

## <a name="add-hello-logic-apps-b2b-solution-toohello-operations-management-suite-oms"></a>Přidat hello B2B aplikace logiky řešení toohello Operations Management Suite (OMS)

toohave OMS sledování zpráv B2B pro svou aplikaci logiky, je nutné přidat hello **B2B aplikace logiky** portál OMS toohello řešení. Další informace o [přidání řešení tooOMS](../log-analytics/log-analytics-get-started.md).

1. V hello [portál Azure](https://portal.azure.com), zvolte **více služeb**. Vyhledejte "analýzy protokolů" a potom vyberte **analýzy protokolů** jak je vidět tady:

   ![Najít analýzy protokolů](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)

2. V části **analýzy protokolů**, najděte a vyberte pracovní prostor OMS. 

   ![Vyberte pracovní prostor OMS](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. V části **správy**, zvolte **portálu OMS**.

   ![Zvolte portálu OMS](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. Když se otevře okno hello OMS domovskou stránku, zvolte **řešení Galerie**.    

   ![Zvolte řešení Galerie](media/logic-apps-track-b2b-messages-omsportal/omshomepage1.png)

5. V části **všechna řešení**, najít a zvolte **B2B aplikace logiky**.     

   ![Zvolte logiku aplikace B2B](media/logic-apps-track-b2b-messages-omsportal/omshomepage2.png)

6. V části **B2B aplikace logiky**, zvolte **přidat**.

   ![Vyberte Přidat](media/logic-apps-track-b2b-messages-omsportal/omshomepage3.png)

   Na domovské stránce hello OMS hello dlaždici **zpráv B2B aplikace logiky** se teď zobrazí. 
   Tuto dlaždici aktualizuje počet zpráv hello při zpracování zpráv B2B.

   ![OMS domovské stránce dlaždice zpráv B2B aplikace logiky](media/logic-apps-track-b2b-messages-omsportal/omshomepage4.png)

<a name="message-status-details"></a>

## <a name="track-message-status-and-details-in-hello-operations-management-suite"></a>Sledovat stav zprávu a podrobnosti v hello Operations Management Suite

1. Po zpracování zpráv B2B můžete zobrazit stav hello a podrobnosti o těchto zpráv. Na domovské stránce hello OMS, zvolte hello **zpráv B2B aplikace logiky** dlaždici.

   ![Počet aktualizované zpráv](media/logic-apps-track-b2b-messages-omsportal/omshomepage6.png)

   > [!NOTE]
   > Ve výchozím nastavení, hello **zpráv B2B aplikace logiky** dlaždice zobrazuje data podle jednoho dne. toochange hello data obor tooa jiný interval, možnost Řízení oboru hello hello horní části stránky OMS hello:
   > 
   > ![Změnit rozsah dat](media/logic-apps-track-b2b-messages-omsportal/change-interval.png)
   >

2. Po hello zpráva stav zobrazení řídicího panelu můžete zobrazit další podrobnosti o konkrétní zpráva typu, který zobrazuje data podle jednoho dne. Vyberte dlaždici hello **AS2**, **X12**, nebo **EDIFACT**.

   ![Stav zobrazení zpráv](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

   Pro vybrané dlaždice se zobrazí seznam zpráv. 
   toolearn Další informace o vlastnosti hello u každého typu zprávy najdete popisy vlastností tato zpráva:

   * [Vlastnosti zprávy AS2](#as2-message-properties)
   * [X12 zprávy vlastnosti](#x12-message-properties)
   * [Vlastnosti zprávy EDIFACT](#EDIFACT-message-properties)

   Zde je například jak může vypadat seznam zpráv AS2:

   ![Zobrazení zpráv AS2](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

3. tooview nebo export hello vstupy a výstupy specifické pro zprávy, vyberte tyto zprávy a zvolte **Stáhnout**. Když se zobrazí výzva, uložte hello .zip souboru tooyour místního počítače a potom extrahujte tento soubor. 

   Hello extrahovanou složku zahrnuje složku pro každou vybranou zprávu. 
   Pokud jste nastavili potvrzení hello zpráva složka také obsahuje soubory s podrobnostmi o potvrzení. 
   Každou zprávu složku má minimálně tyto soubory: 
   
   * Čitelný soubory s hello vstup datové části a výstupní datové části Podrobnosti
   * Kódované soubory s hello vstupy a výstupy

   U každého typu zprávy můžete najít hello složku a název formáty souborů:

   * [AS2 složku a název souboru formáty](#as2-folder-file-names)
   * [X12 složku a název souboru formáty](#x12-folder-file-names)
   * [EDIFACT složku a název souboru formáty](#edifact-folder-file-names)

   ![Stažení souborů zpráv](media/logic-apps-track-b2b-messages-omsportal/download-messages.png)

4. spustit všechny akce, které mají stejný hello tooview ID, na hello **hledání protokolů** vyberte zprávu hello seznamu zpráv.

   Můžete řadit podle sloupce, nebo vyhledejte konkrétní výsledky tyto akce.

   ![Akce s hello stejné ID spuštění](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

   * Zvolte toosearch výsledky s předem dotazy **Oblíbené**.

   * Další informace [jak toobuild dotazuje přidáním filtry](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md). 
   Další informace o [jak toofind dat pomocí protokolu prohledává v analýzy protokolů](../log-analytics/log-analytics-log-searches.md).

   * dotaz toochange hello vyhledávacího pole aktualizace hello dotaz s hello sloupců a hodnot, které chcete toouse jako filtry.

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>Popisy vlastností a formáty názvu pro AS2, X 12 a EDIFACT zprávy

U každého typu zprávy tady jsou popisy vlastností hello a formáty názvu pro zprávu stažené soubory.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>Popisy vlastností zpráv AS2

Tady jsou hello popisy vlastností pro každou zprávu AS2.

| Vlastnost | Popis |
| --- | --- |
| Odesílatel | Hello hosta partnera zadaný v **přijímat nastavení**, nebo hello hostitele partnera zadaný v **odeslat nastavení** pro smlouvy AS2 |
| Příjemce | Hello hostitele partnera zadaný v **přijímat nastavení**, nebo partnera hosta hello zadaný v **odeslat nastavení** pro smlouvy AS2 |
| Aplikace logiky | aplikace logiky Hello, kde se nastaví hello AS2 akce |
| Status | stav zpráv v AS2 Hello <br>Úspěch = přijatých nebo odeslaných zprávu platný AS2. Žádné MDN je nastavený. <br>Úspěch = přijatých nebo odeslaných zprávu platný AS2. Nastavení a obdrží MDN nebo odesílání MDN. <br>Se nezdařilo = přijatá neplatná zpráva AS2. Žádné MDN je nastavený. <br>Čekající = přijatých nebo odeslaných zprávu platný AS2. MDN nastavení a MDN se očekává. |
| Potvrzení | stav zpráv v MDN Hello <br>Přijatá = přijatých nebo odeslaných kladné MDN. <br>Čekající na vyřízení = čekání tooreceive nebo poslat MDN. <br>Odmítl = přijatých nebo odeslaných záporné MDN. <br>Není vyžadována = MDN není nastaven v hello smlouvy. |
| Směr | Hello směr zprávy AS2 |
| ID korelace | Hello ID, které koreluje všechny hello triggery a akce v aplikaci logiky |
| ID zprávy | ID zprávy Hello AS2 z hlavičky zpráv hello AS2 |
| časové razítko | Hello čas, kdy hello AS2 akce zpracována uvítací zprávu |
|          |             |

<a name="as2-folder-file-names"></a>

### <a name="as2-name-formats-for-downloaded-message-files"></a>Formáty názvu AS2 pro soubory stažené zpráv

Tady jsou hello formáty názvu pro každou složku zpráva stažené AS2 a soubory.

| Složka nebo soubor | Formát názvu |
| :------------- | :---------- |
| Složka zpráv | [odesílatele] \_[příjemce]\_AS2\_[ID korelace]\_[ID zprávy]\_[časové razítko] |
| Vstupní, výstupní a v případě nastavení, potvrzení soubory | **Datová část vstupního**: [odesílatele]\_[příjemce]\_AS2\_[ID korelace]\_input_payload.txt </p>**Výstupní datové**: [odesílatele]\_[příjemce]\_AS2\_[ID korelace]\_výstup\_payload.txt </p></p>**Vstupy**: [odesílatele]\_[příjemce]\_AS2\_[ID korelace]\_inputs.txt </p></p>**Výstupy**: [odesílatele]\_[příjemce]\_AS2\_[ID korelace]\_outputs.txt |
|          |             |

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>X12 zprávy popisy vlastností

Zde je uveden popis hello vlastnost pro každý X12 zprávy.

| Vlastnost | Popis |
| --- | --- |
| Odesílatel | Hello hosta partnera zadaný v **přijímat nastavení**, nebo hello hostitele partnera zadaný v **odeslat nastavení** pro X12 smlouvy |
| Příjemce | Hello hostitele partnera zadaný v **přijímat nastavení**, nebo partnera hosta hello zadaný v **odeslat nastavení** pro X12 smlouvy |
| Aplikace logiky | aplikace logiky Hello, kde se nastaví hello X12 akce |
| Status | stav zpráv v Hello X12 <br>Úspěch = přijatých nebo odeslaných platný X12 zprávy. Je nastavený žádný funkční potvrzení. <br>Úspěch = přijatých nebo odeslaných platný X12 zprávy. Funkční potvrzení se nastavení služby a přijímají nebo odesílání funkční potvrzení. <br>Se nezdařilo = přijatých nebo odeslaných neplatný X12 zprávy. <br>Čekající = přijatých nebo odeslaných platný X12 zprávy. Funkční potvrzení nastavení a funkční potvrzení se očekává. |
| Potvrzení | Funkční stav Ack (997) <br>Přijatá = přijatých nebo odeslaných kladné funkční ack. <br>Odmítl = přijatých nebo odeslaných záporné funkční ack. <br>Čekající na vyřízení = očekává funkční potvrzení, ale nebyl přijat. <br>Čekající na vyřízení = generované funkční potvrzení, ale nelze odeslat toopartner. <br>Není vyžadována = funkčnosti ack není nastaven. |
| Směr | Směr Hello X12 zprávy |
| ID korelace | Hello ID, které koreluje všechny hello triggery a akce v aplikaci logiky |
| Msg typu | Hello EDI X 12 zpráva typu |
| ICN | Hello Interchange číslo řízení hello X12 zprávy |
| TSCN | Hello transakce nastavit řízení číslo hello X12 zprávy |
| časové razítko | Hello čas, kdy akce hello X12 zpracována uvítací zprávu |
|          |             |

<a name="x12-folder-file-names"></a>

### <a name="x12-name-formats-for-downloaded-message-files"></a>X12 název formáty souborů stažené zpráv

Tady jsou formáty názvu hello pro každý stáhnout X12 složky a soubory zpráv.

| Složka nebo soubor | Formát názvu |
| :------------- | :---------- |
| Složka zpráv | [odesílatele] \_[příjemce]\_X12\_[číslo datového přenosu řízení]\_[globální – ovládací prvek číslo]\_[transakce set řízení number]\_[časové razítko] |
| Vstupní, výstupní a v případě nastavení, potvrzení soubory | **Datová část vstupního**: [odesílatele]\_[příjemce]\_X12\_[číslo datového přenosu řízení]\_input_payload.txt </p>**Výstupní datové**: [odesílatele]\_[příjemce]\_X12\_[číslo datového přenosu řízení]\_výstup\_payload.txt </p></p>**Vstupy**: [odesílatele]\_[příjemce]\_X12\_[číslo datového přenosu řízení]\_inputs.txt </p></p>**Výstupy**: [odesílatele]\_[příjemce]\_X12\_[číslo datového přenosu řízení]\_outputs.txt |
|          |             |

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>Popisy vlastností EDIFACT zpráv

Tady jsou hello popisy vlastností pro každou zprávu EDIFACT.

| Vlastnost | Popis |
| --- | --- |
| Odesílatel | Hello hosta partnera zadaný v **přijímat nastavení**, nebo hello hostitele partnera zadaný v **odeslat nastavení** pro smlouvy EDIFACT |
| Příjemce | Hello hostitele partnera zadaný v **přijímat nastavení**, nebo partnera hosta hello zadaný v **odeslat nastavení** pro smlouvy EDIFACT |
| Aplikace logiky | aplikace logiky Hello, kde se nastaví hello EDIFACT akce |
| Status | stav zpráv v EDIFACT Hello <br>Úspěch = přijatých nebo odeslaných zprávu platný EDIFACT. Je nastavený žádný funkční potvrzení. <br>Úspěch = přijatých nebo odeslaných zprávu platný EDIFACT. Funkční potvrzení se nastavení služby a přijímají nebo odesílání funkční potvrzení. <br>Se nezdařilo = přijatých nebo odeslaných neplatná zpráva EDIFACT <br>Čekající = přijatých nebo odeslaných zprávu platný EDIFACT. Funkční potvrzení nastavení a funkční potvrzení se očekává. |
| Potvrzení | Funkční stav Ack (997) <br>Přijatá = přijatých nebo odeslaných kladné funkční ack. <br>Odmítl = přijatých nebo odeslaných záporné funkční ack. <br>Čekající na vyřízení = očekává funkční potvrzení, ale nebyl přijat. <br>Čekající na vyřízení = generované funkční potvrzení, ale nelze odeslat toopartner. <br>Není vyžadována = funkční Ack není nastaven. |
| Směr | Hello směr EDIFACT zprávy |
| ID korelace | Hello ID, které koreluje všechny hello triggery a akce v aplikaci logiky |
| Msg typu | Typ zprávy EDIFACT Hello |
| ICN | Hello Interchange číslo ovládací prvek pro zprávu EDIFACT hello |
| TSCN | Hello transakce nastavit řízení číslo zprávy EDIFACT hello |
| časové razítko | Hello čas, kdy hello EDIFACT akce zpracována uvítací zprávu |
|          |               |

<a name="edifact-folder-file-names"></a>

### <a name="edifact-name-formats-for-downloaded-message-files"></a>Formáty názvu EDIFACT pro soubory stažené zpráv

Tady jsou hello formáty názvu pro každou složku zpráva stažené EDIFACT a soubory.

| Složka nebo soubor | Formát názvu |
| :------------- | :---------- |
| Složka zpráv | [odesílatele] \_[příjemce]\_EDIFACT\_[číslo datového přenosu řízení]\_[globální – ovládací prvek číslo]\_[transakce set řízení number]\_[časové razítko] |
| Vstupní, výstupní a v případě nastavení, potvrzení soubory | **Datová část vstupního**: [odesílatele]\_[příjemce]\_EDIFACT\_[číslo datového přenosu řízení]\_input_payload.txt </p>**Výstupní datové**: [odesílatele]\_[příjemce]\_EDIFACT\_[číslo datového přenosu řízení]\_výstup\_payload.txt </p></p>**Vstupy**: [odesílatele]\_[příjemce]\_EDIFACT\_[číslo datového přenosu řízení]\_inputs.txt </p></p>**Výstupy**: [odesílatele]\_[příjemce]\_EDIFACT\_[číslo datového přenosu řízení]\_outputs.txt |
|          |             |

## <a name="next-steps"></a>Další kroky

* [Dotaz pro B2B zprávy v Operations Management Suite](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md)
* [Schémata sledování AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Schémata sledování X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Vlastní sledování schémata](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)