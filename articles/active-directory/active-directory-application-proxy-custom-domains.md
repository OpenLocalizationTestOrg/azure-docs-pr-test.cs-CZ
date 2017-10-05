---
title: "Vlastní domény v Azure AD Application Proxy | Microsoft Docs"
description: "Správa vlastních domén v Azure AD Application Proxy tak, aby adresa URL pro aplikaci je stejný bez ohledu na to, kde uživatelé k němu přístup."
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
ms.openlocfilehash: 1dde300780c8d1f7ea9eee4c92de06bcf70a1f12
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a><span data-ttu-id="f9b3f-103">Práce s vlastní domény v Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="f9b3f-103">Working with custom domains in Azure AD Application Proxy</span></span>

<span data-ttu-id="f9b3f-104">Při publikování aplikace prostřednictvím Proxy aplikace služby Active Directory Azure můžete vytvořit externí adresu URL pro vaše uživatele, který se má přejít při pracují vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-104">When you publish an application through Azure Active Directory Application Proxy, you create an external URL for your users to go to when they're working remotely.</span></span> <span data-ttu-id="f9b3f-105">Tato adresa URL získá výchozí doménu *yourtenant.msappproxy.net*.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-105">This URL gets the default domain *yourtenant.msappproxy.net*.</span></span> <span data-ttu-id="f9b3f-106">Například pokud jste publikovali aplikaci s názvem výdajů a váš klient je s názvem Contoso, pak je externí adresa URL by byl https://expenses-contoso.msappproxy.net.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-106">For example, if you published an app named Expenses and your tenant is named Contoso, then the external URL would be https://expenses-contoso.msappproxy.net.</span></span> <span data-ttu-id="f9b3f-107">Pokud chcete použít vlastní název domény, nakonfigurujte vlastní doménu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-107">If you want to use your own domain name, configure a custom domain for your application.</span></span> 

<span data-ttu-id="f9b3f-108">Doporučujeme, abyste nastavili vlastní domény pro vaše aplikace, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-108">We recommend that you set up custom domains for your applications whenever possible.</span></span> <span data-ttu-id="f9b3f-109">Mezi výhody vlastní domény patří:</span><span class="sxs-lookup"><span data-stu-id="f9b3f-109">Some of the benefits of custom domains include:</span></span>

- <span data-ttu-id="f9b3f-110">Uživatelé dostanou do aplikace se stejnou adresu URL, jestli pracují uvnitř nebo vně vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-110">Your users can get to the application with the same URL, whether they are working inside or outside of your network.</span></span>
- <span data-ttu-id="f9b3f-111">Pokud všechny aplikace mají stejné interní a externí adresy URL, pak odkazy v jedné aplikace, které odkazují na jiný nadále pracovat i mimo podnikovou síť.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-111">If all of your applications have the same internal and external URLs, then links in one application that point to another continue to work even outside the corporate network.</span></span> 
- <span data-ttu-id="f9b3f-112">Řízení branding a vytvoření adresy URL, které chcete.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-112">You control your branding, and create the URLs you want.</span></span> 


## <a name="configure-a-custom-domain"></a><span data-ttu-id="f9b3f-113">Konfigurace vlastní domény</span><span class="sxs-lookup"><span data-stu-id="f9b3f-113">Configure a custom domain</span></span>

### <a name="prerequisites"></a><span data-ttu-id="f9b3f-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f9b3f-114">Prerequisites</span></span>

<span data-ttu-id="f9b3f-115">Než začnete konfigurovat vlastní doménu, ujistěte se, abyste měli připravit následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="f9b3f-115">Before you configure a custom domain, make sure that you have the following requirements prepared:</span></span> 
- <span data-ttu-id="f9b3f-116">A [ověřit doménu přidat do Azure Active Directory](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f9b3f-116">A [verified domain added to Azure Active Directory](active-directory-domains-add-azure-portal.md).</span></span>
- <span data-ttu-id="f9b3f-117">Vlastní certifikát pro doménu, ve formátu souboru PFX.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-117">A custom certificate for the domain, in the form of a PFX file.</span></span> 
- <span data-ttu-id="f9b3f-118">Místní aplikace [publikované prostřednictvím Proxy aplikace](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f9b3f-118">An on-premises app [published through Application Proxy](application-proxy-publish-azure-portal.md).</span></span>

### <a name="configure-your-custom-domain"></a><span data-ttu-id="f9b3f-119">Konfigurace vlastní domény</span><span class="sxs-lookup"><span data-stu-id="f9b3f-119">Configure your custom domain</span></span>

<span data-ttu-id="f9b3f-120">Až budete mít tyto tři požadavky připraven, nastavit vlastní domény takto:</span><span class="sxs-lookup"><span data-stu-id="f9b3f-120">When you have those three requirements ready, follow these steps to set up your custom domain:</span></span>

1. <span data-ttu-id="f9b3f-121">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f9b3f-121">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f9b3f-122">Přejděte na **Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace** a vyberte aplikaci, kterou chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-122">Navigate to **Azure Active Directory** > **Enterprise applications** > **All applications** and choose the app you want to manage.</span></span>
3. <span data-ttu-id="f9b3f-123">Vyberte **Proxy aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-123">Select **Application Proxy**.</span></span> 
4. <span data-ttu-id="f9b3f-124">V poli externí adresu URL pomocí rozevíracího seznamu vyberte vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-124">In the External URL field, use the dropdown list to select your custom domain.</span></span> <span data-ttu-id="f9b3f-125">Pokud nevidíte doménu v seznamu, pak jej nebyla ověřena ještě.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-125">If you don't see your domain in the list, then it hasn't been verified yet.</span></span> 
5. <span data-ttu-id="f9b3f-126">Vyberte **uložit**</span><span class="sxs-lookup"><span data-stu-id="f9b3f-126">Select **Save**</span></span>
5. <span data-ttu-id="f9b3f-127">**Certifikát** nepovolíte pole, která byla zakázána.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-127">The **Certificate** field that was disabled becomes enabled.</span></span> <span data-ttu-id="f9b3f-128">Toto políčko zaškrtněte.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-128">Select this field.</span></span> 

   ![Klikněte na tlačítko pro odeslání certifikátu](./media/active-directory-application-proxy-custom-domains/certificate.png)

   <span data-ttu-id="f9b3f-130">Pokud jste již odeslali certifikát pro tuto doménu, pole certifikátu zobrazí informace o certifikátu.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-130">If you already uploaded a certificate for this domain, the Certificate field displays the certificate information.</span></span> 

6. <span data-ttu-id="f9b3f-131">Nahrát na server certifikát PFX a zadejte heslo pro certifikát.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-131">Upload the PFX certificate and enter the password for the certificate.</span></span> 
7. <span data-ttu-id="f9b3f-132">Vyberte **Uložit** uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-132">Select **Save** to save your changes.</span></span> 
8. <span data-ttu-id="f9b3f-133">Přidat [záznam DNS](../dns/dns-operations-recordsets-portal.md) , přesměruje nový externí adresu URL k doméně msappproxy.net.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-133">Add a [DNS record](../dns/dns-operations-recordsets-portal.md) that redirects the new external URL to the msappproxy.net domain.</span></span> 

>[!TIP] 
><span data-ttu-id="f9b3f-134">Potřebujete jenom nahrát jeden certifikát na vlastní domény.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-134">You only need to upload one certificate per custom domain.</span></span> <span data-ttu-id="f9b3f-135">Jakmile nahrajete certifikát, můžete zvolit vlastní domény při publikování nové aplikace a není nutné provést další konfiguraci, s výjimkou záznamu DNS.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-135">Once you upload a certificate, you can choose the custom domain when you publish a new app and not have to do additional configuration except for the DNS record.</span></span> 

## <a name="manage-certificates"></a><span data-ttu-id="f9b3f-136">Správa certifikátů</span><span class="sxs-lookup"><span data-stu-id="f9b3f-136">Manage certificates</span></span>

### <a name="certificate-format"></a><span data-ttu-id="f9b3f-137">Formát certifikátu</span><span class="sxs-lookup"><span data-stu-id="f9b3f-137">Certificate format</span></span>
<span data-ttu-id="f9b3f-138">Neexistuje žádné omezení v certifikátu podpis metody.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-138">There is no restriction on the certificate signature methods.</span></span> <span data-ttu-id="f9b3f-139">Šifrování ECC (Elliptic Curve), alternativní název předmětu (SAN) a další běžné typy certifikátů jsou všechny podporované.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-139">Elliptic Curve Cryptography (ECC), Subject Alternative Name (SAN), and other common certificate types are all supported.</span></span> 

<span data-ttu-id="f9b3f-140">Certifikát se zástupným znakem můžete použít také zástupného odpovídá požadované externí adresu URL.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-140">You can use a wildcard certificate as long as the wildcard matches the desired external URL.</span></span> 

<span data-ttu-id="f9b3f-141">Můžete použít certifikáty podepsané svým držitelem, také.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-141">You can use self-signed certificates, as well.</span></span> <span data-ttu-id="f9b3f-142">Pokud používáte privátní certifikační autority, by měly být veřejné CDP (certifikát odvolání bodu distribučního bodu) pro certifikát.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-142">If you’re using a private certificate authority, the CDP (certificate revocation point distribution point) for the certificate should be public.</span></span>

### <a name="changing-the-domain"></a><span data-ttu-id="f9b3f-143">Změna domény</span><span class="sxs-lookup"><span data-stu-id="f9b3f-143">Changing the domain</span></span>
<span data-ttu-id="f9b3f-144">V rozevíracím seznamu externí adresu URL pro vaši aplikaci se zobrazí všechny ověřené domény.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-144">All verified domains appear in the External URL dropdown list for your application.</span></span> <span data-ttu-id="f9b3f-145">Změnit doménu, právě aktualizujte toto pole pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-145">To change the domain, just update that field for the application.</span></span> <span data-ttu-id="f9b3f-146">Pokud chcete doménu není v seznamu [přidejte jej jako ověřené domény](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f9b3f-146">If the domain you want isn't in the list, [add it as a verified domain](active-directory-domains-add-azure-portal.md).</span></span> <span data-ttu-id="f9b3f-147">Pokud vyberete doméně, která mají přidružený certifikát ještě, postupujte podle kroků 5 až 7 se přidat certifikát.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-147">If you select a domain that doesn't have an associated certificate yet, follow steps 5-7 to add the certificate.</span></span> <span data-ttu-id="f9b3f-148">Ujistěte se, že aktualizujete záznam DNS pro přesměrování z nové externí adresu URL.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-148">Then, make sure you update the DNS record to redirect from the new external URL.</span></span> 

### <a name="certificate-management"></a><span data-ttu-id="f9b3f-149">Správa certifikátů</span><span class="sxs-lookup"><span data-stu-id="f9b3f-149">Certificate management</span></span>
<span data-ttu-id="f9b3f-150">Pokud aplikace sdílet externího hostitele, můžete použít stejný certifikát pro více aplikací.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-150">You can use the same certificate for multiple applications unless the applications share an external host.</span></span> 

<span data-ttu-id="f9b3f-151">Se zobrazí upozornění, když vyprší platnost certifikátu, s výzvou k odeslání dalšího certifikátu prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-151">You get a warning when a certificate expires, telling you to upload another certificate through the portal.</span></span> <span data-ttu-id="f9b3f-152">Pokud je certifikát odvolán, může se uživatelům zobrazí upozornění zabezpečení při přístupu k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-152">If the certificate is revoked, your users may see a security warning when accessing the application.</span></span> <span data-ttu-id="f9b3f-153">Neprovádějte jsme kontroly odvolání certifikátů.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-153">We don’t perform revocation checks for certificates.</span></span>  <span data-ttu-id="f9b3f-154">Chcete-li aktualizovat certifikát pro danou aplikaci, přejděte do aplikace a postupujte podle kroků 5 až 7 pro konfiguraci vlastních domén na publikované aplikace nahrát nový certifikát.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-154">To update the certificate for a given application, navigate to the application and follow steps 5-7 for configuring custom domains on published applications to upload a new certificate.</span></span> <span data-ttu-id="f9b3f-155">Pokud starý certifikát není používán jinými aplikacemi, je automaticky odstraněna.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-155">If the old certificate is not being used by other applications, it is deleted automatically.</span></span> 

<span data-ttu-id="f9b3f-156">Aktuálně všechny správy certifikátů je prostřednictvím stránky jednotlivých aplikací, takže budete muset spravovat certifikáty v kontextu příslušné aplikace.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-156">Currently all certificate management is through individual application pages so you need to manage certificates in the context of the relevant applications.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f9b3f-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f9b3f-157">Next steps</span></span>
* <span data-ttu-id="f9b3f-158">[Povolit jednotné přihlašování](active-directory-application-proxy-sso-using-kcd.md) pro vaše publikované aplikace pomocí ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-158">[Enable single sign-on](active-directory-application-proxy-sso-using-kcd.md) to your published apps with Azure AD authentication.</span></span>
* <span data-ttu-id="f9b3f-159">[Povolení podmíněného přístupu](active-directory-application-proxy-conditional-access.md) k publikovaným aplikacím.</span><span class="sxs-lookup"><span data-stu-id="f9b3f-159">[Enable conditional access](active-directory-application-proxy-conditional-access.md) to your published apps.</span></span>
* [<span data-ttu-id="f9b3f-160">Přidání vlastního názvu domény do Azure AD</span><span class="sxs-lookup"><span data-stu-id="f9b3f-160">Add your custom domain name to Azure AD</span></span>](active-directory-domains-add-azure-portal.md)


