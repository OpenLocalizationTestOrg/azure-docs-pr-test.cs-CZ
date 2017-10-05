---
title: "Převede uzel odkazy a Proxy aplikace Azure AD adresy URL | Microsoft Docs"
description: "Popisuje základní informace o Azure AD Application Proxy konektory."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 57218346d236b376d2227e0ffaea6c6dd5ebe855
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a><span data-ttu-id="95aa9-103">Přesměrování pevně zakódované odkazy k aplikacím publikovaným pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="95aa9-103">Redirect hardcoded links for apps published with Azure AD Application Proxy</span></span>

<span data-ttu-id="95aa9-104">Proxy aplikace služby Azure AD umožňuje místní aplikace k dispozici pro uživatele, kteří jsou vzdálené nebo na jejich vlastní zařízení.</span><span class="sxs-lookup"><span data-stu-id="95aa9-104">Azure AD Application Proxy makes your on-premises apps available to users who are remote or on their own devices.</span></span> <span data-ttu-id="95aa9-105">Některé aplikace, ale měla vyvinuté pomocí místních odkazů vložených v kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="95aa9-105">Some apps, however, were developed with local links embedded in the HTML.</span></span> <span data-ttu-id="95aa9-106">Tyto odkazy správně nefungují, pokud je aplikace používat vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="95aa9-106">These links don't work correctly when the app is used remotely.</span></span> <span data-ttu-id="95aa9-107">Pokud máte několik místní aplikace, přejděte na sobě navzájem, uživatelé očekávají, že na odkazy pokračovat v práci při jejich nejste v kanceláři.</span><span class="sxs-lookup"><span data-stu-id="95aa9-107">When you have several on-premises applications point to each other, your users expect the links to keep working when they're not at the office.</span></span> 

<span data-ttu-id="95aa9-108">Ke konfiguraci externí adresy URL aplikací být stejné jako jejich interní adresy URL je nejlepší způsob, jak se ujistěte, že odkazy fungovat stejně uvnitř i vně podnikové sítě.</span><span class="sxs-lookup"><span data-stu-id="95aa9-108">The best way to make sure that links work the same both inside and outside of your corporate network is to configure the external URLs of your apps to be the same as their internal URLs.</span></span> <span data-ttu-id="95aa9-109">Použití [vlastní domény](active-directory-application-proxy-custom-domains.md) nakonfigurovat tak, aby měl název vaší firemní domény místo výchozí domény proxy aplikace vašeho externí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="95aa9-109">Use [custom domains](active-directory-application-proxy-custom-domains.md) to configure your external URLs to have your corporate domain name instead of the default application proxy domain.</span></span>

<span data-ttu-id="95aa9-110">Pokud nemůžete použít vlastní domény ve vašem klientovi, funkci překlad odkazů proxy aplikací zajišťuje vaše odkazy práce bez ohledu na to, kde jsou vaši uživatelé.</span><span class="sxs-lookup"><span data-stu-id="95aa9-110">If you can't use custom domains in your tenant, then the link translation feature of Application Proxy keeps your links working no matter where your users are.</span></span> <span data-ttu-id="95aa9-111">Pokud máte aplikace, které přejděte přímo na vnitřních koncových bodů nebo porty, můžete namapovat tyto interní adresy URL publikované externí URL Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="95aa9-111">When you have apps that point directly to internal endpoints or ports, you can map these internal URLs to the published external Application Proxy URLs.</span></span> <span data-ttu-id="95aa9-112">Pokud je povoleno překlad odkaz, a hledá Proxy aplikací pomocí jazyka HTML, CSS a vyberte značky jazyka JavaScript pro publikované interní odkazy.</span><span class="sxs-lookup"><span data-stu-id="95aa9-112">When link translation is enabled, and Application Proxy searches through HTML, CSS, and select JavaScript tags for published internal links.</span></span> <span data-ttu-id="95aa9-113">Potom služba Proxy aplikace znamená, že je tak, aby uživatelé získají bez přerušení prostředí.</span><span class="sxs-lookup"><span data-stu-id="95aa9-113">Then the Application Proxy service translates them so that your users get an uninterrupted experience.</span></span>

>[!NOTE]
><span data-ttu-id="95aa9-114">Funkce překladu odkaz je pro klienty, kteří ať důvodu, nelze použít vlastní domény tak, aby měl stejné interní a externí adresy URL pro svoje aplikace.</span><span class="sxs-lookup"><span data-stu-id="95aa9-114">The link translation feature is for tenants that, for whatever reason, can't use custom domains to have the same internal and external URLs for their apps.</span></span> <span data-ttu-id="95aa9-115">Před povolením této funkce najdete v části Pokud [vlastních domén v Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md) může fungovat pro vás.</span><span class="sxs-lookup"><span data-stu-id="95aa9-115">Before you enable this feature, see if [custom domains in Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md) can work for you.</span></span>
>
><span data-ttu-id="95aa9-116">Nebo, pokud je aplikace, budete muset nakonfigurovat s odkazem překlad služby SharePoint, najdete v části [konfigurace mapování alternativních adres URL pro službu SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx) jiný způsob mapování odkazy.</span><span class="sxs-lookup"><span data-stu-id="95aa9-116">Or, if the application you need to configure with link translation is SharePoint, see [Configure alternate access mappings for SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx) for another approach to mapping links.</span></span>

## <a name="how-link-translation-works"></a><span data-ttu-id="95aa9-117">Jak funguje překlad propojení</span><span class="sxs-lookup"><span data-stu-id="95aa9-117">How link translation works</span></span>

<span data-ttu-id="95aa9-118">Po ověření pokud proxy server předává data aplikací pro uživatele, Proxy aplikace vyhledá aplikace pro pevně zakódované odkazy a nahradí je jejich odpovídajících publikovaná externí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="95aa9-118">After authentication, when the proxy server passes the application data to the user, Application Proxy scans the application for hardcoded links and replaces them with their respective, published external URLs.</span></span>

<span data-ttu-id="95aa9-119">Proxy aplikace se předpokládá, že aplikace jsou kódování ve formátu UTF-8.</span><span class="sxs-lookup"><span data-stu-id="95aa9-119">Application Proxy assumes that applications are encoded in UTF-8.</span></span> <span data-ttu-id="95aa9-120">Pokud není tento případ, zadejte typ kódování v hlavičce http odpovědi jako `Content-Type:text/html;charset=utf-8`.</span><span class="sxs-lookup"><span data-stu-id="95aa9-120">If that's not the case, specify the encoding type in an http response header, like `Content-Type:text/html;charset=utf-8`.</span></span>

### <a name="which-links-are-affected"></a><span data-ttu-id="95aa9-121">Odkazy, které se vztahuje?</span><span class="sxs-lookup"><span data-stu-id="95aa9-121">Which links are affected?</span></span>

<span data-ttu-id="95aa9-122">Funkci překlad odkazů pouze vyhledá odkazy, které jsou v kódu značek v těle aplikace.</span><span class="sxs-lookup"><span data-stu-id="95aa9-122">The link translation feature only looks for links that are in code tags in the body of an app.</span></span> <span data-ttu-id="95aa9-123">Proxy aplikace má samostatné funkce pro překlad adres URL nebo soubory cookie v záhlaví.</span><span class="sxs-lookup"><span data-stu-id="95aa9-123">Application Proxy has a separate feature for translating cookies or URLs in headers.</span></span> 

<span data-ttu-id="95aa9-124">Existují dva typy běžné interní odkazů v místním aplikacím:</span><span class="sxs-lookup"><span data-stu-id="95aa9-124">There are two common types of internal links in on-premises applications:</span></span>

- <span data-ttu-id="95aa9-125">**Relativní odkazy interní** , přejděte na příkaz sdíleného prostředku do místního souboru struktury jako `/claims/claims.html`.</span><span class="sxs-lookup"><span data-stu-id="95aa9-125">**Relative internal links** that point to a shared resource in a local file structure like `/claims/claims.html`.</span></span> <span data-ttu-id="95aa9-126">Tyto odkazy se automaticky fungovat v aplikacích, které jsou publikované prostřednictvím Proxy aplikace a pokračovat v práci s nebo bez překladu odkaz.</span><span class="sxs-lookup"><span data-stu-id="95aa9-126">These links automatically work in apps that are published through Application Proxy, and continue to work with or without link translation.</span></span> 
- <span data-ttu-id="95aa9-127">**Vnitřní propojení pevně zakódované** do jiných aplikací místní jako `http://expenses` nebo publikovat soubory, jako jsou `http://expenses/logo.jpg`.</span><span class="sxs-lookup"><span data-stu-id="95aa9-127">**Hardcoded internal links** to other on-premises apps like `http://expenses` or published files like `http://expenses/logo.jpg`.</span></span> <span data-ttu-id="95aa9-128">Funkci překlad odkazů na interní odkazy pevně zakódované funguje a je tak, aby odkazoval na externí adresy URL, které je třeba projít vzdálení uživatelé změní.</span><span class="sxs-lookup"><span data-stu-id="95aa9-128">The link translation feature works on hardcoded internal links, and changes them to point to the external URLs that remote users need to go through.</span></span>

### <a name="how-do-apps-link-to-each-other"></a><span data-ttu-id="95aa9-129">Jak aplikace propojit k sobě navzájem?</span><span class="sxs-lookup"><span data-stu-id="95aa9-129">How do apps link to each other?</span></span>

<span data-ttu-id="95aa9-130">Překlad odkaz je povolena pro každou aplikaci tak, že budete mít kontrolu nad činnost koncového uživatele na úrovni pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="95aa9-130">Link translation is enabled for each application, so that you have control over the user experience at the per-app level.</span></span> <span data-ttu-id="95aa9-131">Zapnout překlad odkaz pro aplikaci, když chcete, aby odkazy *z* aplikaci k převodu, není odkazy *k* tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="95aa9-131">Turn on link translation for an app when you want the links *from* that app to be translated, not links *to* that app.</span></span> 

<span data-ttu-id="95aa9-132">Předpokládejme například, že máte tři aplikacích publikovaných prostřednictvím Proxy aplikace všech propojení mezi sebou: výhody, náklady a cesta.</span><span class="sxs-lookup"><span data-stu-id="95aa9-132">For example, suppose that you have three applications published through Application Proxy that all link to each other: Benefits, Expenses, and Travel.</span></span> <span data-ttu-id="95aa9-133">Je čtvrtá aplikaci a zpětnou vazbu, která není publikována prostřednictvím Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="95aa9-133">There's a fourth app, Feedback, that isn't published through Application Proxy.</span></span>

<span data-ttu-id="95aa9-134">Když povolíte překlad odkaz pro aplikaci výhody, odkazy na výdaje a cesta přesměrování na externí adresy URL pro tyto aplikace, ale odkaz zpětná vazba není přesměrován, protože neexistuje žádná externí adresa URL.</span><span class="sxs-lookup"><span data-stu-id="95aa9-134">When you enable link translation for the Benefits app, the links to Expenses and Travel are redirected to the external URLs for those apps, but the link to Feedback is not redirected because there is no external URL.</span></span> <span data-ttu-id="95aa9-135">Nefungují odkazy z výdajů a cesta zpět na výhody, protože odkaz překlad nebyla povolena pro tyto dvě aplikace.</span><span class="sxs-lookup"><span data-stu-id="95aa9-135">Links from Expenses and Travel back to Benefits don't work, because link translation has not been enabled for those two apps.</span></span>

![Odkazy z výhody do jiných aplikací, pokud je povoleno překlad odkaz](./media/application-proxy-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a><span data-ttu-id="95aa9-137">Odkazy, které nejsou přeloženy?</span><span class="sxs-lookup"><span data-stu-id="95aa9-137">Which links aren't translated?</span></span>

<span data-ttu-id="95aa9-138">Pro zvýšení výkonu a zabezpečení, nejsou přeložit některé odkazy:</span><span class="sxs-lookup"><span data-stu-id="95aa9-138">To improve performance and security, some links aren't translated:</span></span>

- <span data-ttu-id="95aa9-139">Odkazy není uvnitř značky kódu.</span><span class="sxs-lookup"><span data-stu-id="95aa9-139">Links not inside of code tags.</span></span> 
- <span data-ttu-id="95aa9-140">Odkazy nejsou v HTML, CSS a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="95aa9-140">Links not in HTML, CSS, or JavaScript.</span></span> 
- <span data-ttu-id="95aa9-141">Vnitřní propojení otevřít z jiných aplikací.</span><span class="sxs-lookup"><span data-stu-id="95aa9-141">Internal links opened from other programs.</span></span> <span data-ttu-id="95aa9-142">Odkazy budou odesílat prostřednictvím e-mailu nebo pomocí rychlé zprávy nebo součástí jiné dokumenty, nebude možné přeložit.</span><span class="sxs-lookup"><span data-stu-id="95aa9-142">Links sent through email or instant message, or included in other documents, won't be translated.</span></span> <span data-ttu-id="95aa9-143">Uživatelé musí vědět, přejít na externí adresu URL.</span><span class="sxs-lookup"><span data-stu-id="95aa9-143">The users need to know to go to the external URL.</span></span>

<span data-ttu-id="95aa9-144">Pokud potřebujete podporovat některý z těchto dvou scénářů, použijte místo odkaz překlad stejné interní a externí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="95aa9-144">If you need to support one of these two scenarios, use the same internal and external URLs instead of link translation.</span></span>  

## <a name="enable-link-translation"></a><span data-ttu-id="95aa9-145">Povolit překlad odkaz</span><span class="sxs-lookup"><span data-stu-id="95aa9-145">Enable link translation</span></span>

<span data-ttu-id="95aa9-146">Začínáme s překlad odkaz je stejně snadná jako kliknutí na tlačítko:</span><span class="sxs-lookup"><span data-stu-id="95aa9-146">Getting started with link translation is as easy as clicking a button:</span></span>

1. <span data-ttu-id="95aa9-147">Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.</span><span class="sxs-lookup"><span data-stu-id="95aa9-147">Sign in to the [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="95aa9-148">Přejděte na **Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace** > vyberte aplikaci, kterou chcete spravovat > **proxy aplikace**.</span><span class="sxs-lookup"><span data-stu-id="95aa9-148">Go to **Azure Active Directory** > **Enterprise applications** > **All applications** > select the app you want to manage > **Application proxy**.</span></span>
3. <span data-ttu-id="95aa9-149">Zapnout **překládat adresy URL v textu aplikace** k **Ano**.</span><span class="sxs-lookup"><span data-stu-id="95aa9-149">Turn **Translate URLs in application body** to **Yes**.</span></span>

   ![Vyberte možnost Ano překládat adresy URL v textu aplikace](./media/application-proxy-link-translation/select_yes.png)<span data-ttu-id="95aa9-151">.</span><span class="sxs-lookup"><span data-stu-id="95aa9-151">.</span></span>
4. <span data-ttu-id="95aa9-152">Vyberte **Uložit** proveďte změny.</span><span class="sxs-lookup"><span data-stu-id="95aa9-152">Select **Save** to apply your changes.</span></span>

<span data-ttu-id="95aa9-153">Teď když vaši uživatelé přístup k této aplikaci, proxy server automaticky vyhledá interní adresy URL, které byly publikovány prostřednictvím Proxy aplikace na klientovi.</span><span class="sxs-lookup"><span data-stu-id="95aa9-153">Now, when your users access this application, the proxy will automatically scan for internal URLs that have been published through Application Proxy on your tenant.</span></span>

## <a name="send-feedback"></a><span data-ttu-id="95aa9-154">Pošlete svůj názor</span><span class="sxs-lookup"><span data-stu-id="95aa9-154">Send feedback</span></span>

<span data-ttu-id="95aa9-155">Chceme, abyste Ujistěte se, tato funkce fungovat pro všechny aplikace.</span><span class="sxs-lookup"><span data-stu-id="95aa9-155">We want your help to make this feature work for all your apps.</span></span> <span data-ttu-id="95aa9-156">Jsme hledání více než 30 značky v kódu HTML a CSS a jsou vzhledem k tomu, který JavaScript případů pro podporu.</span><span class="sxs-lookup"><span data-stu-id="95aa9-156">We search over 30 tags in HTML and CSS, and are considering which JavaScript cases to support.</span></span> <span data-ttu-id="95aa9-157">Pokud máte příkladem generovaného odkazy, které nejsou se překlad vztahuje, pošlete fragment kódu do [zpětnou vazbu Proxy aplikací](mailto:aadapfeedback@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="95aa9-157">If you have an example of generated links that aren't being translated, send a code snippet to [Application Proxy Feedback](mailto:aadapfeedback@microsoft.com).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="95aa9-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95aa9-158">Next steps</span></span>
<span data-ttu-id="95aa9-159">[Použití vlastních domén s Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md) mít stejnou interní a externí adresu URL</span><span class="sxs-lookup"><span data-stu-id="95aa9-159">[Use custom domains with Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md) to have the same internal and external URL</span></span>

[<span data-ttu-id="95aa9-160">Konfigurace mapování alternativních adres URL pro službu SharePoint 2013</span><span class="sxs-lookup"><span data-stu-id="95aa9-160">Configure alternate access mappings for SharePoint 2013</span></span>](https://technet.microsoft.com/library/cc263208.aspx)