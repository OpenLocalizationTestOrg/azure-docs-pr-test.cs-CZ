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
# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a><span data-ttu-id="1f318-103">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1f318-103">Create an Azure Database for MySQL server using Azure CLI</span></span>
<span data-ttu-id="1f318-104">Tento rychlý start popisuje, jak toouse hello toocreate rozhraní příkazového řádku Azure Azure databáze MySQL server ve skupině prostředků Azure pro přibližně pět minut.</span><span class="sxs-lookup"><span data-stu-id="1f318-104">This quickstart describes how toouse hello Azure CLI toocreate an Azure Database for MySQL server in an Azure resource group in about five minutes.</span></span> <span data-ttu-id="1f318-105">Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="1f318-105">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span>

<span data-ttu-id="1f318-106">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="1f318-106">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="1f318-107">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1f318-107">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="1f318-108">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="1f318-108">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="1f318-109">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1f318-109">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="1f318-110">Pokud máte více předplatných, vyberte odpovídající předplatné hello, ve kterém hello prostředek neexistuje nebo se fakturuje pro.</span><span class="sxs-lookup"><span data-stu-id="1f318-110">If you have multiple subscriptions, choose hello appropriate subscription in which hello resource exists or is billed for.</span></span> <span data-ttu-id="1f318-111">Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#set) určité ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="1f318-111">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="1f318-112">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="1f318-112">Create a resource group</span></span>
<span data-ttu-id="1f318-113">Vytvoření [skupina prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) pomocí hello [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1f318-113">Create an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) using hello [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> <span data-ttu-id="1f318-114">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.</span><span class="sxs-lookup"><span data-stu-id="1f318-114">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span>

<span data-ttu-id="1f318-115">Hello následující příklad vytvoří skupinu prostředků s názvem `myresourcegroup` v hello `westus` umístění.</span><span class="sxs-lookup"><span data-stu-id="1f318-115">hello following example creates a resource group named `myresourcegroup` in hello `westus` location.</span></span>

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="1f318-116">Vytvoření serveru Azure Database for MySQL</span><span class="sxs-lookup"><span data-stu-id="1f318-116">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="1f318-117">Vytvoření databáze Azure pro server databáze MySQL s hello **az mysql server vytvořit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="1f318-117">Create an Azure Database for MySQL server with hello **az mysql server create** command.</span></span> <span data-ttu-id="1f318-118">Server může spravovat více databází.</span><span class="sxs-lookup"><span data-stu-id="1f318-118">A server can manage multiple databases.</span></span> <span data-ttu-id="1f318-119">Obvykle se pro jednotlivé projekty nebo uživatele používají samostatné databáze.</span><span class="sxs-lookup"><span data-stu-id="1f318-119">Typically, a separate database is used for each project or for each user.</span></span>

<span data-ttu-id="1f318-120">Hello následující příklad vytvoří databázi Azure pro server databáze MySQL, které jsou umístěné v `westus` ve skupině prostředků hello `myresourcegroup` s názvem `myserver4demo`.</span><span class="sxs-lookup"><span data-stu-id="1f318-120">hello following example creates an Azure Database for MySQL server located in `westus` in hello resource group `myresourcegroup` with name `myserver4demo`.</span></span> <span data-ttu-id="1f318-121">protokol správce má Hello server v s názvem `myadmin` a heslo `Password01!`.</span><span class="sxs-lookup"><span data-stu-id="1f318-121">hello server has an administrator log in named `myadmin` and password `Password01!`.</span></span> <span data-ttu-id="1f318-122">Hello server je vytvořen s **základní** úroveň výkonu a **50** výpočetní jednotky sdílena mezi všechny hello databází na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="1f318-122">hello server is created with **Basic** performance tier and **50** compute units shared between all hello databases in hello server.</span></span> <span data-ttu-id="1f318-123">Můžete škálovat výpočetní a úložnou nahoru nebo dolů v závislosti na potřebám aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="1f318-123">You can scale compute and storage up or down depending on hello application needs.</span></span>

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name myserver4demo --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a><span data-ttu-id="1f318-124">Konfigurace pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="1f318-124">Configure firewall rule</span></span>
<span data-ttu-id="1f318-125">Vytvoření databáze MySQL pravidla brány firewall na úrovni serveru pomocí hello Azure **az mysql pravidla brány firewall-vytvořit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="1f318-125">Create an Azure Database for MySQL server-level firewall rule using hello **az mysql server firewall-rule create** command.</span></span> <span data-ttu-id="1f318-126">Pravidlo brány firewall na úrovni serveru umožňuje externí aplikací, jako je například hello **mysql.exe** nástroj příkazového řádku nebo MySQL Workbench tooconnect tooyour server prostřednictvím brány firewall služby Azure MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="1f318-126">A server-level firewall rule allows an external application, such as hello **mysql.exe** command-line tool or MySQL Workbench tooconnect tooyour server through hello Azure MySQL service firewall.</span></span> 

<span data-ttu-id="1f318-127">Hello následující příklad vytvoří pravidlo brány firewall pro rozsah předdefinovaných adres, který v tomto příkladu je hello celý možné rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="1f318-127">hello following example creates a firewall rule for a predefined address range, which in this example is hello entire possible range of IP addresses.</span></span>

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server myserver4demo --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a><span data-ttu-id="1f318-128">Konfigurace nastavení SSL</span><span class="sxs-lookup"><span data-stu-id="1f318-128">Configure SSL settings</span></span>
<span data-ttu-id="1f318-129">Ve výchozím nastavení se připojení SSL mezi serverem a klientskými aplikacemi vynucuje.</span><span class="sxs-lookup"><span data-stu-id="1f318-129">By default, SSL connections between your server and client applications are enforced.</span></span>  <span data-ttu-id="1f318-130">To zajišťuje zabezpečení "za provozu" hello dat šifrování hello datový proud přes internet.</span><span class="sxs-lookup"><span data-stu-id="1f318-130">This ensures security of "in-motion" data by encrypting hello data stream over hello internet.</span></span>  <span data-ttu-id="1f318-131">toomake tento rychlý start snadnější, Zakážeme připojení SSL pro váš server.</span><span class="sxs-lookup"><span data-stu-id="1f318-131">toomake this quick start easier, we disable SSL connections for your server.</span></span>  <span data-ttu-id="1f318-132">Pro provozní servery toto nastavení nedoporučujeme.</span><span class="sxs-lookup"><span data-stu-id="1f318-132">This is not recommended for production servers.</span></span>  <span data-ttu-id="1f318-133">Další informace najdete v tématu [připojení konfigurace protokolu SSL ve vaší aplikaci toosecurely connect tooAzure databáze pro databázi MySQL](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="1f318-133">For more information, see [Configure SSL connectivity in your application toosecurely connect tooAzure Database for MySQL](./howto-configure-ssl.md).</span></span>

<span data-ttu-id="1f318-134">Hello následující příklad zakazuje vynucení SSL na vašem serveru MySQL.</span><span class="sxs-lookup"><span data-stu-id="1f318-134">hello following example disables enforcing SSL on your MySQL server.</span></span>
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name myserver4demo -g -n --ssl-enforcement Disabled
 ```

## <a name="get-hello-connection-information"></a><span data-ttu-id="1f318-135">Získat informace o připojení hello</span><span class="sxs-lookup"><span data-stu-id="1f318-135">Get hello connection information</span></span>

<span data-ttu-id="1f318-136">tooconnect tooyour serveru, musíte tooprovide informace a přístup k přihlašovacím údajům hostitele.</span><span class="sxs-lookup"><span data-stu-id="1f318-136">tooconnect tooyour server, you need tooprovide host information and access credentials.</span></span>

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name myserver4demo
```

<span data-ttu-id="1f318-137">Výsledkem Hello je ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="1f318-137">hello result is in JSON format.</span></span> <span data-ttu-id="1f318-138">Poznamenejte si hello **Plně_kvalifikovaný_název_domény** a **administratorLogin**.</span><span class="sxs-lookup"><span data-stu-id="1f318-138">Make a note of hello **fullyQualifiedDomainName** and **administratorLogin**.</span></span>
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

## <a name="connect-toohello-server-using-hello-mysqlexe-command-line-tool"></a><span data-ttu-id="1f318-139">Připojení serveru toohello pomocí nástroje příkazového řádku mysql.exe hello</span><span class="sxs-lookup"><span data-stu-id="1f318-139">Connect toohello server using hello mysql.exe command-line tool</span></span>
<span data-ttu-id="1f318-140">Připojení serveru tooyour pomocí hello **mysql.exe** nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="1f318-140">Connect tooyour server using hello **mysql.exe** command-line tool.</span></span> <span data-ttu-id="1f318-141">MySQL můžete stáhnout [odsud](https://dev.mysql.com/downloads/) a nainstalovat do svého počítače.</span><span class="sxs-lookup"><span data-stu-id="1f318-141">You can download MySQL from [here](https://dev.mysql.com/downloads/) and install it on your computer.</span></span> <span data-ttu-id="1f318-142">Místo toho mohou také kliknutím hello **zkuste ho** tlačítko ukázky kódu, nebo hello `>_` tlačítko hello horním pravém panelu nástrojů v hello portál Azure a spuštění hello **prostředí cloudu Azure**.</span><span class="sxs-lookup"><span data-stu-id="1f318-142">Instead you may also click hello **Try It** button on code samples, or hello  `>_` button from hello upper right toolbar in hello Azure portal, and launch hello **Azure Cloud Shell**.</span></span>

<span data-ttu-id="1f318-143">Zadejte další příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="1f318-143">Type hello next commands:</span></span> 

1. <span data-ttu-id="1f318-144">Připojení pomocí serveru toohello **mysql** nástroj příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="1f318-144">Connect toohello server using **mysql** command-line tool:</span></span>
```azurecli-interactive
 mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. <span data-ttu-id="1f318-145">Zobrazení stavu serveru:</span><span class="sxs-lookup"><span data-stu-id="1f318-145">View server status:</span></span>
```sql
 mysql> status
```
<span data-ttu-id="1f318-146">Pokud všechno proběhne správně, musí nástroj příkazového řádku hello výstup hello následující text:</span><span class="sxs-lookup"><span data-stu-id="1f318-146">If everything goes well, hello command-line tool should output hello following text:</span></span>

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
> <span data-ttu-id="1f318-147">Další příkazy najdete v [Referenční příručce k MySQL 5.7 – v kapitole 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).</span><span class="sxs-lookup"><span data-stu-id="1f318-147">For additional commands, see [MySQL 5.7 Reference Manual - Chapter 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).</span></span>

## <a name="connect-toohello-server-using-hello-mysql-workbench-gui-tool"></a><span data-ttu-id="1f318-148">Připojení serveru toohello pomocí nástroje hello MySQL Workbench grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="1f318-148">Connect toohello server using hello MySQL Workbench GUI tool</span></span>
1.  <span data-ttu-id="1f318-149">Spusťte hello MySQL Workbench aplikace na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="1f318-149">Launch hello MySQL Workbench application on your client computer.</span></span> <span data-ttu-id="1f318-150">MySQL Workbench můžete stáhnout a nainstalovat [odtud](https://dev.mysql.com/downloads/workbench/).</span><span class="sxs-lookup"><span data-stu-id="1f318-150">You can download and install MySQL Workbench from [here](https://dev.mysql.com/downloads/workbench/).</span></span>

2.  <span data-ttu-id="1f318-151">V hello **nastavit připojení k nové** dialogovém okně zadejte následující informace hello **parametry** karty:</span><span class="sxs-lookup"><span data-stu-id="1f318-151">In hello **Setup New Connection** dialog box, enter hello following information on **Parameters** tab:</span></span>

   ![nastavení nového připojení](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| <span data-ttu-id="1f318-153">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="1f318-153">**Setting**</span></span> | <span data-ttu-id="1f318-154">**Navrhovaná hodnota**</span><span class="sxs-lookup"><span data-stu-id="1f318-154">**Suggested Value**</span></span> | <span data-ttu-id="1f318-155">**Popis**</span><span class="sxs-lookup"><span data-stu-id="1f318-155">**Description**</span></span> |
|---|---|---|
|   <span data-ttu-id="1f318-156">Název připojení</span><span class="sxs-lookup"><span data-stu-id="1f318-156">Connection Name</span></span> | <span data-ttu-id="1f318-157">My Connection</span><span class="sxs-lookup"><span data-stu-id="1f318-157">My Connection</span></span> | <span data-ttu-id="1f318-158">Zadejte jmenovku pro toto připojení (může být libovolná).</span><span class="sxs-lookup"><span data-stu-id="1f318-158">Specify a label for this connection (this can be anything)</span></span> |
| <span data-ttu-id="1f318-159">Způsob připojení</span><span class="sxs-lookup"><span data-stu-id="1f318-159">Connection Method</span></span> | <span data-ttu-id="1f318-160">zvolte Standardní (TCP/IP)</span><span class="sxs-lookup"><span data-stu-id="1f318-160">choose Standard (TCP/IP)</span></span> | <span data-ttu-id="1f318-161">Použít protokol TCP/IP protokol tooconnect tooAzure databáze pro databázi MySQL ></span><span class="sxs-lookup"><span data-stu-id="1f318-161">Use TCP/IP protocol tooconnect tooAzure Database for MySQL></span></span> |
| <span data-ttu-id="1f318-162">Název hostitele</span><span class="sxs-lookup"><span data-stu-id="1f318-162">Hostname</span></span> | <span data-ttu-id="1f318-163">myserver4demo.mysql.database.azure.com</span><span class="sxs-lookup"><span data-stu-id="1f318-163">myserver4demo.mysql.database.azure.com</span></span> | <span data-ttu-id="1f318-164">Název serveru, který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="1f318-164">Server name you previously noted.</span></span> |
| <span data-ttu-id="1f318-165">Port</span><span class="sxs-lookup"><span data-stu-id="1f318-165">Port</span></span> | <span data-ttu-id="1f318-166">3306</span><span class="sxs-lookup"><span data-stu-id="1f318-166">3306</span></span> | <span data-ttu-id="1f318-167">se používá Hello výchozí port pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="1f318-167">hello default port for MySQL is used.</span></span> |
| <span data-ttu-id="1f318-168">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="1f318-168">Username</span></span> | myadmin@myserver4demo | <span data-ttu-id="1f318-169">Hello přihlašovací jméno správce serveru, který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="1f318-169">hello server admin login you previously noted.</span></span> |
| <span data-ttu-id="1f318-170">Heslo</span><span class="sxs-lookup"><span data-stu-id="1f318-170">Password</span></span> | **** | <span data-ttu-id="1f318-171">Použití hello správce heslo účtu, který jste nakonfigurovali dříve.</span><span class="sxs-lookup"><span data-stu-id="1f318-171">Use hello admin account password you configured earlier.</span></span> |

<span data-ttu-id="1f318-172">Klikněte na tlačítko **Test připojení** tootest, pokud jsou správně nakonfigurovány všechny parametry.</span><span class="sxs-lookup"><span data-stu-id="1f318-172">Click **Test Connection** tootest if all parameters are correctly configured.</span></span>
<span data-ttu-id="1f318-173">Nyní klikněte na připojení hello toosuccessfully připojení toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="1f318-173">Now, you can click hello connection toosuccessfully connect toohello server.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="1f318-174">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="1f318-174">Clean up resources</span></span>
<span data-ttu-id="1f318-175">Pokud nepotřebujete tyto prostředky pro jiné rychlý start nebo kurz, můžete je odstranit pomocí tohoto postupu hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1f318-175">If you don't need these resources for another quickstart/tutorial, you can delete them by doing hello following command:</span></span> 

```azurecli-interactive
az group delete --name myresourcegroup
```

## <a name="next-steps"></a><span data-ttu-id="1f318-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f318-176">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1f318-177">Návrh databáze MySQL pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="1f318-177">Design a MySQL Database with Azure CLI</span></span>](./tutorial-design-database-using-cli.md)
