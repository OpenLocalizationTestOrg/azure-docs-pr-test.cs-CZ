---
title: "Monitorování správy rozhraní API s Azure monitorování | Microsoft Docs"
description: "Naučte se monitorovat pomocí monitorování Azure služby Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 0f64947755c79739bb6f15325929bd074cfd7210
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-api-management-with-azure-monitor"></a><span data-ttu-id="c19e2-103">Monitorování API Management s Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="c19e2-103">Monitor API Management with Azure Monitor</span></span>
<span data-ttu-id="c19e2-104">Azure monitorování je služba Azure, která poskytuje jednoho zdroje pro monitorování všech prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="c19e2-104">Azure Monitor is an Azure service that provides a single source for monitoring all your Azure resources.</span></span> <span data-ttu-id="c19e2-105">S Azure monitorování můžete vizualizovat, dotaz, směrovat, archivaci a provádět akce na metriky a protokoly pocházejících z prostředků Azure, jako je například Správa rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c19e2-105">With Azure Monitor, you can visualize, query, route, archive, and take actions on the metrics and logs coming from Azure resources such as API Management.</span></span> 

<span data-ttu-id="c19e2-106">Následující video ukazuje, jak monitorovat pomocí monitorování Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="c19e2-106">The following video shows how to monitor API Management using Azure Monitor.</span></span> <span data-ttu-id="c19e2-107">Další informace o monitorování Azure najdete v tématu [Začínáme s Azure monitorování].</span><span class="sxs-lookup"><span data-stu-id="c19e2-107">For more information about Azure Monitor, see [Get Started with Azure Monitor].</span></span> 


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>
 
## <a name="metrics"></a><span data-ttu-id="c19e2-108">Metriky</span><span class="sxs-lookup"><span data-stu-id="c19e2-108">Metrics</span></span>
<span data-ttu-id="c19e2-109">API Management aktuálně vysílá pět metriky a plánujeme přidat další v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="c19e2-109">API Management currently emits five metrics and we plan to add more in the future.</span></span> <span data-ttu-id="c19e2-110">Tyto metriky jsou vygenerované každou minutu, která poskytuje téměř v reálném čase přehled o stavu a stavu vašich rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c19e2-110">These metrics are emitted every minute, giving you near real-time visibility into the state and health of your APIs.</span></span> <span data-ttu-id="c19e2-111">Následuje souhrn metriky:</span><span class="sxs-lookup"><span data-stu-id="c19e2-111">Following is a summary of the metrics:</span></span>
* <span data-ttu-id="c19e2-112">Celkový počet požadavků brány: počet rozhraní API požadavků v období.</span><span class="sxs-lookup"><span data-stu-id="c19e2-112">Total Gateway Requests: the number of API requests in the period.</span></span> 
* <span data-ttu-id="c19e2-113">Úspěšné požadavky brány: počet žádostí o rozhraní API, které přijaly kódy úspěšné odpovědi HTTP včetně 304, 307 a nic menší než 301 (například 200).</span><span class="sxs-lookup"><span data-stu-id="c19e2-113">Successful Gateway Requests: the number of API requests that received successful HTTP response codes including 304, 307 and anything smaller than 301 (for example, 200).</span></span> 
* <span data-ttu-id="c19e2-114">Neúspěšné požadavky brány: počet žádostí o rozhraní API, které obdržel chybné kódy odpovědi HTTP včetně 400 a nic větší než 500.</span><span class="sxs-lookup"><span data-stu-id="c19e2-114">Failed Gateway Requests: the number of API requests that received erroneous HTTP response codes including 400 and anything larger than 500.</span></span>
* <span data-ttu-id="c19e2-115">Neoprávněné žádosti brány: počet žádostí o rozhraní API, které přijaly, včetně 401, 403 a 429 kódy odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="c19e2-115">Unauthorized Gateway Requests: the number of API requests that received HTTP response codes including 401, 403, and 429.</span></span> 
* <span data-ttu-id="c19e2-116">Další požadavky na brány: počet žádostí o rozhraní API, které přijaly kódy odpovědí protokolu HTTP, které nepatří do žádné z výše uvedených skupin (například 418).</span><span class="sxs-lookup"><span data-stu-id="c19e2-116">Other Gateway Requests: the number of API requests that received HTTP response codes that do not belong to any of the preceding categories (for example, 418).</span></span>

<span data-ttu-id="c19e2-117">Můžete přistupovat metriky ve službě API Management, nebo přístup metrik Azure prostředky v Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="c19e2-117">You can access metrics in your API Management service, or access metrics of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="c19e2-118">Chcete-li zobrazit metriky ve službě API Management:</span><span class="sxs-lookup"><span data-stu-id="c19e2-118">To view metrics in your API Management service:</span></span>
1. <span data-ttu-id="c19e2-119">Otevřete portál Azure.</span><span class="sxs-lookup"><span data-stu-id="c19e2-119">Open the Azure portal.</span></span>
2. <span data-ttu-id="c19e2-120">Přejděte do služby API Management.</span><span class="sxs-lookup"><span data-stu-id="c19e2-120">Go to your API Management service.</span></span>
3. <span data-ttu-id="c19e2-121">Klikněte na tlačítko **metriky**.</span><span class="sxs-lookup"><span data-stu-id="c19e2-121">Click **Metrics**.</span></span>

![Okno metriky][metrics-blade]

<span data-ttu-id="c19e2-123">Další informace o tom, jak používat metriky najdete v tématu [přehled metriky].</span><span class="sxs-lookup"><span data-stu-id="c19e2-123">For more information about how to use Metrics, see [Overview of Metrics].</span></span>

## <a name="activity-logs"></a><span data-ttu-id="c19e2-124">Protokoly aktivit</span><span class="sxs-lookup"><span data-stu-id="c19e2-124">Activity Logs</span></span>
<span data-ttu-id="c19e2-125">Protokoly aktivity získat přehled o činnosti, které byly provedeny v vaše služby API Management.</span><span class="sxs-lookup"><span data-stu-id="c19e2-125">Activity logs provide insight into the operations that were performed on your API Management services.</span></span> <span data-ttu-id="c19e2-126">Se dřív označovala jako "protokoly auditu" nebo "operační protokoly".</span><span class="sxs-lookup"><span data-stu-id="c19e2-126">It was previously known as "audit logs" or "operational logs".</span></span> <span data-ttu-id="c19e2-127">Pomocí protokolů z aktivity, můžete určit ", kdo a kdy" pro všechny operace (PUT, POST, DELETE) na vaše rozhraní API správy služby zápisu.</span><span class="sxs-lookup"><span data-stu-id="c19e2-127">Using activity logs, you can determine the "what, who, and when" for any write operations (PUT, POST, DELETE) taken on your API Management services.</span></span> 

> [!NOTE]
> <span data-ttu-id="c19e2-128">Protokoly aktivity nezahrnují operace čtení (GET) nebo operace provádět na klasickém portálu vydavatele nebo pomocí původní rozhraní API pro správu.</span><span class="sxs-lookup"><span data-stu-id="c19e2-128">Activity logs do not include read (GET) operations or operations performed in the classic Publisher Portal or using the original Management APIs.</span></span>

<span data-ttu-id="c19e2-129">Lze zobrazit protokoly aktivit ve službě API Management nebo přístup k protokolům všechny prostředky Azure v Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="c19e2-129">You can access activity logs in your API Management service, or access logs of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="c19e2-130">Pokud chcete zobrazit aktivity záznamy ve službě API Management:</span><span class="sxs-lookup"><span data-stu-id="c19e2-130">To view activity logs in your API Management service:</span></span>
1. <span data-ttu-id="c19e2-131">Otevřete portál Azure.</span><span class="sxs-lookup"><span data-stu-id="c19e2-131">Open the Azure portal.</span></span>
2. <span data-ttu-id="c19e2-132">Přejděte do služby API Management.</span><span class="sxs-lookup"><span data-stu-id="c19e2-132">Go to your API Management service.</span></span>
3. <span data-ttu-id="c19e2-133">Klikněte na tlačítko **protokol aktivit**.</span><span class="sxs-lookup"><span data-stu-id="c19e2-133">Click **Activity log**.</span></span>

![Okno protokoly aktivit][activity-logs-blade]

<span data-ttu-id="c19e2-135">Další informace o tom, jak používat metriky najdete v tématu [protokoly aktivity přehled].</span><span class="sxs-lookup"><span data-stu-id="c19e2-135">For more information about how to use Metrics, see [Overview of Activity Logs].</span></span>

## <a name="alerts"></a><span data-ttu-id="c19e2-136">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="c19e2-136">Alerts</span></span>
<span data-ttu-id="c19e2-137">Můžete nakonfigurovat přijímat výstrahy založené na protokolech metriky a aktivity.</span><span class="sxs-lookup"><span data-stu-id="c19e2-137">You can configure to receive alerts based on metrics and activity logs.</span></span> <span data-ttu-id="c19e2-138">Azure monitorování umožňuje nakonfigurovat výstrahu při aktivaci, proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="c19e2-138">Azure Monitor allows you to configure an alert to do the following when it triggers:</span></span>

* <span data-ttu-id="c19e2-139">Odeslat oznámení e-mailu</span><span class="sxs-lookup"><span data-stu-id="c19e2-139">Send an email notification</span></span>
* <span data-ttu-id="c19e2-140">Volat webhook, jehož</span><span class="sxs-lookup"><span data-stu-id="c19e2-140">Call a webhook</span></span>
* <span data-ttu-id="c19e2-141">Vyvolání aplikace logiky Azure</span><span class="sxs-lookup"><span data-stu-id="c19e2-141">Invoke an Azure Logic App</span></span>

<span data-ttu-id="c19e2-142">Pravidla výstrah můžete nakonfigurovat ve službě API Management, nebo v Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="c19e2-142">You can configure alert rules in your API Management service, or in Azure Monitor.</span></span> <span data-ttu-id="c19e2-143">Konfigurace je ve službě API Management:</span><span class="sxs-lookup"><span data-stu-id="c19e2-143">To configure them in API Management:</span></span> 
1. <span data-ttu-id="c19e2-144">Otevřete portál Azure.</span><span class="sxs-lookup"><span data-stu-id="c19e2-144">Open the Azure portal.</span></span>
2. <span data-ttu-id="c19e2-145">Přejděte do služby API Management.</span><span class="sxs-lookup"><span data-stu-id="c19e2-145">Go to your API Management service.</span></span>
3. <span data-ttu-id="c19e2-146">Klikněte na tlačítko **výstrah pravidla**.</span><span class="sxs-lookup"><span data-stu-id="c19e2-146">Click **Alert rules**.</span></span>

![Okno pravidla výstrah][alert-rules-blade]

<span data-ttu-id="c19e2-148">Další informace o používání výstrah najdete v tématu [přehled výstrah].</span><span class="sxs-lookup"><span data-stu-id="c19e2-148">For more information about using Alerts, see [Overview of Alerts].</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="c19e2-149">Diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="c19e2-149">Diagnostic Logs</span></span>
<span data-ttu-id="c19e2-150">Diagnostické protokoly poskytují bohaté informace o operacích a chyby, které jsou důležité pro auditování i pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="c19e2-150">Diagnostic logs provide rich information about operations and errors that are important for auditing as well as troubleshooting purposes.</span></span> <span data-ttu-id="c19e2-151">Diagnostické protokoly se liší od protokoly aktivity.</span><span class="sxs-lookup"><span data-stu-id="c19e2-151">Diagnostics logs differ from activity logs.</span></span> <span data-ttu-id="c19e2-152">Protokoly aktivity poskytují přehled o činnosti, které byly provedeny v vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="c19e2-152">Activity logs provide insights into the operations that were performed on your Azure resources.</span></span> <span data-ttu-id="c19e2-153">Diagnostické protokoly získat přehled o operace, aby prostředku provedeny sám sebe.</span><span class="sxs-lookup"><span data-stu-id="c19e2-153">Diagnostics logs provide insight into operations that your resource performed itself.</span></span>

<span data-ttu-id="c19e2-154">API Management aktuálně poskytuje diagnostické protokoly (každou hodinu dávkově) o jednotlivých API žádosti s každou položku s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="c19e2-154">API Management currently provides diagnostics logs (batched hourly) about individual API request with each entry having the following structure:</span></span>

```
{
    "Tenant": "",
      "DeploymentName": "",
      "time": "",
      "resourceId": "",
      "category": "GatewayLogs",
      "operationName": "Microsoft.ApiManagement/GatewayLogs",
      "durationMs": ,
      "Level": ,
      "properties": "{
          "ApiId": "",
          "OperationId": "",
          "ProductId": "",
          "SubscriptionId": "",
          "Method": "",
          "Url": "",
          "RequestSize": ,
          "ServiceTime": "",
          "BackendMethod": "",
          "BackendUrl": "",
          "BackendResponseCode": ,
          "ResponseCode": ,
          "ResponseSize": ,
          "Cache": "",
          "UserId"
      }"
 }
```

<span data-ttu-id="c19e2-155">Můžete přístup k diagnostickým protokolům ve službě API Management, nebo přístup k protokolům všechny prostředky Azure v Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="c19e2-155">You can access diagnostic logs in your API Management service, or access logs of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="c19e2-156">Chcete-li zobrazit diagnostické protokoly ve službě API Management:</span><span class="sxs-lookup"><span data-stu-id="c19e2-156">To view diagnostic logs in your API Management service:</span></span>
1. <span data-ttu-id="c19e2-157">Otevřete portál Azure.</span><span class="sxs-lookup"><span data-stu-id="c19e2-157">Open the Azure portal.</span></span>
2. <span data-ttu-id="c19e2-158">Přejděte do služby API Management.</span><span class="sxs-lookup"><span data-stu-id="c19e2-158">Go to your API Management service.</span></span>
3. <span data-ttu-id="c19e2-159">Klikněte na tlačítko **protokolů diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="c19e2-159">Click **Diagnostic log**.</span></span>

![Okno Diagnostické protokoly][diagnostic-logs-blade]

<span data-ttu-id="c19e2-161">Další informace o tom, jak používat metriky najdete v tématu [přehled diagnostické protokoly].</span><span class="sxs-lookup"><span data-stu-id="c19e2-161">For more information about how to use Metrics, see [Overview of Diagnostic Logs].</span></span>

## <a name="next-step"></a><span data-ttu-id="c19e2-162">Další krok</span><span class="sxs-lookup"><span data-stu-id="c19e2-162">Next Step</span></span>

* <span data-ttu-id="c19e2-163">[Začínáme s Azure monitorování]</span><span class="sxs-lookup"><span data-stu-id="c19e2-163">[Get Started with Azure Monitor]</span></span>
* <span data-ttu-id="c19e2-164">[přehled metriky]</span><span class="sxs-lookup"><span data-stu-id="c19e2-164">[Overview of Metrics]</span></span>
* <span data-ttu-id="c19e2-165">[protokoly aktivity přehled]</span><span class="sxs-lookup"><span data-stu-id="c19e2-165">[Overview of Activity Logs]</span></span>
* <span data-ttu-id="c19e2-166">[přehled diagnostické protokoly]</span><span class="sxs-lookup"><span data-stu-id="c19e2-166">[Overview of Diagnostic Logs]</span></span>
* <span data-ttu-id="c19e2-167">[přehled výstrah]</span><span class="sxs-lookup"><span data-stu-id="c19e2-167">[Overview of Alerts]</span></span>

<span data-ttu-id="c19e2-168">[Začínáme s Azure monitorování]: ../monitoring-and-diagnostics/monitoring-get-started.md</span><span class="sxs-lookup"><span data-stu-id="c19e2-168">[Get Started with Azure Monitor]: ../monitoring-and-diagnostics/monitoring-get-started.md</span></span>
<span data-ttu-id="c19e2-169">[přehled metriky]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md</span><span class="sxs-lookup"><span data-stu-id="c19e2-169">[Overview of Metrics]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md</span></span>
<span data-ttu-id="c19e2-170">[protokoly aktivity přehled]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md</span><span class="sxs-lookup"><span data-stu-id="c19e2-170">[Overview of Activity Logs]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md</span></span>
<span data-ttu-id="c19e2-171">[přehled diagnostické protokoly]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md</span><span class="sxs-lookup"><span data-stu-id="c19e2-171">[Overview of Diagnostic Logs]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md</span></span>
<span data-ttu-id="c19e2-172">[přehled výstrah]: ../monitoring-and-diagnostics/insights-alerts-portal.md</span><span class="sxs-lookup"><span data-stu-id="c19e2-172">[Overview of Alerts]: ../monitoring-and-diagnostics/insights-alerts-portal.md</span></span>



[metrics-blade]: ./media/api-management-azure-monitor/api-management-metrics-blade.png
[activity-logs-blade]: ./media/api-management-azure-monitor/api-management-activity-logs-blade.png
[alert-rules-blade]: ./media/api-management-azure-monitor/api-management-alert-rules-blade.png
[diagnostic-logs-blade]: ./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png
