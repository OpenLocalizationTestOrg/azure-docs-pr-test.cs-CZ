---
title: "Migrovat název DNS active do služby Azure App Service | Microsoft Docs"
description: "Zjistěte, jak migrovat vlastní název domény DNS, který je již přiřazen k lokalitě v za provozu do služby Azure App Service bez odstávky."
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
ms.openlocfilehash: 89308c1c684a639950467b72d26703cc07c50c14
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-an-active-dns-name-to-azure-app-service"></a><span data-ttu-id="91c5f-103">Migrovat název DNS active do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="91c5f-103">Migrate an active DNS name to Azure App Service</span></span>

<span data-ttu-id="91c5f-104">Tento článek ukazuje, jak migrovat active názvu DNS, aby [Azure App Service](../app-service/app-service-value-prop-what-is.md) bez odstávky.</span><span class="sxs-lookup"><span data-stu-id="91c5f-104">This article shows you how to migrate an active DNS name to [Azure App Service](../app-service/app-service-value-prop-what-is.md) without any downtime.</span></span>

<span data-ttu-id="91c5f-105">Když provádíte migraci za provozu lokality a jeho název domény DNS do služby App Service, tento název DNS je již obsluhující provoz za provozu.</span><span class="sxs-lookup"><span data-stu-id="91c5f-105">When you migrate a live site and its DNS domain name to App Service, that DNS name is already serving live traffic.</span></span> <span data-ttu-id="91c5f-106">Výpadek při překlad DNS během migrace vyhnete tak, že ho preventivně vazby active název DNS do aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="91c5f-106">You can avoid downtime in DNS resolution during the migration by binding the active DNS name to your App Service app preemptively.</span></span>

<span data-ttu-id="91c5f-107">Pokud si nejste obavy o výpadek při překlad názvů DNS, najdete v části [mapovat existující vlastní název DNS pro službu Azure Web Apps](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="91c5f-107">If you're not worried about downtime in DNS resolution, see [Map an existing custom DNS name to Azure Web Apps](app-service-web-tutorial-custom-domain.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="91c5f-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="91c5f-108">Prerequisites</span></span>

<span data-ttu-id="91c5f-109">Chcete-li provést tento postup:</span><span class="sxs-lookup"><span data-stu-id="91c5f-109">To complete this how-to:</span></span>

- <span data-ttu-id="91c5f-110">[Ujistěte se, že aplikaci aplikační služby není v úrovni FREE](app-service-web-tutorial-custom-domain.md#checkpricing).</span><span class="sxs-lookup"><span data-stu-id="91c5f-110">[Make sure that your App Service app is not in FREE tier](app-service-web-tutorial-custom-domain.md#checkpricing).</span></span>

## <a name="bind-the-domain-name-preemptively"></a><span data-ttu-id="91c5f-111">Název domény ho preventivně vazby</span><span class="sxs-lookup"><span data-stu-id="91c5f-111">Bind the domain name preemptively</span></span>

<span data-ttu-id="91c5f-112">Pokud jste ho preventivně vytvořit vazbu vlastní doménu, musíte provést obě tyto před provedením jakýchkoli změn se záznamy DNS:</span><span class="sxs-lookup"><span data-stu-id="91c5f-112">When you bind a custom domain preemptively, you accomplish both of the following before making any changes to your DNS records:</span></span>

- <span data-ttu-id="91c5f-113">Ověřit vlastnictví domény</span><span class="sxs-lookup"><span data-stu-id="91c5f-113">Verify domain ownership</span></span>
- <span data-ttu-id="91c5f-114">Povolit název domény pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="91c5f-114">Enable the domain name for your app</span></span>

<span data-ttu-id="91c5f-115">Při migraci nakonec váš vlastní název DNS ze staré lokality do aplikace služby App Service, budou v překlad DNS existovat nedojde k žádnému výpadku.</span><span class="sxs-lookup"><span data-stu-id="91c5f-115">When you finally migrate your custom DNS name from the old site to the App Service app, there will be no downtime in DNS resolution.</span></span>

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a><span data-ttu-id="91c5f-116">Vytvořit záznam ověření domény</span><span class="sxs-lookup"><span data-stu-id="91c5f-116">Create domain verification record</span></span>

<span data-ttu-id="91c5f-117">Pokud chcete ověřit vlastnictví domény, přidejte záznam TXT.</span><span class="sxs-lookup"><span data-stu-id="91c5f-117">To verify domain ownership, Add a TXT record.</span></span> <span data-ttu-id="91c5f-118">Záznam TXT mapuje z _awverify.&lt; subdoména >_ k  _&lt;appname >. azurewebsites.net_.</span><span class="sxs-lookup"><span data-stu-id="91c5f-118">The TXT record maps from _awverify.&lt;subdomain>_ to _&lt;appname>.azurewebsites.net_.</span></span> 

<span data-ttu-id="91c5f-119">Záznam TXT, které potřebujete, závisí na záznam DNS, kterou chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="91c5f-119">The TXT record you need depends on the DNS record you want to migrate.</span></span> <span data-ttu-id="91c5f-120">Příklady, najdete v následující tabulce (`@` obvykle představuje kořenové domény):</span><span class="sxs-lookup"><span data-stu-id="91c5f-120">For examples, see the following table (`@` typically represents the root domain):</span></span>  

| <span data-ttu-id="91c5f-121">Příklad záznamu DNS</span><span class="sxs-lookup"><span data-stu-id="91c5f-121">DNS record example</span></span> | <span data-ttu-id="91c5f-122">TXT hostitele</span><span class="sxs-lookup"><span data-stu-id="91c5f-122">TXT Host</span></span> | <span data-ttu-id="91c5f-123">Hodnota TXT</span><span class="sxs-lookup"><span data-stu-id="91c5f-123">TXT Value</span></span> |
| - | - | - |
| <span data-ttu-id="91c5f-124">@ (uživatel root)</span><span class="sxs-lookup"><span data-stu-id="91c5f-124">@ (root)</span></span> | <span data-ttu-id="91c5f-125">_awverify_</span><span class="sxs-lookup"><span data-stu-id="91c5f-125">_awverify_</span></span> | <span data-ttu-id="91c5f-126">_&lt;AppName >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="91c5f-126">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="91c5f-127">WWW (pod)</span><span class="sxs-lookup"><span data-stu-id="91c5f-127">www (sub)</span></span> | <span data-ttu-id="91c5f-128">_awverify.www_</span><span class="sxs-lookup"><span data-stu-id="91c5f-128">_awverify.www_</span></span> | <span data-ttu-id="91c5f-129">_&lt;AppName >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="91c5f-129">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="91c5f-130">\*(zástupný znak)</span><span class="sxs-lookup"><span data-stu-id="91c5f-130">\* (wildcard)</span></span> | <span data-ttu-id="91c5f-131">_awverify.\*_</span><span class="sxs-lookup"><span data-stu-id="91c5f-131">_awverify.\*_</span></span> | <span data-ttu-id="91c5f-132">_&lt;AppName >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="91c5f-132">_&lt;appname>.azurewebsites.net_</span></span> |

<span data-ttu-id="91c5f-133">Na stránce záznamy DNS Všimněte si, typ záznamu DNS název, který chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="91c5f-133">In your DNS records page, note the record type of the DNS name you want to migrate.</span></span> <span data-ttu-id="91c5f-134">App Service podporuje mapování z CNAME a záznamy.</span><span class="sxs-lookup"><span data-stu-id="91c5f-134">App Service supports mappings from CNAME and A records.</span></span>

### <a name="enable-the-domain-for-your-app"></a><span data-ttu-id="91c5f-135">Povolit domény pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="91c5f-135">Enable the domain for your app</span></span>

<span data-ttu-id="91c5f-136">V [portál Azure](https://portal.azure.com), v levém navigačním panelu stránku aplikace, vyberte **vlastní domény**.</span><span class="sxs-lookup"><span data-stu-id="91c5f-136">In the [Azure portal](https://portal.azure.com), in the left navigation of the app page, select **Custom domains**.</span></span> 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="91c5f-138">V **vlastní domény** vyberte  **+**  ikonu vedle **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="91c5f-138">In the **Custom domains** page, select the **+** icon next to **Add hostname**.</span></span>

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="91c5f-140">Zadejte název plně kvalifikované domény přidat záznam TXT, jako například `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="91c5f-140">Type the fully qualified domain name that you added the TXT record for, such as `www.contoso.com`.</span></span> <span data-ttu-id="91c5f-141">Pro doménu zástupných znaků (například \*. contoso.com), můžete použít libovolný název DNS, který odpovídá zástupné domény.</span><span class="sxs-lookup"><span data-stu-id="91c5f-141">For a wildcard domain (like \*.contoso.com), you can use any DNS name that matches the wildcard domain.</span></span> 

<span data-ttu-id="91c5f-142">Vyberte **ověření**.</span><span class="sxs-lookup"><span data-stu-id="91c5f-142">Select **Validate**.</span></span>

<span data-ttu-id="91c5f-143">**Přidat název hostitele** se aktivuje tlačítko.</span><span class="sxs-lookup"><span data-stu-id="91c5f-143">The **Add hostname** button is activated.</span></span> 

<span data-ttu-id="91c5f-144">Ujistěte se, že **typ záznamu Hostname** nastavená na typ záznamu DNS, kterou chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="91c5f-144">Make sure that **Hostname record type** is set to the DNS record type you want to migrate.</span></span>

<span data-ttu-id="91c5f-145">Vyberte **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="91c5f-145">Select **Add hostname**.</span></span>

![Přidání názvu DNS do aplikace](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

<span data-ttu-id="91c5f-147">Může trvat nějakou dobu, než nový název hostitele v aplikaci projeví **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="91c5f-147">It might take some time for the new hostname to be reflected in the app's **Custom domains** page.</span></span> <span data-ttu-id="91c5f-148">Aktualizujte prohlížeč aktualizovat data.</span><span class="sxs-lookup"><span data-stu-id="91c5f-148">Try refreshing the browser to update the data.</span></span>

![Přidat záznam CNAME](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

<span data-ttu-id="91c5f-150">Váš vlastní název DNS je teď povolené v aplikaci Azure.</span><span class="sxs-lookup"><span data-stu-id="91c5f-150">Your custom DNS name is now enabled in your Azure app.</span></span> 

## <a name="remap-the-active-dns-name"></a><span data-ttu-id="91c5f-151">Přemapování active název DNS</span><span class="sxs-lookup"><span data-stu-id="91c5f-151">Remap the active DNS name</span></span>

<span data-ttu-id="91c5f-152">Pouze vlevo věc udělat je přemapování vaše active záznam DNS tak, aby odkazoval na službu App Service.</span><span class="sxs-lookup"><span data-stu-id="91c5f-152">The only thing left to do is remapping your active DNS record to point to App Service.</span></span> <span data-ttu-id="91c5f-153">Pravé teď stále odkazuje na vaše staré lokality.</span><span class="sxs-lookup"><span data-stu-id="91c5f-153">Right now, it still points to your old site.</span></span>

<a name="info"></a>

### <a name="copy-the-apps-ip-address-a-record-only"></a><span data-ttu-id="91c5f-154">Zkopírujte adresu IP aplikace (jenom záznam)</span><span class="sxs-lookup"><span data-stu-id="91c5f-154">Copy the app's IP address (A record only)</span></span>

<span data-ttu-id="91c5f-155">Pokud jsou přemapování záznam CNAME, tuto část přeskočte.</span><span class="sxs-lookup"><span data-stu-id="91c5f-155">If you are remapping a CNAME record, skip this section.</span></span> 

<span data-ttu-id="91c5f-156">Přemapování záznam A, musíte aplikace App Service externí IP adresu, která se zobrazí v **vlastní domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="91c5f-156">To remap an A record, you need the App Service app's external IP address, which is shown in the **Custom domains** page.</span></span>

<span data-ttu-id="91c5f-157">Zavřít **přidat název hostitele** stránky tak, že vyberete **X** v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="91c5f-157">Close the **Add hostname** page by selecting **X** in the upper-right corner.</span></span> 

<span data-ttu-id="91c5f-158">V **vlastní domény** stránky, zkopírujte aplikace IP adresu.</span><span class="sxs-lookup"><span data-stu-id="91c5f-158">In the **Custom domains** page, copy the app's IP address.</span></span>

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-the-dns-record"></a><span data-ttu-id="91c5f-160">Aktualizace záznamu DNS</span><span class="sxs-lookup"><span data-stu-id="91c5f-160">Update the DNS record</span></span>

<span data-ttu-id="91c5f-161">Zpět na stránce záznamy DNS vaší domény poskytovatele, vyberte záznam DNS, který chcete přemapovat.</span><span class="sxs-lookup"><span data-stu-id="91c5f-161">Back in the DNS records page of your domain provider, select the DNS record to remap.</span></span>

<span data-ttu-id="91c5f-162">Pro `contoso.com` kořen domény příklad, přemapovat záznam CNAME nebo A jako příklady v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="91c5f-162">For the `contoso.com` root domain example, remap the A or CNAME record like the examples in the following table:</span></span> 

| <span data-ttu-id="91c5f-163">Příklad plně kvalifikovaný název domény</span><span class="sxs-lookup"><span data-stu-id="91c5f-163">FQDN example</span></span> | <span data-ttu-id="91c5f-164">Typ záznamu</span><span class="sxs-lookup"><span data-stu-id="91c5f-164">Record type</span></span> | <span data-ttu-id="91c5f-165">Hostitel</span><span class="sxs-lookup"><span data-stu-id="91c5f-165">Host</span></span> | <span data-ttu-id="91c5f-166">Hodnota</span><span class="sxs-lookup"><span data-stu-id="91c5f-166">Value</span></span> |
| - | - | - | - |
| <span data-ttu-id="91c5f-167">contoso.com (uživatel root)</span><span class="sxs-lookup"><span data-stu-id="91c5f-167">contoso.com (root)</span></span> | <span data-ttu-id="91c5f-168">A</span><span class="sxs-lookup"><span data-stu-id="91c5f-168">A</span></span> | `@` | <span data-ttu-id="91c5f-169">IP adresu z [zkopírujte aplikace IP adresu](#info)</span><span class="sxs-lookup"><span data-stu-id="91c5f-169">IP address from [Copy the app's IP address](#info)</span></span> |
| <span data-ttu-id="91c5f-170">www.contoso.com (pod)</span><span class="sxs-lookup"><span data-stu-id="91c5f-170">www.contoso.com (sub)</span></span> | <span data-ttu-id="91c5f-171">CNAME</span><span class="sxs-lookup"><span data-stu-id="91c5f-171">CNAME</span></span> | `www` | <span data-ttu-id="91c5f-172">_&lt;AppName >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="91c5f-172">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="91c5f-173">\*. contoso.com (zástupný znak)</span><span class="sxs-lookup"><span data-stu-id="91c5f-173">\*.contoso.com (wildcard)</span></span> | <span data-ttu-id="91c5f-174">CNAME</span><span class="sxs-lookup"><span data-stu-id="91c5f-174">CNAME</span></span> | _\*_ | <span data-ttu-id="91c5f-175">_&lt;AppName >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="91c5f-175">_&lt;appname>.azurewebsites.net_</span></span> |

<span data-ttu-id="91c5f-176">Uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="91c5f-176">Save your settings.</span></span>

<span data-ttu-id="91c5f-177">Dotazy DNS by se měl spustit řešení do aplikace služby App Service hned se stane šíření záznamů DNS.</span><span class="sxs-lookup"><span data-stu-id="91c5f-177">DNS queries should start resolving to your App Service app immediately after DNS propagation happens.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91c5f-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="91c5f-178">Next steps</span></span>

<span data-ttu-id="91c5f-179">Naučte se vytvořit vazbu vlastní certifikát SSL služby App Service.</span><span class="sxs-lookup"><span data-stu-id="91c5f-179">Learn how to bind a custom SSL certificate to App Service.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="91c5f-180">Vytvoření vazby stávající vlastní certifikát SSL pro službu Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="91c5f-180">Bind an existing custom SSL certificate to Azure Web Apps</span></span>](app-service-web-tutorial-custom-ssl.md)
