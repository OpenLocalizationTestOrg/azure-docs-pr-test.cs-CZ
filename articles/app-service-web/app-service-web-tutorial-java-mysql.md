---
title: "Vytvořit webovou aplikaci Java a MySQL v Azure"
description: "Zjistěte, jak získat aplikaci Java, která se připojuje ke službě databáze Azure MySQL práce v Azure App Service."
services: app-service\web
documentationcenter: Java
author: bbenz
manager: jeffsand
editor: jasonwhowell
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: bbenz
ms.custom: mvc
ms.openlocfilehash: eb2d59939c4e4486bb14bb143a4a18f9bc1478e1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="build-a-java-and-mysql-web-app-in-azure"></a><span data-ttu-id="6076e-103">Vytvořit webovou aplikaci Java a MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="6076e-103">Build a Java and MySQL web app in Azure</span></span>

<span data-ttu-id="6076e-104">Tento kurz ukazuje, jak vytvořit webovou aplikaci Java v Azure a připojte ho k databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="6076e-104">This tutorial shows you how to create a Java web app in Azure and connect it to a MySQL database.</span></span> <span data-ttu-id="6076e-105">Jakmile budete hotovi, budete mít [pružiny spouštěcí](https://projects.spring.io/spring-boot/) ukládání dat v aplikaci [Azure Database pro databázi MySQL](https://docs.microsoft.com/azure/mysql/overview) systémem [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview).</span><span class="sxs-lookup"><span data-stu-id="6076e-105">When you are finished, you will have a [Spring Boot](https://projects.spring.io/spring-boot/) application storing data in [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/overview) running on [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview).</span></span>

![Java aplikace spuštěné v Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

<span data-ttu-id="6076e-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="6076e-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6076e-108">Vytvoření databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="6076e-108">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="6076e-109">Ukázkovou aplikaci připojit k databázi</span><span class="sxs-lookup"><span data-stu-id="6076e-109">Connect a sample app to the database</span></span>
> * <span data-ttu-id="6076e-110">Nasazení aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="6076e-110">Deploy the app to Azure</span></span>
> * <span data-ttu-id="6076e-111">Aktualizace a opětovné nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="6076e-111">Update and redeploy the app</span></span>
> * <span data-ttu-id="6076e-112">Diagnostické protokoly datového proudu z Azure</span><span class="sxs-lookup"><span data-stu-id="6076e-112">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="6076e-113">Sledování aplikace na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6076e-113">Monitor the app in the Azure portal</span></span>


## <a name="prerequisites"></a><span data-ttu-id="6076e-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6076e-114">Prerequisites</span></span>

1. [<span data-ttu-id="6076e-115">Stáhněte a nainstalujte Git</span><span class="sxs-lookup"><span data-stu-id="6076e-115">Download and install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="6076e-116">Stáhněte a nainstalujte sadu JDK Java 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="6076e-116">Download and install the Java 7 JDK or above</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [<span data-ttu-id="6076e-117">Stáhnout, nainstalovat a spustit MySQL</span><span class="sxs-lookup"><span data-stu-id="6076e-117">Download, install, and start MySQL</span></span>](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="6076e-118">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6076e-118">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="6076e-119">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="6076e-119">Run `az --version` to find the version.</span></span> <span data-ttu-id="6076e-120">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6076e-120">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prepare-local-mysql"></a><span data-ttu-id="6076e-121">Příprava místního MySQL</span><span class="sxs-lookup"><span data-stu-id="6076e-121">Prepare local MySQL</span></span> 

<span data-ttu-id="6076e-122">V tomto kroku vytvoříte databázi v místním serveru MySQL pro použití při testování aplikace místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6076e-122">In this step, you create a database in a local MySQL server for use in testing the app locally on your machine.</span></span>

### <a name="connect-to-mysql-server"></a><span data-ttu-id="6076e-123">Připojení k serveru databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="6076e-123">Connect to MySQL server</span></span>

<span data-ttu-id="6076e-124">Okno terminálu připojte k místní server MySQL.</span><span class="sxs-lookup"><span data-stu-id="6076e-124">In a terminal window, connect to your local MySQL server.</span></span> <span data-ttu-id="6076e-125">Chcete-li spustit všechny příkazy v tomto kurzu můžete toto okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="6076e-125">You can use this terminal window to run all the commands in this tutorial.</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="6076e-126">Pokud se zobrazí výzva k zadání hesla, zadejte heslo pro `root` účtu.</span><span class="sxs-lookup"><span data-stu-id="6076e-126">If you're prompted for a password, enter the password for the `root` account.</span></span> <span data-ttu-id="6076e-127">Pokud si nepamatujete heslo kořenového účtu, najdete v části [MySQL: jak resetovat hesla kořenového](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span><span class="sxs-lookup"><span data-stu-id="6076e-127">If you don't remember your root account password, see [MySQL: How to Reset the Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span></span>

<span data-ttu-id="6076e-128">Pokud váš příkaz úspěšně proběhne, serveru databáze MySQL již spuštěna.</span><span class="sxs-lookup"><span data-stu-id="6076e-128">If your command runs successfully, then your MySQL server is already running.</span></span> <span data-ttu-id="6076e-129">Pokud ne, ujistěte se, zda je místní server MySQL spuštěná pomocí následujících [kroky po instalaci MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span><span class="sxs-lookup"><span data-stu-id="6076e-129">If not, make sure that your local MySQL server is started by following the [MySQL post-installation steps](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span></span>

### <a name="create-a-database"></a><span data-ttu-id="6076e-130">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="6076e-130">Create a database</span></span> 

<span data-ttu-id="6076e-131">V `mysql` výzvu, vytvořte databázi a tabulku položkami seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="6076e-131">In the `mysql` prompt, create a database and a table for the to-do items.</span></span>

```sql
CREATE DATABASE tododb;
```

<span data-ttu-id="6076e-132">Ukončení připojení k serveru zadáním `quit`.</span><span class="sxs-lookup"><span data-stu-id="6076e-132">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="create-and-run-the-sample-app"></a><span data-ttu-id="6076e-133">Vytvoření a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="6076e-133">Create and run the sample app</span></span> 

<span data-ttu-id="6076e-134">V tomto kroku klonovat ukázkové pružiny spouštěcí aplikace, bude sloužit místní databázi MySQL a spustit ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6076e-134">In this step, you clone sample Spring boot app, configure it to use the local MySQL database, and run it on your computer.</span></span> 

### <a name="clone-the-sample"></a><span data-ttu-id="6076e-135">Clone – ukázka</span><span class="sxs-lookup"><span data-stu-id="6076e-135">Clone the sample</span></span>

<span data-ttu-id="6076e-136">V okně terminálu přejděte do pracovního adresáře a klonovat úložiště v ukázkové.</span><span class="sxs-lookup"><span data-stu-id="6076e-136">In the terminal window, navigate to a working directory and clone the sample repository.</span></span> 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-the-app-to-use-the-mysql-database"></a><span data-ttu-id="6076e-137">Nakonfiguruje aplikaci, kterou chcete použít databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="6076e-137">Configure the app to use the MySQL database</span></span>

<span data-ttu-id="6076e-138">Aktualizace `spring.datasource.password` a hodnota v *spring-boot-mysql-todo/src/main/resources/application.properties* pomocí stejného hesla kořenové použitý k otevření MySQL řádku:</span><span class="sxs-lookup"><span data-stu-id="6076e-138">Update the `spring.datasource.password` and  value in *spring-boot-mysql-todo/src/main/resources/application.properties* with the same root password used to open the MySQL prompt:</span></span>

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-the-sample"></a><span data-ttu-id="6076e-139">Sestavit a spustit ukázku</span><span class="sxs-lookup"><span data-stu-id="6076e-139">Build and run the sample</span></span>

<span data-ttu-id="6076e-140">Sestavit a spustit ukázku pomocí obálku Maven zahrnuté v úložišti:</span><span class="sxs-lookup"><span data-stu-id="6076e-140">Build and run the sample using the Maven wrapper included in the repo:</span></span>

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

<span data-ttu-id="6076e-141">Otevřete prohlížeč na adrese http://localhost: 8080 zobrazíte v ukázce v akci.</span><span class="sxs-lookup"><span data-stu-id="6076e-141">Open your browser to http://localhost:8080 to see in the sample in action.</span></span> <span data-ttu-id="6076e-142">Při přidávání úkolů do seznamu, použijte následující příkazy SQL v řádku MySQL k zobrazení dat uložených v MySQL.</span><span class="sxs-lookup"><span data-stu-id="6076e-142">As you add tasks to the list,  use the following SQL commands in the MySQL prompt to view the data stored in MySQL.</span></span>

```SQL
use testdb;
select * from todo_item;
```

<span data-ttu-id="6076e-143">Zastavte aplikaci zasažení `Ctrl` + `C` v terminálu.</span><span class="sxs-lookup"><span data-stu-id="6076e-143">Stop the application by hitting `Ctrl`+`C` in the terminal.</span></span> 

## <a name="create-an-azure-mysql-database"></a><span data-ttu-id="6076e-144">Vytvoření databáze MySQL na Azure</span><span class="sxs-lookup"><span data-stu-id="6076e-144">Create an Azure MySQL database</span></span>

<span data-ttu-id="6076e-145">V tomto kroku vytvoříte [Azure Database pro databázi MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) pomocí [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6076e-145">In this step, you create an [Azure Database for MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) instance using the [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="6076e-146">Nakonfigurujete ukázkovou aplikaci pro tuto databázi použít později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6076e-146">You configure the sample application to use this database later on in the tutorial.</span></span>

<span data-ttu-id="6076e-147">Pomocí Azure CLI 2.0 na okno terminálu vytvořit prostředky potřebné k hostování aplikace v jazyce Java v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="6076e-147">Use the Azure CLI 2.0 in a terminal window to create the resources needed to host your Java application in Azure appservice.</span></span> <span data-ttu-id="6076e-148">Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="6076e-148">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span> 

```azurecli-interactive 
az login 
```   

### <a name="create-a-resource-group"></a><span data-ttu-id="6076e-149">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6076e-149">Create a resource group</span></span>

<span data-ttu-id="6076e-150">Vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) s [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="6076e-150">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="6076e-151">Skupinu prostředků Azure je logický kontejner, kde jsou související prostředky jako webové aplikace, databáze a účtů úložiště nasadit a spravovat.</span><span class="sxs-lookup"><span data-stu-id="6076e-151">An Azure resource group is a logical container where related resources like web apps, databases, and storage accounts are deployed and managed.</span></span> 

<span data-ttu-id="6076e-152">Následující příklad vytvoří skupinu prostředků v oblasti Severní Evropa:</span><span class="sxs-lookup"><span data-stu-id="6076e-152">The following example creates a resource group in the North Europe region:</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

<span data-ttu-id="6076e-153">Zobrazíte možné hodnoty, které můžete použít pro `--location`, použijte [míst seznamu služby App Service az](/cli/azure/appservice#list-locations) příkaz.</span><span class="sxs-lookup"><span data-stu-id="6076e-153">To see the possible values you can use for `--location`, use the [az appservice list-locations](/cli/azure/appservice#list-locations) command.</span></span>

### <a name="create-a-mysql-server"></a><span data-ttu-id="6076e-154">Vytvoření databáze MySQL serveru</span><span class="sxs-lookup"><span data-stu-id="6076e-154">Create a MySQL server</span></span>

<span data-ttu-id="6076e-155">Vytvoření serveru ve službě Azure Database pro databázi MySQL (Preview) pomocí [az mysql server vytvořit](/cli/azure/mysql/server#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="6076e-155">Create a server in Azure Database for MySQL (Preview) with the [az mysql server create](/cli/azure/mysql/server#create) command.</span></span>    
<span data-ttu-id="6076e-156">Nahraďte vlastní jedinečný název serveru databáze MySQL, kde uvidíte `<mysql_server_name>` zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="6076e-156">Substitute your own unique MySQL server name where you see the `<mysql_server_name>` placeholder.</span></span> <span data-ttu-id="6076e-157">Tento název je součástí název hostitele serveru MySQL, `<mysql_server_name>.mysql.database.azure.com`, takže ho musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="6076e-157">This name is part of your MySQL server's hostname, `<mysql_server_name>.mysql.database.azure.com`, so it needs to be globally unique.</span></span> <span data-ttu-id="6076e-158">Také nahraďte `<admin_user>` a `<admin_password>` vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="6076e-158">Also substitute `<admin_user>` and `<admin_password>` with your own values.</span></span>

```azurecli-interactive
az mysql server create --name <mysql_server_name> \ 
    --resource-group myResourceGroup \ 
    --location "North Europe" \
    --admin-user <admin_user> \ 
    --admin-password <admin_password>
```

<span data-ttu-id="6076e-159">Při vytvoření serveru MySQL rozhraní příkazového řádku Azure obsahuje informace o podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="6076e-159">When the MySQL server is created, the Azure CLI shows information similar to the following example:</span></span>

```json
{
  "administratorLogin": "admin_user",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mysql_server_name.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysql_server_name",
  "location": "northeurope",
  "name": "mysql_server_name",
  "resourceGroup": "mysqlJavaResourceGroup",
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a><span data-ttu-id="6076e-160">Konfigurace brány firewall serveru</span><span class="sxs-lookup"><span data-stu-id="6076e-160">Configure server firewall</span></span>

<span data-ttu-id="6076e-161">Vytvořte pravidlo brány firewall pro váš server MySQL a povolíte připojení klienta pomocí [az mysql pravidla brány firewall-vytvořit](/cli/azure/mysql/server/firewall-rule#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="6076e-161">Create a firewall rule for your MySQL server to allow client connections by using the [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#create) command.</span></span> 

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name>  \ 
    --resource-group myResourceGroup \ 
    --start-ip-address 0.0.0.0 \ 
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="6076e-162">Azure databáze pro databázi MySQL (Preview) automaticky aktuálně neumožňuje připojení ze služby Azure.</span><span class="sxs-lookup"><span data-stu-id="6076e-162">Azure Database for MySQL (Preview) does not currently automatically enable connections from Azure services.</span></span> <span data-ttu-id="6076e-163">Jak budou dynamicky přiřazovat IP adresy v Azure, je lepší, pokud chcete povolit všechny IP adresy pro nyní.</span><span class="sxs-lookup"><span data-stu-id="6076e-163">As IP addresses in Azure are dynamically assigned, it is better to enable all IP addresses for now.</span></span> <span data-ttu-id="6076e-164">Služba se stále jeho verze preview, lepší metody pro zabezpečení databáze bude povolena.</span><span class="sxs-lookup"><span data-stu-id="6076e-164">As the service continues its preview, better methods for securing your database will be enabled.</span></span>

## <a name="configure-the-azure-mysql-database"></a><span data-ttu-id="6076e-165">Konfiguraci databáze MySQL na Azure</span><span class="sxs-lookup"><span data-stu-id="6076e-165">Configure the Azure MySQL database</span></span>

<span data-ttu-id="6076e-166">V okně terminálu ve vašem počítači připojte k serveru databáze MySQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="6076e-166">In the terminal window on your computer, connect to the MySQL server in Azure.</span></span> <span data-ttu-id="6076e-167">Použít hodnotu zadanou dříve pro `<admin_user>` a `<mysql_server_name>`.</span><span class="sxs-lookup"><span data-stu-id="6076e-167">Use the value you specified previously for `<admin_user>` and `<mysql_server_name>`.</span></span>

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a><span data-ttu-id="6076e-168">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="6076e-168">Create a database</span></span> 

<span data-ttu-id="6076e-169">V `mysql` výzvu, vytvořte databázi a tabulku položkami seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="6076e-169">In the `mysql` prompt, create a database and a table for the to-do items.</span></span>

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a><span data-ttu-id="6076e-170">Vytvořit uživatele s oprávněními</span><span class="sxs-lookup"><span data-stu-id="6076e-170">Create a user with permissions</span></span>

<span data-ttu-id="6076e-171">Vytvoření uživatele databáze a pojmenujte ho všechna oprávnění `tododb` databáze.</span><span class="sxs-lookup"><span data-stu-id="6076e-171">Create a database user and give it all privileges in the `tododb` database.</span></span> <span data-ttu-id="6076e-172">Nahraďte zástupné symboly `<Javaapp_user>` a `<Javaapp_password>` s vlastními jedinečným názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="6076e-172">Replace the placeholders `<Javaapp_user>` and `<Javaapp_password>` with your own unique app name.</span></span>

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* TO '<Javaapp_user>';
```

<span data-ttu-id="6076e-173">Ukončení připojení k serveru zadáním `quit`.</span><span class="sxs-lookup"><span data-stu-id="6076e-173">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="deploy-the-sample-to-azure-app-service"></a><span data-ttu-id="6076e-174">Ukázka nasazení do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6076e-174">Deploy the sample to Azure App Service</span></span>

<span data-ttu-id="6076e-175">Vytvořit plán aplikační služby Azure s **volné** cenová úroveň pomocí [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) rozhraní příkazového řádku příkaz.</span><span class="sxs-lookup"><span data-stu-id="6076e-175">Create an Azure App Service plan with the **FREE** pricing tier using the  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="6076e-176">Plán aplikační služby definuje fyzické prostředky, které jsou použity k hostování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6076e-176">The appservice plan defines the physical resources used to host your apps.</span></span> <span data-ttu-id="6076e-177">Všechny aplikace, které jsou přiřazené plán služby App Service sdílení těchto prostředků, což umožňuje uložit nákladů při hostování více aplikací.</span><span class="sxs-lookup"><span data-stu-id="6076e-177">All applications assigned to an appservice plan share these resources, allowing you to save cost when hosting multiple apps.</span></span> 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

<span data-ttu-id="6076e-178">Až bude plán připravena, rozhraní příkazového řádku Azure ukazuje podobné výstupu v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="6076e-178">When the plan is ready, the Azure CLI shows similar output to the following example:</span></span>

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a><span data-ttu-id="6076e-179">Vytvoření webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="6076e-179">Create an Azure Web app</span></span>

 <span data-ttu-id="6076e-180">Použití [az webapp vytvořit](/cli/azure/appservice/web#create) rozhraní příkazového řádku příkaz k vytvoření definice webové aplikace v `myAppServicePlan` plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="6076e-180">Use the [az webapp create](/cli/azure/appservice/web#create) CLI command to create a web app definition in the `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="6076e-181">Definice webové aplikace adresa URL pro přístup k vaší aplikace pomocí poskytuje a konfiguruje celou řadu možností pro nasazení kódu do Azure.</span><span class="sxs-lookup"><span data-stu-id="6076e-181">The web app definition provides a URL to access your application with and configures several options to deploy your code to Azure.</span></span> 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

<span data-ttu-id="6076e-182">Nahraďte `<app_name>` zástupný symbol vlastní jedinečným názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="6076e-182">Substitute the `<app_name>` placeholder with your own unique app name.</span></span> <span data-ttu-id="6076e-183">Tento jedinečný název je součástí výchozí název domény pro webovou aplikaci, tak název musí být jedinečný v rámci všech aplikací v Azure.</span><span class="sxs-lookup"><span data-stu-id="6076e-183">This unique name is part of the default domain name for the web app, so the name needs to be unique across all apps in Azure.</span></span> <span data-ttu-id="6076e-184">Můžete namapovat zadání názvu vlastní domény do webové aplikace ještě před zveřejněním pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="6076e-184">You can map a custom domain name entry to the web app before you expose it to your users.</span></span>

<span data-ttu-id="6076e-185">Při definici webové aplikace je připraven, rozhraní příkazového řádku Azure uvádí informace podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="6076e-185">When the web app definition is ready, the Azure CLI shows information similar to the following example:</span></span> 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a><span data-ttu-id="6076e-186">Konfigurace Java</span><span class="sxs-lookup"><span data-stu-id="6076e-186">Configure Java</span></span> 

<span data-ttu-id="6076e-187">Nastavení konfigurace modulu runtime Java, která vaše aplikace, musí se [aktualizace konfigurace webové služby App Service az](/cli/azure/appservice/web/config#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="6076e-187">Set up the Java runtime configuration that your app needs with the  [az appservice web config update](/cli/azure/appservice/web/config#update) command.</span></span>

<span data-ttu-id="6076e-188">Následující příkaz nakonfiguruje webové aplikace ke spuštění na poslední JDK 8 Java a [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="6076e-188">The following command configures the web app to run on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

### <a name="configure-the-app-to-use-the-azure-sql-database"></a><span data-ttu-id="6076e-189">Konfigurace aplikace, které chcete použít databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="6076e-189">Configure the app to use the Azure SQL database</span></span>

<span data-ttu-id="6076e-190">Než spustíte ukázkovou aplikaci, nastavte nastavení aplikace na webovou aplikaci k používání databáze MySQL na Azure, kterou jste vytvořili v Azure.</span><span class="sxs-lookup"><span data-stu-id="6076e-190">Before running the sample app, set application settings on the web app to use the Azure MySQL database you created in Azure.</span></span> <span data-ttu-id="6076e-191">Tyto vlastnosti jsou umístěny do webové aplikace jako proměnné prostředí a přepsat s hodnotami nastavenými v application.properties uvnitř zabalené webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6076e-191">These properties are exposed to the web application as environment variables and override the values set in the application.properties inside the packaged web app.</span></span> 

<span data-ttu-id="6076e-192">Nastavení aplikace pomocí [az webapp konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) v rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="6076e-192">Set application settings using [az webapp config appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) in the CLI:</span></span>

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_URL="jdbc:mysql://<mysql_server_name>.mysql.database.azure.com:3306/tododb?verifyServerCertificate=true&useSSL=true&requireSSL=false" \
    --resource-group myResourceGroup \
    --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_USERNAME=Javaapp_user@mysql_server_name  \
    --resource-group myResourceGroup \ 
    --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_URL=Javaapp_password \
    --resource-group myResourceGroup \ 
    --name <app_name>
```

### <a name="get-ftp-deployment-credentials"></a><span data-ttu-id="6076e-193">Získat přihlašovací údaje pro nasazení serveru FTP</span><span class="sxs-lookup"><span data-stu-id="6076e-193">Get FTP deployment credentials</span></span> 
<span data-ttu-id="6076e-194">Můžete nasadit aplikace do služby Azure App Service různými způsoby, včetně FTP, místní Git, GitHub, Visual Studio Team Services a BitBucket.</span><span class="sxs-lookup"><span data-stu-id="6076e-194">You can deploy your application to Azure appservice in various ways including FTP, local Git, GitHub, Visual Studio Team Services, and BitBucket.</span></span> <span data-ttu-id="6076e-195">V tomto příkladu FTP k nasazení. Soubor WAR dříve vytvořené na místním počítači do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="6076e-195">For this example, FTP to deploy the .WAR file built previously on your local machine to Azure App Service.</span></span>

<span data-ttu-id="6076e-196">Chcete-li zjistit, jaké přihlašovací údaje předávat podél příkaz ftp do webové aplikace, použijte [az služby App Service web nasazení seznamu publikování profily](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) příkaz:</span><span class="sxs-lookup"><span data-stu-id="6076e-196">To determine what credentials to pass along in an ftp command to the Web App, Use [az appservice web deployment list-publishing-profiles](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) command:</span></span> 

```azurecli-interactive
az webapp deployment list-publishing-profiles \ 
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" \ 
    --output json
```

```JSON
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-069.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "app_name\\$app_name"
  }
]
```

### <a name="upload-the-app-using-ftp"></a><span data-ttu-id="6076e-197">Nahrání aplikace pomocí protokolu FTP</span><span class="sxs-lookup"><span data-stu-id="6076e-197">Upload the app using FTP</span></span>

<span data-ttu-id="6076e-198">Vaše oblíbené nástroje FTP použít k nasazení. Soubor WAR */site/wwwroot/webapps* složky na adresu serveru, který je převzat ze `URL` pole v předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="6076e-198">Use your favorite FTP tool to deploy the .WAR file to the */site/wwwroot/webapps* folder on the server address taken from the `URL` field in the previous command.</span></span> <span data-ttu-id="6076e-199">Odeberte existující adresář aplikace výchozí (uživatel ROOT) a nahraďte existující ROOT.war s. Soubor WAR součástí výše v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6076e-199">Remove the existing default (ROOT) application directory and replace the existing ROOT.war with the .WAR file built in the earlier in the tutorial.</span></span>

```bash
ftp waws-prod-blu-069.ftp.azurewebsites.windows.net
Connected to waws-prod-blu-069.drip.azurewebsites.windows.net.
220 Microsoft FTP Service
Name (waws-prod-blu-069.ftp.azurewebsites.windows.net:raisa): app_name\$app_name
331 Password required
Password:
cd /site/wwwroot/webapps
mdelete -i ROOT/*
rmdir ROOT/
put target/TodoDemo-0.0.1-SNAPSHOT.war ROOT.war
```

### <a name="test-the-web-app"></a><span data-ttu-id="6076e-200">Test webové aplikace</span><span class="sxs-lookup"><span data-stu-id="6076e-200">Test the web app</span></span>

<span data-ttu-id="6076e-201">Přejděte do `http://<app_name>.azurewebsites.net/` a přidejte do seznamu několik úloh.</span><span class="sxs-lookup"><span data-stu-id="6076e-201">Browse to `http://<app_name>.azurewebsites.net/` and add a few tasks to the list.</span></span> 

![Java aplikace spuštěné v Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

<span data-ttu-id="6076e-203">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="6076e-203">**Congratulations!**</span></span> <span data-ttu-id="6076e-204">Používáte datové aplikace v jazyce Java v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="6076e-204">You're running a data-driven Java app in Azure App Service.</span></span>

## <a name="update-the-app-and-redeploy"></a><span data-ttu-id="6076e-205">Aktualizace a opětovné nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="6076e-205">Update the app and redeploy</span></span>

<span data-ttu-id="6076e-206">Aktualizujte aplikaci pro zahrnutí sloupec v seznamu úkolů určitý den, položka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="6076e-206">Update the application to include an additional column in the todo list for what day the item was created.</span></span> <span data-ttu-id="6076e-207">Spouštěcí pružiny zpracovává aktualizace schématu databáze pro vás jako změn datových modelů beze změny stávajících záznamů databáze.</span><span class="sxs-lookup"><span data-stu-id="6076e-207">Spring Boot handles updating the database schema for you as the data model changes without altering your existing database records.</span></span>

1. <span data-ttu-id="6076e-208">V lokálním systému, otevře *src/main/java/com/example/fabrikam/TodoItem.java* a přidejte následující importy pro třídu:</span><span class="sxs-lookup"><span data-stu-id="6076e-208">On your local system, open up *src/main/java/com/example/fabrikam/TodoItem.java* and add the following imports to the class:</span></span>   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. <span data-ttu-id="6076e-209">Přidat `String` vlastnost `timeCreated` k *src/main/java/com/example/fabrikam/TodoItem.java*, inicializace s časovým razítkem na vytvoření objektu.</span><span class="sxs-lookup"><span data-stu-id="6076e-209">Add a `String` property `timeCreated` to *src/main/java/com/example/fabrikam/TodoItem.java*, initializing it with a timestamp at object creation.</span></span> <span data-ttu-id="6076e-210">Přidání mechanismy získání nebo nastavení pro nové `timeCreated` vlastnost během úprav tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="6076e-210">Add getters/setters for the new `timeCreated` property while you are editing this file.</span></span>

    ```java
    private String name;
    private boolean complete;
    private String timeCreated;
    ...

    public TodoItem(String category, String name) {
       this.category = category;
       this.name = name;
       this.complete = false;
       this.timeCreated = new SimpleDateFormat("MMMM dd, YYYY").format(Calendar.getInstance().getTime());
    }
    ...
    public void setTimeCreated(String timeCreated) {
       this.timeCreated = timeCreated;
    }

    public String getTimeCreated() {
        return timeCreated;
    }
    ```

3. <span data-ttu-id="6076e-211">Aktualizace *src/main/java/com/example/fabrikam/TodoDemoController.java* řádek v `updateTodo` metodu a nastavit časové razítko:</span><span class="sxs-lookup"><span data-stu-id="6076e-211">Update *src/main/java/com/example/fabrikam/TodoDemoController.java* with a line in the `updateTodo` method to set the timestamp:</span></span>

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. <span data-ttu-id="6076e-212">Přidáte podporu pro nové pole v šabloně Thymeleaf.</span><span class="sxs-lookup"><span data-stu-id="6076e-212">Add support for the new field in the Thymeleaf template.</span></span> <span data-ttu-id="6076e-213">Aktualizace *src/main/resources/templates/index.html* s novou hlavičkou tabulky pro časové razítko a nové pole, které chcete zobrazit hodnotu časového razítka v každém řádku dat tabulky.</span><span class="sxs-lookup"><span data-stu-id="6076e-213">Update *src/main/resources/templates/index.html* with a new table header for the timestamp, and a new field to display the value of the timestamp in each table data row.</span></span>

    ```html
    <th>Name</th>
    <th>Category</th>
    <th>Time Created</th>
    <th>Complete</th>
    ...
    <td th:text="${item.category}">item_category</td><input type="hidden" th:field="*{todoList[__${i.index}__].category}"/>
    <td th:text="${item.timeCreated}">item_time_created</td><input type="hidden" th:field="*{todoList[__${i.index}__].timeCreated}"/>
    <td><input type="checkbox" th:checked="${item.complete} == true" th:field="*{todoList[__${i.index}__].complete}"/></td>
    ```

5. <span data-ttu-id="6076e-214">Znovu sestavte aplikaci:</span><span class="sxs-lookup"><span data-stu-id="6076e-214">Rebuild the application:</span></span>

    ```bash
    mvnw clean package 
    ```

6. <span data-ttu-id="6076e-215">FTP aktualizaci. WAR jako před, odebrání stávající *lokality/wwwroot/webapps/ROOT* adresáře a *ROOT.war*, pak odesílání aktualizovaný. Soubor WAR jako ROOT.war.</span><span class="sxs-lookup"><span data-stu-id="6076e-215">FTP the updated .WAR as before, removing the existing *site/wwwroot/webapps/ROOT* directory and *ROOT.war*, then uploading the updated .WAR file as ROOT.war.</span></span> 

<span data-ttu-id="6076e-216">Při aktualizaci aplikace, **čas vytvoření** sloupec je nyní viditelné.</span><span class="sxs-lookup"><span data-stu-id="6076e-216">When you refresh the app, a **Time Created** column is now visible.</span></span> <span data-ttu-id="6076e-217">Když přidáte novou úlohu, aplikace bude naplnit časové razítko.</span><span class="sxs-lookup"><span data-stu-id="6076e-217">When you add a new task, the app will populate the timestamp automatically.</span></span> <span data-ttu-id="6076e-218">Stávající úlohy zůstat beze změny a pracovat s aplikací, i když základní datový model byl změněn.</span><span class="sxs-lookup"><span data-stu-id="6076e-218">Your existing tasks remain unchanged and work with the app even though the underlying data model has changed.</span></span> 

![Aplikace v jazyce Java aktualizovat pomocí nového sloupce](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a><span data-ttu-id="6076e-220">Diagnostické protokoly datového proudu</span><span class="sxs-lookup"><span data-stu-id="6076e-220">Stream diagnostic logs</span></span> 

<span data-ttu-id="6076e-221">Při spuštění aplikace v jazyce Java v Azure App Service, můžete získat protokoly konzoly přesměruje přímo do terminálu.</span><span class="sxs-lookup"><span data-stu-id="6076e-221">While your Java application runs in Azure App Service, you can get the console logs piped directly to your terminal.</span></span> <span data-ttu-id="6076e-222">Tímto způsobem můžete získat stejné diagnostické zprávy pomoci při ladění chyb aplikace.</span><span class="sxs-lookup"><span data-stu-id="6076e-222">That way, you can get the same diagnostic messages to help you debug application errors.</span></span>

<span data-ttu-id="6076e-223">Spusťte protokolu streamování pomocí [az webapp protokolu poškozené databáze za](/cli/azure/appservice/web/log#tail) příkaz.</span><span class="sxs-lookup"><span data-stu-id="6076e-223">To start log streaming, use the [az webapp log tail](/cli/azure/appservice/web/log#tail) command.</span></span>

```azurecli-interactive 
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="6076e-224">Správa Azure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="6076e-224">Manage your Azure web app</span></span>

<span data-ttu-id="6076e-225">Přejděte na portálu Azure najdete v části webové aplikace, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6076e-225">Go to the Azure portal to see the web app you created.</span></span>

<span data-ttu-id="6076e-226">Chcete-li to provést, přihlaste se na adrese [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6076e-226">To do this, sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>

<span data-ttu-id="6076e-227">V levé nabídce klikněte na **App Service** a pak klikněte na název vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="6076e-227">From the left menu, click **App Service**, then click the name of your Azure web app.</span></span>

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-tutorial-java-mysql/access-portal.png)

<span data-ttu-id="6076e-229">Ve výchozím nastavení bude okno vaší webové aplikace obsahovat stránku **Přehled**.</span><span class="sxs-lookup"><span data-stu-id="6076e-229">By default, your web app's blade shows the **Overview** page.</span></span> <span data-ttu-id="6076e-230">Tato stránka poskytuje přehled, jak si vaše aplikace stojí.</span><span class="sxs-lookup"><span data-stu-id="6076e-230">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="6076e-231">Zde můžete také provést úlohy správy, jako zastavení, spuštění, restartování a delete.</span><span class="sxs-lookup"><span data-stu-id="6076e-231">Here, you can also perform management tasks like stop, start, restart, and delete.</span></span> <span data-ttu-id="6076e-232">Karty na levé straně okna obsahují další stránky konfigurace, které můžete otevřít.</span><span class="sxs-lookup"><span data-stu-id="6076e-232">The tabs on the left side of the blade show the different configuration pages you can open.</span></span>

![Okno App Service na webu Azure Portal](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

<span data-ttu-id="6076e-234">Tyto karty v okně zobrazují mnoho skvělých funkcí, které můžete do své webové aplikace přidat.</span><span class="sxs-lookup"><span data-stu-id="6076e-234">These tabs in the blade show the many great features you can add to your web app.</span></span> <span data-ttu-id="6076e-235">Následující seznam obsahuje jen několik možností:</span><span class="sxs-lookup"><span data-stu-id="6076e-235">The following list gives you just a few of the possibilities:</span></span>
* <span data-ttu-id="6076e-236">Mapování vlastního názvu DNS</span><span class="sxs-lookup"><span data-stu-id="6076e-236">Map a custom DNS name</span></span>
* <span data-ttu-id="6076e-237">Vazba vlastního certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="6076e-237">Bind a custom SSL certificate</span></span>
* <span data-ttu-id="6076e-238">Konfigurace průběžného nasazování</span><span class="sxs-lookup"><span data-stu-id="6076e-238">Configure continuous deployment</span></span>
* <span data-ttu-id="6076e-239">Vertikální i horizontální navýšení kapacity</span><span class="sxs-lookup"><span data-stu-id="6076e-239">Scale up and out</span></span>
* <span data-ttu-id="6076e-240">Přidání ověřování uživatelů</span><span class="sxs-lookup"><span data-stu-id="6076e-240">Add user authentication</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="6076e-241">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="6076e-241">Clean up resources</span></span>

<span data-ttu-id="6076e-242">Pokud nepotřebujete tyto prostředky pro jiné kurzu (najdete v části [další kroky](#next)), můžete je odstranit spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="6076e-242">If you don't need these resources for another tutorial (see [Next steps](#next)), you can delete them by running the following command:</span></span> 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="6076e-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6076e-243">Next steps</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6076e-244">Vytvoření databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="6076e-244">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="6076e-245">Připojení k MySQL ukázkovou aplikaci Java</span><span class="sxs-lookup"><span data-stu-id="6076e-245">Connect a sample Java app to the MySQL</span></span>
> * <span data-ttu-id="6076e-246">Nasazení aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="6076e-246">Deploy the app to Azure</span></span>
> * <span data-ttu-id="6076e-247">Aktualizace a opětovné nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="6076e-247">Update and redeploy the app</span></span>
> * <span data-ttu-id="6076e-248">Diagnostické protokoly datového proudu z Azure</span><span class="sxs-lookup"><span data-stu-id="6076e-248">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="6076e-249">Spravovat aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6076e-249">Manage the app in the Azure portal</span></span>

<span data-ttu-id="6076e-250">Přechodu na dalším kurzu se dozvíte, jak namapovat vlastní název DNS do aplikace.</span><span class="sxs-lookup"><span data-stu-id="6076e-250">Advance to the next tutorial to learn how to map a custom DNS name to the app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="6076e-251">Mapování existujícího vlastního názvu DNS na Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="6076e-251">Map an existing custom DNS name to Azure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)