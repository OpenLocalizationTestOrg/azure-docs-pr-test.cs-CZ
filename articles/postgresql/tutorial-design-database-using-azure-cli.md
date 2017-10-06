---
title: "Navrhnout první databáze Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento kurz ukazuje, jak tooDesign vaše první Azure databázi PostgreSQL pomocí rozhraní příkazového řádku Azure."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 06/13/2017
ms.openlocfilehash: 7914925c090e0b6f3e7c8a999eedb0b2baf83d7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-postgresql-using-azure-cli"></a>Navrhnout první databáze Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure 
V tomto kurzu použijete rozhraní příkazového řádku Azure (rozhraní příkazového řádku) a dalších nástrojů toolearn jak na:
> [!div class="checklist"]
> * Vytvoření Azure Database for PostgreSQL
> * Konfigurace brány firewall serveru hello
> * Použití [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) nástroj toocreate databáze
> * Načíst ukázková data
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

Můžete použít hello prostředí cloudu Azure v prohlížeči hello nebo [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli) na vlastní počítače toorun hello bloky kódu v tomto kurzu.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Pokud máte více předplatných, vyberte odpovídající předplatné hello, ve kterém hello prostředek neexistuje nebo se fakturuje pro. Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#set) určité ID předplatného.
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

Hello následující příklad vytvoří na serveru s názvem `mypgserver-20170401` ve vaší skupině prostředků `myresourcegroup` s přihlašovací jméno správce serveru `mylogin`. Název serveru, mapuje tooDNS název a je požadovaná toobe globálně jedinečné v Azure. SUBSTITUTE hello `<server_admin_password>` vlastní hodnotou.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401 --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
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
>

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

## <a name="connect-tooazure-database-for-postgresql-database-using-psql"></a>Připojit tooAzure databáze pro databázi PostgreSQL pomocí psql
Pokud má klientský počítač PostgreSQL nainstalován, můžete použít místní instanci [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), nebo hello Azure Cloud Console tooconnect tooan Azure PostgreSQL serveru. Použijeme hello psql nástroj příkazového řádku tooconnect toohello databáze Azure nyní pro PostgreSQL server.

1. Spusťte následující psql příkaz tooconnect tooan databáze Azure pro PostgreSQL server hello
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Například následující příkaz hello připojuje toohello výchozí databázi, která je volána **postgres** na vašem serveru PostgreSQL **mypgserver 20170401.postgres.database.azure.com** pomocí přihlašovacích údajů k přístupu. Zadejte hello `<server_admin_password>` jste zvolili při budete vyzváni k zadání hesla.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 ---dbname=postgres
```

2.  Až se server připojený toohello, vytvořte prázdnou databázi hello příkazového řádku.
```sql
CREATE DATABASE mypgsqldb;
```

3.  Na příkazovém řádku hello spustit následující příkaz tooswitch připojení toohello nově vytvořený databáze hello **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="create-tables-in-hello-database"></a>Vytváření tabulek v databázi hello
Teď, když víte, jak tooconnect toohello databáze Azure pro PostgreSQL, jsme můžete projít postupy toocomplete některé základní úlohy.

Jsme nejprve vytvořit tabulku a načíst určitými daty. Umožňuje vytvořit tabulku, která sleduje informace o inventáři.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

Můžete zjistit hello nově vytvořený tabulky v hello seznam tabulek teď zadáním:
```sql
\dt
```

## <a name="load-data-into-hello-tables"></a>Načtení dat do tabulky hello
Teď, když máme tabulku, jsme do něj vložte některá data. V okně spusťte příkazový řádek text hello spusťte následující dotaz tooinsert hello některé řádky dat.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Máte nyní dva řádky ukázková data do hello tabulky, které jste vytvořili dříve.

## <a name="query-and-update-hello-data-in-hello-tables"></a>Dotazování a aktualizovat hello data v tabulkách hello
Spusťte následující dotaz tooretrieve informace z tabulky databáze hello hello. 
```sql
SELECT * FROM inventory;
```

Můžete také aktualizovat hello data v tabulkách hello
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Při načítání dat, získá Hello řádek příslušným způsobem aktualizuje.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a>Obnovit do databáze tooa předchozího bodu v čase
Představte si, že jste omylem odstranili tabulku. Toto je něco, které nelze snadno obnovit z. Azure databázi PostgreSQL vám umožní toogo back tooany bodu v čase (v hello poslední too7 dní (Basic) a 35 dní (standardní)) a obnovit tento nový server tooa bodu v čase. Můžete použít tento nový server toorecover odstraněná data. Následující kroky obnovení hello ukázkový server tooa bod před přidáním tabulky hello Hello.

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

Hello `az postgres server restore` příkaz potřebuje hello následující parametry:
| Nastavení | Navrhovaná hodnota | Popis  |
| --- | --- | --- |
| --skupiny prostředků |  myResourceGroup |  Skupinu prostředků, ve které hello zdrojového serveru existuje.  |
| – Název | Obnovit mypgserver | Název Hello hello nový server, který je vytvořen pomocí příkazu restore hello. |
| obnovení bodu v čase | 2017-04-13T13:59:00Z | Vyberte bod v čase toorestore k. Toto datum a čas musí být v období uchovávání záloh hello zdrojového serveru. Použití datum a čas ve formátu ISO 8601. Například můžete použít vlastní místním časovém pásmu, jako například `2017-04-13T05:59:00-08:00`, nebo použijte formát času UTC zulština `2017-04-13T13:59:00Z`. |
| --zdrojového serveru | mypgserver 20170401 | Hello název nebo ID hello zdrojový server toorestore z. |

Obnovení serveru tooa v daném okamžiku se vytvoří nový server, kopírování jako původní server hello od hello bodu v čase, který zadáte. Hello umístění a cenovou úroveň hodnoty pro server hello obnovit jsou hello stejný jako zdrojový server hello.

příkaz Hello je synchronní a vrátí po obnovení serveru hello. Po dokončení obnovení hello vyhledejte hello nový server, který byl vytvořen. Ověřte hello data byla obnovena podle očekávání.


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili jak toouse rozhraní příkazového řádku Azure (rozhraní příkazového řádku) a další nástroje na:
> [!div class="checklist"]
> * Vytvoření Azure Database for PostgreSQL
> * Konfigurace brány firewall serveru hello
> * Použití [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) nástroj toocreate databáze
> * Načíst ukázková data
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

V dalším kroku zjistěte, jak toouse hello podobných úloh s Azure toodo portálu, přečtěte si v tomto kurzu: [navrhnout první databáze Azure pro použití PostgreSQL hello portálu Azure](tutorial-design-database-using-azure-portal.md)
