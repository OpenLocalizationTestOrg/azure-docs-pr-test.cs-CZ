---
title: "Autorizovat vývojářským účtům pomocí Azure Active Directory B2C - Azure API Management | Microsoft Docs"
description: "Zjistěte, jak mají autorizovat uživatelé pomocí Azure Active Directory B2C ve službě API Management."
services: api-management
documentationcenter: API Management
author: miaojiang
manager: erikre
editor: 
ms.assetid: 33a69a83-94f2-4e4e-9cef-f2a5af3c9732
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: eb7deb1a79d9db9ac5cfbea69b8d3c564eb55577
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-authorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a><span data-ttu-id="d6efa-103">Jak se mají autorizovat vývojářským účtům pomocí Azure Active Directory B2C ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="d6efa-103">How to authorize developer accounts by using Azure Active Directory B2C in Azure API Management</span></span>
## <a name="overview"></a><span data-ttu-id="d6efa-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="d6efa-104">Overview</span></span>
<span data-ttu-id="d6efa-105">Azure Active Directory B2C je cloudové řešení správy identity pro určených webové a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6efa-105">Azure Active Directory B2C is a cloud identity management solution for consumer-facing web and mobile applications.</span></span> <span data-ttu-id="d6efa-106">Můžete ho použít ke správě přístupu na portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="d6efa-106">You can use it to manage access to your developer portal.</span></span> <span data-ttu-id="d6efa-107">Tento průvodce vám ukáže konfigurací, která je vyžadována ve službě API Management k integraci s Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="d6efa-107">This guide shows you the configuration that's required in your API Management service to integrate with Azure Active Directory B2C.</span></span> <span data-ttu-id="d6efa-108">Informace o povolení přístup k portálu pro vývojáře pomocí klasického Azure Active Directory najdete v tématu [autorizace vývojářským účtům pomocí služby Azure Active Directory].</span><span class="sxs-lookup"><span data-stu-id="d6efa-108">For information about enabling access to the developer portal by using classic Azure Active Directory, see [How to authorize developer accounts using Azure Active Directory].</span></span>

> [!NOTE]
> <span data-ttu-id="d6efa-109">Pokud chcete provést kroky v této příručce, musí mít první klienta služby Azure Active Directory B2C vytvořit aplikaci v.</span><span class="sxs-lookup"><span data-stu-id="d6efa-109">To complete the steps in this guide, you must first have an Azure Active Directory B2C tenant to create an application in.</span></span> <span data-ttu-id="d6efa-110">Také je potřeba mít registrace a přihlášení zásady, které jsou připravené.</span><span class="sxs-lookup"><span data-stu-id="d6efa-110">Also, you need to have signup and signin policies ready.</span></span> <span data-ttu-id="d6efa-111">Další informace najdete v tématu [přehled Azure Active Directory B2C].</span><span class="sxs-lookup"><span data-stu-id="d6efa-111">For more information, see [Azure Active Directory B2C overview].</span></span>

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a><span data-ttu-id="d6efa-112">Autorizovat vývojářským účtům pomocí Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="d6efa-112">Authorize developer accounts by using Azure Active Directory B2C</span></span>

1. <span data-ttu-id="d6efa-113">Chcete-li začít, klikněte na tlačítko **portál vydavatele** na portálu Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="d6efa-113">To get started, click **Publisher portal** in the Azure portal for your API Management service.</span></span> <span data-ttu-id="d6efa-114">Tím přejdete na portál vydavatele služby API Management.</span><span class="sxs-lookup"><span data-stu-id="d6efa-114">This takes you to the API Management publisher portal.</span></span>

   ![Portál vydavatele][api-management-management-console]

   > [!NOTE]
   > <span data-ttu-id="d6efa-116">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v [Začínáme s Azure API Management kurzu] [Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="d6efa-116">If you haven't yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management tutorial][Get started with Azure API Management].</span></span>

2. <span data-ttu-id="d6efa-117">Na **API Management** nabídky, klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="d6efa-117">On the **API Management** menu, click **Security**.</span></span> <span data-ttu-id="d6efa-118">Na **identity** , zvolte **Azure Active Directory B2C**.</span><span class="sxs-lookup"><span data-stu-id="d6efa-118">On the **Identities** tab, choose **Azure Active Directory B2C**.</span></span>

  ![Externí identity 1][api-management-howto-aad-b2c-security-tab]

3. <span data-ttu-id="d6efa-120">Poznamenejte si **adresy URL pro přesměrování** a přepnout na Azure Active Directory B2C na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d6efa-120">Make a note of the **Redirect URL** and switch over to Azure Active Directory B2C in the Azure portal.</span></span>

  ![Externí identity 2][api-management-howto-aad-b2c-security-tab-reply-url]

4. <span data-ttu-id="d6efa-122">Klikněte **aplikace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d6efa-122">Click the **Applications** button.</span></span>

  ![Zaregistrujte novou aplikaci 1][api-management-howto-aad-b2c-portal-menu]

5. <span data-ttu-id="d6efa-124">Klikněte **přidat** tlačítko vytvořte novou aplikaci Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="d6efa-124">Click the **Add** button to create a new Azure Active Directory B2C application.</span></span>

  ![Zaregistrujte novou aplikaci 2][api-management-howto-aad-b2c-add-button]

6. <span data-ttu-id="d6efa-126">V **novou aplikaci** okno, zadejte název aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6efa-126">In the **New application** blade, enter a name for the application.</span></span> <span data-ttu-id="d6efa-127">Zvolte **Ano** pod **webové aplikaci nebo webové rozhraní API**a zvolte **Ano** pod **povolit implicitní tok**.</span><span class="sxs-lookup"><span data-stu-id="d6efa-127">Choose **Yes** under **Web App/Web API**, and choose **Yes** under **Allow implicit flow**.</span></span> <span data-ttu-id="d6efa-128">Zkopírujte **adresy URL pro přesměrování** z **Azure Active Directory B2C** části **identity** v portálu vydavatele a vložte ji do **odpovědi Adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="d6efa-128">Then copy the **Redirect URL** from the **Azure Active Directory B2C** section of the **Identities** tab in the publisher portal, and paste it into the **Reply URL** text box.</span></span>

  ![Zaregistrujte novou aplikaci 3][api-management-howto-aad-b2c-app-details]

7. <span data-ttu-id="d6efa-130">Klikněte na tlačítko **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d6efa-130">Click the **Create** button.</span></span> <span data-ttu-id="d6efa-131">Při vytvoření aplikace se zobrazí v **aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="d6efa-131">When the application is created, it appears in the **Applications** blade.</span></span> <span data-ttu-id="d6efa-132">Klikněte na název aplikace zobrazíte její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d6efa-132">Click the application name to see its details.</span></span>

  ![Zaregistrujte novou aplikaci 4][api-management-howto-aad-b2c-app-created]

8. <span data-ttu-id="d6efa-134">Z **vlastnosti** zkopírujte **ID aplikace** do schránky.</span><span class="sxs-lookup"><span data-stu-id="d6efa-134">From the **Properties** blade, copy the **Application ID** to the clipboard.</span></span>

  ![ID aplikace 1][api-management-howto-aad-b2c-app-id]

9. <span data-ttu-id="d6efa-136">Přepněte zpět na portál vydavatele a ID do vložte **Id klienta** textové pole.</span><span class="sxs-lookup"><span data-stu-id="d6efa-136">Switch back to the publisher portal and paste the ID into the **Client Id** text box.</span></span>

  ![ID aplikace 2][api-management-howto-aad-b2c-client-id]

10. <span data-ttu-id="d6efa-138">Přepněte zpět na portálu Azure, klikněte na tlačítko **klíče** tlačítko a potom klikněte na **vygenerovat klíč**.</span><span class="sxs-lookup"><span data-stu-id="d6efa-138">Switch back to the Azure portal, click the **Keys** button, and then click **Generate key**.</span></span> <span data-ttu-id="d6efa-139">Klikněte na tlačítko **Uložit** k uložení konfigurace a zobrazení **klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d6efa-139">Click **Save** to save the configuration and display the **App key**.</span></span> <span data-ttu-id="d6efa-140">Klíč zkopírujte do schránky.</span><span class="sxs-lookup"><span data-stu-id="d6efa-140">Copy the key to the clipboard.</span></span>

  ![Aplikace klíč 1][api-management-howto-aad-b2c-app-key]

11. <span data-ttu-id="d6efa-142">Přepněte zpět na portál vydavatele a vložte klíč do **tajný klíč klienta** textové pole.</span><span class="sxs-lookup"><span data-stu-id="d6efa-142">Switch back to the publisher portal and paste the key into the **Client Secret** text box.</span></span>

  ![Klíč aplikace 2][api-management-howto-aad-b2c-client-secret]

12. <span data-ttu-id="d6efa-144">Zadejte název domény klienta Azure Active Directory B2C v **povolené klienta**.</span><span class="sxs-lookup"><span data-stu-id="d6efa-144">Specify the domain name of the Azure Active Directory B2C tenant in **Allowed Tenant**.</span></span>

  ![Povolené klienta][api-management-howto-aad-b2c-allowed-tenant]

13. <span data-ttu-id="d6efa-146">Zadejte **zásady registrace** a **přihlášení zásady**.</span><span class="sxs-lookup"><span data-stu-id="d6efa-146">Specify the **Signup Policy** and **Signin Policy**.</span></span> <span data-ttu-id="d6efa-147">Volitelně můžete zadat také **zásady úpravy profilu** a **zásady resetování hesel**.</span><span class="sxs-lookup"><span data-stu-id="d6efa-147">Optionally, you can also provide the **Profile Editing Policy** and **Password Reset Policy**.</span></span>

  ![Zásady][api-management-howto-aad-b2c-policies]

  > [!NOTE]
  > <span data-ttu-id="d6efa-149">Další informace o zásadách najdete v tématu [Azure Active Directory B2C: rozšiřitelný rámec zásad].</span><span class="sxs-lookup"><span data-stu-id="d6efa-149">For more information on policies, see [Azure Active Directory B2C: Extensible policy framework].</span></span>

14. <span data-ttu-id="d6efa-150">Po zadání požadované konfigurace, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d6efa-150">After you've specified the desired configuration, click **Save**.</span></span>

  <span data-ttu-id="d6efa-151">Po uložení změn se vývojáři bude moct vytvářet nové účty a přihlaste se k portálu pro vývojáře pomocí Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="d6efa-151">After the changes are saved, developers will be able to create new accounts and sign in to the developer portal by using Azure Active Directory B2C.</span></span>

## <a name="sign-up-for-a-developer-account-by-using-azure-active-directory-b2c"></a><span data-ttu-id="d6efa-152">Zaregistrujte si účet pro vývojáře pomocí Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="d6efa-152">Sign up for a developer account by using Azure Active Directory B2C</span></span>

1. <span data-ttu-id="d6efa-153">Zaregistrujte si účet pro vývojáře pomocí Azure Active Directory B2C, otevřete nové okno prohlížeče a přejděte na portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="d6efa-153">To sign up for a developer account by using Azure Active Directory B2C, open a new browser window and go to the developer portal.</span></span> <span data-ttu-id="d6efa-154">Klikněte **zaregistrovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d6efa-154">Click the **Sign up** button.</span></span>

   ![Portál pro vývojáře 1][api-management-howto-aad-b2c-dev-portal]

2. <span data-ttu-id="d6efa-156">Zvolte se zaregistrovat **Azure Active Directory B2C**.</span><span class="sxs-lookup"><span data-stu-id="d6efa-156">Choose to sign up with **Azure Active Directory B2C**.</span></span>

   ![Portál pro vývojáře 2][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. <span data-ttu-id="d6efa-158">Budete přesměrováni na zásady registrace, který jste nakonfigurovali v předchozím oddílu.</span><span class="sxs-lookup"><span data-stu-id="d6efa-158">You're redirected to the signup policy that you configured in the previous section.</span></span> <span data-ttu-id="d6efa-159">Můžete přihlásit pomocí e-mailovou adresu nebo jeden z vaší účtů na sociálních.</span><span class="sxs-lookup"><span data-stu-id="d6efa-159">Choose to sign up by using your email address or one of your existing social accounts.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d6efa-160">Pokud Azure Active Directory B2C je jedinou možností, které je zapnutá **identity** karta na portálu vydavatele je budete přesměrováni na zásady registrace přímo.</span><span class="sxs-lookup"><span data-stu-id="d6efa-160">If Azure Active Directory B2C is the only option that's enabled on the **Identities** tab in the publisher portal, you'll be redirected to the signup policy directly.</span></span>

   ![Portál pro vývojáře][api-management-howto-aad-b2c-dev-portal-b2c-options]

   <span data-ttu-id="d6efa-162">Po dokončení registrace budete přesměrováni zpět na portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="d6efa-162">When the signup is complete, you're redirected back to the developer portal.</span></span> <span data-ttu-id="d6efa-163">Jste teď přihlášení na portál pro vývojáře pro instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="d6efa-163">You're now signed in to the developer portal for your API Management service instance.</span></span>

    ![Registrace je dokončena.][api-management-registration-complete]

## <a name="next-steps"></a><span data-ttu-id="d6efa-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d6efa-165">Next steps</span></span>

*  <span data-ttu-id="d6efa-166">[přehled Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="d6efa-166">[Azure Active Directory B2C overview]</span></span>
*  <span data-ttu-id="d6efa-167">[Azure Active Directory B2C: rozšiřitelný rámec zásad]</span><span class="sxs-lookup"><span data-stu-id="d6efa-167">[Azure Active Directory B2C: Extensible policy framework]</span></span>
*  <span data-ttu-id="d6efa-168">[Použít účet Microsoft jako poskytovatel identit v Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="d6efa-168">[Use a Microsoft account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="d6efa-169">[Použít účet Google jako poskytovatel identit v Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="d6efa-169">[Use a Google account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="d6efa-170">[Použít účet LinkedIn jako poskytovatel identit v Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="d6efa-170">[Use a LinkedIn account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="d6efa-171">[Použít účet Facebook jako poskytovatel identit v Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="d6efa-171">[Use a Facebook account as an identity provider in Azure Active Directory B2C]</span></span>




[api-management-howto-aad-b2c-security-tab]: ./media/api-management-howto-aad-b2c/api-management-b2c-security-tab.PNG
[api-management-howto-aad-b2c-security-tab-reply-url]: ./media/api-management-howto-aad-b2c/api-management-b2c-security-tab-reply-url.PNG
[api-management-howto-aad-b2c-portal-menu]: ./media/api-management-howto-aad-b2c/api-management-b2c-portal-menu.PNG
[api-management-howto-aad-b2c-add-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-add-button.PNG
[api-management-howto-aad-b2c-app-details]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-details.PNG
[api-management-howto-aad-b2c-app-created]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-created.PNG
[api-management-howto-aad-b2c-app-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-id.PNG
[api-management-howto-aad-b2c-client-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-id.PNG
[api-management-howto-aad-b2c-app-key]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key.PNG
[api-management-howto-aad-b2c-app-key-saved]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key-saved.PNG
[api-management-howto-aad-b2c-client-secret]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-secret.PNG
[api-management-howto-aad-b2c-allowed-tenant]: ./media/api-management-howto-aad-b2c/api-management-b2c-allowed-tenant.PNG
[api-management-howto-aad-b2c-policies]: ./media/api-management-howto-aad-b2c/api-management-b2c-policies.PNG
[api-management-howto-aad-b2c-dev-portal]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-button.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-options]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-options.PNG
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.PNG
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-b2c-security-tab.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph
<span data-ttu-id="d6efa-172">[přehled Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview</span><span class="sxs-lookup"><span data-stu-id="d6efa-172">[Azure Active Directory B2C overview]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview</span></span>
<span data-ttu-id="d6efa-173">[autorizace vývojářským účtům pomocí služby Azure Active Directory]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad</span><span class="sxs-lookup"><span data-stu-id="d6efa-173">[How to authorize developer accounts using Azure Active Directory]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad</span></span>
<span data-ttu-id="d6efa-174">[Azure Active Directory B2C: rozšiřitelný rámec zásad]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies</span><span class="sxs-lookup"><span data-stu-id="d6efa-174">[Azure Active Directory B2C: Extensible policy framework]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies</span></span>
<span data-ttu-id="d6efa-175">[Použít účet Microsoft jako poskytovatel identit v Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app</span><span class="sxs-lookup"><span data-stu-id="d6efa-175">[Use a Microsoft account as an identity provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app</span></span>
<span data-ttu-id="d6efa-176">[Použít účet Google jako poskytovatel identit v Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app</span><span class="sxs-lookup"><span data-stu-id="d6efa-176">[Use a Google account as an identity provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app</span></span>
<span data-ttu-id="d6efa-177">[Použít účet Facebook jako poskytovatel identit v Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app</span><span class="sxs-lookup"><span data-stu-id="d6efa-177">[Use a Facebook account as an identity provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app</span></span>
<span data-ttu-id="d6efa-178">[Použít účet LinkedIn jako poskytovatel identit v Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-li-app</span><span class="sxs-lookup"><span data-stu-id="d6efa-178">[Use a LinkedIn account as an identity provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-li-app</span></span>

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
