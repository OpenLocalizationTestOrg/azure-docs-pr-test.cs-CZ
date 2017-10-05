---
title: "Ladění pomocí operace a protokoly služby ve Stream Analytics | Microsoft Docs"
description: "Postupy: použití protokoly operací Stream Analytics"
keywords: "protokoly služby"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: a2ed9676-f0bd-4398-87c8-a592779ac728
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: c95d240ebef6a84228eb98db70002792fcfbdea6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a><span data-ttu-id="0bf61-104">Ladění pomocí služby a operace protokoly úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0bf61-104">Debug Stream Analytics jobs using service and operation logs</span></span>
<span data-ttu-id="0bf61-105">Všech služeb Azure, zadejte provozní protokolování zpráv uživatelům, aby podrobnosti záznamu týkající se operací správy.</span><span class="sxs-lookup"><span data-stu-id="0bf61-105">All Azure services supply operational logging messages to users to record details related to management operations.</span></span> <span data-ttu-id="0bf61-106">V Azure Stream Analytics tyto informace slouží k ladění účely, například zobrazení stavu úlohy, průběh úlohy a zprávy o selhání sledovat průběh úlohy v čase, z začít zpracování na výstup.</span><span class="sxs-lookup"><span data-stu-id="0bf61-106">In Azure Stream Analytics, this information can be used for debugging purposes such as viewing job status, job progress, and failure messages to track the progress of a job over time, from start to processing to output.</span></span>

## <a name="find-operation-logs-in-the-azure-management-portal"></a><span data-ttu-id="0bf61-107">Najít operaci protokoly na portálu správy Azure</span><span class="sxs-lookup"><span data-stu-id="0bf61-107">Find operation logs in the Azure Management portal</span></span>
<span data-ttu-id="0bf61-108">Protokoly operací je přístupná dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="0bf61-108">Operation Logs can be accessed in two ways:</span></span>  

* <span data-ttu-id="0bf61-109">Řídicí panel úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0bf61-109">Dashboard of the Stream Analytics job</span></span>  
* <span data-ttu-id="0bf61-110">Správa služeb v portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="0bf61-110">Management Services in the Azure Classic portal</span></span>  

## <a name="dashboard-of-the-stream-analytics-job"></a><span data-ttu-id="0bf61-111">Řídicí panel úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0bf61-111">Dashboard of the Stream Analytics job</span></span>
<span data-ttu-id="0bf61-112">Odkaz na odpovídající protokoly úlohy Stream Analytics se zobrazí na kartě úlohy řídicího panelu. Pokud kliknete na tento odkaz, nastaví filtry tak, že se zobrazí nejnovější protokoly pro tuto konkrétní úlohu.</span><span class="sxs-lookup"><span data-stu-id="0bf61-112">A link to the corresponding logs of a Stream Analytics job is displayed on the job’s Dashboard tab. If you click on that link, it will set the filters in a way that it shows latest logs for that specific job.</span></span>

  ![Vyberte protokoly služby správy](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a><span data-ttu-id="0bf61-114">Služby správy</span><span class="sxs-lookup"><span data-stu-id="0bf61-114">Management Services</span></span>
<span data-ttu-id="0bf61-115">Ručně přejděte na protokoly operací pro Stream Analytics a dalších služeb v portálu Azure Classic:</span><span class="sxs-lookup"><span data-stu-id="0bf61-115">To manually navigate to the Operation Logs for Stream Analytics and other services in the Azure Classic portal:</span></span>

1. <span data-ttu-id="0bf61-116">Klikněte na **Management Services** v [portálu Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="0bf61-116">Click on **Management Services** in the [Azure Classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="0bf61-117">Vyberte **Stream Analytics** pro **typ** a název úlohy pro **název služby**.</span><span class="sxs-lookup"><span data-stu-id="0bf61-117">Select **Stream Analytics** for **Type** and the name of the job for **Service Name**.</span></span>  
   
   ![Vyberte služby Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a><span data-ttu-id="0bf61-119">Vyhledání protokolů auditu na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0bf61-119">Find audit logs in the Azure portal</span></span>
<span data-ttu-id="0bf61-120">Chcete-li najít operační protokoly pro úlohu služby Stream Analytics na portálu Azure, klikněte na tlačítko **Procházet** a pak vyberte **protokoly auditu**.</span><span class="sxs-lookup"><span data-stu-id="0bf61-120">To find operational logs for your Stream Analytics job in the Azure portal, Click **Browse** and then select **Audit logs**.</span></span>

  ![Portálu Azure vyberte Stream Analytics](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

<span data-ttu-id="0bf61-122">Otevře se okno zobrazení událostí za posledních 7 dnů pro všechny prostředky ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="0bf61-122">This will open a blade showing events from the last 7 days for all resources in your subscription.</span></span>  <span data-ttu-id="0bf61-123">Můžete filtrovat, zobrazí se události zadejte typ nebo časového rámce kliknutím **filtru** příkaz.</span><span class="sxs-lookup"><span data-stu-id="0bf61-123">You can filter to see events of a specify type or time frame by clicking the **Filter** command.</span></span>

  ![Portálu Azure vyberte Stream Analytics](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a><span data-ttu-id="0bf61-125">Získat podrobnosti protokolu</span><span class="sxs-lookup"><span data-stu-id="0bf61-125">Get log details</span></span>
<span data-ttu-id="0bf61-126">Můžete filtrovat podle časové rozmezí a stav chcete zobrazit protokoly pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="0bf61-126">You can filter by Time Range and Status to view the logs for your job.</span></span>

<span data-ttu-id="0bf61-127">V portálu pro správu Azure, klikněte na **podrobnosti** tlačítko v dolní části okna zobrazíte další podrobnosti o zvolenou událost.</span><span class="sxs-lookup"><span data-stu-id="0bf61-127">In the Azure Management portal, click on the **Details** button at the bottom of the window to view more details about a selected event.</span></span> 

  ![Vyberte podrobnosti](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

<span data-ttu-id="0bf61-129">Na portálu Azure klikněte na položky protokolu chcete zobrazit podrobné události v ho.</span><span class="sxs-lookup"><span data-stu-id="0bf61-129">In the Azure portal, click on a log entry to see the detailed events inside it.</span></span>

  ![Portálu Azure vyberte podrobnosti](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

<span data-ttu-id="0bf61-131">Odtud můžete otevřít **podrobností** tak, že kliknete na události.</span><span class="sxs-lookup"><span data-stu-id="0bf61-131">From there, you can open the **Detail** blade by clicking on the event.</span></span>

  ![Portálu Azure vyberte podrobnosti](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a><span data-ttu-id="0bf61-133">Ladění neúspěšnou úlohu</span><span class="sxs-lookup"><span data-stu-id="0bf61-133">Debug a failed job</span></span>
<span data-ttu-id="0bf61-134">V portálu pro správu Azure klikněte na ikonu hledání a typ "se nezdařilo".</span><span class="sxs-lookup"><span data-stu-id="0bf61-134">In the Azure Management portal, click on the Search icon and type ‘failed’.</span></span> <span data-ttu-id="0bf61-135">To dává výsledku všechny protokoly s chybami.</span><span class="sxs-lookup"><span data-stu-id="0bf61-135">This gives a result of all logs with failures.</span></span> 

  ![Ladění neúspěšnou úlohu](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

<span data-ttu-id="0bf61-137">Na portálu Azure můžete filtrovat podle úrovně zprávu zobrazíte **kritický** události.</span><span class="sxs-lookup"><span data-stu-id="0bf61-137">In the Azure portal, you can filter by level of message to view **Critical** events.</span></span>

  ![Portál Azure ladění](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

<span data-ttu-id="0bf61-139">Můžete vybrat některý z chyby a klikněte na **podrobnosti** Další informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="0bf61-139">You can select any one of the failures, and click on the **Details** for more information on the error.</span></span>  <span data-ttu-id="0bf61-140">Některé chybové zprávy také poskytují informace o tom, jak zmírnit problém.</span><span class="sxs-lookup"><span data-stu-id="0bf61-140">Some error messages also provide information about how to mitigate the issue.</span></span> 

  ![Podrobnosti o operaci](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

<span data-ttu-id="0bf61-142">V případě, že budete muset kontaktovat [podporu](https://azure.microsoft.com/support/options/) nebo zadejte informace o týmu prostřednictvím [fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), je potřeba počítat s provozní údaje, konkrétně **ID korelace**.</span><span class="sxs-lookup"><span data-stu-id="0bf61-142">In case you need to contact [Support](https://azure.microsoft.com/support/options/) or provide information to the team via the [MSDN forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), please note the Operation Details, specifically the **Correlation ID**.</span></span> 

## <a name="get-help"></a><span data-ttu-id="0bf61-143">Podpora</span><span class="sxs-lookup"><span data-stu-id="0bf61-143">Get help</span></span>
<span data-ttu-id="0bf61-144">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="0bf61-144">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="0bf61-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0bf61-145">Next steps</span></span>
* [<span data-ttu-id="0bf61-146">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0bf61-146">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="0bf61-147">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0bf61-147">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="0bf61-148">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0bf61-148">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="0bf61-149">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="0bf61-149">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="0bf61-150">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="0bf61-150">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

