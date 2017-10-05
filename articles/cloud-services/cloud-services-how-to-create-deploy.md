---
title: "Postup vytvoření a nasazení cloudové služby | Microsoft Docs"
description: "Zjistěte, jak vytvořit a nasadit v Azure pomocí metody rychlého vytvoření cloudové služby."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 0ea78ccc-5e7d-40f8-bdb6-478c0eb0e265
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 2a2172a78bfd3ac923edbc9de366b035629dd27b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-create-and-deploy-a-cloud-service"></a><span data-ttu-id="28ace-103">Postup vytvoření a nasazení cloudové služby</span><span class="sxs-lookup"><span data-stu-id="28ace-103">How to Create and Deploy a Cloud Service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="28ace-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="28ace-104">Azure portal</span></span>](cloud-services-how-to-create-deploy-portal.md)
> * [<span data-ttu-id="28ace-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="28ace-105">Azure classic portal</span></span>](cloud-services-how-to-create-deploy.md)
> 
> 

<span data-ttu-id="28ace-106">Portál Azure classic nabízí dva způsoby, kterými můžete vytvořit a nasadit cloudové služby: **rychle vytvořit** a **vytvořit vlastní**.</span><span class="sxs-lookup"><span data-stu-id="28ace-106">The Azure classic portal provides two ways for you to create and deploy a cloud service: **Quick Create** and **Custom Create**.</span></span>

<span data-ttu-id="28ace-107">Toto téma vysvětluje, jak lze pomocí metody rychlého vytvoření vytvořit novou cloudovou službu a pak použít **nahrát** nahrání a nasazení balíčku cloudové služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="28ace-107">This topic explains how to use the Quick Create method to create a new cloud service and then use **Upload** to upload and deploy a cloud service package in Azure.</span></span> <span data-ttu-id="28ace-108">Pokud použijete tuto metodu, umožňuje portálu Azure classic k dispozici užitečné odkazy pro dokončení všechny požadavky rovnou.</span><span class="sxs-lookup"><span data-stu-id="28ace-108">When you use this method, the Azure classic portal makes available convenient links for completing all requirements as you go.</span></span> <span data-ttu-id="28ace-109">Pokud jste připravení nasadit cloudové služby, když ho vytvoříte, můžete provést jak na současně pomocí **vytvořit vlastní**.</span><span class="sxs-lookup"><span data-stu-id="28ace-109">If you're ready to deploy your cloud service when you create it, you can do both at the same time using **Custom Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="28ace-110">Pokud hodláte publikovat cloudové služby z Visual Studio Team Services (služby VSTS), použijte **rychle vytvořit**a poté nastavit publikování služby VSTS z **rychlý Start** nebo na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="28ace-110">If you plan to publish your cloud service from Visual Studio Team Services (VSTS), use **Quick Create**, and then set up VSTS publishing from **Quick Start** or the dashboard.</span></span>
> 
> 

## <a name="concepts"></a><span data-ttu-id="28ace-111">Koncepty</span><span class="sxs-lookup"><span data-stu-id="28ace-111">Concepts</span></span>
<span data-ttu-id="28ace-112">Tři součásti jsou nutné k nasazení aplikace jako cloudové služby v Azure:</span><span class="sxs-lookup"><span data-stu-id="28ace-112">Three components are required in order to deploy an application as a cloud service in Azure:</span></span>

* <span data-ttu-id="28ace-113">**Definice služby**</span><span class="sxs-lookup"><span data-stu-id="28ace-113">**Service Definition**</span></span>  
  <span data-ttu-id="28ace-114">Soubor definice služby (.csdef) cloudu definuje model služeb, včetně počet rolí.</span><span class="sxs-lookup"><span data-stu-id="28ace-114">The cloud service definition file (.csdef) defines the service model, including the number of roles.</span></span>
* <span data-ttu-id="28ace-115">**Konfigurace služby**</span><span class="sxs-lookup"><span data-stu-id="28ace-115">**Service Configuration**</span></span>  
  <span data-ttu-id="28ace-116">Cloudové služby konfiguračního souboru (.cscfg) poskytuje nastavení konfigurace pro cloudové služby a jednotlivé role, včetně počtu instancí role.</span><span class="sxs-lookup"><span data-stu-id="28ace-116">The cloud service configuration file (.cscfg) provides configuration settings for the cloud service and individual roles, including the number of role instances.</span></span>
* <span data-ttu-id="28ace-117">**Balíček služby**</span><span class="sxs-lookup"><span data-stu-id="28ace-117">**Service Package**</span></span>  
  <span data-ttu-id="28ace-118">Balíček služby (.cspkg) obsahuje kódu aplikace a konfigurace a soubor definice služby.</span><span class="sxs-lookup"><span data-stu-id="28ace-118">The service package (.cspkg) contains the application code and configurations and the service definition file.</span></span>

<span data-ttu-id="28ace-119">Další informace o těchto a postup vytvoření balíčku [zde](cloud-services-model-and-package.md).</span><span class="sxs-lookup"><span data-stu-id="28ace-119">You can learn more about these and how to create a package [here](cloud-services-model-and-package.md).</span></span>

## <a name="prepare-your-app"></a><span data-ttu-id="28ace-120">Příprava aplikace</span><span class="sxs-lookup"><span data-stu-id="28ace-120">Prepare your app</span></span>
<span data-ttu-id="28ace-121">Před nasazením cloudové služby, musíte vytvořit balíček cloudové služby (.cspkg) z kódu aplikace a cloudové služby konfiguračního souboru (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="28ace-121">Before you can deploy a cloud service, you must create the cloud service package (.cspkg) from your application code and a cloud service configuration file (.cscfg).</span></span> <span data-ttu-id="28ace-122">Azure SDK poskytuje nástroje pro přípravu tyto soubory požadované k nasazení.</span><span class="sxs-lookup"><span data-stu-id="28ace-122">The Azure SDK provides tools for preparing these required deployment files.</span></span> <span data-ttu-id="28ace-123">Můžete nainstalovat sadu SDK z [Azure stáhne](https://azure.microsoft.com/downloads/) stránky v jazyce, ve kterém chcete vyvíjet kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="28ace-123">You can install the SDK from the [Azure Downloads](https://azure.microsoft.com/downloads/) page, in the language in which you prefer to develop your application code.</span></span>

<span data-ttu-id="28ace-124">Funkce služby tři cloudu vyžadovat zvláštní konfigurace před exportem balíčku služby:</span><span class="sxs-lookup"><span data-stu-id="28ace-124">Three cloud service features require special configurations before you export a service package:</span></span>

* <span data-ttu-id="28ace-125">Pokud chcete nasadit cloudovou službu, která používá Secure Sockets Layer (SSL) pro šifrování dat [konfiguraci aplikace](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) pro protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="28ace-125">If you want to deploy a cloud service that uses Secure Sockets Layer (SSL) for data encryption, [configure your application](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) for SSL.</span></span>
* <span data-ttu-id="28ace-126">Pokud chcete nakonfigurovat připojení ke vzdálené ploše do instance rolí, [konfiguraci rolí](cloud-services-role-enable-remote-desktop.md) pro vzdálenou plochu.</span><span class="sxs-lookup"><span data-stu-id="28ace-126">If you want to configure Remote Desktop connections to role instances, [configure the roles](cloud-services-role-enable-remote-desktop.md) for Remote Desktop.</span></span>
* <span data-ttu-id="28ace-127">Pokud chcete nakonfigurovat podrobné monitorování pro cloudové služby, povolte pro Cloudová služba Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="28ace-127">If you want to configure verbose monitoring for your cloud service, enable Azure Diagnostics for the cloud service.</span></span> <span data-ttu-id="28ace-128">*Minimální monitorování* (výchozí možnost monitorování úroveň) použije čítače výkonu shromážděné z hostitele operační systémy pro instance rolí (virtuální počítače).</span><span class="sxs-lookup"><span data-stu-id="28ace-128">*Minimal monitoring* (the default monitoring level) uses performance counters gathered from the host operating systems for role instances (virtual machines).</span></span> <span data-ttu-id="28ace-129">"Podrobné monitorování * shromažďuje další metriky na základě dat o výkonu v rámci instance rolí povolit blíže analyzovat problémy, ke kterým došlo během zpracování aplikace.</span><span class="sxs-lookup"><span data-stu-id="28ace-129">"Verbose monitoring* gathers additional metrics based on performance data within the role instances to enable closer analysis of issues that occur during application processing.</span></span> <span data-ttu-id="28ace-130">Postup povolení Azure Diagnostics naleznete v tématu [povolení diagnostiky v Azure](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="28ace-130">To find out how to enable Azure Diagnostics, see [Enabling Diagnostics in Azure](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="28ace-131">Postup vytvoření cloudové služby se nasazení webové role nebo rolí pracovního procesu, je nutné [vytvořit balíček služby](cloud-services-model-and-package.md#servicepackagecspkg).</span><span class="sxs-lookup"><span data-stu-id="28ace-131">To create a cloud service with deployments of web roles or worker roles, you must [create the service package](cloud-services-model-and-package.md#servicepackagecspkg).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="28ace-132">Než začnete</span><span class="sxs-lookup"><span data-stu-id="28ace-132">Before you begin</span></span>
* <span data-ttu-id="28ace-133">Pokud jste nenainstalovali sadu Azure SDK, klikněte na tlačítko **instalovat sadu SDK Azure** otevřete [Azure stáhne stránky](https://azure.microsoft.com/downloads/)a pak stažení sady SDK pro jazyk, ve kterém chcete vyvíjet kódu.</span><span class="sxs-lookup"><span data-stu-id="28ace-133">If you haven't installed the Azure SDK, click **Install Azure SDK** to open the [Azure Downloads page](https://azure.microsoft.com/downloads/), and then download the SDK for the language in which you prefer to develop your code.</span></span> <span data-ttu-id="28ace-134">(Budete mít příležitost udělat to později.)</span><span class="sxs-lookup"><span data-stu-id="28ace-134">(You'll have an opportunity to do this later.)</span></span>
* <span data-ttu-id="28ace-135">Pokud všechny instance role vyžadovat certifikát, vytvořte certifikáty.</span><span class="sxs-lookup"><span data-stu-id="28ace-135">If any role instances require a certificate, create the certificates.</span></span> <span data-ttu-id="28ace-136">Cloudové služby vyžadují soubor .pfx s privátním klíčem.</span><span class="sxs-lookup"><span data-stu-id="28ace-136">Cloud services require a .pfx file with a private key.</span></span> <span data-ttu-id="28ace-137">Můžete [nahrát certifikáty do Azure](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) jak vytvořit a nasadit cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="28ace-137">You can [upload the certificates to Azure](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) as you create and deploy the cloud service.</span></span>
* <span data-ttu-id="28ace-138">Pokud plánujete nasadit cloudovou službu do skupiny vztahů, vytvořte skupinu vztahů.</span><span class="sxs-lookup"><span data-stu-id="28ace-138">If you plan to deploy the cloud service to an affinity group, create the affinity group.</span></span> <span data-ttu-id="28ace-139">Skupiny vztahů můžete použít k nasazení cloudové služby a jinými službami Azure do stejného umístění, v oblasti.</span><span class="sxs-lookup"><span data-stu-id="28ace-139">You can use an affinity group to deploy your cloud service and other Azure services to the same location in a region.</span></span> <span data-ttu-id="28ace-140">Můžete vytvořit skupinu vztahů v **sítě** oblasti Azure classic na portálu **skupiny vztahů** stránky.</span><span class="sxs-lookup"><span data-stu-id="28ace-140">You can create the affinity group in the **Networks** area of the Azure classic portal, on the **Affinity Groups** page.</span></span>

## <a name="how-to-create-a-cloud-service-using-quick-create"></a><span data-ttu-id="28ace-141">Postupy: vytvoření cloudové služby pomocí metody rychlého vytvoření</span><span class="sxs-lookup"><span data-stu-id="28ace-141">How to: Create a cloud service using Quick Create</span></span>
1. <span data-ttu-id="28ace-142">V [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **nový**>**výpočetní**>**Cloudová služba**>**rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="28ace-142">In the [Azure classic portal](http://manage.windowsazure.com/), click **New**>**Compute**>**Cloud Service**>**Quick Create**.</span></span>
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)
2. <span data-ttu-id="28ace-144">V **URL**, zadejte název subdomény pro použití v veřejnou adresu URL pro přístup k vaší cloudové služby v rámci nasazení v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="28ace-144">In **URL**, enter a subdomain name to use in the public URL for accessing your cloud service in production deployments.</span></span> <span data-ttu-id="28ace-145">Formát adresy URL pro nasazení v produkčním prostředí je: http://*myURL*. cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="28ace-145">The URL format for production deployments is: http://*myURL*.cloudapp.net.</span></span>
3. <span data-ttu-id="28ace-146">V **oblast nebo skupinu vztahů**, vyberte zeměpisnou oblast nebo skupinu vztahů nasadit cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="28ace-146">In **Region or Affinity Group**, select the geographic region or affinity group to deploy the cloud service to.</span></span> <span data-ttu-id="28ace-147">Pokud chcete nasadit cloudové služby na stejné umístění jako jinými službami Azure v rámci oblasti, vyberte skupinu vztahů.</span><span class="sxs-lookup"><span data-stu-id="28ace-147">Select an affinity group if you want to deploy your cloud service to the same location as other Azure services within a region.</span></span>
4. <span data-ttu-id="28ace-148">Klikněte na **Vytvořit cloudovou službu**.</span><span class="sxs-lookup"><span data-stu-id="28ace-148">Click **Create Cloud Service**.</span></span>
   
    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)
   
    <span data-ttu-id="28ace-150">V oblasti zpráv v dolní části okna můžete sledovat stav procesu.</span><span class="sxs-lookup"><span data-stu-id="28ace-150">You can monitor the status of the process in the message area at the bottom of the window.</span></span>
   
    <span data-ttu-id="28ace-151">**Cloudové služby** oblasti otevře s novou cloudovou službu, zobrazí.</span><span class="sxs-lookup"><span data-stu-id="28ace-151">The **Cloud Services** area opens, with the new cloud service displayed.</span></span> <span data-ttu-id="28ace-152">Když se stav změní na vytvořen, vytvoření cloudové služby byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="28ace-152">When the status changes to Created, cloud service creation has completed successfully.</span></span>
   
    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)

## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a><span data-ttu-id="28ace-154">Postupy: nahrát na server certifikát pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="28ace-154">How to: Upload a certificate for a cloud service</span></span>
1. <span data-ttu-id="28ace-155">V [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**, klikněte na název cloudové služby a pak klikněte na tlačítko **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="28ace-155">In the [Azure classic portal](http://manage.windowsazure.com/), click **Cloud Services**, click the name of the cloud service, and then click **Certificates**.</span></span>
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)
2. <span data-ttu-id="28ace-157">Klikněte na možnost **nahrání certifikátu** nebo **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="28ace-157">Click either **Upload a certificate** or **Upload**.</span></span>
3. <span data-ttu-id="28ace-158">V **soubor**, použijte **Procházet** výběr certifikátu (soubor .pfx).</span><span class="sxs-lookup"><span data-stu-id="28ace-158">In **File**, use **Browse** to select the certificate (.pfx file).</span></span>
4. <span data-ttu-id="28ace-159">V **heslo**, zadejte privátní klíč pro certifikát.</span><span class="sxs-lookup"><span data-stu-id="28ace-159">In **Password**, enter the private key for the certificate.</span></span>
5. <span data-ttu-id="28ace-160">Klikněte na tlačítko **OK** (zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="28ace-160">Click **OK** (checkmark).</span></span>
   
    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)
   
    <span data-ttu-id="28ace-162">Můžete sledovat průběh nahrávání v oblasti zpráv vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="28ace-162">You can watch the progress of the upload in the message area, shown below.</span></span> <span data-ttu-id="28ace-163">Po dokončení nahrávání se certifikát přidat do tabulky.</span><span class="sxs-lookup"><span data-stu-id="28ace-163">When the upload completes, the certificate is added to the table.</span></span> <span data-ttu-id="28ace-164">V oblasti zpráv klikněte na tlačítko OK zprávu zavřete.</span><span class="sxs-lookup"><span data-stu-id="28ace-164">In the message area, click OK to close the message.</span></span>
   
    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a><span data-ttu-id="28ace-166">Postupy: nasazení cloudové služby</span><span class="sxs-lookup"><span data-stu-id="28ace-166">How to: Deploy a cloud service</span></span>
1. <span data-ttu-id="28ace-167">V [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**, klikněte na název cloudové služby a pak klikněte na tlačítko **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="28ace-167">In the [Azure classic portal](http://manage.windowsazure.com/), click **Cloud Services**, click the name of the cloud service, and then click **Dashboard**.</span></span>
2. <span data-ttu-id="28ace-168">Klikněte na možnost **nahrát nový produkční nasazení** nebo **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="28ace-168">Click either **Upload a new production deployment** or **Upload**.</span></span>
3. <span data-ttu-id="28ace-169">V **označení nasazení**, zadejte název nové nasazení – například MyCloudServicev4.</span><span class="sxs-lookup"><span data-stu-id="28ace-169">In **Deployment label**, enter a name for the new deployment - for example, MyCloudServicev4.</span></span>
4. <span data-ttu-id="28ace-170">V **balíček**, použijte **Procházet** vyberte souboru balíku služeb (.cspkg) používat.</span><span class="sxs-lookup"><span data-stu-id="28ace-170">In **Package**, use **Browse** to select the service package file (.cspkg) to use.</span></span>
5. <span data-ttu-id="28ace-171">V **konfigurace**, použijte **Procházet** a vyberte soubor konfigurace služby (.cscfg) používat.</span><span class="sxs-lookup"><span data-stu-id="28ace-171">In **Configuration**, use **Browse** to select the service configure file (.cscfg) to use.</span></span>
6. <span data-ttu-id="28ace-172">Pokud cloudové služby bude obsahovat všechny role s jedinou instancí, vyberte **nasadit, i když jedna nebo víc rolí obsahuje jednu instanci** zaškrtávacího políčka umožníte nasazení, aby bylo možné pokračovat.</span><span class="sxs-lookup"><span data-stu-id="28ace-172">If the cloud service will include any roles with only one instance, select the **Deploy even if one or more roles contain a single instance** check box to enable the deployment to proceed.</span></span>
   
    <span data-ttu-id="28ace-173">Azure můžete pouze zaručit 99,95 % přístup ke cloudové službě během údržby a Služba aktualizací Pokud má aspoň dvě instance každé role.</span><span class="sxs-lookup"><span data-stu-id="28ace-173">Azure can only guarantee 99.95 percent access to the cloud service during maintenance and service updates if every role has at least two instances.</span></span> <span data-ttu-id="28ace-174">V případě potřeby můžete přidat další role instance na **škálování** stránky po nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="28ace-174">If needed, you can add additional role instances on the **Scale** page after you deploy the cloud service.</span></span> <span data-ttu-id="28ace-175">Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="28ace-175">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>
7. <span data-ttu-id="28ace-176">Klikněte na tlačítko **OK** (zaškrtnutí) zahájíte nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="28ace-176">Click **OK** (checkmark) to begin the cloud service deployment.</span></span>
   
    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)
   
    <span data-ttu-id="28ace-178">Můžete sledovat stav nasazení v oblasti zpráv.</span><span class="sxs-lookup"><span data-stu-id="28ace-178">You can monitor the status of the deployment in the message area.</span></span> <span data-ttu-id="28ace-179">Kliknutím na tlačítko OK skrýt zprávy.</span><span class="sxs-lookup"><span data-stu-id="28ace-179">Click OK to hide the message.</span></span>
   
    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a><span data-ttu-id="28ace-181">Zkontrolujte vaše nasazení bylo úspěšně dokončeno</span><span class="sxs-lookup"><span data-stu-id="28ace-181">Verify your deployment completed successfully</span></span>
1. <span data-ttu-id="28ace-182">Klikněte na tlačítko **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="28ace-182">Click **Dashboard**.</span></span>
   
    <span data-ttu-id="28ace-183">Stav by měl zobrazit, zda je služba **systémem**.</span><span class="sxs-lookup"><span data-stu-id="28ace-183">The status should show that the service is **Running**.</span></span>
2. <span data-ttu-id="28ace-184">V části **rychlého přehledu**, klikněte na adresu URL webu ve webovém prohlížeči otevřít cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="28ace-184">Under **quick glance**, click the site URL to open your cloud service in a web browser.</span></span>
   
    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


## <a name="next-steps"></a><span data-ttu-id="28ace-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="28ace-186">Next steps</span></span>
* <span data-ttu-id="28ace-187">[Obecná konfigurace cloudové služby](cloud-services-how-to-configure.md).</span><span class="sxs-lookup"><span data-stu-id="28ace-187">[General configuration of your cloud service](cloud-services-how-to-configure.md).</span></span>
* <span data-ttu-id="28ace-188">Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="28ace-188">Configure a [custom domain name](cloud-services-custom-domain-name.md).</span></span>
* <span data-ttu-id="28ace-189">[Správa služby cloud](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="28ace-189">[Manage your cloud service](cloud-services-how-to-manage.md).</span></span>
* <span data-ttu-id="28ace-190">Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).</span><span class="sxs-lookup"><span data-stu-id="28ace-190">Configure [ssl certificates](cloud-services-configure-ssl-certificate.md).</span></span>

