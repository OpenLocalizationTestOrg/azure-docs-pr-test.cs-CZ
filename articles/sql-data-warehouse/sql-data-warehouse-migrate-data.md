---
title: "aaaMigrate tooSQL vaše data datového skladu | Microsoft Docs"
description: "Tipy pro migraci vaše data tooAzure SQL Data Warehouse na vývoj řešení."
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
ms.openlocfilehash: fe4c6b7e82094c59c45e06be6da225fee1b707ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-data"></a><span data-ttu-id="8a38b-103">Migrace dat</span><span class="sxs-lookup"><span data-stu-id="8a38b-103">Migrate Your Data</span></span>
<span data-ttu-id="8a38b-104">Data lze přesunout z různých zdrojů do SQL Data Warehouse pomocí různých nástrojů.</span><span class="sxs-lookup"><span data-stu-id="8a38b-104">Data can be moved from different sources into your SQL Data Warehouse with a variety tools.</span></span>  <span data-ttu-id="8a38b-105">Kopírování ADF, SSIS a všechny bcp můžou být použité tooachieve tohoto cíle.</span><span class="sxs-lookup"><span data-stu-id="8a38b-105">ADF Copy, SSIS, and bcp can all be used tooachieve this goal.</span></span> <span data-ttu-id="8a38b-106">Při rostoucí hello množství dat, musí však rozmyslete rozdělení hello proces migrace dat do kroků.</span><span class="sxs-lookup"><span data-stu-id="8a38b-106">However, as hello amount of data increases you should think about breaking down hello data migration process into steps.</span></span> <span data-ttu-id="8a38b-107">To umožňuje hello možnost toooptimize každý krok pro výkon i pro odolnost tooensure smooth dat migrace.</span><span class="sxs-lookup"><span data-stu-id="8a38b-107">This affords you hello opportunity toooptimize each step both for performance and for resilience tooensure a smooth data migration.</span></span>

<span data-ttu-id="8a38b-108">Tento článek popisuje nejprve hello jednoduché migrační scénáře ADF Copy, SSIS a bcp.</span><span class="sxs-lookup"><span data-stu-id="8a38b-108">This article first discusses hello simple migration scenarios of ADF Copy, SSIS, and bcp.</span></span> <span data-ttu-id="8a38b-109">Následně vypadá trochu hlouběji do jak lze optimalizovat hello migrace.</span><span class="sxs-lookup"><span data-stu-id="8a38b-109">It then look a little deeper into how hello migration can be optimized.</span></span>

## <a name="azure-data-factory-adf-copy"></a><span data-ttu-id="8a38b-110">Azure Data Factory (ADF) kopie</span><span class="sxs-lookup"><span data-stu-id="8a38b-110">Azure Data Factory (ADF) copy</span></span>
<span data-ttu-id="8a38b-111">[Kopírování ADF] [ ADF Copy] je součástí [Azure Data Factory][Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="8a38b-111">[ADF Copy][ADF Copy] is part of [Azure Data Factory][Azure Data Factory].</span></span> <span data-ttu-id="8a38b-112">Můžete kopírovat ADF tooexport datové soubory tooflat umístěný v místním úložišti plochých souborů tooremote uchovávat v Azure blob storage nebo přímo do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8a38b-112">You can use ADF Copy tooexport your data tooflat files residing on local storage, tooremote flat files held in Azure blob storage or directly into SQL Data Warehouse.</span></span>

<span data-ttu-id="8a38b-113">Pokud vaše data se spustí v plochých souborů, pak musíte nejdřív tootransfer se objekt blob úložiště tooAzure před zahájením zatížení do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8a38b-113">If your data starts in flat files, then you will first need tootransfer it tooAzure storage blob before initiating a load it into SQL Data Warehouse.</span></span> <span data-ttu-id="8a38b-114">Jakmile hello data se přenáší do úložiště objektů blob v Azure můžete toouse [ADF kopie] [ ADF Copy] znovu toopush hello dat do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8a38b-114">Once hello data is transferred into Azure blob storage you can choose toouse [ADF Copy][ADF Copy] again toopush hello data into SQL Data Warehouse.</span></span>

<span data-ttu-id="8a38b-115">PolyBase také nabízí možnost vysoce výkonné pro načítání dat hello.</span><span class="sxs-lookup"><span data-stu-id="8a38b-115">PolyBase also provides a high-performance option for loading hello data.</span></span> <span data-ttu-id="8a38b-116">Ale, která znamená, pomocí dvou nástrojů místo jeden.</span><span class="sxs-lookup"><span data-stu-id="8a38b-116">However, that does mean using two tools instead of one.</span></span> <span data-ttu-id="8a38b-117">Pokud jste třeba nejlepší výkon hello pak pomocí funkce PolyBase.</span><span class="sxs-lookup"><span data-stu-id="8a38b-117">If you need hello best performance then use PolyBase.</span></span> <span data-ttu-id="8a38b-118">Pokud chcete, aby prostředí jednotný nástroj (a hello dat není masivní) ADF je vaše odpověď.</span><span class="sxs-lookup"><span data-stu-id="8a38b-118">If you want a single tool experience (and hello data is not massive) then ADF is your answer.</span></span>


> 
> 

<span data-ttu-id="8a38b-119">HEAD přes toohello následujícího článku pro některé dobře [ADF ukázky][ADF samples].</span><span class="sxs-lookup"><span data-stu-id="8a38b-119">Head over toohello following article for some great [ADF samples][ADF samples].</span></span>

## <a name="integration-services"></a><span data-ttu-id="8a38b-120">Integrační služby</span><span class="sxs-lookup"><span data-stu-id="8a38b-120">Integration Services</span></span>
<span data-ttu-id="8a38b-121">Integration Services (SSIS) je výkonný a flexibilní extrahovat transformace a načítání (ETL) nástroj, který podporuje komplexní pracovní postupy, transformaci dat a několik možností načítání data.</span><span class="sxs-lookup"><span data-stu-id="8a38b-121">Integration Services (SSIS) is a powerful and flexible Extract Transform and Load (ETL) tool that supports complex workflows, data transformation, and several data loading options.</span></span> <span data-ttu-id="8a38b-122">Použití služby SSIS toosimply přenos dat tooAzure nebo v rámci širší migrace.</span><span class="sxs-lookup"><span data-stu-id="8a38b-122">Use SSIS toosimply transfer data tooAzure or as part of a broader migration.</span></span>

> [!NOTE]
> <span data-ttu-id="8a38b-123">SSIS můžete exportovat tooUTF-8 bez hello značka pořadí bajtů v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="8a38b-123">SSIS can export tooUTF-8 without hello byte order mark in hello file.</span></span> <span data-ttu-id="8a38b-124">tooconfigure, to je nutné nejprve použít hello odvodit sloupec součástí tooconvert hello textová data v hello datového toku toouse hello 65001 UTF-8 znaková stránka.</span><span class="sxs-lookup"><span data-stu-id="8a38b-124">tooconfigure this you must first use hello derived column component tooconvert hello character data in hello data flow toouse hello 65001 UTF-8 code page.</span></span> <span data-ttu-id="8a38b-125">Jakmile byly převedeny hello sloupců, zapisovat hello data toohello plochý soubor cílový adaptér zajistit, že 65001 také byla vybrána jako hello znaková stránka pro soubor hello.</span><span class="sxs-lookup"><span data-stu-id="8a38b-125">Once hello columns have been converted, write hello data toohello flat file destination adapter ensuring that 65001 has also been selected as hello code page for hello file.</span></span>
> 
> 

<span data-ttu-id="8a38b-126">SSIS připojí tooSQL datového skladu, stejně jako se bude připojovat tooa nasazení systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8a38b-126">SSIS connects tooSQL Data Warehouse just as it would connect tooa SQL Server deployment.</span></span> <span data-ttu-id="8a38b-127">Připojení se ale potřebovat toobe Správci připojení ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="8a38b-127">However, your connections will need toobe using an ADO.NET connection manager.</span></span> <span data-ttu-id="8a38b-128">Můžete také dbát na tooconfigure hello "Použijte příkaz bulk insert Pokud je k dispozici" nastavení toomaximize propustnost.</span><span class="sxs-lookup"><span data-stu-id="8a38b-128">You should also take care tooconfigure hello "Use bulk insert when available" setting toomaximize throughput.</span></span> <span data-ttu-id="8a38b-129">Naleznete toohello [ADO.NET cílový adaptér] [ ADO.NET destination adapter] článku toolearn Další informace o této vlastnosti</span><span class="sxs-lookup"><span data-stu-id="8a38b-129">Please refer toohello [ADO.NET destination adapter][ADO.NET destination adapter] article toolearn more about this property</span></span>

> [!NOTE]
> <span data-ttu-id="8a38b-130">Připojení tooAzure SQL Data Warehouse pomocí OLEDB není podporováno.</span><span class="sxs-lookup"><span data-stu-id="8a38b-130">Connecting tooAzure SQL Data Warehouse by using OLEDB is not supported.</span></span>
> 
> 

<span data-ttu-id="8a38b-131">Kromě toho je vždy hello možnost, že balíček může selhat z důvodu toothrottling nebo síťové problémy.</span><span class="sxs-lookup"><span data-stu-id="8a38b-131">In addition, there is always hello possibility that a package might fail due toothrottling or network issues.</span></span> <span data-ttu-id="8a38b-132">Návrh balíčků tak, že lze obnovit v hello bodem selhání, bez opakovaného fungovat dokončení před selháním hello.</span><span class="sxs-lookup"><span data-stu-id="8a38b-132">Design packages so they can be resumed at hello point of failure, without redoing work that completed before hello failure.</span></span>

<span data-ttu-id="8a38b-133">Další informace naleznete hello [SSIS dokumentaci][SSIS documentation].</span><span class="sxs-lookup"><span data-stu-id="8a38b-133">For more information consult hello [SSIS documentation][SSIS documentation].</span></span>

## <a name="bcp"></a><span data-ttu-id="8a38b-134">bcp</span><span class="sxs-lookup"><span data-stu-id="8a38b-134">bcp</span></span>
<span data-ttu-id="8a38b-135">BCP je nástroj příkazového řádku, která je určená pro export a import dat plochý soubor.</span><span class="sxs-lookup"><span data-stu-id="8a38b-135">bcp is a command-line utility that is designed for flat file data import and export.</span></span> <span data-ttu-id="8a38b-136">Některé transformace může probíhat během exportu dat.</span><span class="sxs-lookup"><span data-stu-id="8a38b-136">Some transformation can take place during data export.</span></span> <span data-ttu-id="8a38b-137">jednoduché transformace tooperform pomocí dotazu tooselect a transformovat hello data.</span><span class="sxs-lookup"><span data-stu-id="8a38b-137">tooperform simple transformations use a query tooselect and transform hello data.</span></span> <span data-ttu-id="8a38b-138">Po exportu, pak dají načíst přímo do databáze SQL Data Warehouse hello cíl hello hello plochých souborů.</span><span class="sxs-lookup"><span data-stu-id="8a38b-138">Once exported, hello flat files can then be loaded directly into hello target hello SQL Data Warehouse database.</span></span>

> [!NOTE]
> <span data-ttu-id="8a38b-139">Často je text hello tooencapsulate vhodné transformace použít během data exportovat v zobrazení zdroje systému hello.</span><span class="sxs-lookup"><span data-stu-id="8a38b-139">It is often a good idea tooencapsulate hello transformations used during data export in a view on hello source system.</span></span> <span data-ttu-id="8a38b-140">To zajišťuje, že se uchovávají hello logiku a hello proces opakovatelných.</span><span class="sxs-lookup"><span data-stu-id="8a38b-140">This ensures that hello logic is retained and hello process is repeatable.</span></span>
> 
> 

<span data-ttu-id="8a38b-141">Výhody bcp jsou:</span><span class="sxs-lookup"><span data-stu-id="8a38b-141">Advantages of bcp are:</span></span>

* <span data-ttu-id="8a38b-142">Jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="8a38b-142">Simplicity.</span></span> <span data-ttu-id="8a38b-143">příkazy BCP jsou jednoduché toobuild a provést</span><span class="sxs-lookup"><span data-stu-id="8a38b-143">bcp commands are simple toobuild and execute</span></span>
* <span data-ttu-id="8a38b-144">Znovu spustit zatížení proces.</span><span class="sxs-lookup"><span data-stu-id="8a38b-144">Re-startable load process.</span></span> <span data-ttu-id="8a38b-145">Jednou exportovaný hello zatížení může být provedeny v libovolném počtu</span><span class="sxs-lookup"><span data-stu-id="8a38b-145">Once exported hello load can be executed any number of times</span></span>

<span data-ttu-id="8a38b-146">Omezení bcp jsou:</span><span class="sxs-lookup"><span data-stu-id="8a38b-146">Limitations of bcp are:</span></span>

* <span data-ttu-id="8a38b-147">BCP pracuje s tabulce pouze plochých souborů.</span><span class="sxs-lookup"><span data-stu-id="8a38b-147">bcp works with tabulated flat files only.</span></span> <span data-ttu-id="8a38b-148">Nefunguje s soubory, jako jsou xml nebo JSON</span><span class="sxs-lookup"><span data-stu-id="8a38b-148">It does not work with files such as xml or JSON</span></span>
* <span data-ttu-id="8a38b-149">Funkce transformace dat jsou omezené toohello export jenom stage a jednoduchý ve své podstatě</span><span class="sxs-lookup"><span data-stu-id="8a38b-149">Data transformation capabilities are limited toohello export stage only and are simple in nature</span></span>
* <span data-ttu-id="8a38b-150">BCP nebyla přizpůsobena toobe robustní při načítání dat přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="8a38b-150">bcp has not been adapted toobe robust when loading data over hello internet.</span></span> <span data-ttu-id="8a38b-151">Nestability sítě může způsobit chybu zatížení.</span><span class="sxs-lookup"><span data-stu-id="8a38b-151">Any network instability may cause a load error.</span></span>
* <span data-ttu-id="8a38b-152">využívá BCP hello schématu aplikace hello cílové databáze předchozí toohello zatížení</span><span class="sxs-lookup"><span data-stu-id="8a38b-152">bcp relies on hello schema being present in hello target database prior toohello load</span></span>

<span data-ttu-id="8a38b-153">Další informace najdete v tématu [pomocí bcp tooload dat do SQL Data Warehouse][Use bcp tooload data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="8a38b-153">For more information, see [Use bcp tooload data into SQL Data Warehouse][Use bcp tooload data into SQL Data Warehouse].</span></span>

## <a name="optimizing-data-migration"></a><span data-ttu-id="8a38b-154">Optimalizace migrace dat</span><span class="sxs-lookup"><span data-stu-id="8a38b-154">Optimizing data migration</span></span>
<span data-ttu-id="8a38b-155">Proces migrace dat SQLDW můžete efektivně rozdělit na tři samostatné kroky:</span><span class="sxs-lookup"><span data-stu-id="8a38b-155">A SQLDW data migration process can be effectively broken down into three discrete steps:</span></span>

1. <span data-ttu-id="8a38b-156">Export zdroje dat</span><span class="sxs-lookup"><span data-stu-id="8a38b-156">Export of source data</span></span>
2. <span data-ttu-id="8a38b-157">Přenos dat tooAzure</span><span class="sxs-lookup"><span data-stu-id="8a38b-157">Transfer of data tooAzure</span></span>
3. <span data-ttu-id="8a38b-158">Načtení do hello cílová SQLDW databáze</span><span class="sxs-lookup"><span data-stu-id="8a38b-158">Load into hello target SQLDW database</span></span>

<span data-ttu-id="8a38b-159">Každý krok může být jednotlivě optimalizované toocreate proces migrace robustní, znovu spustit a odolné, který maximalizuje výkon při každém kroku.</span><span class="sxs-lookup"><span data-stu-id="8a38b-159">Each step can be individually optimized toocreate a robust, re-startable and resilient migration process that maximizes performance at each step.</span></span>

## <a name="optimizing-data-load"></a><span data-ttu-id="8a38b-160">Optimalizace načtení dat</span><span class="sxs-lookup"><span data-stu-id="8a38b-160">Optimizing data load</span></span>
<span data-ttu-id="8a38b-161">Prohlížení tyto v obráceném pořadí na chvíli; Hello nejrychlejší způsob, jak tooload dat je prostřednictvím PolyBase.</span><span class="sxs-lookup"><span data-stu-id="8a38b-161">Looking at these in reverse order for a moment; hello fastest way tooload data is via PolyBase.</span></span> <span data-ttu-id="8a38b-162">Optimalizace pro proces zatížení PolyBase umístí požadavky na hello předchozích kroků, je nejlepší toounderstand to předem.</span><span class="sxs-lookup"><span data-stu-id="8a38b-162">Optimizing for a PolyBase load process places prerequisites on hello preceding steps so it's best toounderstand this upfront.</span></span> <span data-ttu-id="8a38b-163">Jsou:</span><span class="sxs-lookup"><span data-stu-id="8a38b-163">They are:</span></span>

1. <span data-ttu-id="8a38b-164">Kódování dat souborů</span><span class="sxs-lookup"><span data-stu-id="8a38b-164">Encoding of data files</span></span>
2. <span data-ttu-id="8a38b-165">Formátu dat souborů</span><span class="sxs-lookup"><span data-stu-id="8a38b-165">Format of data files</span></span>
3. <span data-ttu-id="8a38b-166">Umístění datových souborů</span><span class="sxs-lookup"><span data-stu-id="8a38b-166">Location of data files</span></span>

### <a name="encoding"></a><span data-ttu-id="8a38b-167">Encoding</span><span class="sxs-lookup"><span data-stu-id="8a38b-167">Encoding</span></span>
<span data-ttu-id="8a38b-168">PolyBase vyžaduje toobe datové soubory ve formátu UTF-8 nebo UTF-16FE.</span><span class="sxs-lookup"><span data-stu-id="8a38b-168">PolyBase requires data files toobe UTF-8 or UTF-16FE.</span></span> 



### <a name="format-of-data-files"></a><span data-ttu-id="8a38b-169">Formátu dat souborů</span><span class="sxs-lookup"><span data-stu-id="8a38b-169">Format of data files</span></span>
<span data-ttu-id="8a38b-170">PolyBase vyžaduje pevnou řádek ukončovací \n nebo nový řádek.</span><span class="sxs-lookup"><span data-stu-id="8a38b-170">PolyBase mandates a fixed row terminator of \n or newline.</span></span> <span data-ttu-id="8a38b-171">Datové soubory musí odpovídat toothis standard.</span><span class="sxs-lookup"><span data-stu-id="8a38b-171">Your data files must conform toothis standard.</span></span> <span data-ttu-id="8a38b-172">Nejsou k dispozici žádné omezení konců řetězec nebo sloupec.</span><span class="sxs-lookup"><span data-stu-id="8a38b-172">There aren't any restrictions on string or column terminators.</span></span>

<span data-ttu-id="8a38b-173">Budete mít toodefine každý sloupec v souboru hello jako součást externí tabulku v PolyBase.</span><span class="sxs-lookup"><span data-stu-id="8a38b-173">You will have toodefine every column in hello file as part of your external table in PolyBase.</span></span> <span data-ttu-id="8a38b-174">Ujistěte se, že všechny sloupce exportovaný jsou povinné a že hello typy v souladu s toohello požadované standardy.</span><span class="sxs-lookup"><span data-stu-id="8a38b-174">Make sure that all exported columns are required and that hello types conform toohello required standards.</span></span>

<span data-ttu-id="8a38b-175">Zpět naleznete toohello [migrovat schéma] najdete v článku o podporované datové typy.</span><span class="sxs-lookup"><span data-stu-id="8a38b-175">Please refer back toohello [migrate your schema] article for detail on supported data types.</span></span>

### <a name="location-of-data-files"></a><span data-ttu-id="8a38b-176">Umístění datových souborů</span><span class="sxs-lookup"><span data-stu-id="8a38b-176">Location of data files</span></span>
<span data-ttu-id="8a38b-177">SQL Data Warehouse používá výhradně PolyBase tooload dat z Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="8a38b-177">SQL Data Warehouse uses PolyBase tooload data from Azure Blob Storage exclusively.</span></span> <span data-ttu-id="8a38b-178">V důsledku toho hello dat musí nejprve přenesení do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="8a38b-178">Consequently, hello data must have been first transferred into blob storage.</span></span>

## <a name="optimizing-data-transfer"></a><span data-ttu-id="8a38b-179">Optimalizace přenos dat</span><span class="sxs-lookup"><span data-stu-id="8a38b-179">Optimizing data transfer</span></span>
<span data-ttu-id="8a38b-180">Jedna z částí nejpomalejší hello migrace dat je hello přenos dat tooAzure hello.</span><span class="sxs-lookup"><span data-stu-id="8a38b-180">One of hello slowest parts of data migration is hello transfer of hello data tooAzure.</span></span> <span data-ttu-id="8a38b-181">Pouze šířku pásma sítě může být problém, ale také spolehlivost sítě může být vážnou překážkou průběh.</span><span class="sxs-lookup"><span data-stu-id="8a38b-181">Not only can network bandwidth be an issue but also network reliability can seriously hamper progress.</span></span> <span data-ttu-id="8a38b-182">Ve výchozím nastavení je tooAzure přenášení dat přes internet proto hello případné chyby přenos, ke kterým dochází přiměřeně pravděpodobně hello.</span><span class="sxs-lookup"><span data-stu-id="8a38b-182">By default migrating data tooAzure is over hello internet so hello chances of transfer errors occurring are reasonably likely.</span></span> <span data-ttu-id="8a38b-183">Tyto chyby však může vyžadovat opětovné odeslání celé nebo částečně toobe data.</span><span class="sxs-lookup"><span data-stu-id="8a38b-183">However, these errors may require data toobe re-sent either in whole or in part.</span></span>

<span data-ttu-id="8a38b-184">Naštěstí máte několik možností tooimprove hello rychlost a pružnost tohoto procesu:</span><span class="sxs-lookup"><span data-stu-id="8a38b-184">Fortunately you have several options tooimprove hello speed and resilience of this process:</span></span>

### <a name="expressrouteexpressroute"></a><span data-ttu-id="8a38b-185">[ExpressRoute][ExpressRoute]</span><span class="sxs-lookup"><span data-stu-id="8a38b-185">[ExpressRoute][ExpressRoute]</span></span>
<span data-ttu-id="8a38b-186">Může být vhodné pomocí tooconsider [ExpressRoute] [ ExpressRoute] toospeed až hello přenosu.</span><span class="sxs-lookup"><span data-stu-id="8a38b-186">You may want tooconsider using [ExpressRoute][ExpressRoute] toospeed up hello transfer.</span></span> <span data-ttu-id="8a38b-187">[ExpressRoute] [ ExpressRoute] poskytuje vám tooAzure vytvořeného privátní připojení tak hello připojení nejde přes hello veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="8a38b-187">[ExpressRoute][ExpressRoute] provides you with an established private connection tooAzure so hello connection does not go over hello public internet.</span></span> <span data-ttu-id="8a38b-188">Toto je rozhodně není povinný krok.</span><span class="sxs-lookup"><span data-stu-id="8a38b-188">This is by no means a mandatory step.</span></span> <span data-ttu-id="8a38b-189">Ale ho bude při nabízení data tooAzure z místního zlepšit propustnost nebo společném umístění.</span><span class="sxs-lookup"><span data-stu-id="8a38b-189">However, it will improve throughput when pushing data tooAzure from an on-premises or co-location facility.</span></span>

<span data-ttu-id="8a38b-190">Hello výhody použití [ExpressRoute] [ ExpressRoute] jsou:</span><span class="sxs-lookup"><span data-stu-id="8a38b-190">hello benefits of using [ExpressRoute][ExpressRoute] are:</span></span>

1. <span data-ttu-id="8a38b-191">Větší spolehlivost</span><span class="sxs-lookup"><span data-stu-id="8a38b-191">Increased reliability</span></span>
2. <span data-ttu-id="8a38b-192">Vyšší rychlost sítě</span><span class="sxs-lookup"><span data-stu-id="8a38b-192">Faster network speed</span></span>
3. <span data-ttu-id="8a38b-193">Nižší latenci sítě</span><span class="sxs-lookup"><span data-stu-id="8a38b-193">Lower network latency</span></span>
4. <span data-ttu-id="8a38b-194">vyšší zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="8a38b-194">higher network security</span></span>

<span data-ttu-id="8a38b-195">[ExpressRoute] [ ExpressRoute] je výhodné pro různé scénáře, ne jenom hello migrace.</span><span class="sxs-lookup"><span data-stu-id="8a38b-195">[ExpressRoute][ExpressRoute] is beneficial for a number of scenarios; not just hello migration.</span></span>

<span data-ttu-id="8a38b-196">Zajímá vás to?</span><span class="sxs-lookup"><span data-stu-id="8a38b-196">Interested?</span></span> <span data-ttu-id="8a38b-197">Další informace a ceny prosím naleznete hello [dokumentace ExpressRoute][ExpressRoute documentation].</span><span class="sxs-lookup"><span data-stu-id="8a38b-197">For more information and pricing please visit hello [ExpressRoute documentation][ExpressRoute documentation].</span></span>

### <a name="azure-import-and-export-service"></a><span data-ttu-id="8a38b-198">Azure Import a Export služby</span><span class="sxs-lookup"><span data-stu-id="8a38b-198">Azure Import and Export Service</span></span>
<span data-ttu-id="8a38b-199">Hello Azure Import a Export služby je proces přenosu dat určená pro velké (GB ++) toomassive (TB ++) přenosů dat do Azure.</span><span class="sxs-lookup"><span data-stu-id="8a38b-199">hello Azure Import and Export Service is a data transfer process designed for large (GB++) toomassive (TB++) transfers of data into Azure.</span></span> <span data-ttu-id="8a38b-200">Se týká psaní toodisks vaše data a jejich přesouvání tooan datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="8a38b-200">It involves writing your data toodisks and shipping them tooan Azure data center.</span></span> <span data-ttu-id="8a38b-201">obsah disku Hello budou načteny potom do objektů BLOB služby Azure Storage vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="8a38b-201">hello disk contents will then be loaded into Azure Storage Blobs on your behalf.</span></span>

<span data-ttu-id="8a38b-202">Souhrnné zobrazení procesu exportu importovat hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="8a38b-202">A high-level view of hello import export process is as follows:</span></span>

1. <span data-ttu-id="8a38b-203">Konfigurace Azure Blob Storage kontejneru tooreceive hello dat</span><span class="sxs-lookup"><span data-stu-id="8a38b-203">Configure an Azure Blob Storage container tooreceive hello data</span></span>
2. <span data-ttu-id="8a38b-204">Export toolocal úložiště dat</span><span class="sxs-lookup"><span data-stu-id="8a38b-204">Export your data toolocal storage</span></span>
3. <span data-ttu-id="8a38b-205">Zkopírujte hello data too3.5 palec SATA II/III jednotky pevného disku pomocí hello [Azure Import/Export nástroj]</span><span class="sxs-lookup"><span data-stu-id="8a38b-205">Copy hello data too3.5 inch SATA II/III hard disk drives using hello [Azure Import/Export Tool]</span></span>
4. <span data-ttu-id="8a38b-206">Vytvoření úlohy importu pomocí hello Azure Import a Export služba poskytující soubory deníku hello vyprodukované hello [Azure Import/Export nástroj]</span><span class="sxs-lookup"><span data-stu-id="8a38b-206">Create an Import Job using hello Azure Import and Export Service providing hello journal files produced by hello [Azure Import/Export Tool]</span></span>
5. <span data-ttu-id="8a38b-207">Dodávat hello disky určenou Azure datové centrum</span><span class="sxs-lookup"><span data-stu-id="8a38b-207">Ship hello disks your nominated Azure data center</span></span>
6. <span data-ttu-id="8a38b-208">Vaše data jsou přenášená tooyour kontejneru Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="8a38b-208">Your data is transferred tooyour Azure Blob Storage container</span></span>
7. <span data-ttu-id="8a38b-209">Načtení dat hello do SQLDW pomocí PolyBase</span><span class="sxs-lookup"><span data-stu-id="8a38b-209">Load hello data into SQLDW using PolyBase</span></span>

### <a name="azcopyazcopy-utility"></a><span data-ttu-id="8a38b-210">[AZCopy][AZCopy] nástroj</span><span class="sxs-lookup"><span data-stu-id="8a38b-210">[AZCopy][AZCopy] utility</span></span>
<span data-ttu-id="8a38b-211">Hello [AZCopy][AZCopy] nástroj je skvělý nástroj pro získávání dat přenesených do objektů BLOB služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="8a38b-211">hello [AZCopy][AZCopy] utility is a great tool for getting your data transferred into Azure Storage Blobs.</span></span> <span data-ttu-id="8a38b-212">Je určený pro malé (MB ++) toovery velké (GB ++) přenosy dat.</span><span class="sxs-lookup"><span data-stu-id="8a38b-212">It is designed for small (MB++) toovery large (GB++) data transfers.</span></span> <span data-ttu-id="8a38b-213">[AZCopy] byl také navrženou tooprovide dobrý odolné propustnost při přenosu dat tooAzure a tak je, že je služba skvělou volbou pro krok přenos dat hello.</span><span class="sxs-lookup"><span data-stu-id="8a38b-213">[AZCopy] has also been designed tooprovide good resilient throughput when transferring data tooAzure and so is a great choice for hello data transfer step.</span></span> <span data-ttu-id="8a38b-214">Jednou přenášená načtením hello dat do SQL Data Warehouse pomocí PolyBase.</span><span class="sxs-lookup"><span data-stu-id="8a38b-214">Once transferred you can load hello data using PolyBase into SQL Data Warehouse.</span></span> <span data-ttu-id="8a38b-215">AZCopy taky můžete začlenit do vaší služby SSIS balíčky pomocí "Spustit proces" úlohy.</span><span class="sxs-lookup"><span data-stu-id="8a38b-215">You can also incorporate AZCopy into your SSIS packages using an "Execute Process" task.</span></span>

<span data-ttu-id="8a38b-216">toouse AZCopy, bude nejprve nutné toodownload a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="8a38b-216">toouse AZCopy you will first need toodownload and install it.</span></span> <span data-ttu-id="8a38b-217">Je [produkční verzi] [ production version] a [verze preview] [ preview version] k dispozici.</span><span class="sxs-lookup"><span data-stu-id="8a38b-217">There is a [production version][production version] and a [preview version][preview version] available.</span></span>

<span data-ttu-id="8a38b-218">tooupload soubor ze systému souborů, které budete potřebovat příkaz jako hello jeden níže:</span><span class="sxs-lookup"><span data-stu-id="8a38b-218">tooupload a file from your file system you will need a command like hello one below:</span></span>

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="8a38b-219">Proces vysoké úrovně souhrn může být:</span><span class="sxs-lookup"><span data-stu-id="8a38b-219">A high-level process summary could be:</span></span>

1. <span data-ttu-id="8a38b-220">Konfigurace úložiště Azure blob kontejneru tooreceive hello dat</span><span class="sxs-lookup"><span data-stu-id="8a38b-220">Configure an Azure storage blob container tooreceive hello data</span></span>
2. <span data-ttu-id="8a38b-221">Export toolocal úložiště dat</span><span class="sxs-lookup"><span data-stu-id="8a38b-221">Export your data toolocal storage</span></span>
3. <span data-ttu-id="8a38b-222">AZCopy daty v kontejneru Azure Blob Storage hello</span><span class="sxs-lookup"><span data-stu-id="8a38b-222">AZCopy your data in hello Azure Blob Storage container</span></span>
4. <span data-ttu-id="8a38b-223">Načtení hello dat do SQL Data Warehouse pomocí PolyBase</span><span class="sxs-lookup"><span data-stu-id="8a38b-223">Load hello data into SQL Data Warehouse using PolyBase</span></span>

<span data-ttu-id="8a38b-224">Úplné dokumentaci k dispozici: [AZCopy][AZCopy].</span><span class="sxs-lookup"><span data-stu-id="8a38b-224">Full documentation available: [AZCopy][AZCopy].</span></span>

## <a name="optimizing-data-export"></a><span data-ttu-id="8a38b-225">Optimalizace export dat</span><span class="sxs-lookup"><span data-stu-id="8a38b-225">Optimizing data export</span></span>
<span data-ttu-id="8a38b-226">Tooensuring, hello export vyhovuje toohello požadavky definované polybase můžete kromě toho můžete hledat toooptimize hello export proces hello tooimprove hello dat další.</span><span class="sxs-lookup"><span data-stu-id="8a38b-226">In addition tooensuring that hello export conforms toohello requirements laid out by PolyBase you can also seek toooptimize hello export of hello data tooimprove hello process further.</span></span>



### <a name="data-compression"></a><span data-ttu-id="8a38b-227">Komprese dat</span><span class="sxs-lookup"><span data-stu-id="8a38b-227">Data compression</span></span>
<span data-ttu-id="8a38b-228">PolyBase čte data komprimované gzip.</span><span class="sxs-lookup"><span data-stu-id="8a38b-228">PolyBase can read gzip compressed data.</span></span> <span data-ttu-id="8a38b-229">Pokud jsou možné toocompress datové soubory toogzip pak můžete minimalizovat hello množství dat, které se instaluje přes síť hello.</span><span class="sxs-lookup"><span data-stu-id="8a38b-229">If you are able toocompress your data toogzip files then you will minimize hello amount of data being pushed over hello network.</span></span>

### <a name="multiple-files"></a><span data-ttu-id="8a38b-230">Více souborů</span><span class="sxs-lookup"><span data-stu-id="8a38b-230">Multiple files</span></span>
<span data-ttu-id="8a38b-231">Rozdělení rozsáhlé tabulky do několika souborů umožňuje nejenom tooimprove exportovat rychlost, také pomáhá s přenosu re-startability a hello celkové možnosti správy dat hello jednou v úložišti objektů blob Azure hello.</span><span class="sxs-lookup"><span data-stu-id="8a38b-231">Breaking up large tables into several files not only helps tooimprove export speed, it also helps with transfer re-startability, and hello overall manageability of hello data once in hello Azure blob storage.</span></span> <span data-ttu-id="8a38b-232">Jeden z hello mnoho dobrý funkce PolyBase je, že bude číst všechny hello soubory do složky a s nimi zacházet jako jedna tabulka.</span><span class="sxs-lookup"><span data-stu-id="8a38b-232">One of hello many nice features of PolyBase is that it will read all hello files inside a folder and treat it as one table.</span></span> <span data-ttu-id="8a38b-233">Proto je vhodné tooisolate hello souborů pro každou tabulku do vlastní složky.</span><span class="sxs-lookup"><span data-stu-id="8a38b-233">It is therefore a good idea tooisolate hello files for each table into its own folder.</span></span>

<span data-ttu-id="8a38b-234">PolyBase také podporuje funkci označuje jako "rekurzivní složky traversal".</span><span class="sxs-lookup"><span data-stu-id="8a38b-234">PolyBase also supports a feature known as "recursive folder traversal".</span></span> <span data-ttu-id="8a38b-235">Tuto funkci můžete používat toofurther vylepšit hello organizace vaší tooimprove exportovaná data vaší dat správy.</span><span class="sxs-lookup"><span data-stu-id="8a38b-235">You can use this feature toofurther enhance hello organization of your exported data tooimprove your data management.</span></span>

<span data-ttu-id="8a38b-236">toolearn Další informace o načtení dat pomocí funkce PolyBase, najdete v části [použijte PolyBase tooload dat do SQL Data Warehouse][Use PolyBase tooload data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="8a38b-236">toolearn more about loading data with PolyBase, see [Use PolyBase tooload data into SQL Data Warehouse][Use PolyBase tooload data into SQL Data Warehouse].</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a38b-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a38b-237">Next steps</span></span>
<span data-ttu-id="8a38b-238">Další informace o migraci najdete v tématu [migrace vašeho řešení tooSQL datového skladu][Migrate your solution tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="8a38b-238">For more about migration, see [Migrate your solution tooSQL Data Warehouse][Migrate your solution tooSQL Data Warehouse].</span></span>
<span data-ttu-id="8a38b-239">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="8a38b-239">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[AZCopy]:../storage/common/storage-use-azcopy.md
[ADF Copy]: ../data-factory/data-factory-data-movement-activities.md 
[ADF samples]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[development overview]: sql-data-warehouse-overview-develop.md
[Migrate your solution tooSQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Use bcp tooload data into SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[Use PolyBase tooload data into SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute documentation]: http://azure.microsoft.com/documentation/services/expressroute/

[production version]: http://aka.ms/downloadazcopy/
[preview version]: http://aka.ms/downloadazcopypr/
[ADO.NET destination adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS documentation]: https://msdn.microsoft.com/library/ms141026.aspx
