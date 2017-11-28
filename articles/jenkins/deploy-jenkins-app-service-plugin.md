---
title: "Nasazení do Azure App Service pomocí modulu plug-in volaných | Microsoft Docs"
description: "Další informace o použití modulu plug-in Azure App Service volaných nasazení webové aplikace v jazyce Java do Azure ve volaných"
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 7/24/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 646daad1785f3de067544b6dd38abfcb6bc67d4a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-to-azure-app-service-with-jenkins-plugin"></a><span data-ttu-id="12816-103">Nasazení do Azure App Service pomocí modulu plug-in volaných</span><span class="sxs-lookup"><span data-stu-id="12816-103">Deploy to Azure App Service with Jenkins plugin</span></span> 
<span data-ttu-id="12816-104">Nasadit webovou aplikaci Java do Azure, můžete použít rozhraní příkazového řádku Azure v [volaných kanálu](/azure/jenkins/execute-cli-jenkins-pipeline) můžete taky použít [modul plug-in Azure App Service volaných](https://plugins.jenkins.io/azure-app-service).</span><span class="sxs-lookup"><span data-stu-id="12816-104">To deploy a Java web app to Azure, you can use Azure CLI in [Jenkins Pipeline](/azure/jenkins/execute-cli-jenkins-pipeline) or you can make use of the [Azure App Service Jenkins plugin](https://plugins.jenkins.io/azure-app-service).</span></span> <span data-ttu-id="12816-105">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="12816-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12816-106">Konfigurace volaných k nasazení do Azure App Service pomocí služby FTP</span><span class="sxs-lookup"><span data-stu-id="12816-106">Configure Jenkins to deploy to Azure App Service through FTP</span></span> 
> * <span data-ttu-id="12816-107">Konfigurace volaných k nasazení do Azure App Service v Linuxu prostřednictvím Docker</span><span class="sxs-lookup"><span data-stu-id="12816-107">Configure Jenkins to deploy to Azure App Service on Linux through Docker</span></span> 

## <a name="create-and-configure-jenkins-instance"></a><span data-ttu-id="12816-108">Vytvořit a nakonfigurovat volaných instance</span><span class="sxs-lookup"><span data-stu-id="12816-108">Create and Configure Jenkins instance</span></span>
<span data-ttu-id="12816-109">Pokud již nemáte volaných master, začínat [šablona řešení](install-jenkins-solution-template.md), což zahrnuje JDK8 a následující požadované moduly plug-in:</span><span class="sxs-lookup"><span data-stu-id="12816-109">If you do not already have a Jenkins master, start with the [Solution Template](install-jenkins-solution-template.md), which includes JDK8 and the following required plugins:</span></span>

* <span data-ttu-id="12816-110">[Modul plug-in klienta volaných Git](https://plugins.jenkins.io/git-client) v.2.4.6</span><span class="sxs-lookup"><span data-stu-id="12816-110">[Jenkins Git client plugin](https://plugins.jenkins.io/git-client) v.2.4.6</span></span> 
* <span data-ttu-id="12816-111">[Modul plug-in docker Commons](https://plugins.jenkins.io/docker-commons) v.1.4.0</span><span class="sxs-lookup"><span data-stu-id="12816-111">[Docker Commons Plugin](https://plugins.jenkins.io/docker-commons) v.1.4.0</span></span>
* <span data-ttu-id="12816-112">[Přihlašovací údaje Azure](https://plugins.jenkins.io/azure-credentials) v.1.2</span><span class="sxs-lookup"><span data-stu-id="12816-112">[Azure Credentials](https://plugins.jenkins.io/azure-credentials) v.1.2</span></span>
* <span data-ttu-id="12816-113">[Aplikační služba Azure](https://plugins.jenkins.io/azure-app-server) v.0.1</span><span class="sxs-lookup"><span data-stu-id="12816-113">[Azure App Service](https://plugins.jenkins.io/azure-app-server) v.0.1</span></span>

<span data-ttu-id="12816-114">Modul plug-in služby App Service můžete nasadit webovou aplikaci ve všech jazycích (například C#, PHP, Java a node.js atd.), které jsou podporované službou Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="12816-114">You can use the App Service plugin to deploy Web App in all languages (for instance, C#, PHP, Java, and node.js etc.) supported by Azure App Service.</span></span> <span data-ttu-id="12816-115">V tomto kurzu používáme ukázkovou aplikaci Java, [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample).</span><span class="sxs-lookup"><span data-stu-id="12816-115">In this tutorial, we are using the sample Java app, [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample).</span></span> <span data-ttu-id="12816-116">Chcete-li rozvětvit úložiště k účtu GitHub, klikněte **rozvětvení** tlačítko v horním pravém rohu.</span><span class="sxs-lookup"><span data-stu-id="12816-116">To fork the repo to your own GitHub account, click the **Fork** button in the top right-hand corner.</span></span>  

<span data-ttu-id="12816-117">Java JDK a Maven jsou vyžadovány pro sestavení projektu Java.</span><span class="sxs-lookup"><span data-stu-id="12816-117">Java JDK and Maven are required for building the Java project.</span></span> <span data-ttu-id="12816-118">Ujistěte se, že pokud použijete jeden pro nepřetržitou integraci se v hlavním volaných nebo agenta virtuálního počítače nainstalovat součásti.</span><span class="sxs-lookup"><span data-stu-id="12816-118">Make sure you install the components in the Jenkins master or the VM agent if you use one for continuous integration.</span></span> 

<span data-ttu-id="12816-119">Pokud chcete nainstalovat, přihlaste se k instanci volaných pomocí SSH a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="12816-119">To install, log in to the Jenkins instance using SSH and run the following commands:</span></span>

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

<span data-ttu-id="12816-120">Pro nasazení do služby App Service v systému Linux, musíte taky nainstalovat Docker na hlavním serveru volaných nebo agenta virtuálního počítače použít pro sestavení.</span><span class="sxs-lookup"><span data-stu-id="12816-120">For deploying to App Service on Linux, you also need to install Docker on the Jenkins master or the VM agent used for build.</span></span> <span data-ttu-id="12816-121">Najdete v tomto článku k instalaci Docker: https://docs.docker.com/engine/installation/linux/ubuntu/.</span><span class="sxs-lookup"><span data-stu-id="12816-121">Refer to this article to install Docker: https://docs.docker.com/engine/installation/linux/ubuntu/.</span></span>

## <a name="add-azure-service-principal-to-jenkins-credential"></a><span data-ttu-id="12816-122">Přidat objekt zabezpečení služby Azure k volaných pověření</span><span class="sxs-lookup"><span data-stu-id="12816-122">Add Azure service principal to Jenkins credential</span></span>

<span data-ttu-id="12816-123">Objektu zabezpečení služby Azure, je potřeba k nasazení do Azure.</span><span class="sxs-lookup"><span data-stu-id="12816-123">An Azure service principal is needed to deploy to Azure.</span></span> 

<ol>
<li><span data-ttu-id="12816-124">Použití [rozhraní příkazového řádku Azure](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) nebo [portál Azure](/azure/azure-resource-manager/resource-group-create-service-principal-portal) k vytvoření objektu služby Azure</span><span class="sxs-lookup"><span data-stu-id="12816-124">Use [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) or [Azure portal](/azure/azure-resource-manager/resource-group-create-service-principal-portal) to create an Azure service principal</span></span></li>
<li><span data-ttu-id="12816-125">V rámci volaných řídicí panel, klikněte na **pověření -> Systém ->**.</span><span class="sxs-lookup"><span data-stu-id="12816-125">Within the Jenkins dashboard, click **Credentials -> System ->**.</span></span> <span data-ttu-id="12816-126">Klikněte na tlačítko **globální credentials(unrestricted)**.</span><span class="sxs-lookup"><span data-stu-id="12816-126">Click **Global credentials(unrestricted)**.</span></span></li>
<li><span data-ttu-id="12816-127">Klikněte na tlačítko **přidat přihlašovací údaje** přidat hlavní název služby Microsoft Azure tak, že vyplníte ID předplatného, ID klienta, sdílený tajný klíč klienta a koncový bod tokenu OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="12816-127">Click **Add Credentials** to add a Microsoft Azure service principal by filling out the Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span> <span data-ttu-id="12816-128">Zadejte ID, **mySp**, pro použití v postupných krocích.</span><span class="sxs-lookup"><span data-stu-id="12816-128">Provide an ID, **mySp**, for use in subsequent steps.</span></span></li>
</ol>

## <a name="azure-app-service-plugin"></a><span data-ttu-id="12816-129">Modul plug-in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="12816-129">Azure App Service plugin</span></span>

<span data-ttu-id="12816-130">V1.0 modul plug-in Azure App Service podporuje průběžné nasazování do webové aplikace Azure prostřednictvím:</span><span class="sxs-lookup"><span data-stu-id="12816-130">Azure App Service plugin v1.0 supports continuous deployment to Azure Web App through:</span></span>

* <span data-ttu-id="12816-131">Git a FTP</span><span class="sxs-lookup"><span data-stu-id="12816-131">Git and FTP</span></span>
* <span data-ttu-id="12816-132">Docker pro webovou aplikaci v systému Linux</span><span class="sxs-lookup"><span data-stu-id="12816-132">Docker for Web App on Linux</span></span>

## <a name="configure-jenkins-to-deploy-web-app-through-ftp-using-the-jenkins-dashboard"></a><span data-ttu-id="12816-133">Konfigurace volaných nasadit webovou aplikaci pomocí služby FTP volaných řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="12816-133">Configure Jenkins to deploy Web App through FTP using the Jenkins dashboard</span></span>

<span data-ttu-id="12816-134">Postup nasazení projektu do webové aplikace Azure, můžete nahrát artefaktů sestavení (například .war souboru v jazyce Java) pomocí Git a FTP.</span><span class="sxs-lookup"><span data-stu-id="12816-134">To deploy your project to Azure Web App, you can upload your build artifacts (for example, .war file in Java) using Git or FTP.</span></span>

<span data-ttu-id="12816-135">Před nastavením pro úlohu ve volaných, potřebujete plán aplikační služby Azure a webovou aplikaci pro spuštění aplikace v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="12816-135">Before setting up the job in Jenkins, you need an Azure App Service plan and a Web App for running the Java app.</span></span>


1. <span data-ttu-id="12816-136">Vytvořit plán aplikační služby Azure s **volné** cenová úroveň pomocí [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) rozhraní příkazového řádku příkaz.</span><span class="sxs-lookup"><span data-stu-id="12816-136">Create an Azure App Service plan with the **FREE** pricing tier using the  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="12816-137">Plán aplikační služby definuje fyzické prostředky, které jsou použity k hostování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="12816-137">The appservice plan defines the physical resources used to host your apps.</span></span> <span data-ttu-id="12816-138">Všechny aplikace, které jsou přiřazené plán služby App Service sdílení těchto prostředků, což umožňuje uložit nákladů při hostování více aplikací.</span><span class="sxs-lookup"><span data-stu-id="12816-138">All applications assigned to an appservice plan share these resources, allowing you to save cost when hosting multiple apps.</span></span>
2. <span data-ttu-id="12816-139">Vytvořte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="12816-139">Create a Web App.</span></span> <span data-ttu-id="12816-140">Můžete použít [portál Azure](/azure/app-service-web/web-sites-configure) nebo použijte následující příkaz Az rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="12816-140">You can either use the [Azure portal](/azure/app-service-web/web-sites-configure) or use the following Az CLI command:</span></span>
```azurecli-interactive 
az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
```

3. <span data-ttu-id="12816-141">Ujistěte se, že nastavíte konfiguraci Java runtime, která vaše aplikace musí.</span><span class="sxs-lookup"><span data-stu-id="12816-141">Make sure you set up the Java runtime configuration that your app needs.</span></span> <span data-ttu-id="12816-142">Následující příkaz rozhraní příkazového řádku Azure nakonfiguruje webové aplikace ke spuštění na poslední JDK 8 Java a [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="12816-142">The following Azure CLI command configures the web app to run on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>
```azurecli-interactive
az webapp config set \
--name <myAppName> \
--resource-group <myResourceGroup> \
--java-version 1.8 \
--java-container Tomcat \
--java-container-version 8.0
```

### <a name="set-up-the-jenkins-job"></a><span data-ttu-id="12816-143">Nastavení úloh volaných</span><span class="sxs-lookup"><span data-stu-id="12816-143">Set up the Jenkins job</span></span>


1. <span data-ttu-id="12816-144">Vytvořte novou **volný styl** projekt v volaných řídicí panel</span><span class="sxs-lookup"><span data-stu-id="12816-144">Create a new **freestyle** project in Jenkins Dashboard</span></span>
2. <span data-ttu-id="12816-145">Konfigurace **správu zdrojového kódu** používat vaše místní pokračovatelem [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) tím, že poskytuje **adresu URL úložiště**.</span><span class="sxs-lookup"><span data-stu-id="12816-145">Configure **Source Code Management** to use your local fork of [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) by providing the **Repository URL**.</span></span> <span data-ttu-id="12816-146">Příklad: http://github.com/&lt;yourID > / javawebappsample.</span><span class="sxs-lookup"><span data-stu-id="12816-146">For example: http://github.com/&lt;yourID>/javawebappsample.</span></span>
3. <span data-ttu-id="12816-147">Přidejte krok sestavení projekt pomocí nástroje Maven sestavíte.</span><span class="sxs-lookup"><span data-stu-id="12816-147">Add a Build step to build the project using Maven.</span></span> <span data-ttu-id="12816-148">Učinit přidáním **spustit prostředí**.</span><span class="sxs-lookup"><span data-stu-id="12816-148">Do so by adding an **Execute shell**.</span></span> <span data-ttu-id="12816-149">V tomto příkladu potřebujeme další krok přejmenujte soubor *.war v cílové složce na ROOT.war.</span><span class="sxs-lookup"><span data-stu-id="12816-149">For this example, we need an additional step to rename the *.war file in target folder to ROOT.war.</span></span>   
```bash
mvn clean package
mv target/*.war target/ROOT.war
```

4. <span data-ttu-id="12816-150">Přidat akci po sestavení tak, že vyberete **publikování webové aplikace Azure**.</span><span class="sxs-lookup"><span data-stu-id="12816-150">Add a post-build action by selecting **Publish an Azure Web App**.</span></span>
5. <span data-ttu-id="12816-151">Zadejte, "mySp" Azure instanční objekt uložená v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="12816-151">Supply, "mySp", the Azure service principal stored in previous step.</span></span>
6. <span data-ttu-id="12816-152">V **konfigurace aplikací** zvolte prostředků skupiny a webové aplikace v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="12816-152">In **App Configuration** section, choose the resource group and web app in your subscription.</span></span> <span data-ttu-id="12816-153">Tento modul plug-in automaticky zjistí, zda webová aplikace je Windows nebo linuxu.</span><span class="sxs-lookup"><span data-stu-id="12816-153">The plugin automatically detects whether the Web App is Windows or Linux-based.</span></span> <span data-ttu-id="12816-154">Pro webovou aplikaci systému Windows se zobrazí možnost "publikovat soubory".</span><span class="sxs-lookup"><span data-stu-id="12816-154">For a Windows-based Web App, the option "Publish Files" is presented.</span></span>
7. <span data-ttu-id="12816-155">Zadejte soubory, které chcete nasadit (například pokud používáte Java war balíčku.) Zdrojový a cílový adresář jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="12816-155">Fill in the files you want to deploy (for example, a war package if you're using Java.) Source Directory and Target Directory are optional.</span></span> <span data-ttu-id="12816-156">Parametry umožňují určit zdrojové a cílové složky při nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="12816-156">The parameters allow you to specify source and target folders when uploading files.</span></span> <span data-ttu-id="12816-157">Webové aplikace v jazyce Java v Azure běží na serveru Tomcat.</span><span class="sxs-lookup"><span data-stu-id="12816-157">Java web app on Azure is run in a Tomcat server.</span></span> <span data-ttu-id="12816-158">Proto můžete nahrávat na server války se balíček umístěna složka webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="12816-158">So you upload you war package to webapps folder.</span></span> <span data-ttu-id="12816-159">V tomto příkladu nastavit **zdrojový adresář** "cílit na" a "webapps" zadat **cílový adresář**.</span><span class="sxs-lookup"><span data-stu-id="12816-159">For this example, set **Source Directory** to "target" and supply "webapps" for **Target Directory**.</span></span>
8. <span data-ttu-id="12816-160">Pokud chcete nasadit do přihrádky než produkční, můžete také nastavit **slotu** název.</span><span class="sxs-lookup"><span data-stu-id="12816-160">If you want to deploy to a slot other than production, you can also set **Slot** Name.</span></span>
9. <span data-ttu-id="12816-161">Uložte projekt a sestavte jej.</span><span class="sxs-lookup"><span data-stu-id="12816-161">Save the project and build it.</span></span> <span data-ttu-id="12816-162">Webové aplikace se nasadí do Azure, po dokončení sestavení.</span><span class="sxs-lookup"><span data-stu-id="12816-162">Your web app is deployed to Azure when build is complete.</span></span>

### <a name="deploy-web-app-through-ftp-using-jenkins-pipeline"></a><span data-ttu-id="12816-163">Nasazení webové aplikace pomocí služby FTP volaných zřetězením příkazů</span><span class="sxs-lookup"><span data-stu-id="12816-163">Deploy Web App through FTP using Jenkins pipeline</span></span>

<span data-ttu-id="12816-164">Tento modul plug-in je připravené pro kanál.</span><span class="sxs-lookup"><span data-stu-id="12816-164">The plugin is pipeline-ready.</span></span> <span data-ttu-id="12816-165">Najdete ukázku v úložišti GitHub.</span><span class="sxs-lookup"><span data-stu-id="12816-165">You can refer to a sample in the GitHub repo.</span></span>

1. <span data-ttu-id="12816-166">V Githubu webového uživatelského rozhraní, otevřete **Jenkinsfile_ftp_plugin** souboru.</span><span class="sxs-lookup"><span data-stu-id="12816-166">In GitHub web UI, open **Jenkinsfile_ftp_plugin** file.</span></span> <span data-ttu-id="12816-167">Klikněte na ikonu tužky upravit tento soubor se aktualizují skupinu prostředků a název webové aplikace na řádku 11 a 12 v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="12816-167">Click the pencil icon to edit this file to update the resource group and name of your web app on line 11 and 12 respectively.</span></span>    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. <span data-ttu-id="12816-168">Změňte řádek 14 až ID přihlašovacích údajů v instanci volaných aktualizace.</span><span class="sxs-lookup"><span data-stu-id="12816-168">Change line 14 to update credential ID in your Jenkins instance.</span></span>    
```java
withCredentials([azureServicePrincipal('<mySp>')]) {
```

### <a name="create-a-jenkins-pipeline"></a><span data-ttu-id="12816-169">Vytvoření kanálu volaných</span><span class="sxs-lookup"><span data-stu-id="12816-169">Create a Jenkins pipeline</span></span>

1. <span data-ttu-id="12816-170">Otevřete volaných ve webovém prohlížeči, klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="12816-170">Open Jenkins in a web browser, click **New Item**.</span></span>
2. <span data-ttu-id="12816-171">Zadejte název pro úlohy, vyberte **kanálu**.</span><span class="sxs-lookup"><span data-stu-id="12816-171">Provide a name for the job and select **Pipeline**.</span></span> <span data-ttu-id="12816-172">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="12816-172">Click **OK**.</span></span>
3. <span data-ttu-id="12816-173">Klikněte **kanálu** kartě Další.</span><span class="sxs-lookup"><span data-stu-id="12816-173">Click the **Pipeline** tab next.</span></span>
4. <span data-ttu-id="12816-174">Pro **definice**, vyberte **kanálu skript z SCM**.</span><span class="sxs-lookup"><span data-stu-id="12816-174">For **Definition**, select **Pipeline script from SCM**.</span></span>
5. <span data-ttu-id="12816-175">Pro **SCM**, vyberte **Git**.</span><span class="sxs-lookup"><span data-stu-id="12816-175">For **SCM**, select **Git**.</span></span> <span data-ttu-id="12816-176">Zadejte adresu URL webu GitHub pro vaše forked úložišti: https:&lt;vaše forked úložišti > .git</span><span class="sxs-lookup"><span data-stu-id="12816-176">Enter the GitHub URL for your forked repo: https:&lt;your forked repo>.git</span></span>
6. <span data-ttu-id="12816-177">Aktualizace **skript cesta** na "Jenkinsfile_ftp_plugin"</span><span class="sxs-lookup"><span data-stu-id="12816-177">Update **Script Path** to "Jenkinsfile_ftp_plugin"</span></span>
7. <span data-ttu-id="12816-178">Klikněte na tlačítko **Uložit** a spusťte úlohu.</span><span class="sxs-lookup"><span data-stu-id="12816-178">Click **Save** and run the job.</span></span>

## <a name="configure-jenkins-to-deploy-web-app-on-linux-through-docker"></a><span data-ttu-id="12816-179">Konfigurace volaných nasazení webové aplikace v systému Linux pomocí Docker</span><span class="sxs-lookup"><span data-stu-id="12816-179">Configure Jenkins to deploy Web App on Linux through Docker</span></span>

<span data-ttu-id="12816-180">Kromě Git/FTP webové aplikace v systému Linux podporuje nasazení pomocí Docker.</span><span class="sxs-lookup"><span data-stu-id="12816-180">Apart from Git/FTP, Web App on Linux supports deployment using Docker.</span></span> <span data-ttu-id="12816-181">Pokud chcete nasadit, pomocí Docker, potřebujete poskytovat soubor Docker, který balíčky vaší webové aplikace s běh služby do bitové kopie docker.</span><span class="sxs-lookup"><span data-stu-id="12816-181">To deploy using Docker, you need to provide a Dockerfile that packages your web app with service runtime into a docker image.</span></span> <span data-ttu-id="12816-182">Potom tento modul plug-in vytvoří bitovou kopii, doručí do registru docker a nasadí bitovou kopii do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="12816-182">Then the plugin builds the image, pushes it to a docker registry and deploys the image to your web app.</span></span>

<span data-ttu-id="12816-183">Webové aplikace v systému Linux také podporuje tradiční způsobů jako Git a FTP, ale jenom pro předdefinované jazyků (.NET Core, Node.js, PHP a Ruby).</span><span class="sxs-lookup"><span data-stu-id="12816-183">Web App on Linux also supports traditional ways like Git and FTP, but only for built-in languages (.NET Core, Node.js, PHP, and Ruby).</span></span> <span data-ttu-id="12816-184">Pro jiné jazyky potřebujete společně balíček runtime kód a služby vaší aplikace do bitové kopie docker a nasadit pomocí docker.</span><span class="sxs-lookup"><span data-stu-id="12816-184">For other languages, you need to package your application code and service runtime together into a docker image and use docker to deploy.</span></span>

<span data-ttu-id="12816-185">Před nastavením pro úlohu ve volaných, potřebujete Azure aplikační službu v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="12816-185">Before setting up the job in Jenkins, you need an Azure app service on Linux.</span></span> <span data-ttu-id="12816-186">Kontejner registru je také potřeba uchovávat a spravovat privátní kontejner imagí Dockeru.</span><span class="sxs-lookup"><span data-stu-id="12816-186">A container registry is also needed to store and manage your private Docker container images.</span></span> <span data-ttu-id="12816-187">Můžete použít DockerHub; v tomto příkladu používáme registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="12816-187">You can use DockerHub; we are using Azure Container Registry for this example.</span></span>

* <span data-ttu-id="12816-188">Můžete provést kroky [sem](/azure/app-service-web/app-service-linux-how-to-create-web-app) k vytvoření webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="12816-188">You can follow the steps [here](/azure/app-service-web/app-service-linux-how-to-create-web-app) to create a Web App on Linux</span></span> 
* <span data-ttu-id="12816-189">Azure registru kontejneru je spravovaný [Docker registru] (https://docs.docker.com/registry/) služby podle 2.0 registru Docker open source.</span><span class="sxs-lookup"><span data-stu-id="12816-189">Azure Container Registry is a managed [Docker registry] (https://docs.docker.com/registry/) service based on the open-source Docker Registry 2.0.</span></span> <span data-ttu-id="12816-190">Postupujte podle kroků [sem] (/ azure/container-registry/container-registry-get-started-azure-cli) Další informace o tom, jak to provést.</span><span class="sxs-lookup"><span data-stu-id="12816-190">Follow the steps [here] (/azure/container-registry/container-registry-get-started-azure-cli) for more guidance on how to do so.</span></span> <span data-ttu-id="12816-191">Můžete také použít DockerHub.</span><span class="sxs-lookup"><span data-stu-id="12816-191">You can also use DockerHub.</span></span>

### <a name="to-deploy-using-docker"></a><span data-ttu-id="12816-192">Nasazení pomocí docker:</span><span class="sxs-lookup"><span data-stu-id="12816-192">To deploy using docker:</span></span>

1. <span data-ttu-id="12816-193">Vytvoření nového projektu volný styl volaných řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="12816-193">Create a new freestyle project in Jenkins Dashboard.</span></span>
2. <span data-ttu-id="12816-194">Konfigurace **správu zdrojového kódu** používat vaše místní pokračovatelem [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) tím, že poskytuje **adresu URL úložiště**.</span><span class="sxs-lookup"><span data-stu-id="12816-194">Configure **Source Code Management** to use your local fork of [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) by providing the **Repository URL**.</span></span> <span data-ttu-id="12816-195">Příklad: http://github.com/&lt;yourid > / javawebappsample.</span><span class="sxs-lookup"><span data-stu-id="12816-195">For example: http://github.com/&lt;yourid>/javawebappsample.</span></span>
<span data-ttu-id="12816-196">Přidejte krok sestavení projekt pomocí nástroje Maven sestavíte.</span><span class="sxs-lookup"><span data-stu-id="12816-196">Add a Build step to build the project using Maven.</span></span> <span data-ttu-id="12816-197">Učinit přidáním **spustit prostředí** a přidejte následující řádek v **příkaz**:</span><span class="sxs-lookup"><span data-stu-id="12816-197">Do so by adding an **Execute shell** and add the following line in **Command**:</span></span>    
```bash
mvn clean package
```

3. <span data-ttu-id="12816-198">Přidat akci po sestavení tak, že vyberete **publikování webové aplikace Azure**.</span><span class="sxs-lookup"><span data-stu-id="12816-198">Add a post-build action by selecting **Publish an Azure Web App**.</span></span>
4. <span data-ttu-id="12816-199">Zadejte, **mySp**, Azure instanční objekt uložená v předchozím kroku jako přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="12816-199">Supply, **mySp**, the Azure service principal stored in previous step as Azure Credentials.</span></span>
5. <span data-ttu-id="12816-200">V **konfigurace aplikací** zvolte skupinu prostředků a Linux webové aplikace v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="12816-200">In **App Configuration** section, choose the resource group and a Linux web app in your subscription.</span></span>
6. <span data-ttu-id="12816-201">Zvolte publikování přes Docker.</span><span class="sxs-lookup"><span data-stu-id="12816-201">Choose Publish via Docker.</span></span>
7. <span data-ttu-id="12816-202">Vyplňte **soubor Docker** cesta.</span><span class="sxs-lookup"><span data-stu-id="12816-202">Fill in **Dockerfile** path.</span></span> <span data-ttu-id="12816-203">Můžete ponechat výchozí "nebo soubor Docker" pro **Docker registru URL**, dodávky ve formátu https://&lt;myRegistry >. azurecr.io, pokud používáte Azure kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="12816-203">You can keep the default "/Dockerfile" For **Docker registry URL**, supply in the format of https://&lt;myRegistry>.azurecr.io if you use Azure Container Registry.</span></span> <span data-ttu-id="12816-204">Necháte prázdné, pokud používáte DockerHub.</span><span class="sxs-lookup"><span data-stu-id="12816-204">Leave it blank if you use DockerHub.</span></span>
8. <span data-ttu-id="12816-205">Pro **registru pověření**, přidat přihlašovací údaje pro registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="12816-205">For **Registry credentials**, add the credential for the Azure Container Registry.</span></span> <span data-ttu-id="12816-206">ID uživatele a heslo můžete získat spuštěním následujících příkazů v rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="12816-206">You can get the userid and password by running the following commands in Azure CLI.</span></span> <span data-ttu-id="12816-207">První příkaz umožňuje účet správce.</span><span class="sxs-lookup"><span data-stu-id="12816-207">The first command enables the administrator account.</span></span>    
```azurecli-interactive
az acr update -n <yourRegistry> --admin-enabled true
az acr credential show -n <yourRegistry>
```

9. <span data-ttu-id="12816-208">Název bitové kopie docker a značky v **Upřesnit** kartě jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="12816-208">The docker image name and tag in **Advanced** tab are optional.</span></span> <span data-ttu-id="12816-209">Ve výchozím nastavení je název bitové kopie získali z název bitové kopie, které jste nakonfigurovali v portálu Azure (v nastavení kontejner Docker.) Značky se generují z $BUILD_NUMBER.</span><span class="sxs-lookup"><span data-stu-id="12816-209">By default, image name is obtained from the image name you configured in Azure portal (in Docker Container setting.) The tag is generated from $BUILD_NUMBER.</span></span> <span data-ttu-id="12816-210">Ujistěte se, zadejte název bitové kopie na buď portálu Azure nebo zadejte hodnotu pro **Docker Image** v **Upřesnit** kartě. V tomto příkladu zadejte "&lt;yourRegistry >.azurecr.io/calculator" pro **Docker image** a nechte **značka obrázku Docker** prázdné.</span><span class="sxs-lookup"><span data-stu-id="12816-210">Make sure you specify the image name in either Azure portal or supply a value for **Docker Image** in **Advanced** tab. For this example, supply "&lt;yourRegistry>.azurecr.io/calculator" for **Docker image** and leave **Docker Image Tag** blank.</span></span>
10. <span data-ttu-id="12816-211">Poznámka: nasazení selže, pokud použijete předdefinované nastavení Docker bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="12816-211">Note deployment fails if you use built-in Docker image setting.</span></span> <span data-ttu-id="12816-212">Ujistěte se, že změna konfigurace docker používat vlastní image v kontejner Docker nastavení na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="12816-212">Make sure you change docker config to use custom image in Docker Container setting in Azure portal.</span></span> <span data-ttu-id="12816-213">Předdefinované bitové kopie použijte k nasazení přístupu nahrávání souboru.</span><span class="sxs-lookup"><span data-stu-id="12816-213">For built-in image, use file upload approach to deploy.</span></span>
11. <span data-ttu-id="12816-214">Podobně jako u Postup nahrání souboru, můžete vybrat jiný slot než produkční.</span><span class="sxs-lookup"><span data-stu-id="12816-214">Similar to file upload approach, you can choose a different slot other than production.</span></span>
12. <span data-ttu-id="12816-215">Uložte a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="12816-215">Save and build the project.</span></span> <span data-ttu-id="12816-216">Zobrazí bitové kopie kontejneru vložena do registru a webové aplikace je nasazená.</span><span class="sxs-lookup"><span data-stu-id="12816-216">You see your container image is pushed to your registry and web app is deployed.</span></span>

### <a name="deploy-to-web-app-on-linux-through-docker-using-jenkins-pipeline"></a><span data-ttu-id="12816-217">Nasazení do webové aplikace v systému Linux pomocí Docker volaných zřetězením příkazů</span><span class="sxs-lookup"><span data-stu-id="12816-217">Deploy to Web App on Linux through Docker using Jenkins pipeline</span></span>

1. <span data-ttu-id="12816-218">V Githubu webového uživatelského rozhraní, otevřete **Jenkinsfile_container_plugin** souboru.</span><span class="sxs-lookup"><span data-stu-id="12816-218">In GitHub web UI, open **Jenkinsfile_container_plugin** file.</span></span> <span data-ttu-id="12816-219">Klikněte na ikonu tužky upravit tento soubor se aktualizují skupinu prostředků a název webové aplikace na řádku 11 a 12 v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="12816-219">Click the pencil icon to edit this file to update the resource group and name of your web app on line 11 and 12 respectively.</span></span>    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. <span data-ttu-id="12816-220">Změňte řádek 13 k registru serveru kontejneru</span><span class="sxs-lookup"><span data-stu-id="12816-220">Change line 13 to your container registry server</span></span>    
```java
def registryServer = '<registryURL>'
```    

3. <span data-ttu-id="12816-221">Změňte řádek 16 aktualizovat ID pověření v instanci volaných</span><span class="sxs-lookup"><span data-stu-id="12816-221">Change line 16 to update credential ID in your Jenkins instance</span></span>    
```java
azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
```    
### <a name="create-jenkins-pipeline"></a><span data-ttu-id="12816-222">Vytvoření kanálu volaných</span><span class="sxs-lookup"><span data-stu-id="12816-222">Create Jenkins pipeline</span></span>    

1. <span data-ttu-id="12816-223">Otevřete volaných ve webovém prohlížeči, klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="12816-223">Open Jenkins in a web browser, click **New Item**.</span></span>
2. <span data-ttu-id="12816-224">Zadejte název pro úlohy, vyberte **kanálu**.</span><span class="sxs-lookup"><span data-stu-id="12816-224">Provide a name for the job and select **Pipeline**.</span></span> <span data-ttu-id="12816-225">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="12816-225">Click **OK**.</span></span>
3. <span data-ttu-id="12816-226">Klikněte **kanálu** kartě Další.</span><span class="sxs-lookup"><span data-stu-id="12816-226">Click the **Pipeline** tab next.</span></span>
4. <span data-ttu-id="12816-227">Pro **definice**, vyberte **kanálu skript z SCM**.</span><span class="sxs-lookup"><span data-stu-id="12816-227">For **Definition**, select **Pipeline script from SCM**.</span></span>
5. <span data-ttu-id="12816-228">Pro **SCM**, vyberte **Git**.</span><span class="sxs-lookup"><span data-stu-id="12816-228">For **SCM**, select **Git**.</span></span>
6. <span data-ttu-id="12816-229">Zadejte adresu URL webu GitHub pro vaše forked úložišti: https:&lt;vaše forked úložišti > .git</span><span class="sxs-lookup"><span data-stu-id="12816-229">Enter the GitHub URL for your forked repo: https:&lt;your forked repo>.git</span></span></li>
<span data-ttu-id="12816-230">Aktualizace 7, **skript cesta** na "Jenkinsfile_container_plugin"</span><span class="sxs-lookup"><span data-stu-id="12816-230">7, Update **Script Path** to "Jenkinsfile_container_plugin"</span></span>
8. <span data-ttu-id="12816-231">Klikněte na tlačítko **Uložit** a spusťte úlohu.</span><span class="sxs-lookup"><span data-stu-id="12816-231">Click **Save** and run the job.</span></span>

## <a name="verify-your-web-app"></a><span data-ttu-id="12816-232">Ověření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="12816-232">Verify your web app</span></span>

1. <span data-ttu-id="12816-233">Chcete-li ověřit WAR souboru úspěšně nasazena do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="12816-233">To verify the WAR file is deployed successfully to your web app.</span></span> <span data-ttu-id="12816-234">Otevřete webový prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="12816-234">Open a web browser.</span></span>
2. <span data-ttu-id="12816-235">Přejděte na http://&lt;app_name >.azurewebsites.net/api/calculator/ping zobrazí:</span><span class="sxs-lookup"><span data-stu-id="12816-235">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/ping You see:</span></span>    
     <span data-ttu-id="12816-236">Vítá vás webovou aplikaci Java!</span><span class="sxs-lookup"><span data-stu-id="12816-236">Welcome to Java Web App!!!</span></span> <span data-ttu-id="12816-237">Tato hodnota aktualizuje!</span><span class="sxs-lookup"><span data-stu-id="12816-237">This is updated!</span></span>
   <span data-ttu-id="12816-238">Sun června 17 16:39:10 UTC 2017</span><span class="sxs-lookup"><span data-stu-id="12816-238">Sun Jun 17 16:39:10 UTC 2017</span></span>
3. <span data-ttu-id="12816-239">Přejděte na http://&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Nahraďte &lt;x > a &lt;y > s všechna čísla) Chcete-li získat součet hodnot x a y</span><span class="sxs-lookup"><span data-stu-id="12816-239">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) to get the sum of x and y</span></span>        
    ![Kalkulačky: Přidat](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-app-service-on-linux"></a><span data-ttu-id="12816-241">Pro službu App service v systému Linux</span><span class="sxs-lookup"><span data-stu-id="12816-241">For App service on Linux</span></span>

* <span data-ttu-id="12816-242">Pokud chcete ověřit, v Azure CLI, spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="12816-242">To verify, in Azure CLI, run:</span></span>

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    <span data-ttu-id="12816-243">Získáte následující výsledek:</span><span class="sxs-lookup"><span data-stu-id="12816-243">You get the following result:</span></span>
    
    ```
    [
    "calculator"
    ]
    ```
    
    <span data-ttu-id="12816-244">Přejděte na http://&lt;app_name >.azurewebsites.net/api/calculator/ping.</span><span class="sxs-lookup"><span data-stu-id="12816-244">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/ping.</span></span> <span data-ttu-id="12816-245">Zobrazí se zpráva:</span><span class="sxs-lookup"><span data-stu-id="12816-245">You see the message:</span></span> 
    
        Welcome to Java Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    <span data-ttu-id="12816-246">Přejděte na http://&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Nahraďte &lt;x > a &lt;y > s všechna čísla) Chcete-li získat součet hodnot x a y</span><span class="sxs-lookup"><span data-stu-id="12816-246">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) to get the sum of x and y</span></span>
    
## <a name="next-steps"></a><span data-ttu-id="12816-247">Další kroky</span><span class="sxs-lookup"><span data-stu-id="12816-247">Next steps</span></span>

<span data-ttu-id="12816-248">V tomto kurzu použijete modul plug-in Azure App Service k nasazení do Azure.</span><span class="sxs-lookup"><span data-stu-id="12816-248">In this tutorial, you use the Azure App Service plugin to deploy to Azure.</span></span>

<span data-ttu-id="12816-249">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="12816-249">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12816-250">Konfigurace volaných k nasazení služby Azure App Service pomocí služby FTP</span><span class="sxs-lookup"><span data-stu-id="12816-250">Configure Jenkins to deploy Azure App Service through FTP</span></span> 
> * <span data-ttu-id="12816-251">Konfigurace volaných k nasazení do Azure App Service v Linuxu prostřednictvím Docker</span><span class="sxs-lookup"><span data-stu-id="12816-251">Configure Jenkins to deploy to Azure App Service on Linux through Docker</span></span> 