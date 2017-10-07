---
title: "ukázka výstrahy akce aaaWebhook v OMS Log Analytics | Microsoft Docs"
description: "Jednu z akcí hello můžete spustit v odpovědi tooa výstraha analýzy protokolů * webhooku *, což vám umožní tooinvoke externího procesu prostřednictvím jedné žádosti HTTP. Tento článek vás provede příklad vytvoření webhooku akce výstrahou analýzy protokolů pomocí Slack."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 13c39f0f-fd3c-472d-8324-ddf7538be45e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: bwren
ms.openlocfilehash: e60bdc4922347073d572c2e4719461b13e8e7d1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-alert-webhook-action-in-oms-log-analytics-toosend-message-tooslack"></a>Vytvoří akci, výstrah webhooku v tooSlack zpráva toosend OMS analýzy protokolů
Jednu z akcí hello můžete spustit v odpovědi tooa [analýzy protokolů výstraha](log-analytics-alerts.md) je *webhooku*, což vám umožní tooinvoke externího procesu prostřednictvím jedné žádosti HTTP.  Další informace o podrobnosti o výstrahy a pomocí webhooků v [výstrahy v analýzy protokolů](log-analytics-alerts.md)

V tomto článku se budeme zabývat příklad vytvoření webhooku akce výstrahou analýzy protokolů pomocí Slack, což je službou zasílání zpráv.

> [!NOTE]
> Toocomplete Slack účet musí mít tato ukázka.  Můžete si zaregistrovat bezplatný účet v [slack.com](http://slack.com).
> 
> 

## <a name="step-1---enable-webhooks-in-slack"></a>Krok 1 – povolit webhooky v Slack
1. Přihlaste se tooSlack na [slack.com](http://slack.com).
2. Vyberte kanál v hello **kanály** část v levém podokně hello.  Toto je kanál hello odeslána zpráva hello.  Můžete vybrat jednu z hello výchozí kanálů, jako **Obecné** nebo **náhodných**.  V případě produkční by pravděpodobně vytvoříte speciální kanál, jako **criticalservicealerts**. <br>
   
   ![Kanálů slack](media/log-analytics-alerts-webhooks/oms-webhooks01.png)
3. Klikněte na tlačítko **přidání aplikace nebo vlastní integrace** tooopen hello adresář aplikace.
4. Typ *webhooky* do vyhledávacího pole hello a potom vyberte **příchozí Webhooky**. <br>
   
   ![Kanálů slack](media/log-analytics-alerts-webhooks/oms-webhooks02.png)
5. Klikněte na tlačítko **nainstalovat** další tooyour název týmu.
6. Klikněte na tlačítko **Přidat konfiguraci**.
7. Vyberte hello hello kanál kliknete toouse v tomto příkladu a potom klikněte na **přidat příchozí Webhooky integrace**.  
8. Kopírování hello **adresa URL Webhooku**.  Budete vkládat to do konfigurace výstrah hello. <br>
   
    ![Kanálů slack](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Krok 2 – vytvořte pravidlo výstrahy v analýzy protokolů
1. [Vytvořit pravidlo výstrahy](log-analytics-alerts.md) s hello následující nastavení.
   * Dotaz:```    Type=Event EventLevelName=error ```
   * Kontrolovat tuto výstrahu každých: 5 minut
   * je Hello počet výsledků: větší než 10
   * Přes tento časový interval: 60 minut
   * Vyberte **Ano** pro **Webhooku** a **ne** pro hello dalších akcí.
2. Vložení hello Slack adresu URL do hello **adresa URL Webhooku** pole.
3. Vyberte možnost hello příliš**zahrnout vlastní datovou část JSON**.
4. Zvětralého očekává datové části ve formátu JSON s parametrem s názvem *text*.  Toto je text hello, který se zobrazí v uvítací zprávu, kterou vytvoří.  Můžete použít jeden nebo více výstrah parametrů hello pomocí hello  *#*  symbolů, stejně jako hello následující ukázka.
   
    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds hello over threshold of #thresholdvalue ."
    }
    ```
   
    ![Příklad datové části JSON](media/log-analytics-alerts-webhooks/oms-webhooks07.png)
5. Klikněte na tlačítko **Uložit** pravidlo výstrahy toosave hello.
6. Počkejte dostatek času výstrahy toobe vytvořen a zkontrolujte zprávu, která budou podobná následující toohello Slack.
   
   ![Příklad webhooku v Slack](media/log-analytics-alerts-webhooks/oms-webhooks08.png)

### <a name="advanced-webhook-payload-for-slack"></a>Rozšířené datové části webhooku pro Slack
Můžete upravit hojně příchozí zprávy s Slack. Další informace najdete v tématu [příchozí Webhooky](https://api.slack.com/incoming-webhooks) na webu Slack hello. Toto je složitější toocreate datové části bohaté zprávu s formátování:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link tooSearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


To by vygeneroval zprávu v Slack podobné následující toohello.

![Příklad zprávy v systému Slack](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Souhrn
S pravidlo výstrahy zavedené by měla mít tooSlack zpráva byla odeslána pokaždé, když je splněna kritéria hello.  

Toto je pouze jeden z příkladů akci, která můžete vytvořit v odpovědi tooan výstrahy.  Můžete vytvořit webhooku akce, která volá jiné externí služby, akce toostart runbook sady runbook v Azure Automation, nebo e-mailu akce toosend tooyourself e-mailu nebo ostatní příjemci.   

## <a name="next-steps"></a>Další kroky
* Další informace o dalších [výstrahy akce v analýzy protokolů](log-analytics-alerts-actions.md) včetně dalších akcí.


