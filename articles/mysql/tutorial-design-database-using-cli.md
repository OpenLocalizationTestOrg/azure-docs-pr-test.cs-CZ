---
title: "aaaDesign vaše první Azure databáze pro databázi MySQL. rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento kurz vysvětluje, jak toocreate a správě Azure databáze MySQL serveru a databáze pomocí hello příkazového řádku Azure CLI 2.0."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.custom: mvc
ms.openlocfilehash: 6339913c2af58e0e4c4eabb69097a5c9c245781c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Navrhnout první databáze Azure pro databázi MySQL

Azure databáze pro databázi MySQL je služba relační databáze v hello cloudu společnosti Microsoft podle MySQL Community Edition databázového stroje. V tomto kurzu použijete rozhraní příkazového řádku Azure (rozhraní příkazového řádku) a dalších nástrojů toolearn jak na:

> [!div class="checklist"]
> * Vytvoření Azure databáze pro databázi MySQL
> * Konfigurace brány firewall serveru hello
> * Použití [nástroj pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate databáze
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
Vytvoření [skupina prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) s [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) příkaz. Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.

Hello následující příklad vytvoří skupinu prostředků s názvem `mycliresource` v hello `westus` umístění.

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Vytvoření serveru Azure Database for MySQL
Vytvoření databáze Azure pro server databáze MySQL se serverem mysql az hello vytvoření příkazu. Server může spravovat více databází. Obvykle se pro jednotlivé projekty nebo uživatele používají samostatné databáze.

Hello následující příklad vytvoří databázi Azure pro server databáze MySQL, které jsou umístěné v `westus` ve skupině prostředků hello `mycliresource` s názvem `mycliserver`. protokol správce má Hello server v s názvem `myadmin` a heslo `Password01!`. Hello server je vytvořen s **základní** úroveň výkonu a **50** výpočetní jednotky sdílena mezi všechny hello databází na serveru hello. Můžete škálovat výpočetní a úložnou nahoru nebo dolů v závislosti na potřebám aplikace hello.

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver
--location westus --user myadmin --password Password01!
--performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Konfigurace pravidla brány firewall
Vytvoření databáze Azure pro pravidlo brány firewall na úrovni serveru MySQL s hello az mysql pravidla brány firewall-vytvoření příkazu. Pravidlo brány firewall na úrovni serveru umožňuje externí aplikací, jako například **mysql** nástroj příkazového řádku nebo MySQL Workbench tooconnect tooyour server prostřednictvím brány firewall služby Azure MySQL hello. 

Hello následující příklad vytvoří pravidlo brány firewall pro rozsah předdefinovaných adres. Tento příklad ukazuje hello celý možné rozsah IP adres.

```azurecli-interactive
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="get-hello-connection-information"></a>Získat informace o připojení hello

tooconnect tooyour serveru, musíte tooprovide informace a přístup k přihlašovacím údajům hostitele.
```azurecli-interactive
az mysql server show --resource-group mycliresource --name mycliserver
```

Výsledkem Hello je ve formátu JSON. Poznamenejte si hello **Plně_kvalifikovaný_název_domény** a **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mycliserver.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mycliresource/providers/Microsoft.DBforMySQL/servers/mycliserver",
  "location": "westus",
  "name": "mycliserver",
  "resourceGroup": "mycliresource",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "MYSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

## <a name="connect-toohello-server-using-mysql"></a>Připojení serveru toohello pomocí mysql
Použití [nástroj pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) tooestablish tooyour připojení databáze Azure pro server databáze MySQL. V tomto příkladu je hello příkaz:
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>Vytvoření prázdné databáze
Jakmile jste server připojený toohello, vytvořte prázdnou databázi.
```sql
mysql> CREATE DATABASE mysampledb;
```

V řádku hello spusťte následující příkaz tooswitch hello připojení toothis nově vytvořený databáze hello:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-hello-database"></a>Vytváření tabulek v databázi hello
Teď, když víte, jak tooconnect toohello Azure Database pro databázi MySQL, jsme můžete projít postupy toocomplete některé základní úlohy.

Jsme nejprve vytvořit tabulku a načíst určitými daty. Umožňuje vytvořit tabulku, která ukládá informace o inventáři.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-hello-tables"></a>Načtení dat do tabulky hello
Teď, když máme tabulku, jsme do něj vložte některá data. V okně hello spusťte příkazový řádek spusťte hello následující dotaz tooinsert některé řádky dat.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Nyní máte dva řádky ukázková data do hello tabulky, které jste vytvořili dříve.

## <a name="query-and-update-hello-data-in-hello-tables"></a>Dotazování a aktualizovat hello data v tabulkách hello
Spusťte následující dotaz tooretrieve informace z tabulky databáze hello hello.
```sql
SELECT * FROM inventory;
```

Můžete také aktualizovat hello data v tabulkách hello.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Při načítání dat, získá Hello řádek příslušným způsobem aktualizuje.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a>Obnovit do databáze tooa předchozího bodu v čase
Představte si, že jste omylem odstranili této tabulky. Toto je něco, které nelze snadno obnovit z. Azure databáze pro databázi MySQL vám umožní toogo back tooany bodu v čase v hello poslední až too35 dnů a obnovit tento bod v časové tooa nový server. Můžete použít tento nový server toorecover odstraněná data. Následující kroky obnovení hello ukázkový server tooa bod před přidáním tabulky hello Hello.

Hello obnovení musíte hello následující informace:

- Bod obnovení: Vyberte bodu v čase, k níž dojde před hello server byl změněn. Musí být větší než nebo roven hodnotě nejstarší zálohování hodnota toohello zdrojové databáze.
- Cílový server: Zadejte nový název serveru, který chcete toorestore k
- Zdrojový server: Zadejte název hello hello serveru chcete toorestore z
- Umístění: Nelze vybrat hello oblast, ve výchozím nastavení je stejný jako zdrojový server hello

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

toorestore hello server a [obnovení tooa v daném okamžiku](./howto-restore-server-portal.md) před hello tabulka byla odstraněna. Obnovení do serveru tooa jiného bodu v čase vytvoří duplicitní nový server jako původní server hello hello bodu v čase, zadáte, za předpokladu, že je v rámci hello dobu uchování vašeho [vrstvy služby](./concepts-service-tiers.md).

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se dozvěděli na:
> [!div class="checklist"]
> * Vytvoření Azure databáze pro databázi MySQL
> * Konfigurace brány firewall serveru hello
> * Použití [nástroj pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate databáze
> * Načíst ukázková data
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

> [!div class="nextstepaction"]
> [Azure databáze pro databázi MySQL – ukázky rozhraní příkazového řádku Azure](./sample-scripts-azure-cli.md)
