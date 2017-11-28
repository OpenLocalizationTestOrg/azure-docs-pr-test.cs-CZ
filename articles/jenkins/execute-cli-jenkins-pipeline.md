---
title: "Spuštění rozhraní Azure CLI s volaných | Microsoft Docs"
description: "Naučte se používat rozhraní příkazového řádku Azure k nasazení webové aplikace v jazyce Java do Azure v volaných kanálu"
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
ms.openlocfilehash: 5ca8338d4bf343f08fe70081cff755fa76a126a9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-to-azure-app-service-with-jenkins-and-the-azure-cli"></a><span data-ttu-id="5b44d-103">Nasazení do Azure App Service pomocí volaných a rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5b44d-103">Deploy to Azure App Service with Jenkins and the Azure CLI</span></span>
<span data-ttu-id="5b44d-104">Chcete-li nasadit webovou aplikaci Java do Azure, můžete použít rozhraní příkazového řádku Azure v [volaných kanálu](https://jenkins.io/doc/book/pipeline/).</span><span class="sxs-lookup"><span data-stu-id="5b44d-104">To deploy a Java web app to Azure, you can use Azure CLI in [Jenkins Pipeline](https://jenkins.io/doc/book/pipeline/).</span></span> <span data-ttu-id="5b44d-105">V tomto kurzu vytvoříte kanál CI/CD na virtuální počítač Azure včetně postup:</span><span class="sxs-lookup"><span data-stu-id="5b44d-105">In this tutorial, you create a CI/CD pipeline on an Azure VM including how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5b44d-106">Vytvoření virtuálního počítače volaných</span><span class="sxs-lookup"><span data-stu-id="5b44d-106">Create a Jenkins VM</span></span>
> * <span data-ttu-id="5b44d-107">Konfigurace volaných</span><span class="sxs-lookup"><span data-stu-id="5b44d-107">Configure Jenkins</span></span>
> * <span data-ttu-id="5b44d-108">Vytvoření webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="5b44d-108">Create a web app in Azure</span></span>
> * <span data-ttu-id="5b44d-109">Příprava úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="5b44d-109">Prepare a GitHub repository</span></span>
> * <span data-ttu-id="5b44d-110">Vytvoření kanálu volaných</span><span class="sxs-lookup"><span data-stu-id="5b44d-110">Create Jenkins pipeline</span></span>
> * <span data-ttu-id="5b44d-111">Spusťte kanál a ověřte webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="5b44d-111">Run the pipeline and verify the web app</span></span>

<span data-ttu-id="5b44d-112">Tento kurz vyžaduje Azure CLI verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5b44d-112">This tutorial requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="5b44d-113">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="5b44d-113">To find the version, run `az --version`.</span></span> <span data-ttu-id="5b44d-114">Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="5b44d-114">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a><span data-ttu-id="5b44d-115">Vytvořit a nakonfigurovat volaných instance</span><span class="sxs-lookup"><span data-stu-id="5b44d-115">Create and Configure Jenkins instance</span></span>
<span data-ttu-id="5b44d-116">Pokud již nemáte volaných master, začínat [šablona řešení](install-jenkins-solution-template.md), což zahrnuje požadované [přihlašovací údaje Azure](https://plugins.jenkins.io/azure-credentials) modulu plug-in ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="5b44d-116">If you do not already have a Jenkins master, start with the [Solution Template](install-jenkins-solution-template.md), which includes the required [Azure Credentials](https://plugins.jenkins.io/azure-credentials) plugin by default.</span></span> 

<span data-ttu-id="5b44d-117">Modul plug-in přihlašovacích údajů Azure umožňuje ukládání ve volaných hlavní přihlašovací údaje služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="5b44d-117">The Azure Credential plugin allows you to store Microsoft Azure service principal credentials in Jenkins.</span></span> <span data-ttu-id="5b44d-118">Ve verzi 1.2 jsme doplnili podporu, tak, že volaných kanálu můžete získat přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="5b44d-118">In version 1.2, we added the support so that Jenkins Pipeline can get the Azure credentials.</span></span> 

<span data-ttu-id="5b44d-119">Ujistěte se, že máte verze 1.2 nebo vyšší:</span><span class="sxs-lookup"><span data-stu-id="5b44d-119">Ensure you have version 1.2 or later:</span></span>
* <span data-ttu-id="5b44d-120">V rámci volaných řídicí panel, klikněte na **volaných spravovat -> modul plug-in Manager ->** a vyhledejte **přihlašovacích údajů Azure**.</span><span class="sxs-lookup"><span data-stu-id="5b44d-120">Within the Jenkins dashboard, click **Manage Jenkins -> Plugin Manager ->** and search for **Azure Credential**.</span></span> 
* <span data-ttu-id="5b44d-121">Tento modul plug-in aktualizace, pokud je verze starší než 1.2.</span><span class="sxs-lookup"><span data-stu-id="5b44d-121">Update the plugin if the version is earlier than 1.2.</span></span>

<span data-ttu-id="5b44d-122">V hlavním volaných také vyžadují Java JDK a Maven.</span><span class="sxs-lookup"><span data-stu-id="5b44d-122">Java JDK and Maven are also required in the Jenkins master.</span></span> <span data-ttu-id="5b44d-123">Pokud chcete nainstalovat, přihlaste se k hlavní volaných pomocí protokolu SSH a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5b44d-123">To install, log in to Jenkins master using SSH and run the following commands:</span></span>
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-to-jenkins-credential"></a><span data-ttu-id="5b44d-124">Přidat objekt zabezpečení služby Azure k volaných pověření</span><span class="sxs-lookup"><span data-stu-id="5b44d-124">Add Azure service principal to Jenkins credential</span></span>

<span data-ttu-id="5b44d-125">Azure přihlašovacích údajů je potřeba provést rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5b44d-125">An Azure credential is needed to execute Azure CLI.</span></span>

* <span data-ttu-id="5b44d-126">V rámci volaných řídicí panel, klikněte na **pověření -> Systém ->**.</span><span class="sxs-lookup"><span data-stu-id="5b44d-126">Within the Jenkins dashboard, click **Credentials -> System ->**.</span></span> <span data-ttu-id="5b44d-127">Klikněte na tlačítko **globální credentials(unrestricted)**.</span><span class="sxs-lookup"><span data-stu-id="5b44d-127">Click **Global credentials(unrestricted)**.</span></span>
* <span data-ttu-id="5b44d-128">Klikněte na tlačítko **přidat přihlašovací údaje** přidat [objektu služby Microsoft Azure](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) tak, že vyplníte ID předplatného, ID klienta, sdílený tajný klíč klienta a koncový bod tokenu OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="5b44d-128">Click **Add Credentials** to add a [Microsoft Azure service principal](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) by filling out the Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span> <span data-ttu-id="5b44d-129">Zadejte ID pro použití v následném kroku.</span><span class="sxs-lookup"><span data-stu-id="5b44d-129">Provide an ID for use in subsequent step.</span></span>

![Přidejte pověření](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-the-java-web-app"></a><span data-ttu-id="5b44d-131">Vytvoření Azure App Service pro nasazení webové aplikace Java</span><span class="sxs-lookup"><span data-stu-id="5b44d-131">Create an Azure App Service for deploying the Java web app</span></span>

<span data-ttu-id="5b44d-132">Vytvořit plán aplikační služby Azure s **volné** cenová úroveň pomocí [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) rozhraní příkazového řádku příkaz.</span><span class="sxs-lookup"><span data-stu-id="5b44d-132">Create an Azure App Service plan with the **FREE** pricing tier using the  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="5b44d-133">Plán aplikační služby definuje fyzické prostředky, které jsou použity k hostování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b44d-133">The appservice plan defines the physical resources used to host your apps.</span></span> <span data-ttu-id="5b44d-134">Všechny aplikace, které jsou přiřazené plán služby App Service sdílení těchto prostředků, což umožňuje uložit nákladů při hostování více aplikací.</span><span class="sxs-lookup"><span data-stu-id="5b44d-134">All applications assigned to an appservice plan share these resources, allowing you to save cost when hosting multiple apps.</span></span> 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

<span data-ttu-id="5b44d-135">Až bude plán připravena, rozhraní příkazového řádku Azure ukazuje podobné výstupu v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="5b44d-135">When the plan is ready, the Azure CLI shows similar output to the following example:</span></span>

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

### <a name="create-an-azure-web-app"></a><span data-ttu-id="5b44d-136">Vytvoření webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="5b44d-136">Create an Azure Web app</span></span>

 <span data-ttu-id="5b44d-137">Použití [az webapp vytvořit](/cli/azure/appservice/web#create) rozhraní příkazového řádku příkaz k vytvoření definice webové aplikace v `myAppServicePlan` plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="5b44d-137">Use the [az webapp create](/cli/azure/appservice/web#create) CLI command to create a web app definition in the `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="5b44d-138">Definice webové aplikace adresa URL pro přístup k vaší aplikace pomocí poskytuje a konfiguruje celou řadu možností pro nasazení kódu do Azure.</span><span class="sxs-lookup"><span data-stu-id="5b44d-138">The web app definition provides a URL to access your application with and configures several options to deploy your code to Azure.</span></span> 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

<span data-ttu-id="5b44d-139">Nahraďte `<app_name>` zástupný symbol vlastní jedinečným názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b44d-139">Substitute the `<app_name>` placeholder with your own unique app name.</span></span> <span data-ttu-id="5b44d-140">Tento jedinečný název je součástí výchozí název domény pro webovou aplikaci, tak název musí být jedinečný v rámci všech aplikací v Azure.</span><span class="sxs-lookup"><span data-stu-id="5b44d-140">This unique name is part of the default domain name for the web app, so the name needs to be unique across all apps in Azure.</span></span> <span data-ttu-id="5b44d-141">Můžete namapovat zadání názvu vlastní domény do webové aplikace ještě před zveřejněním pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="5b44d-141">You can map a custom domain name entry to the web app before you expose it to your users.</span></span>

<span data-ttu-id="5b44d-142">Při definici webové aplikace je připraven, rozhraní příkazového řádku Azure uvádí informace podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="5b44d-142">When the web app definition is ready, the Azure CLI shows information similar to the following example:</span></span> 

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

### <a name="configure-java"></a><span data-ttu-id="5b44d-143">Konfigurace Java</span><span class="sxs-lookup"><span data-stu-id="5b44d-143">Configure Java</span></span> 

<span data-ttu-id="5b44d-144">Nastavení konfigurace modulu runtime Java, která vaše aplikace, musí se [aktualizace konfigurace webové služby App Service az](/cli/azure/appservice/web/config#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="5b44d-144">Set up the Java runtime configuration that your app needs with the  [az appservice web config update](/cli/azure/appservice/web/config#update) command.</span></span>

<span data-ttu-id="5b44d-145">Následující příkaz nakonfiguruje webové aplikace ke spuštění na poslední JDK 8 Java a [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="5b44d-145">The following command configures the web app to run on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a><span data-ttu-id="5b44d-146">Příprava úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="5b44d-146">Prepare a GitHub Repository</span></span>
<span data-ttu-id="5b44d-147">Otevřete [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) úložišti.</span><span class="sxs-lookup"><span data-stu-id="5b44d-147">Open the [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) repo.</span></span> <span data-ttu-id="5b44d-148">Chcete-li rozvětvit úložiště k účtu GitHub, klikněte **rozvětvení** tlačítko v horním pravém rohu.</span><span class="sxs-lookup"><span data-stu-id="5b44d-148">To fork the repo to your own GitHub account, click the **Fork** button in the top right-hand corner.</span></span>

* <span data-ttu-id="5b44d-149">V Githubu webového uživatelského rozhraní, otevřete **Jenkinsfile** souboru.</span><span class="sxs-lookup"><span data-stu-id="5b44d-149">In GitHub web UI, open **Jenkinsfile** file.</span></span> <span data-ttu-id="5b44d-150">Klikněte na ikonu tužky upravit tento soubor se aktualizují skupinu prostředků a název webové aplikace na řádku 20 a 21 v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="5b44d-150">Click the pencil icon to edit this file to update the resource group and name of your web app on line 20 and 21 respectively.</span></span>

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* <span data-ttu-id="5b44d-151">Změňte řádek 23 aktualizovat ID pověření v instanci volaných</span><span class="sxs-lookup"><span data-stu-id="5b44d-151">Change line 23 to update credential ID in your Jenkins instance</span></span>

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a><span data-ttu-id="5b44d-152">Vytvoření kanálu volaných</span><span class="sxs-lookup"><span data-stu-id="5b44d-152">Create Jenkins pipeline</span></span>
<span data-ttu-id="5b44d-153">Otevřete volaných ve webovém prohlížeči, klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="5b44d-153">Open Jenkins in a web browser, click **New Item**.</span></span> 

* <span data-ttu-id="5b44d-154">Zadejte název pro úlohy, vyberte **kanálu**.</span><span class="sxs-lookup"><span data-stu-id="5b44d-154">Provide a name for the job and select **Pipeline**.</span></span> <span data-ttu-id="5b44d-155">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b44d-155">Click **OK**.</span></span>
* <span data-ttu-id="5b44d-156">Klikněte **kanálu** kartě Další.</span><span class="sxs-lookup"><span data-stu-id="5b44d-156">Click the **Pipeline** tab next.</span></span> 
* <span data-ttu-id="5b44d-157">Pro **definice**, vyberte **kanálu skript z SCM**.</span><span class="sxs-lookup"><span data-stu-id="5b44d-157">For **Definition**, select **Pipeline script from SCM**.</span></span>
* <span data-ttu-id="5b44d-158">Pro **SCM**, vyberte **Git**.</span><span class="sxs-lookup"><span data-stu-id="5b44d-158">For **SCM**, select **Git**.</span></span>
* <span data-ttu-id="5b44d-159">Zadejte adresu URL webu GitHub pro vaše forked úložišti: https:\<vaše forked úložišti\>.git</span><span class="sxs-lookup"><span data-stu-id="5b44d-159">Enter the GitHub URL for your forked repo: https:\<your forked repo\>.git</span></span>
* <span data-ttu-id="5b44d-160">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="5b44d-160">Click **Save**</span></span>

## <a name="test-your-pipeline"></a><span data-ttu-id="5b44d-161">Testování vaší kanálu</span><span class="sxs-lookup"><span data-stu-id="5b44d-161">Test your pipeline</span></span>
* <span data-ttu-id="5b44d-162">Přejděte do kanálu, který jste vytvořili, klikněte na tlačítko **nyní sestavení**</span><span class="sxs-lookup"><span data-stu-id="5b44d-162">Go to the pipeline you created, click **Build Now**</span></span>
* <span data-ttu-id="5b44d-163">Sestavení uspěli za několik sekund, a můžete přejít do sestavení a klikněte na tlačítko **výstup konzoly** a zobrazit podrobnosti</span><span class="sxs-lookup"><span data-stu-id="5b44d-163">A build should succeed in a few seconds, and you can go to the build and click **Console Output** to see the details</span></span>

## <a name="verify-your-web-app"></a><span data-ttu-id="5b44d-164">Ověření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="5b44d-164">Verify your web app</span></span>
<span data-ttu-id="5b44d-165">Chcete-li ověřit WAR souboru úspěšně nasazena do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b44d-165">To verify the WAR file is deployed successfully to your web app.</span></span> <span data-ttu-id="5b44d-166">Otevřete webový prohlížeč:</span><span class="sxs-lookup"><span data-stu-id="5b44d-166">Open a web browser:</span></span>

* <span data-ttu-id="5b44d-167">Přejděte na http://&lt;app_name >.azurewebsites.net/api/calculator/ping</span><span class="sxs-lookup"><span data-stu-id="5b44d-167">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/ping</span></span>  
<span data-ttu-id="5b44d-168">Zobrazí:</span><span class="sxs-lookup"><span data-stu-id="5b44d-168">You see:</span></span>

        Welcome to Java Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* <span data-ttu-id="5b44d-169">Přejděte na http://&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Nahraďte &lt;x > a &lt;y > s všechna čísla) Chcete-li získat součet hodnot x a y</span><span class="sxs-lookup"><span data-stu-id="5b44d-169">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) to get the sum of x and y</span></span>

![Kalkulačky: Přidat](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-to-azure-web-app-on-linux"></a><span data-ttu-id="5b44d-171">Nasazení do Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="5b44d-171">Deploy to Azure Web App on Linux</span></span>
<span data-ttu-id="5b44d-172">Teď, když víte, jak používat rozhraní příkazového řádku Azure v svůj volaných kanál, můžete upravit skript, který chcete nasadit do webové aplikace Azure v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="5b44d-172">Now that you know how to use Azure CLI in your Jenkins pipeline, you can modify the script to deploy to an Azure Web App on Linux.</span></span>

<span data-ttu-id="5b44d-173">Webové aplikace v systému Linux podporuje jiný způsob, jak provést nasazení, který má používat Docker.</span><span class="sxs-lookup"><span data-stu-id="5b44d-173">Web App on Linux supports a different way to do the deployment, which is to use Docker.</span></span> <span data-ttu-id="5b44d-174">Pokud chcete nasadit, musíte zadat soubor Docker, který balíčky vaší webové aplikace s běh služby do bitové kopie Docker.</span><span class="sxs-lookup"><span data-stu-id="5b44d-174">To deploy, you need to provide a Dockerfile that packages your web app with service runtime into a Docker image.</span></span> <span data-ttu-id="5b44d-175">Tento modul plug-in pak vytvořit bitovou kopii, poslat ho přímo Docker registru a nasazení bitové kopie do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b44d-175">The plugin will then build the image, push it to a Docker registry and deploy the image to your web app.</span></span>

* <span data-ttu-id="5b44d-176">Postupujte podle kroků [sem](/azure/app-service-web/app-service-linux-how-to-create-web-app) k vytvoření webové aplikace Azure systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="5b44d-176">Follow the steps [here](/azure/app-service-web/app-service-linux-how-to-create-web-app) to create an Azure Web App running on Linux.</span></span>
* <span data-ttu-id="5b44d-177">Podle pokynů v této instalaci Docker ve vaší instanci volaných [článku](https://docs.docker.com/engine/installation/linux/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="5b44d-177">Install Docker on your Jenkins instance by following the instructions in this [article](https://docs.docker.com/engine/installation/linux/ubuntu/).</span></span>
* <span data-ttu-id="5b44d-178">Vytvoření kontejneru registru na portálu Azure pomocí kroků [zde](/azure/container-registry/container-registry-get-started-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="5b44d-178">Create a Container Registry in the Azure portal by using the steps [here](/azure/container-registry/container-registry-get-started-azure-cli).</span></span>
* <span data-ttu-id="5b44d-179">Ve stejném [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) forked úložišti, upravit **Jenkinsfile2** souboru:</span><span class="sxs-lookup"><span data-stu-id="5b44d-179">In the same [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) repo you forked, edit the **Jenkinsfile2** file:</span></span>
    * <span data-ttu-id="5b44d-180">Řádek 18 21, aktualizujte na názvy skupiny prostředků, webové aplikace a ACR v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="5b44d-180">Line 18-21, update to the names of your resource group, web app, and ACR respectively.</span></span> 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * <span data-ttu-id="5b44d-181">Řádek 24, aktualizace \<azsrvprincipal\> na vaše ID přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="5b44d-181">Line 24, update \<azsrvprincipal\> to your credential ID</span></span>
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* <span data-ttu-id="5b44d-182">Vytvořit nový kanál volaných, protože při nasazení do Azure webové aplikace v systému Windows, pouze v tomto případě, použijete **Jenkinsfile2** místo.</span><span class="sxs-lookup"><span data-stu-id="5b44d-182">Create a new Jenkins pipeline as you did when deploying to Azure web app in Windows, only this time, use **Jenkinsfile2** instead.</span></span>
* <span data-ttu-id="5b44d-183">Spustíte novou úlohu.</span><span class="sxs-lookup"><span data-stu-id="5b44d-183">Run your new job.</span></span>
* <span data-ttu-id="5b44d-184">Pokud chcete ověřit, v Azure CLI, spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="5b44d-184">To verify, in Azure CLI, run:</span></span>

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    <span data-ttu-id="5b44d-185">Získáte následující výsledek:</span><span class="sxs-lookup"><span data-stu-id="5b44d-185">You get the following result:</span></span>
    
    ```
    [
    "calculator"
    ]
    ```
    
    <span data-ttu-id="5b44d-186">Přejděte na http://&lt;app_name >.azurewebsites.net/api/calculator/ping.</span><span class="sxs-lookup"><span data-stu-id="5b44d-186">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/ping.</span></span> <span data-ttu-id="5b44d-187">Zobrazí se zpráva:</span><span class="sxs-lookup"><span data-stu-id="5b44d-187">You see the message:</span></span> 
    
        Welcome to Java Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    <span data-ttu-id="5b44d-188">Přejděte na http://&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Nahraďte &lt;x > a &lt;y > s všechna čísla) Chcete-li získat součet hodnot x a y</span><span class="sxs-lookup"><span data-stu-id="5b44d-188">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) to get the sum of x and y</span></span>
    
## <a name="next-steps"></a><span data-ttu-id="5b44d-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5b44d-189">Next steps</span></span>
<span data-ttu-id="5b44d-190">V tomto kurzu jste nakonfigurovali volaných kanál, který rezervuje zdrojový kód v úložišti GitHub.</span><span class="sxs-lookup"><span data-stu-id="5b44d-190">In this tutorial, you configured a Jenkins pipeline that checks out the source code in GitHub repo.</span></span> <span data-ttu-id="5b44d-191">Spustí Maven k sestavení souboru war a potom pomocí rozhraní příkazového řádku Azure k nasazení do Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5b44d-191">Runs Maven to build a war file and then uses Azure CLI to deploy to Azure App Service.</span></span> <span data-ttu-id="5b44d-192">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="5b44d-192">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5b44d-193">Vytvoření virtuálního počítače volaných</span><span class="sxs-lookup"><span data-stu-id="5b44d-193">Create a Jenkins VM</span></span>
> * <span data-ttu-id="5b44d-194">Konfigurace volaných</span><span class="sxs-lookup"><span data-stu-id="5b44d-194">Configure Jenkins</span></span>
> * <span data-ttu-id="5b44d-195">Vytvoření webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="5b44d-195">Create a web app in Azure</span></span>
> * <span data-ttu-id="5b44d-196">Příprava úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="5b44d-196">Prepare a GitHub repository</span></span>
> * <span data-ttu-id="5b44d-197">Vytvoření kanálu volaných</span><span class="sxs-lookup"><span data-stu-id="5b44d-197">Create Jenkins pipeline</span></span>
> * <span data-ttu-id="5b44d-198">Spusťte kanál a ověřte webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="5b44d-198">Run the pipeline and verify the web app</span></span>
