---
title: "Porty nad rámec 1433 pro databázi SQL. | Microsoft Docs"
description: "Připojení klienta z ADO.NET do Azure SQL Database někdy obcházení proxy serveru a komunikovat přímo s databází. Na významu nabývají jiné porty než 1433."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
ms.assetid: 3f17106a-92fd-4aa4-b6a9-1daa29421f64
ms.service: sql-database
ms.custom: develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: sstein
ms.openlocfilehash: d47ee8c794d1e231507dae6bb4aa88bf19ce6418
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="ports-beyond-1433-for-adonet-45"></a><span data-ttu-id="b1671-104">Porty nad rámec 1433 pro technologii ADO.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b1671-104">Ports beyond 1433 for ADO.NET 4.5</span></span>
<span data-ttu-id="b1671-105">Toto téma popisuje chování připojení databáze SQL Azure pro klienty, kteří používají ADO.NET 4.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b1671-105">This topic describes the Azure SQL Database connection behavior for clients that use ADO.NET 4.5 or a later version.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="b1671-106">Informace o architektuře připojení najdete v tématu [architektura připojení k databázi SQL Azure](sql-database-connectivity-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="b1671-106">For information about connectivity architecture, see [Azure SQL Database connectivity architecture](sql-database-connectivity-architecture.md).</span></span>
>

## <a name="outside-vs-inside"></a><span data-ttu-id="b1671-107">Mimo vs uvnitř</span><span class="sxs-lookup"><span data-stu-id="b1671-107">Outside vs inside</span></span>
<span data-ttu-id="b1671-108">Pro připojení k databázi SQL Azure, musíte nejprve požádáme zda váš klientský program běží *mimo* nebo *uvnitř* hranic cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="b1671-108">For connections to Azure SQL Database, we must first ask whether your client program runs *outside* or *inside* the Azure cloud boundary.</span></span> <span data-ttu-id="b1671-109">Témata popisují dvě běžné scénáře.</span><span class="sxs-lookup"><span data-stu-id="b1671-109">The subsections discuss two common scenarios.</span></span>

#### <a name="outside-client-runs-on-your-desktop-computer"></a><span data-ttu-id="b1671-110">*Mimo:* klient spustí ve stolním počítači</span><span class="sxs-lookup"><span data-stu-id="b1671-110">*Outside:* Client runs on your desktop computer</span></span>
<span data-ttu-id="b1671-111">Port 1433 je jediný port, který musí být otevřený v počítači, který je hostitelem klientské aplikace SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b1671-111">Port 1433 is the only port that must be open on your desktop computer that hosts your SQL Database client application.</span></span>

#### <a name="inside-client-runs-on-azure"></a><span data-ttu-id="b1671-112">*Uvnitř:* klienta běží na Azure</span><span class="sxs-lookup"><span data-stu-id="b1671-112">*Inside:* Client runs on Azure</span></span>
<span data-ttu-id="b1671-113">Váš klient běží v rámci hranic cloudu Azure, používá, můžete takzvaný *přímé trasy* komunikovat se serverem SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b1671-113">When your client runs inside the Azure cloud boundary, it uses what we can call a *direct route* to interact with the SQL Database server.</span></span> <span data-ttu-id="b1671-114">Po vytvoření připojení, další interakce mezi klientem a databáze zahrnovat žádný proxy server middleware.</span><span class="sxs-lookup"><span data-stu-id="b1671-114">After a connection is established, further interactions between the client and database involve no middleware proxy.</span></span>

<span data-ttu-id="b1671-115">Pořadí je následující:</span><span class="sxs-lookup"><span data-stu-id="b1671-115">The sequence is as follows:</span></span>

1. <span data-ttu-id="b1671-116">ADO.NET 4.5 (nebo novější) zahájí stručný interakci s cloudu Azure a přijímá číslo dynamicky identifikovaných portu.</span><span class="sxs-lookup"><span data-stu-id="b1671-116">ADO.NET 4.5 (or later) initiates a brief interaction with the Azure cloud, and receives a dynamically identified port number.</span></span>
   
   * <span data-ttu-id="b1671-117">Číslo portu dynamicky identifikovaných je v rozsahu 11000 11999 nebo 14000 14999.</span><span class="sxs-lookup"><span data-stu-id="b1671-117">The dynamically identified port number is in the range of 11000-11999 or 14000-14999.</span></span>
2. <span data-ttu-id="b1671-118">ADO.NET pak připojí k serveru služby SQL Database přímo, s žádné middlewaru v rozmezí.</span><span class="sxs-lookup"><span data-stu-id="b1671-118">ADO.NET then connects to the SQL Database server directly, with no middleware in between.</span></span>
3. <span data-ttu-id="b1671-119">Dotazy jsou odesílány přímo do databáze a výsledky se vrátí přímo do klienta.</span><span class="sxs-lookup"><span data-stu-id="b1671-119">Queries are sent directly to the database, and results are returned directly to the client.</span></span>

<span data-ttu-id="b1671-120">Zajistěte, aby port, který rozsahů 11000 11999 a 14000 14999 na Azure klientský počítač zůstává k dispozici pro technologii ADO.NET 4.5 klienta interakce s databází SQL.</span><span class="sxs-lookup"><span data-stu-id="b1671-120">Ensure that the port ranges of 11000-11999 and 14000-14999 on your Azure client machine are left available for ADO.NET 4.5 client interactions with SQL Database.</span></span>

* <span data-ttu-id="b1671-121">Porty v rozsahu musí být konkrétně bezplatné jiné odchozí blokování.</span><span class="sxs-lookup"><span data-stu-id="b1671-121">In particular, ports in the range must be free of any other outbound blockers.</span></span>
* <span data-ttu-id="b1671-122">Na vašem virtuálním počítači Azure **brány Windows Firewall s pokročilým zabezpečením** řídí nastavení portu.</span><span class="sxs-lookup"><span data-stu-id="b1671-122">On your Azure VM, the **Windows Firewall with Advanced Security** controls the port settings.</span></span>
  
  * <span data-ttu-id="b1671-123">Můžete použít [brány firewall na uživatelské rozhraní](http://msdn.microsoft.com/library/cc646023.aspx) přidat pravidlo, pro který nastavíte **TCP** protokolu společně s rozsah portů se syntaxí, jako **11000 11999**.</span><span class="sxs-lookup"><span data-stu-id="b1671-123">You can use the [firewall's user interface](http://msdn.microsoft.com/library/cc646023.aspx) to add a rule for which you specify the **TCP** protocol along with a port range with the syntax like **11000-11999**.</span></span>

## <a name="version-clarifications"></a><span data-ttu-id="b1671-124">Objasnění, která verze</span><span class="sxs-lookup"><span data-stu-id="b1671-124">Version clarifications</span></span>
<span data-ttu-id="b1671-125">Tato část vysvětluje zástupných názvů, které odkazují na verze produktu.</span><span class="sxs-lookup"><span data-stu-id="b1671-125">This section clarifies the monikers that refer to product versions.</span></span> <span data-ttu-id="b1671-126">Uvádí taky některé dvojic verze mezi produkty.</span><span class="sxs-lookup"><span data-stu-id="b1671-126">It also lists some pairings of versions between products.</span></span>

#### <a name="adonet"></a><span data-ttu-id="b1671-127">ADO.NET</span><span class="sxs-lookup"><span data-stu-id="b1671-127">ADO.NET</span></span>
* <span data-ttu-id="b1671-128">ADO.NET 4.0 podporuje protokol TDS 7.3, ale není 7.4.</span><span class="sxs-lookup"><span data-stu-id="b1671-128">ADO.NET 4.0 supports the TDS 7.3 protocol, but not 7.4.</span></span>
* <span data-ttu-id="b1671-129">ADO.NET 4.5 a novější podporuje protokol TDS 7.4.</span><span class="sxs-lookup"><span data-stu-id="b1671-129">ADO.NET 4.5 and later supports the TDS 7.4 protocol.</span></span>

## <a name="related-links"></a><span data-ttu-id="b1671-130">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="b1671-130">Related links</span></span>
* <span data-ttu-id="b1671-131">ADO.NET 4.6 byla vydána 20. července 2015.</span><span class="sxs-lookup"><span data-stu-id="b1671-131">ADO.NET 4.6 was released on July 20, 2015.</span></span> <span data-ttu-id="b1671-132">Blog sdělení od týmu .NET je k dispozici [zde](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).</span><span class="sxs-lookup"><span data-stu-id="b1671-132">A blog announcement from the .NET team is available [here](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).</span></span>
* <span data-ttu-id="b1671-133">ADO.NET 4.5 byla vydána 15 srpen 2012.</span><span class="sxs-lookup"><span data-stu-id="b1671-133">ADO.NET 4.5 was released on August 15, 2012.</span></span> <span data-ttu-id="b1671-134">Blog sdělení od týmu .NET je k dispozici [zde](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).</span><span class="sxs-lookup"><span data-stu-id="b1671-134">A blog announcement from the .NET team is available [here](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).</span></span>
  
  * <span data-ttu-id="b1671-135">Příspěvek blogu o ADO.NET 4.5.1 je k dispozici [zde](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).</span><span class="sxs-lookup"><span data-stu-id="b1671-135">A blog post about ADO.NET 4.5.1 is available [here](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).</span></span>
* [<span data-ttu-id="b1671-136">Seznam verzí protokolu TDS.</span><span class="sxs-lookup"><span data-stu-id="b1671-136">TDS protocol version list</span></span>](http://www.freetds.org/userguide/tdshistory.htm)
* [<span data-ttu-id="b1671-137">Přehled vývoje SQL databáze</span><span class="sxs-lookup"><span data-stu-id="b1671-137">SQL Database Development Overview</span></span>](sql-database-develop-overview.md)
* [<span data-ttu-id="b1671-138">Azure SQL Database brány firewall</span><span class="sxs-lookup"><span data-stu-id="b1671-138">Azure SQL Database firewall</span></span>](sql-database-firewall-configure.md)
* [<span data-ttu-id="b1671-139">Postupy: Konfigurace nastavení brány firewall pro službu SQL Database</span><span class="sxs-lookup"><span data-stu-id="b1671-139">How to: Configure firewall settings on SQL Database</span></span>](sql-database-configure-firewall-settings.md)

