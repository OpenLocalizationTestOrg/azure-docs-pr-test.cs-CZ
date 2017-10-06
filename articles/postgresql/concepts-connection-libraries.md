---
title: "Knihovny připojení pro databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Tento článek popisuje několik knihovny a ovladače, které mohou vývojáři použít při kódování tooconnect aplikace a dotaz databáze Azure pro PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/15/2017
ms.openlocfilehash: 1f7234499d8abe37f8de9008e3158765b1fb0bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connection-libraries-for-azure-database-for-postgresql"></a>Knihovny připojení pro databázi Azure pro PostgreSQL
Toto téma obsahuje seznam knihoven a ovladače pro použití pro vývojáře pro programování aplikací tooconnect a dotaz Azure databázi PostgreSQL.

## <a name="client-interfaces"></a>Rozhraní klienta
Většina jazyk klienta knihovny tooconnect tooPostgreSQL serveru jsou externí projekty a distribuují nezávisle. Tyto jsou podporovány na platformách systému Windows, Linuxu a Macu. Některé ovladače hello oblíbených klienta, jsou uvedeny:

| **Jazyk** | **Rozhraní klienta** | **Další informace** | **Stáhnout** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | Rozhraní API DB 2.0 kompatibilní | [Stáhnout](http://initd.org/psycopg/download/) |
| PHP | [PHP pgsql](https://php.net/manual/en/book.pgsql.php) | Rozšíření databáze | [Instalace](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [Balíčku npm PG](https://www.npmjs.com/package/pg) | Čistý JavaScript neblokující klienta | [Instalace](https://www.npmjs.com/package/pg) |
| Java | [JDBC](http://jdbc.postgresql.org/) | Ovladač JDBC typu 4 | [Stáhnout](https://jdbc.postgresql.org/download.html)  |
| Ruby | [PG gem](https://deveiate.org/code/pg/) | Poznámky Ruby rozhraní | [Stáhnout](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Přejít | [Balíček pq](https://godoc.org/github.com/lib/pq) | Čistý přejděte postgres ovladačů | [Instalace](https://github.com/lib/pq/blob/master/README.md) |
| C\#NEBO ROZHRANÍ .NET | [Npgsql](http://www.npgsql.org/) | Zprostředkovatel dat ADO.NET | [Stáhnout](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | Ovladač ODBC | [Stáhnout](http://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Primární rozhraní jazyka C | Zahrnuje |
| C++ | [libpqxx](http://pqxx.org/) | Nový styl rozhraní C++ | [Stáhnout](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Další kroky
Přečtěte si tyto elementy QuickStart o tooconnect a dotaz Azure databázi PostgreSQL pomocí vámi zvolený jazyk:

[Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [.NET (C#)](./connect-csharp.md)
