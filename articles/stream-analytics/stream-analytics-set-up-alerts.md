---
title: "aaaSet si výstrahy pro dotazy v Stream Analytics | Microsoft Docs"
description: "Principy Stream Analytics výstrahy"
keywords: "nastavení výstrah"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 9952e2cf-b335-4a5c-8f45-8d3e1eda2e20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/26/2017
ms.author: jeffstok
ms.openlocfilehash: 7b1d90d1468311186567c8518e0283ea6b88c3f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Nastavit výstrahy pro úlohy Azure Stream Analytics
## <a name="introduction-monitor-page"></a>Úvod: Stránka sledování
Nastavením výstrahy tootrigger výstrahu při metriky dosáhne podmínku, která zadáte. Může například nastavit výstrahy pro podmínku jako hello následující:

`If there are zero input events in hello last 5 minutes, send email notification toosa-admin@example.com`

Pravidla můžete nastavit na metriky prostřednictvím portálu hello nebo se dají konfigurovat [prostřednictvím kódu programu](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) přes protokoly operací data.

## <a name="set-up-alerts-in-hello-azure-portal"></a>Nastavení výstrah v hello portálu Azure
1. V hello portálu Azure otevřete hello Stream Analytics úlohu, kterou chcete toocreate výstrahu pro. 

2. V hello **úlohy** okně klikněte na tlačítko hello **monitorování** části.  

3. V hello **metrika** okně klikněte na tlačítko hello **přidat upozornění** příkaz.

      ![Nastavení portálu Azure](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. Zadejte název a popis.

5. Použijte hello selektory toodefine hello podmínky v rámci které hello bude odeslána výstraha.

6. Zadejte informace o tom, kde by měl navštěvují hello výstrahy.

      ![Nastavení oznámení pro úlohu Azure streamování Analytics](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

Další podrobnosti o konfiguraci výstrah v hello portálu Azure najdete [dostávat oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  


## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-get-started.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

