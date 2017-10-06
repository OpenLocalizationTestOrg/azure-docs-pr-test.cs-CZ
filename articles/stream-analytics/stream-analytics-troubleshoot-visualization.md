---
title: "aaaVisualize úloh služby Stream Analytics a odstraňování potíží | Microsoft Docs"
description: "Zjistěte, jak toovisualize úloha Stream Analytics kanálu pro samoobslužné řešení problémů pomocí funkce diagram hello diagnostiky."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: d87841cd-c59f-4a46-b46e-8b904fdc12e9
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 8a6715be601fdc47b8d9caf4112da161dad22618
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a><span data-ttu-id="32a06-103">Vizualizace a řešení potíží s úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="32a06-103">Visualize and troubleshoot Stream Analytics jobs</span></span>
<span data-ttu-id="32a06-104">V Stream Analytics stejně jako u jiných technologií cloudové řešení potíží s je někdy potřebné toolook do proč úlohu nevytváří hello očekávaný výstup (nebo žádný výstup k tomuto účelu).</span><span class="sxs-lookup"><span data-stu-id="32a06-104">In Stream Analytics, as with other cloud-based technologies, troubleshooting is sometimes needed toolook into why a job does not produce hello expected output (or any output for that matter).</span></span> <span data-ttu-id="32a06-105">Myslete na to Stream Analytics poskytuje možnost hello pro vizualizaci úlohu streamování.</span><span class="sxs-lookup"><span data-stu-id="32a06-105">With this in mind, Stream Analytics provides hello capability for visualizing a streaming job.</span></span> <span data-ttu-id="32a06-106">Toto je také užitečné jako nástroj pro modelování a má hello straně benefit pro ty, které vyžaduje dokumentaci práci.</span><span class="sxs-lookup"><span data-stu-id="32a06-106">This is also handy as a modeling tool and has hello side benefit for those requiring documentation of their work.</span></span>

<span data-ttu-id="32a06-107">V hello vizualizaci jsou viditelné a také hello dotazu spouštěna vstupy hello panelu a pak nakonfigurovat všechny výstupy hello.</span><span class="sxs-lookup"><span data-stu-id="32a06-107">In hello visualization panel hello inputs are visible as well as hello query being executed and then all hello outputs configured.</span></span> <span data-ttu-id="32a06-108">Problémy s připojením nebo konfigurace se může stát více zřejmá a také může být užitečné toosee vizuální reprezentace vaší konfigurace.</span><span class="sxs-lookup"><span data-stu-id="32a06-108">Connectivity or configuration issues can become more apparent and it can also be helpful toosee a visual representation of your configuration.</span></span>

## <a name="using-hello-diagnosis-diagram-tool"></a><span data-ttu-id="32a06-109">Pomocí nástroje diagram diagnostiku hello</span><span class="sxs-lookup"><span data-stu-id="32a06-109">Using hello diagnosis diagram tool</span></span>
<span data-ttu-id="32a06-110">hello tooaccess hello tento vizualizér, jednoduše klikněte na tlačítko "Diagnostiku diagram" v okně "Nastavení" hello úlohy Stream Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="32a06-110">tooaccess this visualizer, simply click on hello “Diagnosis diagram” button in hello “Settings” blade of hello of hello Stream Analytics job.</span></span>

![Stream-Analytics-troubleshoot-Visualization-Diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

<span data-ttu-id="32a06-112">Veškerý vstup a výstup je barevně tooindicate hello aktuální stav této součásti, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="32a06-112">Every input and output is color coded tooindicate hello current state of that component, as shown below.</span></span>

![Stream-Analytics-troubleshoot-Visualization-Input-map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

<span data-ttu-id="32a06-114">Pokud chce uživatel hello toolook na zprostředkující kroky toounderstand hello datového toku vzorům dotazů uvnitř úlohy, nástroj pro vizualizaci hello poskytuje zobrazení obsahuje rozpis hello hello dotazu do jeho součásti kroky a pořadí toku hello.</span><span class="sxs-lookup"><span data-stu-id="32a06-114">When hello user wants toolook at intermediate query steps toounderstand hello data flow patterns inside a job, hello visualization tool provides a view of hello breakdown of hello query into its component steps and hello flow sequence.</span></span> <span data-ttu-id="32a06-115">Kliknutím na každého kroku dotazu se zobrazí odpovídající části hello v dotazu úpravy podokně podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="32a06-115">Clicking on each query step will show hello corresponding section in a query editing pane as illustrated.</span></span> 

![Stream-Analytics-troubleshoot-Visualization-Intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a><span data-ttu-id="32a06-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="32a06-117">Next steps</span></span>
* [<span data-ttu-id="32a06-118">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="32a06-118">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="32a06-119">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="32a06-119">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="32a06-120">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="32a06-120">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="32a06-121">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="32a06-121">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="32a06-122">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="32a06-122">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

