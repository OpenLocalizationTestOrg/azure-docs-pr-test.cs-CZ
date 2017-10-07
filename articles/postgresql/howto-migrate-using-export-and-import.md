---
title: "aaaMigrate databázi pomocí Import a Export v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Popisuje, jak extrahovat databázi PostgreSQL do souboru skriptu a importovat hello data do hello cílová databáze z tohoto souboru."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/14/2017
ms.openlocfilehash: 5ca4650782b02e1067c5a8a3710f2dfbc8c76063
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>PostgreSQL databázi migrujte pomocí exportu a importu
Můžete použít [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract databázi PostgreSQL do souboru skriptu a [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooimport hello data do databáze cílové hello z tohoto souboru.

## <a name="prerequisites"></a>Požadavky
toostep prostřednictvím této jak tooguide, budete potřebovat:
- [Databáze Azure pro PostgreSQL server](quickstart-create-server-database-portal.md) s přístup tooallow pravidla brány firewall a databáze v něm.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) nainstalovaný nástroj příkazového řádku
- [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) nainstalovaný nástroj příkazového řádku

Postupujte podle těchto kroků tooexport a importovat databázi PostgreSQL.

## <a name="create-a-script-file-using-pgdump-that-contains-hello-data-toobe-loaded"></a>Vytvoření souboru skriptu pomocí pg_dump, který obsahuje toobe hello data načíst
tooexport vaší existující PostgreSQL databáze místně nebo v souboru skriptu sql tooa virtuálního počítače spusťte následující příkaz v prostředí existující hello:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Například, pokud máte místní server a databáze názvem **testdb** v ní
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-hello-data-on-target-azure-database-for-postrgesql"></a>Import dat hello na cíli databáze Azure pro PostrgeSQL
Můžete použít hello psql příkazového řádku a hello -d, – dbname parametr tooimport hello data do databáze Azure pro PostrgeSQL jsme vytvořili a načtení dat ze souboru sql hello.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
Tento příklad používá nástroj psql a souboru skriptu s názvem **testdb.sql** z předchozí krok tooimport dat do databáze hello **mypgsqldb** na cílovém serveru ** mypgserver 20170401.postgres.database.azure.com**.
```bash
psql --file=testdb.sql --host=mypgserver-20170401.database.windows.net --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb
```

## <a name="next-steps"></a>Další kroky
- najdete v databázi PostgreSQL pomocí výpisu a obnovení, toomigrate [PostgreSQL databázi migrujte pomocí výpisu a obnovení](howto-migrate-using-dump-and-restore.md)
