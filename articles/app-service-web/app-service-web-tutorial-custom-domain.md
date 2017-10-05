---
title: "Mapovat existující vlastní název DNS pro službu Azure Web Apps | Microsoft Docs"
description: "Naučte se přidávat existující vlastní název domény DNS (jednoduché doména) pro webovou aplikaci, back-end mobilní aplikace nebo aplikace API v Azure App Service."
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
ms.openlocfilehash: 973cda462e8d258cc848e1036891c7f8af043102
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="map-an-existing-custom-dns-name-to-azure-web-apps"></a><span data-ttu-id="3c2d3-103">Mapovat existující vlastní název DNS pro službu Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="3c2d3-103">Map an existing custom DNS name to Azure Web Apps</span></span>

<span data-ttu-id="3c2d3-104">[Azure Web Apps](app-service-web-overview.md) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-104">[Azure Web Apps](app-service-web-overview.md) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="3c2d3-105">V tomto kurzu se dozvíte, jak mapovat existující vlastní název DNS Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-105">This tutorial shows you how to map an existing custom DNS name to Azure Web Apps.</span></span>

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

<span data-ttu-id="3c2d3-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="3c2d3-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3c2d3-108">Mapování subdoména (například `www.contoso.com`) pomocí záznamu CNAME</span><span class="sxs-lookup"><span data-stu-id="3c2d3-108">Map a subdomain (for example, `www.contoso.com`) by using a CNAME record</span></span>
> * <span data-ttu-id="3c2d3-109">Mapování kořenové domény (například `contoso.com`) pomocí záznamu a.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-109">Map a root domain (for example, `contoso.com`) by using an A record</span></span>
> * <span data-ttu-id="3c2d3-110">Namapovat doménu zástupných znaků (například `*.contoso.com`) pomocí záznamu CNAME</span><span class="sxs-lookup"><span data-stu-id="3c2d3-110">Map a wildcard domain (for example, `*.contoso.com`) by using a CNAME record</span></span>
> * <span data-ttu-id="3c2d3-111">Automatizovat mapování domény pomocí skriptů</span><span class="sxs-lookup"><span data-stu-id="3c2d3-111">Automate domain mapping with scripts</span></span>

<span data-ttu-id="3c2d3-112">Můžete použít buď **záznam CNAME** nebo **záznam** mapovat vlastní název DNS služby App Service.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-112">You can use either a **CNAME record** or an **A record** to map a custom DNS name to App Service.</span></span> 

> [!NOTE]
> <span data-ttu-id="3c2d3-113">Doporučujeme použít záznam CNAME pro všechny vlastní názvy DNS kromě kořenové domény (například `contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-113">We recommend that you use a CNAME for all custom DNS names except a root domain (for example, `contoso.com`).</span></span>

<span data-ttu-id="3c2d3-114">K migraci za provozu lokality a jeho název domény DNS do služby App Service, v tématu [migrovat aktivním názvem DNS do služby Azure App Service](app-service-custom-domain-name-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-114">To migrate a live site and its DNS domain name to App Service, see [Migrate an active DNS name to Azure App Service](app-service-custom-domain-name-migrate.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c2d3-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3c2d3-115">Prerequisites</span></span>

<span data-ttu-id="3c2d3-116">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="3c2d3-116">To complete this tutorial:</span></span>

* <span data-ttu-id="3c2d3-117">[Vytvořit aplikaci služby App Service](/azure/app-service/), nebo použijte aplikaci, kterou jste vytvořili pro jiné kurzu.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-117">[Create an App Service app](/azure/app-service/), or use an app that you created for another tutorial.</span></span>
* <span data-ttu-id="3c2d3-118">Zakupte název domény a ujistěte se, že máte přístup do registru DNS u svého poskytovatele domény (například GoDaddy).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-118">Purchase a domain name and make sure you have access to the DNS registry for your domain provider (such as GoDaddy).</span></span>

  <span data-ttu-id="3c2d3-119">Chcete-li například přidat záznamy DNS pro `contoso.com` a `www.contoso.com`, musí být schopný nakonfigurovat nastavení DNS `contoso.com` kořenové domény.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-119">For example, to add DNS entries for `contoso.com` and `www.contoso.com`, you must be able to configure the DNS settings for the `contoso.com` root domain.</span></span>

  > [!NOTE]
  > <span data-ttu-id="3c2d3-120">Pokud nemáte existující domény název, vezměte v úvahu [zakoupení domény pomocí portálu Azure](custom-dns-web-site-buydomains-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-120">If you don't have an existing domain name, consider [purchasing a domain using the Azure portal](custom-dns-web-site-buydomains-web-app.md).</span></span> 

## <a name="prepare-the-app"></a><span data-ttu-id="3c2d3-121">Příprava aplikace</span><span class="sxs-lookup"><span data-stu-id="3c2d3-121">Prepare the app</span></span>

<span data-ttu-id="3c2d3-122">Pro mapování názvu DNS do webové aplikace, webové aplikace na [plán služby App Service](https://azure.microsoft.com/pricing/details/app-service/) musí být placené vrstvy (**sdílené**, **základní**, **standardní**, nebo  **Premium**).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-122">To map a custom DNS name to a web app, the web app's [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be a paid tier (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> <span data-ttu-id="3c2d3-123">V tomto kroku je třeba zkontrolovat, že aplikace App Service je v podporovaném cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-123">In this step, you make sure that the App Service app is in the supported pricing tier.</span></span>

### <a name="sign-in-to-azure"></a><span data-ttu-id="3c2d3-124">Přihlášení k Azure</span><span class="sxs-lookup"><span data-stu-id="3c2d3-124">Sign in to Azure</span></span>

<span data-ttu-id="3c2d3-125">Otevřete [portál Azure](https://portal.azure.com) a přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-125">Open the [Azure portal](https://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="navigate-to-the-app-in-the-azure-portal"></a><span data-ttu-id="3c2d3-126">Přejděte na aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3c2d3-126">Navigate to the app in the Azure portal</span></span>

<span data-ttu-id="3c2d3-127">V nabídce vlevo vyberte **App Services**a pak vyberte název aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-127">From the left menu, select **App Services**, and then select the name of the app.</span></span>

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/select-app.png)

<span data-ttu-id="3c2d3-129">Zobrazí na stránce Správa aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-129">You see the management page of the App Service app.</span></span>  

<a name="checkpricing"></a>

### <a name="check-the-pricing-tier"></a><span data-ttu-id="3c2d3-130">Zkontrolujte cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="3c2d3-130">Check the pricing tier</span></span>

<span data-ttu-id="3c2d3-131">V levém navigačním panelu stránku aplikace, přejděte na **nastavení** a vyberte **škálování (plán služby App Service)**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-131">In the left navigation of the app page, scroll to the **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Škálování nabídky](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

<span data-ttu-id="3c2d3-133">Aktuální úroveň aplikace je označený modré ohraničení.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-133">The app's current tier is highlighted by a blue border.</span></span> <span data-ttu-id="3c2d3-134">Zkontrolujte, zda není v aplikaci **volné** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-134">Check to make sure that the app is not in the **Free** tier.</span></span> <span data-ttu-id="3c2d3-135">Vlastní DNS není podporována v **volné** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-135">Custom DNS is not supported in the **Free** tier.</span></span> 

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

<span data-ttu-id="3c2d3-137">Pokud plán služby App Service není **volné**ukončit **zvolte cenovou úroveň** stránky a přejít na [mapování záznam CNAME](#cname).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-137">If the App Service plan is not **Free**, close the **Choose your pricing tier** page and skip to [Map a CNAME record](#cname).</span></span>

<a name="scaleup"></a>

### <a name="scale-up-the-app-service-plan"></a><span data-ttu-id="3c2d3-138">Škálovat plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="3c2d3-138">Scale up the App Service plan</span></span>

<span data-ttu-id="3c2d3-139">Vyberte některé z vrstvy a bezplatnou (**sdílené**, **základní**, **standardní**, nebo **Premium**).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-139">Select any of the non-free tiers (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> 

<span data-ttu-id="3c2d3-140">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-140">Click **Select**.</span></span>

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

<span data-ttu-id="3c2d3-142">Když se zobrazí následující oznámení, je dokončena operace škálování.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-142">When you see the following notification, the scale operation is complete.</span></span>

![Potvrzení operace škálování](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a><span data-ttu-id="3c2d3-144">Mapa záznam CNAME</span><span class="sxs-lookup"><span data-stu-id="3c2d3-144">Map a CNAME record</span></span>

<span data-ttu-id="3c2d3-145">V kurzu příkladu přidat záznam CNAME pro `www` subdomény (například `www.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-145">In the tutorial example, you add a CNAME record for the `www` subdomain (for example, `www.contoso.com`).</span></span>

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a><span data-ttu-id="3c2d3-146">Vytvořit záznam CNAME</span><span class="sxs-lookup"><span data-stu-id="3c2d3-146">Create the CNAME record</span></span>

<span data-ttu-id="3c2d3-147">Přidejte záznam CNAME pro mapování subdoména k výchozí hostitele aplikace (`<app_name>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-147">Add a CNAME record to map a subdomain to the app's default hostname (`<app_name>.azurewebsites.net`).</span></span>

<span data-ttu-id="3c2d3-148">Pro `www.contoso.com` příklad domény, přidejte záznam CNAME, který mapuje název `www` k `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-148">For the `www.contoso.com` domain example, add a CNAME record that maps the name `www` to `<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="3c2d3-149">Po přidání CNAME stránky záznamů DNS vypadá jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="3c2d3-149">After you add the CNAME, the DNS records page looks like the following example:</span></span>

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-the-cname-record-mapping-in-azure"></a><span data-ttu-id="3c2d3-151">Povolit mapování záznam CNAME ve službě Azure</span><span class="sxs-lookup"><span data-stu-id="3c2d3-151">Enable the CNAME record mapping in Azure</span></span>

<span data-ttu-id="3c2d3-152">V levém navigačním panelu stránku aplikace v portálu Azure, vyberte **vlastní domény**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-152">In the left navigation of the app page in the Azure portal, select **Custom domains**.</span></span> 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="3c2d3-154">V **vlastní domény** stránku aplikace a přidat vlastní plně kvalifikovaný název DNS (`www.contoso.com`) do seznamu.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-154">In the **Custom domains** page of the app, add the fully qualified custom DNS name (`www.contoso.com`) to the list.</span></span>

<span data-ttu-id="3c2d3-155">Vyberte  **+**  ikonu vedle **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-155">Select the **+** icon next to **Add hostname**.</span></span>

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="3c2d3-157">Zadejte název plně kvalifikované domény přidat záznam CNAME pro, jako například `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-157">Type the fully qualified domain name that you added a CNAME record for, such as `www.contoso.com`.</span></span> 

<span data-ttu-id="3c2d3-158">Vyberte **ověření**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-158">Select **Validate**.</span></span>

<span data-ttu-id="3c2d3-159">**Přidat název hostitele** se aktivuje tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-159">The **Add hostname** button is activated.</span></span> 

<span data-ttu-id="3c2d3-160">Ujistěte se, že **typ záznamu Hostname** je nastaven na **CNAME (www.example.com nebo všechny subdomény)**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-160">Make sure that **Hostname record type** is set to **CNAME (www.example.com or any subdomain)**.</span></span>

<span data-ttu-id="3c2d3-161">Vyberte **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-161">Select **Add hostname**.</span></span>

![Přidání názvu DNS do aplikace](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

<span data-ttu-id="3c2d3-163">Může trvat nějakou dobu, než nový název hostitele v aplikaci projeví **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-163">It might take some time for the new hostname to be reflected in the app's **Custom domains** page.</span></span> <span data-ttu-id="3c2d3-164">Aktualizujte prohlížeč aktualizovat data.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-164">Try refreshing the browser to update the data.</span></span>

![Přidat záznam CNAME](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

<span data-ttu-id="3c2d3-166">Pokud je vynechán krok nebo provedené překlepem někde dříve, se zobrazí chyba ověření v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-166">If you missed a step or made a typo somewhere earlier, you see a verification error at the bottom of the page.</span></span>

![Chyba ověření](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a><span data-ttu-id="3c2d3-168">Mapa záznamu a.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-168">Map an A record</span></span>

<span data-ttu-id="3c2d3-169">V kurzu příkladu přidáte záznamu A pro kořenové domény (například `contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-169">In the tutorial example, you add an A record for the root domain (for example, `contoso.com`).</span></span> 

<a name="info"></a>

### <a name="copy-the-apps-ip-address"></a><span data-ttu-id="3c2d3-170">Zkopírujte adresu IP aplikace</span><span class="sxs-lookup"><span data-stu-id="3c2d3-170">Copy the app's IP address</span></span>

<span data-ttu-id="3c2d3-171">Pokud chcete mapovat záznam A, musíte aplikace externí IP adresu.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-171">To map an A record, you need the app's external IP address.</span></span> <span data-ttu-id="3c2d3-172">Tuto IP adresu můžete najít v dané aplikaci **vlastní domény** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-172">You can find this IP address in the app's **Custom domains** page in the Azure portal.</span></span>

<span data-ttu-id="3c2d3-173">V levém navigačním panelu stránku aplikace v portálu Azure, vyberte **vlastní domény**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-173">In the left navigation of the app page in the Azure portal, select **Custom domains**.</span></span> 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="3c2d3-175">V **vlastní domény** stránky, zkopírujte aplikace IP adresu.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-175">In the **Custom domains** page, copy the app's IP address.</span></span>

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-a-record"></a><span data-ttu-id="3c2d3-177">Vytvořte záznam a.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-177">Create the A record</span></span>

<span data-ttu-id="3c2d3-178">Pro mapování záznam do aplikace, služby App Service vyžaduje **dva** záznamy DNS:</span><span class="sxs-lookup"><span data-stu-id="3c2d3-178">To map an A record to an app, App Service requires **two** DNS records:</span></span>

- <span data-ttu-id="3c2d3-179">**A** záznam aplikace IP adresu.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-179">An **A** record to map to the app's IP address.</span></span>
- <span data-ttu-id="3c2d3-180">A **TXT** záznam k mapování na název hostitele výchozí aplikace `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-180">A **TXT** record to map to the app's default hostname `<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="3c2d3-181">Služby App Service používá tento záznam pouze v době konfigurace, chcete-li ověřit, že vlastníte vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-181">App Service uses this record only at configuration time, to verify that you own the custom domain.</span></span> <span data-ttu-id="3c2d3-182">Jakmile vaši vlastní doménu ověřit a nakonfigurovat ve službě App Service, můžete odstranit tento záznam TXT.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-182">After your custom domain is validated and configured in App Service, you can delete this TXT record.</span></span> 

<span data-ttu-id="3c2d3-183">Pro `contoso.com` například domény vytvořit záznamy A a TXT podle následující tabulky (`@` obvykle představuje kořenové domény).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-183">For the `contoso.com` domain example, create the A and TXT records according to the following table (`@` typically represents the root domain).</span></span> 

| <span data-ttu-id="3c2d3-184">Typ záznamu</span><span class="sxs-lookup"><span data-stu-id="3c2d3-184">Record type</span></span> | <span data-ttu-id="3c2d3-185">Hostitel</span><span class="sxs-lookup"><span data-stu-id="3c2d3-185">Host</span></span> | <span data-ttu-id="3c2d3-186">Hodnota</span><span class="sxs-lookup"><span data-stu-id="3c2d3-186">Value</span></span> |
| - | - | - |
| <span data-ttu-id="3c2d3-187">A</span><span class="sxs-lookup"><span data-stu-id="3c2d3-187">A</span></span> | `@` | <span data-ttu-id="3c2d3-188">IP adresu z [zkopírujte aplikace IP adresu](#info)</span><span class="sxs-lookup"><span data-stu-id="3c2d3-188">IP address from [Copy the app's IP address](#info)</span></span> |
| <span data-ttu-id="3c2d3-189">TXT</span><span class="sxs-lookup"><span data-stu-id="3c2d3-189">TXT</span></span> | `@` | `<app_name>.azurewebsites.net` |

<span data-ttu-id="3c2d3-190">Když se přidají záznamy, vypadá stránky záznamů DNS v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="3c2d3-190">When the records are added, the DNS records page looks like the following example:</span></span>

![Stránky záznamů DNS](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-the-a-record-mapping-in-the-app"></a><span data-ttu-id="3c2d3-192">Povolit záznam mapování A v aplikaci</span><span class="sxs-lookup"><span data-stu-id="3c2d3-192">Enable the A record mapping in the app</span></span>

<span data-ttu-id="3c2d3-193">Zpět v dané aplikaci **vlastní domény** na portálu Azure, přidejte vlastní plně kvalifikovaný název DNS (například `contoso.com`) do seznamu.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-193">Back in the app's **Custom domains** page in the Azure portal, add the fully qualified custom DNS name (for example, `contoso.com`) to the list.</span></span>

<span data-ttu-id="3c2d3-194">Vyberte  **+**  ikonu vedle **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-194">Select the **+** icon next to **Add hostname**.</span></span>

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

<span data-ttu-id="3c2d3-196">Zadejte název plně kvalifikované domény nakonfigurovat záznam A, jako například `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-196">Type the fully qualified domain name that you configured the A record for, such as `contoso.com`.</span></span>

<span data-ttu-id="3c2d3-197">Vyberte **ověření**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-197">Select **Validate**.</span></span>

<span data-ttu-id="3c2d3-198">**Přidat název hostitele** se aktivuje tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-198">The **Add hostname** button is activated.</span></span> 

<span data-ttu-id="3c2d3-199">Ujistěte se, že **typ záznamu Hostname** je nastaven na **záznam (example.com)**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-199">Make sure that **Hostname record type** is set to **A record (example.com)**.</span></span>

<span data-ttu-id="3c2d3-200">Vyberte **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-200">Select **Add hostname**.</span></span>

![Přidání názvu DNS do aplikace](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

<span data-ttu-id="3c2d3-202">Může trvat nějakou dobu, než nový název hostitele v aplikaci projeví **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-202">It might take some time for the new hostname to be reflected in the app's **Custom domains** page.</span></span> <span data-ttu-id="3c2d3-203">Aktualizujte prohlížeč aktualizovat data.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-203">Try refreshing the browser to update the data.</span></span>

![Přidat záznam](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

<span data-ttu-id="3c2d3-205">Pokud je vynechán krok nebo provedené překlepem někde dříve, se zobrazí chyba ověření v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-205">If you missed a step or made a typo somewhere earlier, you see a verification error at the bottom of the page.</span></span>

![Chyba ověření](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a><span data-ttu-id="3c2d3-207">Namapovat doménu zástupný znak</span><span class="sxs-lookup"><span data-stu-id="3c2d3-207">Map a wildcard domain</span></span>

<span data-ttu-id="3c2d3-208">V kurzu příkladu se mapují [název DNS zástupný znak](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (například `*.contoso.com`) do aplikace služby App Service přidáním záznam CNAME.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-208">In the tutorial example, you map a [wildcard DNS name](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (for example, `*.contoso.com`) to the App Service app by adding a CNAME record.</span></span> 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a><span data-ttu-id="3c2d3-209">Vytvořit záznam CNAME</span><span class="sxs-lookup"><span data-stu-id="3c2d3-209">Create the CNAME record</span></span>

<span data-ttu-id="3c2d3-210">Přidejte záznam CNAME pro mapování názvu zástupné k výchozí hostitele aplikace (`<app_name>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-210">Add a CNAME record to map a wildcard name to the app's default hostname (`<app_name>.azurewebsites.net`).</span></span>

<span data-ttu-id="3c2d3-211">Pro `*.contoso.com` například domény záznam CNAME se mapování názvu `*` k `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-211">For the `*.contoso.com` domain example, the CNAME record will map the name `*` to `<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="3c2d3-212">Při přidání CNAME stránky záznamů DNS vypadá jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="3c2d3-212">When the CNAME is added, the DNS records page looks like the following example:</span></span>

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-the-cname-record-mapping-in-the-app"></a><span data-ttu-id="3c2d3-214">Povolit mapování záznam CNAME v aplikaci</span><span class="sxs-lookup"><span data-stu-id="3c2d3-214">Enable the CNAME record mapping in the app</span></span>

<span data-ttu-id="3c2d3-215">Nyní můžete přidat všechny subdomény, která odpovídá názvu zástupné k aplikaci (například `sub1.contoso.com` a `sub2.contoso.com` odpovídat `*.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-215">You can now add any subdomain that matches the wildcard name to the app (for example, `sub1.contoso.com` and `sub2.contoso.com` match `*.contoso.com`).</span></span> 

<span data-ttu-id="3c2d3-216">V levém navigačním panelu stránku aplikace v portálu Azure, vyberte **vlastní domény**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-216">In the left navigation of the app page in the Azure portal, select **Custom domains**.</span></span> 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="3c2d3-218">Vyberte  **+**  ikonu vedle **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-218">Select the **+** icon next to **Add hostname**.</span></span>

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="3c2d3-220">Zadejte název plně kvalifikované domény, který odpovídá domény zástupných znaků (například `sub1.contoso.com`) a potom vyberte **ověřením**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-220">Type a fully qualified domain name that matches the wildcard domain (for example, `sub1.contoso.com`), and then select **Validate**.</span></span>

<span data-ttu-id="3c2d3-221">**Přidat název hostitele** se aktivuje tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-221">The **Add hostname** button is activated.</span></span> 

<span data-ttu-id="3c2d3-222">Ujistěte se, že **typ záznamu Hostname** je nastaven na **záznam CNAME (www.example.com nebo všechny subdomény)**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-222">Make sure that **Hostname record type** is set to **CNAME record (www.example.com or any subdomain)**.</span></span>

<span data-ttu-id="3c2d3-223">Vyberte **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-223">Select **Add hostname**.</span></span>

![Přidání názvu DNS do aplikace](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

<span data-ttu-id="3c2d3-225">Může trvat nějakou dobu, než nový název hostitele v aplikaci projeví **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-225">It might take some time for the new hostname to be reflected in the app's **Custom domains** page.</span></span> <span data-ttu-id="3c2d3-226">Aktualizujte prohlížeč aktualizovat data.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-226">Try refreshing the browser to update the data.</span></span>

<span data-ttu-id="3c2d3-227">Vyberte  **+**  ikonu Přidat jiné název hostitele, který odpovídá zástupné domény.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-227">Select the **+** icon again to add another hostname that matches the wildcard domain.</span></span> <span data-ttu-id="3c2d3-228">Například přidejte `sub2.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-228">For example, add `sub2.contoso.com`.</span></span>

![Přidat záznam CNAME](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a><span data-ttu-id="3c2d3-230">Testování v prohlížeči</span><span class="sxs-lookup"><span data-stu-id="3c2d3-230">Test in browser</span></span>

<span data-ttu-id="3c2d3-231">Přejděte na názvy DNS, který jste nakonfigurovali dříve (například `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, a `sub2.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-231">Browse to the DNS name(s) that you configured earlier (for example, `contoso.com`,  `www.contoso.com`, `sub1.contoso.com`, and `sub2.contoso.com`).</span></span>

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="automate-with-scripts"></a><span data-ttu-id="3c2d3-233">Automatizovat pomocí skriptů</span><span class="sxs-lookup"><span data-stu-id="3c2d3-233">Automate with scripts</span></span>

<span data-ttu-id="3c2d3-234">Můžete automatizovat správu vlastních domén s skripty, pomocí [rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli) nebo [prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-234">You can automate management of custom domains with scripts, using the [Azure CLI](/cli/azure/install-azure-cli) or [Azure PowerShell](/powershell/azure/overview).</span></span> 

### <a name="azure-cli"></a><span data-ttu-id="3c2d3-235">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3c2d3-235">Azure CLI</span></span> 

<span data-ttu-id="3c2d3-236">Následující příkaz přidá do aplikace služby App Service nakonfigurované vlastní název DNS.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-236">The following command adds a configured custom DNS name to an App Service app.</span></span> 

```bash 
az appservice web config hostname add \
    --webapp <app_name> \
    --resource-group <resource_group_name> \ 
    --name <fully_qualified_domain_name> 
``` 

<span data-ttu-id="3c2d3-237">Další informace najdete v tématu [namapovat vlastní doménu do webové aplikace](scripts/app-service-cli-configure-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-237">For more information, see [Map a custom domain to a web app](scripts/app-service-cli-configure-custom-domain.md).</span></span> 

### <a name="azure-powershell"></a><span data-ttu-id="3c2d3-238">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c2d3-238">Azure PowerShell</span></span> 

<span data-ttu-id="3c2d3-239">Následující příkaz přidá do aplikace služby App Service nakonfigurované vlastní název DNS.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-239">The following command adds a configured custom DNS name to an App Service app.</span></span> 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

<span data-ttu-id="3c2d3-240">Další informace najdete v tématu [přiřadit vlastní doménu do webové aplikace](scripts/app-service-powershell-configure-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="3c2d3-240">For more information, see [Assign a custom domain to a web app](scripts/app-service-powershell-configure-custom-domain.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c2d3-241">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c2d3-241">Next steps</span></span>

<span data-ttu-id="3c2d3-242">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="3c2d3-242">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3c2d3-243">Mapa subdoména pomocí záznamu CNAME</span><span class="sxs-lookup"><span data-stu-id="3c2d3-243">Map a subdomain by using a CNAME record</span></span>
> * <span data-ttu-id="3c2d3-244">Mapa kořenové domény pomocí záznamu a.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-244">Map a root domain by using an A record</span></span>
> * <span data-ttu-id="3c2d3-245">Mapa zástupné domény pomocí záznamu CNAME</span><span class="sxs-lookup"><span data-stu-id="3c2d3-245">Map a wildcard domain by using a CNAME record</span></span>
> * <span data-ttu-id="3c2d3-246">Automatizovat mapování domény pomocí skriptů</span><span class="sxs-lookup"><span data-stu-id="3c2d3-246">Automate domain mapping with scripts</span></span>

<span data-ttu-id="3c2d3-247">Přechodu na v dalším kurzu se dozvíte, jak vytvořit vazbu vlastní certifikát SSL pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3c2d3-247">Advance to the next tutorial to learn how to bind a custom SSL certificate to a web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3c2d3-248">Vytvoření vazby stávající vlastní certifikát SSL pro službu Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="3c2d3-248">Bind an existing custom SSL certificate to Azure Web Apps</span></span>](app-service-web-tutorial-custom-ssl.md)
