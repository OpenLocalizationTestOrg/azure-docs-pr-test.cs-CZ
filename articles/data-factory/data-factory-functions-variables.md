---
title: "aaaData funkce objektu pro vytváření a systémové proměnné | Microsoft Docs"
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
ms.openlocfilehash: d2936c2821797947bb37d9775226a6c19c4b8ab9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---functions-and-system-variables"></a><span data-ttu-id="89948-103">Azure Data Factory – funkce a systémové proměnné</span><span class="sxs-lookup"><span data-stu-id="89948-103">Azure Data Factory - Functions and System Variables</span></span>
<span data-ttu-id="89948-104">Tento článek obsahuje informace o funkcích a proměnné, podporované službou Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="89948-104">This article provides information about functions and variables supported by Azure Data Factory.</span></span>

## <a name="data-factory-system-variables"></a><span data-ttu-id="89948-105">Data Factory systémové proměnné</span><span class="sxs-lookup"><span data-stu-id="89948-105">Data Factory system variables</span></span>
| <span data-ttu-id="89948-106">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="89948-106">Variable Name</span></span> | <span data-ttu-id="89948-107">Popis</span><span class="sxs-lookup"><span data-stu-id="89948-107">Description</span></span> | <span data-ttu-id="89948-108">Objekt oboru</span><span class="sxs-lookup"><span data-stu-id="89948-108">Object Scope</span></span> | <span data-ttu-id="89948-109">Obor JSON a případy použití</span><span class="sxs-lookup"><span data-stu-id="89948-109">JSON Scope and Use Cases</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="89948-110">WindowStart</span><span class="sxs-lookup"><span data-stu-id="89948-110">WindowStart</span></span> |<span data-ttu-id="89948-111">Začátek časového intervalu pro aktuální aktivity při spuštění okna</span><span class="sxs-lookup"><span data-stu-id="89948-111">Start of time interval for current activity run window</span></span> |<span data-ttu-id="89948-112">Aktivity</span><span class="sxs-lookup"><span data-stu-id="89948-112">activity</span></span> |<ol><li><span data-ttu-id="89948-113">Zadejte data výběr dotazy.</span><span class="sxs-lookup"><span data-stu-id="89948-113">Specify data selection queries.</span></span> <span data-ttu-id="89948-114">Najdete v části konektor články v hello odkazuje [aktivity přesunu dat](data-factory-data-movement-activities.md) článku.</span><span class="sxs-lookup"><span data-stu-id="89948-114">See connector articles referenced in hello [Data Movement Activities](data-factory-data-movement-activities.md) article.</span></span></li> |
| <span data-ttu-id="89948-115">WindowEnd</span><span class="sxs-lookup"><span data-stu-id="89948-115">WindowEnd</span></span> |<span data-ttu-id="89948-116">Konec časového intervalu pro aktuální aktivity při spuštění okna</span><span class="sxs-lookup"><span data-stu-id="89948-116">End of time interval for current activity run window</span></span> |<span data-ttu-id="89948-117">Aktivity</span><span class="sxs-lookup"><span data-stu-id="89948-117">activity</span></span> |<span data-ttu-id="89948-118">stejné jako WindowStart.</span><span class="sxs-lookup"><span data-stu-id="89948-118">same as WindowStart.</span></span> |
| <span data-ttu-id="89948-119">SliceStart</span><span class="sxs-lookup"><span data-stu-id="89948-119">SliceStart</span></span> |<span data-ttu-id="89948-120">Začátek časového intervalu pro datový řez vytvářen</span><span class="sxs-lookup"><span data-stu-id="89948-120">Start of time interval for data  slice being produced</span></span> |<span data-ttu-id="89948-121">Aktivity</span><span class="sxs-lookup"><span data-stu-id="89948-121">activity</span></span><br/><span data-ttu-id="89948-122">Datové sady</span><span class="sxs-lookup"><span data-stu-id="89948-122">dataset</span></span> |<ol><li><span data-ttu-id="89948-123">Zadejte složku dynamické cesty a názvy souborů při práci s [objektů Blob v Azure](data-factory-azure-blob-connector.md) a [datové sady systém souborů](data-factory-onprem-file-system-connector.md).</span><span class="sxs-lookup"><span data-stu-id="89948-123">Specify dynamic folder paths and file names while working with [Azure Blob](data-factory-azure-blob-connector.md) and [File System datasets](data-factory-onprem-file-system-connector.md).</span></span></li><li><span data-ttu-id="89948-124">Určení závislostí vstupní pomocí funkce objekt pro vytváření dat v kolekci vstupy aktivity.</span><span class="sxs-lookup"><span data-stu-id="89948-124">Specify input dependencies with data factory functions in activity inputs collection.</span></span></li></ol> |
| <span data-ttu-id="89948-125">SliceEnd</span><span class="sxs-lookup"><span data-stu-id="89948-125">SliceEnd</span></span> |<span data-ttu-id="89948-126">Konec časového intervalu pro aktuální datový řez.</span><span class="sxs-lookup"><span data-stu-id="89948-126">End of time interval for current data slice.</span></span> |<span data-ttu-id="89948-127">Aktivity</span><span class="sxs-lookup"><span data-stu-id="89948-127">activity</span></span><br/><span data-ttu-id="89948-128">Datové sady</span><span class="sxs-lookup"><span data-stu-id="89948-128">dataset</span></span> |<span data-ttu-id="89948-129">stejné jako SliceStart.</span><span class="sxs-lookup"><span data-stu-id="89948-129">same as SliceStart.</span></span> |

> [!NOTE]
> <span data-ttu-id="89948-130">Aktuálně objekt pro vytváření dat vyžaduje, že hello naplánovat uvedené v hello aktivity přesně odpovídá hello plán zadaný v dostupnost hello výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="89948-130">Currently data factory requires that hello schedule specified in hello activity exactly matches hello schedule specified in availability of hello output dataset.</span></span> <span data-ttu-id="89948-131">Proto WindowStart, WindowEnd a SliceStart a SliceEnd vždy mapování toohello stejné časové období a jednu výstupní řez.</span><span class="sxs-lookup"><span data-stu-id="89948-131">Therefore, WindowStart, WindowEnd, and SliceStart and SliceEnd always map toohello same time period and a single output slice.</span></span>
> 

### <a name="example-for-using-a-system-variable"></a><span data-ttu-id="89948-132">Příklad použití systémové proměnné</span><span class="sxs-lookup"><span data-stu-id="89948-132">Example for using a system variable</span></span>
<span data-ttu-id="89948-133">V následujícím příkladu, rok, měsíc, den a čas hello **SliceStart** extrahují do samostatné proměnné, které jsou používány **folderPath** a **fileName** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="89948-133">In hello following example, year, month, day, and time of **SliceStart** are extracted into separate variables that are used by **folderPath** and **fileName** properties.</span></span>

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

## <a name="data-factory-functions"></a><span data-ttu-id="89948-134">Funkce pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="89948-134">Data Factory functions</span></span>
<span data-ttu-id="89948-135">Můžete použít funkce v datové továrně společně s systémové proměnné pro hello následující účely:</span><span class="sxs-lookup"><span data-stu-id="89948-135">You can use functions in data factory along with system variables for hello following purposes:</span></span>

1. <span data-ttu-id="89948-136">Určení dotazy výběru dat (najdete v článcích konektor odkazuje hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku.</span><span class="sxs-lookup"><span data-stu-id="89948-136">Specifying data selection queries (see connector articles referenced by hello [Data Movement Activities](data-factory-data-movement-activities.md) article.</span></span>
   
   <span data-ttu-id="89948-137">Hello tooinvoke syntaxe je funkci pro vytváření dat:  **$$ <function>**  pro dotazy výběru dat a dalších vlastností hello aktivity a datové sady.</span><span class="sxs-lookup"><span data-stu-id="89948-137">hello syntax tooinvoke a data factory function is: **$$<function>** for data selection queries and other properties in hello activity and datasets.</span></span>  
2. <span data-ttu-id="89948-138">Určení vstupní závislosti pomocí funkce objekt pro vytváření dat v kolekci vstupy aktivity.</span><span class="sxs-lookup"><span data-stu-id="89948-138">Specifying input dependencies with data factory functions in activity inputs collection.</span></span>
   
    <span data-ttu-id="89948-139">$$ není potřeba pro zadání vstupní závislost výrazy.</span><span class="sxs-lookup"><span data-stu-id="89948-139">$$ is not needed for specifying input dependency expressions.</span></span>     

<span data-ttu-id="89948-140">V následující ukázce hello **sqlReaderQuery** vlastnost v souboru JSON je přiřazena hodnota tooa vrácený hello `Text.Format` funkce.</span><span class="sxs-lookup"><span data-stu-id="89948-140">In hello following sample, **sqlReaderQuery** property in a JSON file is assigned tooa value returned by hello `Text.Format` function.</span></span> <span data-ttu-id="89948-141">Tato ukázka také používá systém proměnné s názvem **WindowStart**, která reprezentuje čas zahájení hello hello aktivity při spuštění okna.</span><span class="sxs-lookup"><span data-stu-id="89948-141">This sample also uses a system variable named **WindowStart**, which represents hello start time of hello activity run window.</span></span>

```json
{
    "Type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
}
```

<span data-ttu-id="89948-142">V tématu [vlastní řetězců data a času formát](https://msdn.microsoft.com/library/8kb3ddd4.aspx) téma, které popisuje různé možnosti formátování můžete použít (Příklad: zobrazit oproti rrrr).</span><span class="sxs-lookup"><span data-stu-id="89948-142">See [Custom Date and Time Format Strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx) topic that describes different formatting options you can use (for example: ay vs. yyyy).</span></span> 

### <a name="functions"></a><span data-ttu-id="89948-143">Funkce</span><span class="sxs-lookup"><span data-stu-id="89948-143">Functions</span></span>
<span data-ttu-id="89948-144">Následující tabulky Hello seznam všech funkcí hello v Azure Data Factory:</span><span class="sxs-lookup"><span data-stu-id="89948-144">hello following tables list all hello functions in Azure Data Factory:</span></span>

| <span data-ttu-id="89948-145">Kategorie</span><span class="sxs-lookup"><span data-stu-id="89948-145">Category</span></span> | <span data-ttu-id="89948-146">Funkce</span><span class="sxs-lookup"><span data-stu-id="89948-146">Function</span></span> | <span data-ttu-id="89948-147">Parametry</span><span class="sxs-lookup"><span data-stu-id="89948-147">Parameters</span></span> | <span data-ttu-id="89948-148">Popis</span><span class="sxs-lookup"><span data-stu-id="89948-148">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="89948-149">Čas</span><span class="sxs-lookup"><span data-stu-id="89948-149">Time</span></span> |<span data-ttu-id="89948-150">AddHours(X,Y)</span><span class="sxs-lookup"><span data-stu-id="89948-150">AddHours(X,Y)</span></span> |<span data-ttu-id="89948-151">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-151">X: DateTime</span></span> <br/><br/><span data-ttu-id="89948-152">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="89948-152">Y: int</span></span> |<span data-ttu-id="89948-153">Přidá Y hodin toohello zadaný čas X.</span><span class="sxs-lookup"><span data-stu-id="89948-153">Adds Y hours toohello given time X.</span></span> <br/><br/><span data-ttu-id="89948-154">Příklad:`9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="89948-154">Example: `9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM`</span></span> |
| <span data-ttu-id="89948-155">Čas</span><span class="sxs-lookup"><span data-stu-id="89948-155">Time</span></span> |<span data-ttu-id="89948-156">AddMinutes(X,Y)</span><span class="sxs-lookup"><span data-stu-id="89948-156">AddMinutes(X,Y)</span></span> |<span data-ttu-id="89948-157">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-157">X: DateTime</span></span> <br/><br/><span data-ttu-id="89948-158">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="89948-158">Y: int</span></span> |<span data-ttu-id="89948-159">Přidá tooX minut Y.</span><span class="sxs-lookup"><span data-stu-id="89948-159">Adds Y minutes tooX.</span></span><br/><br/><span data-ttu-id="89948-160">Příklad:`9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM`</span><span class="sxs-lookup"><span data-stu-id="89948-160">Example: `9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM`</span></span> |
| <span data-ttu-id="89948-161">Čas</span><span class="sxs-lookup"><span data-stu-id="89948-161">Time</span></span> |<span data-ttu-id="89948-162">StartOfHour(X)</span><span class="sxs-lookup"><span data-stu-id="89948-162">StartOfHour(X)</span></span> |<span data-ttu-id="89948-163">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-163">X: Datetime</span></span> |<span data-ttu-id="89948-164">Získá hello počáteční čas pro hello hodinu reprezentována hello hodinu součást X.</span><span class="sxs-lookup"><span data-stu-id="89948-164">Gets hello starting time for hello hour represented by hello hour component of X.</span></span> <br/><br/><span data-ttu-id="89948-165">Příklad:`StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="89948-165">Example: `StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM`</span></span> |
| <span data-ttu-id="89948-166">Datum</span><span class="sxs-lookup"><span data-stu-id="89948-166">Date</span></span> |<span data-ttu-id="89948-167">AddDays(X,Y)</span><span class="sxs-lookup"><span data-stu-id="89948-167">AddDays(X,Y)</span></span> |<span data-ttu-id="89948-168">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-168">X: DateTime</span></span><br/><br/><span data-ttu-id="89948-169">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="89948-169">Y: int</span></span> |<span data-ttu-id="89948-170">Přidá tooX Y dní.</span><span class="sxs-lookup"><span data-stu-id="89948-170">Adds Y days tooX.</span></span> <br/><br/><span data-ttu-id="89948-171">Příklad: 9/15/2013 12:00:00 PM + 2 dny = 9/17/2013 12:00:00 PM.</span><span class="sxs-lookup"><span data-stu-id="89948-171">Example: 9/15/2013 12:00:00 PM + 2 days = 9/17/2013 12:00:00 PM.</span></span><br/><br/><span data-ttu-id="89948-172">Počet dnů můžete odečtena příliš zadáním Y jako záporné číslo.</span><span class="sxs-lookup"><span data-stu-id="89948-172">You can subtract days too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="89948-173">Příklad: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="89948-173">Example: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`.</span></span> |
| <span data-ttu-id="89948-174">Datum</span><span class="sxs-lookup"><span data-stu-id="89948-174">Date</span></span> |<span data-ttu-id="89948-175">AddMonths(X,Y)</span><span class="sxs-lookup"><span data-stu-id="89948-175">AddMonths(X,Y)</span></span> |<span data-ttu-id="89948-176">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-176">X: DateTime</span></span><br/><br/><span data-ttu-id="89948-177">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="89948-177">Y: int</span></span> |<span data-ttu-id="89948-178">Přidá tooX Y měsíců.</span><span class="sxs-lookup"><span data-stu-id="89948-178">Adds Y months tooX.</span></span><br/><br/><span data-ttu-id="89948-179">`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="89948-179">`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.</span></span><br/><br/><span data-ttu-id="89948-180">Je příliš odečíst měsíců zadáním Y jako záporné číslo.</span><span class="sxs-lookup"><span data-stu-id="89948-180">You can subtract months too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="89948-181">Příklad: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="89948-181">Example: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.</span></span>|
| <span data-ttu-id="89948-182">Datum</span><span class="sxs-lookup"><span data-stu-id="89948-182">Date</span></span> |<span data-ttu-id="89948-183">AddQuarters(X,Y)</span><span class="sxs-lookup"><span data-stu-id="89948-183">AddQuarters(X,Y)</span></span> |<span data-ttu-id="89948-184">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-184">X: DateTime</span></span> <br/><br/><span data-ttu-id="89948-185">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="89948-185">Y: int</span></span> |<span data-ttu-id="89948-186">Přidá Y * tooX 3 měsíce.</span><span class="sxs-lookup"><span data-stu-id="89948-186">Adds Y * 3 months tooX.</span></span><br/><br/><span data-ttu-id="89948-187">Příklad:`9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="89948-187">Example: `9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM`</span></span> |
| <span data-ttu-id="89948-188">Datum</span><span class="sxs-lookup"><span data-stu-id="89948-188">Date</span></span> |<span data-ttu-id="89948-189">AddWeeks(X,Y)</span><span class="sxs-lookup"><span data-stu-id="89948-189">AddWeeks(X,Y)</span></span> |<span data-ttu-id="89948-190">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-190">X: DateTime</span></span><br/><br/><span data-ttu-id="89948-191">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="89948-191">Y: int</span></span> |<span data-ttu-id="89948-192">Přidá Y * tooX 7 dnů</span><span class="sxs-lookup"><span data-stu-id="89948-192">Adds Y * 7 days tooX</span></span><br/><br/><span data-ttu-id="89948-193">Příklad: 9/15/2013 12:00:00 PM + 1 týden = 9/22/2013 12:00:00 PM</span><span class="sxs-lookup"><span data-stu-id="89948-193">Example: 9/15/2013 12:00:00 PM + 1 week = 9/22/2013 12:00:00 PM</span></span><br/><br/><span data-ttu-id="89948-194">Týdny lze odečíst příliš zadáním Y jako záporné číslo.</span><span class="sxs-lookup"><span data-stu-id="89948-194">You can subtract weeks too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="89948-195">Příklad: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="89948-195">Example: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`.</span></span> |
| <span data-ttu-id="89948-196">Datum</span><span class="sxs-lookup"><span data-stu-id="89948-196">Date</span></span> |<span data-ttu-id="89948-197">AddYears(X,Y)</span><span class="sxs-lookup"><span data-stu-id="89948-197">AddYears(X,Y)</span></span> |<span data-ttu-id="89948-198">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-198">X: DateTime</span></span><br/><br/><span data-ttu-id="89948-199">/ Y: int</span><span class="sxs-lookup"><span data-stu-id="89948-199">Y: int</span></span> |<span data-ttu-id="89948-200">Přidá tooX let Y.</span><span class="sxs-lookup"><span data-stu-id="89948-200">Adds Y years tooX.</span></span><br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 year = 9/15/2014 12:00:00 PM`<br/><br/><span data-ttu-id="89948-201">Je příliš odečíst let zadáním Y jako záporné číslo.</span><span class="sxs-lookup"><span data-stu-id="89948-201">You can subtract years too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="89948-202">Příklad: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="89948-202">Example: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`.</span></span> |
| <span data-ttu-id="89948-203">Datum</span><span class="sxs-lookup"><span data-stu-id="89948-203">Date</span></span> |<span data-ttu-id="89948-204">Day(X)</span><span class="sxs-lookup"><span data-stu-id="89948-204">Day(X)</span></span> |<span data-ttu-id="89948-205">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-205">X: DateTime</span></span> |<span data-ttu-id="89948-206">Získá hello den součást X.</span><span class="sxs-lookup"><span data-stu-id="89948-206">Gets hello day component of X.</span></span><br/><br/><span data-ttu-id="89948-207">Příklad: `Day of 9/15/2013 12:00:00 PM is 9`.</span><span class="sxs-lookup"><span data-stu-id="89948-207">Example: `Day of 9/15/2013 12:00:00 PM is 9`.</span></span> |
| <span data-ttu-id="89948-208">Datum</span><span class="sxs-lookup"><span data-stu-id="89948-208">Date</span></span> |<span data-ttu-id="89948-209">DayOfWeek(X)</span><span class="sxs-lookup"><span data-stu-id="89948-209">DayOfWeek(X)</span></span> |<span data-ttu-id="89948-210">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-210">X: DateTime</span></span> |<span data-ttu-id="89948-211">Získá hello den týdne součást X.</span><span class="sxs-lookup"><span data-stu-id="89948-211">Gets hello day of week component of X.</span></span><br/><br/><span data-ttu-id="89948-212">Příklad: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`.</span><span class="sxs-lookup"><span data-stu-id="89948-212">Example: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`.</span></span> |
| <span data-ttu-id="89948-213">Datum</span><span class="sxs-lookup"><span data-stu-id="89948-213">Date</span></span> |<span data-ttu-id="89948-214">DayOfYear(X)</span><span class="sxs-lookup"><span data-stu-id="89948-214">DayOfYear(X)</span></span> |<span data-ttu-id="89948-215">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-215">X: DateTime</span></span> |<span data-ttu-id="89948-216">Získá hello den v roce hello reprezentována hello roku součást X.</span><span class="sxs-lookup"><span data-stu-id="89948-216">Gets hello day in hello year represented by hello year component of X.</span></span><br/><br/><span data-ttu-id="89948-217">Příklady:</span><span class="sxs-lookup"><span data-stu-id="89948-217">Examples:</span></span><br/>`12/1/2015: day 335 of 2015`<br/>`12/31/2015: day 365 of 2015`<br/>`12/31/2016: day 366 of 2016 (Leap Year)` |
| <span data-ttu-id="89948-218">Datum</span><span class="sxs-lookup"><span data-stu-id="89948-218">Date</span></span> |<span data-ttu-id="89948-219">DaysInMonth(X)</span><span class="sxs-lookup"><span data-stu-id="89948-219">DaysInMonth(X)</span></span> |<span data-ttu-id="89948-220">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-220">X: DateTime</span></span> |<span data-ttu-id="89948-221">Získá hello dny v měsíci hello reprezentované měsíc komponentou hello parametru X.</span><span class="sxs-lookup"><span data-stu-id="89948-221">Gets hello days in hello month represented by hello month component of parameter X.</span></span><br/><br/><span data-ttu-id="89948-222">Příklad: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in hello September month`.</span><span class="sxs-lookup"><span data-stu-id="89948-222">Example: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in hello September month`.</span></span> |
| <span data-ttu-id="89948-223">Datum</span><span class="sxs-lookup"><span data-stu-id="89948-223">Date</span></span> |<span data-ttu-id="89948-224">EndOfDay(X)</span><span class="sxs-lookup"><span data-stu-id="89948-224">EndOfDay(X)</span></span> |<span data-ttu-id="89948-225">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-225">X: DateTime</span></span> |<span data-ttu-id="89948-226">Získá hello datum a čas, který představuje konec hello hello den (komponenta dne) X.</span><span class="sxs-lookup"><span data-stu-id="89948-226">Gets hello date-time that represents hello end of hello day (day component) of X.</span></span><br/><br/><span data-ttu-id="89948-227">Příklad: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`.</span><span class="sxs-lookup"><span data-stu-id="89948-227">Example: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`.</span></span> |
| <span data-ttu-id="89948-228">Datum</span><span class="sxs-lookup"><span data-stu-id="89948-228">Date</span></span> |<span data-ttu-id="89948-229">EndOfMonth(X)</span><span class="sxs-lookup"><span data-stu-id="89948-229">EndOfMonth(X)</span></span> |<span data-ttu-id="89948-230">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-230">X: DateTime</span></span> |<span data-ttu-id="89948-231">Získá hello Konec měsíce hello reprezentována měsíc součást parametr X.</span><span class="sxs-lookup"><span data-stu-id="89948-231">Gets hello end of hello month represented by month component of parameter X.</span></span> <br/><br/><span data-ttu-id="89948-232">Příklad: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (datum a čas, který představuje hello Konec dne měsíce)</span><span class="sxs-lookup"><span data-stu-id="89948-232">Example: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (date time that represents hello end of September month)</span></span> |
| <span data-ttu-id="89948-233">Datum</span><span class="sxs-lookup"><span data-stu-id="89948-233">Date</span></span> |<span data-ttu-id="89948-234">StartOfDay(X)</span><span class="sxs-lookup"><span data-stu-id="89948-234">StartOfDay(X)</span></span> |<span data-ttu-id="89948-235">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-235">X: DateTime</span></span> |<span data-ttu-id="89948-236">Získá hello začátek dne hello reprezentována hello den součást parametr X.</span><span class="sxs-lookup"><span data-stu-id="89948-236">Gets hello start of hello day represented by hello day component of parameter X.</span></span><br/><br/><span data-ttu-id="89948-237">Příklad: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`.</span><span class="sxs-lookup"><span data-stu-id="89948-237">Example: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`.</span></span> |
| <span data-ttu-id="89948-238">Data a času</span><span class="sxs-lookup"><span data-stu-id="89948-238">DateTime</span></span> |<span data-ttu-id="89948-239">From(X)</span><span class="sxs-lookup"><span data-stu-id="89948-239">From(X)</span></span> |<span data-ttu-id="89948-240">X: řetězec</span><span class="sxs-lookup"><span data-stu-id="89948-240">X: String</span></span> |<span data-ttu-id="89948-241">Analyzovat řetězec X tooa datum a čas.</span><span class="sxs-lookup"><span data-stu-id="89948-241">Parse string X tooa date time.</span></span> |
| <span data-ttu-id="89948-242">Data a času</span><span class="sxs-lookup"><span data-stu-id="89948-242">DateTime</span></span> |<span data-ttu-id="89948-243">Ticks(X)</span><span class="sxs-lookup"><span data-stu-id="89948-243">Ticks(X)</span></span> |<span data-ttu-id="89948-244">X: data a času</span><span class="sxs-lookup"><span data-stu-id="89948-244">X: DateTime</span></span> |<span data-ttu-id="89948-245">Získá hello rysky vlastnost hello parametru X. Jeden značek rovná 100 nanosekundách.</span><span class="sxs-lookup"><span data-stu-id="89948-245">Gets hello ticks property of hello parameter X. One tick equals 100 nanoseconds.</span></span> <span data-ttu-id="89948-246">Hodnota této vlastnosti Hello představuje hello počet značek, které uplynuly od 12:00:00, 1. ledna 0001.</span><span class="sxs-lookup"><span data-stu-id="89948-246">hello value of this property represents hello number of ticks that have elapsed since 12:00:00 midnight, January 1, 0001.</span></span> |
| <span data-ttu-id="89948-247">Text</span><span class="sxs-lookup"><span data-stu-id="89948-247">Text</span></span> |<span data-ttu-id="89948-248">Format(X)</span><span class="sxs-lookup"><span data-stu-id="89948-248">Format(X)</span></span> |<span data-ttu-id="89948-249">X: proměnnou string</span><span class="sxs-lookup"><span data-stu-id="89948-249">X: String variable</span></span> |<span data-ttu-id="89948-250">Formáty hello text (použijte `\\'` kombinaci tooescape `'` znaků).</span><span class="sxs-lookup"><span data-stu-id="89948-250">Formats hello text (use `\\'` combination tooescape `'` character).</span></span>|

> [!IMPORTANT]
> <span data-ttu-id="89948-251">Při práci s funkcí v jiné funkci, není nutné toouse  **$$**  předponu pro vnitřní funkce hello.</span><span class="sxs-lookup"><span data-stu-id="89948-251">When using a function within another function, you do not need toouse **$$** prefix for hello inner function.</span></span> <span data-ttu-id="89948-252">Například: $$Text.Format ('PartitionKey eq \\' my_pkey_filter_value\\' a RowKey ge \\' {0: rrrr MM-dd hh: mm:}\\", Time.AddHours (SliceStart, -6)).</span><span class="sxs-lookup"><span data-stu-id="89948-252">For example: $$Text.Format('PartitionKey eq \\'my_pkey_filter_value\\' and RowKey ge \\'{0: yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(SliceStart, -6)).</span></span> <span data-ttu-id="89948-253">V tomto příkladu, Všimněte si, že  **$$**  předponu nepoužívá pro hello **Time.AddHours** funkce.</span><span class="sxs-lookup"><span data-stu-id="89948-253">In this example, notice that **$$** prefix is not used for hello **Time.AddHours** function.</span></span> 

#### <a name="example"></a><span data-ttu-id="89948-254">Příklad</span><span class="sxs-lookup"><span data-stu-id="89948-254">Example</span></span>
<span data-ttu-id="89948-255">V hello jsou následující příklad, vstupní a výstupní parametry pro aktivitu Hive hello určit pomocí hello `Text.Format` funkce a proměnné SliceStart systému.</span><span class="sxs-lookup"><span data-stu-id="89948-255">In hello following example, input and output parameters for hello Hive activity are determined by using hello `Text.Format` function and SliceStart system variable.</span></span> 

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

### <a name="example-2"></a><span data-ttu-id="89948-256">Příklad 2</span><span class="sxs-lookup"><span data-stu-id="89948-256">Example 2</span></span>

<span data-ttu-id="89948-257">V následujícím příkladu hello hello parametr data a času pro hello aktivity uložené procedury je určen pomocí hello Text.</span><span class="sxs-lookup"><span data-stu-id="89948-257">In hello following example, hello DateTime parameter for hello Stored Procedure Activity is determined by using hello Text.</span></span> <span data-ttu-id="89948-258">Formátování funkce a proměnné SliceStart hello.</span><span class="sxs-lookup"><span data-stu-id="89948-258">Format function and hello SliceStart variable.</span></span> 

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

### <a name="example-3"></a><span data-ttu-id="89948-259">Příklad 3</span><span class="sxs-lookup"><span data-stu-id="89948-259">Example 3</span></span>
<span data-ttu-id="89948-260">tooread data z předchozí den místo den reprezentována hello SliceStart, pomocí funkce přidat dny hello, jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="89948-260">tooread data from previous day instead of day represented by hello SliceStart, use hello AddDays function as shown in hello following example:</span></span> 

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

<span data-ttu-id="89948-261">V tématu [vlastní řetězců data a času formát](https://msdn.microsoft.com/library/8kb3ddd4.aspx) téma, které popisuje různé možnosti formátování můžete použít (Příklad: RR oproti rrrr).</span><span class="sxs-lookup"><span data-stu-id="89948-261">See [Custom Date and Time Format Strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx) topic that describes different formatting options you can use (for example: yy vs. yyyy).</span></span> 

