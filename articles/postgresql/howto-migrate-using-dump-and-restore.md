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
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a><span data-ttu-id="fe9a2-103">PostgreSQL databázi migrujte pomocí výpisu a obnovení</span><span class="sxs-lookup"><span data-stu-id="fe9a2-103">Migrate your PostgreSQL database using dump and restore</span></span>
<span data-ttu-id="fe9a2-104">Můžete použít [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract databázi PostgreSQL do souboru výpisu a [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) toorestore hello PostgreSQL databáze ze souboru archivu vytvořené pg_dump.</span><span class="sxs-lookup"><span data-stu-id="fe9a2-104">You can use [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract a PostgreSQL database into a dump file and [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) toorestore hello PostgreSQL database from an archive file created by pg_dump.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe9a2-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fe9a2-105">Prerequisites</span></span>
<span data-ttu-id="fe9a2-106">toostep prostřednictvím této jak tooguide, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="fe9a2-106">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="fe9a2-107">[Databáze Azure pro PostgreSQL server](quickstart-create-server-database-portal.md) s přístup tooallow pravidla brány firewall a databáze v něm.</span><span class="sxs-lookup"><span data-stu-id="fe9a2-107">An [Azure Database for PostgreSQL server](quickstart-create-server-database-portal.md) with firewall rules tooallow access and database under it.</span></span>
- <span data-ttu-id="fe9a2-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) a [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) nainstalované nástroje příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="fe9a2-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) and [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) command-line utilities installed</span></span>

<span data-ttu-id="fe9a2-109">Postupujte podle těchto kroků toodump a obnovit databázi PostgreSQL:</span><span class="sxs-lookup"><span data-stu-id="fe9a2-109">Follow these steps toodump and restore your PostgreSQL database:</span></span>

## <a name="create-a-dump-file-using-pgdump-that-contains-hello-data-toobe-loaded"></a><span data-ttu-id="fe9a2-110">Vytvoření souboru výpisu pomocí pg_dump, který obsahuje toobe hello data načíst</span><span class="sxs-lookup"><span data-stu-id="fe9a2-110">Create a dump file using pg_dump that contains hello data toobe loaded</span></span>
<span data-ttu-id="fe9a2-111">tooback až existující PostgreSQL databáze místně nebo ve virtuálním počítači, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="fe9a2-111">tooback up an existing PostgreSQL database on-premises or in a VM, run hello following command:</span></span>
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
<span data-ttu-id="fe9a2-112">Například, pokud máte místní server a databáze názvem **testdb** v ní</span><span class="sxs-lookup"><span data-stu-id="fe9a2-112">For example, if you have a local server and a database called **testdb** in it</span></span>
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-hello-data-into-hello-target-azure-database-for-postrgesql-using-pgrestore"></a><span data-ttu-id="fe9a2-113">Obnovení dat hello do hello cílové databáze Azure pro PostrgeSQL pomocí pg_restore</span><span class="sxs-lookup"><span data-stu-id="fe9a2-113">Restore hello data into hello target Azure Database for PostrgeSQL using pg_restore</span></span>
<span data-ttu-id="fe9a2-114">Jakmile vytvoříte hello cílová databáze, můžete použít příkaz pg_restore hello a hello -d, – dbname parametr toorestore hello data do hello cílová databáze ze souboru s výpisem hello.</span><span class="sxs-lookup"><span data-stu-id="fe9a2-114">Once you have created hello target database, you can use hello pg_restore command and hello -d, --dbname parameter toorestore hello data into hello target database from hello dump file.</span></span>
```bash
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
<span data-ttu-id="fe9a2-115">V tomto příkladu obnovit hello data ze souboru s výpisem hello **testdb.dump** do databáze hello **mypgsqldb** na cílovém serveru **mypgserver-20170401.postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="fe9a2-115">In this example, restore hello data from hello dump file **testdb.dump** into hello database **mypgsqldb** on target server **mypgserver-20170401.postgres.database.azure.com**.</span></span>
```bash
pg_restore -v --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a><span data-ttu-id="fe9a2-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe9a2-116">Next steps</span></span>
- <span data-ttu-id="fe9a2-117">najdete v databázi PostgreSQL pomocí exportu a importu, toomigrate [PostgreSQL databázi migrujte pomocí exportu a importu](howto-migrate-using-export-and-import.md)</span><span class="sxs-lookup"><span data-stu-id="fe9a2-117">toomigrate a PostgreSQL database using export and import, see [Migrate your PostgreSQL database using export and import](howto-migrate-using-export-and-import.md)</span></span>
