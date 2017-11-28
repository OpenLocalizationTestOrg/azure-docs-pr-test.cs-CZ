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
# <a name="design-your-first-azure-database-for-mysql-database"></a><span data-ttu-id="a628f-103">Navrhnout první databáze Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="a628f-103">Design your first Azure Database for MySQL database</span></span>

<span data-ttu-id="a628f-104">Azure databáze pro databázi MySQL je služba relační databáze v hello cloudu společnosti Microsoft podle MySQL Community Edition databázového stroje.</span><span class="sxs-lookup"><span data-stu-id="a628f-104">Azure Database for MySQL is a relational database service in hello Microsoft cloud based on MySQL Community Edition database engine.</span></span> <span data-ttu-id="a628f-105">V tomto kurzu použijete rozhraní příkazového řádku Azure (rozhraní příkazového řádku) a dalších nástrojů toolearn jak na:</span><span class="sxs-lookup"><span data-stu-id="a628f-105">In this tutorial, you use Azure CLI (command-line interface) and other utilities toolearn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a628f-106">Vytvoření Azure databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="a628f-106">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="a628f-107">Konfigurace brány firewall serveru hello</span><span class="sxs-lookup"><span data-stu-id="a628f-107">Configure hello server firewall</span></span>
> * <span data-ttu-id="a628f-108">Použití [nástroj pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate databáze</span><span class="sxs-lookup"><span data-stu-id="a628f-108">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate a database</span></span>
> * <span data-ttu-id="a628f-109">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="a628f-109">Load sample data</span></span>
> * <span data-ttu-id="a628f-110">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="a628f-110">Query data</span></span>
> * <span data-ttu-id="a628f-111">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="a628f-111">Update data</span></span>
> * <span data-ttu-id="a628f-112">Obnovení dat</span><span class="sxs-lookup"><span data-stu-id="a628f-112">Restore data</span></span>

<span data-ttu-id="a628f-113">Můžete použít hello prostředí cloudu Azure v prohlížeči hello nebo [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli) na vlastní počítače toorun hello bloky kódu v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="a628f-113">You may use hello Azure Cloud Shell in hello browser, or [Install Azure CLI 2.0]( /cli/azure/install-azure-cli) on your own computer toorun hello code blocks in this tutorial.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a628f-114">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a628f-114">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="a628f-115">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="a628f-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="a628f-116">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a628f-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="a628f-117">Pokud máte více předplatných, vyberte odpovídající předplatné hello, ve kterém hello prostředek neexistuje nebo se fakturuje pro.</span><span class="sxs-lookup"><span data-stu-id="a628f-117">If you have multiple subscriptions, choose hello appropriate subscription in which hello resource exists or is billed for.</span></span> <span data-ttu-id="a628f-118">Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#set) určité ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="a628f-118">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="a628f-119">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a628f-119">Create a resource group</span></span>
<span data-ttu-id="a628f-120">Vytvoření [skupina prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) s [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a628f-120">Create an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) with [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> <span data-ttu-id="a628f-121">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.</span><span class="sxs-lookup"><span data-stu-id="a628f-121">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span>

<span data-ttu-id="a628f-122">Hello následující příklad vytvoří skupinu prostředků s názvem `mycliresource` v hello `westus` umístění.</span><span class="sxs-lookup"><span data-stu-id="a628f-122">hello following example creates a resource group named `mycliresource` in hello `westus` location.</span></span>

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="a628f-123">Vytvoření serveru Azure Database for MySQL</span><span class="sxs-lookup"><span data-stu-id="a628f-123">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="a628f-124">Vytvoření databáze Azure pro server databáze MySQL se serverem mysql az hello vytvoření příkazu.</span><span class="sxs-lookup"><span data-stu-id="a628f-124">Create an Azure Database for MySQL server with hello az mysql server create command.</span></span> <span data-ttu-id="a628f-125">Server může spravovat více databází.</span><span class="sxs-lookup"><span data-stu-id="a628f-125">A server can manage multiple databases.</span></span> <span data-ttu-id="a628f-126">Obvykle se pro jednotlivé projekty nebo uživatele používají samostatné databáze.</span><span class="sxs-lookup"><span data-stu-id="a628f-126">Typically, a separate database is used for each project or for each user.</span></span>

<span data-ttu-id="a628f-127">Hello následující příklad vytvoří databázi Azure pro server databáze MySQL, které jsou umístěné v `westus` ve skupině prostředků hello `mycliresource` s názvem `mycliserver`.</span><span class="sxs-lookup"><span data-stu-id="a628f-127">hello following example creates an Azure Database for MySQL server located in `westus` in hello resource group `mycliresource` with name `mycliserver`.</span></span> <span data-ttu-id="a628f-128">protokol správce má Hello server v s názvem `myadmin` a heslo `Password01!`.</span><span class="sxs-lookup"><span data-stu-id="a628f-128">hello server has an administrator log in named `myadmin` and password `Password01!`.</span></span> <span data-ttu-id="a628f-129">Hello server je vytvořen s **základní** úroveň výkonu a **50** výpočetní jednotky sdílena mezi všechny hello databází na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="a628f-129">hello server is created with **Basic** performance tier and **50** compute units shared between all hello databases in hello server.</span></span> <span data-ttu-id="a628f-130">Můžete škálovat výpočetní a úložnou nahoru nebo dolů v závislosti na potřebám aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a628f-130">You can scale compute and storage up or down depending on hello application needs.</span></span>

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver
--location westus --user myadmin --password Password01!
--performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a><span data-ttu-id="a628f-131">Konfigurace pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="a628f-131">Configure firewall rule</span></span>
<span data-ttu-id="a628f-132">Vytvoření databáze Azure pro pravidlo brány firewall na úrovni serveru MySQL s hello az mysql pravidla brány firewall-vytvoření příkazu.</span><span class="sxs-lookup"><span data-stu-id="a628f-132">Create an Azure Database for MySQL server-level firewall rule with hello az mysql server firewall-rule create command.</span></span> <span data-ttu-id="a628f-133">Pravidlo brány firewall na úrovni serveru umožňuje externí aplikací, jako například **mysql** nástroj příkazového řádku nebo MySQL Workbench tooconnect tooyour server prostřednictvím brány firewall služby Azure MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="a628f-133">A server-level firewall rule allows an external application, such as **mysql** command-line tool or MySQL Workbench tooconnect tooyour server through hello Azure MySQL service firewall.</span></span> 

<span data-ttu-id="a628f-134">Hello následující příklad vytvoří pravidlo brány firewall pro rozsah předdefinovaných adres.</span><span class="sxs-lookup"><span data-stu-id="a628f-134">hello following example creates a firewall rule for a predefined address range.</span></span> <span data-ttu-id="a628f-135">Tento příklad ukazuje hello celý možné rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="a628f-135">This example shows hello entire possible range of IP addresses.</span></span>

```azurecli-interactive
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="get-hello-connection-information"></a><span data-ttu-id="a628f-136">Získat informace o připojení hello</span><span class="sxs-lookup"><span data-stu-id="a628f-136">Get hello connection information</span></span>

<span data-ttu-id="a628f-137">tooconnect tooyour serveru, musíte tooprovide informace a přístup k přihlašovacím údajům hostitele.</span><span class="sxs-lookup"><span data-stu-id="a628f-137">tooconnect tooyour server, you need tooprovide host information and access credentials.</span></span>
```azurecli-interactive
az mysql server show --resource-group mycliresource --name mycliserver
```

<span data-ttu-id="a628f-138">Výsledkem Hello je ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="a628f-138">hello result is in JSON format.</span></span> <span data-ttu-id="a628f-139">Poznamenejte si hello **Plně_kvalifikovaný_název_domény** a **administratorLogin**.</span><span class="sxs-lookup"><span data-stu-id="a628f-139">Make a note of hello **fullyQualifiedDomainName** and **administratorLogin**.</span></span>
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

## <a name="connect-toohello-server-using-mysql"></a><span data-ttu-id="a628f-140">Připojení serveru toohello pomocí mysql</span><span class="sxs-lookup"><span data-stu-id="a628f-140">Connect toohello server using mysql</span></span>
<span data-ttu-id="a628f-141">Použití [nástroj pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) tooestablish tooyour připojení databáze Azure pro server databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="a628f-141">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) tooestablish a connection tooyour Azure Database for MySQL server.</span></span> <span data-ttu-id="a628f-142">V tomto příkladu je hello příkaz:</span><span class="sxs-lookup"><span data-stu-id="a628f-142">In this example, hello command is:</span></span>
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a><span data-ttu-id="a628f-143">Vytvoření prázdné databáze</span><span class="sxs-lookup"><span data-stu-id="a628f-143">Create a blank database</span></span>
<span data-ttu-id="a628f-144">Jakmile jste server připojený toohello, vytvořte prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="a628f-144">Once you’re connected toohello server, create a blank database.</span></span>
```sql
mysql> CREATE DATABASE mysampledb;
```

<span data-ttu-id="a628f-145">V řádku hello spusťte následující příkaz tooswitch hello připojení toothis nově vytvořený databáze hello:</span><span class="sxs-lookup"><span data-stu-id="a628f-145">At hello prompt, run hello following command tooswitch hello connection toothis newly created database:</span></span>
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-hello-database"></a><span data-ttu-id="a628f-146">Vytváření tabulek v databázi hello</span><span class="sxs-lookup"><span data-stu-id="a628f-146">Create tables in hello database</span></span>
<span data-ttu-id="a628f-147">Teď, když víte, jak tooconnect toohello Azure Database pro databázi MySQL, jsme můžete projít postupy toocomplete některé základní úlohy.</span><span class="sxs-lookup"><span data-stu-id="a628f-147">Now that you know how tooconnect toohello Azure Database for MySQL database, we can go over how toocomplete some basic tasks.</span></span>

<span data-ttu-id="a628f-148">Jsme nejprve vytvořit tabulku a načíst určitými daty.</span><span class="sxs-lookup"><span data-stu-id="a628f-148">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="a628f-149">Umožňuje vytvořit tabulku, která ukládá informace o inventáři.</span><span class="sxs-lookup"><span data-stu-id="a628f-149">Let's create a table that stores inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-hello-tables"></a><span data-ttu-id="a628f-150">Načtení dat do tabulky hello</span><span class="sxs-lookup"><span data-stu-id="a628f-150">Load data into hello tables</span></span>
<span data-ttu-id="a628f-151">Teď, když máme tabulku, jsme do něj vložte některá data.</span><span class="sxs-lookup"><span data-stu-id="a628f-151">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="a628f-152">V okně hello spusťte příkazový řádek spusťte hello následující dotaz tooinsert některé řádky dat.</span><span class="sxs-lookup"><span data-stu-id="a628f-152">At hello open command prompt window, run hello following query tooinsert some rows of data.</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="a628f-153">Nyní máte dva řádky ukázková data do hello tabulky, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="a628f-153">Now you have two rows of sample data into hello table you created earlier.</span></span>

## <a name="query-and-update-hello-data-in-hello-tables"></a><span data-ttu-id="a628f-154">Dotazování a aktualizovat hello data v tabulkách hello</span><span class="sxs-lookup"><span data-stu-id="a628f-154">Query and update hello data in hello tables</span></span>
<span data-ttu-id="a628f-155">Spusťte následující dotaz tooretrieve informace z tabulky databáze hello hello.</span><span class="sxs-lookup"><span data-stu-id="a628f-155">Execute hello following query tooretrieve information from hello database table.</span></span>
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="a628f-156">Můžete také aktualizovat hello data v tabulkách hello.</span><span class="sxs-lookup"><span data-stu-id="a628f-156">You can also update hello data in hello tables.</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="a628f-157">Při načítání dat, získá Hello řádek příslušným způsobem aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="a628f-157">hello row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a><span data-ttu-id="a628f-158">Obnovit do databáze tooa předchozího bodu v čase</span><span class="sxs-lookup"><span data-stu-id="a628f-158">Restore a database tooa previous point in time</span></span>
<span data-ttu-id="a628f-159">Představte si, že jste omylem odstranili této tabulky.</span><span class="sxs-lookup"><span data-stu-id="a628f-159">Imagine you have accidentally deleted this table.</span></span> <span data-ttu-id="a628f-160">Toto je něco, které nelze snadno obnovit z.</span><span class="sxs-lookup"><span data-stu-id="a628f-160">This is something you cannot easily recover from.</span></span> <span data-ttu-id="a628f-161">Azure databáze pro databázi MySQL vám umožní toogo back tooany bodu v čase v hello poslední až too35 dnů a obnovit tento bod v časové tooa nový server.</span><span class="sxs-lookup"><span data-stu-id="a628f-161">Azure Database for MySQL allows you toogo back tooany point in time in hello last up too35 days and restore this point in time tooa new server.</span></span> <span data-ttu-id="a628f-162">Můžete použít tento nový server toorecover odstraněná data.</span><span class="sxs-lookup"><span data-stu-id="a628f-162">You can use this new server toorecover your deleted data.</span></span> <span data-ttu-id="a628f-163">Následující kroky obnovení hello ukázkový server tooa bod před přidáním tabulky hello Hello.</span><span class="sxs-lookup"><span data-stu-id="a628f-163">hello following steps restore hello sample server tooa point before hello table was added.</span></span>

<span data-ttu-id="a628f-164">Hello obnovení musíte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="a628f-164">For hello Restore you need hello following information:</span></span>

- <span data-ttu-id="a628f-165">Bod obnovení: Vyberte bodu v čase, k níž dojde před hello server byl změněn.</span><span class="sxs-lookup"><span data-stu-id="a628f-165">Restore point: Select a point-in-time that occurs before hello server was changed.</span></span> <span data-ttu-id="a628f-166">Musí být větší než nebo roven hodnotě nejstarší zálohování hodnota toohello zdrojové databáze.</span><span class="sxs-lookup"><span data-stu-id="a628f-166">Must be greater than or equal toohello source database's Oldest backup value.</span></span>
- <span data-ttu-id="a628f-167">Cílový server: Zadejte nový název serveru, který chcete toorestore k</span><span class="sxs-lookup"><span data-stu-id="a628f-167">Target server: Provide a new server name you want toorestore to</span></span>
- <span data-ttu-id="a628f-168">Zdrojový server: Zadejte název hello hello serveru chcete toorestore z</span><span class="sxs-lookup"><span data-stu-id="a628f-168">Source server: Provide hello name of hello server you want toorestore from</span></span>
- <span data-ttu-id="a628f-169">Umístění: Nelze vybrat hello oblast, ve výchozím nastavení je stejný jako zdrojový server hello</span><span class="sxs-lookup"><span data-stu-id="a628f-169">Location: You cannot select hello region, by default it is same as hello source server</span></span>

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

<span data-ttu-id="a628f-170">toorestore hello server a [obnovení tooa v daném okamžiku](./howto-restore-server-portal.md) před hello tabulka byla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="a628f-170">toorestore hello server and [restore tooa point-in-time](./howto-restore-server-portal.md) before hello table was deleted.</span></span> <span data-ttu-id="a628f-171">Obnovení do serveru tooa jiného bodu v čase vytvoří duplicitní nový server jako původní server hello hello bodu v čase, zadáte, za předpokladu, že je v rámci hello dobu uchování vašeho [vrstvy služby](./concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="a628f-171">Restoring a server tooa different point in time creates a duplicate new server as hello original server as of hello point in time you specify, provided that it is within hello retention period for your [service tier](./concepts-service-tiers.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a628f-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a628f-172">Next Steps</span></span>
<span data-ttu-id="a628f-173">V tomto kurzu jste se dozvěděli na:</span><span class="sxs-lookup"><span data-stu-id="a628f-173">In this tutorial you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="a628f-174">Vytvoření Azure databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="a628f-174">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="a628f-175">Konfigurace brány firewall serveru hello</span><span class="sxs-lookup"><span data-stu-id="a628f-175">Configure hello server firewall</span></span>
> * <span data-ttu-id="a628f-176">Použití [nástroj pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate databáze</span><span class="sxs-lookup"><span data-stu-id="a628f-176">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate a database</span></span>
> * <span data-ttu-id="a628f-177">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="a628f-177">Load sample data</span></span>
> * <span data-ttu-id="a628f-178">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="a628f-178">Query data</span></span>
> * <span data-ttu-id="a628f-179">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="a628f-179">Update data</span></span>
> * <span data-ttu-id="a628f-180">Obnovení dat</span><span class="sxs-lookup"><span data-stu-id="a628f-180">Restore data</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a628f-181">Azure databáze pro databázi MySQL – ukázky rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="a628f-181">Azure Database for MySQL - Azure CLI samples</span></span>](./sample-scripts-azure-cli.md)
