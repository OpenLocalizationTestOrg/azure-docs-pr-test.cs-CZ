---
title: "aaaHow toocreate a nasazení cloudové služby | Microsoft Docs"
description: "Zjistěte, jak toocreate a nasazení cloudové služby pomocí hello portálu Azure."
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
ms.openlocfilehash: dc8b81a594f3514e662c49c9a46a33da8a551f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-deploy-a-cloud-service"></a><span data-ttu-id="ea246-103">Jak toocreate a nasazení cloudové služby</span><span class="sxs-lookup"><span data-stu-id="ea246-103">How toocreate and deploy a cloud service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ea246-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ea246-104">Azure portal</span></span>](cloud-services-how-to-create-deploy-portal.md)
> * [<span data-ttu-id="ea246-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="ea246-105">Azure classic portal</span></span>](cloud-services-how-to-create-deploy.md)
>
>

<span data-ttu-id="ea246-106">Hello portál Azure nabízí dva způsoby, abyste toocreate a nasazení cloudové služby: *rychle vytvořit* a *vytvořit vlastní*.</span><span class="sxs-lookup"><span data-stu-id="ea246-106">hello Azure portal provides two ways for you toocreate and deploy a cloud service: *Quick Create* and *Custom Create*.</span></span>

<span data-ttu-id="ea246-107">Tento článek vysvětluje, jak toouse hello toocreate metoda rychle vytvořit novou cloudovou službu a pak použijte **nahrát** tooupload a nasadit balíček cloudové služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="ea246-107">This article explains how toouse hello Quick Create method toocreate a new cloud service and then use **Upload** tooupload and deploy a cloud service package in Azure.</span></span> <span data-ttu-id="ea246-108">Pokud použijete tuto metodu, díky hello portál Azure k dispozici užitečné odkazy pro dokončení všechny požadavky rovnou.</span><span class="sxs-lookup"><span data-stu-id="ea246-108">When you use this method, hello Azure portal makes available convenient links for completing all requirements as you go.</span></span> <span data-ttu-id="ea246-109">Pokud jste připravené toodeploy vaše cloudové služby při vytváření, můžete provést jak na hello současně pomocí vytvořit vlastní.</span><span class="sxs-lookup"><span data-stu-id="ea246-109">If you're ready toodeploy your cloud service when you create it, you can do both at hello same time using Custom Create.</span></span>

> [!NOTE]
> <span data-ttu-id="ea246-110">Pokud máte v plánu toopublish cloudové služby z Visual Studio Team Services (služby VSTS), použijte rychle vytvořit a potom nastavit publikování služby VSTS z řídicího panelu Azure Quickstart nebo hello hello.</span><span class="sxs-lookup"><span data-stu-id="ea246-110">If you plan toopublish your cloud service from Visual Studio Team Services (VSTS), use Quick Create, and then set up VSTS publishing from hello Azure Quickstart or hello dashboard.</span></span> <span data-ttu-id="ea246-111">Další informace najdete v tématu [tooAzure nastavené průběžné doručování podle pomocí Visual Studio Team Services][TFSTutorialForCloudService], nebo naleznete v nápovědě pro hello **rychlý Start** stránky.</span><span class="sxs-lookup"><span data-stu-id="ea246-111">For more information, see [Continuous Delivery tooAzure by Using Visual Studio Team Services][TFSTutorialForCloudService], or see help for hello **Quick Start** page.</span></span>
>
>

## <a name="concepts"></a><span data-ttu-id="ea246-112">Koncepty</span><span class="sxs-lookup"><span data-stu-id="ea246-112">Concepts</span></span>
<span data-ttu-id="ea246-113">Požadované toodeploy aplikace jako cloudové služby v Azure se tři komponenty:</span><span class="sxs-lookup"><span data-stu-id="ea246-113">Three components are required toodeploy an application as a cloud service in Azure:</span></span>

* <span data-ttu-id="ea246-114">**Definice služby**</span><span class="sxs-lookup"><span data-stu-id="ea246-114">**Service Definition**</span></span>  
  <span data-ttu-id="ea246-115">Soubor definice Hello cloudové služby (.csdef) definuje hello model služeb, včetně hello počet rolí.</span><span class="sxs-lookup"><span data-stu-id="ea246-115">hello cloud service definition file (.csdef) defines hello service model, including hello number of roles.</span></span>
* <span data-ttu-id="ea246-116">**Konfigurace služby**</span><span class="sxs-lookup"><span data-stu-id="ea246-116">**Service Configuration**</span></span>  
  <span data-ttu-id="ea246-117">Hello cloudové služby konfiguračního souboru (.cscfg) poskytuje nastavení konfigurace pro hello Cloudová služba a jednotlivé role, včetně hello počet instancí role.</span><span class="sxs-lookup"><span data-stu-id="ea246-117">hello cloud service configuration file (.cscfg) provides configuration settings for hello cloud service and individual roles, including hello number of role instances.</span></span>
* <span data-ttu-id="ea246-118">**Balíček služby**</span><span class="sxs-lookup"><span data-stu-id="ea246-118">**Service Package**</span></span>  
  <span data-ttu-id="ea246-119">balíček služby Hello (.cspkg) obsahuje kód aplikace hello a konfigurace a souboru definice služby hello.</span><span class="sxs-lookup"><span data-stu-id="ea246-119">hello service package (.cspkg) contains hello application code and configurations and hello service definition file.</span></span>

<span data-ttu-id="ea246-120">Další informace o těchto a jak toocreate balíček [zde](cloud-services-model-and-package.md).</span><span class="sxs-lookup"><span data-stu-id="ea246-120">You can learn more about these and how toocreate a package [here](cloud-services-model-and-package.md).</span></span>

## <a name="prepare-your-app"></a><span data-ttu-id="ea246-121">Příprava aplikace</span><span class="sxs-lookup"><span data-stu-id="ea246-121">Prepare your app</span></span>
<span data-ttu-id="ea246-122">Před nasazením cloudové služby, je nutné vytvořit balíček hello cloudové služby (.cspkg) z kódu aplikace a cloudové služby konfiguračního souboru (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="ea246-122">Before you can deploy a cloud service, you must create hello cloud service package (.cspkg) from your application code and a cloud service configuration file (.cscfg).</span></span> <span data-ttu-id="ea246-123">Hello Azure SDK poskytuje nástroje pro přípravu tyto soubory požadované k nasazení.</span><span class="sxs-lookup"><span data-stu-id="ea246-123">hello Azure SDK provides tools for preparing these required deployment files.</span></span> <span data-ttu-id="ea246-124">Hello SDK můžete nainstalovat z hello [Azure stáhne](https://azure.microsoft.com/downloads/) stránku hello jazyk, ve kterém chcete toodevelop kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="ea246-124">You can install hello SDK from hello [Azure Downloads](https://azure.microsoft.com/downloads/) page, in hello language in which you prefer toodevelop your application code.</span></span>

<span data-ttu-id="ea246-125">Funkce služby tři cloudu vyžadovat zvláštní konfigurace před exportem balíčku služby:</span><span class="sxs-lookup"><span data-stu-id="ea246-125">Three cloud service features require special configurations before you export a service package:</span></span>

* <span data-ttu-id="ea246-126">Pokud chcete, aby toodeploy Cloudová služba, která používá Secure Sockets Layer (SSL) pro šifrování dat [konfiguraci aplikace](cloud-services-configure-ssl-certificate-portal.md#modify) pro protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="ea246-126">If you want toodeploy a cloud service that uses Secure Sockets Layer (SSL) for data encryption, [configure your application](cloud-services-configure-ssl-certificate-portal.md#modify) for SSL.</span></span>
* <span data-ttu-id="ea246-127">Pokud chcete, aby tooconfigure instance toorole připojení vzdálené plochy, [konfiguraci rolí hello](cloud-services-role-enable-remote-desktop-new-portal.md) pro vzdálenou plochu.</span><span class="sxs-lookup"><span data-stu-id="ea246-127">If you want tooconfigure Remote Desktop connections toorole instances, [configure hello roles](cloud-services-role-enable-remote-desktop-new-portal.md) for Remote Desktop.</span></span>
* <span data-ttu-id="ea246-128">Pokud chcete tooconfigure podrobné monitorování pro cloudové služby, povolte pro hello Cloudová služba Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="ea246-128">If you want tooconfigure verbose monitoring for your cloud service, enable Azure Diagnostics for hello cloud service.</span></span> <span data-ttu-id="ea246-129">*Minimální monitorování* (hello výchozí sledování úrovně) použije čítače výkonu získané z hello hostitelských operačních systémech pro instance rolí (virtuální počítače).</span><span class="sxs-lookup"><span data-stu-id="ea246-129">*Minimal monitoring* (hello default monitoring level) uses performance counters gathered from hello host operating systems for role instances (virtual machines).</span></span> <span data-ttu-id="ea246-130">*Podrobné monitorování* shromažďuje další metriky na základě dat o výkonu v rámci hello role instance tooenable blíže analyzovat problémy, ke kterým došlo během zpracování aplikace.</span><span class="sxs-lookup"><span data-stu-id="ea246-130">*Verbose monitoring* gathers additional metrics based on performance data within hello role instances tooenable closer analysis of issues that occur during application processing.</span></span> <span data-ttu-id="ea246-131">v tématu tooenable Azure Diagnostics, toofind jak [povolení diagnostiky v Azure](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="ea246-131">toofind out how tooenable Azure Diagnostics, see [Enabling diagnostics in Azure](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="ea246-132">toocreate je Cloudová služba se nasazení webové role nebo rolí pracovního procesu, je nutné [vytvořit balíček služby hello](cloud-services-model-and-package.md#servicepackagecspkg).</span><span class="sxs-lookup"><span data-stu-id="ea246-132">toocreate a cloud service with deployments of web roles or worker roles, you must [create hello service package](cloud-services-model-and-package.md#servicepackagecspkg).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ea246-133">Než začnete</span><span class="sxs-lookup"><span data-stu-id="ea246-133">Before you begin</span></span>
* <span data-ttu-id="ea246-134">Pokud jste nenainstalovali hello Azure SDK, klikněte na tlačítko **instalovat sadu SDK Azure** tooopen hello [Azure stáhne stránky](https://azure.microsoft.com/downloads/)a potom stáhněte hello SDK pro jazyk hello, ve kterém chcete toodevelop vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="ea246-134">If you haven't installed hello Azure SDK, click **Install Azure SDK** tooopen hello [Azure Downloads page](https://azure.microsoft.com/downloads/), and then download hello SDK for hello language in which you prefer toodevelop your code.</span></span> <span data-ttu-id="ea246-135">(Budete mít toodo možnost to později.)</span><span class="sxs-lookup"><span data-stu-id="ea246-135">(You'll have an opportunity toodo this later.)</span></span>
* <span data-ttu-id="ea246-136">Pokud všechny instance role vyžadovat certifikát, vytvořte hello certifikáty.</span><span class="sxs-lookup"><span data-stu-id="ea246-136">If any role instances require a certificate, create hello certificates.</span></span> <span data-ttu-id="ea246-137">Cloudové služby vyžadují soubor .pfx s privátním klíčem.</span><span class="sxs-lookup"><span data-stu-id="ea246-137">Cloud services require a .pfx file with a private key.</span></span> <span data-ttu-id="ea246-138">Jak vytvořit a nasadit hello cloudové služby můžete nahrát tooAzure certifikáty hello.</span><span class="sxs-lookup"><span data-stu-id="ea246-138">You can upload hello certificates tooAzure as you create and deploy hello cloud service.</span></span>

## <a name="create-and-deploy"></a><span data-ttu-id="ea246-139">Vytvoření a nasazení</span><span class="sxs-lookup"><span data-stu-id="ea246-139">Create and deploy</span></span>
1. <span data-ttu-id="ea246-140">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ea246-140">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="ea246-141">Klikněte na tlačítko **nový > výpočetní**a potom přejděte dolů na klikněte na tlačítko tooand **cloudové služby**.</span><span class="sxs-lookup"><span data-stu-id="ea246-141">Click **New > Compute**, and then scroll down tooand click **Cloud Service**.</span></span>

    ![Publikování cloudové služby](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. <span data-ttu-id="ea246-143">V nové hello **Cloudová služba** okno, zadejte hodnotu pro hello **název DNS**.</span><span class="sxs-lookup"><span data-stu-id="ea246-143">In hello new **Cloud Service** blade, enter a value for hello **DNS name**.</span></span>
4. <span data-ttu-id="ea246-144">Vytvořte novou **skupiny prostředků** nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="ea246-144">Create a new **Resource Group** or select an existing one.</span></span>
5. <span data-ttu-id="ea246-145">Vyberte **Umístění**.</span><span class="sxs-lookup"><span data-stu-id="ea246-145">Select a **Location**.</span></span>
6. <span data-ttu-id="ea246-146">Klikněte na tlačítko **balíček**.</span><span class="sxs-lookup"><span data-stu-id="ea246-146">Click **Package**.</span></span> <span data-ttu-id="ea246-147">Otevře se hello **nahrání balíčku** okno.</span><span class="sxs-lookup"><span data-stu-id="ea246-147">This will open hello **Upload a package** blade.</span></span> <span data-ttu-id="ea246-148">Vyplňte hello požadované pole.</span><span class="sxs-lookup"><span data-stu-id="ea246-148">Fill in hello required fields.</span></span> <span data-ttu-id="ea246-149">Pokud některé z vaší rolí obsahuje jednu instanci, ujistěte se, **nasadit, i když jedna nebo víc rolí obsahuje jednu instanci** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="ea246-149">If any of your roles contain a single instance, ensure **Deploy even if one or more roles contain a single instance** is selected.</span></span>
7. <span data-ttu-id="ea246-150">Ujistěte se, že **spuštění nasazení** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="ea246-150">Make sure that **Start deployment** is selected.</span></span>
8. <span data-ttu-id="ea246-151">Klikněte na tlačítko **OK** kterého bude zavřena hello **nahrání balíčku** okno.</span><span class="sxs-lookup"><span data-stu-id="ea246-151">Click **OK** which will close hello **Upload a package** blade.</span></span>
9. <span data-ttu-id="ea246-152">Pokud nemáte žádné certifikáty tooadd, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ea246-152">If you do not have any certificates tooadd, click **Create**.</span></span>

    ![Publikování cloudové služby](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a><span data-ttu-id="ea246-154">Nahrání certifikátu</span><span class="sxs-lookup"><span data-stu-id="ea246-154">Upload a certificate</span></span>
<span data-ttu-id="ea246-155">Pokud byl balíček pro nasazení [nakonfigurované toouse certifikáty](cloud-services-configure-ssl-certificate-portal.md#modify), teď můžete nahrát certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="ea246-155">If your deployment package was [configured toouse certificates](cloud-services-configure-ssl-certificate-portal.md#modify), you can upload hello certificate now.</span></span>

1. <span data-ttu-id="ea246-156">Vyberte **certifikáty**a na hello **přidat certifikáty** okně, vyberte soubor .pfx certifikátu SSL hello a pak zadejte hello **heslo** hello certifikátu</span><span class="sxs-lookup"><span data-stu-id="ea246-156">Select **Certificates**, and on hello **Add certificates** blade, select hello SSL certificate .pfx file, and then provide hello **Password** for hello certificate,</span></span>
2. <span data-ttu-id="ea246-157">Klikněte na tlačítko **připojit certifikát**a potom klikněte na **OK** na hello **přidat certifikáty** okno.</span><span class="sxs-lookup"><span data-stu-id="ea246-157">Click **Attach certificate**, and then click **OK** on hello **Add certificates** blade.</span></span>
3. <span data-ttu-id="ea246-158">Klikněte na tlačítko **vytvořit** na hello **Cloudová služba** okno.</span><span class="sxs-lookup"><span data-stu-id="ea246-158">Click **Create** on hello **Cloud Service** blade.</span></span> <span data-ttu-id="ea246-159">Při nasazení hello dosáhl hello **připraven** stavu, abyste mohli pokračovat toohello další kroky.</span><span class="sxs-lookup"><span data-stu-id="ea246-159">When hello deployment has reached hello **Ready** status, you can proceed toohello next steps.</span></span>

    ![Publikování cloudové služby](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a><span data-ttu-id="ea246-161">Zkontrolujte vaše nasazení bylo úspěšně dokončeno</span><span class="sxs-lookup"><span data-stu-id="ea246-161">Verify your deployment completed successfully</span></span>
1. <span data-ttu-id="ea246-162">Klikněte na tlačítko instance hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="ea246-162">Click hello cloud service instance.</span></span>

    <span data-ttu-id="ea246-163">Hello stavu by měl zobrazit, zda je služba hello **systémem**.</span><span class="sxs-lookup"><span data-stu-id="ea246-163">hello status should show that hello service is **Running**.</span></span>
2. <span data-ttu-id="ea246-164">V části **Essentials**, klikněte na tlačítko hello **adresa URL webu** tooopen vaše cloudové služby ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ea246-164">Under **Essentials**, click hello **Site URL** tooopen your cloud service in a web browser.</span></span>

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a><span data-ttu-id="ea246-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ea246-166">Next steps</span></span>
* <span data-ttu-id="ea246-167">[Obecná konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ea246-167">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="ea246-168">Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ea246-168">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="ea246-169">[Správa služby cloud](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ea246-169">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
* <span data-ttu-id="ea246-170">Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ea246-170">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
