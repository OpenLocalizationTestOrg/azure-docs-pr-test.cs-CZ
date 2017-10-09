---
title: "testování dotazů aaaAzure Stream Analytics | Microsoft Docs"
description: "Jak tootest své dotazy v úlohy Stream Analytics."
keywords: "Test dotazu, řešení potíží s dotazu"
documentation center: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 3b141d98332fdc170e696e181c8446796a86f78e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="test-azure-stream-analytics-queries-in-hello-azure-portal"></a><span data-ttu-id="e74e2-104">Testování dotazů Azure Stream Analytics v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e74e2-104">Test Azure Stream Analytics queries in hello Azure portal</span></span>

<span data-ttu-id="e74e2-105">Pomocí služby Azure Stream Analytics můžete otestovat dotazy v hello portál Azure bez nutnosti toostart nebo zastavení úlohy.</span><span class="sxs-lookup"><span data-stu-id="e74e2-105">With Azure Stream Analytics, you can test queries in hello Azure portal without needing toostart or stop a job.</span></span>

## <a name="test-hello-input"></a><span data-ttu-id="e74e2-106">Test hello vstup</span><span class="sxs-lookup"><span data-stu-id="e74e2-106">Test hello input</span></span>

1. <span data-ttu-id="e74e2-107">Klikněte pravým tlačítkem na vstupy tootest s ukázkovými daty vstupní a potom vyberte **nahrát ukázková data ze souboru**.</span><span class="sxs-lookup"><span data-stu-id="e74e2-107">tootest with sample input data, right-click any of your inputs, and then select **Upload sample data from file**.</span></span>

    ![Stream analytics dotaz editor testu dotazu](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. <span data-ttu-id="e74e2-109">Po dokončení nahrávání hello klikněte na tlačítko **Test** tootest tento dotaz hello ukázková data, která jste zadali.</span><span class="sxs-lookup"><span data-stu-id="e74e2-109">After hello upload is complete, click **Test** tootest this query against hello sample data you have provided.</span></span>

    ![Dotaz služby Stream analytics editor testu ukázková data](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

<span data-ttu-id="e74e2-111">Hello výstup tohoto dotazu se zobrazí v prohlížeči hello s výsledky odkaz ke stažení, budete chtít výstup testu hello toosave pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="e74e2-111">hello output of your query is displayed in hello browser, with Download results link should you want toosave hello test output for later use.</span></span> <span data-ttu-id="e74e2-112">Teď můžete snadno a interaktivně upravte dotaz a otestovat ji opakovaně toosee jak výstup hello změny.</span><span class="sxs-lookup"><span data-stu-id="e74e2-112">You can now easily and iteratively modify your query and test it repeatedly toosee how hello output changes.</span></span>

![Stream Analytics query editor ukázkový výstup](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

<span data-ttu-id="e74e2-114">S více výstupů použitých v dotazu můžete zobrazit výsledky hello pro obě výstupy samostatně a snadno přepínat mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="e74e2-114">With multiple outputs used in a query, you can see hello results for both outputs separately and easily toggle between them.</span></span>

<span data-ttu-id="e74e2-115">Jakmile budete spokojeni s hello výsledky zobrazené v prohlížeči hello, můžete uložit dotazu, spusťte úlohu a nechat zpracovat události bez chyby.</span><span class="sxs-lookup"><span data-stu-id="e74e2-115">After you are satisfied with hello results shown in hello browser, you can save your query, start your job, and let it process events without error.</span></span>

## <a name="get-help"></a><span data-ttu-id="e74e2-116">Podpora</span><span class="sxs-lookup"><span data-stu-id="e74e2-116">Get help</span></span>

<span data-ttu-id="e74e2-117">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="e74e2-117">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e74e2-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e74e2-118">Next steps</span></span>

* [<span data-ttu-id="e74e2-119">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e74e2-119">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="e74e2-120">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e74e2-120">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="e74e2-121">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e74e2-121">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="e74e2-122">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="e74e2-122">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="e74e2-123">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="e74e2-123">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
