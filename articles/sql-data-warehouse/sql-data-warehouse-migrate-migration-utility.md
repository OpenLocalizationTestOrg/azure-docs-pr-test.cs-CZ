---
title: "Migrace: Datového skladu nástroje Migrace | Microsoft Docs"
description: "Migrujte tooSQL datového skladu."
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
ms.openlocfilehash: c89909883fb42b0b04dd87a9973e5ee3e30d8f0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-warehouse-migration-utility-preview"></a><span data-ttu-id="7179b-103">Nástroj migrace data Warehouse (Preview)</span><span class="sxs-lookup"><span data-stu-id="7179b-103">Data Warehouse Migration Utility (Preview)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="7179b-104">[Stáhněte si nástroj pro migraci][Download Migration Utility]</span><span class="sxs-lookup"><span data-stu-id="7179b-104">[Download Migration Utility][Download Migration Utility]</span></span>
> 
> 

<span data-ttu-id="7179b-105">Nástroj migrace datového skladu Hello je nástroj určený toomigrate schéma a data z tooAzure SQL Server a databáze SQL Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7179b-105">hello Data Warehouse Migration Utility is a tool designed toomigrate schema and data from SQL Server and Azure SQL Database tooAzure SQL Data Warehouse.</span></span> <span data-ttu-id="7179b-106">Během migrace schématu hello nástroj automaticky mapuje hello odpovídající schématu z toodestination zdroje.</span><span class="sxs-lookup"><span data-stu-id="7179b-106">During schema migration, hello tool automatically maps hello corresponding schema from source toodestination.</span></span> <span data-ttu-id="7179b-107">Po migraci schématu hello hello nástrojů poskytuje hello možnost toomove data se automaticky generovaných skriptů.</span><span class="sxs-lookup"><span data-stu-id="7179b-107">After hello schema has been migrated, hello tools provides hello option toomove data with automatically generated scripts.</span></span>

<span data-ttu-id="7179b-108">Kromě toho tooschema a data migrace, tento nástroj umožňuje hello možnost toogenerate kompatibility sestavy, které shrnují problémům s kompatibilitou mezi hello zdrojové a cílové instance, které by jinak znemožňovaly zjednodušený migrace.</span><span class="sxs-lookup"><span data-stu-id="7179b-108">In addition tooschema and data migration, this tool gives you hello option toogenerate compatibility reports which summarize incompatibilities between hello target and source instances which would prevent streamlined migration.</span></span>

## <a name="get-started"></a><span data-ttu-id="7179b-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="7179b-109">Get started</span></span>
<span data-ttu-id="7179b-110">Předpokladem pro instalaci budete potřebovat hello BCP nástroj příkazového řádku toorun migrační skripty a Office tooview hello kompatibility sestavy.</span><span class="sxs-lookup"><span data-stu-id="7179b-110">As a prerequisite for installation, you will need hello BCP command-line utility toorun migration scripts and Office tooview hello compatibility report.</span></span> <span data-ttu-id="7179b-111">Po spuštění hello bude spustitelný soubor, který byl stažen výzvami tooaccept standardní smlouvy EULA předtím, než se nainstaluje nástroj hello.</span><span class="sxs-lookup"><span data-stu-id="7179b-111">After launching hello executable that is downloaded you will be prompted tooaccept a standard EULA before hello tool will be installed.</span></span>

<span data-ttu-id="7179b-112">Navíc toorun hello Utiliy migrace, můžete se třeba hello jeden následující oprávnění v databázi hello hledáte toomigrate: vytvoření databáze, ALTER ANY DATABASE nebo ANY definice zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7179b-112">In addition, toorun hello Migration Utiliy, you will need hello one following permissions on hello database that you are looking toomigrate: CREATE DATABASE, ALTER ANY DATABASE or VIEW ANY DEFINITION.</span></span>

### <a name="launching-hello-tool-and-connecting"></a><span data-ttu-id="7179b-113">Spuštění nástroje hello a připojení</span><span class="sxs-lookup"><span data-stu-id="7179b-113">Launching hello tool and connecting</span></span>
<span data-ttu-id="7179b-114">Nainstalujte nástroj pro spuštění hello kliknutím na ikony na ploše hello zobrazené post.</span><span class="sxs-lookup"><span data-stu-id="7179b-114">Launch hello tool by clicking on hello desktop icon which appears post install.</span></span> <span data-ttu-id="7179b-115">Při otevírání hello nástroj, zobrazí se výzva počáteční připojení stránky, kde si můžete vybrat zdroj a cíl pro nástroj pro migraci hello.</span><span class="sxs-lookup"><span data-stu-id="7179b-115">Upon opening hello tool, you will be prompted with an initial connection page where you can choose your source and destination for hello migration tool.</span></span> <span data-ttu-id="7179b-116">V současné době podporujeme systému SQL Server a Azure SQL Database jako zdroje a SQL Data Warehouse jako cíl.</span><span class="sxs-lookup"><span data-stu-id="7179b-116">At this time, we support SQL Server and Azure SQL Database as sources and SQL Data Warehouse as a destination.</span></span> <span data-ttu-id="7179b-117">Po výběru to zobrazí se výzva tooconnect tooyour zdrojový server naplnění v názvu serveru a ověřování a potom klikněte na "Připojit".</span><span class="sxs-lookup"><span data-stu-id="7179b-117">After selecting this you will be asked tooconnect tooyour source server by filling in server name and authenticating and then clicking ‘Connect’.</span></span>

<span data-ttu-id="7179b-118">Po ověření, zobrazí nástroj hello seznam databází, které se nacházejí v hello serveru, který jste připojeni k.</span><span class="sxs-lookup"><span data-stu-id="7179b-118">After authenticating, hello tool will show a list of databases that are present in hello server which you are connected to.</span></span> <span data-ttu-id="7179b-119">Hello migrace můžete začít tak, že vyberete databázi, která chcete toomigrate a kliknete na 'Migrací vybrané'.</span><span class="sxs-lookup"><span data-stu-id="7179b-119">You can begin hello migration by selecting a database that you would like toomigrate and then clicking on ‘Migrate selected’.</span></span>

## <a name="migration-report"></a><span data-ttu-id="7179b-120">Sestavy migrace</span><span class="sxs-lookup"><span data-stu-id="7179b-120">Migration report</span></span>
<span data-ttu-id="7179b-121">Výběr zkontrolovat kompatibilitu databáze v nástroji hello způsobí vygenerování sestavy shrnutí všech nekompatibility objektu v databázi hello požadujete toomigrate.</span><span class="sxs-lookup"><span data-stu-id="7179b-121">Selecting ‘Check Database Compatibility’ in hello tool will generate a report summarizing all object incompatibilities in hello database you requested toomigrate.</span></span> <span data-ttu-id="7179b-122">Širší seznam některých hello funkci systému SQL Server, který se nenachází v SQL Data Warehouse najdete v našich [dokumentace k migraci][migration documentation].</span><span class="sxs-lookup"><span data-stu-id="7179b-122">A broader list of some of hello SQL Server functionality that is not present in SQL Data Warehouse can be found in our [migration documentation][migration documentation].</span></span> <span data-ttu-id="7179b-123">Po vygenerování sestavy hello bude možné toosave a hello otevřete sestavu v aplikaci Excel.</span><span class="sxs-lookup"><span data-stu-id="7179b-123">After hello report is generated you will be able toosave and open hello report in Excel.</span></span>

<span data-ttu-id="7179b-124">Pamatujte, že při generování hello migrace schématu, identifikovány jako 'Objektu' je nutné upravit pořadí tooallow okamžité migraci dat většiny problémů.</span><span class="sxs-lookup"><span data-stu-id="7179b-124">Please note that when generating hello migration schema, most issues identified as ‘Object’ will be adjusted in order tooallow immediate migration of that data.</span></span> <span data-ttu-id="7179b-125">Přečtěte si tooensure změny hello nechcete toomake další úpravy před použitím schématu hello.</span><span class="sxs-lookup"><span data-stu-id="7179b-125">Please review hello changes tooensure you do not want toomake additional adjustments before applying hello schema.</span></span>

## <a name="migrate-schema"></a><span data-ttu-id="7179b-126">Migrace schématu</span><span class="sxs-lookup"><span data-stu-id="7179b-126">Migrate schema</span></span>
<span data-ttu-id="7179b-127">Po připojení, vygeneruje výběr 'migraci schématu skript pro migraci schématu pro hello vybrané tabulky.</span><span class="sxs-lookup"><span data-stu-id="7179b-127">After connecting, selecting ‘Migrate Schema’ will generate a schema migration script for hello selected tables.</span></span> <span data-ttu-id="7179b-128">Tato struktura skriptu porty hello hello tabulky mapuje nekompatibilní datové typy toomore kompatibilní formuláře a vytvoří zabezpečovací přihlašovací údaje a schéma, pokud to vyplývá z hello uživatele v nastavení migrace hello.</span><span class="sxs-lookup"><span data-stu-id="7179b-128">This script ports hello structure of hello table, maps incompatible data types toomore compatible forms, and creates security credentials and schema if this is indicated by hello user in hello migration settings.</span></span> <span data-ttu-id="7179b-129">Tento kód můžete spustit proti hello cílové instanci SQL Data Warehouse, uložili soubor tooa, zkopírovat tooyour schránky nebo ani upravit v řádku před provedením další akce.</span><span class="sxs-lookup"><span data-stu-id="7179b-129">This code can be run against hello targeted SQL Data Warehouse instance, saved tooa file, copied tooyour clipboard, or even edited in-line before taking further action.</span></span>  

<span data-ttu-id="7179b-130">Jak je uvedeno výše, až se migrace schématu zkontrolujte hello migrace změní této hello udělal nástroj v pořadí tooensure, že plně rozumíte jim.</span><span class="sxs-lookup"><span data-stu-id="7179b-130">As noted above, when migrating schema review hello migration changes that hello tool has made in order tooensure that that you fully understand them.</span></span>  

## <a name="migrate-data"></a><span data-ttu-id="7179b-131">Migrace dat</span><span class="sxs-lookup"><span data-stu-id="7179b-131">Migrate data</span></span>
<span data-ttu-id="7179b-132">Kliknutím na možnost "migrovat Data hello může generovat BCP skripty, které se přesune datové soubory první tooflat na serveru, a pak přímo do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7179b-132">By clicking hello ‘Migrate Data’ option you can generate BCP scripts that will move your data first tooflat files on your server, and then directly into your SQL Data Warehouse.</span></span> <span data-ttu-id="7179b-133">Doporučujeme tento proces pro přesun malé množství dat a jako opakování nejsou předdefinované a selhání může dojít, pokud dojde ke ztrátě hello síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="7179b-133">We recommend this process for moving small amounts of data and, as retries are not built-in and failures may occur if there is a loss of hello network connection.</span></span> <span data-ttu-id="7179b-134">V pořadí toorun, to, budete potřebovat toohave hello BCP nainstalovaný nástroj příkazového řádku a hello schéma pro hello data musí již byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="7179b-134">In order toorun this, you will need toohave hello BCP command-line utility installed and hello schema for hello data must already have been created.</span></span>

<span data-ttu-id="7179b-135">Po vyplnění hello parametry výše můžete jednoduše potřebovat tooclick spuštění migrace a sadu dva balíčky se budou generovat tooyour zadané umístění.</span><span class="sxs-lookup"><span data-stu-id="7179b-135">After you have filled out hello parameters above you simply need tooclick run migration and a set of two packages will be generated tooyour specified location.</span></span> <span data-ttu-id="7179b-136">Spusťte soubor exportu hello v pořadí tooexport data ze zdroje migrace do plochých souborů a spusťte soubor importu hello v pořadí tooimport data do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7179b-136">Run hello export file in order tooexport data from your migration source into flat files, and run hello import file in order tooimport your data into SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7179b-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7179b-137">Next steps</span></span>
<span data-ttu-id="7179b-138">Teď, když jste migrovali některá data, podívejte se jak příliš[vyvíjet][develop].</span><span class="sxs-lookup"><span data-stu-id="7179b-138">Now that you've migrated some data, check out how too[develop][develop].</span></span>

<!--Image references-->

<!--Article references-->
[migration documentation]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Download Migration Utility]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
