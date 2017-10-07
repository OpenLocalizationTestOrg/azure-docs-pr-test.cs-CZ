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
# <a name="build-a-php-and-mysql-web-app-in-azure"></a>Vytvoření webové aplikace PHP a databáze MySQL v Azure

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů. Tento kurz ukazuje, jak toocreate PHP webové aplikace v Azure a připojte ho tooa databáze MySQL. Jakmile budete hotovi, budete mít [Laravel](https://laravel.com/) aplikaci spuštěnou na Azure App Service Web Apps.

![Aplikace PHP, které jsou spuštěné v Azure App Service](./media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření databáze MySQL v Azure
> * Připojit tooMySQL aplikace PHP
> * Nasazení aplikace tooAzure hello
> * Aktualizovat hello datový model a znovu nasaďte aplikace hello
> * Diagnostické protokoly datového proudu z Azure
> * Spravovat aplikace hello v hello portálu Azure

## <a name="prerequisites"></a>Požadavky

toocomplete v tomto kurzu:

* [Nainstalovat Git](https://git-scm.com/).
* [Instalace PHP 5.6.4 nebo novější](http://php.net/downloads.php)
* [Nainstalujte autora](https://getcomposer.org/doc/00-intro.md)
* Povolit následující rozšíření PHP Laravel potřebám hello: OpenSSL, PDO MySQL, Mbstring, Tokenizátor, XML
* [Instalace a spuštění MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Příprava místního MySQL

V tomto kroku vytvoříte databázi v místní server MySQL pro použití v tomto kurzu.

### <a name="connect-toolocal-mysql-server"></a>Připojení serveru toolocal MySQL

V okně terminálu připojte místní server MySQL tooyour. Všechny příkazy hello toto okno terminálu toorun můžete použít v tomto kurzu.

```bash
mysql -u root -p
```

Pokud se zobrazí výzva k zadání hesla, zadejte heslo hello hello `root` účtu. Pokud si nepamatujete heslo kořenového účtu, najdete v části [MySQL: jak tooReset hello kořenové heslo](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Pokud váš příkaz úspěšně proběhne, MySQL serveru běží. Pokud ne, ujistěte se, že místní server MySQL se spustí následující hello [kroky po instalaci MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

### <a name="create-a-database-locally"></a>Vytvoření databáze místně

V hello `mysql` výzvu, vytvořit databázi.

```sql 
CREATE DATABASE sampledb;
```

Ukončení připojení k serveru zadáním `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a>Vytvoření aplikace PHP místně
V tomto kroku získat ukázkovou aplikaci Laravel, konfigurovat jeho připojení k databázi a spustit místně. 

### <a name="clone-hello-sample"></a>Ukázka hello klonování

V okně terminálu hello `cd` tooa pracovní adresář.

Spusťte následující příkaz tooclone hello Ukázka úložiště hello.

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

`cd`tooyour klonovaný adresář.
Instalovat hello požadované balíčky.

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a>Konfigurace připojení databáze MySQL

V kořenovém úložišti hello, vytvořte soubor s názvem *.env*. Kopírování hello následující proměnné do hello *.env* souboru. Nahraďte hello  _&lt;root_password >_ zástupný symbol hello MySQL kořenové heslo uživatele.

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

Informace o tom, jak Laravel používá hello _.env_ souborů najdete v tématu [konfigurace prostředí Laravel](https://laravel.com/docs/5.4/configuration#environment-configuration).

### <a name="run-hello-sample-locally"></a>Hello ukázku spustit místně

Spustit [migrace databáze Laravel](https://laravel.com/docs/5.4/migrations) toocreate hello tabulky potřebám aplikace hello. toosee tabulky, které jsou vytvořené v hello migrace, vyhledejte v hello _databáze nebo migrace_ adresáře v úložišti Git hello.

```bash
php artisan migrate
```

Vygenerujte nový klíč Laravel aplikace.

```bash
php artisan key:generate
```

Spusťte aplikaci hello.

```bash
php artisan serve
```

Přejděte příliš`http://localhost:8000` v prohlížeči. Přidáte několik úloh na stránce hello.

![PHP připojí úspěšně tooMySQL](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

Zadejte toostop PHP, `Ctrl + C` v terminálu hello.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>Vytvoření databáze MySQL v Azure

V tomto kroku vytvoříte databázi MySQL v [Azure Database pro databázi MySQL (Preview)](/azure/mysql). Později nakonfigurujte hello PHP aplikace tooconnect toothis databáze.

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>Vytvoření databáze MySQL serveru

Vytvoření serveru ve službě Azure Database pro databázi MySQL (Preview) s hello [az mysql server vytvořit](/cli/azure/mysql/server#create) příkaz.

V hello následující příkaz, nahraďte název serveru MySQL, kde uvidíte hello  _&lt;mysql_server_name >_ zástupný symbol (platnými znaky jsou `a-z`, `0-9`, a `-`). Tento název je součástí názvu hostitele serveru MySQL hello (`<mysql_server_name>.database.windows.net`), je nutné toobe globálně jedinečný.

```azurecli-interactive
az mysql server create \
    --name <mysql_server_name> \
    --resource-group myResourceGroup \
    --location "North Europe" \
    --admin-user adminuser \
    --admin-password MySQLAzure2017
```

Při vytvoření hello MySQL serveru je hello rozhraní příkazového řádku Azure obsahuje informace o podobné toohello následující ukázka:

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

### <a name="configure-server-firewall"></a>Konfigurace brány firewall serveru

Vytvořit pravidlo brány firewall pro klientské MySQL serveru tooallow připojení pomocí hello [az mysql pravidla brány firewall-vytvořit](/cli/azure/mysql/server/firewall-rule#create) příkaz.

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name> \
    --resource-group myResourceGroup \
    --start-ip-address 0.0.0.0 \
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure databáze pro databázi MySQL (Preview) nepodporuje omezit aktuálně připojení pouze tooAzure služby. Jak budou dynamicky přiřazovat IP adresy v Azure, je lepší tooenable všechny IP adresy. Služba Hello je ve verzi preview. Plánujeme lepší metody pro zabezpečení databáze.
>
>

### <a name="connect-tooproduction-mysql-server-locally"></a>Připojení tooproduction MySQL místně serveru

V okně terminálu hello Připojte server toohello MySQL v Azure. Použít hodnotu hello jste zadali dřív pro  _&lt;mysql_server_name >_.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

Pokud budete vyzváni k zadání hesla, použijte _$tr0ngPa$ w0rd!_, který jste zadali při vytváření databáze hello.

### <a name="create-a-production-database"></a>Vytvoření provozní databáze

V hello `mysql` výzvu, vytvořit databázi.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Vytvořit uživatele s oprávněními

Vytvořte uživatele databáze názvem _phpappuser_ a pojmenujte ho všechna oprávnění v hello `sampledb` databáze.

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* too'phpappuser';
```

Ukončení připojení serveru hello zadáním `quit`.

```sql
quit
```

## <a name="connect-app-tooazure-mysql"></a>Připojení aplikace tooAzure MySQL

V tomto kroku připojíte hello PHP aplikace toohello databáze MySQL, kterou jste vytvořili ve službě Azure Database pro databázi MySQL (Preview).

<a name="devconfig"></a>

### <a name="configure-hello-database-connection"></a>Konfigurace připojení k databázi hello

V kořenovém úložišti hello, vytvořte _. env.production_ hello soubor a zkopírujte do něj následující proměnné. Nahraďte zástupný symbol hello  _&lt;mysql_server_name >_.

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

Uložte změny hello.

> [!TIP]
> toosecure MySQL informace o připojení, tento soubor je již vyloučen z úložiště Git hello (viz _.gitignore_ v kořenovém úložišti hello). Později zjistíte, jak tooconfigure proměnné prostředí v App Service tooconnect tooyour databáze v databázi Azure pro databázi MySQL (Preview). Proměnné prostředí, nemusíte hello *.env* souboru ve službě App Service.
>

### <a name="configure-ssl-certificate"></a>Nakonfigurujte certifikát protokolu SSL

Ve výchozím nastavení vynucuje Azure Database pro databázi MySQL připojení SSL od klientů. tooconnect tooyour databáze MySQL v Azure, je nutné použít _.pem_ certifikát SSL.

Otevřete _config/database.php_ a přidejte hello _sslmode_ a _možnosti_ parametry příliš`connections.mysql`, jak je znázorněno v následujícím kódu hello.

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/certificate.pem', 
    ] : []
],
```

toolearn jak toogenerate to _certificate.pem_, najdete v části [připojení konfigurace protokolu SSL ve vaší aplikaci toosecurely connect tooAzure databáze pro databázi MySQL](../mysql/howto-configure-ssl.md).

> [!TIP]
> Cesta Hello _/ssl/certificate.pem_ body existující tooan _certificate.pem_ souboru v úložišti Git hello. Tento soubor je poskytován pro usnadnění práce v tomto kurzu. Pro nejlepší postup by neměl potvrdit vaše _.pem_ certifikáty do správy zdrojového kódu. 
>

### <a name="test-hello-application-locally"></a>Testování aplikace hello místně

Spustit migrace databáze Laravel s _. env.production_ jako hello prostředí soubor toocreate hello tabulek v databázi MySQL v Azure Database pro databázi MySQL (Preview). Nezapomeňte, že _. env.production_ má databáze MySQL tooyour informace o připojení hello v Azure.

```bash
php artisan migrate --env=production --force
```

_. env.production_ ještě nemá platnou aplikaci klíč. Vygenerujte nový token pro něj v terminálu hello.

```bash
php artisan key:generate --env=production --force
```

Spuštění ukázkové aplikace hello s _. env.production_ jako soubor hello prostředí.

```bash
php artisan serve --env=production
```

Přejděte příliš`http://localhost:8000`. Pokud hello stránka načte bez chyb, hello aplikace PHP se připojuje toohello databáze MySQL v Azure.

Přidáte několik úloh na stránce hello.

![PHP připojí úspěšně tooAzure databáze pro databázi MySQL (Preview)](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

Zadejte toostop PHP, `Ctrl + C` v terminálu hello.

### <a name="commit-your-changes"></a>Potvrdit změny

Spusťte následující příkazy toocommit Git hello změny:

```bash
git add .
git commit -m "database.php updates"
```

Aplikace je připravená toobe nasazení.

## <a name="deploy-tooazure"></a>Nasazení tooAzure

V tomto kroku nasadíte hello připojené MySQL PHP aplikace tooAzure služby App Service.

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

[!INCLUDE [Create app service plan no h](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a>Vytvoření webové aplikace

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-no-h.md)]

### <a name="set-hello-php-version"></a>Nastavit verzi PHP hello

Nastavit hello PHP verzi, která hello aplikace vyžaduje pomocí hello [az webapp konfigurace sady](/cli/azure/webapp/config#set) příkaz.

Hello následující příkaz nastaví too_7.0_ verze PHP hello.

```azurecli-interactive
az webapp config set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --php-version 7.0
```

### <a name="configure-database-settings"></a>Konfigurace nastavení databáze

Jak bylo uvedeno dříve, můžete se připojit databáze Azure MySQL tooyour použití proměnných prostředí ve službě App Service.

Ve službě App Service, můžete nastavit proměnné prostředí jako _nastavení aplikace_ pomocí hello [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config/appsettings#set) příkaz.

Hello následující příkaz nakonfiguruje nastavení aplikace hello `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, a `DB_PASSWORD`. Nahraďte zástupné symboly hello  _&lt;appname >_ a  _&lt;mysql_server_name >_.

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017" MYSQL_SSL="true"
```

Můžete použít hello PHP [GETENV –](http://www.php.net/manual/function.getenv.php) metoda tooaccess hello nastavení. Hello Laravel kód používá [env](https://laravel.com/docs/5.4/helpers#method-env) obálku přes hello PHP `getenv`. Například hello MySQL konfigurace v _config/database.php_ vypadá hello následující kód:

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

### <a name="configure-laravel-environment-variables"></a>Konfigurace Laravel proměnné prostředí

Laravel musí klíčem aplikace ve službě App Service. Můžete ho nakonfigurovat pomocí nastavení aplikace.

Použití `php artisan` toogenerate nový klíč aplikace bez uložení too_.env_.

```bash
php artisan key:generate --show
```

Nastavit klíč aplikace hello v hello služby App Service webovou aplikaci pomocí hello [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config/appsettings#set) příkaz. Nahraďte zástupné symboly hello  _&lt;appname >_ a  _&lt;outputofphpartisankey: generování >_.

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

`APP_DEBUG="true"`informuje, že Laravel tooreturn ladicí informace při nasazení webové aplikace hello výskyt chyb. Když spustíte produkční aplikace, nastavte ji příliš`false`, což je bezpečnější.

### <a name="set-hello-virtual-application-path"></a>Cesta virtuální aplikace sady hello

Nastavit cestu virtuální aplikace hello pro hello webovou aplikaci. Tento krok je nezbytný, protože hello [životního cyklu aplikace Laravel](https://laravel.com/docs/5.4/lifecycle) začíná v hello _veřejné_ adresář místo kořenového adresáře aplikace hello. Jiné architektury PHP, jejichž životního cyklu spuštění v kořenovém adresáři hello můžete pracovat bez ruční konfigurace hello cesta virtuální aplikace.

Cesta virtuální aplikace sady hello pomocí hello [aktualizace prostředků az](/cli/azure/resource#update) příkaz. Nahraďte hello  _&lt;appname >_ zástupný symbol.

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

Ve výchozím nastavení, Azure App Service odkazuje cesta virtuální aplikace hello kořenové (_/_) toohello kořenového adresáře hello nasazené soubory aplikace (_sites\wwwroot_).

### <a name="configure-a-deployment-user"></a>Konfigurace uživatele nasazení

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="configure-local-git-deployment"></a>Konfigurace nasazení z místního Gitu

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git-no-h.md)]

### <a name="push-tooazure-from-git"></a>Z Git push tooAzure

Přidejte místní úložiště Git Azure vzdálené tooyour.

```bash
git remote add azure <paste_copied_url_here>
```

Push toohello Azure vzdálené toodeploy hello PHP aplikací. Zobrazí se výzva pro hello heslo, které jste zadali dříve v rámci vytváření hello hello nasazení uživatele.

```bash
git push azure master
```

Během nasazení Azure App Service komunikuje s Gitem jejím průběhu.

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
> Můžete si všimnout, že proces nasazení hello nainstaluje [autora](https://getcomposer.org/) balíčky na konci hello. Aplikační služby nejde spustit tyto automatizaci během nasazení výchozí, takže toto úložiště ukázkové má tři další souborů v jeho tooenable kořenový adresář:
>
> - `.deployment`– Tento soubor informuje služby App Service toorun `bash deploy.sh` jako hello vlastní nasazení skriptu.
> - `deploy.sh`-hello vlastní nasazení skriptu. Při kontrole souboru hello je se zobrazí, že běží `php composer.phar install` po `npm install`.
> - `composer.phar`– Správce balíčků autora hello.
>
> Tento přístup tooadd můžete použít všechny krok tooyour nasazení na základě Git tooApp služby. Další informace najdete v tématu [vlastní skript nasazení](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).
>

### <a name="browse-toohello-azure-web-app"></a>Procházet toohello webové aplikace Azure

Procházet příliš`http://<app_name>.azurewebsites.net` a přidání seznamu toohello několik úloh.

![Aplikace PHP, které jsou spuštěné v Azure App Service](./media/app-service-web-tutorial-php-mysql/php-mysql-in-azure.png)

Blahopřejeme, používáte PHP aplikace na základě dat ve službě Azure App Service.

## <a name="update-model-locally-and-redeploy"></a>Aktualizace modelu místně a znovu nasaďte

V tomto kroku provedete Jednoduchá změna toohello `task` dat modelu a hello webapp a potom publikovat tooAzure aktualizace hello.

Pro scénář hello úlohy upravit hello aplikace tak, že můžete označit úlohu jako dokončenou.

### <a name="add-a-column"></a>Přidá sloupec

V terminálu hello přejděte toohello kořenovém hello úložiště Git.

Generovat nové migrace databáze pro hello `tasks` tabulky:

```bash
php artisan make:migration add_complete_column --table=tasks
```

Tento příkaz zobrazí hello název souboru hello migrace, aby se vygenerovala. Tento soubor v _databáze nebo migrace_ a otevřete ji.

Nahraďte hello `up` metoda s hello následující kód:

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

Hello předchozí kód přidá boolean sloupec v hello `tasks` tabulka s názvem `complete`.

Nahraďte hello `down` metoda s hello následující kód pro akce vrácení zpět hello:

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

V terminálu hello spusťte Laravel databáze migrace toomake hello změny v místní databázi hello.

```bash
php artisan migrate
```

Podle hello [zásady vytváření názvů Laravel](https://laravel.com/docs/5.4/eloquent#defining-models), hello modelu `Task` (najdete v části _app/Task.php_) mapuje toohello `tasks` tabulky ve výchozím nastavení.

### <a name="update-application-logic"></a>Aktualizace aplikace logiky

Otevřete hello *routes/web.php* souboru. aplikace Hello definuje jeho trasy a obchodní logiku sem.

Na konci hello hello souboru přidejte trasu s hello následující kód:

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

Hello předchozí kód je jednoduchý aktualizace toohello datový model přepnutím hello hodnotu `complete`.

### <a name="update-hello-view"></a>Aktualizace zobrazení hello

Otevřete hello *resources/views/tasks.blade.php* souboru. Najde hello `<tr>` počáteční značce a nahraďte ho:

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

Hello předcházející kód změny hello řádek barev v závislosti na tom, zda text hello úkol je dokončen.

V dalším řádku hello máte hello následující kód:

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

Celý řádek hello nahraďte hello následující kód:

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

Hello předchozí kód přidá tlačítko hello odeslání, který odkazuje na hello trasy, která jste definovali dříve.

### <a name="test-hello-changes-locally"></a>Test hello místně změny

Z kořenového adresáře hello hello úložiště Git spusťte hello vývojový server.

```bash
php artisan serve
```

toosee hello změna stavu úlohy, přejděte příliš`http://localhost:8000` a zaškrtněte políčko, vyberte hello.

![Tootask přidané zaškrtávací políčko](./media/app-service-web-tutorial-php-mysql/complete-checkbox.png)

Zadejte toostop PHP, `Ctrl + C` v terminálu hello.

### <a name="publish-changes-tooazure"></a>Publikování změn tooAzure

V terminálu hello spusťte migrace databáze Laravel s hello produkční připojovací řetězec toomake hello změnu v hodnotě hello databáze Azure.

```bash
php artisan migrate --env=production --force
```

Potvrďte všechny změny hello v úložišti Git a potom odešlete tooAzure změny kódu hello.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Jednou hello `git push` je dokončit, přejděte toohello službě Azure web app a testování hello nové funkce.

![Model a databáze změny publikovány tooAzure](media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

Pokud jste přidali všechny úlohy, jsou uchovány v databázi hello. Schéma dat toohello aktualizace ponechat stávající data beze změn.

## <a name="stream-diagnostic-logs"></a>Diagnostické protokoly datového proudu

V průběhu hello aplikace PHP ve službě Azure App Service můžete získat hello konzoly protokoly vytvoření kanálu tooyour terminálu. Tímto způsobem můžete získat hello stejné diagnostické zprávy toohelp ladění chyb aplikace.

toostart protokolu streamování, použijte hello [az webapp protokolu poškozené databáze za](/cli/azure/webapp/log#tail) příkaz.

```azurecli-interactive
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup
```

Po zahájení vysílání datového proudu protokolu, aktualizujte hello webové aplikace Azure v prohlížeči tooget hello některé webový provoz. Nyní můžete vidět Terminálové vytvoření kanálu toohello protokoly konzoly. Pokud nevidíte protokoly konzoly okamžitě, kontrola znovu za 30 sekund.

toostop protokolu streamování na kdykoli, typ `Ctrl` + `C`.

> [!TIP]
> Aplikace PHP, můžete použít standardní hello [error_log()](http://php.net/manual/function.error-log.php) toooutput toohello konzoly. Hello ukázková aplikace používá tuto metodu v _app/Http/routes.php_.
>
> Jako webová architektura [Laravel používá Monolog](https://laravel.com/docs/5.4/errors) jako zprostředkovatel protokolování hello. toosee způsobu tooget Monolog toooutput zprávy toohello konzoly, najdete v [PHP: jak toouse monolog toolog tooconsole (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).
>
>

## <a name="manage-hello-azure-web-app"></a>Spravovat hello webové aplikace Azure

Přejděte toohello [portál Azure](https://portal.azure.com) toomanage hello webovou aplikaci jste vytvořili.

V levé nabídce hello, klikněte na **App Services**a potom klikněte na název hello Azure webové aplikace.

![Portálu tooAzure webové aplikace](./media/app-service-web-tutorial-php-mysql/access-portal.png)

Zobrazí se stránka s přehledem vaší webové aplikace. Zde můžete provádět základní správu úkoly, jako je zastavení, spuštění, restartování, procházet a delete.

levé nabídce Hello poskytuje stránky pro konfiguraci vaší aplikace.

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-php-mysql/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření databáze MySQL v Azure
> * Připojit tooMySQL aplikace PHP
> * Nasazení aplikace tooAzure hello
> * Aktualizovat hello datový model a znovu nasaďte aplikace hello
> * Diagnostické protokoly datového proudu z Azure
> * Spravovat aplikace hello v hello portálu Azure

Posunutí toohello další kurz toolearn jak toomap vlastní DNS název tooa webové aplikace.

> [!div class="nextstepaction"]
> [Mapovat existující vlastní DNS název tooAzure webové aplikace](app-service-web-tutorial-custom-domain.md)
