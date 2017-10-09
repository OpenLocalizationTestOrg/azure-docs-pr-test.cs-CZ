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
# <a name="create-an-azure-database-for-postgresql-using-hello-azure-cli"></a><span data-ttu-id="51acc-103">Vytvoření databáze Azure pro PostgreSQL pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="51acc-103">Create an Azure Database for PostgreSQL using hello Azure CLI</span></span>
<span data-ttu-id="51acc-104">Azure databázi PostgreSQL je spravovaná služba, která vám umožní toorun, spravovat a škálování vysoce dostupné databáze PostgreSQL v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="51acc-104">Azure Database for PostgreSQL is a managed service that enables you toorun, manage, and scale highly available PostgreSQL databases in hello cloud.</span></span> <span data-ttu-id="51acc-105">Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="51acc-105">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="51acc-106">Tento rychlý start se dozvíte, jak toocreate Azure databáze pro server PostgreSQL v [skupina prostředků Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="51acc-106">This quickstart shows you how toocreate an Azure Database for PostgreSQL server in an [Azure resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) using hello Azure CLI.</span></span>

<span data-ttu-id="51acc-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="51acc-107">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="51acc-108">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="51acc-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="51acc-109">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="51acc-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="51acc-110">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="51acc-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="51acc-111">Pokud máte více předplatných, zvolte hello příslušné předplatné, ve kterém bude účtován hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="51acc-111">If you have multiple subscriptions, choose hello appropriate subscription in which hello resource will be billed.</span></span> <span data-ttu-id="51acc-112">Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#set) určité ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="51acc-112">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="51acc-113">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="51acc-113">Create a resource group</span></span>

<span data-ttu-id="51acc-114">Vytvoření [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="51acc-114">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="51acc-115">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.</span><span class="sxs-lookup"><span data-stu-id="51acc-115">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="51acc-116">Hello následující příklad vytvoří skupinu prostředků s názvem `myresourcegroup` v hello `westus` umístění.</span><span class="sxs-lookup"><span data-stu-id="51acc-116">hello following example creates a resource group named `myresourcegroup` in hello `westus` location.</span></span>
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="51acc-117">Vytvoření serveru Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="51acc-117">Create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="51acc-118">Vytvoření [Azure databázi PostgreSQL serveru](overview.md) pomocí hello [az postgres server vytvořit](/cli/azure/postgres/server#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="51acc-118">Create an [Azure Database for PostgreSQL server](overview.md) using hello [az postgres server create](/cli/azure/postgres/server#create) command.</span></span> <span data-ttu-id="51acc-119">Server obsahuje soubor databází spravovaných jako skupina.</span><span class="sxs-lookup"><span data-stu-id="51acc-119">A server contains a group of databases managed as a group.</span></span> 

<span data-ttu-id="51acc-120">Hello následující příklad vytvoří server s názvem `mypgserver-20170401` ve vaší skupině prostředků `myresourcegroup` s přihlašovací jméno správce serveru `mylogin`.</span><span class="sxs-lookup"><span data-stu-id="51acc-120">hello following example creates a server named `mypgserver-20170401` in your resource group `myresourcegroup` with server admin login `mylogin`.</span></span> <span data-ttu-id="51acc-121">Mapuje název tooDNS Hello název serveru a je požadovaná toobe globálně jedinečné v Azure.</span><span class="sxs-lookup"><span data-stu-id="51acc-121">hello name of a server maps tooDNS name and is thus required toobe globally unique in Azure.</span></span> <span data-ttu-id="51acc-122">SUBSTITUTE hello `<server_admin_password>` vlastní hodnotou.</span><span class="sxs-lookup"><span data-stu-id="51acc-122">Substitute hello `<server_admin_password>` with your own value.</span></span>
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> <span data-ttu-id="51acc-123">Hello přihlašovací jméno správce serveru a heslo, které tady zadáte jsou požadované toolog toohello serveru a její databáze dále v této úvodní.</span><span class="sxs-lookup"><span data-stu-id="51acc-123">hello server admin login and password that you specify here are required toolog in toohello server and its databases later in this quick start.</span></span> <span data-ttu-id="51acc-124">Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="51acc-124">Remember or record this information for later use.</span></span>

<span data-ttu-id="51acc-125">Ve výchozím nastavení se databáze **postgres** vytvoří v rámci vašeho serveru.</span><span class="sxs-lookup"><span data-stu-id="51acc-125">By default, **postgres** database gets created under your server.</span></span> <span data-ttu-id="51acc-126">Hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) databáze je výchozí databáze určené výhradně pro uživatele, nástroje a aplikace třetích stran.</span><span class="sxs-lookup"><span data-stu-id="51acc-126">hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is a default database meant for use by users, utilities, and third-party applications.</span></span> 


## <a name="configure-a-server-level-firewall-rule"></a><span data-ttu-id="51acc-127">Konfigurace pravidla brány firewall na úrovni serveru</span><span class="sxs-lookup"><span data-stu-id="51acc-127">Configure a server-level firewall rule</span></span>

<span data-ttu-id="51acc-128">Vytvoření pravidla brány firewall na úrovni serveru Azure PostgreSQL s hello [az postgres pravidla brány firewall-vytvořit](/cli/azure/postgres/server/firewall-rule#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="51acc-128">Create an Azure PostgreSQL server-level firewall rule with hello [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> <span data-ttu-id="51acc-129">Pravidlo brány firewall na úrovni serveru umožňuje externí aplikací, jako například [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) nebo [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server přes bránu firewall služby Azure PostgreSQL hello.</span><span class="sxs-lookup"><span data-stu-id="51acc-129">A server-level firewall rule allows an external application, such as [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) or [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server through hello Azure PostgreSQL service firewall.</span></span> 

<span data-ttu-id="51acc-130">Můžete nastavit pravidlo brány firewall, které pokrývá IP rozsah toobe tooconnect mít z vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="51acc-130">You can set a firewall rule that covers an IP range toobe able tooconnect from your network.</span></span> <span data-ttu-id="51acc-131">Hello následující příklad používá [az postgres pravidla brány firewall-vytvořit](/cli/azure/postgres/server/firewall-rule#create) toocreate pravidlo brány firewall `AllowAllIps` rozsah adres IP.</span><span class="sxs-lookup"><span data-stu-id="51acc-131">hello following example uses [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) toocreate a firewall rule `AllowAllIps` for an IP address range.</span></span> <span data-ttu-id="51acc-132">tooopen všechny IP adresy, použijte 0.0.0.0 jako hello počáteční IP adresa a 255.255.255.255 jako hello koncová adresa.</span><span class="sxs-lookup"><span data-stu-id="51acc-132">tooopen all IP addresses, use 0.0.0.0 as hello starting IP address and 255.255.255.255 as hello ending address.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="51acc-133">Server Azure PostgreSQL komunikuje přes port 5432.</span><span class="sxs-lookup"><span data-stu-id="51acc-133">Azure PostgreSQL server communicates over port 5432.</span></span> <span data-ttu-id="51acc-134">Pokud se připojujete z podnikové sítě, nemusí být odchozí provoz přes port 5432 bránou firewall vaší sítě povolený.</span><span class="sxs-lookup"><span data-stu-id="51acc-134">When connecting from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="51acc-135">Máte oddělení IT, otevřete port 5432 tooconnect tooyour databáze Azure SQL server.</span><span class="sxs-lookup"><span data-stu-id="51acc-135">Have your IT department open port 5432 tooconnect tooyour Azure SQL Database server.</span></span>

## <a name="get-hello-connection-information"></a><span data-ttu-id="51acc-136">Získat informace o připojení hello</span><span class="sxs-lookup"><span data-stu-id="51acc-136">Get hello connection information</span></span>

<span data-ttu-id="51acc-137">tooconnect tooyour serveru, musíte tooprovide informace a přístup k přihlašovacím údajům hostitele.</span><span class="sxs-lookup"><span data-stu-id="51acc-137">tooconnect tooyour server, you need tooprovide host information and access credentials.</span></span>
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

<span data-ttu-id="51acc-138">Výsledkem Hello je ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="51acc-138">hello result is in JSON format.</span></span> <span data-ttu-id="51acc-139">Poznamenejte si hello **administratorLogin** a **Plně_kvalifikovaný_název_domény**.</span><span class="sxs-lookup"><span data-stu-id="51acc-139">Make a note of hello **administratorLogin** and **fullyQualifiedDomainName**.</span></span>
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

## <a name="connect-toopostgresql-database-using-psql"></a><span data-ttu-id="51acc-140">Připojit databáze tooPostgreSQL pomocí psql</span><span class="sxs-lookup"><span data-stu-id="51acc-140">Connect tooPostgreSQL database using psql</span></span>

<span data-ttu-id="51acc-141">Pokud má klientský počítač PostgreSQL nainstalován, můžete použít místní instanci [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooconnect tooan Azure PostgreSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="51acc-141">If your client computer has PostgreSQL installed, you can use a local instance of [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooconnect tooan Azure PostgreSQL server.</span></span> <span data-ttu-id="51acc-142">Teď umožňuje použít hello psql nástroj příkazového řádku tooconnect toohello Azure PostgreSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="51acc-142">Let's now use hello psql command-line utility tooconnect toohello Azure PostgreSQL server.</span></span>

1. <span data-ttu-id="51acc-143">Spusťte následující psql příkaz tooconnect tooan databáze Azure pro PostgreSQL server hello</span><span class="sxs-lookup"><span data-stu-id="51acc-143">Run hello following psql command tooconnect tooan Azure Database for PostgreSQL server</span></span>
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  <span data-ttu-id="51acc-144">Například následující příkaz hello připojuje toohello výchozí databázi, která je volána **postgres** na vašem serveru PostgreSQL **mypgserver 20170401.postgres.database.azure.com** pomocí přihlašovacích údajů k přístupu.</span><span class="sxs-lookup"><span data-stu-id="51acc-144">For example, hello following command connects toohello default database called **postgres** on your PostgreSQL server **mypgserver-20170401.postgres.database.azure.com** using access credentials.</span></span> <span data-ttu-id="51acc-145">Zadejte hello `<server_admin_password>` jste zvolili při budete vyzváni k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="51acc-145">Enter hello `<server_admin_password>` you chose when prompted for password.</span></span>
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  <span data-ttu-id="51acc-146">Až se server připojený toohello, vytvořte prázdnou databázi hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="51acc-146">Once you are connected toohello server, create a blank database at hello prompt.</span></span>
```sql
CREATE DATABASE mypgsqldb;
```

3.  <span data-ttu-id="51acc-147">Na příkazovém řádku hello spustit následující příkaz tooswitch připojení toohello nově vytvořený databáze hello **mypgsqldb**:</span><span class="sxs-lookup"><span data-stu-id="51acc-147">At hello prompt, execute hello following command tooswitch connection toohello newly created database **mypgsqldb**:</span></span>
```sql
\c mypgsqldb
```

## <a name="connect-toopostgresql-database-using-pgadmin"></a><span data-ttu-id="51acc-148">Připojit databáze tooPostgreSQL pomocí pgAdmin</span><span class="sxs-lookup"><span data-stu-id="51acc-148">Connect tooPostgreSQL database using pgAdmin</span></span>

<span data-ttu-id="51acc-149">tooconnect tooAzure PostgreSQL serveru pomocí hello grafického uživatelského rozhraní nástroje _pgAdmin_</span><span class="sxs-lookup"><span data-stu-id="51acc-149">tooconnect tooAzure PostgreSQL server using hello GUI tool _pgAdmin_</span></span>
1.  <span data-ttu-id="51acc-150">Spusťte hello _pgAdmin_ aplikace na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="51acc-150">Launch hello _pgAdmin_ application on your client computer.</span></span> <span data-ttu-id="51acc-151">_pgAdmin_ můžete nainstalovat ze stránky http://www.pgadmin.org/.</span><span class="sxs-lookup"><span data-stu-id="51acc-151">You can install _pgAdmin_ from http://www.pgadmin.org/.</span></span>
2.  <span data-ttu-id="51acc-152">Zvolte **přidejte nový Server** z hello **rychlé odkazy** nabídky.</span><span class="sxs-lookup"><span data-stu-id="51acc-152">Choose **Add New Server** from hello **Quick Links** menu.</span></span>
3.  <span data-ttu-id="51acc-153">V hello **vytvoření - serveru** dialogové okno **Obecné** zadejte jedinečný popisný název pro hello server.</span><span class="sxs-lookup"><span data-stu-id="51acc-153">In hello **Create - Server** dialog box **General** tab, enter a unique friendly Name for hello server.</span></span> <span data-ttu-id="51acc-154">Může to být třeba **Azure PostgreSQL Server**.</span><span class="sxs-lookup"><span data-stu-id="51acc-154">Say **Azure PostgreSQL Server**.</span></span>
 <span data-ttu-id="51acc-155">![Nástroj pgAdmin – Vytvořit – server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)</span><span class="sxs-lookup"><span data-stu-id="51acc-155">![pgAdmin tool - create - server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)</span></span>
4.  <span data-ttu-id="51acc-156">V hello **vytvoření - serveru** dialogové okno, **připojení** karty:</span><span class="sxs-lookup"><span data-stu-id="51acc-156">In hello **Create - Server** dialog box, **Connection** tab:</span></span>
    - <span data-ttu-id="51acc-157">Zadejte název serveru plně kvalifikovaný hello (například **mypgserver 20170401.postgres.database.azure.com**) v hello **název hostitele / adres** pole.</span><span class="sxs-lookup"><span data-stu-id="51acc-157">Enter hello fully qualified server name (for example, **mypgserver-20170401.postgres.database.azure.com**) in hello **Host Name/ Address** box.</span></span> 
    - <span data-ttu-id="51acc-158">Zadejte port 5432 do hello **Port** pole.</span><span class="sxs-lookup"><span data-stu-id="51acc-158">Enter port 5432 into hello **Port** box.</span></span> 
    - <span data-ttu-id="51acc-159">Zadejte hello **přihlašovací jméno správce serveru (user@mypgserver)** získali dříve v této rychlý start a heslo, které jste zadali při vytváření hello server do hello **uživatelské jméno** a **heslo** oknech v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="51acc-159">Enter hello **Server admin login (user@mypgserver)** obtained earlier in this quickstart and password you entered when you created hello server into hello **Username** and **Password** boxes, respectively.</span></span>
    - <span data-ttu-id="51acc-160">U položky **Režim SSL** vyberte možnost **Vyžadovat**.</span><span class="sxs-lookup"><span data-stu-id="51acc-160">Select **SSL Mode** as **Require**.</span></span> <span data-ttu-id="51acc-161">Ve výchozím nastavení jsou všechny servery Azure PostgreSQL vytvořeny se zapnutým vynucováním SSL.</span><span class="sxs-lookup"><span data-stu-id="51acc-161">By default, all Azure PostgreSQL servers are created with SSL enforcing turned ON.</span></span> <span data-ttu-id="51acc-162">tooturn vypnout vynucování SSL, najdete v podrobnostech v [vynucování SSL](./concepts-ssl-connection-security.md).</span><span class="sxs-lookup"><span data-stu-id="51acc-162">tooturn OFF SSL enforcing, see details in [Enforcing SSL](./concepts-ssl-connection-security.md).</span></span>

    ![pgAdmin – Vytvořit – server](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.  <span data-ttu-id="51acc-164">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="51acc-164">Click **Save**.</span></span>
6.  <span data-ttu-id="51acc-165">V levém podokně Prohlížeč hello rozbalte hello **skupin serverů**.</span><span class="sxs-lookup"><span data-stu-id="51acc-165">In hello Browser left pane, expand hello **Server Groups**.</span></span> <span data-ttu-id="51acc-166">Vyberte svůj server **Azure PostgreSQL Server**.</span><span class="sxs-lookup"><span data-stu-id="51acc-166">Choose your server **Azure PostgreSQL Server**.</span></span>
7.  <span data-ttu-id="51acc-167">Zvolte hello **Server** připojené k a potom vyberte **databáze** v něm.</span><span class="sxs-lookup"><span data-stu-id="51acc-167">Choose hello **Server** you connected to, and then choose **Databases** under it.</span></span> 
8.  <span data-ttu-id="51acc-168">Klikněte pravým tlačítkem na **databáze** tooCreate databáze.</span><span class="sxs-lookup"><span data-stu-id="51acc-168">Right-click on **Databases** tooCreate a Database.</span></span>
9.  <span data-ttu-id="51acc-169">Vyberte název databáze **mypgsqldb** a hello vlastník pro něj jako přihlašovací jméno správce serveru **mylogin**.</span><span class="sxs-lookup"><span data-stu-id="51acc-169">Choose a database name **mypgsqldb** and hello owner for it as server admin login **mylogin**.</span></span>
10. <span data-ttu-id="51acc-170">Klikněte na tlačítko **Uložit** toocreate prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="51acc-170">Click **Save** toocreate a blank database.</span></span>
11. <span data-ttu-id="51acc-171">V hello **prohlížeče**, rozbalte položku hello **servery** skupiny.</span><span class="sxs-lookup"><span data-stu-id="51acc-171">In hello **Browser**, expand hello **Servers** group.</span></span> <span data-ttu-id="51acc-172">Rozbalte server hello jste vytvořili a zobrazí hello databáze **mypgsqldb** v něm.</span><span class="sxs-lookup"><span data-stu-id="51acc-172">Expand hello server you created, and see hello database **mypgsqldb** under it.</span></span>
 <span data-ttu-id="51acc-173">![Nástroj pgAdmin – Vytvořit – databáze](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)</span><span class="sxs-lookup"><span data-stu-id="51acc-173">![pgAdmin - Create - Database](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)</span></span>


## <a name="clean-up-resources"></a><span data-ttu-id="51acc-174">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="51acc-174">Clean up resources</span></span>

<span data-ttu-id="51acc-175">Vyčištění všechny prostředky, které jste vytvořili v rychlý start hello odstraněním hello [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="51acc-175">Clean up all resources you created in hello quickstart by deleting hello [Azure resource group](../azure-resource-manager/resource-group-overview.md).</span></span>

> [!TIP]
> <span data-ttu-id="51acc-176">Další rychlé starty v této kolekci vycházejí z tohoto rychlého startu.</span><span class="sxs-lookup"><span data-stu-id="51acc-176">Other quickstarts in this collection build upon this quick start.</span></span> <span data-ttu-id="51acc-177">Pokud máte v plánu toocontinue na toowork s následné rychlých průvodců, proveďte není vyčistit hello prostředky vytvořené v tento rychlý start.</span><span class="sxs-lookup"><span data-stu-id="51acc-177">If you plan toocontinue on toowork with subsequent quickstarts, do not clean up hello resources created in this quickstart.</span></span> <span data-ttu-id="51acc-178">Pokud neplánujete toocontinue, použijte následující postup toodelete hello všechny prostředky, které jsou vytvořené tento rychlý start v hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="51acc-178">If you do not plan toocontinue, use hello following steps toodelete all resources created by this quickstart in hello Azure CLI.</span></span>

```azurecli-interactive
az group delete --name myresourcegroup
```

<span data-ttu-id="51acc-179">Pokud vás právě zajímají toodelete hello jeden nově vytvořený server, můžete spustit [odstranění serveru postgres az](/cli/azure/postgres/server#delete) příkaz.</span><span class="sxs-lookup"><span data-stu-id="51acc-179">If you just would like toodelete hello one newly created server, you can run [az postgres server delete](/cli/azure/postgres/server#delete) command.</span></span>
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="next-steps"></a><span data-ttu-id="51acc-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51acc-180">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="51acc-181">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="51acc-181">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
