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
# <a name="design-your-first-azure-database-for-postgresql-using-azure-cli"></a><span data-ttu-id="433e2-103">Navrhnout první databáze Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="433e2-103">Design your first Azure Database for PostgreSQL using Azure CLI</span></span> 
<span data-ttu-id="433e2-104">V tomto kurzu použijete rozhraní příkazového řádku Azure (rozhraní příkazového řádku) a dalších nástrojů toolearn jak na:</span><span class="sxs-lookup"><span data-stu-id="433e2-104">In this tutorial, you use Azure CLI (command-line interface) and other utilities toolearn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="433e2-105">Vytvoření Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="433e2-105">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="433e2-106">Konfigurace brány firewall serveru hello</span><span class="sxs-lookup"><span data-stu-id="433e2-106">Configure hello server firewall</span></span>
> * <span data-ttu-id="433e2-107">Použití [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) nástroj toocreate databáze</span><span class="sxs-lookup"><span data-stu-id="433e2-107">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility toocreate a database</span></span>
> * <span data-ttu-id="433e2-108">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="433e2-108">Load sample data</span></span>
> * <span data-ttu-id="433e2-109">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="433e2-109">Query data</span></span>
> * <span data-ttu-id="433e2-110">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="433e2-110">Update data</span></span>
> * <span data-ttu-id="433e2-111">Obnovení dat</span><span class="sxs-lookup"><span data-stu-id="433e2-111">Restore data</span></span>

<span data-ttu-id="433e2-112">Můžete použít hello prostředí cloudu Azure v prohlížeči hello nebo [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli) na vlastní počítače toorun hello bloky kódu v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="433e2-112">You may use hello Azure Cloud Shell in hello browser, or [Install Azure CLI 2.0]( /cli/azure/install-azure-cli) on your own computer toorun hello code blocks in this tutorial.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="433e2-113">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="433e2-113">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="433e2-114">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="433e2-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="433e2-115">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="433e2-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="433e2-116">Pokud máte více předplatných, vyberte odpovídající předplatné hello, ve kterém hello prostředek neexistuje nebo se fakturuje pro.</span><span class="sxs-lookup"><span data-stu-id="433e2-116">If you have multiple subscriptions, choose hello appropriate subscription in which hello resource exists or is billed for.</span></span> <span data-ttu-id="433e2-117">Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#set) určité ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="433e2-117">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="433e2-118">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="433e2-118">Create a resource group</span></span>
<span data-ttu-id="433e2-119">Vytvoření [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="433e2-119">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="433e2-120">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.</span><span class="sxs-lookup"><span data-stu-id="433e2-120">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="433e2-121">Hello následující příklad vytvoří skupinu prostředků s názvem `myresourcegroup` v hello `westus` umístění.</span><span class="sxs-lookup"><span data-stu-id="433e2-121">hello following example creates a resource group named `myresourcegroup` in hello `westus` location.</span></span>
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="433e2-122">Vytvoření serveru Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="433e2-122">Create an Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="433e2-123">Vytvoření [Azure databázi PostgreSQL serveru](overview.md) pomocí hello [az postgres server vytvořit](/cli/azure/postgres/server#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="433e2-123">Create an [Azure Database for PostgreSQL server](overview.md) using hello [az postgres server create](/cli/azure/postgres/server#create) command.</span></span> <span data-ttu-id="433e2-124">Server obsahuje soubor databází spravovaných jako skupina.</span><span class="sxs-lookup"><span data-stu-id="433e2-124">A server contains a group of databases managed as a group.</span></span> 

<span data-ttu-id="433e2-125">Hello následující příklad vytvoří na serveru s názvem `mypgserver-20170401` ve vaší skupině prostředků `myresourcegroup` s přihlašovací jméno správce serveru `mylogin`.</span><span class="sxs-lookup"><span data-stu-id="433e2-125">hello following example creates a server called `mypgserver-20170401` in your resource group `myresourcegroup` with server admin login `mylogin`.</span></span> <span data-ttu-id="433e2-126">Název serveru, mapuje tooDNS název a je požadovaná toobe globálně jedinečné v Azure.</span><span class="sxs-lookup"><span data-stu-id="433e2-126">Name of a server maps tooDNS name and is thus required toobe globally unique in Azure.</span></span> <span data-ttu-id="433e2-127">SUBSTITUTE hello `<server_admin_password>` vlastní hodnotou.</span><span class="sxs-lookup"><span data-stu-id="433e2-127">Substitute hello `<server_admin_password>` with your own value.</span></span>
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401 --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> <span data-ttu-id="433e2-128">Hello přihlašovací jméno správce serveru a heslo, které tady zadáte jsou požadované toolog toohello serveru a její databáze dále v této úvodní.</span><span class="sxs-lookup"><span data-stu-id="433e2-128">hello server admin login and password that you specify here are required toolog in toohello server and its databases later in this quick start.</span></span> <span data-ttu-id="433e2-129">Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="433e2-129">Remember or record this information for later use.</span></span>

<span data-ttu-id="433e2-130">Ve výchozím nastavení se databáze **postgres** vytvoří v rámci vašeho serveru.</span><span class="sxs-lookup"><span data-stu-id="433e2-130">By default, **postgres** database gets created under your server.</span></span> <span data-ttu-id="433e2-131">Hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) databáze je výchozí databáze určené výhradně pro uživatele, nástroje a aplikace třetích stran.</span><span class="sxs-lookup"><span data-stu-id="433e2-131">hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is a default database meant for use by users, utilities, and third-party applications.</span></span> 


## <a name="configure-a-server-level-firewall-rule"></a><span data-ttu-id="433e2-132">Konfigurace pravidla brány firewall na úrovni serveru</span><span class="sxs-lookup"><span data-stu-id="433e2-132">Configure a server-level firewall rule</span></span>

<span data-ttu-id="433e2-133">Vytvoření pravidla brány firewall na úrovni serveru Azure PostgreSQL s hello [az postgres pravidla brány firewall-vytvořit](/cli/azure/postgres/server/firewall-rule#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="433e2-133">Create an Azure PostgreSQL server-level firewall rule with hello [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> <span data-ttu-id="433e2-134">Pravidlo brány firewall na úrovni serveru umožňuje externí aplikací, jako například [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) nebo [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server přes bránu firewall služby Azure PostgreSQL hello.</span><span class="sxs-lookup"><span data-stu-id="433e2-134">A server-level firewall rule allows an external application, such as [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) or [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server through hello Azure PostgreSQL service firewall.</span></span> 

<span data-ttu-id="433e2-135">Můžete nastavit pravidlo brány firewall, které pokrývá IP rozsah toobe tooconnect mít z vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="433e2-135">You can set a firewall rule that covers an IP range toobe able tooconnect from your network.</span></span> <span data-ttu-id="433e2-136">Hello následující příklad používá [az postgres pravidla brány firewall-vytvořit](/cli/azure/postgres/server/firewall-rule#create) toocreate pravidlo brány firewall `AllowAllIps` rozsah adres IP.</span><span class="sxs-lookup"><span data-stu-id="433e2-136">hello following example uses [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) toocreate a firewall rule `AllowAllIps` for an IP address range.</span></span> <span data-ttu-id="433e2-137">tooopen všechny IP adresy, použijte 0.0.0.0 jako hello počáteční IP adresa a 255.255.255.255 jako hello koncová adresa.</span><span class="sxs-lookup"><span data-stu-id="433e2-137">tooopen all IP addresses, use 0.0.0.0 as hello starting IP address and 255.255.255.255 as hello ending address.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="433e2-138">Server Azure PostgreSQL komunikuje přes port 5432.</span><span class="sxs-lookup"><span data-stu-id="433e2-138">Azure PostgreSQL server communicates over port 5432.</span></span> <span data-ttu-id="433e2-139">Pokud se připojujete z podnikové sítě, nemusí být odchozí provoz přes port 5432 bránou firewall vaší sítě povolený.</span><span class="sxs-lookup"><span data-stu-id="433e2-139">When connecting from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="433e2-140">Máte oddělení IT, otevřete port 5432 tooconnect tooyour databáze Azure SQL server.</span><span class="sxs-lookup"><span data-stu-id="433e2-140">Have your IT department open port 5432 tooconnect tooyour Azure SQL Database server.</span></span>
>

## <a name="get-hello-connection-information"></a><span data-ttu-id="433e2-141">Získat informace o připojení hello</span><span class="sxs-lookup"><span data-stu-id="433e2-141">Get hello connection information</span></span>

<span data-ttu-id="433e2-142">tooconnect tooyour serveru, musíte tooprovide informace a přístup k přihlašovacím údajům hostitele.</span><span class="sxs-lookup"><span data-stu-id="433e2-142">tooconnect tooyour server, you need tooprovide host information and access credentials.</span></span>
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

<span data-ttu-id="433e2-143">Výsledkem Hello je ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="433e2-143">hello result is in JSON format.</span></span> <span data-ttu-id="433e2-144">Poznamenejte si hello **administratorLogin** a **Plně_kvalifikovaný_název_domény**.</span><span class="sxs-lookup"><span data-stu-id="433e2-144">Make a note of hello **administratorLogin** and **fullyQualifiedDomainName**.</span></span>
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

## <a name="connect-tooazure-database-for-postgresql-database-using-psql"></a><span data-ttu-id="433e2-145">Připojit tooAzure databáze pro databázi PostgreSQL pomocí psql</span><span class="sxs-lookup"><span data-stu-id="433e2-145">Connect tooAzure Database for PostgreSQL database using psql</span></span>
<span data-ttu-id="433e2-146">Pokud má klientský počítač PostgreSQL nainstalován, můžete použít místní instanci [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), nebo hello Azure Cloud Console tooconnect tooan Azure PostgreSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="433e2-146">If your client computer has PostgreSQL installed, you can use a local instance of [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), or hello Azure Cloud Console tooconnect tooan Azure PostgreSQL server.</span></span> <span data-ttu-id="433e2-147">Použijeme hello psql nástroj příkazového řádku tooconnect toohello databáze Azure nyní pro PostgreSQL server.</span><span class="sxs-lookup"><span data-stu-id="433e2-147">Let's now use hello psql command-line utility tooconnect toohello Azure Database for PostgreSQL server.</span></span>

1. <span data-ttu-id="433e2-148">Spusťte následující psql příkaz tooconnect tooan databáze Azure pro PostgreSQL server hello</span><span class="sxs-lookup"><span data-stu-id="433e2-148">Run hello following psql command tooconnect tooan Azure Database for PostgreSQL server</span></span>
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  <span data-ttu-id="433e2-149">Například následující příkaz hello připojuje toohello výchozí databázi, která je volána **postgres** na vašem serveru PostgreSQL **mypgserver 20170401.postgres.database.azure.com** pomocí přihlašovacích údajů k přístupu.</span><span class="sxs-lookup"><span data-stu-id="433e2-149">For example, hello following command connects toohello default database called **postgres** on your PostgreSQL server **mypgserver-20170401.postgres.database.azure.com** using access credentials.</span></span> <span data-ttu-id="433e2-150">Zadejte hello `<server_admin_password>` jste zvolili při budete vyzváni k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="433e2-150">Enter hello `<server_admin_password>` you chose when prompted for password.</span></span>
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 ---dbname=postgres
```

2.  <span data-ttu-id="433e2-151">Až se server připojený toohello, vytvořte prázdnou databázi hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="433e2-151">Once you are connected toohello server, create a blank database at hello prompt.</span></span>
```sql
CREATE DATABASE mypgsqldb;
```

3.  <span data-ttu-id="433e2-152">Na příkazovém řádku hello spustit následující příkaz tooswitch připojení toohello nově vytvořený databáze hello **mypgsqldb**:</span><span class="sxs-lookup"><span data-stu-id="433e2-152">At hello prompt, execute hello following command tooswitch connection toohello newly created database **mypgsqldb**:</span></span>
```sql
\c mypgsqldb
```

## <a name="create-tables-in-hello-database"></a><span data-ttu-id="433e2-153">Vytváření tabulek v databázi hello</span><span class="sxs-lookup"><span data-stu-id="433e2-153">Create tables in hello database</span></span>
<span data-ttu-id="433e2-154">Teď, když víte, jak tooconnect toohello databáze Azure pro PostgreSQL, jsme můžete projít postupy toocomplete některé základní úlohy.</span><span class="sxs-lookup"><span data-stu-id="433e2-154">Now that you know how tooconnect toohello Azure Database for PostgreSQL, we can go over how toocomplete some basic tasks.</span></span>

<span data-ttu-id="433e2-155">Jsme nejprve vytvořit tabulku a načíst určitými daty.</span><span class="sxs-lookup"><span data-stu-id="433e2-155">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="433e2-156">Umožňuje vytvořit tabulku, která sleduje informace o inventáři.</span><span class="sxs-lookup"><span data-stu-id="433e2-156">Let's create a table that tracks inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

<span data-ttu-id="433e2-157">Můžete zjistit hello nově vytvořený tabulky v hello seznam tabulek teď zadáním:</span><span class="sxs-lookup"><span data-stu-id="433e2-157">You can see hello newly created table in hello list of tables now by typing:</span></span>
```sql
\dt
```

## <a name="load-data-into-hello-tables"></a><span data-ttu-id="433e2-158">Načtení dat do tabulky hello</span><span class="sxs-lookup"><span data-stu-id="433e2-158">Load data into hello tables</span></span>
<span data-ttu-id="433e2-159">Teď, když máme tabulku, jsme do něj vložte některá data.</span><span class="sxs-lookup"><span data-stu-id="433e2-159">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="433e2-160">V okně spusťte příkazový řádek text hello spusťte následující dotaz tooinsert hello některé řádky dat.</span><span class="sxs-lookup"><span data-stu-id="433e2-160">At hello open command prompt window, run hello following query tooinsert some rows of data</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="433e2-161">Máte nyní dva řádky ukázková data do hello tabulky, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="433e2-161">You have now two rows of sample data into hello table you created earlier.</span></span>

## <a name="query-and-update-hello-data-in-hello-tables"></a><span data-ttu-id="433e2-162">Dotazování a aktualizovat hello data v tabulkách hello</span><span class="sxs-lookup"><span data-stu-id="433e2-162">Query and update hello data in hello tables</span></span>
<span data-ttu-id="433e2-163">Spusťte následující dotaz tooretrieve informace z tabulky databáze hello hello.</span><span class="sxs-lookup"><span data-stu-id="433e2-163">Execute hello following query tooretrieve information from hello database table.</span></span> 
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="433e2-164">Můžete také aktualizovat hello data v tabulkách hello</span><span class="sxs-lookup"><span data-stu-id="433e2-164">You can also update hello data in hello tables</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="433e2-165">Při načítání dat, získá Hello řádek příslušným způsobem aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="433e2-165">hello row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a><span data-ttu-id="433e2-166">Obnovit do databáze tooa předchozího bodu v čase</span><span class="sxs-lookup"><span data-stu-id="433e2-166">Restore a database tooa previous point in time</span></span>
<span data-ttu-id="433e2-167">Představte si, že jste omylem odstranili tabulku.</span><span class="sxs-lookup"><span data-stu-id="433e2-167">Imagine you have accidentally deleted a table.</span></span> <span data-ttu-id="433e2-168">Toto je něco, které nelze snadno obnovit z.</span><span class="sxs-lookup"><span data-stu-id="433e2-168">This is something you cannot easily recover from.</span></span> <span data-ttu-id="433e2-169">Azure databázi PostgreSQL vám umožní toogo back tooany bodu v čase (v hello poslední too7 dní (Basic) a 35 dní (standardní)) a obnovit tento nový server tooa bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="433e2-169">Azure Database for PostgreSQL allows you toogo back tooany point-in-time (in hello last up too7 days (Basic) and 35 days (Standard)) and restore this point-in-time tooa new server.</span></span> <span data-ttu-id="433e2-170">Můžete použít tento nový server toorecover odstraněná data.</span><span class="sxs-lookup"><span data-stu-id="433e2-170">You can use this new server toorecover your deleted data.</span></span> <span data-ttu-id="433e2-171">Následující kroky obnovení hello ukázkový server tooa bod před přidáním tabulky hello Hello.</span><span class="sxs-lookup"><span data-stu-id="433e2-171">hello following steps restore hello sample server tooa point before hello table was added.</span></span>

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

<span data-ttu-id="433e2-172">Hello `az postgres server restore` příkaz potřebuje hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="433e2-172">hello `az postgres server restore` command needs hello following parameters:</span></span>
| <span data-ttu-id="433e2-173">Nastavení</span><span class="sxs-lookup"><span data-stu-id="433e2-173">Setting</span></span> | <span data-ttu-id="433e2-174">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="433e2-174">Suggested value</span></span> | <span data-ttu-id="433e2-175">Popis</span><span class="sxs-lookup"><span data-stu-id="433e2-175">Description</span></span>  |
| --- | --- | --- |
| <span data-ttu-id="433e2-176">--skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="433e2-176">--resource-group</span></span> |  <span data-ttu-id="433e2-177">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="433e2-177">myResourceGroup</span></span> |  <span data-ttu-id="433e2-178">Skupinu prostředků, ve které hello zdrojového serveru existuje.</span><span class="sxs-lookup"><span data-stu-id="433e2-178">The resource group in which hello source server exists.</span></span>  |
| <span data-ttu-id="433e2-179">– Název</span><span class="sxs-lookup"><span data-stu-id="433e2-179">--name</span></span> | <span data-ttu-id="433e2-180">Obnovit mypgserver</span><span class="sxs-lookup"><span data-stu-id="433e2-180">mypgserver-restored</span></span> | <span data-ttu-id="433e2-181">Název Hello hello nový server, který je vytvořen pomocí příkazu restore hello.</span><span class="sxs-lookup"><span data-stu-id="433e2-181">hello name of hello new server that is created by hello restore command.</span></span> |
| <span data-ttu-id="433e2-182">obnovení bodu v čase</span><span class="sxs-lookup"><span data-stu-id="433e2-182">restore-point-in-time</span></span> | <span data-ttu-id="433e2-183">2017-04-13T13:59:00Z</span><span class="sxs-lookup"><span data-stu-id="433e2-183">2017-04-13T13:59:00Z</span></span> | <span data-ttu-id="433e2-184">Vyberte bod v čase toorestore k.</span><span class="sxs-lookup"><span data-stu-id="433e2-184">Select a point-in-time toorestore to.</span></span> <span data-ttu-id="433e2-185">Toto datum a čas musí být v období uchovávání záloh hello zdrojového serveru.</span><span class="sxs-lookup"><span data-stu-id="433e2-185">This date and time must be within hello source server's backup retention period.</span></span> <span data-ttu-id="433e2-186">Použití datum a čas ve formátu ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="433e2-186">Use ISO8601 date and time format.</span></span> <span data-ttu-id="433e2-187">Například můžete použít vlastní místním časovém pásmu, jako například `2017-04-13T05:59:00-08:00`, nebo použijte formát času UTC zulština `2017-04-13T13:59:00Z`.</span><span class="sxs-lookup"><span data-stu-id="433e2-187">For example, you may use your own local timezone, such as `2017-04-13T05:59:00-08:00`, or use UTC Zulu format `2017-04-13T13:59:00Z`.</span></span> |
| <span data-ttu-id="433e2-188">--zdrojového serveru</span><span class="sxs-lookup"><span data-stu-id="433e2-188">--source-server</span></span> | <span data-ttu-id="433e2-189">mypgserver 20170401</span><span class="sxs-lookup"><span data-stu-id="433e2-189">mypgserver-20170401</span></span> | <span data-ttu-id="433e2-190">Hello název nebo ID hello zdrojový server toorestore z.</span><span class="sxs-lookup"><span data-stu-id="433e2-190">hello name or ID of hello source server toorestore from.</span></span> |

<span data-ttu-id="433e2-191">Obnovení serveru tooa v daném okamžiku se vytvoří nový server, kopírování jako původní server hello od hello bodu v čase, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="433e2-191">Restoring a server tooa point-in-time creates a new server, copying as hello original server as of hello point in time you specify.</span></span> <span data-ttu-id="433e2-192">Hello umístění a cenovou úroveň hodnoty pro server hello obnovit jsou hello stejný jako zdrojový server hello.</span><span class="sxs-lookup"><span data-stu-id="433e2-192">hello location and pricing tier values for hello restored server are hello same as hello source server.</span></span>

<span data-ttu-id="433e2-193">příkaz Hello je synchronní a vrátí po obnovení serveru hello.</span><span class="sxs-lookup"><span data-stu-id="433e2-193">hello command is synchronous, and will return after hello server is restored.</span></span> <span data-ttu-id="433e2-194">Po dokončení obnovení hello vyhledejte hello nový server, který byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="433e2-194">Once hello restore finishes, locate hello new server that was created.</span></span> <span data-ttu-id="433e2-195">Ověřte hello data byla obnovena podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="433e2-195">Verify hello data was restored as expected.</span></span>


## <a name="next-steps"></a><span data-ttu-id="433e2-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="433e2-196">Next steps</span></span>
<span data-ttu-id="433e2-197">V tomto kurzu jste se naučili jak toouse rozhraní příkazového řádku Azure (rozhraní příkazového řádku) a další nástroje na:</span><span class="sxs-lookup"><span data-stu-id="433e2-197">In this tutorial, you learned how toouse Azure CLI (command-line interface) and other utilities to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="433e2-198">Vytvoření Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="433e2-198">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="433e2-199">Konfigurace brány firewall serveru hello</span><span class="sxs-lookup"><span data-stu-id="433e2-199">Configure hello server firewall</span></span>
> * <span data-ttu-id="433e2-200">Použití [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) nástroj toocreate databáze</span><span class="sxs-lookup"><span data-stu-id="433e2-200">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility toocreate a database</span></span>
> * <span data-ttu-id="433e2-201">Načíst ukázková data</span><span class="sxs-lookup"><span data-stu-id="433e2-201">Load sample data</span></span>
> * <span data-ttu-id="433e2-202">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="433e2-202">Query data</span></span>
> * <span data-ttu-id="433e2-203">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="433e2-203">Update data</span></span>
> * <span data-ttu-id="433e2-204">Obnovení dat</span><span class="sxs-lookup"><span data-stu-id="433e2-204">Restore data</span></span>

<span data-ttu-id="433e2-205">V dalším kroku zjistěte, jak toouse hello podobných úloh s Azure toodo portálu, přečtěte si v tomto kurzu: [navrhnout první databáze Azure pro použití PostgreSQL hello portálu Azure](tutorial-design-database-using-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="433e2-205">Next, learn how toouse hello Azure portal toodo similar tasks, review this tutorial: [Design your first Azure Database for PostgreSQL using hello Azure portal](tutorial-design-database-using-azure-portal.md)</span></span>
