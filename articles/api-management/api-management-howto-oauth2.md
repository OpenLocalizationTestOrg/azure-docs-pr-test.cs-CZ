---
title: "aaaAuthorize vývojářským účtům pomocí OAuth 2.0 ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak uživatelé tooauthorize pomocí OAuth 2.0 ve službě API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 78c48247-64f0-4708-b2d0-98b61a821283
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 934901dd6df399470a3257bf7a3a9b9fb5f40d5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthorize-developer-accounts-using-oauth-20-in-azure-api-management"></a><span data-ttu-id="fc6b4-103">Jak účtů tooauthorize vývojáře pomocí OAuth 2.0 ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="fc6b4-103">How tooauthorize developer accounts using OAuth 2.0 in Azure API Management</span></span>
<span data-ttu-id="fc6b4-104">Mnoho rozhraní API podporují [OAuth 2.0](http://oauth.net/2/) toosecure hello rozhraní API a ujistěte se, že jenom uživatelé mají přístup a získají pouze přístup k prostředkům toowhich jejich máte nárok.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-104">Many APIs support [OAuth 2.0](http://oauth.net/2/) toosecure hello API and ensure that only valid users have access, and they can only access resources toowhich they're entitled.</span></span> <span data-ttu-id="fc6b4-105">V konzole vývojáře interaktivní pořadí toouse Azure API Management na takové rozhraní API služby hello vám umožní tooconfigure vaše toowork instance služby s OAuth 2.0 povoleno rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-105">In order toouse Azure API Management's interactive Developer Console with such APIs, hello service allows you tooconfigure your service instance toowork with your OAuth 2.0 enabled API.</span></span>

## <span data-ttu-id="fc6b4-106"><a name="prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="fc6b4-106"><a name="prerequisites"> </a>Prerequisites</span></span>
<span data-ttu-id="fc6b4-107">Tento průvodce vám ukáže, jak tooconfigure vaše rozhraní API správy služby instance toouse autorizace OAuth 2.0 pro vývojáře účty, ale nezobrazuje jak zprostředkovatele tooconfigure OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-107">This guide shows you how tooconfigure your API Management service instance toouse OAuth 2.0 authorization for developer accounts, but does not show you how tooconfigure an OAuth 2.0 provider.</span></span> <span data-ttu-id="fc6b4-108">hello Hello konfigurace pro každý OAuth 2.0 zprostředkovatele se liší, i když hello kroky jsou podobné a hello požadované údaje použít při konfiguraci OAuth 2.0 ve vaší instance služby API Management jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-108">hello configuration for each OAuth 2.0 provider is different, although hello steps are similar, and hello required pieces of information used in configuring OAuth 2.0 in your API Management service instance are hello same.</span></span> <span data-ttu-id="fc6b4-109">Toto téma ukazuje příklady pomocí služby Azure Active Directory jako zprostředkovatel OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-109">This topic shows examples using Azure Active Directory as an OAuth 2.0 provider.</span></span>

> [!NOTE]
> <span data-ttu-id="fc6b4-110">Další informace o konfiguraci OAuth 2.0 pomocí služby Azure Active Directory najdete v tématu hello [WebApp. GraphAPI DotNet] [ WebApp-GraphAPI-DotNet] ukázka.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-110">For more information on configuring OAuth 2.0 using Azure Active Directory, see hello [WebApp-GraphAPI-DotNet][WebApp-GraphAPI-DotNet] sample.</span></span>
> 
> 

## <span data-ttu-id="fc6b4-111"><a name="step1"></a>Konfigurace serveru autorizace OAuth 2.0 ve službě API Management</span><span class="sxs-lookup"><span data-stu-id="fc6b4-111"><a name="step1"> </a>Configure an OAuth 2.0 authorization server in API Management</span></span>
<span data-ttu-id="fc6b4-112">tooget začít, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-112">tooget started, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Portál vydavatele][api-management-management-console]

> [!NOTE]
> <span data-ttu-id="fc6b4-114">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="fc6b4-115">Klikněte na tlačítko **zabezpečení** z hello **API Management** nabídky na levé straně, klikněte na tlačítko hello **OAuth 2.0**a potom klikněte na **serveru ověřování přidat**.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-115">Click **Security** from hello **API Management** menu on hello left, click **OAuth 2.0**, and then click **Add authorization server**.</span></span>

![OAuth 2.0][api-management-oauth2]

<span data-ttu-id="fc6b4-117">Po kliknutí na **serveru ověřování přidat**, se zobrazí formulář hello nové ověřování serveru.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-117">After clicking **Add authorization server**, hello new authorization server form is displayed.</span></span>

![Nový server][api-management-oauth2-server-1]

<span data-ttu-id="fc6b4-119">Zadejte název a volitelný popis v hello **název** a **popis** pole.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-119">Enter a name and an optional description in hello **Name** and **Description** fields.</span></span> 

> [!NOTE]
> <span data-ttu-id="fc6b4-120">Tato pole jsou použité tooidentify hello OAuth 2.0 autorizace serveru v rámci hello aktuální instanci služby API Management a jejich hodnoty nepocházejí ze serveru hello OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-120">These fields are used tooidentify hello OAuth 2.0 authorization server within hello current API Management service instance and their values do not come from hello OAuth 2.0 server.</span></span>
> 
> 

<span data-ttu-id="fc6b4-121">Zadejte hello **adresa URL stránky registrace klienta**.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-121">Enter hello **Client registration page URL**.</span></span> <span data-ttu-id="fc6b4-122">Tato stránka je, kde uživatelé mohou vytvářet a spravovat své účty a se liší v závislosti na používá zprostředkovatele hello OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-122">This page is where users can create and manage their accounts, and varies depending on hello OAuth 2.0 provider used.</span></span> <span data-ttu-id="fc6b4-123">Hello **adresa URL stránky registrace klienta** body toohello stránka, kterou můžou uživatelé použít toocreate a nakonfigurovat svoje vlastní účty zprostředkovatelů OAuth 2.0, které podporují správu uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-123">hello **Client registration page URL** points toohello page that users can use toocreate and configure their own accounts for OAuth 2.0 providers that support user management of accounts.</span></span> <span data-ttu-id="fc6b4-124">Některé organizace nenakonfigurujete nebo tato funkce se použít i v případě, že ji podporuje zprostředkovatele hello OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-124">Some organizations do not configure or use this functionality even if hello OAuth 2.0 provider supports it.</span></span> <span data-ttu-id="fc6b4-125">Pokud poskytovatel OAuth 2.0 správu uživatelů pro účty konfigurované nemá, zadejte URL zástupný symbol zde například hello URL vaší společnosti, nebo adresu URL, jako `https://placeholder.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-125">If your OAuth 2.0 provider does not have user management of accounts configured, enter a placeholder URL here such as hello URL of your company, or a URL such as `https://placeholder.contoso.com`.</span></span>

<span data-ttu-id="fc6b4-126">Další část Hello hello formulář obsahuje hello **typy udělení autorizačního kódu**, **adresu URL koncového bodu autorizace**, a **metoda autorizace požadavku** nastavení.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-126">hello next section of hello form contains hello **Authorization code grant types**, **Authorization endpoint URL**, and **Authorization request method** settings.</span></span>

![Nový server][api-management-oauth2-server-2]

<span data-ttu-id="fc6b4-128">Zadejte hello **typy udělení autorizačního kódu** kontrolou hello požadovaných typů.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-128">Specify hello **Authorization code grant types** by checking hello desired types.</span></span> <span data-ttu-id="fc6b4-129">**Autorizační kód** je zadána ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-129">**Authorization code** is specified by default.</span></span>

<span data-ttu-id="fc6b4-130">Zadejte hello **adresu URL koncového bodu autorizace**.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-130">Enter hello **Authorization endpoint URL**.</span></span> <span data-ttu-id="fc6b4-131">Pro Azure Active Directory, bude tato adresa URL podobné toohello následující adresu URL, kde `<client_id>` je nahrazena hello id klienta, který identifikuje aplikačního serveru toohello OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-131">For Azure Active Directory, this URL will be similar toohello following URL, where `<client_id>` is replaced with hello client id that identifies your application toohello OAuth 2.0 server.</span></span>

`https://login.microsoftonline.com/<client_id>/oauth2/authorize`

<span data-ttu-id="fc6b4-132">Hello **metoda autorizace požadavku** Určuje, jak je hello autorizace požadavek odeslán server toohello OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-132">hello **Authorization request method** specifies how hello authorization request is sent toohello OAuth 2.0 server.</span></span> <span data-ttu-id="fc6b4-133">Ve výchozím nastavení **získat** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-133">By default **GET** is selected.</span></span>

<span data-ttu-id="fc6b4-134">Další části Hello je, kde hello **adresu URL koncového bodu Token**, **metody ověřování klienta**, **přístupový token odesílání metoda**, a **výchozí obor** nejsou zadány.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-134">hello next section is where hello **Token endpoint URL**, **Client authentication methods**, **Access token sending method**, and **Default scope** are specified.</span></span>

![Nový server][api-management-oauth2-server-3]

<span data-ttu-id="fc6b4-136">Pro server Azure Active Directory OAuth 2.0, hello **adresu URL koncového bodu Token** bude mít hello formátu, kde `<APPID>` má formát hello `yourapp.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-136">For an Azure Active Directory OAuth 2.0 server, hello **Token endpoint URL** will have hello following format, where `<APPID>`  has hello format of `yourapp.onmicrosoft.com`.</span></span>

`https://login.microsoftonline.com/<APPID>/oauth2/token`

<span data-ttu-id="fc6b4-137">Hello výchozí nastavení pro **metody ověřování klienta** je **základní**, a **přístupový token odesílání metoda** je **autorizační hlavičky**.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-137">hello default setting for **Client authentication methods** is **Basic**, and  **Access token sending method** is **Authorization header**.</span></span> <span data-ttu-id="fc6b4-138">Tyto hodnoty jsou nakonfigurované v této části hello formuláře, společně s hello **výchozí obor**.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-138">These values are configured on this section of hello form, along with hello **Default scope**.</span></span>

<span data-ttu-id="fc6b4-139">Hello **pověření klienta** část obsahuje hello **ID klienta** a **tajný klíč klienta**, které se získají během procesu vytváření a konfigurace hello vaší OAuth 2.0 Server.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-139">hello **Client credentials** section contains hello **Client ID** and **Client secret**, which are obtained during hello creation and configuration process of your OAuth 2.0 server.</span></span> <span data-ttu-id="fc6b4-140">Jednou hello **ID klienta** a **tajný klíč klienta** jsou nastaveny, hello **redirect_uri** pro hello **autorizační kód** se vygeneruje.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-140">Once hello **Client ID** and **Client secret** are specified, hello **redirect_uri** for hello **authorization code** is generated.</span></span> <span data-ttu-id="fc6b4-141">Tento identifikátor URI je adresa URL odpovědi hello tooconfigure používané v konfiguraci serveru OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-141">This URI is used tooconfigure hello reply URL in your OAuth 2.0 server configuration.</span></span>

![Nový server][api-management-oauth2-server-4]

<span data-ttu-id="fc6b4-143">Pokud **typy udělení autorizačního kódu** je nastaven příliš**heslo vlastníka prostředku**, hello **oprávnění hesla vlastníka prostředku** části je použité toospecify ty pověření; v opačném případě můžete toto pole ponecháte prázdné.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-143">If **Authorization code grant types** is set too**Resource owner password**, hello **Resource owner password credentials** section is used toospecify those credentials; otherwise you can leave it blank.</span></span>

![Nový server][api-management-oauth2-server-5]

<span data-ttu-id="fc6b4-145">Po dokončení hello formuláře klikněte na tlačítko **Uložit** konfigurace serveru autorizace toosave hello rozhraní API správy OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-145">Once hello form is complete, click **Save** toosave hello API Management OAuth 2.0 authorization server configuration.</span></span> <span data-ttu-id="fc6b4-146">Po konfiguraci serveru hello je uložen, můžete nakonfigurovat rozhraní API toouse tuto konfiguraci, jak je znázorněno v další části hello.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-146">Once hello server configuration is saved, you can configure APIs toouse this configuration, as shown in hello next section.</span></span>

## <span data-ttu-id="fc6b4-147"><a name="step2"></a>Nakonfigurovat rozhraní API toouse autorizace uživatelů OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="fc6b4-147"><a name="step2"> </a>Configure an API toouse OAuth 2.0 user authorization</span></span>
<span data-ttu-id="fc6b4-148">Klikněte na tlačítko **rozhraní API** z hello **API Management** levé nabídce na hello, klikněte na název hello hello potřeby rozhraní API, klikněte na **zabezpečení**a potom zaškrtněte políčko hello pro **OAuth 2.0**.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-148">Click **APIs** from hello **API Management** menu on hello left, click hello name of hello desired API, click **Security**, and then check hello box for **OAuth 2.0**.</span></span>

![Autorizace uživatelů][api-management-user-authorization]

<span data-ttu-id="fc6b4-150">Vyberte hello potřeby **serveru ověřování** hello rozevíracího seznamu a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-150">Select hello desired **Authorization server** from hello drop-down list, and click **Save**.</span></span>

![Autorizace uživatelů][api-management-user-authorization-save]

## <span data-ttu-id="fc6b4-152"><a name="step3"></a>Testování autorizace uživatelů hello OAuth 2.0 v hello portál pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="fc6b4-152"><a name="step3"> </a>Test hello OAuth 2.0 user authorization in hello Developer Portal</span></span>
<span data-ttu-id="fc6b4-153">Jakmile jste nakonfigurovali vašeho serveru ověřování OAuth 2.0 a nakonfigurované vaše rozhraní API toouse tento server, jej můžete otestovat tak, že budete toohello portál pro vývojáře a volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-153">Once you have configured your OAuth 2.0 authorization server and configured your API toouse that server, you can test it by going toohello Developer Portal and calling an API.</span></span>  <span data-ttu-id="fc6b4-154">Klikněte na tlačítko **portál pro vývojáře** v pravé horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-154">Click **Developer portal** in hello top right menu.</span></span>

![Portál pro vývojáře][api-management-developer-portal-menu]

<span data-ttu-id="fc6b4-156">Klikněte na tlačítko **rozhraní API** v horní nabídce hello a vyberte **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-156">Click **APIs** in hello top menu and select **Echo API**.</span></span>

![Echo API][api-management-apis-echo-api]

> [!NOTE]
> <span data-ttu-id="fc6b4-158">Pokud máte pouze jedno rozhraní API nakonfigurovaný nebo viditelné tooyour účet, klikněte na rozhraní API přejdete přímo toohello operací pro toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-158">If you have only one API configured or visible tooyour account, then clicking APIs takes you directly toohello operations for that API.</span></span>
> 
> 

<span data-ttu-id="fc6b4-159">Vyberte hello **GET Resource** operace, klikněte na tlačítko **otevřít konzolu**a potom vyberte **autorizační kód** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-159">Select hello **GET Resource** operation, click **Open Console**, and then select **Authorization code** from hello drop-down.</span></span>

![Otevření konzoly][api-management-open-console]

<span data-ttu-id="fc6b4-161">Když **autorizační kód** je vybrána, automaticky otevírané okno se zobrazí s hello přihlašovací formulář zprostředkovatele hello OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-161">When **Authorization code** is selected, a pop-up window is displayed with hello sign-in form of hello OAuth 2.0 provider.</span></span> <span data-ttu-id="fc6b4-162">V tomto příkladu je hello přihlašovací formulář poskytovaný Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-162">In this example hello sign-in form is provided by Azure Active Directory.</span></span>

> [!NOTE]
> <span data-ttu-id="fc6b4-163">Pokud máte automaticky otevíraná okna zakázán, bude vyzván tooenable je hello prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-163">If you have pop-ups disabled you will be prompted tooenable them by hello browser.</span></span> <span data-ttu-id="fc6b4-164">Když je povolit, vyberte **autorizační kód** znovu a hello zobrazí se přihlašovací formulář.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-164">After you enable them, select **Authorization code** again and hello sign-in form will be displayed.</span></span>
> 
> 

![Přihlášení][api-management-oauth2-signin]

<span data-ttu-id="fc6b4-166">Po přihlášení, hello **hlavičky požadavku** se naplní `Authorization : Bearer` záhlaví, který opravňuje hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-166">Once you have signed in, hello **Request headers** are populated with an `Authorization : Bearer` header that authorizes hello request.</span></span>

![Token hlavičky požadavku][api-management-request-header-token]

<span data-ttu-id="fc6b4-168">V tomto okamžiku můžete konfigurovat hello potřeby hodnoty pro hello zbývající parametry a odeslat žádost o hello.</span><span class="sxs-lookup"><span data-stu-id="fc6b4-168">At this point you can configure hello desired values for hello remaining parameters, and submit hello request.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="fc6b4-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fc6b4-169">Next steps</span></span>
<span data-ttu-id="fc6b4-170">Další informace o používání OAuth 2.0 a API Management najdete v tématu hello následující videa a doprovodné [článku](api-management-howto-protect-backend-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="fc6b4-170">For more information about using OAuth 2.0 and API Management, see hello following video and accompanying [article](api-management-howto-protect-backend-with-aad.md).</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


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

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API toouse OAuth 2.0 user authorization]: #step2
[Test hello OAuth 2.0 user authorization in hello Developer Portal]: #step3
[Next steps]: #next-steps

