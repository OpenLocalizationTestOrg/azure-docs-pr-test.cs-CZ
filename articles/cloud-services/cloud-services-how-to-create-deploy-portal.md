---
title: "Postup vytvoření a nasazení cloudové služby | Microsoft Docs"
description: "Zjistěte, jak vytvořit a nasadit cloudové služby pomocí portálu Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 56ea2f14-34a2-4ed9-857c-82be4c9d0579
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: e5ce666f1d826c7901c9fd5e7fafe6171139c3ad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-deploy-a-cloud-service"></a><span data-ttu-id="9e44b-103">Postup vytvoření a nasazení cloudové služby</span><span class="sxs-lookup"><span data-stu-id="9e44b-103">How to create and deploy a cloud service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9e44b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9e44b-104">Azure portal</span></span>](cloud-services-how-to-create-deploy-portal.md)
> * [<span data-ttu-id="9e44b-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="9e44b-105">Azure classic portal</span></span>](cloud-services-how-to-create-deploy.md)
>
>

<span data-ttu-id="9e44b-106">Portál Azure nabízí dva způsoby, můžete vytvořit a nasadit cloudové služby: *rychle vytvořit* a *vytvořit vlastní*.</span><span class="sxs-lookup"><span data-stu-id="9e44b-106">The Azure portal provides two ways for you to create and deploy a cloud service: *Quick Create* and *Custom Create*.</span></span>

<span data-ttu-id="9e44b-107">Tento článek vysvětluje, jak pomocí metody rychlého vytvoření vytvořit novou cloudovou službu a pak použít **nahrát** nahrání a nasazení balíčku cloudové služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="9e44b-107">This article explains how to use the Quick Create method to create a new cloud service and then use **Upload** to upload and deploy a cloud service package in Azure.</span></span> <span data-ttu-id="9e44b-108">Pokud použijete tuto metodu, umožňuje portálu Azure k dispozici užitečné odkazy pro dokončení všechny požadavky rovnou.</span><span class="sxs-lookup"><span data-stu-id="9e44b-108">When you use this method, the Azure portal makes available convenient links for completing all requirements as you go.</span></span> <span data-ttu-id="9e44b-109">Pokud jste připravení nasadit cloudové služby, když ho vytvoříte, můžete to udělat i ve stejnou dobu pomocí vytvořit vlastní.</span><span class="sxs-lookup"><span data-stu-id="9e44b-109">If you're ready to deploy your cloud service when you create it, you can do both at the same time using Custom Create.</span></span>

> [!NOTE]
> <span data-ttu-id="9e44b-110">Pokud hodláte publikovat cloudové služby z Visual Studio Team Services (služby VSTS), použijte rychle vytvořit a potom nastavit publikování služby VSTS z rychlého startu Azure nebo na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="9e44b-110">If you plan to publish your cloud service from Visual Studio Team Services (VSTS), use Quick Create, and then set up VSTS publishing from the Azure Quickstart or the dashboard.</span></span> <span data-ttu-id="9e44b-111">Další informace najdete v tématu [nastavené průběžné doručování do Azure pomocí pomocí Visual Studio Team Services][TFSTutorialForCloudService], nebo naleznete v nápovědě **rychlý Start** stránky.</span><span class="sxs-lookup"><span data-stu-id="9e44b-111">For more information, see [Continuous Delivery to Azure by Using Visual Studio Team Services][TFSTutorialForCloudService], or see help for the **Quick Start** page.</span></span>
>
>

## <a name="concepts"></a><span data-ttu-id="9e44b-112">Koncepty</span><span class="sxs-lookup"><span data-stu-id="9e44b-112">Concepts</span></span>
<span data-ttu-id="9e44b-113">Tři součásti jsou nutné k nasazení aplikace jako cloudové služby v Azure:</span><span class="sxs-lookup"><span data-stu-id="9e44b-113">Three components are required to deploy an application as a cloud service in Azure:</span></span>

* <span data-ttu-id="9e44b-114">**Definice služby**</span><span class="sxs-lookup"><span data-stu-id="9e44b-114">**Service Definition**</span></span>  
  <span data-ttu-id="9e44b-115">Soubor definice služby (.csdef) cloudu definuje model služeb, včetně počet rolí.</span><span class="sxs-lookup"><span data-stu-id="9e44b-115">The cloud service definition file (.csdef) defines the service model, including the number of roles.</span></span>
* <span data-ttu-id="9e44b-116">**Konfigurace služby**</span><span class="sxs-lookup"><span data-stu-id="9e44b-116">**Service Configuration**</span></span>  
  <span data-ttu-id="9e44b-117">Cloudové služby konfiguračního souboru (.cscfg) poskytuje nastavení konfigurace pro cloudové služby a jednotlivé role, včetně počtu instancí role.</span><span class="sxs-lookup"><span data-stu-id="9e44b-117">The cloud service configuration file (.cscfg) provides configuration settings for the cloud service and individual roles, including the number of role instances.</span></span>
* <span data-ttu-id="9e44b-118">**Balíček služby**</span><span class="sxs-lookup"><span data-stu-id="9e44b-118">**Service Package**</span></span>  
  <span data-ttu-id="9e44b-119">Balíček služby (.cspkg) obsahuje kódu aplikace a konfigurace a soubor definice služby.</span><span class="sxs-lookup"><span data-stu-id="9e44b-119">The service package (.cspkg) contains the application code and configurations and the service definition file.</span></span>

<span data-ttu-id="9e44b-120">Další informace o těchto a postup vytvoření balíčku [zde](cloud-services-model-and-package.md).</span><span class="sxs-lookup"><span data-stu-id="9e44b-120">You can learn more about these and how to create a package [here](cloud-services-model-and-package.md).</span></span>

## <a name="prepare-your-app"></a><span data-ttu-id="9e44b-121">Příprava aplikace</span><span class="sxs-lookup"><span data-stu-id="9e44b-121">Prepare your app</span></span>
<span data-ttu-id="9e44b-122">Před nasazením cloudové služby, musíte vytvořit balíček cloudové služby (.cspkg) z kódu aplikace a cloudové služby konfiguračního souboru (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="9e44b-122">Before you can deploy a cloud service, you must create the cloud service package (.cspkg) from your application code and a cloud service configuration file (.cscfg).</span></span> <span data-ttu-id="9e44b-123">Azure SDK poskytuje nástroje pro přípravu tyto soubory požadované k nasazení.</span><span class="sxs-lookup"><span data-stu-id="9e44b-123">The Azure SDK provides tools for preparing these required deployment files.</span></span> <span data-ttu-id="9e44b-124">Můžete nainstalovat sadu SDK z [Azure stáhne](https://azure.microsoft.com/downloads/) stránky v jazyce, ve kterém chcete vyvíjet kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e44b-124">You can install the SDK from the [Azure Downloads](https://azure.microsoft.com/downloads/) page, in the language in which you prefer to develop your application code.</span></span>

<span data-ttu-id="9e44b-125">Funkce služby tři cloudu vyžadovat zvláštní konfigurace před exportem balíčku služby:</span><span class="sxs-lookup"><span data-stu-id="9e44b-125">Three cloud service features require special configurations before you export a service package:</span></span>

* <span data-ttu-id="9e44b-126">Pokud chcete nasadit cloudovou službu, která používá Secure Sockets Layer (SSL) pro šifrování dat [konfiguraci aplikace](cloud-services-configure-ssl-certificate-portal.md#modify) pro protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="9e44b-126">If you want to deploy a cloud service that uses Secure Sockets Layer (SSL) for data encryption, [configure your application](cloud-services-configure-ssl-certificate-portal.md#modify) for SSL.</span></span>
* <span data-ttu-id="9e44b-127">Pokud chcete nakonfigurovat připojení ke vzdálené ploše do instance rolí, [konfiguraci rolí](cloud-services-role-enable-remote-desktop-new-portal.md) pro vzdálenou plochu.</span><span class="sxs-lookup"><span data-stu-id="9e44b-127">If you want to configure Remote Desktop connections to role instances, [configure the roles](cloud-services-role-enable-remote-desktop-new-portal.md) for Remote Desktop.</span></span>
* <span data-ttu-id="9e44b-128">Pokud chcete nakonfigurovat podrobné monitorování pro cloudové služby, povolte pro Cloudová služba Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="9e44b-128">If you want to configure verbose monitoring for your cloud service, enable Azure Diagnostics for the cloud service.</span></span> <span data-ttu-id="9e44b-129">*Minimální monitorování* (výchozí možnost monitorování úroveň) použije čítače výkonu shromážděné z hostitele operační systémy pro instance rolí (virtuální počítače).</span><span class="sxs-lookup"><span data-stu-id="9e44b-129">*Minimal monitoring* (the default monitoring level) uses performance counters gathered from the host operating systems for role instances (virtual machines).</span></span> <span data-ttu-id="9e44b-130">*Podrobné monitorování* shromažďuje další metriky na základě dat o výkonu v rámci instance rolí povolit blíže analyzovat problémy, ke kterým došlo během zpracování aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e44b-130">*Verbose monitoring* gathers additional metrics based on performance data within the role instances to enable closer analysis of issues that occur during application processing.</span></span> <span data-ttu-id="9e44b-131">Postup povolení Azure Diagnostics naleznete v tématu [povolení diagnostiky v Azure](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="9e44b-131">To find out how to enable Azure Diagnostics, see [Enabling diagnostics in Azure](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="9e44b-132">Postup vytvoření cloudové služby se nasazení webové role nebo rolí pracovního procesu, je nutné [vytvořit balíček služby](cloud-services-model-and-package.md#servicepackagecspkg).</span><span class="sxs-lookup"><span data-stu-id="9e44b-132">To create a cloud service with deployments of web roles or worker roles, you must [create the service package](cloud-services-model-and-package.md#servicepackagecspkg).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="9e44b-133">Než začnete</span><span class="sxs-lookup"><span data-stu-id="9e44b-133">Before you begin</span></span>
* <span data-ttu-id="9e44b-134">Pokud jste nenainstalovali sadu Azure SDK, klikněte na tlačítko **instalovat sadu SDK Azure** otevřete [Azure stáhne stránky](https://azure.microsoft.com/downloads/)a pak stažení sady SDK pro jazyk, ve kterém chcete vyvíjet kódu.</span><span class="sxs-lookup"><span data-stu-id="9e44b-134">If you haven't installed the Azure SDK, click **Install Azure SDK** to open the [Azure Downloads page](https://azure.microsoft.com/downloads/), and then download the SDK for the language in which you prefer to develop your code.</span></span> <span data-ttu-id="9e44b-135">(Budete mít příležitost udělat to později.)</span><span class="sxs-lookup"><span data-stu-id="9e44b-135">(You'll have an opportunity to do this later.)</span></span>
* <span data-ttu-id="9e44b-136">Pokud všechny instance role vyžadovat certifikát, vytvořte certifikáty.</span><span class="sxs-lookup"><span data-stu-id="9e44b-136">If any role instances require a certificate, create the certificates.</span></span> <span data-ttu-id="9e44b-137">Cloudové služby vyžadují soubor .pfx s privátním klíčem.</span><span class="sxs-lookup"><span data-stu-id="9e44b-137">Cloud services require a .pfx file with a private key.</span></span> <span data-ttu-id="9e44b-138">Certifikáty můžete nahrát do Azure, jak vytvořit a nasadit cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9e44b-138">You can upload the certificates to Azure as you create and deploy the cloud service.</span></span>

## <a name="create-and-deploy"></a><span data-ttu-id="9e44b-139">Vytvoření a nasazení</span><span class="sxs-lookup"><span data-stu-id="9e44b-139">Create and deploy</span></span>
1. <span data-ttu-id="9e44b-140">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9e44b-140">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="9e44b-141">Klikněte na tlačítko **nový > výpočetní**a poté přejděte dolů k a klikněte na tlačítko **Cloudová služba**.</span><span class="sxs-lookup"><span data-stu-id="9e44b-141">Click **New > Compute**, and then scroll down to and click **Cloud Service**.</span></span>

    ![Publikování cloudové služby](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. <span data-ttu-id="9e44b-143">V novém **Cloudová služba** okno, zadejte hodnotu **název DNS**.</span><span class="sxs-lookup"><span data-stu-id="9e44b-143">In the new **Cloud Service** blade, enter a value for the **DNS name**.</span></span>
4. <span data-ttu-id="9e44b-144">Vytvořte novou **skupiny prostředků** nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="9e44b-144">Create a new **Resource Group** or select an existing one.</span></span>
5. <span data-ttu-id="9e44b-145">Vyberte **Umístění**.</span><span class="sxs-lookup"><span data-stu-id="9e44b-145">Select a **Location**.</span></span>
6. <span data-ttu-id="9e44b-146">Klikněte na tlačítko **balíček**.</span><span class="sxs-lookup"><span data-stu-id="9e44b-146">Click **Package**.</span></span> <span data-ttu-id="9e44b-147">Otevře se **nahrání balíčku** okno.</span><span class="sxs-lookup"><span data-stu-id="9e44b-147">This will open the **Upload a package** blade.</span></span> <span data-ttu-id="9e44b-148">Vyplňte požadovaná pole.</span><span class="sxs-lookup"><span data-stu-id="9e44b-148">Fill in the required fields.</span></span> <span data-ttu-id="9e44b-149">Pokud některé z vaší rolí obsahuje jednu instanci, ujistěte se, **nasadit, i když jedna nebo víc rolí obsahuje jednu instanci** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="9e44b-149">If any of your roles contain a single instance, ensure **Deploy even if one or more roles contain a single instance** is selected.</span></span>
7. <span data-ttu-id="9e44b-150">Ujistěte se, že **spuštění nasazení** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="9e44b-150">Make sure that **Start deployment** is selected.</span></span>
8. <span data-ttu-id="9e44b-151">Klikněte na tlačítko **OK** který bude ukončen **nahrání balíčku** okno.</span><span class="sxs-lookup"><span data-stu-id="9e44b-151">Click **OK** which will close the **Upload a package** blade.</span></span>
9. <span data-ttu-id="9e44b-152">Pokud nemáte žádné certifikáty, které chcete přidat, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9e44b-152">If you do not have any certificates to add, click **Create**.</span></span>

    ![Publikování cloudové služby](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a><span data-ttu-id="9e44b-154">Nahrání certifikátu</span><span class="sxs-lookup"><span data-stu-id="9e44b-154">Upload a certificate</span></span>
<span data-ttu-id="9e44b-155">Pokud byl balíček pro nasazení [nakonfigurované na používání certifikátů](cloud-services-configure-ssl-certificate-portal.md#modify), teď můžete nahrát certifikát.</span><span class="sxs-lookup"><span data-stu-id="9e44b-155">If your deployment package was [configured to use certificates](cloud-services-configure-ssl-certificate-portal.md#modify), you can upload the certificate now.</span></span>

1. <span data-ttu-id="9e44b-156">Vyberte **certifikáty**a na **přidat certifikáty** okně, vyberte soubor .pfx certifikátu SSL a pak zadejte **heslo** pro certifikát</span><span class="sxs-lookup"><span data-stu-id="9e44b-156">Select **Certificates**, and on the **Add certificates** blade, select the SSL certificate .pfx file, and then provide the **Password** for the certificate,</span></span>
2. <span data-ttu-id="9e44b-157">Klikněte na tlačítko **připojit certifikát**a potom klikněte na **OK** na **přidat certifikáty** okno.</span><span class="sxs-lookup"><span data-stu-id="9e44b-157">Click **Attach certificate**, and then click **OK** on the **Add certificates** blade.</span></span>
3. <span data-ttu-id="9e44b-158">Klikněte na tlačítko **vytvořit** na **Cloudová služba** okno.</span><span class="sxs-lookup"><span data-stu-id="9e44b-158">Click **Create** on the **Cloud Service** blade.</span></span> <span data-ttu-id="9e44b-159">Pokud bylo dosaženo nasazení **připraven** stav, můžete pokračovat na další kroky.</span><span class="sxs-lookup"><span data-stu-id="9e44b-159">When the deployment has reached the **Ready** status, you can proceed to the next steps.</span></span>

    ![Publikování cloudové služby](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a><span data-ttu-id="9e44b-161">Zkontrolujte vaše nasazení bylo úspěšně dokončeno</span><span class="sxs-lookup"><span data-stu-id="9e44b-161">Verify your deployment completed successfully</span></span>
1. <span data-ttu-id="9e44b-162">Klikněte na tlačítko instance cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9e44b-162">Click the cloud service instance.</span></span>

    <span data-ttu-id="9e44b-163">Stav by měl zobrazit, zda je služba **systémem**.</span><span class="sxs-lookup"><span data-stu-id="9e44b-163">The status should show that the service is **Running**.</span></span>
2. <span data-ttu-id="9e44b-164">V části **Essentials**, klikněte **adresa URL webu** ve webovém prohlížeči otevřít cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9e44b-164">Under **Essentials**, click the **Site URL** to open your cloud service in a web browser.</span></span>

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a><span data-ttu-id="9e44b-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9e44b-166">Next steps</span></span>
* <span data-ttu-id="9e44b-167">[Obecná konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9e44b-167">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="9e44b-168">Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9e44b-168">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="9e44b-169">[Správa služby cloud](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9e44b-169">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
* <span data-ttu-id="9e44b-170">Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9e44b-170">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
