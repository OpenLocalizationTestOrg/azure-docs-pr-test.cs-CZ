---
title: "protokoly serveru aaaConfigure a přístup pro PostgreSQL pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak server hello tooconfigure a přístup protokolech v Azure databázi PostgreSQL pomocí příkazového řádku Azure CLI."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 924d4e7634cc48405bcb0f813cf61d8b72ad8aa0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Konfigurace a přístup k protokolům serveru pomocí rozhraní příkazového řádku Azure
Můžete si stáhnout hello PostgreSQL serveru protokoly chyb pomocí hello rozhraní příkazového řádku (Azure CLI). Přístup k protokolům tootransaction však není podporován. 

## <a name="prerequisites"></a>Požadavky
toostep prostřednictvím této jak tooguide, budete potřebovat:
- [Azure databázi PostgreSQL serveru.](quickstart-create-server-database-azure-cli.md)
- Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) příkazového řádku nástroje nebo použití hello prostředí cloudu Azure v prohlížeči hello.

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Konfigurace protokolování pro databázi Azure pro PostgreSQL
Můžete nakonfigurovat hello serveru tooaccess dotazu protokoly a protokoly chyb. Protokoly chyb může obsahovat informace o automatické vakuu, připojení a kontrolní body.
1. Povolení protokolování
2. Aktualizace protokolu\_prohlášení a protokolu\_min\_doba trvání\_protokolování dotazu tooenable – příkaz
3. Doba uchování aktualizace

Další informace najdete v tématu [přizpůsobení parametry konfigurace serveru](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Seznam protokolů pro databázi Azure pro PostgreSQL server
soubory k dispozici protokolu hello toolist pro váš server, spusťte hello [az postgres protokoly serveru seznamu](/cli/azure/postgres/server-logs#list) příkaz.

Můžete vytvořit seznam hello soubory protokolu pro server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup**a nasměrovat ho tooa textového souboru s názvem **protokolu\_soubory\_seznam.txt.**
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mypgserver-20170401 > log_files_list.txt
```
## <a name="download-logs-locally-from-hello-server"></a>Stažení protokolů místně z hello server
Hello [az postgres – protokoly serveru stáhnout](/cli/azure/postgres/server-logs#download) příkaz vám umožní toodownload jednotlivých protokolových souborů pro váš server. 

Tento příklad stahování hello konkrétní protokol pro hello server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup** tooyour místní prostředí.
```azurecli-interactive
az postgres server-logs download --name 20170414-mypgserver-20170401-postgresql.log --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="next-steps"></a>Další kroky
- toolearn Další informace o protokoly serveru, najdete v části [protokoly serveru v Azure databáze PostgreSQL](concepts-server-logs.md)
- Další informace o parametry serveru, najdete v části [přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-parameters-using-cli.md)
