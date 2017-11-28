---
title: "aaaDebug v protokolech operace a služby Stream Analytics | Microsoft Docs"
description: "Jak toouse Stream Analytics protokoly operací"
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
ms.openlocfilehash: d3dd27706ccc879a724e1894b33d47021d972f31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a><span data-ttu-id="d53a2-104">Ladění pomocí služby a operace protokoly úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d53a2-104">Debug Stream Analytics jobs using service and operation logs</span></span>
<span data-ttu-id="d53a2-105">Všechny služby Azure napájení provozní protokolování zpráv toousers toorecord podrobnosti související s toomanagement operace.</span><span class="sxs-lookup"><span data-stu-id="d53a2-105">All Azure services supply operational logging messages toousers toorecord details related toomanagement operations.</span></span> <span data-ttu-id="d53a2-106">V Azure Stream Analytics tyto informace slouží pro účely, například zobrazení stavu úlohy, průběh úlohy a selhání zprávy tootrack hello průběh úlohy v čase od toooutput tooprocessing spuštění ladění.</span><span class="sxs-lookup"><span data-stu-id="d53a2-106">In Azure Stream Analytics, this information can be used for debugging purposes such as viewing job status, job progress, and failure messages tootrack hello progress of a job over time, from start tooprocessing toooutput.</span></span>

## <a name="find-operation-logs-in-hello-azure-management-portal"></a><span data-ttu-id="d53a2-107">Najít operaci protokoly na portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="d53a2-107">Find operation logs in hello Azure Management portal</span></span>
<span data-ttu-id="d53a2-108">Protokoly operací je přístupná dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="d53a2-108">Operation Logs can be accessed in two ways:</span></span>  

* <span data-ttu-id="d53a2-109">Řídicí panel úlohy Stream Analytics hello</span><span class="sxs-lookup"><span data-stu-id="d53a2-109">Dashboard of hello Stream Analytics job</span></span>  
* <span data-ttu-id="d53a2-110">Správa služeb v portálu Azure Classic hello</span><span class="sxs-lookup"><span data-stu-id="d53a2-110">Management Services in hello Azure Classic portal</span></span>  

## <a name="dashboard-of-hello-stream-analytics-job"></a><span data-ttu-id="d53a2-111">Řídicí panel úlohy Stream Analytics hello</span><span class="sxs-lookup"><span data-stu-id="d53a2-111">Dashboard of hello Stream Analytics job</span></span>
<span data-ttu-id="d53a2-112">Odkaz toohello, odpovídající protokoly úlohy Stream Analytics se zobrazí na kartě úlohy hello řídicího panelu. Pokud kliknete na tento odkaz, nastaví hello filtry tak, že se zobrazí nejnovější protokoly pro tuto konkrétní úlohu.</span><span class="sxs-lookup"><span data-stu-id="d53a2-112">A link toohello corresponding logs of a Stream Analytics job is displayed on hello job’s Dashboard tab. If you click on that link, it will set hello filters in a way that it shows latest logs for that specific job.</span></span>

  ![Vyberte protokoly služby správy](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a><span data-ttu-id="d53a2-114">Služby správy</span><span class="sxs-lookup"><span data-stu-id="d53a2-114">Management Services</span></span>
<span data-ttu-id="d53a2-115">toomanually přejděte protokoly operací toohello Stream Analytics a dalších služeb v portálu Azure Classic hello:</span><span class="sxs-lookup"><span data-stu-id="d53a2-115">toomanually navigate toohello Operation Logs for Stream Analytics and other services in hello Azure Classic portal:</span></span>

1. <span data-ttu-id="d53a2-116">Klikněte na **Management Services** v hello [portálu Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="d53a2-116">Click on **Management Services** in hello [Azure Classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="d53a2-117">Vyberte **Stream Analytics** pro **typ** a název hello hello úlohy pro **název služby**.</span><span class="sxs-lookup"><span data-stu-id="d53a2-117">Select **Stream Analytics** for **Type** and hello name of hello job for **Service Name**.</span></span>  
   
   ![Vyberte služby Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-hello-azure-portal"></a><span data-ttu-id="d53a2-119">Najít protokoly auditu v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d53a2-119">Find audit logs in hello Azure portal</span></span>
<span data-ttu-id="d53a2-120">Klikněte na tlačítko toofind operační protokoly pro úlohu služby Stream Analytics v hello portál Azure, **Procházet** a pak vyberte **protokoly auditu**.</span><span class="sxs-lookup"><span data-stu-id="d53a2-120">toofind operational logs for your Stream Analytics job in hello Azure portal, Click **Browse** and then select **Audit logs**.</span></span>

  ![Portálu Azure vyberte Stream Analytics](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

<span data-ttu-id="d53a2-122">Otevře se okno zobrazení událostí z hello posledních 7 dnů pro všechny prostředky ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="d53a2-122">This will open a blade showing events from hello last 7 days for all resources in your subscription.</span></span>  <span data-ttu-id="d53a2-123">Můžete filtrovat události toosee zadejte typ nebo časového rámce kliknutím hello **filtru** příkaz.</span><span class="sxs-lookup"><span data-stu-id="d53a2-123">You can filter toosee events of a specify type or time frame by clicking hello **Filter** command.</span></span>

  ![Portálu Azure vyberte Stream Analytics](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a><span data-ttu-id="d53a2-125">Získat podrobnosti protokolu</span><span class="sxs-lookup"><span data-stu-id="d53a2-125">Get log details</span></span>
<span data-ttu-id="d53a2-126">Můžete filtrovat podle časové rozmezí a stav tooview hello protokoly pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="d53a2-126">You can filter by Time Range and Status tooview hello logs for your job.</span></span>

<span data-ttu-id="d53a2-127">V portálu pro správu Azure hello, klikněte na hello **podrobnosti** tlačítko hello dolní části okna tooview hello další podrobnosti o zvolenou událost.</span><span class="sxs-lookup"><span data-stu-id="d53a2-127">In hello Azure Management portal, click on hello **Details** button at hello bottom of hello window tooview more details about a selected event.</span></span> 

  ![Vyberte podrobnosti](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

<span data-ttu-id="d53a2-129">Portál Azure, klikněte na položku toosee protokolu v hello hello podrobné události v ho.</span><span class="sxs-lookup"><span data-stu-id="d53a2-129">In hello Azure portal, click on a log entry toosee hello detailed events inside it.</span></span>

  ![Portálu Azure vyberte podrobnosti](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

<span data-ttu-id="d53a2-131">Odtud můžete otevřít hello **podrobností** tak, že kliknete na hello událostí.</span><span class="sxs-lookup"><span data-stu-id="d53a2-131">From there, you can open hello **Detail** blade by clicking on hello event.</span></span>

  ![Portálu Azure vyberte podrobnosti](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a><span data-ttu-id="d53a2-133">Ladění neúspěšnou úlohu</span><span class="sxs-lookup"><span data-stu-id="d53a2-133">Debug a failed job</span></span>
<span data-ttu-id="d53a2-134">Na portálu pro správu Azure hello klikněte na ikonu hledání hello a zadejte "se nezdařilo".</span><span class="sxs-lookup"><span data-stu-id="d53a2-134">In hello Azure Management portal, click on hello Search icon and type ‘failed’.</span></span> <span data-ttu-id="d53a2-135">To dává výsledku všechny protokoly s chybami.</span><span class="sxs-lookup"><span data-stu-id="d53a2-135">This gives a result of all logs with failures.</span></span> 

  ![Ladění neúspěšnou úlohu](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

<span data-ttu-id="d53a2-137">V hello portálu Azure, můžete filtrovat podle úrovně zpráva tooview **kritický** události.</span><span class="sxs-lookup"><span data-stu-id="d53a2-137">In hello Azure portal, you can filter by level of message tooview **Critical** events.</span></span>

  ![Portál Azure ladění](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

<span data-ttu-id="d53a2-139">Můžete vybrat některého hello selhání a klikněte na hello **podrobnosti** pro další informace o chybě hello.</span><span class="sxs-lookup"><span data-stu-id="d53a2-139">You can select any one of hello failures, and click on hello **Details** for more information on hello error.</span></span>  <span data-ttu-id="d53a2-140">Některé chybové zprávy také poskytují informace o tom, jak toomitigate hello problém.</span><span class="sxs-lookup"><span data-stu-id="d53a2-140">Some error messages also provide information about how toomitigate hello issue.</span></span> 

  ![Podrobnosti o operaci](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

<span data-ttu-id="d53a2-142">V případě, že potřebujete toocontact [podporu](https://azure.microsoft.com/support/options/) nebo zadejte informace o toohello team prostřednictvím hello [fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), je potřeba počítat s hello provozní údaje, konkrétně hello **ID korelace**.</span><span class="sxs-lookup"><span data-stu-id="d53a2-142">In case you need toocontact [Support](https://azure.microsoft.com/support/options/) or provide information toohello team via hello [MSDN forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), please note hello Operation Details, specifically hello **Correlation ID**.</span></span> 

## <a name="get-help"></a><span data-ttu-id="d53a2-143">Podpora</span><span class="sxs-lookup"><span data-stu-id="d53a2-143">Get help</span></span>
<span data-ttu-id="d53a2-144">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="d53a2-144">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="d53a2-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d53a2-145">Next steps</span></span>
* [<span data-ttu-id="d53a2-146">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d53a2-146">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="d53a2-147">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d53a2-147">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="d53a2-148">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d53a2-148">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="d53a2-149">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="d53a2-149">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="d53a2-150">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d53a2-150">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

