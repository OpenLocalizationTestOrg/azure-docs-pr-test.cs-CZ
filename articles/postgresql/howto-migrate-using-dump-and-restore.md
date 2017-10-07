---
title: "aaaHow tooDump a obnovení v Azure databázi PostgreSQL | Microsoft Docs"
description: "Popisuje, jak tooextract PostgreSQL databáze do souboru výpisu a obnovte databázi PostgreSQL hello ze souboru archivu vytvořené pg_dump v databázi Azure pro PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/14/2017
ms.openlocfilehash: 9ad28e9dec3927b0f80b5e6bab6423cc944f5156
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>PostgreSQL databázi migrujte pomocí výpisu a obnovení
Můžete použít [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract databázi PostgreSQL do souboru výpisu a [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) toorestore hello PostgreSQL databáze ze souboru archivu vytvořené pg_dump.

## <a name="prerequisites"></a>Požadavky
toostep prostřednictvím této jak tooguide, budete potřebovat:
- [Databáze Azure pro PostgreSQL server](quickstart-create-server-database-portal.md) s přístup tooallow pravidla brány firewall a databáze v něm.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) a [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) nainstalované nástroje příkazového řádku

Postupujte podle těchto kroků toodump a obnovit databázi PostgreSQL:

## <a name="create-a-dump-file-using-pgdump-that-contains-hello-data-toobe-loaded"></a>Vytvoření souboru výpisu pomocí pg_dump, který obsahuje toobe hello data načíst
tooback až existující PostgreSQL databáze místně nebo ve virtuálním počítači, spusťte následující příkaz hello:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Například, pokud máte místní server a databáze názvem **testdb** v ní
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-hello-data-into-hello-target-azure-database-for-postrgesql-using-pgrestore"></a>Obnovení dat hello do hello cílové databáze Azure pro PostrgeSQL pomocí pg_restore
Jakmile vytvoříte hello cílová databáze, můžete použít příkaz pg_restore hello a hello -d, – dbname parametr toorestore hello data do hello cílová databáze ze souboru s výpisem hello.
```bash
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
V tomto příkladu obnovit hello data ze souboru s výpisem hello **testdb.dump** do databáze hello **mypgsqldb** na cílovém serveru **mypgserver-20170401.postgres.database.azure.com**.
```bash
pg_restore -v --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Další kroky
- najdete v databázi PostgreSQL pomocí exportu a importu, toomigrate [PostgreSQL databázi migrujte pomocí exportu a importu](howto-migrate-using-export-and-import.md)
