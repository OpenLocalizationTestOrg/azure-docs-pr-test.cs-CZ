---
title: "aaaBuild webové aplikace PHP a databáze MySQL v Azure | Microsoft Docs"
description: "Zjistěte, jak tooget aplikace PHP v Azure funguje s připojení tooa MySQL databáze v Azure."
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
ms.openlocfilehash: 3c050b30e2e2c80d011bed989cd5f8cecac35d15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-php-and-mysql-web-app-in-azure"></a><span data-ttu-id="fe574-103">Vytvoření webové aplikace PHP a databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="fe574-103">Build a PHP and MySQL web app in Azure</span></span>

<span data-ttu-id="fe574-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="fe574-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="fe574-105">Tento kurz ukazuje, jak toocreate PHP webové aplikace v Azure a připojte ho tooa databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="fe574-105">This tutorial shows how toocreate a PHP web app in Azure and connect it tooa MySQL database.</span></span> <span data-ttu-id="fe574-106">Jakmile budete hotovi, budete mít [Laravel](https://laravel.com/) aplikaci spuštěnou na Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="fe574-106">When you're finished, you'll have a [Laravel](https://laravel.com/) app running on Azure App Service Web Apps.</span></span>

![Aplikace PHP, které jsou spuštěné v Azure App Service](./media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

<span data-ttu-id="fe574-108">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="fe574-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fe574-109">Vytvoření databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="fe574-109">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="fe574-110">Připojit tooMySQL aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="fe574-110">Connect a PHP app tooMySQL</span></span>
> * <span data-ttu-id="fe574-111">Nasazení aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="fe574-111">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="fe574-112">Aktualizovat hello datový model a znovu nasaďte aplikace hello</span><span class="sxs-lookup"><span data-stu-id="fe574-112">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="fe574-113">Diagnostické protokoly datového proudu z Azure</span><span class="sxs-lookup"><span data-stu-id="fe574-113">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="fe574-114">Spravovat aplikace hello v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fe574-114">Manage hello app in hello Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe574-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fe574-115">Prerequisites</span></span>

<span data-ttu-id="fe574-116">toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="fe574-116">toocomplete this tutorial:</span></span>

* <span data-ttu-id="fe574-117">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="fe574-117">[Install Git](https://git-scm.com/)</span></span>
* [<span data-ttu-id="fe574-118">Instalace PHP 5.6.4 nebo novější</span><span class="sxs-lookup"><span data-stu-id="fe574-118">Install PHP 5.6.4 or above</span></span>](http://php.net/downloads.php)
* [<span data-ttu-id="fe574-119">Nainstalujte autora</span><span class="sxs-lookup"><span data-stu-id="fe574-119">Install Composer</span></span>](https://getcomposer.org/doc/00-intro.md)
* <span data-ttu-id="fe574-120">Povolit následující rozšíření PHP Laravel potřebám hello: OpenSSL, PDO MySQL, Mbstring, Tokenizátor, XML</span><span class="sxs-lookup"><span data-stu-id="fe574-120">Enable hello following PHP extensions Laravel needs: OpenSSL, PDO-MySQL, Mbstring, Tokenizer, XML</span></span>
* [<span data-ttu-id="fe574-121">Instalace a spuštění MySQL</span><span class="sxs-lookup"><span data-stu-id="fe574-121">Install and start MySQL</span></span>](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a><span data-ttu-id="fe574-122">Příprava místního MySQL</span><span class="sxs-lookup"><span data-stu-id="fe574-122">Prepare local MySQL</span></span>

<span data-ttu-id="fe574-123">V tomto kroku vytvoříte databázi v místní server MySQL pro použití v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="fe574-123">In this step, you create a database in your local MySQL server for your use in this tutorial.</span></span>

### <a name="connect-toolocal-mysql-server"></a><span data-ttu-id="fe574-124">Připojení serveru toolocal MySQL</span><span class="sxs-lookup"><span data-stu-id="fe574-124">Connect toolocal MySQL server</span></span>

<span data-ttu-id="fe574-125">V okně terminálu připojte místní server MySQL tooyour.</span><span class="sxs-lookup"><span data-stu-id="fe574-125">In a terminal window, connect tooyour local MySQL server.</span></span> <span data-ttu-id="fe574-126">Všechny příkazy hello toto okno terminálu toorun můžete použít v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="fe574-126">You can use this terminal window toorun all hello commands in this tutorial.</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="fe574-127">Pokud se zobrazí výzva k zadání hesla, zadejte heslo hello hello `root` účtu.</span><span class="sxs-lookup"><span data-stu-id="fe574-127">If you're prompted for a password, enter hello password for hello `root` account.</span></span> <span data-ttu-id="fe574-128">Pokud si nepamatujete heslo kořenového účtu, najdete v části [MySQL: jak tooReset hello kořenové heslo](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span><span class="sxs-lookup"><span data-stu-id="fe574-128">If you don't remember your root account password, see [MySQL: How tooReset hello Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span></span>

<span data-ttu-id="fe574-129">Pokud váš příkaz úspěšně proběhne, MySQL serveru běží.</span><span class="sxs-lookup"><span data-stu-id="fe574-129">If your command runs successfully, then your MySQL server is running.</span></span> <span data-ttu-id="fe574-130">Pokud ne, ujistěte se, že místní server MySQL se spustí následující hello [kroky po instalaci MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span><span class="sxs-lookup"><span data-stu-id="fe574-130">If not, make sure that your local MySQL server is started by following hello [MySQL post-installation steps](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span></span>

### <a name="create-a-database-locally"></a><span data-ttu-id="fe574-131">Vytvoření databáze místně</span><span class="sxs-lookup"><span data-stu-id="fe574-131">Create a database locally</span></span>

<span data-ttu-id="fe574-132">V hello `mysql` výzvu, vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="fe574-132">At hello `mysql` prompt, create a database.</span></span>

```sql 
CREATE DATABASE sampledb;
```

<span data-ttu-id="fe574-133">Ukončení připojení k serveru zadáním `quit`.</span><span class="sxs-lookup"><span data-stu-id="fe574-133">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a><span data-ttu-id="fe574-134">Vytvoření aplikace PHP místně</span><span class="sxs-lookup"><span data-stu-id="fe574-134">Create a PHP app locally</span></span>
<span data-ttu-id="fe574-135">V tomto kroku získat ukázkovou aplikaci Laravel, konfigurovat jeho připojení k databázi a spustit místně.</span><span class="sxs-lookup"><span data-stu-id="fe574-135">In this step, you get a Laravel sample application, configure its database connection, and run it locally.</span></span> 

### <a name="clone-hello-sample"></a><span data-ttu-id="fe574-136">Ukázka hello klonování</span><span class="sxs-lookup"><span data-stu-id="fe574-136">Clone hello sample</span></span>

<span data-ttu-id="fe574-137">V okně terminálu hello `cd` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="fe574-137">In hello terminal window, `cd` tooa working directory.</span></span>

<span data-ttu-id="fe574-138">Spusťte následující příkaz tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-138">Run hello following command tooclone hello sample repository.</span></span>

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

<span data-ttu-id="fe574-139">`cd`tooyour klonovaný adresář.</span><span class="sxs-lookup"><span data-stu-id="fe574-139">`cd` tooyour cloned directory.</span></span>
<span data-ttu-id="fe574-140">Instalovat hello požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="fe574-140">Install hello required packages.</span></span>

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a><span data-ttu-id="fe574-141">Konfigurace připojení databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="fe574-141">Configure MySQL connection</span></span>

<span data-ttu-id="fe574-142">V kořenovém úložišti hello, vytvořte soubor s názvem *.env*.</span><span class="sxs-lookup"><span data-stu-id="fe574-142">In hello repository root, create a file named *.env*.</span></span> <span data-ttu-id="fe574-143">Kopírování hello následující proměnné do hello *.env* souboru.</span><span class="sxs-lookup"><span data-stu-id="fe574-143">Copy hello following variables into hello *.env* file.</span></span> <span data-ttu-id="fe574-144">Nahraďte hello  _&lt;root_password >_ zástupný symbol hello MySQL kořenové heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="fe574-144">Replace hello _&lt;root_password>_ placeholder with hello MySQL root user's password.</span></span>

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

<span data-ttu-id="fe574-145">Informace o tom, jak Laravel používá hello _.env_ souborů najdete v tématu [konfigurace prostředí Laravel](https://laravel.com/docs/5.4/configuration#environment-configuration).</span><span class="sxs-lookup"><span data-stu-id="fe574-145">For information on how Laravel uses hello _.env_ file, see [Laravel Environment Configuration](https://laravel.com/docs/5.4/configuration#environment-configuration).</span></span>

### <a name="run-hello-sample-locally"></a><span data-ttu-id="fe574-146">Hello ukázku spustit místně</span><span class="sxs-lookup"><span data-stu-id="fe574-146">Run hello sample locally</span></span>

<span data-ttu-id="fe574-147">Spustit [migrace databáze Laravel](https://laravel.com/docs/5.4/migrations) toocreate hello tabulky potřebám aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-147">Run [Laravel database migrations](https://laravel.com/docs/5.4/migrations) toocreate hello tables hello application needs.</span></span> <span data-ttu-id="fe574-148">toosee tabulky, které jsou vytvořené v hello migrace, vyhledejte v hello _databáze nebo migrace_ adresáře v úložišti Git hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-148">toosee which tables are created in hello migrations, look in hello _database/migrations_ directory in hello Git repository.</span></span>

```bash
php artisan migrate
```

<span data-ttu-id="fe574-149">Vygenerujte nový klíč Laravel aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe574-149">Generate a new Laravel application key.</span></span>

```bash
php artisan key:generate
```

<span data-ttu-id="fe574-150">Spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-150">Run hello application.</span></span>

```bash
php artisan serve
```

<span data-ttu-id="fe574-151">Přejděte příliš`http://localhost:8000` v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="fe574-151">Navigate too`http://localhost:8000` in a browser.</span></span> <span data-ttu-id="fe574-152">Přidáte několik úloh na stránce hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-152">Add a few tasks in hello page.</span></span>

![PHP připojí úspěšně tooMySQL](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

<span data-ttu-id="fe574-154">Zadejte toostop PHP, `Ctrl + C` v terminálu hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-154">toostop PHP, type `Ctrl + C` in hello terminal.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a><span data-ttu-id="fe574-155">Vytvoření databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="fe574-155">Create MySQL in Azure</span></span>

<span data-ttu-id="fe574-156">V tomto kroku vytvoříte databázi MySQL v [Azure Database pro databázi MySQL (Preview)](/azure/mysql).</span><span class="sxs-lookup"><span data-stu-id="fe574-156">In this step, you create a MySQL database in [Azure Database for MySQL (Preview)](/azure/mysql).</span></span> <span data-ttu-id="fe574-157">Později nakonfigurujte hello PHP aplikace tooconnect toothis databáze.</span><span class="sxs-lookup"><span data-stu-id="fe574-157">Later, you configure hello PHP application tooconnect toothis database.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="fe574-158">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="fe574-158">Create a resource group</span></span>

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a><span data-ttu-id="fe574-159">Vytvoření databáze MySQL serveru</span><span class="sxs-lookup"><span data-stu-id="fe574-159">Create a MySQL server</span></span>

<span data-ttu-id="fe574-160">Vytvoření serveru ve službě Azure Database pro databázi MySQL (Preview) s hello [az mysql server vytvořit](/cli/azure/mysql/server#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fe574-160">Create a server in Azure Database for MySQL (Preview) with hello [az mysql server create](/cli/azure/mysql/server#create) command.</span></span>

<span data-ttu-id="fe574-161">V hello následující příkaz, nahraďte název serveru MySQL, kde uvidíte hello  _&lt;mysql_server_name >_ zástupný symbol (platnými znaky jsou `a-z`, `0-9`, a `-`).</span><span class="sxs-lookup"><span data-stu-id="fe574-161">In hello following command, substitute your MySQL server name where you see hello _&lt;mysql_server_name>_ placeholder (valid characters are `a-z`, `0-9`, and `-`).</span></span> <span data-ttu-id="fe574-162">Tento název je součástí názvu hostitele serveru MySQL hello (`<mysql_server_name>.database.windows.net`), je nutné toobe globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="fe574-162">This name is part of hello MySQL server's hostname  (`<mysql_server_name>.database.windows.net`), it needs toobe globally unique.</span></span>

```azurecli-interactive
az mysql server create \
    --name <mysql_server_name> \
    --resource-group myResourceGroup \
    --location "North Europe" \
    --admin-user adminuser \
    --admin-password MySQLAzure2017
```

<span data-ttu-id="fe574-163">Při vytvoření hello MySQL serveru je hello rozhraní příkazového řádku Azure obsahuje informace o podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="fe574-163">When hello MySQL server is created, hello Azure CLI shows information similar toohello following example:</span></span>

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

### <a name="configure-server-firewall"></a><span data-ttu-id="fe574-164">Konfigurace brány firewall serveru</span><span class="sxs-lookup"><span data-stu-id="fe574-164">Configure server firewall</span></span>

<span data-ttu-id="fe574-165">Vytvořit pravidlo brány firewall pro klientské MySQL serveru tooallow připojení pomocí hello [az mysql pravidla brány firewall-vytvořit](/cli/azure/mysql/server/firewall-rule#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fe574-165">Create a firewall rule for your MySQL server tooallow client connections by using hello [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#create) command.</span></span>

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name> \
    --resource-group myResourceGroup \
    --start-ip-address 0.0.0.0 \
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="fe574-166">Azure databáze pro databázi MySQL (Preview) nepodporuje omezit aktuálně připojení pouze tooAzure služby.</span><span class="sxs-lookup"><span data-stu-id="fe574-166">Azure Database for MySQL (Preview) doesn't currently limit connections only tooAzure services.</span></span> <span data-ttu-id="fe574-167">Jak budou dynamicky přiřazovat IP adresy v Azure, je lepší tooenable všechny IP adresy.</span><span class="sxs-lookup"><span data-stu-id="fe574-167">As IP addresses in Azure are dynamically assigned, it is better tooenable all IP addresses.</span></span> <span data-ttu-id="fe574-168">Služba Hello je ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="fe574-168">hello service is in preview.</span></span> <span data-ttu-id="fe574-169">Plánujeme lepší metody pro zabezpečení databáze.</span><span class="sxs-lookup"><span data-stu-id="fe574-169">Better methods for securing your database are planned.</span></span>
>
>

### <a name="connect-tooproduction-mysql-server-locally"></a><span data-ttu-id="fe574-170">Připojení tooproduction MySQL místně serveru</span><span class="sxs-lookup"><span data-stu-id="fe574-170">Connect tooproduction MySQL server locally</span></span>

<span data-ttu-id="fe574-171">V okně terminálu hello Připojte server toohello MySQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="fe574-171">In hello terminal window, connect toohello MySQL server in Azure.</span></span> <span data-ttu-id="fe574-172">Použít hodnotu hello jste zadali dřív pro  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="fe574-172">Use hello value you specified previously for _&lt;mysql_server_name>_.</span></span>

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

<span data-ttu-id="fe574-173">Pokud budete vyzváni k zadání hesla, použijte _$tr0ngPa$ w0rd!_, který jste zadali při vytváření databáze hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-173">When prompted for a password, use _$tr0ngPa$w0rd!_, which you specified when you created hello database.</span></span>

### <a name="create-a-production-database"></a><span data-ttu-id="fe574-174">Vytvoření provozní databáze</span><span class="sxs-lookup"><span data-stu-id="fe574-174">Create a production database</span></span>

<span data-ttu-id="fe574-175">V hello `mysql` výzvu, vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="fe574-175">At hello `mysql` prompt, create a database.</span></span>

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a><span data-ttu-id="fe574-176">Vytvořit uživatele s oprávněními</span><span class="sxs-lookup"><span data-stu-id="fe574-176">Create a user with permissions</span></span>

<span data-ttu-id="fe574-177">Vytvořte uživatele databáze názvem _phpappuser_ a pojmenujte ho všechna oprávnění v hello `sampledb` databáze.</span><span class="sxs-lookup"><span data-stu-id="fe574-177">Create a database user called _phpappuser_ and give it all privileges in hello `sampledb` database.</span></span>

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* too'phpappuser';
```

<span data-ttu-id="fe574-178">Ukončení připojení serveru hello zadáním `quit`.</span><span class="sxs-lookup"><span data-stu-id="fe574-178">Exit hello server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="connect-app-tooazure-mysql"></a><span data-ttu-id="fe574-179">Připojení aplikace tooAzure MySQL</span><span class="sxs-lookup"><span data-stu-id="fe574-179">Connect app tooAzure MySQL</span></span>

<span data-ttu-id="fe574-180">V tomto kroku připojíte hello PHP aplikace toohello databáze MySQL, kterou jste vytvořili ve službě Azure Database pro databázi MySQL (Preview).</span><span class="sxs-lookup"><span data-stu-id="fe574-180">In this step, you connect hello PHP application toohello MySQL database you created in Azure Database for MySQL (Preview).</span></span>

<a name="devconfig"></a>

### <a name="configure-hello-database-connection"></a><span data-ttu-id="fe574-181">Konfigurace připojení k databázi hello</span><span class="sxs-lookup"><span data-stu-id="fe574-181">Configure hello database connection</span></span>

<span data-ttu-id="fe574-182">V kořenovém úložišti hello, vytvořte _. env.production_ hello soubor a zkopírujte do něj následující proměnné.</span><span class="sxs-lookup"><span data-stu-id="fe574-182">In hello repository root, create an _.env.production_ file and copy hello following variables into it.</span></span> <span data-ttu-id="fe574-183">Nahraďte zástupný symbol hello  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="fe574-183">Replace hello placeholder _&lt;mysql_server_name>_.</span></span>

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

<span data-ttu-id="fe574-184">Uložte změny hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-184">Save hello changes.</span></span>

> [!TIP]
> <span data-ttu-id="fe574-185">toosecure MySQL informace o připojení, tento soubor je již vyloučen z úložiště Git hello (viz _.gitignore_ v kořenovém úložišti hello).</span><span class="sxs-lookup"><span data-stu-id="fe574-185">toosecure your MySQL connection information, this file is already excluded from hello Git repository (See _.gitignore_ in hello repository root).</span></span> <span data-ttu-id="fe574-186">Později zjistíte, jak tooconfigure proměnné prostředí v App Service tooconnect tooyour databáze v databázi Azure pro databázi MySQL (Preview).</span><span class="sxs-lookup"><span data-stu-id="fe574-186">Later, you learn how tooconfigure environment variables in App Service tooconnect tooyour database in Azure Database for MySQL (Preview).</span></span> <span data-ttu-id="fe574-187">Proměnné prostředí, nemusíte hello *.env* souboru ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="fe574-187">With environment variables, you don't need hello *.env* file in App Service.</span></span>
>

### <a name="configure-ssl-certificate"></a><span data-ttu-id="fe574-188">Nakonfigurujte certifikát protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="fe574-188">Configure SSL certificate</span></span>

<span data-ttu-id="fe574-189">Ve výchozím nastavení vynucuje Azure Database pro databázi MySQL připojení SSL od klientů.</span><span class="sxs-lookup"><span data-stu-id="fe574-189">By default, Azure Database for MySQL enforces SSL connections from clients.</span></span> <span data-ttu-id="fe574-190">tooconnect tooyour databáze MySQL v Azure, je nutné použít _.pem_ certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="fe574-190">tooconnect tooyour MySQL database in Azure, you must use a _.pem_ SSL certificate.</span></span>

<span data-ttu-id="fe574-191">Otevřete _config/database.php_ a přidejte hello _sslmode_ a _možnosti_ parametry příliš`connections.mysql`, jak je znázorněno v následujícím kódu hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-191">Open _config/database.php_ and add hello _sslmode_ and _options_ parameters too`connections.mysql`, as shown in hello following code.</span></span>

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/certificate.pem', 
    ] : []
],
```

<span data-ttu-id="fe574-192">toolearn jak toogenerate to _certificate.pem_, najdete v části [připojení konfigurace protokolu SSL ve vaší aplikaci toosecurely connect tooAzure databáze pro databázi MySQL](../mysql/howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="fe574-192">toolearn how toogenerate this _certificate.pem_, see [Configure SSL connectivity in your application toosecurely connect tooAzure Database for MySQL](../mysql/howto-configure-ssl.md).</span></span>

> [!TIP]
> <span data-ttu-id="fe574-193">Cesta Hello _/ssl/certificate.pem_ body existující tooan _certificate.pem_ souboru v úložišti Git hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-193">hello path _/ssl/certificate.pem_ points tooan existing _certificate.pem_ file in hello Git repository.</span></span> <span data-ttu-id="fe574-194">Tento soubor je poskytován pro usnadnění práce v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="fe574-194">This file is provided for convenience in this tutorial.</span></span> <span data-ttu-id="fe574-195">Pro nejlepší postup by neměl potvrdit vaše _.pem_ certifikáty do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="fe574-195">For best practice, you should not commit your _.pem_ certificates into source control.</span></span> 
>

### <a name="test-hello-application-locally"></a><span data-ttu-id="fe574-196">Testování aplikace hello místně</span><span class="sxs-lookup"><span data-stu-id="fe574-196">Test hello application locally</span></span>

<span data-ttu-id="fe574-197">Spustit migrace databáze Laravel s _. env.production_ jako hello prostředí soubor toocreate hello tabulek v databázi MySQL v Azure Database pro databázi MySQL (Preview).</span><span class="sxs-lookup"><span data-stu-id="fe574-197">Run Laravel database migrations with _.env.production_ as hello environment file toocreate hello tables in your MySQL database in Azure Database for MySQL (Preview).</span></span> <span data-ttu-id="fe574-198">Nezapomeňte, že _. env.production_ má databáze MySQL tooyour informace o připojení hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="fe574-198">Remember that _.env.production_ has hello connection information tooyour MySQL database in Azure.</span></span>

```bash
php artisan migrate --env=production --force
```

<span data-ttu-id="fe574-199">_. env.production_ ještě nemá platnou aplikaci klíč.</span><span class="sxs-lookup"><span data-stu-id="fe574-199">_.env.production_ doesn't have a valid application key yet.</span></span> <span data-ttu-id="fe574-200">Vygenerujte nový token pro něj v terminálu hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-200">Generate a new one for it in hello terminal.</span></span>

```bash
php artisan key:generate --env=production --force
```

<span data-ttu-id="fe574-201">Spuštění ukázkové aplikace hello s _. env.production_ jako soubor hello prostředí.</span><span class="sxs-lookup"><span data-stu-id="fe574-201">Run hello sample application with _.env.production_ as hello environment file.</span></span>

```bash
php artisan serve --env=production
```

<span data-ttu-id="fe574-202">Přejděte příliš`http://localhost:8000`.</span><span class="sxs-lookup"><span data-stu-id="fe574-202">Navigate too`http://localhost:8000`.</span></span> <span data-ttu-id="fe574-203">Pokud hello stránka načte bez chyb, hello aplikace PHP se připojuje toohello databáze MySQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="fe574-203">If hello page loads without errors, hello PHP application is connecting toohello MySQL database in Azure.</span></span>

<span data-ttu-id="fe574-204">Přidáte několik úloh na stránce hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-204">Add a few tasks in hello page.</span></span>

![PHP připojí úspěšně tooAzure databáze pro databázi MySQL (Preview)](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

<span data-ttu-id="fe574-206">Zadejte toostop PHP, `Ctrl + C` v terminálu hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-206">toostop PHP, type `Ctrl + C` in hello terminal.</span></span>

### <a name="commit-your-changes"></a><span data-ttu-id="fe574-207">Potvrdit změny</span><span class="sxs-lookup"><span data-stu-id="fe574-207">Commit your changes</span></span>

<span data-ttu-id="fe574-208">Spusťte následující příkazy toocommit Git hello změny:</span><span class="sxs-lookup"><span data-stu-id="fe574-208">Run hello following Git commands toocommit your changes:</span></span>

```bash
git add .
git commit -m "database.php updates"
```

<span data-ttu-id="fe574-209">Aplikace je připravená toobe nasazení.</span><span class="sxs-lookup"><span data-stu-id="fe574-209">Your app is ready toobe deployed.</span></span>

## <a name="deploy-tooazure"></a><span data-ttu-id="fe574-210">Nasazení tooAzure</span><span class="sxs-lookup"><span data-stu-id="fe574-210">Deploy tooAzure</span></span>

<span data-ttu-id="fe574-211">V tomto kroku nasadíte hello připojené MySQL PHP aplikace tooAzure služby App Service.</span><span class="sxs-lookup"><span data-stu-id="fe574-211">In this step, you deploy hello MySQL-connected PHP application tooAzure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="fe574-212">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="fe574-212">Create an App Service plan</span></span>

[!INCLUDE [Create app service plan no h](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a><span data-ttu-id="fe574-213">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="fe574-213">Create a web app</span></span>

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-no-h.md)]

### <a name="set-hello-php-version"></a><span data-ttu-id="fe574-214">Nastavit verzi PHP hello</span><span class="sxs-lookup"><span data-stu-id="fe574-214">Set hello PHP version</span></span>

<span data-ttu-id="fe574-215">Nastavit hello PHP verzi, která hello aplikace vyžaduje pomocí hello [az webapp konfigurace sady](/cli/azure/webapp/config#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fe574-215">Set hello PHP version that hello application requires by using hello [az webapp config set](/cli/azure/webapp/config#set) command.</span></span>

<span data-ttu-id="fe574-216">Hello následující příkaz nastaví too_7.0_ verze PHP hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-216">hello following command sets hello PHP version too_7.0_.</span></span>

```azurecli-interactive
az webapp config set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --php-version 7.0
```

### <a name="configure-database-settings"></a><span data-ttu-id="fe574-217">Konfigurace nastavení databáze</span><span class="sxs-lookup"><span data-stu-id="fe574-217">Configure database settings</span></span>

<span data-ttu-id="fe574-218">Jak bylo uvedeno dříve, můžete se připojit databáze Azure MySQL tooyour použití proměnných prostředí ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="fe574-218">As pointed out previously, you can connect tooyour Azure MySQL database using environment variables in App Service.</span></span>

<span data-ttu-id="fe574-219">Ve službě App Service, můžete nastavit proměnné prostředí jako _nastavení aplikace_ pomocí hello [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config/appsettings#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fe574-219">In App Service, you set environment variables as _app settings_ by using hello [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#set) command.</span></span>

<span data-ttu-id="fe574-220">Hello následující příkaz nakonfiguruje nastavení aplikace hello `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, a `DB_PASSWORD`.</span><span class="sxs-lookup"><span data-stu-id="fe574-220">hello following command configures hello app settings `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, and `DB_PASSWORD`.</span></span> <span data-ttu-id="fe574-221">Nahraďte zástupné symboly hello  _&lt;appname >_ a  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="fe574-221">Replace hello placeholders _&lt;appname>_ and _&lt;mysql_server_name>_.</span></span>

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017" MYSQL_SSL="true"
```

<span data-ttu-id="fe574-222">Můžete použít hello PHP [GETENV –](http://www.php.net/manual/function.getenv.php) metoda tooaccess hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="fe574-222">You can use hello PHP [getenv](http://www.php.net/manual/function.getenv.php) method tooaccess hello settings.</span></span> <span data-ttu-id="fe574-223">Hello Laravel kód používá [env](https://laravel.com/docs/5.4/helpers#method-env) obálku přes hello PHP `getenv`.</span><span class="sxs-lookup"><span data-stu-id="fe574-223">hello Laravel code uses an [env](https://laravel.com/docs/5.4/helpers#method-env) wrapper over hello PHP `getenv`.</span></span> <span data-ttu-id="fe574-224">Například hello MySQL konfigurace v _config/database.php_ vypadá hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="fe574-224">For example, hello MySQL configuration in _config/database.php_ looks like hello following code:</span></span>

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

### <a name="configure-laravel-environment-variables"></a><span data-ttu-id="fe574-225">Konfigurace Laravel proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="fe574-225">Configure Laravel environment variables</span></span>

<span data-ttu-id="fe574-226">Laravel musí klíčem aplikace ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="fe574-226">Laravel needs an application key in App Service.</span></span> <span data-ttu-id="fe574-227">Můžete ho nakonfigurovat pomocí nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe574-227">You can configure it with app settings.</span></span>

<span data-ttu-id="fe574-228">Použití `php artisan` toogenerate nový klíč aplikace bez uložení too_.env_.</span><span class="sxs-lookup"><span data-stu-id="fe574-228">Use `php artisan` toogenerate a new application key without saving it too_.env_.</span></span>

```bash
php artisan key:generate --show
```

<span data-ttu-id="fe574-229">Nastavit klíč aplikace hello v hello služby App Service webovou aplikaci pomocí hello [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config/appsettings#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fe574-229">Set hello application key in hello App Service web app by using hello [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#set) command.</span></span> <span data-ttu-id="fe574-230">Nahraďte zástupné symboly hello  _&lt;appname >_ a  _&lt;outputofphpartisankey: generování >_.</span><span class="sxs-lookup"><span data-stu-id="fe574-230">Replace hello placeholders _&lt;appname>_ and _&lt;outputofphpartisankey:generate>_.</span></span>

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

<span data-ttu-id="fe574-231">`APP_DEBUG="true"`informuje, že Laravel tooreturn ladicí informace při nasazení webové aplikace hello výskyt chyb.</span><span class="sxs-lookup"><span data-stu-id="fe574-231">`APP_DEBUG="true"` tells Laravel tooreturn debugging information when hello deployed web app encounters errors.</span></span> <span data-ttu-id="fe574-232">Když spustíte produkční aplikace, nastavte ji příliš`false`, což je bezpečnější.</span><span class="sxs-lookup"><span data-stu-id="fe574-232">When running a production application, set it too`false`, which is more secure.</span></span>

### <a name="set-hello-virtual-application-path"></a><span data-ttu-id="fe574-233">Cesta virtuální aplikace sady hello</span><span class="sxs-lookup"><span data-stu-id="fe574-233">Set hello virtual application path</span></span>

<span data-ttu-id="fe574-234">Nastavit cestu virtuální aplikace hello pro hello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fe574-234">Set hello virtual application path for hello web app.</span></span> <span data-ttu-id="fe574-235">Tento krok je nezbytný, protože hello [životního cyklu aplikace Laravel](https://laravel.com/docs/5.4/lifecycle) začíná v hello _veřejné_ adresář místo kořenového adresáře aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-235">This step is required because hello [Laravel application lifecycle](https://laravel.com/docs/5.4/lifecycle) begins in hello _public_ directory instead of hello application's root directory.</span></span> <span data-ttu-id="fe574-236">Jiné architektury PHP, jejichž životního cyklu spuštění v kořenovém adresáři hello můžete pracovat bez ruční konfigurace hello cesta virtuální aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe574-236">Other PHP frameworks whose lifecycle start in hello root directory can work without manual configuration of hello virtual application path.</span></span>

<span data-ttu-id="fe574-237">Cesta virtuální aplikace sady hello pomocí hello [aktualizace prostředků az](/cli/azure/resource#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fe574-237">Set hello virtual application path by using hello [az resource update](/cli/azure/resource#update) command.</span></span> <span data-ttu-id="fe574-238">Nahraďte hello  _&lt;appname >_ zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="fe574-238">Replace hello _&lt;appname>_ placeholder.</span></span>

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

<span data-ttu-id="fe574-239">Ve výchozím nastavení, Azure App Service odkazuje cesta virtuální aplikace hello kořenové (_/_) toohello kořenového adresáře hello nasazené soubory aplikace (_sites\wwwroot_).</span><span class="sxs-lookup"><span data-stu-id="fe574-239">By default, Azure App Service points hello root virtual application path (_/_) toohello root directory of hello deployed application files (_sites\wwwroot_).</span></span>

### <a name="configure-a-deployment-user"></a><span data-ttu-id="fe574-240">Konfigurace uživatele nasazení</span><span class="sxs-lookup"><span data-stu-id="fe574-240">Configure a deployment user</span></span>

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="configure-local-git-deployment"></a><span data-ttu-id="fe574-241">Konfigurace nasazení z místního Gitu</span><span class="sxs-lookup"><span data-stu-id="fe574-241">Configure local Git deployment</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git-no-h.md)]

### <a name="push-tooazure-from-git"></a><span data-ttu-id="fe574-242">Z Git push tooAzure</span><span class="sxs-lookup"><span data-stu-id="fe574-242">Push tooAzure from Git</span></span>

<span data-ttu-id="fe574-243">Přidejte místní úložiště Git Azure vzdálené tooyour.</span><span class="sxs-lookup"><span data-stu-id="fe574-243">Add an Azure remote tooyour local Git repository.</span></span>

```bash
git remote add azure <paste_copied_url_here>
```

<span data-ttu-id="fe574-244">Push toohello Azure vzdálené toodeploy hello PHP aplikací.</span><span class="sxs-lookup"><span data-stu-id="fe574-244">Push toohello Azure remote toodeploy hello PHP application.</span></span> <span data-ttu-id="fe574-245">Zobrazí se výzva pro hello heslo, které jste zadali dříve v rámci vytváření hello hello nasazení uživatele.</span><span class="sxs-lookup"><span data-stu-id="fe574-245">You are prompted for hello password you supplied earlier as part of hello creation of hello deployment user.</span></span>

```bash
git push azure master
```

<span data-ttu-id="fe574-246">Během nasazení Azure App Service komunikuje s Gitem jejím průběhu.</span><span class="sxs-lookup"><span data-stu-id="fe574-246">During deployment, Azure App Service communicates its progress with Git.</span></span>

```bash
Counting objects: 3, done.
Delta compression using up too8 threads.
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
> <span data-ttu-id="fe574-247">Můžete si všimnout, že proces nasazení hello nainstaluje [autora](https://getcomposer.org/) balíčky na konci hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-247">You may notice that hello deployment process installs [Composer](https://getcomposer.org/) packages at hello end.</span></span> <span data-ttu-id="fe574-248">Aplikační služby nejde spustit tyto automatizaci během nasazení výchozí, takže toto úložiště ukázkové má tři další souborů v jeho tooenable kořenový adresář:</span><span class="sxs-lookup"><span data-stu-id="fe574-248">App Service does not run these automations during default deployment, so this sample repository has three additional files in its root directory tooenable it:</span></span>
>
> - <span data-ttu-id="fe574-249">`.deployment`– Tento soubor informuje služby App Service toorun `bash deploy.sh` jako hello vlastní nasazení skriptu.</span><span class="sxs-lookup"><span data-stu-id="fe574-249">`.deployment` - This file tells App Service toorun `bash deploy.sh` as hello custom deployment script.</span></span>
> - <span data-ttu-id="fe574-250">`deploy.sh`-hello vlastní nasazení skriptu.</span><span class="sxs-lookup"><span data-stu-id="fe574-250">`deploy.sh` - hello custom deployment script.</span></span> <span data-ttu-id="fe574-251">Při kontrole souboru hello je se zobrazí, že běží `php composer.phar install` po `npm install`.</span><span class="sxs-lookup"><span data-stu-id="fe574-251">If you review hello file, you will see that it runs `php composer.phar install` after `npm install`.</span></span>
> - <span data-ttu-id="fe574-252">`composer.phar`– Správce balíčků autora hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-252">`composer.phar` - hello Composer package manager.</span></span>
>
> <span data-ttu-id="fe574-253">Tento přístup tooadd můžete použít všechny krok tooyour nasazení na základě Git tooApp služby.</span><span class="sxs-lookup"><span data-stu-id="fe574-253">You can use this approach tooadd any step tooyour Git-based deployment tooApp Service.</span></span> <span data-ttu-id="fe574-254">Další informace najdete v tématu [vlastní skript nasazení](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).</span><span class="sxs-lookup"><span data-stu-id="fe574-254">For more information, see [Custom Deployment Script](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).</span></span>
>

### <a name="browse-toohello-azure-web-app"></a><span data-ttu-id="fe574-255">Procházet toohello webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="fe574-255">Browse toohello Azure web app</span></span>

<span data-ttu-id="fe574-256">Procházet příliš`http://<app_name>.azurewebsites.net` a přidání seznamu toohello několik úloh.</span><span class="sxs-lookup"><span data-stu-id="fe574-256">Browse too`http://<app_name>.azurewebsites.net` and add a few tasks toohello list.</span></span>

![Aplikace PHP, které jsou spuštěné v Azure App Service](./media/app-service-web-tutorial-php-mysql/php-mysql-in-azure.png)

<span data-ttu-id="fe574-258">Blahopřejeme, používáte PHP aplikace na základě dat ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="fe574-258">Congratulations, you're running a data-driven PHP app in Azure App Service.</span></span>

## <a name="update-model-locally-and-redeploy"></a><span data-ttu-id="fe574-259">Aktualizace modelu místně a znovu nasaďte</span><span class="sxs-lookup"><span data-stu-id="fe574-259">Update model locally and redeploy</span></span>

<span data-ttu-id="fe574-260">V tomto kroku provedete Jednoduchá změna toohello `task` dat modelu a hello webapp a potom publikovat tooAzure aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-260">In this step, you make a simple change toohello `task` data model and hello webapp, and then publish hello update tooAzure.</span></span>

<span data-ttu-id="fe574-261">Pro scénář hello úlohy upravit hello aplikace tak, že můžete označit úlohu jako dokončenou.</span><span class="sxs-lookup"><span data-stu-id="fe574-261">For hello tasks scenario, you modify hello application so that you can mark a task as complete.</span></span>

### <a name="add-a-column"></a><span data-ttu-id="fe574-262">Přidá sloupec</span><span class="sxs-lookup"><span data-stu-id="fe574-262">Add a column</span></span>

<span data-ttu-id="fe574-263">V terminálu hello přejděte toohello kořenovém hello úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="fe574-263">In hello terminal, navigate toohello root of hello Git repository.</span></span>

<span data-ttu-id="fe574-264">Generovat nové migrace databáze pro hello `tasks` tabulky:</span><span class="sxs-lookup"><span data-stu-id="fe574-264">Generate a new database migration for hello `tasks` table:</span></span>

```bash
php artisan make:migration add_complete_column --table=tasks
```

<span data-ttu-id="fe574-265">Tento příkaz zobrazí hello název souboru hello migrace, aby se vygenerovala.</span><span class="sxs-lookup"><span data-stu-id="fe574-265">This command shows you hello name of hello migration file that's generated.</span></span> <span data-ttu-id="fe574-266">Tento soubor v _databáze nebo migrace_ a otevřete ji.</span><span class="sxs-lookup"><span data-stu-id="fe574-266">Find this file in _database/migrations_ and open it.</span></span>

<span data-ttu-id="fe574-267">Nahraďte hello `up` metoda s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="fe574-267">Replace hello `up` method with hello following code:</span></span>

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

<span data-ttu-id="fe574-268">Hello předchozí kód přidá boolean sloupec v hello `tasks` tabulka s názvem `complete`.</span><span class="sxs-lookup"><span data-stu-id="fe574-268">hello preceding code adds a boolean column in hello `tasks` table called `complete`.</span></span>

<span data-ttu-id="fe574-269">Nahraďte hello `down` metoda s hello následující kód pro akce vrácení zpět hello:</span><span class="sxs-lookup"><span data-stu-id="fe574-269">Replace hello `down` method with hello following code for hello rollback action:</span></span>

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

<span data-ttu-id="fe574-270">V terminálu hello spusťte Laravel databáze migrace toomake hello změny v místní databázi hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-270">In hello terminal, run Laravel database migrations toomake hello change in hello local database.</span></span>

```bash
php artisan migrate
```

<span data-ttu-id="fe574-271">Podle hello [zásady vytváření názvů Laravel](https://laravel.com/docs/5.4/eloquent#defining-models), hello modelu `Task` (najdete v části _app/Task.php_) mapuje toohello `tasks` tabulky ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="fe574-271">Based on hello [Laravel naming convention](https://laravel.com/docs/5.4/eloquent#defining-models), hello model `Task` (see _app/Task.php_) maps toohello `tasks` table by default.</span></span>

### <a name="update-application-logic"></a><span data-ttu-id="fe574-272">Aktualizace aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="fe574-272">Update application logic</span></span>

<span data-ttu-id="fe574-273">Otevřete hello *routes/web.php* souboru.</span><span class="sxs-lookup"><span data-stu-id="fe574-273">Open hello *routes/web.php* file.</span></span> <span data-ttu-id="fe574-274">aplikace Hello definuje jeho trasy a obchodní logiku sem.</span><span class="sxs-lookup"><span data-stu-id="fe574-274">hello application defines its routes and business logic here.</span></span>

<span data-ttu-id="fe574-275">Na konci hello hello souboru přidejte trasu s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="fe574-275">At hello end of hello file, add a route with hello following code:</span></span>

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

<span data-ttu-id="fe574-276">Hello předchozí kód je jednoduchý aktualizace toohello datový model přepnutím hello hodnotu `complete`.</span><span class="sxs-lookup"><span data-stu-id="fe574-276">hello preceding code makes a simple update toohello data model by toggling hello value of `complete`.</span></span>

### <a name="update-hello-view"></a><span data-ttu-id="fe574-277">Aktualizace zobrazení hello</span><span class="sxs-lookup"><span data-stu-id="fe574-277">Update hello view</span></span>

<span data-ttu-id="fe574-278">Otevřete hello *resources/views/tasks.blade.php* souboru.</span><span class="sxs-lookup"><span data-stu-id="fe574-278">Open hello *resources/views/tasks.blade.php* file.</span></span> <span data-ttu-id="fe574-279">Najde hello `<tr>` počáteční značce a nahraďte ho:</span><span class="sxs-lookup"><span data-stu-id="fe574-279">Find hello `<tr>` opening tag and replace it with:</span></span>

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

<span data-ttu-id="fe574-280">Hello předcházející kód změny hello řádek barev v závislosti na tom, zda text hello úkol je dokončen.</span><span class="sxs-lookup"><span data-stu-id="fe574-280">hello preceding code changes hello row color depending on whether hello task is complete.</span></span>

<span data-ttu-id="fe574-281">V dalším řádku hello máte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="fe574-281">In hello next line, you have hello following code:</span></span>

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

<span data-ttu-id="fe574-282">Celý řádek hello nahraďte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="fe574-282">Replace hello entire line with hello following code:</span></span>

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

<span data-ttu-id="fe574-283">Hello předchozí kód přidá tlačítko hello odeslání, který odkazuje na hello trasy, která jste definovali dříve.</span><span class="sxs-lookup"><span data-stu-id="fe574-283">hello preceding code adds hello submit button that references hello route that you defined earlier.</span></span>

### <a name="test-hello-changes-locally"></a><span data-ttu-id="fe574-284">Test hello místně změny</span><span class="sxs-lookup"><span data-stu-id="fe574-284">Test hello changes locally</span></span>

<span data-ttu-id="fe574-285">Z kořenového adresáře hello hello úložiště Git spusťte hello vývojový server.</span><span class="sxs-lookup"><span data-stu-id="fe574-285">From hello root directory of hello Git repository, run hello development server.</span></span>

```bash
php artisan serve
```

<span data-ttu-id="fe574-286">toosee hello změna stavu úlohy, přejděte příliš`http://localhost:8000` a zaškrtněte políčko, vyberte hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-286">toosee hello task status change, navigate too`http://localhost:8000` and select hello checkbox.</span></span>

![Tootask přidané zaškrtávací políčko](./media/app-service-web-tutorial-php-mysql/complete-checkbox.png)

<span data-ttu-id="fe574-288">Zadejte toostop PHP, `Ctrl + C` v terminálu hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-288">toostop PHP, type `Ctrl + C` in hello terminal.</span></span>

### <a name="publish-changes-tooazure"></a><span data-ttu-id="fe574-289">Publikování změn tooAzure</span><span class="sxs-lookup"><span data-stu-id="fe574-289">Publish changes tooAzure</span></span>

<span data-ttu-id="fe574-290">V terminálu hello spusťte migrace databáze Laravel s hello produkční připojovací řetězec toomake hello změnu v hodnotě hello databáze Azure.</span><span class="sxs-lookup"><span data-stu-id="fe574-290">In hello terminal, run Laravel database migrations with hello production connection string toomake hello change in hello Azure database.</span></span>

```bash
php artisan migrate --env=production --force
```

<span data-ttu-id="fe574-291">Potvrďte všechny změny hello v úložišti Git a potom odešlete tooAzure změny kódu hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-291">Commit all hello changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

<span data-ttu-id="fe574-292">Jednou hello `git push` je dokončit, přejděte toohello službě Azure web app a testování hello nové funkce.</span><span class="sxs-lookup"><span data-stu-id="fe574-292">Once hello `git push` is complete, navigate toohello Azure web app and test hello new functionality.</span></span>

![Model a databáze změny publikovány tooAzure](media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

<span data-ttu-id="fe574-294">Pokud jste přidali všechny úlohy, jsou uchovány v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-294">If you added any tasks, they are retained in hello database.</span></span> <span data-ttu-id="fe574-295">Schéma dat toohello aktualizace ponechat stávající data beze změn.</span><span class="sxs-lookup"><span data-stu-id="fe574-295">Updates toohello data schema leave existing data intact.</span></span>

## <a name="stream-diagnostic-logs"></a><span data-ttu-id="fe574-296">Diagnostické protokoly datového proudu</span><span class="sxs-lookup"><span data-stu-id="fe574-296">Stream diagnostic logs</span></span>

<span data-ttu-id="fe574-297">V průběhu hello aplikace PHP ve službě Azure App Service můžete získat hello konzoly protokoly vytvoření kanálu tooyour terminálu.</span><span class="sxs-lookup"><span data-stu-id="fe574-297">While hello PHP application runs in Azure App Service, you can get hello console logs piped tooyour terminal.</span></span> <span data-ttu-id="fe574-298">Tímto způsobem můžete získat hello stejné diagnostické zprávy toohelp ladění chyb aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe574-298">That way, you can get hello same diagnostic messages toohelp you debug application errors.</span></span>

<span data-ttu-id="fe574-299">toostart protokolu streamování, použijte hello [az webapp protokolu poškozené databáze za](/cli/azure/webapp/log#tail) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fe574-299">toostart log streaming, use hello [az webapp log tail](/cli/azure/webapp/log#tail) command.</span></span>

```azurecli-interactive
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup
```

<span data-ttu-id="fe574-300">Po zahájení vysílání datového proudu protokolu, aktualizujte hello webové aplikace Azure v prohlížeči tooget hello některé webový provoz.</span><span class="sxs-lookup"><span data-stu-id="fe574-300">Once log streaming has started, refresh hello Azure web app in hello browser tooget some web traffic.</span></span> <span data-ttu-id="fe574-301">Nyní můžete vidět Terminálové vytvoření kanálu toohello protokoly konzoly.</span><span class="sxs-lookup"><span data-stu-id="fe574-301">You can now see console logs piped toohello terminal.</span></span> <span data-ttu-id="fe574-302">Pokud nevidíte protokoly konzoly okamžitě, kontrola znovu za 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="fe574-302">If you don't see console logs immediately, check again in 30 seconds.</span></span>

<span data-ttu-id="fe574-303">toostop protokolu streamování na kdykoli, typ `Ctrl` + `C`.</span><span class="sxs-lookup"><span data-stu-id="fe574-303">toostop log streaming at anytime, type `Ctrl`+`C`.</span></span>

> [!TIP]
> <span data-ttu-id="fe574-304">Aplikace PHP, můžete použít standardní hello [error_log()](http://php.net/manual/function.error-log.php) toooutput toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="fe574-304">A PHP application can use hello standard [error_log()](http://php.net/manual/function.error-log.php) toooutput toohello console.</span></span> <span data-ttu-id="fe574-305">Hello ukázková aplikace používá tuto metodu v _app/Http/routes.php_.</span><span class="sxs-lookup"><span data-stu-id="fe574-305">hello sample application uses this approach in _app/Http/routes.php_.</span></span>
>
> <span data-ttu-id="fe574-306">Jako webová architektura [Laravel používá Monolog](https://laravel.com/docs/5.4/errors) jako zprostředkovatel protokolování hello.</span><span class="sxs-lookup"><span data-stu-id="fe574-306">As a web framework, [Laravel uses Monolog](https://laravel.com/docs/5.4/errors) as hello logging provider.</span></span> <span data-ttu-id="fe574-307">toosee způsobu tooget Monolog toooutput zprávy toohello konzoly, najdete v [PHP: jak toouse monolog toolog tooconsole (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).</span><span class="sxs-lookup"><span data-stu-id="fe574-307">toosee how tooget Monolog toooutput messages toohello console, see [PHP: How toouse monolog toolog tooconsole (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).</span></span>
>
>

## <a name="manage-hello-azure-web-app"></a><span data-ttu-id="fe574-308">Spravovat hello webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="fe574-308">Manage hello Azure web app</span></span>

<span data-ttu-id="fe574-309">Přejděte toohello [portál Azure](https://portal.azure.com) toomanage hello webovou aplikaci jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="fe574-309">Go toohello [Azure portal](https://portal.azure.com) toomanage hello web app you created.</span></span>

<span data-ttu-id="fe574-310">V levé nabídce hello, klikněte na **App Services**a potom klikněte na název hello Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe574-310">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portálu tooAzure webové aplikace](./media/app-service-web-tutorial-php-mysql/access-portal.png)

<span data-ttu-id="fe574-312">Zobrazí se stránka s přehledem vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe574-312">You see your web app's Overview page.</span></span> <span data-ttu-id="fe574-313">Zde můžete provádět základní správu úkoly, jako je zastavení, spuštění, restartování, procházet a delete.</span><span class="sxs-lookup"><span data-stu-id="fe574-313">Here, you can perform basic management tasks like  stop, start, restart, browse, and delete.</span></span>

<span data-ttu-id="fe574-314">levé nabídce Hello poskytuje stránky pro konfiguraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe574-314">hello left menu provides pages for configuring your app.</span></span>

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-php-mysql/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="fe574-316">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe574-316">Next steps</span></span>

<span data-ttu-id="fe574-317">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="fe574-317">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fe574-318">Vytvoření databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="fe574-318">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="fe574-319">Připojit tooMySQL aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="fe574-319">Connect a PHP app tooMySQL</span></span>
> * <span data-ttu-id="fe574-320">Nasazení aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="fe574-320">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="fe574-321">Aktualizovat hello datový model a znovu nasaďte aplikace hello</span><span class="sxs-lookup"><span data-stu-id="fe574-321">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="fe574-322">Diagnostické protokoly datového proudu z Azure</span><span class="sxs-lookup"><span data-stu-id="fe574-322">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="fe574-323">Spravovat aplikace hello v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fe574-323">Manage hello app in hello Azure portal</span></span>

<span data-ttu-id="fe574-324">Posunutí toohello další kurz toolearn jak toomap vlastní DNS název tooa webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe574-324">Advance toohello next tutorial toolearn how toomap a custom DNS name tooa web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fe574-325">Mapovat existující vlastní DNS název tooAzure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="fe574-325">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
