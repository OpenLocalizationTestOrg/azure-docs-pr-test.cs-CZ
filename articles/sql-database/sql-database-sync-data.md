---
title: Synchronizaci dat (Preview) | Microsoft Docs
description: "Tento přehled zavádí synchronizaci dat SQL Azure (Preview)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: douglasl
ms.openlocfilehash: 926938a8ed20167e1f17a9883007cd993897f14a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync"></a><span data-ttu-id="ca27c-103">Synchronizaci dat mezi několika databází cloudu a místně s synchronizaci dat SQL</span><span class="sxs-lookup"><span data-stu-id="ca27c-103">Sync data across multiple cloud and on-premises databases with SQL Data Sync</span></span>

<span data-ttu-id="ca27c-104">Synchronizaci dat SQL je služba založená na Azure SQL Database, která umožňuje synchronizaci dat, kterou vyberete obousměrně napříč více databází SQL a instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ca27c-104">SQL Data Sync is a service built on Azure SQL Database that lets you synchronize the data you select bi-directionally across multiple SQL databases and SQL Server instances.</span></span>

<span data-ttu-id="ca27c-105">Synchronizace dat je založena kolem koncept synchronizace skupiny.</span><span class="sxs-lookup"><span data-stu-id="ca27c-105">Data Sync is based around the concept of a Sync Group.</span></span> <span data-ttu-id="ca27c-106">Synchronizace skupiny je skupina databází, které chcete synchronizovat.</span><span class="sxs-lookup"><span data-stu-id="ca27c-106">A Sync Group is a group of databases that you want to synchronize.</span></span>

<span data-ttu-id="ca27c-107">Synchronizace skupiny má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="ca27c-107">A Sync Group has the following properties:</span></span>

-   <span data-ttu-id="ca27c-108">**Schématu synchronizace** popisuje data, která se synchronizuje.</span><span class="sxs-lookup"><span data-stu-id="ca27c-108">The **Sync Schema** describes which data is being synchronized.</span></span>

-   <span data-ttu-id="ca27c-109">**Směr synchronizace** může být obousměrně nebo můžete pouze v jednom směru toku.</span><span class="sxs-lookup"><span data-stu-id="ca27c-109">The **Sync Direction** can be bi-directional or can flow in only one direction.</span></span> <span data-ttu-id="ca27c-110">To znamená, může být směr synchronizace *rozbočovače na člen* nebo *člena do centra*, nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="ca27c-110">That is, the Sync Direction can be *Hub to Member* or *Member to Hub*, or both.</span></span>

-   <span data-ttu-id="ca27c-111">**Interval synchronizace** jak často dochází k synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="ca27c-111">The **Sync Interval** is how often synchronization occurs.</span></span>

-   <span data-ttu-id="ca27c-112">**Zásady překladu IP adres konflikt** je úrovni zásady skupiny, který může být *rozbočovače wins* nebo *člen wins*.</span><span class="sxs-lookup"><span data-stu-id="ca27c-112">The **Conflict Resolution Policy** is a group level policy, which can be *Hub wins* or *Member wins*.</span></span>

<span data-ttu-id="ca27c-113">Synchronizaci dat používá k synchronizaci dat hvězdicové topologii.</span><span class="sxs-lookup"><span data-stu-id="ca27c-113">Data Sync uses a hub and spoke topology to synchronize data.</span></span> <span data-ttu-id="ca27c-114">Definujte jedna z databází ve skupině jako databázi rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="ca27c-114">You define one of the databases in the group as the Hub Database.</span></span> <span data-ttu-id="ca27c-115">Zbytek databáze jsou databází člena.</span><span class="sxs-lookup"><span data-stu-id="ca27c-115">The rest of the databases are member databases.</span></span> <span data-ttu-id="ca27c-116">Synchronizace nastávají jenom mezi rozbočovače a jednotlivé členy.</span><span class="sxs-lookup"><span data-stu-id="ca27c-116">Sync occurs only between the Hub and individual members.</span></span>
-   <span data-ttu-id="ca27c-117">**Rozbočovače databáze** musí být databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="ca27c-117">The **Hub Database** must be an Azure SQL Database.</span></span>
-   <span data-ttu-id="ca27c-118">**Databází člena** může být databáze SQL, databáze systému SQL Server pro místní nebo instance systému SQL Server na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="ca27c-118">The **member databases** can be either SQL Databases, on-premises SQL Server databases, or SQL Server instances on Azure virtual machines.</span></span>
-   <span data-ttu-id="ca27c-119">**Databáze Sync** obsahuje metadata a protokolu pro synchronizaci dat. Synchronizace databáze musí být, že Azure SQL Database umístěný ve stejné oblasti jako databázi rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="ca27c-119">The **Sync Database** contains the metadata and log for Data Sync. The Sync Database has to be an Azure SQL Database located in the same region as the Hub Database.</span></span> <span data-ttu-id="ca27c-120">Databáze Sync je zákazník vytvořit a zákazníka, které jsou ve vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="ca27c-120">The Sync Database is customer created and customer owned.</span></span>

> [!NOTE]
> <span data-ttu-id="ca27c-121">Pokud používáte databázi na místní, budete muset [konfigurace místního agenta.](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-sql-data-sync)</span><span class="sxs-lookup"><span data-stu-id="ca27c-121">If you're using an on premises database, you have to [configure a local agent.](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-sql-data-sync)</span></span>

![Synchronizaci dat mezi databázemi](media/sql-database-sync-data/sync-data-overview.png)

## <a name="when-to-use-data-sync"></a><span data-ttu-id="ca27c-123">Kdy používat synchronizaci dat</span><span class="sxs-lookup"><span data-stu-id="ca27c-123">When to use Data Sync</span></span>

<span data-ttu-id="ca27c-124">Synchronizace dat je užitečné v případech, kde data musí být pořád aktuální napříč několika databází SQL Azure nebo databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ca27c-124">Data Sync is useful in cases where data needs to be kept up to date across several Azure SQL Databases or SQL Server databases.</span></span> <span data-ttu-id="ca27c-125">Zde jsou nejčastěji se využívá případů pro synchronizaci dat:</span><span class="sxs-lookup"><span data-stu-id="ca27c-125">Here are the main use cases for Data Sync:</span></span>

-   <span data-ttu-id="ca27c-126">**Synchronizace dat hybridní:** se synchronizací dat, abyste zajistili, že data synchronizovat mezi vaší místní databáze a databáze SQL Azure povolit hybridní aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca27c-126">**Hybrid Data Synchronization:** With Data Sync, you can keep data synchronized between your on-premises databases and Azure SQL Databases to enable hybrid applications.</span></span> <span data-ttu-id="ca27c-127">Tato funkce může odvolat pro zákazníky, kteří jsou s přechodu do cloudu a chcete uvést některé své aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="ca27c-127">This capability may appeal to customers who are considering moving to the cloud and would like to put some of their application in Azure.</span></span>

-   <span data-ttu-id="ca27c-128">**Distribuované aplikace:** v mnoha případech je vhodné k rozdělení různé úlohy do různých databází.</span><span class="sxs-lookup"><span data-stu-id="ca27c-128">**Distributed Applications:** In many cases, it's beneficial to separate different workloads across different databases.</span></span> <span data-ttu-id="ca27c-129">Například pokud máte velké provozní databáze, ale také muset spustit úlohy vytváření sestav nebo analýza tato data, je vhodné mít druhý databázi pro tento další úlohy.</span><span class="sxs-lookup"><span data-stu-id="ca27c-129">For example, if you have a large production database, but you also need to run a reporting or analytics workload on this data, it's helpful to have a second database for this additional workload.</span></span> <span data-ttu-id="ca27c-130">Tento postup minimalizuje dopad na výkon na vaše produkční úlohy.</span><span class="sxs-lookup"><span data-stu-id="ca27c-130">This approach minimizes the performance impact on your production workload.</span></span> <span data-ttu-id="ca27c-131">Můžete používat synchronizaci dat aby tyto dva databáze synchronizovány.</span><span class="sxs-lookup"><span data-stu-id="ca27c-131">You can use Data Sync to keep these two databases synchronized.</span></span>

-   <span data-ttu-id="ca27c-132">**Globálně distribuované aplikace:** mnoho firem rozmístěny v několika oblastech a i několika zemích.</span><span class="sxs-lookup"><span data-stu-id="ca27c-132">**Globally Distributed Applications:** Many businesses span several regions and even several countries.</span></span> <span data-ttu-id="ca27c-133">Chcete-li minimalizovat latence sítě, je vhodné vaše data v oblasti blízko vás.</span><span class="sxs-lookup"><span data-stu-id="ca27c-133">To minimize network latency, it's best to have your data in a region close to you.</span></span> <span data-ttu-id="ca27c-134">S synchronizaci dat se snadnou vejdou databází v oblastech po celém světě synchronizovány.</span><span class="sxs-lookup"><span data-stu-id="ca27c-134">With Data Sync, you can easily keep databases in regions around the world synchronized.</span></span>

<span data-ttu-id="ca27c-135">Není doporučeno synchronizaci dat pro následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="ca27c-135">We don't recommend Data Sync for the following scenarios:</span></span>

-   <span data-ttu-id="ca27c-136">Zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="ca27c-136">Disaster Recovery</span></span>

-   <span data-ttu-id="ca27c-137">Škálování pro čtení</span><span class="sxs-lookup"><span data-stu-id="ca27c-137">Read Scale</span></span>

-   <span data-ttu-id="ca27c-138">ETL (OLTP na OLAP)</span><span class="sxs-lookup"><span data-stu-id="ca27c-138">ETL (OLTP to OLAP)</span></span>

-   <span data-ttu-id="ca27c-139">Migrace ze systému SQL Server pro místní databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="ca27c-139">Migration from on-premises SQL Server to Azure SQL Database</span></span>

## <a name="how-does-data-sync-work"></a><span data-ttu-id="ca27c-140">Jak funguje synchronizaci dat?</span><span class="sxs-lookup"><span data-stu-id="ca27c-140">How does Data Sync work?</span></span> 

-   <span data-ttu-id="ca27c-141">**Sledování změn dat:** synchronizaci dat sleduje změny pomocí vložit, aktualizovat a odstranit aktivační události.</span><span class="sxs-lookup"><span data-stu-id="ca27c-141">**Tracking data changes:** Data Sync tracks changes using insert, update, and delete triggers.</span></span> <span data-ttu-id="ca27c-142">Změny se zaznamenávají v tabulce straně v uživatelské databázi.</span><span class="sxs-lookup"><span data-stu-id="ca27c-142">The changes are recorded in a side table in the user database.</span></span>

-   <span data-ttu-id="ca27c-143">**Synchronizace dat:** synchronizaci dat slouží v hvězdicové modelu.</span><span class="sxs-lookup"><span data-stu-id="ca27c-143">**Synchronizing data:** Data Sync is designed in a Hub and Spoke model.</span></span> <span data-ttu-id="ca27c-144">Rozbočovače synchronizuje s každý člen jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="ca27c-144">The Hub syncs with each member individually.</span></span> <span data-ttu-id="ca27c-145">Změny z centra, se stáhnou do člen a pak změny z člen odeslány do rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="ca27c-145">Changes from the Hub are downloaded to the member and then changes from the member are uploaded to the Hub.</span></span>

-   <span data-ttu-id="ca27c-146">**Řešení konfliktů:** synchronizaci dat nabízí dvě možnosti pro řešení konfliktů *rozbočovače wins* nebo *člen wins*.</span><span class="sxs-lookup"><span data-stu-id="ca27c-146">**Resolving conflicts:** Data Sync provides two options for conflict resolution, *Hub wins* or *Member wins*.</span></span>
    -   <span data-ttu-id="ca27c-147">Pokud vyberete *rozbočovače wins*, změny v centru vždy přepsat změny v člena.</span><span class="sxs-lookup"><span data-stu-id="ca27c-147">If you select *Hub wins*, the changes in the hub always overwrite changes in the member.</span></span>
    -   <span data-ttu-id="ca27c-148">Pokud vyberete *člen wins*, změny v změn přepsání člena v centru.</span><span class="sxs-lookup"><span data-stu-id="ca27c-148">If you select *Member wins*, the changes in the member overwrite changes in the hub.</span></span> <span data-ttu-id="ca27c-149">Pokud existuje více než jednoho člena, konečná hodnota závisí na člena, který synchronizuje nejdřív.</span><span class="sxs-lookup"><span data-stu-id="ca27c-149">If there's more than one member, the final value depends on which member syncs first.</span></span>

## <a name="limitations-and-considerations"></a><span data-ttu-id="ca27c-150">Omezení a důležité informace</span><span class="sxs-lookup"><span data-stu-id="ca27c-150">Limitations and considerations</span></span>

### <a name="performance-impact"></a><span data-ttu-id="ca27c-151">Vliv na výkon</span><span class="sxs-lookup"><span data-stu-id="ca27c-151">Performance impact</span></span>
<span data-ttu-id="ca27c-152">Synchronizace dat se používají vložit, aktualizovat a odstranit aktivačních událostí ke sledování změn.</span><span class="sxs-lookup"><span data-stu-id="ca27c-152">Data Sync uses insert, update, and delete triggers to track changes.</span></span> <span data-ttu-id="ca27c-153">Vytvoří straně tabulky v databázi uživatelů pro sledování změn.</span><span class="sxs-lookup"><span data-stu-id="ca27c-153">It creates side tables in the user database for change tracking.</span></span> <span data-ttu-id="ca27c-154">Tyto aktivity sledování změn mají vliv na vaše zatížení databáze.</span><span class="sxs-lookup"><span data-stu-id="ca27c-154">These change tracking activities have an impact on your database workload.</span></span> <span data-ttu-id="ca27c-155">Vyhodnocení vašeho vrstvy služeb a upgradujte v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="ca27c-155">Assess your service tier and upgrade if needed.</span></span>

### <a name="eventual-consistency"></a><span data-ttu-id="ca27c-156">Konzistence typu případné</span><span class="sxs-lookup"><span data-stu-id="ca27c-156">Eventual consistency</span></span>
<span data-ttu-id="ca27c-157">Vzhledem k tomu, že synchronizace dat je na základě aktivační události, není zaručena konzistence transakcí.</span><span class="sxs-lookup"><span data-stu-id="ca27c-157">Since Data Sync is trigger-based, transactional consistency is not guaranteed.</span></span> <span data-ttu-id="ca27c-158">Microsoft zaručuje, že jsou všechny změny provedené nakonec a synchronizaci dat není způsobit ztrátu dat..</span><span class="sxs-lookup"><span data-stu-id="ca27c-158">Microsoft guarantees that all changes are made eventually and that Data Sync does not cause data loss.</span></span>

### <a name="unsupported-data-types"></a><span data-ttu-id="ca27c-159">Nepodporované datové typy</span><span class="sxs-lookup"><span data-stu-id="ca27c-159">Unsupported data types</span></span>

-   <span data-ttu-id="ca27c-160">FileStream</span><span class="sxs-lookup"><span data-stu-id="ca27c-160">FileStream</span></span>

-   <span data-ttu-id="ca27c-161">UDT SQL/CLR</span><span class="sxs-lookup"><span data-stu-id="ca27c-161">SQL/CLR UDT</span></span>

-   <span data-ttu-id="ca27c-162">Kolekci XMLSchemaCollection (XML podporována)</span><span class="sxs-lookup"><span data-stu-id="ca27c-162">XMLSchemaCollection (XML supported)</span></span>

-   <span data-ttu-id="ca27c-163">Kurzor, časové razítko, Hierarchyid</span><span class="sxs-lookup"><span data-stu-id="ca27c-163">Cursor, Timestamp, Hierarchyid</span></span>

### <a name="requirements"></a><span data-ttu-id="ca27c-164">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ca27c-164">Requirements</span></span>

-   <span data-ttu-id="ca27c-165">Každá tabulka musí mít primární klíč.</span><span class="sxs-lookup"><span data-stu-id="ca27c-165">Each table must have a primary key.</span></span>

-   <span data-ttu-id="ca27c-166">Tabulka nemůže obsahovat sloupec identity, který není primární klíč.</span><span class="sxs-lookup"><span data-stu-id="ca27c-166">A table cannot have an identity column that is not the primary key.</span></span>

-   <span data-ttu-id="ca27c-167">Názvy objektů (databáze, tabulek a sloupců) nesmí obsahovat tisknutelná znaků tečkou (.), zbývající hranaté závorky ([]), nebo právo hranatá závorka (]).</span><span class="sxs-lookup"><span data-stu-id="ca27c-167">The names of objects (databases, tables, and columns) cannot contain the printable characters period (.), left square bracket ([), or right square bracket (]).</span></span>

### <a name="limitations-on-service-and-database-dimensions"></a><span data-ttu-id="ca27c-168">Omezení dimenzí služby a databáze</span><span class="sxs-lookup"><span data-stu-id="ca27c-168">Limitations on service and database dimensions</span></span>

|                                                                 |                        |                             |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| <span data-ttu-id="ca27c-169">**Dimenze**</span><span class="sxs-lookup"><span data-stu-id="ca27c-169">**Dimensions**</span></span>                                                      | <span data-ttu-id="ca27c-170">**Limit**</span><span class="sxs-lookup"><span data-stu-id="ca27c-170">**Limit**</span></span>              | <span data-ttu-id="ca27c-171">**Alternativní řešení**</span><span class="sxs-lookup"><span data-stu-id="ca27c-171">**Workaround**</span></span>              |
| <span data-ttu-id="ca27c-172">Maximální počet skupin synchronizace všechny databáze může patřit do.</span><span class="sxs-lookup"><span data-stu-id="ca27c-172">Maximum number of sync groups any database can belong to.</span></span>       | <span data-ttu-id="ca27c-173">5</span><span class="sxs-lookup"><span data-stu-id="ca27c-173">5</span></span>                      |                             |
| <span data-ttu-id="ca27c-174">Maximální počet koncových bodů ve skupině jedním synchronizačním</span><span class="sxs-lookup"><span data-stu-id="ca27c-174">Maximum number of endpoints in a single sync group</span></span>              | <span data-ttu-id="ca27c-175">30</span><span class="sxs-lookup"><span data-stu-id="ca27c-175">30</span></span>                     | <span data-ttu-id="ca27c-176">Vytvoření více skupin synchronizace</span><span class="sxs-lookup"><span data-stu-id="ca27c-176">Create multiple sync groups</span></span> |
| <span data-ttu-id="ca27c-177">Maximální počet koncových bodů místně v jednom synchronizace skupiny.</span><span class="sxs-lookup"><span data-stu-id="ca27c-177">Maximum number of on-premises endpoints in a single sync group.</span></span> | <span data-ttu-id="ca27c-178">5</span><span class="sxs-lookup"><span data-stu-id="ca27c-178">5</span></span>                      | <span data-ttu-id="ca27c-179">Vytvoření více skupin synchronizace</span><span class="sxs-lookup"><span data-stu-id="ca27c-179">Create multiple sync groups</span></span> |
| <span data-ttu-id="ca27c-180">Databáze, tabulky, schémat a sloupce</span><span class="sxs-lookup"><span data-stu-id="ca27c-180">Database, table, schema, and column names</span></span>                       | <span data-ttu-id="ca27c-181">50 znaků na název</span><span class="sxs-lookup"><span data-stu-id="ca27c-181">50 characters per name</span></span> |                             |
| <span data-ttu-id="ca27c-182">Tabulky ve skupině pro synchronizaci</span><span class="sxs-lookup"><span data-stu-id="ca27c-182">Tables in a sync group</span></span>                                          | <span data-ttu-id="ca27c-183">500</span><span class="sxs-lookup"><span data-stu-id="ca27c-183">500</span></span>                    | <span data-ttu-id="ca27c-184">Vytvoření více skupin synchronizace</span><span class="sxs-lookup"><span data-stu-id="ca27c-184">Create multiple sync groups</span></span> |
| <span data-ttu-id="ca27c-185">Sloupce v tabulce v skupiny synchronizace</span><span class="sxs-lookup"><span data-stu-id="ca27c-185">Columns in a table in a sync group</span></span>                              | <span data-ttu-id="ca27c-186">1000</span><span class="sxs-lookup"><span data-stu-id="ca27c-186">1000</span></span>                   |                             |
| <span data-ttu-id="ca27c-187">Velikost řádek dat v tabulce</span><span class="sxs-lookup"><span data-stu-id="ca27c-187">Data row size on a table</span></span>                                        | <span data-ttu-id="ca27c-188">24 mb</span><span class="sxs-lookup"><span data-stu-id="ca27c-188">24 Mb</span></span>                  |                             |
| <span data-ttu-id="ca27c-189">Interval minimální synchronizace</span><span class="sxs-lookup"><span data-stu-id="ca27c-189">Minimum sync interval</span></span>                                           | <span data-ttu-id="ca27c-190">5 minut</span><span class="sxs-lookup"><span data-stu-id="ca27c-190">5 Minutes</span></span>              |                             |

## <a name="common-questions"></a><span data-ttu-id="ca27c-191">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="ca27c-191">Common questions</span></span>

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a><span data-ttu-id="ca27c-192">Jak často můžete synchronizaci dat synchronizovat data?</span><span class="sxs-lookup"><span data-stu-id="ca27c-192">How frequently can Data Sync synchronize my data?</span></span> 
<span data-ttu-id="ca27c-193">Minimální frekvence se každých pět minut.</span><span class="sxs-lookup"><span data-stu-id="ca27c-193">The minimum frequency is every five minutes.</span></span>

### <a name="can-i-use-data-sync-to-sync-between-sql-server-on-premises-databases-only"></a><span data-ttu-id="ca27c-194">Můžete používat synchronizaci dat k synchronizaci mezi místní databáze systému SQL Server pouze?</span><span class="sxs-lookup"><span data-stu-id="ca27c-194">Can I use Data Sync to sync between SQL Server on-premises databases only?</span></span> 
<span data-ttu-id="ca27c-195">Ne přímo.</span><span class="sxs-lookup"><span data-stu-id="ca27c-195">Not directly.</span></span> <span data-ttu-id="ca27c-196">Pro synchronizaci mezi místní databáze systému SQL Server nepřímo, ale vytvoření centra databáze v Azure a následným přidáním do skupiny synchronizace místní databáze.</span><span class="sxs-lookup"><span data-stu-id="ca27c-196">You can sync between SQL Server on-premises databases indirectly, however, by creating a Hub database in Azure, and then adding the on-premises databases to the sync group.</span></span>
   
### <a name="can-i-use-data-sync-to-seed-data-from-my-production-database-to-an-empty-database-and-then-keep-them-synchronized"></a><span data-ttu-id="ca27c-197">Lze používat synchronizaci dat na počáteční hodnoty data z provozní databáze pro prázdnou databázi a pak je synchronizovat zachovat?</span><span class="sxs-lookup"><span data-stu-id="ca27c-197">Can I use Data Sync to seed data from my production database to an empty database, and then keep them synchronized?</span></span> 
<span data-ttu-id="ca27c-198">Ano.</span><span class="sxs-lookup"><span data-stu-id="ca27c-198">Yes.</span></span> <span data-ttu-id="ca27c-199">Vytvoření schématu ručně v nové databáze pomocí skriptování z původní.</span><span class="sxs-lookup"><span data-stu-id="ca27c-199">Create the schema manually in the new database by scripting it from the original.</span></span> <span data-ttu-id="ca27c-200">Jakmile vytvoříte schéma, přidáte do skupiny synchronizace zkopírovat data a udržovat synchronizované tabulky.</span><span class="sxs-lookup"><span data-stu-id="ca27c-200">After you create the schema, add the tables to a sync group to copy the data and keep it synced.</span></span>

### <a name="why-do-i-see-tables-that-i-did-not-create"></a><span data-ttu-id="ca27c-201">Proč se zobrazuje, tabulky, které I nevytvořila?</span><span class="sxs-lookup"><span data-stu-id="ca27c-201">Why do I see tables that I did not create?</span></span>  
<span data-ttu-id="ca27c-202">Synchronizaci dat vytváří straně tabulky v databázi pro sledování změn.</span><span class="sxs-lookup"><span data-stu-id="ca27c-202">Data Sync creates side tables in your database for change tracking.</span></span> <span data-ttu-id="ca27c-203">Neodstraňujte ho nebo synchronizaci dat přestane fungovat.</span><span class="sxs-lookup"><span data-stu-id="ca27c-203">Don't delete them or Data Sync stops working.</span></span>
   
### <a name="i-got-an-error-message-that-said-cannot-insert-the-value-null-into-the-column-column-column-does-not-allow-nulls-what-does-this-mean-and-how-can-i-fix-the-error"></a><span data-ttu-id="ca27c-204">Chybová zpráva, která je uvedená "nelze vložit hodnoty NULL do sloupce \<sloupec\>.</span><span class="sxs-lookup"><span data-stu-id="ca27c-204">I got an error message that said "cannot insert the value NULL into the column \<column\>.</span></span> <span data-ttu-id="ca27c-205">Sloupec nepovoluje hodnoty Null."</span><span class="sxs-lookup"><span data-stu-id="ca27c-205">Column does not allow nulls."</span></span> <span data-ttu-id="ca27c-206">Co to znamená, a jak můžete opravit chyby?</span><span class="sxs-lookup"><span data-stu-id="ca27c-206">What does this mean, and how can I fix the error?</span></span> 
<span data-ttu-id="ca27c-207">Tato chybová zpráva indikuje mezi dvěma následující problémy:</span><span class="sxs-lookup"><span data-stu-id="ca27c-207">This error message indicates one of the two following issues:</span></span>
1.  <span data-ttu-id="ca27c-208">Může být tabulky bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="ca27c-208">There may be a table without a primary key.</span></span> <span data-ttu-id="ca27c-209">Chcete-li tento problém vyřešit, přidejte primární klíč pro všechny tabulky, které se synchronizuje.</span><span class="sxs-lookup"><span data-stu-id="ca27c-209">To fix this issue, add a primary key to all the tables you're syncing.</span></span>
2.  <span data-ttu-id="ca27c-210">Může být klauzule WHERE v příkazu CREATE INDEX.</span><span class="sxs-lookup"><span data-stu-id="ca27c-210">There may be a WHERE clause in your CREATE INDEX statement.</span></span> <span data-ttu-id="ca27c-211">Synchronizace nezpracovává tuto podmínku.</span><span class="sxs-lookup"><span data-stu-id="ca27c-211">Sync does not handle this condition.</span></span> <span data-ttu-id="ca27c-212">Chcete-li tento problém vyřešit, odeberte klauzuli WHERE nebo ručně změnit všechny databáze.</span><span class="sxs-lookup"><span data-stu-id="ca27c-212">To fix this issue, remove the WHERE clause or manually make the changes to all databases.</span></span> 
 
### <a name="how-does-data-sync-handle-circular-references-that-is-when-the-same-data-is-synced-in-multiple-sync-groups-and-keeps-changing-as-a-result"></a><span data-ttu-id="ca27c-213">Jak synchronizaci dat zpracovat. cyklické odkazy?</span><span class="sxs-lookup"><span data-stu-id="ca27c-213">How does Data Sync handle circular references?</span></span> <span data-ttu-id="ca27c-214">To znamená, když stejná data je synchronizovaný v několika skupinách pro synchronizaci a udržuje v důsledku změny?</span><span class="sxs-lookup"><span data-stu-id="ca27c-214">That is, when the same data is synced in multiple sync groups, and keeps changing as a result?</span></span>
<span data-ttu-id="ca27c-215">Synchronizaci dat nemůže pracovat. cyklické odkazy.</span><span class="sxs-lookup"><span data-stu-id="ca27c-215">Data Sync doesn’t handle circular references.</span></span> <span data-ttu-id="ca27c-216">Ujistěte se, že je vyhnout.</span><span class="sxs-lookup"><span data-stu-id="ca27c-216">Be sure to avoid them.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="ca27c-217">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ca27c-217">Next steps</span></span>

<span data-ttu-id="ca27c-218">Další informace o synchronizaci dat SQL najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="ca27c-218">For more info about SQL Data Sync, see:</span></span>

-   [<span data-ttu-id="ca27c-219">Začínáme s synchronizaci dat SQL</span><span class="sxs-lookup"><span data-stu-id="ca27c-219">Getting Started with SQL Data Sync</span></span>](sql-database-get-started-sql-data-sync.md)

-   <span data-ttu-id="ca27c-220">Dokončete příklady prostředí PowerShell, které ukazují, jak nakonfigurovat synchronizaci dat SQL:</span><span class="sxs-lookup"><span data-stu-id="ca27c-220">Complete PowerShell examples that show how to configure SQL Data Sync:</span></span>
    -   [<span data-ttu-id="ca27c-221">Pomocí prostředí PowerShell k synchronizaci mezi více databází Azure SQL</span><span class="sxs-lookup"><span data-stu-id="ca27c-221">Use PowerShell to sync between multiple Azure SQL databases</span></span>](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [<span data-ttu-id="ca27c-222">Synchronizace mezi databáze SQL Azure a místní databáze SQL serveru pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ca27c-222">Use PowerShell to sync between an Azure SQL Database and a SQL Server on-premises database</span></span>](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [<span data-ttu-id="ca27c-223">Stáhněte si úplnou synchronizaci dat SQL technická dokumentace</span><span class="sxs-lookup"><span data-stu-id="ca27c-223">Download the complete SQL Data Sync technical documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)

-   [<span data-ttu-id="ca27c-224">Stáhněte si dokumentaci rozhraní API REST synchronizaci dat SQL</span><span class="sxs-lookup"><span data-stu-id="ca27c-224">Download the SQL Data Sync REST API documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

<span data-ttu-id="ca27c-225">Další informace o databázi SQL najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="ca27c-225">For more info about SQL Database, see:</span></span>

-   [<span data-ttu-id="ca27c-226">Databáze SQL – přehled</span><span class="sxs-lookup"><span data-stu-id="ca27c-226">SQL Database Overview</span></span>](sql-database-technical-overview.md)

-   [<span data-ttu-id="ca27c-227">Správa životního cyklu databáze</span><span class="sxs-lookup"><span data-stu-id="ca27c-227">Database Lifecycle Management</span></span>](https://msdn.microsoft.com/library/jj907294.aspx)
