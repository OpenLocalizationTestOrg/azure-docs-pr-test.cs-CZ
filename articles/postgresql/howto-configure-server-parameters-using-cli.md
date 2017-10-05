---
title: "Konfigurovat parametry služby ve službě Azure Database pro PostgreSQL | Microsoft Docs"
description: "Tento článek popisuje, jak nakonfigurovat parametry služby ve službě Azure Database pro PostgreSQL pomocí příkazového řádku Azure CLI."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: c8a3b5a0225c2cede180d8d57681f2e1a6c6cc3a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a>Přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure
Můžete zobrazit seznam, zobrazit a aktualizovat parametry konfigurace pro server Azure PostgreSQL pomocí rozhraní příkazového řádku (Azure CLI). Však pouze podmnožinu modul konfigurace jsou zveřejněné na úrovni serveru a je možné upravit. 

## <a name="prerequisites"></a>Požadavky
Chcete-li krok tímto průvodcem postupy, je třeba:
- Server a databáze [vytvoření databáze Azure pro PostgreSQL](quickstart-create-server-database-azure-cli.md)
- Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) příkaz nástroj řádku nebo pomocí prostředí cloudové služby Azure v prohlížeči.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>Seznam parametrů konfigurace serveru pro databázi Azure pro PostgreSQL serveru
Chcete-li zobrazit seznam všech změn parametrů v serveru a jejich hodnoty, spusťte [seznamu konfigurací serveru postgres az](/cli/azure/postgres/server/configuration#list) příkaz.

Můžete vytvořit seznam parametrů konfigurace serveru pro server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup**.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="show-server-configuration-parameter-details"></a>Zobrazit podrobnosti parametr konfigurace serveru
Chcete-li zobrazit podrobnosti o konkrétní konfiguračního parametru pro server, spusťte [az postgres serveru konfigurace zobrazit](/cli/azure/postgres/server/configuration#show) příkaz.

Tento příklad zobrazuje podrobnosti o **protokolu\_min\_zprávy** parametr konfigurace serveru pro server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup.**
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="modify-server-configuration-parameter-value"></a>Změnit hodnotu parametru konfigurace serveru
Můžete také upravit hodnotu určité parametr konfigurace serveru, a tím se aktualizuje základní hodnotu konfigurace pro modul serveru PostgreSQL. Aktualizace konfigurace pomocí [az postgres serveru konfigurační sady](/cli/azure/postgres/server/configuration#set) příkaz. 

Aktualizace **protokolu\_min\_zprávy** parametr konfigurace serveru serveru **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup.**
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401 --value INFO
```
Pokud chcete resetovat hodnota parametru konfigurace, rozhodnete jednoduše vynechte nepovinný `--value` parametr a službu použije se výchozí hodnota. Ve výše příkladu ho bude vypadat:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
Tím **protokolu\_min\_zprávy** konfigurace na výchozí hodnotu **upozornění**. Další informace o konfiguraci serveru a povolenou hodnoty, naleznete v dokumentaci k PostgreSQL na [konfigurace serveru](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Další kroky
- Konfigurace a přístup k protokolům serveru najdete v tématu [protokoly serveru v Azure databáze PostgreSQL](concepts-server-logs.md)
