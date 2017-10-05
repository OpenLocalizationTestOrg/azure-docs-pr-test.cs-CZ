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
ms.date: 06/14/2017
ms.openlocfilehash: 5e306d516d04789e4526bfd09bf99139b83573ba
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a><span data-ttu-id="362f7-103">PostgreSQL databázi migrujte pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="362f7-103">Migrate your PostgreSQL database using export and import</span></span>
<span data-ttu-id="362f7-104">Můžete použít [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) extrahovat databázi PostgreSQL do souboru skriptu a [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) pro import dat do cílové databáze z tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="362f7-104">You can use [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) to extract a PostgreSQL database into a script file and [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) to import the data into the target database from that file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="362f7-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="362f7-105">Prerequisites</span></span>
<span data-ttu-id="362f7-106">Chcete-li krok tímto průvodcem postupy, je třeba:</span><span class="sxs-lookup"><span data-stu-id="362f7-106">To step through this how-to guide, you need:</span></span>
- <span data-ttu-id="362f7-107">[Databáze Azure pro PostgreSQL server](quickstart-create-server-database-portal.md) se pravidla brány firewall umožňující přístup a databáze v něm.</span><span class="sxs-lookup"><span data-stu-id="362f7-107">An [Azure Database for PostgreSQL server](quickstart-create-server-database-portal.md) with firewall rules to allow access and database under it.</span></span>
- <span data-ttu-id="362f7-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) nainstalovaný nástroj příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="362f7-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) command-line utility installed</span></span>
- <span data-ttu-id="362f7-109">[psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) nainstalovaný nástroj příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="362f7-109">[psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) command-line utility installed</span></span>

<span data-ttu-id="362f7-110">Postupujte podle těchto kroků pro export a import databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="362f7-110">Follow these steps to export and import your PostgreSQL database.</span></span>

## <a name="create-a-script-file-using-pgdump-that-contains-the-data-to-be-loaded"></a><span data-ttu-id="362f7-111">Vytvoření souboru skriptu pomocí pg_dump, který obsahuje data, která mají být načten</span><span class="sxs-lookup"><span data-stu-id="362f7-111">Create a script file using pg_dump that contains the data to be loaded</span></span>
<span data-ttu-id="362f7-112">K exportu existující PostgreSQL databáze místní nebo do virtuálního počítače do souboru skriptu sql, spusťte následující příkaz v vaše stávající prostředí:</span><span class="sxs-lookup"><span data-stu-id="362f7-112">To export your existing PostgreSQL database on-premises or in a VM to a sql script file, run the following command in your existing environment:</span></span>
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
<span data-ttu-id="362f7-113">Například, pokud máte místní server a databáze názvem **testdb** v ní</span><span class="sxs-lookup"><span data-stu-id="362f7-113">For example, if you have a local server and a database called **testdb** in it</span></span>
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postrgesql"></a><span data-ttu-id="362f7-114">Umožňuje importovat data na cílové databázi Azure pro PostrgeSQL</span><span class="sxs-lookup"><span data-stu-id="362f7-114">Import the data on target Azure Database for PostrgeSQL</span></span>
<span data-ttu-id="362f7-115">Můžete použít psql příkazového řádku a -d, – dbname parametr pro import dat do databáze Azure pro PostrgeSQL jsme vytvořili a načtení dat ze souboru sql.</span><span class="sxs-lookup"><span data-stu-id="362f7-115">You can use the psql command line and the -d, --dbname parameter to import the data into Azure Database for PostrgeSQL we created and load data from the sql file.</span></span>
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
<span data-ttu-id="362f7-116">Tento příklad používá nástroj psql a souboru skriptu s názvem **testdb.sql** z předchozího kroku k importu dat do databáze **mypgsqldb** na cílovém serveru **mypgserver 20170401.postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="362f7-116">This example uses psql utility and a script file named **testdb.sql** from previous step to import data into the database **mypgsqldb** on target server **mypgserver-20170401.postgres.database.azure.com**.</span></span>
```bash
psql --file=testdb.sql --host=mypgserver-20170401.database.windows.net --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb
```

## <a name="next-steps"></a><span data-ttu-id="362f7-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="362f7-117">Next steps</span></span>
- <span data-ttu-id="362f7-118">K migraci databázi PostgreSQL pomocí výpisu a obnovení, najdete v části [PostgreSQL databázi migrujte pomocí výpisu a obnovení](howto-migrate-using-dump-and-restore.md)</span><span class="sxs-lookup"><span data-stu-id="362f7-118">To migrate a PostgreSQL database using dump and restore, see [Migrate your PostgreSQL database using dump and restore](howto-migrate-using-dump-and-restore.md)</span></span>