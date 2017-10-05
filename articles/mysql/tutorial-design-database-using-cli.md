---
title: "Navrhnout první databáze Azure pro databázi MySQL. rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento kurz vysvětluje, jak vytvořit a spravovat Azure databáze MySQL server a databáze pomocí příkazového řádku Azure CLI 2.0."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.custom: mvc
ms.openlocfilehash: 590cba6cb58d0c0eaedb9f122ac048c33988004d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a><span data-ttu-id="8182d-103">Navrhnout první databáze Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="8182d-103">Design your first Azure Database for MySQL database</span></span>

<span data-ttu-id="8182d-104">Služba relační databáze v Microsoft cloudu podle databázový stroj MySQL Community Edition je Azure databáze pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="8182d-104">Azure Database for MySQL is a relational database service in the Microsoft cloud based on MySQL Community Edition database engine.</span></span> <span data-ttu-id="8182d-105">V tomto kurzu budete používat rozhraní příkazového řádku Azure (rozhraní příkazového řádku) a další nástroje pro další postup:</span><span class="sxs-lookup"><span data-stu-id="8182d-105">In this tutorial, you use Azure CLI (command-line interface) and other utilities to learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8182d-106">Vytvoření Azure databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="8182d-106">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="8182d-107">Konfigurace brány firewall serveru</span><span class="sxs-lookup"><span data-stu-id="8182d-107">Configure the server firewall</span></span>
> * <span data-ttu-id="8182d-108">Použití [nástroj pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) k vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="8182d-108">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) to create a database</span></span>
> * <span data-ttu-id="8182d-109">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="8182d-109">Load sample data</span></span>
> * <span data-ttu-id="8182d-110">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="8182d-110">Query data</span></span>
> * <span data-ttu-id="8182d-111">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="8182d-111">Update data</span></span>
> * <span data-ttu-id="8182d-112">Obnovení dat</span><span class="sxs-lookup"><span data-stu-id="8182d-112">Restore data</span></span>

<span data-ttu-id="8182d-113">Můžete použít prostředí cloudové služby Azure v prohlížeči nebo [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli) ve vašem počítači ke spuštění bloky kódu v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="8182d-113">You may use the Azure Cloud Shell in the browser, or [Install Azure CLI 2.0]( /cli/azure/install-azure-cli) on your own computer to run the code blocks in this tutorial.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="8182d-114">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8182d-114">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="8182d-115">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="8182d-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="8182d-116">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8182d-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="8182d-117">Pokud máte více předplatných, vyberte odpovídající předplatné, ve kterém tento prostředek existuje nebo ve kterém se fakturuje.</span><span class="sxs-lookup"><span data-stu-id="8182d-117">If you have multiple subscriptions, choose the appropriate subscription in which the resource exists or is billed for.</span></span> <span data-ttu-id="8182d-118">Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#set) určité ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="8182d-118">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="8182d-119">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="8182d-119">Create a resource group</span></span>
<span data-ttu-id="8182d-120">Vytvoření [skupina prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) s [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="8182d-120">Create an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) with [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> <span data-ttu-id="8182d-121">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.</span><span class="sxs-lookup"><span data-stu-id="8182d-121">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span>

<span data-ttu-id="8182d-122">Následující příklad vytvoří skupinu prostředků s názvem `mycliresource` v umístění `westus`.</span><span class="sxs-lookup"><span data-stu-id="8182d-122">The following example creates a resource group named `mycliresource` in the `westus` location.</span></span>

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="8182d-123">Vytvoření serveru Azure Database for MySQL</span><span class="sxs-lookup"><span data-stu-id="8182d-123">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="8182d-124">Vytvoření databáze Azure pro server databáze MySQL se serverem mysql az vytvořit příkaz.</span><span class="sxs-lookup"><span data-stu-id="8182d-124">Create an Azure Database for MySQL server with the az mysql server create command.</span></span> <span data-ttu-id="8182d-125">Server může spravovat více databází.</span><span class="sxs-lookup"><span data-stu-id="8182d-125">A server can manage multiple databases.</span></span> <span data-ttu-id="8182d-126">Obvykle se pro jednotlivé projekty nebo uživatele používají samostatné databáze.</span><span class="sxs-lookup"><span data-stu-id="8182d-126">Typically, a separate database is used for each project or for each user.</span></span>

<span data-ttu-id="8182d-127">Následující příklad vytvoří server Azure Database for MySQL, jehož umístěním je `westus` ve skupině prostředků `mycliresource` s názvem `mycliserver`.</span><span class="sxs-lookup"><span data-stu-id="8182d-127">The following example creates an Azure Database for MySQL server located in `westus` in the resource group `mycliresource` with name `mycliserver`.</span></span> <span data-ttu-id="8182d-128">Server má správce s přihlašovacím jménem `myadmin` a heslem `Password01!`.</span><span class="sxs-lookup"><span data-stu-id="8182d-128">The server has an administrator log in named `myadmin` and password `Password01!`.</span></span> <span data-ttu-id="8182d-129">Server je vytvořen s úrovní výkonu **Basic** a **50** výpočetními jednotkami sdílenými mezi všemi databázemi na serveru.</span><span class="sxs-lookup"><span data-stu-id="8182d-129">The server is created with **Basic** performance tier and **50** compute units shared between all the databases in the server.</span></span> <span data-ttu-id="8182d-130">Výpočetní jednotky a úložiště můžete škálovat nahoru nebo dolů podle potřeb aplikace.</span><span class="sxs-lookup"><span data-stu-id="8182d-130">You can scale compute and storage up or down depending on the application needs.</span></span>

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver
--location westus --user myadmin --password Password01!
--performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a><span data-ttu-id="8182d-131">Konfigurace pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="8182d-131">Configure firewall rule</span></span>
<span data-ttu-id="8182d-132">Vytvoření databáze Azure pro pravidlo brány firewall na úrovni serveru MySQL s az mysql serveru pravidlo brány firewall-vytvoření příkazu.</span><span class="sxs-lookup"><span data-stu-id="8182d-132">Create an Azure Database for MySQL server-level firewall rule with the az mysql server firewall-rule create command.</span></span> <span data-ttu-id="8182d-133">Pravidlo brány firewall na úrovni serveru umožňuje externí aplikací, jako například **mysql** nástroj příkazového řádku nebo MySQL Workbench, aby se připojení k serveru přes bránu firewall služby Azure MySQL.</span><span class="sxs-lookup"><span data-stu-id="8182d-133">A server-level firewall rule allows an external application, such as **mysql** command-line tool or MySQL Workbench to connect to your server through the Azure MySQL service firewall.</span></span> 

<span data-ttu-id="8182d-134">Následující příklad vytvoří pravidlo brány firewall pro rozsah předdefinovaných adres.</span><span class="sxs-lookup"><span data-stu-id="8182d-134">The following example creates a firewall rule for a predefined address range.</span></span> <span data-ttu-id="8182d-135">Tento příklad ukazuje celý možné rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="8182d-135">This example shows the entire possible range of IP addresses.</span></span>

```azurecli-interactive
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="get-the-connection-information"></a><span data-ttu-id="8182d-136">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="8182d-136">Get the connection information</span></span>

<span data-ttu-id="8182d-137">Pokud se chcete připojit k serveru, budete muset zadat informace o hostiteli a přihlašovací údaje pro přístup.</span><span class="sxs-lookup"><span data-stu-id="8182d-137">To connect to your server, you need to provide host information and access credentials.</span></span>
```azurecli-interactive
az mysql server show --resource-group mycliresource --name mycliserver
```

<span data-ttu-id="8182d-138">Výsledek je ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="8182d-138">The result is in JSON format.</span></span> <span data-ttu-id="8182d-139">Poznamenejte si **fullyQualifiedDomainName** a **administratorLogin**.</span><span class="sxs-lookup"><span data-stu-id="8182d-139">Make a note of the **fullyQualifiedDomainName** and **administratorLogin**.</span></span>
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

## <a name="connect-to-the-server-using-mysql"></a><span data-ttu-id="8182d-140">Připojit k serveru pomocí mysql</span><span class="sxs-lookup"><span data-stu-id="8182d-140">Connect to the server using mysql</span></span>
<span data-ttu-id="8182d-141">Použití [nástroj pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) k navázání připojení k vaší databázi Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="8182d-141">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) to establish a connection to your Azure Database for MySQL server.</span></span> <span data-ttu-id="8182d-142">V tomto příkladu je příkaz:</span><span class="sxs-lookup"><span data-stu-id="8182d-142">In this example, the command is:</span></span>
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a><span data-ttu-id="8182d-143">Vytvoření prázdné databáze</span><span class="sxs-lookup"><span data-stu-id="8182d-143">Create a blank database</span></span>
<span data-ttu-id="8182d-144">Po připojení k serveru vytvořte prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="8182d-144">Once you’re connected to the server, create a blank database.</span></span>
```sql
mysql> CREATE DATABASE mysampledb;
```

<span data-ttu-id="8182d-145">Do příkazového řádku spusťte následující příkaz k přepínači připojení k této nově vytvořené databáze:</span><span class="sxs-lookup"><span data-stu-id="8182d-145">At the prompt, run the following command to switch the connection to this newly created database:</span></span>
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a><span data-ttu-id="8182d-146">Vytváření tabulek v databázi</span><span class="sxs-lookup"><span data-stu-id="8182d-146">Create tables in the database</span></span>
<span data-ttu-id="8182d-147">Teď, když víte, jak se připojit k databázi Azure pro databázi MySQL, jsme projít jak provést některé základní úlohy.</span><span class="sxs-lookup"><span data-stu-id="8182d-147">Now that you know how to connect to the Azure Database for MySQL database, we can go over how to complete some basic tasks.</span></span>

<span data-ttu-id="8182d-148">Jsme nejprve vytvořit tabulku a načíst určitými daty.</span><span class="sxs-lookup"><span data-stu-id="8182d-148">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="8182d-149">Umožňuje vytvořit tabulku, která ukládá informace o inventáři.</span><span class="sxs-lookup"><span data-stu-id="8182d-149">Let's create a table that stores inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a><span data-ttu-id="8182d-150">Načtení dat do tabulky</span><span class="sxs-lookup"><span data-stu-id="8182d-150">Load data into the tables</span></span>
<span data-ttu-id="8182d-151">Teď, když máme tabulku, jsme do něj vložte některá data.</span><span class="sxs-lookup"><span data-stu-id="8182d-151">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="8182d-152">V okně Otevřít příkazového řádku spusťte následující dotaz vložit některé řádky dat.</span><span class="sxs-lookup"><span data-stu-id="8182d-152">At the open command prompt window, run the following query to insert some rows of data.</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="8182d-153">Nyní máte dva řádky ukázkových dat do tabulky, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="8182d-153">Now you have two rows of sample data into the table you created earlier.</span></span>

## <a name="query-and-update-the-data-in-the-tables"></a><span data-ttu-id="8182d-154">Dotaz a aktualizovat data v tabulkách</span><span class="sxs-lookup"><span data-stu-id="8182d-154">Query and update the data in the tables</span></span>
<span data-ttu-id="8182d-155">Spustíte následující dotaz pro načtení informací z tabulky databáze.</span><span class="sxs-lookup"><span data-stu-id="8182d-155">Execute the following query to retrieve information from the database table.</span></span>
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="8182d-156">Můžete také aktualizovat data v tabulkách.</span><span class="sxs-lookup"><span data-stu-id="8182d-156">You can also update the data in the tables.</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="8182d-157">Načtení dat získá příslušným způsobem aktualizuje řádek.</span><span class="sxs-lookup"><span data-stu-id="8182d-157">The row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a><span data-ttu-id="8182d-158">Obnovení databáze k dřívějšímu bodu v čase</span><span class="sxs-lookup"><span data-stu-id="8182d-158">Restore a database to a previous point in time</span></span>
<span data-ttu-id="8182d-159">Představte si, že jste omylem odstranili této tabulky.</span><span class="sxs-lookup"><span data-stu-id="8182d-159">Imagine you have accidentally deleted this table.</span></span> <span data-ttu-id="8182d-160">Toto je něco, které nelze snadno obnovit z.</span><span class="sxs-lookup"><span data-stu-id="8182d-160">This is something you cannot easily recover from.</span></span> <span data-ttu-id="8182d-161">Databáze pro databázi MySQL Azure umožňuje vrátit do libovolného bodu v čase v poslední až 35 dnů a obnovit tento bod v čase na nový server.</span><span class="sxs-lookup"><span data-stu-id="8182d-161">Azure Database for MySQL allows you to go back to any point in time in the last up to 35 days and restore this point in time to a new server.</span></span> <span data-ttu-id="8182d-162">Tento nový server můžete obnovit odstraněná data.</span><span class="sxs-lookup"><span data-stu-id="8182d-162">You can use this new server to recover your deleted data.</span></span> <span data-ttu-id="8182d-163">Ukázka serveru bod před přidáním tabulky obnovit následující kroky.</span><span class="sxs-lookup"><span data-stu-id="8182d-163">The following steps restore the sample server to a point before the table was added.</span></span>

<span data-ttu-id="8182d-164">Pro obnovení je třeba následující informace:</span><span class="sxs-lookup"><span data-stu-id="8182d-164">For the Restore you need the following information:</span></span>

- <span data-ttu-id="8182d-165">Bod obnovení: Vyberte bodu v čase, k níž dojde před server byl změněn.</span><span class="sxs-lookup"><span data-stu-id="8182d-165">Restore point: Select a point-in-time that occurs before the server was changed.</span></span> <span data-ttu-id="8182d-166">Musí být větší než nebo rovna hodnotě pro nejstarší zálohy zdrojové databáze.</span><span class="sxs-lookup"><span data-stu-id="8182d-166">Must be greater than or equal to the source database's Oldest backup value.</span></span>
- <span data-ttu-id="8182d-167">Cílový server: Zadejte nový název serveru, kterou chcete obnovit</span><span class="sxs-lookup"><span data-stu-id="8182d-167">Target server: Provide a new server name you want to restore to</span></span>
- <span data-ttu-id="8182d-168">Zdrojový server: Zadejte název serveru, kterou chcete obnovit z</span><span class="sxs-lookup"><span data-stu-id="8182d-168">Source server: Provide the name of the server you want to restore from</span></span>
- <span data-ttu-id="8182d-169">Umístění: Nelze vyberte oblast, ve výchozím nastavení je stejné jako na zdrojovém serveru</span><span class="sxs-lookup"><span data-stu-id="8182d-169">Location: You cannot select the region, by default it is same as the source server</span></span>

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

<span data-ttu-id="8182d-170">K obnovení serveru a [obnovení v daném okamžiku](./howto-restore-server-portal.md) předtím, než tabulka byla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="8182d-170">To restore the server and [restore to a point-in-time](./howto-restore-server-portal.md) before the table was deleted.</span></span> <span data-ttu-id="8182d-171">Obnovení serveru do jiného bodu v čase vytvoří duplicitní nový server jako původní server od bodu v čase, které zadáte, za předpokladu, že je v rámci dobu uchování vašeho [vrstvy služby](./concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="8182d-171">Restoring a server to a different point in time creates a duplicate new server as the original server as of the point in time you specify, provided that it is within the retention period for your [service tier](./concepts-service-tiers.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8182d-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8182d-172">Next Steps</span></span>
<span data-ttu-id="8182d-173">V tomto kurzu jste se dozvěděli na:</span><span class="sxs-lookup"><span data-stu-id="8182d-173">In this tutorial you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="8182d-174">Vytvoření Azure databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="8182d-174">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="8182d-175">Konfigurace brány firewall serveru</span><span class="sxs-lookup"><span data-stu-id="8182d-175">Configure the server firewall</span></span>
> * <span data-ttu-id="8182d-176">Použití [nástroj pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) k vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="8182d-176">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) to create a database</span></span>
> * <span data-ttu-id="8182d-177">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="8182d-177">Load sample data</span></span>
> * <span data-ttu-id="8182d-178">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="8182d-178">Query data</span></span>
> * <span data-ttu-id="8182d-179">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="8182d-179">Update data</span></span>
> * <span data-ttu-id="8182d-180">Obnovení dat</span><span class="sxs-lookup"><span data-stu-id="8182d-180">Restore data</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8182d-181">Azure databáze pro databázi MySQL – ukázky rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="8182d-181">Azure Database for MySQL - Azure CLI samples</span></span>](./sample-scripts-azure-cli.md)
