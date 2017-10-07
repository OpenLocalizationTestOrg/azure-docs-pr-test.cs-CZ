---
title: "aaaPorts nad rámec 1433 pro databázi SQL. | Microsoft Docs"
description: "Připojení klienta z ADO.NET tooAzure SQL Database někdy Nepoužívat proxy server hello a komunikovat přímo s databází hello. Na významu nabývají jiné porty než 1433."
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
ms.openlocfilehash: a35ff2d827ae3fa29b3ea855dbb7ed78583c82eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ports-beyond-1433-for-adonet-45"></a><span data-ttu-id="03694-104">Porty nad rámec 1433 pro technologii ADO.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="03694-104">Ports beyond 1433 for ADO.NET 4.5</span></span>
<span data-ttu-id="03694-105">Toto téma popisuje chování připojení hello Azure SQL Database pro klienty, kteří používají ADO.NET 4.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="03694-105">This topic describes hello Azure SQL Database connection behavior for clients that use ADO.NET 4.5 or a later version.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="03694-106">Informace o architektuře připojení najdete v tématu [architektura připojení k databázi SQL Azure](sql-database-connectivity-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="03694-106">For information about connectivity architecture, see [Azure SQL Database connectivity architecture](sql-database-connectivity-architecture.md).</span></span>
>

## <a name="outside-vs-inside"></a><span data-ttu-id="03694-107">Mimo vs uvnitř</span><span class="sxs-lookup"><span data-stu-id="03694-107">Outside vs inside</span></span>
<span data-ttu-id="03694-108">Pro připojení tooAzure SQL Database, musíte nejprve požádáme zda váš klientský program běží *mimo* nebo *uvnitř* hranic hello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="03694-108">For connections tooAzure SQL Database, we must first ask whether your client program runs *outside* or *inside* hello Azure cloud boundary.</span></span> <span data-ttu-id="03694-109">Hello témata popisují dvě běžné scénáře.</span><span class="sxs-lookup"><span data-stu-id="03694-109">hello subsections discuss two common scenarios.</span></span>

#### <a name="outside-client-runs-on-your-desktop-computer"></a><span data-ttu-id="03694-110">*Mimo:* klient spustí ve stolním počítači</span><span class="sxs-lookup"><span data-stu-id="03694-110">*Outside:* Client runs on your desktop computer</span></span>
<span data-ttu-id="03694-111">Port 1433 je hello jediný port, který musí být otevřený v počítači, který je hostitelem klientské aplikace SQL Database.</span><span class="sxs-lookup"><span data-stu-id="03694-111">Port 1433 is hello only port that must be open on your desktop computer that hosts your SQL Database client application.</span></span>

#### <a name="inside-client-runs-on-azure"></a><span data-ttu-id="03694-112">*Uvnitř:* klienta běží na Azure</span><span class="sxs-lookup"><span data-stu-id="03694-112">*Inside:* Client runs on Azure</span></span>
<span data-ttu-id="03694-113">Váš klient běží v rámci hranic hello cloudu Azure, používá, můžete takzvaný *přímé trasy* toointeract hello databáze SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="03694-113">When your client runs inside hello Azure cloud boundary, it uses what we can call a *direct route* toointeract with hello SQL Database server.</span></span> <span data-ttu-id="03694-114">Po vytvoření připojení, další interakce mezi hello klienta a databáze zahrnovat žádný proxy server middleware.</span><span class="sxs-lookup"><span data-stu-id="03694-114">After a connection is established, further interactions between hello client and database involve no middleware proxy.</span></span>

<span data-ttu-id="03694-115">pořadí Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="03694-115">hello sequence is as follows:</span></span>

1. <span data-ttu-id="03694-116">ADO.NET 4.5 (nebo novější) zahájí stručný interakci s hello cloudu Azure a přijímá číslo dynamicky identifikovaných portu.</span><span class="sxs-lookup"><span data-stu-id="03694-116">ADO.NET 4.5 (or later) initiates a brief interaction with hello Azure cloud, and receives a dynamically identified port number.</span></span>
   
   * <span data-ttu-id="03694-117">číslo portu Hello dynamicky identifikovat je v rozsahu hello 11000 11999 nebo 14000 14999.</span><span class="sxs-lookup"><span data-stu-id="03694-117">hello dynamically identified port number is in hello range of 11000-11999 or 14000-14999.</span></span>
2. <span data-ttu-id="03694-118">ADO.NET pak připojí toohello databáze SQL server přímo, bez middlewaru v rozmezí.</span><span class="sxs-lookup"><span data-stu-id="03694-118">ADO.NET then connects toohello SQL Database server directly, with no middleware in between.</span></span>
3. <span data-ttu-id="03694-119">Dotazy jsou odesílány přímo toohello databáze a výsledky se vrátí přímo toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="03694-119">Queries are sent directly toohello database, and results are returned directly toohello client.</span></span>

<span data-ttu-id="03694-120">Ujistěte se, že hello port, který rozsahů 11000 11999 a 14000 14999 na Azure klientský počítač zůstává k dispozici pro technologii ADO.NET 4.5 klienta interakce s databází SQL.</span><span class="sxs-lookup"><span data-stu-id="03694-120">Ensure that hello port ranges of 11000-11999 and 14000-14999 on your Azure client machine are left available for ADO.NET 4.5 client interactions with SQL Database.</span></span>

* <span data-ttu-id="03694-121">Porty v rozsahu hello konkrétně musí být bezplatné jiné odchozí blokování.</span><span class="sxs-lookup"><span data-stu-id="03694-121">In particular, ports in hello range must be free of any other outbound blockers.</span></span>
* <span data-ttu-id="03694-122">Na vašem virtuálním počítači Azure hello **brány Windows Firewall s pokročilým zabezpečením** ovládací prvky hello nastavení portu.</span><span class="sxs-lookup"><span data-stu-id="03694-122">On your Azure VM, hello **Windows Firewall with Advanced Security** controls hello port settings.</span></span>
  
  * <span data-ttu-id="03694-123">Můžete použít hello [brány firewall na uživatelské rozhraní](http://msdn.microsoft.com/library/cc646023.aspx) tooadd a pravidlo, pro který nastavíte hello **TCP** jako protokol společně s rozsah portů se syntaxí hello **11000 11999**.</span><span class="sxs-lookup"><span data-stu-id="03694-123">You can use hello [firewall's user interface](http://msdn.microsoft.com/library/cc646023.aspx) tooadd a rule for which you specify hello **TCP** protocol along with a port range with hello syntax like **11000-11999**.</span></span>

## <a name="version-clarifications"></a><span data-ttu-id="03694-124">Objasnění, která verze</span><span class="sxs-lookup"><span data-stu-id="03694-124">Version clarifications</span></span>
<span data-ttu-id="03694-125">Tato část vysvětluje hello zástupných názvů, které odkazují tooproduct verze.</span><span class="sxs-lookup"><span data-stu-id="03694-125">This section clarifies hello monikers that refer tooproduct versions.</span></span> <span data-ttu-id="03694-126">Uvádí taky některé dvojic verze mezi produkty.</span><span class="sxs-lookup"><span data-stu-id="03694-126">It also lists some pairings of versions between products.</span></span>

#### <a name="adonet"></a><span data-ttu-id="03694-127">ADO.NET</span><span class="sxs-lookup"><span data-stu-id="03694-127">ADO.NET</span></span>
* <span data-ttu-id="03694-128">ADO.NET 4.0 podporuje protokol hello TDS 7.3, ale není 7.4.</span><span class="sxs-lookup"><span data-stu-id="03694-128">ADO.NET 4.0 supports hello TDS 7.3 protocol, but not 7.4.</span></span>
* <span data-ttu-id="03694-129">ADO.NET 4.5 a novější podporuje protokol TDS 7.4 hello.</span><span class="sxs-lookup"><span data-stu-id="03694-129">ADO.NET 4.5 and later supports hello TDS 7.4 protocol.</span></span>

## <a name="related-links"></a><span data-ttu-id="03694-130">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="03694-130">Related links</span></span>
* <span data-ttu-id="03694-131">ADO.NET 4.6 byla vydána 20. července 2015.</span><span class="sxs-lookup"><span data-stu-id="03694-131">ADO.NET 4.6 was released on July 20, 2015.</span></span> <span data-ttu-id="03694-132">Blog sdělení od týmu .NET hello je k dispozici [zde](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).</span><span class="sxs-lookup"><span data-stu-id="03694-132">A blog announcement from hello .NET team is available [here](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).</span></span>
* <span data-ttu-id="03694-133">ADO.NET 4.5 byla vydána 15 srpen 2012.</span><span class="sxs-lookup"><span data-stu-id="03694-133">ADO.NET 4.5 was released on August 15, 2012.</span></span> <span data-ttu-id="03694-134">Blog sdělení od týmu .NET hello je k dispozici [zde](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).</span><span class="sxs-lookup"><span data-stu-id="03694-134">A blog announcement from hello .NET team is available [here](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).</span></span>
  
  * <span data-ttu-id="03694-135">Příspěvek blogu o ADO.NET 4.5.1 je k dispozici [zde](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).</span><span class="sxs-lookup"><span data-stu-id="03694-135">A blog post about ADO.NET 4.5.1 is available [here](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).</span></span>
* [<span data-ttu-id="03694-136">Seznam verzí protokolu TDS.</span><span class="sxs-lookup"><span data-stu-id="03694-136">TDS protocol version list</span></span>](http://www.freetds.org/userguide/tdshistory.htm)
* [<span data-ttu-id="03694-137">Přehled vývoje SQL databáze</span><span class="sxs-lookup"><span data-stu-id="03694-137">SQL Database Development Overview</span></span>](sql-database-develop-overview.md)
* [<span data-ttu-id="03694-138">Azure SQL Database brány firewall</span><span class="sxs-lookup"><span data-stu-id="03694-138">Azure SQL Database firewall</span></span>](sql-database-firewall-configure.md)
* [<span data-ttu-id="03694-139">Postupy: Konfigurace nastavení brány firewall pro službu SQL Database</span><span class="sxs-lookup"><span data-stu-id="03694-139">How to: Configure firewall settings on SQL Database</span></span>](sql-database-configure-firewall-settings.md)

