---
title: aaaBuild webovou aplikaci Java a MySQL v Azure
description: "Zjistěte, jak tooget aplikace v jazyce Java, připojí se služba databáze Azure MySQL toohello práce v Azure App Service."
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
ms.openlocfilehash: 0820ee9c2b7bf8fcaa22287c27a7ab848a1c4927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-java-and-mysql-web-app-in-azure"></a>Vytvořit webovou aplikaci Java a MySQL v Azure

Tento kurz ukazuje, jak toocreate Java webové aplikace v Azure a připojte ho tooa databáze MySQL. Jakmile budete hotovi, budete mít [pružiny spouštěcí](https://projects.spring.io/spring-boot/) ukládání dat v aplikaci [Azure Database pro databázi MySQL](https://docs.microsoft.com/azure/mysql/overview) systémem [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview).

![Java aplikace spuštěné v Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření databáze MySQL v Azure
> * Připojit ukázkové aplikaci toohello databáze
> * Nasazení aplikace tooAzure hello
> * Aktualizace a znovu nasaďte aplikace hello
> * Diagnostické protokoly datového proudu z Azure
> * Monitorování aplikace hello v hello portálu Azure


## <a name="prerequisites"></a>Požadavky

1. [Stáhněte a nainstalujte Git](https://git-scm.com/)
1. [Stáhněte a nainstalujte hello Java 7 JDK nebo novější](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [Stáhnout, nainstalovat a spustit MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="prepare-local-mysql"></a>Příprava místního MySQL 

V tomto kroku vytvoříte databázi v místním serveru MySQL pro použití v testování aplikace hello místně na vašem počítači.

### <a name="connect-toomysql-server"></a>Připojení serveru tooMySQL

V okně terminálu připojte místní server MySQL tooyour. Všechny příkazy hello toto okno terminálu toorun můžete použít v tomto kurzu.

```bash
mysql -u root -p
```

Pokud se zobrazí výzva k zadání hesla, zadejte heslo hello hello `root` účtu. Pokud si nepamatujete heslo kořenového účtu, najdete v části [MySQL: jak tooReset hello kořenové heslo](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Pokud váš příkaz úspěšně proběhne, serveru databáze MySQL již spuštěna. Pokud ne, ujistěte se, že místní server MySQL se spustí následující hello [kroky po instalaci MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

### <a name="create-a-database"></a>Vytvoření databáze 

V hello `mysql` výzvu, vytvořte databázi a tabulku hello položkami seznamu úkolů.

```sql
CREATE DATABASE tododb;
```

Ukončení připojení k serveru zadáním `quit`.

```sql
quit
```

## <a name="create-and-run-hello-sample-app"></a>Vytvořte a spusťte ukázkové aplikace hello 

V tomto kroku naklonujte ukázkovou aplikaci spouštěcí pružiny, ho nakonfigurovat místní databázi MySQL toouse hello a spustit ve vašem počítači. 

### <a name="clone-hello-sample"></a>Ukázka hello klonování

V okně terminálu hello přejděte tooa práce adresáře a naklonujte úložiště ukázkové hello. 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-hello-app-toouse-hello-mysql-database"></a>Konfiguraci databáze MySQL hello toouse aplikace hello

Aktualizace hello `spring.datasource.password` a hodnota v *spring-boot-mysql-todo/src/main/resources/application.properties* s hello používá stejné kořenové heslo tooopen hello MySQL řádku:

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-hello-sample"></a>Sestavení a spuštění ukázkových hello

Sestavení a spuštění hello ukázku pomocí obálku Maven hello součástí hello úložiště:

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

Otevřete váš prohlížeč toohttp://localhost:8080 toosee v ukázce hello v akci. Při přidávání seznamu toohello úlohy, použijte následující příkaz SQL příkazů v hello MySQL výzva tooview hello data uložená v MySQL hello.

```SQL
use testdb;
select * from todo_item;
```

Zastavte aplikaci hello stiskne `Ctrl` + `C` v terminálu hello. 

## <a name="create-an-azure-mysql-database"></a>Vytvoření databáze MySQL na Azure

V tomto kroku vytvoříte [Azure Database pro databázi MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) instanci pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). Nakonfigurujete hello ukázkové aplikace toouse tuto databázi později v kurzu hello.

Hello použití Azure CLI 2.0 v okno terminálu toocreate hello prostředky potřebné toohost aplikace v jazyce Java v Azure App Service. Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů. 

```azurecli-interactive 
az login 
```   

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupinu prostředků Azure je logický kontejner, kde jsou související prostředky jako webové aplikace, databáze a účtů úložiště nasadit a spravovat. 

Hello následující příklad vytvoří skupinu prostředků v oblasti Severní Evropa hello:

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

toosee hello možných hodnot můžete použít pro `--location`, použijte hello [míst seznamu služby App Service az](/cli/azure/appservice#list-locations) příkaz.

### <a name="create-a-mysql-server"></a>Vytvoření databáze MySQL serveru

Vytvoření serveru ve službě Azure Database pro databázi MySQL (Preview) s hello [az mysql server vytvořit](/cli/azure/mysql/server#create) příkaz.    
Nahraďte vlastní jedinečný název serveru databáze MySQL, kde uvidíte hello `<mysql_server_name>` zástupný symbol. Tento název je součástí název hostitele serveru MySQL, `<mysql_server_name>.mysql.database.azure.com`, takže je nutné toobe globálně jedinečný. Také nahraďte `<admin_user>` a `<admin_password>` vlastními hodnotami.

```azurecli-interactive
az mysql server create --name <mysql_server_name> \ 
    --resource-group myResourceGroup \ 
    --location "North Europe" \
    --admin-user <admin_user> \ 
    --admin-password <admin_password>
```

Při vytvoření hello MySQL serveru je hello rozhraní příkazového řádku Azure obsahuje informace o podobné toohello následující ukázka:

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

### <a name="configure-server-firewall"></a>Konfigurace brány firewall serveru

Vytvořit pravidlo brány firewall pro klientské MySQL serveru tooallow připojení pomocí hello [az mysql pravidla brány firewall-vytvořit](/cli/azure/mysql/server/firewall-rule#create) příkaz. 

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name>  \ 
    --resource-group myResourceGroup \ 
    --start-ip-address 0.0.0.0 \ 
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure databáze pro databázi MySQL (Preview) automaticky aktuálně neumožňuje připojení ze služby Azure. Jak budou dynamicky přiřazovat IP adresy v Azure, je lepší tooenable všechny IP adresy pro teď. Služba hello stále jeho verze preview, lepší metody pro zabezpečení databáze bude povolena.

## <a name="configure-hello-azure-mysql-database"></a>Konfiguraci databáze Azure MySQL hello

V okně terminálu hello ve vašem počítači připojte server toohello MySQL v Azure. Použít hodnotu hello jste zadali dřív pro `<admin_user>` a `<mysql_server_name>`.

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a>Vytvoření databáze 

V hello `mysql` výzvu, vytvořte databázi a tabulku hello položkami seznamu úkolů.

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a>Vytvořit uživatele s oprávněními

Vytvoření uživatele databáze a pojmenujte ho všechna oprávnění v hello `tododb` databáze. Nahraďte zástupné symboly hello `<Javaapp_user>` a `<Javaapp_password>` s vlastními jedinečným názvem aplikace.

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* too'<Javaapp_user>';
```

Ukončení připojení k serveru zadáním `quit`.

```sql
quit
```

## <a name="deploy-hello-sample-tooazure-app-service"></a>Nasazení ukázkové tooAzure hello služby App Service

Vytvořte plán aplikační služby Azure s hello **volné** cenová úroveň pomocí hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) rozhraní příkazového řádku příkaz. plán aplikační služby Hello definuje toohost hello fyzické prostředky, které používá vaše aplikace. Všechny aplikace, které jsou přiřazené plán aplikační služby tooan sdílení těchto prostředků, umožní vám toosave nákladů při hostování více aplikací. 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

Při plánování hello je připraven, výstup hello rozhraní příkazového řádku Azure zobrazuje podobné toohello následující ukázka:

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

### <a name="create-an-azure-web-app"></a>Vytvoření webové aplikace Azure

 Použití hello [az webapp vytvořit](/cli/azure/appservice/web#create) rozhraní příkazového řádku příkaz toocreate definici webové aplikace v hello `myAppServicePlan` plán služby App Service. definice Hello webové aplikace poskytuje tooaccess adresa URL vaší aplikace pomocí a konfiguruje několik možností toodeploy tooAzure vašeho kódu. 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

SUBSTITUTE hello `<app_name>` zástupný symbol vlastní jedinečným názvem aplikace. Tento jedinečný název je součástí hello výchozí název domény pro webovou aplikaci hello, takže název hello musí toobe jedinečný mezi všechny aplikace v Azure. Můžete namapovat vlastní doménu název položky toohello webové aplikace ještě před zveřejněním tooyour uživatele.

Při definici hello webové aplikace je připraven, hello rozhraní příkazového řádku Azure ukazuje následující příklad podobné toohello informace: 

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

### <a name="configure-java"></a>Konfigurace Java 

Nastavení konfigurace hello Java runtime, která vaše aplikace, musí se hello [aktualizace konfigurace webové služby App Service az](/cli/azure/appservice/web/config#update) příkaz.

Hello následující příkaz nakonfiguruje hello webové aplikace toorun na poslední JDK 8 Java a [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

### <a name="configure-hello-app-toouse-hello-azure-sql-database"></a>Konfigurace hello aplikace toouse hello Azure SQL database

Před spuštěním hello ukázkové aplikace, nastavení aplikací na hello webové aplikace toouse hello Azure databáze MySQL, kterou jste vytvořili v Azure. Tyto vlastnosti jsou zveřejněné toohello webové aplikace jako proměnné prostředí a přepsat hello hodnotami nastavenými v hello application.properties uvnitř hello zabalené webové aplikace. 

Nastavení aplikace pomocí [az webapp konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) v hello rozhraní příkazového řádku:

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

### <a name="get-ftp-deployment-credentials"></a>Získat přihlašovací údaje pro nasazení serveru FTP 
Můžete nasadit vaše aplikace tooAzure služby App Service různými způsoby, včetně FTP, místní Git, GitHub, Visual Studio Team Services a BitBucket. V tomto příkladu FTP toodeploy hello. Soubor WAR dříve vytvořené na vašem místním počítači tooAzure služby App Service.

toodetermine co pověření toopass společně v ftp příkaz toohello webové aplikace, použijte [az služby App Service web nasazení seznamu publikování profily](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) příkaz: 

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

### <a name="upload-hello-app-using-ftp"></a>Nahrání aplikace hello pomocí protokolu FTP

Použijte váš oblíbený hello toodeploy nástroj FTP. Toohello soubor WAR */site/wwwroot/webapps* složky na adresu serveru hello převzaty z hello `URL` v předchozí příkaz hello. Odeberte hello existující adresář aplikace výchozí (uživatel ROOT) a nahraďte hello existující ROOT.war s hello. Soubor WAR součástí hello v kurzu hello.

```bash
ftp waws-prod-blu-069.ftp.azurewebsites.windows.net
Connected toowaws-prod-blu-069.drip.azurewebsites.windows.net.
220 Microsoft FTP Service
Name (waws-prod-blu-069.ftp.azurewebsites.windows.net:raisa): app_name\$app_name
331 Password required
Password:
cd /site/wwwroot/webapps
mdelete -i ROOT/*
rmdir ROOT/
put target/TodoDemo-0.0.1-SNAPSHOT.war ROOT.war
```

### <a name="test-hello-web-app"></a>Testování hello webové aplikace

Procházet příliš`http://<app_name>.azurewebsites.net/` a přidání seznamu toohello několik úloh. 

![Java aplikace spuštěné v Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

**Blahopřejeme!** Používáte datové aplikace v jazyce Java v Azure App Service.

## <a name="update-hello-app-and-redeploy"></a>Aktualizace aplikace hello a znovu ho zaveďte

Aktualizujte tooinclude aplikace hello sloupec v seznamu úkolů hello, pro jaké den hello položka byla vytvořena. Spouštěcí pružiny zpracovává aktualizace schématu databáze hello můžete jako změn datových modelů hello beze změny stávajících záznamů databáze.

1. V lokálním systému, otevře *src/main/java/com/example/fabrikam/TodoItem.java* a přidejte následující hello importuje toohello třídy:   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. Přidat `String` vlastnost `timeCreated` příliš*src/main/java/com/example/fabrikam/TodoItem.java*, inicializace s časovým razítkem na vytvoření objektu. Přidat mechanismy získání nebo nastavení pro nové hello `timeCreated` vlastnost během úprav tohoto souboru.

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

3. Aktualizace *src/main/java/com/example/fabrikam/TodoDemoController.java* řádek v hello `updateTodo` metoda tooset hello časové razítko:

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. Přidáte podporu pro nové pole hello v šabloně Thymeleaf hello. Aktualizace *src/main/resources/templates/index.html* s novou hlavičkou tabulky pro časové razítko hello a nová hodnota toodisplay hello pole hello časového razítka v každém řádku dat tabulky.

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

5. Opětovné sestavení aplikace hello:

    ```bash
    mvnw clean package 
    ```

6. Aktualizovat hello FTP. WAR jako před, odebrání stávající hello *lokality/wwwroot/webapps/ROOT* adresáře a *ROOT.war*, pak odesílání hello aktualizovat. Soubor WAR jako ROOT.war. 

Při aktualizaci aplikace hello **čas vytvoření** sloupec je nyní viditelné. Když přidáte novou úlohu, aplikace hello automaticky naplnit hello časové razítko. Vaše stávající úlohy zůstat beze změny a pracovat s aplikací hello to i v případě, že došlo ke změně hello základní datový model. 

![Aplikace v jazyce Java aktualizovat pomocí nového sloupce](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a>Diagnostické protokoly datového proudu 

Při spuštění aplikace v jazyce Java v Azure App Service, můžete získat hello konzoly protokoly přesměruje přímo tooyour terminálu. Tímto způsobem můžete získat hello stejné diagnostické zprávy toohelp ladění chyb aplikace.

toostart protokolu streamování, použijte hello [az webapp protokolu poškozené databáze za](/cli/azure/appservice/web/log#tail) příkaz.

```azurecli-interactive 
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a>Správa Azure webové aplikace

Přejděte toohello Azure portálu toosee hello webovou aplikaci, kterou jste vytvořili.

toodo, přihlaste se příliš[https://portal.azure.com](https://portal.azure.com).

V levé nabídce hello, klikněte na **služby App Service**, pak klikněte na název hello Azure webové aplikace.

![Portálu tooAzure webové aplikace](./media/app-service-web-tutorial-java-mysql/access-portal.png)

Ve výchozím nastavení, zobrazí okno vaší webové aplikace hello **přehled** stránky. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Zde můžete také provést úlohy správy, jako zastavení, spuštění, restartování a delete. Hello karty na levé straně okna hello hello zobrazit stránky hello jinou konfiguraci, které můžete otevřít.

![Okno App Service na webu Azure Portal](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

Tyto karty v okně hello zobrazit hello mnoho funkcí můžete přidat tooyour webové aplikace. Hello následující seznam vám poskytuje několik možností, jak hello:
* Mapování vlastního názvu DNS
* Vazba vlastního certifikátu SSL
* Konfigurace průběžného nasazování
* Vertikální i horizontální navýšení kapacity
* Přidání ověřování uživatelů

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud nepotřebujete tyto prostředky pro jiné kurzu (najdete v části [další kroky](#next)), můžete je odstranit spuštěním hello následující příkaz: 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a>Další kroky

> [!div class="checklist"]
> * Vytvoření databáze MySQL v Azure
> * Připojit toohello aplikace Java ukázkové databáze MySQL
> * Nasazení aplikace tooAzure hello
> * Aktualizace a znovu nasaďte aplikace hello
> * Diagnostické protokoly datového proudu z Azure
> * Spravovat aplikace hello v hello portálu Azure

Posunutí toohello další kurz toolearn jak toomap vlastní DNS název toohello aplikace.

> [!div class="nextstepaction"] 
> [Mapovat existující vlastní DNS název tooAzure webové aplikace](app-service-web-tutorial-custom-domain.md)
