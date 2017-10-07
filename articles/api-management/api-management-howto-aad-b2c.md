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
# <a name="how-tooauthorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a>Jak tooauthorize vývojáře účtů pomocí Azure Active Directory B2C ve službě Azure API Management
## <a name="overview"></a>Přehled
Azure Active Directory B2C je cloudové řešení správy identity pro určených webové a mobilní aplikace. Můžete ji použít portál pro vývojáře tooyour toomanage přístup. Tato příručka obsahuje hello konfigurací, která je vyžadována ve vaší toointegrate služby API Management s Azure Active Directory B2C. Informace o povolení portál pro vývojáře toohello přístup pomocí klasického Azure Active Directory najdete v tématu [jak tooauthorize vývojáře účtů pomocí služby Azure Active Directory].

> [!NOTE]
> toocomplete hello kroky v tomto průvodci, musíte nejprve mít toocreate klienta Azure Active Directory B2C aplikace. Navíc musíte zásady registrace a přihlášení toohave připraven. Další informace najdete v tématu [přehled Azure Active Directory B2C].

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a>Autorizovat vývojářským účtům pomocí Azure Active Directory B2C

1. tooget začít, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management. Tím přejdete portál vydavatele toohello API Management.

   ![Portál vydavatele][api-management-management-console]

   > [!NOTE]
   > Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management kurzu][Get started with Azure API Management].

2. Na hello **API Management** nabídky, klikněte na tlačítko **zabezpečení**. Na hello **identity** , zvolte **Azure Active Directory B2C**.

  ![Externí identity 1][api-management-howto-aad-b2c-security-tab]

3. Poznamenejte si hello **adresy URL pro přesměrování** a přepnout tooAzure Active Directory B2C v hello portálu Azure.

  ![Externí identity 2][api-management-howto-aad-b2c-security-tab-reply-url]

4. Klikněte na tlačítko hello **aplikace** tlačítko.

  ![Zaregistrujte novou aplikaci 1][api-management-howto-aad-b2c-portal-menu]

5. Klikněte na tlačítko hello **přidat** tlačítko aplikace toocreate nové Azure Active Directory B2C.

  ![Zaregistrujte novou aplikaci 2][api-management-howto-aad-b2c-add-button]

6. V hello **novou aplikaci** okno, zadejte název aplikace hello. Zvolte **Ano** pod **webové aplikaci nebo webové rozhraní API**a zvolte **Ano** pod **povolit implicitní tok**. Potom kopie hello **adresy URL pro přesměrování** z hello **Azure Active Directory B2C** části hello **identity** v hello portálu vydavatele a vložte jej do hello **Adresa URL odpovědi** textové pole.

  ![Zaregistrujte novou aplikaci 3][api-management-howto-aad-b2c-app-details]

7. Klikněte na tlačítko hello **vytvořit** tlačítko. Když aplikace hello je vytvořen, zobrazí se v hello **aplikace** okno. Klikněte na tlačítko toosee název aplikace hello její podrobnosti.

  ![Zaregistrujte novou aplikaci 4][api-management-howto-aad-b2c-app-created]

8. Z hello **vlastnosti** okno, kopie hello **ID aplikace** toohello schránky.

  ![ID aplikace 1][api-management-howto-aad-b2c-app-id]

9. Přepnout zpět toohello portál vydavatele a hello ID vložte do hello **Id klienta** textové pole.

  ![ID aplikace 2][api-management-howto-aad-b2c-client-id]

10. Přepnout zpět toohello portálu Azure, klikněte na tlačítko hello **klíče** tlačítko a potom klikněte na **vygenerovat klíč**. Klikněte na tlačítko **Uložit** toosave hello konfiguraci a zobrazení hello **klíč aplikace**. Kopírování hello klíče toohello schránky.

  ![Aplikace klíč 1][api-management-howto-aad-b2c-app-key]

11. Přepínač back toohello vydavatele portál a vložit hello klíč do hello **tajný klíč klienta** textové pole.

  ![Klíč aplikace 2][api-management-howto-aad-b2c-client-secret]

12. Zadejte název domény hello klienta hello Azure Active Directory B2C v **povolené klienta**.

  ![Povolené klienta][api-management-howto-aad-b2c-allowed-tenant]

13. Zadejte hello **zásady registrace** a **přihlášení zásady**. Volitelně můžete zadat taky hello **zásady úpravy profilu** a **zásady resetování hesel**.

  ![Zásady][api-management-howto-aad-b2c-policies]

  > [!NOTE]
  > Další informace o zásadách najdete v tématu [Azure Active Directory B2C: rozšiřitelný rámec zásad].

14. Po zadání hello požadované konfigurace, klikněte na tlačítko **Uložit**.

  Po uložení změn hello vývojáři bude možné toocreate nové účty a přihlaste se pomocí Azure Active Directory B2C toohello portál pro vývojáře.

## <a name="sign-up-for-a-developer-account-by-using-azure-active-directory-b2c"></a>Zaregistrujte si účet pro vývojáře pomocí Azure Active Directory B2C

1. toosign službu vývojářského účtu pomocí Azure Active Directory B2C, otevřete nové okno prohlížeče a přejděte toohello portál pro vývojáře. Klikněte na tlačítko hello **zaregistrovat** tlačítko.

   ![Portál pro vývojáře 1][api-management-howto-aad-b2c-dev-portal]

2. Zvolte toosign nahoru s **Azure Active Directory B2C**.

   ![Portál pro vývojáře 2][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. Jste přesměrovaného toohello zásady registrace, který jste nakonfigurovali v předchozím oddílu hello. Pomocí e-mailovou adresu nebo jeden z vaší účtů na sociálních zvolte toosign.

   > [!NOTE]
   > Pokud Azure Active Directory B2C je hello pouze možnost, která je povolena na hello **identity** kartě hello portálu vydavatele, budete mít zásady registrace přesměrovaného toohello přímo.

   ![Portál pro vývojáře][api-management-howto-aad-b2c-dev-portal-b2c-options]

   Po dokončení registrace hello jste přesměrovaného back toohello portál pro vývojáře. Portál pro vývojáře toohello jste teď přihlášení pro instanci služby API Management.

    ![Registrace je dokončena.][api-management-registration-complete]

## <a name="next-steps"></a>Další kroky

*  [přehled Azure Active Directory B2C]
*  [Azure Active Directory B2C: rozšiřitelný rámec zásad]
*  [Použít účet Microsoft jako poskytovatel identit v Azure Active Directory B2C]
*  [Použít účet Google jako poskytovatel identit v Azure Active Directory B2C]
*  [Použít účet LinkedIn jako poskytovatel identit v Azure Active Directory B2C]
*  [Použít účet Facebook jako poskytovatel identit v Azure Active Directory B2C]




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
