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
# <a name="ports-beyond-1433-for-adonet-45"></a>Porty nad rámec 1433 pro technologii ADO.NET 4.5
Toto téma popisuje chování připojení hello Azure SQL Database pro klienty, kteří používají ADO.NET 4.5 nebo novější. 

> [!IMPORTANT]
> Informace o architektuře připojení najdete v tématu [architektura připojení k databázi SQL Azure](sql-database-connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Mimo vs uvnitř
Pro připojení tooAzure SQL Database, musíte nejprve požádáme zda váš klientský program běží *mimo* nebo *uvnitř* hranic hello cloudu Azure. Hello témata popisují dvě běžné scénáře.

#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Mimo:* klient spustí ve stolním počítači
Port 1433 je hello jediný port, který musí být otevřený v počítači, který je hostitelem klientské aplikace SQL Database.

#### <a name="inside-client-runs-on-azure"></a>*Uvnitř:* klienta běží na Azure
Váš klient běží v rámci hranic hello cloudu Azure, používá, můžete takzvaný *přímé trasy* toointeract hello databáze SQL serveru. Po vytvoření připojení, další interakce mezi hello klienta a databáze zahrnovat žádný proxy server middleware.

pořadí Hello vypadá takto:

1. ADO.NET 4.5 (nebo novější) zahájí stručný interakci s hello cloudu Azure a přijímá číslo dynamicky identifikovaných portu.
   
   * číslo portu Hello dynamicky identifikovat je v rozsahu hello 11000 11999 nebo 14000 14999.
2. ADO.NET pak připojí toohello databáze SQL server přímo, bez middlewaru v rozmezí.
3. Dotazy jsou odesílány přímo toohello databáze a výsledky se vrátí přímo toohello klienta.

Ujistěte se, že hello port, který rozsahů 11000 11999 a 14000 14999 na Azure klientský počítač zůstává k dispozici pro technologii ADO.NET 4.5 klienta interakce s databází SQL.

* Porty v rozsahu hello konkrétně musí být bezplatné jiné odchozí blokování.
* Na vašem virtuálním počítači Azure hello **brány Windows Firewall s pokročilým zabezpečením** ovládací prvky hello nastavení portu.
  
  * Můžete použít hello [brány firewall na uživatelské rozhraní](http://msdn.microsoft.com/library/cc646023.aspx) tooadd a pravidlo, pro který nastavíte hello **TCP** jako protokol společně s rozsah portů se syntaxí hello **11000 11999**.

## <a name="version-clarifications"></a>Objasnění, která verze
Tato část vysvětluje hello zástupných názvů, které odkazují tooproduct verze. Uvádí taky některé dvojic verze mezi produkty.

#### <a name="adonet"></a>ADO.NET
* ADO.NET 4.0 podporuje protokol hello TDS 7.3, ale není 7.4.
* ADO.NET 4.5 a novější podporuje protokol TDS 7.4 hello.

## <a name="related-links"></a>Související odkazy
* ADO.NET 4.6 byla vydána 20. července 2015. Blog sdělení od týmu .NET hello je k dispozici [zde](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).
* ADO.NET 4.5 byla vydána 15 srpen 2012. Blog sdělení od týmu .NET hello je k dispozici [zde](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
  
  * Příspěvek blogu o ADO.NET 4.5.1 je k dispozici [zde](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).
* [Seznam verzí protokolu TDS.](http://www.freetds.org/userguide/tdshistory.htm)
* [Přehled vývoje SQL databáze](sql-database-develop-overview.md)
* [Azure SQL Database brány firewall](sql-database-firewall-configure.md)
* [Postupy: Konfigurace nastavení brány firewall pro službu SQL Database](sql-database-configure-firewall-settings.md)

