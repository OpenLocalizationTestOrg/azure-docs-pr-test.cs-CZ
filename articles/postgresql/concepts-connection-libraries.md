---
title: "Knihovny připojení pro databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Tento článek popisuje několik knihovny a ovladače, vývojáři mohou použít při kódování aplikace k připojení a dotazování databáze Azure pro PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/15/2017
ms.openlocfilehash: 1f99ef7fefb1ff9d35f564a1f0ad77c8dd64659e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connection-libraries-for-azure-database-for-postgresql"></a>Knihovny připojení pro databázi Azure pro PostgreSQL
Toto téma obsahuje seznam knihoven a ovladače pro použití pro vývojáře pro programování aplikací k připojení a dotazování databáze Azure pro PostgreSQL.

## <a name="client-interfaces"></a>Rozhraní klienta
Většina jazyk klientské knihovny pro připojení k serveru PostgreSQL jsou externí projekty a distribuují nezávisle. Tyto jsou podporovány na platformách systému Windows, Linuxu a Macu. Některé ovladače oblíbených klienta, jsou uvedeny:

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
Přečtěte si tyto elementy QuickStart o tom, jak připojení a dotazování databáze Azure pro PostgreSQL pomocí vámi zvolený jazyk:

[Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [.NET (C#)](./connect-csharp.md)
