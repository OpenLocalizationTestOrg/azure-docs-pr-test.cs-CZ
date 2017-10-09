---
title: aaaUse Power BI s SQL Data Warehouse | Microsoft Docs
description: "Tipy pro používání Power BI s datovým skladem SQL Azure pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: b12bee87-2268-40c2-81bf-ab27588b32e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: a3a347493d07af6824a561567f05894cfe3c0471
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-power-bi-with-sql-data-warehouse"></a><span data-ttu-id="4626c-103">SQL Data Warehouse pomocí Power BI</span><span class="sxs-lookup"><span data-stu-id="4626c-103">Use Power BI with SQL Data Warehouse</span></span>
<span data-ttu-id="4626c-104">Stejně jako u databáze SQL Azure SQL Data Warehouse přímé připojení umožňuje uživatele tooleverage výkonné logické přenos směrem dolů vedle hello analytické možnosti Power BI.</span><span class="sxs-lookup"><span data-stu-id="4626c-104">As with Azure SQL Database, SQL Data Warehouse Direct Connect allows user tooleverage powerful logical pushdown alongside hello analytical capabilities of Power BI.</span></span>  <span data-ttu-id="4626c-105">S přímé připojení dotazy jsou odesílány zpět tooyour Azure SQL Data Warehouse v reálném čase jako zkoumat hello data.</span><span class="sxs-lookup"><span data-stu-id="4626c-105">With Direct Connect, queries are sent back tooyour Azure SQL Data Warehouse in real time as you explore hello data.</span></span>  <span data-ttu-id="4626c-106">To, společně s měřítkem hello služby SQL Data Warehouse, umožňuje uživatelům toocreate dynamické sestavy minut před terabajtů dat.</span><span class="sxs-lookup"><span data-stu-id="4626c-106">This, combined with hello scale of SQL Data Warehouse, enables users toocreate dynamic reports in minutes against terabytes of data.</span></span>  <span data-ttu-id="4626c-107">Kromě toho hello zavedení hello otevřete v tlačítko Power BI umožňuje uživatelům toodirectly připojit Power BI tootheir SQL Data Warehouse, aniž by shromažďovat informace z dalších částí Azure.</span><span class="sxs-lookup"><span data-stu-id="4626c-107">In addition, hello introduction of hello Open in Power BI button allows users toodirectly connect Power BI tootheir SQL Data Warehouse without collecting information from other parts of Azure.</span></span>

<span data-ttu-id="4626c-108">Při používání přímé připojení prosím Poznámka:</span><span class="sxs-lookup"><span data-stu-id="4626c-108">When using Direct Connect please note:</span></span>

* <span data-ttu-id="4626c-109">Zadejte název plně kvalifikovaný server hello při připojování (podrobnosti viz níže)</span><span class="sxs-lookup"><span data-stu-id="4626c-109">Specify hello fully qualified server name when connecting (see below for more details)</span></span>
* <span data-ttu-id="4626c-110">Ujistěte se, pravidla brány firewall pro hello databáze jsou nakonfigurované příliš "Povolit přístup tooAzure služby".</span><span class="sxs-lookup"><span data-stu-id="4626c-110">Ensure firewall rules for hello database are configured too"Allow access tooAzure services".</span></span>
* <span data-ttu-id="4626c-111">Všechny akce, jako výběrem sloupce nebo přidáním filtru se bude dotazovat přímo hello datového skladu</span><span class="sxs-lookup"><span data-stu-id="4626c-111">Every action such as selecting a column or adding a filter will  directly query hello data warehouse</span></span>
* <span data-ttu-id="4626c-112">Dlaždice se aktualizuje přibližně každých 15 minut (aktualizace nevyžaduje toobe naplánované)</span><span class="sxs-lookup"><span data-stu-id="4626c-112">Tiles are refreshed approximately every 15 minutes (refresh does not need toobe scheduled)</span></span>
* <span data-ttu-id="4626c-113">Modul otázky A odpovědi není k dispozici pro přímé připojení datové sady</span><span class="sxs-lookup"><span data-stu-id="4626c-113">Q&A is not available for Direct Connect datasets</span></span>
* <span data-ttu-id="4626c-114">Změny schématu nejsou automaticky převzata</span><span class="sxs-lookup"><span data-stu-id="4626c-114">Schema changes are not picked up automatically</span></span>
* <span data-ttu-id="4626c-115">Všechny dotazy přímé připojení bude časový limit po dvou minutách</span><span class="sxs-lookup"><span data-stu-id="4626c-115">All Direct Connect queries will time out after 2 minutes</span></span>

<span data-ttu-id="4626c-116">Při dalším tooimprove hello prostředí může změnit těchto omezení a poznámky.</span><span class="sxs-lookup"><span data-stu-id="4626c-116">These restrictions and notes may change as we continue tooimprove hello experiences.</span></span> <span data-ttu-id="4626c-117">tooconnect Hello kroky jsou podrobně popsány níže.</span><span class="sxs-lookup"><span data-stu-id="4626c-117">hello steps tooconnect are detailed below.</span></span>  

## <a name="using-hello-open-in-power-bi-button"></a><span data-ttu-id="4626c-118">Pomocí tlačítka hello otevřít v Power BI</span><span class="sxs-lookup"><span data-stu-id="4626c-118">Using hello ‘Open in Power BI’ button</span></span>
<span data-ttu-id="4626c-119">Hello nejjednodušší způsob, jak toomove mezi SQL Data Warehouse a Power BI je s hello otevřete v tlačítko Power BI.</span><span class="sxs-lookup"><span data-stu-id="4626c-119">hello easiest way toomove between your SQL Data Warehouse and Power BI is with hello Open in Power BI button.</span></span> <span data-ttu-id="4626c-120">Toto tlačítko, které vám umožní tooseamlessly začněte vytvářet nové řídicí panely v Power BI.</span><span class="sxs-lookup"><span data-stu-id="4626c-120">This button allows you tooseamlessly begin creating new dashboards in Power BI.</span></span>  

1. <span data-ttu-id="4626c-121">tooget spuštění přejděte instanci SQL Data Warehouse tooyour v hello portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="4626c-121">tooget started navigate tooyour SQL Data Warehouse instance in hello Azure Classic Portal.</span></span>
2. <span data-ttu-id="4626c-122">Klikněte na tlačítko Otevřít v Power BI hello.</span><span class="sxs-lookup"><span data-stu-id="4626c-122">Click hello 'Open in Power BI' button.</span></span>
3. <span data-ttu-id="4626c-123">Pokud jsme nejsou možné toosign v přímo, nebo pokud nemáte účet Power BI, budete potřebovat toosign v.</span><span class="sxs-lookup"><span data-stu-id="4626c-123">If we are not able toosign you in directly, or if you do not have a Power BI account, you will need toosign-in.</span></span>  
4. <span data-ttu-id="4626c-124">Budete přesměrováni, že předem vyplnit toohello stránka pro připojení k SQL Data Warehouse, hello informace z SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4626c-124">You will be directed toohello SQL Data Warehouse connection page, with hello information from your SQL Data Warehouse pre-populated.</span></span>
5. <span data-ttu-id="4626c-125">Po zadání přihlašovacích údajů bude plně připojené tooyour SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4626c-125">After entering your credentials you will be fully connected tooyour SQL Data Warehouse.</span></span>

## <a name="connecting-through-hello-power-bi-portal"></a><span data-ttu-id="4626c-126">Připojení prostřednictvím portálu hello Power BI</span><span class="sxs-lookup"><span data-stu-id="4626c-126">Connecting through hello Power BI portal</span></span>
<span data-ttu-id="4626c-127">V přidání toousing hello otevřete v Power BI tlačítko mohou uživatelé také připojit tootheir SQL Data Warehouse prostřednictvím hello portál Power BI.</span><span class="sxs-lookup"><span data-stu-id="4626c-127">In addition toousing hello Open in Power BI button, users can also connect tootheir SQL Data Warehouse through hello Power BI Portal.</span></span>

1. <span data-ttu-id="4626c-128">Klikněte na tlačítko 'načíst Data, na hello dolní části navigačního podokna hello.</span><span class="sxs-lookup"><span data-stu-id="4626c-128">Click 'Get Data' at hello bottom of hello navigation pane.</span></span>
2. <span data-ttu-id="4626c-129">Vyberte "Databáze".</span><span class="sxs-lookup"><span data-stu-id="4626c-129">Select 'Databases'.</span></span>
3. <span data-ttu-id="4626c-130">Jednou na stránce databáze hello, vyberte, Azure SQL Data Warehouse a klepněte na tlačítko "Připojit".</span><span class="sxs-lookup"><span data-stu-id="4626c-130">Once on hello Databases page, select 'Azure SQL Data Warehouse' and then click 'Connect'.</span></span>
4. <span data-ttu-id="4626c-131">Zadejte hello potřebné informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="4626c-131">Enter hello necessary connection information.</span></span>  <span data-ttu-id="4626c-132">Název serveru a název databáze najdete v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4626c-132">Your server name and database name can be found in hello Azure Portal.</span></span>
5. <span data-ttu-id="4626c-133">Budete přesměrováni zpět, hlavní stránky toohello Power BI a po připojení se nový záznam v části "datové sady, se zobrazí s názvem hello vaší instance.</span><span class="sxs-lookup"><span data-stu-id="4626c-133">You will be directed back toohello main page of Power BI and after your connection is made a new entry under 'Datasets' will appear with hello name of your instance.</span></span>  
6. <span data-ttu-id="4626c-134">Kliknutím na nová datová sada tooexplore hello všechny hello tabulek a zobrazení v databázi.</span><span class="sxs-lookup"><span data-stu-id="4626c-134">You can click on hello new dataset tooexplore all of hello tables and views in your database.</span></span> <span data-ttu-id="4626c-135">Výběrem sloupce odešle zdroj back toohello dotazu dynamicky vytváření vizuál.</span><span class="sxs-lookup"><span data-stu-id="4626c-135">Selecting a column will send a query back toohello source, dynamically creating your visual.</span></span> <span data-ttu-id="4626c-136">Tyto vizuály lze uložit do nové sestavy a připnuté zpět tooyour řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="4626c-136">These visuals can be saved in a new report and pinned back tooyour dashboard.</span></span>

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
