---
title: "Vytvoření databáze Azure pro PostgreSQL pomocí hello rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Rychlý start toocreate průvodce a spravovat databáze Azure pro PostgreSQL server pomocí rozhraní příkazového řádku Azure (rozhraní příkazového řádku)."
services: postgresql
author: sanagama
ms.author: sanagama
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 06/13/2017
ms.openlocfilehash: 946aa3cbf5ff9f5ac4e51248412d3da5d718141e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-postgresql-using-hello-azure-cli"></a>Vytvoření databáze Azure pro PostgreSQL pomocí hello rozhraní příkazového řádku Azure
Azure databázi PostgreSQL je spravovaná služba, která vám umožní toorun, spravovat a škálování vysoce dostupné databáze PostgreSQL v cloudu hello. Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech. Tento rychlý start se dozvíte, jak toocreate Azure databáze pro server PostgreSQL v [skupina prostředků Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) pomocí hello rozhraní příkazového řádku Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Pokud máte více předplatných, zvolte hello příslušné předplatné, ve kterém bude účtován hello prostředků. Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#set) určité ID předplatného.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvoření [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina. Hello následující příklad vytvoří skupinu prostředků s názvem `myresourcegroup` v hello `westus` umístění.
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a>Vytvoření serveru Azure Database for PostgreSQL

Vytvoření [Azure databázi PostgreSQL serveru](overview.md) pomocí hello [az postgres server vytvořit](/cli/azure/postgres/server#create) příkaz. Server obsahuje soubor databází spravovaných jako skupina. 

Hello následující příklad vytvoří server s názvem `mypgserver-20170401` ve vaší skupině prostředků `myresourcegroup` s přihlašovací jméno správce serveru `mylogin`. Mapuje název tooDNS Hello název serveru a je požadovaná toobe globálně jedinečné v Azure. SUBSTITUTE hello `<server_admin_password>` vlastní hodnotou.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> Hello přihlašovací jméno správce serveru a heslo, které tady zadáte jsou požadované toolog toohello serveru a její databáze dále v této úvodní. Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.

Ve výchozím nastavení se databáze **postgres** vytvoří v rámci vašeho serveru. Hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) databáze je výchozí databáze určené výhradně pro uživatele, nástroje a aplikace třetích stran. 


## <a name="configure-a-server-level-firewall-rule"></a>Konfigurace pravidla brány firewall na úrovni serveru

Vytvoření pravidla brány firewall na úrovni serveru Azure PostgreSQL s hello [az postgres pravidla brány firewall-vytvořit](/cli/azure/postgres/server/firewall-rule#create) příkaz. Pravidlo brány firewall na úrovni serveru umožňuje externí aplikací, jako například [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) nebo [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server přes bránu firewall služby Azure PostgreSQL hello. 

Můžete nastavit pravidlo brány firewall, které pokrývá IP rozsah toobe tooconnect mít z vaší sítě. Hello následující příklad používá [az postgres pravidla brány firewall-vytvořit](/cli/azure/postgres/server/firewall-rule#create) toocreate pravidlo brány firewall `AllowAllIps` rozsah adres IP. tooopen všechny IP adresy, použijte 0.0.0.0 jako hello počáteční IP adresa a 255.255.255.255 jako hello koncová adresa.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Server Azure PostgreSQL komunikuje přes port 5432. Pokud se připojujete z podnikové sítě, nemusí být odchozí provoz přes port 5432 bránou firewall vaší sítě povolený. Máte oddělení IT, otevřete port 5432 tooconnect tooyour databáze Azure SQL server.

## <a name="get-hello-connection-information"></a>Získat informace o připojení hello

tooconnect tooyour serveru, musíte tooprovide informace a přístup k přihlašovacím údajům hostitele.
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

Výsledkem Hello je ve formátu JSON. Poznamenejte si hello **administratorLogin** a **Plně_kvalifikovaný_název_domény**.
```json
{
  "administratorLogin": "mylogin",
  "fullyQualifiedDomainName": "mypgserver-20170401.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mypgserver-20170401",
  "location": "westus",
  "name": "mypgserver-20170401",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "PGSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 51200,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

## <a name="connect-toopostgresql-database-using-psql"></a>Připojit databáze tooPostgreSQL pomocí psql

Pokud má klientský počítač PostgreSQL nainstalován, můžete použít místní instanci [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooconnect tooan Azure PostgreSQL serveru. Teď umožňuje použít hello psql nástroj příkazového řádku tooconnect toohello Azure PostgreSQL serveru.

1. Spusťte následující psql příkaz tooconnect tooan databáze Azure pro PostgreSQL server hello
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Například následující příkaz hello připojuje toohello výchozí databázi, která je volána **postgres** na vašem serveru PostgreSQL **mypgserver 20170401.postgres.database.azure.com** pomocí přihlašovacích údajů k přístupu. Zadejte hello `<server_admin_password>` jste zvolili při budete vyzváni k zadání hesla.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  Až se server připojený toohello, vytvořte prázdnou databázi hello příkazového řádku.
```sql
CREATE DATABASE mypgsqldb;
```

3.  Na příkazovém řádku hello spustit následující příkaz tooswitch připojení toohello nově vytvořený databáze hello **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="connect-toopostgresql-database-using-pgadmin"></a>Připojit databáze tooPostgreSQL pomocí pgAdmin

tooconnect tooAzure PostgreSQL serveru pomocí hello grafického uživatelského rozhraní nástroje _pgAdmin_
1.  Spusťte hello _pgAdmin_ aplikace na klientském počítači. _pgAdmin_ můžete nainstalovat ze stránky http://www.pgadmin.org/.
2.  Zvolte **přidejte nový Server** z hello **rychlé odkazy** nabídky.
3.  V hello **vytvoření - serveru** dialogové okno **Obecné** zadejte jedinečný popisný název pro hello server. Může to být třeba **Azure PostgreSQL Server**.
 ![Nástroj pgAdmin – Vytvořit – server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)
4.  V hello **vytvoření - serveru** dialogové okno, **připojení** karty:
    - Zadejte název serveru plně kvalifikovaný hello (například **mypgserver 20170401.postgres.database.azure.com**) v hello **název hostitele / adres** pole. 
    - Zadejte port 5432 do hello **Port** pole. 
    - Zadejte hello **přihlašovací jméno správce serveru (user@mypgserver)** získali dříve v této rychlý start a heslo, které jste zadali při vytváření hello server do hello **uživatelské jméno** a **heslo** oknech v uvedeném pořadí.
    - U položky **Režim SSL** vyberte možnost **Vyžadovat**. Ve výchozím nastavení jsou všechny servery Azure PostgreSQL vytvořeny se zapnutým vynucováním SSL. tooturn vypnout vynucování SSL, najdete v podrobnostech v [vynucování SSL](./concepts-ssl-connection-security.md).

    ![pgAdmin – Vytvořit – server](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.  Klikněte na **Uložit**.
6.  V levém podokně Prohlížeč hello rozbalte hello **skupin serverů**. Vyberte svůj server **Azure PostgreSQL Server**.
7.  Zvolte hello **Server** připojené k a potom vyberte **databáze** v něm. 
8.  Klikněte pravým tlačítkem na **databáze** tooCreate databáze.
9.  Vyberte název databáze **mypgsqldb** a hello vlastník pro něj jako přihlašovací jméno správce serveru **mylogin**.
10. Klikněte na tlačítko **Uložit** toocreate prázdnou databázi.
11. V hello **prohlížeče**, rozbalte položku hello **servery** skupiny. Rozbalte server hello jste vytvořili a zobrazí hello databáze **mypgsqldb** v něm.
 ![Nástroj pgAdmin – Vytvořit – databáze](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)


## <a name="clean-up-resources"></a>Vyčištění prostředků

Vyčištění všechny prostředky, které jste vytvořili v rychlý start hello odstraněním hello [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md).

> [!TIP]
> Další rychlé starty v této kolekci vycházejí z tohoto rychlého startu. Pokud máte v plánu toocontinue na toowork s následné rychlých průvodců, proveďte není vyčistit hello prostředky vytvořené v tento rychlý start. Pokud neplánujete toocontinue, použijte následující postup toodelete hello všechny prostředky, které jsou vytvořené tento rychlý start v hello rozhraní příkazového řádku Azure.

```azurecli-interactive
az group delete --name myresourcegroup
```

Pokud vás právě zajímají toodelete hello jeden nově vytvořený server, můžete spustit [odstranění serveru postgres az](/cli/azure/postgres/server#delete) příkaz.
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)
