---
title: "Vytvoření Azure Database for PostgreSQL pomocí rozhraní CLI Azure | Dokumentace Microsoftu"
description: "Úvodní příručka k vytvoření a správě serveru Azure Database for PostgreSQL pomocí rozhraní CLI Azure (rozhraní příkazového řádku)."
services: postgresql
author: sanagama
ms.author: sanagama
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 06/13/2017
ms.openlocfilehash: d78243abc140c7b3f0b99bdf56821b7920568550
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-azure-database-for-postgresql-using-the-azure-cli"></a><span data-ttu-id="b9c3b-103">Vytvoření Azure Database for PostgreSQL pomocí rozhraní CLI Azure</span><span class="sxs-lookup"><span data-stu-id="b9c3b-103">Create an Azure Database for PostgreSQL using the Azure CLI</span></span>
<span data-ttu-id="b9c3b-104">Azure Database for PostgreSQL je spravovaná služba, která umožňuje spouštět, spravovat a škálovat vysoce dostupné databáze PostgreSQL v cloudu.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-104">Azure Database for PostgreSQL is a managed service that enables you to run, manage, and scale highly available PostgreSQL databases in the cloud.</span></span> <span data-ttu-id="b9c3b-105">Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-105">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span> <span data-ttu-id="b9c3b-106">V tomto rychlém startu se dozvíte, jak vytvořit server Azure Database for PostgreSQL ve [skupině prostředků Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) pomocí rozhraní CLI Azure.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-106">This quickstart shows you how to create an Azure Database for PostgreSQL server in an [Azure resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) using the Azure CLI.</span></span>

<span data-ttu-id="b9c3b-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-107">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b9c3b-108">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="b9c3b-109">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="b9c3b-110">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b9c3b-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="b9c3b-111">Pokud máte více předplatných, vyberte odpovídající předplatné, ve kterém bude prostředek účtován.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-111">If you have multiple subscriptions, choose the appropriate subscription in which the resource will be billed.</span></span> <span data-ttu-id="b9c3b-112">Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#set) určité ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-112">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="b9c3b-113">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="b9c3b-113">Create a resource group</span></span>

<span data-ttu-id="b9c3b-114">Vytvořte [skupinu prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="b9c3b-114">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="b9c3b-115">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-115">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="b9c3b-116">Následující příklad vytvoří skupinu prostředků s názvem `myresourcegroup` v umístění `westus`.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-116">The following example creates a resource group named `myresourcegroup` in the `westus` location.</span></span>
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="b9c3b-117">Vytvoření serveru Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="b9c3b-117">Create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="b9c3b-118">Vytvořte [server Azure Database for PostgreSQL](overview.md) pomocí příkazu [az postgres server create](/cli/azure/postgres/server#create).</span><span class="sxs-lookup"><span data-stu-id="b9c3b-118">Create an [Azure Database for PostgreSQL server](overview.md) using the [az postgres server create](/cli/azure/postgres/server#create) command.</span></span> <span data-ttu-id="b9c3b-119">Server obsahuje soubor databází spravovaných jako skupina.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-119">A server contains a group of databases managed as a group.</span></span> 

<span data-ttu-id="b9c3b-120">Následující příklad vytvoří ve skupině prostředků `myresourcegroup` server s názvem `mypgserver-20170401` a přihlašovacím jménem správce serveru `mylogin`.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-120">The following example creates a server named `mypgserver-20170401` in your resource group `myresourcegroup` with server admin login `mylogin`.</span></span> <span data-ttu-id="b9c3b-121">Název serveru se mapuje na název DNS, a proto musí být v rámci Azure globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-121">The name of a server maps to DNS name and is thus required to be globally unique in Azure.</span></span> <span data-ttu-id="b9c3b-122">Nahraďte položku `<server_admin_password>` vlastní hodnotou.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-122">Substitute the `<server_admin_password>` with your own value.</span></span>
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> <span data-ttu-id="b9c3b-123">Zde zadané jméno správce serveru a heslo se vyžadují k přihlášení na server a jeho databáze dále v tomto rychlém startu.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-123">The server admin login and password that you specify here are required to log in to the server and its databases later in this quick start.</span></span> <span data-ttu-id="b9c3b-124">Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-124">Remember or record this information for later use.</span></span>

<span data-ttu-id="b9c3b-125">Ve výchozím nastavení se databáze **postgres** vytvoří v rámci vašeho serveru.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-125">By default, **postgres** database gets created under your server.</span></span> <span data-ttu-id="b9c3b-126">Databáze [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) je výchozí databáze určená pro uživatele, nástroje a aplikace třetích stran.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-126">The [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is a default database meant for use by users, utilities, and third-party applications.</span></span> 


## <a name="configure-a-server-level-firewall-rule"></a><span data-ttu-id="b9c3b-127">Konfigurace pravidla brány firewall na úrovni serveru</span><span class="sxs-lookup"><span data-stu-id="b9c3b-127">Configure a server-level firewall rule</span></span>

<span data-ttu-id="b9c3b-128">Pomocí příkazu [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) vytvořte pravidlo brány firewall na úrovni serveru Azure PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-128">Create an Azure PostgreSQL server-level firewall rule with the [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> <span data-ttu-id="b9c3b-129">Pravidlo brány firewall na úrovni serveru umožňuje externí aplikaci, jako je třeba [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) nebo [PgAdmin](https://www.pgadmin.org/), aby se k vašemu serveru připojila prostřednictvím brány firewall služby Azure PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-129">A server-level firewall rule allows an external application, such as [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) or [PgAdmin](https://www.pgadmin.org/) to connect to your server through the Azure PostgreSQL service firewall.</span></span> 

<span data-ttu-id="b9c3b-130">Pokud se chcete připojovat ze své sítě, můžete nastavit pravidlo brány firewall, které pokrývá rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-130">You can set a firewall rule that covers an IP range to be able to connect from your network.</span></span> <span data-ttu-id="b9c3b-131">Následující příklad vytváří pomocí příkazu [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) pravidlo brány firewall `AllowAllIps` pro rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-131">The following example uses [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) to create a firewall rule `AllowAllIps` for an IP address range.</span></span> <span data-ttu-id="b9c3b-132">Chcete-li otevřít všechny IP adresy, použijte jako počáteční IP adresu 0.0.0.0 a jako koncovou adresu 255.255.255.255.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-132">To open all IP addresses, use 0.0.0.0 as the starting IP address and 255.255.255.255 as the ending address.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="b9c3b-133">Server Azure PostgreSQL komunikuje přes port 5432.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-133">Azure PostgreSQL server communicates over port 5432.</span></span> <span data-ttu-id="b9c3b-134">Pokud se připojujete z podnikové sítě, nemusí být odchozí provoz přes port 5432 bránou firewall vaší sítě povolený.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-134">When connecting from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="b9c3b-135">Pořádejte oddělení IT, aby otevřeli port 5432 pro připojení k vašemu serveru Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-135">Have your IT department open port 5432 to connect to your Azure SQL Database server.</span></span>

## <a name="get-the-connection-information"></a><span data-ttu-id="b9c3b-136">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="b9c3b-136">Get the connection information</span></span>

<span data-ttu-id="b9c3b-137">Pokud se chcete připojit k serveru, budete muset zadat informace o hostiteli a přihlašovací údaje pro přístup.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-137">To connect to your server, you need to provide host information and access credentials.</span></span>
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

<span data-ttu-id="b9c3b-138">Výsledek je ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-138">The result is in JSON format.</span></span> <span data-ttu-id="b9c3b-139">Poznamenejte si položky **administratorLogin** a **fullyQualifiedDomainName**.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-139">Make a note of the **administratorLogin** and **fullyQualifiedDomainName**.</span></span>
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

## <a name="connect-to-postgresql-database-using-psql"></a><span data-ttu-id="b9c3b-140">Připojení k databázi PostgreSQL pomocí nástroje psql</span><span class="sxs-lookup"><span data-stu-id="b9c3b-140">Connect to PostgreSQL database using psql</span></span>

<span data-ttu-id="b9c3b-141">Pokud má klientský počítač nainstalovaný systém PostgreSQL, můžete se připojit k serveru Azure PostgreSQL pomocí místní instance nástroje [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html).</span><span class="sxs-lookup"><span data-stu-id="b9c3b-141">If your client computer has PostgreSQL installed, you can use a local instance of [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) to connect to an Azure PostgreSQL server.</span></span> <span data-ttu-id="b9c3b-142">Teď se pomocí nástroje příkazového řádku psql připojíme k serveru Azure PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-142">Let's now use the psql command-line utility to connect to the Azure PostgreSQL server.</span></span>

1. <span data-ttu-id="b9c3b-143">Spusťte následující příkaz psql pro připojení k serveru Azure Database for PostgreSQL:</span><span class="sxs-lookup"><span data-stu-id="b9c3b-143">Run the following psql command to connect to an Azure Database for PostgreSQL server</span></span>
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  <span data-ttu-id="b9c3b-144">Třeba tento příkaz provádí pomocí přihlašovacích údajů pro přístup připojení k výchozí databázi s názvem **postgres** na serveru PostgreSQL **mypgserver-20170401.postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-144">For example, the following command connects to the default database called **postgres** on your PostgreSQL server **mypgserver-20170401.postgres.database.azure.com** using access credentials.</span></span> <span data-ttu-id="b9c3b-145">Zadejte heslo `<server_admin_password>`, které jste zvolili při zobrazení výzvy k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-145">Enter the `<server_admin_password>` you chose when prompted for password.</span></span>
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  <span data-ttu-id="b9c3b-146">Po připojení k serveru vytvořte na příkazovém řádku prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-146">Once you are connected to the server, create a blank database at the prompt.</span></span>
```sql
CREATE DATABASE mypgsqldb;
```

3.  <span data-ttu-id="b9c3b-147">Na příkazovém řádku spusťte následující příkaz, který přepne připojení na nově vytvořenou databázi **mypgsqldb**:</span><span class="sxs-lookup"><span data-stu-id="b9c3b-147">At the prompt, execute the following command to switch connection to the newly created database **mypgsqldb**:</span></span>
```sql
\c mypgsqldb
```

## <a name="connect-to-postgresql-database-using-pgadmin"></a><span data-ttu-id="b9c3b-148">Připojení k databázi PostgreSQL pomocí aplikace pgAdmin</span><span class="sxs-lookup"><span data-stu-id="b9c3b-148">Connect to PostgreSQL database using pgAdmin</span></span>

<span data-ttu-id="b9c3b-149">Připojení k serveru Azure PostgreSQL pomocí grafického uživatelského rozhraní aplikace _pgAdmin_</span><span class="sxs-lookup"><span data-stu-id="b9c3b-149">To connect to Azure PostgreSQL server using the GUI tool _pgAdmin_</span></span>
1.  <span data-ttu-id="b9c3b-150">Na klientském počítači spusťte aplikaci _pgAdmin_.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-150">Launch the _pgAdmin_ application on your client computer.</span></span> <span data-ttu-id="b9c3b-151">_pgAdmin_ můžete nainstalovat ze stránky http://www.pgadmin.org/.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-151">You can install _pgAdmin_ from http://www.pgadmin.org/.</span></span>
2.  <span data-ttu-id="b9c3b-152">V nabídce **Rychlé odkazy** zvolte **Přidat nový server**.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-152">Choose **Add New Server** from the **Quick Links** menu.</span></span>
3.  <span data-ttu-id="b9c3b-153">V dialogovém okně **Vytvořit – server** na kartě **Obecné** zadejte jedinečný popisný název serveru.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-153">In the **Create - Server** dialog box **General** tab, enter a unique friendly Name for the server.</span></span> <span data-ttu-id="b9c3b-154">Může to být třeba **Azure PostgreSQL Server**.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-154">Say **Azure PostgreSQL Server**.</span></span>
 <span data-ttu-id="b9c3b-155">![Nástroj pgAdmin – Vytvořit – server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)</span><span class="sxs-lookup"><span data-stu-id="b9c3b-155">![pgAdmin tool - create - server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)</span></span>
4.  <span data-ttu-id="b9c3b-156">V dialogovém okně **Vytvořit – server** přejděte na kartu **Připojení**:</span><span class="sxs-lookup"><span data-stu-id="b9c3b-156">In the **Create - Server** dialog box, **Connection** tab:</span></span>
    - <span data-ttu-id="b9c3b-157">Do pole **Název/adresa hostitele** zadejte plně kvalifikovaný název serveru (třeba **mypgserver-20170401.postgres.database.azure.com**).</span><span class="sxs-lookup"><span data-stu-id="b9c3b-157">Enter the fully qualified server name (for example, **mypgserver-20170401.postgres.database.azure.com**) in the **Host Name/ Address** box.</span></span> 
    - <span data-ttu-id="b9c3b-158">Do pole **Port** zadejte port 5432.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-158">Enter port 5432 into the **Port** box.</span></span> 
    - <span data-ttu-id="b9c3b-159">Zadejte **přihlášení správce serveru (user@mypgserver)**, které jste získali dříve v tomto rychlém startu, a heslo, které jste zadali při vytváření serveru, do polí **Uživatelské jméno** a **Heslo**.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-159">Enter the **Server admin login (user@mypgserver)** obtained earlier in this quickstart and password you entered when you created the server into the **Username** and **Password** boxes, respectively.</span></span>
    - <span data-ttu-id="b9c3b-160">U položky **Režim SSL** vyberte možnost **Vyžadovat**.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-160">Select **SSL Mode** as **Require**.</span></span> <span data-ttu-id="b9c3b-161">Ve výchozím nastavení se všechny servery Azure PostgreSQL vytvářejí se zapnutým vynucováním SSL.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-161">By default, all Azure PostgreSQL servers are created with SSL enforcing turned ON.</span></span> <span data-ttu-id="b9c3b-162">Pokud chcete vynucování SSL vypnout, přečtěte si podrobnosti v tématu [Vynucování SSL](./concepts-ssl-connection-security.md).</span><span class="sxs-lookup"><span data-stu-id="b9c3b-162">To turn OFF SSL enforcing, see details in [Enforcing SSL](./concepts-ssl-connection-security.md).</span></span>

    ![pgAdmin – Vytvořit – server](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.  <span data-ttu-id="b9c3b-164">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-164">Click **Save**.</span></span>
6.  <span data-ttu-id="b9c3b-165">V levém podokně prohlížeče rozbalte **Skupiny serverů**.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-165">In the Browser left pane, expand the **Server Groups**.</span></span> <span data-ttu-id="b9c3b-166">Vyberte svůj server **Azure PostgreSQL Server**.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-166">Choose your server **Azure PostgreSQL Server**.</span></span>
7.  <span data-ttu-id="b9c3b-167">Vyberte **Server**, ke kterému jste se připojili, a potom vyberte jeho **Databáze**.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-167">Choose the **Server** you connected to, and then choose **Databases** under it.</span></span> 
8.  <span data-ttu-id="b9c3b-168">Pravým tlačítkem myši klikněte na **Databáze** a vytvořte databázi.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-168">Right-click on **Databases** to Create a Database.</span></span>
9.  <span data-ttu-id="b9c3b-169">Vyberte název databáze **mypgsqldb** a jako vlastníka zadejte přihlašovací jméno správce serveru **mylogin**.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-169">Choose a database name **mypgsqldb** and the owner for it as server admin login **mylogin**.</span></span>
10. <span data-ttu-id="b9c3b-170">Kliknutím na **Uložit** vytvořte prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-170">Click **Save** to create a blank database.</span></span>
11. <span data-ttu-id="b9c3b-171">V okně **Prohlížeč** rozbalte skupinu **Server**.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-171">In the **Browser**, expand the **Servers** group.</span></span> <span data-ttu-id="b9c3b-172">Rozbalte server, který jste vytvořili. Uvidíte pod ním databázi **mypgsqldb**.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-172">Expand the server you created, and see the database **mypgsqldb** under it.</span></span>
 <span data-ttu-id="b9c3b-173">![Nástroj pgAdmin – Vytvořit – databáze](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)</span><span class="sxs-lookup"><span data-stu-id="b9c3b-173">![pgAdmin - Create - Database](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)</span></span>


## <a name="clean-up-resources"></a><span data-ttu-id="b9c3b-174">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="b9c3b-174">Clean up resources</span></span>

<span data-ttu-id="b9c3b-175">Všechny prostředky, které jste v rychlém startu vytvořili, můžete vyčistit odstraněním [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b9c3b-175">Clean up all resources you created in the quickstart by deleting the [Azure resource group](../azure-resource-manager/resource-group-overview.md).</span></span>

> [!TIP]
> <span data-ttu-id="b9c3b-176">Další rychlé starty v této kolekci vycházejí z tohoto rychlého startu.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-176">Other quickstarts in this collection build upon this quick start.</span></span> <span data-ttu-id="b9c3b-177">Pokud chcete pokračovat v práci s dalšími rychlými starty, neprovádějte čištění prostředků vytvořených v rámci tohoto rychlého startu.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-177">If you plan to continue on to work with subsequent quickstarts, do not clean up the resources created in this quickstart.</span></span> <span data-ttu-id="b9c3b-178">Pokud pokračovat nechcete, pomocí následujících kroků odstraňte všechny prostředky, které jste v tomto rychlém startu vytvořili na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b9c3b-178">If you do not plan to continue, use the following steps to delete all resources created by this quickstart in the Azure CLI.</span></span>

```azurecli-interactive
az group delete --name myresourcegroup
```

<span data-ttu-id="b9c3b-179">Pokud chcete jenom odstranit nově vytvořený server, můžete spustit příkaz [az postgres server delete](/cli/azure/postgres/server#delete).</span><span class="sxs-lookup"><span data-stu-id="b9c3b-179">If you just would like to delete the one newly created server, you can run [az postgres server delete](/cli/azure/postgres/server#delete) command.</span></span>
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="next-steps"></a><span data-ttu-id="b9c3b-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b9c3b-180">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b9c3b-181">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="b9c3b-181">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
