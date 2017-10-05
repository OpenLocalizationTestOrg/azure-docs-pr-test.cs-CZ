---
title: "Pomocí zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio | Microsoft Docs"
description: "Další informace o použití zobrazení provádění vrcholů na zkoušku úloh Data Lake Analytics."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/13/2016
ms.author: jgao
ms.openlocfilehash: b788e7bc8ded86ebd49cc0be73e5b4e1bcbeaba3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a><span data-ttu-id="ab466-103">Použití zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab466-103">Use the Vertex Execution View in Data Lake Tools for Visual Studio</span></span>
<span data-ttu-id="ab466-104">Další informace o použití zobrazení provádění vrcholů na zkoušku úloh Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="ab466-104">Learn how to use the Vertex Execution View to exam Data Lake Analytics jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab466-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ab466-105">Prerequisites</span></span>

<span data-ttu-id="ab466-106">Potřebujete základní znalosti o vyvíjet skript U-SQL pomocí nástrojů Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ab466-106">You need basic knowledge of using Data Lake Tools for Visual Studio to develop U-SQL script.</span></span>  <span data-ttu-id="ab466-107">V tématu [kurz: vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ab466-107">See [Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>

## <a name="open-the-vertex-execution-view"></a><span data-ttu-id="ab466-108">Otevřete zobrazení provádění vrcholů</span><span class="sxs-lookup"><span data-stu-id="ab466-108">Open the Vertex Execution View</span></span>
<span data-ttu-id="ab466-109">Otevřete úlohy U-SQL v nástrojů Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ab466-109">Open a U-SQL job in Data Lake Tools for Visual Studio.</span></span> <span data-ttu-id="ab466-110">Klikněte na tlačítko **nebo zobrazení provádění vrcholů** v levém dolním rohu.</span><span class="sxs-lookup"><span data-stu-id="ab466-110">Click **Vertex Execution View** in the bottom left corner.</span></span> <span data-ttu-id="ab466-111">Můžete být vyzváni, abyste nejdřív načíst profily a může trvat nějakou dobu v závislosti na připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="ab466-111">You may be prompted to load profiles first and it can take some time depending on your network connectivity.</span></span>

![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a><span data-ttu-id="ab466-113">Pochopení nebo zobrazení provádění vrcholů</span><span class="sxs-lookup"><span data-stu-id="ab466-113">Understand Vertex Execution View</span></span>
<span data-ttu-id="ab466-114">Zobrazení provádění vrcholů má tři části:</span><span class="sxs-lookup"><span data-stu-id="ab466-114">The Vertex Execution View has three parts:</span></span>

![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

<span data-ttu-id="ab466-116">**Vrchol selektor** na levém umožňuje vybrat vrcholy funkce (například top 10 dat pro čtení, nebo zvolte fázi).</span><span class="sxs-lookup"><span data-stu-id="ab466-116">The **Vertex selector** on the left lets you select vertices by features (such as top 10 data read, or choose by stage).</span></span> <span data-ttu-id="ab466-117">Jednou z nejčastěji používaných filtry je zobrazíte **vrcholy kritické cesty**.</span><span class="sxs-lookup"><span data-stu-id="ab466-117">One of the most commonly-used filters is to see the **vertices on critical path**.</span></span> <span data-ttu-id="ab466-118">**Kritické cesty** je nejdelší řetězu vrcholy úlohy U-SQL.</span><span class="sxs-lookup"><span data-stu-id="ab466-118">The **Critical path** is the longest chain of vertices of a U-SQL job.</span></span> <span data-ttu-id="ab466-119">Principy kritické cesty je užitečné pro své úlohy optimalizace kontrolou, které vrchol trvá nejdelší dobu.</span><span class="sxs-lookup"><span data-stu-id="ab466-119">Understanding the critical path is useful for optimizing your jobs by checking which vertex takes the longest time.</span></span>
  
![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

<span data-ttu-id="ab466-121">Zobrazuje podokno nahoře uprostřed podívejte **systémem stav všechny vrcholy**.</span><span class="sxs-lookup"><span data-stu-id="ab466-121">The top center pane shows the **running status of all the vertices**.</span></span>
  
![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

<span data-ttu-id="ab466-123">V dolním podokně center zobrazí informace o jednotlivých vrchol:</span><span class="sxs-lookup"><span data-stu-id="ab466-123">The bottom center pane shows information about each vertex:</span></span>
* <span data-ttu-id="ab466-124">Název procesu: Název instance vrchol.</span><span class="sxs-lookup"><span data-stu-id="ab466-124">Process Name: The name of the vertex instance.</span></span> <span data-ttu-id="ab466-125">Se skládá z různých částí v StageName | VertexName | VertexRunInstance.</span><span class="sxs-lookup"><span data-stu-id="ab466-125">It is composed of different parts in StageName|VertexName|VertexRunInstance.</span></span> <span data-ttu-id="ab466-126">Například vrchol [62] .v1 SV7_Split znamená druhý běžící instance (.v1, index od 0) čísla vrchol 62 ve fázi SV7_Split.</span><span class="sxs-lookup"><span data-stu-id="ab466-126">For example, the SV7_Split[62].v1 vertex stands for the second running instance (.v1, index starting from 0) of Vertex number 62 in Stage SV7_Split.</span></span>
* <span data-ttu-id="ab466-127">Celkový počet dat pro čtení nebo Written: Data byla číst nebo zapisovat pomocí této vrchol.</span><span class="sxs-lookup"><span data-stu-id="ab466-127">Total Data Read/Written: The data was read/written by this vertex.</span></span>
* <span data-ttu-id="ab466-128">Stav stavu nebo ukončení: Konečný stav po ukončení vrchol.</span><span class="sxs-lookup"><span data-stu-id="ab466-128">State/Exit Status: The final status when the vertex is ended.</span></span>
* <span data-ttu-id="ab466-129">Ukončovací kód nebo selhání typ: Chyba při vrchol se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="ab466-129">Exit Code/Failure Type: The error when the vertex failed.</span></span>
* <span data-ttu-id="ab466-130">Vytvoření důvod: Proč vrchol byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="ab466-130">Creation Reason: Why the vertex was created.</span></span>
* <span data-ttu-id="ab466-131">Latence fronty latence/PN prostředků latenci nebo proces: čas potřebný pro vrchol čekání na prostředky, ke zpracování dat a zůstane ve frontě.</span><span class="sxs-lookup"><span data-stu-id="ab466-131">Resource Latency/Process Latency/PN Queue Latency: the time taken for the vertex to wait for resources, to process data, and to stay in the queue.</span></span>
* <span data-ttu-id="ab466-132">Identifikátor GUID procesů nebo autora: Identifikátor GUID pro aktuální spuštěné vrchol nebo jeho tvůrce.</span><span class="sxs-lookup"><span data-stu-id="ab466-132">Process/Creator GUID: GUID for the current running vertex or its creator.</span></span>
* <span data-ttu-id="ab466-133">Verze: N-tý instance spuštěné vrchol (systém může nové instance třídy vrchol naplánovat, pro mnoho důvodů, například převzetí služeb při selhání, výpočetní redundance atd.)</span><span class="sxs-lookup"><span data-stu-id="ab466-133">Version: the N-th instance of the running vertex (the system might schedule new instances of a vertex for many reasons, for example failover, compute redundancy, etc.)</span></span>
* <span data-ttu-id="ab466-134">Verze vytvořena, když.</span><span class="sxs-lookup"><span data-stu-id="ab466-134">Version Created Time.</span></span>
* <span data-ttu-id="ab466-135">Zpracování vytvořit počáteční čas nebo proces zařazeno ve frontě čas nebo proces počáteční čas nebo proces dokončete čas: při zahájení procesu vrchol vytvoření; Při spuštění procesu vrchol do fronty; Při spuštění procesu určité vrchol; Po dokončení určité vrchol.</span><span class="sxs-lookup"><span data-stu-id="ab466-135">Process Create Start Time/Process Queued Time/Process Start Time/Process Complete Time: when the vertex process starts creation; when the vertex process starts to queue; when the certain vertex process starts; when the certain vertex is completed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab466-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab466-136">Next steps</span></span>
* <span data-ttu-id="ab466-137">Pokud chcete protokolovat diagnostické informace, přečtěte si téma [Zobrazení protokolů diagnostiky pro Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="ab466-137">To log diagnostics information, see [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)</span></span>
* <span data-ttu-id="ab466-138">Pokud chcete zobrazit komplexnější dotaz, přejděte k tématu [Analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="ab466-138">To see a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="ab466-139">Chcete-li zobrazit podrobnosti o úlohách, najdete v části [použití úlohy prohlížeče a zobrazení úloh pro úlohy Azure Data lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md)</span><span class="sxs-lookup"><span data-stu-id="ab466-139">To view job details, see [Use Job Browser and Job View for Azure Data lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md)</span></span>
