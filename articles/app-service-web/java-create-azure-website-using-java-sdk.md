---
title: "aaaCreate webové aplikace ve službě Azure App Service pomocí hello Azure SDK pro jazyk Java"
description: "Zjistěte, jak hello toocreate webové aplikace v Azure App Service programově pomocí sady Azure SDK pro jazyk Java."
tags: azure-classic-portal
services: app-service-web
documentationcenter: Java
author: donntrenton
manager: erikre
editor: jimbe
ms.assetid: 8954c456-1275-4d57-aff4-ca7d6374b71e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 02/25/2016
ms.author: v-donntr
ms.openlocfilehash: 42ba86b7fbb5668b3675198d0c5bb454525f706b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-in-azure-app-service-using-hello-azure-sdk-for-java"></a><span data-ttu-id="34eea-103">Vytvoření webové aplikace v Azure App Service pomocí hello Azure SDK pro jazyk Java</span><span class="sxs-lookup"><span data-stu-id="34eea-103">Create a Web App in Azure App Service using hello Azure SDK for Java</span></span>
<!-- Azure Active Directory workflow is not yet available on hello Azure Portal -->

## <a name="overview"></a><span data-ttu-id="34eea-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="34eea-104">Overview</span></span>
<span data-ttu-id="34eea-105">Tento návod ukazuje, jak toocreate sadu Azure SDK pro aplikace Java, která vytvoří webovou aplikaci v [Azure App Service][Azure App Service], pak nasazení tooit aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-105">This walkthrough shows you how toocreate an Azure SDK for Java application that creates a Web App in [Azure App Service][Azure App Service], then deploy an application tooit.</span></span> <span data-ttu-id="34eea-106">Skládá se ze dvou částí:</span><span class="sxs-lookup"><span data-stu-id="34eea-106">It consists of two parts:</span></span>

* <span data-ttu-id="34eea-107">Část 1 ukazuje, jak toobuild aplikaci Java, vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="34eea-107">Part 1 demonstrates how toobuild a Java application that creates a web app.</span></span>
* <span data-ttu-id="34eea-108">Část 2 ukazuje, jak toocreate jednoduché JSP "Hello World" aplikace a pak použijte k serveru FTP klienta toodeploy kód tooApp služby.</span><span class="sxs-lookup"><span data-stu-id="34eea-108">Part 2 demonstrates how toocreate a simple JSP "Hello World" application, then use an FTP client toodeploy code tooApp Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="34eea-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="34eea-109">Prerequisites</span></span>
### <a name="software-installations"></a><span data-ttu-id="34eea-110">Instalace softwaru</span><span class="sxs-lookup"><span data-stu-id="34eea-110">Software Installations</span></span>
<span data-ttu-id="34eea-111">Hello AzureWebDemo kódu aplikace v tomto článku byla zapsána pomocí sady Azure Java SDK 0.7.0, které můžete nainstalovat pomocí hello [instalačního programu webové platformy] [ Web Platform Installer] (WebPI).</span><span class="sxs-lookup"><span data-stu-id="34eea-111">hello AzureWebDemo application code in this article was written using Azure Java SDK 0.7.0, which you can install using hello [Web Platform Installer][Web Platform Installer] (WebPI).</span></span> <span data-ttu-id="34eea-112">Kromě toho, ujistěte se, že toouse hello nejnovější verzi hello [nástrojů Azure pro Eclipse][Azure Toolkit for Eclipse].</span><span class="sxs-lookup"><span data-stu-id="34eea-112">In addition, make sure toouse hello latest version of hello [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="34eea-113">Po instalaci hello SDK, aktualizace hello závislosti ve vašem projektu Eclipse spuštěním **aktualizovat Index** v **Maven úložiště**, pak znovu přidejte hello nejnovější verze jednotlivých balíčků v hello  **Závislosti** okno.</span><span class="sxs-lookup"><span data-stu-id="34eea-113">After you install hello SDK, update hello dependencies in your Eclipse project by running **Update Index** in **Maven Repositories**, then re-add hello latest version of each package in hello **Dependencies** window.</span></span> <span data-ttu-id="34eea-114">Hello verzi nainstalovaného softwaru v prostředí Eclipse můžete ověřit kliknutím **pomoci > podrobné informace o instalaci**; můžete by měl mít aspoň hello následující verze:</span><span class="sxs-lookup"><span data-stu-id="34eea-114">You can verify hello version of your installed software in Eclipse by clicking **Help > Installation Details**; you should have at least hello following versions:</span></span>

* <span data-ttu-id="34eea-115">Balíček pro knihovny Microsoft Azure Libraries for Java 0.7.0.20150309</span><span class="sxs-lookup"><span data-stu-id="34eea-115">Package for Microsoft Azure Libraries for Java 0.7.0.20150309</span></span>
* <span data-ttu-id="34eea-116">Integrované vývojové prostředí pro vývojáře v jazyce Java EE 4.4.2.20150219 Eclipse</span><span class="sxs-lookup"><span data-stu-id="34eea-116">Eclipse IDE for Java EE Developers 4.4.2.20150219</span></span>

### <a name="create-and-configure-cloud-resources-in-azure"></a><span data-ttu-id="34eea-117">Vytvoření a konfigurace cloudové prostředky v Azure</span><span class="sxs-lookup"><span data-stu-id="34eea-117">Create and Configure Cloud Resources in Azure</span></span>
<span data-ttu-id="34eea-118">Před zahájením tohoto postupu, potřebujete toohave aktivní předplatné Azure a nastavit výchozí Active Directory (AD) v Azure.</span><span class="sxs-lookup"><span data-stu-id="34eea-118">Before you begin this procedure, you need toohave an active Azure subscription and set up a default Active Directory (AD) on Azure.</span></span>

### <a name="create-an-active-directory-ad-in-azure"></a><span data-ttu-id="34eea-119">Vytvořte v Azure Active Directory (AD)</span><span class="sxs-lookup"><span data-stu-id="34eea-119">Create an Active Directory (AD) in Azure</span></span>
<span data-ttu-id="34eea-120">Pokud jste již nemají Active Directory (AD) na vaše předplatné Azure, přihlaste se k hello [portál Azure classic] [ Azure classic portal] s vaším účtem Microsoft.</span><span class="sxs-lookup"><span data-stu-id="34eea-120">If you do not already have an Active Directory (AD) on your Azure subscription, log into hello [Azure classic portal][Azure classic portal] with your Microsoft account.</span></span> <span data-ttu-id="34eea-121">Pokud máte více předplatných, klikněte na tlačítko **odběry** a vyberte hello výchozí adresář pro předplatné hello chcete toouse pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="34eea-121">If you have multiple subscriptions, click **Subscriptions** and select hello default directory for hello subscription you want toouse for this project.</span></span> <span data-ttu-id="34eea-122">Pak klikněte na tlačítko **použít** zobrazení odběru toothat tooswitch.</span><span class="sxs-lookup"><span data-stu-id="34eea-122">Then click **Apply** tooswitch toothat subscription view.</span></span>

1. <span data-ttu-id="34eea-123">Vyberte **služby Active Directory** hello nabídce na levé straně.</span><span class="sxs-lookup"><span data-stu-id="34eea-123">Select **Active Directory** from hello menu at left.</span></span> <span data-ttu-id="34eea-124">**Klikněte na tlačítko Nový > adresář > vytvořit vlastní**.</span><span class="sxs-lookup"><span data-stu-id="34eea-124">**Click New > Directory > Custom Create**.</span></span>
2. <span data-ttu-id="34eea-125">V **přidat adresář**, vyberte **vytvořte nový adresář**.</span><span class="sxs-lookup"><span data-stu-id="34eea-125">In **Add Directory**, select **Create New Directory**.</span></span>
3. <span data-ttu-id="34eea-126">V **název**, zadejte název adresáře.</span><span class="sxs-lookup"><span data-stu-id="34eea-126">In **Name**, enter a directory name.</span></span>
4. <span data-ttu-id="34eea-127">V **domény**, zadejte název domény.</span><span class="sxs-lookup"><span data-stu-id="34eea-127">In **Domain**, enter a domain name.</span></span> <span data-ttu-id="34eea-128">Toto je název základní domény, který je zahrnut ve výchozím nastavení s adresářem; má hello formuláře `<domain_name>.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="34eea-128">This is a basic domain name that is included by default with your directory; it has hello form `<domain_name>.onmicrosoft.com`.</span></span> <span data-ttu-id="34eea-129">Můžete pojmenovat podle názvu adresáře hello nebo jiný název domény, který vlastníte.</span><span class="sxs-lookup"><span data-stu-id="34eea-129">You can name it based on hello directory name or another domain name that you own.</span></span> <span data-ttu-id="34eea-130">Později můžete přidat jiný název domény, který vaše organizace už používá.</span><span class="sxs-lookup"><span data-stu-id="34eea-130">Later, you can add another domain name that your organization already uses.</span></span>
5. <span data-ttu-id="34eea-131">V **zemi nebo oblast**, vyberte národní prostředí.</span><span class="sxs-lookup"><span data-stu-id="34eea-131">In **Country or region**, select your locale.</span></span>

<span data-ttu-id="34eea-132">Další informace o AD, najdete v části [co je adresář Azure AD][What is an Azure AD directory]?</span><span class="sxs-lookup"><span data-stu-id="34eea-132">For more information on AD, see [What is an Azure AD directory][What is an Azure AD directory]?</span></span>

### <a name="create-a-management-certificate-for-azure"></a><span data-ttu-id="34eea-133">Vytvoření certifikátu správy pro Azure.</span><span class="sxs-lookup"><span data-stu-id="34eea-133">Create a Management Certificate for Azure</span></span>
<span data-ttu-id="34eea-134">Hello Azure SDK pro jazyk Java používá tooauthenticate certifikáty správy s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="34eea-134">hello Azure SDK for Java uses management certificates tooauthenticate with Azure subscriptions.</span></span> <span data-ttu-id="34eea-135">Toto jsou certifikáty X.509 v3, že používáte tooauthenticate klientská aplikace, která používá tooact hello Service Management API jménem prostředky předplatného toomanage vlastníka předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-135">These are X.509 v3 certificates you use tooauthenticate a client application that uses hello Service Management API tooact on behalf of hello subscription owner toomanage subscription resources.</span></span>

<span data-ttu-id="34eea-136">Hello kód v tomto postupu používá certifikát podepsaný svým držitelem tooauthenticate s Azure.</span><span class="sxs-lookup"><span data-stu-id="34eea-136">hello code in this procedure uses a self-signed certificate tooauthenticate with Azure.</span></span> <span data-ttu-id="34eea-137">Pro tento postup potřebujete toocreate certifikát a nahrajte ho toohello [portál Azure classic] [ Azure classic portal] předem.</span><span class="sxs-lookup"><span data-stu-id="34eea-137">For this procedure, you need toocreate a certificate and upload it toohello [Azure classic portal][Azure classic portal] beforehand.</span></span> <span data-ttu-id="34eea-138">To zahrnuje hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="34eea-138">This involves hello following steps:</span></span>

* <span data-ttu-id="34eea-139">Generování souboru PFX představující klientský certifikát a uložte ho místně.</span><span class="sxs-lookup"><span data-stu-id="34eea-139">Generate a PFX file representing your client certificate and save it locally.</span></span>
* <span data-ttu-id="34eea-140">Vygenerujte certifikát správy (soubor CER) ze souboru PFX hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-140">Generate a management certificate (CER file) from hello PFX file.</span></span>
* <span data-ttu-id="34eea-141">Nahrajte tooyour soubor CER hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="34eea-141">Upload hello CER file tooyour Azure subscription.</span></span>
* <span data-ttu-id="34eea-142">Soubor PFX hello převeďte JKS, protože Java používá tento formát tooauthenticate pomocí certifikátů.</span><span class="sxs-lookup"><span data-stu-id="34eea-142">Convert hello PFX file into JKS, because Java uses that format tooauthenticate using certificates.</span></span>
* <span data-ttu-id="34eea-143">Zápis aplikace hello ověřovací kód, který odkazuje toohello místního JKS souboru.</span><span class="sxs-lookup"><span data-stu-id="34eea-143">Write hello application's authentication code, which refers toohello local JKS file.</span></span>

<span data-ttu-id="34eea-144">Po dokončení tohoto postupu hello CER certifikát se bude nacházet ve vašem předplatném Azure a certifikát JKS hello se bude nacházet na místním disku.</span><span class="sxs-lookup"><span data-stu-id="34eea-144">When you complete this procedure, hello CER certificate will reside in your Azure subscription and hello JKS certificate will reside on your local drive.</span></span> <span data-ttu-id="34eea-145">Další informace o certifikáty pro správu najdete v tématu [vytvoření a nahrání certifikátu pro správu pro Azure][Create and Upload a Management Certificate for Azure].</span><span class="sxs-lookup"><span data-stu-id="34eea-145">For more information on management certificates, see [Create and Upload a Management Certificate for Azure][Create and Upload a Management Certificate for Azure].</span></span>

#### <a name="create-a-certificate"></a><span data-ttu-id="34eea-146">Vytvoření certifikátu</span><span class="sxs-lookup"><span data-stu-id="34eea-146">Create a certificate</span></span>
<span data-ttu-id="34eea-147">toocreate vlastní certifikátu podepsaného svým držitelem, otevřete konzolu příkaz v operačním systému a spusťte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-147">toocreate your own self-signed certificate, open a command console on your operating system and run hello following commands.</span></span>

> <span data-ttu-id="34eea-148">**Poznámka:** hello počítače, na kterém spustíte tento příkaz musí mít hello JDK nainstalována.</span><span class="sxs-lookup"><span data-stu-id="34eea-148">**Note:**  hello computer on which you run this command must have hello JDK installed.</span></span> <span data-ttu-id="34eea-149">Hello cesta toohello keytool navíc závisí na hello umístění, ve kterém nainstalujete hello JDK.</span><span class="sxs-lookup"><span data-stu-id="34eea-149">Also, hello path toohello keytool depends on hello location in which you install hello JDK.</span></span> <span data-ttu-id="34eea-150">Další informace najdete v tématu [klíč a nástroj pro správu certifikát (keytool)] [ Key and Certificate Management Tool (keytool)] v online dokumentaci Java hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-150">For more information, see [Key and Certificate Management Tool (keytool)][Key and Certificate Management Tool (keytool)] in hello Java online docs.</span></span>
> 
> 

<span data-ttu-id="34eea-151">soubor .pfx toocreate hello:</span><span class="sxs-lookup"><span data-stu-id="34eea-151">toocreate hello .pfx file:</span></span>

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

<span data-ttu-id="34eea-152">soubor .cer toocreate hello:</span><span class="sxs-lookup"><span data-stu-id="34eea-152">toocreate hello .cer file:</span></span>

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

<span data-ttu-id="34eea-153">Kde:</span><span class="sxs-lookup"><span data-stu-id="34eea-153">where:</span></span>

* <span data-ttu-id="34eea-154">`<java-install-dir>`je hello cesta toohello adresář, do které jste nainstalovali Java.</span><span class="sxs-lookup"><span data-stu-id="34eea-154">`<java-install-dir>` is hello path toohello directory in which you installed Java.</span></span>
* <span data-ttu-id="34eea-155">`<keystore-id>`je identifikátor položky hello úložiště klíčů (například `AzureRemoteAccess`).</span><span class="sxs-lookup"><span data-stu-id="34eea-155">`<keystore-id>` is hello keystore entry identifier (for example, `AzureRemoteAccess`).</span></span>
* <span data-ttu-id="34eea-156">`<cert-store-dir>`je hello cesta toohello adresář, ve kterém chcete toostore certifikáty (například `C:/Certificates`).</span><span class="sxs-lookup"><span data-stu-id="34eea-156">`<cert-store-dir>` is hello path toohello directory in which you want toostore certificates (for example `C:/Certificates`).</span></span>
* <span data-ttu-id="34eea-157">`<cert-file-name>`je hello název souboru certifikátu hello (například `AzureWebDemoCert`).</span><span class="sxs-lookup"><span data-stu-id="34eea-157">`<cert-file-name>` is hello name of hello certificate file (for example `AzureWebDemoCert`).</span></span>
* <span data-ttu-id="34eea-158">`<password>`je heslo hello zvolíte certifikát hello tooprotect; musí být alespoň 6 znaků.</span><span class="sxs-lookup"><span data-stu-id="34eea-158">`<password>` is hello password you choose tooprotect hello certificate; it must be at least 6 characters long.</span></span> <span data-ttu-id="34eea-159">Žádné heslo, můžete zadat, i když se to nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="34eea-159">You can enter no password, although this is not recommended.</span></span>
* <span data-ttu-id="34eea-160">`<dname>`hello toobe X.500 rozlišující název přidružené alias a slouží jako hello vystavitele a pole předmětu v certifikátu podepsaného svým držitelem hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-160">`<dname>` is hello X.500 Distinguished Name toobe associated with alias, and is used as hello issuer and subject fields in hello self-signed certificate.</span></span>

<span data-ttu-id="34eea-161">Další informace najdete v tématu [vytvoření a nahrání certifikátu pro správu pro Azure][Create and Upload a Management Certificate for Azure].</span><span class="sxs-lookup"><span data-stu-id="34eea-161">For more information, see [Create and Upload a Management Certificate for Azure][Create and Upload a Management Certificate for Azure].</span></span>

#### <a name="upload-hello-certificate"></a><span data-ttu-id="34eea-162">Nahrát na server certifikát hello</span><span class="sxs-lookup"><span data-stu-id="34eea-162">Upload hello certificate</span></span>
<span data-ttu-id="34eea-163">tooupload tooAzure certifikát podepsaný svým držitelem přejděte toohello **nastavení** stránky portálu classic hello a pak klikněte na hello **certifikáty pro správu** kartě. Klikněte na tlačítko **nahrát** dole hello hello stránky a přejděte toohello umístění souboru CER hello jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="34eea-163">tooupload a self-signed certificate tooAzure, go toohello **Settings** page in hello classic portal, then click hello **Management Certificates** tab. Click **Upload** at hello bottom of hello page and navigate toohello location of hello CER file you created.</span></span>

#### <a name="convert-hello-pfx-file-into-jks"></a><span data-ttu-id="34eea-164">Převést soubor PFX hello JKS</span><span class="sxs-lookup"><span data-stu-id="34eea-164">Convert hello PFX file into JKS</span></span>
<span data-ttu-id="34eea-165">V hello příkazového řádku systému Windows (spuštění jako správce), disk cd toohello adresář obsahující hello certifikáty a spusťte hello následující příkaz, kde `<java-install-dir>` je hello adresář, do které jste nainstalovali Java ve vašem počítači:</span><span class="sxs-lookup"><span data-stu-id="34eea-165">In hello Windows Command Prompt (running as admin), cd toohello directory containing hello certificates and run hello following command, where `<java-install-dir>` is hello directory in which you installed Java on your computer:</span></span>

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. <span data-ttu-id="34eea-166">Po zobrazení výzvy zadejte heslo úložiště klíčů cílové hello; bude jím hello heslo pro soubor JKS hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-166">When prompted, enter hello destination keystore password; this will be hello password for hello JKS file.</span></span>
2. <span data-ttu-id="34eea-167">Po zobrazení výzvy zadejte heslo úložiště klíčů zdroj hello; Toto je hello heslo, které jste zadali pro soubor PFX hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-167">When prompted, enter hello source keystore password; this is hello password you specified for hello PFX file.</span></span>

<span data-ttu-id="34eea-168">dvě hesla Hello nemají toobe hello stejné.</span><span class="sxs-lookup"><span data-stu-id="34eea-168">hello two passwords do not have toobe hello same.</span></span> <span data-ttu-id="34eea-169">Žádné heslo, můžete zadat, i když se to nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="34eea-169">You can enter no password, although this is not recommended.</span></span>

## <a name="build-a-web-app-creation-application"></a><span data-ttu-id="34eea-170">Vytvoření aplikace vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="34eea-170">Build a Web App creation application</span></span>
### <a name="create-hello-eclipse-workspace-and-maven-project"></a><span data-ttu-id="34eea-171">Vytvoření hello Eclipse prostoru a projekt Maven</span><span class="sxs-lookup"><span data-stu-id="34eea-171">Create hello Eclipse Workspace and Maven Project</span></span>
<span data-ttu-id="34eea-172">V této části vytvoříte pracovní prostor a projekt Maven pro hello webové aplikace vytváření aplikace, s názvem AzureWebDemo.</span><span class="sxs-lookup"><span data-stu-id="34eea-172">In this section you create a workspace and a Maven project for hello web app creation application, named AzureWebDemo.</span></span>

1. <span data-ttu-id="34eea-173">Vytvořte nový projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="34eea-173">Create a new Maven project.</span></span> <span data-ttu-id="34eea-174">Klikněte na tlačítko **soubor > Nový > Projekt Maven**.</span><span class="sxs-lookup"><span data-stu-id="34eea-174">Click **File > New > Maven Project**.</span></span> <span data-ttu-id="34eea-175">V **nový projekt Maven**, vyberte **vytvoření jednoduché projektu** a **používat výchozí umístění prostoru**.</span><span class="sxs-lookup"><span data-stu-id="34eea-175">In **New Maven Project**, select **Create a simple project** and **Use default workspace location**.</span></span>
2. <span data-ttu-id="34eea-176">Na druhé stránce hello **nový projekt Maven**, zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="34eea-176">On hello second page of **New Maven Project**, specify hello following:</span></span>
   
   * <span data-ttu-id="34eea-177">ID skupiny:`com.<username>.azure.webdemo`</span><span class="sxs-lookup"><span data-stu-id="34eea-177">Group ID: `com.<username>.azure.webdemo`</span></span>
   * <span data-ttu-id="34eea-178">ID artefaktů: AzureWebDemo</span><span class="sxs-lookup"><span data-stu-id="34eea-178">Artifact ID: AzureWebDemo</span></span>
   * <span data-ttu-id="34eea-179">Verze: 0.0.1-SNAPSHOT</span><span class="sxs-lookup"><span data-stu-id="34eea-179">Version: 0.0.1-SNAPSHOT</span></span>
   * <span data-ttu-id="34eea-180">Balení: jar</span><span class="sxs-lookup"><span data-stu-id="34eea-180">Packaging: jar</span></span>
   * <span data-ttu-id="34eea-181">Název: AzureWebDemo</span><span class="sxs-lookup"><span data-stu-id="34eea-181">Name: AzureWebDemo</span></span>
     
     <span data-ttu-id="34eea-182">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="34eea-182">Click **Finish**.</span></span>
3. <span data-ttu-id="34eea-183">Otevřete soubor pom.xml hello nový projekt v prohlížeči projektu.</span><span class="sxs-lookup"><span data-stu-id="34eea-183">Open hello new project's pom.xml file in Project Explorer.</span></span> <span data-ttu-id="34eea-184">Vyberte hello **závislosti** kartě. Toto je nový projekt, jsou uvedeny ještě žádné balíčky.</span><span class="sxs-lookup"><span data-stu-id="34eea-184">Select hello **Dependencies** tab. As this is a new project, no packages are listed yet.</span></span>
4. <span data-ttu-id="34eea-185">Otevřete hello Maven úložiště zobrazení.</span><span class="sxs-lookup"><span data-stu-id="34eea-185">Open hello Maven Repositories view.</span></span> <span data-ttu-id="34eea-186">**Klikněte na okno > Zobrazit zobrazení > jiné > Maven > Maven úložiště** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="34eea-186">**Click Window > Show View > Other > Maven > Maven Repositories** and click **OK**.</span></span> <span data-ttu-id="34eea-187">Hello **Maven úložiště** zobrazení se objeví v hello dolní části hello IDE.</span><span class="sxs-lookup"><span data-stu-id="34eea-187">hello **Maven Repositories** view will appear at hello bottom of hello IDE.</span></span>
5. <span data-ttu-id="34eea-188">Otevřete **globální úložiště**, klikněte pravým tlačítkem na hello **centrální** úložiště a vyberte **Rebuild Index**.</span><span class="sxs-lookup"><span data-stu-id="34eea-188">Open **Global Repositories**, right-click hello **central** repository, and select **Rebuild Index**.</span></span>
   
    ![][1]
   
    <span data-ttu-id="34eea-189">Tento krok může trvat několik minut v závislosti na rychlosti připojení hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-189">This step can take several minutes depending on hello speed of your connection.</span></span> <span data-ttu-id="34eea-190">Když znovu sestaví hello index, byste měli vidět hello balíčků Microsoft Azure v hello **centrální** Maven úložiště.</span><span class="sxs-lookup"><span data-stu-id="34eea-190">When hello index rebuilds, you should see hello Microsoft Azure packages in hello **central** Maven repository.</span></span>
6. <span data-ttu-id="34eea-191">V **závislosti**, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="34eea-191">In **Dependencies**, click **Add**.</span></span> <span data-ttu-id="34eea-192">V **zadejte ID skupiny...**  zadejte `azure-management`.</span><span class="sxs-lookup"><span data-stu-id="34eea-192">In **Enter Group ID...** enter `azure-management`.</span></span> <span data-ttu-id="34eea-193">Vyberte hello balíčky pro základní správu a správu App Service Web Apps:</span><span class="sxs-lookup"><span data-stu-id="34eea-193">Select hello packages for base management and App Service Web Apps management:</span></span>
   
        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites
   
   > <span data-ttu-id="34eea-194">**Poznámka:** hello závislosti při aktualizaci po nové verze verze, je nutné toore-přidejte všechny závislosti hello v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="34eea-194">**Note:** If you are updating hello dependencies after a new version release, you need toore-add each of hello dependencies in this list.</span></span>
   > <span data-ttu-id="34eea-195">Po kliknutí na tlačítko **přidat** a vyberte každá závislost, zobrazí se s hello nové číslo verze v hello **závislosti** seznamu.</span><span class="sxs-lookup"><span data-stu-id="34eea-195">After you click **Add** and select each dependency, it appears with hello new version number in hello **Dependencies** list.</span></span>
   > 
   > 

<span data-ttu-id="34eea-196">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="34eea-196">Click **OK**.</span></span> <span data-ttu-id="34eea-197">Hello Azure balíčky, pak se zobrazí v hello **závislosti** seznamu.</span><span class="sxs-lookup"><span data-stu-id="34eea-197">hello Azure packages then appear in hello **Dependencies** list.</span></span>

### <a name="writing-java-code-toocreate-a-web-app-by-calling-hello-azure-sdk"></a><span data-ttu-id="34eea-198">Psaní kódu v jazyce Java tooCreate webovou aplikaci pomocí volání hello Azure SDK</span><span class="sxs-lookup"><span data-stu-id="34eea-198">Writing Java Code tooCreate a Web App by Calling hello Azure SDK</span></span>
<span data-ttu-id="34eea-199">Dále napište hello kód, který volá rozhraní API v hello Azure SDK pro jazyk Java toocreate hello webové aplikace App Service.</span><span class="sxs-lookup"><span data-stu-id="34eea-199">Next, write hello code that calls APIs in hello Azure SDK for Java toocreate hello App Service web app.</span></span>

1. <span data-ttu-id="34eea-200">Vytvořte kód Java třídy toocontain hello hlavní vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="34eea-200">Create a Java class toocontain hello main entry point code.</span></span> <span data-ttu-id="34eea-201">V prohlížeči projektu klikněte pravým tlačítkem na uzel projektu hello a vyberte **nový > třída**.</span><span class="sxs-lookup"><span data-stu-id="34eea-201">In Project Explorer, right-click on hello project node and select **New > Class**.</span></span>
2. <span data-ttu-id="34eea-202">V **nová třída Java**, pojmenujte třídu hello `WebCreator` a zkontrolujte hello **veřejné statické void main** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="34eea-202">In **New Java Class**, name hello class `WebCreator` and check hello **public static void main** checkbox.</span></span> <span data-ttu-id="34eea-203">Výběr Hello by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="34eea-203">hello selections should appear as follows:</span></span>
   
    ![][2]
3. <span data-ttu-id="34eea-204">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="34eea-204">Click **Finish**.</span></span> <span data-ttu-id="34eea-205">soubor WebCreator.java Hello se zobrazí v prohlížeči projektu.</span><span class="sxs-lookup"><span data-stu-id="34eea-205">hello WebCreator.java file appears in Project Explorer.</span></span>

### <a name="calling-hello-azure-api-toocreate-an-app-service-web-app"></a><span data-ttu-id="34eea-206">Volání rozhraní API služby Azure tooCreate hello webové aplikace App Service</span><span class="sxs-lookup"><span data-stu-id="34eea-206">Calling hello Azure API tooCreate an App Service Web App</span></span>
#### <a name="add-necessary-imports"></a><span data-ttu-id="34eea-207">Přidejte potřebné importy</span><span class="sxs-lookup"><span data-stu-id="34eea-207">Add necessary imports</span></span>
<span data-ttu-id="34eea-208">V WebCreator.java přidejte následující importy; hello Tyto importy poskytují přístup tooclasses v hello knihovny správy pro použití rozhraní API Správce Azure:</span><span class="sxs-lookup"><span data-stu-id="34eea-208">In WebCreator.java, add hello following imports; these imports provide access tooclasses in hello management libraries for consuming Azure APIs:</span></span>

    // General imports
    import java.net.URI;
    import java.util.ArrayList;

    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;

    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;

    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;

    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-hello-main-entry-point-class"></a><span data-ttu-id="34eea-209">Definování hello hlavní vstupní bod – třída</span><span class="sxs-lookup"><span data-stu-id="34eea-209">Define hello main entry point class</span></span>
<span data-ttu-id="34eea-210">Protože hello účelem hello AzureWebDemo aplikace je toocreate webové aplikace App Service, name hello hlavní třídy pro tuto aplikaci `WebAppCreator`.</span><span class="sxs-lookup"><span data-stu-id="34eea-210">Because hello purpose of hello AzureWebDemo application is toocreate an App Service Web App, name hello main class for this application `WebAppCreator`.</span></span> <span data-ttu-id="34eea-211">Tato třída poskytuje hello hlavní vstupní bod kód, který volá hello Azure Service Management API toocreate hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-211">This class provides hello main entry point code that calls hello Azure Service Management API toocreate hello web app.</span></span>

<span data-ttu-id="34eea-212">Přidejte následující definice parametr hello webové aplikace a webový prostor hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-212">Add hello following parameter definitions for hello web app and webspace.</span></span> <span data-ttu-id="34eea-213">Je nutné tooprovide své vlastní informace ID a certifikát pro předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="34eea-213">You will need tooprovide your own Azure subscription ID and certificate information.</span></span>

    public class WebAppCreator {

        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";

        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

<span data-ttu-id="34eea-214">Kde:</span><span class="sxs-lookup"><span data-stu-id="34eea-214">where:</span></span>

* <span data-ttu-id="34eea-215">`<subscription-id>`je ID hello předplatného Azure, ve kterém chcete toocreate hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="34eea-215">`<subscription-id>` is hello Azure subscription ID in which you want toocreate hello resource.</span></span>
* <span data-ttu-id="34eea-216">`<certificate-store-path>`je hello cestu a název souboru toohello JKS soubor v adresáři úložiště místní certifikát.</span><span class="sxs-lookup"><span data-stu-id="34eea-216">`<certificate-store-path>` is hello path and filename toohello JKS file in your local certificate store directory.</span></span> <span data-ttu-id="34eea-217">Například `C:/Certificates/CertificateName.jks` pro Linux a `C:\Certificates\CertificateName.jks` pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="34eea-217">For example, `C:/Certificates/CertificateName.jks` for Linux and `C:\Certificates\CertificateName.jks` for Windows.</span></span>
* <span data-ttu-id="34eea-218">`<certificate-password>`je hello heslo, které jste zadali při vytváření vašeho JKS certifikátu.</span><span class="sxs-lookup"><span data-stu-id="34eea-218">`<certificate-password>` is hello password you specified when you created your JKS certificate.</span></span>
* <span data-ttu-id="34eea-219">`webAppName`může být jakýkoli název, který zvolíte; Tento postup používá název hello `WebDemoWebApp`.</span><span class="sxs-lookup"><span data-stu-id="34eea-219">`webAppName` can be any name you choose; this procedure uses hello name `WebDemoWebApp`.</span></span> <span data-ttu-id="34eea-220">Hello úplný název domény je hello `webAppName` s hello `domainName` připojí, takže v tomto případě hello úplné doména je `webdemowebapp.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="34eea-220">hello full domain name is hello `webAppName` with hello `domainName` appended, so in this case hello full domain is `webdemowebapp.azurewebsites.net`.</span></span>
* <span data-ttu-id="34eea-221">`domainName`musí být zadán jako v příkladu nahoře.</span><span class="sxs-lookup"><span data-stu-id="34eea-221">`domainName` should be specified as shown above.</span></span>
* <span data-ttu-id="34eea-222">`webSpaceName`musí být jedna z hodnot hello definované v hello [WebSpaceNames] [ WebSpaceNames] třídy.</span><span class="sxs-lookup"><span data-stu-id="34eea-222">`webSpaceName` should be one of hello values defined in hello [WebSpaceNames][WebSpaceNames] class.</span></span>
* <span data-ttu-id="34eea-223">`appServicePlanName`musí být zadán jako v příkladu nahoře.</span><span class="sxs-lookup"><span data-stu-id="34eea-223">`appServicePlanName` should be specified as shown above.</span></span>

> <span data-ttu-id="34eea-224">**Poznámka:** pokaždé, když jste tuto aplikaci spustit, musíte hodnotu hello toochange `webAppName` a `appServicePlanName` (nebo odstranit hello webovou aplikaci na portálu Azure hello) před spuštěním hello aplikaci znovu.</span><span class="sxs-lookup"><span data-stu-id="34eea-224">**Note:** Each time you run this application, you need toochange hello value of `webAppName` and `appServicePlanName` (or delete hello web app on hello Azure Portal) before running hello application again.</span></span> <span data-ttu-id="34eea-225">Jinak se spuštění selže, protože hello stejný prostředek již existuje v Azure.</span><span class="sxs-lookup"><span data-stu-id="34eea-225">Otherwise, execution will fail because hello same resource already exists on Azure.</span></span>
> 
> 

#### <a name="define-hello-web-creation-method"></a><span data-ttu-id="34eea-226">Zadejte způsob vytvoření webové hello</span><span class="sxs-lookup"><span data-stu-id="34eea-226">Define hello web creation method</span></span>
<span data-ttu-id="34eea-227">V dalším kroku definujte metoda toocreate hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-227">Next, define a method toocreate hello web app.</span></span> <span data-ttu-id="34eea-228">Tato metoda `createWebApp`, určuje parametry hello hello webové aplikace a webový prostor hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-228">This method, `createWebApp`, specifies hello parameters of hello web app and hello webspace.</span></span> <span data-ttu-id="34eea-229">Také vytváří a konfiguruje hello App Service Web Apps správy klienta, který je definovaný hello [WebSiteManagementClient] [ WebSiteManagementClient] objektu.</span><span class="sxs-lookup"><span data-stu-id="34eea-229">It also creates and configures hello App Service Web Apps management client, which is defined by hello [WebSiteManagementClient][WebSiteManagementClient] object.</span></span> <span data-ttu-id="34eea-230">Klient správy Hello je klíče toocreating webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-230">hello management client is key toocreating Web Apps.</span></span> <span data-ttu-id="34eea-231">Poskytuje RESTful webové služby, které umožňují toomanage aplikací webové aplikace (provádění operací, například jako vytvoření, aktualizace a odstranění) voláním rozhraní API pro správu služby hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-231">It provides RESTful web services that allow applications toomanage web apps (performing operations such as create, update, and delete) by calling hello service management API.</span></span>

    private static void createWebApp() throws Exception {

        // Specify configuration settings for hello App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path toohello JKS file
            keyStorePassword,  // Password for hello JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create hello App Service Web Apps management client toocall Azure APIs
        // and pass it hello App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for hello web app with hello specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that hello server farm name takes hello Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define hello web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create hello web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output hello HTTP status code of hello response; 200 indicates hello request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output hello name of hello web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

<span data-ttu-id="34eea-232">Kód Hello výstup hello stav protokolu HTTP odpovědi hello indikující úspěch nebo selhání a pokud bylo úspěšné, bude výstup hello název hello vytvořili webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="34eea-232">hello code will output hello HTTP status of hello response indicating success or failure, and if successful, will output hello name of hello created web app.</span></span>

#### <a name="define-hello-main-method"></a><span data-ttu-id="34eea-233">Definování hello main() – metoda</span><span class="sxs-lookup"><span data-stu-id="34eea-233">Define hello main() method</span></span>
<span data-ttu-id="34eea-234">Zadejte kód metoda main() hello volání createWebApp() toocreate hello webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="34eea-234">Provide hello main() method code that calls createWebApp() toocreate hello web app.</span></span>

<span data-ttu-id="34eea-235">Nakonec volání `createWebApp` z `main`:</span><span class="sxs-lookup"><span data-stu-id="34eea-235">Finally, call `createWebApp` from `main`:</span></span>

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-hello-application-and-verify-web-app-creation"></a><span data-ttu-id="34eea-236">Spuštění aplikace hello a ověřit vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="34eea-236">Run hello application and verify web app creation</span></span>
<span data-ttu-id="34eea-237">Klikněte na tlačítko tooverify, které vaše aplikace běží, **spustit > Spustit**.</span><span class="sxs-lookup"><span data-stu-id="34eea-237">tooverify that your application runs, click **Run > Run**.</span></span> <span data-ttu-id="34eea-238">Po dokončení spuštění aplikace hello byste měli vidět následující výstup v konzole Eclipse hello hello:</span><span class="sxs-lookup"><span data-stu-id="34eea-238">When hello application completes running, you should see hello following output in hello Eclipse console:</span></span>

    ----------
    Web app created - HTTP response 200

    ----------

    Name of web app created: WebDemoWebApp

    ----------

<span data-ttu-id="34eea-239">Přihlaste se k portálu Azure classic hello a klikněte na tlačítko **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="34eea-239">Log into hello Azure classic portal and click **Web Apps**.</span></span> <span data-ttu-id="34eea-240">Hello nové webové aplikace by měla zobrazí v seznamu webové aplikace hello během několika minut.</span><span class="sxs-lookup"><span data-stu-id="34eea-240">hello new web app should appear in hello Web Apps list within a few minutes.</span></span>

## <a name="deploying-an-application-toohello-web-app"></a><span data-ttu-id="34eea-241">Nasazení aplikace toohello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="34eea-241">Deploying an Application toohello Web App</span></span>
<span data-ttu-id="34eea-242">Po spustili AzureWebDemo a vytvořili hello novou webovou aplikaci, přihlaste se k portálu classic hello, klikněte na **webové aplikace**a vyberte **WebDemoWebApp** v hello **webové aplikace** seznamu.</span><span class="sxs-lookup"><span data-stu-id="34eea-242">After you have run AzureWebDemo and created hello new web app, log into hello classic portal, click **Web Apps**, and select **WebDemoWebApp** in hello **Web Apps** list.</span></span> <span data-ttu-id="34eea-243">Na stránce řídicího panelu hello webové aplikace, klikněte na tlačítko **Procházet** (nebo klikněte na adresu URL hello, `webdemowebapp.azurewebsites.net`) toonavigate tooit.</span><span class="sxs-lookup"><span data-stu-id="34eea-243">In hello web app's dashboard page, click **Browse** (or click hello URL, `webdemowebapp.azurewebsites.net`) toonavigate tooit.</span></span> <span data-ttu-id="34eea-244">Uvidíte zástupný symbol prázdné stránky, protože žádný obsah ještě nebyla toohello publikované webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-244">You will see a blank placeholder page, because no content has been published toohello web app yet.</span></span>

<span data-ttu-id="34eea-245">Budou vedle vytvořit aplikaci "Hello World" a nasaďte ho toohello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-245">Next you will create a "Hello World" application and deploy it toohello web app.</span></span>

### <a name="create-a-jsp-hello-world-application"></a><span data-ttu-id="34eea-246">Vytvoření aplikace JSP Hello World</span><span class="sxs-lookup"><span data-stu-id="34eea-246">Create a JSP Hello World application</span></span>
#### <a name="create-hello-application"></a><span data-ttu-id="34eea-247">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="34eea-247">Create hello application</span></span>
<span data-ttu-id="34eea-248">V pořadí toodemonstrate jak hello toodeploy toohello webové aplikace, následující postup ukazuje, jak toocreate jednoduchou aplikaci Java "Hello, World" a nahrajte ho toohello aplikace služby webové aplikace, kterou vaše aplikace vytvořena.</span><span class="sxs-lookup"><span data-stu-id="34eea-248">In order toodemonstrate how toodeploy an application toohello web, hello following procedure shows you how toocreate a simple "Hello World" Java application and upload it toohello App Service Web App that your application created.</span></span>

1. <span data-ttu-id="34eea-249">Klikněte na tlačítko **soubor > Nový > dynamického webového projektu**.</span><span class="sxs-lookup"><span data-stu-id="34eea-249">Click **File > New > Dynamic Web Project**.</span></span> <span data-ttu-id="34eea-250">Pojmenujte ji `JSPHello`.</span><span class="sxs-lookup"><span data-stu-id="34eea-250">Name it `JSPHello`.</span></span> <span data-ttu-id="34eea-251">Není nutné toochange další nastavení v tomto dialogu.</span><span class="sxs-lookup"><span data-stu-id="34eea-251">You do not need toochange any other settings in this dialog.</span></span> <span data-ttu-id="34eea-252">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="34eea-252">Click **Finish**.</span></span>
   
    ![][3]
2. <span data-ttu-id="34eea-253">V prohlížeči projektu rozbalte hello **JSPHello** projektu, klikněte pravým tlačítkem na **WebContent**, pak klikněte na tlačítko **nový > soubor JSP**.</span><span class="sxs-lookup"><span data-stu-id="34eea-253">In Project Explorer, expand hello **JSPHello** project, right-click **WebContent**, then click **New > JSP File**.</span></span> <span data-ttu-id="34eea-254">V dialogovém okně Nový soubor JSP hello, název nového souboru hello `index.jsp`.</span><span class="sxs-lookup"><span data-stu-id="34eea-254">In hello New JSP File dialog, name hello new file `index.jsp`.</span></span> <span data-ttu-id="34eea-255">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="34eea-255">Click **Next**.</span></span>
3. <span data-ttu-id="34eea-256">V hello **vybrat šablonu JSP** vyberte **nový soubor JSP (html)** a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="34eea-256">In hello **Select JSP Template** dialog, select **New JSP File (html)** and click **Finish**.</span></span>
4. <span data-ttu-id="34eea-257">V index.jsp, přidejte následující kód v hello hello `<head>` a `<body>` značky částech:</span><span class="sxs-lookup"><span data-stu-id="34eea-257">In index.jsp, add hello following code in hello `<head>` and `<body>` tag sections:</span></span>
   
        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
   
        <body>
          Hello, hello time is <%= date %> 
        </body>

#### <a name="run-hello-hello-world-application-in-localhost"></a><span data-ttu-id="34eea-258">Spuštění aplikace Hello World hello v localhost</span><span class="sxs-lookup"><span data-stu-id="34eea-258">Run hello Hello World application in localhost</span></span>
<span data-ttu-id="34eea-259">Než spustíte tuto aplikaci, musíte tooconfigure několik vlastností.</span><span class="sxs-lookup"><span data-stu-id="34eea-259">Before you run this application, you need tooconfigure a few properties.</span></span>

1. <span data-ttu-id="34eea-260">Klikněte pravým tlačítkem na hello **JSPHello** projektu a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="34eea-260">Right-click hello **JSPHello** project and select **Properties**.</span></span>
2. <span data-ttu-id="34eea-261">V hello **vlastnosti** dialogové okno: vyberte **cesta sestavení Java**, vyberte hello **pořadí a Export** zkontrolujte **prostředí JRE systémová knihovna**, pak klikněte na **Až** toomove ho toohello horní části seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-261">In hello **Properties** dialog: select **Java Build Path**, select hello **Order and Export** tab, check **JRE System Library**, then click **Up** toomove it toohello top of hello list.</span></span>
   
    ![][4]
3. <span data-ttu-id="34eea-262">Také v hello **vlastnosti** dialogu: vyberte **Targeted Runtimes** a klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="34eea-262">Also in hello **Properties** dialog: select **Targeted Runtimes** and click **New**.</span></span>
4. <span data-ttu-id="34eea-263">V hello **nové běhové prostředí serveru** dialogovém okně, vyberte server, jako **Apache Tomcat v7.0** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="34eea-263">In hello **New Server Runtime Environment** dialog, select a server such as **Apache Tomcat v7.0** and click **Next**.</span></span> <span data-ttu-id="34eea-264">V hello **Tomcat Server** dialogové okno, sada **název** příliš`Apache Tomcat v7.0`a nastavte **instalační adresář Tomcat** toohello adresář, do které jste nainstalovali verzi hello Chcete-li toouse server tomcat.</span><span class="sxs-lookup"><span data-stu-id="34eea-264">In hello **Tomcat Server** dialog, set **Name** too`Apache Tomcat v7.0`, and set **Tomcat Installation Directory** toohello directory in which you installed hello version of Tomcat server you want toouse.</span></span>
   
    ![][5]
   
    <span data-ttu-id="34eea-265">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="34eea-265">Click **Finish**.</span></span>
5. <span data-ttu-id="34eea-266">Pak se vraťte toohello **Targeted Runtimes** stránku hello **vlastnosti** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="34eea-266">You then return toohello **Targeted Runtimes** page of hello **Properties** dialog.</span></span> <span data-ttu-id="34eea-267">Vyberte **Apache Tomcat v7.0**, pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="34eea-267">Select **Apache Tomcat v7.0**, then click **OK**.</span></span>
   
    ![][6]
6. <span data-ttu-id="34eea-268">V hello Eclipse **spustit** nabídky, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="34eea-268">In hello Eclipse **Run** menu, click **Run**.</span></span> <span data-ttu-id="34eea-269">V hello **spustit jako** dialogovém okně, vyberte **spustit na serveru**.</span><span class="sxs-lookup"><span data-stu-id="34eea-269">In hello **Run As** dialog, select **Run on Server**.</span></span> <span data-ttu-id="34eea-270">V hello **spustit na serveru** dialogovém okně, vyberte **Tomcat v7.0 Server**:</span><span class="sxs-lookup"><span data-stu-id="34eea-270">In hello **Run on Server** dialog, select **Tomcat v7.0 Server**:</span></span>
   
    ![][7]
   
    <span data-ttu-id="34eea-271">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="34eea-271">Click **Finish**.</span></span>
7. <span data-ttu-id="34eea-272">Když hello spouštět aplikace, měli byste vidět hello **JSPHello** stránky se zobrazí v okně localhost v prostředí Eclipse (`http://localhost:8080/JSPHello/`), zobrazování hello následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="34eea-272">When hello application runs, you should see hello **JSPHello** page appear in a localhost window in Eclipse (`http://localhost:8080/JSPHello/`), displaying hello following message:</span></span>
   
    `Hello World, hello time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="export-hello-application-as-a-war"></a><span data-ttu-id="34eea-273">Exportovat jako WAR aplikace hello</span><span class="sxs-lookup"><span data-stu-id="34eea-273">Export hello application as a WAR</span></span>
<span data-ttu-id="34eea-274">Exportujte souborů projektu webové hello jako webový archiv (WAR) soubor tak, aby ho mohli nasadit toohello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-274">Export hello web project files as a web archive (WAR) file so that you can deploy it toohello web app.</span></span> <span data-ttu-id="34eea-275">Hello následující souborů projektu webové jsou umístěny ve složce WebContent hello:</span><span class="sxs-lookup"><span data-stu-id="34eea-275">hello following web project files reside in hello WebContent folder:</span></span>

    META-INF
    WEB-INF
    index.jsp

1. <span data-ttu-id="34eea-276">Klikněte pravým tlačítkem na složku WebContent hello a vyberte **exportovat**.</span><span class="sxs-lookup"><span data-stu-id="34eea-276">Right-click hello WebContent folder and select **Export**.</span></span>
2. <span data-ttu-id="34eea-277">V hello **Exportovat vyberte** dialogové okno, klikněte na tlačítko **webové > WAR** souboru a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="34eea-277">In hello **Export Select** dialog, click **Web > WAR** file, then click **Next**.</span></span>
3. <span data-ttu-id="34eea-278">V hello **WAR Export** dialogovém okně, vyberte adresář src hello v aktuálním projektu hello a obsahovat název hello hello souboru WAR na konci hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-278">In hello **WAR Export** dialog, select hello src directory in hello current project, and include hello name of hello WAR file at hello end.</span></span> <span data-ttu-id="34eea-279">Například:</span><span class="sxs-lookup"><span data-stu-id="34eea-279">For example:</span></span>
   
    `<project-path>/JSPHello/src/JSPHello.war`

<span data-ttu-id="34eea-280">Další informace o nasazení WAR soubory, najdete v části [přidat tooAzure aplikace Java App Service Web Apps](web-sites-java-add-app.md).</span><span class="sxs-lookup"><span data-stu-id="34eea-280">For more information on deploying WAR files, see [Add a Java application tooAzure App Service Web Apps](web-sites-java-add-app.md).</span></span>

### <a name="deploying-hello-hello-world-application-using-ftp"></a><span data-ttu-id="34eea-281">Nasazení hello Hello World aplikace pomocí FTP</span><span class="sxs-lookup"><span data-stu-id="34eea-281">Deploying hello Hello World Application Using FTP</span></span>
<span data-ttu-id="34eea-282">Vyberte aplikaci hello toopublish klienta FTP třetích stran.</span><span class="sxs-lookup"><span data-stu-id="34eea-282">Select a third-party FTP client toopublish hello application.</span></span> <span data-ttu-id="34eea-283">Tento postup popisuje dvě možnosti: konzoly Kudu hello integrovaný do Azure; a FileZilla oblíbených nástroj s praktické, grafické uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="34eea-283">This procedure describes two options: hello Kudu console built into Azure; and FileZilla, a popular tool with a convenient, graphical UI.</span></span>

> <span data-ttu-id="34eea-284">**Poznámka:** hello Azure nástrojů Eclipse podporuje účty toostorage nasazení a cloudové služby, ale aktuálně nepodporuje nasazení tooweb aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-284">**Note:** hello Azure Toolkit for Eclipse supports deployment toostorage accounts and cloud services, but does not currently support deployment tooweb apps.</span></span> <span data-ttu-id="34eea-285">Můžete nasadit toostorage účty a cloudových služeb pomocí projektu nasazení aplikace Azure, jak je popsáno v [vytvoření aplikace Hello World služby Azure v prostředí Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), ale není tooweb aplikací.</span><span class="sxs-lookup"><span data-stu-id="34eea-285">You can deploy toostorage accounts and cloud services using an Azure Deployment Project as described in [Creating a Hello World Application for Azure in Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), but not tooweb apps.</span></span> <span data-ttu-id="34eea-286">Používejte jiné metody, jako je například protokol FTP nebo Githubu tootransfer soubory tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-286">Use other methods such as FTP or GitHub tootransfer files tooyour web app.</span></span>
> 
> <span data-ttu-id="34eea-287">**Poznámka:** nedoporučujeme pomocí FTP z příkazového řádku Windows hello (hello příkazového řádku FTP.EXE nástroj, který se dodává s Windows).</span><span class="sxs-lookup"><span data-stu-id="34eea-287">**Note:** We do not recommend using FTP from hello Windows command prompt (hello command-line FTP.EXE utility that ships with Windows).</span></span> <span data-ttu-id="34eea-288">Klienti FTP, které používají active FTP, jako je například FTP.EXE, často převzetí služeb při selhání toowork brány firewall.</span><span class="sxs-lookup"><span data-stu-id="34eea-288">FTP clients that use active FTP, such as FTP.EXE, often fail toowork over firewalls.</span></span> <span data-ttu-id="34eea-289">Aktivní FTP určuje interní adresa založený na síti LAN, server toowhich k serveru FTP se pravděpodobně nezdaří tooconnect.</span><span class="sxs-lookup"><span data-stu-id="34eea-289">Active FTP specifies an internal LAN-based address, toowhich an FTP server will likely fail tooconnect.</span></span>
> 
> 

<span data-ttu-id="34eea-290">Další informace o nasazení tooan webové aplikace App Service pomocí protokolu FTP najdete v části hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="34eea-290">For more information on deployment tooan App Service web app using FTP, see hello following topics:</span></span>

* [<span data-ttu-id="34eea-291">Nasazení pomocí nástroje FTP</span><span class="sxs-lookup"><span data-stu-id="34eea-291">Deploy using an FTP utility</span></span>](web-sites-deploy.md)

#### <a name="set-up-deployment-credentials"></a><span data-ttu-id="34eea-292">Nastavit přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="34eea-292">Set up deployment credentials</span></span>
<span data-ttu-id="34eea-293">Zajistěte, aby spustíte hello **AzureWebDemo** toocreate aplikace webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-293">Make sure you have run hello **AzureWebDemo** application toocreate a web app.</span></span> <span data-ttu-id="34eea-294">Přenese soubory toothis umístění.</span><span class="sxs-lookup"><span data-stu-id="34eea-294">You will transfer files toothis location.</span></span>

1. <span data-ttu-id="34eea-295">Přihlaste se k portálu classic hello a klikněte na tlačítko **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="34eea-295">Log into hello classic portal and click **Web Apps**.</span></span> <span data-ttu-id="34eea-296">Zajistěte, aby **WebDemoWebApp** se zobrazí v hello seznam webových aplikací a ujistěte se, zda je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="34eea-296">Make sure **WebDemoWebApp** appears in hello list of web apps, and make sure that it is running.</span></span> <span data-ttu-id="34eea-297">Klikněte na tlačítko **WebDemoWebApp** tooopen jeho **řídicí panel** stránky.</span><span class="sxs-lookup"><span data-stu-id="34eea-297">Click **WebDemoWebApp** tooopen its **Dashboard** page.</span></span>
2. <span data-ttu-id="34eea-298">Na hello **řídicí panel** v části **rychlý přehled**, klikněte na tlačítko **nastavit přihlašovací údaje nasazení** (Pokud již máte přihlašovací údaje nasazení, to čte  **Resetovat přihlašovací údaje nasazení**).</span><span class="sxs-lookup"><span data-stu-id="34eea-298">On hello **Dashboard** page, under **Quick Glance**, click **Set up your deployment credentials** (if you already have deployment credentials, this reads **Reset your deployment credentials**).</span></span>
   
    <span data-ttu-id="34eea-299">Přihlašovací údaje nasazení jsou přidruženy k účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="34eea-299">Deployment credentials are associated with a Microsoft account.</span></span> <span data-ttu-id="34eea-300">Potřebujete toospecify uživatelské jméno a heslo, které můžete použít toodeploy pomocí Git a FTP.</span><span class="sxs-lookup"><span data-stu-id="34eea-300">You need toospecify a username and password that you can use toodeploy using Git and FTP.</span></span> <span data-ttu-id="34eea-301">Ve všech předplatných Azure spojené s vaším účtem Microsoft můžete použít tyto přihlašovací údaje toodeploy tooany webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-301">You can use these credentials toodeploy tooany web app in all Azure subscriptions associated with your Microsoft account.</span></span> <span data-ttu-id="34eea-302">Git a FTP nasazení pověření zadejte v dialogovém okně hello a záznamů hello uživatelské jméno a heslo pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="34eea-302">Provide Git and FTP deployment credentials in hello dialog, and record hello username and password for future use.</span></span>

#### <a name="get-ftp-connection-information"></a><span data-ttu-id="34eea-303">Získat informace o připojení FTP</span><span class="sxs-lookup"><span data-stu-id="34eea-303">Get FTP connection information</span></span>
<span data-ttu-id="34eea-304">toouse FTP toodeploy aplikace soubory toohello nově vytvořit webovou aplikaci, musíte tooobtain informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="34eea-304">toouse FTP toodeploy application files toohello newly created web app, you need tooobtain connection information.</span></span> <span data-ttu-id="34eea-305">Existují dva způsoby tooobtain informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="34eea-305">There are two ways tooobtain connection information.</span></span> <span data-ttu-id="34eea-306">Jedním ze způsobů je toovisit hello webové aplikace **řídicí panel** stránka; hello druhý způsob je profil publikování se toodownload hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-306">One way is toovisit hello web app's **Dashboard** page; hello other way is toodownload hello web app's publish profile.</span></span> <span data-ttu-id="34eea-307">profil publikování se Hello je soubor XML, který poskytuje informace, jako je název a přihlašovací údaje hostitele FTP pro vaše webové aplikace v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="34eea-307">hello publish profile is an XML file that provides information such as FTP host name and logon credentials for your web apps in Azure App Service.</span></span> <span data-ttu-id="34eea-308">Toto uživatelské jméno a heslo toodeploy tooany webové aplikace můžete použít v Všechna předplatná spojená s hello účet Azure, ne jenom tato jednoho.</span><span class="sxs-lookup"><span data-stu-id="34eea-308">You can use this username and password toodeploy tooany web app in all subscriptions associated with hello Azure account, not only this one.</span></span>

<span data-ttu-id="34eea-309">informace o připojení tooobtain FTP v okně hello webové aplikace v hello [portálu Azure][Azure Portal]:</span><span class="sxs-lookup"><span data-stu-id="34eea-309">tooobtain FTP connection information from hello web app's blade in hello [Azure Portal][Azure Portal]:</span></span>

1. <span data-ttu-id="34eea-310">V části **Essentials**, najít a zkopírovat hello **název hostitele FTP**.</span><span class="sxs-lookup"><span data-stu-id="34eea-310">Under **Essentials**, find and copy hello **FTP hostname**.</span></span> <span data-ttu-id="34eea-311">Toto je identifikátor URI, které jsou podobné příliš`ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="34eea-311">This is a URI similar too`ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.</span></span>
2. <span data-ttu-id="34eea-312">V části **Essentials**, najít a zkopírovat **uživatelské jméno FTP/nasazení**.</span><span class="sxs-lookup"><span data-stu-id="34eea-312">Under **Essentials**, find and copy **FTP/Deployment username**.</span></span> <span data-ttu-id="34eea-313">To bude mít hello formuláře *webappname\deployment-username*; například `WebDemoWebApp\deployer77`.</span><span class="sxs-lookup"><span data-stu-id="34eea-313">This will have hello form *webappname\deployment-username*; for example `WebDemoWebApp\deployer77`.</span></span>

<span data-ttu-id="34eea-314">profil publikování se informace o připojení FTP tooobtain z hello:</span><span class="sxs-lookup"><span data-stu-id="34eea-314">tooobtain FTP connection information from hello publish profile:</span></span>

1. <span data-ttu-id="34eea-315">V okně hello webové aplikace, klikněte na tlačítko **profilu publikování Get**.</span><span class="sxs-lookup"><span data-stu-id="34eea-315">In hello web app's blade, click **Get publish profile**.</span></span> <span data-ttu-id="34eea-316">To bude stahovat místní jednotku tooyour soubor .publishsettings.</span><span class="sxs-lookup"><span data-stu-id="34eea-316">This will download a .publishsettings file tooyour local drive.</span></span>
2. <span data-ttu-id="34eea-317">Otevřete soubor .publishsettings hello v editoru XML nebo textovém editoru a najděte hello `<publishProfile>` element obsahující `publishMethod="FTP"`.</span><span class="sxs-lookup"><span data-stu-id="34eea-317">Open hello .publishsettings file in an XML editor or text editor and find hello `<publishProfile>` element containing `publishMethod="FTP"`.</span></span> <span data-ttu-id="34eea-318">By měl vypadat jako následující hello:</span><span class="sxs-lookup"><span data-stu-id="34eea-318">It should look like hello following:</span></span>
   
        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>
3. <span data-ttu-id="34eea-319">Všimněte si, že webová aplikace hello `publishProfile` nastavení mapování toohello správce webu FileZilla nastavení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="34eea-319">Note that hello web app's `publishProfile` settings map toohello FileZilla Site Manager settings as follows:</span></span>

* <span data-ttu-id="34eea-320">`publishUrl`je stejný jako hello **název hostitele FTP**, hello hodnotu nastavenou v **hostitele**.</span><span class="sxs-lookup"><span data-stu-id="34eea-320">`publishUrl` is hello same as **FTP host name**, hello value you set in **Host**.</span></span>
* <span data-ttu-id="34eea-321">`publishMethod="FTP"`znamená, že nastavíte **protokol** příliš**FTP - File Transfer Protocol**, a **šifrování** příliš**pomocí protokolu FTP prostý**.</span><span class="sxs-lookup"><span data-stu-id="34eea-321">`publishMethod="FTP"` means that you set **Protocol** too**FTP - File Transfer Protocol**, and **Encryption** too**Use plain FTP**.</span></span>
* <span data-ttu-id="34eea-322">`userName`a `userPWD` jsou klíče pro hello skutečnými hodnotami uživatelské jméno a heslo jste zadali, když resetujete hello přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="34eea-322">`userName` and `userPWD` are keys for hello actual username and password values you specified when you reset hello deployment credentials.</span></span> <span data-ttu-id="34eea-323">`userName`je stejný jako hello **nasazení / FTP uživatele**.</span><span class="sxs-lookup"><span data-stu-id="34eea-323">`userName` is hello same as **Deployment / FTP user**.</span></span> <span data-ttu-id="34eea-324">Mapují příliš**uživatele** a **heslo** v FileZilla.</span><span class="sxs-lookup"><span data-stu-id="34eea-324">They map too**User** and **Password** in FileZilla.</span></span>
* <span data-ttu-id="34eea-325">`ftpPassiveMode="True"`znamená, že lokality hello FTP používá pasivní přenos FTP; Vyberte **pasivní** na hello **nastavení přenosu** kartě.</span><span class="sxs-lookup"><span data-stu-id="34eea-325">`ftpPassiveMode="True"` means that hello FTP site uses passive FTP transfer; select **Passive** on hello **Transfer Settings** tab.</span></span>

#### <a name="configure-hello-web-app-toohost-a-java-application"></a><span data-ttu-id="34eea-326">Konfigurovat webovou aplikaci toohost hello aplikace Java</span><span class="sxs-lookup"><span data-stu-id="34eea-326">Configure hello Web App toohost a Java application</span></span>
<span data-ttu-id="34eea-327">Před publikováním aplikace hello musíte toochange několik nastavení konfigurace tak, aby hello webové aplikace může hostovat aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="34eea-327">Before you publish hello application, you need toochange a few configuration settings so that hello web app can host a Java application.</span></span>

1. <span data-ttu-id="34eea-328">V portálu classic hello přejděte toohello webové aplikace **řídicí panel** a klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="34eea-328">In hello classic portal, go toohello web app's **Dashboard** page and click **Configure**.</span></span> <span data-ttu-id="34eea-329">Na hello **konfigurace** stránky, zadejte následující nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-329">On hello **Configure** page, specify hello following settings.</span></span>
2. <span data-ttu-id="34eea-330">V **verzi Javy** hello výchozí hodnota je **vypnout**; vyberte verzi Javy hello cíle vaší aplikace; například 1.7.0_51.</span><span class="sxs-lookup"><span data-stu-id="34eea-330">In **Java version** hello default is **Off**; select hello Java version your application targets; for example 1.7.0_51.</span></span> <span data-ttu-id="34eea-331">Až to uděláte, také zkontrolujte, zda **webový kontejner** nastavena tooa verzi serveru Tomcat.</span><span class="sxs-lookup"><span data-stu-id="34eea-331">After you do this, also make sure that **Web container** is set tooa version of Tomcat Server.</span></span>
3. <span data-ttu-id="34eea-332">V **výchozí dokumenty**přidejte index.jsp a přesunout nahoru toohello horní části seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-332">In **Default Documents**, add index.jsp and move it up toohello top of hello list.</span></span> <span data-ttu-id="34eea-333">(hello výchozí soubor pro webové aplikace je hostingstart.html.)</span><span class="sxs-lookup"><span data-stu-id="34eea-333">(hello default file for web apps is hostingstart.html.)</span></span>
4. <span data-ttu-id="34eea-334">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="34eea-334">Click **Save**.</span></span>

#### <a name="publish-your-application-using-kudu"></a><span data-ttu-id="34eea-335">Publikování aplikace pomocí modulu Kudu</span><span class="sxs-lookup"><span data-stu-id="34eea-335">Publish your application using Kudu</span></span>
<span data-ttu-id="34eea-336">Jedním ze způsobů toopublish hello aplikace je toouse hello ladění Kudu konzoly integrovaný do Azure.</span><span class="sxs-lookup"><span data-stu-id="34eea-336">One way toopublish hello application is toouse hello Kudu debug console built into Azure.</span></span> <span data-ttu-id="34eea-337">Kudu se označuje toobe stabilní a v souladu s App Service Web Apps a Tomcat Server.</span><span class="sxs-lookup"><span data-stu-id="34eea-337">Kudu is known toobe stable and consistent with App Service Web Apps and Tomcat Server.</span></span> <span data-ttu-id="34eea-338">Můžete procházením tooa URL hello následující formulář získali přístup ke konzole hello pro webovou aplikaci hello:</span><span class="sxs-lookup"><span data-stu-id="34eea-338">You access hello console for hello web app by browsing tooa URL of hello following form:</span></span>

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. <span data-ttu-id="34eea-339">Pro tento postup je umístěn v následující adresu URL; hello hello Kudu konzoly Procházet toothis umístění:</span><span class="sxs-lookup"><span data-stu-id="34eea-339">For this procedure, hello Kudu console is located at hello following URL; browse toothis location:</span></span>
   
    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`
2. <span data-ttu-id="34eea-340">Hello horní nabídce vyberte **ladění konzoly > CMD**.</span><span class="sxs-lookup"><span data-stu-id="34eea-340">From hello top menu, select **Debug Console > CMD**.</span></span>
3. <span data-ttu-id="34eea-341">V příkazovém řádku hello konzoly, přejděte příliš`/site/wwwroot` (nebo klikněte na tlačítko `site`, pak `wwwroot` v zobrazení adresáře hello hello horní části stránky hello):</span><span class="sxs-lookup"><span data-stu-id="34eea-341">In hello console command line, navigate too`/site/wwwroot` (or click `site`, then `wwwroot` in hello directory view at hello top of hello page):</span></span>
   
    `cd /site/wwwroot`
4. <span data-ttu-id="34eea-342">Po zadání **verzi Javy**, Tomcat server měli vytvořit adresáře webapps.</span><span class="sxs-lookup"><span data-stu-id="34eea-342">After you specify **Java version**, Tomcat server should create a webapps directory.</span></span> <span data-ttu-id="34eea-343">V příkazovém řádku hello konzoly přejděte adresáře webapps toohello:</span><span class="sxs-lookup"><span data-stu-id="34eea-343">In hello console command line, navigate toohello webapps directory:</span></span>
   
    `mkdir webapps`
   
    `cd webapps`
5. <span data-ttu-id="34eea-344">Přetáhněte JSPHello.war z `<project-path>/JSPHello/src/` a umístěte jej do zobrazení adresáře Kudu hello pod `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="34eea-344">Drag JSPHello.war from `<project-path>/JSPHello/src/` and drop it into hello Kudu directory view under `/site/wwwroot/webapps`.</span></span> <span data-ttu-id="34eea-345">Není ji přetáhněte toohello "Přetáhněte sem tooupload a zip" oblast, protože Tomcat bude rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="34eea-345">Do not drag it toohello "Drag here tooupload and zip" area, because Tomcat will unzip it.</span></span>
   
   ![][8]

<span data-ttu-id="34eea-346">V první JSPHello.war se zobrazí v oblasti directory hello samostatně:</span><span class="sxs-lookup"><span data-stu-id="34eea-346">At first JSPHello.war appears in hello directory area by itself:</span></span>

  ![][9]

<span data-ttu-id="34eea-347">V krátkém čase (pravděpodobně méně než 5 minut) bude Tomcat Server rozbalte soubor WAR hello do adresář rozbalené JSPHello.</span><span class="sxs-lookup"><span data-stu-id="34eea-347">In a short time (probably less than 5 minutes) Tomcat Server will unzip hello WAR file into an unpacked JSPHello directory.</span></span> <span data-ttu-id="34eea-348">Klikněte na tlačítko hello kořenový adresář toosee, zda index.jsp byla rozbalené a zkopírovat existuje.</span><span class="sxs-lookup"><span data-stu-id="34eea-348">Click hello ROOT directory toosee whether index.jsp has been unzipped and copied there.</span></span> <span data-ttu-id="34eea-349">Pokud ano, přejděte zpět toohello webapps directory toosee zda hello vybaleno JSPHello adresář byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="34eea-349">If so, navigate back toohello webapps directory toosee whether hello unpacked JSPHello directory has been created.</span></span> <span data-ttu-id="34eea-350">Pokud se tyto položky nezobrazí, počkejte a opakujte.</span><span class="sxs-lookup"><span data-stu-id="34eea-350">If you do not see these items, wait and repeat.</span></span>

  ![][10]

#### <a name="publish-your-application-using-filezilla-optional"></a><span data-ttu-id="34eea-351">Publikování aplikace pomocí FileZilla (volitelné)</span><span class="sxs-lookup"><span data-stu-id="34eea-351">Publish your application using FileZilla (optional)</span></span>
<span data-ttu-id="34eea-352">Jiný nástroj, můžete použít aplikaci hello toopublish je FileZilla oblíbených klienta FTP třetích stran s praktické, grafické uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="34eea-352">Another tool you can use toopublish hello application is FileZilla, a popular third-party FTP client with a convenient, graphical UI.</span></span> <span data-ttu-id="34eea-353">Můžete stáhnout a nainstalovat FileZilla z [http://filezilla-project.org/](http://filezilla-project.org/) Pokud již jste ji.</span><span class="sxs-lookup"><span data-stu-id="34eea-353">You can download and install FileZilla from [http://filezilla-project.org/](http://filezilla-project.org/) if you do not already have it.</span></span> <span data-ttu-id="34eea-354">Další informace o používání hello klienta najdete v tématu hello [FileZilla dokumentace](https://wiki.filezilla-project.org/Documentation) a tuto položku blogu na [klienti FTP – část 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).</span><span class="sxs-lookup"><span data-stu-id="34eea-354">For more information on using hello client, see hello [FileZilla documentation](https://wiki.filezilla-project.org/Documentation) and this blog entry on [FTP Clients - Part 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).</span></span>

1. <span data-ttu-id="34eea-355">V FileZilla, klikněte na **soubor > Správce webu**.</span><span class="sxs-lookup"><span data-stu-id="34eea-355">In FileZilla, click **File > Site Manager**.</span></span>
2. <span data-ttu-id="34eea-356">V hello **správce webu** dialogové okno, klikněte na tlačítko **nové lokality**.</span><span class="sxs-lookup"><span data-stu-id="34eea-356">In hello **Site Manager** dialog, click **New Site**.</span></span> <span data-ttu-id="34eea-357">Nový prázdný web FTP se zobrazí v **vyberte položku** výzvou tooprovide název.</span><span class="sxs-lookup"><span data-stu-id="34eea-357">A new blank FTP site will appear in **Select Entry** prompting you tooprovide a name.</span></span> <span data-ttu-id="34eea-358">Tento postup, pojmenujte ji `AzureWebDemo-FTP`.</span><span class="sxs-lookup"><span data-stu-id="34eea-358">For this procedure, name it `AzureWebDemo-FTP`.</span></span>
   
    <span data-ttu-id="34eea-359">Na hello **Obecné** zadejte hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="34eea-359">On hello **General** tab, specify hello following settings:</span></span>
   
   * <span data-ttu-id="34eea-360">**Hostitele:** Enter hello **název hostitele FTP** který jste zkopírovali z řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-360">**Host:** Enter hello **FTP Host Name** that you copied from hello dashboard.</span></span>
   * <span data-ttu-id="34eea-361">**Port:** (necháte prázdné, to je pasivní přenos a hello server určí hello port toouse.)</span><span class="sxs-lookup"><span data-stu-id="34eea-361">**Port:** (Leave this blank, as this is a passive transfer and hello server will determine hello port toouse.)</span></span>
   * <span data-ttu-id="34eea-362">**Protokol:** protokol pro přenos souborů FTP</span><span class="sxs-lookup"><span data-stu-id="34eea-362">**Protocol:** FTP File Transfer Protocol</span></span>
   * <span data-ttu-id="34eea-363">**Šifrování:** použít prostý FTP</span><span class="sxs-lookup"><span data-stu-id="34eea-363">**Encryption:** Use plain FTP</span></span>
   * <span data-ttu-id="34eea-364">**Typ přihlášení:** normální</span><span class="sxs-lookup"><span data-stu-id="34eea-364">**Logon Type:** Normal</span></span>
   * <span data-ttu-id="34eea-365">**Uživatel:** Enter hello nasazení / FTP uživatele, který jste zkopírovali z řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-365">**User:** Enter hello Deployment / FTP user that you copied from hello dashboard.</span></span> <span data-ttu-id="34eea-366">Toto je hello úplné FTP uživatelského jména, která má hello formuláře *webappname\username*.</span><span class="sxs-lookup"><span data-stu-id="34eea-366">This is hello full FTP username, which has hello form *webappname\username*.</span></span>
   * <span data-ttu-id="34eea-367">**Heslo:** zadejte hello heslo, kterou jste zadali, když nastavíte přihlašovací údaje nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-367">**Password:** Enter hello password that you specified when you set hello deployment credentials.</span></span>
     
     <span data-ttu-id="34eea-368">Na hello **nastavení přenosu** vyberte **pasivní**.</span><span class="sxs-lookup"><span data-stu-id="34eea-368">On hello **Transfer Settings** tab, select **Passive**.</span></span>
3. <span data-ttu-id="34eea-369">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="34eea-369">Click **Connect**.</span></span> <span data-ttu-id="34eea-370">Pokud úspěšné, je FileZilla konzoly se zobrazí `Status: Connected` zprávu a problém `LIST` příkaz obsah adresáře toolist hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-370">If successful, FileZilla's console will display a `Status: Connected` message and issue a `LIST` command toolist hello directory contents.</span></span>
4. <span data-ttu-id="34eea-371">V hello **místní** lokality panelů, vyberte hello zdrojový adresář ve který hello JSPHello.war soubor nachází; hello cesta bude podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="34eea-371">In hello **Local** site panel, select hello source directory in which hello JSPHello.war file resides; hello path will be similar toohello following:</span></span>
   
    `<project-path>/JSPHello/src/`
5. <span data-ttu-id="34eea-372">V hello **vzdáleného** lokality panelů, vyberte hello cílovou složku.</span><span class="sxs-lookup"><span data-stu-id="34eea-372">In hello **Remote** site panel, select hello destination folder.</span></span> <span data-ttu-id="34eea-373">Budete nasazovat toohello soubor WAR hello `webapps` adresář v kořenovém adresáři hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-373">You will deploy hello WAR file toohello `webapps` directory under hello web app's root.</span></span> <span data-ttu-id="34eea-374">Přejděte příliš`/site/wwwroot`, klikněte pravým tlačítkem na `wwwroot`a vyberte **vytvořit adresář**.</span><span class="sxs-lookup"><span data-stu-id="34eea-374">Navigate too`/site/wwwroot`, right-click on `wwwroot`, and select **Create directory**.</span></span> <span data-ttu-id="34eea-375">Adresář se jménem hello `webapps` a zadejte tento adresář.</span><span class="sxs-lookup"><span data-stu-id="34eea-375">Name hello directory `webapps` and enter that directory.</span></span>
6. <span data-ttu-id="34eea-376">Přenos JSPHello.war příliš`/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="34eea-376">Transfer JSPHello.war too`/site/wwwroot/webapps`.</span></span> <span data-ttu-id="34eea-377">Vyberte JSPHello.war v hello **místní** soubor seznamu, klikněte na něj pravým tlačítkem myši a vyberte **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="34eea-377">Select JSPHello.war in hello **Local** file list, right-click on it and select **Upload**.</span></span> <span data-ttu-id="34eea-378">Měli byste vidět se zobrazí v `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="34eea-378">You should see it appear in `/site/wwwroot/webapps`.</span></span>
7. <span data-ttu-id="34eea-379">Po zkopírování adresáře webapps toohello JSPHello.war bude automaticky rozbalte Tomcat Server (rozbalte) hello souborů v souboru WAR hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-379">After you copy JSPHello.war toohello webapps directory, Tomcat Server will automatically unpack (unzip) hello files in hello WAR file.</span></span> <span data-ttu-id="34eea-380">I když Tomcat Server začne rozbalování téměř okamžitě, může trvat dlouho čas (pravděpodobně hodiny) pro soubory tooappear hello v klientovi hello FTP.</span><span class="sxs-lookup"><span data-stu-id="34eea-380">Although Tomcat Server begins unpacking almost immediately, it might take a long time (possibly hours) for hello files tooappear in hello FTP client.</span></span>

#### <a name="run-hello-hello-world-application-on-hello-web-app"></a><span data-ttu-id="34eea-381">Spuštění aplikace Hello World hello na hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="34eea-381">Run hello Hello World application on hello Web App</span></span>
1. <span data-ttu-id="34eea-382">Po nahrát soubor WAR hello a ověřit, že Tomcat server vytvořil rozbalené `JSPHello` adresáře, procházet příliš`http://webdemowebapp.azurewebsites.net/JSPHello` toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="34eea-382">After you have uploaded hello WAR file and verified that Tomcat server has created an unpacked `JSPHello` directory, browse too`http://webdemowebapp.azurewebsites.net/JSPHello` toorun hello application.</span></span>
   
   > <span data-ttu-id="34eea-383">**Poznámka:** Pokud kliknete na tlačítko **Procházet** z portálu classic hello, může získat výchozí webovou stránku hello, oznámením "této webové aplikace Java na základě úspěšně vytvořila."</span><span class="sxs-lookup"><span data-stu-id="34eea-383">**Note:** If you click **Browse** from hello classic portal, you might get hello default webpage, saying "This Java based web application has been successfully created."</span></span> <span data-ttu-id="34eea-384">Můžete mít webovou stránku hello toorefresh ve výstupu aplikace hello pořadí tooview místo výchozí webovou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-384">You might have toorefresh hello webpage in order tooview hello application output instead of hello default webpage.</span></span>
   > 
   > 
2. <span data-ttu-id="34eea-385">Při spuštění aplikace hello byste měli vidět na webové stránce s hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="34eea-385">When hello application runs, you should see a web page with hello following output:</span></span>
   
    `Hello World, hello time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="clean-up-azure-resources"></a><span data-ttu-id="34eea-386">Vyčištění prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="34eea-386">Clean up Azure resources</span></span>
<span data-ttu-id="34eea-387">Tento postup vytvoří webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="34eea-387">This procedure creates an App Service web app.</span></span> <span data-ttu-id="34eea-388">Také existuje bude platit pro prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="34eea-388">You will be billed for hello resource as long as it exists.</span></span> <span data-ttu-id="34eea-389">Pokud máte v plánu toocontinue použití hello webové aplikace pro testování nebo pro vývoj, měli byste zvážit, zastavení nebo odstranění.</span><span class="sxs-lookup"><span data-stu-id="34eea-389">Unless you plan toocontinue using hello web app for testing or development, you should consider stopping or deleting it.</span></span> <span data-ttu-id="34eea-390">Webovou aplikaci, která byla zastavena stále zpoplatněná malé poplatků, ale můžete ji restartovat kdykoli.</span><span class="sxs-lookup"><span data-stu-id="34eea-390">A web app that has been stopped will still incur a small charge, but you can restart it at any time.</span></span> <span data-ttu-id="34eea-391">Odstraněním webové aplikace vymaže veškerá data, které jste odeslali tooit.</span><span class="sxs-lookup"><span data-stu-id="34eea-391">Deleting a web app erases all data you have uploaded tooit.</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

[1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
[2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
[3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
[4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
[5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
[6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
[7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
[8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
[9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
[10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png


[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web Platform Installer]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure classic portal]: https://manage.windowsazure.com
[What is an Azure AD directory]: http://technet.microsoft.com/library/jj573650.aspx
[Create and Upload a Management Certificate for Azure]: ../cloud-services/cloud-services-certs-create.md
[Key and Certificate Management Tool (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure Portal]: https://portal.azure.com
