---
title: "parametry služby hello aaaConfigure v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Tento článek popisuje, jak tooconfigure parametry hello služby v Azure databázi PostgreSQL použití hello příkazového řádku Azure CLI."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 84a11de24ba87fc0eb6744aaa4b53f65a183903d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a>Přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure
Můžete zobrazit seznam, zobrazit a aktualizovat parametry konfigurace pro server Azure PostgreSQL pomocí hello rozhraní příkazového řádku (Azure CLI). Však pouze podmnožinu modul konfigurace jsou zveřejněné na úrovni serveru a je možné upravit. 

## <a name="prerequisites"></a>Požadavky
toostep prostřednictvím této jak tooguide, budete potřebovat:
- Server a databáze [vytvoření databáze Azure pro PostgreSQL](quickstart-create-server-database-azure-cli.md)
- Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) příkazového řádku nástroje nebo použití hello prostředí cloudu Azure v prohlížeči hello.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>Seznam parametrů konfigurace serveru pro databázi Azure pro PostgreSQL serveru
spuštění všech změn parametrů v serveru a jejich hodnoty toolist hello [seznamu konfigurací serveru postgres az](/cli/azure/postgres/server/configuration#list) příkaz.

Můžete vytvořit seznam hello parametry konfigurace serveru pro hello server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup**.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="show-server-configuration-parameter-details"></a>Zobrazit podrobnosti parametr konfigurace serveru
tooshow podrobnosti o konkrétní konfiguračního parametru pro server, spusťte hello [az postgres serveru konfigurace zobrazit](/cli/azure/postgres/server/configuration#show) příkaz.

Tento příklad zobrazuje podrobnosti o hello **protokolu\_min\_zprávy** parametr konfigurace serveru pro server **mypgserver 20170401.postgres.database.azure.com** v části Skupina prostředků **myresourcegroup.**
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="modify-server-configuration-parameter-value"></a>Změnit hodnotu parametru konfigurace serveru
Můžete také upravit hello hodnotu určité parametr konfigurace serveru, a tím se aktualizuje hello základní hodnota konfigurace pro modul server hello PostgreSQL. tooupdate hello konfigurace použití hello [az postgres serveru konfigurační sady](/cli/azure/postgres/server/configuration#set) příkaz. 

tooupdate hello **protokolu\_min\_zprávy** parametr konfigurace serveru serveru **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup.**
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401 --value INFO
```
Pokud chcete tooreset hello hodnoty parametru konfigurace, jednoduše vyberte tooleave out hello volitelné `--value` parametr a hello služby budou platit hello výchozí hodnotu. Ve výše příkladu ho bude vypadat:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
Tím obnovíte hello **protokolu\_min\_zprávy** konfigurace toohello výchozí hodnota **upozornění**. Další informace o konfiguraci serveru a povolenou hodnoty, naleznete v dokumentaci k PostgreSQL na [konfigurace serveru](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Další kroky
- protokoly serveru tooconfigure a přístup, najdete v části [protokoly serveru v Azure databáze PostgreSQL](concepts-server-logs.md)
