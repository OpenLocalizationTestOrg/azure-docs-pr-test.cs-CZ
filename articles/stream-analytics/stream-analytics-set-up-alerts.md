---
title: "Nastavení výstrah pro dotazy v Stream Analytics | Microsoft Docs"
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
ms.openlocfilehash: 75b1b256eea7295f5a464996e2f34ae301c715fd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a><span data-ttu-id="973d6-104">Nastavit výstrahy pro úlohy Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="973d6-104">Set up alerts for Azure Stream Analytics jobs</span></span>
## <a name="introduction-monitor-page"></a><span data-ttu-id="973d6-105">Úvod: Stránka sledování</span><span class="sxs-lookup"><span data-stu-id="973d6-105">Introduction: Monitor page</span></span>
<span data-ttu-id="973d6-106">Výstrahy můžete nastavit, které spustí výstrahu, když metriky dosáhne podmínku, která zadáte.</span><span class="sxs-lookup"><span data-stu-id="973d6-106">You can set up alerts to trigger an alert when a metric reaches a condition that you specify.</span></span> <span data-ttu-id="973d6-107">Může například nastavit výstrahy pro podmínku takto:</span><span class="sxs-lookup"><span data-stu-id="973d6-107">For example, you might set up an alert for a condition like the following:</span></span>

`If there are zero input events in the last 5 minutes, send email notification to sa-admin@example.com`

<span data-ttu-id="973d6-108">Pravidla můžete nastavit na metriky prostřednictvím portálu nebo se dají konfigurovat [prostřednictvím kódu programu](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) přes protokoly operací data.</span><span class="sxs-lookup"><span data-stu-id="973d6-108">Rules can be set up on metrics through the portal, or can be configured [programmatically](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) over Operation Logs data.</span></span>

## <a name="set-up-alerts-in-the-azure-portal"></a><span data-ttu-id="973d6-109">Nastavit výstrahy na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="973d6-109">Set up alerts in the Azure portal</span></span>
1. <span data-ttu-id="973d6-110">Na portálu Azure otevřete úlohu služby Stream Analytics, kterou chcete vytvořit výstrahu pro.</span><span class="sxs-lookup"><span data-stu-id="973d6-110">In the Azure portal, open the Stream Analytics job you want to create an alert for.</span></span> 

2. <span data-ttu-id="973d6-111">V **úlohy** okně klikněte na tlačítko **monitorování** části.</span><span class="sxs-lookup"><span data-stu-id="973d6-111">In the **Job** blade, click the **Monitoring** section.</span></span>  

3. <span data-ttu-id="973d6-112">V **metrika** okně klikněte **přidat upozornění** příkaz.</span><span class="sxs-lookup"><span data-stu-id="973d6-112">In the **Metric** blade, click the **Add alert** command.</span></span>

      ![Nastavení portálu Azure](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. <span data-ttu-id="973d6-114">Zadejte název a popis.</span><span class="sxs-lookup"><span data-stu-id="973d6-114">Enter a name and a description.</span></span>

5. <span data-ttu-id="973d6-115">Použijte na selektory můžete definovat podmínku, pod kterým bude zasílat výstrahu.</span><span class="sxs-lookup"><span data-stu-id="973d6-115">Use the selectors to define the condition under which the alert will be sent.</span></span>

6. <span data-ttu-id="973d6-116">Zadejte informace o tom, kde by měl navštěvují výstrahy.</span><span class="sxs-lookup"><span data-stu-id="973d6-116">Provide information about where the alert should go.</span></span>

      ![Nastavení oznámení pro úlohu Azure streamování Analytics](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

<span data-ttu-id="973d6-118">Další podrobnosti týkající se konfigurace výstrahy na portálu Azure najdete [dostávat oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="973d6-118">For more detail on configuring alerts in the Azure portal, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>  


## <a name="get-help"></a><span data-ttu-id="973d6-119">Podpora</span><span class="sxs-lookup"><span data-stu-id="973d6-119">Get help</span></span>
<span data-ttu-id="973d6-120">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="973d6-120">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="973d6-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="973d6-121">Next steps</span></span>
* [<span data-ttu-id="973d6-122">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="973d6-122">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="973d6-123">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="973d6-123">Get started using Azure Stream Analytics</span></span>](stream-analytics-get-started.md)
* [<span data-ttu-id="973d6-124">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="973d6-124">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="973d6-125">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="973d6-125">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="973d6-126">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="973d6-126">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

