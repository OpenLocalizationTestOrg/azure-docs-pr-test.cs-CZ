---
title: "Knihovny připojení pro databázi SQL. | Microsoft Docs"
description: "Obsahuje odkazy pro stahování modulů, které umožňují připojení k systému SQL Server a databáze SQL z mnoha rozličných programovacích jazyků klienta. Moduly jsou vydávány komunitou nebo společností Microsoft."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: genemi
ms.assetid: 13d899d3-cf46-4e4d-8919-cf4b41ca836d
ms.service: sql-database
ms.custom: develop apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: genemi
ms.openlocfilehash: 082abf57b139b9f7d44774dce3a80e20b97f0e3b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connectivity-libraries-and-frameworks-for-microsoft-sql-server"></a>Připojení knihoven a architektur pro Microsoft SQL Server

Podívejte se na naše [získávání kurzů](http://aka.ms/sqldev) rychle začít pracovat s programovacích jazyků, například C#, Java, Node.js, PHP a Python a sestavení aplikace pomocí systému SQL Server v systému Linux nebo Windows nebo Docker v systému macOS.

Následující tabulka uvádí připojení knihovny nebo *ovladače* používaný klientských aplikací z různých jazyků pro připojení k a používat Microsoft SQL Server spuštěn místně nebo v cloudu, na systému Linux, Windows nebo Docker a také pro Azure SQL Database a Azure SQL Data Warehouse. 

| Jazyk | Platforma | Další zdroje | Ke stažení | Začínáme |
| :-- | :-- | :-- | :-- | :-- |
| C# | Systému Windows, Linux, macOS | [Microsoft ADO.NET pro SQL Server](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [Stáhnout](https://www.microsoft.com/net/download/) | [Začínáme](https://www.microsoft.com/en-us/sql-server/developer-get-started/csharp/ubuntu)
| Java | Systému Windows, Linux, macOS | [Ovladač Microsoft JDBC pro SQL Server](http://msdn.microsoft.com/library/mt484311.aspx) | [Stáhnout](https://go.microsoft.com/fwlink/?linkid=852460) |  [Začínáme](https://www.microsoft.com/en-us/sql-server/developer-get-started/java/ubuntu)
| PHP | Systému Windows, Linux, macOS| [Ovladač systému PHP SQL pro SQL Server](http://msdn.microsoft.com/library/dn865013.aspx) | Operační systém: <br/> \*[Windows](https://www.microsoft.com/download/details.aspx?id=20098) <br/> \*[Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \*[systému macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Začínáme](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/ubuntu)
| Node.js | Systému Windows, Linux, macOS | [Node.js ovladač pro systém SQL Server](http://msdn.microsoft.com/library/mt652093.aspx) | [Instalace](https://msdn.microsoft.com/library/mt652094.aspx) |  [Začínáme](https://www.microsoft.com/en-us/sql-server/developer-get-started/node/ubuntu)
| Python | Systému Windows, Linux, macOS | [Ovladač systému SQL Python](http://msdn.microsoft.com/library/mt652092.aspx) | Instalace volby: <br/> \*[pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \*[pyodbc](http://msdn.microsoft.com/library/mt763257.aspx) |  [Začínáme](https://www.microsoft.com/en-us/sql-server/developer-get-started/python/ubuntu)
| Ruby | Systému Windows, Linux, macOS | [Poznámky Ruby ovladač pro systém SQL Server](http://msdn.microsoft.com/library/mt691981.aspx) | [Instalace](https://msdn.microsoft.com/library/mt711041.aspx) | [Začínáme](https://www.microsoft.com/en-us/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Systému Windows, Linux, macOS | [Ovladač Microsoft ODBC pro SQL Server](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) | [Stáhnout](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) |  

Následující tabulka uvádí několik příkladů architektury relační mapování ORM (Object) a webové platformy, které klientské aplikace můžete použít s Microsoft SQL Server spuštěn místně nebo v cloudu, na systému Linux, Windows nebo Docker a také pro Azure SQL Database a Azure SQL Data Warehouse. 

| Jazyk | Platforma | ORM(s) |
| :-- | :-- | :-- |
| C# | Systému Windows, Linux, macOS | [Entity Framework](https://docs.microsoft.com/en-us/ef)<br>[Základní Entity Framework](https://docs.microsoft.com/en-us/ef/core/index) |
| Java | Systému Windows, Linux, macOS |[Hibernace ORM](http://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Systému Windows, Linux, macOS | [Sequelize ORM](http://docs.sequelizejs.com) |
| Python | Systému Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Systému Windows, Linux, macOS | [Ruby, na které](http://rubyonrails.org/) |

## <a name="related-links"></a>Související odkazy
- [SQL Server ovladače](http://msdn.microsoft.com/library/mt654049.aspx) pro připojení z klientské aplikace
- [Připojení k SQL Database s použitím rozhraní .NET (C#)](sql-database-connect-query-dotnet.md)
- [Připojení k SQL Database s použitím jazyka PHP](sql-database-connect-query-php.md)
- [Připojení k SQL Database s použitím prostředí Node.js](sql-database-connect-query-nodejs.md)
- [Připojení k SQL Database s použitím jazyka Java](sql-database-connect-query-java.md)
- [Připojení k SQL Database s použitím jazyka Python](sql-database-connect-query-python.md)
- [Připojení k SQL Database s použitím prostředí Ruby](sql-database-connect-query-ruby.md)
