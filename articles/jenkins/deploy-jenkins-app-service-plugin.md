---
title: "aaaDeploy tooAzure App Service pomocí modulu plug-in volaných | Microsoft Docs"
description: "Zjistěte, jak toodeploy modul plug-in Azure App Service volaných toouse Java webové aplikace tooAzure ve volaných"
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
ms.openlocfilehash: 080be7277555ce7d688dccdf38eef309e7a7b194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-app-service-with-jenkins-plugin"></a><span data-ttu-id="c54ce-103">Nasazení pomocí modulu plug-in volaných tooAzure služby App Service</span><span class="sxs-lookup"><span data-stu-id="c54ce-103">Deploy tooAzure App Service with Jenkins plugin</span></span> 
<span data-ttu-id="c54ce-104">toodeploy tooAzure webové aplikace Java, můžete použít rozhraní příkazového řádku Azure v [volaných kanálu](/azure/jenkins/execute-cli-jenkins-pipeline) můžete taky použít hello [modul plug-in Azure App Service volaných](https://plugins.jenkins.io/azure-app-service).</span><span class="sxs-lookup"><span data-stu-id="c54ce-104">toodeploy a Java web app tooAzure, you can use Azure CLI in [Jenkins Pipeline](/azure/jenkins/execute-cli-jenkins-pipeline) or you can make use of hello [Azure App Service Jenkins plugin](https://plugins.jenkins.io/azure-app-service).</span></span> <span data-ttu-id="c54ce-105">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="c54ce-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c54ce-106">Konfigurace volaných toodeploy tooAzure služby App Service pomocí služby FTP</span><span class="sxs-lookup"><span data-stu-id="c54ce-106">Configure Jenkins toodeploy tooAzure App Service through FTP</span></span> 
> * <span data-ttu-id="c54ce-107">Konfigurace volaných toodeploy tooAzure služby App Service v Linuxu prostřednictvím Docker</span><span class="sxs-lookup"><span data-stu-id="c54ce-107">Configure Jenkins toodeploy tooAzure App Service on Linux through Docker</span></span> 

## <a name="create-and-configure-jenkins-instance"></a><span data-ttu-id="c54ce-108">Vytvořit a nakonfigurovat volaných instance</span><span class="sxs-lookup"><span data-stu-id="c54ce-108">Create and Configure Jenkins instance</span></span>
<span data-ttu-id="c54ce-109">Pokud již nemáte volaných master, začínat hello [šablona řešení](install-jenkins-solution-template.md), což zahrnuje JDK8 a hello následující požadované moduly plug-in:</span><span class="sxs-lookup"><span data-stu-id="c54ce-109">If you do not already have a Jenkins master, start with hello [Solution Template](install-jenkins-solution-template.md), which includes JDK8 and hello following required plugins:</span></span>

* <span data-ttu-id="c54ce-110">[Modul plug-in klienta volaných Git](https://plugins.jenkins.io/git-client) v.2.4.6</span><span class="sxs-lookup"><span data-stu-id="c54ce-110">[Jenkins Git client plugin](https://plugins.jenkins.io/git-client) v.2.4.6</span></span> 
* <span data-ttu-id="c54ce-111">[Modul plug-in docker Commons](https://plugins.jenkins.io/docker-commons) v.1.4.0</span><span class="sxs-lookup"><span data-stu-id="c54ce-111">[Docker Commons Plugin](https://plugins.jenkins.io/docker-commons) v.1.4.0</span></span>
* <span data-ttu-id="c54ce-112">[Přihlašovací údaje Azure](https://plugins.jenkins.io/azure-credentials) v.1.2</span><span class="sxs-lookup"><span data-stu-id="c54ce-112">[Azure Credentials](https://plugins.jenkins.io/azure-credentials) v.1.2</span></span>
* <span data-ttu-id="c54ce-113">[Aplikační služba Azure](https://plugins.jenkins.io/azure-app-server) v.0.1</span><span class="sxs-lookup"><span data-stu-id="c54ce-113">[Azure App Service](https://plugins.jenkins.io/azure-app-server) v.0.1</span></span>

<span data-ttu-id="c54ce-114">Můžete použít hello toodeploy modul plug-in služby App Service webovou aplikaci ve všech jazycích (například C#, PHP, Java a node.js atd.), které jsou podporované službou Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c54ce-114">You can use hello App Service plugin toodeploy Web App in all languages (for instance, C#, PHP, Java, and node.js etc.) supported by Azure App Service.</span></span> <span data-ttu-id="c54ce-115">V tomto kurzu používáme hello ukázkovou aplikaci Java, [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample).</span><span class="sxs-lookup"><span data-stu-id="c54ce-115">In this tutorial, we are using hello sample Java app, [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample).</span></span> <span data-ttu-id="c54ce-116">toofork hello úložišti tooyour vlastní účet GitHub, klikněte na tlačítko hello **rozvětvení** tlačítko v horním pravém rohu hello.</span><span class="sxs-lookup"><span data-stu-id="c54ce-116">toofork hello repo tooyour own GitHub account, click hello **Fork** button in hello top right-hand corner.</span></span>  

<span data-ttu-id="c54ce-117">Jsou vyžadovány pro sestavení projektu Java hello Java JDK a Maven.</span><span class="sxs-lookup"><span data-stu-id="c54ce-117">Java JDK and Maven are required for building hello Java project.</span></span> <span data-ttu-id="c54ce-118">Ujistěte se, že pokud použijete jeden pro nepřetržitou integraci instalujete hello součásti v hlavní volaných hello nebo hello agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c54ce-118">Make sure you install hello components in hello Jenkins master or hello VM agent if you use one for continuous integration.</span></span> 

<span data-ttu-id="c54ce-119">tooinstall, přihlaste instance volaných toohello pomocí protokolu SSH a spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="c54ce-119">tooinstall, log in toohello Jenkins instance using SSH and run hello following commands:</span></span>

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

<span data-ttu-id="c54ce-120">Pro nasazení tooApp služby v systému Linux, musíte taky tooinstall Docker v předloze volaných hello nebo hello agenta virtuálního počítače použít pro sestavení.</span><span class="sxs-lookup"><span data-stu-id="c54ce-120">For deploying tooApp Service on Linux, you also need tooinstall Docker on hello Jenkins master or hello VM agent used for build.</span></span> <span data-ttu-id="c54ce-121">Najdete v článku tooinstall toothis Docker: https://docs.docker.com/engine/installation/linux/ubuntu/.</span><span class="sxs-lookup"><span data-stu-id="c54ce-121">Refer toothis article tooinstall Docker: https://docs.docker.com/engine/installation/linux/ubuntu/.</span></span>

## <a name="add-azure-service-principal-toojenkins-credential"></a><span data-ttu-id="c54ce-122">Přidat přihlašovací údaje služby Azure hlavní tooJenkins</span><span class="sxs-lookup"><span data-stu-id="c54ce-122">Add Azure service principal tooJenkins credential</span></span>

<span data-ttu-id="c54ce-123">Objektu zabezpečení služby Azure je potřebné toodeploy tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c54ce-123">An Azure service principal is needed toodeploy tooAzure.</span></span> 

<ol>
<li><span data-ttu-id="c54ce-124">Použití [rozhraní příkazového řádku Azure](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) nebo [portál Azure](/azure/azure-resource-manager/resource-group-create-service-principal-portal) toocreate objektu zabezpečení služby Azure</span><span class="sxs-lookup"><span data-stu-id="c54ce-124">Use [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) or [Azure portal](/azure/azure-resource-manager/resource-group-create-service-principal-portal) toocreate an Azure service principal</span></span></li>
<li><span data-ttu-id="c54ce-125">V rámci hello volaných řídicí panel, klikněte na **pověření -> Systém ->**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-125">Within hello Jenkins dashboard, click **Credentials -> System ->**.</span></span> <span data-ttu-id="c54ce-126">Klikněte na tlačítko **globální credentials(unrestricted)**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-126">Click **Global credentials(unrestricted)**.</span></span></li>
<li><span data-ttu-id="c54ce-127">Klikněte na tlačítko **přidat přihlašovací údaje** tooadd objekt služby Microsoft Azure tak, že vyplníte hello ID předplatného, ID klienta, sdílený tajný klíč klienta a koncový bod tokenu OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="c54ce-127">Click **Add Credentials** tooadd a Microsoft Azure service principal by filling out hello Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span> <span data-ttu-id="c54ce-128">Zadejte ID, **mySp**, pro použití v postupných krocích.</span><span class="sxs-lookup"><span data-stu-id="c54ce-128">Provide an ID, **mySp**, for use in subsequent steps.</span></span></li>
</ol>

## <a name="azure-app-service-plugin"></a><span data-ttu-id="c54ce-129">Modul plug-in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c54ce-129">Azure App Service plugin</span></span>

<span data-ttu-id="c54ce-130">V1.0 modul plug-in Azure App Service podporuje průběžné nasazování tooAzure webové aplikace prostřednictvím:</span><span class="sxs-lookup"><span data-stu-id="c54ce-130">Azure App Service plugin v1.0 supports continuous deployment tooAzure Web App through:</span></span>

* <span data-ttu-id="c54ce-131">Git a FTP</span><span class="sxs-lookup"><span data-stu-id="c54ce-131">Git and FTP</span></span>
* <span data-ttu-id="c54ce-132">Docker pro webovou aplikaci v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c54ce-132">Docker for Web App on Linux</span></span>

## <a name="configure-jenkins-toodeploy-web-app-through-ftp-using-hello-jenkins-dashboard"></a><span data-ttu-id="c54ce-133">Konfigurace volaných toodeploy webové aplikace pomocí služby FTP pomocí řídicího panelu volaných hello</span><span class="sxs-lookup"><span data-stu-id="c54ce-133">Configure Jenkins toodeploy Web App through FTP using hello Jenkins dashboard</span></span>

<span data-ttu-id="c54ce-134">toodeploy tooAzure vašeho projektu webové aplikace, můžete nahrát artefaktů sestavení (například .war souboru v jazyce Java) pomocí Git a FTP.</span><span class="sxs-lookup"><span data-stu-id="c54ce-134">toodeploy your project tooAzure Web App, you can upload your build artifacts (for example, .war file in Java) using Git or FTP.</span></span>

<span data-ttu-id="c54ce-135">Před nastavením hello úlohu ve volaných, potřebujete pro spuštěné aplikaci Java hello plán aplikační služby Azure a webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c54ce-135">Before setting up hello job in Jenkins, you need an Azure App Service plan and a Web App for running hello Java app.</span></span>


1. <span data-ttu-id="c54ce-136">Vytvořte plán aplikační služby Azure s hello **volné** cenová úroveň pomocí hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) rozhraní příkazového řádku příkaz.</span><span class="sxs-lookup"><span data-stu-id="c54ce-136">Create an Azure App Service plan with hello **FREE** pricing tier using hello  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="c54ce-137">plán aplikační služby Hello definuje toohost hello fyzické prostředky, které používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="c54ce-137">hello appservice plan defines hello physical resources used toohost your apps.</span></span> <span data-ttu-id="c54ce-138">Všechny aplikace, které jsou přiřazené plán aplikační služby tooan sdílení těchto prostředků, umožní vám toosave nákladů při hostování více aplikací.</span><span class="sxs-lookup"><span data-stu-id="c54ce-138">All applications assigned tooan appservice plan share these resources, allowing you toosave cost when hosting multiple apps.</span></span>
2. <span data-ttu-id="c54ce-139">Vytvořte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c54ce-139">Create a Web App.</span></span> <span data-ttu-id="c54ce-140">Můžete buď použít hello [portál Azure](/azure/app-service-web/web-sites-configure) nebo hello použijte následující příkaz Az rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="c54ce-140">You can either use hello [Azure portal](/azure/app-service-web/web-sites-configure) or use hello following Az CLI command:</span></span>
```azurecli-interactive 
az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
```

3. <span data-ttu-id="c54ce-141">Ujistěte se, že nastavíte hello Java runtime konfigurace, které vaše aplikace musí.</span><span class="sxs-lookup"><span data-stu-id="c54ce-141">Make sure you set up hello Java runtime configuration that your app needs.</span></span> <span data-ttu-id="c54ce-142">Následující příkaz rozhraní příkazového řádku Azure Hello konfiguruje hello webové aplikace toorun na poslední JDK 8 Java a [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="c54ce-142">hello following Azure CLI command configures hello web app toorun on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>
```azurecli-interactive
az webapp config set \
--name <myAppName> \
--resource-group <myResourceGroup> \
--java-version 1.8 \
--java-container Tomcat \
--java-container-version 8.0
```

### <a name="set-up-hello-jenkins-job"></a><span data-ttu-id="c54ce-143">Nastavení úloh volaných hello</span><span class="sxs-lookup"><span data-stu-id="c54ce-143">Set up hello Jenkins job</span></span>


1. <span data-ttu-id="c54ce-144">Vytvořte novou **volný styl** projekt v volaných řídicí panel</span><span class="sxs-lookup"><span data-stu-id="c54ce-144">Create a new **freestyle** project in Jenkins Dashboard</span></span>
2. <span data-ttu-id="c54ce-145">Konfigurace **správu zdrojového kódu** toouse vaše místní pokračovatelem [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) tím, že poskytuje hello **adresu URL úložiště**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-145">Configure **Source Code Management** toouse your local fork of [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) by providing hello **Repository URL**.</span></span> <span data-ttu-id="c54ce-146">Příklad: http://github.com/&lt;yourID > / javawebappsample.</span><span class="sxs-lookup"><span data-stu-id="c54ce-146">For example: http://github.com/&lt;yourID>/javawebappsample.</span></span>
3. <span data-ttu-id="c54ce-147">Přidáte sestavení krok toobuild hello projekt pomocí nástroje Maven.</span><span class="sxs-lookup"><span data-stu-id="c54ce-147">Add a Build step toobuild hello project using Maven.</span></span> <span data-ttu-id="c54ce-148">Učinit přidáním **spustit prostředí**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-148">Do so by adding an **Execute shell**.</span></span> <span data-ttu-id="c54ce-149">V tomto příkladu potřebujeme další krok toorename hello *.war souboru v cílové složce tooROOT.war.</span><span class="sxs-lookup"><span data-stu-id="c54ce-149">For this example, we need an additional step toorename hello *.war file in target folder tooROOT.war.</span></span>   
```bash
mvn clean package
mv target/*.war target/ROOT.war
```

4. <span data-ttu-id="c54ce-150">Přidat akci po sestavení tak, že vyberete **publikování webové aplikace Azure**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-150">Add a post-build action by selecting **Publish an Azure Web App**.</span></span>
5. <span data-ttu-id="c54ce-151">Zadejte, "mySp", hello Azure instanční objekt uložená v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="c54ce-151">Supply, "mySp", hello Azure service principal stored in previous step.</span></span>
6. <span data-ttu-id="c54ce-152">V **konfigurace aplikací** zvolte hello prostředků skupiny a webové aplikace v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="c54ce-152">In **App Configuration** section, choose hello resource group and web app in your subscription.</span></span> <span data-ttu-id="c54ce-153">modul plug-in Hello automaticky zjistí, zda text hello webové aplikace je Windows nebo linuxu.</span><span class="sxs-lookup"><span data-stu-id="c54ce-153">hello plugin automatically detects whether hello Web App is Windows or Linux-based.</span></span> <span data-ttu-id="c54ce-154">Pro webovou aplikaci systému Windows se zobrazí možnost hello "Publikovat soubory".</span><span class="sxs-lookup"><span data-stu-id="c54ce-154">For a Windows-based Web App, hello option "Publish Files" is presented.</span></span>
7. <span data-ttu-id="c54ce-155">Zadejte v souborech hello chcete toodeploy (například war balíček Pokud používáte Java.) Zdrojový a cílový adresář jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="c54ce-155">Fill in hello files you want toodeploy (for example, a war package if you're using Java.) Source Directory and Target Directory are optional.</span></span> <span data-ttu-id="c54ce-156">Parametry Hello povolit toospecify zdrojové a cílové složky při nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="c54ce-156">hello parameters allow you toospecify source and target folders when uploading files.</span></span> <span data-ttu-id="c54ce-157">Webové aplikace v jazyce Java v Azure běží na serveru Tomcat.</span><span class="sxs-lookup"><span data-stu-id="c54ce-157">Java web app on Azure is run in a Tomcat server.</span></span> <span data-ttu-id="c54ce-158">Proto můžete nahrávat na server války se balíček umístěna složka webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="c54ce-158">So you upload you war package to webapps folder.</span></span> <span data-ttu-id="c54ce-159">V tomto příkladu nastavit **zdrojový adresář** příliš "cíle" a "webapps" zadat **cílový adresář**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-159">For this example, set **Source Directory** too"target" and supply "webapps" for **Target Directory**.</span></span>
8. <span data-ttu-id="c54ce-160">Pokud chcete toodeploy tooa slotu než produkční, můžete také nastavit **slotu** název.</span><span class="sxs-lookup"><span data-stu-id="c54ce-160">If you want toodeploy tooa slot other than production, you can also set **Slot** Name.</span></span>
9. <span data-ttu-id="c54ce-161">Uložit hello projektu a sestavte jej.</span><span class="sxs-lookup"><span data-stu-id="c54ce-161">Save hello project and build it.</span></span> <span data-ttu-id="c54ce-162">Webové aplikace je nasazená tooAzure po dokončení sestavení.</span><span class="sxs-lookup"><span data-stu-id="c54ce-162">Your web app is deployed tooAzure when build is complete.</span></span>

### <a name="deploy-web-app-through-ftp-using-jenkins-pipeline"></a><span data-ttu-id="c54ce-163">Nasazení webové aplikace pomocí služby FTP volaných zřetězením příkazů</span><span class="sxs-lookup"><span data-stu-id="c54ce-163">Deploy Web App through FTP using Jenkins pipeline</span></span>

<span data-ttu-id="c54ce-164">modul plug-in Hello je připravené pro kanál.</span><span class="sxs-lookup"><span data-stu-id="c54ce-164">hello plugin is pipeline-ready.</span></span> <span data-ttu-id="c54ce-165">Ukázka tooa v úložišti GitHub hello lze odkazovat.</span><span class="sxs-lookup"><span data-stu-id="c54ce-165">You can refer tooa sample in hello GitHub repo.</span></span>

1. <span data-ttu-id="c54ce-166">V Githubu webového uživatelského rozhraní, otevřete **Jenkinsfile_ftp_plugin** souboru.</span><span class="sxs-lookup"><span data-stu-id="c54ce-166">In GitHub web UI, open **Jenkinsfile_ftp_plugin** file.</span></span> <span data-ttu-id="c54ce-167">Klikněte na tlačítko tooedit ikonu tužky hello tato skupina prostředků hello tooupdate souboru a název webové aplikace na řádku 11 a 12 v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="c54ce-167">Click hello pencil icon tooedit this file tooupdate hello resource group and name of your web app on line 11 and 12 respectively.</span></span>    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. <span data-ttu-id="c54ce-168">Změňte ID pověření 14 tooupdate řádek v instanci volaných.</span><span class="sxs-lookup"><span data-stu-id="c54ce-168">Change line 14 tooupdate credential ID in your Jenkins instance.</span></span>    
```java
withCredentials([azureServicePrincipal('<mySp>')]) {
```

### <a name="create-a-jenkins-pipeline"></a><span data-ttu-id="c54ce-169">Vytvoření kanálu volaných</span><span class="sxs-lookup"><span data-stu-id="c54ce-169">Create a Jenkins pipeline</span></span>

1. <span data-ttu-id="c54ce-170">Otevřete volaných ve webovém prohlížeči, klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-170">Open Jenkins in a web browser, click **New Item**.</span></span>
2. <span data-ttu-id="c54ce-171">Zadejte název úlohy hello a vyberte **kanálu**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-171">Provide a name for hello job and select **Pipeline**.</span></span> <span data-ttu-id="c54ce-172">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-172">Click **OK**.</span></span>
3. <span data-ttu-id="c54ce-173">Klikněte na tlačítko hello **kanálu** kartě Další.</span><span class="sxs-lookup"><span data-stu-id="c54ce-173">Click hello **Pipeline** tab next.</span></span>
4. <span data-ttu-id="c54ce-174">Pro **definice**, vyberte **kanálu skript z SCM**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-174">For **Definition**, select **Pipeline script from SCM**.</span></span>
5. <span data-ttu-id="c54ce-175">Pro **SCM**, vyberte **Git**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-175">For **SCM**, select **Git**.</span></span> <span data-ttu-id="c54ce-176">Zadejte hello Githubu adresu URL pro váš forked úložišti: https:&lt;vaše forked úložišti > .git</span><span class="sxs-lookup"><span data-stu-id="c54ce-176">Enter hello GitHub URL for your forked repo: https:&lt;your forked repo>.git</span></span>
6. <span data-ttu-id="c54ce-177">Aktualizace **cestu ke skriptu** příliš "Jenkinsfile_ftp_plugin"</span><span class="sxs-lookup"><span data-stu-id="c54ce-177">Update **Script Path** too"Jenkinsfile_ftp_plugin"</span></span>
7. <span data-ttu-id="c54ce-178">Klikněte na tlačítko **Uložit** a úlohu spusťte hello.</span><span class="sxs-lookup"><span data-stu-id="c54ce-178">Click **Save** and run hello job.</span></span>

## <a name="configure-jenkins-toodeploy-web-app-on-linux-through-docker"></a><span data-ttu-id="c54ce-179">Konfigurace volaných toodeploy webové aplikace v systému Linux pomocí Docker</span><span class="sxs-lookup"><span data-stu-id="c54ce-179">Configure Jenkins toodeploy Web App on Linux through Docker</span></span>

<span data-ttu-id="c54ce-180">Kromě Git/FTP webové aplikace v systému Linux podporuje nasazení pomocí Docker.</span><span class="sxs-lookup"><span data-stu-id="c54ce-180">Apart from Git/FTP, Web App on Linux supports deployment using Docker.</span></span> <span data-ttu-id="c54ce-181">toodeploy pomocí Docker, je nutné tooprovide soubor Docker, který balíčky vaší webové aplikace s běh služby do bitové kopie docker.</span><span class="sxs-lookup"><span data-stu-id="c54ce-181">toodeploy using Docker, you need tooprovide a Dockerfile that packages your web app with service runtime into a docker image.</span></span> <span data-ttu-id="c54ce-182">Potom modul plug-in hello sestavení hello image, nabízených oznámení tooa docker registru a nasadí hello image tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c54ce-182">Then hello plugin builds hello image, pushes it tooa docker registry and deploys hello image tooyour web app.</span></span>

<span data-ttu-id="c54ce-183">Webové aplikace v systému Linux také podporuje tradiční způsobů jako Git a FTP, ale jenom pro předdefinované jazyků (.NET Core, Node.js, PHP a Ruby).</span><span class="sxs-lookup"><span data-stu-id="c54ce-183">Web App on Linux also supports traditional ways like Git and FTP, but only for built-in languages (.NET Core, Node.js, PHP, and Ruby).</span></span> <span data-ttu-id="c54ce-184">Pro jiné jazyky toopackage vaší aplikace kód a služby modulu runtime společně do bitové kopie docker a pomocí docker toodeploy.</span><span class="sxs-lookup"><span data-stu-id="c54ce-184">For other languages, you need toopackage your application code and service runtime together into a docker image and use docker toodeploy.</span></span>

<span data-ttu-id="c54ce-185">Před nastavením hello úlohu ve volaných, potřebujete Azure aplikační službu v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="c54ce-185">Before setting up hello job in Jenkins, you need an Azure app service on Linux.</span></span> <span data-ttu-id="c54ce-186">Kontejner registru je také potřebné toostore a spravovat privátní kontejner imagí Dockeru.</span><span class="sxs-lookup"><span data-stu-id="c54ce-186">A container registry is also needed toostore and manage your private Docker container images.</span></span> <span data-ttu-id="c54ce-187">Můžete použít DockerHub; v tomto příkladu používáme registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="c54ce-187">You can use DockerHub; we are using Azure Container Registry for this example.</span></span>

* <span data-ttu-id="c54ce-188">Můžete provést kroky pro hello [sem](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c54ce-188">You can follow hello steps [here](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate a Web App on Linux</span></span> 
* <span data-ttu-id="c54ce-189">Azure registru kontejneru je spravovaný [Docker registru] služby (https://docs.docker.com/registry/) na základě hello open-source Docker registru 2.0.</span><span class="sxs-lookup"><span data-stu-id="c54ce-189">Azure Container Registry is a managed [Docker registry] (https://docs.docker.com/registry/) service based on hello open-source Docker Registry 2.0.</span></span> <span data-ttu-id="c54ce-190">Postupujte podle kroků hello [sem] (/ azure/container-registry/container-registry-get-started-azure-cli) pro další pokyny toodo tak.</span><span class="sxs-lookup"><span data-stu-id="c54ce-190">Follow hello steps [here] (/azure/container-registry/container-registry-get-started-azure-cli) for more guidance on how toodo so.</span></span> <span data-ttu-id="c54ce-191">Můžete také použít DockerHub.</span><span class="sxs-lookup"><span data-stu-id="c54ce-191">You can also use DockerHub.</span></span>

### <a name="toodeploy-using-docker"></a><span data-ttu-id="c54ce-192">pomocí docker toodeploy:</span><span class="sxs-lookup"><span data-stu-id="c54ce-192">toodeploy using docker:</span></span>

1. <span data-ttu-id="c54ce-193">Vytvoření nového projektu volný styl volaných řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="c54ce-193">Create a new freestyle project in Jenkins Dashboard.</span></span>
2. <span data-ttu-id="c54ce-194">Konfigurace **správu zdrojového kódu** toouse vaše místní pokračovatelem [jednoduché webové aplikace Java pro Azure](https://github.com/azure-devops/javawebappsample) tím, že poskytuje hello **adresu URL úložiště**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-194">Configure **Source Code Management** toouse your local fork of [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) by providing hello **Repository URL**.</span></span> <span data-ttu-id="c54ce-195">Příklad: http://github.com/&lt;yourid > / javawebappsample.</span><span class="sxs-lookup"><span data-stu-id="c54ce-195">For example: http://github.com/&lt;yourid>/javawebappsample.</span></span>
<span data-ttu-id="c54ce-196">Přidáte sestavení krok toobuild hello projekt pomocí nástroje Maven.</span><span class="sxs-lookup"><span data-stu-id="c54ce-196">Add a Build step toobuild hello project using Maven.</span></span> <span data-ttu-id="c54ce-197">Učinit přidáním **spustit prostředí** a přidejte následující řádek ve hello **příkaz**:</span><span class="sxs-lookup"><span data-stu-id="c54ce-197">Do so by adding an **Execute shell** and add hello following line in **Command**:</span></span>    
```bash
mvn clean package
```

3. <span data-ttu-id="c54ce-198">Přidat akci po sestavení tak, že vyberete **publikování webové aplikace Azure**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-198">Add a post-build action by selecting **Publish an Azure Web App**.</span></span>
4. <span data-ttu-id="c54ce-199">Zadejte, **mySp**, hello Azure instanční objekt uložená v předchozím kroku jako přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="c54ce-199">Supply, **mySp**, hello Azure service principal stored in previous step as Azure Credentials.</span></span>
5. <span data-ttu-id="c54ce-200">V **konfigurace aplikací** zvolte skupinu prostředků hello a Linux webové aplikace v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="c54ce-200">In **App Configuration** section, choose hello resource group and a Linux web app in your subscription.</span></span>
6. <span data-ttu-id="c54ce-201">Zvolte publikování přes Docker.</span><span class="sxs-lookup"><span data-stu-id="c54ce-201">Choose Publish via Docker.</span></span>
7. <span data-ttu-id="c54ce-202">Vyplňte **soubor Docker** cesta.</span><span class="sxs-lookup"><span data-stu-id="c54ce-202">Fill in **Dockerfile** path.</span></span> <span data-ttu-id="c54ce-203">Můžete ponechat výchozí hello "nebo soubor Docker" pro **Docker registru URL**, dodávky ve formátu hello https://&lt;myRegistry >. azurecr.io, pokud používáte Azure kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="c54ce-203">You can keep hello default "/Dockerfile" For **Docker registry URL**, supply in hello format of https://&lt;myRegistry>.azurecr.io if you use Azure Container Registry.</span></span> <span data-ttu-id="c54ce-204">Necháte prázdné, pokud používáte DockerHub.</span><span class="sxs-lookup"><span data-stu-id="c54ce-204">Leave it blank if you use DockerHub.</span></span>
8. <span data-ttu-id="c54ce-205">Pro **registru pověření**, přidat hello přihlašovací údaje pro hello registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="c54ce-205">For **Registry credentials**, add hello credential for hello Azure Container Registry.</span></span> <span data-ttu-id="c54ce-206">Spuštěním následujících příkazů v Azure CLI hello můžete získat hello ID uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="c54ce-206">You can get hello userid and password by running hello following commands in Azure CLI.</span></span> <span data-ttu-id="c54ce-207">První příkaz Hello umožňuje hello účet správce.</span><span class="sxs-lookup"><span data-stu-id="c54ce-207">hello first command enables hello administrator account.</span></span>    
```azurecli-interactive
az acr update -n <yourRegistry> --admin-enabled true
az acr credential show -n <yourRegistry>
```

9. <span data-ttu-id="c54ce-208">Hello docker název bitové kopie a značky v **Upřesnit** kartě jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="c54ce-208">hello docker image name and tag in **Advanced** tab are optional.</span></span> <span data-ttu-id="c54ce-209">Ve výchozím nastavení, je získat název bitové kopie z bitové kopie hello název jste nakonfigurovali v Azure portálu (v kontejner Docker nastavení.) hello značky se generují z $BUILD_NUMBER.</span><span class="sxs-lookup"><span data-stu-id="c54ce-209">By default, image name is obtained from hello image name you configured in Azure portal (in Docker Container setting.) hello tag is generated from $BUILD_NUMBER.</span></span> <span data-ttu-id="c54ce-210">Ujistěte se, zadejte název bitové kopie hello buď portálu Azure nebo zadejte hodnotu pro **Docker Image** v **Upřesnit** kartě. V tomto příkladu zadejte "&lt;yourRegistry >.azurecr.io/calculator" pro **Docker image** a nechte **značka obrázku Docker** prázdné.</span><span class="sxs-lookup"><span data-stu-id="c54ce-210">Make sure you specify hello image name in either Azure portal or supply a value for **Docker Image** in **Advanced** tab. For this example, supply "&lt;yourRegistry>.azurecr.io/calculator" for **Docker image** and leave **Docker Image Tag** blank.</span></span>
10. <span data-ttu-id="c54ce-211">Poznámka: nasazení selže, pokud použijete předdefinované nastavení Docker bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c54ce-211">Note deployment fails if you use built-in Docker image setting.</span></span> <span data-ttu-id="c54ce-212">Ujistěte se, že změníte docker konfigurace toouse vlastní image ve kontejner Docker nastavení na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c54ce-212">Make sure you change docker config toouse custom image in Docker Container setting in Azure portal.</span></span> <span data-ttu-id="c54ce-213">Předdefinované bitové kopie použijte toodeploy přístup nahrávání souboru.</span><span class="sxs-lookup"><span data-stu-id="c54ce-213">For built-in image, use file upload approach toodeploy.</span></span>
11. <span data-ttu-id="c54ce-214">Podobný postup nahrání toofile, můžete vybrat jiný slot než produkční.</span><span class="sxs-lookup"><span data-stu-id="c54ce-214">Similar toofile upload approach, you can choose a different slot other than production.</span></span>
12. <span data-ttu-id="c54ce-215">Uložte a sestavení projektu hello.</span><span class="sxs-lookup"><span data-stu-id="c54ce-215">Save and build hello project.</span></span> <span data-ttu-id="c54ce-216">Zobrazí bitové kopie kontejneru se posune tooyour registru a webové aplikace je nasazená.</span><span class="sxs-lookup"><span data-stu-id="c54ce-216">You see your container image is pushed tooyour registry and web app is deployed.</span></span>

### <a name="deploy-tooweb-app-on-linux-through-docker-using-jenkins-pipeline"></a><span data-ttu-id="c54ce-217">Nasazení tooWeb aplikace v systému Linux pomocí Docker volaných zřetězením příkazů</span><span class="sxs-lookup"><span data-stu-id="c54ce-217">Deploy tooWeb App on Linux through Docker using Jenkins pipeline</span></span>

1. <span data-ttu-id="c54ce-218">V Githubu webového uživatelského rozhraní, otevřete **Jenkinsfile_container_plugin** souboru.</span><span class="sxs-lookup"><span data-stu-id="c54ce-218">In GitHub web UI, open **Jenkinsfile_container_plugin** file.</span></span> <span data-ttu-id="c54ce-219">Klikněte na tlačítko tooedit ikonu tužky hello tato skupina prostředků hello tooupdate souboru a název webové aplikace na řádku 11 a 12 v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="c54ce-219">Click hello pencil icon tooedit this file tooupdate hello resource group and name of your web app on line 11 and 12 respectively.</span></span>    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. <span data-ttu-id="c54ce-220">Změna řádku 13 tooyour kontejneru registru serveru</span><span class="sxs-lookup"><span data-stu-id="c54ce-220">Change line 13 tooyour container registry server</span></span>    
```java
def registryServer = '<registryURL>'
```    

3. <span data-ttu-id="c54ce-221">Změňte ID pověření 16 tooupdate řádek v instanci volaných</span><span class="sxs-lookup"><span data-stu-id="c54ce-221">Change line 16 tooupdate credential ID in your Jenkins instance</span></span>    
```java
azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
```    
### <a name="create-jenkins-pipeline"></a><span data-ttu-id="c54ce-222">Vytvoření kanálu volaných</span><span class="sxs-lookup"><span data-stu-id="c54ce-222">Create Jenkins pipeline</span></span>    

1. <span data-ttu-id="c54ce-223">Otevřete volaných ve webovém prohlížeči, klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-223">Open Jenkins in a web browser, click **New Item**.</span></span>
2. <span data-ttu-id="c54ce-224">Zadejte název úlohy hello a vyberte **kanálu**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-224">Provide a name for hello job and select **Pipeline**.</span></span> <span data-ttu-id="c54ce-225">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-225">Click **OK**.</span></span>
3. <span data-ttu-id="c54ce-226">Klikněte na tlačítko hello **kanálu** kartě Další.</span><span class="sxs-lookup"><span data-stu-id="c54ce-226">Click hello **Pipeline** tab next.</span></span>
4. <span data-ttu-id="c54ce-227">Pro **definice**, vyberte **kanálu skript z SCM**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-227">For **Definition**, select **Pipeline script from SCM**.</span></span>
5. <span data-ttu-id="c54ce-228">Pro **SCM**, vyberte **Git**.</span><span class="sxs-lookup"><span data-stu-id="c54ce-228">For **SCM**, select **Git**.</span></span>
6. <span data-ttu-id="c54ce-229">Zadejte hello Githubu adresu URL pro váš forked úložišti: https:&lt;vaše forked úložišti > .git</span><span class="sxs-lookup"><span data-stu-id="c54ce-229">Enter hello GitHub URL for your forked repo: https:&lt;your forked repo>.git</span></span></li>
<span data-ttu-id="c54ce-230">Aktualizace 7, **cestu ke skriptu** příliš "Jenkinsfile_container_plugin"</span><span class="sxs-lookup"><span data-stu-id="c54ce-230">7, Update **Script Path** too"Jenkinsfile_container_plugin"</span></span>
8. <span data-ttu-id="c54ce-231">Klikněte na tlačítko **Uložit** a úlohu spusťte hello.</span><span class="sxs-lookup"><span data-stu-id="c54ce-231">Click **Save** and run hello job.</span></span>

## <a name="verify-your-web-app"></a><span data-ttu-id="c54ce-232">Ověření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c54ce-232">Verify your web app</span></span>

1. <span data-ttu-id="c54ce-233">soubor WAR hello tooverify úspěšně nasadil tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c54ce-233">tooverify hello WAR file is deployed successfully tooyour web app.</span></span> <span data-ttu-id="c54ce-234">Otevřete webový prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="c54ce-234">Open a web browser.</span></span>
2. <span data-ttu-id="c54ce-235">Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/ping zobrazí:</span><span class="sxs-lookup"><span data-stu-id="c54ce-235">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/ping You see:</span></span>    
     <span data-ttu-id="c54ce-236">Vítejte tooJava webové aplikace!</span><span class="sxs-lookup"><span data-stu-id="c54ce-236">Welcome tooJava Web App!!!</span></span> <span data-ttu-id="c54ce-237">Tato hodnota aktualizuje!</span><span class="sxs-lookup"><span data-stu-id="c54ce-237">This is updated!</span></span>
   <span data-ttu-id="c54ce-238">Sun června 17 16:39:10 UTC 2017</span><span class="sxs-lookup"><span data-stu-id="c54ce-238">Sun Jun 17 16:39:10 UTC 2017</span></span>
3. <span data-ttu-id="c54ce-239">Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Nahraďte &lt;x > a &lt;y > s všechna čísla) tooget hello součet hodnot x a y</span><span class="sxs-lookup"><span data-stu-id="c54ce-239">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) tooget hello sum of x and y</span></span>        
    ![Kalkulačky: Přidat](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-app-service-on-linux"></a><span data-ttu-id="c54ce-241">Pro službu App service v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c54ce-241">For App service on Linux</span></span>

* <span data-ttu-id="c54ce-242">tooverify v Azure CLI, spusťte:</span><span class="sxs-lookup"><span data-stu-id="c54ce-242">tooverify, in Azure CLI, run:</span></span>

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    <span data-ttu-id="c54ce-243">Můžete získat hello následující výsledek:</span><span class="sxs-lookup"><span data-stu-id="c54ce-243">You get hello following result:</span></span>
    
    ```
    [
    "calculator"
    ]
    ```
    
    <span data-ttu-id="c54ce-244">Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/ping.</span><span class="sxs-lookup"><span data-stu-id="c54ce-244">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/ping.</span></span> <span data-ttu-id="c54ce-245">Zobrazí zpráva hello:</span><span class="sxs-lookup"><span data-stu-id="c54ce-245">You see hello message:</span></span> 
    
        Welcome tooJava Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    <span data-ttu-id="c54ce-246">Přejděte toohttp: / /&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (Nahraďte &lt;x > a &lt;y > s všechna čísla) tooget hello součet hodnot x a y</span><span class="sxs-lookup"><span data-stu-id="c54ce-246">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) tooget hello sum of x and y</span></span>
    
## <a name="next-steps"></a><span data-ttu-id="c54ce-247">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c54ce-247">Next steps</span></span>

<span data-ttu-id="c54ce-248">V tomto kurzu použijete tooAzure toodeploy modul plug-in Azure App Service hello.</span><span class="sxs-lookup"><span data-stu-id="c54ce-248">In this tutorial, you use hello Azure App Service plugin toodeploy tooAzure.</span></span>

<span data-ttu-id="c54ce-249">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="c54ce-249">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c54ce-250">Konfigurace volaných toodeploy Azure App Service pomocí služby FTP</span><span class="sxs-lookup"><span data-stu-id="c54ce-250">Configure Jenkins toodeploy Azure App Service through FTP</span></span> 
> * <span data-ttu-id="c54ce-251">Konfigurace volaných toodeploy tooAzure služby App Service v Linuxu prostřednictvím Docker</span><span class="sxs-lookup"><span data-stu-id="c54ce-251">Configure Jenkins toodeploy tooAzure App Service on Linux through Docker</span></span> 
