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
# <a name="ports-beyond-1433-for-adonet-45"></a>Porty nad rámec 1433 pro technologii ADO.NET 4.5
Toto téma popisuje chování připojení databáze SQL Azure pro klienty, kteří používají ADO.NET 4.5 nebo novější. 

> [!IMPORTANT]
> Informace o architektuře připojení najdete v tématu [architektura připojení k databázi SQL Azure](sql-database-connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Mimo vs uvnitř
Pro připojení k databázi SQL Azure, musíte nejprve požádáme zda váš klientský program běží *mimo* nebo *uvnitř* hranic cloudu Azure. Témata popisují dvě běžné scénáře.

#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Mimo:* klient spustí ve stolním počítači
Port 1433 je jediný port, který musí být otevřený v počítači, který je hostitelem klientské aplikace SQL Database.

#### <a name="inside-client-runs-on-azure"></a>*Uvnitř:* klienta běží na Azure
Váš klient běží v rámci hranic cloudu Azure, používá, můžete takzvaný *přímé trasy* komunikovat se serverem SQL Database. Po vytvoření připojení, další interakce mezi klientem a databáze zahrnovat žádný proxy server middleware.

Pořadí je následující:

1. ADO.NET 4.5 (nebo novější) zahájí stručný interakci s cloudu Azure a přijímá číslo dynamicky identifikovaných portu.
   
   * Číslo portu dynamicky identifikovaných je v rozsahu 11000 11999 nebo 14000 14999.
2. ADO.NET pak připojí k serveru služby SQL Database přímo, s žádné middlewaru v rozmezí.
3. Dotazy jsou odesílány přímo do databáze a výsledky se vrátí přímo do klienta.

Zajistěte, aby port, který rozsahů 11000 11999 a 14000 14999 na Azure klientský počítač zůstává k dispozici pro technologii ADO.NET 4.5 klienta interakce s databází SQL.

* Porty v rozsahu musí být konkrétně bezplatné jiné odchozí blokování.
* Na vašem virtuálním počítači Azure **brány Windows Firewall s pokročilým zabezpečením** řídí nastavení portu.
  
  * Můžete použít [brány firewall na uživatelské rozhraní](http://msdn.microsoft.com/library/cc646023.aspx) přidat pravidlo, pro který nastavíte **TCP** protokolu společně s rozsah portů se syntaxí, jako **11000 11999**.

## <a name="version-clarifications"></a>Objasnění, která verze
Tato část vysvětluje zástupných názvů, které odkazují na verze produktu. Uvádí taky některé dvojic verze mezi produkty.

#### <a name="adonet"></a>ADO.NET
* ADO.NET 4.0 podporuje protokol TDS 7.3, ale není 7.4.
* ADO.NET 4.5 a novější podporuje protokol TDS 7.4.

## <a name="related-links"></a>Související odkazy
* ADO.NET 4.6 byla vydána 20. července 2015. Blog sdělení od týmu .NET je k dispozici [zde](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).
* ADO.NET 4.5 byla vydána 15 srpen 2012. Blog sdělení od týmu .NET je k dispozici [zde](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
  
  * Příspěvek blogu o ADO.NET 4.5.1 je k dispozici [zde](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).
* [Seznam verzí protokolu TDS.](http://www.freetds.org/userguide/tdshistory.htm)
* [Přehled vývoje SQL databáze](sql-database-develop-overview.md)
* [Azure SQL Database brány firewall](sql-database-firewall-configure.md)
* [Postupy: Konfigurace nastavení brány firewall pro službu SQL Database](sql-database-configure-firewall-settings.md)

