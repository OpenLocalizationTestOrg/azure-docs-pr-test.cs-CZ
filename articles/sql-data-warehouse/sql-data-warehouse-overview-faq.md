---
title: "aaaAzure SQL Data Warehouse – nejčastější dotazy | Microsoft Docs"
description: "Tento článek obsahuje seznam si nejčastější dotazy k Azure SQL Data Warehouse od zákazníků a vývojářů"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: 812CA525-3BF3-49DF-8DF3-FB4342464F4F
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 3/1/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: 09fd3f65d9507b09fcb8f477742c7d020add2755
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-frequently-asked-questions"></a><span data-ttu-id="3687e-103">SQL Data Warehouse nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="3687e-103">SQL Data Warehouse Frequently asked questions</span></span>

## <a name="general"></a><span data-ttu-id="3687e-104">Obecné</span><span class="sxs-lookup"><span data-stu-id="3687e-104">General</span></span>

<span data-ttu-id="3687e-105">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="3687e-105">Q.</span></span> <span data-ttu-id="3687e-106">Co nabízí SQL DW zabezpečení dat?</span><span class="sxs-lookup"><span data-stu-id="3687e-106">What does SQL DW offer for data security?</span></span>

<span data-ttu-id="3687e-107">A.</span><span class="sxs-lookup"><span data-stu-id="3687e-107">A.</span></span> <span data-ttu-id="3687e-108">SQL DW nabízí několik řešení pro ochranu dat, jako je například šifrování TDE a auditování.</span><span class="sxs-lookup"><span data-stu-id="3687e-108">SQL DW offers several solutions for protecting data such as TDE and auditing.</span></span> <span data-ttu-id="3687e-109">Další informace najdete v tématu [zabezpečení].</span><span class="sxs-lookup"><span data-stu-id="3687e-109">For more information, see [Security].</span></span>

<span data-ttu-id="3687e-110">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="3687e-110">Q.</span></span> <span data-ttu-id="3687e-111">Kde lze zjistit SQL datového skladu je jaké právní či obchodní normy kompatibilní s?</span><span class="sxs-lookup"><span data-stu-id="3687e-111">Where can I find out what legal or business standards is SQL DW compliant with?</span></span>

<span data-ttu-id="3687e-112">A.</span><span class="sxs-lookup"><span data-stu-id="3687e-112">A.</span></span> <span data-ttu-id="3687e-113">Navštivte hello [Microsoft Compliance] stránky pro různé nabídky dodržování předpisů produktu, jako je například SOC a ISO.</span><span class="sxs-lookup"><span data-stu-id="3687e-113">Visit hello [Microsoft Compliance] page for various compliance offerings by product such as SOC and ISO.</span></span> <span data-ttu-id="3687e-114">Nejdřív vyberte podle názvu dodržování předpisů, pak rozbalte Azure v hello Microsoftu v oboru cloudu služby oddílu na pravé straně hello toosee stránku hello jaké služby jsou služby Azure, jsou kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="3687e-114">First choose by Compliance title, then expand Azure in hello Microsoft in-scope cloud services section on hello right side of hello page toosee what services are Azure services are compliant.</span></span>

<span data-ttu-id="3687e-115">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="3687e-115">Q.</span></span> <span data-ttu-id="3687e-116">Můžete připojit PowerBI?</span><span class="sxs-lookup"><span data-stu-id="3687e-116">Can I connect PowerBI?</span></span>

<span data-ttu-id="3687e-117">A.</span><span class="sxs-lookup"><span data-stu-id="3687e-117">A.</span></span> <span data-ttu-id="3687e-118">Ano!</span><span class="sxs-lookup"><span data-stu-id="3687e-118">Yes!</span></span> <span data-ttu-id="3687e-119">Když PowerBI podporuje přímý dotaz s datovým Skladem SQL, není určena pro velké množství uživatelů nebo dat v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="3687e-119">Though PowerBI supports direct query with SQL DW, it’s not intended for large number of users or real-time data.</span></span> <span data-ttu-id="3687e-120">Pro produkční použití PowerBI doporučujeme používat PowerBI na Azure Analysis Services nebo na Analysis Service IaaS.</span><span class="sxs-lookup"><span data-stu-id="3687e-120">For production use of PowerBI, we recommend using PowerBI on top of Azure Analysis Services or Analysis Service IaaS.</span></span> 

<span data-ttu-id="3687e-121">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="3687e-121">Q.</span></span> <span data-ttu-id="3687e-122">Jaké jsou limity SQL Data Warehouse kapacity?</span><span class="sxs-lookup"><span data-stu-id="3687e-122">What are SQL Data Warehouse Capacity Limits?</span></span>

<span data-ttu-id="3687e-123">A.</span><span class="sxs-lookup"><span data-stu-id="3687e-123">A.</span></span> <span data-ttu-id="3687e-124">V tématu naše aktuální [limity kapacity] stránky.</span><span class="sxs-lookup"><span data-stu-id="3687e-124">See our current [capacity limits] page.</span></span> 

<span data-ttu-id="3687e-125">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="3687e-125">Q.</span></span> <span data-ttu-id="3687e-126">Proč můj škálování nebo pozastavení nebo obnovení trvá tak dlouho?</span><span class="sxs-lookup"><span data-stu-id="3687e-126">Why is my Scale/Pause/Resume taking so long?</span></span>

<span data-ttu-id="3687e-127">A.</span><span class="sxs-lookup"><span data-stu-id="3687e-127">A.</span></span> <span data-ttu-id="3687e-128">Různé faktory mohou mít vliv na hello dobu výpočetní operace správy.</span><span class="sxs-lookup"><span data-stu-id="3687e-128">A variety of factors can influence hello time for compute management operations.</span></span> <span data-ttu-id="3687e-129">Společné případ dlouho běžící operace je vrácení transakcí zpět.</span><span class="sxs-lookup"><span data-stu-id="3687e-129">A common case for  long running operations is transactional rollback.</span></span> <span data-ttu-id="3687e-130">Při zahájení operace škálování nebo pozastavení blokovány všechny příchozí relace a jsou nečekaně dotazy.</span><span class="sxs-lookup"><span data-stu-id="3687e-130">When a scale or pause operation is initiated, all incoming sessions are blocked and queries are drained.</span></span> <span data-ttu-id="3687e-131">Transakce v pořadí tooleave hello systém stabilní, musí být vrácena zpět před započetím operace.</span><span class="sxs-lookup"><span data-stu-id="3687e-131">In order tooleave hello system in a stable state, transactions must be rolled back before an operation can commence.</span></span> <span data-ttu-id="3687e-132">Dobrý den větší počet hello a větší velikost hello protokolu transakcí, hello delší hello operace bude zastaveno obnovení hello systému tooa stabilního stavu.</span><span class="sxs-lookup"><span data-stu-id="3687e-132">hello greater hello number and larger hello log size of transactions, hello longer hello operation will be stalled restoring hello system tooa stable state.</span></span>

## <a name="user-support"></a><span data-ttu-id="3687e-133">Podpora uživatelů</span><span class="sxs-lookup"><span data-stu-id="3687e-133">User support</span></span>

<span data-ttu-id="3687e-134">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="3687e-134">Q.</span></span> <span data-ttu-id="3687e-135">Jsou žádosti o funkci, kde ji odeslat?</span><span class="sxs-lookup"><span data-stu-id="3687e-135">I have a feature request, where do I submit it?</span></span>

<span data-ttu-id="3687e-136">A.</span><span class="sxs-lookup"><span data-stu-id="3687e-136">A.</span></span> <span data-ttu-id="3687e-137">Pokud máte žádosti o funkci, odešlete ji na našem [UserVoice] stránky</span><span class="sxs-lookup"><span data-stu-id="3687e-137">If you have a feature request, submit it on our [UserVoice] page</span></span>

<span data-ttu-id="3687e-138">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="3687e-138">Q.</span></span> <span data-ttu-id="3687e-139">Jak můžete provést x?</span><span class="sxs-lookup"><span data-stu-id="3687e-139">How can I do x?</span></span>

<span data-ttu-id="3687e-140">A.</span><span class="sxs-lookup"><span data-stu-id="3687e-140">A.</span></span> <span data-ttu-id="3687e-141">Pomoc při vývoji s SQL Data Warehouse, můžete klást otázky na našem [Stack Overflow] stránky.</span><span class="sxs-lookup"><span data-stu-id="3687e-141">For help in developing with SQL Data Warehouse, you can ask questions on our [Stack Overflow] page.</span></span> 

<span data-ttu-id="3687e-142">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="3687e-142">Q.</span></span> <span data-ttu-id="3687e-143">Jak mohu odeslat lístek podpory?</span><span class="sxs-lookup"><span data-stu-id="3687e-143">How do I submit a support ticket?</span></span>

<span data-ttu-id="3687e-144">A.</span><span class="sxs-lookup"><span data-stu-id="3687e-144">A.</span></span> <span data-ttu-id="3687e-145">[Lístky podpory] podává prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3687e-145">[Support Tickets] can be filed through Azure portal.</span></span>

## <a name="sql-languagefeature-support"></a><span data-ttu-id="3687e-146">Podpora jazyka nebo funkce serveru SQL</span><span class="sxs-lookup"><span data-stu-id="3687e-146">SQL language/feature support</span></span> 

<span data-ttu-id="3687e-147">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="3687e-147">Q.</span></span> <span data-ttu-id="3687e-148">Jaké datové typy SQL Data Warehouse podporuje?</span><span class="sxs-lookup"><span data-stu-id="3687e-148">What datatypes does SQL Data Warehouse support?</span></span>

<span data-ttu-id="3687e-149">A.</span><span class="sxs-lookup"><span data-stu-id="3687e-149">A.</span></span> <span data-ttu-id="3687e-150">Najdete v části SQL Data Warehouse [datové typy].</span><span class="sxs-lookup"><span data-stu-id="3687e-150">See SQL Data Warehouse [data types].</span></span>

<span data-ttu-id="3687e-151">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="3687e-151">Q.</span></span> <span data-ttu-id="3687e-152">Jaké funkce tabulky podporujete?</span><span class="sxs-lookup"><span data-stu-id="3687e-152">What table features do you support?</span></span>

<span data-ttu-id="3687e-153">A.</span><span class="sxs-lookup"><span data-stu-id="3687e-153">A.</span></span> <span data-ttu-id="3687e-154">I když SQL Data Warehouse podporuje mnoho funkcí, některé nejsou podporovány a jsou dokumentovány v článku [nepodporované funkce tabulky].</span><span class="sxs-lookup"><span data-stu-id="3687e-154">While SQL Data Warehouse supports many features, some are not supported and are documented in [Unsupported Table Features].</span></span>

## <a name="tooling-and-administration"></a><span data-ttu-id="3687e-155">Nástroje a Správa</span><span class="sxs-lookup"><span data-stu-id="3687e-155">Tooling and administration</span></span>

<span data-ttu-id="3687e-156">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="3687e-156">Q.</span></span> <span data-ttu-id="3687e-157">Podporujete databázové projekty v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3687e-157">Do you support Database projects in Visual Studio.</span></span>

<span data-ttu-id="3687e-158">A.</span><span class="sxs-lookup"><span data-stu-id="3687e-158">A.</span></span> <span data-ttu-id="3687e-159">Aktuálně nepodporujeme databázové projekty v sadě Visual Studio pro SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3687e-159">We currently do not support Database projects in Visual Studio for SQL Data Warehouse.</span></span> <span data-ttu-id="3687e-160">Pokud chcete toocast hlas tooget tuto funkci, navštivte naše User Voice [databázové projekty funkci požadavku].</span><span class="sxs-lookup"><span data-stu-id="3687e-160">If you'd like toocast a vote tooget this feature, visit our User Voice [Database projects feature request].</span></span>

<span data-ttu-id="3687e-161">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="3687e-161">Q.</span></span> <span data-ttu-id="3687e-162">Podporuje SQL Data Warehouse rozhraní REST API?</span><span class="sxs-lookup"><span data-stu-id="3687e-162">Does SQL Data Warehouse support REST APIs?</span></span>

<span data-ttu-id="3687e-163">A.</span><span class="sxs-lookup"><span data-stu-id="3687e-163">A.</span></span> <span data-ttu-id="3687e-164">Ano.</span><span class="sxs-lookup"><span data-stu-id="3687e-164">Yes.</span></span> <span data-ttu-id="3687e-165">Většina funkcí REST, které lze použít s databází SQL je také dostupná v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3687e-165">Most REST functionality that can be used with SQL Database is also available with SQL Data Warehouse.</span></span> <span data-ttu-id="3687e-166">Můžete najít informace o rozhraní API v rámci stránky dokumentace k REST nebo [MSDN].</span><span class="sxs-lookup"><span data-stu-id="3687e-166">You can find API information within REST documentation pages or [MSDN].</span></span>


## <a name="loading"></a><span data-ttu-id="3687e-167">Načítá se</span><span class="sxs-lookup"><span data-stu-id="3687e-167">Loading</span></span>

<span data-ttu-id="3687e-168">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="3687e-168">Q.</span></span> <span data-ttu-id="3687e-169">Jaké ovladače klienta podporujete?</span><span class="sxs-lookup"><span data-stu-id="3687e-169">What client drivers do you support?</span></span>

<span data-ttu-id="3687e-170">A.</span><span class="sxs-lookup"><span data-stu-id="3687e-170">A.</span></span> <span data-ttu-id="3687e-171">Podpora ovladačů pro datového skladu lze najít v hello [připojovací řetězce] stránky</span><span class="sxs-lookup"><span data-stu-id="3687e-171">Driver support for DW can be found on hello [Connection Strings] page</span></span>

<span data-ttu-id="3687e-172">Otázka: jaký formáty souborů jsou podporovány službou SQL Data Warehouse polybase?</span><span class="sxs-lookup"><span data-stu-id="3687e-172">Q: What file formats are supported by PolyBase with SQL Data Warehouse?</span></span>

<span data-ttu-id="3687e-173">Odpověď: Orc, RC, Parquet a ploché odděleného textu</span><span class="sxs-lookup"><span data-stu-id="3687e-173">A: Orc, RC, Parquet, and flat delimited text</span></span>

<span data-ttu-id="3687e-174">Otázka: co lze je možné připojit pomocí PolyBase toofrom SQL DW?</span><span class="sxs-lookup"><span data-stu-id="3687e-174">Q: What can I connect toofrom SQL DW using PolyBase?</span></span> 

<span data-ttu-id="3687e-175">Odpověď: [Azure Data Lake Store] a [objektů BLOB služby Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="3687e-175">A: [Azure Data Lake Store] and [Azure Storage Blobs]</span></span>

<span data-ttu-id="3687e-176">Otázka: je přenos směrem dolů výpočetní možné při připojování tooAzure úložiště objektů BLOB nebo ADLS?</span><span class="sxs-lookup"><span data-stu-id="3687e-176">Q: Is computation pushdown possible  when connecting tooAzure Storage Blobs or ADLS?</span></span> 

<span data-ttu-id="3687e-177">A: PolyBase datového skladu SQL Ne, komunikuje jenom hello úložiště součásti.</span><span class="sxs-lookup"><span data-stu-id="3687e-177">A: No, SQL DW PolyBase only interacts hello storage components.</span></span> 

<span data-ttu-id="3687e-178">Otázka: je možné připojit tooHDI?</span><span class="sxs-lookup"><span data-stu-id="3687e-178">Q: Can I connect tooHDI?</span></span>

<span data-ttu-id="3687e-179">Odpověď: HDI můžete použít buď ADLS nebo WASB jako vrstva HDFS hello.</span><span class="sxs-lookup"><span data-stu-id="3687e-179">A: HDI can use either ADLS or WASB as hello HDFS layer.</span></span> <span data-ttu-id="3687e-180">Pokud máte buď jako HDFS vrstvě, můžete načíst data do datového skladu SQL.</span><span class="sxs-lookup"><span data-stu-id="3687e-180">If you have either as your HDFS layer, then you can load that data into SQL DW.</span></span> <span data-ttu-id="3687e-181">Však nelze vygenerovat přenos směrem dolů výpočetní toohello HDI instance.</span><span class="sxs-lookup"><span data-stu-id="3687e-181">However, you cannot generate pushdown computation toohello HDI instance.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3687e-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3687e-182">Next steps</span></span>
<span data-ttu-id="3687e-183">Další informace o SQL Data Warehouse jako celku najdete v tématu naše [přehled] stránky.</span><span class="sxs-lookup"><span data-stu-id="3687e-183">For more information on SQL Data Warehouse as a whole, see our [Overview] page.</span></span>


<!-- Article references -->
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[připojovací řetězce]: ./sql-data-warehouse-connection-strings.md
[Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Lístky podpory]: ./sql-data-warehouse-get-started-create-support-ticket.md
[zabezpečení]: ./sql-data-warehouse-overview-manage-security.md
[Microsoft Compliance]: https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings
[limity kapacity]: ./sql-data-warehouse-service-capacity-limits.md
[datové typy]: ./sql-data-warehouse-tables-data-types.md
[nepodporované funkce tabulky]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Azure Data Lake Store]: ./sql-data-warehouse-load-from-azure-data-lake-store.md
[objektů BLOB služby Azure Storage]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[databázové projekty funkci požadavku]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
[MSDN]: https://msdn.microsoft.com/en-us/library/azure/mt163685.aspx
[přehled]: ./sql-data-warehouse-overview-faq.md