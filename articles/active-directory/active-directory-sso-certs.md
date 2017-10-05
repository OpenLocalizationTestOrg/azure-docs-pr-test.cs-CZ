---
title: "Spravovat federačních certifikátů ve službě Azure AD | Microsoft Docs"
description: "Zjistěte, jak přizpůsobit datum vypršení platnosti federačních certifikátů a jak obnovit certifikáty, jejichž platnost brzy vyprší."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: f516f7f0-b25a-4901-8247-f5964666ce23
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2017
ms.author: jeedes
ms.openlocfilehash: 1283b570200f05003658824760ecbb6722f241d9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a><span data-ttu-id="6f219-103">Spravovat certifikáty pro federované jednotné přihlašování v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6f219-103">Manage certificates for federated single sign-on in Azure Active Directory</span></span>
<span data-ttu-id="6f219-104">Tento článek popisuje běžné otázky a informace související s certifikáty, které vytvoří Azure Active Directory (Azure AD) k vytvoření federovaného jednotného přihlašování (SSO) do aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6f219-104">This article covers common questions and information related to the certificates that Azure Active Directory (Azure AD) creates to establish federated single sign-on (SSO) to your SaaS applications.</span></span> <span data-ttu-id="6f219-105">Přidáte aplikace v galerii aplikací Azure AD nebo pomocí šablony aplikace bez galerie.</span><span class="sxs-lookup"><span data-stu-id="6f219-105">Add applications from the Azure AD app gallery or by using a non-gallery application template.</span></span> <span data-ttu-id="6f219-106">Konfigurace aplikace s použitím možnosti federované jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6f219-106">Configure the application by using the federated SSO option.</span></span>

<span data-ttu-id="6f219-107">Tento článek se týká jenom na aplikace, které jsou nakonfigurovány pro použití Azure AD jednotného přihlašování pomocí SAML federování, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="6f219-107">This article is relevant only to apps that are configured to use Azure AD SSO through SAML federation, as shown in the following example:</span></span>

![Azure AD jednotné přihlášení](./media/active-directory-sso-certs/saml_sso.PNG)

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a><span data-ttu-id="6f219-109">Automaticky vygeneruje certifikát pro galerii a bez Galerie aplikace</span><span class="sxs-lookup"><span data-stu-id="6f219-109">Auto-generated certificate for gallery and non-gallery applications</span></span>
<span data-ttu-id="6f219-110">Při přidání nové aplikace z galerie a konfiguraci základě SAML přihlašování, Azure AD vygeneruje certifikát pro aplikaci, která je platný po dobu tří let.</span><span class="sxs-lookup"><span data-stu-id="6f219-110">When you add a new application from the gallery and configure a SAML-based sign-on, Azure AD generates a certificate for the application that is valid for three years.</span></span> <span data-ttu-id="6f219-111">Tento certifikát od si můžete stáhnout **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="6f219-111">You can download this certificate from the **SAML Signing Certificate** section.</span></span> <span data-ttu-id="6f219-112">Pro aplikace, galerie může zobrazit v této části můžete stáhnout certifikát nebo metadata, v závislosti na požadavek na aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f219-112">For gallery applications, this section might show an option to download the certificate or metadata, depending on the requirement of the application.</span></span>

![Azure AD jednotné přihlašování](./media/active-directory-sso-certs/saml_certificate_download.png)

## <a name="customize-the-expiration-date-for-your-federation-certificate-and-roll-it-over-to-a-new-certificate"></a><span data-ttu-id="6f219-114">Přizpůsobení datum vypršení platnosti vašeho certifikátu federace a přejít do nového certifikátu</span><span class="sxs-lookup"><span data-stu-id="6f219-114">Customize the expiration date for your federation certificate and roll it over to a new certificate</span></span>
<span data-ttu-id="6f219-115">Ve výchozím nastavení jsou nastavené certifikáty vyprší po tři roky.</span><span class="sxs-lookup"><span data-stu-id="6f219-115">By default, certificates are set to expire after three years.</span></span> <span data-ttu-id="6f219-116">Pomocí následujících kroků můžete různých platnosti certifikátu.</span><span class="sxs-lookup"><span data-stu-id="6f219-116">You can choose a different expiration date for your certificate by completing the following steps.</span></span>
<span data-ttu-id="6f219-117">Na snímcích obrazovky používat Salesforce z důvodu příklad, ale tyto kroky provést u všech federovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6f219-117">The screenshots use Salesforce for the sake of example, but these steps can apply to any federated SaaS app.</span></span>

1. <span data-ttu-id="6f219-118">V [portál Azure](https://aad.portal.azure.com), klikněte na tlačítko **podniková aplikace** v levém podokně a pak klikněte na tlačítko **novou aplikaci** na **přehled** stránky:</span><span class="sxs-lookup"><span data-stu-id="6f219-118">In the [Azure portal](https://aad.portal.azure.com), click **Enterprise application** in the left pane and then click **New application** on the **Overview** page:</span></span>

   ![Otevřete průvodce Konfigurace jednotného přihlašování](./media/active-directory-sso-certs/enterprise_application_new_application.png)

2. <span data-ttu-id="6f219-120">Vyhledejte Galerie aplikace a pak vyberte aplikaci, kterou chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="6f219-120">Search for the gallery application and then select the application that you want to add.</span></span> <span data-ttu-id="6f219-121">Pokud nemůžete najít požadované aplikace, přidání aplikace pomocí **aplikace bez Galerie** možnost.</span><span class="sxs-lookup"><span data-stu-id="6f219-121">If you cannot find the required application, add the application by using the **Non-gallery application** option.</span></span> <span data-ttu-id="6f219-122">Tato funkce je k dispozici pouze v Azure AD Premium (P1 a P2) SKU.</span><span class="sxs-lookup"><span data-stu-id="6f219-122">This feature is available only in the Azure AD Premium (P1 and P2) SKU.</span></span>

    ![Azure AD jednotné přihlašování](./media/active-directory-sso-certs/add_gallery_application.png)

3. <span data-ttu-id="6f219-124">Klikněte na tlačítko **jednotného přihlašování** na odkaz v levém podokně a změňte **režimu přihlašování** k **na základě SAML přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6f219-124">Click the **Single sign-on** link in the left pane and change **Single Sign-on Mode** to **SAML-based Sign-on**.</span></span> <span data-ttu-id="6f219-125">Tento krok vygeneruje certifikát tři roky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6f219-125">This step generates a three-year certificate for your application.</span></span>

4. <span data-ttu-id="6f219-126">Chcete-li vytvořit nový certifikát, klikněte na tlačítko **vytvořit nový certifikát** na odkaz v **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="6f219-126">To create a new certificate, click the **Create new certificate** link in the **SAML Signing Certificate** section.</span></span>

    ![Vygenerovat nový certifikát](./media/active-directory-sso-certs/create_new_certficate.png)

5. <span data-ttu-id="6f219-128">**Vytvořit nový certifikát** odkaz otevře ovládacího prvku kalendář.</span><span class="sxs-lookup"><span data-stu-id="6f219-128">The **Create a new certificate** link opens the calendar control.</span></span> <span data-ttu-id="6f219-129">Můžete nastavit žádné datum a čas až tři roky od dnešního data.</span><span class="sxs-lookup"><span data-stu-id="6f219-129">You can set any date and time up to three years from the current date.</span></span> <span data-ttu-id="6f219-130">Vybrané datum a čas je nové datum vypršení platnosti a čas nový certifikát.</span><span class="sxs-lookup"><span data-stu-id="6f219-130">The selected date and time is the new expiration date and time of your new certificate.</span></span> <span data-ttu-id="6f219-131">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6f219-131">Click **Save**.</span></span>

    ![Stáhněte si potom odešlete certifikát](./media/active-directory-sso-certs/certifcate_date_selection.PNG)

6. <span data-ttu-id="6f219-133">Nový certifikát je nyní k dispozici ke stažení.</span><span class="sxs-lookup"><span data-stu-id="6f219-133">Now the new certificate is available to download.</span></span> <span data-ttu-id="6f219-134">Klikněte **certifikát** odkaz na stažení.</span><span class="sxs-lookup"><span data-stu-id="6f219-134">Click the **Certificate** link to download it.</span></span> <span data-ttu-id="6f219-135">V tomto okamžiku by váš certifikát není aktivní.</span><span class="sxs-lookup"><span data-stu-id="6f219-135">At this point, your certificate is not active.</span></span> <span data-ttu-id="6f219-136">Pokud chcete přejít na tento certifikát, vyberte **aktivujte nový certifikát** zaškrtávací políčko a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6f219-136">When you want to roll over to this certificate, select the **Make new certificate active** check box and click **Save**.</span></span> <span data-ttu-id="6f219-137">Od tohoto okamžiku spustí Azure AD pomocí nového certifikátu pro podepisování odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6f219-137">From that point, Azure AD starts using the new certificate for signing the response.</span></span>

7.  <span data-ttu-id="6f219-138">Informace o tom, na kterou odešlete certifikát k určité aplikaci SaaS, klikněte **kurz konfigurace zobrazení aplikace** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6f219-138">To learn how to upload the certificate to your particular SaaS application, click the **View application configuration tutorial** link.</span></span>

## <a name="renew-a-certificate-that-will-soon-expire"></a><span data-ttu-id="6f219-139">Obnovení certifikátu, kterému brzy vyprší</span><span class="sxs-lookup"><span data-stu-id="6f219-139">Renew a certificate that will soon expire</span></span>
<span data-ttu-id="6f219-140">Následující kroky obnovení musí mít za následek žádné významné výpadky pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="6f219-140">The following renewal steps should result in no significant downtime for your users.</span></span> <span data-ttu-id="6f219-141">Snímky obrazovky v této části funkce Salesforce jako příklad, ale tyto kroky můžete použít na všech federovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6f219-141">The screenshots in this section feature Salesforce as an example, but these steps can apply to any federated SaaS app.</span></span>

1. <span data-ttu-id="6f219-142">Na **Azure Active Directory** aplikace **jednotného přihlašování** stránky, vygenerujte nový certifikát pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6f219-142">On the **Azure Active Directory** application **Single sign-on** page, generate the new certificate for your application.</span></span> <span data-ttu-id="6f219-143">To provedete kliknutím **vytvořit nový certifikát** na odkaz v **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="6f219-143">You can do this by clicking the **Create new certificate** link in the **SAML Signing Certificate** section.</span></span>

    ![Vygenerovat nový certifikát](./media/active-directory-sso-certs/create_new_certficate.png)

2. <span data-ttu-id="6f219-145">Vyberte požadovanou vypršení platnosti datum a čas nového certifikátu a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6f219-145">Select the desired expiration date and time for your new certificate and click **Save**.</span></span>

3. <span data-ttu-id="6f219-146">Stáhnout certifikát v **certifikát pro podpis SAML** možnost.</span><span class="sxs-lookup"><span data-stu-id="6f219-146">Download the certificate in the **SAML Signing certificate** option.</span></span> <span data-ttu-id="6f219-147">Nahrajte nový certifikát na obrazovku aplikace SaaS jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6f219-147">Upload the new certificate to the SaaS application's single sign-on configuration screen.</span></span> <span data-ttu-id="6f219-148">Chcete-li se dozvědět, jak to provést u konkrétní aplikace SaaS, klikněte na tlačítko **kurz konfigurace zobrazení aplikace** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6f219-148">To learn how to do this for your particular SaaS application, click the **View application configuration tutorial** link.</span></span>
   
4. <span data-ttu-id="6f219-149">Pokud chcete aktivovat nový certifikát ve službě Azure AD, vyberte **aktivujte nový certifikát** zaškrtávací políčko a klikněte na **Uložit** tlačítka v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="6f219-149">To activate the new certificate in Azure AD, select the **Make new certificate active** check box and click the **Save** button at the top of the page.</span></span> <span data-ttu-id="6f219-150">To zahrnuje přes nový certifikát na straně Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6f219-150">This rolls over the new certificate on the Azure AD side.</span></span> <span data-ttu-id="6f219-151">Změní stav certifikátu z **nový** k **Active**.</span><span class="sxs-lookup"><span data-stu-id="6f219-151">The status of the certificate changes from **New** to **Active**.</span></span> <span data-ttu-id="6f219-152">Od tohoto okamžiku spustí Azure AD pomocí nového certifikátu pro podepisování odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6f219-152">From that point, Azure AD starts using the new certificate for signing the response.</span></span> 
   
    ![Vygenerovat nový certifikát](./media/active-directory-sso-certs/new_certificate_download.png)

## <a name="related-articles"></a><span data-ttu-id="6f219-154">Související články</span><span class="sxs-lookup"><span data-stu-id="6f219-154">Related articles</span></span>
* [<span data-ttu-id="6f219-155">Seznam kurzů k integraci aplikací SaaS v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6f219-155">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6f219-156">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6f219-156">Article index for application management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="6f219-157">Přístup k aplikaci a jednotné přihlašování s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6f219-157">Application access and single sign-on with Azure Active Directory</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="6f219-158">Řešení potíží s na základě SAML jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6f219-158">Troubleshooting SAML-based single sign-on</span></span>](active-directory-saml-debugging.md)
