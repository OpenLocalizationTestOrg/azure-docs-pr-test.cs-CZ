---
title: "Azure CLI: Vytvoření databáze SQL | Dokumentace Microsoftu"
description: "Zjistěte, jak hello toocreate logického serveru SQL Database, pravidla brány firewall na úrovni serveru a databází pomocí rozhraní příkazového řádku Azure."
keywords: "kurz k sql database, vytvoření databáze sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
ms.openlocfilehash: 9b1ffb17eabeb70a000ff0c997128832b07aa4fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-single-azure-sql-database-using-hello-azure-cli"></a>Vytvoření jedné databáze Azure SQL pomocí hello rozhraní příkazového řádku Azure

Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech. Tato příručka podrobnosti o pomocí rozhraní příkazového řádku Azure toodeploy hello Azure SQL database v [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) v [logického serveru Azure SQL Database](sql-database-features.md).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, zda používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="define-variables"></a>Definování proměnných

Definování proměnné pro použití ve skriptech hello tento rychlý start.

```azurecli-interactive
# hello data center and resource name for your resources
export resourcegroupname = myResourceGroup
export location = westeurope
# hello logical server name: Use a random value or replace with your own value (do not capitalize)
export servername = server-$RANDOM
# Set an admin login and password for your database
export adminlogin = ServerAdmin
export password = ChangeYourAdminPassword1
# hello ip address range that you want tooallow tooaccess your DB
export startip = "0.0.0.0"
export endip = "0.0.0.0"
# hello database name
export databasename = mySampleDatabase
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvoření [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina. Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westeurope` umístění.

```azurecli-interactive
az group create --name $resourcegroupname --location $location
```
## <a name="create-a-logical-server"></a>Vytvoření logického serveru

Vytvoření [logického serveru Azure SQL Database](sql-database-features.md) pomocí hello [az sql server vytvořit](/cli/azure/sql/server#create) příkaz. Logický server obsahuje soubor databází spravovaných jako skupina. Hello následující příklad vytvoří náhodným názvem serveru ve vaší skupině prostředků se k přihlášení správce s názvem `ServerAdmin` a heslo `ChangeYourAdminPassword1`. Podle potřeby tyto předdefinované hodnoty nahraďte.

```azurecli-interactive
az sql server create --name $servername --resource-group $resourcegroupname --location $location \
    --admin-user $adminlogin --admin-password $password
```

## <a name="configure-a-server-firewall-rule"></a>Konfigurace pravidla brány firewall serveru

Vytvoření [pravidlo brány firewall na úrovni serveru Azure SQL Database](sql-database-firewall-configure.md) pomocí hello [vytvoření brány firewall serveru sql az](/cli/azure/sql/server/firewall-rule#create) příkaz. Pravidlo brány firewall na úrovni serveru umožňuje externí aplikací, jako je například SQL Server Management Studio nebo hello SQLCMD nástroj tooconnect tooa SQL database přes bránu firewall služby SQL Database hello. V následujícím příkladu hello je otevřené brány firewall hello pouze ostatní prostředky služby Azure. externí připojení tooenable změnu hello IP adres tooan příslušnou adresu pro vaše prostředí. tooopen všechny IP adresy, použijte 0.0.0.0 jako hello počáteční IP adresa a 255.255.255.255 jako hello koncová adresa.  

```azurecli-interactive
az sql server firewall-rule create --resource-group $resourcegroupname --server $servername \
    -n AllowYourIp --start-ip-address $startip --end-ip-address $endip
```

> [!NOTE]
> SQL Database komunikuje přes port 1433. Pokud se pokoušíte tooconnect z podnikové sítě, odchozí provoz přes port 1433 nemusí mít povolený bránou firewall vaší sítě. Pokud ano, nebudou se moct tooconnect serveru Azure SQL Database tooyour, dokud vaše IT oddělení otevře port 1433.
>

## <a name="create-a-database-in-hello-server-with-sample-data"></a>Vytvořit databázi na serveru hello s ukázkovými daty

Vytvořit databázi s [úroveň výkonu S0](sql-database-service-tiers.md) v hello serveru pomocí hello [vytvořit databázi sql az](/cli/azure/sql/db#create) příkaz. Hello následující příklad vytvoří databázi názvem `mySampleDatabase` a zatížením hello AdventureWorksLT ukázková data do této databáze. Nahraďte tyto předdefinované hodnoty podle potřeby (jiné rychlé spuštění v této kolekci sestavení při hello hodnoty v této úvodní).

```azurecli-interactive
az sql db create --resource-group $resourcegroupname --server $servername \
    --name $databasename --sample-name AdventureWorksLT --service-objective S0
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu. 

> [!TIP]
> Pokud máte v plánu toocontinue na toowork s několik rychlé spuštění, vyčistěte hello prostředky vytvořené v této rychlé nespouštějte. Pokud neplánujete toocontinue, použijte následující kroky toodelete všechny prostředky vytvořeny pomocí této úvodní v hello portál Azure hello.
>

```azurecli-interactive
az group delete --name $resourcegroupname
```

## <a name="next-steps"></a>Další kroky

Teď, když máte databázi, můžete se k ní připojit a provádět dotazování pomocí vašich oblíbených nástrojů. Další informace získáte výběrem vašeho nástroje níže:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)

