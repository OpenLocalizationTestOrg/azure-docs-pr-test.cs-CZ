---
title: "aaaMonitor s monitorováním Azure API Management | Microsoft Docs"
description: "Zjistěte, jak toomonitor Azure API Management služby pomocí Azure monitorování."
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
ms.openlocfilehash: 5012d8ed57ea4f94ea6bc1b7c4e1102516ec4414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-api-management-with-azure-monitor"></a><span data-ttu-id="3e207-103">Monitorování API Management s Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="3e207-103">Monitor API Management with Azure Monitor</span></span>
<span data-ttu-id="3e207-104">Azure monitorování je služba Azure, která poskytuje jednoho zdroje pro monitorování všech prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="3e207-104">Azure Monitor is an Azure service that provides a single source for monitoring all your Azure resources.</span></span> <span data-ttu-id="3e207-105">S Azure monitorování můžete vizualizovat, dotaz, směrovat, archivaci a provádět akce na hello metriky a protokoly pocházejících z prostředků Azure, jako je například Správa rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3e207-105">With Azure Monitor, you can visualize, query, route, archive, and take actions on hello metrics and logs coming from Azure resources such as API Management.</span></span> 

<span data-ttu-id="3e207-106">následující video ukazuje, jak Hello toomonitor pomocí monitorování Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="3e207-106">hello following video shows how toomonitor API Management using Azure Monitor.</span></span> <span data-ttu-id="3e207-107">Další informace o monitorování Azure najdete v tématu [Začínáme s Azure monitorování].</span><span class="sxs-lookup"><span data-stu-id="3e207-107">For more information about Azure Monitor, see [Get Started with Azure Monitor].</span></span> 


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>
 
## <a name="metrics"></a><span data-ttu-id="3e207-108">Metriky</span><span class="sxs-lookup"><span data-stu-id="3e207-108">Metrics</span></span>
<span data-ttu-id="3e207-109">API Management aktuálně vysílá pět metriky a plánujeme tooadd další v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="3e207-109">API Management currently emits five metrics and we plan tooadd more in hello future.</span></span> <span data-ttu-id="3e207-110">Tyto metriky jsou vygenerované každou minutu, která poskytuje téměř v reálném čase přehled hello stavu a stavu vašich rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3e207-110">These metrics are emitted every minute, giving you near real-time visibility into hello state and health of your APIs.</span></span> <span data-ttu-id="3e207-111">Následuje souhrn hello metriky:</span><span class="sxs-lookup"><span data-stu-id="3e207-111">Following is a summary of hello metrics:</span></span>
* <span data-ttu-id="3e207-112">Celkový počet požadavků brány: hello počet rozhraní API požadavků v období hello.</span><span class="sxs-lookup"><span data-stu-id="3e207-112">Total Gateway Requests: hello number of API requests in hello period.</span></span> 
* <span data-ttu-id="3e207-113">Úspěšné požadavky brány: hello počet žádostí o rozhraní API, které přijaly kódy úspěšné odpovědi HTTP včetně 304, 307 a nic menší než 301 (například 200).</span><span class="sxs-lookup"><span data-stu-id="3e207-113">Successful Gateway Requests: hello number of API requests that received successful HTTP response codes including 304, 307 and anything smaller than 301 (for example, 200).</span></span> 
* <span data-ttu-id="3e207-114">Neúspěšné požadavky brány: hello počet žádostí o rozhraní API, které obdržel chybné kódy odpovědi HTTP včetně 400 a nic větší než 500.</span><span class="sxs-lookup"><span data-stu-id="3e207-114">Failed Gateway Requests: hello number of API requests that received erroneous HTTP response codes including 400 and anything larger than 500.</span></span>
* <span data-ttu-id="3e207-115">Neoprávněné žádosti brány: hello počet žádostí o rozhraní API, které přijaly, včetně 401, 403 a 429 kódy odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e207-115">Unauthorized Gateway Requests: hello number of API requests that received HTTP response codes including 401, 403, and 429.</span></span> 
* <span data-ttu-id="3e207-116">Další požadavky na brány: hello počet žádostí o rozhraní API, které přijaly kódy odpovědí protokolu HTTP, které nejsou součástí tooany hello předchozích kategorií (například 418).</span><span class="sxs-lookup"><span data-stu-id="3e207-116">Other Gateway Requests: hello number of API requests that received HTTP response codes that do not belong tooany of hello preceding categories (for example, 418).</span></span>

<span data-ttu-id="3e207-117">Můžete přistupovat metriky ve službě API Management, nebo přístup metrik Azure prostředky v Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="3e207-117">You can access metrics in your API Management service, or access metrics of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="3e207-118">Metrika tooview ve službě API Management:</span><span class="sxs-lookup"><span data-stu-id="3e207-118">tooview metrics in your API Management service:</span></span>
1. <span data-ttu-id="3e207-119">Otevřete hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3e207-119">Open hello Azure portal.</span></span>
2. <span data-ttu-id="3e207-120">Přejděte tooyour služba API Management.</span><span class="sxs-lookup"><span data-stu-id="3e207-120">Go tooyour API Management service.</span></span>
3. <span data-ttu-id="3e207-121">Klikněte na tlačítko **metriky**.</span><span class="sxs-lookup"><span data-stu-id="3e207-121">Click **Metrics**.</span></span>

![Okno metriky][metrics-blade]

<span data-ttu-id="3e207-123">Další informace o tom, najdete v části toouse metriky, [přehled metriky].</span><span class="sxs-lookup"><span data-stu-id="3e207-123">For more information about how toouse Metrics, see [Overview of Metrics].</span></span>

## <a name="activity-logs"></a><span data-ttu-id="3e207-124">Protokoly aktivit</span><span class="sxs-lookup"><span data-stu-id="3e207-124">Activity Logs</span></span>
<span data-ttu-id="3e207-125">Protokoly aktivity získat přehled o hello operace, které byly provedeny na vaše služby API Management.</span><span class="sxs-lookup"><span data-stu-id="3e207-125">Activity logs provide insight into hello operations that were performed on your API Management services.</span></span> <span data-ttu-id="3e207-126">Se dřív označovala jako "protokoly auditu" nebo "operační protokoly".</span><span class="sxs-lookup"><span data-stu-id="3e207-126">It was previously known as "audit logs" or "operational logs".</span></span> <span data-ttu-id="3e207-127">Pomocí protokolů z aktivity, můžete určit hello ", kdo a kdy" pro všechny operace (PUT, POST, DELETE) na vaše rozhraní API správy služby zápisu.</span><span class="sxs-lookup"><span data-stu-id="3e207-127">Using activity logs, you can determine hello "what, who, and when" for any write operations (PUT, POST, DELETE) taken on your API Management services.</span></span> 

> [!NOTE]
> <span data-ttu-id="3e207-128">Protokoly aktivity nezahrnují operace čtení (GET) nebo operace provedené v hello portálu classic vydavatele nebo pomocí hello původní rozhraní API pro správu.</span><span class="sxs-lookup"><span data-stu-id="3e207-128">Activity logs do not include read (GET) operations or operations performed in hello classic Publisher Portal or using hello original Management APIs.</span></span>

<span data-ttu-id="3e207-129">Lze zobrazit protokoly aktivit ve službě API Management nebo přístup k protokolům všechny prostředky Azure v Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="3e207-129">You can access activity logs in your API Management service, or access logs of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="3e207-130">Aktivita tooview protokolů ve službě API Management:</span><span class="sxs-lookup"><span data-stu-id="3e207-130">tooview activity logs in your API Management service:</span></span>
1. <span data-ttu-id="3e207-131">Otevřete hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3e207-131">Open hello Azure portal.</span></span>
2. <span data-ttu-id="3e207-132">Přejděte tooyour služba API Management.</span><span class="sxs-lookup"><span data-stu-id="3e207-132">Go tooyour API Management service.</span></span>
3. <span data-ttu-id="3e207-133">Klikněte na tlačítko **protokol aktivit**.</span><span class="sxs-lookup"><span data-stu-id="3e207-133">Click **Activity log**.</span></span>

![Okno protokoly aktivit][activity-logs-blade]

<span data-ttu-id="3e207-135">Další informace o tom, najdete v části toouse metriky, [protokoly aktivity přehled].</span><span class="sxs-lookup"><span data-stu-id="3e207-135">For more information about how toouse Metrics, see [Overview of Activity Logs].</span></span>

## <a name="alerts"></a><span data-ttu-id="3e207-136">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="3e207-136">Alerts</span></span>
<span data-ttu-id="3e207-137">Můžete nakonfigurovat tooreceive výstrahy založené na protokolech metriky a aktivity.</span><span class="sxs-lookup"><span data-stu-id="3e207-137">You can configure tooreceive alerts based on metrics and activity logs.</span></span> <span data-ttu-id="3e207-138">Azure monitorování umožňuje tooconfigure výstrahy toodo hello následující při aktivaci:</span><span class="sxs-lookup"><span data-stu-id="3e207-138">Azure Monitor allows you tooconfigure an alert toodo hello following when it triggers:</span></span>

* <span data-ttu-id="3e207-139">Odeslat oznámení e-mailu</span><span class="sxs-lookup"><span data-stu-id="3e207-139">Send an email notification</span></span>
* <span data-ttu-id="3e207-140">Volat webhook, jehož</span><span class="sxs-lookup"><span data-stu-id="3e207-140">Call a webhook</span></span>
* <span data-ttu-id="3e207-141">Vyvolání aplikace logiky Azure</span><span class="sxs-lookup"><span data-stu-id="3e207-141">Invoke an Azure Logic App</span></span>

<span data-ttu-id="3e207-142">Pravidla výstrah můžete nakonfigurovat ve službě API Management, nebo v Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="3e207-142">You can configure alert rules in your API Management service, or in Azure Monitor.</span></span> <span data-ttu-id="3e207-143">tooconfigure je ve službě API Management:</span><span class="sxs-lookup"><span data-stu-id="3e207-143">tooconfigure them in API Management:</span></span> 
1. <span data-ttu-id="3e207-144">Otevřete hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3e207-144">Open hello Azure portal.</span></span>
2. <span data-ttu-id="3e207-145">Přejděte tooyour služba API Management.</span><span class="sxs-lookup"><span data-stu-id="3e207-145">Go tooyour API Management service.</span></span>
3. <span data-ttu-id="3e207-146">Klikněte na tlačítko **výstrah pravidla**.</span><span class="sxs-lookup"><span data-stu-id="3e207-146">Click **Alert rules**.</span></span>

![Okno pravidla výstrah][alert-rules-blade]

<span data-ttu-id="3e207-148">Další informace o používání výstrah najdete v tématu [přehled výstrah].</span><span class="sxs-lookup"><span data-stu-id="3e207-148">For more information about using Alerts, see [Overview of Alerts].</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="3e207-149">Diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="3e207-149">Diagnostic Logs</span></span>
<span data-ttu-id="3e207-150">Diagnostické protokoly poskytují bohaté informace o operacích a chyby, které jsou důležité pro auditování i pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="3e207-150">Diagnostic logs provide rich information about operations and errors that are important for auditing as well as troubleshooting purposes.</span></span> <span data-ttu-id="3e207-151">Diagnostické protokoly se liší od protokoly aktivity.</span><span class="sxs-lookup"><span data-stu-id="3e207-151">Diagnostics logs differ from activity logs.</span></span> <span data-ttu-id="3e207-152">Protokoly aktivity poskytují přehled o hello operace, které byly provedeny na vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="3e207-152">Activity logs provide insights into hello operations that were performed on your Azure resources.</span></span> <span data-ttu-id="3e207-153">Diagnostické protokoly získat přehled o operace, aby prostředku provedeny sám sebe.</span><span class="sxs-lookup"><span data-stu-id="3e207-153">Diagnostics logs provide insight into operations that your resource performed itself.</span></span>

<span data-ttu-id="3e207-154">API Management aktuálně poskytuje diagnostické protokoly (každou hodinu dávkově) o jednotlivých API žádosti s každou položku s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="3e207-154">API Management currently provides diagnostics logs (batched hourly) about individual API request with each entry having hello following structure:</span></span>

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

<span data-ttu-id="3e207-155">Můžete přístup k diagnostickým protokolům ve službě API Management, nebo přístup k protokolům všechny prostředky Azure v Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="3e207-155">You can access diagnostic logs in your API Management service, or access logs of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="3e207-156">diagnostické protokoly tooview ve službě API Management:</span><span class="sxs-lookup"><span data-stu-id="3e207-156">tooview diagnostic logs in your API Management service:</span></span>
1. <span data-ttu-id="3e207-157">Otevřete hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3e207-157">Open hello Azure portal.</span></span>
2. <span data-ttu-id="3e207-158">Přejděte tooyour služba API Management.</span><span class="sxs-lookup"><span data-stu-id="3e207-158">Go tooyour API Management service.</span></span>
3. <span data-ttu-id="3e207-159">Klikněte na tlačítko **protokolů diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="3e207-159">Click **Diagnostic log**.</span></span>

![Okno Diagnostické protokoly][diagnostic-logs-blade]

<span data-ttu-id="3e207-161">Další informace o tom, najdete v části toouse metriky, [přehled diagnostické protokoly].</span><span class="sxs-lookup"><span data-stu-id="3e207-161">For more information about how toouse Metrics, see [Overview of Diagnostic Logs].</span></span>

## <a name="next-step"></a><span data-ttu-id="3e207-162">Další krok</span><span class="sxs-lookup"><span data-stu-id="3e207-162">Next Step</span></span>

* <span data-ttu-id="3e207-163">[Začínáme s Azure monitorování]</span><span class="sxs-lookup"><span data-stu-id="3e207-163">[Get Started with Azure Monitor]</span></span>
* <span data-ttu-id="3e207-164">[přehled metriky]</span><span class="sxs-lookup"><span data-stu-id="3e207-164">[Overview of Metrics]</span></span>
* <span data-ttu-id="3e207-165">[protokoly aktivity přehled]</span><span class="sxs-lookup"><span data-stu-id="3e207-165">[Overview of Activity Logs]</span></span>
* <span data-ttu-id="3e207-166">[přehled diagnostické protokoly]</span><span class="sxs-lookup"><span data-stu-id="3e207-166">[Overview of Diagnostic Logs]</span></span>
* <span data-ttu-id="3e207-167">[přehled výstrah]</span><span class="sxs-lookup"><span data-stu-id="3e207-167">[Overview of Alerts]</span></span>

[Začínáme s Azure monitorování]: ../monitoring-and-diagnostics/monitoring-get-started.md
[přehled metriky]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md
[protokoly aktivity přehled]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[přehled diagnostické protokoly]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[přehled výstrah]: ../monitoring-and-diagnostics/insights-alerts-portal.md



[metrics-blade]: ./media/api-management-azure-monitor/api-management-metrics-blade.png
[activity-logs-blade]: ./media/api-management-azure-monitor/api-management-activity-logs-blade.png
[alert-rules-blade]: ./media/api-management-azure-monitor/api-management-alert-rules-blade.png
[diagnostic-logs-blade]: ./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png
