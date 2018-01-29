---
title: "Knihovny připojení pro databázi SQL. | Microsoft Docs"
description: "Obsahuje odkazy pro stahování modulů, které umožňují připojení k systému SQL Server a databáze SQL z mnoha rozličných programovacích jazyků klienta."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: genemi
ms.assetid: 13d899d3-cf46-4e4d-8919-cf4b41ca836d
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: genemi
ms.openlocfilehash: a45393804aa5398bc0c40ca3f3cb6c97b106b81c
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="connectivity-libraries-and-frameworks-for-sql-server"></a>Připojení knihoven a architektur pro SQL Server

Podívejte se na naše [získat kurzy](http://aka.ms/sqldev) rychle začít s programovacích jazyků, například C#, Java, Node.js, PHP a Python. Potom sestavení aplikace pomocí systému SQL Server v systému Linux nebo Windows nebo Docker v systému macOS.

Následující tabulka uvádí připojení knihovny nebo *ovladače* používaný klientských aplikací z různých jazyků pro připojení k a používat SQL Server běžící na místě nebo v cloudu. Můžete je používat na systému Linux, Windows nebo Docker a použít je pro připojení k databázi SQL Azure a Azure SQL Data Warehouse. 

| Jazyk | Platforma | Další zdroje informací: | Ke stažení | Začínáme |
| :-- | :-- | :-- | :-- | :-- |
| C# | Systému Windows, Linux, macOS | [Microsoft ADO.NET pro SQL Server](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [Stáhnout](https://www.microsoft.com/net/download/) | [Začínáme](https://www.microsoft.com/en-us/sql-server/developer-get-started/csharp/ubuntu)
| Java | Systému Windows, Linux, macOS | [Ovladač Microsoft JDBC pro systém SQL Server](http://msdn.microsoft.com/library/mt484311.aspx) | [Stáhnout](https://go.microsoft.com/fwlink/?linkid=852460) |  [Začínáme](https://www.microsoft.com/en-us/sql-server/developer-get-started/java/ubuntu)
| PHP | Systému Windows, Linux, macOS| [Ovladač systému PHP SQL pro SQL Server](http://msdn.microsoft.com/library/dn865013.aspx) | Operační systém: <br/> \*[Windows](https://www.microsoft.com/download/details.aspx?id=55642) <br/> \*[Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \*[systému macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Začínáme](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/ubuntu)
| Node.js | Systému Windows, Linux, macOS | [Node.js ovladač pro systém SQL Server](http://msdn.microsoft.com/library/mt652093.aspx) | [Instalace](https://msdn.microsoft.com/library/mt652094.aspx) |  [Začínáme](https://www.microsoft.com/en-us/sql-server/developer-get-started/node/ubuntu)
| Python | Systému Windows, Linux, macOS | [Ovladač systému Python SQL](http://msdn.microsoft.com/library/mt652092.aspx) | Instalace volby: <br/> \*[pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \*[pyodbc](http://msdn.microsoft.com/library/mt763257.aspx) |  [Začínáme](https://www.microsoft.com/en-us/sql-server/developer-get-started/python/ubuntu)
| Ruby | Systému Windows, Linux, macOS | [Poznámky Ruby ovladač pro systém SQL Server](http://msdn.microsoft.com/library/mt691981.aspx) | [Instalace](https://msdn.microsoft.com/library/mt711041.aspx) | [Začínáme](https://www.microsoft.com/en-us/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Systému Windows, Linux, macOS | [Ovladač Microsoft ODBC pro SQL Server](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) | [Stáhnout](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) |  

V následující tabulce jsou uvedeny příklady architektury relační objekt mapování (ORM) a webové rozhraní, které klientské aplikace můžete použít s místní aplikace SQL Server nebo v cloudu. Můžete použít rozhraní v systému Linux, Windows nebo Docker a použít je pro připojení k SQL Database a SQL Data Warehouse. 

| Jazyk | Platforma | ORM(s) |
| :-- | :-- | :-- |
| C# | Systému Windows, Linux, macOS | [Entity Framework](https://docs.microsoft.com/ef)<br>[Základní Entity Framework](https://docs.microsoft.com/ef/core/index) |
| Java | Systému Windows, Linux, macOS |[Hibernace ORM](http://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Systému Windows, Linux, macOS | [Sequelize ORM](http://docs.sequelizejs.com) |
| Python | Systému Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Systému Windows, Linux, macOS | [Ruby, na které](http://rubyonrails.org/) |
||||

## <a name="related-links"></a>Související odkazy
- [Ovladače systému SQL Server](http://msdn.microsoft.com/library/mt654049.aspx) používané pro připojení z klientské aplikace
- Připojení k databázi SQL:
    - [Připojení k SQL Database s použitím rozhraní .NET (C#)](sql-database-connect-query-dotnet.md)
    - [Připojení k SQL Database s použitím jazyka PHP](sql-database-connect-query-php.md)
    - [Připojení k SQL Database s použitím prostředí Node.js](sql-database-connect-query-nodejs.md)
    - [Připojení k SQL Database s použitím jazyka Java](sql-database-connect-query-java.md)
    - [Připojení k SQL Database s použitím jazyka Python](sql-database-connect-query-python.md)
    - [Připojení k SQL Database s použitím prostředí Ruby](sql-database-connect-query-ruby.md)
- Příklady kódu pro logiku opakování:
    - [Pružným způsobem připojení k serveru SQL s ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
    - [Pružným způsobem připojení k serveru SQL s PHP][step-4-connect-resiliently-to-sql-with-php-p42h]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

