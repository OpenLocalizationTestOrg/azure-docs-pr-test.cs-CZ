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
# <a name="migrate-your-postgresql-database-using-export-and-import"></a><span data-ttu-id="c56b7-103">PostgreSQL databázi migrujte pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="c56b7-103">Migrate your PostgreSQL database using export and import</span></span>
<span data-ttu-id="c56b7-104">Můžete použít [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract databázi PostgreSQL do souboru skriptu a [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooimport hello data do databáze cílové hello z tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="c56b7-104">You can use [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract a PostgreSQL database into a script file and [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooimport hello data into hello target database from that file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c56b7-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c56b7-105">Prerequisites</span></span>
<span data-ttu-id="c56b7-106">toostep prostřednictvím této jak tooguide, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="c56b7-106">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="c56b7-107">[Databáze Azure pro PostgreSQL server](quickstart-create-server-database-portal.md) s přístup tooallow pravidla brány firewall a databáze v něm.</span><span class="sxs-lookup"><span data-stu-id="c56b7-107">An [Azure Database for PostgreSQL server](quickstart-create-server-database-portal.md) with firewall rules tooallow access and database under it.</span></span>
- <span data-ttu-id="c56b7-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) nainstalovaný nástroj příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="c56b7-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) command-line utility installed</span></span>
- <span data-ttu-id="c56b7-109">[psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) nainstalovaný nástroj příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="c56b7-109">[psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) command-line utility installed</span></span>

<span data-ttu-id="c56b7-110">Postupujte podle těchto kroků tooexport a importovat databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="c56b7-110">Follow these steps tooexport and import your PostgreSQL database.</span></span>

## <a name="create-a-script-file-using-pgdump-that-contains-hello-data-toobe-loaded"></a><span data-ttu-id="c56b7-111">Vytvoření souboru skriptu pomocí pg_dump, který obsahuje toobe hello data načíst</span><span class="sxs-lookup"><span data-stu-id="c56b7-111">Create a script file using pg_dump that contains hello data toobe loaded</span></span>
<span data-ttu-id="c56b7-112">tooexport vaší existující PostgreSQL databáze místně nebo v souboru skriptu sql tooa virtuálního počítače spusťte následující příkaz v prostředí existující hello:</span><span class="sxs-lookup"><span data-stu-id="c56b7-112">tooexport your existing PostgreSQL database on-premises or in a VM tooa sql script file, run hello following command in your existing environment:</span></span>
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
<span data-ttu-id="c56b7-113">Například, pokud máte místní server a databáze názvem **testdb** v ní</span><span class="sxs-lookup"><span data-stu-id="c56b7-113">For example, if you have a local server and a database called **testdb** in it</span></span>
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-hello-data-on-target-azure-database-for-postrgesql"></a><span data-ttu-id="c56b7-114">Import dat hello na cíli databáze Azure pro PostrgeSQL</span><span class="sxs-lookup"><span data-stu-id="c56b7-114">Import hello data on target Azure Database for PostrgeSQL</span></span>
<span data-ttu-id="c56b7-115">Můžete použít hello psql příkazového řádku a hello -d, – dbname parametr tooimport hello data do databáze Azure pro PostrgeSQL jsme vytvořili a načtení dat ze souboru sql hello.</span><span class="sxs-lookup"><span data-stu-id="c56b7-115">You can use hello psql command line and hello -d, --dbname parameter tooimport hello data into Azure Database for PostrgeSQL we created and load data from hello sql file.</span></span>
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
<span data-ttu-id="c56b7-116">Tento příklad používá nástroj psql a souboru skriptu s názvem **testdb.sql** z předchozí krok tooimport dat do databáze hello **mypgsqldb** na cílovém serveru ** mypgserver 20170401.postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="c56b7-116">This example uses psql utility and a script file named **testdb.sql** from previous step tooimport data into hello database **mypgsqldb** on target server **mypgserver-20170401.postgres.database.azure.com**.</span></span>
```bash
psql --file=testdb.sql --host=mypgserver-20170401.database.windows.net --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb
```

## <a name="next-steps"></a><span data-ttu-id="c56b7-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c56b7-117">Next steps</span></span>
- <span data-ttu-id="c56b7-118">najdete v databázi PostgreSQL pomocí výpisu a obnovení, toomigrate [PostgreSQL databázi migrujte pomocí výpisu a obnovení](howto-migrate-using-dump-and-restore.md)</span><span class="sxs-lookup"><span data-stu-id="c56b7-118">toomigrate a PostgreSQL database using dump and restore, see [Migrate your PostgreSQL database using dump and restore](howto-migrate-using-dump-and-restore.md)</span></span>
