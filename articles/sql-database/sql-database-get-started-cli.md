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
# <a name="create-a-single-azure-sql-database-using-hello-azure-cli"></a><span data-ttu-id="26c22-104">Vytvoření jedné databáze Azure SQL pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="26c22-104">Create a single Azure SQL database using hello Azure CLI</span></span>

<span data-ttu-id="26c22-105">Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech.</span><span class="sxs-lookup"><span data-stu-id="26c22-105">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="26c22-106">Tato příručka podrobnosti o pomocí rozhraní příkazového řádku Azure toodeploy hello Azure SQL database v [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) v [logického serveru Azure SQL Database](sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="26c22-106">This guide details using hello Azure CLI toodeploy an Azure SQL database in an [Azure resource group](../azure-resource-manager/resource-group-overview.md) in an [Azure SQL Database logical server](sql-database-features.md).</span></span>

<span data-ttu-id="26c22-107">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="26c22-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="26c22-108">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, zda používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="26c22-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="26c22-109">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="26c22-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="26c22-110">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="26c22-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="define-variables"></a><span data-ttu-id="26c22-111">Definování proměnných</span><span class="sxs-lookup"><span data-stu-id="26c22-111">Define variables</span></span>

<span data-ttu-id="26c22-112">Definování proměnné pro použití ve skriptech hello tento rychlý start.</span><span class="sxs-lookup"><span data-stu-id="26c22-112">Define variables for use in hello scripts in this quick start.</span></span>

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

## <a name="create-a-resource-group"></a><span data-ttu-id="26c22-113">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="26c22-113">Create a resource group</span></span>

<span data-ttu-id="26c22-114">Vytvoření [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="26c22-114">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="26c22-115">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina.</span><span class="sxs-lookup"><span data-stu-id="26c22-115">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="26c22-116">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westeurope` umístění.</span><span class="sxs-lookup"><span data-stu-id="26c22-116">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` location.</span></span>

```azurecli-interactive
az group create --name $resourcegroupname --location $location
```
## <a name="create-a-logical-server"></a><span data-ttu-id="26c22-117">Vytvoření logického serveru</span><span class="sxs-lookup"><span data-stu-id="26c22-117">Create a logical server</span></span>

<span data-ttu-id="26c22-118">Vytvoření [logického serveru Azure SQL Database](sql-database-features.md) pomocí hello [az sql server vytvořit](/cli/azure/sql/server#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="26c22-118">Create an [Azure SQL Database logical server](sql-database-features.md) using hello [az sql server create](/cli/azure/sql/server#create) command.</span></span> <span data-ttu-id="26c22-119">Logický server obsahuje soubor databází spravovaných jako skupina.</span><span class="sxs-lookup"><span data-stu-id="26c22-119">A logical server contains a group of databases managed as a group.</span></span> <span data-ttu-id="26c22-120">Hello následující příklad vytvoří náhodným názvem serveru ve vaší skupině prostředků se k přihlášení správce s názvem `ServerAdmin` a heslo `ChangeYourAdminPassword1`.</span><span class="sxs-lookup"><span data-stu-id="26c22-120">hello following example creates a randomly named server in your resource group with an admin login named `ServerAdmin` and a password of `ChangeYourAdminPassword1`.</span></span> <span data-ttu-id="26c22-121">Podle potřeby tyto předdefinované hodnoty nahraďte.</span><span class="sxs-lookup"><span data-stu-id="26c22-121">Replace these pre-defined values as desired.</span></span>

```azurecli-interactive
az sql server create --name $servername --resource-group $resourcegroupname --location $location \
    --admin-user $adminlogin --admin-password $password
```

## <a name="configure-a-server-firewall-rule"></a><span data-ttu-id="26c22-122">Konfigurace pravidla brány firewall serveru</span><span class="sxs-lookup"><span data-stu-id="26c22-122">Configure a server firewall rule</span></span>

<span data-ttu-id="26c22-123">Vytvoření [pravidlo brány firewall na úrovni serveru Azure SQL Database](sql-database-firewall-configure.md) pomocí hello [vytvoření brány firewall serveru sql az](/cli/azure/sql/server/firewall-rule#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="26c22-123">Create an [Azure SQL Database server-level firewall rule](sql-database-firewall-configure.md) using hello [az sql server firewall create](/cli/azure/sql/server/firewall-rule#create) command.</span></span> <span data-ttu-id="26c22-124">Pravidlo brány firewall na úrovni serveru umožňuje externí aplikací, jako je například SQL Server Management Studio nebo hello SQLCMD nástroj tooconnect tooa SQL database přes bránu firewall služby SQL Database hello.</span><span class="sxs-lookup"><span data-stu-id="26c22-124">A server-level firewall rule allows an external application, such as SQL Server Management Studio or hello SQLCMD utility tooconnect tooa SQL database through hello SQL Database service firewall.</span></span> <span data-ttu-id="26c22-125">V následujícím příkladu hello je otevřené brány firewall hello pouze ostatní prostředky služby Azure.</span><span class="sxs-lookup"><span data-stu-id="26c22-125">In hello following example, hello firewall is only opened for other Azure resources.</span></span> <span data-ttu-id="26c22-126">externí připojení tooenable změnu hello IP adres tooan příslušnou adresu pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="26c22-126">tooenable external connectivity, change hello IP address tooan appropriate address for your environment.</span></span> <span data-ttu-id="26c22-127">tooopen všechny IP adresy, použijte 0.0.0.0 jako hello počáteční IP adresa a 255.255.255.255 jako hello koncová adresa.</span><span class="sxs-lookup"><span data-stu-id="26c22-127">tooopen all IP addresses, use 0.0.0.0 as hello starting IP address and 255.255.255.255 as hello ending address.</span></span>  

```azurecli-interactive
az sql server firewall-rule create --resource-group $resourcegroupname --server $servername \
    -n AllowYourIp --start-ip-address $startip --end-ip-address $endip
```

> [!NOTE]
> <span data-ttu-id="26c22-128">SQL Database komunikuje přes port 1433.</span><span class="sxs-lookup"><span data-stu-id="26c22-128">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="26c22-129">Pokud se pokoušíte tooconnect z podnikové sítě, odchozí provoz přes port 1433 nemusí mít povolený bránou firewall vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="26c22-129">If you are trying tooconnect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="26c22-130">Pokud ano, nebudou se moct tooconnect serveru Azure SQL Database tooyour, dokud vaše IT oddělení otevře port 1433.</span><span class="sxs-lookup"><span data-stu-id="26c22-130">If so, you will not be able tooconnect tooyour Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="create-a-database-in-hello-server-with-sample-data"></a><span data-ttu-id="26c22-131">Vytvořit databázi na serveru hello s ukázkovými daty</span><span class="sxs-lookup"><span data-stu-id="26c22-131">Create a database in hello server with sample data</span></span>

<span data-ttu-id="26c22-132">Vytvořit databázi s [úroveň výkonu S0](sql-database-service-tiers.md) v hello serveru pomocí hello [vytvořit databázi sql az](/cli/azure/sql/db#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="26c22-132">Create a database with an [S0 performance level](sql-database-service-tiers.md) in hello server using hello [az sql db create](/cli/azure/sql/db#create) command.</span></span> <span data-ttu-id="26c22-133">Hello následující příklad vytvoří databázi názvem `mySampleDatabase` a zatížením hello AdventureWorksLT ukázková data do této databáze.</span><span class="sxs-lookup"><span data-stu-id="26c22-133">hello following example creates a database called `mySampleDatabase` and loads hello AdventureWorksLT sample data into this database.</span></span> <span data-ttu-id="26c22-134">Nahraďte tyto předdefinované hodnoty podle potřeby (jiné rychlé spuštění v této kolekci sestavení při hello hodnoty v této úvodní).</span><span class="sxs-lookup"><span data-stu-id="26c22-134">Replace these predefined values as desired (other quick starts in this collection build upon hello values in this quick start).</span></span>

```azurecli-interactive
az sql db create --resource-group $resourcegroupname --server $servername \
    --name $databasename --sample-name AdventureWorksLT --service-objective S0
```

## <a name="clean-up-resources"></a><span data-ttu-id="26c22-135">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="26c22-135">Clean up resources</span></span>

<span data-ttu-id="26c22-136">Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu.</span><span class="sxs-lookup"><span data-stu-id="26c22-136">Other quick starts in this collection build upon this quick start.</span></span> 

> [!TIP]
> <span data-ttu-id="26c22-137">Pokud máte v plánu toocontinue na toowork s několik rychlé spuštění, vyčistěte hello prostředky vytvořené v této rychlé nespouštějte.</span><span class="sxs-lookup"><span data-stu-id="26c22-137">If you plan toocontinue on toowork with subsequent quick starts, do not clean up hello resources created in this quick start.</span></span> <span data-ttu-id="26c22-138">Pokud neplánujete toocontinue, použijte následující kroky toodelete všechny prostředky vytvořeny pomocí této úvodní v hello portál Azure hello.</span><span class="sxs-lookup"><span data-stu-id="26c22-138">If you do not plan toocontinue, use hello following steps toodelete all resources created by this quick start in hello Azure portal.</span></span>
>

```azurecli-interactive
az group delete --name $resourcegroupname
```

## <a name="next-steps"></a><span data-ttu-id="26c22-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="26c22-139">Next steps</span></span>

<span data-ttu-id="26c22-140">Teď, když máte databázi, můžete se k ní připojit a provádět dotazování pomocí vašich oblíbených nástrojů.</span><span class="sxs-lookup"><span data-stu-id="26c22-140">Now that you have a database, you can connect and query using your favorite tools.</span></span> <span data-ttu-id="26c22-141">Další informace získáte výběrem vašeho nástroje níže:</span><span class="sxs-lookup"><span data-stu-id="26c22-141">Learn more by choosing your tool below:</span></span>

- [<span data-ttu-id="26c22-142">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="26c22-142">SQL Server Management Studio</span></span>](sql-database-connect-query-ssms.md)
- [<span data-ttu-id="26c22-143">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="26c22-143">Visual Studio Code</span></span>](sql-database-connect-query-vscode.md)
- [<span data-ttu-id="26c22-144">.NET</span><span class="sxs-lookup"><span data-stu-id="26c22-144">.NET</span></span>](sql-database-connect-query-dotnet.md)
- [<span data-ttu-id="26c22-145">PHP</span><span class="sxs-lookup"><span data-stu-id="26c22-145">PHP</span></span>](sql-database-connect-query-php.md)
- [<span data-ttu-id="26c22-146">Node.js</span><span class="sxs-lookup"><span data-stu-id="26c22-146">Node.js</span></span>](sql-database-connect-query-nodejs.md)
- [<span data-ttu-id="26c22-147">Java</span><span class="sxs-lookup"><span data-stu-id="26c22-147">Java</span></span>](sql-database-connect-query-java.md)
- [<span data-ttu-id="26c22-148">Python</span><span class="sxs-lookup"><span data-stu-id="26c22-148">Python</span></span>](sql-database-connect-query-python.md)
- [<span data-ttu-id="26c22-149">Ruby</span><span class="sxs-lookup"><span data-stu-id="26c22-149">Ruby</span></span>](sql-database-connect-query-ruby.md)

