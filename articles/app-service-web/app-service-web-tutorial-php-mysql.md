---
title: "Vytvoření webové aplikace PHP a databáze MySQL v Azure | Microsoft Docs"
description: "Další informace o získání aplikace PHP v Azure, funguje připojení k databázi MySQL v Azure."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 14feb4f3-5095-496e-9a40-690e1414bd73
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 07/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6e8d8962180f7534b0b9074f03ecc8ea431ae1a4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="build-a-php-and-mysql-web-app-in-azure"></a><span data-ttu-id="bdb9d-103">Vytvoření webové aplikace PHP a databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-103">Build a PHP and MySQL web app in Azure</span></span>

<span data-ttu-id="bdb9d-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="bdb9d-105">Tento kurz ukazuje, jak vytvořit webovou aplikaci PHP v Azure a připojte ho k databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-105">This tutorial shows how to create a PHP web app in Azure and connect it to a MySQL database.</span></span> <span data-ttu-id="bdb9d-106">Jakmile budete hotovi, budete mít [Laravel](https://laravel.com/) aplikaci spuštěnou na Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-106">When you're finished, you'll have a [Laravel](https://laravel.com/) app running on Azure App Service Web Apps.</span></span>

![Aplikace PHP, které jsou spuštěné v Azure App Service](./media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

<span data-ttu-id="bdb9d-108">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bdb9d-109">Vytvoření databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-109">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="bdb9d-110">Připojení aplikace PHP k MySQL</span><span class="sxs-lookup"><span data-stu-id="bdb9d-110">Connect a PHP app to MySQL</span></span>
> * <span data-ttu-id="bdb9d-111">Nasazení aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-111">Deploy the app to Azure</span></span>
> * <span data-ttu-id="bdb9d-112">Aktualizovat datový model a aplikaci znovu nasaďte</span><span class="sxs-lookup"><span data-stu-id="bdb9d-112">Update the data model and redeploy the app</span></span>
> * <span data-ttu-id="bdb9d-113">Diagnostické protokoly datového proudu z Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-113">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="bdb9d-114">Spravovat aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-114">Manage the app in the Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bdb9d-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bdb9d-115">Prerequisites</span></span>

<span data-ttu-id="bdb9d-116">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-116">To complete this tutorial:</span></span>

* <span data-ttu-id="bdb9d-117">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-117">[Install Git](https://git-scm.com/)</span></span>
* [<span data-ttu-id="bdb9d-118">Instalace PHP 5.6.4 nebo novější</span><span class="sxs-lookup"><span data-stu-id="bdb9d-118">Install PHP 5.6.4 or above</span></span>](http://php.net/downloads.php)
* [<span data-ttu-id="bdb9d-119">Nainstalujte autora</span><span class="sxs-lookup"><span data-stu-id="bdb9d-119">Install Composer</span></span>](https://getcomposer.org/doc/00-intro.md)
* <span data-ttu-id="bdb9d-120">Povolit následující rozšíření PHP Laravel potřebám: OpenSSL, PDO MySQL, Mbstring, Tokenizátor, XML</span><span class="sxs-lookup"><span data-stu-id="bdb9d-120">Enable the following PHP extensions Laravel needs: OpenSSL, PDO-MySQL, Mbstring, Tokenizer, XML</span></span>
* [<span data-ttu-id="bdb9d-121">Instalace a spuštění MySQL</span><span class="sxs-lookup"><span data-stu-id="bdb9d-121">Install and start MySQL</span></span>](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a><span data-ttu-id="bdb9d-122">Příprava místního MySQL</span><span class="sxs-lookup"><span data-stu-id="bdb9d-122">Prepare local MySQL</span></span>

<span data-ttu-id="bdb9d-123">V tomto kroku vytvoříte databázi v místní server MySQL pro použití v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-123">In this step, you create a database in your local MySQL server for your use in this tutorial.</span></span>

### <a name="connect-to-local-mysql-server"></a><span data-ttu-id="bdb9d-124">Připojení k místnímu serveru MySQL</span><span class="sxs-lookup"><span data-stu-id="bdb9d-124">Connect to local MySQL server</span></span>

<span data-ttu-id="bdb9d-125">Okno terminálu připojte k místní server MySQL.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-125">In a terminal window, connect to your local MySQL server.</span></span> <span data-ttu-id="bdb9d-126">Chcete-li spustit všechny příkazy v tomto kurzu můžete toto okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-126">You can use this terminal window to run all the commands in this tutorial.</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="bdb9d-127">Pokud se zobrazí výzva k zadání hesla, zadejte heslo pro `root` účtu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-127">If you're prompted for a password, enter the password for the `root` account.</span></span> <span data-ttu-id="bdb9d-128">Pokud si nepamatujete heslo kořenového účtu, najdete v části [MySQL: jak resetovat hesla kořenového](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-128">If you don't remember your root account password, see [MySQL: How to Reset the Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span></span>

<span data-ttu-id="bdb9d-129">Pokud váš příkaz úspěšně proběhne, MySQL serveru běží.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-129">If your command runs successfully, then your MySQL server is running.</span></span> <span data-ttu-id="bdb9d-130">Pokud ne, ujistěte se, zda je místní server MySQL spuštěná pomocí následujících [kroky po instalaci MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-130">If not, make sure that your local MySQL server is started by following the [MySQL post-installation steps](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span></span>

### <a name="create-a-database-locally"></a><span data-ttu-id="bdb9d-131">Vytvoření databáze místně</span><span class="sxs-lookup"><span data-stu-id="bdb9d-131">Create a database locally</span></span>

<span data-ttu-id="bdb9d-132">Na `mysql` výzvu, vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-132">At the `mysql` prompt, create a database.</span></span>

```sql 
CREATE DATABASE sampledb;
```

<span data-ttu-id="bdb9d-133">Ukončení připojení k serveru zadáním `quit`.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-133">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a><span data-ttu-id="bdb9d-134">Vytvoření aplikace PHP místně</span><span class="sxs-lookup"><span data-stu-id="bdb9d-134">Create a PHP app locally</span></span>
<span data-ttu-id="bdb9d-135">V tomto kroku získat ukázkovou aplikaci Laravel, konfigurovat jeho připojení k databázi a spustit místně.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-135">In this step, you get a Laravel sample application, configure its database connection, and run it locally.</span></span> 

### <a name="clone-the-sample"></a><span data-ttu-id="bdb9d-136">Clone – ukázka</span><span class="sxs-lookup"><span data-stu-id="bdb9d-136">Clone the sample</span></span>

<span data-ttu-id="bdb9d-137">V okně terminálu `cd` do pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-137">In the terminal window, `cd` to a working directory.</span></span>

<span data-ttu-id="bdb9d-138">Ukázkové úložiště naklonujete spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-138">Run the following command to clone the sample repository.</span></span>

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

<span data-ttu-id="bdb9d-139">`cd`do vašeho klonovaný adresáře.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-139">`cd` to your cloned directory.</span></span>
<span data-ttu-id="bdb9d-140">Nainstalujte požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-140">Install the required packages.</span></span>

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a><span data-ttu-id="bdb9d-141">Konfigurace připojení databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="bdb9d-141">Configure MySQL connection</span></span>

<span data-ttu-id="bdb9d-142">V kořenovém adresáři úložiště, vytvořte soubor s názvem *.env*.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-142">In the repository root, create a file named *.env*.</span></span> <span data-ttu-id="bdb9d-143">Zkopírujte následující proměnné do *.env* souboru.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-143">Copy the following variables into the *.env* file.</span></span> <span data-ttu-id="bdb9d-144">Nahraďte  _&lt;root_password >_ zástupný symbol MySQL kořenového hesla.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-144">Replace the _&lt;root_password>_ placeholder with the MySQL root user's password.</span></span>

```
APP_ENV=local
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=sampledb
DB_USERNAME=root
DB_PASSWORD=<root_password>
```

<span data-ttu-id="bdb9d-145">Informace o tom, jak Laravel používá _.env_ souborů najdete v tématu [konfigurace prostředí Laravel](https://laravel.com/docs/5.4/configuration#environment-configuration).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-145">For information on how Laravel uses the _.env_ file, see [Laravel Environment Configuration](https://laravel.com/docs/5.4/configuration#environment-configuration).</span></span>

### <a name="run-the-sample-locally"></a><span data-ttu-id="bdb9d-146">Spustit ukázku místně</span><span class="sxs-lookup"><span data-stu-id="bdb9d-146">Run the sample locally</span></span>

<span data-ttu-id="bdb9d-147">Spustit [migrace databáze Laravel](https://laravel.com/docs/5.4/migrations) k vytvoření tabulek aplikace potřebuje.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-147">Run [Laravel database migrations](https://laravel.com/docs/5.4/migrations) to create the tables the application needs.</span></span> <span data-ttu-id="bdb9d-148">Pokud chcete zjistit, které tabulky jsou vytvořené v byla migrace, vyhledejte v _databáze nebo migrace_ adresáře v úložišti Git.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-148">To see which tables are created in the migrations, look in the _database/migrations_ directory in the Git repository.</span></span>

```bash
php artisan migrate
```

<span data-ttu-id="bdb9d-149">Vygenerujte nový klíč Laravel aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-149">Generate a new Laravel application key.</span></span>

```bash
php artisan key:generate
```

<span data-ttu-id="bdb9d-150">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-150">Run the application.</span></span>

```bash
php artisan serve
```

<span data-ttu-id="bdb9d-151">V prohlížeči přejděte na `http://localhost:8000`.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-151">Navigate to `http://localhost:8000` in a browser.</span></span> <span data-ttu-id="bdb9d-152">Na stránce přidáte několik úloh.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-152">Add a few tasks in the page.</span></span>

![PHP úspěšně připojí k MySQL](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

<span data-ttu-id="bdb9d-154">Chcete-li zastavit PHP, zadejte `Ctrl + C` v terminálu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-154">To stop PHP, type `Ctrl + C` in the terminal.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a><span data-ttu-id="bdb9d-155">Vytvoření databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-155">Create MySQL in Azure</span></span>

<span data-ttu-id="bdb9d-156">V tomto kroku vytvoříte databázi MySQL v [Azure Database pro databázi MySQL (Preview)](/azure/mysql).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-156">In this step, you create a MySQL database in [Azure Database for MySQL (Preview)](/azure/mysql).</span></span> <span data-ttu-id="bdb9d-157">Později nakonfigurujete aplikace PHP pro připojení k této databázi.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-157">Later, you configure the PHP application to connect to this database.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="bdb9d-158">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="bdb9d-158">Create a resource group</span></span>

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a><span data-ttu-id="bdb9d-159">Vytvoření databáze MySQL serveru</span><span class="sxs-lookup"><span data-stu-id="bdb9d-159">Create a MySQL server</span></span>

<span data-ttu-id="bdb9d-160">Vytvoření serveru ve službě Azure Database pro databázi MySQL (Preview) pomocí [az mysql server vytvořit](/cli/azure/mysql/server#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-160">Create a server in Azure Database for MySQL (Preview) with the [az mysql server create](/cli/azure/mysql/server#create) command.</span></span>

<span data-ttu-id="bdb9d-161">V následujícím příkazu nahraďte název serveru MySQL, kde uvidíte  _&lt;mysql_server_name >_ zástupný symbol (platnými znaky jsou `a-z`, `0-9`, a `-`).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-161">In the following command, substitute your MySQL server name where you see the _&lt;mysql_server_name>_ placeholder (valid characters are `a-z`, `0-9`, and `-`).</span></span> <span data-ttu-id="bdb9d-162">Tento název je součástí názvu hostitele serveru MySQL (`<mysql_server_name>.database.windows.net`), musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-162">This name is part of the MySQL server's hostname  (`<mysql_server_name>.database.windows.net`), it needs to be globally unique.</span></span>

```azurecli-interactive
az mysql server create \
    --name <mysql_server_name> \
    --resource-group myResourceGroup \
    --location "North Europe" \
    --admin-user adminuser \
    --admin-password MySQLAzure2017
```

<span data-ttu-id="bdb9d-163">Při vytvoření serveru MySQL rozhraní příkazového řádku Azure obsahuje informace o podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-163">When the MySQL server is created, the Azure CLI shows information similar to the following example:</span></span>

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a><span data-ttu-id="bdb9d-164">Konfigurace brány firewall serveru</span><span class="sxs-lookup"><span data-stu-id="bdb9d-164">Configure server firewall</span></span>

<span data-ttu-id="bdb9d-165">Vytvořte pravidlo brány firewall pro váš server MySQL a povolíte připojení klienta pomocí [az mysql pravidla brány firewall-vytvořit](/cli/azure/mysql/server/firewall-rule#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-165">Create a firewall rule for your MySQL server to allow client connections by using the [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#create) command.</span></span>

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name> \
    --resource-group myResourceGroup \
    --start-ip-address 0.0.0.0 \
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="bdb9d-166">Azure databáze pro databázi MySQL (Preview) nepodporuje aktuálně omezení počtu připojení pouze ke službám Azure.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-166">Azure Database for MySQL (Preview) doesn't currently limit connections only to Azure services.</span></span> <span data-ttu-id="bdb9d-167">Jak budou dynamicky přiřazovat IP adresy v Azure, je lepší povolit všechny IP adresy.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-167">As IP addresses in Azure are dynamically assigned, it is better to enable all IP addresses.</span></span> <span data-ttu-id="bdb9d-168">Služba není ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-168">The service is in preview.</span></span> <span data-ttu-id="bdb9d-169">Plánujeme lepší metody pro zabezpečení databáze.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-169">Better methods for securing your database are planned.</span></span>
>
>

### <a name="connect-to-production-mysql-server-locally"></a><span data-ttu-id="bdb9d-170">Připojení k serveru pro produkční MySQL místně</span><span class="sxs-lookup"><span data-stu-id="bdb9d-170">Connect to production MySQL server locally</span></span>

<span data-ttu-id="bdb9d-171">V okně terminálu připojte k serveru databáze MySQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-171">In the terminal window, connect to the MySQL server in Azure.</span></span> <span data-ttu-id="bdb9d-172">Použít hodnotu zadanou dříve pro  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-172">Use the value you specified previously for _&lt;mysql_server_name>_.</span></span>

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

<span data-ttu-id="bdb9d-173">Pokud budete vyzváni k zadání hesla, použijte _$tr0ngPa$ w0rd!_, který jste zadali při vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-173">When prompted for a password, use _$tr0ngPa$w0rd!_, which you specified when you created the database.</span></span>

### <a name="create-a-production-database"></a><span data-ttu-id="bdb9d-174">Vytvoření provozní databáze</span><span class="sxs-lookup"><span data-stu-id="bdb9d-174">Create a production database</span></span>

<span data-ttu-id="bdb9d-175">Na `mysql` výzvu, vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-175">At the `mysql` prompt, create a database.</span></span>

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a><span data-ttu-id="bdb9d-176">Vytvořit uživatele s oprávněními</span><span class="sxs-lookup"><span data-stu-id="bdb9d-176">Create a user with permissions</span></span>

<span data-ttu-id="bdb9d-177">Vytvořte uživatele databáze názvem _phpappuser_ a pojmenujte ho všechna oprávnění `sampledb` databáze.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-177">Create a database user called _phpappuser_ and give it all privileges in the `sampledb` database.</span></span>

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'phpappuser';
```

<span data-ttu-id="bdb9d-178">Ukončení připojení k serveru zadáním `quit`.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-178">Exit the server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a><span data-ttu-id="bdb9d-179">Připojení aplikace k Azure MySQL</span><span class="sxs-lookup"><span data-stu-id="bdb9d-179">Connect app to Azure MySQL</span></span>

<span data-ttu-id="bdb9d-180">V tomto kroku připojíte aplikace PHP pro databázi MySQL, kterou jste vytvořili v Azure Database pro databázi MySQL (Preview).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-180">In this step, you connect the PHP application to the MySQL database you created in Azure Database for MySQL (Preview).</span></span>

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a><span data-ttu-id="bdb9d-181">Konfigurace připojení k databázi</span><span class="sxs-lookup"><span data-stu-id="bdb9d-181">Configure the database connection</span></span>

<span data-ttu-id="bdb9d-182">V kořenovém adresáři úložiště vytvořit _. env.production_ soubor a zkopírujte do něj následující proměnné.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-182">In the repository root, create an _.env.production_ file and copy the following variables into it.</span></span> <span data-ttu-id="bdb9d-183">Nahraďte zástupný symbol  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-183">Replace the placeholder _&lt;mysql_server_name>_.</span></span>

```
APP_ENV=production
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=<mysql_server_name>.database.windows.net
DB_DATABASE=sampledb
DB_USERNAME=phpappuser@<mysql_server_name>
DB_PASSWORD=MySQLAzure2017
MYSQL_SSL=true
```

<span data-ttu-id="bdb9d-184">Uložte změny.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-184">Save the changes.</span></span>

> [!TIP]
> <span data-ttu-id="bdb9d-185">Zabezpečit informace o připojení databáze MySQL, tento soubor je již vyloučen z úložiště Git (viz _.gitignore_ v kořenovém adresáři úložiště).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-185">To secure your MySQL connection information, this file is already excluded from the Git repository (See _.gitignore_ in the repository root).</span></span> <span data-ttu-id="bdb9d-186">Později zjistíte, jak nakonfigurovat proměnné prostředí ve službě App Service připojit k vaší databázi ve službě Azure Database pro databázi MySQL (Preview).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-186">Later, you learn how to configure environment variables in App Service to connect to your database in Azure Database for MySQL (Preview).</span></span> <span data-ttu-id="bdb9d-187">Proměnné prostředí, nemusíte *.env* souboru ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-187">With environment variables, you don't need the *.env* file in App Service.</span></span>
>

### <a name="configure-ssl-certificate"></a><span data-ttu-id="bdb9d-188">Nakonfigurujte certifikát protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="bdb9d-188">Configure SSL certificate</span></span>

<span data-ttu-id="bdb9d-189">Ve výchozím nastavení vynucuje Azure Database pro databázi MySQL připojení SSL od klientů.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-189">By default, Azure Database for MySQL enforces SSL connections from clients.</span></span> <span data-ttu-id="bdb9d-190">Pro připojení k databázi MySQL v Azure, je nutné použít _.pem_ certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-190">To connect to your MySQL database in Azure, you must use a _.pem_ SSL certificate.</span></span>

<span data-ttu-id="bdb9d-191">Otevřete _config/database.php_ a přidejte _sslmode_ a _možnosti_ parametry, které `connections.mysql`, jak je znázorněno v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-191">Open _config/database.php_ and add the _sslmode_ and _options_ parameters to `connections.mysql`, as shown in the following code.</span></span>

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/certificate.pem', 
    ] : []
],
```

<span data-ttu-id="bdb9d-192">Další informace o generování to _certificate.pem_, najdete v části [připojení SSL konfigurace v aplikaci pro zabezpečené připojení k databázi Azure pro databázi MySQL](../mysql/howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-192">To learn how to generate this _certificate.pem_, see [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](../mysql/howto-configure-ssl.md).</span></span>

> [!TIP]
> <span data-ttu-id="bdb9d-193">Cesta _/ssl/certificate.pem_ ukazuje na stávající _certificate.pem_ souboru v úložišti Git.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-193">The path _/ssl/certificate.pem_ points to an existing _certificate.pem_ file in the Git repository.</span></span> <span data-ttu-id="bdb9d-194">Tento soubor je poskytován pro usnadnění práce v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-194">This file is provided for convenience in this tutorial.</span></span> <span data-ttu-id="bdb9d-195">Pro nejlepší postup by neměl potvrdit vaše _.pem_ certifikáty do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-195">For best practice, you should not commit your _.pem_ certificates into source control.</span></span> 
>

### <a name="test-the-application-locally"></a><span data-ttu-id="bdb9d-196">Testování aplikace místně</span><span class="sxs-lookup"><span data-stu-id="bdb9d-196">Test the application locally</span></span>

<span data-ttu-id="bdb9d-197">Spustit migrace databáze Laravel s _. env.production_ jako soubor prostředí k vytvoření tabulky v databázi MySQL v Azure Database pro databázi MySQL (Preview).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-197">Run Laravel database migrations with _.env.production_ as the environment file to create the tables in your MySQL database in Azure Database for MySQL (Preview).</span></span> <span data-ttu-id="bdb9d-198">Nezapomeňte, že _. env.production_ obsahuje informace o připojení k vaší databázi MySQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-198">Remember that _.env.production_ has the connection information to your MySQL database in Azure.</span></span>

```bash
php artisan migrate --env=production --force
```

<span data-ttu-id="bdb9d-199">_. env.production_ ještě nemá platnou aplikaci klíč.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-199">_.env.production_ doesn't have a valid application key yet.</span></span> <span data-ttu-id="bdb9d-200">Vygenerujte nový token pro něj v terminálu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-200">Generate a new one for it in the terminal.</span></span>

```bash
php artisan key:generate --env=production --force
```

<span data-ttu-id="bdb9d-201">Spuštění ukázkové aplikace s _. env.production_ jako soubor prostředí.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-201">Run the sample application with _.env.production_ as the environment file.</span></span>

```bash
php artisan serve --env=production
```

<span data-ttu-id="bdb9d-202">Přejděte na `http://localhost:8000`.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-202">Navigate to `http://localhost:8000`.</span></span> <span data-ttu-id="bdb9d-203">Pokud stránka načte bez chyb, aplikace PHP se připojuje k databázi MySQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-203">If the page loads without errors, the PHP application is connecting to the MySQL database in Azure.</span></span>

<span data-ttu-id="bdb9d-204">Na stránce přidáte několik úloh.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-204">Add a few tasks in the page.</span></span>

![PHP připojí úspěšně do Azure Database pro databázi MySQL (Preview)](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

<span data-ttu-id="bdb9d-206">Chcete-li zastavit PHP, zadejte `Ctrl + C` v terminálu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-206">To stop PHP, type `Ctrl + C` in the terminal.</span></span>

### <a name="commit-your-changes"></a><span data-ttu-id="bdb9d-207">Potvrdit změny</span><span class="sxs-lookup"><span data-stu-id="bdb9d-207">Commit your changes</span></span>

<span data-ttu-id="bdb9d-208">Spusťte následující příkazy Git potvrzení změny:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-208">Run the following Git commands to commit your changes:</span></span>

```bash
git add .
git commit -m "database.php updates"
```

<span data-ttu-id="bdb9d-209">Je připravená k nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-209">Your app is ready to be deployed.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="bdb9d-210">Nasazení do Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-210">Deploy to Azure</span></span>

<span data-ttu-id="bdb9d-211">V tomto kroku nasadíte aplikaci PHP MySQL připojení do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-211">In this step, you deploy the MySQL-connected PHP application to Azure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="bdb9d-212">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="bdb9d-212">Create an App Service plan</span></span>

[!INCLUDE [Create app service plan no h](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a><span data-ttu-id="bdb9d-213">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="bdb9d-213">Create a web app</span></span>

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-no-h.md)]

### <a name="set-the-php-version"></a><span data-ttu-id="bdb9d-214">Nastavit verzi PHP</span><span class="sxs-lookup"><span data-stu-id="bdb9d-214">Set the PHP version</span></span>

<span data-ttu-id="bdb9d-215">Nastavit verzi PHP, která aplikace vyžaduje pomocí [az webapp konfigurace sady](/cli/azure/webapp/config#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-215">Set the PHP version that the application requires by using the [az webapp config set](/cli/azure/webapp/config#set) command.</span></span>

<span data-ttu-id="bdb9d-216">Tento příkaz nastaví verzi PHP na _7.0_.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-216">The following command sets the PHP version to _7.0_.</span></span>

```azurecli-interactive
az webapp config set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --php-version 7.0
```

### <a name="configure-database-settings"></a><span data-ttu-id="bdb9d-217">Konfigurace nastavení databáze</span><span class="sxs-lookup"><span data-stu-id="bdb9d-217">Configure database settings</span></span>

<span data-ttu-id="bdb9d-218">Jak bylo uvedeno dříve, můžete připojit k vaší databázi Azure MySQL použití proměnných prostředí ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-218">As pointed out previously, you can connect to your Azure MySQL database using environment variables in App Service.</span></span>

<span data-ttu-id="bdb9d-219">Ve službě App Service, můžete nastavit proměnné prostředí jako _nastavení aplikace_ pomocí [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config/appsettings#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-219">In App Service, you set environment variables as _app settings_ by using the [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#set) command.</span></span>

<span data-ttu-id="bdb9d-220">Následující příkaz nakonfiguruje nastavení aplikace `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, a `DB_PASSWORD`.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-220">The following command configures the app settings `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, and `DB_PASSWORD`.</span></span> <span data-ttu-id="bdb9d-221">Nahraďte zástupné symboly  _&lt;appname >_ a  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-221">Replace the placeholders _&lt;appname>_ and _&lt;mysql_server_name>_.</span></span>

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017" MYSQL_SSL="true"
```

<span data-ttu-id="bdb9d-222">Můžete použít PHP [GETENV –](http://www.php.net/manual/function.getenv.php) metoda pro přístup k nastavení.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-222">You can use the PHP [getenv](http://www.php.net/manual/function.getenv.php) method to access the settings.</span></span> <span data-ttu-id="bdb9d-223">Laravel kód používá [env](https://laravel.com/docs/5.4/helpers#method-env) obálku přes PHP `getenv`.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-223">the Laravel code uses an [env](https://laravel.com/docs/5.4/helpers#method-env) wrapper over the PHP `getenv`.</span></span> <span data-ttu-id="bdb9d-224">Například MySQL konfigurace v _config/database.php_ vypadá jako následující kód:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-224">For example, the MySQL configuration in _config/database.php_ looks like the following code:</span></span>

```php
'mysql' => [
    'driver'    => 'mysql',
    'host'      => env('DB_HOST', 'localhost'),
    'database'  => env('DB_DATABASE', 'forge'),
    'username'  => env('DB_USERNAME', 'forge'),
    'password'  => env('DB_PASSWORD', ''),
    ...
],
```

### <a name="configure-laravel-environment-variables"></a><span data-ttu-id="bdb9d-225">Konfigurace Laravel proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="bdb9d-225">Configure Laravel environment variables</span></span>

<span data-ttu-id="bdb9d-226">Laravel musí klíčem aplikace ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-226">Laravel needs an application key in App Service.</span></span> <span data-ttu-id="bdb9d-227">Můžete ho nakonfigurovat pomocí nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-227">You can configure it with app settings.</span></span>

<span data-ttu-id="bdb9d-228">Použití `php artisan` vygenerovat nový klíč aplikace bez uložení do _.env_.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-228">Use `php artisan` to generate a new application key without saving it to _.env_.</span></span>

```bash
php artisan key:generate --show
```

<span data-ttu-id="bdb9d-229">Nastavit klíč aplikace ve službě App Service webové aplikace pomocí [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config/appsettings#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-229">Set the application key in the App Service web app by using the [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#set) command.</span></span> <span data-ttu-id="bdb9d-230">Nahraďte zástupné symboly  _&lt;appname >_ a  _&lt;outputofphpartisankey: generování >_.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-230">Replace the placeholders _&lt;appname>_ and _&lt;outputofphpartisankey:generate>_.</span></span>

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

<span data-ttu-id="bdb9d-231">`APP_DEBUG="true"`informuje Laravel vrátí informace o ladění v případě chyby zaznamená nasazené webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-231">`APP_DEBUG="true"` tells Laravel to return debugging information when the deployed web app encounters errors.</span></span> <span data-ttu-id="bdb9d-232">Když spustíte produkční aplikace, nastavte ji na `false`, což je bezpečnější.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-232">When running a production application, set it to `false`, which is more secure.</span></span>

### <a name="set-the-virtual-application-path"></a><span data-ttu-id="bdb9d-233">Nastavte cestu virtuální aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-233">Set the virtual application path</span></span>

<span data-ttu-id="bdb9d-234">Nastavte cestu virtuální aplikace pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-234">Set the virtual application path for the web app.</span></span> <span data-ttu-id="bdb9d-235">Tento krok je nezbytný, protože [životního cyklu aplikace Laravel](https://laravel.com/docs/5.4/lifecycle) začíná v _veřejné_ adresář místo kořenový adresář aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-235">This step is required because the [Laravel application lifecycle](https://laravel.com/docs/5.4/lifecycle) begins in the _public_ directory instead of the application's root directory.</span></span> <span data-ttu-id="bdb9d-236">Ostatní platformy PHP, jejichž životního cyklu spuštění v kořenovém adresáři můžete pracovat bez ruční konfigurace cesta virtuální aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-236">Other PHP frameworks whose lifecycle start in the root directory can work without manual configuration of the virtual application path.</span></span>

<span data-ttu-id="bdb9d-237">Nastavit cestu virtuální aplikace pomocí [aktualizace prostředků az](/cli/azure/resource#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-237">Set the virtual application path by using the [az resource update](/cli/azure/resource#update) command.</span></span> <span data-ttu-id="bdb9d-238">Nahraďte  _&lt;appname >_ zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-238">Replace the _&lt;appname>_ placeholder.</span></span>

```azurecli-interactive
az resource update \
    --name web \
    --resource-group myResourceGroup \
    --namespace Microsoft.Web \
    --resource-type config \
    --parent sites/<app_name> \
    --set properties.virtualApplications[0].physicalPath="site\wwwroot\public" \
    --api-version 2015-06-01
```

<span data-ttu-id="bdb9d-239">Ve výchozím nastavení, Azure App Service body kořenovou cestu virtuální aplikace (_/_) do kořenového adresáře souborů nasazené aplikace (_sites\wwwroot_).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-239">By default, Azure App Service points the root virtual application path (_/_) to the root directory of the deployed application files (_sites\wwwroot_).</span></span>

### <a name="configure-a-deployment-user"></a><span data-ttu-id="bdb9d-240">Konfigurace uživatele nasazení</span><span class="sxs-lookup"><span data-stu-id="bdb9d-240">Configure a deployment user</span></span>

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="configure-local-git-deployment"></a><span data-ttu-id="bdb9d-241">Konfigurace nasazení z místního Gitu</span><span class="sxs-lookup"><span data-stu-id="bdb9d-241">Configure local Git deployment</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git-no-h.md)]

### <a name="push-to-azure-from-git"></a><span data-ttu-id="bdb9d-242">Přenos z Gitu do Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-242">Push to Azure from Git</span></span>

<span data-ttu-id="bdb9d-243">Přidejte vzdálené úložiště Azure do místního úložiště Gitu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-243">Add an Azure remote to your local Git repository.</span></span>

```bash
git remote add azure <paste_copied_url_here>
```

<span data-ttu-id="bdb9d-244">Doručte do Azure vzdálené nasazení aplikace PHP.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-244">Push to the Azure remote to deploy the PHP application.</span></span> <span data-ttu-id="bdb9d-245">Zobrazí se výzva k zadání hesla, který jste zadali dříve v rámci vytváření nasazení uživatele.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-245">You are prompted for the password you supplied earlier as part of the creation of the deployment user.</span></span>

```bash
git push azure master
```

<span data-ttu-id="bdb9d-246">Během nasazení Azure App Service komunikuje s Gitem jejím průběhu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-246">During deployment, Azure App Service communicates its progress with Git.</span></span>

```bash
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 291 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'a5e076db9c'.
remote: Running custom deployment command...
remote: Running deployment command...
...
< Output has been truncated for readability >
```

> [!NOTE]
> <span data-ttu-id="bdb9d-247">Můžete si všimnout, že proces nasazení nainstaluje [autora](https://getcomposer.org/) balíčky na konci.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-247">You may notice that the deployment process installs [Composer](https://getcomposer.org/) packages at the end.</span></span> <span data-ttu-id="bdb9d-248">Služby App Service tyto automatizaci během nasazení výchozí nespustí, takže toto úložiště ukázkové má tři další soubory v jeho kořenový adresář povolit:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-248">App Service does not run these automations during default deployment, so this sample repository has three additional files in its root directory to enable it:</span></span>
>
> - <span data-ttu-id="bdb9d-249">`.deployment`– Tento soubor informuje služby App Service ke spuštění `bash deploy.sh` jako vlastní nasazení skriptu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-249">`.deployment` - This file tells App Service to run `bash deploy.sh` as the custom deployment script.</span></span>
> - <span data-ttu-id="bdb9d-250">`deploy.sh`-Vlastní skript nasazení.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-250">`deploy.sh` - The custom deployment script.</span></span> <span data-ttu-id="bdb9d-251">Při kontrole souboru je se zobrazí, že běží `php composer.phar install` po `npm install`.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-251">If you review the file, you will see that it runs `php composer.phar install` after `npm install`.</span></span>
> - <span data-ttu-id="bdb9d-252">`composer.phar`-Autora Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-252">`composer.phar` - The Composer package manager.</span></span>
>
> <span data-ttu-id="bdb9d-253">Tento postup můžete použít k přidání jakéhokoli kroku k nasazení na základě Git do služby App Service.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-253">You can use this approach to add any step to your Git-based deployment to App Service.</span></span> <span data-ttu-id="bdb9d-254">Další informace najdete v tématu [vlastní skript nasazení](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-254">For more information, see [Custom Deployment Script](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).</span></span>
>

### <a name="browse-to-the-azure-web-app"></a><span data-ttu-id="bdb9d-255">Přejděte do webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-255">Browse to the Azure web app</span></span>

<span data-ttu-id="bdb9d-256">Přejděte do `http://<app_name>.azurewebsites.net` a přidejte do seznamu několik úloh.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-256">Browse to `http://<app_name>.azurewebsites.net` and add a few tasks to the list.</span></span>

![Aplikace PHP, které jsou spuštěné v Azure App Service](./media/app-service-web-tutorial-php-mysql/php-mysql-in-azure.png)

<span data-ttu-id="bdb9d-258">Blahopřejeme, používáte PHP aplikace na základě dat ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-258">Congratulations, you're running a data-driven PHP app in Azure App Service.</span></span>

## <a name="update-model-locally-and-redeploy"></a><span data-ttu-id="bdb9d-259">Aktualizace modelu místně a znovu nasaďte</span><span class="sxs-lookup"><span data-stu-id="bdb9d-259">Update model locally and redeploy</span></span>

<span data-ttu-id="bdb9d-260">V tomto kroku provedete jednoduché změnu `task` datový model a webové aplikace a pak publikujte aktualizace do Azure.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-260">In this step, you make a simple change to the `task` data model and the webapp, and then publish the update to Azure.</span></span>

<span data-ttu-id="bdb9d-261">Pro tento scénář úlohy upravit aplikaci tak, že můžete označit úlohu jako dokončenou.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-261">For the tasks scenario, you modify the application so that you can mark a task as complete.</span></span>

### <a name="add-a-column"></a><span data-ttu-id="bdb9d-262">Přidá sloupec</span><span class="sxs-lookup"><span data-stu-id="bdb9d-262">Add a column</span></span>

<span data-ttu-id="bdb9d-263">V terminálu přejděte do kořenového úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-263">In the terminal, navigate to the root of the Git repository.</span></span>

<span data-ttu-id="bdb9d-264">Generovat nové migrace databáze pro `tasks` tabulky:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-264">Generate a new database migration for the `tasks` table:</span></span>

```bash
php artisan make:migration add_complete_column --table=tasks
```

<span data-ttu-id="bdb9d-265">Tento příkaz zobrazí název souboru migrace, aby se vygenerovala.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-265">This command shows you the name of the migration file that's generated.</span></span> <span data-ttu-id="bdb9d-266">Tento soubor v _databáze nebo migrace_ a otevřete ji.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-266">Find this file in _database/migrations_ and open it.</span></span>

<span data-ttu-id="bdb9d-267">Nahraďte `up` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-267">Replace the `up` method with the following code:</span></span>

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

<span data-ttu-id="bdb9d-268">Předchozí kód přidá boolean sloupec `tasks` tabulka s názvem `complete`.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-268">The preceding code adds a boolean column in the `tasks` table called `complete`.</span></span>

<span data-ttu-id="bdb9d-269">Nahraďte `down` metodu akce vrácení zpět s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-269">Replace the `down` method with the following code for the rollback action:</span></span>

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

<span data-ttu-id="bdb9d-270">V terminálu spuštění migrace databáze Laravel udělat změnu v místní databázi.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-270">In the terminal, run Laravel database migrations to make the change in the local database.</span></span>

```bash
php artisan migrate
```

<span data-ttu-id="bdb9d-271">Na základě [Laravel názvů](https://laravel.com/docs/5.4/eloquent#defining-models), modelu `Task` (najdete v části _app/Task.php_) se mapuje `tasks` tabulky ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-271">Based on the [Laravel naming convention](https://laravel.com/docs/5.4/eloquent#defining-models), the model `Task` (see _app/Task.php_) maps to the `tasks` table by default.</span></span>

### <a name="update-application-logic"></a><span data-ttu-id="bdb9d-272">Aktualizace aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="bdb9d-272">Update application logic</span></span>

<span data-ttu-id="bdb9d-273">Otevřete *routes/web.php* souboru.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-273">Open the *routes/web.php* file.</span></span> <span data-ttu-id="bdb9d-274">Aplikace definuje jeho trasy a obchodní logiku sem.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-274">The application defines its routes and business logic here.</span></span>

<span data-ttu-id="bdb9d-275">Na konci souboru přidejte trasu následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-275">At the end of the file, add a route with the following code:</span></span>

```php
/**
 * Toggle Task completeness
 */
Route::post('/task/{id}', function ($id) {
    error_log('INFO: post /task/'.$id);
    $task = Task::findOrFail($id);

    $task->complete = !$task->complete;
    $task->save();

    return redirect('/');
});
```

<span data-ttu-id="bdb9d-276">Předchozí kód je jednoduchý aktualizace do datového modelu přepnutím hodnotu `complete`.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-276">The preceding code makes a simple update to the data model by toggling the value of `complete`.</span></span>

### <a name="update-the-view"></a><span data-ttu-id="bdb9d-277">Aktualizace zobrazení</span><span class="sxs-lookup"><span data-stu-id="bdb9d-277">Update the view</span></span>

<span data-ttu-id="bdb9d-278">Otevřete *resources/views/tasks.blade.php* souboru.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-278">Open the *resources/views/tasks.blade.php* file.</span></span> <span data-ttu-id="bdb9d-279">Najít `<tr>` počáteční značce a nahraďte ho:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-279">Find the `<tr>` opening tag and replace it with:</span></span>

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

<span data-ttu-id="bdb9d-280">Předchozí kód změní barvu řádek v závislosti na tom, zda je úloha dokončena.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-280">The preceding code changes the row color depending on whether the task is complete.</span></span>

<span data-ttu-id="bdb9d-281">V dalším řádku máte následující kód:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-281">In the next line, you have the following code:</span></span>

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

<span data-ttu-id="bdb9d-282">Celý řádek nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-282">Replace the entire line with the following code:</span></span>

```html
<td>
    <form action="{{ url('task/'.$task->id) }}" method="POST">
        {{ csrf_field() }}

        <button type="submit" class="btn btn-xs">
            <i class="fa {{$task->complete ? 'fa-check-square-o' : 'fa-square-o'}}"></i>
        </button>
        {{ $task->name }}
    </form>
</td>
```

<span data-ttu-id="bdb9d-283">Předchozí kód přidá tlačítko pro odeslání, který odkazuje na trasy, která jste definovali dříve.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-283">The preceding code adds the submit button that references the route that you defined earlier.</span></span>

### <a name="test-the-changes-locally"></a><span data-ttu-id="bdb9d-284">Testování, místně</span><span class="sxs-lookup"><span data-stu-id="bdb9d-284">Test the changes locally</span></span>

<span data-ttu-id="bdb9d-285">V kořenovém adresáři úložiště Git spusťte vývojový server.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-285">From the root directory of the Git repository, run the development server.</span></span>

```bash
php artisan serve
```

<span data-ttu-id="bdb9d-286">Chcete-li zobrazit stav úlohy změnit, přejděte na `http://localhost:8000` a zaškrtněte políčko.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-286">To see the task status change, navigate to `http://localhost:8000` and select the checkbox.</span></span>

![Přidání zaškrtnutím políčka úloh](./media/app-service-web-tutorial-php-mysql/complete-checkbox.png)

<span data-ttu-id="bdb9d-288">Chcete-li zastavit PHP, zadejte `Ctrl + C` v terminálu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-288">To stop PHP, type `Ctrl + C` in the terminal.</span></span>

### <a name="publish-changes-to-azure"></a><span data-ttu-id="bdb9d-289">Publikování změn do Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-289">Publish changes to Azure</span></span>

<span data-ttu-id="bdb9d-290">V terminálu spusťte migrace databáze Laravel s provozním připojovací řetězec k provedení změn v databázi Azure.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-290">In the terminal, run Laravel database migrations with the production connection string to make the change in the Azure database.</span></span>

```bash
php artisan migrate --env=production --force
```

<span data-ttu-id="bdb9d-291">Potvrďte všechny změny v úložišti Git a potom odešlete změny kódu do Azure.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-291">Commit all the changes in Git, and then push the code changes to Azure.</span></span>

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

<span data-ttu-id="bdb9d-292">Jednou `git push` je dokončení, přejděte na webové aplikace Azure a testování nových funkcí.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-292">Once the `git push` is complete, navigate to the Azure web app and test the new functionality.</span></span>

![Model a databáze změny, které jsou publikovány do služby Azure](media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

<span data-ttu-id="bdb9d-294">Pokud jste přidali všechny úlohy, se uchovávají v databázi.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-294">If you added any tasks, they are retained in the database.</span></span> <span data-ttu-id="bdb9d-295">Aktualizace schématu data ponechat stávající data beze změn.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-295">Updates to the data schema leave existing data intact.</span></span>

## <a name="stream-diagnostic-logs"></a><span data-ttu-id="bdb9d-296">Diagnostické protokoly datového proudu</span><span class="sxs-lookup"><span data-stu-id="bdb9d-296">Stream diagnostic logs</span></span>

<span data-ttu-id="bdb9d-297">Při spuštění aplikace PHP ve službě Azure App Service, můžete získat protokoly konzoly přesměruje do terminálu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-297">While the PHP application runs in Azure App Service, you can get the console logs piped to your terminal.</span></span> <span data-ttu-id="bdb9d-298">Tímto způsobem můžete získat stejné diagnostické zprávy pomoci při ladění chyb aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-298">That way, you can get the same diagnostic messages to help you debug application errors.</span></span>

<span data-ttu-id="bdb9d-299">Spusťte protokolu streamování pomocí [az webapp protokolu poškozené databáze za](/cli/azure/webapp/log#tail) příkaz.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-299">To start log streaming, use the [az webapp log tail](/cli/azure/webapp/log#tail) command.</span></span>

```azurecli-interactive
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup
```

<span data-ttu-id="bdb9d-300">Po zahájení vysílání datového proudu protokolu, aktualizujte webové aplikace Azure v prohlížeči získat některé webový provoz.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-300">Once log streaming has started, refresh the Azure web app in the browser to get some web traffic.</span></span> <span data-ttu-id="bdb9d-301">Nyní můžete vidět protokoly konzoly přesměruje do terminálu.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-301">You can now see console logs piped to the terminal.</span></span> <span data-ttu-id="bdb9d-302">Pokud nevidíte protokoly konzoly okamžitě, kontrola znovu za 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-302">If you don't see console logs immediately, check again in 30 seconds.</span></span>

<span data-ttu-id="bdb9d-303">Chcete-li zastavit streamování protokolu v kdykoli, zadejte `Ctrl` + `C`.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-303">To stop log streaming at anytime, type `Ctrl`+`C`.</span></span>

> [!TIP]
> <span data-ttu-id="bdb9d-304">Aplikace PHP, můžete použít standardní [error_log()](http://php.net/manual/function.error-log.php) na výstup do konzoly.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-304">A PHP application can use the standard [error_log()](http://php.net/manual/function.error-log.php) to output to the console.</span></span> <span data-ttu-id="bdb9d-305">Ukázková aplikace používá tuto metodu v _app/Http/routes.php_.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-305">The sample application uses this approach in _app/Http/routes.php_.</span></span>
>
> <span data-ttu-id="bdb9d-306">Jako webová architektura [Laravel používá Monolog](https://laravel.com/docs/5.4/errors) jako zprostředkovatel protokolování.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-306">As a web framework, [Laravel uses Monolog](https://laravel.com/docs/5.4/errors) as the logging provider.</span></span> <span data-ttu-id="bdb9d-307">Jak získat Monolog k výstupu zpráv do konzoly, najdete v sekci [PHP: jak používat k přihlášení do konzoly (php://out) monolog](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-307">To see how to get Monolog to output messages to the console, see [PHP: How to use monolog to log to console (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).</span></span>
>
>

## <a name="manage-the-azure-web-app"></a><span data-ttu-id="bdb9d-308">Správa webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-308">Manage the Azure web app</span></span>

<span data-ttu-id="bdb9d-309">Pokud chcete spravovat webovou aplikaci, kterou jste vytvořili, přejděte na web [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bdb9d-309">Go to the [Azure portal](https://portal.azure.com) to manage the web app you created.</span></span>

<span data-ttu-id="bdb9d-310">V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-310">From the left menu, click **App Services**, and then click the name of your Azure web app.</span></span>

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-tutorial-php-mysql/access-portal.png)

<span data-ttu-id="bdb9d-312">Zobrazí se stránka s přehledem vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-312">You see your web app's Overview page.</span></span> <span data-ttu-id="bdb9d-313">Zde můžete provádět základní správu úkoly, jako je zastavení, spuštění, restartování, procházet a delete.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-313">Here, you can perform basic management tasks like  stop, start, restart, browse, and delete.</span></span>

<span data-ttu-id="bdb9d-314">V levé nabídce poskytuje stránky pro konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-314">The left menu provides pages for configuring your app.</span></span>

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-php-mysql/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="bdb9d-316">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bdb9d-316">Next steps</span></span>

<span data-ttu-id="bdb9d-317">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="bdb9d-317">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bdb9d-318">Vytvoření databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-318">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="bdb9d-319">Připojení aplikace PHP k MySQL</span><span class="sxs-lookup"><span data-stu-id="bdb9d-319">Connect a PHP app to MySQL</span></span>
> * <span data-ttu-id="bdb9d-320">Nasazení aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-320">Deploy the app to Azure</span></span>
> * <span data-ttu-id="bdb9d-321">Aktualizovat datový model a aplikaci znovu nasaďte</span><span class="sxs-lookup"><span data-stu-id="bdb9d-321">Update the data model and redeploy the app</span></span>
> * <span data-ttu-id="bdb9d-322">Diagnostické protokoly datového proudu z Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-322">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="bdb9d-323">Spravovat aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="bdb9d-323">Manage the app in the Azure portal</span></span>

<span data-ttu-id="bdb9d-324">Přechodu na dalším kurzu se dozvíte, jak namapovat vlastní název DNS pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bdb9d-324">Advance to the next tutorial to learn how to map a custom DNS name to a web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bdb9d-325">Mapování existujícího vlastního názvu DNS na Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="bdb9d-325">Map an existing custom DNS name to Azure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
