---
title: "Migrace: Datového skladu nástroje Migrace | Microsoft Docs"
description: Migrace do SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 4d7ad981-ef31-4513-b337-50bdc4709c09
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 2466e823c448ada4dc7bc5769b1b7f10bbb5dc7d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="data-warehouse-migration-utility-preview"></a><span data-ttu-id="3caa5-103">Nástroj migrace data Warehouse (Preview)</span><span class="sxs-lookup"><span data-stu-id="3caa5-103">Data Warehouse Migration Utility (Preview)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="3caa5-104">[Stáhněte si nástroj pro migraci][Download Migration Utility]</span><span class="sxs-lookup"><span data-stu-id="3caa5-104">[Download Migration Utility][Download Migration Utility]</span></span>
> 
> 

<span data-ttu-id="3caa5-105">Nástroj datového skladu migrace je nástroj sloužící k migraci schéma a data ze systému SQL Server a databáze SQL Azure do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3caa5-105">The Data Warehouse Migration Utility is a tool designed to migrate schema and data from SQL Server and Azure SQL Database to Azure SQL Data Warehouse.</span></span> <span data-ttu-id="3caa5-106">Během migrace schématu nástroj automaticky mapuje odpovídající schématu ze zdrojového do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="3caa5-106">During schema migration, the tool automatically maps the corresponding schema from source to destination.</span></span> <span data-ttu-id="3caa5-107">Po migraci schématu nástroje poskytuje možnost pro přesun dat se automaticky generovaných skriptů.</span><span class="sxs-lookup"><span data-stu-id="3caa5-107">After the schema has been migrated, the tools provides the option to move data with automatically generated scripts.</span></span>

<span data-ttu-id="3caa5-108">Kromě schéma a data migrace tento nástroj vám dává možnost k vygenerování sestavy kompatibility, které shrnují problémům s kompatibilitou mezi zdrojové a cílové instance, které by jinak znemožňovaly efektivní migrace.</span><span class="sxs-lookup"><span data-stu-id="3caa5-108">In addition to schema and data migration, this tool gives you the option to generate compatibility reports which summarize incompatibilities between the target and source instances which would prevent streamlined migration.</span></span>

## <a name="get-started"></a><span data-ttu-id="3caa5-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="3caa5-109">Get started</span></span>
<span data-ttu-id="3caa5-110">Předpokladem pro instalaci musíte nástroj příkazového řádku BCP spouštět skripty pro migraci a Office tak, aby zobrazení sestavy kompatibility.</span><span class="sxs-lookup"><span data-stu-id="3caa5-110">As a prerequisite for installation, you will need the BCP command-line utility to run migration scripts and Office to view the compatibility report.</span></span> <span data-ttu-id="3caa5-111">Po spuštění spustitelného souboru, který byl stažen se zobrazí výzva k přijmout standardní smlouvy EULA předtím, než se nainstaluje nástroj.</span><span class="sxs-lookup"><span data-stu-id="3caa5-111">After launching the executable that is downloaded you will be prompted to accept a standard EULA before the tool will be installed.</span></span>

<span data-ttu-id="3caa5-112">Kromě toho, pokud chcete spustit Utiliy migrace, budete potřebovat ten databáze, která pokud chcete migrovat následující oprávnění: vytvoření databáze, ALTER ANY DATABASE nebo ANY definice zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3caa5-112">In addition, to run the Migration Utiliy, you will need the one following permissions on the database that you are looking to migrate: CREATE DATABASE, ALTER ANY DATABASE or VIEW ANY DEFINITION.</span></span>

### <a name="launching-the-tool-and-connecting"></a><span data-ttu-id="3caa5-113">Spuštění nástroje a připojení</span><span class="sxs-lookup"><span data-stu-id="3caa5-113">Launching the tool and connecting</span></span>
<span data-ttu-id="3caa5-114">Spusťte nástroj kliknutím na ikonu plochy, která se zobrazí po instalaci.</span><span class="sxs-lookup"><span data-stu-id="3caa5-114">Launch the tool by clicking on the desktop icon which appears post install.</span></span> <span data-ttu-id="3caa5-115">Po otevření nástroje, zobrazí se výzva počáteční připojení stránky, kde si můžete vybrat zdroj a cíl pro nástroj pro migraci.</span><span class="sxs-lookup"><span data-stu-id="3caa5-115">Upon opening the tool, you will be prompted with an initial connection page where you can choose your source and destination for the migration tool.</span></span> <span data-ttu-id="3caa5-116">V současné době podporujeme systému SQL Server a Azure SQL Database jako zdroje a SQL Data Warehouse jako cíl.</span><span class="sxs-lookup"><span data-stu-id="3caa5-116">At this time, we support SQL Server and Azure SQL Database as sources and SQL Data Warehouse as a destination.</span></span> <span data-ttu-id="3caa5-117">Po vyberete tuto možnost se zobrazí výzva k připojení k serveru zdroj naplnění v názvu serveru a ověřování a potom klikněte na "Připojit".</span><span class="sxs-lookup"><span data-stu-id="3caa5-117">After selecting this you will be asked to connect to your source server by filling in server name and authenticating and then clicking ‘Connect’.</span></span>

<span data-ttu-id="3caa5-118">Po ověření, zobrazí nástroj seznam databází, které se nacházejí na serveru, které jste připojeni k.</span><span class="sxs-lookup"><span data-stu-id="3caa5-118">After authenticating, the tool will show a list of databases that are present in the server which you are connected to.</span></span> <span data-ttu-id="3caa5-119">Výběrem databázi, která chcete migrovat a kliknete na 'migrací vybraná, můžete začít migrace.</span><span class="sxs-lookup"><span data-stu-id="3caa5-119">You can begin the migration by selecting a database that you would like to migrate and then clicking on ‘Migrate selected’.</span></span>

## <a name="migration-report"></a><span data-ttu-id="3caa5-120">Sestavy migrace</span><span class="sxs-lookup"><span data-stu-id="3caa5-120">Migration report</span></span>
<span data-ttu-id="3caa5-121">Výběr zkontrolovat kompatibilitu databáze v nástroji bude vygenerovat sestavu shrnutí všech nekompatibility objektu v databázi, že požadujete k migraci.</span><span class="sxs-lookup"><span data-stu-id="3caa5-121">Selecting ‘Check Database Compatibility’ in the tool will generate a report summarizing all object incompatibilities in the database you requested to migrate.</span></span> <span data-ttu-id="3caa5-122">Širší seznam některých funkcí systému SQL Server, který se nenachází v SQL Data Warehouse najdete v našich [dokumentace k migraci][migration documentation].</span><span class="sxs-lookup"><span data-stu-id="3caa5-122">A broader list of some of the SQL Server functionality that is not present in SQL Data Warehouse can be found in our [migration documentation][migration documentation].</span></span> <span data-ttu-id="3caa5-123">Po vygenerování sestavy budou moci uložit a otevřete sestavu v aplikaci Excel.</span><span class="sxs-lookup"><span data-stu-id="3caa5-123">After the report is generated you will be able to save and open the report in Excel.</span></span>

<span data-ttu-id="3caa5-124">Pamatujte, že při generování schéma migrace identifikovány jako 'Objekt' se upraví, aby bylo možné povolit okamžitou migraci dat většiny problémů.</span><span class="sxs-lookup"><span data-stu-id="3caa5-124">Please note that when generating the migration schema, most issues identified as ‘Object’ will be adjusted in order to allow immediate migration of that data.</span></span> <span data-ttu-id="3caa5-125">Zkontrolujte změny Ujistěte se, že nechcete provádět další úpravy před použitím schématu.</span><span class="sxs-lookup"><span data-stu-id="3caa5-125">Please review the changes to ensure you do not want to make additional adjustments before applying the schema.</span></span>

## <a name="migrate-schema"></a><span data-ttu-id="3caa5-126">Migrace schématu</span><span class="sxs-lookup"><span data-stu-id="3caa5-126">Migrate schema</span></span>
<span data-ttu-id="3caa5-127">Po připojení, vygeneruje výběr 'migraci schématu skript pro migraci schématu pro vybrané tabulky.</span><span class="sxs-lookup"><span data-stu-id="3caa5-127">After connecting, selecting ‘Migrate Schema’ will generate a schema migration script for the selected tables.</span></span> <span data-ttu-id="3caa5-128">Tento skript porty struktura tabulky, nekompatibilní data mapy typy do více kompatibilní formulářů a vytvoří zabezpečovací přihlašovací údaje a schéma, pokud je to uvedeno uživatelem v nastavení migrace.</span><span class="sxs-lookup"><span data-stu-id="3caa5-128">This script ports the structure of the table, maps incompatible data types to more compatible forms, and creates security credentials and schema if this is indicated by the user in the migration settings.</span></span> <span data-ttu-id="3caa5-129">Tento kód můžete spustit proti cílové instanci SQL Data Warehouse, uloží do souboru, zkopírovat do schránky nebo ani upravit v řádku před provedením další akce.</span><span class="sxs-lookup"><span data-stu-id="3caa5-129">This code can be run against the targeted SQL Data Warehouse instance, saved to a file, copied to your clipboard, or even edited in-line before taking further action.</span></span>  

<span data-ttu-id="3caa5-130">Jak je uvedeno výše, při migraci schématu zkontrolujte migrace změny nástroj udělal aby se zajistilo, že plně rozumíte jim.</span><span class="sxs-lookup"><span data-stu-id="3caa5-130">As noted above, when migrating schema review the migration changes that the tool has made in order to ensure that that you fully understand them.</span></span>  

## <a name="migrate-data"></a><span data-ttu-id="3caa5-131">Migrace dat</span><span class="sxs-lookup"><span data-stu-id="3caa5-131">Migrate data</span></span>
<span data-ttu-id="3caa5-132">Kliknutím na možnost "migrace dat může generovat BCP skripty, které bude přesunutí dat nejprve do plochých souborů na serveru, a pak přímo do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3caa5-132">By clicking the ‘Migrate Data’ option you can generate BCP scripts that will move your data first to flat files on your server, and then directly into your SQL Data Warehouse.</span></span> <span data-ttu-id="3caa5-133">Doporučujeme tento proces pro přesun malé množství dat a jako opakování nejsou předdefinované a selhání může dojít, pokud dojde ke ztrátě síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="3caa5-133">We recommend this process for moving small amounts of data and, as retries are not built-in and failures may occur if there is a loss of the network connection.</span></span> <span data-ttu-id="3caa5-134">Aby bylo možné ji spustit, musíte mít nainstalovaný nástroj příkazového řádku BCP a schéma pro data musí již byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="3caa5-134">In order to run this, you will need to have the BCP command-line utility installed and the schema for the data must already have been created.</span></span>

<span data-ttu-id="3caa5-135">Po vyplnění výše uvedené parametry jednoduše muset kliknout na spuštění migrace a sadu dva balíčky se budou generovat do zadaného umístění.</span><span class="sxs-lookup"><span data-stu-id="3caa5-135">After you have filled out the parameters above you simply need to click run migration and a set of two packages will be generated to your specified location.</span></span> <span data-ttu-id="3caa5-136">Spusťte soubor exportu aby bylo možné exportovat data ze zdroje migrace do plochých souborů a spusťte soubor importu Chcete-li importovat data do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3caa5-136">Run the export file in order to export data from your migration source into flat files, and run the import file in order to import your data into SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3caa5-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3caa5-137">Next steps</span></span>
<span data-ttu-id="3caa5-138">Teď, když jste migrovali některá data, podívejte se na postup [vyvíjet][develop].</span><span class="sxs-lookup"><span data-stu-id="3caa5-138">Now that you've migrated some data, check out how to [develop][develop].</span></span>

<!--Image references-->

<!--Article references-->
[migration documentation]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Download Migration Utility]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
