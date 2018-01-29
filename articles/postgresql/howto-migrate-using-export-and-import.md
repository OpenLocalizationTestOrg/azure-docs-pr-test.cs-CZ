---
title: "Migrace databáze pomocí importu a exportu v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Popisuje, jak extrahovat databázi PostgreSQL do souboru skriptu a importovat data do cílové databáze z tohoto souboru."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/03/2017
ms.openlocfilehash: ddbfd9ef8b2ae4c3c851afc18b010b234b654c81
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/13/2018
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>PostgreSQL databázi migrujte pomocí exportu a importu
Můžete použít [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) extrahovat databázi PostgreSQL do souboru skriptu a [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) pro import dat do cílové databáze z tohoto souboru.

## <a name="prerequisites"></a>Požadavky
Chcete-li krok tímto průvodcem postupy, je třeba:
- [Databáze Azure pro PostgreSQL server](quickstart-create-server-database-portal.md) se pravidla brány firewall umožňující přístup a databáze v něm.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) nainstalovaný nástroj příkazového řádku
- [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) nainstalovaný nástroj příkazového řádku

Postupujte podle těchto kroků pro export a import databázi PostgreSQL.

## <a name="create-a-script-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Vytvoření souboru skriptu pomocí pg_dump, který obsahuje data, která mají být načten
K exportu existující PostgreSQL databáze místní nebo do virtuálního počítače do souboru skriptu sql, spusťte následující příkaz v vaše stávající prostředí:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Například, pokud máte místní server a databáze názvem **testdb** v něm:
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postgresql"></a>Umožňuje importovat data na cílové databázi Azure pro PostgreSQL
Pomocí příkazového řádku psql a parametr – dbname (-d) pro import dat do databáze Azure pro PostgreSQL serveru a načtení dat ze souboru sql.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
Tento příklad používá nástroj psql a souboru skriptu s názvem **testdb.sql** z předchozího kroku k importu dat do databáze **mypgsqldb** na cílovém serveru  **mypgserver 20170401.postgres.database.azure.com**.
```bash
psql --file=testdb.sql --host=mypgserver-20170401.database.windows.net --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb
```

## <a name="next-steps"></a>Další postup
- K migraci databázi PostgreSQL pomocí výpisu a obnovení, najdete v části [PostgreSQL databázi migrujte pomocí výpisu a obnovení](howto-migrate-using-dump-and-restore.md)
