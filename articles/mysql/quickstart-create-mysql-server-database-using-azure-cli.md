---
title: "Rychlý start: Vytvoření serveru Azure Database for MySQL – rozhraní příkazového řádku Azure | Dokumentace Microsoftu"
description: "Tento rychlý start popisuje, jak použít rozhraní příkazového řádku Azure k vytvoření serveru Azure Database for MySQL ve skupině prostředků Azure."
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
ms.openlocfilehash: 04fc441aee7a4c8adc4f02d5e51b2d9e64400f55
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a><span data-ttu-id="ba4b9-103">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ba4b9-103">Create an Azure Database for MySQL server using Azure CLI</span></span>
<span data-ttu-id="ba4b9-104">Tento rychlý start popisuje, jak za pět minut vytvořit pomocí Azure CLI server Azure Database for MySQL ve skupině prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-104">This quickstart describes how to use the Azure CLI to create an Azure Database for MySQL server in an Azure resource group in about five minutes.</span></span> <span data-ttu-id="ba4b9-105">Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-105">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span>

<span data-ttu-id="ba4b9-106">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-106">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ba4b9-107">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-107">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="ba4b9-108">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-108">Run `az --version` to find the version.</span></span> <span data-ttu-id="ba4b9-109">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ba4b9-109">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="ba4b9-110">Pokud máte více předplatných, vyberte odpovídající předplatné, ve kterém tento prostředek existuje nebo ve kterém se fakturuje.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-110">If you have multiple subscriptions, choose the appropriate subscription in which the resource exists or is billed for.</span></span> <span data-ttu-id="ba4b9-111">Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#set) určité ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-111">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="ba4b9-112">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="ba4b9-112">Create a resource group</span></span>
<span data-ttu-id="ba4b9-113">Vytvořte [skupinu prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) pomocí příkazu [az group create](https://docs.microsoft.com/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="ba4b9-113">Create an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) using the [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> <span data-ttu-id="ba4b9-114">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-114">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span>

<span data-ttu-id="ba4b9-115">Následující příklad vytvoří skupinu prostředků s názvem `myresourcegroup` v umístění `westus`.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-115">The following example creates a resource group named `myresourcegroup` in the `westus` location.</span></span>

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="ba4b9-116">Vytvoření serveru Azure Database for MySQL</span><span class="sxs-lookup"><span data-stu-id="ba4b9-116">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="ba4b9-117">Vytvořte server Azure Database for MySQL pomocí příkazu **az mysql server create**.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-117">Create an Azure Database for MySQL server with the **az mysql server create** command.</span></span> <span data-ttu-id="ba4b9-118">Server může spravovat více databází.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-118">A server can manage multiple databases.</span></span> <span data-ttu-id="ba4b9-119">Obvykle se pro jednotlivé projekty nebo uživatele používají samostatné databáze.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-119">Typically, a separate database is used for each project or for each user.</span></span>

<span data-ttu-id="ba4b9-120">Následující příklad vytvoří server Azure Database for MySQL, jehož umístěním je `westus` ve skupině prostředků `myresourcegroup` s názvem `myserver4demo`.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-120">The following example creates an Azure Database for MySQL server located in `westus` in the resource group `myresourcegroup` with name `myserver4demo`.</span></span> <span data-ttu-id="ba4b9-121">Server má správce s přihlašovacím jménem `myadmin` a heslem `Password01!`.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-121">The server has an administrator log in named `myadmin` and password `Password01!`.</span></span> <span data-ttu-id="ba4b9-122">Server je vytvořen s úrovní výkonu **Basic** a **50** výpočetními jednotkami sdílenými mezi všemi databázemi na serveru.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-122">The server is created with **Basic** performance tier and **50** compute units shared between all the databases in the server.</span></span> <span data-ttu-id="ba4b9-123">Výpočetní jednotky a úložiště můžete škálovat nahoru nebo dolů podle potřeb aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-123">You can scale compute and storage up or down depending on the application needs.</span></span>

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name myserver4demo --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a><span data-ttu-id="ba4b9-124">Konfigurace pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="ba4b9-124">Configure firewall rule</span></span>
<span data-ttu-id="ba4b9-125">Vytvořte pravidlo brány firewall na úrovni serveru pro Azure Database for MySQL pomocí příkazu **az mysql server firewall-rule create**.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-125">Create an Azure Database for MySQL server-level firewall rule using the **az mysql server firewall-rule create** command.</span></span> <span data-ttu-id="ba4b9-126">Pravidlo brány firewall na úrovni serveru umožňuje externí aplikaci, jako je například nástroj příkazového řádku **mysql.exe** nebo MySQL Workbench, aby se k vašemu serveru připojila prostřednictvím brány firewall služby Azure MySQL.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-126">A server-level firewall rule allows an external application, such as the **mysql.exe** command-line tool or MySQL Workbench to connect to your server through the Azure MySQL service firewall.</span></span> 

<span data-ttu-id="ba4b9-127">Následující příklad vytvoří pravidlo brány firewall pro předdefinovaný rozsah adres, který je v tomto příkladu tvořen celým možným rozsahem IP adres.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-127">The following example creates a firewall rule for a predefined address range, which in this example is the entire possible range of IP addresses.</span></span>

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server myserver4demo --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a><span data-ttu-id="ba4b9-128">Konfigurace nastavení SSL</span><span class="sxs-lookup"><span data-stu-id="ba4b9-128">Configure SSL settings</span></span>
<span data-ttu-id="ba4b9-129">Ve výchozím nastavení se připojení SSL mezi serverem a klientskými aplikacemi vynucuje.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-129">By default, SSL connections between your server and client applications are enforced.</span></span>  <span data-ttu-id="ba4b9-130">Zajišťuje se tak zabezpečení dat „v pohybu“ prostřednictvím šifrování datového streamu v internetu.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-130">This ensures security of "in-motion" data by encrypting the data stream over the internet.</span></span>  <span data-ttu-id="ba4b9-131">Abychom tento rychlý start zjednodušili, zakážeme u vašeho serveru připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-131">To make this quick start easier, we disable SSL connections for your server.</span></span>  <span data-ttu-id="ba4b9-132">Pro provozní servery toto nastavení nedoporučujeme.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-132">This is not recommended for production servers.</span></span>  <span data-ttu-id="ba4b9-133">Další informace najdete v tématu [Konfigurace připojení SSL v aplikaci pro zabezpečené připojení k Azure Database for MySQL](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="ba4b9-133">For more information, see [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md).</span></span>

<span data-ttu-id="ba4b9-134">Následující příklad zakazuje vynucování SSL na serveru MySQL.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-134">The following example disables enforcing SSL on your MySQL server.</span></span>
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name myserver4demo -g -n --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a><span data-ttu-id="ba4b9-135">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="ba4b9-135">Get the connection information</span></span>

<span data-ttu-id="ba4b9-136">Pokud se chcete připojit k serveru, budete muset zadat informace o hostiteli a přihlašovací údaje pro přístup.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-136">To connect to your server, you need to provide host information and access credentials.</span></span>

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name myserver4demo
```

<span data-ttu-id="ba4b9-137">Výsledek je ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-137">The result is in JSON format.</span></span> <span data-ttu-id="ba4b9-138">Poznamenejte si **fullyQualifiedDomainName** a **administratorLogin**.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-138">Make a note of the **fullyQualifiedDomainName** and **administratorLogin**.</span></span>
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

## <a name="connect-to-the-server-using-the-mysqlexe-command-line-tool"></a><span data-ttu-id="ba4b9-139">Připojení k serveru pomocí nástroje příkazového řádku mysql.exe</span><span class="sxs-lookup"><span data-stu-id="ba4b9-139">Connect to the server using the mysql.exe command-line tool</span></span>
<span data-ttu-id="ba4b9-140">Připojte se k serveru pomocí nástroje příkazového řádku **mysql.exe**.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-140">Connect to your server using the **mysql.exe** command-line tool.</span></span> <span data-ttu-id="ba4b9-141">MySQL můžete stáhnout [odsud](https://dev.mysql.com/downloads/) a nainstalovat do svého počítače.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-141">You can download MySQL from [here](https://dev.mysql.com/downloads/) and install it on your computer.</span></span> <span data-ttu-id="ba4b9-142">Místo toho můžete také kliknout na tlačítko **Vyzkoušet** ve vzorových kódech nebo na tlačítko `>_` v pravém horním panelu nástrojů na webu Azure Portal a spustit **Azure Cloud Shell**.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-142">Instead you may also click the **Try It** button on code samples, or the  `>_` button from the upper right toolbar in the Azure portal, and launch the **Azure Cloud Shell**.</span></span>

<span data-ttu-id="ba4b9-143">Zadejte další příkazy:</span><span class="sxs-lookup"><span data-stu-id="ba4b9-143">Type the next commands:</span></span> 

1. <span data-ttu-id="ba4b9-144">Připojení k serveru pomocí nástroje příkazového řádku **mysql**:</span><span class="sxs-lookup"><span data-stu-id="ba4b9-144">Connect to the server using **mysql** command-line tool:</span></span>
```azurecli-interactive
 mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. <span data-ttu-id="ba4b9-145">Zobrazení stavu serveru:</span><span class="sxs-lookup"><span data-stu-id="ba4b9-145">View server status:</span></span>
```sql
 mysql> status
```
<span data-ttu-id="ba4b9-146">Pokud vše půjde dobře, měl by výstupem nástroje příkazového řádku být následující text:</span><span class="sxs-lookup"><span data-stu-id="ba4b9-146">If everything goes well, the command-line tool should output the following text:</span></span>

```dos
C:\Users\>mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

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
> <span data-ttu-id="ba4b9-147">Další příkazy najdete v [Referenční příručce k MySQL 5.7 – v kapitole 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).</span><span class="sxs-lookup"><span data-stu-id="ba4b9-147">For additional commands, see [MySQL 5.7 Reference Manual - Chapter 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).</span></span>

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a><span data-ttu-id="ba4b9-148">Připojení k serveru pomocí nástroje grafického uživatelského rozhraní MySQL Workbench</span><span class="sxs-lookup"><span data-stu-id="ba4b9-148">Connect to the server using the MySQL Workbench GUI tool</span></span>
1.  <span data-ttu-id="ba4b9-149">Na klientském počítači spusťte aplikaci MySQL Workbench.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-149">Launch the MySQL Workbench application on your client computer.</span></span> <span data-ttu-id="ba4b9-150">MySQL Workbench můžete stáhnout a nainstalovat [odtud](https://dev.mysql.com/downloads/workbench/).</span><span class="sxs-lookup"><span data-stu-id="ba4b9-150">You can download and install MySQL Workbench from [here](https://dev.mysql.com/downloads/workbench/).</span></span>

2.  <span data-ttu-id="ba4b9-151">V dialogovém okně pro **nastavení nového připojení** zadejte na kartě **Parametry** následující informace:</span><span class="sxs-lookup"><span data-stu-id="ba4b9-151">In the **Setup New Connection** dialog box, enter the following information on **Parameters** tab:</span></span>

   ![nastavení nového připojení](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| <span data-ttu-id="ba4b9-153">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="ba4b9-153">**Setting**</span></span> | <span data-ttu-id="ba4b9-154">**Navrhovaná hodnota**</span><span class="sxs-lookup"><span data-stu-id="ba4b9-154">**Suggested Value**</span></span> | <span data-ttu-id="ba4b9-155">**Popis**</span><span class="sxs-lookup"><span data-stu-id="ba4b9-155">**Description**</span></span> |
|---|---|---|
|   <span data-ttu-id="ba4b9-156">Název připojení</span><span class="sxs-lookup"><span data-stu-id="ba4b9-156">Connection Name</span></span> | <span data-ttu-id="ba4b9-157">My Connection</span><span class="sxs-lookup"><span data-stu-id="ba4b9-157">My Connection</span></span> | <span data-ttu-id="ba4b9-158">Zadejte jmenovku pro toto připojení (může být libovolná).</span><span class="sxs-lookup"><span data-stu-id="ba4b9-158">Specify a label for this connection (this can be anything)</span></span> |
| <span data-ttu-id="ba4b9-159">Způsob připojení</span><span class="sxs-lookup"><span data-stu-id="ba4b9-159">Connection Method</span></span> | <span data-ttu-id="ba4b9-160">zvolte Standardní (TCP/IP)</span><span class="sxs-lookup"><span data-stu-id="ba4b9-160">choose Standard (TCP/IP)</span></span> | <span data-ttu-id="ba4b9-161">Pro připojení k Azure Database for MySQL použijte protokol TCP/IP.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-161">Use TCP/IP protocol to connect to Azure Database for MySQL></span></span> |
| <span data-ttu-id="ba4b9-162">Název hostitele</span><span class="sxs-lookup"><span data-stu-id="ba4b9-162">Hostname</span></span> | <span data-ttu-id="ba4b9-163">myserver4demo.mysql.database.azure.com</span><span class="sxs-lookup"><span data-stu-id="ba4b9-163">myserver4demo.mysql.database.azure.com</span></span> | <span data-ttu-id="ba4b9-164">Název serveru, který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-164">Server name you previously noted.</span></span> |
| <span data-ttu-id="ba4b9-165">Port</span><span class="sxs-lookup"><span data-stu-id="ba4b9-165">Port</span></span> | <span data-ttu-id="ba4b9-166">3306</span><span class="sxs-lookup"><span data-stu-id="ba4b9-166">3306</span></span> | <span data-ttu-id="ba4b9-167">Použije se výchozí port pro MySQL.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-167">The default port for MySQL is used.</span></span> |
| <span data-ttu-id="ba4b9-168">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="ba4b9-168">Username</span></span> | myadmin@myserver4demo | <span data-ttu-id="ba4b9-169">Přihlašovací jméno správce serveru, které jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-169">The server admin login you previously noted.</span></span> |
| <span data-ttu-id="ba4b9-170">Heslo</span><span class="sxs-lookup"><span data-stu-id="ba4b9-170">Password</span></span> | **** | <span data-ttu-id="ba4b9-171">Použijte heslo správce, které jste nakonfigurovali v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-171">Use the admin account password you configured earlier.</span></span> |

<span data-ttu-id="ba4b9-172">Pokud chcete otestovat, jestli jsou všechny parametry správně nakonfigurované, klikněte na **Test připojení**.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-172">Click **Test Connection** to test if all parameters are correctly configured.</span></span>
<span data-ttu-id="ba4b9-173">Teď se můžete kliknutím na toto připojení úspěšně připojit k serveru.</span><span class="sxs-lookup"><span data-stu-id="ba4b9-173">Now, you can click the connection to successfully connect to the server.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="ba4b9-174">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="ba4b9-174">Clean up resources</span></span>
<span data-ttu-id="ba4b9-175">Pokud tyto prostředky nepotřebujete pro další rychlý start nebo kurz, můžete je pomocí následujícího příkazu odstranit:</span><span class="sxs-lookup"><span data-stu-id="ba4b9-175">If you don't need these resources for another quickstart/tutorial, you can delete them by doing the following command:</span></span> 

```azurecli-interactive
az group delete --name myresourcegroup
```

## <a name="next-steps"></a><span data-ttu-id="ba4b9-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba4b9-176">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ba4b9-177">Návrh databáze MySQL pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="ba4b9-177">Design a MySQL Database with Azure CLI</span></span>](./tutorial-design-database-using-cli.md)
