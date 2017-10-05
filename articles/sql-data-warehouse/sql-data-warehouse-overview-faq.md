---
title: "Nejčastější dotazy k Azure SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: 4c00710ecc0c91f8407eca81b78176075fcbd6ad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sql-data-warehouse-frequently-asked-questions"></a><span data-ttu-id="fe39b-103">SQL Data Warehouse nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="fe39b-103">SQL Data Warehouse Frequently asked questions</span></span>

## <a name="general"></a><span data-ttu-id="fe39b-104">Obecné</span><span class="sxs-lookup"><span data-stu-id="fe39b-104">General</span></span>

<span data-ttu-id="fe39b-105">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="fe39b-105">Q.</span></span> <span data-ttu-id="fe39b-106">Co nabízí SQL DW zabezpečení dat?</span><span class="sxs-lookup"><span data-stu-id="fe39b-106">What does SQL DW offer for data security?</span></span>

<span data-ttu-id="fe39b-107">A.</span><span class="sxs-lookup"><span data-stu-id="fe39b-107">A.</span></span> <span data-ttu-id="fe39b-108">SQL DW nabízí několik řešení pro ochranu dat, jako je například šifrování TDE a auditování.</span><span class="sxs-lookup"><span data-stu-id="fe39b-108">SQL DW offers several solutions for protecting data such as TDE and auditing.</span></span> <span data-ttu-id="fe39b-109">Další informace najdete v tématu [zabezpečení].</span><span class="sxs-lookup"><span data-stu-id="fe39b-109">For more information, see [Security].</span></span>

<span data-ttu-id="fe39b-110">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="fe39b-110">Q.</span></span> <span data-ttu-id="fe39b-111">Kde lze zjistit SQL datového skladu je jaké právní či obchodní normy kompatibilní s?</span><span class="sxs-lookup"><span data-stu-id="fe39b-111">Where can I find out what legal or business standards is SQL DW compliant with?</span></span>

<span data-ttu-id="fe39b-112">A.</span><span class="sxs-lookup"><span data-stu-id="fe39b-112">A.</span></span> <span data-ttu-id="fe39b-113">Přejděte [Microsoft Compliance] stránky pro různé nabídky dodržování předpisů produktu, jako je například SOC a ISO.</span><span class="sxs-lookup"><span data-stu-id="fe39b-113">Visit the [Microsoft Compliance] page for various compliance offerings by product such as SOC and ISO.</span></span> <span data-ttu-id="fe39b-114">Nejdřív vyberte podle názvu dodržování předpisů, pak rozbalte Azure v části služby Microsoft cloud v oboru na pravé straně stránky uvidíte, jaké služby jsou Azure jsou služby kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="fe39b-114">First choose by Compliance title, then expand Azure in the Microsoft in-scope cloud services section on the right side of the page to see what services are Azure services are compliant.</span></span>

<span data-ttu-id="fe39b-115">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="fe39b-115">Q.</span></span> <span data-ttu-id="fe39b-116">Můžete připojit PowerBI?</span><span class="sxs-lookup"><span data-stu-id="fe39b-116">Can I connect PowerBI?</span></span>

<span data-ttu-id="fe39b-117">A.</span><span class="sxs-lookup"><span data-stu-id="fe39b-117">A.</span></span> <span data-ttu-id="fe39b-118">Ano!</span><span class="sxs-lookup"><span data-stu-id="fe39b-118">Yes!</span></span> <span data-ttu-id="fe39b-119">Když PowerBI podporuje přímý dotaz s datovým Skladem SQL, není určena pro velké množství uživatelů nebo dat v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="fe39b-119">Though PowerBI supports direct query with SQL DW, it’s not intended for large number of users or real-time data.</span></span> <span data-ttu-id="fe39b-120">Pro produkční použití PowerBI doporučujeme používat PowerBI na Azure Analysis Services nebo na Analysis Service IaaS.</span><span class="sxs-lookup"><span data-stu-id="fe39b-120">For production use of PowerBI, we recommend using PowerBI on top of Azure Analysis Services or Analysis Service IaaS.</span></span> 

<span data-ttu-id="fe39b-121">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="fe39b-121">Q.</span></span> <span data-ttu-id="fe39b-122">Jaké jsou limity SQL Data Warehouse kapacity?</span><span class="sxs-lookup"><span data-stu-id="fe39b-122">What are SQL Data Warehouse Capacity Limits?</span></span>

<span data-ttu-id="fe39b-123">A.</span><span class="sxs-lookup"><span data-stu-id="fe39b-123">A.</span></span> <span data-ttu-id="fe39b-124">V tématu naše aktuální [limity kapacity] stránky.</span><span class="sxs-lookup"><span data-stu-id="fe39b-124">See our current [capacity limits] page.</span></span> 

<span data-ttu-id="fe39b-125">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="fe39b-125">Q.</span></span> <span data-ttu-id="fe39b-126">Proč můj škálování nebo pozastavení nebo obnovení trvá tak dlouho?</span><span class="sxs-lookup"><span data-stu-id="fe39b-126">Why is my Scale/Pause/Resume taking so long?</span></span>

<span data-ttu-id="fe39b-127">A.</span><span class="sxs-lookup"><span data-stu-id="fe39b-127">A.</span></span> <span data-ttu-id="fe39b-128">Různé faktory mohou mít vliv na dobu výpočetní operace správy.</span><span class="sxs-lookup"><span data-stu-id="fe39b-128">A variety of factors can influence the time for compute management operations.</span></span> <span data-ttu-id="fe39b-129">Společné případ dlouho běžící operace je vrácení transakcí zpět.</span><span class="sxs-lookup"><span data-stu-id="fe39b-129">A common case for  long running operations is transactional rollback.</span></span> <span data-ttu-id="fe39b-130">Při zahájení operace škálování nebo pozastavení blokovány všechny příchozí relace a jsou nečekaně dotazy.</span><span class="sxs-lookup"><span data-stu-id="fe39b-130">When a scale or pause operation is initiated, all incoming sessions are blocked and queries are drained.</span></span> <span data-ttu-id="fe39b-131">Aby bylo možné ponechat systém stabilní, musí transakce vrácena zpět před započetím operace.</span><span class="sxs-lookup"><span data-stu-id="fe39b-131">In order to leave the system in a stable state, transactions must be rolled back before an operation can commence.</span></span> <span data-ttu-id="fe39b-132">Větší počet a větší velikost protokolu transakcí, tím déle operaci bude být zastaven a proces obnovení systému stabilního stavu.</span><span class="sxs-lookup"><span data-stu-id="fe39b-132">The greater the number and larger the log size of transactions, the longer the operation will be stalled restoring the system to a stable state.</span></span>

## <a name="user-support"></a><span data-ttu-id="fe39b-133">Podpora uživatelů</span><span class="sxs-lookup"><span data-stu-id="fe39b-133">User support</span></span>

<span data-ttu-id="fe39b-134">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="fe39b-134">Q.</span></span> <span data-ttu-id="fe39b-135">Jsou žádosti o funkci, kde ji odeslat?</span><span class="sxs-lookup"><span data-stu-id="fe39b-135">I have a feature request, where do I submit it?</span></span>

<span data-ttu-id="fe39b-136">A.</span><span class="sxs-lookup"><span data-stu-id="fe39b-136">A.</span></span> <span data-ttu-id="fe39b-137">Pokud máte žádosti o funkci, odešlete ji na našem [UserVoice] stránky</span><span class="sxs-lookup"><span data-stu-id="fe39b-137">If you have a feature request, submit it on our [UserVoice] page</span></span>

<span data-ttu-id="fe39b-138">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="fe39b-138">Q.</span></span> <span data-ttu-id="fe39b-139">Jak můžete provést x?</span><span class="sxs-lookup"><span data-stu-id="fe39b-139">How can I do x?</span></span>

<span data-ttu-id="fe39b-140">A.</span><span class="sxs-lookup"><span data-stu-id="fe39b-140">A.</span></span> <span data-ttu-id="fe39b-141">Pomoc při vývoji s SQL Data Warehouse, můžete klást otázky na našem [Stack Overflow] stránky.</span><span class="sxs-lookup"><span data-stu-id="fe39b-141">For help in developing with SQL Data Warehouse, you can ask questions on our [Stack Overflow] page.</span></span> 

<span data-ttu-id="fe39b-142">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="fe39b-142">Q.</span></span> <span data-ttu-id="fe39b-143">Jak mohu odeslat lístek podpory?</span><span class="sxs-lookup"><span data-stu-id="fe39b-143">How do I submit a support ticket?</span></span>

<span data-ttu-id="fe39b-144">A.</span><span class="sxs-lookup"><span data-stu-id="fe39b-144">A.</span></span> <span data-ttu-id="fe39b-145">[Lístky podpory] podává prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fe39b-145">[Support Tickets] can be filed through Azure portal.</span></span>

## <a name="sql-languagefeature-support"></a><span data-ttu-id="fe39b-146">Podpora jazyka nebo funkce serveru SQL</span><span class="sxs-lookup"><span data-stu-id="fe39b-146">SQL language/feature support</span></span> 

<span data-ttu-id="fe39b-147">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="fe39b-147">Q.</span></span> <span data-ttu-id="fe39b-148">Jaké datové typy SQL Data Warehouse podporuje?</span><span class="sxs-lookup"><span data-stu-id="fe39b-148">What datatypes does SQL Data Warehouse support?</span></span>

<span data-ttu-id="fe39b-149">A.</span><span class="sxs-lookup"><span data-stu-id="fe39b-149">A.</span></span> <span data-ttu-id="fe39b-150">Najdete v části SQL Data Warehouse [datové typy].</span><span class="sxs-lookup"><span data-stu-id="fe39b-150">See SQL Data Warehouse [data types].</span></span>

<span data-ttu-id="fe39b-151">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="fe39b-151">Q.</span></span> <span data-ttu-id="fe39b-152">Jaké funkce tabulky podporujete?</span><span class="sxs-lookup"><span data-stu-id="fe39b-152">What table features do you support?</span></span>

<span data-ttu-id="fe39b-153">A.</span><span class="sxs-lookup"><span data-stu-id="fe39b-153">A.</span></span> <span data-ttu-id="fe39b-154">I když SQL Data Warehouse podporuje mnoho funkcí, některé nejsou podporovány a jsou dokumentovány v článku [nepodporované funkce tabulky].</span><span class="sxs-lookup"><span data-stu-id="fe39b-154">While SQL Data Warehouse supports many features, some are not supported and are documented in [Unsupported Table Features].</span></span>

## <a name="tooling-and-administration"></a><span data-ttu-id="fe39b-155">Nástroje a Správa</span><span class="sxs-lookup"><span data-stu-id="fe39b-155">Tooling and administration</span></span>

<span data-ttu-id="fe39b-156">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="fe39b-156">Q.</span></span> <span data-ttu-id="fe39b-157">Podporujete databázové projekty v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fe39b-157">Do you support Database projects in Visual Studio.</span></span>

<span data-ttu-id="fe39b-158">A.</span><span class="sxs-lookup"><span data-stu-id="fe39b-158">A.</span></span> <span data-ttu-id="fe39b-159">Aktuálně nepodporujeme databázové projekty v sadě Visual Studio pro SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="fe39b-159">We currently do not support Database projects in Visual Studio for SQL Data Warehouse.</span></span> <span data-ttu-id="fe39b-160">Pokud chcete převést hlas získat tuto funkci, navštivte naše User Voice [databázové projekty funkci požadavku].</span><span class="sxs-lookup"><span data-stu-id="fe39b-160">If you'd like to cast a vote to get this feature, visit our User Voice [Database projects feature request].</span></span>

<span data-ttu-id="fe39b-161">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="fe39b-161">Q.</span></span> <span data-ttu-id="fe39b-162">Podporuje SQL Data Warehouse rozhraní REST API?</span><span class="sxs-lookup"><span data-stu-id="fe39b-162">Does SQL Data Warehouse support REST APIs?</span></span>

<span data-ttu-id="fe39b-163">A.</span><span class="sxs-lookup"><span data-stu-id="fe39b-163">A.</span></span> <span data-ttu-id="fe39b-164">Ano.</span><span class="sxs-lookup"><span data-stu-id="fe39b-164">Yes.</span></span> <span data-ttu-id="fe39b-165">Většina funkcí REST, které lze použít s databází SQL je také dostupná v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="fe39b-165">Most REST functionality that can be used with SQL Database is also available with SQL Data Warehouse.</span></span> <span data-ttu-id="fe39b-166">Můžete najít informace o rozhraní API v rámci stránky dokumentace k REST nebo [MSDN].</span><span class="sxs-lookup"><span data-stu-id="fe39b-166">You can find API information within REST documentation pages or [MSDN].</span></span>


## <a name="loading"></a><span data-ttu-id="fe39b-167">Načítá se</span><span class="sxs-lookup"><span data-stu-id="fe39b-167">Loading</span></span>

<span data-ttu-id="fe39b-168">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="fe39b-168">Q.</span></span> <span data-ttu-id="fe39b-169">Jaké ovladače klienta podporujete?</span><span class="sxs-lookup"><span data-stu-id="fe39b-169">What client drivers do you support?</span></span>

<span data-ttu-id="fe39b-170">A.</span><span class="sxs-lookup"><span data-stu-id="fe39b-170">A.</span></span> <span data-ttu-id="fe39b-171">Podporu ovladačů pro datového skladu naleznete na [připojovací řetězce] stránky</span><span class="sxs-lookup"><span data-stu-id="fe39b-171">Driver support for DW can be found on the [Connection Strings] page</span></span>

<span data-ttu-id="fe39b-172">Otázka: jaký formáty souborů jsou podporovány službou SQL Data Warehouse polybase?</span><span class="sxs-lookup"><span data-stu-id="fe39b-172">Q: What file formats are supported by PolyBase with SQL Data Warehouse?</span></span>

<span data-ttu-id="fe39b-173">Odpověď: Orc, RC, Parquet a ploché odděleného textu</span><span class="sxs-lookup"><span data-stu-id="fe39b-173">A: Orc, RC, Parquet, and flat delimited text</span></span>

<span data-ttu-id="fe39b-174">Otázka: co je možné připojit k z datového skladu SQL pomocí PolyBase?</span><span class="sxs-lookup"><span data-stu-id="fe39b-174">Q: What can I connect to from SQL DW using PolyBase?</span></span> 

<span data-ttu-id="fe39b-175">Odpověď: [Azure Data Lake Store] a [objektů BLOB služby Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="fe39b-175">A: [Azure Data Lake Store] and [Azure Storage Blobs]</span></span>

<span data-ttu-id="fe39b-176">Otázka: je přenos směrem dolů výpočetní možné při připojení k Azure Storage Blobs nebo ADLS?</span><span class="sxs-lookup"><span data-stu-id="fe39b-176">Q: Is computation pushdown possible  when connecting to Azure Storage Blobs or ADLS?</span></span> 

<span data-ttu-id="fe39b-177">A: PolyBase datového skladu SQL Ne, komunikuje pouze komponenty úložiště.</span><span class="sxs-lookup"><span data-stu-id="fe39b-177">A: No, SQL DW PolyBase only interacts the storage components.</span></span> 

<span data-ttu-id="fe39b-178">Otázka: je možné připojit k HDI?</span><span class="sxs-lookup"><span data-stu-id="fe39b-178">Q: Can I connect to HDI?</span></span>

<span data-ttu-id="fe39b-179">Odpověď: HDI můžete použít buď ADLS nebo WASB jako vrstva HDFS.</span><span class="sxs-lookup"><span data-stu-id="fe39b-179">A: HDI can use either ADLS or WASB as the HDFS layer.</span></span> <span data-ttu-id="fe39b-180">Pokud máte buď jako HDFS vrstvě, můžete načíst data do datového skladu SQL.</span><span class="sxs-lookup"><span data-stu-id="fe39b-180">If you have either as your HDFS layer, then you can load that data into SQL DW.</span></span> <span data-ttu-id="fe39b-181">Však nelze vygenerovat výpočetní přenos směrem dolů k instanci HDI.</span><span class="sxs-lookup"><span data-stu-id="fe39b-181">However, you cannot generate pushdown computation to the HDI instance.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="fe39b-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe39b-182">Next steps</span></span>
<span data-ttu-id="fe39b-183">Další informace o SQL Data Warehouse jako celku najdete v tématu naše [přehled] stránky.</span><span class="sxs-lookup"><span data-stu-id="fe39b-183">For more information on SQL Data Warehouse as a whole, see our [Overview] page.</span></span>


<!-- Article references -->
<span data-ttu-id="fe39b-184">[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse</span><span class="sxs-lookup"><span data-stu-id="fe39b-184">[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse</span></span>
<span data-ttu-id="fe39b-185">[připojovací řetězce]: ./sql-data-warehouse-connection-strings.md</span><span class="sxs-lookup"><span data-stu-id="fe39b-185">[Connection Strings]: ./sql-data-warehouse-connection-strings.md</span></span>
<span data-ttu-id="fe39b-186">[Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw</span><span class="sxs-lookup"><span data-stu-id="fe39b-186">[Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw</span></span>
<span data-ttu-id="fe39b-187">[Lístky podpory]: ./sql-data-warehouse-get-started-create-support-ticket.md</span><span class="sxs-lookup"><span data-stu-id="fe39b-187">[Support Tickets]: ./sql-data-warehouse-get-started-create-support-ticket.md</span></span>
<span data-ttu-id="fe39b-188">[zabezpečení]: ./sql-data-warehouse-overview-manage-security.md</span><span class="sxs-lookup"><span data-stu-id="fe39b-188">[Security]: ./sql-data-warehouse-overview-manage-security.md</span></span>
<span data-ttu-id="fe39b-189">[Microsoft Compliance]: https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings</span><span class="sxs-lookup"><span data-stu-id="fe39b-189">[Microsoft Compliance]: https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings</span></span>
<span data-ttu-id="fe39b-190">[limity kapacity]: ./sql-data-warehouse-service-capacity-limits.md</span><span class="sxs-lookup"><span data-stu-id="fe39b-190">[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md</span></span>
<span data-ttu-id="fe39b-191">[datové typy]: ./sql-data-warehouse-tables-data-types.md</span><span class="sxs-lookup"><span data-stu-id="fe39b-191">[data types]: ./sql-data-warehouse-tables-data-types.md</span></span>
<span data-ttu-id="fe39b-192">[nepodporované funkce tabulky]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features</span><span class="sxs-lookup"><span data-stu-id="fe39b-192">[Unsupported Table Features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features</span></span>
<span data-ttu-id="fe39b-193">[Azure Data Lake Store]: ./sql-data-warehouse-load-from-azure-data-lake-store.md</span><span class="sxs-lookup"><span data-stu-id="fe39b-193">[Azure Data Lake Store]: ./sql-data-warehouse-load-from-azure-data-lake-store.md</span></span>
<span data-ttu-id="fe39b-194">[objektů BLOB služby Azure Storage]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md</span><span class="sxs-lookup"><span data-stu-id="fe39b-194">[Azure Storage Blobs]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md</span></span>
<span data-ttu-id="fe39b-195">[databázové projekty funkci požadavku]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu</span><span class="sxs-lookup"><span data-stu-id="fe39b-195">[Database projects feature request]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu</span></span>
<span data-ttu-id="fe39b-196">[MSDN]: https://msdn.microsoft.com/en-us/library/azure/mt163685.aspx</span><span class="sxs-lookup"><span data-stu-id="fe39b-196">[MSDN]: https://msdn.microsoft.com/en-us/library/azure/mt163685.aspx</span></span>
<span data-ttu-id="fe39b-197">[přehled]: ./sql-data-warehouse-overview-faq.md</span><span class="sxs-lookup"><span data-stu-id="fe39b-197">[Overview]: ./sql-data-warehouse-overview-faq.md</span></span>