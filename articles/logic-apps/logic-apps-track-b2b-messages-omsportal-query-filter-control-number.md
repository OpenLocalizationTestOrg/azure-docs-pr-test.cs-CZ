---
title: "aaaQuery zpráv B2B v Operations Management Suite - Azure Logic Apps | Microsoft Docs"
description: "Vytvoření dotazů tootrack AS2, X 12 a EDIFACT zprávy v hello Operations Management Suite"
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
ms.openlocfilehash: aee6644ff19add8f074ed5f1725db87b1d3b74b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="query-for-as2-x12-and-edifact-messages-in-hello-microsoft-operations-management-suite-oms"></a>Dotaz pro AS2, X 12 a EDIFACT zprávy v hello Microsoft Operations Management Suite (OMS)

toofind hello AS2, X 12 a EDIFACT zprávy, které sledujete s [Azure Log Analytics](../log-analytics/log-analytics-overview.md) v hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), můžete vytvořit dotazy, které jsou založené na konkrétní akce filtru kritéria. Například můžete najít na základě různých řízení konkrétní výměnu zpráv.

## <a name="requirements"></a>Požadavky

* Aplikace logiky, který je nastavený s protokolování diagnostiky. Další informace [jak toocreate aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) a [jak tooset protokolování pro tuto aplikaci logiky](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Integrace účet, který je nastavený s sledování a protokolování. Další informace [jak toocreate účet integrace](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) a [jak tooset sledování a protokolování pro tento účet](../logic-apps/logic-apps-monitor-b2b-message.md).

* Pokud jste to ještě neudělali, [publikování diagnostických dat tooLog Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) a [nastavení sledování v OMS zpráv](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Po splnění požadavků na předchozí hello, měli byste mít pracovního prostoru v hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Měli byste použít hello stejný pracovní prostor OMS pro sledování vaší komunikace B2B v OMS. 
>  
> Pokud nemáte pracovním prostorem OMS, přečtěte si [jak toocreate pracovním prostorem OMS](../log-analytics/log-analytics-get-started.md).

## <a name="create-message-queries-with-filters-in-hello-operations-management-suite-portal"></a>Vytváření dotazů zprávu s filtry portálu hello Operations Management Suite

Tento příklad ukazuje, jak můžete najít podle jejich výměnu řízení počtu zpráv.

> [!TIP] 
> Pokud znáte název pracovní prostor OMS, přejděte tooyour prostoru Domovská stránka (`https://{your-workspace-name}.portal.mms.microsoft.com`) a začít od kroku 4. Jinak začněte v kroku 1.

1. V hello [portál Azure](https://portal.azure.com), zvolte **více služeb**. Vyhledejte "analýzy protokolů" a potom vyberte **analýzy protokolů** jak je vidět tady:

   ![Najít analýzy protokolů](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. V části **analýzy protokolů**, najděte a vyberte pracovní prostor OMS.

   ![Vyberte pracovní prostor OMS](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. V části **správy**, zvolte **portálu OMS**.

   ![Zvolte portálu OMS](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/omsportalpage.png)

4. Na domovské stránce OMS, zvolte **hledání protokolů**.

   ![Na domovské stránce OMS zvolte "Hledání protokolů"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   -nebo-

   ![V nabídce OMS hello zvolte "Hledání protokolů"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

5. Hello vyhledávacího pole zadejte pole, které chcete toofind a stiskněte klávesu **Enter**. Když začnete psát, ukazuje OMS je možné shody a operací, které můžete použít. Další informace o [jak toofind data v analýzy protokolů](../log-analytics/log-analytics-log-searches.md).

   Tento příklad vyhledá pro události se **typ = AzureDiagnostics**.

   ![Začněte psát řetězec dotazu](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

6. V levém panelu hello zvolte hello časový rámec, které chcete tooview. Zvolte tooadd tooyour dotazu filter, **+ přidat**.

   ![Přidat filtr tooquery](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

7. V části **přidat filtry**, zadejte název filtru hello hello filtru chcete vyhledávat. Vyberte filtr hello a zvolte **+ přidat**.

   toofind hello výměnu řízení číslo, tento příklad hello slovo "výměnu" vyhledá a vybere **event_record_messageProperties_interchangeControlNumber_s** jako filtr hello.

   ![Vyberte filtr](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

9. V levém panelu hello vyberte hello filtru hodnotu, kterou chcete toouse a zvolte **použít**.

   Tento příklad vybere hello výměnu řízení číslo pro zprávy hello, kterou chceme.

   ![Vyberte hodnotu filtru](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

10. Nyní se vraťte toohello dotaz, který vytváříte. Dotaz se aktualizovala se vybraný filtr událostí a hodnotou. Předchozí výsledky jsou nyní příliš filtrovány.

    ![Vrátí tooyour dotaz s filtrované výsledky](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a>Své uložit pro budoucí použití

1. Z dotazu na hello **hledání protokolů** vyberte **Uložit**. Zadejte název dotazu, vyberte kategorii a zvolte **Uložit**.

   ![Dejte dotazu, název a kategorii](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. tooview dotazu, zvolte **Oblíbené**.

   ![Zvolte "Oblíbené"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. V části **uložená hledání**, vyberte svůj dotaz tak, aby hello výsledky lze zobrazit. dotaz hello upravit dotaz hello tooupdate vyhledávat odlišné výsledky.

   ![Vyberte dotaz](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-hello-operations-management-suite-portal"></a>Najít a spustit uložené dotazy na portálu služby Operations Management Suite hello

1. Otevřete domovskou stránku pracovní prostor OMS (`https://{your-workspace-name}.portal.mms.microsoft.com`) a zvolte **hledání protokolů**.

   ![Na domovské stránce OMS zvolte "Hledání protokolů"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   -nebo-

   ![V nabídce OMS hello zvolte "Hledání protokolů"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. Na hello **hledání protokolů** – Domovská stránka, zvolte **Oblíbené**.

   ![Zvolte "Oblíbené"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. V části **uložená hledání**, vyberte svůj dotaz tak, aby hello výsledky lze zobrazit. dotaz hello upravit dotaz hello tooupdate vyhledávat odlišné výsledky.

   ![Vyberte dotaz](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a>Další kroky

* [Schémata sledování AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Schémata sledování X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Vlastní sledování schémata](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)