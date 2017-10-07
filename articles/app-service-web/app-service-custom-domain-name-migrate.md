---
title: "aaaMigrate active DNS název tooAzure služby App Service | Microsoft Docs"
description: "Zjistěte, jak toomigrate vlastní název domény DNS, který je již přiřazen tooa live lokality tooAzure služby App Service bez odstávky."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
tags: top-support-issue
ms.assetid: 10da5b8a-1823-41a3-a2ff-a0717c2b5c2d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: cephalin
ms.openlocfilehash: fbc4cc30dcb87efb2e867cb65c5404b667661e62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-an-active-dns-name-tooazure-app-service"></a><span data-ttu-id="004a3-103">Migrace aktivní tooAzure název DNS služby App Service</span><span class="sxs-lookup"><span data-stu-id="004a3-103">Migrate an active DNS name tooAzure App Service</span></span>

<span data-ttu-id="004a3-104">Tento článek ukazuje, jak toomigrate active DNS název příliš[Azure App Service](../app-service/app-service-value-prop-what-is.md) bez odstávky.</span><span class="sxs-lookup"><span data-stu-id="004a3-104">This article shows you how toomigrate an active DNS name too[Azure App Service](../app-service/app-service-value-prop-what-is.md) without any downtime.</span></span>

<span data-ttu-id="004a3-105">Při migraci za provozu lokality a jeho DNS domény název tooApp služby, je tento název DNS obsluhující již provoz za provozu.</span><span class="sxs-lookup"><span data-stu-id="004a3-105">When you migrate a live site and its DNS domain name tooApp Service, that DNS name is already serving live traffic.</span></span> <span data-ttu-id="004a3-106">Výpadek při překlad DNS během migrace hello vyhnete tak, že ho preventivně vazby hello active DNS název tooyour aplikaci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="004a3-106">You can avoid downtime in DNS resolution during hello migration by binding hello active DNS name tooyour App Service app preemptively.</span></span>

<span data-ttu-id="004a3-107">Pokud si nejste obavy o výpadek při překlad názvů DNS, najdete v části [mapovat existující vlastní DNS název tooAzure webové aplikace](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="004a3-107">If you're not worried about downtime in DNS resolution, see [Map an existing custom DNS name tooAzure Web Apps](app-service-web-tutorial-custom-domain.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="004a3-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="004a3-108">Prerequisites</span></span>

<span data-ttu-id="004a3-109">toocomplete tento postup:</span><span class="sxs-lookup"><span data-stu-id="004a3-109">toocomplete this how-to:</span></span>

- <span data-ttu-id="004a3-110">[Ujistěte se, že aplikaci aplikační služby není v úrovni FREE](app-service-web-tutorial-custom-domain.md#checkpricing).</span><span class="sxs-lookup"><span data-stu-id="004a3-110">[Make sure that your App Service app is not in FREE tier](app-service-web-tutorial-custom-domain.md#checkpricing).</span></span>

## <a name="bind-hello-domain-name-preemptively"></a><span data-ttu-id="004a3-111">Název domény hello ho preventivně vazby</span><span class="sxs-lookup"><span data-stu-id="004a3-111">Bind hello domain name preemptively</span></span>

<span data-ttu-id="004a3-112">Pokud jste ho preventivně vytvořit vazbu vlastní doménu, musíte provést obě následující hello před provedením jakýchkoli změn se záznamy DNS:</span><span class="sxs-lookup"><span data-stu-id="004a3-112">When you bind a custom domain preemptively, you accomplish both of hello following before making any changes to your DNS records:</span></span>

- <span data-ttu-id="004a3-113">Ověřit vlastnictví domény</span><span class="sxs-lookup"><span data-stu-id="004a3-113">Verify domain ownership</span></span>
- <span data-ttu-id="004a3-114">Povolit hello název domény pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="004a3-114">Enable hello domain name for your app</span></span>

<span data-ttu-id="004a3-115">Při migraci nakonec váš vlastní název DNS ze staré lokality toohello hello aplikaci služby App Service, budou v překlad DNS existovat nedojde k žádnému výpadku.</span><span class="sxs-lookup"><span data-stu-id="004a3-115">When you finally migrate your custom DNS name from hello old site toohello App Service app, there will be no downtime in DNS resolution.</span></span>

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a><span data-ttu-id="004a3-116">Vytvořit záznam ověření domény</span><span class="sxs-lookup"><span data-stu-id="004a3-116">Create domain verification record</span></span>

<span data-ttu-id="004a3-117">vlastnictví domény s tooverify, zaznamenejte přidat TXT.</span><span class="sxs-lookup"><span data-stu-id="004a3-117">tooverify domain ownership, Add a TXT record.</span></span> <span data-ttu-id="004a3-118">Hello záznam TXT mapuje z _awverify.&lt; subdoména >_ too_&lt;appname >. azurewebsites.net_.</span><span class="sxs-lookup"><span data-stu-id="004a3-118">hello TXT record maps from _awverify.&lt;subdomain>_ too_&lt;appname>.azurewebsites.net_.</span></span> 

<span data-ttu-id="004a3-119">Hello záznam TXT, které potřebujete, závisí na hello chcete toomigrate záznam DNS.</span><span class="sxs-lookup"><span data-stu-id="004a3-119">hello TXT record you need depends on hello DNS record you want toomigrate.</span></span> <span data-ttu-id="004a3-120">Příklady najdete v tématu hello následující tabulka (`@` obvykle představuje hello kořenové domény):</span><span class="sxs-lookup"><span data-stu-id="004a3-120">For examples, see hello following table (`@` typically represents hello root domain):</span></span>  

| <span data-ttu-id="004a3-121">Příklad záznamu DNS</span><span class="sxs-lookup"><span data-stu-id="004a3-121">DNS record example</span></span> | <span data-ttu-id="004a3-122">TXT hostitele</span><span class="sxs-lookup"><span data-stu-id="004a3-122">TXT Host</span></span> | <span data-ttu-id="004a3-123">Hodnota TXT</span><span class="sxs-lookup"><span data-stu-id="004a3-123">TXT Value</span></span> |
| - | - | - |
| <span data-ttu-id="004a3-124">@ (uživatel root)</span><span class="sxs-lookup"><span data-stu-id="004a3-124">@ (root)</span></span> | <span data-ttu-id="004a3-125">_awverify_</span><span class="sxs-lookup"><span data-stu-id="004a3-125">_awverify_</span></span> | <span data-ttu-id="004a3-126">_&lt;AppName >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="004a3-126">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="004a3-127">WWW (pod)</span><span class="sxs-lookup"><span data-stu-id="004a3-127">www (sub)</span></span> | <span data-ttu-id="004a3-128">_awverify.www_</span><span class="sxs-lookup"><span data-stu-id="004a3-128">_awverify.www_</span></span> | <span data-ttu-id="004a3-129">_&lt;AppName >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="004a3-129">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="004a3-130">\*(zástupný znak)</span><span class="sxs-lookup"><span data-stu-id="004a3-130">\* (wildcard)</span></span> | <span data-ttu-id="004a3-131">_awverify.\*_</span><span class="sxs-lookup"><span data-stu-id="004a3-131">_awverify.\*_</span></span> | <span data-ttu-id="004a3-132">_&lt;AppName >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="004a3-132">_&lt;appname>.azurewebsites.net_</span></span> |

<span data-ttu-id="004a3-133">Poznamenejte si hello typ záznamu hello název DNS, které chcete toomigrate v stránku záznamů DNS.</span><span class="sxs-lookup"><span data-stu-id="004a3-133">In your DNS records page, note hello record type of hello DNS name you want toomigrate.</span></span> <span data-ttu-id="004a3-134">App Service podporuje mapování z CNAME a záznamy.</span><span class="sxs-lookup"><span data-stu-id="004a3-134">App Service supports mappings from CNAME and A records.</span></span>

### <a name="enable-hello-domain-for-your-app"></a><span data-ttu-id="004a3-135">Povolit hello domény pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="004a3-135">Enable hello domain for your app</span></span>

<span data-ttu-id="004a3-136">V hello [portál Azure](https://portal.azure.com), v levém navigačním stránky aplikace hello hello, vyberte **vlastní domény**.</span><span class="sxs-lookup"><span data-stu-id="004a3-136">In hello [Azure portal](https://portal.azure.com), in hello left navigation of hello app page, select **Custom domains**.</span></span> 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="004a3-138">V hello **vlastní domény** stránky, vyberte hello  **+**  ikonu další příliš**přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="004a3-138">In hello **Custom domains** page, select hello **+** icon next too**Add hostname**.</span></span>

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="004a3-140">Typ hello plně kvalifikovaný název domény přidali hello záznam TXT, jako například `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="004a3-140">Type hello fully qualified domain name that you added hello TXT record for, such as `www.contoso.com`.</span></span> <span data-ttu-id="004a3-141">Pro doménu zástupných znaků (například \*. contoso.com), můžete použít libovolný název DNS, který odpovídá hello zástupné domény.</span><span class="sxs-lookup"><span data-stu-id="004a3-141">For a wildcard domain (like \*.contoso.com), you can use any DNS name that matches hello wildcard domain.</span></span> 

<span data-ttu-id="004a3-142">Vyberte **ověření**.</span><span class="sxs-lookup"><span data-stu-id="004a3-142">Select **Validate**.</span></span>

<span data-ttu-id="004a3-143">Hello **přidat název hostitele** se aktivuje tlačítko.</span><span class="sxs-lookup"><span data-stu-id="004a3-143">hello **Add hostname** button is activated.</span></span> 

<span data-ttu-id="004a3-144">Ujistěte se, že **typ záznamu Hostname** je nastaven typ záznamu toohello DNS chcete toomigrate.</span><span class="sxs-lookup"><span data-stu-id="004a3-144">Make sure that **Hostname record type** is set toohello DNS record type you want toomigrate.</span></span>

<span data-ttu-id="004a3-145">Vyberte **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="004a3-145">Select **Add hostname**.</span></span>

![Přidat aplikaci toohello název DNS](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

<span data-ttu-id="004a3-147">Může trvat nějakou dobu hello nový název hostitele toobe promítnuta aplikace hello **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="004a3-147">It might take some time for hello new hostname toobe reflected in hello app's **Custom domains** page.</span></span> <span data-ttu-id="004a3-148">Aktualizujte data hello tooupdate hello prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="004a3-148">Try refreshing hello browser tooupdate hello data.</span></span>

![Přidat záznam CNAME](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

<span data-ttu-id="004a3-150">Váš vlastní název DNS je teď povolené v aplikaci Azure.</span><span class="sxs-lookup"><span data-stu-id="004a3-150">Your custom DNS name is now enabled in your Azure app.</span></span> 

## <a name="remap-hello-active-dns-name"></a><span data-ttu-id="004a3-151">Přemapování název DNS služby active hello</span><span class="sxs-lookup"><span data-stu-id="004a3-151">Remap hello active DNS name</span></span>

<span data-ttu-id="004a3-152">Hello pouze jedinou levém toodo je přemapování vaše active tooApp toopoint záznamů DNS služby.</span><span class="sxs-lookup"><span data-stu-id="004a3-152">hello only thing left toodo is remapping your active DNS record toopoint tooApp Service.</span></span> <span data-ttu-id="004a3-153">Pravé teď stále odkazuje tooyour staré lokality.</span><span class="sxs-lookup"><span data-stu-id="004a3-153">Right now, it still points tooyour old site.</span></span>

<a name="info"></a>

### <a name="copy-hello-apps-ip-address-a-record-only"></a><span data-ttu-id="004a3-154">Zkopírujte aplikace hello IP adresu (pouze záznam)</span><span class="sxs-lookup"><span data-stu-id="004a3-154">Copy hello app's IP address (A record only)</span></span>

<span data-ttu-id="004a3-155">Pokud jsou přemapování záznam CNAME, tuto část přeskočte.</span><span class="sxs-lookup"><span data-stu-id="004a3-155">If you are remapping a CNAME record, skip this section.</span></span> 

<span data-ttu-id="004a3-156">záznam tooremap A, musíte aplikace App Service hello externí IP adresu, která se zobrazí v hello **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="004a3-156">tooremap an A record, you need hello App Service app's external IP address, which is shown in hello **Custom domains** page.</span></span>

<span data-ttu-id="004a3-157">Zavřít hello **přidat název hostitele** stránky tak, že vyberete **X** v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="004a3-157">Close hello **Add hostname** page by selecting **X** in hello upper-right corner.</span></span> 

<span data-ttu-id="004a3-158">V hello **vlastní domény** stránky, zkopírujte aplikace hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="004a3-158">In hello **Custom domains** page, copy hello app's IP address.</span></span>

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-hello-dns-record"></a><span data-ttu-id="004a3-160">Aktualizace záznamu DNS hello</span><span class="sxs-lookup"><span data-stu-id="004a3-160">Update hello DNS record</span></span>

<span data-ttu-id="004a3-161">Zpět v hello stránky záznamů DNS vaší domény poskytovatele vyberte tooremap záznamů DNS hello.</span><span class="sxs-lookup"><span data-stu-id="004a3-161">Back in hello DNS records page of your domain provider, select hello DNS record tooremap.</span></span>

<span data-ttu-id="004a3-162">Pro hello `contoso.com` kořen domény příklad, přemapovat hello záznam CNAME nebo A jako příklady hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="004a3-162">For hello `contoso.com` root domain example, remap hello A or CNAME record like hello examples in hello following table:</span></span> 

| <span data-ttu-id="004a3-163">Příklad plně kvalifikovaný název domény</span><span class="sxs-lookup"><span data-stu-id="004a3-163">FQDN example</span></span> | <span data-ttu-id="004a3-164">Typ záznamu</span><span class="sxs-lookup"><span data-stu-id="004a3-164">Record type</span></span> | <span data-ttu-id="004a3-165">Hostitel</span><span class="sxs-lookup"><span data-stu-id="004a3-165">Host</span></span> | <span data-ttu-id="004a3-166">Hodnota</span><span class="sxs-lookup"><span data-stu-id="004a3-166">Value</span></span> |
| - | - | - | - |
| <span data-ttu-id="004a3-167">contoso.com (uživatel root)</span><span class="sxs-lookup"><span data-stu-id="004a3-167">contoso.com (root)</span></span> | <span data-ttu-id="004a3-168">A</span><span class="sxs-lookup"><span data-stu-id="004a3-168">A</span></span> | `@` | <span data-ttu-id="004a3-169">IP adresu z [aplikace hello kopie IP adresu](#info)</span><span class="sxs-lookup"><span data-stu-id="004a3-169">IP address from [Copy hello app's IP address](#info)</span></span> |
| <span data-ttu-id="004a3-170">www.contoso.com (pod)</span><span class="sxs-lookup"><span data-stu-id="004a3-170">www.contoso.com (sub)</span></span> | <span data-ttu-id="004a3-171">CNAME</span><span class="sxs-lookup"><span data-stu-id="004a3-171">CNAME</span></span> | `www` | <span data-ttu-id="004a3-172">_&lt;AppName >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="004a3-172">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="004a3-173">\*. contoso.com (zástupný znak)</span><span class="sxs-lookup"><span data-stu-id="004a3-173">\*.contoso.com (wildcard)</span></span> | <span data-ttu-id="004a3-174">CNAME</span><span class="sxs-lookup"><span data-stu-id="004a3-174">CNAME</span></span> | _\*_ | <span data-ttu-id="004a3-175">_&lt;AppName >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="004a3-175">_&lt;appname>.azurewebsites.net_</span></span> |

<span data-ttu-id="004a3-176">Uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="004a3-176">Save your settings.</span></span>

<span data-ttu-id="004a3-177">Dotazy DNS by se měl spustit okamžitě po šíření záznamů DNS se stane řešení tooyour aplikaci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="004a3-177">DNS queries should start resolving tooyour App Service app immediately after DNS propagation happens.</span></span>

## <a name="next-steps"></a><span data-ttu-id="004a3-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="004a3-178">Next steps</span></span>

<span data-ttu-id="004a3-179">Zjistěte, jak toobind vlastní SSL certifikát tooApp služby.</span><span class="sxs-lookup"><span data-stu-id="004a3-179">Learn how toobind a custom SSL certificate tooApp Service.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="004a3-180">Vytvořit vazbu existující vlastní SSL certifikát tooAzure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="004a3-180">Bind an existing custom SSL certificate tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-ssl.md)
