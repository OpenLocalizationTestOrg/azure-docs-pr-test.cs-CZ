---
title: "Koupit vlastní název domény pro Azure Web Apps"
description: "Zjistěte, jak koupit vlastní doménu s webovou aplikaci v Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: 3cb22b935624041ab51e64028a1b668fd694f9b5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="buy-a-custom-domain-name-for-azure-web-apps"></a><span data-ttu-id="1f26b-103">Koupit vlastní název domény pro Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="1f26b-103">Buy a custom domain name for Azure Web Apps</span></span>

<span data-ttu-id="1f26b-104">Domény nejvyšší úrovně, které jsou spravovány přímo v Azure jsou domény služby App Service (preview).</span><span class="sxs-lookup"><span data-stu-id="1f26b-104">App Service domains (preview) are top-level domains that are managed directly in Azure.</span></span> <span data-ttu-id="1f26b-105">Se snadno spravovat vlastní domény pro [Azure Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1f26b-105">They make it easy to manage custom domains for [Azure Web Apps](app-service-web-overview.md).</span></span> <span data-ttu-id="1f26b-106">V tomto kurzu se dozvíte, jak zakoupit domény služby App Service a názvy DNS přiřadit Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="1f26b-106">This tutorial shows you how to buy an App Service domain and assign DNS names to Azure Web Apps.</span></span>

<span data-ttu-id="1f26b-107">Tento článek je pro službu Azure App Service (webové aplikace, aplikace API, mobilní aplikace, Logic Apps).</span><span class="sxs-lookup"><span data-stu-id="1f26b-107">This article is for Azure App Service (Web Apps, API Apps, Mobile Apps, Logic Apps).</span></span> <span data-ttu-id="1f26b-108">Virtuální počítač Azure nebo Azure Storage, najdete v části [domény přiřadit aplikační služby na virtuálním počítači Azure nebo Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/).</span><span class="sxs-lookup"><span data-stu-id="1f26b-108">For Azure VM or Azure Storage, see [Assign App Service domain to Azure VM or Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/).</span></span> <span data-ttu-id="1f26b-109">Cloudové služby, najdete v části [konfigurace vlastního názvu domény pro cloudové služby Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1f26b-109">For Cloud Services, see [Configuring a custom domain name for an Azure cloud service](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f26b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1f26b-110">Prerequisites</span></span>

<span data-ttu-id="1f26b-111">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="1f26b-111">To complete this tutorial:</span></span>

* <span data-ttu-id="1f26b-112">[Vytvořit aplikaci služby App Service](/azure/app-service/), nebo použijte aplikaci, kterou jste vytvořili pro jiné kurzu.</span><span class="sxs-lookup"><span data-stu-id="1f26b-112">[Create an App Service app](/azure/app-service/), or use an app that you created for another tutorial.</span></span>

## <a name="prepare-the-app"></a><span data-ttu-id="1f26b-113">Příprava aplikace</span><span class="sxs-lookup"><span data-stu-id="1f26b-113">Prepare the app</span></span>

<span data-ttu-id="1f26b-114">Použít vlastní domény ve službě Azure Web Apps, webové aplikace na [plán služby App Service](https://azure.microsoft.com/pricing/details/app-service/) musí být placené vrstvy (**sdílené**, **základní**, **standardní**, nebo  **Premium**).</span><span class="sxs-lookup"><span data-stu-id="1f26b-114">To use custom domains in Azure Web Apps, your web app's [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be a paid tier (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> <span data-ttu-id="1f26b-115">V tomto kroku je třeba zkontrolovat, že webová aplikace je v podporovaném cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="1f26b-115">In this step, you make sure that the web app is in the supported pricing tier.</span></span>

### <a name="sign-in-to-azure"></a><span data-ttu-id="1f26b-116">Přihlášení k Azure</span><span class="sxs-lookup"><span data-stu-id="1f26b-116">Sign in to Azure</span></span>

<span data-ttu-id="1f26b-117">Otevřete [portál Azure](https://portal.azure.com) a přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="1f26b-117">Open the [Azure portal](https://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="navigate-to-the-app-in-the-azure-portal"></a><span data-ttu-id="1f26b-118">Přejděte na aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1f26b-118">Navigate to the app in the Azure portal</span></span>

<span data-ttu-id="1f26b-119">V nabídce vlevo vyberte **App Services**a pak vyberte název aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f26b-119">From the left menu, select **App Services**, and then select the name of the app.</span></span>

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/select-app.png)

<span data-ttu-id="1f26b-121">Zobrazí na stránce Správa aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="1f26b-121">You see the management page of the App Service app.</span></span>  

### <a name="check-the-pricing-tier"></a><span data-ttu-id="1f26b-122">Zkontrolujte cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="1f26b-122">Check the pricing tier</span></span>

<span data-ttu-id="1f26b-123">V levém navigačním panelu stránku aplikace, přejděte na **nastavení** a vyberte **škálování (plán služby App Service)**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-123">In the left navigation of the app page, scroll to the **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Škálování nabídky](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

<span data-ttu-id="1f26b-125">Aktuální úroveň aplikace je označený modré ohraničení.</span><span class="sxs-lookup"><span data-stu-id="1f26b-125">The app's current tier is highlighted by a blue border.</span></span> <span data-ttu-id="1f26b-126">Zkontrolujte, zda není v aplikaci **volné** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="1f26b-126">Check to make sure that the app is not in the **Free** tier.</span></span> <span data-ttu-id="1f26b-127">Vlastní DNS není podporována v **volné** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="1f26b-127">Custom DNS is not supported in the **Free** tier.</span></span> 

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

<span data-ttu-id="1f26b-129">Pokud plán služby App Service není **volné**ukončit **zvolte cenovou úroveň** stránky a přejít na [koupit domény](#buy-the-domain).</span><span class="sxs-lookup"><span data-stu-id="1f26b-129">If the App Service plan is not **Free**, close the **Choose your pricing tier** page and skip to [Buy the domain](#buy-the-domain).</span></span>

### <a name="scale-up-the-app-service-plan"></a><span data-ttu-id="1f26b-130">Škálovat plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="1f26b-130">Scale up the App Service plan</span></span>

<span data-ttu-id="1f26b-131">Vyberte některé z vrstvy a bezplatnou (**sdílené**, **základní**, **standardní**, nebo **Premium**).</span><span class="sxs-lookup"><span data-stu-id="1f26b-131">Select any of the non-free tiers (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> 

<span data-ttu-id="1f26b-132">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-132">Click **Select**.</span></span>

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

<span data-ttu-id="1f26b-134">Když se zobrazí následující oznámení, je dokončena operace škálování.</span><span class="sxs-lookup"><span data-stu-id="1f26b-134">When you see the following notification, the scale operation is complete.</span></span>

![Potvrzení operace škálování](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-the-domain"></a><span data-ttu-id="1f26b-136">Kupte si domény</span><span class="sxs-lookup"><span data-stu-id="1f26b-136">Buy the domain</span></span>

### <a name="sign-in-to-azure"></a><span data-ttu-id="1f26b-137">Přihlášení k Azure</span><span class="sxs-lookup"><span data-stu-id="1f26b-137">Sign in to Azure</span></span>
<span data-ttu-id="1f26b-138">Otevřete [portál Azure](https://portal.azure.com/) a přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="1f26b-138">Open the [Azure portal](https://portal.azure.com/) and sign in with your Azure account.</span></span>

### <a name="launch-buy-domains"></a><span data-ttu-id="1f26b-139">Spusťte koupit domén</span><span class="sxs-lookup"><span data-stu-id="1f26b-139">Launch Buy domains</span></span>
<span data-ttu-id="1f26b-140">V **webové aplikace** , klikněte na název vaší webové aplikaci, vyberte **nastavení**a potom vyberte **vlastní domény**</span><span class="sxs-lookup"><span data-stu-id="1f26b-140">In the **Web Apps** tab, click the name of your web app, select **Settings**, and then select **Custom domains**</span></span>
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

<span data-ttu-id="1f26b-141">V **vlastní domény** klikněte na tlačítko **koupit domény**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-141">In the **Custom domains** page, click **Buy domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

### <a name="configure-the-domain-purchase"></a><span data-ttu-id="1f26b-142">Konfigurace domény nákupu</span><span class="sxs-lookup"><span data-stu-id="1f26b-142">Configure the domain purchase</span></span>

<span data-ttu-id="1f26b-143">V **aplikace služby domény** stránky v **hledat domény** pole, zadejte název domény, které chcete koupit a zadejte `Enter`.</span><span class="sxs-lookup"><span data-stu-id="1f26b-143">In the **App Service Domain** page, in the **Search for domain** box, type the domain name you want to buy and type `Enter`.</span></span> <span data-ttu-id="1f26b-144">Navržené domény k dispozici se zobrazí pod textové pole.</span><span class="sxs-lookup"><span data-stu-id="1f26b-144">The suggested available domains are shown just below the text box.</span></span> <span data-ttu-id="1f26b-145">Vyberte jednu nebo více domén, které chcete koupit.</span><span class="sxs-lookup"><span data-stu-id="1f26b-145">Select one or more domains you want to buy.</span></span> 
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

<span data-ttu-id="1f26b-146">Klikněte **kontaktní údaje** a vyplňte formulář domény kontaktní informace.</span><span class="sxs-lookup"><span data-stu-id="1f26b-146">Click the **Contact Information** and fill out the domain's contact information form.</span></span> <span data-ttu-id="1f26b-147">Po dokončení klikněte na tlačítko **OK** vrátit na stránku služby domény aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f26b-147">When finished, click **OK** to return to the App Service Domain page.</span></span>
   
<span data-ttu-id="1f26b-148">Je důležité, vyplňte všechna povinná pole s tolik přesnost míře.</span><span class="sxs-lookup"><span data-stu-id="1f26b-148">It is important that you fill out all required fields with as much accuracy as possible.</span></span> <span data-ttu-id="1f26b-149">Nesprávná data kontaktní informace může způsobit selhání přikoupení domén.</span><span class="sxs-lookup"><span data-stu-id="1f26b-149">Incorrect data for contact information can result in failure to purchase domains.</span></span> 

<span data-ttu-id="1f26b-150">Potom vyberte požadované možnosti pro vaši doménu.</span><span class="sxs-lookup"><span data-stu-id="1f26b-150">Next, select the desired options for your domain.</span></span> <span data-ttu-id="1f26b-151">V následující tabulce najdete vysvětlení:</span><span class="sxs-lookup"><span data-stu-id="1f26b-151">See the following table for explanations:</span></span>

| <span data-ttu-id="1f26b-152">Nastavení</span><span class="sxs-lookup"><span data-stu-id="1f26b-152">Setting</span></span> | <span data-ttu-id="1f26b-153">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="1f26b-153">Suggested Value</span></span> | <span data-ttu-id="1f26b-154">Popis</span><span class="sxs-lookup"><span data-stu-id="1f26b-154">Description</span></span> |
|-|-|-|
|<span data-ttu-id="1f26b-155">Automatické obnovení</span><span class="sxs-lookup"><span data-stu-id="1f26b-155">Auto renew</span></span> | <span data-ttu-id="1f26b-156">**Povolení**</span><span class="sxs-lookup"><span data-stu-id="1f26b-156">**Enable**</span></span> | <span data-ttu-id="1f26b-157">Obnovuje doménu služby aplikace automaticky každý rok.</span><span class="sxs-lookup"><span data-stu-id="1f26b-157">Renews your App Service Domain automatically every year.</span></span> <span data-ttu-id="1f26b-158">Platební karty je účtován za stejnou cenu nákupu v době obnovení.</span><span class="sxs-lookup"><span data-stu-id="1f26b-158">Your credit card is charged the same purchase price at the time of renewal.</span></span> |
|<span data-ttu-id="1f26b-159">Ochrana osobních údajů</span><span class="sxs-lookup"><span data-stu-id="1f26b-159">Privacy protection</span></span> | <span data-ttu-id="1f26b-160">Povolení</span><span class="sxs-lookup"><span data-stu-id="1f26b-160">Enable</span></span> | <span data-ttu-id="1f26b-161">Vyjádřit výslovný souhlas pro "Ochrany osobních údajů", který je součástí kupní ceny _zdarma_ (s výjimkou domény nejvyšší úrovně, jejichž registru nepodporují ochrany osobních údajů, jako například _. co.in_, _. co.uk_ a tak dále).</span><span class="sxs-lookup"><span data-stu-id="1f26b-161">Opt in to "Privacy protection", which is included in the purchase price _for free_ (except for top-level domains whose registry do not support privacy protection, such as _.co.in_, _.co.uk_, and so on).</span></span> |
| <span data-ttu-id="1f26b-162">Přiřadit výchozí názvy hostitelů</span><span class="sxs-lookup"><span data-stu-id="1f26b-162">Assign default hostnames</span></span> | <span data-ttu-id="1f26b-163">**Webová** a**@**</span><span class="sxs-lookup"><span data-stu-id="1f26b-163">**www** and **@**</span></span> | <span data-ttu-id="1f26b-164">Vazby požadovaným názvem hostitele, vyberte v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="1f26b-164">Select the desired hostname bindings, if desired.</span></span> <span data-ttu-id="1f26b-165">Po dokončení nákupu operace domény vaší webové aplikace jsou přístupné na vybrané názvy hostitelů.</span><span class="sxs-lookup"><span data-stu-id="1f26b-165">When the domain purchase operation is complete, your web app can be accessed at the selected hostnames.</span></span> <span data-ttu-id="1f26b-166">Pokud webová aplikace je za [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), nevidíte možnost k přidělení kořenové domény (@), protože nemá podporu záznamů A Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="1f26b-166">If the web app is behind [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), you don't see the option to assign the root domain (@), because Traffic Manager does not support A records.</span></span> <span data-ttu-id="1f26b-167">Vám po dokončení nákupu domény provádět změny přiřazení názvu hostitele.</span><span class="sxs-lookup"><span data-stu-id="1f26b-167">You can make changes to the hostname assignments after the domain purchase completes.</span></span> |

### <a name="accept-terms-and-purchase"></a><span data-ttu-id="1f26b-168">Přijmout podmínky a nákupu</span><span class="sxs-lookup"><span data-stu-id="1f26b-168">Accept terms and purchase</span></span>

<span data-ttu-id="1f26b-169">Klikněte na tlačítko **právní podmínky** přečtěte si podmínky a že poplatky a pak klikněte na **koupit**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-169">Click **Legal Terms** to review the terms and the charges, then click **Buy**.</span></span>

> [!NOTE]
> <span data-ttu-id="1f26b-170">Aplikační služby domény použít Azure DNS k hostování domény.</span><span class="sxs-lookup"><span data-stu-id="1f26b-170">App Service Domains use Azure DNS to host the domains.</span></span> <span data-ttu-id="1f26b-171">Kromě poplatek za zápis domain poplatky za používání pro Azure DNS použít.</span><span class="sxs-lookup"><span data-stu-id="1f26b-171">In addition to the domain registration fee, usage charges for Azure DNS apply.</span></span> <span data-ttu-id="1f26b-172">Informace najdete v tématu [Azure DNS ceny](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="1f26b-172">For information, see [Azure DNS Pricing](https://azure.microsoft.com/pricing/details/dns/).</span></span>
>
>

<span data-ttu-id="1f26b-173">Zpět v **aplikace služby domény** klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-173">Back in the **App Service Domain** page, click **OK**.</span></span> <span data-ttu-id="1f26b-174">Když probíhá operace, zobrazí se následující oznámení:</span><span class="sxs-lookup"><span data-stu-id="1f26b-174">While the operation is in progress, you see the following notifications:</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-the-hostnames"></a><span data-ttu-id="1f26b-175">Testování názvy hostitelů</span><span class="sxs-lookup"><span data-stu-id="1f26b-175">Test the hostnames</span></span>

<span data-ttu-id="1f26b-176">Pokud jste přiřadili výchozí názvy hostitelů do vaší webové aplikace, je také zobrazit upozornění na úspěch pro každý vybraný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="1f26b-176">If you have assigned default hostnames to your web app, you also see a success notification for each selected hostname.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

<span data-ttu-id="1f26b-177">Zobrazí vybraný názvy hostitelů v **vlastní domény** stránky v **názvy hostitelů** části.</span><span class="sxs-lookup"><span data-stu-id="1f26b-177">You also see the selected hostnames in the **Custom domains** page, in the **Hostnames** section.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

<span data-ttu-id="1f26b-178">Chcete-li otestovat názvy hostitelů, přejděte na uvedené názvy hostitelů v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="1f26b-178">To test the hostnames, navigate to the listed hostnames in the browser.</span></span> <span data-ttu-id="1f26b-179">V příkladu v předchozí snímek obrazovky, zkuste přejdete na _kontoso.net_ a _www.kontoso.net_.</span><span class="sxs-lookup"><span data-stu-id="1f26b-179">In the example in the preceding screenshot, try navigating to _kontoso.net_ and _www.kontoso.net_.</span></span>

## <a name="assign-hostnames-to-web-app"></a><span data-ttu-id="1f26b-180">Přiřadit názvy hostitelů do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="1f26b-180">Assign hostnames to web app</span></span>

<span data-ttu-id="1f26b-181">Pokud se rozhodnete přiřadit jeden nebo více názvy hostitelů výchozí webové aplikace během procesu nákupu, nebo pokud je třeba přiřadit název hostitele není uvedený, můžete kdykoli přiřadit název hostitele v.</span><span class="sxs-lookup"><span data-stu-id="1f26b-181">If you choose not to assign one or more default hostnames to your web app during the purchase process, or if you need to assign a hostname not listed, you can assign a hostname at anytime.</span></span>

<span data-ttu-id="1f26b-182">Názvy hostitelů v doméně služby aplikaci můžete přiřadit také do jiné webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f26b-182">You can also assign hostnames in the App Service Domain to any other web app.</span></span> <span data-ttu-id="1f26b-183">Postup závisí na tom, jestli domény služby, aplikace a webové aplikace patří do stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="1f26b-183">The steps depend on whether the App Service Domain and the web app belong to the same subscription.</span></span>

- <span data-ttu-id="1f26b-184">Jiné předplatné: namapovat vlastní záznamy DNS od domény služby aplikace do webové aplikace jako externě zakoupené domény.</span><span class="sxs-lookup"><span data-stu-id="1f26b-184">Different subscription: Map custom DNS records from the App Service Domain to the web app like an externally purchased domain.</span></span> <span data-ttu-id="1f26b-185">Informace o přidání vlastní názvy DNS do domény služby App Service najdete v tématu [vlastní záznamy DNS spravovat](#custom).</span><span class="sxs-lookup"><span data-stu-id="1f26b-185">For information on adding custom DNS names to an App Service Domain, see [Manage custom DNS records](#custom).</span></span> <span data-ttu-id="1f26b-186">Mapování externí zakoupené domény do webové aplikace naleznete v tématu [mapovat existující vlastní název DNS pro službu Azure Web Apps](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="1f26b-186">To map an external purchased domain to a web app, see [Map an existing custom DNS name to Azure Web Apps](app-service-web-tutorial-custom-domain.md).</span></span> 
- <span data-ttu-id="1f26b-187">Stejného předplatného: použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="1f26b-187">Same subscription: Use the following steps.</span></span>

### <a name="launch-add-hostname"></a><span data-ttu-id="1f26b-188">Spuštění přidat název hostitele</span><span class="sxs-lookup"><span data-stu-id="1f26b-188">Launch add hostname</span></span>
<span data-ttu-id="1f26b-189">V **App Services** vyberte název vaší webové aplikace, kterou chcete přiřadit názvů hostitelů, vyberte **nastavení**a potom vyberte **vlastní domény**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-189">In the **App Services** page, select the name of your web app that you want to assign hostnames to, select **Settings**, and then select **Custom domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

<span data-ttu-id="1f26b-190">Ujistěte se, že zakoupené domény, je uvedena ve **doménami aplikací služby** tématu, ale nezaškrtnete políčko.</span><span class="sxs-lookup"><span data-stu-id="1f26b-190">Make sure that your purchased domain is listed in the **App Service Domains** section, but don't select it.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> <span data-ttu-id="1f26b-191">Všechny aplikace služby domény v rámci stejného předplatného jsou uvedeny ve webové aplikaci **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="1f26b-191">All App Service Domains in the same subscription are shown in the web app's **Custom domains** page.</span></span> <span data-ttu-id="1f26b-192">Pokud vaše doména je v rámci předplatného webové aplikace, ale nejde ji zobrazit ve webové aplikaci **vlastní domény** stránky, zkuste to znovu **vlastní domény** stránky nebo obnovení webové stránky.</span><span class="sxs-lookup"><span data-stu-id="1f26b-192">If your domain is in the web app's subscription, but you cannot see it in the web app's **Custom domains** page, try reopening the **Custom domains** page or refresh the webpage.</span></span> <span data-ttu-id="1f26b-193">Zkontrolujte také, oznámení zvonku v horní části portálu Azure průběh nebo vytvoření chyby.</span><span class="sxs-lookup"><span data-stu-id="1f26b-193">Also, check the notification bell at the top of the Azure portal for progress or creation failures.</span></span>
>
>

<span data-ttu-id="1f26b-194">Vyberte **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-194">Select **Add hostname**.</span></span>

### <a name="configure-hostname"></a><span data-ttu-id="1f26b-195">Nakonfigurujte název hostitele</span><span class="sxs-lookup"><span data-stu-id="1f26b-195">Configure hostname</span></span>
<span data-ttu-id="1f26b-196">V **přidat název hostitele** dialogové okno, zadejte plně kvalifikovaný název domény vaší domény služby aplikace nebo jakékoli subdomény.</span><span class="sxs-lookup"><span data-stu-id="1f26b-196">In the **Add hostname** dialog, type the fully qualified domain name of your App Service Domain or any subdomain.</span></span> <span data-ttu-id="1f26b-197">Například:</span><span class="sxs-lookup"><span data-stu-id="1f26b-197">For example:</span></span>

- <span data-ttu-id="1f26b-198">kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="1f26b-198">kontoso.net</span></span>
- <span data-ttu-id="1f26b-199">www.kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="1f26b-199">www.kontoso.net</span></span>
- <span data-ttu-id="1f26b-200">ABC.kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="1f26b-200">abc.kontoso.net</span></span>

<span data-ttu-id="1f26b-201">Po dokončení vyberte **ověřením**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-201">When finished, select **Validate**.</span></span> <span data-ttu-id="1f26b-202">Typ záznamu název hostitele je automaticky vybrána pro vás.</span><span class="sxs-lookup"><span data-stu-id="1f26b-202">The hostname record type is automatically selected for you.</span></span>

<span data-ttu-id="1f26b-203">Vyberte **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-203">Select **Add hostname**.</span></span>

<span data-ttu-id="1f26b-204">Po dokončení operace se zobrazí upozornění na úspěch přiřazené název hostitele.</span><span class="sxs-lookup"><span data-stu-id="1f26b-204">When the operation is complete, you see a success notification for the assigned hostname.</span></span>  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a><span data-ttu-id="1f26b-205">Zavřete přidat název hostitele</span><span class="sxs-lookup"><span data-stu-id="1f26b-205">Close add hostname</span></span>
<span data-ttu-id="1f26b-206">V **přidat název hostitele** přiřaďte jiné název hostitele do webové aplikace, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="1f26b-206">In the **Add hostname** page, assign any other hostname to your web app, as desired.</span></span> <span data-ttu-id="1f26b-207">Po dokončení zavřete **přidat název hostitele** stránky.</span><span class="sxs-lookup"><span data-stu-id="1f26b-207">When finished, close the **Add hostname** page.</span></span>

<span data-ttu-id="1f26b-208">Teď byste měli vidět nově přiřazené hostname(s) ve vaší aplikaci **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="1f26b-208">You should now see the newly assigned hostname(s) in your app's **Custom domains** page.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-the-hostnames"></a><span data-ttu-id="1f26b-209">Testování názvy hostitelů</span><span class="sxs-lookup"><span data-stu-id="1f26b-209">Test the hostnames</span></span>

<span data-ttu-id="1f26b-210">Přejděte do seznamu názvy hostitelů v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="1f26b-210">Navigate to the listed hostnames in the browser.</span></span> <span data-ttu-id="1f26b-211">V příkladu v předchozí snímek obrazovky, zkuste přejdete na _abc.kontoso.net_.</span><span class="sxs-lookup"><span data-stu-id="1f26b-211">In the example in the preceding screenshot, try navigating to _abc.kontoso.net_.</span></span>

<a name="custom" />

## <a name="manage-custom-dns-records"></a><span data-ttu-id="1f26b-212">Spravovat vlastní záznamy DNS</span><span class="sxs-lookup"><span data-stu-id="1f26b-212">Manage custom DNS records</span></span>

<span data-ttu-id="1f26b-213">V Azure, záznamy DNS pro doménu služby App Service se spravují pomocí [Azure DNS](https://azure.microsoft.com/services/dns/).</span><span class="sxs-lookup"><span data-stu-id="1f26b-213">In Azure, DNS records for an App Service Domain are managed using [Azure DNS](https://azure.microsoft.com/services/dns/).</span></span> <span data-ttu-id="1f26b-214">Můžete přidat, odebrat a aktualizovat záznamy DNS, stejně jako pro externě zakoupené doménu.</span><span class="sxs-lookup"><span data-stu-id="1f26b-214">You can add, remove, and update DNS records, just like for an externally purchased domain.</span></span>

### <a name="open-app-service-domain"></a><span data-ttu-id="1f26b-215">Otevřete aplikaci služby domény</span><span class="sxs-lookup"><span data-stu-id="1f26b-215">Open App Service Domain</span></span>

<span data-ttu-id="1f26b-216">Na portálu Azure v levé nabídce vyberte **více služeb** > **doménami aplikací služby**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-216">In the Azure portal, from the left menu, select **More Services** > **App Service Domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

<span data-ttu-id="1f26b-217">Vyberte doménu pro správu.</span><span class="sxs-lookup"><span data-stu-id="1f26b-217">Select the domain to manage.</span></span> 

### <a name="access-dns-zone"></a><span data-ttu-id="1f26b-218">Zóna DNS přístup</span><span class="sxs-lookup"><span data-stu-id="1f26b-218">Access DNS zone</span></span>

<span data-ttu-id="1f26b-219">V levé nabídce domény, vyberte **zónu DNS**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-219">In the domain's left menu, select **DNS zone**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

<span data-ttu-id="1f26b-220">Tato akce otevře [zónu DNS](../dns/dns-zones-records.md) stránku vaší domény služby aplikace v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="1f26b-220">This action opens the [DNS zone](../dns/dns-zones-records.md) page of your App Service Domain in Azure DNS.</span></span> <span data-ttu-id="1f26b-221">Informace o tom, jak upravit záznamy DNS najdete v tématu [správa zóny DNS na webu Azure portal](../dns/dns-operations-dnszones-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1f26b-221">For information on how to edit DNS records, see [How to manage DNS Zones in the Azure portal](../dns/dns-operations-dnszones-portal.md).</span></span>

## <a name="cancel-purchase-delete-domain"></a><span data-ttu-id="1f26b-222">Zrušit nákup (odstranit domény)</span><span class="sxs-lookup"><span data-stu-id="1f26b-222">Cancel purchase (delete domain)</span></span>

<span data-ttu-id="1f26b-223">Po zakoupení služby doména aplikace máte zrušit nákup pro vrátit celou částku pět dní.</span><span class="sxs-lookup"><span data-stu-id="1f26b-223">After you purchase the App Service Domain, you have five days to cancel your purchase for a full refund.</span></span> <span data-ttu-id="1f26b-224">Po pět dní můžete odstranit doménu služby aplikace ale nemůže přijímat náhrada.</span><span class="sxs-lookup"><span data-stu-id="1f26b-224">After five days, you can delete the App Service Domain, but cannot receive a refund.</span></span>

### <a name="open-app-service-domain"></a><span data-ttu-id="1f26b-225">Otevřete aplikaci služby domény</span><span class="sxs-lookup"><span data-stu-id="1f26b-225">Open App Service Domain</span></span>

<span data-ttu-id="1f26b-226">Na portálu Azure v levé nabídce vyberte **více služeb** > **doménami aplikací služby**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-226">In the Azure portal, from the left menu, select **More Services** > **App Service Domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

<span data-ttu-id="1f26b-227">Vyberte domény, ke které chcete zrušit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="1f26b-227">Select the domain to you want to cancel or delete.</span></span> 

### <a name="delete-hostname-bindings"></a><span data-ttu-id="1f26b-228">Odstranit vazby názvů hostitelů.</span><span class="sxs-lookup"><span data-stu-id="1f26b-228">Delete hostname bindings</span></span>

<span data-ttu-id="1f26b-229">V levé nabídce domény, vyberte **vazby názvů hostitelů**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-229">In the domain's left menu, select **Hostname bindings**.</span></span> <span data-ttu-id="1f26b-230">Vazby názvů hostitelů ze všech služeb Azure jsou zde uvedeny.</span><span class="sxs-lookup"><span data-stu-id="1f26b-230">The hostname bindings from all Azure services are listed here.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

<span data-ttu-id="1f26b-231">Doména aplikace služby nelze odstranit, dokud nejsou odstraněny všechny vazby názvů hostitelů.</span><span class="sxs-lookup"><span data-stu-id="1f26b-231">You cannot delete the App Service Domain until all hostname bindings are deleted.</span></span>

<span data-ttu-id="1f26b-232">Odstranit vazbu každý název hostitele tak, že vyberete **...**   >  **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-232">Delete each hostname binding by selecting **...** > **Delete**.</span></span> <span data-ttu-id="1f26b-233">Po odstranění se všechny vazby, vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-233">After all the bindings are deleted, select **Save**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a><span data-ttu-id="1f26b-234">Zrušit nebo odstranění</span><span class="sxs-lookup"><span data-stu-id="1f26b-234">Cancel or delete</span></span>

<span data-ttu-id="1f26b-235">V levé nabídce domény, vyberte **přehled**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-235">In the domain's left menu, select **Overview**.</span></span> 

<span data-ttu-id="1f26b-236">Pokud nebyl uplynutí období zrušení na zakoupené doménu, vyberte **zrušit nákup**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-236">If the cancellation period on the purchased domain has not elapsed, select **Cancel purchase**.</span></span> <span data-ttu-id="1f26b-237">Jinak se zobrazí **odstranit** tlačítko místo.</span><span class="sxs-lookup"><span data-stu-id="1f26b-237">Otherwise, you see a **Delete** button instead.</span></span> <span data-ttu-id="1f26b-238">Chcete-li odstranit doménu bez náhrady, vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="1f26b-238">To delete the domain without a refund, select **Delete**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

<span data-ttu-id="1f26b-239">Vyberte **OK** k potvrzení operace.</span><span class="sxs-lookup"><span data-stu-id="1f26b-239">Select **OK** to confirm the operation.</span></span> <span data-ttu-id="1f26b-240">Pokud nechcete, aby bylo možné pokračovat, klikněte kamkoli mimo dialogové okno pro potvrzení.</span><span class="sxs-lookup"><span data-stu-id="1f26b-240">If you don't want to proceed, click anywhere outside of the confirmation dialog.</span></span>

<span data-ttu-id="1f26b-241">Po dokončení operace doména je vydaná ze svého předplatného a k dispozici pro každý, kdo k nákupu znovu.</span><span class="sxs-lookup"><span data-stu-id="1f26b-241">After the operation is complete, the domain is released from your subscription and available for anyone to purchase again.</span></span> 
