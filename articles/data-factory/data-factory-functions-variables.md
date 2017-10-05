---
title: "Funkce pro vytváření dat a systémové proměnné | Microsoft Docs"
description: "Obsahuje seznam funkcí Azure Data Factory a systémové proměnné"
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
services: data-factory
ms.assetid: b6b3c2ae-b0e8-4e28-90d8-daf20421660d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 72a966bdc271f86b9568d3310d2e22d83b447594
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-data-factory---functions-and-system-variables"></a><span data-ttu-id="91d58-103">Azure Data Factory – funkce a systémové proměnné</span><span class="sxs-lookup"><span data-stu-id="91d58-103">Azure Data Factory - Functions and System Variables</span></span>
<span data-ttu-id="91d58-104">Tento článek obsahuje informace o funkcích a proměnné, podporované službou Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="91d58-104">This article provides information about functions and variables supported by Azure Data Factory.</span></span>

## <a name="data-factory-system-variables"></a><span data-ttu-id="91d58-105">Data Factory systémové proměnné</span><span class="sxs-lookup"><span data-stu-id="91d58-105">Data Factory system variables</span></span>
| <span data-ttu-id="91d58-106">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="91d58-106">Variable Name</span></span> | <span data-ttu-id="91d58-107">Popis</span><span class="sxs-lookup"><span data-stu-id="91d58-107">Description</span></span> | <span data-ttu-id="91d58-108">Objekt oboru</span><span class="sxs-lookup"><span data-stu-id="91d58-108">Object Scope</span></span> | <span data-ttu-id="91d58-109">Obor JSON a případy použití</span><span class="sxs-lookup"><span data-stu-id="91d58-109">JSON Scope and Use Cases</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="91d58-110">WindowStart</span><span class="sxs-lookup"><span data-stu-id="91d58-110">WindowStart</span></span> |<span data-ttu-id="91d58-111">Začátek časového intervalu pro aktuální aktivity při spuštění okna</span><span class="sxs-lookup"><span data-stu-id="91d58-111">Start of time interval for current activity run window</span></span> |<span data-ttu-id="91d58-112">Aktivity</span><span class="sxs-lookup"><span data-stu-id="91d58-112">activity</span></span> |<ol><li><span data-ttu-id="91d58-113">Zadejte data výběr dotazy.</span><span class="sxs-lookup"><span data-stu-id="91d58-113">Specify data selection queries.</span></span> <span data-ttu-id="91d58-114">Najdete v části konektor články v odkazuje [aktivity přesunu dat](data-factory-data-movement-activities.md) článku.</span><span class="sxs-lookup"><span data-stu-id="91d58-114">See connector articles referenced in the [Data Movement Activities](data-factory-data-movement-activities.md) article.</span></span></li> |
| <span data-ttu-id="91d58-115">WindowEnd</span><span class="sxs-lookup"><span data-stu-id="91d58-115">WindowEnd</span></span> |<span data-ttu-id="91d58-116">Konec časového intervalu pro aktuální aktivity při spuštění okna</span><span class="sxs-lookup"><span data-stu-id="91d58-116">End of time interval for current activity run window</span></span> |<span data-ttu-id="91d58-117">Aktivity</span><span class="sxs-lookup"><span data-stu-id="91d58-117">activity</span></span> |<span data-ttu-id="91d58-118">stejné jako WindowStart.</span><span class="sxs-lookup"><span data-stu-id="91d58-118">same as WindowStart.</span></span> |
| <span data-ttu-id="91d58-119">SliceStart</span><span class="sxs-lookup"><span data-stu-id="91d58-119">SliceStart</span></span> |<span data-ttu-id="91d58-120">Začátek časového intervalu pro datový řez vytvářen</span><span class="sxs-lookup"><span data-stu-id="91d58-120">Start of time interval for data  slice being produced</span></span> |<span data-ttu-id="91d58-121">Aktivity</span><span class="sxs-lookup"><span data-stu-id="91d58-121">activity</span></span><br/><span data-ttu-id="91d58-122">Datové sady</span><span class="sxs-lookup"><span data-stu-id="91d58-122">dataset</span></span> |<ol><li><span data-ttu-id="91d58-123">Zadejte složku dynamické cesty a názvy souborů při práci s [objektů Blob v Azure](data-factory-azure-blob-connector.md) a [datové sady systém souborů](data-factory-onprem-file-system-connector.md).</span><span class="sxs-lookup"><span data-stu-id="91d58-123">Specify dynamic folder paths and file names while working with [Azure Blob](data-factory-azure-blob-connector.md) and [File System datasets](data-factory-onprem-file-system-connector.md).</span></span></li><li><span data-ttu-id="91d58-124">Určení závislostí vstupní pomocí funkce objekt pro vytváření dat v kolekci vstupy aktivity.</span><span class="sxs-lookup"><span data-stu-id="91d58-124">Specify input dependencies with data factory functions in activity inputs collection.</span></span></li></ol> |
| <span data-ttu-id="91d58-125">SliceEnd</span><span class="sxs-lookup"><span data-stu-id="91d58-125">SliceEnd</span></span> |<span data-ttu-id="91d58-126">Konec časového intervalu pro aktuální datový řez.</span><span class="sxs-lookup"><span data-stu-id="91d58-126">End of time interval for current data slice.</span></span> |<span data-ttu-id="91d58-127">Aktivity</span><span class="sxs-lookup"><span data-stu-id="91d58-127">activity</span></span><br/><span data-ttu-id="91d58-128">Datové sady</span><span class="sxs-lookup"><span data-stu-id="91d58-128">dataset</span></span> |<span data-ttu-id="91d58-129">stejné jako SliceStart.</span><span class="sxs-lookup"><span data-stu-id="91d58-129">same as SliceStart.</span></span> |

> [!NOTE]
> <span data-ttu-id="91d58-130">Objekt pro vytváření dat aktuálně vyžaduje, aby plán určeného v aktivitě přesně odpovídá plán zadaný v dostupnosti výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="91d58-130">Currently data factory requires that the schedule specified in the activity exactly matches the schedule specified in availability of the output dataset.</span></span> <span data-ttu-id="91d58-131">Proto WindowStart, WindowEnd a SliceStart a SliceEnd vždy mapují na stejné časové období a jednu výstupní řez.</span><span class="sxs-lookup"><span data-stu-id="91d58-131">Therefore, WindowStart, WindowEnd, and SliceStart and SliceEnd always map to the same time period and a single output slice.</span></span>
> 

### <a name="example-for-using-a-system-variable"></a><span data-ttu-id="91d58-132">Příklad použití systémové proměnné</span><span class="sxs-lookup"><span data-stu-id="91d58-132">Example for using a system variable</span></span>
<span data-ttu-id="91d58-133">V následujícím příkladu, rok, měsíc, den a čas **SliceStart** extrahují do samostatné proměnné, které jsou používány **folderPath** a **fileName** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="91d58-133">In the following example, year, month, day, and time of **SliceStart** are extracted into separate variables that are used by **folderPath** and **fileName** properties.</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

## <a name="data-factory-functions"></a><span data-ttu-id="91d58-134">Funkce pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="91d58-134">Data Factory functions</span></span>
<span data-ttu-id="91d58-135">Můžete použít funkce v datové továrně společně s systémové proměnné pro následující účely:</span><span class="sxs-lookup"><span data-stu-id="91d58-135">You can use functions in data factory along with system variables for the following purposes:</span></span>

1. <span data-ttu-id="91d58-136">Určení dotazy výběru dat (najdete v článcích konektor odkazuje [aktivity přesunu dat](data-factory-data-movement-activities.md) článku.</span><span class="sxs-lookup"><span data-stu-id="91d58-136">Specifying data selection queries (see connector articles referenced by the [Data Movement Activities](data-factory-data-movement-activities.md) article.</span></span>
   
   <span data-ttu-id="91d58-137">Tady je syntax k vyvolání funkce objektu pro vytváření dat:  **$$ <function>**  pro dotazy výběru dat a další vlastnosti aktivity a datové sady.</span><span class="sxs-lookup"><span data-stu-id="91d58-137">The syntax to invoke a data factory function is: **$$<function>** for data selection queries and other properties in the activity and datasets.</span></span>  
2. <span data-ttu-id="91d58-138">Určení vstupní závislosti pomocí funkce objekt pro vytváření dat v kolekci vstupy aktivity.</span><span class="sxs-lookup"><span data-stu-id="91d58-138">Specifying input dependencies with data factory functions in activity inputs collection.</span></span>
   
    <span data-ttu-id="91d58-139">$$ není potřeba pro zadání vstupní závislost výrazy.</span><span class="sxs-lookup"><span data-stu-id="91d58-139">$$ is not needed for specifying input dependency expressions.</span></span>     

<span data-ttu-id="91d58-140">V následující ukázce **sqlReaderQuery** vlastnost v souboru JSON je přiřazena k hodnoty vrácené `Text.Format` funkce.</span><span class="sxs-lookup"><span data-stu-id="91d58-140">In the following sample, **sqlReaderQuery** property in a JSON file is assigned to a value returned by the `Text.Format` function.</span></span> <span data-ttu-id="91d58-141">Tato ukázka také používá systém proměnné s názvem **WindowStart**, která reprezentuje čas zahájení okna aktivity při spuštění.</span><span class="sxs-lookup"><span data-stu-id="91d58-141">This sample also uses a system variable named **WindowStart**, which represents the start time of the activity run window.</span></span>

```json
{
    "Type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
}
```

<span data-ttu-id="91d58-142">V tématu [vlastní řetězců data a času formát](https://msdn.microsoft.com/library/8kb3ddd4.aspx) téma, které popisuje různé možnosti formátování můžete použít (Příklad: zobrazit oproti rrrr).</span><span class="sxs-lookup"><span data-stu-id="91d58-142">See [Custom Date and Time Format Strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx) topic that describes different formatting options you can use (for example: ay vs. yyyy).</span></span> 

### <a name="functions"></a><span data-ttu-id="91d58-143">Funkce</span><span class="sxs-lookup"><span data-stu-id="91d58-143">Functions</span></span>
<span data-ttu-id="91d58-144">V následujících tabulkách najdete všechny funkce v Azure Data Factory:</span><span class="sxs-lookup"><span data-stu-id="91d58-144">The following tables list all the functions in Azure Data Factory:</span></span>

| <span data-ttu-id="91d58-145">Kategorie</span><span class="sxs-lookup"><span data-stu-id="91d58-145">Category</span></span> | <span data-ttu-id="91d58-146">Funkce</span><span class="sxs-lookup"><span data-stu-id="91d58-146">Function</span></span> | <span data-ttu-id="91d58-147">Parametry</span><span class="sxs-lookup"><span data-stu-id="91d58-147">Parameters</span></span> | <span data-ttu-id="91d58-148">Popis</span><span class="sxs-lookup"><span data-stu-id="91d58-148">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="91d58-149">Čas</span><span class="sxs-lookup"><span data-stu-id="91d58-149">Time</span></span> |<span data-ttu-id="91d58-150">AddHours(X,Y)</span><span class="sxs-lookup"><span data-stu-id="91d58-150">AddHours(X,Y)</span></span> |<span data-ttu-id="91d58-151">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-151">X: DateTime</span></span> <br/><br/><span data-ttu-id="91d58-152">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="91d58-152">Y: int</span></span> |<span data-ttu-id="91d58-153">Přidá do okamžiku X Y hodin.</span><span class="sxs-lookup"><span data-stu-id="91d58-153">Adds Y hours to the given time X.</span></span> <br/><br/><span data-ttu-id="91d58-154">Příklad:`9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="91d58-154">Example: `9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM`</span></span> |
| <span data-ttu-id="91d58-155">Čas</span><span class="sxs-lookup"><span data-stu-id="91d58-155">Time</span></span> |<span data-ttu-id="91d58-156">AddMinutes(X,Y)</span><span class="sxs-lookup"><span data-stu-id="91d58-156">AddMinutes(X,Y)</span></span> |<span data-ttu-id="91d58-157">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-157">X: DateTime</span></span> <br/><br/><span data-ttu-id="91d58-158">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="91d58-158">Y: int</span></span> |<span data-ttu-id="91d58-159">Přidá do X minut Y.</span><span class="sxs-lookup"><span data-stu-id="91d58-159">Adds Y minutes to X.</span></span><br/><br/><span data-ttu-id="91d58-160">Příklad:`9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM`</span><span class="sxs-lookup"><span data-stu-id="91d58-160">Example: `9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM`</span></span> |
| <span data-ttu-id="91d58-161">Čas</span><span class="sxs-lookup"><span data-stu-id="91d58-161">Time</span></span> |<span data-ttu-id="91d58-162">StartOfHour(X)</span><span class="sxs-lookup"><span data-stu-id="91d58-162">StartOfHour(X)</span></span> |<span data-ttu-id="91d58-163">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-163">X: Datetime</span></span> |<span data-ttu-id="91d58-164">Získá počáteční čas za hodinu reprezentována komponentu hodin hodnoty X.</span><span class="sxs-lookup"><span data-stu-id="91d58-164">Gets the starting time for the hour represented by the hour component of X.</span></span> <br/><br/><span data-ttu-id="91d58-165">Příklad:`StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="91d58-165">Example: `StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM`</span></span> |
| <span data-ttu-id="91d58-166">Datum</span><span class="sxs-lookup"><span data-stu-id="91d58-166">Date</span></span> |<span data-ttu-id="91d58-167">AddDays(X,Y)</span><span class="sxs-lookup"><span data-stu-id="91d58-167">AddDays(X,Y)</span></span> |<span data-ttu-id="91d58-168">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-168">X: DateTime</span></span><br/><br/><span data-ttu-id="91d58-169">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="91d58-169">Y: int</span></span> |<span data-ttu-id="91d58-170">Přidá Y dní X.</span><span class="sxs-lookup"><span data-stu-id="91d58-170">Adds Y days to X.</span></span> <br/><br/><span data-ttu-id="91d58-171">Příklad: 9/15/2013 12:00:00 PM + 2 dny = 9/17/2013 12:00:00 PM.</span><span class="sxs-lookup"><span data-stu-id="91d58-171">Example: 9/15/2013 12:00:00 PM + 2 days = 9/17/2013 12:00:00 PM.</span></span><br/><br/><span data-ttu-id="91d58-172">Počet dnů můžete odečtena příliš zadáním Y jako záporné číslo.</span><span class="sxs-lookup"><span data-stu-id="91d58-172">You can subtract days too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="91d58-173">Příklad: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="91d58-173">Example: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`.</span></span> |
| <span data-ttu-id="91d58-174">Datum</span><span class="sxs-lookup"><span data-stu-id="91d58-174">Date</span></span> |<span data-ttu-id="91d58-175">AddMonths(X,Y)</span><span class="sxs-lookup"><span data-stu-id="91d58-175">AddMonths(X,Y)</span></span> |<span data-ttu-id="91d58-176">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-176">X: DateTime</span></span><br/><br/><span data-ttu-id="91d58-177">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="91d58-177">Y: int</span></span> |<span data-ttu-id="91d58-178">Přidá Y měsíců X.</span><span class="sxs-lookup"><span data-stu-id="91d58-178">Adds Y months to X.</span></span><br/><br/><span data-ttu-id="91d58-179">`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="91d58-179">`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.</span></span><br/><br/><span data-ttu-id="91d58-180">Je příliš odečíst měsíců zadáním Y jako záporné číslo.</span><span class="sxs-lookup"><span data-stu-id="91d58-180">You can subtract months too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="91d58-181">Příklad: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="91d58-181">Example: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.</span></span>|
| <span data-ttu-id="91d58-182">Datum</span><span class="sxs-lookup"><span data-stu-id="91d58-182">Date</span></span> |<span data-ttu-id="91d58-183">AddQuarters(X,Y)</span><span class="sxs-lookup"><span data-stu-id="91d58-183">AddQuarters(X,Y)</span></span> |<span data-ttu-id="91d58-184">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-184">X: DateTime</span></span> <br/><br/><span data-ttu-id="91d58-185">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="91d58-185">Y: int</span></span> |<span data-ttu-id="91d58-186">Přidá Y * 3 měsíců X.</span><span class="sxs-lookup"><span data-stu-id="91d58-186">Adds Y * 3 months to X.</span></span><br/><br/><span data-ttu-id="91d58-187">Příklad:`9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="91d58-187">Example: `9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM`</span></span> |
| <span data-ttu-id="91d58-188">Datum</span><span class="sxs-lookup"><span data-stu-id="91d58-188">Date</span></span> |<span data-ttu-id="91d58-189">AddWeeks(X,Y)</span><span class="sxs-lookup"><span data-stu-id="91d58-189">AddWeeks(X,Y)</span></span> |<span data-ttu-id="91d58-190">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-190">X: DateTime</span></span><br/><br/><span data-ttu-id="91d58-191">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="91d58-191">Y: int</span></span> |<span data-ttu-id="91d58-192">Přidá Y * 7 dnů, X</span><span class="sxs-lookup"><span data-stu-id="91d58-192">Adds Y * 7 days to X</span></span><br/><br/><span data-ttu-id="91d58-193">Příklad: 9/15/2013 12:00:00 PM + 1 týden = 9/22/2013 12:00:00 PM</span><span class="sxs-lookup"><span data-stu-id="91d58-193">Example: 9/15/2013 12:00:00 PM + 1 week = 9/22/2013 12:00:00 PM</span></span><br/><br/><span data-ttu-id="91d58-194">Týdny lze odečíst příliš zadáním Y jako záporné číslo.</span><span class="sxs-lookup"><span data-stu-id="91d58-194">You can subtract weeks too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="91d58-195">Příklad: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="91d58-195">Example: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`.</span></span> |
| <span data-ttu-id="91d58-196">Datum</span><span class="sxs-lookup"><span data-stu-id="91d58-196">Date</span></span> |<span data-ttu-id="91d58-197">AddYears(X,Y)</span><span class="sxs-lookup"><span data-stu-id="91d58-197">AddYears(X,Y)</span></span> |<span data-ttu-id="91d58-198">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-198">X: DateTime</span></span><br/><br/><span data-ttu-id="91d58-199">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="91d58-199">Y: int</span></span> |<span data-ttu-id="91d58-200">Přidá Y let X.</span><span class="sxs-lookup"><span data-stu-id="91d58-200">Adds Y years to X.</span></span><br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 year = 9/15/2014 12:00:00 PM`<br/><br/><span data-ttu-id="91d58-201">Je příliš odečíst let zadáním Y jako záporné číslo.</span><span class="sxs-lookup"><span data-stu-id="91d58-201">You can subtract years too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="91d58-202">Příklad: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="91d58-202">Example: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`.</span></span> |
| <span data-ttu-id="91d58-203">Datum</span><span class="sxs-lookup"><span data-stu-id="91d58-203">Date</span></span> |<span data-ttu-id="91d58-204">Day(X)</span><span class="sxs-lookup"><span data-stu-id="91d58-204">Day(X)</span></span> |<span data-ttu-id="91d58-205">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-205">X: DateTime</span></span> |<span data-ttu-id="91d58-206">Získá komponentu den hodnoty X.</span><span class="sxs-lookup"><span data-stu-id="91d58-206">Gets the day component of X.</span></span><br/><br/><span data-ttu-id="91d58-207">Příklad: `Day of 9/15/2013 12:00:00 PM is 9`.</span><span class="sxs-lookup"><span data-stu-id="91d58-207">Example: `Day of 9/15/2013 12:00:00 PM is 9`.</span></span> |
| <span data-ttu-id="91d58-208">Datum</span><span class="sxs-lookup"><span data-stu-id="91d58-208">Date</span></span> |<span data-ttu-id="91d58-209">DayOfWeek(X)</span><span class="sxs-lookup"><span data-stu-id="91d58-209">DayOfWeek(X)</span></span> |<span data-ttu-id="91d58-210">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-210">X: DateTime</span></span> |<span data-ttu-id="91d58-211">Získá den týdne součást X.</span><span class="sxs-lookup"><span data-stu-id="91d58-211">Gets the day of week component of X.</span></span><br/><br/><span data-ttu-id="91d58-212">Příklad: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`.</span><span class="sxs-lookup"><span data-stu-id="91d58-212">Example: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`.</span></span> |
| <span data-ttu-id="91d58-213">Datum</span><span class="sxs-lookup"><span data-stu-id="91d58-213">Date</span></span> |<span data-ttu-id="91d58-214">DayOfYear(X)</span><span class="sxs-lookup"><span data-stu-id="91d58-214">DayOfYear(X)</span></span> |<span data-ttu-id="91d58-215">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-215">X: DateTime</span></span> |<span data-ttu-id="91d58-216">Získá den v roce reprezentována komponentu roku X.</span><span class="sxs-lookup"><span data-stu-id="91d58-216">Gets the day in the year represented by the year component of X.</span></span><br/><br/><span data-ttu-id="91d58-217">Příklady:</span><span class="sxs-lookup"><span data-stu-id="91d58-217">Examples:</span></span><br/>`12/1/2015: day 335 of 2015`<br/>`12/31/2015: day 365 of 2015`<br/>`12/31/2016: day 366 of 2016 (Leap Year)` |
| <span data-ttu-id="91d58-218">Datum</span><span class="sxs-lookup"><span data-stu-id="91d58-218">Date</span></span> |<span data-ttu-id="91d58-219">DaysInMonth(X)</span><span class="sxs-lookup"><span data-stu-id="91d58-219">DaysInMonth(X)</span></span> |<span data-ttu-id="91d58-220">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-220">X: DateTime</span></span> |<span data-ttu-id="91d58-221">Získá dní v měsíci reprezentována komponentu měsíce parametr X.</span><span class="sxs-lookup"><span data-stu-id="91d58-221">Gets the days in the month represented by the month component of parameter X.</span></span><br/><br/><span data-ttu-id="91d58-222">Příklad: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in the September month`.</span><span class="sxs-lookup"><span data-stu-id="91d58-222">Example: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in the September month`.</span></span> |
| <span data-ttu-id="91d58-223">Datum</span><span class="sxs-lookup"><span data-stu-id="91d58-223">Date</span></span> |<span data-ttu-id="91d58-224">EndOfDay(X)</span><span class="sxs-lookup"><span data-stu-id="91d58-224">EndOfDay(X)</span></span> |<span data-ttu-id="91d58-225">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-225">X: DateTime</span></span> |<span data-ttu-id="91d58-226">Získá datum a čas, který představuje konec dne (komponenta dne) X.</span><span class="sxs-lookup"><span data-stu-id="91d58-226">Gets the date-time that represents the end of the day (day component) of X.</span></span><br/><br/><span data-ttu-id="91d58-227">Příklad: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`.</span><span class="sxs-lookup"><span data-stu-id="91d58-227">Example: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`.</span></span> |
| <span data-ttu-id="91d58-228">Datum</span><span class="sxs-lookup"><span data-stu-id="91d58-228">Date</span></span> |<span data-ttu-id="91d58-229">EndOfMonth(X)</span><span class="sxs-lookup"><span data-stu-id="91d58-229">EndOfMonth(X)</span></span> |<span data-ttu-id="91d58-230">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-230">X: DateTime</span></span> |<span data-ttu-id="91d58-231">Získá Konec měsíce reprezentována měsíc součást parametr X.</span><span class="sxs-lookup"><span data-stu-id="91d58-231">Gets the end of the month represented by month component of parameter X.</span></span> <br/><br/><span data-ttu-id="91d58-232">Příklad: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (datum a čas, který představuje konec dne měsíce)</span><span class="sxs-lookup"><span data-stu-id="91d58-232">Example: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (date time that represents the end of September month)</span></span> |
| <span data-ttu-id="91d58-233">Datum</span><span class="sxs-lookup"><span data-stu-id="91d58-233">Date</span></span> |<span data-ttu-id="91d58-234">StartOfDay(X)</span><span class="sxs-lookup"><span data-stu-id="91d58-234">StartOfDay(X)</span></span> |<span data-ttu-id="91d58-235">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-235">X: DateTime</span></span> |<span data-ttu-id="91d58-236">Získá začátek dne reprezentována komponentu den parametru X.</span><span class="sxs-lookup"><span data-stu-id="91d58-236">Gets the start of the day represented by the day component of parameter X.</span></span><br/><br/><span data-ttu-id="91d58-237">Příklad: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`.</span><span class="sxs-lookup"><span data-stu-id="91d58-237">Example: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`.</span></span> |
| <span data-ttu-id="91d58-238">Data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-238">DateTime</span></span> |<span data-ttu-id="91d58-239">From(X)</span><span class="sxs-lookup"><span data-stu-id="91d58-239">From(X)</span></span> |<span data-ttu-id="91d58-240">X: řetězec</span><span class="sxs-lookup"><span data-stu-id="91d58-240">X: String</span></span> |<span data-ttu-id="91d58-241">Analyzovat řetězec X datum a čas.</span><span class="sxs-lookup"><span data-stu-id="91d58-241">Parse string X to a date time.</span></span> |
| <span data-ttu-id="91d58-242">Data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-242">DateTime</span></span> |<span data-ttu-id="91d58-243">Ticks(X)</span><span class="sxs-lookup"><span data-stu-id="91d58-243">Ticks(X)</span></span> |<span data-ttu-id="91d58-244">X: data a času</span><span class="sxs-lookup"><span data-stu-id="91d58-244">X: DateTime</span></span> |<span data-ttu-id="91d58-245">Získá o rysky vlastnost parametru X. Jeden značek rovná 100 nanosekundách.</span><span class="sxs-lookup"><span data-stu-id="91d58-245">Gets the ticks property of the parameter X. One tick equals 100 nanoseconds.</span></span> <span data-ttu-id="91d58-246">Hodnota této vlastnosti představuje počet značek, které uplynuly od 12:00:00, 1. ledna 0001.</span><span class="sxs-lookup"><span data-stu-id="91d58-246">The value of this property represents the number of ticks that have elapsed since 12:00:00 midnight, January 1, 0001.</span></span> |
| <span data-ttu-id="91d58-247">Text</span><span class="sxs-lookup"><span data-stu-id="91d58-247">Text</span></span> |<span data-ttu-id="91d58-248">Format(X)</span><span class="sxs-lookup"><span data-stu-id="91d58-248">Format(X)</span></span> |<span data-ttu-id="91d58-249">X: proměnnou string</span><span class="sxs-lookup"><span data-stu-id="91d58-249">X: String variable</span></span> |<span data-ttu-id="91d58-250">Formáty text (použijte `\\'` kombinaci, abyste se vyhnuli `'` znaků).</span><span class="sxs-lookup"><span data-stu-id="91d58-250">Formats the text (use `\\'` combination to escape `'` character).</span></span>|

> [!IMPORTANT]
> <span data-ttu-id="91d58-251">Při práci s funkcí v jiné funkci, není potřeba použít  **$$**  předponu pro vnitřní funkce.</span><span class="sxs-lookup"><span data-stu-id="91d58-251">When using a function within another function, you do not need to use **$$** prefix for the inner function.</span></span> <span data-ttu-id="91d58-252">Například: $$Text.Format ('PartitionKey eq \\' my_pkey_filter_value\\' a RowKey ge \\' {0: rrrr MM-dd hh: mm:}\\", Time.AddHours (SliceStart, -6)).</span><span class="sxs-lookup"><span data-stu-id="91d58-252">For example: $$Text.Format('PartitionKey eq \\'my_pkey_filter_value\\' and RowKey ge \\'{0: yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(SliceStart, -6)).</span></span> <span data-ttu-id="91d58-253">V tomto příkladu, Všimněte si, že  **$$**  předponu nepoužívá pro **Time.AddHours** funkce.</span><span class="sxs-lookup"><span data-stu-id="91d58-253">In this example, notice that **$$** prefix is not used for the **Time.AddHours** function.</span></span> 

#### <a name="example"></a><span data-ttu-id="91d58-254">Příklad</span><span class="sxs-lookup"><span data-stu-id="91d58-254">Example</span></span>
<span data-ttu-id="91d58-255">V následujícím příkladu jsou vstupní a výstupní parametry pro aktivitu Hive určit pomocí `Text.Format` funkce a proměnné SliceStart systému.</span><span class="sxs-lookup"><span data-stu-id="91d58-255">In the following example, input and output parameters for the Hive activity are determined by using the `Text.Format` function and SliceStart system variable.</span></span> 

```json  
{
    "name": "HiveActivitySamplePipeline",
        "properties": {
    "activities": [
            {
            "name": "HiveActivitySample",
            "type": "HDInsightHive",
            "inputs": [
                    {
                    "name": "HiveSampleIn"
                    }
            ],
            "outputs": [
                    {
                    "name": "HiveSampleOut"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                }
            }
            }
    ]
    }
}
```

### <a name="example-2"></a><span data-ttu-id="91d58-256">Příklad 2</span><span class="sxs-lookup"><span data-stu-id="91d58-256">Example 2</span></span>

<span data-ttu-id="91d58-257">V následujícím příkladu je určen parametr data a času pro aktivity uložené procedury pomocí textu.</span><span class="sxs-lookup"><span data-stu-id="91d58-257">In the following example, the DateTime parameter for the Stored Procedure Activity is determined by using the Text.</span></span> <span data-ttu-id="91d58-258">Formát funkce a proměnné SliceStart.</span><span class="sxs-lookup"><span data-stu-id="91d58-258">Format function and the SliceStart variable.</span></span> 

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
            "start": "2016-08-02T00:00:00Z",
            "end": "2016-08-02T05:00:00Z",
        "isPaused": false
    }
}
```

### <a name="example-3"></a><span data-ttu-id="91d58-259">Příklad 3</span><span class="sxs-lookup"><span data-stu-id="91d58-259">Example 3</span></span>
<span data-ttu-id="91d58-260">Číst data z předchozí den místo den reprezentována vlastnosti SliceStart, pomocí funkce přidat dny, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="91d58-260">To read data from previous day instead of day represented by the SliceStart, use the AddDays function as shown in the following example:</span></span> 

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-01-01T08:00:00",
        "end": "2017-01-01T11:00:00",
        "description": "hive activity",
        "activities": [
            {
                "name": "SampleHiveActivity",
                "inputs": [
                    {
                        "name": "MyAzureBlobInput",
                        "startTime": "Date.AddDays(SliceStart, -1)",
                        "endTime": "Date.AddDays(SliceEnd, -1)"
                    }
                ],
                "outputs": [
                    {
                        "name": "MyAzureBlobOutput"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adftutorial\\hivequery.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Year": "$$Text.Format('{0:yyyy}',WindowsStart)",
                        "Month": "$$Text.Format('{0:MM}',WindowStart)",
                        "Day": "$$Text.Format('{0:dd}',WindowStart)"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 2,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

<span data-ttu-id="91d58-261">V tématu [vlastní řetězců data a času formát](https://msdn.microsoft.com/library/8kb3ddd4.aspx) téma, které popisuje různé možnosti formátování můžete použít (Příklad: RR oproti rrrr).</span><span class="sxs-lookup"><span data-stu-id="91d58-261">See [Custom Date and Time Format Strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx) topic that describes different formatting options you can use (for example: yy vs. yyyy).</span></span> 

