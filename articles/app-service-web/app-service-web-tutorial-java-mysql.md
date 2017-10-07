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
# <a name="build-a-java-and-mysql-web-app-in-azure"></a><span data-ttu-id="041a4-103">Vytvořit webovou aplikaci Java a MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="041a4-103">Build a Java and MySQL web app in Azure</span></span>

<span data-ttu-id="041a4-104">Tento kurz ukazuje, jak toocreate Java webové aplikace v Azure a připojte ho tooa databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="041a4-104">This tutorial shows you how toocreate a Java web app in Azure and connect it tooa MySQL database.</span></span> <span data-ttu-id="041a4-105">Jakmile budete hotovi, budete mít [pružiny spouštěcí](https://projects.spring.io/spring-boot/) ukládání dat v aplikaci [Azure Database pro databázi MySQL](https://docs.microsoft.com/azure/mysql/overview) systémem [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview).</span><span class="sxs-lookup"><span data-stu-id="041a4-105">When you are finished, you will have a [Spring Boot](https://projects.spring.io/spring-boot/) application storing data in [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/overview) running on [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview).</span></span>

![Java aplikace spuštěné v Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

<span data-ttu-id="041a4-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="041a4-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="041a4-108">Vytvoření databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="041a4-108">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="041a4-109">Připojit ukázkové aplikaci toohello databáze</span><span class="sxs-lookup"><span data-stu-id="041a4-109">Connect a sample app toohello database</span></span>
> * <span data-ttu-id="041a4-110">Nasazení aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="041a4-110">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="041a4-111">Aktualizace a znovu nasaďte aplikace hello</span><span class="sxs-lookup"><span data-stu-id="041a4-111">Update and redeploy hello app</span></span>
> * <span data-ttu-id="041a4-112">Diagnostické protokoly datového proudu z Azure</span><span class="sxs-lookup"><span data-stu-id="041a4-112">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="041a4-113">Monitorování aplikace hello v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="041a4-113">Monitor hello app in hello Azure portal</span></span>


## <a name="prerequisites"></a><span data-ttu-id="041a4-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="041a4-114">Prerequisites</span></span>

1. [<span data-ttu-id="041a4-115">Stáhněte a nainstalujte Git</span><span class="sxs-lookup"><span data-stu-id="041a4-115">Download and install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="041a4-116">Stáhněte a nainstalujte hello Java 7 JDK nebo novější</span><span class="sxs-lookup"><span data-stu-id="041a4-116">Download and install hello Java 7 JDK or above</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [<span data-ttu-id="041a4-117">Stáhnout, nainstalovat a spustit MySQL</span><span class="sxs-lookup"><span data-stu-id="041a4-117">Download, install, and start MySQL</span></span>](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="041a4-118">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="041a4-118">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="041a4-119">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="041a4-119">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="041a4-120">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="041a4-120">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prepare-local-mysql"></a><span data-ttu-id="041a4-121">Příprava místního MySQL</span><span class="sxs-lookup"><span data-stu-id="041a4-121">Prepare local MySQL</span></span> 

<span data-ttu-id="041a4-122">V tomto kroku vytvoříte databázi v místním serveru MySQL pro použití v testování aplikace hello místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="041a4-122">In this step, you create a database in a local MySQL server for use in testing hello app locally on your machine.</span></span>

### <a name="connect-toomysql-server"></a><span data-ttu-id="041a4-123">Připojení serveru tooMySQL</span><span class="sxs-lookup"><span data-stu-id="041a4-123">Connect tooMySQL server</span></span>

<span data-ttu-id="041a4-124">V okně terminálu připojte místní server MySQL tooyour.</span><span class="sxs-lookup"><span data-stu-id="041a4-124">In a terminal window, connect tooyour local MySQL server.</span></span> <span data-ttu-id="041a4-125">Všechny příkazy hello toto okno terminálu toorun můžete použít v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="041a4-125">You can use this terminal window toorun all hello commands in this tutorial.</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="041a4-126">Pokud se zobrazí výzva k zadání hesla, zadejte heslo hello hello `root` účtu.</span><span class="sxs-lookup"><span data-stu-id="041a4-126">If you're prompted for a password, enter hello password for hello `root` account.</span></span> <span data-ttu-id="041a4-127">Pokud si nepamatujete heslo kořenového účtu, najdete v části [MySQL: jak tooReset hello kořenové heslo](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span><span class="sxs-lookup"><span data-stu-id="041a4-127">If you don't remember your root account password, see [MySQL: How tooReset hello Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span></span>

<span data-ttu-id="041a4-128">Pokud váš příkaz úspěšně proběhne, serveru databáze MySQL již spuštěna.</span><span class="sxs-lookup"><span data-stu-id="041a4-128">If your command runs successfully, then your MySQL server is already running.</span></span> <span data-ttu-id="041a4-129">Pokud ne, ujistěte se, že místní server MySQL se spustí následující hello [kroky po instalaci MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span><span class="sxs-lookup"><span data-stu-id="041a4-129">If not, make sure that your local MySQL server is started by following hello [MySQL post-installation steps](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span></span>

### <a name="create-a-database"></a><span data-ttu-id="041a4-130">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="041a4-130">Create a database</span></span> 

<span data-ttu-id="041a4-131">V hello `mysql` výzvu, vytvořte databázi a tabulku hello položkami seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="041a4-131">In hello `mysql` prompt, create a database and a table for hello to-do items.</span></span>

```sql
CREATE DATABASE tododb;
```

<span data-ttu-id="041a4-132">Ukončení připojení k serveru zadáním `quit`.</span><span class="sxs-lookup"><span data-stu-id="041a4-132">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="create-and-run-hello-sample-app"></a><span data-ttu-id="041a4-133">Vytvořte a spusťte ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="041a4-133">Create and run hello sample app</span></span> 

<span data-ttu-id="041a4-134">V tomto kroku naklonujte ukázkovou aplikaci spouštěcí pružiny, ho nakonfigurovat místní databázi MySQL toouse hello a spustit ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="041a4-134">In this step, you clone sample Spring boot app, configure it toouse hello local MySQL database, and run it on your computer.</span></span> 

### <a name="clone-hello-sample"></a><span data-ttu-id="041a4-135">Ukázka hello klonování</span><span class="sxs-lookup"><span data-stu-id="041a4-135">Clone hello sample</span></span>

<span data-ttu-id="041a4-136">V okně terminálu hello přejděte tooa práce adresáře a naklonujte úložiště ukázkové hello.</span><span class="sxs-lookup"><span data-stu-id="041a4-136">In hello terminal window, navigate tooa working directory and clone hello sample repository.</span></span> 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-hello-app-toouse-hello-mysql-database"></a><span data-ttu-id="041a4-137">Konfiguraci databáze MySQL hello toouse aplikace hello</span><span class="sxs-lookup"><span data-stu-id="041a4-137">Configure hello app toouse hello MySQL database</span></span>

<span data-ttu-id="041a4-138">Aktualizace hello `spring.datasource.password` a hodnota v *spring-boot-mysql-todo/src/main/resources/application.properties* s hello používá stejné kořenové heslo tooopen hello MySQL řádku:</span><span class="sxs-lookup"><span data-stu-id="041a4-138">Update hello `spring.datasource.password` and  value in *spring-boot-mysql-todo/src/main/resources/application.properties* with hello same root password used tooopen hello MySQL prompt:</span></span>

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-hello-sample"></a><span data-ttu-id="041a4-139">Sestavení a spuštění ukázkových hello</span><span class="sxs-lookup"><span data-stu-id="041a4-139">Build and run hello sample</span></span>

<span data-ttu-id="041a4-140">Sestavení a spuštění hello ukázku pomocí obálku Maven hello součástí hello úložiště:</span><span class="sxs-lookup"><span data-stu-id="041a4-140">Build and run hello sample using hello Maven wrapper included in hello repo:</span></span>

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

<span data-ttu-id="041a4-141">Otevřete váš prohlížeč toohttp://localhost:8080 toosee v ukázce hello v akci.</span><span class="sxs-lookup"><span data-stu-id="041a4-141">Open your browser toohttp://localhost:8080 toosee in hello sample in action.</span></span> <span data-ttu-id="041a4-142">Při přidávání seznamu toohello úlohy, použijte následující příkaz SQL příkazů v hello MySQL výzva tooview hello data uložená v MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="041a4-142">As you add tasks toohello list,  use hello following SQL commands in hello MySQL prompt tooview hello data stored in MySQL.</span></span>

```SQL
use testdb;
select * from todo_item;
```

<span data-ttu-id="041a4-143">Zastavte aplikaci hello stiskne `Ctrl` + `C` v terminálu hello.</span><span class="sxs-lookup"><span data-stu-id="041a4-143">Stop hello application by hitting `Ctrl`+`C` in hello terminal.</span></span> 

## <a name="create-an-azure-mysql-database"></a><span data-ttu-id="041a4-144">Vytvoření databáze MySQL na Azure</span><span class="sxs-lookup"><span data-stu-id="041a4-144">Create an Azure MySQL database</span></span>

<span data-ttu-id="041a4-145">V tomto kroku vytvoříte [Azure Database pro databázi MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) instanci pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="041a4-145">In this step, you create an [Azure Database for MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) instance using hello [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="041a4-146">Nakonfigurujete hello ukázkové aplikace toouse tuto databázi později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="041a4-146">You configure hello sample application toouse this database later on in hello tutorial.</span></span>

<span data-ttu-id="041a4-147">Hello použití Azure CLI 2.0 v okno terminálu toocreate hello prostředky potřebné toohost aplikace v jazyce Java v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="041a4-147">Use hello Azure CLI 2.0 in a terminal window toocreate hello resources needed toohost your Java application in Azure appservice.</span></span> <span data-ttu-id="041a4-148">Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="041a4-148">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> 

```azurecli-interactive 
az login 
```   

### <a name="create-a-resource-group"></a><span data-ttu-id="041a4-149">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="041a4-149">Create a resource group</span></span>

<span data-ttu-id="041a4-150">Vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="041a4-150">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="041a4-151">Skupinu prostředků Azure je logický kontejner, kde jsou související prostředky jako webové aplikace, databáze a účtů úložiště nasadit a spravovat.</span><span class="sxs-lookup"><span data-stu-id="041a4-151">An Azure resource group is a logical container where related resources like web apps, databases, and storage accounts are deployed and managed.</span></span> 

<span data-ttu-id="041a4-152">Hello následující příklad vytvoří skupinu prostředků v oblasti Severní Evropa hello:</span><span class="sxs-lookup"><span data-stu-id="041a4-152">hello following example creates a resource group in hello North Europe region:</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

<span data-ttu-id="041a4-153">toosee hello možných hodnot můžete použít pro `--location`, použijte hello [míst seznamu služby App Service az](/cli/azure/appservice#list-locations) příkaz.</span><span class="sxs-lookup"><span data-stu-id="041a4-153">toosee hello possible values you can use for `--location`, use hello [az appservice list-locations](/cli/azure/appservice#list-locations) command.</span></span>

### <a name="create-a-mysql-server"></a><span data-ttu-id="041a4-154">Vytvoření databáze MySQL serveru</span><span class="sxs-lookup"><span data-stu-id="041a4-154">Create a MySQL server</span></span>

<span data-ttu-id="041a4-155">Vytvoření serveru ve službě Azure Database pro databázi MySQL (Preview) s hello [az mysql server vytvořit](/cli/azure/mysql/server#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="041a4-155">Create a server in Azure Database for MySQL (Preview) with hello [az mysql server create](/cli/azure/mysql/server#create) command.</span></span>    
<span data-ttu-id="041a4-156">Nahraďte vlastní jedinečný název serveru databáze MySQL, kde uvidíte hello `<mysql_server_name>` zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="041a4-156">Substitute your own unique MySQL server name where you see hello `<mysql_server_name>` placeholder.</span></span> <span data-ttu-id="041a4-157">Tento název je součástí název hostitele serveru MySQL, `<mysql_server_name>.mysql.database.azure.com`, takže je nutné toobe globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="041a4-157">This name is part of your MySQL server's hostname, `<mysql_server_name>.mysql.database.azure.com`, so it needs toobe globally unique.</span></span> <span data-ttu-id="041a4-158">Také nahraďte `<admin_user>` a `<admin_password>` vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="041a4-158">Also substitute `<admin_user>` and `<admin_password>` with your own values.</span></span>

```azurecli-interactive
az mysql server create --name <mysql_server_name> \ 
    --resource-group myResourceGroup \ 
    --location "North Europe" \
    --admin-user <admin_user> \ 
    --admin-password <admin_password>
```

<span data-ttu-id="041a4-159">Při vytvoření hello MySQL serveru je hello rozhraní příkazového řádku Azure obsahuje informace o podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="041a4-159">When hello MySQL server is created, hello Azure CLI shows information similar toohello following example:</span></span>

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

### <a name="configure-server-firewall"></a><span data-ttu-id="041a4-160">Konfigurace brány firewall serveru</span><span class="sxs-lookup"><span data-stu-id="041a4-160">Configure server firewall</span></span>

<span data-ttu-id="041a4-161">Vytvořit pravidlo brány firewall pro klientské MySQL serveru tooallow připojení pomocí hello [az mysql pravidla brány firewall-vytvořit](/cli/azure/mysql/server/firewall-rule#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="041a4-161">Create a firewall rule for your MySQL server tooallow client connections by using hello [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#create) command.</span></span> 

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name>  \ 
    --resource-group myResourceGroup \ 
    --start-ip-address 0.0.0.0 \ 
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="041a4-162">Azure databáze pro databázi MySQL (Preview) automaticky aktuálně neumožňuje připojení ze služby Azure.</span><span class="sxs-lookup"><span data-stu-id="041a4-162">Azure Database for MySQL (Preview) does not currently automatically enable connections from Azure services.</span></span> <span data-ttu-id="041a4-163">Jak budou dynamicky přiřazovat IP adresy v Azure, je lepší tooenable všechny IP adresy pro teď.</span><span class="sxs-lookup"><span data-stu-id="041a4-163">As IP addresses in Azure are dynamically assigned, it is better tooenable all IP addresses for now.</span></span> <span data-ttu-id="041a4-164">Služba hello stále jeho verze preview, lepší metody pro zabezpečení databáze bude povolena.</span><span class="sxs-lookup"><span data-stu-id="041a4-164">As hello service continues its preview, better methods for securing your database will be enabled.</span></span>

## <a name="configure-hello-azure-mysql-database"></a><span data-ttu-id="041a4-165">Konfiguraci databáze Azure MySQL hello</span><span class="sxs-lookup"><span data-stu-id="041a4-165">Configure hello Azure MySQL database</span></span>

<span data-ttu-id="041a4-166">V okně terminálu hello ve vašem počítači připojte server toohello MySQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="041a4-166">In hello terminal window on your computer, connect toohello MySQL server in Azure.</span></span> <span data-ttu-id="041a4-167">Použít hodnotu hello jste zadali dřív pro `<admin_user>` a `<mysql_server_name>`.</span><span class="sxs-lookup"><span data-stu-id="041a4-167">Use hello value you specified previously for `<admin_user>` and `<mysql_server_name>`.</span></span>

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a><span data-ttu-id="041a4-168">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="041a4-168">Create a database</span></span> 

<span data-ttu-id="041a4-169">V hello `mysql` výzvu, vytvořte databázi a tabulku hello položkami seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="041a4-169">In hello `mysql` prompt, create a database and a table for hello to-do items.</span></span>

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a><span data-ttu-id="041a4-170">Vytvořit uživatele s oprávněními</span><span class="sxs-lookup"><span data-stu-id="041a4-170">Create a user with permissions</span></span>

<span data-ttu-id="041a4-171">Vytvoření uživatele databáze a pojmenujte ho všechna oprávnění v hello `tododb` databáze.</span><span class="sxs-lookup"><span data-stu-id="041a4-171">Create a database user and give it all privileges in hello `tododb` database.</span></span> <span data-ttu-id="041a4-172">Nahraďte zástupné symboly hello `<Javaapp_user>` a `<Javaapp_password>` s vlastními jedinečným názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="041a4-172">Replace hello placeholders `<Javaapp_user>` and `<Javaapp_password>` with your own unique app name.</span></span>

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* too'<Javaapp_user>';
```

<span data-ttu-id="041a4-173">Ukončení připojení k serveru zadáním `quit`.</span><span class="sxs-lookup"><span data-stu-id="041a4-173">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="deploy-hello-sample-tooazure-app-service"></a><span data-ttu-id="041a4-174">Nasazení ukázkové tooAzure hello služby App Service</span><span class="sxs-lookup"><span data-stu-id="041a4-174">Deploy hello sample tooAzure App Service</span></span>

<span data-ttu-id="041a4-175">Vytvořte plán aplikační služby Azure s hello **volné** cenová úroveň pomocí hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) rozhraní příkazového řádku příkaz.</span><span class="sxs-lookup"><span data-stu-id="041a4-175">Create an Azure App Service plan with hello **FREE** pricing tier using hello  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="041a4-176">plán aplikační služby Hello definuje toohost hello fyzické prostředky, které používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="041a4-176">hello appservice plan defines hello physical resources used toohost your apps.</span></span> <span data-ttu-id="041a4-177">Všechny aplikace, které jsou přiřazené plán aplikační služby tooan sdílení těchto prostředků, umožní vám toosave nákladů při hostování více aplikací.</span><span class="sxs-lookup"><span data-stu-id="041a4-177">All applications assigned tooan appservice plan share these resources, allowing you toosave cost when hosting multiple apps.</span></span> 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

<span data-ttu-id="041a4-178">Při plánování hello je připraven, výstup hello rozhraní příkazového řádku Azure zobrazuje podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="041a4-178">When hello plan is ready, hello Azure CLI shows similar output toohello following example:</span></span>

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

### <a name="create-an-azure-web-app"></a><span data-ttu-id="041a4-179">Vytvoření webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="041a4-179">Create an Azure Web app</span></span>

 <span data-ttu-id="041a4-180">Použití hello [az webapp vytvořit](/cli/azure/appservice/web#create) rozhraní příkazového řádku příkaz toocreate definici webové aplikace v hello `myAppServicePlan` plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="041a4-180">Use hello [az webapp create](/cli/azure/appservice/web#create) CLI command toocreate a web app definition in hello `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="041a4-181">definice Hello webové aplikace poskytuje tooaccess adresa URL vaší aplikace pomocí a konfiguruje několik možností toodeploy tooAzure vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="041a4-181">hello web app definition provides a URL tooaccess your application with and configures several options toodeploy your code tooAzure.</span></span> 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

<span data-ttu-id="041a4-182">SUBSTITUTE hello `<app_name>` zástupný symbol vlastní jedinečným názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="041a4-182">Substitute hello `<app_name>` placeholder with your own unique app name.</span></span> <span data-ttu-id="041a4-183">Tento jedinečný název je součástí hello výchozí název domény pro webovou aplikaci hello, takže název hello musí toobe jedinečný mezi všechny aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="041a4-183">This unique name is part of hello default domain name for hello web app, so hello name needs toobe unique across all apps in Azure.</span></span> <span data-ttu-id="041a4-184">Můžete namapovat vlastní doménu název položky toohello webové aplikace ještě před zveřejněním tooyour uživatele.</span><span class="sxs-lookup"><span data-stu-id="041a4-184">You can map a custom domain name entry toohello web app before you expose it tooyour users.</span></span>

<span data-ttu-id="041a4-185">Při definici hello webové aplikace je připraven, hello rozhraní příkazového řádku Azure ukazuje následující příklad podobné toohello informace:</span><span class="sxs-lookup"><span data-stu-id="041a4-185">When hello web app definition is ready, hello Azure CLI shows information similar toohello following example:</span></span> 

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

### <a name="configure-java"></a><span data-ttu-id="041a4-186">Konfigurace Java</span><span class="sxs-lookup"><span data-stu-id="041a4-186">Configure Java</span></span> 

<span data-ttu-id="041a4-187">Nastavení konfigurace hello Java runtime, která vaše aplikace, musí se hello [aktualizace konfigurace webové služby App Service az](/cli/azure/appservice/web/config#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="041a4-187">Set up hello Java runtime configuration that your app needs with hello  [az appservice web config update](/cli/azure/appservice/web/config#update) command.</span></span>

<span data-ttu-id="041a4-188">Hello následující příkaz nakonfiguruje hello webové aplikace toorun na poslední JDK 8 Java a [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="041a4-188">hello following command configures hello web app toorun on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

### <a name="configure-hello-app-toouse-hello-azure-sql-database"></a><span data-ttu-id="041a4-189">Konfigurace hello aplikace toouse hello Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="041a4-189">Configure hello app toouse hello Azure SQL database</span></span>

<span data-ttu-id="041a4-190">Před spuštěním hello ukázkové aplikace, nastavení aplikací na hello webové aplikace toouse hello Azure databáze MySQL, kterou jste vytvořili v Azure.</span><span class="sxs-lookup"><span data-stu-id="041a4-190">Before running hello sample app, set application settings on hello web app toouse hello Azure MySQL database you created in Azure.</span></span> <span data-ttu-id="041a4-191">Tyto vlastnosti jsou zveřejněné toohello webové aplikace jako proměnné prostředí a přepsat hello hodnotami nastavenými v hello application.properties uvnitř hello zabalené webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="041a4-191">These properties are exposed toohello web application as environment variables and override hello values set in hello application.properties inside hello packaged web app.</span></span> 

<span data-ttu-id="041a4-192">Nastavení aplikace pomocí [az webapp konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) v hello rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="041a4-192">Set application settings using [az webapp config appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) in hello CLI:</span></span>

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

### <a name="get-ftp-deployment-credentials"></a><span data-ttu-id="041a4-193">Získat přihlašovací údaje pro nasazení serveru FTP</span><span class="sxs-lookup"><span data-stu-id="041a4-193">Get FTP deployment credentials</span></span> 
<span data-ttu-id="041a4-194">Můžete nasadit vaše aplikace tooAzure služby App Service různými způsoby, včetně FTP, místní Git, GitHub, Visual Studio Team Services a BitBucket.</span><span class="sxs-lookup"><span data-stu-id="041a4-194">You can deploy your application tooAzure appservice in various ways including FTP, local Git, GitHub, Visual Studio Team Services, and BitBucket.</span></span> <span data-ttu-id="041a4-195">V tomto příkladu FTP toodeploy hello. Soubor WAR dříve vytvořené na vašem místním počítači tooAzure služby App Service.</span><span class="sxs-lookup"><span data-stu-id="041a4-195">For this example, FTP toodeploy hello .WAR file built previously on your local machine tooAzure App Service.</span></span>

<span data-ttu-id="041a4-196">toodetermine co pověření toopass společně v ftp příkaz toohello webové aplikace, použijte [az služby App Service web nasazení seznamu publikování profily](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) příkaz:</span><span class="sxs-lookup"><span data-stu-id="041a4-196">toodetermine what credentials toopass along in an ftp command toohello Web App, Use [az appservice web deployment list-publishing-profiles](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) command:</span></span> 

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

### <a name="upload-hello-app-using-ftp"></a><span data-ttu-id="041a4-197">Nahrání aplikace hello pomocí protokolu FTP</span><span class="sxs-lookup"><span data-stu-id="041a4-197">Upload hello app using FTP</span></span>

<span data-ttu-id="041a4-198">Použijte váš oblíbený hello toodeploy nástroj FTP. Toohello soubor WAR */site/wwwroot/webapps* složky na adresu serveru hello převzaty z hello `URL` v předchozí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="041a4-198">Use your favorite FTP tool toodeploy hello .WAR file toohello */site/wwwroot/webapps* folder on hello server address taken from hello `URL` field in hello previous command.</span></span> <span data-ttu-id="041a4-199">Odeberte hello existující adresář aplikace výchozí (uživatel ROOT) a nahraďte hello existující ROOT.war s hello. Soubor WAR součástí hello v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="041a4-199">Remove hello existing default (ROOT) application directory and replace hello existing ROOT.war with hello .WAR file built in hello earlier in hello tutorial.</span></span>

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

### <a name="test-hello-web-app"></a><span data-ttu-id="041a4-200">Testování hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="041a4-200">Test hello web app</span></span>

<span data-ttu-id="041a4-201">Procházet příliš`http://<app_name>.azurewebsites.net/` a přidání seznamu toohello několik úloh.</span><span class="sxs-lookup"><span data-stu-id="041a4-201">Browse too`http://<app_name>.azurewebsites.net/` and add a few tasks toohello list.</span></span> 

![Java aplikace spuštěné v Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

<span data-ttu-id="041a4-203">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="041a4-203">**Congratulations!**</span></span> <span data-ttu-id="041a4-204">Používáte datové aplikace v jazyce Java v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="041a4-204">You're running a data-driven Java app in Azure App Service.</span></span>

## <a name="update-hello-app-and-redeploy"></a><span data-ttu-id="041a4-205">Aktualizace aplikace hello a znovu ho zaveďte</span><span class="sxs-lookup"><span data-stu-id="041a4-205">Update hello app and redeploy</span></span>

<span data-ttu-id="041a4-206">Aktualizujte tooinclude aplikace hello sloupec v seznamu úkolů hello, pro jaké den hello položka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="041a4-206">Update hello application tooinclude an additional column in hello todo list for what day hello item was created.</span></span> <span data-ttu-id="041a4-207">Spouštěcí pružiny zpracovává aktualizace schématu databáze hello můžete jako změn datových modelů hello beze změny stávajících záznamů databáze.</span><span class="sxs-lookup"><span data-stu-id="041a4-207">Spring Boot handles updating hello database schema for you as hello data model changes without altering your existing database records.</span></span>

1. <span data-ttu-id="041a4-208">V lokálním systému, otevře *src/main/java/com/example/fabrikam/TodoItem.java* a přidejte následující hello importuje toohello třídy:</span><span class="sxs-lookup"><span data-stu-id="041a4-208">On your local system, open up *src/main/java/com/example/fabrikam/TodoItem.java* and add hello following imports toohello class:</span></span>   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. <span data-ttu-id="041a4-209">Přidat `String` vlastnost `timeCreated` příliš*src/main/java/com/example/fabrikam/TodoItem.java*, inicializace s časovým razítkem na vytvoření objektu.</span><span class="sxs-lookup"><span data-stu-id="041a4-209">Add a `String` property `timeCreated` too*src/main/java/com/example/fabrikam/TodoItem.java*, initializing it with a timestamp at object creation.</span></span> <span data-ttu-id="041a4-210">Přidat mechanismy získání nebo nastavení pro nové hello `timeCreated` vlastnost během úprav tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="041a4-210">Add getters/setters for hello new `timeCreated` property while you are editing this file.</span></span>

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

3. <span data-ttu-id="041a4-211">Aktualizace *src/main/java/com/example/fabrikam/TodoDemoController.java* řádek v hello `updateTodo` metoda tooset hello časové razítko:</span><span class="sxs-lookup"><span data-stu-id="041a4-211">Update *src/main/java/com/example/fabrikam/TodoDemoController.java* with a line in hello `updateTodo` method tooset hello timestamp:</span></span>

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. <span data-ttu-id="041a4-212">Přidáte podporu pro nové pole hello v šabloně Thymeleaf hello.</span><span class="sxs-lookup"><span data-stu-id="041a4-212">Add support for hello new field in hello Thymeleaf template.</span></span> <span data-ttu-id="041a4-213">Aktualizace *src/main/resources/templates/index.html* s novou hlavičkou tabulky pro časové razítko hello a nová hodnota toodisplay hello pole hello časového razítka v každém řádku dat tabulky.</span><span class="sxs-lookup"><span data-stu-id="041a4-213">Update *src/main/resources/templates/index.html* with a new table header for hello timestamp, and a new field toodisplay hello value of hello timestamp in each table data row.</span></span>

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

5. <span data-ttu-id="041a4-214">Opětovné sestavení aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="041a4-214">Rebuild hello application:</span></span>

    ```bash
    mvnw clean package 
    ```

6. <span data-ttu-id="041a4-215">Aktualizovat hello FTP. WAR jako před, odebrání stávající hello *lokality/wwwroot/webapps/ROOT* adresáře a *ROOT.war*, pak odesílání hello aktualizovat. Soubor WAR jako ROOT.war.</span><span class="sxs-lookup"><span data-stu-id="041a4-215">FTP hello updated .WAR as before, removing hello existing *site/wwwroot/webapps/ROOT* directory and *ROOT.war*, then uploading hello updated .WAR file as ROOT.war.</span></span> 

<span data-ttu-id="041a4-216">Při aktualizaci aplikace hello **čas vytvoření** sloupec je nyní viditelné.</span><span class="sxs-lookup"><span data-stu-id="041a4-216">When you refresh hello app, a **Time Created** column is now visible.</span></span> <span data-ttu-id="041a4-217">Když přidáte novou úlohu, aplikace hello automaticky naplnit hello časové razítko.</span><span class="sxs-lookup"><span data-stu-id="041a4-217">When you add a new task, hello app will populate hello timestamp automatically.</span></span> <span data-ttu-id="041a4-218">Vaše stávající úlohy zůstat beze změny a pracovat s aplikací hello to i v případě, že došlo ke změně hello základní datový model.</span><span class="sxs-lookup"><span data-stu-id="041a4-218">Your existing tasks remain unchanged and work with hello app even though hello underlying data model has changed.</span></span> 

![Aplikace v jazyce Java aktualizovat pomocí nového sloupce](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a><span data-ttu-id="041a4-220">Diagnostické protokoly datového proudu</span><span class="sxs-lookup"><span data-stu-id="041a4-220">Stream diagnostic logs</span></span> 

<span data-ttu-id="041a4-221">Při spuštění aplikace v jazyce Java v Azure App Service, můžete získat hello konzoly protokoly přesměruje přímo tooyour terminálu.</span><span class="sxs-lookup"><span data-stu-id="041a4-221">While your Java application runs in Azure App Service, you can get hello console logs piped directly tooyour terminal.</span></span> <span data-ttu-id="041a4-222">Tímto způsobem můžete získat hello stejné diagnostické zprávy toohelp ladění chyb aplikace.</span><span class="sxs-lookup"><span data-stu-id="041a4-222">That way, you can get hello same diagnostic messages toohelp you debug application errors.</span></span>

<span data-ttu-id="041a4-223">toostart protokolu streamování, použijte hello [az webapp protokolu poškozené databáze za](/cli/azure/appservice/web/log#tail) příkaz.</span><span class="sxs-lookup"><span data-stu-id="041a4-223">toostart log streaming, use hello [az webapp log tail](/cli/azure/appservice/web/log#tail) command.</span></span>

```azurecli-interactive 
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="041a4-224">Správa Azure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="041a4-224">Manage your Azure web app</span></span>

<span data-ttu-id="041a4-225">Přejděte toohello Azure portálu toosee hello webovou aplikaci, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="041a4-225">Go toohello Azure portal toosee hello web app you created.</span></span>

<span data-ttu-id="041a4-226">toodo, přihlaste se příliš[https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="041a4-226">toodo this, sign in too[https://portal.azure.com](https://portal.azure.com).</span></span>

<span data-ttu-id="041a4-227">V levé nabídce hello, klikněte na **služby App Service**, pak klikněte na název hello Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="041a4-227">From hello left menu, click **App Service**, then click hello name of your Azure web app.</span></span>

![Portálu tooAzure webové aplikace](./media/app-service-web-tutorial-java-mysql/access-portal.png)

<span data-ttu-id="041a4-229">Ve výchozím nastavení, zobrazí okno vaší webové aplikace hello **přehled** stránky.</span><span class="sxs-lookup"><span data-stu-id="041a4-229">By default, your web app's blade shows hello **Overview** page.</span></span> <span data-ttu-id="041a4-230">Tato stránka poskytuje přehled, jak si vaše aplikace stojí.</span><span class="sxs-lookup"><span data-stu-id="041a4-230">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="041a4-231">Zde můžete také provést úlohy správy, jako zastavení, spuštění, restartování a delete.</span><span class="sxs-lookup"><span data-stu-id="041a4-231">Here, you can also perform management tasks like stop, start, restart, and delete.</span></span> <span data-ttu-id="041a4-232">Hello karty na levé straně okna hello hello zobrazit stránky hello jinou konfiguraci, které můžete otevřít.</span><span class="sxs-lookup"><span data-stu-id="041a4-232">hello tabs on hello left side of hello blade show hello different configuration pages you can open.</span></span>

![Okno App Service na webu Azure Portal](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

<span data-ttu-id="041a4-234">Tyto karty v okně hello zobrazit hello mnoho funkcí můžete přidat tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="041a4-234">These tabs in hello blade show hello many great features you can add tooyour web app.</span></span> <span data-ttu-id="041a4-235">Hello následující seznam vám poskytuje několik možností, jak hello:</span><span class="sxs-lookup"><span data-stu-id="041a4-235">hello following list gives you just a few of hello possibilities:</span></span>
* <span data-ttu-id="041a4-236">Mapování vlastního názvu DNS</span><span class="sxs-lookup"><span data-stu-id="041a4-236">Map a custom DNS name</span></span>
* <span data-ttu-id="041a4-237">Vazba vlastního certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="041a4-237">Bind a custom SSL certificate</span></span>
* <span data-ttu-id="041a4-238">Konfigurace průběžného nasazování</span><span class="sxs-lookup"><span data-stu-id="041a4-238">Configure continuous deployment</span></span>
* <span data-ttu-id="041a4-239">Vertikální i horizontální navýšení kapacity</span><span class="sxs-lookup"><span data-stu-id="041a4-239">Scale up and out</span></span>
* <span data-ttu-id="041a4-240">Přidání ověřování uživatelů</span><span class="sxs-lookup"><span data-stu-id="041a4-240">Add user authentication</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="041a4-241">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="041a4-241">Clean up resources</span></span>

<span data-ttu-id="041a4-242">Pokud nepotřebujete tyto prostředky pro jiné kurzu (najdete v části [další kroky](#next)), můžete je odstranit spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="041a4-242">If you don't need these resources for another tutorial (see [Next steps](#next)), you can delete them by running hello following command:</span></span> 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="041a4-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="041a4-243">Next steps</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="041a4-244">Vytvoření databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="041a4-244">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="041a4-245">Připojit toohello aplikace Java ukázkové databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="041a4-245">Connect a sample Java app toohello MySQL</span></span>
> * <span data-ttu-id="041a4-246">Nasazení aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="041a4-246">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="041a4-247">Aktualizace a znovu nasaďte aplikace hello</span><span class="sxs-lookup"><span data-stu-id="041a4-247">Update and redeploy hello app</span></span>
> * <span data-ttu-id="041a4-248">Diagnostické protokoly datového proudu z Azure</span><span class="sxs-lookup"><span data-stu-id="041a4-248">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="041a4-249">Spravovat aplikace hello v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="041a4-249">Manage hello app in hello Azure portal</span></span>

<span data-ttu-id="041a4-250">Posunutí toohello další kurz toolearn jak toomap vlastní DNS název toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="041a4-250">Advance toohello next tutorial toolearn how toomap a custom DNS name toohello app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="041a4-251">Mapovat existující vlastní DNS název tooAzure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="041a4-251">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
