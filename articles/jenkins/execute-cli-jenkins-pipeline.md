---
title: "aaaExecute hello rozhraní příkazového řádku Azure s volaných | Microsoft Docs"
description: "Zjistěte, jak toouse rozhraní příkazového řádku Azure toodeploy Java webové aplikace tooAzure v volaných kanálu"
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: jenkins
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 4bd1e12e6de1f010453ff51c835f84e7361962f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-app-service-with-jenkins-and-hello-azure-cli"></a><span data-ttu-id="068fb-103">Nasazení tooAzure App Service pomocí volaných a hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="068fb-103">Deploy tooAzure App Service with Jenkins and hello Azure CLI</span></span>
<span data-ttu-id="068fb-104">toodeploy tooAzure webové aplikace Java, můžete použít rozhraní příkazového řádku Azure v [volaných kanálu](https://jenkins.io/doc/book/pipeline/).</span><span class="sxs-lookup"><span data-stu-id="068fb-104">toodeploy a Java web app tooAzure, you can use Azure CLI in [Jenkins Pipeline](https://jenkins.io/doc/book/pipeline/).</span></span> <span data-ttu-id="068fb-105">V tomto kurzu vytvoříte kanál CI/CD na virtuální počítač Azure včetně postup:</span><span class="sxs-lookup"><span data-stu-id="068fb-105">In this tutorial, you create a CI/CD pipeline on an Azure VM including how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="068fb-106">Vytvoření virtuálního počítače volaných</span><span class="sxs-lookup"><span data-stu-id="068fb-106">Create a Jenkins VM</span></span>
> * <span data-ttu-id="068fb-107">Konfigurace volaných</span><span class="sxs-lookup"><span data-stu-id="068fb-107">Configure Jenkins</span></span>
> * <span data-ttu-id="068fb-108">Vytvoření webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="068fb-108">Create a web app in Azure</span></span>
> * <span data-ttu-id="068fb-109">Příprava úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="068fb-109">Prepare a GitHub repository</span></span>
> * <span data-ttu-id="068fb-110">Vytvoření kanálu volaných</span><span class="sxs-lookup"><span data-stu-id="068fb-110">Create Jenkins pipeline</span></span>
> * <span data-ttu-id="068fb-111">Spusťte hello kanálu a ověřte hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="068fb-111">Run hello pipeline and verify hello web app</span></span>

<span data-ttu-id="068fb-112">Tento kurz vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="068fb-112">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="068fb-113">verze hello toofind, spusťte `az --version`.</span><span class="sxs-lookup"><span data-stu-id="068fb-113">toofind hello version, run `az --version`.</span></span> <span data-ttu-id="068fb-114">Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="068fb-114">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a><span data-ttu-id="068fb-115">Vytvořit a nakonfigurovat volaných instance</span><span class="sxs-lookup"><span data-stu-id="068fb-115">Create and Configure Jenkins instance</span></span>
<span data-ttu-id="068fb-116">Pokud již nemáte volaných master, začínat hello [šablona řešení](install-jenkins-solution-template.md), což zahrnuje hello požadované [přihlašovací údaje Azure](https://plugins.jenkins.io/azure-credentials) modulu plug-in ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="068fb-116">If you do not already have a Jenkins master, start with hello [Solution Template](install-jenkins-solution-template.md), which includes hello required [Azure Credentials](https://plugins.jenkins.io/azure-credentials) plugin by default.</span></span> 

<span data-ttu-id="068fb-117">modul plug-in Hello přihlašovacích údajů Azure umožňuje hlavní pověření služby Microsoft Azure toostore ve volaných.</span><span class="sxs-lookup"><span data-stu-id="068fb-117">hello Azure Credential plugin allows you toostore Microsoft Azure service principal credentials in Jenkins.</span></span> <span data-ttu-id="068fb-118">Ve verzi 1.2 jsme doplnili podporu hello tak, že volaných kanálu můžete získat hello přihlašovacích údajů služby Azure.</span><span class="sxs-lookup"><span data-stu-id="068fb-118">In version 1.2, we added hello support so that Jenkins Pipeline can get hello Azure credentials.</span></span> 

<span data-ttu-id="068fb-119">Ujistěte se, že máte verze 1.2 nebo vyšší:</span><span class="sxs-lookup"><span data-stu-id="068fb-119">Ensure you have version 1.2 or later:</span></span>
* <span data-ttu-id="068fb-120">V rámci hello volaných řídicí panel, klikněte na **volaných spravovat -> modul plug-in Manager ->** a vyhledejte **přihlašovacích údajů Azure**.</span><span class="sxs-lookup"><span data-stu-id="068fb-120">Within hello Jenkins dashboard, click **Manage Jenkins -> Plugin Manager ->** and search for **Azure Credential**.</span></span> 
* <span data-ttu-id="068fb-121">Aktualizace modulu plug-in hello, pokud je starší než 1.2 hello verze.</span><span class="sxs-lookup"><span data-stu-id="068fb-121">Update hello plugin if hello version is earlier than 1.2.</span></span>

<span data-ttu-id="068fb-122">V hlavním volaných hello také vyžadují Java JDK a Maven.</span><span class="sxs-lookup"><span data-stu-id="068fb-122">Java JDK and Maven are also required in hello Jenkins master.</span></span> <span data-ttu-id="068fb-123">tooinstall, přihlaste se pomocí protokolu SSH hlavní tooJenkins a spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="068fb-123">tooinstall, log in tooJenkins master using SSH and run hello following commands:</span></span>
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-toojenkins-credential"></a><span data-ttu-id="068fb-124">Přidat přihlašovací údaje služby Azure hlavní tooJenkins</span><span class="sxs-lookup"><span data-stu-id="068fb-124">Add Azure service principal tooJenkins credential</span></span>

<span data-ttu-id="068fb-125">Azure přihlašovacích údajů je potřebné tooexecute rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="068fb-125">An Azure credential is needed tooexecute Azure CLI.</span></span>

* <span data-ttu-id="068fb-126">V rámci hello volaných řídicí panel, klikněte na **pověření -> Systém ->**.</span><span class="sxs-lookup"><span data-stu-id="068fb-126">Within hello Jenkins dashboard, click **Credentials -> System ->**.</span></span> <span data-ttu-id="068fb-127">Klikněte na tlačítko **globální credentials(unrestricted)**.</span><span class="sxs-lookup"><span data-stu-id="068fb-127">Click **Global credentials(unrestricted)**.</span></span>
* <span data-ttu-id="068fb-128">Klikněte na tlačítko **přidat přihlašovací údaje** tooadd [objektu služby Microsoft Azure](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) tak, že vyplníte hello ID předplatného, ID klienta, sdílený tajný klíč klienta a koncový bod tokenu OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="068fb-128">Click **Add Credentials** tooadd a [Microsoft Azure service principal](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) by filling out hello Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span> <span data-ttu-id="068fb-129">Zadejte ID pro použití v následném kroku.</span><span class="sxs-lookup"><span data-stu-id="068fb-129">Provide an ID for use in subsequent step.</span></span>

![Přidejte pověření](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-hello-java-web-app"></a><span data-ttu-id="068fb-131">Vytvoření Azure App Service pro nasazení webové aplikace v jazyce Java hello</span><span class="sxs-lookup"><span data-stu-id="068fb-131">Create an Azure App Service for deploying hello Java web app</span></span>

<span data-ttu-id="068fb-132">Vytvořte plán aplikační služby Azure s hello **volné** cenová úroveň pomocí hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) rozhraní příkazového řádku příkaz.</span><span class="sxs-lookup"><span data-stu-id="068fb-132">Create an Azure App Service plan with hello **FREE** pricing tier using hello  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="068fb-133">plán aplikační služby Hello definuje toohost hello fyzické prostředky, které používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="068fb-133">hello appservice plan defines hello physical resources used toohost your apps.</span></span> <span data-ttu-id="068fb-134">Všechny aplikace, které jsou přiřazené plán aplikační služby tooan sdílení těchto prostředků, umožní vám toosave nákladů při hostování více aplikací.</span><span class="sxs-lookup"><span data-stu-id="068fb-134">All applications assigned tooan appservice plan share these resources, allowing you toosave cost when hosting multiple apps.</span></span> 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

<span data-ttu-id="068fb-135">Při plánování hello je připraven, výstup hello rozhraní příkazového řádku Azure zobrazuje podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="068fb-135">When hello plan is ready, hello Azure CLI shows similar output toohello following example:</span></span>

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

### <a name="create-an-azure-web-app"></a><span data-ttu-id="068fb-136">Vytvoření webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="068fb-136">Create an Azure Web app</span></span>

 <span data-ttu-id="068fb-137">Použití hello [az webapp vytvořit](/cli/azure/appservice/web#create) rozhraní příkazového řádku příkaz toocreate definici webové aplikace v hello `myAppServicePlan` plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="068fb-137">Use hello [az webapp create](/cli/azure/appservice/web#create) CLI command toocreate a web app definition in hello `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="068fb-138">definice Hello webové aplikace poskytuje tooaccess adresa URL vaší aplikace pomocí a konfiguruje několik možností toodeploy tooAzure vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="068fb-138">hello web app definition provides a URL tooaccess your application with and configures several options toodeploy your code tooAzure.</span></span> 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

<span data-ttu-id="068fb-139">SUBSTITUTE hello `<app_name>` zástupný symbol vlastní jedinečným názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="068fb-139">Substitute hello `<app_name>` placeholder with your own unique app name.</span></span> <span data-ttu-id="068fb-140">Tento jedinečný název je součástí hello výchozí název domény pro webovou aplikaci hello, takže název hello musí toobe jedinečný mezi všechny aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="068fb-140">This unique name is part of hello default domain name for hello web app, so hello name needs toobe unique across all apps in Azure.</span></span> <span data-ttu-id="068fb-141">Můžete namapovat vlastní doménu název položky toohello webové aplikace ještě před zveřejněním tooyour uživatele.</span><span class="sxs-lookup"><span data-stu-id="068fb-141">You can map a custom domain name entry toohello web app before you expose it tooyour users.</span></span>

<span data-ttu-id="068fb-142">Při definici hello webové aplikace je připraven, hello rozhraní příkazového řádku Azure ukazuje následující příklad podobné toohello informace:</span><span class="sxs-lookup"><span data-stu-id="068fb-142">When hello web app definition is ready, hello Azure CLI shows information similar toohello following example:</span></span> 

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

### <a name="configure-java"></a><span data-ttu-id="068fb-143">Konfigurace Java</span><span class="sxs-lookup"><span data-stu-id="068fb-143">Configure Java</span></span> 

<span data-ttu-id="068fb-144">Nastavení konfigurace hello Java runtime, která vaše aplikace, musí se hello [aktualizace konfigurace webové služby App Service az](/cli/azure/appservice/web/config#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="068fb-144">Set up hello Java runtime configuration that your app needs with hello  [az appservice web config update](/cli/azure/appservice/web/config#update) command.</span></span>

<span data-ttu-id="068fb-145">Hello následující příkaz nakonfiguruje hello webové aplikace toorun na poslední JDK 8 Java a [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="068fb-145">hello following command configures hello web app toorun on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a><span data-ttu-id="068fb-146">Příprava úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="068fb-146">Prepare a GitHub Repository</span></span>
<span data-ttu-id="068fb-147">Otevřete hello [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) úložišti.</span><span class="sxs-lookup"><span data-stu-id="068fb-147">Open hello [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) repo.</span></span> <span data-ttu-id="068fb-148">toofork hello úložišti tooyour vlastní účet GitHub, klikněte na tlačítko hello **rozvětvení** tlačítko v horním pravém rohu hello.</span><span class="sxs-lookup"><span data-stu-id="068fb-148">toofork hello repo tooyour own GitHub account, click hello **Fork** button in hello top right-hand corner.</span></span>

* <span data-ttu-id="068fb-149">V Githubu webového uživatelského rozhraní, otevřete **Jenkinsfile** souboru.</span><span class="sxs-lookup"><span data-stu-id="068fb-149">In GitHub web UI, open **Jenkinsfile** file.</span></span> <span data-ttu-id="068fb-150">Klikněte na tlačítko tooedit ikonu tužky hello tato skupina prostředků hello tooupdate souboru a název webové aplikace na řádku 20 a 21 v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="068fb-150">Click hello pencil icon tooedit this file tooupdate hello resource group and name of your web app on line 20 and 21 respectively.</span></span>

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* <span data-ttu-id="068fb-151">Změňte ID pověření 23 tooupdate řádek v instanci volaných</span><span class="sxs-lookup"><span data-stu-id="068fb-151">Change line 23 tooupdate credential ID in your Jenkins instance</span></span>

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a><span data-ttu-id="068fb-152">Vytvoření kanálu volaných</span><span class="sxs-lookup"><span data-stu-id="068fb-152">Create Jenkins pipeline</span></span>
<span data-ttu-id="068fb-153">Otevřete volaných ve webovém prohlížeči, klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="068fb-153">Open Jenkins in a web browser, click **New Item**.</span></span> 

* <span data-ttu-id="068fb-154">Zadejte název úlohy hello a vyberte **kanálu**.</span><span class="sxs-lookup"><span data-stu-id="068fb-154">Provide a name for hello job and select **Pipeline**.</span></span> <span data-ttu-id="068fb-155">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="068fb-155">Click **OK**.</span></span>
* <span data-ttu-id="068fb-156">Klikněte na tlačítko hello **kanálu** kartě Další.</span><span class="sxs-lookup"><span data-stu-id="068fb-156">Click hello **Pipeline** tab next.</span></span> 
* <span data-ttu-id="068fb-157">Pro **definice**, vyberte **kanálu skript z SCM**.</span><span class="sxs-lookup"><span data-stu-id="068fb-157">For **Definition**, select **Pipeline script from SCM**.</span></span>
* <span data-ttu-id="068fb-158">Pro **SCM**, vyberte **Git**.</span><span class="sxs-lookup"><span data-stu-id="068fb-158">For **SCM**, select **Git**.</span></span>
* <span data-ttu-id="068fb-159">Zadejte hello Githubu adresu URL pro váš forked úložišti: https:\<vaše forked úložišti\>.git</span><span class="sxs-lookup"><span data-stu-id="068fb-159">Enter hello GitHub URL for your forked repo: https:\<your forked repo\>.git</span></span>
* <span data-ttu-id="068fb-160">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="068fb-160">Click **Save**</span></span>

## <a name="test-your-pipeline"></a><span data-ttu-id="068fb-161">Testování vaší kanálu</span><span class="sxs-lookup"><span data-stu-id="068fb-161">Test your pipeline</span></span>
* <span data-ttu-id="068fb-162">Přejděte toohello kanálu, které jste vytvořili, klikněte na tlačítko **nyní sestavení**</span><span class="sxs-lookup"><span data-stu-id="068fb-162">Go toohello pipeline you created, click **Build Now**</span></span>
* <span data-ttu-id="068fb-163">Sestavení uspěli za několik sekund, a můžete se vrátit toohello sestavení a klikněte na tlačítko **výstup konzoly** toosee hello podrobnosti</span><span class="sxs-lookup"><span data-stu-id="068fb-163">A build should succeed in a few seconds, and you can go toohello build and click **Console Output** toosee hello details</span></span>

## <a name="verify-your-web-app"></a><span data-ttu-id="068fb-164">Ověření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="068fb-164">Verify your web app</span></span>
<span data-ttu-id="068fb-165">soubor WAR hello tooverify úspěšně nasadil tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="068fb-165">tooverify hello WAR file is deployed successfully tooyour web app.</span></span> <span data-ttu-id="068fb-166">Otevřete webový prohlížeč:</span><span class="sxs-lookup"><span data-stu-id="068fb-166">Open a web browser:</span></span>

* <span data-ttu-id="068fb-167">Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/ping</span><span class="sxs-lookup"><span data-stu-id="068fb-167">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/ping</span></span>  
<span data-ttu-id="068fb-168">Zobrazí:</span><span class="sxs-lookup"><span data-stu-id="068fb-168">You see:</span></span>

        Welcome tooJava Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* <span data-ttu-id="068fb-169">Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Nahraďte &lt;x > a &lt;y > s všechna čísla) tooget hello součet hodnot x a y</span><span class="sxs-lookup"><span data-stu-id="068fb-169">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) tooget hello sum of x and y</span></span>

![Kalkulačky: Přidat](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-tooazure-web-app-on-linux"></a><span data-ttu-id="068fb-171">Nasazení tooAzure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="068fb-171">Deploy tooAzure Web App on Linux</span></span>
<span data-ttu-id="068fb-172">Teď, když víte, jak toouse rozhraní příkazového řádku Azure ve vašem volaných kanálu, můžete upravit hello skriptu toodeploy tooan webové aplikace Azure v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="068fb-172">Now that you know how toouse Azure CLI in your Jenkins pipeline, you can modify hello script toodeploy tooan Azure Web App on Linux.</span></span>

<span data-ttu-id="068fb-173">Webové aplikace v systému Linux podporuje nasazení hello toodo jiný způsob, který je toouse Docker.</span><span class="sxs-lookup"><span data-stu-id="068fb-173">Web App on Linux supports a different way toodo hello deployment, which is toouse Docker.</span></span> <span data-ttu-id="068fb-174">toodeploy, musíte tooprovide soubor Docker, který balíčky vaší webové aplikace s běh služby do bitové kopie Docker.</span><span class="sxs-lookup"><span data-stu-id="068fb-174">toodeploy, you need tooprovide a Dockerfile that packages your web app with service runtime into a Docker image.</span></span> <span data-ttu-id="068fb-175">modul plug-in Hello pak sestavení hello bitové kopie, poslat ho tooa Docker registru a nasazení hello image tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="068fb-175">hello plugin will then build hello image, push it tooa Docker registry and deploy hello image tooyour web app.</span></span>

* <span data-ttu-id="068fb-176">Postupujte podle kroků hello [sem](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate na Azure webová aplikace spuštěná v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="068fb-176">Follow hello steps [here](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate an Azure Web App running on Linux.</span></span>
* <span data-ttu-id="068fb-177">Nainstalujte Docker ve vaší instanci volaných podle hello pokyny v tomto [článku](https://docs.docker.com/engine/installation/linux/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="068fb-177">Install Docker on your Jenkins instance by following hello instructions in this [article](https://docs.docker.com/engine/installation/linux/ubuntu/).</span></span>
* <span data-ttu-id="068fb-178">Vytvoření kontejneru registru v hello portálu Azure pomocí kroků hello [zde](/azure/container-registry/container-registry-get-started-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="068fb-178">Create a Container Registry in hello Azure portal by using hello steps [here](/azure/container-registry/container-registry-get-started-azure-cli).</span></span>
* <span data-ttu-id="068fb-179">V hello stejné [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) úložišti vám forked, upravit hello **Jenkinsfile2** souboru:</span><span class="sxs-lookup"><span data-stu-id="068fb-179">In hello same [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) repo you forked, edit hello **Jenkinsfile2** file:</span></span>
    * <span data-ttu-id="068fb-180">Řádek 18 21, aktualizovat názvy toohello skupinu prostředků, webové aplikace a ACR v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="068fb-180">Line 18-21, update toohello names of your resource group, web app, and ACR respectively.</span></span> 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * <span data-ttu-id="068fb-181">Řádek 24, aktualizace \<azsrvprincipal\> ID tooyour pověření</span><span class="sxs-lookup"><span data-stu-id="068fb-181">Line 24, update \<azsrvprincipal\> tooyour credential ID</span></span>
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* <span data-ttu-id="068fb-182">Vytvořit nový kanál volaných stejně jako při nasazení tooAzure webové aplikace v systému Windows, pouze tentokrát použijte **Jenkinsfile2** místo.</span><span class="sxs-lookup"><span data-stu-id="068fb-182">Create a new Jenkins pipeline as you did when deploying tooAzure web app in Windows, only this time, use **Jenkinsfile2** instead.</span></span>
* <span data-ttu-id="068fb-183">Spustíte novou úlohu.</span><span class="sxs-lookup"><span data-stu-id="068fb-183">Run your new job.</span></span>
* <span data-ttu-id="068fb-184">tooverify v Azure CLI, spusťte:</span><span class="sxs-lookup"><span data-stu-id="068fb-184">tooverify, in Azure CLI, run:</span></span>

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    <span data-ttu-id="068fb-185">Můžete získat hello následující výsledek:</span><span class="sxs-lookup"><span data-stu-id="068fb-185">You get hello following result:</span></span>
    
    ```
    [
    "calculator"
    ]
    ```
    
    <span data-ttu-id="068fb-186">Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/ping.</span><span class="sxs-lookup"><span data-stu-id="068fb-186">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/ping.</span></span> <span data-ttu-id="068fb-187">Zobrazí zpráva hello:</span><span class="sxs-lookup"><span data-stu-id="068fb-187">You see hello message:</span></span> 
    
        Welcome tooJava Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    <span data-ttu-id="068fb-188">Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Nahraďte &lt;x > a &lt;y > s všechna čísla) tooget hello součet hodnot x a y</span><span class="sxs-lookup"><span data-stu-id="068fb-188">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) tooget hello sum of x and y</span></span>
    
## <a name="next-steps"></a><span data-ttu-id="068fb-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="068fb-189">Next steps</span></span>
<span data-ttu-id="068fb-190">V tomto kurzu jste nakonfigurovali volaných kanál, který rezervuje hello zdrojový kód v úložišti GitHub.</span><span class="sxs-lookup"><span data-stu-id="068fb-190">In this tutorial, you configured a Jenkins pipeline that checks out hello source code in GitHub repo.</span></span> <span data-ttu-id="068fb-191">Spustí soubor war Maven toobuild a pak používá tooAzure toodeploy rozhraní příkazového řádku Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="068fb-191">Runs Maven toobuild a war file and then uses Azure CLI toodeploy tooAzure App Service.</span></span> <span data-ttu-id="068fb-192">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="068fb-192">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="068fb-193">Vytvoření virtuálního počítače volaných</span><span class="sxs-lookup"><span data-stu-id="068fb-193">Create a Jenkins VM</span></span>
> * <span data-ttu-id="068fb-194">Konfigurace volaných</span><span class="sxs-lookup"><span data-stu-id="068fb-194">Configure Jenkins</span></span>
> * <span data-ttu-id="068fb-195">Vytvoření webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="068fb-195">Create a web app in Azure</span></span>
> * <span data-ttu-id="068fb-196">Příprava úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="068fb-196">Prepare a GitHub repository</span></span>
> * <span data-ttu-id="068fb-197">Vytvoření kanálu volaných</span><span class="sxs-lookup"><span data-stu-id="068fb-197">Create Jenkins pipeline</span></span>
> * <span data-ttu-id="068fb-198">Spusťte hello kanálu a ověřte hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="068fb-198">Run hello pipeline and verify hello web app</span></span>
