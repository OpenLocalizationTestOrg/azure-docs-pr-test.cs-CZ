---
title: "událost aaaReal čas zpracování pomocí zpracování událostí Stream Analytics | Microsoft Docs"
description: "Zjistěte, jak můžou spolupracovat sadu Azure services pro povolení zpracování událostí v reálném čase a analýzy."
keywords: "zpracování v reálném čase, událostí zpracování, referenční architektura"
services: stream-analytics,event-hubs,storage,sql-database
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: 
ms.assetid: 11af48bc-313c-4527-8c80-91088dc9f3c6
ms.service: stream-analytics
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: jeffstok
ms.openlocfilehash: a43c503d709609ba61e9932822d30bc2208906ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a><span data-ttu-id="8685c-104">Referenční architektura: událostí v reálném čase zpracování pomocí služby Microsoft Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8685c-104">Reference architecture: Real-time event processing with Microsoft Azure Stream Analytics</span></span>
<span data-ttu-id="8685c-105">Hello referenční architektura pro zpracování pomocí služby Azure Stream Analytics událostí v reálném čase je určený tooprovide obecný plán pro nasazení v reálném čase platforma jako služba (PaaS) řešení zpracování datového proudu s Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="8685c-105">hello reference architecture for real-time event processing with Azure Stream Analytics is intended tooprovide a generic blueprint for deploying a real-time platform as a service (PaaS) stream-processing solution with Microsoft Azure.</span></span>

## <a name="summary"></a><span data-ttu-id="8685c-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="8685c-106">Summary</span></span>
<span data-ttu-id="8685c-107">Obvyklým řešení pro analýzu měla vycházet z funkcí, jako je ETL (extrakce, transformace, zatížení) a datových skladů, kde data jsou uložené před tooanalysis.</span><span class="sxs-lookup"><span data-stu-id="8685c-107">Traditionally, analytics solutions have been based on capabilities such as ETL (extract, transform, load) and data warehousing, where data is stored prior tooanalysis.</span></span> <span data-ttu-id="8685c-108">Změna požadavky, včetně další data rychle které nabízíte tento existující toohello limit modelu.</span><span class="sxs-lookup"><span data-stu-id="8685c-108">Changing requirements, including more rapidly arriving data, are pushing this existing model toohello limit.</span></span> <span data-ttu-id="8685c-109">Hello možnost tooanalyze dat v rámci přesunutí datové proudy předchozí toostorage je jedno řešení a i když není novou funkci, hello přístup nebyl přijat široce mezi všechny pocházejícími odvětví.</span><span class="sxs-lookup"><span data-stu-id="8685c-109">hello ability tooanalyze data within moving streams prior toostorage is one solution, and while it is not a new capability, hello approach has not been widely adopted across all industry verticals.</span></span> 

<span data-ttu-id="8685c-110">Microsoft Azure poskytuje katalog rozsáhlé analytics technologií, které podporují řadu řešení pro různé scénáře a požadavky na podporu.</span><span class="sxs-lookup"><span data-stu-id="8685c-110">Microsoft Azure provides an extensive catalog of analytics technologies that are capable of supporting an array of different solution scenarios and requirements.</span></span> <span data-ttu-id="8685c-111">Vyberte, které toodeploy služby Azure pro-komplexní řešení může být složité zadané hello spektra nabídek.</span><span class="sxs-lookup"><span data-stu-id="8685c-111">Selecting which Azure services toodeploy for an end-to-end solution can be a challenge given hello breadth of offerings.</span></span> <span data-ttu-id="8685c-112">Tento dokument je navrženou toodescribe hello funkce a vzájemná spolupráce z hello různé služby Azure, které podporují řešení vysílání datového proudu událostí.</span><span class="sxs-lookup"><span data-stu-id="8685c-112">This paper is designed toodescribe hello capabilities and interoperation of hello various Azure services that support an event-streaming solution.</span></span> <span data-ttu-id="8685c-113">Také vysvětluje některé hello scénáře, ve kterých zákazníci využívat tento typ přístupu.</span><span class="sxs-lookup"><span data-stu-id="8685c-113">It also explains some of hello scenarios in which customers can benefit from this type of approach.</span></span>

## <a name="contents"></a><span data-ttu-id="8685c-114">Obsah</span><span class="sxs-lookup"><span data-stu-id="8685c-114">Contents</span></span>
* <span data-ttu-id="8685c-115">Shrnutí</span><span class="sxs-lookup"><span data-stu-id="8685c-115">Executive Summary</span></span>
* <span data-ttu-id="8685c-116">Analýza tooReal čas Úvod</span><span class="sxs-lookup"><span data-stu-id="8685c-116">Introduction tooReal-Time Analytics</span></span>
* <span data-ttu-id="8685c-117">Nabízená hodnota dat v reálném čase v Azure</span><span class="sxs-lookup"><span data-stu-id="8685c-117">Value Proposition of Real-Time Data in Azure</span></span>
* <span data-ttu-id="8685c-118">Časté scénáře pro analýzu v reálném čase</span><span class="sxs-lookup"><span data-stu-id="8685c-118">Common Scenarios for Real-Time Analytics</span></span>
* <span data-ttu-id="8685c-119">Architektura a komponenty</span><span class="sxs-lookup"><span data-stu-id="8685c-119">Architecture and Components</span></span>
  * <span data-ttu-id="8685c-120">Zdroje dat</span><span class="sxs-lookup"><span data-stu-id="8685c-120">Data Sources</span></span>
  * <span data-ttu-id="8685c-121">Vrstva integraci dat.</span><span class="sxs-lookup"><span data-stu-id="8685c-121">Data-Integration Layer</span></span>
  * <span data-ttu-id="8685c-122">Vrstva analýzu v reálném čase</span><span class="sxs-lookup"><span data-stu-id="8685c-122">Real-time Analytics Layer</span></span>
  * <span data-ttu-id="8685c-123">Vrstvy úložiště dat</span><span class="sxs-lookup"><span data-stu-id="8685c-123">Data Storage Layer</span></span>
  * <span data-ttu-id="8685c-124">Prezentace / spotřeba vrstvy</span><span class="sxs-lookup"><span data-stu-id="8685c-124">Presentation / Consumption Layer</span></span>
* <span data-ttu-id="8685c-125">Závěr</span><span class="sxs-lookup"><span data-stu-id="8685c-125">Conclusion</span></span>

<span data-ttu-id="8685c-126">**Autor:** Charlese Feddersen, architekt řešení datového centra Statistika vynikající výsledky, společnosti Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="8685c-126">**Author:** Charles Feddersen, Solution Architect, Data Insights Center of Excellence, Microsoft Corporation</span></span>

<span data-ttu-id="8685c-127">**Publikovat:** leden 2015</span><span class="sxs-lookup"><span data-stu-id="8685c-127">**Published:** January 2015</span></span>

<span data-ttu-id="8685c-128">**Revize:** 1.0</span><span class="sxs-lookup"><span data-stu-id="8685c-128">**Revision:** 1.0</span></span>

<span data-ttu-id="8685c-129">**Stáhnout:** [událostí v reálném čase zpracování pomocí služby Microsoft Azure Stream Analytics](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)</span><span class="sxs-lookup"><span data-stu-id="8685c-129">**Download:** [Real-Time Event Processing with Microsoft Azure Stream Analytics](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)</span></span>

## <a name="get-help"></a><span data-ttu-id="8685c-130">Podpora</span><span class="sxs-lookup"><span data-stu-id="8685c-130">Get help</span></span>
<span data-ttu-id="8685c-131">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="8685c-131">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="8685c-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8685c-132">Next steps</span></span>
* [<span data-ttu-id="8685c-133">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8685c-133">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="8685c-134">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8685c-134">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="8685c-135">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8685c-135">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="8685c-136">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="8685c-136">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="8685c-137">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8685c-137">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

