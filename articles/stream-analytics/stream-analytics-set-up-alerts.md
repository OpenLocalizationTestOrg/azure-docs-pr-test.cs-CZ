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
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a><span data-ttu-id="77dc0-104">Nastavit výstrahy pro úlohy Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="77dc0-104">Set up alerts for Azure Stream Analytics jobs</span></span>
## <a name="introduction-monitor-page"></a><span data-ttu-id="77dc0-105">Úvod: Stránka sledování</span><span class="sxs-lookup"><span data-stu-id="77dc0-105">Introduction: Monitor page</span></span>
<span data-ttu-id="77dc0-106">Nastavením výstrahy tootrigger výstrahu při metriky dosáhne podmínku, která zadáte.</span><span class="sxs-lookup"><span data-stu-id="77dc0-106">You can set up alerts tootrigger an alert when a metric reaches a condition that you specify.</span></span> <span data-ttu-id="77dc0-107">Může například nastavit výstrahy pro podmínku jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="77dc0-107">For example, you might set up an alert for a condition like hello following:</span></span>

`If there are zero input events in hello last 5 minutes, send email notification toosa-admin@example.com`

<span data-ttu-id="77dc0-108">Pravidla můžete nastavit na metriky prostřednictvím portálu hello nebo se dají konfigurovat [prostřednictvím kódu programu](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) přes protokoly operací data.</span><span class="sxs-lookup"><span data-stu-id="77dc0-108">Rules can be set up on metrics through hello portal, or can be configured [programmatically](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) over Operation Logs data.</span></span>

## <a name="set-up-alerts-in-hello-azure-portal"></a><span data-ttu-id="77dc0-109">Nastavení výstrah v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="77dc0-109">Set up alerts in hello Azure portal</span></span>
1. <span data-ttu-id="77dc0-110">V hello portálu Azure otevřete hello Stream Analytics úlohu, kterou chcete toocreate výstrahu pro.</span><span class="sxs-lookup"><span data-stu-id="77dc0-110">In hello Azure portal, open hello Stream Analytics job you want toocreate an alert for.</span></span> 

2. <span data-ttu-id="77dc0-111">V hello **úlohy** okně klikněte na tlačítko hello **monitorování** části.</span><span class="sxs-lookup"><span data-stu-id="77dc0-111">In hello **Job** blade, click hello **Monitoring** section.</span></span>  

3. <span data-ttu-id="77dc0-112">V hello **metrika** okně klikněte na tlačítko hello **přidat upozornění** příkaz.</span><span class="sxs-lookup"><span data-stu-id="77dc0-112">In hello **Metric** blade, click hello **Add alert** command.</span></span>

      ![Nastavení portálu Azure](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. <span data-ttu-id="77dc0-114">Zadejte název a popis.</span><span class="sxs-lookup"><span data-stu-id="77dc0-114">Enter a name and a description.</span></span>

5. <span data-ttu-id="77dc0-115">Použijte hello selektory toodefine hello podmínky v rámci které hello bude odeslána výstraha.</span><span class="sxs-lookup"><span data-stu-id="77dc0-115">Use hello selectors toodefine hello condition under which hello alert will be sent.</span></span>

6. <span data-ttu-id="77dc0-116">Zadejte informace o tom, kde by měl navštěvují hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="77dc0-116">Provide information about where hello alert should go.</span></span>

      ![Nastavení oznámení pro úlohu Azure streamování Analytics](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

<span data-ttu-id="77dc0-118">Další podrobnosti o konfiguraci výstrah v hello portálu Azure najdete [dostávat oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="77dc0-118">For more detail on configuring alerts in hello Azure portal, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>  


## <a name="get-help"></a><span data-ttu-id="77dc0-119">Podpora</span><span class="sxs-lookup"><span data-stu-id="77dc0-119">Get help</span></span>
<span data-ttu-id="77dc0-120">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="77dc0-120">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="77dc0-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77dc0-121">Next steps</span></span>
* [<span data-ttu-id="77dc0-122">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="77dc0-122">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="77dc0-123">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="77dc0-123">Get started using Azure Stream Analytics</span></span>](stream-analytics-get-started.md)
* [<span data-ttu-id="77dc0-124">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="77dc0-124">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="77dc0-125">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="77dc0-125">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="77dc0-126">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="77dc0-126">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

