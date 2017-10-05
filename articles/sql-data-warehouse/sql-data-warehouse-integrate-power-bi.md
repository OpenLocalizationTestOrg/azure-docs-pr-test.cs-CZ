---
title: "Používat Power BI s SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: 4b7609fc5d6ce7bf0e3bd3ebf6d8f52e93a40a75
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-power-bi-with-sql-data-warehouse"></a><span data-ttu-id="8141d-103">SQL Data Warehouse pomocí Power BI</span><span class="sxs-lookup"><span data-stu-id="8141d-103">Use Power BI with SQL Data Warehouse</span></span>
<span data-ttu-id="8141d-104">Jako s Azure SQL Database, SQL Data Warehouse přímé připojení umožňuje uživatelům využívat výkonné logické přenos směrem dolů vedle analytické možnosti Power BI.</span><span class="sxs-lookup"><span data-stu-id="8141d-104">As with Azure SQL Database, SQL Data Warehouse Direct Connect allows user to leverage powerful logical pushdown alongside the analytical capabilities of Power BI.</span></span>  <span data-ttu-id="8141d-105">Přímé připojení dotazy jsou odeslán zpět do Azure SQL Data Warehouse v reálném čase jako zkoumat data.</span><span class="sxs-lookup"><span data-stu-id="8141d-105">With Direct Connect, queries are sent back to your Azure SQL Data Warehouse in real time as you explore the data.</span></span>  <span data-ttu-id="8141d-106">To, v kombinaci s měřítkem služby SQL Data Warehouse, umožňuje uživatelům vytvářet dynamické sestavy v minutách proti terabajtů dat.</span><span class="sxs-lookup"><span data-stu-id="8141d-106">This, combined with the scale of SQL Data Warehouse, enables users to create dynamic reports in minutes against terabytes of data.</span></span>  <span data-ttu-id="8141d-107">Kromě toho zavedení Open in tlačítko Power BI umožňuje uživatelům připojovat přímo Power BI do jejich SQL Data Warehouse bez shromažďovat informace z dalších částí Azure.</span><span class="sxs-lookup"><span data-stu-id="8141d-107">In addition, the introduction of the Open in Power BI button allows users to directly connect Power BI to their SQL Data Warehouse without collecting information from other parts of Azure.</span></span>

<span data-ttu-id="8141d-108">Při používání přímé připojení prosím Poznámka:</span><span class="sxs-lookup"><span data-stu-id="8141d-108">When using Direct Connect please note:</span></span>

* <span data-ttu-id="8141d-109">Zadejte název plně kvalifikovaný serveru při připojování (podrobnosti viz níže)</span><span class="sxs-lookup"><span data-stu-id="8141d-109">Specify the fully qualified server name when connecting (see below for more details)</span></span>
* <span data-ttu-id="8141d-110">Ujistěte se, že jsou pravidla brány firewall pro databázi nakonfigurovaná na "Povolit přístup ke službám Azure".</span><span class="sxs-lookup"><span data-stu-id="8141d-110">Ensure firewall rules for the database are configured to "Allow access to Azure services".</span></span>
* <span data-ttu-id="8141d-111">Všechny akce, jako výběrem sloupce nebo přidáním filtru se přímo dotazování datového skladu</span><span class="sxs-lookup"><span data-stu-id="8141d-111">Every action such as selecting a column or adding a filter will  directly query the data warehouse</span></span>
* <span data-ttu-id="8141d-112">Dlaždice se aktualizuje přibližně každých 15 minut (aktualizace nemusí být naplánováno)</span><span class="sxs-lookup"><span data-stu-id="8141d-112">Tiles are refreshed approximately every 15 minutes (refresh does not need to be scheduled)</span></span>
* <span data-ttu-id="8141d-113">Modul otázky A odpovědi není k dispozici pro přímé připojení datové sady</span><span class="sxs-lookup"><span data-stu-id="8141d-113">Q&A is not available for Direct Connect datasets</span></span>
* <span data-ttu-id="8141d-114">Změny schématu nejsou automaticky převzata</span><span class="sxs-lookup"><span data-stu-id="8141d-114">Schema changes are not picked up automatically</span></span>
* <span data-ttu-id="8141d-115">Všechny dotazy přímé připojení bude časový limit po dvou minutách</span><span class="sxs-lookup"><span data-stu-id="8141d-115">All Direct Connect queries will time out after 2 minutes</span></span>

<span data-ttu-id="8141d-116">Těchto omezení a poznámky může změnit, protože zdokonalování zkušenosti.</span><span class="sxs-lookup"><span data-stu-id="8141d-116">These restrictions and notes may change as we continue to improve the experiences.</span></span> <span data-ttu-id="8141d-117">Kroky pro připojení jsou podrobně popsány níže.</span><span class="sxs-lookup"><span data-stu-id="8141d-117">The steps to connect are detailed below.</span></span>  

## <a name="using-the-open-in-power-bi-button"></a><span data-ttu-id="8141d-118">Pomocí tlačítko Otevřít v Power BI</span><span class="sxs-lookup"><span data-stu-id="8141d-118">Using the ‘Open in Power BI’ button</span></span>
<span data-ttu-id="8141d-119">Nejjednodušší způsob, jak přesunout mezi SQL Data Warehouse a Power BI je s Open in tlačítko Power BI.</span><span class="sxs-lookup"><span data-stu-id="8141d-119">The easiest way to move between your SQL Data Warehouse and Power BI is with the Open in Power BI button.</span></span> <span data-ttu-id="8141d-120">Toto tlačítko umožňuje bezproblémově začněte vytvářet nové řídicí panely v Power BI.</span><span class="sxs-lookup"><span data-stu-id="8141d-120">This button allows you to seamlessly begin creating new dashboards in Power BI.</span></span>  

1. <span data-ttu-id="8141d-121">Abyste mohli začít přejděte k instanci SQL Data Warehouse na portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="8141d-121">To get started navigate to your SQL Data Warehouse instance in the Azure Classic Portal.</span></span>
2. <span data-ttu-id="8141d-122">Klikněte na tlačítko Otevřít v Power BI.</span><span class="sxs-lookup"><span data-stu-id="8141d-122">Click the 'Open in Power BI' button.</span></span>
3. <span data-ttu-id="8141d-123">Pokud nelze pro přihlášení přímo, nebo pokud nemáte účet Power BI, musíte se přihlásit.</span><span class="sxs-lookup"><span data-stu-id="8141d-123">If we are not able to sign you in directly, or if you do not have a Power BI account, you will need to sign-in.</span></span>  
4. <span data-ttu-id="8141d-124">Budete přesměrováni na stránku pro připojení k SQL Data Warehouse, informace z SQL Data Warehouse předem vyplnit.</span><span class="sxs-lookup"><span data-stu-id="8141d-124">You will be directed to the SQL Data Warehouse connection page, with the information from your SQL Data Warehouse pre-populated.</span></span>
5. <span data-ttu-id="8141d-125">Po zadání přihlašovacích údajů budete plně připojení k SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8141d-125">After entering your credentials you will be fully connected to your SQL Data Warehouse.</span></span>

## <a name="connecting-through-the-power-bi-portal"></a><span data-ttu-id="8141d-126">Připojení prostřednictvím portálu Power BI</span><span class="sxs-lookup"><span data-stu-id="8141d-126">Connecting through the Power BI portal</span></span>
<span data-ttu-id="8141d-127">Kromě používání Open in tlačítko Power BI, můžete také uživatelé připojit ke své SQL Data Warehouse prostřednictvím portálu Power BI.</span><span class="sxs-lookup"><span data-stu-id="8141d-127">In addition to using the Open in Power BI button, users can also connect to their SQL Data Warehouse through the Power BI Portal.</span></span>

1. <span data-ttu-id="8141d-128">Klikněte na tlačítko 'načíst Data, v dolní části navigačního podokna.</span><span class="sxs-lookup"><span data-stu-id="8141d-128">Click 'Get Data' at the bottom of the navigation pane.</span></span>
2. <span data-ttu-id="8141d-129">Vyberte "Databáze".</span><span class="sxs-lookup"><span data-stu-id="8141d-129">Select 'Databases'.</span></span>
3. <span data-ttu-id="8141d-130">Jednou na stránce databáze vyberte 'Azure SQL Data Warehouse a klepněte na tlačítko "Připojit".</span><span class="sxs-lookup"><span data-stu-id="8141d-130">Once on the Databases page, select 'Azure SQL Data Warehouse' and then click 'Connect'.</span></span>
4. <span data-ttu-id="8141d-131">Zadejte připojovací údaje.</span><span class="sxs-lookup"><span data-stu-id="8141d-131">Enter the necessary connection information.</span></span>  <span data-ttu-id="8141d-132">Název serveru a název databáze naleznete na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="8141d-132">Your server name and database name can be found in the Azure Portal.</span></span>
5. <span data-ttu-id="8141d-133">Budete přesměrováni zpět na hlavní stránce Power BI a po připojení se nový záznam v části "datové sady, zobrazí se název vaší instance.</span><span class="sxs-lookup"><span data-stu-id="8141d-133">You will be directed back to the main page of Power BI and after your connection is made a new entry under 'Datasets' will appear with the name of your instance.</span></span>  
6. <span data-ttu-id="8141d-134">Kliknutím na nová datová sada a prozkoumejte všechny tabulky a zobrazení v databázi.</span><span class="sxs-lookup"><span data-stu-id="8141d-134">You can click on the new dataset to explore all of the tables and views in your database.</span></span> <span data-ttu-id="8141d-135">Výběrem sloupce odešle dotaz zpět do zdroje, dynamicky vytváření vizuál.</span><span class="sxs-lookup"><span data-stu-id="8141d-135">Selecting a column will send a query back to the source, dynamically creating your visual.</span></span> <span data-ttu-id="8141d-136">Tyto vizuály lze uložit v nové sestavy a připnuté zpět na řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="8141d-136">These visuals can be saved in a new report and pinned back to your dashboard.</span></span>

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
