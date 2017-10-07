---
title: "aaaCustom domén v Azure AD Application Proxy | Microsoft Docs"
description: "Správa vlastních domén v Azure AD Application Proxy, abyste tuto hello adresu URL pro aplikaci hello je hello stejný bez ohledu na to, kde uživatelé k němu přístup."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7a433c411976077210a2435c3c087991c7430755
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a><span data-ttu-id="4a8ce-103">Práce s vlastní domény v Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="4a8ce-103">Working with custom domains in Azure AD Application Proxy</span></span>

<span data-ttu-id="4a8ce-104">Při publikování aplikace prostřednictvím Proxy aplikace služby Active Directory Azure, vytvořte externí adresu URL pro vaše toowhen toogo uživatelů, které pracují vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-104">When you publish an application through Azure Active Directory Application Proxy, you create an external URL for your users toogo toowhen they're working remotely.</span></span> <span data-ttu-id="4a8ce-105">Tato adresa URL získá hello výchozí doménu *yourtenant.msappproxy.net*.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-105">This URL gets hello default domain *yourtenant.msappproxy.net*.</span></span> <span data-ttu-id="4a8ce-106">Například pokud jste publikovali aplikaci s názvem výdaje a váš klient je s názvem Contoso, pak hello externí adresa URL bude https://expenses-contoso.msappproxy.net.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-106">For example, if you published an app named Expenses and your tenant is named Contoso, then hello external URL would be https://expenses-contoso.msappproxy.net.</span></span> <span data-ttu-id="4a8ce-107">Pokud chcete toouse svůj vlastní název domény, nakonfigurujte vlastní doménu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-107">If you want toouse your own domain name, configure a custom domain for your application.</span></span> 

<span data-ttu-id="4a8ce-108">Doporučujeme, abyste nastavili vlastní domény pro vaše aplikace, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-108">We recommend that you set up custom domains for your applications whenever possible.</span></span> <span data-ttu-id="4a8ce-109">Mezi výhody hello vlastní domény patří:</span><span class="sxs-lookup"><span data-stu-id="4a8ce-109">Some of hello benefits of custom domains include:</span></span>

- <span data-ttu-id="4a8ce-110">Uživatelé dostanou toohello aplikace s hello stejnou adresu URL, jestli pracují uvnitř nebo vně vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-110">Your users can get toohello application with hello same URL, whether they are working inside or outside of your network.</span></span>
- <span data-ttu-id="4a8ce-111">Pokud mají všechny aplikace hello stejné interní a externí adresy URL, pokračujte odkazy v jedné aplikace, které odkazují tooanother toowork i mimo firemní síť hello.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-111">If all of your applications have hello same internal and external URLs, then links in one application that point tooanother continue toowork even outside hello corporate network.</span></span> 
- <span data-ttu-id="4a8ce-112">Řízení branding a vytvoření adresy URL hello chcete.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-112">You control your branding, and create hello URLs you want.</span></span> 


## <a name="configure-a-custom-domain"></a><span data-ttu-id="4a8ce-113">Konfigurace vlastní domény</span><span class="sxs-lookup"><span data-stu-id="4a8ce-113">Configure a custom domain</span></span>

### <a name="prerequisites"></a><span data-ttu-id="4a8ce-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4a8ce-114">Prerequisites</span></span>

<span data-ttu-id="4a8ce-115">Než začnete konfigurovat vlastní doménu, ujistěte se, že máte hello následující požadavky, které jsou připravené:</span><span class="sxs-lookup"><span data-stu-id="4a8ce-115">Before you configure a custom domain, make sure that you have hello following requirements prepared:</span></span> 
- <span data-ttu-id="4a8ce-116">A [ověřené domény přidat tooAzure služby Active Directory](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4a8ce-116">A [verified domain added tooAzure Active Directory](active-directory-domains-add-azure-portal.md).</span></span>
- <span data-ttu-id="4a8ce-117">Vlastní certifikát pro doménu hello hello tvar souboru PFX.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-117">A custom certificate for hello domain, in hello form of a PFX file.</span></span> 
- <span data-ttu-id="4a8ce-118">Místní aplikace [publikované prostřednictvím Proxy aplikace](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4a8ce-118">An on-premises app [published through Application Proxy](application-proxy-publish-azure-portal.md).</span></span>

### <a name="configure-your-custom-domain"></a><span data-ttu-id="4a8ce-119">Konfigurace vlastní domény</span><span class="sxs-lookup"><span data-stu-id="4a8ce-119">Configure your custom domain</span></span>

<span data-ttu-id="4a8ce-120">Až budete mít tyto tři požadavky připraven, postupujte podle těchto kroků tooset si vaši vlastní doménu:</span><span class="sxs-lookup"><span data-stu-id="4a8ce-120">When you have those three requirements ready, follow these steps tooset up your custom domain:</span></span>

1. <span data-ttu-id="4a8ce-121">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4a8ce-121">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4a8ce-122">Přejděte příliš**Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace** a vyberte aplikaci, hello chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-122">Navigate too**Azure Active Directory** > **Enterprise applications** > **All applications** and choose hello app you want toomanage.</span></span>
3. <span data-ttu-id="4a8ce-123">Vyberte **Proxy aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-123">Select **Application Proxy**.</span></span> 
4. <span data-ttu-id="4a8ce-124">V poli hello externí adresu URL použijte hello rozevíracího seznamu tooselect vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-124">In hello External URL field, use hello dropdown list tooselect your custom domain.</span></span> <span data-ttu-id="4a8ce-125">Pokud nevidíte doménu v seznamu hello, pak jej nebyla ověřena ještě.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-125">If you don't see your domain in hello list, then it hasn't been verified yet.</span></span> 
5. <span data-ttu-id="4a8ce-126">Vyberte **uložit**</span><span class="sxs-lookup"><span data-stu-id="4a8ce-126">Select **Save**</span></span>
5. <span data-ttu-id="4a8ce-127">Hello **certifikát** nepovolíte pole, která byla zakázána.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-127">hello **Certificate** field that was disabled becomes enabled.</span></span> <span data-ttu-id="4a8ce-128">Toto políčko zaškrtněte.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-128">Select this field.</span></span> 

   ![Klikněte na tlačítko tooupload certifikát](./media/active-directory-application-proxy-custom-domains/certificate.png)

   <span data-ttu-id="4a8ce-130">Pokud jste již odeslali certifikát pro tuto doménu, pole certifikátu hello zobrazí informace o certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-130">If you already uploaded a certificate for this domain, hello Certificate field displays hello certificate information.</span></span> 

6. <span data-ttu-id="4a8ce-131">Nahrání certifikátu PFX hello a zadejte hello heslo pro certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-131">Upload hello PFX certificate and enter hello password for hello certificate.</span></span> 
7. <span data-ttu-id="4a8ce-132">Vyberte **Uložit** toosave změny.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-132">Select **Save** toosave your changes.</span></span> 
8. <span data-ttu-id="4a8ce-133">Přidat [záznam DNS](../dns/dns-operations-recordsets-portal.md) , přesměrování hello nové externí domény msappproxy.net toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-133">Add a [DNS record](../dns/dns-operations-recordsets-portal.md) that redirects hello new external URL toohello msappproxy.net domain.</span></span> 

>[!TIP] 
><span data-ttu-id="4a8ce-134">Potřebujete jenom jeden certifikát tooupload za vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-134">You only need tooupload one certificate per custom domain.</span></span> <span data-ttu-id="4a8ce-135">Jakmile nahrajete certifikát, můžete zvolit vlastní domény hello při publikování nové aplikace a není nutné toodo další konfigurace s výjimkou záznamu DNS hello.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-135">Once you upload a certificate, you can choose hello custom domain when you publish a new app and not have toodo additional configuration except for hello DNS record.</span></span> 

## <a name="manage-certificates"></a><span data-ttu-id="4a8ce-136">Správa certifikátů</span><span class="sxs-lookup"><span data-stu-id="4a8ce-136">Manage certificates</span></span>

### <a name="certificate-format"></a><span data-ttu-id="4a8ce-137">Formát certifikátu</span><span class="sxs-lookup"><span data-stu-id="4a8ce-137">Certificate format</span></span>
<span data-ttu-id="4a8ce-138">Neexistuje žádné omezení na hello certifikát podpis metody.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-138">There is no restriction on hello certificate signature methods.</span></span> <span data-ttu-id="4a8ce-139">Šifrování ECC (Elliptic Curve), alternativní název předmětu (SAN) a další běžné typy certifikátů jsou všechny podporované.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-139">Elliptic Curve Cryptography (ECC), Subject Alternative Name (SAN), and other common certificate types are all supported.</span></span> 

<span data-ttu-id="4a8ce-140">Certifikát se zástupným znakem můžete použít také zástupné hello odpovídá hello potřeby externí adresu URL.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-140">You can use a wildcard certificate as long as hello wildcard matches hello desired external URL.</span></span> 

<span data-ttu-id="4a8ce-141">Můžete použít certifikáty podepsané svým držitelem, také.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-141">You can use self-signed certificates, as well.</span></span> <span data-ttu-id="4a8ce-142">Pokud používáte privátní certifikační autority, by měly být veřejné hello distribučního místa seznamu CRL (certifikát odvolání bodu distribučního bodu) pro certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-142">If you’re using a private certificate authority, hello CDP (certificate revocation point distribution point) for hello certificate should be public.</span></span>

### <a name="changing-hello-domain"></a><span data-ttu-id="4a8ce-143">Změna domény hello</span><span class="sxs-lookup"><span data-stu-id="4a8ce-143">Changing hello domain</span></span>
<span data-ttu-id="4a8ce-144">Všechny ověřené domény jsou uvedeny v seznamu rozevírací hello externí adresu URL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-144">All verified domains appear in hello External URL dropdown list for your application.</span></span> <span data-ttu-id="4a8ce-145">toochange hello domény, jenom aktualizace, která pole aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-145">toochange hello domain, just update that field for hello application.</span></span> <span data-ttu-id="4a8ce-146">Pokud chcete domény hello není v seznamu hello [přidejte jej jako ověřené domény](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4a8ce-146">If hello domain you want isn't in hello list, [add it as a verified domain](active-directory-domains-add-azure-portal.md).</span></span> <span data-ttu-id="4a8ce-147">Pokud vyberete domény, který ještě nemá přidružený certifikát, postupujte podle kroků 5 až 7 tooadd hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-147">If you select a domain that doesn't have an associated certificate yet, follow steps 5-7 tooadd hello certificate.</span></span> <span data-ttu-id="4a8ce-148">Ujistěte se, že aktualizujete hello tooredirect záznamů DNS z hello nový externí adresu URL.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-148">Then, make sure you update hello DNS record tooredirect from hello new external URL.</span></span> 

### <a name="certificate-management"></a><span data-ttu-id="4a8ce-149">Správa certifikátů</span><span class="sxs-lookup"><span data-stu-id="4a8ce-149">Certificate management</span></span>
<span data-ttu-id="4a8ce-150">Můžete použít stejný certifikát pro více aplikací, pokud aplikace hello sdílet externím hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-150">You can use hello same certificate for multiple applications unless hello applications share an external host.</span></span> 

<span data-ttu-id="4a8ce-151">Se zobrazí upozornění, když vyprší platnost certifikátu, informující o tooupload jiný certifikát prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-151">You get a warning when a certificate expires, telling you tooupload another certificate through hello portal.</span></span> <span data-ttu-id="4a8ce-152">Hello certifikát je odvolaný, může uživatelům přístup k aplikaci hello zobrazí upozornění zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-152">If hello certificate is revoked, your users may see a security warning when accessing hello application.</span></span> <span data-ttu-id="4a8ce-153">Neprovádějte jsme kontroly odvolání certifikátů.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-153">We don’t perform revocation checks for certificates.</span></span>  <span data-ttu-id="4a8ce-154">tooupdate hello certifikát pro danou aplikaci, přejděte toohello aplikace a postupujte podle kroků 5 až 7 pro konfiguraci vlastních domén na publikované aplikace tooupload nový certifikát.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-154">tooupdate hello certificate for a given application, navigate toohello application and follow steps 5-7 for configuring custom domains on published applications tooupload a new certificate.</span></span> <span data-ttu-id="4a8ce-155">Pokud hello starý certifikát není používán jinými aplikacemi, je automaticky odstraněna.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-155">If hello old certificate is not being used by other applications, it is deleted automatically.</span></span> 

<span data-ttu-id="4a8ce-156">Aktuálně všechny správy certifikátů je prostřednictvím stránky jednotlivých aplikací, takže budete potřebovat certifikáty toomanage v kontextu hello hello příslušných aplikací.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-156">Currently all certificate management is through individual application pages so you need toomanage certificates in hello context of hello relevant applications.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="4a8ce-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4a8ce-157">Next steps</span></span>
* <span data-ttu-id="4a8ce-158">[Povolit jednotné přihlašování](active-directory-application-proxy-sso-using-kcd.md) tooyour publikované aplikace pomocí ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-158">[Enable single sign-on](active-directory-application-proxy-sso-using-kcd.md) tooyour published apps with Azure AD authentication.</span></span>
* <span data-ttu-id="4a8ce-159">[Povolení podmíněného přístupu](active-directory-application-proxy-conditional-access.md) tooyour publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="4a8ce-159">[Enable conditional access](active-directory-application-proxy-conditional-access.md) tooyour published apps.</span></span>
* [<span data-ttu-id="4a8ce-160">Přidejte název tooAzure vaše vlastní domény AD</span><span class="sxs-lookup"><span data-stu-id="4a8ce-160">Add your custom domain name tooAzure AD</span></span>](active-directory-domains-add-azure-portal.md)


