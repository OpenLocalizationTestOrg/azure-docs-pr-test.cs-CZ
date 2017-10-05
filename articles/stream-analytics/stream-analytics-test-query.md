---
title: "Testování dotazů v Azure Stream Analytics | Microsoft Docs"
description: "Postup testování vašich dotazů v úlohy Stream Analytics."
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
ms.openlocfilehash: 16bb3f26ec3a69e5204162db9e54a186cf1ec6a6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="test-azure-stream-analytics-queries-in-the-azure-portal"></a><span data-ttu-id="24b66-104">Testování dotazů Azure Stream Analytics na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="24b66-104">Test Azure Stream Analytics queries in the Azure portal</span></span>

<span data-ttu-id="24b66-105">Pomocí služby Azure Stream Analytics můžete otestovat dotazy na portálu Azure bez nutnosti spuštění nebo zastavení úlohy.</span><span class="sxs-lookup"><span data-stu-id="24b66-105">With Azure Stream Analytics, you can test queries in the Azure portal without needing to start or stop a job.</span></span>

## <a name="test-the-input"></a><span data-ttu-id="24b66-106">Testování vstupu</span><span class="sxs-lookup"><span data-stu-id="24b66-106">Test the input</span></span>

1. <span data-ttu-id="24b66-107">Chcete-li otestovat s ukázkovými daty vstupní, klikněte pravým tlačítkem na vstupy a pak vyberte **nahrát ukázková data ze souboru**.</span><span class="sxs-lookup"><span data-stu-id="24b66-107">To test with sample input data, right-click any of your inputs, and then select **Upload sample data from file**.</span></span>

    ![Stream analytics dotaz editor testu dotazu](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. <span data-ttu-id="24b66-109">Po dokončení nahrávání se klikněte na tlačítko **testování** k testování tento dotaz ukázková data, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="24b66-109">After the upload is complete, click **Test** to test this query against the sample data you have provided.</span></span>

    ![Dotaz služby Stream analytics editor testu ukázková data](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

<span data-ttu-id="24b66-111">Výstup tohoto dotazu se zobrazí v prohlížeči s výsledky odkaz ke stažení, budete chtít uložit výstup testu pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="24b66-111">The output of your query is displayed in the browser, with Download results link should you want to save the test output for later use.</span></span> <span data-ttu-id="24b66-112">Teď můžete snadno a interaktivně upravte dotaz a otestovat opakovaně a najdete v části Jak výstup změny.</span><span class="sxs-lookup"><span data-stu-id="24b66-112">You can now easily and iteratively modify your query and test it repeatedly to see how the output changes.</span></span>

![Stream Analytics query editor ukázkový výstup](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

<span data-ttu-id="24b66-114">S více výstupů použitých v dotazu můžete zobrazit výsledky pro obě výstupy samostatně a snadno přepínat mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="24b66-114">With multiple outputs used in a query, you can see the results for both outputs separately and easily toggle between them.</span></span>

<span data-ttu-id="24b66-115">Jakmile budete spokojeni se na výsledky zobrazené v prohlížeči, můžete uložit dotazu, spusťte úlohu a nechat zpracovat události bez chyby.</span><span class="sxs-lookup"><span data-stu-id="24b66-115">After you are satisfied with the results shown in the browser, you can save your query, start your job, and let it process events without error.</span></span>

## <a name="get-help"></a><span data-ttu-id="24b66-116">Podpora</span><span class="sxs-lookup"><span data-stu-id="24b66-116">Get help</span></span>

<span data-ttu-id="24b66-117">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="24b66-117">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="24b66-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24b66-118">Next steps</span></span>

* [<span data-ttu-id="24b66-119">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="24b66-119">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="24b66-120">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="24b66-120">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="24b66-121">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="24b66-121">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="24b66-122">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="24b66-122">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="24b66-123">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="24b66-123">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
