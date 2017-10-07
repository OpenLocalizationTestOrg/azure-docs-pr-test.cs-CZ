---
title: "Rychlý start: Vytvoření serveru Azure Database for MySQL – rozhraní příkazového řádku Azure | Dokumentace Microsoftu"
description: "Tento rychlý start popisuje, jak toouse hello toocreate rozhraní příkazového řádku Azure Azure databáze MySQL serveru ve skupině prostředků Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: hero-article
ms.date: 06/13/2017
ms.custom: mvc
ms.openlocfilehash: 708d0cce12e812cb464adcf7e83e6f85c196bafe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a>Vytvoření serveru Azure Database for MySQL pomocí Azure CLI
Tento rychlý start popisuje, jak toouse hello toocreate rozhraní příkazového řádku Azure Azure databáze MySQL server ve skupině prostředků Azure pro přibližně pět minut. Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Pokud máte více předplatných, vyberte odpovídající předplatné hello, ve kterém hello prostředek neexistuje nebo se fakturuje pro. Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#set) určité ID předplatného.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Vytvoření [skupina prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) pomocí hello [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) příkaz. Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.

Hello následující příklad vytvoří skupinu prostředků s názvem `myresourcegroup` v hello `westus` umístění.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Vytvoření serveru Azure Database for MySQL
Vytvoření databáze Azure pro server databáze MySQL s hello **az mysql server vytvořit** příkaz. Server může spravovat více databází. Obvykle se pro jednotlivé projekty nebo uživatele používají samostatné databáze.

Hello následující příklad vytvoří databázi Azure pro server databáze MySQL, které jsou umístěné v `westus` ve skupině prostředků hello `myresourcegroup` s názvem `myserver4demo`. protokol správce má Hello server v s názvem `myadmin` a heslo `Password01!`. Hello server je vytvořen s **základní** úroveň výkonu a **50** výpočetní jednotky sdílena mezi všechny hello databází na serveru hello. Můžete škálovat výpočetní a úložnou nahoru nebo dolů v závislosti na potřebám aplikace hello.

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name myserver4demo --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Konfigurace pravidla brány firewall
Vytvoření databáze MySQL pravidla brány firewall na úrovni serveru pomocí hello Azure **az mysql pravidla brány firewall-vytvořit** příkaz. Pravidlo brány firewall na úrovni serveru umožňuje externí aplikací, jako je například hello **mysql.exe** nástroj příkazového řádku nebo MySQL Workbench tooconnect tooyour server prostřednictvím brány firewall služby Azure MySQL hello. 

Hello následující příklad vytvoří pravidlo brány firewall pro rozsah předdefinovaných adres, který v tomto příkladu je hello celý možné rozsah IP adres.

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server myserver4demo --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a>Konfigurace nastavení SSL
Ve výchozím nastavení se připojení SSL mezi serverem a klientskými aplikacemi vynucuje.  To zajišťuje zabezpečení "za provozu" hello dat šifrování hello datový proud přes internet.  toomake tento rychlý start snadnější, Zakážeme připojení SSL pro váš server.  Pro provozní servery toto nastavení nedoporučujeme.  Další informace najdete v tématu [připojení konfigurace protokolu SSL ve vaší aplikaci toosecurely connect tooAzure databáze pro databázi MySQL](./howto-configure-ssl.md).

Hello následující příklad zakazuje vynucení SSL na vašem serveru MySQL.
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name myserver4demo -g -n --ssl-enforcement Disabled
 ```

## <a name="get-hello-connection-information"></a>Získat informace o připojení hello

tooconnect tooyour serveru, musíte tooprovide informace a přístup k přihlašovacím údajům hostitele.

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name myserver4demo
```

Výsledkem Hello je ve formátu JSON. Poznamenejte si hello **Plně_kvalifikovaný_název_domény** a **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "myserver4demo.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/servers/myserver4demo",
  "location": "westus",
  "name": "myserver4demo",
  "resourceGroup": "myresourcegroup",
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

## <a name="connect-toohello-server-using-hello-mysqlexe-command-line-tool"></a>Připojení serveru toohello pomocí nástroje příkazového řádku mysql.exe hello
Připojení serveru tooyour pomocí hello **mysql.exe** nástroj příkazového řádku. MySQL můžete stáhnout [odsud](https://dev.mysql.com/downloads/) a nainstalovat do svého počítače. Místo toho mohou také kliknutím hello **zkuste ho** tlačítko ukázky kódu, nebo hello `>_` tlačítko hello horním pravém panelu nástrojů v hello portál Azure a spuštění hello **prostředí cloudu Azure**.

Zadejte další příkazy hello: 

1. Připojení pomocí serveru toohello **mysql** nástroj příkazového řádku:
```azurecli-interactive
 mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. Zobrazení stavu serveru:
```sql
 mysql> status
```
Pokud všechno proběhne správně, musí nástroj příkazového řádku hello výstup hello následující text:

```dos
C:\Users\>mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
Enter password: ***********
Welcome toohello MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' tooclear hello current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.35, for Win64 (x86_64)

Connection id:          65512
Current database:
Current user:           myadmin@116.230.243.143
SSL:                    Not in use
Using delimiter:        ;
Server version:         5.6.26.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             myserver4demo.mysql.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    gbk
Conn.  characterset:    gbk
TCP port:               3306
Uptime:                 2 days 9 hours 47 min 20 sec

Threads: 4  Questions: 34833  Slow queries: 2  Opens: 84  Flush tables: 4  Open tables: 1  Queries per second avg: 0.167
--------------

mysql>
```

> [!TIP]
> Další příkazy najdete v [Referenční příručce k MySQL 5.7 – v kapitole 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

## <a name="connect-toohello-server-using-hello-mysql-workbench-gui-tool"></a>Připojení serveru toohello pomocí nástroje hello MySQL Workbench grafického uživatelského rozhraní
1.  Spusťte hello MySQL Workbench aplikace na klientském počítači. MySQL Workbench můžete stáhnout a nainstalovat [odtud](https://dev.mysql.com/downloads/workbench/).

2.  V hello **nastavit připojení k nové** dialogovém okně zadejte následující informace hello **parametry** karty:

   ![nastavení nového připojení](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| **Nastavení** | **Navrhovaná hodnota** | **Popis** |
|---|---|---|
|   Název připojení | My Connection | Zadejte jmenovku pro toto připojení (může být libovolná). |
| Způsob připojení | zvolte Standardní (TCP/IP) | Použít protokol TCP/IP protokol tooconnect tooAzure databáze pro databázi MySQL > |
| Název hostitele | myserver4demo.mysql.database.azure.com | Název serveru, který jste si předtím poznamenali. |
| Port | 3306 | se používá Hello výchozí port pro databázi MySQL. |
| Uživatelské jméno | myadmin@myserver4demo | Hello přihlašovací jméno správce serveru, který jste si předtím poznamenali. |
| Heslo | **** | Použití hello správce heslo účtu, který jste nakonfigurovali dříve. |

Klikněte na tlačítko **Test připojení** tootest, pokud jsou správně nakonfigurovány všechny parametry.
Nyní klikněte na připojení hello toosuccessfully připojení toohello serveru.

## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud nepotřebujete tyto prostředky pro jiné rychlý start nebo kurz, můžete je odstranit pomocí tohoto postupu hello následující příkaz: 

```azurecli-interactive
az group delete --name myresourcegroup
```

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Návrh databáze MySQL pomocí rozhraní příkazového řádku Azure](./tutorial-design-database-using-cli.md)
