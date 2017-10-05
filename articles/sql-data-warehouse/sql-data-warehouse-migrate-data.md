---
title: Migrace dat do SQL Data Warehouse | Microsoft Docs
description: "Tipy pro migraci dat do Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: d78f954a-f54c-4aa4-9040-919bc6414887
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/29/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: dbdf1696cd169aa7e5e23f116027a1170347f4ea
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="migrate-your-data"></a><span data-ttu-id="c95bd-103">Migrace dat</span><span class="sxs-lookup"><span data-stu-id="c95bd-103">Migrate Your Data</span></span>
<span data-ttu-id="c95bd-104">Data lze přesunout z různých zdrojů do SQL Data Warehouse pomocí různých nástrojů.</span><span class="sxs-lookup"><span data-stu-id="c95bd-104">Data can be moved from different sources into your SQL Data Warehouse with a variety tools.</span></span>  <span data-ttu-id="c95bd-105">Kopírování ADF, SSIS a bcp můžete použít k dosažení tohoto cíle.</span><span class="sxs-lookup"><span data-stu-id="c95bd-105">ADF Copy, SSIS, and bcp can all be used to achieve this goal.</span></span> <span data-ttu-id="c95bd-106">Jako velikost dat zvyšuje však měli myslíte o rozdělení proces migrace dat do kroků.</span><span class="sxs-lookup"><span data-stu-id="c95bd-106">However, as the amount of data increases you should think about breaking down the data migration process into steps.</span></span> <span data-ttu-id="c95bd-107">To nabízí možnost optimalizovat každý krok pro výkon i pro odolnost k zajištění smooth dat migrace.</span><span class="sxs-lookup"><span data-stu-id="c95bd-107">This affords you the opportunity to optimize each step both for performance and for resilience to ensure a smooth data migration.</span></span>

<span data-ttu-id="c95bd-108">Tento článek popisuje nejprve jednoduché migrační scénáře ADF Copy, SSIS a bcp.</span><span class="sxs-lookup"><span data-stu-id="c95bd-108">This article first discusses the simple migration scenarios of ADF Copy, SSIS, and bcp.</span></span> <span data-ttu-id="c95bd-109">Následně vypadá trochu hlouběji do jak lze optimalizovat migrace.</span><span class="sxs-lookup"><span data-stu-id="c95bd-109">It then look a little deeper into how the migration can be optimized.</span></span>

## <a name="azure-data-factory-adf-copy"></a><span data-ttu-id="c95bd-110">Azure Data Factory (ADF) kopie</span><span class="sxs-lookup"><span data-stu-id="c95bd-110">Azure Data Factory (ADF) copy</span></span>
<span data-ttu-id="c95bd-111">[Kopírování ADF] [ ADF Copy] je součástí [Azure Data Factory][Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="c95bd-111">[ADF Copy][ADF Copy] is part of [Azure Data Factory][Azure Data Factory].</span></span> <span data-ttu-id="c95bd-112">Kopírování ADF můžete exportovat data do plochých souborů, které se nacházejí v místním úložišti, do vzdáleného plochých souborů uchovávat v Azure blob storage nebo přímo do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c95bd-112">You can use ADF Copy to export your data to flat files residing on local storage, to remote flat files held in Azure blob storage or directly into SQL Data Warehouse.</span></span>

<span data-ttu-id="c95bd-113">Pokud vaše data se spustí v plochých souborů, pak budete nejprve muset před zahájením zatížení ho přenést do objektu blob úložiště Azure do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c95bd-113">If your data starts in flat files, then you will first need to transfer it to Azure storage blob before initiating a load it into SQL Data Warehouse.</span></span> <span data-ttu-id="c95bd-114">Jakmile se přenáší data do úložiště objektů blob v Azure můžete použít [ADF kopie] [ ADF Copy] znovu tak, aby nabízel data do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c95bd-114">Once the data is transferred into Azure blob storage you can choose to use [ADF Copy][ADF Copy] again to push the data into SQL Data Warehouse.</span></span>

<span data-ttu-id="c95bd-115">PolyBase také nabízí možnost vysoce výkonné načítání data.</span><span class="sxs-lookup"><span data-stu-id="c95bd-115">PolyBase also provides a high-performance option for loading the data.</span></span> <span data-ttu-id="c95bd-116">Ale, která znamená, pomocí dvou nástrojů místo jeden.</span><span class="sxs-lookup"><span data-stu-id="c95bd-116">However, that does mean using two tools instead of one.</span></span> <span data-ttu-id="c95bd-117">Pokud třeba nejlepší výkon použijte PolyBase.</span><span class="sxs-lookup"><span data-stu-id="c95bd-117">If you need the best performance then use PolyBase.</span></span> <span data-ttu-id="c95bd-118">Pokud chcete prostředí jednotný nástroj (a data nejsou masivní) ADF je vaše odpověď.</span><span class="sxs-lookup"><span data-stu-id="c95bd-118">If you want a single tool experience (and the data is not massive) then ADF is your answer.</span></span>


> 
> 

<span data-ttu-id="c95bd-119">Přejděte přes v následujícím článku pro některé dobře [ADF ukázky][ADF samples].</span><span class="sxs-lookup"><span data-stu-id="c95bd-119">Head over to the following article for some great [ADF samples][ADF samples].</span></span>

## <a name="integration-services"></a><span data-ttu-id="c95bd-120">Integrační služby</span><span class="sxs-lookup"><span data-stu-id="c95bd-120">Integration Services</span></span>
<span data-ttu-id="c95bd-121">Integration Services (SSIS) je výkonný a flexibilní extrahovat transformace a načítání (ETL) nástroj, který podporuje komplexní pracovní postupy, transformaci dat a několik možností načítání data.</span><span class="sxs-lookup"><span data-stu-id="c95bd-121">Integration Services (SSIS) is a powerful and flexible Extract Transform and Load (ETL) tool that supports complex workflows, data transformation, and several data loading options.</span></span> <span data-ttu-id="c95bd-122">Pomocí služby SSIS pouze k přenosu dat do Azure nebo v rámci širší migrace.</span><span class="sxs-lookup"><span data-stu-id="c95bd-122">Use SSIS to simply transfer data to Azure or as part of a broader migration.</span></span>

> [!NOTE]
> <span data-ttu-id="c95bd-123">SSIS můžete exportovat do UTF-8 bez značka pořadí bajtů v souboru.</span><span class="sxs-lookup"><span data-stu-id="c95bd-123">SSIS can export to UTF-8 without the byte order mark in the file.</span></span> <span data-ttu-id="c95bd-124">Ke konfiguraci to musíte nejdřív komponentu odvozených sloupců použijte k převedení dat znak v toku dat pomocí 65001 kódu stránky UTF-8.</span><span class="sxs-lookup"><span data-stu-id="c95bd-124">To configure this you must first use the derived column component to convert the character data in the data flow to use the 65001 UTF-8 code page.</span></span> <span data-ttu-id="c95bd-125">Jakmile sloupce, které byly převedeny, zapsat data do cílové adaptér plochý soubor zajistíte, že 65001 také byla vybrána jako znaková stránka pro soubor.</span><span class="sxs-lookup"><span data-stu-id="c95bd-125">Once the columns have been converted, write the data to the flat file destination adapter ensuring that 65001 has also been selected as the code page for the file.</span></span>
> 
> 

<span data-ttu-id="c95bd-126">SSIS se připojuje k SQL Data Warehouse stejně, jako by se připojit k nasazení systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c95bd-126">SSIS connects to SQL Data Warehouse just as it would connect to a SQL Server deployment.</span></span> <span data-ttu-id="c95bd-127">Připojení však bude muset používat Správce připojení ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="c95bd-127">However, your connections will need to be using an ADO.NET connection manager.</span></span> <span data-ttu-id="c95bd-128">Můžete také dbát na nakonfigurovat "použijte příkaz bulk insert Pokud je k dispozici" nastavení Maximalizovat propustnost.</span><span class="sxs-lookup"><span data-stu-id="c95bd-128">You should also take care to configure the "Use bulk insert when available" setting to maximize throughput.</span></span> <span data-ttu-id="c95bd-129">Podrobnosti najdete [ADO.NET cílový adaptér] [ ADO.NET destination adapter] článku Další informace o této vlastnosti</span><span class="sxs-lookup"><span data-stu-id="c95bd-129">Please refer to the [ADO.NET destination adapter][ADO.NET destination adapter] article to learn more about this property</span></span>

> [!NOTE]
> <span data-ttu-id="c95bd-130">Připojení k Azure SQL Data Warehouse pomocí OLEDB není podporováno.</span><span class="sxs-lookup"><span data-stu-id="c95bd-130">Connecting to Azure SQL Data Warehouse by using OLEDB is not supported.</span></span>
> 
> 

<span data-ttu-id="c95bd-131">Kromě toho je vždy možnost, že balíček se pravděpodobně nezdaří z důvodu omezení nebo síťové problémy.</span><span class="sxs-lookup"><span data-stu-id="c95bd-131">In addition, there is always the possibility that a package might fail due to throttling or network issues.</span></span> <span data-ttu-id="c95bd-132">Návrh balíčky, takže se můžete pokračovat v místě selhání bez opakovaného fungovat dokončení před selháním.</span><span class="sxs-lookup"><span data-stu-id="c95bd-132">Design packages so they can be resumed at the point of failure, without redoing work that completed before the failure.</span></span>

<span data-ttu-id="c95bd-133">Další informace naleznete [SSIS dokumentaci][SSIS documentation].</span><span class="sxs-lookup"><span data-stu-id="c95bd-133">For more information consult the [SSIS documentation][SSIS documentation].</span></span>

## <a name="bcp"></a><span data-ttu-id="c95bd-134">bcp</span><span class="sxs-lookup"><span data-stu-id="c95bd-134">bcp</span></span>
<span data-ttu-id="c95bd-135">BCP je nástroj příkazového řádku, která je určená pro export a import dat plochý soubor.</span><span class="sxs-lookup"><span data-stu-id="c95bd-135">bcp is a command-line utility that is designed for flat file data import and export.</span></span> <span data-ttu-id="c95bd-136">Některé transformace může probíhat během exportu dat.</span><span class="sxs-lookup"><span data-stu-id="c95bd-136">Some transformation can take place during data export.</span></span> <span data-ttu-id="c95bd-137">K provedení jednoduché transformace pomocí dotazu a vyberte transformovat data.</span><span class="sxs-lookup"><span data-stu-id="c95bd-137">To perform simple transformations use a query to select and transform the data.</span></span> <span data-ttu-id="c95bd-138">Po exportu plochých souborů pak dají načíst přímo do cílové databáze SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c95bd-138">Once exported, the flat files can then be loaded directly into the target the SQL Data Warehouse database.</span></span>

> [!NOTE]
> <span data-ttu-id="c95bd-139">Často je vhodné pro zapouzdření transformace použít při exportu dat v zobrazení ve zdrojovém systému.</span><span class="sxs-lookup"><span data-stu-id="c95bd-139">It is often a good idea to encapsulate the transformations used during data export in a view on the source system.</span></span> <span data-ttu-id="c95bd-140">To zajišťuje, že se uchovávají logiku a proces opakovatelných.</span><span class="sxs-lookup"><span data-stu-id="c95bd-140">This ensures that the logic is retained and the process is repeatable.</span></span>
> 
> 

<span data-ttu-id="c95bd-141">Výhody bcp jsou:</span><span class="sxs-lookup"><span data-stu-id="c95bd-141">Advantages of bcp are:</span></span>

* <span data-ttu-id="c95bd-142">Jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="c95bd-142">Simplicity.</span></span> <span data-ttu-id="c95bd-143">příkazy BCP jsou jednoduché sestavení a spuštění</span><span class="sxs-lookup"><span data-stu-id="c95bd-143">bcp commands are simple to build and execute</span></span>
* <span data-ttu-id="c95bd-144">Znovu spustit zatížení proces.</span><span class="sxs-lookup"><span data-stu-id="c95bd-144">Re-startable load process.</span></span> <span data-ttu-id="c95bd-145">Jednou exportovaný zatížení může být provedeny v libovolném počtu</span><span class="sxs-lookup"><span data-stu-id="c95bd-145">Once exported the load can be executed any number of times</span></span>

<span data-ttu-id="c95bd-146">Omezení bcp jsou:</span><span class="sxs-lookup"><span data-stu-id="c95bd-146">Limitations of bcp are:</span></span>

* <span data-ttu-id="c95bd-147">BCP pracuje s tabulce pouze plochých souborů.</span><span class="sxs-lookup"><span data-stu-id="c95bd-147">bcp works with tabulated flat files only.</span></span> <span data-ttu-id="c95bd-148">Nefunguje s soubory, jako jsou xml nebo JSON</span><span class="sxs-lookup"><span data-stu-id="c95bd-148">It does not work with files such as xml or JSON</span></span>
* <span data-ttu-id="c95bd-149">Funkce transformace dat jsou omezeny na fázi exportu a jsou jednoduché ve své podstatě</span><span class="sxs-lookup"><span data-stu-id="c95bd-149">Data transformation capabilities are limited to the export stage only and are simple in nature</span></span>
* <span data-ttu-id="c95bd-150">BCP nebyla přizpůsobena být robustní při načítání dat přes internet.</span><span class="sxs-lookup"><span data-stu-id="c95bd-150">bcp has not been adapted to be robust when loading data over the internet.</span></span> <span data-ttu-id="c95bd-151">Nestability sítě může způsobit chybu zatížení.</span><span class="sxs-lookup"><span data-stu-id="c95bd-151">Any network instability may cause a load error.</span></span>
* <span data-ttu-id="c95bd-152">BCP spoléhá na schéma se nachází v cílové databázi před zatížení</span><span class="sxs-lookup"><span data-stu-id="c95bd-152">bcp relies on the schema being present in the target database prior to the load</span></span>

<span data-ttu-id="c95bd-153">Další informace najdete v tématu [pomocí bcp k načtení dat do SQL Data Warehouse][Use bcp to load data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="c95bd-153">For more information, see [Use bcp to load data into SQL Data Warehouse][Use bcp to load data into SQL Data Warehouse].</span></span>

## <a name="optimizing-data-migration"></a><span data-ttu-id="c95bd-154">Optimalizace migrace dat</span><span class="sxs-lookup"><span data-stu-id="c95bd-154">Optimizing data migration</span></span>
<span data-ttu-id="c95bd-155">Proces migrace dat SQLDW můžete efektivně rozdělit na tři samostatné kroky:</span><span class="sxs-lookup"><span data-stu-id="c95bd-155">A SQLDW data migration process can be effectively broken down into three discrete steps:</span></span>

1. <span data-ttu-id="c95bd-156">Export zdroje dat</span><span class="sxs-lookup"><span data-stu-id="c95bd-156">Export of source data</span></span>
2. <span data-ttu-id="c95bd-157">Přenos dat do Azure</span><span class="sxs-lookup"><span data-stu-id="c95bd-157">Transfer of data to Azure</span></span>
3. <span data-ttu-id="c95bd-158">Načtení do cílové databáze SQLDW</span><span class="sxs-lookup"><span data-stu-id="c95bd-158">Load into the target SQLDW database</span></span>

<span data-ttu-id="c95bd-159">Každý krok může jednotlivě optimalizovaný na vytváření robustní, znovu spustit a odolné migrace proces, který maximalizuje výkon při každém kroku.</span><span class="sxs-lookup"><span data-stu-id="c95bd-159">Each step can be individually optimized to create a robust, re-startable and resilient migration process that maximizes performance at each step.</span></span>

## <a name="optimizing-data-load"></a><span data-ttu-id="c95bd-160">Optimalizace načtení dat</span><span class="sxs-lookup"><span data-stu-id="c95bd-160">Optimizing data load</span></span>
<span data-ttu-id="c95bd-161">Prohlížení tyto v obráceném pořadí na chvíli; nejrychlejší způsob, jak načíst data je prostřednictvím PolyBase.</span><span class="sxs-lookup"><span data-stu-id="c95bd-161">Looking at these in reverse order for a moment; the fastest way to load data is via PolyBase.</span></span> <span data-ttu-id="c95bd-162">Optimalizace pro proces zatížení PolyBase umístí požadavky na předchozí kroky proto je vhodné pro lepší vysvětlení předem.</span><span class="sxs-lookup"><span data-stu-id="c95bd-162">Optimizing for a PolyBase load process places prerequisites on the preceding steps so it's best to understand this upfront.</span></span> <span data-ttu-id="c95bd-163">Jsou:</span><span class="sxs-lookup"><span data-stu-id="c95bd-163">They are:</span></span>

1. <span data-ttu-id="c95bd-164">Kódování dat souborů</span><span class="sxs-lookup"><span data-stu-id="c95bd-164">Encoding of data files</span></span>
2. <span data-ttu-id="c95bd-165">Formátu dat souborů</span><span class="sxs-lookup"><span data-stu-id="c95bd-165">Format of data files</span></span>
3. <span data-ttu-id="c95bd-166">Umístění datových souborů</span><span class="sxs-lookup"><span data-stu-id="c95bd-166">Location of data files</span></span>

### <a name="encoding"></a><span data-ttu-id="c95bd-167">Encoding</span><span class="sxs-lookup"><span data-stu-id="c95bd-167">Encoding</span></span>
<span data-ttu-id="c95bd-168">PolyBase vyžaduje datové soubory ve formátu UTF-8 nebo UTF-16FE.</span><span class="sxs-lookup"><span data-stu-id="c95bd-168">PolyBase requires data files to be UTF-8 or UTF-16FE.</span></span> 



### <a name="format-of-data-files"></a><span data-ttu-id="c95bd-169">Formátu dat souborů</span><span class="sxs-lookup"><span data-stu-id="c95bd-169">Format of data files</span></span>
<span data-ttu-id="c95bd-170">PolyBase vyžaduje pevnou řádek ukončovací \n nebo nový řádek.</span><span class="sxs-lookup"><span data-stu-id="c95bd-170">PolyBase mandates a fixed row terminator of \n or newline.</span></span> <span data-ttu-id="c95bd-171">Tento standard musí odpovídat datové soubory.</span><span class="sxs-lookup"><span data-stu-id="c95bd-171">Your data files must conform to this standard.</span></span> <span data-ttu-id="c95bd-172">Nejsou k dispozici žádné omezení konců řetězec nebo sloupec.</span><span class="sxs-lookup"><span data-stu-id="c95bd-172">There aren't any restrictions on string or column terminators.</span></span>

<span data-ttu-id="c95bd-173">Je nutné definovat všechny sloupce v souboru v rámci vaší externí tabulky v PolyBase.</span><span class="sxs-lookup"><span data-stu-id="c95bd-173">You will have to define every column in the file as part of your external table in PolyBase.</span></span> <span data-ttu-id="c95bd-174">Ujistěte se, že všechny sloupce exportovaný jsou povinné a že typy v souladu s požadované standardy.</span><span class="sxs-lookup"><span data-stu-id="c95bd-174">Make sure that all exported columns are required and that the types conform to the required standards.</span></span>

<span data-ttu-id="c95bd-175">Naleznete zpět [migrovat schéma] najdete v článku o podporované datové typy.</span><span class="sxs-lookup"><span data-stu-id="c95bd-175">Please refer back to the [migrate your schema] article for detail on supported data types.</span></span>

### <a name="location-of-data-files"></a><span data-ttu-id="c95bd-176">Umístění datových souborů</span><span class="sxs-lookup"><span data-stu-id="c95bd-176">Location of data files</span></span>
<span data-ttu-id="c95bd-177">SQL Data Warehouse PolyBase používá k načtení dat z Azure Blob Storage výhradně.</span><span class="sxs-lookup"><span data-stu-id="c95bd-177">SQL Data Warehouse uses PolyBase to load data from Azure Blob Storage exclusively.</span></span> <span data-ttu-id="c95bd-178">V důsledku toho data musí nejprve přenesení do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="c95bd-178">Consequently, the data must have been first transferred into blob storage.</span></span>

## <a name="optimizing-data-transfer"></a><span data-ttu-id="c95bd-179">Optimalizace přenos dat</span><span class="sxs-lookup"><span data-stu-id="c95bd-179">Optimizing data transfer</span></span>
<span data-ttu-id="c95bd-180">Jedna z částí nejpomalejší migrace dat je přenos dat do Azure.</span><span class="sxs-lookup"><span data-stu-id="c95bd-180">One of the slowest parts of data migration is the transfer of the data to Azure.</span></span> <span data-ttu-id="c95bd-181">Pouze šířku pásma sítě může být problém, ale také spolehlivost sítě může být vážnou překážkou průběh.</span><span class="sxs-lookup"><span data-stu-id="c95bd-181">Not only can network bandwidth be an issue but also network reliability can seriously hamper progress.</span></span> <span data-ttu-id="c95bd-182">Ve výchozím nastavení migraci dat do Azure se tak pravděpodobnost chyby přenosu, k nimž jsou to bude přiměřeně pravděpodobně přes internet.</span><span class="sxs-lookup"><span data-stu-id="c95bd-182">By default migrating data to Azure is over the internet so the chances of transfer errors occurring are reasonably likely.</span></span> <span data-ttu-id="c95bd-183">Tyto chyby však může vyžadovat opětovné odeslání celé nebo částečně data.</span><span class="sxs-lookup"><span data-stu-id="c95bd-183">However, these errors may require data to be re-sent either in whole or in part.</span></span>

<span data-ttu-id="c95bd-184">Naštěstí máte několik možností k dosažení vyšší rychlosti a odolnost proti tohoto procesu:</span><span class="sxs-lookup"><span data-stu-id="c95bd-184">Fortunately you have several options to improve the speed and resilience of this process:</span></span>

### <a name="expressrouteexpressroute"></a><span data-ttu-id="c95bd-185">[ExpressRoute][ExpressRoute]</span><span class="sxs-lookup"><span data-stu-id="c95bd-185">[ExpressRoute][ExpressRoute]</span></span>
<span data-ttu-id="c95bd-186">Možná budete chtít zvážit použití [ExpressRoute] [ ExpressRoute] pro urychlení přenosu.</span><span class="sxs-lookup"><span data-stu-id="c95bd-186">You may want to consider using [ExpressRoute][ExpressRoute] to speed up the transfer.</span></span> <span data-ttu-id="c95bd-187">[ExpressRoute] [ ExpressRoute] vám poskytne vytvořené privátní připojení do Azure, připojení nepřenášejí prostřednictvím veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="c95bd-187">[ExpressRoute][ExpressRoute] provides you with an established private connection to Azure so the connection does not go over the public internet.</span></span> <span data-ttu-id="c95bd-188">Toto je rozhodně není povinný krok.</span><span class="sxs-lookup"><span data-stu-id="c95bd-188">This is by no means a mandatory step.</span></span> <span data-ttu-id="c95bd-189">Ale ho bude při předání dat do Azure z místního zlepšit propustnost nebo společném umístění.</span><span class="sxs-lookup"><span data-stu-id="c95bd-189">However, it will improve throughput when pushing data to Azure from an on-premises or co-location facility.</span></span>

<span data-ttu-id="c95bd-190">Výhody použití [ExpressRoute] [ ExpressRoute] jsou:</span><span class="sxs-lookup"><span data-stu-id="c95bd-190">The benefits of using [ExpressRoute][ExpressRoute] are:</span></span>

1. <span data-ttu-id="c95bd-191">Větší spolehlivost</span><span class="sxs-lookup"><span data-stu-id="c95bd-191">Increased reliability</span></span>
2. <span data-ttu-id="c95bd-192">Vyšší rychlost sítě</span><span class="sxs-lookup"><span data-stu-id="c95bd-192">Faster network speed</span></span>
3. <span data-ttu-id="c95bd-193">Nižší latenci sítě</span><span class="sxs-lookup"><span data-stu-id="c95bd-193">Lower network latency</span></span>
4. <span data-ttu-id="c95bd-194">vyšší zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="c95bd-194">higher network security</span></span>

<span data-ttu-id="c95bd-195">[ExpressRoute] [ ExpressRoute] je výhodné pro různé scénáře; nejen migrace.</span><span class="sxs-lookup"><span data-stu-id="c95bd-195">[ExpressRoute][ExpressRoute] is beneficial for a number of scenarios; not just the migration.</span></span>

<span data-ttu-id="c95bd-196">Zajímá vás to?</span><span class="sxs-lookup"><span data-stu-id="c95bd-196">Interested?</span></span> <span data-ttu-id="c95bd-197">Další informace a ceny prosím naleznete [dokumentace ExpressRoute][ExpressRoute documentation].</span><span class="sxs-lookup"><span data-stu-id="c95bd-197">For more information and pricing please visit the [ExpressRoute documentation][ExpressRoute documentation].</span></span>

### <a name="azure-import-and-export-service"></a><span data-ttu-id="c95bd-198">Azure Import a Export služby</span><span class="sxs-lookup"><span data-stu-id="c95bd-198">Azure Import and Export Service</span></span>
<span data-ttu-id="c95bd-199">Azure Import a Export služby je proces přenosu dat pro velké (GB ++) umožňuje masivní (TB ++) přenosů dat do Azure.</span><span class="sxs-lookup"><span data-stu-id="c95bd-199">The Azure Import and Export Service is a data transfer process designed for large (GB++) to massive (TB++) transfers of data into Azure.</span></span> <span data-ttu-id="c95bd-200">Se týká zápisu dat na disky a přesouvání je datové centrum Azure.</span><span class="sxs-lookup"><span data-stu-id="c95bd-200">It involves writing your data to disks and shipping them to an Azure data center.</span></span> <span data-ttu-id="c95bd-201">Obsah disku se pak načíst do Azure Storage Blobs vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="c95bd-201">The disk contents will then be loaded into Azure Storage Blobs on your behalf.</span></span>

<span data-ttu-id="c95bd-202">Souhrnné zobrazení procesu importu export vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c95bd-202">A high-level view of the import export process is as follows:</span></span>

1. <span data-ttu-id="c95bd-203">Konfigurace kontejner úložiště objektů Blob Azure přijmout data</span><span class="sxs-lookup"><span data-stu-id="c95bd-203">Configure an Azure Blob Storage container to receive the data</span></span>
2. <span data-ttu-id="c95bd-204">Exportovat data do místního úložiště</span><span class="sxs-lookup"><span data-stu-id="c95bd-204">Export your data to local storage</span></span>
3. <span data-ttu-id="c95bd-205">Zkopírovat data do 3,5 SATA II/III pevných disků v nástroji [Azure Import/Export]</span><span class="sxs-lookup"><span data-stu-id="c95bd-205">Copy the data to 3.5 inch SATA II/III hard disk drives using the [Azure Import/Export Tool]</span></span>
4. <span data-ttu-id="c95bd-206">Vytvoření úlohy importu pomocí Azure Import a Export služba poskytující soubory deníku vytvořený nástrojem [Azure Import/Export]</span><span class="sxs-lookup"><span data-stu-id="c95bd-206">Create an Import Job using the Azure Import and Export Service providing the journal files produced by the [Azure Import/Export Tool]</span></span>
5. <span data-ttu-id="c95bd-207">Dodávat disky určenou Azure datové centrum</span><span class="sxs-lookup"><span data-stu-id="c95bd-207">Ship the disks your nominated Azure data center</span></span>
6. <span data-ttu-id="c95bd-208">Vaše data se přenáší do vašeho kontejneru Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="c95bd-208">Your data is transferred to your Azure Blob Storage container</span></span>
7. <span data-ttu-id="c95bd-209">Načíst data do SQLDW pomocí PolyBase</span><span class="sxs-lookup"><span data-stu-id="c95bd-209">Load the data into SQLDW using PolyBase</span></span>

### <a name="azcopyazcopy-utility"></a><span data-ttu-id="c95bd-210">[AZCopy][AZCopy] nástroj</span><span class="sxs-lookup"><span data-stu-id="c95bd-210">[AZCopy][AZCopy] utility</span></span>
<span data-ttu-id="c95bd-211">[AZCopy][AZCopy] nástroj je skvělý nástroj pro získávání dat přenesených do objektů BLOB služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c95bd-211">The [AZCopy][AZCopy] utility is a great tool for getting your data transferred into Azure Storage Blobs.</span></span> <span data-ttu-id="c95bd-212">Pro velmi velký přenos dat (GB ++) je určený pro malé (MB ++).</span><span class="sxs-lookup"><span data-stu-id="c95bd-212">It is designed for small (MB++) to very large (GB++) data transfers.</span></span> <span data-ttu-id="c95bd-213">[AZCopy] byl navržen tak, aby poskytují dobrý odolné propustnost při přenosu dat do Azure a tak je je služba skvělou volbou pro krok přenos dat.</span><span class="sxs-lookup"><span data-stu-id="c95bd-213">[AZCopy] has also been designed to provide good resilient throughput when transferring data to Azure and so is a great choice for the data transfer step.</span></span> <span data-ttu-id="c95bd-214">Jednou přenášená můžete načíst data do SQL Data Warehouse pomocí PolyBase.</span><span class="sxs-lookup"><span data-stu-id="c95bd-214">Once transferred you can load the data using PolyBase into SQL Data Warehouse.</span></span> <span data-ttu-id="c95bd-215">AZCopy taky můžete začlenit do vaší služby SSIS balíčky pomocí "Spustit proces" úlohy.</span><span class="sxs-lookup"><span data-stu-id="c95bd-215">You can also incorporate AZCopy into your SSIS packages using an "Execute Process" task.</span></span>

<span data-ttu-id="c95bd-216">Použití nástroje AZCopy nejprve musíte stáhnout a nainstalovat ji.</span><span class="sxs-lookup"><span data-stu-id="c95bd-216">To use AZCopy you will first need to download and install it.</span></span> <span data-ttu-id="c95bd-217">Je [produkční verzi] [ production version] a [verze preview] [ preview version] k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c95bd-217">There is a [production version][production version] and a [preview version][preview version] available.</span></span>

<span data-ttu-id="c95bd-218">Nahrát soubor ze systému souborů budete potřebovat podobná té následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c95bd-218">To upload a file from your file system you will need a command like the one below:</span></span>

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="c95bd-219">Proces vysoké úrovně souhrn může být:</span><span class="sxs-lookup"><span data-stu-id="c95bd-219">A high-level process summary could be:</span></span>

1. <span data-ttu-id="c95bd-220">Konfigurace kontejner objektu blob úložiště Azure pro příjem dat</span><span class="sxs-lookup"><span data-stu-id="c95bd-220">Configure an Azure storage blob container to receive the data</span></span>
2. <span data-ttu-id="c95bd-221">Exportovat data do místního úložiště</span><span class="sxs-lookup"><span data-stu-id="c95bd-221">Export your data to local storage</span></span>
3. <span data-ttu-id="c95bd-222">AZCopy vaše data v kontejneru Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="c95bd-222">AZCopy your data in the Azure Blob Storage container</span></span>
4. <span data-ttu-id="c95bd-223">Načítání dat do SQL Data Warehouse pomocí PolyBase</span><span class="sxs-lookup"><span data-stu-id="c95bd-223">Load the data into SQL Data Warehouse using PolyBase</span></span>

<span data-ttu-id="c95bd-224">Úplné dokumentaci k dispozici: [AZCopy][AZCopy].</span><span class="sxs-lookup"><span data-stu-id="c95bd-224">Full documentation available: [AZCopy][AZCopy].</span></span>

## <a name="optimizing-data-export"></a><span data-ttu-id="c95bd-225">Optimalizace export dat</span><span class="sxs-lookup"><span data-stu-id="c95bd-225">Optimizing data export</span></span>
<span data-ttu-id="c95bd-226">Kromě zajištění, že export splňuje požadavky nastíněny polybase můžete také hledat optimalizovat export data, která mají vylepšit další proces.</span><span class="sxs-lookup"><span data-stu-id="c95bd-226">In addition to ensuring that the export conforms to the requirements laid out by PolyBase you can also seek to optimize the export of the data to improve the process further.</span></span>



### <a name="data-compression"></a><span data-ttu-id="c95bd-227">Komprese dat</span><span class="sxs-lookup"><span data-stu-id="c95bd-227">Data compression</span></span>
<span data-ttu-id="c95bd-228">PolyBase čte data komprimované gzip.</span><span class="sxs-lookup"><span data-stu-id="c95bd-228">PolyBase can read gzip compressed data.</span></span> <span data-ttu-id="c95bd-229">Pokud budete moci komprese dat, aby se soubory gzip se minimalizuje množství dat, které se instaluje přes síť.</span><span class="sxs-lookup"><span data-stu-id="c95bd-229">If you are able to compress your data to gzip files then you will minimize the amount of data being pushed over the network.</span></span>

### <a name="multiple-files"></a><span data-ttu-id="c95bd-230">Více souborů</span><span class="sxs-lookup"><span data-stu-id="c95bd-230">Multiple files</span></span>
<span data-ttu-id="c95bd-231">Rozdělení rozsáhlé tabulky do několika souborů nejen pomáhá zvýšit rychlost exportu, také pomáhá s re-startability přenos a celkové možnosti správy dat jednou v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="c95bd-231">Breaking up large tables into several files not only helps to improve export speed, it also helps with transfer re-startability, and the overall manageability of the data once in the Azure blob storage.</span></span> <span data-ttu-id="c95bd-232">Jedním z mnoha dobrý funkcí PolyBase je, že bude číst všechny soubory do složky a s nimi zacházet jako jedna tabulka.</span><span class="sxs-lookup"><span data-stu-id="c95bd-232">One of the many nice features of PolyBase is that it will read all the files inside a folder and treat it as one table.</span></span> <span data-ttu-id="c95bd-233">Proto je vhodné izolovat soubory pro každou tabulku do vlastní složky.</span><span class="sxs-lookup"><span data-stu-id="c95bd-233">It is therefore a good idea to isolate the files for each table into its own folder.</span></span>

<span data-ttu-id="c95bd-234">PolyBase také podporuje funkci označuje jako "rekurzivní složky traversal".</span><span class="sxs-lookup"><span data-stu-id="c95bd-234">PolyBase also supports a feature known as "recursive folder traversal".</span></span> <span data-ttu-id="c95bd-235">Tato funkce slouží k dalšímu vylepšení útoků organizace data exportovaná ke zlepšení vaší správy dat.</span><span class="sxs-lookup"><span data-stu-id="c95bd-235">You can use this feature to further enhance the organization of your exported data to improve your data management.</span></span>

<span data-ttu-id="c95bd-236">Další informace o načtení dat pomocí funkce PolyBase, najdete v části [PolyBase používá k načtení dat do SQL Data Warehouse][Use PolyBase to load data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="c95bd-236">To learn more about loading data with PolyBase, see [Use PolyBase to load data into SQL Data Warehouse][Use PolyBase to load data into SQL Data Warehouse].</span></span>

## <a name="next-steps"></a><span data-ttu-id="c95bd-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c95bd-237">Next steps</span></span>
<span data-ttu-id="c95bd-238">Další informace o migraci najdete v tématu [migrace vašeho řešení do SQL Data Warehouse][Migrate your solution to SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="c95bd-238">For more about migration, see [Migrate your solution to SQL Data Warehouse][Migrate your solution to SQL Data Warehouse].</span></span>
<span data-ttu-id="c95bd-239">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="c95bd-239">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[AZCopy]:../storage/common/storage-use-azcopy.md
[ADF Copy]: ../data-factory/data-factory-data-movement-activities.md 
[ADF samples]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[development overview]: sql-data-warehouse-overview-develop.md
[Migrate your solution to SQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Use bcp to load data into SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[Use PolyBase to load data into SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute documentation]: http://azure.microsoft.com/documentation/services/expressroute/

[production version]: http://aka.ms/downloadazcopy/
[preview version]: http://aka.ms/downloadazcopypr/
[ADO.NET destination adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS documentation]: https://msdn.microsoft.com/library/ms141026.aspx
