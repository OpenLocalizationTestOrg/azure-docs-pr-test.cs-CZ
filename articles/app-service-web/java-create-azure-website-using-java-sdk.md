---
title: "Vytvoření webové aplikace v Azure App Service pomocí sady Azure SDK pro jazyk Java"
description: "Naučte se vytvořit webovou aplikaci v Azure App Service programově pomocí sady Azure SDK pro jazyk Java."
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
ms.openlocfilehash: 08bb53de8cf437a5a2b1c3b38bce9f81b8349493
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a><span data-ttu-id="70743-103">Vytvoření webové aplikace v Azure App Service pomocí sady Azure SDK pro jazyk Java</span><span class="sxs-lookup"><span data-stu-id="70743-103">Create a Web App in Azure App Service using the Azure SDK for Java</span></span>
<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a><span data-ttu-id="70743-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="70743-104">Overview</span></span>
<span data-ttu-id="70743-105">Tento postup vám ukáže, jak vytvořit sadu Azure SDK pro aplikace Java, která vytvoří webovou aplikaci v [Azure App Service][Azure App Service], pak nasazení aplikace do ní.</span><span class="sxs-lookup"><span data-stu-id="70743-105">This walkthrough shows you how to create an Azure SDK for Java application that creates a Web App in [Azure App Service][Azure App Service], then deploy an application to it.</span></span> <span data-ttu-id="70743-106">Skládá se ze dvou částí:</span><span class="sxs-lookup"><span data-stu-id="70743-106">It consists of two parts:</span></span>

* <span data-ttu-id="70743-107">Část 1 ukazuje, jak sestavit aplikaci Java, která vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70743-107">Part 1 demonstrates how to build a Java application that creates a web app.</span></span>
* <span data-ttu-id="70743-108">Část 2 ukazuje, jak vytvořit jednoduché JSP "Hello World" aplikace a pak použijte k serveru FTP klienta nasadíte kód do služby App Service.</span><span class="sxs-lookup"><span data-stu-id="70743-108">Part 2 demonstrates how to create a simple JSP "Hello World" application, then use an FTP client to deploy code to App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70743-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="70743-109">Prerequisites</span></span>
### <a name="software-installations"></a><span data-ttu-id="70743-110">Instalace softwaru</span><span class="sxs-lookup"><span data-stu-id="70743-110">Software Installations</span></span>
<span data-ttu-id="70743-111">AzureWebDemo kódu aplikace v tomto článku byla zapsána pomocí sady Azure Java SDK 0.7.0, které můžete nainstalovat pomocí [instalačního programu webové platformy] [ Web Platform Installer] (WebPI).</span><span class="sxs-lookup"><span data-stu-id="70743-111">The AzureWebDemo application code in this article was written using Azure Java SDK 0.7.0, which you can install using the [Web Platform Installer][Web Platform Installer] (WebPI).</span></span> <span data-ttu-id="70743-112">Kromě toho, nezapomeňte použít nejnovější verzi [nástrojů Azure pro Eclipse][Azure Toolkit for Eclipse].</span><span class="sxs-lookup"><span data-stu-id="70743-112">In addition, make sure to use the latest version of the [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="70743-113">Po instalaci sady SDK, aktualizace závislosti ve vašem projektu Eclipse spuštěním **aktualizovat Index** v **Maven úložiště**, nejnovější verze jednotlivých balíčků v znovu přidat **závislosti** okno.</span><span class="sxs-lookup"><span data-stu-id="70743-113">After you install the SDK, update the dependencies in your Eclipse project by running **Update Index** in **Maven Repositories**, then re-add the latest version of each package in the **Dependencies** window.</span></span> <span data-ttu-id="70743-114">Kliknutím můžete ověřit verzi nainstalovaného softwaru v prostředí Eclipse **pomoci > podrobné informace o instalaci**; byste měli mít alespoň následující verze:</span><span class="sxs-lookup"><span data-stu-id="70743-114">You can verify the version of your installed software in Eclipse by clicking **Help > Installation Details**; you should have at least the following versions:</span></span>

* <span data-ttu-id="70743-115">Balíček pro knihovny Microsoft Azure Libraries for Java 0.7.0.20150309</span><span class="sxs-lookup"><span data-stu-id="70743-115">Package for Microsoft Azure Libraries for Java 0.7.0.20150309</span></span>
* <span data-ttu-id="70743-116">Integrované vývojové prostředí pro vývojáře v jazyce Java EE 4.4.2.20150219 Eclipse</span><span class="sxs-lookup"><span data-stu-id="70743-116">Eclipse IDE for Java EE Developers 4.4.2.20150219</span></span>

### <a name="create-and-configure-cloud-resources-in-azure"></a><span data-ttu-id="70743-117">Vytvoření a konfigurace cloudové prostředky v Azure</span><span class="sxs-lookup"><span data-stu-id="70743-117">Create and Configure Cloud Resources in Azure</span></span>
<span data-ttu-id="70743-118">Před zahájením tohoto postupu, potřebujete mít aktivní předplatné Azure a nastavit výchozí Active Directory (AD) v Azure.</span><span class="sxs-lookup"><span data-stu-id="70743-118">Before you begin this procedure, you need to have an active Azure subscription and set up a default Active Directory (AD) on Azure.</span></span>

### <a name="create-an-active-directory-ad-in-azure"></a><span data-ttu-id="70743-119">Vytvořte v Azure Active Directory (AD)</span><span class="sxs-lookup"><span data-stu-id="70743-119">Create an Active Directory (AD) in Azure</span></span>
<span data-ttu-id="70743-120">Pokud jste již nemají Active Directory (AD) na vaše předplatné Azure, přihlaste se k [portál Azure classic] [ Azure classic portal] s vaším účtem Microsoft.</span><span class="sxs-lookup"><span data-stu-id="70743-120">If you do not already have an Active Directory (AD) on your Azure subscription, log into the [Azure classic portal][Azure classic portal] with your Microsoft account.</span></span> <span data-ttu-id="70743-121">Pokud máte více předplatných, klikněte na tlačítko **odběry** a vyberte výchozí adresář pro předplatné, které chcete použít pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="70743-121">If you have multiple subscriptions, click **Subscriptions** and select the default directory for the subscription you want to use for this project.</span></span> <span data-ttu-id="70743-122">Pak klikněte na tlačítko **použít** přepnout do zobrazení tohoto odběru.</span><span class="sxs-lookup"><span data-stu-id="70743-122">Then click **Apply** to switch to that subscription view.</span></span>

1. <span data-ttu-id="70743-123">Vyberte **služby Active Directory** z nabídky na levé straně.</span><span class="sxs-lookup"><span data-stu-id="70743-123">Select **Active Directory** from the menu at left.</span></span> <span data-ttu-id="70743-124">**Klikněte na tlačítko Nový > adresář > vytvořit vlastní**.</span><span class="sxs-lookup"><span data-stu-id="70743-124">**Click New > Directory > Custom Create**.</span></span>
2. <span data-ttu-id="70743-125">V **přidat adresář**, vyberte **vytvořte nový adresář**.</span><span class="sxs-lookup"><span data-stu-id="70743-125">In **Add Directory**, select **Create New Directory**.</span></span>
3. <span data-ttu-id="70743-126">V **název**, zadejte název adresáře.</span><span class="sxs-lookup"><span data-stu-id="70743-126">In **Name**, enter a directory name.</span></span>
4. <span data-ttu-id="70743-127">V **domény**, zadejte název domény.</span><span class="sxs-lookup"><span data-stu-id="70743-127">In **Domain**, enter a domain name.</span></span> <span data-ttu-id="70743-128">Toto je název základní domény, který je zahrnut ve výchozím nastavení s adresářem; má formuláře `<domain_name>.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="70743-128">This is a basic domain name that is included by default with your directory; it has the form `<domain_name>.onmicrosoft.com`.</span></span> <span data-ttu-id="70743-129">Můžete pojmenovat podle názvu adresáře nebo jiný název domény, který vlastníte.</span><span class="sxs-lookup"><span data-stu-id="70743-129">You can name it based on the directory name or another domain name that you own.</span></span> <span data-ttu-id="70743-130">Později můžete přidat jiný název domény, který vaše organizace už používá.</span><span class="sxs-lookup"><span data-stu-id="70743-130">Later, you can add another domain name that your organization already uses.</span></span>
5. <span data-ttu-id="70743-131">V **zemi nebo oblast**, vyberte národní prostředí.</span><span class="sxs-lookup"><span data-stu-id="70743-131">In **Country or region**, select your locale.</span></span>

<span data-ttu-id="70743-132">Další informace o AD, najdete v části [co je adresář Azure AD][What is an Azure AD directory]?</span><span class="sxs-lookup"><span data-stu-id="70743-132">For more information on AD, see [What is an Azure AD directory][What is an Azure AD directory]?</span></span>

### <a name="create-a-management-certificate-for-azure"></a><span data-ttu-id="70743-133">Vytvoření certifikátu správy pro Azure.</span><span class="sxs-lookup"><span data-stu-id="70743-133">Create a Management Certificate for Azure</span></span>
<span data-ttu-id="70743-134">Azure SDK pro jazyk Java používá certifikáty pro správu k ověřování pomocí předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="70743-134">The Azure SDK for Java uses management certificates to authenticate with Azure subscriptions.</span></span> <span data-ttu-id="70743-135">Toto jsou certifikáty X.509 v3 sloužící k ověření pravosti aplikace klienta, která používá Service Management API zastupovat vlastník předplatného ke správě prostředků předplatného.</span><span class="sxs-lookup"><span data-stu-id="70743-135">These are X.509 v3 certificates you use to authenticate a client application that uses the Service Management API to act on behalf of the subscription owner to manage subscription resources.</span></span>

<span data-ttu-id="70743-136">Kód v tomto postupu používá certifikát podepsaný svým držitelem k ověření pomocí Azure.</span><span class="sxs-lookup"><span data-stu-id="70743-136">The code in this procedure uses a self-signed certificate to authenticate with Azure.</span></span> <span data-ttu-id="70743-137">Tento postup, musíte vytvořit certifikát a nahrajte ho do [portál Azure classic] [ Azure classic portal] předem.</span><span class="sxs-lookup"><span data-stu-id="70743-137">For this procedure, you need to create a certificate and upload it to the [Azure classic portal][Azure classic portal] beforehand.</span></span> <span data-ttu-id="70743-138">To zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="70743-138">This involves the following steps:</span></span>

* <span data-ttu-id="70743-139">Generování souboru PFX představující klientský certifikát a uložte ho místně.</span><span class="sxs-lookup"><span data-stu-id="70743-139">Generate a PFX file representing your client certificate and save it locally.</span></span>
* <span data-ttu-id="70743-140">Vygenerujte certifikát správy (soubor CER) ze souboru PFX.</span><span class="sxs-lookup"><span data-stu-id="70743-140">Generate a management certificate (CER file) from the PFX file.</span></span>
* <span data-ttu-id="70743-141">Nahrání souboru CER do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="70743-141">Upload the CER file to your Azure subscription.</span></span>
* <span data-ttu-id="70743-142">Soubor PFX převeďte JKS, protože Java použije tento formát ověřování pomocí certifikátů.</span><span class="sxs-lookup"><span data-stu-id="70743-142">Convert the PFX file into JKS, because Java uses that format to authenticate using certificates.</span></span>
* <span data-ttu-id="70743-143">Zápis kódu ověřování aplikace, která odkazuje na místní soubor JKS.</span><span class="sxs-lookup"><span data-stu-id="70743-143">Write the application's authentication code, which refers to the local JKS file.</span></span>

<span data-ttu-id="70743-144">Po dokončení tohoto postupu CER certifikát se bude nacházet ve vašem předplatném Azure a certifikát JKS se bude nacházet na místním disku.</span><span class="sxs-lookup"><span data-stu-id="70743-144">When you complete this procedure, the CER certificate will reside in your Azure subscription and the JKS certificate will reside on your local drive.</span></span> <span data-ttu-id="70743-145">Další informace o certifikáty pro správu najdete v tématu [vytvoření a nahrání certifikátu pro správu pro Azure][Create and Upload a Management Certificate for Azure].</span><span class="sxs-lookup"><span data-stu-id="70743-145">For more information on management certificates, see [Create and Upload a Management Certificate for Azure][Create and Upload a Management Certificate for Azure].</span></span>

#### <a name="create-a-certificate"></a><span data-ttu-id="70743-146">Vytvoření certifikátu</span><span class="sxs-lookup"><span data-stu-id="70743-146">Create a certificate</span></span>
<span data-ttu-id="70743-147">Pokud chcete vytvořit vlastní certifikát podepsaný svým držitelem, otevřete konzolu příkaz v operačním systému a spusťte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="70743-147">To create your own self-signed certificate, open a command console on your operating system and run the following commands.</span></span>

> <span data-ttu-id="70743-148">**Poznámka:** počítač, na kterém je spuštěn tento příkaz musí mít JDK nainstalována.</span><span class="sxs-lookup"><span data-stu-id="70743-148">**Note:**  The computer on which you run this command must have the JDK installed.</span></span> <span data-ttu-id="70743-149">Cesta ke keytool navíc závisí na umístění, ve kterém nainstalujete sadu JDK.</span><span class="sxs-lookup"><span data-stu-id="70743-149">Also, the path to the keytool depends on the location in which you install the JDK.</span></span> <span data-ttu-id="70743-150">Další informace najdete v tématu [klíč a nástroj pro správu certifikát (keytool)] [ Key and Certificate Management Tool (keytool)] v online dokumentaci Java.</span><span class="sxs-lookup"><span data-stu-id="70743-150">For more information, see [Key and Certificate Management Tool (keytool)][Key and Certificate Management Tool (keytool)] in the Java online docs.</span></span>
> 
> 

<span data-ttu-id="70743-151">Chcete-li vytvořit soubor .pfx:</span><span class="sxs-lookup"><span data-stu-id="70743-151">To create the .pfx file:</span></span>

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

<span data-ttu-id="70743-152">Chcete-li vytvořit soubor .cer:</span><span class="sxs-lookup"><span data-stu-id="70743-152">To create the .cer file:</span></span>

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

<span data-ttu-id="70743-153">Kde:</span><span class="sxs-lookup"><span data-stu-id="70743-153">where:</span></span>

* <span data-ttu-id="70743-154">`<java-install-dir>`je cesta k adresáři, do které jste nainstalovali Java.</span><span class="sxs-lookup"><span data-stu-id="70743-154">`<java-install-dir>` is the path to the directory in which you installed Java.</span></span>
* <span data-ttu-id="70743-155">`<keystore-id>`je identifikátor záznamu úložiště klíčů (například `AzureRemoteAccess`).</span><span class="sxs-lookup"><span data-stu-id="70743-155">`<keystore-id>` is the keystore entry identifier (for example, `AzureRemoteAccess`).</span></span>
* <span data-ttu-id="70743-156">`<cert-store-dir>`je cesta k adresáři, ve kterém chcete uložit certifikáty (například `C:/Certificates`).</span><span class="sxs-lookup"><span data-stu-id="70743-156">`<cert-store-dir>` is the path to the directory in which you want to store certificates (for example `C:/Certificates`).</span></span>
* <span data-ttu-id="70743-157">`<cert-file-name>`je název souboru certifikátu (například `AzureWebDemoCert`).</span><span class="sxs-lookup"><span data-stu-id="70743-157">`<cert-file-name>` is the name of the certificate file (for example `AzureWebDemoCert`).</span></span>
* <span data-ttu-id="70743-158">`<password>`je heslo, které jste se rozhodli chránit certifikátu; musí být alespoň 6 znaků.</span><span class="sxs-lookup"><span data-stu-id="70743-158">`<password>` is the password you choose to protect the certificate; it must be at least 6 characters long.</span></span> <span data-ttu-id="70743-159">Žádné heslo, můžete zadat, i když se to nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="70743-159">You can enter no password, although this is not recommended.</span></span>
* <span data-ttu-id="70743-160">`<dname>`je X.500 rozlišující název, který se má přidružit alias a slouží jako pole Vystavitel a předmětu v certifikátu podepsaného svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="70743-160">`<dname>` is the X.500 Distinguished Name to be associated with alias, and is used as the issuer and subject fields in the self-signed certificate.</span></span>

<span data-ttu-id="70743-161">Další informace najdete v tématu [vytvoření a nahrání certifikátu pro správu pro Azure][Create and Upload a Management Certificate for Azure].</span><span class="sxs-lookup"><span data-stu-id="70743-161">For more information, see [Create and Upload a Management Certificate for Azure][Create and Upload a Management Certificate for Azure].</span></span>

#### <a name="upload-the-certificate"></a><span data-ttu-id="70743-162">Nahrát na server certifikát</span><span class="sxs-lookup"><span data-stu-id="70743-162">Upload the certificate</span></span>
<span data-ttu-id="70743-163">Nahrajte certifikát podepsaný svým držitelem do Azure, přejděte na **nastavení** na portálu classic a pak klikněte na **certifikáty pro správu** kartě. Klikněte na tlačítko **nahrát** v dolní části stránky a přejděte do umístění souboru CER jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="70743-163">To upload a self-signed certificate to Azure, go to the **Settings** page in the classic portal, then click the **Management Certificates** tab. Click **Upload** at the bottom of the page and navigate to the location of the CER file you created.</span></span>

#### <a name="convert-the-pfx-file-into-jks"></a><span data-ttu-id="70743-164">Převeďte soubor PFX do JKS</span><span class="sxs-lookup"><span data-stu-id="70743-164">Convert the PFX file into JKS</span></span>
<span data-ttu-id="70743-165">V na příkazovém řádku Windows (spuštěna jako správce), disk cd adresář obsahující certifikáty a spusťte následující příkaz, kde `<java-install-dir>` je adresář, ve kterém je nainstalovaná Java ve vašem počítači:</span><span class="sxs-lookup"><span data-stu-id="70743-165">In the Windows Command Prompt (running as admin), cd to the directory containing the certificates and run the following command, where `<java-install-dir>` is the directory in which you installed Java on your computer:</span></span>

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. <span data-ttu-id="70743-166">Po zobrazení výzvy zadejte heslo cílové úložiště klíčů; bude heslo pro soubor JKS.</span><span class="sxs-lookup"><span data-stu-id="70743-166">When prompted, enter the destination keystore password; this will be the password for the JKS file.</span></span>
2. <span data-ttu-id="70743-167">Po zobrazení výzvy zadejte heslo zdrojové úložiště klíčů; Toto je heslo, které jste zadali pro soubor PFX.</span><span class="sxs-lookup"><span data-stu-id="70743-167">When prompted, enter the source keystore password; this is the password you specified for the PFX file.</span></span>

<span data-ttu-id="70743-168">Zadaná dvě hesla se nemusíte být stejné.</span><span class="sxs-lookup"><span data-stu-id="70743-168">The two passwords do not have to be the same.</span></span> <span data-ttu-id="70743-169">Žádné heslo, můžete zadat, i když se to nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="70743-169">You can enter no password, although this is not recommended.</span></span>

## <a name="build-a-web-app-creation-application"></a><span data-ttu-id="70743-170">Vytvoření aplikace vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="70743-170">Build a Web App creation application</span></span>
### <a name="create-the-eclipse-workspace-and-maven-project"></a><span data-ttu-id="70743-171">Vytvořit pracovní prostor Eclipse a projekt Maven</span><span class="sxs-lookup"><span data-stu-id="70743-171">Create the Eclipse Workspace and Maven Project</span></span>
<span data-ttu-id="70743-172">V této části vytvoříte pracovní prostor a pro webovou aplikaci vytvoření aplikaci s názvem AzureWebDemo projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="70743-172">In this section you create a workspace and a Maven project for the web app creation application, named AzureWebDemo.</span></span>

1. <span data-ttu-id="70743-173">Vytvořte nový projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="70743-173">Create a new Maven project.</span></span> <span data-ttu-id="70743-174">Klikněte na tlačítko **soubor > Nový > Projekt Maven**.</span><span class="sxs-lookup"><span data-stu-id="70743-174">Click **File > New > Maven Project**.</span></span> <span data-ttu-id="70743-175">V **nový projekt Maven**, vyberte **vytvoření jednoduché projektu** a **používat výchozí umístění prostoru**.</span><span class="sxs-lookup"><span data-stu-id="70743-175">In **New Maven Project**, select **Create a simple project** and **Use default workspace location**.</span></span>
2. <span data-ttu-id="70743-176">Na druhé stránce **nový projekt Maven**, zadejte následující:</span><span class="sxs-lookup"><span data-stu-id="70743-176">On the second page of **New Maven Project**, specify the following:</span></span>
   
   * <span data-ttu-id="70743-177">ID skupiny:`com.<username>.azure.webdemo`</span><span class="sxs-lookup"><span data-stu-id="70743-177">Group ID: `com.<username>.azure.webdemo`</span></span>
   * <span data-ttu-id="70743-178">ID artefaktů: AzureWebDemo</span><span class="sxs-lookup"><span data-stu-id="70743-178">Artifact ID: AzureWebDemo</span></span>
   * <span data-ttu-id="70743-179">Verze: 0.0.1-SNAPSHOT</span><span class="sxs-lookup"><span data-stu-id="70743-179">Version: 0.0.1-SNAPSHOT</span></span>
   * <span data-ttu-id="70743-180">Balení: jar</span><span class="sxs-lookup"><span data-stu-id="70743-180">Packaging: jar</span></span>
   * <span data-ttu-id="70743-181">Název: AzureWebDemo</span><span class="sxs-lookup"><span data-stu-id="70743-181">Name: AzureWebDemo</span></span>
     
     <span data-ttu-id="70743-182">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="70743-182">Click **Finish**.</span></span>
3. <span data-ttu-id="70743-183">Otevřete soubor pom.xml nový projekt v prohlížeči projektu.</span><span class="sxs-lookup"><span data-stu-id="70743-183">Open the new project's pom.xml file in Project Explorer.</span></span> <span data-ttu-id="70743-184">Vyberte **závislosti** kartě. Toto je nový projekt, jsou uvedeny ještě žádné balíčky.</span><span class="sxs-lookup"><span data-stu-id="70743-184">Select the **Dependencies** tab. As this is a new project, no packages are listed yet.</span></span>
4. <span data-ttu-id="70743-185">Otevření zobrazení Maven úložiště.</span><span class="sxs-lookup"><span data-stu-id="70743-185">Open the Maven Repositories view.</span></span> <span data-ttu-id="70743-186">**Klikněte na okno > Zobrazit zobrazení > jiné > Maven > Maven úložiště** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="70743-186">**Click Window > Show View > Other > Maven > Maven Repositories** and click **OK**.</span></span> <span data-ttu-id="70743-187">**Maven úložiště** zobrazení se zobrazí v dolní části rozhraní IDE.</span><span class="sxs-lookup"><span data-stu-id="70743-187">The **Maven Repositories** view will appear at the bottom of the IDE.</span></span>
5. <span data-ttu-id="70743-188">Otevřete **globální úložiště**, klikněte pravým tlačítkem myši **centrální** úložiště a vyberte **Rebuild Index**.</span><span class="sxs-lookup"><span data-stu-id="70743-188">Open **Global Repositories**, right-click the **central** repository, and select **Rebuild Index**.</span></span>
   
    ![][1]
   
    <span data-ttu-id="70743-189">Tento krok může trvat několik minut v závislosti na rychlosti připojení.</span><span class="sxs-lookup"><span data-stu-id="70743-189">This step can take several minutes depending on the speed of your connection.</span></span> <span data-ttu-id="70743-190">Když znovu sestaví index, byste měli vidět balíčků Microsoft Azure v **centrální** Maven úložiště.</span><span class="sxs-lookup"><span data-stu-id="70743-190">When the index rebuilds, you should see the Microsoft Azure packages in the **central** Maven repository.</span></span>
6. <span data-ttu-id="70743-191">V **závislosti**, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="70743-191">In **Dependencies**, click **Add**.</span></span> <span data-ttu-id="70743-192">V **zadejte ID skupiny...**  zadejte `azure-management`.</span><span class="sxs-lookup"><span data-stu-id="70743-192">In **Enter Group ID...** enter `azure-management`.</span></span> <span data-ttu-id="70743-193">Vyberte balíčky, pro základní správu a správu App Service Web Apps:</span><span class="sxs-lookup"><span data-stu-id="70743-193">Select the packages for base management and App Service Web Apps management:</span></span>
   
        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites
   
   > <span data-ttu-id="70743-194">**Poznámka:** po novou verzi verzí při aktualizaci závislosti, budete muset znovu přidejte všechny závislosti v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="70743-194">**Note:** If you are updating the dependencies after a new version release, you need to re-add each of the dependencies in this list.</span></span>
   > <span data-ttu-id="70743-195">Po kliknutí na tlačítko **přidat** a vyberte každá závislost, zobrazí se nové číslo verze v **závislosti** seznamu.</span><span class="sxs-lookup"><span data-stu-id="70743-195">After you click **Add** and select each dependency, it appears with the new version number in the **Dependencies** list.</span></span>
   > 
   > 

<span data-ttu-id="70743-196">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="70743-196">Click **OK**.</span></span> <span data-ttu-id="70743-197">Azure balíčky se potom zobrazí v **závislosti** seznamu.</span><span class="sxs-lookup"><span data-stu-id="70743-197">The Azure packages then appear in the **Dependencies** list.</span></span>

### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a><span data-ttu-id="70743-198">Psaní kódu Java k vytvoření webové aplikace při volání sady Azure SDK</span><span class="sxs-lookup"><span data-stu-id="70743-198">Writing Java Code to Create a Web App by Calling the Azure SDK</span></span>
<span data-ttu-id="70743-199">Dále napište kód, který zavolá rozhraní API v Azure SDK pro jazyk Java k vytvoření webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="70743-199">Next, write the code that calls APIs in the Azure SDK for Java to create the App Service web app.</span></span>

1. <span data-ttu-id="70743-200">Vytvořte třídu Java, obsahuje kód hlavní vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="70743-200">Create a Java class to contain the main entry point code.</span></span> <span data-ttu-id="70743-201">V prohlížeči projektu klikněte pravým tlačítkem na uzel projektu a vyberte **nový > třída**.</span><span class="sxs-lookup"><span data-stu-id="70743-201">In Project Explorer, right-click on the project node and select **New > Class**.</span></span>
2. <span data-ttu-id="70743-202">V **nová třída Java**, název třídy `WebCreator` a zkontrolujte **veřejné statické void main** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="70743-202">In **New Java Class**, name the class `WebCreator` and check the **public static void main** checkbox.</span></span> <span data-ttu-id="70743-203">Výběr by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="70743-203">The selections should appear as follows:</span></span>
   
    ![][2]
3. <span data-ttu-id="70743-204">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="70743-204">Click **Finish**.</span></span> <span data-ttu-id="70743-205">Soubor WebCreator.java se zobrazí v prohlížeči projektu.</span><span class="sxs-lookup"><span data-stu-id="70743-205">The WebCreator.java file appears in Project Explorer.</span></span>

### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a><span data-ttu-id="70743-206">Volání rozhraní API Azure k vytvoření webové aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="70743-206">Calling the Azure API to Create an App Service Web App</span></span>
#### <a name="add-necessary-imports"></a><span data-ttu-id="70743-207">Přidejte potřebné importy</span><span class="sxs-lookup"><span data-stu-id="70743-207">Add necessary imports</span></span>
<span data-ttu-id="70743-208">V WebCreator.java přidejte následující importy; Tyto importy poskytnout přístup k třídy v knihovnách správy pro použití rozhraní API Správce Azure:</span><span class="sxs-lookup"><span data-stu-id="70743-208">In WebCreator.java, add the following imports; these imports provide access to classes in the management libraries for consuming Azure APIs:</span></span>

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


#### <a name="define-the-main-entry-point-class"></a><span data-ttu-id="70743-209">Zadejte třídu hlavní vstupní bod</span><span class="sxs-lookup"><span data-stu-id="70743-209">Define the main entry point class</span></span>
<span data-ttu-id="70743-210">Protože účelem AzureWebDemo aplikace je vytvoření webové aplikace App Service, název hlavní třídy pro tuto aplikaci `WebAppCreator`.</span><span class="sxs-lookup"><span data-stu-id="70743-210">Because the purpose of the AzureWebDemo application is to create an App Service Web App, name the main class for this application `WebAppCreator`.</span></span> <span data-ttu-id="70743-211">Tato třída poskytuje kód hlavní vstupní bod, který volá Azure Service Management API k vytvoření webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-211">This class provides the main entry point code that calls the Azure Service Management API to create the web app.</span></span>

<span data-ttu-id="70743-212">Přidejte následující definice parametr pro webovou aplikaci a webový prostor.</span><span class="sxs-lookup"><span data-stu-id="70743-212">Add the following parameter definitions for the web app and webspace.</span></span> <span data-ttu-id="70743-213">Musíte zadat své vlastní informace ID a certifikát pro předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="70743-213">You will need to provide your own Azure subscription ID and certificate information.</span></span>

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

<span data-ttu-id="70743-214">Kde:</span><span class="sxs-lookup"><span data-stu-id="70743-214">where:</span></span>

* <span data-ttu-id="70743-215">`<subscription-id>`je ID předplatného Azure, ve kterém chcete vytvořit prostředek.</span><span class="sxs-lookup"><span data-stu-id="70743-215">`<subscription-id>` is the Azure subscription ID in which you want to create the resource.</span></span>
* <span data-ttu-id="70743-216">`<certificate-store-path>`je cesta a název souboru k souboru JKS ve vašem adresáři úložiště místní certifikát.</span><span class="sxs-lookup"><span data-stu-id="70743-216">`<certificate-store-path>` is the path and filename to the JKS file in your local certificate store directory.</span></span> <span data-ttu-id="70743-217">Například `C:/Certificates/CertificateName.jks` pro Linux a `C:\Certificates\CertificateName.jks` pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="70743-217">For example, `C:/Certificates/CertificateName.jks` for Linux and `C:\Certificates\CertificateName.jks` for Windows.</span></span>
* <span data-ttu-id="70743-218">`<certificate-password>`je heslo, které jste zadali při vytváření vašeho JKS certifikátu.</span><span class="sxs-lookup"><span data-stu-id="70743-218">`<certificate-password>` is the password you specified when you created your JKS certificate.</span></span>
* <span data-ttu-id="70743-219">`webAppName`může být jakýkoli název, který zvolíte; Tento postup používá název `WebDemoWebApp`.</span><span class="sxs-lookup"><span data-stu-id="70743-219">`webAppName` can be any name you choose; this procedure uses the name `WebDemoWebApp`.</span></span> <span data-ttu-id="70743-220">Je plně názvu domény `webAppName` s `domainName` připojí, takže v tomto případě úplné doména je `webdemowebapp.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="70743-220">The full domain name is the `webAppName` with the `domainName` appended, so in this case the full domain is `webdemowebapp.azurewebsites.net`.</span></span>
* <span data-ttu-id="70743-221">`domainName`musí být zadán jako v příkladu nahoře.</span><span class="sxs-lookup"><span data-stu-id="70743-221">`domainName` should be specified as shown above.</span></span>
* <span data-ttu-id="70743-222">`webSpaceName`musí být jedna z hodnot fronty definovaných v [WebSpaceNames] [ WebSpaceNames] třídy.</span><span class="sxs-lookup"><span data-stu-id="70743-222">`webSpaceName` should be one of the values defined in the [WebSpaceNames][WebSpaceNames] class.</span></span>
* <span data-ttu-id="70743-223">`appServicePlanName`musí být zadán jako v příkladu nahoře.</span><span class="sxs-lookup"><span data-stu-id="70743-223">`appServicePlanName` should be specified as shown above.</span></span>

> <span data-ttu-id="70743-224">**Poznámka:** pokaždé, když jste tuto aplikaci spustit, budete muset změnit hodnotu `webAppName` a `appServicePlanName` (nebo odstranit webovou aplikaci na portálu Azure) před spuštěním aplikaci znovu.</span><span class="sxs-lookup"><span data-stu-id="70743-224">**Note:** Each time you run this application, you need to change the value of `webAppName` and `appServicePlanName` (or delete the web app on the Azure Portal) before running the application again.</span></span> <span data-ttu-id="70743-225">Jinak se spuštění selže, protože stejný prostředek již existuje v Azure.</span><span class="sxs-lookup"><span data-stu-id="70743-225">Otherwise, execution will fail because the same resource already exists on Azure.</span></span>
> 
> 

#### <a name="define-the-web-creation-method"></a><span data-ttu-id="70743-226">Definování webové metody vytvoření</span><span class="sxs-lookup"><span data-stu-id="70743-226">Define the web creation method</span></span>
<span data-ttu-id="70743-227">V dalším kroku definujte metodu pro vytvoření webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-227">Next, define a method to create the web app.</span></span> <span data-ttu-id="70743-228">Tato metoda `createWebApp`, určuje parametry webové aplikace a webový prostor.</span><span class="sxs-lookup"><span data-stu-id="70743-228">This method, `createWebApp`, specifies the parameters of the web app and the webspace.</span></span> <span data-ttu-id="70743-229">Také vytváří a konfiguruje App Service Web Apps správy klienta, který je definovaný [WebSiteManagementClient] [ WebSiteManagementClient] objektu.</span><span class="sxs-lookup"><span data-stu-id="70743-229">It also creates and configures the App Service Web Apps management client, which is defined by the [WebSiteManagementClient][WebSiteManagementClient] object.</span></span> <span data-ttu-id="70743-230">Klient pro správu je klíčem k vytvoření webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-230">The management client is key to creating Web Apps.</span></span> <span data-ttu-id="70743-231">Poskytuje RESTful webové služby, která umožňují aplikacím ke správě webových aplikací (provádění operací, například jako vytvoření, aktualizace a odstranění) voláním rozhraní API správy služby.</span><span class="sxs-lookup"><span data-stu-id="70743-231">It provides RESTful web services that allow applications to manage web apps (performing operations such as create, update, and delete) by calling the service management API.</span></span>

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
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
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

<span data-ttu-id="70743-232">Kód bude výstup stav protokolu HTTP odpovědi, která udává úspěch nebo selhání a pokud bylo úspěšné, bude výstup název vytvořenou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70743-232">The code will output the HTTP status of the response indicating success or failure, and if successful, will output the name of the created web app.</span></span>

#### <a name="define-the-main-method"></a><span data-ttu-id="70743-233">Zadejte metodu main()</span><span class="sxs-lookup"><span data-stu-id="70743-233">Define the main() method</span></span>
<span data-ttu-id="70743-234">Zadejte kód main() metoda, která volá createWebApp() k vytvoření webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-234">Provide the main() method code that calls createWebApp() to create the web app.</span></span>

<span data-ttu-id="70743-235">Nakonec volání `createWebApp` z `main`:</span><span class="sxs-lookup"><span data-stu-id="70743-235">Finally, call `createWebApp` from `main`:</span></span>

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a><span data-ttu-id="70743-236">Spusťte aplikaci a ověřte vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="70743-236">Run the application and verify web app creation</span></span>
<span data-ttu-id="70743-237">Chcete-li ověřit, že je aplikace spuštěna, klikněte na tlačítko **spustit > Spustit**.</span><span class="sxs-lookup"><span data-stu-id="70743-237">To verify that your application runs, click **Run > Run**.</span></span> <span data-ttu-id="70743-238">Po dokončení spuštění aplikace byste měli vidět následující výstup v prostředí Eclipse konzole:</span><span class="sxs-lookup"><span data-stu-id="70743-238">When the application completes running, you should see the following output in the Eclipse console:</span></span>

    ----------
    Web app created - HTTP response 200

    ----------

    Name of web app created: WebDemoWebApp

    ----------

<span data-ttu-id="70743-239">Přihlaste se k portálu Azure classic a klikněte na tlačítko **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="70743-239">Log into the Azure classic portal and click **Web Apps**.</span></span> <span data-ttu-id="70743-240">Nové webové aplikace mají objevit v seznamu webové aplikace během několika minut.</span><span class="sxs-lookup"><span data-stu-id="70743-240">The new web app should appear in the Web Apps list within a few minutes.</span></span>

## <a name="deploying-an-application-to-the-web-app"></a><span data-ttu-id="70743-241">Nasazení aplikace do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="70743-241">Deploying an Application to the Web App</span></span>
<span data-ttu-id="70743-242">Poté, co jste spustili AzureWebDemo a vytvořit novou webovou aplikaci, přihlaste se k portálu classic klikněte na **webové aplikace**a vyberte **WebDemoWebApp** v **webové aplikace** seznamu.</span><span class="sxs-lookup"><span data-stu-id="70743-242">After you have run AzureWebDemo and created the new web app, log into the classic portal, click **Web Apps**, and select **WebDemoWebApp** in the **Web Apps** list.</span></span> <span data-ttu-id="70743-243">Na stránce řídicího panelu webové aplikace, klikněte na tlačítko **Procházet** (nebo klikněte na adresu URL, `webdemowebapp.azurewebsites.net`) a přejděte k němu.</span><span class="sxs-lookup"><span data-stu-id="70743-243">In the web app's dashboard page, click **Browse** (or click the URL, `webdemowebapp.azurewebsites.net`) to navigate to it.</span></span> <span data-ttu-id="70743-244">Uvidíte zástupný symbol prázdné stránky, protože žádný obsah má zatím nebyly publikovány do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-244">You will see a blank placeholder page, because no content has been published to the web app yet.</span></span>

<span data-ttu-id="70743-245">Budou vedle vytvořit aplikaci "Hello World" a nasazení do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-245">Next you will create a "Hello World" application and deploy it to the web app.</span></span>

### <a name="create-a-jsp-hello-world-application"></a><span data-ttu-id="70743-246">Vytvoření aplikace JSP Hello World</span><span class="sxs-lookup"><span data-stu-id="70743-246">Create a JSP Hello World application</span></span>
#### <a name="create-the-application"></a><span data-ttu-id="70743-247">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="70743-247">Create the application</span></span>
<span data-ttu-id="70743-248">Chcete-li ukazují, jak nasadit aplikaci na webu, následující postup ukazuje, jak vytvořit jednoduchou aplikaci Java "Hello World" a nahrajte ho do webové aplikace App Service, vytvořené aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-248">In order to demonstrate how to deploy an application to the web, the following procedure shows you how to create a simple "Hello World" Java application and upload it to the App Service Web App that your application created.</span></span>

1. <span data-ttu-id="70743-249">Klikněte na tlačítko **soubor > Nový > dynamického webového projektu**.</span><span class="sxs-lookup"><span data-stu-id="70743-249">Click **File > New > Dynamic Web Project**.</span></span> <span data-ttu-id="70743-250">Pojmenujte ji `JSPHello`.</span><span class="sxs-lookup"><span data-stu-id="70743-250">Name it `JSPHello`.</span></span> <span data-ttu-id="70743-251">Nepotřebujete žádné další nastavení v tomto dialogu můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="70743-251">You do not need to change any other settings in this dialog.</span></span> <span data-ttu-id="70743-252">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="70743-252">Click **Finish**.</span></span>
   
    ![][3]
2. <span data-ttu-id="70743-253">V prohlížeči projektu, rozbalte **JSPHello** projektu, klikněte pravým tlačítkem na **WebContent**, pak klikněte na tlačítko **nový > soubor JSP**.</span><span class="sxs-lookup"><span data-stu-id="70743-253">In Project Explorer, expand the **JSPHello** project, right-click **WebContent**, then click **New > JSP File**.</span></span> <span data-ttu-id="70743-254">V dialogovém okně Nový soubor JSP název nového souboru `index.jsp`.</span><span class="sxs-lookup"><span data-stu-id="70743-254">In the New JSP File dialog, name the new file `index.jsp`.</span></span> <span data-ttu-id="70743-255">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="70743-255">Click **Next**.</span></span>
3. <span data-ttu-id="70743-256">V **vybrat šablonu JSP** vyberte **nový soubor JSP (html)** a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="70743-256">In the **Select JSP Template** dialog, select **New JSP File (html)** and click **Finish**.</span></span>
4. <span data-ttu-id="70743-257">V index.jsp, přidejte následující kód do `<head>` a `<body>` značky částech:</span><span class="sxs-lookup"><span data-stu-id="70743-257">In index.jsp, add the following code in the `<head>` and `<body>` tag sections:</span></span>
   
        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
   
        <body>
          Hello, the time is <%= date %> 
        </body>

#### <a name="run-the-hello-world-application-in-localhost"></a><span data-ttu-id="70743-258">Spuštění aplikace Hello World v localhost</span><span class="sxs-lookup"><span data-stu-id="70743-258">Run the Hello World application in localhost</span></span>
<span data-ttu-id="70743-259">Před spuštěním této aplikace, budete muset nakonfigurovat několik vlastností.</span><span class="sxs-lookup"><span data-stu-id="70743-259">Before you run this application, you need to configure a few properties.</span></span>

1. <span data-ttu-id="70743-260">Klikněte pravým tlačítkem myši **JSPHello** projektu a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="70743-260">Right-click the **JSPHello** project and select **Properties**.</span></span>
2. <span data-ttu-id="70743-261">V **vlastnosti** dialogové okno: vyberte **cesta sestavení Java**, vyberte **pořadí a Export** zkontrolujte **prostředí JRE systémová knihovna**, pak klikněte na tlačítko **až** ho přesunout na začátek seznamu.</span><span class="sxs-lookup"><span data-stu-id="70743-261">In the **Properties** dialog: select **Java Build Path**, select the **Order and Export** tab, check **JRE System Library**, then click **Up** to move it to the top of the list.</span></span>
   
    ![][4]
3. <span data-ttu-id="70743-262">Také v **vlastnosti** dialogu: vyberte **Targeted Runtimes** a klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="70743-262">Also in the **Properties** dialog: select **Targeted Runtimes** and click **New**.</span></span>
4. <span data-ttu-id="70743-263">V **nové běhové prostředí serveru** dialogovém okně, vyberte server, jako **Apache Tomcat v7.0** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="70743-263">In the **New Server Runtime Environment** dialog, select a server such as **Apache Tomcat v7.0** and click **Next**.</span></span> <span data-ttu-id="70743-264">V **Tomcat Server** dialogové okno, sada **název** k `Apache Tomcat v7.0`a nastavte **instalační adresář Tomcat** k adresáři, do které jste nainstalovali verzi serveru Tomcat, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="70743-264">In the **Tomcat Server** dialog, set **Name** to `Apache Tomcat v7.0`, and set **Tomcat Installation Directory** to the directory in which you installed the version of Tomcat server you want to use.</span></span>
   
    ![][5]
   
    <span data-ttu-id="70743-265">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="70743-265">Click **Finish**.</span></span>
5. <span data-ttu-id="70743-266">Budete pak se vraťte do **Targeted Runtimes** stránky **vlastnosti** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="70743-266">You then return to the **Targeted Runtimes** page of the **Properties** dialog.</span></span> <span data-ttu-id="70743-267">Vyberte **Apache Tomcat v7.0**, pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="70743-267">Select **Apache Tomcat v7.0**, then click **OK**.</span></span>
   
    ![][6]
6. <span data-ttu-id="70743-268">V prostředí Eclipse **spustit** nabídky, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="70743-268">In the Eclipse **Run** menu, click **Run**.</span></span> <span data-ttu-id="70743-269">V **spustit jako** dialogovém okně, vyberte **spustit na serveru**.</span><span class="sxs-lookup"><span data-stu-id="70743-269">In the **Run As** dialog, select **Run on Server**.</span></span> <span data-ttu-id="70743-270">V **spustit na serveru** dialogovém okně, vyberte **Tomcat v7.0 Server**:</span><span class="sxs-lookup"><span data-stu-id="70743-270">In the **Run on Server** dialog, select **Tomcat v7.0 Server**:</span></span>
   
    ![][7]
   
    <span data-ttu-id="70743-271">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="70743-271">Click **Finish**.</span></span>
7. <span data-ttu-id="70743-272">Když je aplikace spuštěná, měli byste vidět **JSPHello** stránky se zobrazí v okně localhost v prostředí Eclipse (`http://localhost:8080/JSPHello/`), zobrazí následující zprávu:</span><span class="sxs-lookup"><span data-stu-id="70743-272">When the application runs, you should see the **JSPHello** page appear in a localhost window in Eclipse (`http://localhost:8080/JSPHello/`), displaying the following message:</span></span>
   
    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="export-the-application-as-a-war"></a><span data-ttu-id="70743-273">Export aplikace jako soubor WAR</span><span class="sxs-lookup"><span data-stu-id="70743-273">Export the application as a WAR</span></span>
<span data-ttu-id="70743-274">Exportujte souborů projektu webové jako webový archiv (WAR) soubor tak, aby ji můžete nasadit do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-274">Export the web project files as a web archive (WAR) file so that you can deploy it to the web app.</span></span> <span data-ttu-id="70743-275">Následující souborů projektu webové jsou umístěny ve složce WebContent:</span><span class="sxs-lookup"><span data-stu-id="70743-275">The following web project files reside in the WebContent folder:</span></span>

    META-INF
    WEB-INF
    index.jsp

1. <span data-ttu-id="70743-276">Klikněte pravým tlačítkem na složku WebContent a vyberte **exportovat**.</span><span class="sxs-lookup"><span data-stu-id="70743-276">Right-click the WebContent folder and select **Export**.</span></span>
2. <span data-ttu-id="70743-277">V **Exportovat vyberte** dialogové okno, klikněte na tlačítko **webové > WAR** souboru a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="70743-277">In the **Export Select** dialog, click **Web > WAR** file, then click **Next**.</span></span>
3. <span data-ttu-id="70743-278">V **WAR Export** dialogovém okně, vyberte adresář, src v aktuálním projektu a obsahovat název souboru WAR na konci.</span><span class="sxs-lookup"><span data-stu-id="70743-278">In the **WAR Export** dialog, select the src directory in the current project, and include the name of the WAR file at the end.</span></span> <span data-ttu-id="70743-279">Například:</span><span class="sxs-lookup"><span data-stu-id="70743-279">For example:</span></span>
   
    `<project-path>/JSPHello/src/JSPHello.war`

<span data-ttu-id="70743-280">Další informace o nasazení WAR soubory, najdete v části [přidat aplikace v jazyce Java do Azure App Service Web Apps](web-sites-java-add-app.md).</span><span class="sxs-lookup"><span data-stu-id="70743-280">For more information on deploying WAR files, see [Add a Java application to Azure App Service Web Apps](web-sites-java-add-app.md).</span></span>

### <a name="deploying-the-hello-world-application-using-ftp"></a><span data-ttu-id="70743-281">Nasazení aplikace Hello World pomocí protokolu FTP</span><span class="sxs-lookup"><span data-stu-id="70743-281">Deploying the Hello World Application Using FTP</span></span>
<span data-ttu-id="70743-282">Vyberte klienta FTP třetích stran k publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-282">Select a third-party FTP client to publish the application.</span></span> <span data-ttu-id="70743-283">Tento postup popisuje dvě možnosti: konzole Kudu integrovaný do Azure; a FileZilla oblíbených nástroj s praktické, grafické uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="70743-283">This procedure describes two options: the Kudu console built into Azure; and FileZilla, a popular tool with a convenient, graphical UI.</span></span>

> <span data-ttu-id="70743-284">**Poznámka:** sady nástrojů Azure pro prostředí Eclipse podporuje nasazení na účty úložiště a cloudové služby, ale aktuálně nepodporuje nasazení do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-284">**Note:** The Azure Toolkit for Eclipse supports deployment to storage accounts and cloud services, but does not currently support deployment to web apps.</span></span> <span data-ttu-id="70743-285">Můžete nasadit na účty úložiště a cloudových služeb pomocí projektu nasazení aplikace Azure, jak je popsáno v [vytvoření aplikace Hello World služby Azure v prostředí Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), ale ne na webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-285">You can deploy to storage accounts and cloud services using an Azure Deployment Project as described in [Creating a Hello World Application for Azure in Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), but not to web apps.</span></span> <span data-ttu-id="70743-286">Používejte jiné metody, jako je například protokol FTP nebo GitHub pro přenos souborů do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-286">Use other methods such as FTP or GitHub to transfer files to your web app.</span></span>
> 
> <span data-ttu-id="70743-287">**Poznámka:** nedoporučujeme pomocí protokolu FTP z příkazového řádku Windows (nástroj příkazového řádku FTP.EXE dodávané se systémem Windows).</span><span class="sxs-lookup"><span data-stu-id="70743-287">**Note:** We do not recommend using FTP from the Windows command prompt (the command-line FTP.EXE utility that ships with Windows).</span></span> <span data-ttu-id="70743-288">Klienti FTP, které používají active FTP, jako je například FTP.EXE, často přestat fungovat přes brány firewall.</span><span class="sxs-lookup"><span data-stu-id="70743-288">FTP clients that use active FTP, such as FTP.EXE, often fail to work over firewalls.</span></span> <span data-ttu-id="70743-289">Aktivní FTP určuje interní adresa založený na síti LAN, do které FTP server se pravděpodobně nepodaří připojit.</span><span class="sxs-lookup"><span data-stu-id="70743-289">Active FTP specifies an internal LAN-based address, to which an FTP server will likely fail to connect.</span></span>
> 
> 

<span data-ttu-id="70743-290">Další informace o nasazení do webové aplikace služby App Service pomocí protokolu FTP najdete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="70743-290">For more information on deployment to an App Service web app using FTP, see the following topics:</span></span>

* [<span data-ttu-id="70743-291">Nasazení pomocí nástroje FTP</span><span class="sxs-lookup"><span data-stu-id="70743-291">Deploy using an FTP utility</span></span>](web-sites-deploy.md)

#### <a name="set-up-deployment-credentials"></a><span data-ttu-id="70743-292">Nastavit přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="70743-292">Set up deployment credentials</span></span>
<span data-ttu-id="70743-293">Zajistěte, aby jste spustili **AzureWebDemo** aplikace k vytvoření webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-293">Make sure you have run the **AzureWebDemo** application to create a web app.</span></span> <span data-ttu-id="70743-294">Bude přenos souborů do tohoto umístění.</span><span class="sxs-lookup"><span data-stu-id="70743-294">You will transfer files to this location.</span></span>

1. <span data-ttu-id="70743-295">Přihlaste se k portálu classic a klikněte na tlačítko **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="70743-295">Log into the classic portal and click **Web Apps**.</span></span> <span data-ttu-id="70743-296">Zajistěte, aby **WebDemoWebApp** se zobrazí v seznamu webové aplikace a ujistěte se, zda je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="70743-296">Make sure **WebDemoWebApp** appears in the list of web apps, and make sure that it is running.</span></span> <span data-ttu-id="70743-297">Klikněte na tlačítko **WebDemoWebApp** otevřete jeho **řídicí panel** stránky.</span><span class="sxs-lookup"><span data-stu-id="70743-297">Click **WebDemoWebApp** to open its **Dashboard** page.</span></span>
2. <span data-ttu-id="70743-298">Na **řídicí panel** v části **rychlý přehled**, klikněte na tlačítko **nastavit přihlašovací údaje nasazení** (Pokud již máte přihlašovací údaje nasazení, to čte **resetovat přihlašovací údaje nasazení**).</span><span class="sxs-lookup"><span data-stu-id="70743-298">On the **Dashboard** page, under **Quick Glance**, click **Set up your deployment credentials** (if you already have deployment credentials, this reads **Reset your deployment credentials**).</span></span>
   
    <span data-ttu-id="70743-299">Přihlašovací údaje nasazení jsou přidruženy k účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="70743-299">Deployment credentials are associated with a Microsoft account.</span></span> <span data-ttu-id="70743-300">Je třeba zadat uživatelské jméno a heslo, které můžete nasadit pomocí Git a FTP.</span><span class="sxs-lookup"><span data-stu-id="70743-300">You need to specify a username and password that you can use to deploy using Git and FTP.</span></span> <span data-ttu-id="70743-301">Tyto přihlašovací údaje můžete použít k nasazení na jakoukoli webovou aplikaci ve všech předplatných Azure spojené s vaším účtem Microsoft.</span><span class="sxs-lookup"><span data-stu-id="70743-301">You can use these credentials to deploy to any web app in all Azure subscriptions associated with your Microsoft account.</span></span> <span data-ttu-id="70743-302">Zadejte Git a FTP přihlašovací údaje nasazení v dialogovém okně a zaznamenejte uživatelské jméno a heslo pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="70743-302">Provide Git and FTP deployment credentials in the dialog, and record the username and password for future use.</span></span>

#### <a name="get-ftp-connection-information"></a><span data-ttu-id="70743-303">Získat informace o připojení FTP</span><span class="sxs-lookup"><span data-stu-id="70743-303">Get FTP connection information</span></span>
<span data-ttu-id="70743-304">K nasazení souborů aplikace do nově vytvořené webové aplikace pomocí protokolu FTP, potřebujete získat informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="70743-304">To use FTP to deploy application files to the newly created web app, you need to obtain connection information.</span></span> <span data-ttu-id="70743-305">Existují dva způsoby, jak získat informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="70743-305">There are two ways to obtain connection information.</span></span> <span data-ttu-id="70743-306">Jedním ze způsobů je navštívit webovou aplikaci **řídicí panel** stránka; druhý způsob je ke stažení webové aplikace profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="70743-306">One way is to visit the web app's **Dashboard** page; the other way is to download the web app's publish profile.</span></span> <span data-ttu-id="70743-307">Profil publikování je soubor XML, který poskytuje informace, jako je název a přihlašovací údaje hostitele FTP pro vaše webové aplikace v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="70743-307">The publish profile is an XML file that provides information such as FTP host name and logon credentials for your web apps in Azure App Service.</span></span> <span data-ttu-id="70743-308">Toto uživatelské jméno a heslo slouží k nasazení na jakoukoli webovou aplikaci v Všechna předplatná spojená s účtem Azure, ne jenom následujícímu.</span><span class="sxs-lookup"><span data-stu-id="70743-308">You can use this username and password to deploy to any web app in all subscriptions associated with the Azure account, not only this one.</span></span>

<span data-ttu-id="70743-309">Získat informace o připojení FTP v okně webové aplikace v [portálu Azure][Azure Portal]:</span><span class="sxs-lookup"><span data-stu-id="70743-309">To obtain FTP connection information from the web app's blade in the [Azure Portal][Azure Portal]:</span></span>

1. <span data-ttu-id="70743-310">V části **Essentials**, najít a zkopírovat **název hostitele FTP**.</span><span class="sxs-lookup"><span data-stu-id="70743-310">Under **Essentials**, find and copy the **FTP hostname**.</span></span> <span data-ttu-id="70743-311">Toto je identifikátor URI podobná `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="70743-311">This is a URI similar to `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.</span></span>
2. <span data-ttu-id="70743-312">V části **Essentials**, najít a zkopírovat **uživatelské jméno FTP/nasazení**.</span><span class="sxs-lookup"><span data-stu-id="70743-312">Under **Essentials**, find and copy **FTP/Deployment username**.</span></span> <span data-ttu-id="70743-313">To bude mít formát *webappname\deployment-username*; například `WebDemoWebApp\deployer77`.</span><span class="sxs-lookup"><span data-stu-id="70743-313">This will have the form *webappname\deployment-username*; for example `WebDemoWebApp\deployer77`.</span></span>

<span data-ttu-id="70743-314">Získat informace o připojení FTP z profilu publikování:</span><span class="sxs-lookup"><span data-stu-id="70743-314">To obtain FTP connection information from the publish profile:</span></span>

1. <span data-ttu-id="70743-315">V okně webové aplikace, klikněte na tlačítko **profilu publikování Get**.</span><span class="sxs-lookup"><span data-stu-id="70743-315">In the web app's blade, click **Get publish profile**.</span></span> <span data-ttu-id="70743-316">To se stáhnout soubor .publishsettings na místní jednotku.</span><span class="sxs-lookup"><span data-stu-id="70743-316">This will download a .publishsettings file to your local drive.</span></span>
2. <span data-ttu-id="70743-317">Otevřete soubor .publishsettings v editoru XML nebo textovém editoru a najděte `<publishProfile>` element obsahující `publishMethod="FTP"`.</span><span class="sxs-lookup"><span data-stu-id="70743-317">Open the .publishsettings file in an XML editor or text editor and find the `<publishProfile>` element containing `publishMethod="FTP"`.</span></span> <span data-ttu-id="70743-318">By měl vypadat jako následující:</span><span class="sxs-lookup"><span data-stu-id="70743-318">It should look like the following:</span></span>
   
        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>
3. <span data-ttu-id="70743-319">Všimněte si, že webová aplikace `publishProfile` nastavení mapu, aby správce webu FileZilla nastavení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="70743-319">Note that the web app's `publishProfile` settings map to the FileZilla Site Manager settings as follows:</span></span>

* <span data-ttu-id="70743-320">`publishUrl`je stejný jako **název hostitele FTP**, hodnotu nastavenou v **hostitele**.</span><span class="sxs-lookup"><span data-stu-id="70743-320">`publishUrl` is the same as **FTP host name**, the value you set in **Host**.</span></span>
* <span data-ttu-id="70743-321">`publishMethod="FTP"`znamená, že nastavíte **protokol** k **FTP - File Transfer Protocol**, a **šifrování** k **pomocí protokolu FTP prostý**.</span><span class="sxs-lookup"><span data-stu-id="70743-321">`publishMethod="FTP"` means that you set **Protocol** to **FTP - File Transfer Protocol**, and **Encryption** to **Use plain FTP**.</span></span>
* <span data-ttu-id="70743-322">`userName`a `userPWD` jsou klíče pro skutečné uživatelské jméno a heslo hodnoty, které jste zadali, když resetujete přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="70743-322">`userName` and `userPWD` are keys for the actual username and password values you specified when you reset the deployment credentials.</span></span> <span data-ttu-id="70743-323">`userName`je stejný jako **nasazení / FTP uživatele**.</span><span class="sxs-lookup"><span data-stu-id="70743-323">`userName` is the same as **Deployment / FTP user**.</span></span> <span data-ttu-id="70743-324">Mapují na **uživatele** a **heslo** v FileZilla.</span><span class="sxs-lookup"><span data-stu-id="70743-324">They map to **User** and **Password** in FileZilla.</span></span>
* <span data-ttu-id="70743-325">`ftpPassiveMode="True"`znamená, že tento server FTP použije pasivní přenos FTP; Vyberte **pasivní** na **nastavení přenosu** kartě.</span><span class="sxs-lookup"><span data-stu-id="70743-325">`ftpPassiveMode="True"` means that the FTP site uses passive FTP transfer; select **Passive** on the **Transfer Settings** tab.</span></span>

#### <a name="configure-the-web-app-to-host-a-java-application"></a><span data-ttu-id="70743-326">Konfigurovat webovou aplikaci k hostování aplikace Java</span><span class="sxs-lookup"><span data-stu-id="70743-326">Configure the Web App to host a Java application</span></span>
<span data-ttu-id="70743-327">Před publikováním aplikace, budete muset změnit některá nastavení konfigurace tak, aby webové aplikace může hostovat aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="70743-327">Before you publish the application, you need to change a few configuration settings so that the web app can host a Java application.</span></span>

1. <span data-ttu-id="70743-328">Na portálu classic přejděte na webovou aplikaci **řídicí panel** a klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="70743-328">In the classic portal, go to the web app's **Dashboard** page and click **Configure**.</span></span> <span data-ttu-id="70743-329">Na **konfigurace** stránky, zadejte následující nastavení.</span><span class="sxs-lookup"><span data-stu-id="70743-329">On the **Configure** page, specify the following settings.</span></span>
2. <span data-ttu-id="70743-330">V **verzi Javy** výchozí hodnota je **vypnout**; vyberte verzi jazyka Java cíle vaší aplikace; například 1.7.0_51.</span><span class="sxs-lookup"><span data-stu-id="70743-330">In **Java version** the default is **Off**; select the Java version your application targets; for example 1.7.0_51.</span></span> <span data-ttu-id="70743-331">Až to uděláte, také zkontrolujte, zda **webový kontejner** je nastaven na verzi serveru Tomcat.</span><span class="sxs-lookup"><span data-stu-id="70743-331">After you do this, also make sure that **Web container** is set to a version of Tomcat Server.</span></span>
3. <span data-ttu-id="70743-332">V **výchozí dokumenty**index.jsp a přidejte ho přesunout na začátek seznamu.</span><span class="sxs-lookup"><span data-stu-id="70743-332">In **Default Documents**, add index.jsp and move it up to the top of the list.</span></span> <span data-ttu-id="70743-333">(Výchozí soubor pro webové aplikace je hostingstart.html.)</span><span class="sxs-lookup"><span data-stu-id="70743-333">(The default file for web apps is hostingstart.html.)</span></span>
4. <span data-ttu-id="70743-334">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="70743-334">Click **Save**.</span></span>

#### <a name="publish-your-application-using-kudu"></a><span data-ttu-id="70743-335">Publikování aplikace pomocí modulu Kudu</span><span class="sxs-lookup"><span data-stu-id="70743-335">Publish your application using Kudu</span></span>
<span data-ttu-id="70743-336">Jeden způsob, jak publikovat aplikaci je pomocí konzoly pro ladění Kudu integrovaný do Azure.</span><span class="sxs-lookup"><span data-stu-id="70743-336">One way to publish the application is to use the Kudu debug console built into Azure.</span></span> <span data-ttu-id="70743-337">Kudu se označuje jako stabilní a v souladu s App Service Web Apps a Tomcat Server.</span><span class="sxs-lookup"><span data-stu-id="70743-337">Kudu is known to be stable and consistent with App Service Web Apps and Tomcat Server.</span></span> <span data-ttu-id="70743-338">Můžete přístup do konzoly pro webovou aplikaci tak, že přejde na adresu URL v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="70743-338">You access the console for the web app by browsing to a URL of the following form:</span></span>

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. <span data-ttu-id="70743-339">Pro tento postup je konzola Kudu umístěna na následující adrese URL; Přejděte do tohoto umístění:</span><span class="sxs-lookup"><span data-stu-id="70743-339">For this procedure, the Kudu console is located at the following URL; browse to this location:</span></span>
   
    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`
2. <span data-ttu-id="70743-340">V hlavní nabídce vyberte **ladění konzoly > CMD**.</span><span class="sxs-lookup"><span data-stu-id="70743-340">From the top menu, select **Debug Console > CMD**.</span></span>
3. <span data-ttu-id="70743-341">V konzole příkazového řádku, přejděte na `/site/wwwroot` (nebo klikněte na tlačítko `site`, pak `wwwroot` v zobrazení adresáře v horní části stránky):</span><span class="sxs-lookup"><span data-stu-id="70743-341">In the console command line, navigate to `/site/wwwroot` (or click `site`, then `wwwroot` in the directory view at the top of the page):</span></span>
   
    `cd /site/wwwroot`
4. <span data-ttu-id="70743-342">Po zadání **verzi Javy**, Tomcat server měli vytvořit adresáře webapps.</span><span class="sxs-lookup"><span data-stu-id="70743-342">After you specify **Java version**, Tomcat server should create a webapps directory.</span></span> <span data-ttu-id="70743-343">V příkazovém řádku konzoly přejděte do adresáře webapps:</span><span class="sxs-lookup"><span data-stu-id="70743-343">In the console command line, navigate to the webapps directory:</span></span>
   
    `mkdir webapps`
   
    `cd webapps`
5. <span data-ttu-id="70743-344">Přetáhněte JSPHello.war z `<project-path>/JSPHello/src/` a umístěte jej do zobrazení adresáře Kudu pod `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="70743-344">Drag JSPHello.war from `<project-path>/JSPHello/src/` and drop it into the Kudu directory view under `/site/wwwroot/webapps`.</span></span> <span data-ttu-id="70743-345">Není přetáhněte jej do oblasti "Přetáhněte sem odeslání a zip", protože Tomcat bude rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="70743-345">Do not drag it to the "Drag here to upload and zip" area, because Tomcat will unzip it.</span></span>
   
   ![][8]

<span data-ttu-id="70743-346">V první JSPHello.war se zobrazí v oblasti directory samostatně:</span><span class="sxs-lookup"><span data-stu-id="70743-346">At first JSPHello.war appears in the directory area by itself:</span></span>

  ![][9]

<span data-ttu-id="70743-347">V krátkém čase (pravděpodobně méně než 5 minut) bude Tomcat Server rozbalte soubor WAR do adresář rozbalené JSPHello.</span><span class="sxs-lookup"><span data-stu-id="70743-347">In a short time (probably less than 5 minutes) Tomcat Server will unzip the WAR file into an unpacked JSPHello directory.</span></span> <span data-ttu-id="70743-348">Klikněte na kořenový adresář zda index.jsp má byla rozbalené a zkopírovat existuje.</span><span class="sxs-lookup"><span data-stu-id="70743-348">Click the ROOT directory to see whether index.jsp has been unzipped and copied there.</span></span> <span data-ttu-id="70743-349">Pokud ano, přejděte zpět do adresáře webapps zobrazíte, zda byl vytvořen rozbalené JSPHello adresáře.</span><span class="sxs-lookup"><span data-stu-id="70743-349">If so, navigate back to the webapps directory to see whether the unpacked JSPHello directory has been created.</span></span> <span data-ttu-id="70743-350">Pokud se tyto položky nezobrazí, počkejte a opakujte.</span><span class="sxs-lookup"><span data-stu-id="70743-350">If you do not see these items, wait and repeat.</span></span>

  ![][10]

#### <a name="publish-your-application-using-filezilla-optional"></a><span data-ttu-id="70743-351">Publikování aplikace pomocí FileZilla (volitelné)</span><span class="sxs-lookup"><span data-stu-id="70743-351">Publish your application using FileZilla (optional)</span></span>
<span data-ttu-id="70743-352">Jiný nástroj, který slouží k publikování aplikace je FileZilla oblíbených klienta FTP třetích stran s praktické, grafické uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="70743-352">Another tool you can use to publish the application is FileZilla, a popular third-party FTP client with a convenient, graphical UI.</span></span> <span data-ttu-id="70743-353">Můžete stáhnout a nainstalovat FileZilla z [http://filezilla-project.org/](http://filezilla-project.org/) Pokud již jste ji.</span><span class="sxs-lookup"><span data-stu-id="70743-353">You can download and install FileZilla from [http://filezilla-project.org/](http://filezilla-project.org/) if you do not already have it.</span></span> <span data-ttu-id="70743-354">Další informace o používání klienta najdete v tématu [FileZilla dokumentace](https://wiki.filezilla-project.org/Documentation) a tuto položku blogu na [klienti FTP – část 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).</span><span class="sxs-lookup"><span data-stu-id="70743-354">For more information on using the client, see the [FileZilla documentation](https://wiki.filezilla-project.org/Documentation) and this blog entry on [FTP Clients - Part 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).</span></span>

1. <span data-ttu-id="70743-355">V FileZilla, klikněte na **soubor > Správce webu**.</span><span class="sxs-lookup"><span data-stu-id="70743-355">In FileZilla, click **File > Site Manager**.</span></span>
2. <span data-ttu-id="70743-356">V **správce webu** dialogové okno, klikněte na tlačítko **nové lokality**.</span><span class="sxs-lookup"><span data-stu-id="70743-356">In the **Site Manager** dialog, click **New Site**.</span></span> <span data-ttu-id="70743-357">Nový prázdný web FTP se zobrazí v **vyberte položku** zobrazení výzvy k zadání názvu.</span><span class="sxs-lookup"><span data-stu-id="70743-357">A new blank FTP site will appear in **Select Entry** prompting you to provide a name.</span></span> <span data-ttu-id="70743-358">Tento postup, pojmenujte ji `AzureWebDemo-FTP`.</span><span class="sxs-lookup"><span data-stu-id="70743-358">For this procedure, name it `AzureWebDemo-FTP`.</span></span>
   
    <span data-ttu-id="70743-359">Na **Obecné** určete následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="70743-359">On the **General** tab, specify the following settings:</span></span>
   
   * <span data-ttu-id="70743-360">**Hostitele:** Enter **název hostitele FTP** který jste zkopírovali z řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="70743-360">**Host:** Enter the **FTP Host Name** that you copied from the dashboard.</span></span>
   * <span data-ttu-id="70743-361">**Port:** (necháte prázdné, jak toto je pasivní přenos a server určí port, který se použít.)</span><span class="sxs-lookup"><span data-stu-id="70743-361">**Port:** (Leave this blank, as this is a passive transfer and the server will determine the port to use.)</span></span>
   * <span data-ttu-id="70743-362">**Protokol:** protokol pro přenos souborů FTP</span><span class="sxs-lookup"><span data-stu-id="70743-362">**Protocol:** FTP File Transfer Protocol</span></span>
   * <span data-ttu-id="70743-363">**Šifrování:** použít prostý FTP</span><span class="sxs-lookup"><span data-stu-id="70743-363">**Encryption:** Use plain FTP</span></span>
   * <span data-ttu-id="70743-364">**Typ přihlášení:** normální</span><span class="sxs-lookup"><span data-stu-id="70743-364">**Logon Type:** Normal</span></span>
   * <span data-ttu-id="70743-365">**Uživatel:** zadejte nasazení / FTP uživatele, který jste zkopírovali z řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="70743-365">**User:** Enter the Deployment / FTP user that you copied from the dashboard.</span></span> <span data-ttu-id="70743-366">Toto je úplná username FTP, který má formulář *webappname\username*.</span><span class="sxs-lookup"><span data-stu-id="70743-366">This is the full FTP username, which has the form *webappname\username*.</span></span>
   * <span data-ttu-id="70743-367">**Heslo:** zadejte heslo, kterou jste zadali, když nastavíte přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="70743-367">**Password:** Enter the password that you specified when you set the deployment credentials.</span></span>
     
     <span data-ttu-id="70743-368">Na **nastavení přenosu** vyberte **pasivní**.</span><span class="sxs-lookup"><span data-stu-id="70743-368">On the **Transfer Settings** tab, select **Passive**.</span></span>
3. <span data-ttu-id="70743-369">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="70743-369">Click **Connect**.</span></span> <span data-ttu-id="70743-370">Pokud úspěšné, je FileZilla konzoly se zobrazí `Status: Connected` zprávu a problém `LIST` příkaz Zobrazit obsah adresáře.</span><span class="sxs-lookup"><span data-stu-id="70743-370">If successful, FileZilla's console will display a `Status: Connected` message and issue a `LIST` command to list the directory contents.</span></span>
4. <span data-ttu-id="70743-371">V **místní** lokality panelů, vyberte zdrojový adresář, ve kterém se nachází soubor JSPHello.war; cesta bude vypadat podobně jako následující:</span><span class="sxs-lookup"><span data-stu-id="70743-371">In the **Local** site panel, select the source directory in which the JSPHello.war file resides; the path will be similar to the following:</span></span>
   
    `<project-path>/JSPHello/src/`
5. <span data-ttu-id="70743-372">V **vzdáleného** lokality panelů, vyberte cílovou složku.</span><span class="sxs-lookup"><span data-stu-id="70743-372">In the **Remote** site panel, select the destination folder.</span></span> <span data-ttu-id="70743-373">Budete nasazovat soubor WAR `webapps` adresář v kořenovém adresáři webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-373">You will deploy the WAR file to the `webapps` directory under the web app's root.</span></span> <span data-ttu-id="70743-374">Přejděte na `/site/wwwroot`, klikněte pravým tlačítkem na `wwwroot`a vyberte **vytvořit adresář**.</span><span class="sxs-lookup"><span data-stu-id="70743-374">Navigate to `/site/wwwroot`, right-click on `wwwroot`, and select **Create directory**.</span></span> <span data-ttu-id="70743-375">Název adresáře `webapps` a zadejte tento adresář.</span><span class="sxs-lookup"><span data-stu-id="70743-375">Name the directory `webapps` and enter that directory.</span></span>
6. <span data-ttu-id="70743-376">Přenos JSPHello.war k `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="70743-376">Transfer JSPHello.war to `/site/wwwroot/webapps`.</span></span> <span data-ttu-id="70743-377">Vyberte JSPHello.war v **místní** soubor seznamu, klikněte na něj pravým tlačítkem myši a vyberte **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="70743-377">Select JSPHello.war in the **Local** file list, right-click on it and select **Upload**.</span></span> <span data-ttu-id="70743-378">Měli byste vidět se zobrazí v `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="70743-378">You should see it appear in `/site/wwwroot/webapps`.</span></span>
7. <span data-ttu-id="70743-379">Po zkopírování JSPHello.war do adresáře webapps, bude automaticky rozbalte Tomcat Server (rozbalte) soubory v souboru WAR.</span><span class="sxs-lookup"><span data-stu-id="70743-379">After you copy JSPHello.war to the webapps directory, Tomcat Server will automatically unpack (unzip) the files in the WAR file.</span></span> <span data-ttu-id="70743-380">I když Tomcat Server začne rozbalování téměř okamžitě, může trvat dlouho čas (pravděpodobně hodiny) pro soubory se objeví v klientovi FTP.</span><span class="sxs-lookup"><span data-stu-id="70743-380">Although Tomcat Server begins unpacking almost immediately, it might take a long time (possibly hours) for the files to appear in the FTP client.</span></span>

#### <a name="run-the-hello-world-application-on-the-web-app"></a><span data-ttu-id="70743-381">Spuštění aplikace Hello World na webové aplikace</span><span class="sxs-lookup"><span data-stu-id="70743-381">Run the Hello World application on the Web App</span></span>
1. <span data-ttu-id="70743-382">Po nahrát soubor WAR a ověřit, že Tomcat server vytvořil rozbalené `JSPHello` adresáře, procházet a `http://webdemowebapp.azurewebsites.net/JSPHello` ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="70743-382">After you have uploaded the WAR file and verified that Tomcat server has created an unpacked `JSPHello` directory, browse to `http://webdemowebapp.azurewebsites.net/JSPHello` to run the application.</span></span>
   
   > <span data-ttu-id="70743-383">**Poznámka:** Pokud kliknete na tlačítko **Procházet** z klasického portálu, může získat výchozí webovou stránku, oznámením "této webové aplikace Java na základě úspěšně vytvořila."</span><span class="sxs-lookup"><span data-stu-id="70743-383">**Note:** If you click **Browse** from the classic portal, you might get the default webpage, saying "This Java based web application has been successfully created."</span></span> <span data-ttu-id="70743-384">Možná budete muset aktualizovat webovou stránku, chcete-li zobrazit výstup aplikace místo výchozí webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="70743-384">You might have to refresh the webpage in order to view the application output instead of the default webpage.</span></span>
   > 
   > 
2. <span data-ttu-id="70743-385">Když je aplikace spuštěná, byste měli vidět na webové stránce s následující výstup:</span><span class="sxs-lookup"><span data-stu-id="70743-385">When the application runs, you should see a web page with the following output:</span></span>
   
    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="clean-up-azure-resources"></a><span data-ttu-id="70743-386">Vyčištění prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="70743-386">Clean up Azure resources</span></span>
<span data-ttu-id="70743-387">Tento postup vytvoří webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="70743-387">This procedure creates an App Service web app.</span></span> <span data-ttu-id="70743-388">Také existuje bude platit pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="70743-388">You will be billed for the resource as long as it exists.</span></span> <span data-ttu-id="70743-389">Pokud budete chtít pokračovat v používání webové aplikace pro testování nebo pro vývoj, měli byste zvážit, zastavení nebo odstranění.</span><span class="sxs-lookup"><span data-stu-id="70743-389">Unless you plan to continue using the web app for testing or development, you should consider stopping or deleting it.</span></span> <span data-ttu-id="70743-390">Webovou aplikaci, která byla zastavena stále zpoplatněná malé poplatků, ale můžete ji restartovat kdykoli.</span><span class="sxs-lookup"><span data-stu-id="70743-390">A web app that has been stopped will still incur a small charge, but you can restart it at any time.</span></span> <span data-ttu-id="70743-391">Odstranění webové aplikace vymaže všechna data, které jste odeslali do ní.</span><span class="sxs-lookup"><span data-stu-id="70743-391">Deleting a web app erases all data you have uploaded to it.</span></span>

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
