---
title: "Výpis stavu a obnovení v Azure databázi pro PostgreSQL | Microsoft Docs"
description: "Popisuje, jak extrahovat databázi PostgreSQL do souboru výpisu a obnovit databázi PostgreSQL ze souboru archivu vytvořené pg_dump v databázi Azure pro PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/14/2017
ms.openlocfilehash: 190373c4980b67e16b58700e4b7e65658545c615
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a><span data-ttu-id="1daf7-103">PostgreSQL databázi migrujte pomocí výpisu a obnovení</span><span class="sxs-lookup"><span data-stu-id="1daf7-103">Migrate your PostgreSQL database using dump and restore</span></span>
<span data-ttu-id="1daf7-104">Můžete použít [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) extrahovat databázi PostgreSQL do souboru výpisu a [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) ze souboru archivu vytvořené pg_dump obnovit databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="1daf7-104">You can use [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) to extract a PostgreSQL database into a dump file and [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) to restore the PostgreSQL database from an archive file created by pg_dump.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1daf7-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1daf7-105">Prerequisites</span></span>
<span data-ttu-id="1daf7-106">Chcete-li krok tímto průvodcem postupy, je třeba:</span><span class="sxs-lookup"><span data-stu-id="1daf7-106">To step through this how-to guide, you need:</span></span>
- <span data-ttu-id="1daf7-107">[Databáze Azure pro PostgreSQL server](quickstart-create-server-database-portal.md) se pravidla brány firewall umožňující přístup a databáze v něm.</span><span class="sxs-lookup"><span data-stu-id="1daf7-107">An [Azure Database for PostgreSQL server](quickstart-create-server-database-portal.md) with firewall rules to allow access and database under it.</span></span>
- <span data-ttu-id="1daf7-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) a [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) nainstalované nástroje příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="1daf7-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) and [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) command-line utilities installed</span></span>

<span data-ttu-id="1daf7-109">Postupujte podle těchto kroků dump a obnovit databázi PostgreSQL:</span><span class="sxs-lookup"><span data-stu-id="1daf7-109">Follow these steps to dump and restore your PostgreSQL database:</span></span>

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a><span data-ttu-id="1daf7-110">Vytvoření souboru výpisu pomocí pg_dump, který obsahuje data, která mají být načten</span><span class="sxs-lookup"><span data-stu-id="1daf7-110">Create a dump file using pg_dump that contains the data to be loaded</span></span>
<span data-ttu-id="1daf7-111">Chcete-li zálohovat stávající PostgreSQL databáze místní nebo ve virtuálním počítači, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1daf7-111">To back up an existing PostgreSQL database on-premises or in a VM, run the following command:</span></span>
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
<span data-ttu-id="1daf7-112">Například, pokud máte místní server a databáze názvem **testdb** v ní</span><span class="sxs-lookup"><span data-stu-id="1daf7-112">For example, if you have a local server and a database called **testdb** in it</span></span>
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a><span data-ttu-id="1daf7-113">Obnovení dat do cílové databáze Azure pro PostrgeSQL pomocí pg_restore</span><span class="sxs-lookup"><span data-stu-id="1daf7-113">Restore the data into the target Azure Database for PostrgeSQL using pg_restore</span></span>
<span data-ttu-id="1daf7-114">Po vytvoření cílové databázi, můžete použít příkaz pg_restore a -d, parametr dbname – Chcete-li obnovit data do cílové databáze ze souboru s výpisem stavu.</span><span class="sxs-lookup"><span data-stu-id="1daf7-114">Once you have created the target database, you can use the pg_restore command and the -d, --dbname parameter to restore the data into the target database from the dump file.</span></span>
```bash
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
<span data-ttu-id="1daf7-115">V tomto příkladu, obnovte data ze souboru s výpisem **testdb.dump** do databáze **mypgsqldb** na cílovém serveru **mypgserver 20170401.postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="1daf7-115">In this example, restore the data from the dump file **testdb.dump** into the database **mypgsqldb** on target server **mypgserver-20170401.postgres.database.azure.com**.</span></span>
```bash
pg_restore -v --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a><span data-ttu-id="1daf7-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1daf7-116">Next steps</span></span>
- <span data-ttu-id="1daf7-117">K migraci databázi PostgreSQL pomocí exportu a importu, najdete v části [PostgreSQL databázi migrujte pomocí exportu a importu](howto-migrate-using-export-and-import.md)</span><span class="sxs-lookup"><span data-stu-id="1daf7-117">To migrate a PostgreSQL database using export and import, see [Migrate your PostgreSQL database using export and import](howto-migrate-using-export-and-import.md)</span></span>