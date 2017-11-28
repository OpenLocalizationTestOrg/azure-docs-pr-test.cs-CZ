---
title: "aaaMap existující vlastní DNS název webové aplikace tooAzure | Microsoft Docs"
description: "Zjistěte, jak tooadd existující vlastní domény DNS název webové aplikace tooa (jednoduché domény), back-end mobilní aplikace nebo aplikace API v Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 2c4eea3c56c758ca11355554321ffa52dd2c6b9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="map-an-existing-custom-dns-name-tooazure-web-apps"></a><span data-ttu-id="dd9f6-103">Mapovat existující vlastní DNS název tooAzure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="dd9f6-103">Map an existing custom DNS name tooAzure Web Apps</span></span>

<span data-ttu-id="dd9f6-104">[Azure Web Apps](app-service-web-overview.md) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-104">[Azure Web Apps](app-service-web-overview.md) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="dd9f6-105">Tento kurz ukazuje, jak toomap existující vlastní DNS název tooAzure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-105">This tutorial shows you how toomap an existing custom DNS name tooAzure Web Apps.</span></span>

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

<span data-ttu-id="dd9f6-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="dd9f6-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dd9f6-108">Mapování subdoména (například `www.contoso.com`) pomocí záznamu CNAME</span><span class="sxs-lookup"><span data-stu-id="dd9f6-108">Map a subdomain (for example, `www.contoso.com`) by using a CNAME record</span></span>
> * <span data-ttu-id="dd9f6-109">Mapování kořenové domény (například `contoso.com`) pomocí záznamu a.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-109">Map a root domain (for example, `contoso.com`) by using an A record</span></span>
> * <span data-ttu-id="dd9f6-110">Namapovat doménu zástupných znaků (například `*.contoso.com`) pomocí záznamu CNAME</span><span class="sxs-lookup"><span data-stu-id="dd9f6-110">Map a wildcard domain (for example, `*.contoso.com`) by using a CNAME record</span></span>
> * <span data-ttu-id="dd9f6-111">Automatizovat mapování domény pomocí skriptů</span><span class="sxs-lookup"><span data-stu-id="dd9f6-111">Automate domain mapping with scripts</span></span>

<span data-ttu-id="dd9f6-112">Můžete použít buď **záznam CNAME** nebo **záznam** toomap vlastní DNS název tooApp služby.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-112">You can use either a **CNAME record** or an **A record** toomap a custom DNS name tooApp Service.</span></span> 

> [!NOTE]
> <span data-ttu-id="dd9f6-113">Doporučujeme použít záznam CNAME pro všechny vlastní názvy DNS kromě kořenové domény (například `contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-113">We recommend that you use a CNAME for all custom DNS names except a root domain (for example, `contoso.com`).</span></span>

<span data-ttu-id="dd9f6-114">toomigrate živý web a jeho DNS domény název tooApp služby, najdete v části [migrovat active tooAzure název DNS služby App Service](app-service-custom-domain-name-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-114">toomigrate a live site and its DNS domain name tooApp Service, see [Migrate an active DNS name tooAzure App Service](app-service-custom-domain-name-migrate.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dd9f6-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dd9f6-115">Prerequisites</span></span>

<span data-ttu-id="dd9f6-116">toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="dd9f6-116">toocomplete this tutorial:</span></span>

* <span data-ttu-id="dd9f6-117">[Vytvořit aplikaci služby App Service](/azure/app-service/), nebo použijte aplikaci, kterou jste vytvořili pro jiné kurzu.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-117">[Create an App Service app](/azure/app-service/), or use an app that you created for another tutorial.</span></span>
* <span data-ttu-id="dd9f6-118">Zakupte název domény a ujistěte se, že máte přístup toohello DNS registru pro poskytovatele domény (například GoDaddy).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-118">Purchase a domain name and make sure you have access toohello DNS registry for your domain provider (such as GoDaddy).</span></span>

  <span data-ttu-id="dd9f6-119">Tooadd záznamy DNS, třeba pro `contoso.com` a `www.contoso.com`, musí být schopný tooconfigure hello nastavení DNS pro hello `contoso.com` kořenové domény.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-119">For example, tooadd DNS entries for `contoso.com` and `www.contoso.com`, you must be able tooconfigure hello DNS settings for hello `contoso.com` root domain.</span></span>

  > [!NOTE]
  > <span data-ttu-id="dd9f6-120">Pokud nemáte existující domény název, vezměte v úvahu [zakoupení domény pomocí portálu Azure hello](custom-dns-web-site-buydomains-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-120">If you don't have an existing domain name, consider [purchasing a domain using hello Azure portal](custom-dns-web-site-buydomains-web-app.md).</span></span> 

## <a name="prepare-hello-app"></a><span data-ttu-id="dd9f6-121">Příprava aplikace hello</span><span class="sxs-lookup"><span data-stu-id="dd9f6-121">Prepare hello app</span></span>

<span data-ttu-id="dd9f6-122">toomap vlastní DNS název tooa webové aplikace, hello webové aplikace na [plán služby App Service](https://azure.microsoft.com/pricing/details/app-service/) musí být placené vrstvy (**sdílené**, **základní**, **standardní**, nebo  **Premium**).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-122">toomap a custom DNS name tooa web app, hello web app's [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be a paid tier (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> <span data-ttu-id="dd9f6-123">V tomto kroku budete zajistěte, aby tento hello v hello je aplikace služby App Service podporována cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-123">In this step, you make sure that hello App Service app is in hello supported pricing tier.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="dd9f6-124">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="dd9f6-124">Sign in tooAzure</span></span>

<span data-ttu-id="dd9f6-125">Otevřete hello [portál Azure](https://portal.azure.com) a přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-125">Open hello [Azure portal](https://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="navigate-toohello-app-in-hello-azure-portal"></a><span data-ttu-id="dd9f6-126">Přejděte toohello aplikace v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="dd9f6-126">Navigate toohello app in hello Azure portal</span></span>

<span data-ttu-id="dd9f6-127">Hello levé nabídce vyberte **App Services**a pak vyberte název hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-127">From hello left menu, select **App Services**, and then select hello name of hello app.</span></span>

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/select-app.png)

<span data-ttu-id="dd9f6-129">Zobrazí stránku hello správu hello aplikaci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-129">You see hello management page of hello App Service app.</span></span>  

<a name="checkpricing"></a>

### <a name="check-hello-pricing-tier"></a><span data-ttu-id="dd9f6-130">Zkontrolujte hello cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="dd9f6-130">Check hello pricing tier</span></span>

<span data-ttu-id="dd9f6-131">V levé navigační stránku hello aplikace hello, posuňte toohello **nastavení** a vyberte **škálování (plán služby App Service)**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-131">In hello left navigation of hello app page, scroll toohello **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Škálování nabídky](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

<span data-ttu-id="dd9f6-133">aktuální úroveň aplikace Hello je označený modré ohraničení.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-133">hello app's current tier is highlighted by a blue border.</span></span> <span data-ttu-id="dd9f6-134">Zkontrolujte toomake se, že aplikace hello není hello **volné** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-134">Check toomake sure that hello app is not in hello **Free** tier.</span></span> <span data-ttu-id="dd9f6-135">Vlastní DNS není podporována v hello **volné** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-135">Custom DNS is not supported in hello **Free** tier.</span></span> 

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

<span data-ttu-id="dd9f6-137">Pokud hello plán služby App Service není **volné**ukončit hello **zvolte cenovou úroveň** stránky a přeskočit příliš[mapování záznam CNAME](#cname).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-137">If hello App Service plan is not **Free**, close hello **Choose your pricing tier** page and skip too[Map a CNAME record](#cname).</span></span>

<a name="scaleup"></a>

### <a name="scale-up-hello-app-service-plan"></a><span data-ttu-id="dd9f6-138">Škálování hello plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="dd9f6-138">Scale up hello App Service plan</span></span>

<span data-ttu-id="dd9f6-139">Vyberte některé z vrstvy a bezplatnou hello (**sdílené**, **základní**, **standardní**, nebo **Premium**).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-139">Select any of hello non-free tiers (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> 

<span data-ttu-id="dd9f6-140">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-140">Click **Select**.</span></span>

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

<span data-ttu-id="dd9f6-142">Až uvidíte hello následující oznámení, je dokončena operace škálování hello.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-142">When you see hello following notification, hello scale operation is complete.</span></span>

![Potvrzení operace škálování](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a><span data-ttu-id="dd9f6-144">Mapa záznam CNAME</span><span class="sxs-lookup"><span data-stu-id="dd9f6-144">Map a CNAME record</span></span>

<span data-ttu-id="dd9f6-145">V příkladu kurz hello přidat záznam CNAME pro hello `www` subdomény (například `www.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-145">In hello tutorial example, you add a CNAME record for hello `www` subdomain (for example, `www.contoso.com`).</span></span>

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-cname-record"></a><span data-ttu-id="dd9f6-146">Vytvořit záznam CNAME hello</span><span class="sxs-lookup"><span data-stu-id="dd9f6-146">Create hello CNAME record</span></span>

<span data-ttu-id="dd9f6-147">Přidat aplikaci subdomény toohello výchozí název hostitele toomap záznamů CNAME (`<app_name>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-147">Add a CNAME record toomap a subdomain toohello app's default hostname (`<app_name>.azurewebsites.net`).</span></span>

<span data-ttu-id="dd9f6-148">Pro hello `www.contoso.com` příklad domény, přidejte záznam CNAME, který mapuje název hello `www` příliš`<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-148">For hello `www.contoso.com` domain example, add a CNAME record that maps hello name `www` too`<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="dd9f6-149">Po přidání hello CNAME stránky záznamů DNS hello vypadá hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="dd9f6-149">After you add hello CNAME, hello DNS records page looks like hello following example:</span></span>

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-hello-cname-record-mapping-in-azure"></a><span data-ttu-id="dd9f6-151">Povolit hello mapování záznam CNAME ve službě Azure</span><span class="sxs-lookup"><span data-stu-id="dd9f6-151">Enable hello CNAME record mapping in Azure</span></span>

<span data-ttu-id="dd9f6-152">V hello levé navigační stránku hello aplikace v hello portálu Azure, vyberte **vlastní domény**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-152">In hello left navigation of hello app page in hello Azure portal, select **Custom domains**.</span></span> 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="dd9f6-154">V hello **vlastní domény** stránku hello aplikace a přidat hello plně kvalifikovaný název DNS vlastního (`www.contoso.com`) toohello seznamu.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-154">In hello **Custom domains** page of hello app, add hello fully qualified custom DNS name (`www.contoso.com`) toohello list.</span></span>

<span data-ttu-id="dd9f6-155">Vyberte hello  **+**  ikonu další příliš**přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-155">Select hello **+** icon next too**Add hostname**.</span></span>

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="dd9f6-157">Typ hello plně kvalifikovaný název domény přidat záznam CNAME pro, jako například `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-157">Type hello fully qualified domain name that you added a CNAME record for, such as `www.contoso.com`.</span></span> 

<span data-ttu-id="dd9f6-158">Vyberte **ověření**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-158">Select **Validate**.</span></span>

<span data-ttu-id="dd9f6-159">Hello **přidat název hostitele** se aktivuje tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-159">hello **Add hostname** button is activated.</span></span> 

<span data-ttu-id="dd9f6-160">Ujistěte se, že **typ záznamu Hostname** je nastaven příliš**CNAME (www.example.com nebo všechny subdomény)**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-160">Make sure that **Hostname record type** is set too**CNAME (www.example.com or any subdomain)**.</span></span>

<span data-ttu-id="dd9f6-161">Vyberte **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-161">Select **Add hostname**.</span></span>

![Přidat aplikaci toohello název DNS](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

<span data-ttu-id="dd9f6-163">Může trvat nějakou dobu hello nový název hostitele toobe promítnuta aplikace hello **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-163">It might take some time for hello new hostname toobe reflected in hello app's **Custom domains** page.</span></span> <span data-ttu-id="dd9f6-164">Aktualizujte data hello tooupdate hello prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-164">Try refreshing hello browser tooupdate hello data.</span></span>

![Přidat záznam CNAME](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

<span data-ttu-id="dd9f6-166">Pokud je vynechán krok nebo provedené překlepem někde dříve, se zobrazí chyba ověření v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-166">If you missed a step or made a typo somewhere earlier, you see a verification error at hello bottom of hello page.</span></span>

![Chyba ověření](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a><span data-ttu-id="dd9f6-168">Mapa záznamu a.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-168">Map an A record</span></span>

<span data-ttu-id="dd9f6-169">V příkladu hello kurzu přidáte záznamu A pro hello kořenové domény (například `contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-169">In hello tutorial example, you add an A record for hello root domain (for example, `contoso.com`).</span></span> 

<a name="info"></a>

### <a name="copy-hello-apps-ip-address"></a><span data-ttu-id="dd9f6-170">Zkopírujte aplikace hello IP adresu</span><span class="sxs-lookup"><span data-stu-id="dd9f6-170">Copy hello app's IP address</span></span>

<span data-ttu-id="dd9f6-171">záznam toomap A, musíte aplikace hello externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-171">toomap an A record, you need hello app's external IP address.</span></span> <span data-ttu-id="dd9f6-172">Tuto IP adresu můžete najít v aplikaci hello **vlastní domény** stránku hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-172">You can find this IP address in hello app's **Custom domains** page in hello Azure portal.</span></span>

<span data-ttu-id="dd9f6-173">V hello levé navigační stránku hello aplikace v hello portálu Azure, vyberte **vlastní domény**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-173">In hello left navigation of hello app page in hello Azure portal, select **Custom domains**.</span></span> 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="dd9f6-175">V hello **vlastní domény** stránky, zkopírujte aplikace hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-175">In hello **Custom domains** page, copy hello app's IP address.</span></span>

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-a-record"></a><span data-ttu-id="dd9f6-177">Vytvořit záznam A hello</span><span class="sxs-lookup"><span data-stu-id="dd9f6-177">Create hello A record</span></span>

<span data-ttu-id="dd9f6-178">toomap A záznamů tooan aplikace, služby App Service vyžaduje **dva** záznamy DNS:</span><span class="sxs-lookup"><span data-stu-id="dd9f6-178">toomap an A record tooan app, App Service requires **two** DNS records:</span></span>

- <span data-ttu-id="dd9f6-179">**A** záznam toomap toohello aplikace IP adresu.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-179">An **A** record toomap toohello app's IP address.</span></span>
- <span data-ttu-id="dd9f6-180">A **TXT** záznam hostname výchozí aplikace toohello toomap `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-180">A **TXT** record toomap toohello app's default hostname `<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="dd9f6-181">Služby App Service používá tento záznam pouze v době konfigurace, tooverify, že vlastníte hello vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-181">App Service uses this record only at configuration time, tooverify that you own hello custom domain.</span></span> <span data-ttu-id="dd9f6-182">Jakmile vaši vlastní doménu ověřit a nakonfigurovat ve službě App Service, můžete odstranit tento záznam TXT.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-182">After your custom domain is validated and configured in App Service, you can delete this TXT record.</span></span> 

<span data-ttu-id="dd9f6-183">Pro hello `contoso.com` například domény vytvořit záznamy A a TXT hello podle následující tabulky toohello (`@` obvykle představuje hello kořenové domény).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-183">For hello `contoso.com` domain example, create hello A and TXT records according toohello following table (`@` typically represents hello root domain).</span></span> 

| <span data-ttu-id="dd9f6-184">Typ záznamu</span><span class="sxs-lookup"><span data-stu-id="dd9f6-184">Record type</span></span> | <span data-ttu-id="dd9f6-185">Hostitel</span><span class="sxs-lookup"><span data-stu-id="dd9f6-185">Host</span></span> | <span data-ttu-id="dd9f6-186">Hodnota</span><span class="sxs-lookup"><span data-stu-id="dd9f6-186">Value</span></span> |
| - | - | - |
| <span data-ttu-id="dd9f6-187">A</span><span class="sxs-lookup"><span data-stu-id="dd9f6-187">A</span></span> | `@` | <span data-ttu-id="dd9f6-188">IP adresu z [aplikace hello kopie IP adresu](#info)</span><span class="sxs-lookup"><span data-stu-id="dd9f6-188">IP address from [Copy hello app's IP address](#info)</span></span> |
| <span data-ttu-id="dd9f6-189">TXT</span><span class="sxs-lookup"><span data-stu-id="dd9f6-189">TXT</span></span> | `@` | `<app_name>.azurewebsites.net` |

<span data-ttu-id="dd9f6-190">Při přidávání záznamů hello, vypadá hello stránky záznamů DNS hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="dd9f6-190">When hello records are added, hello DNS records page looks like hello following example:</span></span>

![Stránky záznamů DNS](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-hello-a-record-mapping-in-hello-app"></a><span data-ttu-id="dd9f6-192">Povolit hello mapování záznam v aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="dd9f6-192">Enable hello A record mapping in hello app</span></span>

<span data-ttu-id="dd9f6-193">Zpět v aplikaci hello **vlastní domény** stránku hello portálu Azure, přidejte hello plně kvalifikovaný název vlastního DNS (například `contoso.com`) toohello seznamu.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-193">Back in hello app's **Custom domains** page in hello Azure portal, add hello fully qualified custom DNS name (for example, `contoso.com`) toohello list.</span></span>

<span data-ttu-id="dd9f6-194">Vyberte hello  **+**  ikonu další příliš**přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-194">Select hello **+** icon next too**Add hostname**.</span></span>

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

<span data-ttu-id="dd9f6-196">Typ hello plně kvalifikovaný název domény nakonfigurované hello A záznam, jako například `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-196">Type hello fully qualified domain name that you configured hello A record for, such as `contoso.com`.</span></span>

<span data-ttu-id="dd9f6-197">Vyberte **ověření**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-197">Select **Validate**.</span></span>

<span data-ttu-id="dd9f6-198">Hello **přidat název hostitele** se aktivuje tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-198">hello **Add hostname** button is activated.</span></span> 

<span data-ttu-id="dd9f6-199">Ujistěte se, že **typ záznamu Hostname** je nastaven příliš**záznam (example.com)**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-199">Make sure that **Hostname record type** is set too**A record (example.com)**.</span></span>

<span data-ttu-id="dd9f6-200">Vyberte **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-200">Select **Add hostname**.</span></span>

![Přidat aplikaci toohello název DNS](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

<span data-ttu-id="dd9f6-202">Může trvat nějakou dobu hello nový název hostitele toobe promítnuta aplikace hello **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-202">It might take some time for hello new hostname toobe reflected in hello app's **Custom domains** page.</span></span> <span data-ttu-id="dd9f6-203">Aktualizujte data hello tooupdate hello prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-203">Try refreshing hello browser tooupdate hello data.</span></span>

![Přidat záznam](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

<span data-ttu-id="dd9f6-205">Pokud je vynechán krok nebo provedené překlepem někde dříve, se zobrazí chyba ověření v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-205">If you missed a step or made a typo somewhere earlier, you see a verification error at hello bottom of hello page.</span></span>

![Chyba ověření](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a><span data-ttu-id="dd9f6-207">Namapovat doménu zástupný znak</span><span class="sxs-lookup"><span data-stu-id="dd9f6-207">Map a wildcard domain</span></span>

<span data-ttu-id="dd9f6-208">V příkladu kurz hello se mapují [název DNS zástupný znak](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (například `*.contoso.com`) toohello aplikace App Service přidáním záznam CNAME.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-208">In hello tutorial example, you map a [wildcard DNS name](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (for example, `*.contoso.com`) toohello App Service app by adding a CNAME record.</span></span> 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-cname-record"></a><span data-ttu-id="dd9f6-209">Vytvořit záznam CNAME hello</span><span class="sxs-lookup"><span data-stu-id="dd9f6-209">Create hello CNAME record</span></span>

<span data-ttu-id="dd9f6-210">Přidat aplikaci toohello na zástupný název výchozí název hostitele toomap záznamů CNAME (`<app_name>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-210">Add a CNAME record toomap a wildcard name toohello app's default hostname (`<app_name>.azurewebsites.net`).</span></span>

<span data-ttu-id="dd9f6-211">Pro hello `*.contoso.com` například domény hello záznam CNAME se mapování názvu hello `*` příliš`<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-211">For hello `*.contoso.com` domain example, hello CNAME record will map hello name `*` too`<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="dd9f6-212">Při přidání hello CNAME hello stránky záznamů DNS vypadá hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="dd9f6-212">When hello CNAME is added, hello DNS records page looks like hello following example:</span></span>

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-hello-cname-record-mapping-in-hello-app"></a><span data-ttu-id="dd9f6-214">Povolit hello mapování záznam CNAME v aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="dd9f6-214">Enable hello CNAME record mapping in hello app</span></span>

<span data-ttu-id="dd9f6-215">Nyní můžete přidat všechny subdomény, která odpovídá hello zástupný název toohello aplikace (například `sub1.contoso.com` a `sub2.contoso.com` odpovídat `*.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-215">You can now add any subdomain that matches hello wildcard name toohello app (for example, `sub1.contoso.com` and `sub2.contoso.com` match `*.contoso.com`).</span></span> 

<span data-ttu-id="dd9f6-216">V hello levé navigační stránku hello aplikace v hello portálu Azure, vyberte **vlastní domény**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-216">In hello left navigation of hello app page in hello Azure portal, select **Custom domains**.</span></span> 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="dd9f6-218">Vyberte hello  **+**  ikonu další příliš**přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-218">Select hello **+** icon next too**Add hostname**.</span></span>

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="dd9f6-220">Zadejte název plně kvalifikované domény, který odpovídá hello zástupné domény (například `sub1.contoso.com`) a potom vyberte **ověřením**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-220">Type a fully qualified domain name that matches hello wildcard domain (for example, `sub1.contoso.com`), and then select **Validate**.</span></span>

<span data-ttu-id="dd9f6-221">Hello **přidat název hostitele** se aktivuje tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-221">hello **Add hostname** button is activated.</span></span> 

<span data-ttu-id="dd9f6-222">Ujistěte se, že **typ záznamu Hostname** je nastaven příliš**záznam CNAME (www.example.com nebo všechny subdomény)**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-222">Make sure that **Hostname record type** is set too**CNAME record (www.example.com or any subdomain)**.</span></span>

<span data-ttu-id="dd9f6-223">Vyberte **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-223">Select **Add hostname**.</span></span>

![Přidat aplikaci toohello název DNS](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

<span data-ttu-id="dd9f6-225">Může trvat nějakou dobu hello nový název hostitele toobe promítnuta aplikace hello **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-225">It might take some time for hello new hostname toobe reflected in hello app's **Custom domains** page.</span></span> <span data-ttu-id="dd9f6-226">Aktualizujte data hello tooupdate hello prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-226">Try refreshing hello browser tooupdate hello data.</span></span>

<span data-ttu-id="dd9f6-227">Vyberte hello  **+**  ikonu znovu tooadd jiný název hostitele, odpovídající doméně hello zástupný znak.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-227">Select hello **+** icon again tooadd another hostname that matches hello wildcard domain.</span></span> <span data-ttu-id="dd9f6-228">Například přidejte `sub2.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-228">For example, add `sub2.contoso.com`.</span></span>

![Přidat záznam CNAME](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a><span data-ttu-id="dd9f6-230">Testování v prohlížeči</span><span class="sxs-lookup"><span data-stu-id="dd9f6-230">Test in browser</span></span>

<span data-ttu-id="dd9f6-231">Procházet toohello názvy DNS, který jste nakonfigurovali dříve (například `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, a `sub2.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-231">Browse toohello DNS name(s) that you configured earlier (for example, `contoso.com`,  `www.contoso.com`, `sub1.contoso.com`, and `sub2.contoso.com`).</span></span>

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="automate-with-scripts"></a><span data-ttu-id="dd9f6-233">Automatizovat pomocí skriptů</span><span class="sxs-lookup"><span data-stu-id="dd9f6-233">Automate with scripts</span></span>

<span data-ttu-id="dd9f6-234">Můžete automatizovat správu vlastních domén s skripty, pomocí hello [rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli) nebo [prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-234">You can automate management of custom domains with scripts, using hello [Azure CLI](/cli/azure/install-azure-cli) or [Azure PowerShell](/powershell/azure/overview).</span></span> 

### <a name="azure-cli"></a><span data-ttu-id="dd9f6-235">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="dd9f6-235">Azure CLI</span></span> 

<span data-ttu-id="dd9f6-236">Hello následující příkaz přidá nakonfigurované vlastní DNS název tooan aplikaci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-236">hello following command adds a configured custom DNS name tooan App Service app.</span></span> 

```bash 
az appservice web config hostname add \
    --webapp <app_name> \
    --resource-group <resource_group_name> \ 
    --name <fully_qualified_domain_name> 
``` 

<span data-ttu-id="dd9f6-237">Další informace najdete v tématu [namapovat vlastní doménu tooa webové aplikace](scripts/app-service-cli-configure-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-237">For more information, see [Map a custom domain tooa web app](scripts/app-service-cli-configure-custom-domain.md).</span></span> 

### <a name="azure-powershell"></a><span data-ttu-id="dd9f6-238">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="dd9f6-238">Azure PowerShell</span></span> 

<span data-ttu-id="dd9f6-239">Hello následující příkaz přidá nakonfigurované vlastní DNS název tooan aplikaci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-239">hello following command adds a configured custom DNS name tooan App Service app.</span></span> 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

<span data-ttu-id="dd9f6-240">Další informace najdete v tématu [přiřadit vlastní domény tooa webové aplikace](scripts/app-service-powershell-configure-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="dd9f6-240">For more information, see [Assign a custom domain tooa web app](scripts/app-service-powershell-configure-custom-domain.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd9f6-241">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd9f6-241">Next steps</span></span>

<span data-ttu-id="dd9f6-242">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="dd9f6-242">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dd9f6-243">Mapa subdoména pomocí záznamu CNAME</span><span class="sxs-lookup"><span data-stu-id="dd9f6-243">Map a subdomain by using a CNAME record</span></span>
> * <span data-ttu-id="dd9f6-244">Mapa kořenové domény pomocí záznamu a.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-244">Map a root domain by using an A record</span></span>
> * <span data-ttu-id="dd9f6-245">Mapa zástupné domény pomocí záznamu CNAME</span><span class="sxs-lookup"><span data-stu-id="dd9f6-245">Map a wildcard domain by using a CNAME record</span></span>
> * <span data-ttu-id="dd9f6-246">Automatizovat mapování domény pomocí skriptů</span><span class="sxs-lookup"><span data-stu-id="dd9f6-246">Automate domain mapping with scripts</span></span>

<span data-ttu-id="dd9f6-247">Posunutí toohello další kurz toolearn jak toobind vlastní SSL certifikát tooa webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd9f6-247">Advance toohello next tutorial toolearn how toobind a custom SSL certificate tooa web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="dd9f6-248">Vytvořit vazbu existující vlastní SSL certifikát tooAzure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="dd9f6-248">Bind an existing custom SSL certificate tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-ssl.md)
