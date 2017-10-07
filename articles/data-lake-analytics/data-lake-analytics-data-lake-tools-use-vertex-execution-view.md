---
title: "aaaUse hello nebo zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio | Microsoft Docs"
description: "Zjistěte, jak toouse hello úloh Data Lake Analytics tooexam nebo zobrazení provádění vrcholů."
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
ms.openlocfilehash: fb54d2af8a32aa27a54ff50a73c1b4903677a21e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a><span data-ttu-id="71df1-103">Použití hello nebo zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71df1-103">Use hello Vertex Execution View in Data Lake Tools for Visual Studio</span></span>
<span data-ttu-id="71df1-104">Zjistěte, jak toouse hello úloh Data Lake Analytics tooexam nebo zobrazení provádění vrcholů.</span><span class="sxs-lookup"><span data-stu-id="71df1-104">Learn how toouse hello Vertex Execution View tooexam Data Lake Analytics jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71df1-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="71df1-105">Prerequisites</span></span>

<span data-ttu-id="71df1-106">Potřebujete základní znalosti o pomocí nástrojů Data Lake pro Visual Studio toodevelop U-SQL skriptů.</span><span class="sxs-lookup"><span data-stu-id="71df1-106">You need basic knowledge of using Data Lake Tools for Visual Studio toodevelop U-SQL script.</span></span>  <span data-ttu-id="71df1-107">V tématu [kurz: vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="71df1-107">See [Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>

## <a name="open-hello-vertex-execution-view"></a><span data-ttu-id="71df1-108">Otevřete zobrazení provádění vrcholů hello</span><span class="sxs-lookup"><span data-stu-id="71df1-108">Open hello Vertex Execution View</span></span>
<span data-ttu-id="71df1-109">Otevřete úlohy U-SQL v nástrojů Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71df1-109">Open a U-SQL job in Data Lake Tools for Visual Studio.</span></span> <span data-ttu-id="71df1-110">Klikněte na tlačítko **nebo zobrazení provádění vrcholů** v levém dolním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="71df1-110">Click **Vertex Execution View** in hello bottom left corner.</span></span> <span data-ttu-id="71df1-111">Profily výzvami tooload může být nejprve a může trvat nějakou dobu v závislosti na připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="71df1-111">You may be prompted tooload profiles first and it can take some time depending on your network connectivity.</span></span>

![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a><span data-ttu-id="71df1-113">Pochopení nebo zobrazení provádění vrcholů</span><span class="sxs-lookup"><span data-stu-id="71df1-113">Understand Vertex Execution View</span></span>
<span data-ttu-id="71df1-114">Hello nebo zobrazení provádění vrcholů má tři části:</span><span class="sxs-lookup"><span data-stu-id="71df1-114">hello Vertex Execution View has three parts:</span></span>

![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

<span data-ttu-id="71df1-116">Hello **vrchol selektor** na levém umožňuje hello vyberte vrcholy funkce (například top 10 dat pro čtení, nebo zvolte fázi).</span><span class="sxs-lookup"><span data-stu-id="71df1-116">hello **Vertex selector** on hello left lets you select vertices by features (such as top 10 data read, or choose by stage).</span></span> <span data-ttu-id="71df1-117">Jednou z nejčastěji používaných filtry hello je toosee hello **vrcholy kritické cesty**.</span><span class="sxs-lookup"><span data-stu-id="71df1-117">One of hello most commonly-used filters is toosee hello **vertices on critical path**.</span></span> <span data-ttu-id="71df1-118">Hello **kritické cesty** je hello nejdelší řetězu vrcholy úlohy U-SQL.</span><span class="sxs-lookup"><span data-stu-id="71df1-118">hello **Critical path** is hello longest chain of vertices of a U-SQL job.</span></span> <span data-ttu-id="71df1-119">Principy hello kritická cesta je užitečné pro optimalizaci úlohách kontrolou, které vrchol trvá hello nejdelší dobu.</span><span class="sxs-lookup"><span data-stu-id="71df1-119">Understanding hello critical path is useful for optimizing your jobs by checking which vertex takes hello longest time.</span></span>
  
![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

<span data-ttu-id="71df1-121">Hello nejvyšší prostředním podokně zobrazí hello **systémem stav všechny vrcholy hello**.</span><span class="sxs-lookup"><span data-stu-id="71df1-121">hello top center pane shows hello **running status of all hello vertices**.</span></span>
  
![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

<span data-ttu-id="71df1-123">Hello dolní prostředním podokně se zobrazují informace o jednotlivých vrchol:</span><span class="sxs-lookup"><span data-stu-id="71df1-123">hello bottom center pane shows information about each vertex:</span></span>
* <span data-ttu-id="71df1-124">Název procesu: název hello hello vrchol instance.</span><span class="sxs-lookup"><span data-stu-id="71df1-124">Process Name: hello name of hello vertex instance.</span></span> <span data-ttu-id="71df1-125">Se skládá z různých částí v StageName | VertexName | VertexRunInstance.</span><span class="sxs-lookup"><span data-stu-id="71df1-125">It is composed of different parts in StageName|VertexName|VertexRunInstance.</span></span> <span data-ttu-id="71df1-126">Například hello SV7_Split [62] .v1 vrchol znamená hello druhé spuštěné instance (.v1, index od 0) čísla vrchol 62 ve fázi SV7_Split.</span><span class="sxs-lookup"><span data-stu-id="71df1-126">For example, hello SV7_Split[62].v1 vertex stands for hello second running instance (.v1, index starting from 0) of Vertex number 62 in Stage SV7_Split.</span></span>
* <span data-ttu-id="71df1-127">Celkový počet dat pro čtení nebo Written: hello data byla číst nebo zapisovat pomocí této vrchol.</span><span class="sxs-lookup"><span data-stu-id="71df1-127">Total Data Read/Written: hello data was read/written by this vertex.</span></span>
* <span data-ttu-id="71df1-128">Stav stavu nebo ukončení: hello konečného stavu po ukončení vrchol hello.</span><span class="sxs-lookup"><span data-stu-id="71df1-128">State/Exit Status: hello final status when hello vertex is ended.</span></span>
* <span data-ttu-id="71df1-129">Ukončovací kód nebo selhání typ: hello došlo k chybě při vrchol hello se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="71df1-129">Exit Code/Failure Type: hello error when hello vertex failed.</span></span>
* <span data-ttu-id="71df1-130">Vytvoření důvod: Proč hello vrchol byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="71df1-130">Creation Reason: Why hello vertex was created.</span></span>
* <span data-ttu-id="71df1-131">Latence fronty latence/PN prostředků latenci nebo proces: hello doba hello vrchol toowait pro prostředky, tooprocess data a toostay ve frontě hello.</span><span class="sxs-lookup"><span data-stu-id="71df1-131">Resource Latency/Process Latency/PN Queue Latency: hello time taken for hello vertex toowait for resources, tooprocess data, and toostay in hello queue.</span></span>
* <span data-ttu-id="71df1-132">Identifikátor GUID procesů nebo autora: Identifikátor GUID pro aktuální spuštěné vrchol hello nebo jeho tvůrce.</span><span class="sxs-lookup"><span data-stu-id="71df1-132">Process/Creator GUID: GUID for hello current running vertex or its creator.</span></span>
* <span data-ttu-id="71df1-133">Verze: hello N-tý instanci hello systémem vrchol (hello systému se může naplánovat nové instance třídy vrchol, pro mnoho důvodů, například převzetí služeb při selhání, výpočetní redundance atd.)</span><span class="sxs-lookup"><span data-stu-id="71df1-133">Version: hello N-th instance of hello running vertex (hello system might schedule new instances of a vertex for many reasons, for example failover, compute redundancy, etc.)</span></span>
* <span data-ttu-id="71df1-134">Verze vytvořena, když.</span><span class="sxs-lookup"><span data-stu-id="71df1-134">Version Created Time.</span></span>
* <span data-ttu-id="71df1-135">Zpracování vytvořit počáteční čas nebo proces zařazeno ve frontě čas nebo proces počáteční čas nebo proces dokončete čas: při zahájení procesu vrchol hello vytvoření; Při spuštění procesu vrchol hello tooqueue; Když hello určité spustí proces vrchol; Když hello určité vrchol byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="71df1-135">Process Create Start Time/Process Queued Time/Process Start Time/Process Complete Time: when hello vertex process starts creation; when hello vertex process starts tooqueue; when hello certain vertex process starts; when hello certain vertex is completed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="71df1-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71df1-136">Next steps</span></span>
* <span data-ttu-id="71df1-137">toolog diagnostické informace, najdete v části [přístup k protokolů diagnostiky pro Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)</span><span class="sxs-lookup"><span data-stu-id="71df1-137">toolog diagnostics information, see [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)</span></span>
* <span data-ttu-id="71df1-138">toosee komplexnější dotaz, najdete v části [analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="71df1-138">toosee a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="71df1-139">Podrobnosti úlohy tooview, najdete v tématu [použití úlohy prohlížeče a zobrazení úloh pro úlohy Azure Data lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md)</span><span class="sxs-lookup"><span data-stu-id="71df1-139">tooview job details, see [Use Job Browser and Job View for Azure Data lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md)</span></span>
