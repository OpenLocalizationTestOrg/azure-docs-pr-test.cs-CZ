---
title: "aaaAuthorize vývojářským účtům pomocí Azure Active Directory B2C - Azure API Management | Microsoft Docs"
description: "Zjistěte, jak uživatelé tooauthorize pomocí Azure Active Directory B2C ve službě API Management."
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
ms.openlocfilehash: 28f7cae53138938dbbc848b4afcbf08b72690e37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a><span data-ttu-id="2e135-103">Jak tooauthorize vývojáře účtů pomocí Azure Active Directory B2C ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="2e135-103">How tooauthorize developer accounts by using Azure Active Directory B2C in Azure API Management</span></span>
## <a name="overview"></a><span data-ttu-id="2e135-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="2e135-104">Overview</span></span>
<span data-ttu-id="2e135-105">Azure Active Directory B2C je cloudové řešení správy identity pro určených webové a mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="2e135-105">Azure Active Directory B2C is a cloud identity management solution for consumer-facing web and mobile applications.</span></span> <span data-ttu-id="2e135-106">Můžete ji použít portál pro vývojáře tooyour toomanage přístup.</span><span class="sxs-lookup"><span data-stu-id="2e135-106">You can use it toomanage access tooyour developer portal.</span></span> <span data-ttu-id="2e135-107">Tato příručka obsahuje hello konfigurací, která je vyžadována ve vaší toointegrate služby API Management s Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="2e135-107">This guide shows you hello configuration that's required in your API Management service toointegrate with Azure Active Directory B2C.</span></span> <span data-ttu-id="2e135-108">Informace o povolení portál pro vývojáře toohello přístup pomocí klasického Azure Active Directory najdete v tématu [jak tooauthorize vývojáře účtů pomocí služby Azure Active Directory].</span><span class="sxs-lookup"><span data-stu-id="2e135-108">For information about enabling access toohello developer portal by using classic Azure Active Directory, see [How tooauthorize developer accounts using Azure Active Directory].</span></span>

> [!NOTE]
> <span data-ttu-id="2e135-109">toocomplete hello kroky v tomto průvodci, musíte nejprve mít toocreate klienta Azure Active Directory B2C aplikace.</span><span class="sxs-lookup"><span data-stu-id="2e135-109">toocomplete hello steps in this guide, you must first have an Azure Active Directory B2C tenant toocreate an application in.</span></span> <span data-ttu-id="2e135-110">Navíc musíte zásady registrace a přihlášení toohave připraven.</span><span class="sxs-lookup"><span data-stu-id="2e135-110">Also, you need toohave signup and signin policies ready.</span></span> <span data-ttu-id="2e135-111">Další informace najdete v tématu [přehled Azure Active Directory B2C].</span><span class="sxs-lookup"><span data-stu-id="2e135-111">For more information, see [Azure Active Directory B2C overview].</span></span>

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a><span data-ttu-id="2e135-112">Autorizovat vývojářským účtům pomocí Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="2e135-112">Authorize developer accounts by using Azure Active Directory B2C</span></span>

1. <span data-ttu-id="2e135-113">tooget začít, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="2e135-113">tooget started, click **Publisher portal** in hello Azure portal for your API Management service.</span></span> <span data-ttu-id="2e135-114">Tím přejdete portál vydavatele toohello API Management.</span><span class="sxs-lookup"><span data-stu-id="2e135-114">This takes you toohello API Management publisher portal.</span></span>

   ![Portál vydavatele][api-management-management-console]

   > [!NOTE]
   > <span data-ttu-id="2e135-116">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management kurzu][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="2e135-116">If you haven't yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management tutorial][Get started with Azure API Management].</span></span>

2. <span data-ttu-id="2e135-117">Na hello **API Management** nabídky, klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="2e135-117">On hello **API Management** menu, click **Security**.</span></span> <span data-ttu-id="2e135-118">Na hello **identity** , zvolte **Azure Active Directory B2C**.</span><span class="sxs-lookup"><span data-stu-id="2e135-118">On hello **Identities** tab, choose **Azure Active Directory B2C**.</span></span>

  ![Externí identity 1][api-management-howto-aad-b2c-security-tab]

3. <span data-ttu-id="2e135-120">Poznamenejte si hello **adresy URL pro přesměrování** a přepnout tooAzure Active Directory B2C v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2e135-120">Make a note of hello **Redirect URL** and switch over tooAzure Active Directory B2C in hello Azure portal.</span></span>

  ![Externí identity 2][api-management-howto-aad-b2c-security-tab-reply-url]

4. <span data-ttu-id="2e135-122">Klikněte na tlačítko hello **aplikace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2e135-122">Click hello **Applications** button.</span></span>

  ![Zaregistrujte novou aplikaci 1][api-management-howto-aad-b2c-portal-menu]

5. <span data-ttu-id="2e135-124">Klikněte na tlačítko hello **přidat** tlačítko aplikace toocreate nové Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="2e135-124">Click hello **Add** button toocreate a new Azure Active Directory B2C application.</span></span>

  ![Zaregistrujte novou aplikaci 2][api-management-howto-aad-b2c-add-button]

6. <span data-ttu-id="2e135-126">V hello **novou aplikaci** okno, zadejte název aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2e135-126">In hello **New application** blade, enter a name for hello application.</span></span> <span data-ttu-id="2e135-127">Zvolte **Ano** pod **webové aplikaci nebo webové rozhraní API**a zvolte **Ano** pod **povolit implicitní tok**.</span><span class="sxs-lookup"><span data-stu-id="2e135-127">Choose **Yes** under **Web App/Web API**, and choose **Yes** under **Allow implicit flow**.</span></span> <span data-ttu-id="2e135-128">Potom kopie hello **adresy URL pro přesměrování** z hello **Azure Active Directory B2C** části hello **identity** v hello portálu vydavatele a vložte jej do hello **Adresa URL odpovědi** textové pole.</span><span class="sxs-lookup"><span data-stu-id="2e135-128">Then copy hello **Redirect URL** from hello **Azure Active Directory B2C** section of hello **Identities** tab in hello publisher portal, and paste it into hello **Reply URL** text box.</span></span>

  ![Zaregistrujte novou aplikaci 3][api-management-howto-aad-b2c-app-details]

7. <span data-ttu-id="2e135-130">Klikněte na tlačítko hello **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2e135-130">Click hello **Create** button.</span></span> <span data-ttu-id="2e135-131">Když aplikace hello je vytvořen, zobrazí se v hello **aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="2e135-131">When hello application is created, it appears in hello **Applications** blade.</span></span> <span data-ttu-id="2e135-132">Klikněte na tlačítko toosee název aplikace hello její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2e135-132">Click hello application name toosee its details.</span></span>

  ![Zaregistrujte novou aplikaci 4][api-management-howto-aad-b2c-app-created]

8. <span data-ttu-id="2e135-134">Z hello **vlastnosti** okno, kopie hello **ID aplikace** toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="2e135-134">From hello **Properties** blade, copy hello **Application ID** toohello clipboard.</span></span>

  ![ID aplikace 1][api-management-howto-aad-b2c-app-id]

9. <span data-ttu-id="2e135-136">Přepnout zpět toohello portál vydavatele a hello ID vložte do hello **Id klienta** textové pole.</span><span class="sxs-lookup"><span data-stu-id="2e135-136">Switch back toohello publisher portal and paste hello ID into hello **Client Id** text box.</span></span>

  ![ID aplikace 2][api-management-howto-aad-b2c-client-id]

10. <span data-ttu-id="2e135-138">Přepnout zpět toohello portálu Azure, klikněte na tlačítko hello **klíče** tlačítko a potom klikněte na **vygenerovat klíč**.</span><span class="sxs-lookup"><span data-stu-id="2e135-138">Switch back toohello Azure portal, click hello **Keys** button, and then click **Generate key**.</span></span> <span data-ttu-id="2e135-139">Klikněte na tlačítko **Uložit** toosave hello konfiguraci a zobrazení hello **klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2e135-139">Click **Save** toosave hello configuration and display hello **App key**.</span></span> <span data-ttu-id="2e135-140">Kopírování hello klíče toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="2e135-140">Copy hello key toohello clipboard.</span></span>

  ![Aplikace klíč 1][api-management-howto-aad-b2c-app-key]

11. <span data-ttu-id="2e135-142">Přepínač back toohello vydavatele portál a vložit hello klíč do hello **tajný klíč klienta** textové pole.</span><span class="sxs-lookup"><span data-stu-id="2e135-142">Switch back toohello publisher portal and paste hello key into hello **Client Secret** text box.</span></span>

  ![Klíč aplikace 2][api-management-howto-aad-b2c-client-secret]

12. <span data-ttu-id="2e135-144">Zadejte název domény hello klienta hello Azure Active Directory B2C v **povolené klienta**.</span><span class="sxs-lookup"><span data-stu-id="2e135-144">Specify hello domain name of hello Azure Active Directory B2C tenant in **Allowed Tenant**.</span></span>

  ![Povolené klienta][api-management-howto-aad-b2c-allowed-tenant]

13. <span data-ttu-id="2e135-146">Zadejte hello **zásady registrace** a **přihlášení zásady**.</span><span class="sxs-lookup"><span data-stu-id="2e135-146">Specify hello **Signup Policy** and **Signin Policy**.</span></span> <span data-ttu-id="2e135-147">Volitelně můžete zadat taky hello **zásady úpravy profilu** a **zásady resetování hesel**.</span><span class="sxs-lookup"><span data-stu-id="2e135-147">Optionally, you can also provide hello **Profile Editing Policy** and **Password Reset Policy**.</span></span>

  ![Zásady][api-management-howto-aad-b2c-policies]

  > [!NOTE]
  > <span data-ttu-id="2e135-149">Další informace o zásadách najdete v tématu [Azure Active Directory B2C: rozšiřitelný rámec zásad].</span><span class="sxs-lookup"><span data-stu-id="2e135-149">For more information on policies, see [Azure Active Directory B2C: Extensible policy framework].</span></span>

14. <span data-ttu-id="2e135-150">Po zadání hello požadované konfigurace, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2e135-150">After you've specified hello desired configuration, click **Save**.</span></span>

  <span data-ttu-id="2e135-151">Po uložení změn hello vývojáři bude možné toocreate nové účty a přihlaste se pomocí Azure Active Directory B2C toohello portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="2e135-151">After hello changes are saved, developers will be able toocreate new accounts and sign in toohello developer portal by using Azure Active Directory B2C.</span></span>

## <a name="sign-up-for-a-developer-account-by-using-azure-active-directory-b2c"></a><span data-ttu-id="2e135-152">Zaregistrujte si účet pro vývojáře pomocí Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="2e135-152">Sign up for a developer account by using Azure Active Directory B2C</span></span>

1. <span data-ttu-id="2e135-153">toosign službu vývojářského účtu pomocí Azure Active Directory B2C, otevřete nové okno prohlížeče a přejděte toohello portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="2e135-153">toosign up for a developer account by using Azure Active Directory B2C, open a new browser window and go toohello developer portal.</span></span> <span data-ttu-id="2e135-154">Klikněte na tlačítko hello **zaregistrovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2e135-154">Click hello **Sign up** button.</span></span>

   ![Portál pro vývojáře 1][api-management-howto-aad-b2c-dev-portal]

2. <span data-ttu-id="2e135-156">Zvolte toosign nahoru s **Azure Active Directory B2C**.</span><span class="sxs-lookup"><span data-stu-id="2e135-156">Choose toosign up with **Azure Active Directory B2C**.</span></span>

   ![Portál pro vývojáře 2][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. <span data-ttu-id="2e135-158">Jste přesměrovaného toohello zásady registrace, který jste nakonfigurovali v předchozím oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="2e135-158">You're redirected toohello signup policy that you configured in hello previous section.</span></span> <span data-ttu-id="2e135-159">Pomocí e-mailovou adresu nebo jeden z vaší účtů na sociálních zvolte toosign.</span><span class="sxs-lookup"><span data-stu-id="2e135-159">Choose toosign up by using your email address or one of your existing social accounts.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2e135-160">Pokud Azure Active Directory B2C je hello pouze možnost, která je povolena na hello **identity** kartě hello portálu vydavatele, budete mít zásady registrace přesměrovaného toohello přímo.</span><span class="sxs-lookup"><span data-stu-id="2e135-160">If Azure Active Directory B2C is hello only option that's enabled on hello **Identities** tab in hello publisher portal, you'll be redirected toohello signup policy directly.</span></span>

   ![Portál pro vývojáře][api-management-howto-aad-b2c-dev-portal-b2c-options]

   <span data-ttu-id="2e135-162">Po dokončení registrace hello jste přesměrovaného back toohello portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="2e135-162">When hello signup is complete, you're redirected back toohello developer portal.</span></span> <span data-ttu-id="2e135-163">Portál pro vývojáře toohello jste teď přihlášení pro instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="2e135-163">You're now signed in toohello developer portal for your API Management service instance.</span></span>

    ![Registrace je dokončena.][api-management-registration-complete]

## <a name="next-steps"></a><span data-ttu-id="2e135-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e135-165">Next steps</span></span>

*  <span data-ttu-id="2e135-166">[přehled Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="2e135-166">[Azure Active Directory B2C overview]</span></span>
*  <span data-ttu-id="2e135-167">[Azure Active Directory B2C: rozšiřitelný rámec zásad]</span><span class="sxs-lookup"><span data-stu-id="2e135-167">[Azure Active Directory B2C: Extensible policy framework]</span></span>
*  <span data-ttu-id="2e135-168">[Použít účet Microsoft jako poskytovatel identit v Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="2e135-168">[Use a Microsoft account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="2e135-169">[Použít účet Google jako poskytovatel identit v Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="2e135-169">[Use a Google account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="2e135-170">[Použít účet LinkedIn jako poskytovatel identit v Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="2e135-170">[Use a LinkedIn account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="2e135-171">[Použít účet Facebook jako poskytovatel identit v Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="2e135-171">[Use a Facebook account as an identity provider in Azure Active Directory B2C]</span></span>




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

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing hello Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph
[přehled Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview
[jak tooauthorize vývojáře účtů pomocí služby Azure Active Directory]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad
[Azure Active Directory B2C: rozšiřitelný rámec zásad]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies
[Použít účet Microsoft jako poskytovatel identit v Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app
[Použít účet Google jako poskytovatel identit v Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app
[Použít účet Facebook jako poskytovatel identit v Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app
[Použít účet LinkedIn jako poskytovatel identit v Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-li-app

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API toouse OAuth 2.0 user authorization]: #step2
[Test hello OAuth 2.0 user authorization in hello Developer Portal]: #step3
[Next steps]: #next-steps

[Log in toohello Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
