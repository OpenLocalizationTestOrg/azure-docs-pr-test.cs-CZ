---
title: "aaaBuy vlastní název domény pro Azure Web Apps"
description: "Zjistěte, jak toobuy vlastní doménu, název s webovou aplikaci v Azure App Service."
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
ms.openlocfilehash: 2ff61a3f27020516c917fe105ece99eb2a5754b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="buy-a-custom-domain-name-for-azure-web-apps"></a><span data-ttu-id="a5efb-103">Koupit vlastní název domény pro Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="a5efb-103">Buy a custom domain name for Azure Web Apps</span></span>

<span data-ttu-id="a5efb-104">Domény nejvyšší úrovně, které jsou spravovány přímo v Azure jsou domény služby App Service (preview).</span><span class="sxs-lookup"><span data-stu-id="a5efb-104">App Service domains (preview) are top-level domains that are managed directly in Azure.</span></span> <span data-ttu-id="a5efb-105">Provádění ho snadno toomanage vlastní domény [Azure Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a5efb-105">They make it easy toomanage custom domains for [Azure Web Apps](app-service-web-overview.md).</span></span> <span data-ttu-id="a5efb-106">Tento kurz ukazuje, jak toobuy domény služby App Service a přiřadit DNS názvy tooAzure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a5efb-106">This tutorial shows you how toobuy an App Service domain and assign DNS names tooAzure Web Apps.</span></span>

<span data-ttu-id="a5efb-107">Tento článek je pro službu Azure App Service (webové aplikace, aplikace API, mobilní aplikace, Logic Apps).</span><span class="sxs-lookup"><span data-stu-id="a5efb-107">This article is for Azure App Service (Web Apps, API Apps, Mobile Apps, Logic Apps).</span></span> <span data-ttu-id="a5efb-108">Virtuální počítač Azure nebo Azure Storage, najdete v části [přiřadit App Service domény tooAzure virtuálních počítačů nebo úložiště Azure](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/).</span><span class="sxs-lookup"><span data-stu-id="a5efb-108">For Azure VM or Azure Storage, see [Assign App Service domain tooAzure VM or Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/).</span></span> <span data-ttu-id="a5efb-109">Cloudové služby, najdete v části [konfigurace vlastního názvu domény pro cloudové služby Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a5efb-109">For Cloud Services, see [Configuring a custom domain name for an Azure cloud service](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a5efb-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a5efb-110">Prerequisites</span></span>

<span data-ttu-id="a5efb-111">toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="a5efb-111">toocomplete this tutorial:</span></span>

* <span data-ttu-id="a5efb-112">[Vytvořit aplikaci služby App Service](/azure/app-service/), nebo použijte aplikaci, kterou jste vytvořili pro jiné kurzu.</span><span class="sxs-lookup"><span data-stu-id="a5efb-112">[Create an App Service app](/azure/app-service/), or use an app that you created for another tutorial.</span></span>

## <a name="prepare-hello-app"></a><span data-ttu-id="a5efb-113">Příprava aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a5efb-113">Prepare hello app</span></span>

<span data-ttu-id="a5efb-114">toouse vlastní domény ve službě Azure Web Apps, webové aplikace na [plán služby App Service](https://azure.microsoft.com/pricing/details/app-service/) musí být placené vrstvy (**sdílené**, **základní**, **standardní**, nebo **Premium**).</span><span class="sxs-lookup"><span data-stu-id="a5efb-114">toouse custom domains in Azure Web Apps, your web app's [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be a paid tier (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> <span data-ttu-id="a5efb-115">V tomto kroku budete zajistěte, aby že tento hello webové aplikace v hello podporovaná cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="a5efb-115">In this step, you make sure that hello web app is in hello supported pricing tier.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="a5efb-116">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="a5efb-116">Sign in tooAzure</span></span>

<span data-ttu-id="a5efb-117">Otevřete hello [portál Azure](https://portal.azure.com) a přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="a5efb-117">Open hello [Azure portal](https://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="navigate-toohello-app-in-hello-azure-portal"></a><span data-ttu-id="a5efb-118">Přejděte toohello aplikace v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a5efb-118">Navigate toohello app in hello Azure portal</span></span>

<span data-ttu-id="a5efb-119">Hello levé nabídce vyberte **App Services**a pak vyberte název hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a5efb-119">From hello left menu, select **App Services**, and then select hello name of hello app.</span></span>

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/select-app.png)

<span data-ttu-id="a5efb-121">Zobrazí stránku hello správu hello aplikaci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="a5efb-121">You see hello management page of hello App Service app.</span></span>  

### <a name="check-hello-pricing-tier"></a><span data-ttu-id="a5efb-122">Zkontrolujte hello cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="a5efb-122">Check hello pricing tier</span></span>

<span data-ttu-id="a5efb-123">V levé navigační stránku hello aplikace hello, posuňte toohello **nastavení** a vyberte **škálování (plán služby App Service)**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-123">In hello left navigation of hello app page, scroll toohello **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Škálování nabídky](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

<span data-ttu-id="a5efb-125">aktuální úroveň aplikace Hello je označený modré ohraničení.</span><span class="sxs-lookup"><span data-stu-id="a5efb-125">hello app's current tier is highlighted by a blue border.</span></span> <span data-ttu-id="a5efb-126">Zkontrolujte toomake se, že aplikace hello není hello **volné** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="a5efb-126">Check toomake sure that hello app is not in hello **Free** tier.</span></span> <span data-ttu-id="a5efb-127">Vlastní DNS není podporována v hello **volné** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="a5efb-127">Custom DNS is not supported in hello **Free** tier.</span></span> 

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

<span data-ttu-id="a5efb-129">Pokud hello plán služby App Service není **volné**ukončit hello **zvolte cenovou úroveň** stránky a přeskočit příliš[koupit hello domény](#buy-the-domain).</span><span class="sxs-lookup"><span data-stu-id="a5efb-129">If hello App Service plan is not **Free**, close hello **Choose your pricing tier** page and skip too[Buy hello domain](#buy-the-domain).</span></span>

### <a name="scale-up-hello-app-service-plan"></a><span data-ttu-id="a5efb-130">Škálování hello plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="a5efb-130">Scale up hello App Service plan</span></span>

<span data-ttu-id="a5efb-131">Vyberte některé z vrstvy a bezplatnou hello (**sdílené**, **základní**, **standardní**, nebo **Premium**).</span><span class="sxs-lookup"><span data-stu-id="a5efb-131">Select any of hello non-free tiers (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> 

<span data-ttu-id="a5efb-132">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-132">Click **Select**.</span></span>

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

<span data-ttu-id="a5efb-134">Až uvidíte hello následující oznámení, je dokončena operace škálování hello.</span><span class="sxs-lookup"><span data-stu-id="a5efb-134">When you see hello following notification, hello scale operation is complete.</span></span>

![Potvrzení operace škálování](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-hello-domain"></a><span data-ttu-id="a5efb-136">Zakoupení hello domény</span><span class="sxs-lookup"><span data-stu-id="a5efb-136">Buy hello domain</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="a5efb-137">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="a5efb-137">Sign in tooAzure</span></span>
<span data-ttu-id="a5efb-138">Otevřete hello [portál Azure](https://portal.azure.com/) a přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="a5efb-138">Open hello [Azure portal](https://portal.azure.com/) and sign in with your Azure account.</span></span>

### <a name="launch-buy-domains"></a><span data-ttu-id="a5efb-139">Spusťte koupit domén</span><span class="sxs-lookup"><span data-stu-id="a5efb-139">Launch Buy domains</span></span>
<span data-ttu-id="a5efb-140">V hello **webové aplikace** , klikněte na název vaší webové aplikaci, vyberte hello **nastavení**a potom vyberte **vlastní domény**</span><span class="sxs-lookup"><span data-stu-id="a5efb-140">In hello **Web Apps** tab, click hello name of your web app, select **Settings**, and then select **Custom domains**</span></span>
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

<span data-ttu-id="a5efb-141">V hello **vlastní domény** klikněte na tlačítko **koupit domény**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-141">In hello **Custom domains** page, click **Buy domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

### <a name="configure-hello-domain-purchase"></a><span data-ttu-id="a5efb-142">Konfigurace domény nákupu hello</span><span class="sxs-lookup"><span data-stu-id="a5efb-142">Configure hello domain purchase</span></span>

<span data-ttu-id="a5efb-143">V hello **aplikace služby domény** stránku hello **hledat domény** pole, název domény hello typu chcete toobuy a typ `Enter`.</span><span class="sxs-lookup"><span data-stu-id="a5efb-143">In hello **App Service Domain** page, in hello **Search for domain** box, type hello domain name you want toobuy and type `Enter`.</span></span> <span data-ttu-id="a5efb-144">Hello navržené domény k dispozici níže jsou uvedeny pouze hello textové pole.</span><span class="sxs-lookup"><span data-stu-id="a5efb-144">hello suggested available domains are shown just below hello text box.</span></span> <span data-ttu-id="a5efb-145">Vyberte jednu nebo více domén, které chcete toobuy.</span><span class="sxs-lookup"><span data-stu-id="a5efb-145">Select one or more domains you want toobuy.</span></span> 
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

<span data-ttu-id="a5efb-146">Klikněte na tlačítko hello **kontaktní údaje** a vyplňte formulář hello domény kontaktní informace.</span><span class="sxs-lookup"><span data-stu-id="a5efb-146">Click hello **Contact Information** and fill out hello domain's contact information form.</span></span> <span data-ttu-id="a5efb-147">Po dokončení klikněte na tlačítko **OK** tooreturn toohello aplikace služby domény stránky.</span><span class="sxs-lookup"><span data-stu-id="a5efb-147">When finished, click **OK** tooreturn toohello App Service Domain page.</span></span>
   
<span data-ttu-id="a5efb-148">Je důležité, vyplňte všechna povinná pole s tolik přesnost míře.</span><span class="sxs-lookup"><span data-stu-id="a5efb-148">It is important that you fill out all required fields with as much accuracy as possible.</span></span> <span data-ttu-id="a5efb-149">Nesprávná data kontaktní informace může způsobit selhání toopurchase domén.</span><span class="sxs-lookup"><span data-stu-id="a5efb-149">Incorrect data for contact information can result in failure toopurchase domains.</span></span> 

<span data-ttu-id="a5efb-150">V dalším kroku vyberte hello požadované možnosti pro vaši doménu.</span><span class="sxs-lookup"><span data-stu-id="a5efb-150">Next, select hello desired options for your domain.</span></span> <span data-ttu-id="a5efb-151">Viz následující tabulka pro vysvětlení hello:</span><span class="sxs-lookup"><span data-stu-id="a5efb-151">See hello following table for explanations:</span></span>

| <span data-ttu-id="a5efb-152">Nastavení</span><span class="sxs-lookup"><span data-stu-id="a5efb-152">Setting</span></span> | <span data-ttu-id="a5efb-153">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="a5efb-153">Suggested Value</span></span> | <span data-ttu-id="a5efb-154">Popis</span><span class="sxs-lookup"><span data-stu-id="a5efb-154">Description</span></span> |
|-|-|-|
|<span data-ttu-id="a5efb-155">Automatické obnovení</span><span class="sxs-lookup"><span data-stu-id="a5efb-155">Auto renew</span></span> | <span data-ttu-id="a5efb-156">**Povolení**</span><span class="sxs-lookup"><span data-stu-id="a5efb-156">**Enable**</span></span> | <span data-ttu-id="a5efb-157">Obnovuje doménu služby aplikace automaticky každý rok.</span><span class="sxs-lookup"><span data-stu-id="a5efb-157">Renews your App Service Domain automatically every year.</span></span> <span data-ttu-id="a5efb-158">Platební karty je účtován hello stejné kupní ceny v době obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="a5efb-158">Your credit card is charged hello same purchase price at hello time of renewal.</span></span> |
|<span data-ttu-id="a5efb-159">Ochrana osobních údajů</span><span class="sxs-lookup"><span data-stu-id="a5efb-159">Privacy protection</span></span> | <span data-ttu-id="a5efb-160">Povolení</span><span class="sxs-lookup"><span data-stu-id="a5efb-160">Enable</span></span> | <span data-ttu-id="a5efb-161">Vyjádřit výslovný souhlas příliš "Ochrany osobních údajů", který je součástí hello kupní ceny _zdarma_ (s výjimkou domény nejvyšší úrovně, jejichž registru nepodporují ochrany osobních údajů, jako například _. co.in_, _. Co.uk_a tak dále).</span><span class="sxs-lookup"><span data-stu-id="a5efb-161">Opt in too"Privacy protection", which is included in hello purchase price _for free_ (except for top-level domains whose registry do not support privacy protection, such as _.co.in_, _.co.uk_, and so on).</span></span> |
| <span data-ttu-id="a5efb-162">Přiřadit výchozí názvy hostitelů</span><span class="sxs-lookup"><span data-stu-id="a5efb-162">Assign default hostnames</span></span> | <span data-ttu-id="a5efb-163">**Webová** a**@**</span><span class="sxs-lookup"><span data-stu-id="a5efb-163">**www** and **@**</span></span> | <span data-ttu-id="a5efb-164">Vyberte hello požadované vazby názvů hostitelů, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="a5efb-164">Select hello desired hostname bindings, if desired.</span></span> <span data-ttu-id="a5efb-165">Po dokončení operace nákupu domény hello vaší webové aplikace můžete získat přístup na adrese hello vybrané názvy hostitelů.</span><span class="sxs-lookup"><span data-stu-id="a5efb-165">When hello domain purchase operation is complete, your web app can be accessed at hello selected hostnames.</span></span> <span data-ttu-id="a5efb-166">Pokud je webová aplikace hello za [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), nevidíte hello možnost tooassign hello kořenové domény (@), protože nemá podporu záznamů A Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="a5efb-166">If hello web app is behind [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), you don't see hello option tooassign hello root domain (@), because Traffic Manager does not support A records.</span></span> <span data-ttu-id="a5efb-167">Můžete provádět změny přiřazení názvu hostitele toohello po dokončení nákupu domény hello.</span><span class="sxs-lookup"><span data-stu-id="a5efb-167">You can make changes toohello hostname assignments after hello domain purchase completes.</span></span> |

### <a name="accept-terms-and-purchase"></a><span data-ttu-id="a5efb-168">Přijmout podmínky a nákupu</span><span class="sxs-lookup"><span data-stu-id="a5efb-168">Accept terms and purchase</span></span>

<span data-ttu-id="a5efb-169">Klikněte na tlačítko **právní podmínky** tooreview hello podmínky a hello poplatky, pak klikněte na **koupit**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-169">Click **Legal Terms** tooreview hello terms and hello charges, then click **Buy**.</span></span>

> [!NOTE]
> <span data-ttu-id="a5efb-170">Domény služby App Service pomocí Azure DNS toohost hello domén.</span><span class="sxs-lookup"><span data-stu-id="a5efb-170">App Service Domains use Azure DNS toohost hello domains.</span></span> <span data-ttu-id="a5efb-171">Kromě toho toohello domény registrace poplatek, poplatky za používání pro Azure DNS použije.</span><span class="sxs-lookup"><span data-stu-id="a5efb-171">In addition toohello domain registration fee, usage charges for Azure DNS apply.</span></span> <span data-ttu-id="a5efb-172">Informace najdete v tématu [Azure DNS ceny](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="a5efb-172">For information, see [Azure DNS Pricing](https://azure.microsoft.com/pricing/details/dns/).</span></span>
>
>

<span data-ttu-id="a5efb-173">Zpět v hello **aplikace služby domény** klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-173">Back in hello **App Service Domain** page, click **OK**.</span></span> <span data-ttu-id="a5efb-174">Když probíhá operace hello, uvidíte hello následující oznámení:</span><span class="sxs-lookup"><span data-stu-id="a5efb-174">While hello operation is in progress, you see hello following notifications:</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-hello-hostnames"></a><span data-ttu-id="a5efb-175">Názvy hostitelů hello testu</span><span class="sxs-lookup"><span data-stu-id="a5efb-175">Test hello hostnames</span></span>

<span data-ttu-id="a5efb-176">Pokud jste přiřadili výchozí názvy hostitelů tooyour webové aplikace, je také zobrazit upozornění na úspěch pro každý vybraný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="a5efb-176">If you have assigned default hostnames tooyour web app, you also see a success notification for each selected hostname.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

<span data-ttu-id="a5efb-177">Zobrazí také hello vybrané názvy hostitelů v hello **vlastní domény** stránku hello **názvy hostitelů** části.</span><span class="sxs-lookup"><span data-stu-id="a5efb-177">You also see hello selected hostnames in hello **Custom domains** page, in hello **Hostnames** section.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

<span data-ttu-id="a5efb-178">názvy hostitelů hello tootest, přejděte toohello uvedené názvy hostitelů v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="a5efb-178">tootest hello hostnames, navigate toohello listed hostnames in hello browser.</span></span> <span data-ttu-id="a5efb-179">V příkladu hello v hello předchozím snímku obrazovky zkuste navigace too_kontoso.net_ a _www.kontoso.net_.</span><span class="sxs-lookup"><span data-stu-id="a5efb-179">In hello example in hello preceding screenshot, try navigating too_kontoso.net_ and _www.kontoso.net_.</span></span>

## <a name="assign-hostnames-tooweb-app"></a><span data-ttu-id="a5efb-180">Přiřaďte aplikaci tooweb názvy hostitelů</span><span class="sxs-lookup"><span data-stu-id="a5efb-180">Assign hostnames tooweb app</span></span>

<span data-ttu-id="a5efb-181">Pokud zvolíte není tooassign jeden nebo více výchozí názvy hostitelů tooyour webovou aplikaci během hello procesu nákupu nebo pokud potřebujete tooassign název hostitele není uvedený, můžete přiřadit název hostitele v kdykoli.</span><span class="sxs-lookup"><span data-stu-id="a5efb-181">If you choose not tooassign one or more default hostnames tooyour web app during hello purchase process, or if you need tooassign a hostname not listed, you can assign a hostname at anytime.</span></span>

<span data-ttu-id="a5efb-182">Názvy hostitelů v tooany hello doména aplikace služby lze přiřadit také jiné webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a5efb-182">You can also assign hostnames in hello App Service Domain tooany other web app.</span></span> <span data-ttu-id="a5efb-183">Hello kroky závisí na tom, jestli hello doména služby aplikace a webové aplikace hello patří toohello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="a5efb-183">hello steps depend on whether hello App Service Domain and hello web app belong toohello same subscription.</span></span>

- <span data-ttu-id="a5efb-184">Jiné předplatné: namapovat vlastní záznamy DNS ze hello aplikace služby domény toohello webová aplikace jako externě zakoupené domény.</span><span class="sxs-lookup"><span data-stu-id="a5efb-184">Different subscription: Map custom DNS records from hello App Service Domain toohello web app like an externally purchased domain.</span></span> <span data-ttu-id="a5efb-185">Informace o přidání vlastní DNS názvy tooan doména služby aplikace, najdete v části [vlastní záznamy DNS spravovat](#custom).</span><span class="sxs-lookup"><span data-stu-id="a5efb-185">For information on adding custom DNS names tooan App Service Domain, see [Manage custom DNS records](#custom).</span></span> <span data-ttu-id="a5efb-186">toomap externí zakoupené domény tooa webové aplikace, najdete v části [mapovat existující vlastní DNS název tooAzure webové aplikace](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="a5efb-186">toomap an external purchased domain tooa web app, see [Map an existing custom DNS name tooAzure Web Apps](app-service-web-tutorial-custom-domain.md).</span></span> 
- <span data-ttu-id="a5efb-187">Stejného předplatného: hello použijte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="a5efb-187">Same subscription: Use hello following steps.</span></span>

### <a name="launch-add-hostname"></a><span data-ttu-id="a5efb-188">Spuštění přidat název hostitele</span><span class="sxs-lookup"><span data-stu-id="a5efb-188">Launch add hostname</span></span>
<span data-ttu-id="a5efb-189">V hello **App Services** stránky, vyberte hello název webové aplikace, který chcete tooassign názvů hostitelů, vyberte **nastavení**a potom vyberte **vlastní domény**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-189">In hello **App Services** page, select hello name of your web app that you want tooassign hostnames to, select **Settings**, and then select **Custom domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

<span data-ttu-id="a5efb-190">Ujistěte se, že doménu zakoupené je uvedena ve hello **doménami aplikací služby** tématu, ale nezaškrtnete políčko.</span><span class="sxs-lookup"><span data-stu-id="a5efb-190">Make sure that your purchased domain is listed in hello **App Service Domains** section, but don't select it.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> <span data-ttu-id="a5efb-191">Všechny aplikace služby domény ve hello stejného předplatného se zobrazují v hello webové aplikace **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="a5efb-191">All App Service Domains in hello same subscription are shown in hello web app's **Custom domains** page.</span></span> <span data-ttu-id="a5efb-192">Pokud vaše doména je v předplatném hello webové aplikace, ale se nezobrazí v hello webové aplikace **vlastní domény** stránky, zkuste to znovu hello **vlastní domény** stránky nebo aktualizovat webovou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="a5efb-192">If your domain is in hello web app's subscription, but you cannot see it in hello web app's **Custom domains** page, try reopening hello **Custom domains** page or refresh hello webpage.</span></span> <span data-ttu-id="a5efb-193">Zkontrolujte také, hello oznámení zvonku hello horní části hello portál Azure pro průběh nebo vytvoření chyby.</span><span class="sxs-lookup"><span data-stu-id="a5efb-193">Also, check hello notification bell at hello top of hello Azure portal for progress or creation failures.</span></span>
>
>

<span data-ttu-id="a5efb-194">Vyberte **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-194">Select **Add hostname**.</span></span>

### <a name="configure-hostname"></a><span data-ttu-id="a5efb-195">Nakonfigurujte název hostitele</span><span class="sxs-lookup"><span data-stu-id="a5efb-195">Configure hostname</span></span>
<span data-ttu-id="a5efb-196">V hello **přidat název hostitele** dialogové okno, typ hello plně kvalifikovaný název domény vaší domény služby aplikace nebo jakékoli subdomény.</span><span class="sxs-lookup"><span data-stu-id="a5efb-196">In hello **Add hostname** dialog, type hello fully qualified domain name of your App Service Domain or any subdomain.</span></span> <span data-ttu-id="a5efb-197">Například:</span><span class="sxs-lookup"><span data-stu-id="a5efb-197">For example:</span></span>

- <span data-ttu-id="a5efb-198">kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="a5efb-198">kontoso.net</span></span>
- <span data-ttu-id="a5efb-199">www.kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="a5efb-199">www.kontoso.net</span></span>
- <span data-ttu-id="a5efb-200">ABC.kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="a5efb-200">abc.kontoso.net</span></span>

<span data-ttu-id="a5efb-201">Po dokončení vyberte **ověřením**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-201">When finished, select **Validate**.</span></span> <span data-ttu-id="a5efb-202">Typ záznamu Hello název hostitele je automaticky vybrán za vás.</span><span class="sxs-lookup"><span data-stu-id="a5efb-202">hello hostname record type is automatically selected for you.</span></span>

<span data-ttu-id="a5efb-203">Vyberte **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-203">Select **Add hostname**.</span></span>

<span data-ttu-id="a5efb-204">Po dokončení operace hello zobrazí upozornění na úspěch hello přiřazen název hostitele.</span><span class="sxs-lookup"><span data-stu-id="a5efb-204">When hello operation is complete, you see a success notification for hello assigned hostname.</span></span>  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a><span data-ttu-id="a5efb-205">Zavřete přidat název hostitele</span><span class="sxs-lookup"><span data-stu-id="a5efb-205">Close add hostname</span></span>
<span data-ttu-id="a5efb-206">V hello **přidat název hostitele** přiřaďte žádné jiné hostname tooyour webové aplikace, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="a5efb-206">In hello **Add hostname** page, assign any other hostname tooyour web app, as desired.</span></span> <span data-ttu-id="a5efb-207">Po dokončení zavřete hello **přidat název hostitele** stránky.</span><span class="sxs-lookup"><span data-stu-id="a5efb-207">When finished, close hello **Add hostname** page.</span></span>

<span data-ttu-id="a5efb-208">Teď byste měli vidět hello nově přiřazeny hostname(s) ve vaší aplikaci **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="a5efb-208">You should now see hello newly assigned hostname(s) in your app's **Custom domains** page.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-hello-hostnames"></a><span data-ttu-id="a5efb-209">Názvy hostitelů hello testu</span><span class="sxs-lookup"><span data-stu-id="a5efb-209">Test hello hostnames</span></span>

<span data-ttu-id="a5efb-210">Přejděte toohello uvedené názvy hostitelů v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="a5efb-210">Navigate toohello listed hostnames in hello browser.</span></span> <span data-ttu-id="a5efb-211">V příkladu hello v předchozím snímku obrazovky hello zkuste navigace too_abc.kontoso.net_.</span><span class="sxs-lookup"><span data-stu-id="a5efb-211">In hello example in hello preceding screenshot, try navigating too_abc.kontoso.net_.</span></span>

<a name="custom" />

## <a name="manage-custom-dns-records"></a><span data-ttu-id="a5efb-212">Spravovat vlastní záznamy DNS</span><span class="sxs-lookup"><span data-stu-id="a5efb-212">Manage custom DNS records</span></span>

<span data-ttu-id="a5efb-213">V Azure, záznamy DNS pro doménu služby App Service se spravují pomocí [Azure DNS](https://azure.microsoft.com/services/dns/).</span><span class="sxs-lookup"><span data-stu-id="a5efb-213">In Azure, DNS records for an App Service Domain are managed using [Azure DNS](https://azure.microsoft.com/services/dns/).</span></span> <span data-ttu-id="a5efb-214">Můžete přidat, odebrat a aktualizovat záznamy DNS, stejně jako pro externě zakoupené doménu.</span><span class="sxs-lookup"><span data-stu-id="a5efb-214">You can add, remove, and update DNS records, just like for an externally purchased domain.</span></span>

### <a name="open-app-service-domain"></a><span data-ttu-id="a5efb-215">Otevřete aplikaci služby domény</span><span class="sxs-lookup"><span data-stu-id="a5efb-215">Open App Service Domain</span></span>

<span data-ttu-id="a5efb-216">V hello portál Azure, hello levé nabídce, vyberte **více služeb** > **doménami aplikací služby**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-216">In hello Azure portal, from hello left menu, select **More Services** > **App Service Domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

<span data-ttu-id="a5efb-217">Vyberte domény toomanage hello.</span><span class="sxs-lookup"><span data-stu-id="a5efb-217">Select hello domain toomanage.</span></span> 

### <a name="access-dns-zone"></a><span data-ttu-id="a5efb-218">Zóna DNS přístup</span><span class="sxs-lookup"><span data-stu-id="a5efb-218">Access DNS zone</span></span>

<span data-ttu-id="a5efb-219">V levé nabídce hello domény, vyberte **zónu DNS**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-219">In hello domain's left menu, select **DNS zone**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

<span data-ttu-id="a5efb-220">Tato akce otevře hello [zónu DNS](../dns/dns-zones-records.md) stránku vaší domény služby aplikace v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="a5efb-220">This action opens hello [DNS zone](../dns/dns-zones-records.md) page of your App Service Domain in Azure DNS.</span></span> <span data-ttu-id="a5efb-221">Informace o tom tooedit záznamy DNS, najdete v části [jak hello toomanage zóny DNS v portálu Azure](../dns/dns-operations-dnszones-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a5efb-221">For information on how tooedit DNS records, see [How toomanage DNS Zones in hello Azure portal](../dns/dns-operations-dnszones-portal.md).</span></span>

## <a name="cancel-purchase-delete-domain"></a><span data-ttu-id="a5efb-222">Zrušit nákup (odstranit domény)</span><span class="sxs-lookup"><span data-stu-id="a5efb-222">Cancel purchase (delete domain)</span></span>

<span data-ttu-id="a5efb-223">Po zakoupení hello aplikace služby domény máte pět dní toocancel nákupu pro vrátit celou částku.</span><span class="sxs-lookup"><span data-stu-id="a5efb-223">After you purchase hello App Service Domain, you have five days toocancel your purchase for a full refund.</span></span> <span data-ttu-id="a5efb-224">Po pět dní můžete odstranit hello doména služby aplikace ale nemůže přijímat náhrada.</span><span class="sxs-lookup"><span data-stu-id="a5efb-224">After five days, you can delete hello App Service Domain, but cannot receive a refund.</span></span>

### <a name="open-app-service-domain"></a><span data-ttu-id="a5efb-225">Otevřete aplikaci služby domény</span><span class="sxs-lookup"><span data-stu-id="a5efb-225">Open App Service Domain</span></span>

<span data-ttu-id="a5efb-226">V hello portál Azure, hello levé nabídce, vyberte **více služeb** > **doménami aplikací služby**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-226">In hello Azure portal, from hello left menu, select **More Services** > **App Service Domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

<span data-ttu-id="a5efb-227">Vyberte hello domény tooyou chcete toocancel nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="a5efb-227">Select hello domain tooyou want toocancel or delete.</span></span> 

### <a name="delete-hostname-bindings"></a><span data-ttu-id="a5efb-228">Odstranit vazby názvů hostitelů.</span><span class="sxs-lookup"><span data-stu-id="a5efb-228">Delete hostname bindings</span></span>

<span data-ttu-id="a5efb-229">V levé nabídce hello domény, vyberte **vazby názvů hostitelů**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-229">In hello domain's left menu, select **Hostname bindings**.</span></span> <span data-ttu-id="a5efb-230">vazby názvů hostitelů Hello ze všech služeb Azure jsou zde uvedeny.</span><span class="sxs-lookup"><span data-stu-id="a5efb-230">hello hostname bindings from all Azure services are listed here.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

<span data-ttu-id="a5efb-231">Hello aplikace služby domény nelze odstranit, dokud nejsou odstraněny všechny vazby názvů hostitelů.</span><span class="sxs-lookup"><span data-stu-id="a5efb-231">You cannot delete hello App Service Domain until all hostname bindings are deleted.</span></span>

<span data-ttu-id="a5efb-232">Odstranit vazbu každý název hostitele tak, že vyberete **...**   >  **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-232">Delete each hostname binding by selecting **...** > **Delete**.</span></span> <span data-ttu-id="a5efb-233">Po odstranění se všechny vazby hello, vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-233">After all hello bindings are deleted, select **Save**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a><span data-ttu-id="a5efb-234">Zrušit nebo odstranění</span><span class="sxs-lookup"><span data-stu-id="a5efb-234">Cancel or delete</span></span>

<span data-ttu-id="a5efb-235">V levé nabídce hello domény, vyberte **přehled**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-235">In hello domain's left menu, select **Overview**.</span></span> 

<span data-ttu-id="a5efb-236">Pokud nebyl uplynutí období zrušení hello v doméně hello zakoupili, vyberte **zrušit nákup**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-236">If hello cancellation period on hello purchased domain has not elapsed, select **Cancel purchase**.</span></span> <span data-ttu-id="a5efb-237">Jinak se zobrazí **odstranit** tlačítko místo.</span><span class="sxs-lookup"><span data-stu-id="a5efb-237">Otherwise, you see a **Delete** button instead.</span></span> <span data-ttu-id="a5efb-238">toodelete hello domény bez náhrady, vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="a5efb-238">toodelete hello domain without a refund, select **Delete**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

<span data-ttu-id="a5efb-239">Vyberte **OK** tooconfirm hello operaci.</span><span class="sxs-lookup"><span data-stu-id="a5efb-239">Select **OK** tooconfirm hello operation.</span></span> <span data-ttu-id="a5efb-240">Pokud nechcete, aby tooproceed, klikněte kamkoli mimo dialogové okno potvrzení hello.</span><span class="sxs-lookup"><span data-stu-id="a5efb-240">If you don't want tooproceed, click anywhere outside of hello confirmation dialog.</span></span>

<span data-ttu-id="a5efb-241">Po dokončení operace hello hello doména je vydaná ze svého předplatného a k dispozici pro každý, kdo toopurchase znovu.</span><span class="sxs-lookup"><span data-stu-id="a5efb-241">After hello operation is complete, hello domain is released from your subscription and available for anyone toopurchase again.</span></span> 
