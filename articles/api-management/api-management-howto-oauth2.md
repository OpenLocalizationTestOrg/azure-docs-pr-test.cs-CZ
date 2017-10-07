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
# <a name="how-tooauthorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Jak účtů tooauthorize vývojáře pomocí OAuth 2.0 ve službě Azure API Management
Mnoho rozhraní API podporují [OAuth 2.0](http://oauth.net/2/) toosecure hello rozhraní API a ujistěte se, že jenom uživatelé mají přístup a získají pouze přístup k prostředkům toowhich jejich máte nárok. V konzole vývojáře interaktivní pořadí toouse Azure API Management na takové rozhraní API služby hello vám umožní tooconfigure vaše toowork instance služby s OAuth 2.0 povoleno rozhraní API.

## <a name="prerequisites"></a>Požadavky
Tento průvodce vám ukáže, jak tooconfigure vaše rozhraní API správy služby instance toouse autorizace OAuth 2.0 pro vývojáře účty, ale nezobrazuje jak zprostředkovatele tooconfigure OAuth 2.0. hello Hello konfigurace pro každý OAuth 2.0 zprostředkovatele se liší, i když hello kroky jsou podobné a hello požadované údaje použít při konfiguraci OAuth 2.0 ve vaší instance služby API Management jsou stejné. Toto téma ukazuje příklady pomocí služby Azure Active Directory jako zprostředkovatel OAuth 2.0.

> [!NOTE]
> Další informace o konfiguraci OAuth 2.0 pomocí služby Azure Active Directory najdete v tématu hello [WebApp. GraphAPI DotNet] [ WebApp-GraphAPI-DotNet] ukázka.
> 
> 

## <a name="step1"></a>Konfigurace serveru autorizace OAuth 2.0 ve službě API Management
tooget začít, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management.

![Portál vydavatele][api-management-management-console]

> [!NOTE]
> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.
> 
> 

Klikněte na tlačítko **zabezpečení** z hello **API Management** nabídky na levé straně, klikněte na tlačítko hello **OAuth 2.0**a potom klikněte na **serveru ověřování přidat**.

![OAuth 2.0][api-management-oauth2]

Po kliknutí na **serveru ověřování přidat**, se zobrazí formulář hello nové ověřování serveru.

![Nový server][api-management-oauth2-server-1]

Zadejte název a volitelný popis v hello **název** a **popis** pole. 

> [!NOTE]
> Tato pole jsou použité tooidentify hello OAuth 2.0 autorizace serveru v rámci hello aktuální instanci služby API Management a jejich hodnoty nepocházejí ze serveru hello OAuth 2.0.
> 
> 

Zadejte hello **adresa URL stránky registrace klienta**. Tato stránka je, kde uživatelé mohou vytvářet a spravovat své účty a se liší v závislosti na používá zprostředkovatele hello OAuth 2.0. Hello **adresa URL stránky registrace klienta** body toohello stránka, kterou můžou uživatelé použít toocreate a nakonfigurovat svoje vlastní účty zprostředkovatelů OAuth 2.0, které podporují správu uživatelské účty. Některé organizace nenakonfigurujete nebo tato funkce se použít i v případě, že ji podporuje zprostředkovatele hello OAuth 2.0. Pokud poskytovatel OAuth 2.0 správu uživatelů pro účty konfigurované nemá, zadejte URL zástupný symbol zde například hello URL vaší společnosti, nebo adresu URL, jako `https://placeholder.contoso.com`.

Další část Hello hello formulář obsahuje hello **typy udělení autorizačního kódu**, **adresu URL koncového bodu autorizace**, a **metoda autorizace požadavku** nastavení.

![Nový server][api-management-oauth2-server-2]

Zadejte hello **typy udělení autorizačního kódu** kontrolou hello požadovaných typů. **Autorizační kód** je zadána ve výchozím nastavení.

Zadejte hello **adresu URL koncového bodu autorizace**. Pro Azure Active Directory, bude tato adresa URL podobné toohello následující adresu URL, kde `<client_id>` je nahrazena hello id klienta, který identifikuje aplikačního serveru toohello OAuth 2.0.

`https://login.microsoftonline.com/<client_id>/oauth2/authorize`

Hello **metoda autorizace požadavku** Určuje, jak je hello autorizace požadavek odeslán server toohello OAuth 2.0. Ve výchozím nastavení **získat** je vybrána.

Další části Hello je, kde hello **adresu URL koncového bodu Token**, **metody ověřování klienta**, **přístupový token odesílání metoda**, a **výchozí obor** nejsou zadány.

![Nový server][api-management-oauth2-server-3]

Pro server Azure Active Directory OAuth 2.0, hello **adresu URL koncového bodu Token** bude mít hello formátu, kde `<APPID>` má formát hello `yourapp.onmicrosoft.com`.

`https://login.microsoftonline.com/<APPID>/oauth2/token`

Hello výchozí nastavení pro **metody ověřování klienta** je **základní**, a **přístupový token odesílání metoda** je **autorizační hlavičky**. Tyto hodnoty jsou nakonfigurované v této části hello formuláře, společně s hello **výchozí obor**.

Hello **pověření klienta** část obsahuje hello **ID klienta** a **tajný klíč klienta**, které se získají během procesu vytváření a konfigurace hello vaší OAuth 2.0 Server. Jednou hello **ID klienta** a **tajný klíč klienta** jsou nastaveny, hello **redirect_uri** pro hello **autorizační kód** se vygeneruje. Tento identifikátor URI je adresa URL odpovědi hello tooconfigure používané v konfiguraci serveru OAuth 2.0.

![Nový server][api-management-oauth2-server-4]

Pokud **typy udělení autorizačního kódu** je nastaven příliš**heslo vlastníka prostředku**, hello **oprávnění hesla vlastníka prostředku** části je použité toospecify ty pověření; v opačném případě můžete toto pole ponecháte prázdné.

![Nový server][api-management-oauth2-server-5]

Po dokončení hello formuláře klikněte na tlačítko **Uložit** konfigurace serveru autorizace toosave hello rozhraní API správy OAuth 2.0. Po konfiguraci serveru hello je uložen, můžete nakonfigurovat rozhraní API toouse tuto konfiguraci, jak je znázorněno v další části hello.

## <a name="step2"></a>Nakonfigurovat rozhraní API toouse autorizace uživatelů OAuth 2.0
Klikněte na tlačítko **rozhraní API** z hello **API Management** levé nabídce na hello, klikněte na název hello hello potřeby rozhraní API, klikněte na **zabezpečení**a potom zaškrtněte políčko hello pro **OAuth 2.0**.

![Autorizace uživatelů][api-management-user-authorization]

Vyberte hello potřeby **serveru ověřování** hello rozevíracího seznamu a klikněte na **Uložit**.

![Autorizace uživatelů][api-management-user-authorization-save]

## <a name="step3"></a>Testování autorizace uživatelů hello OAuth 2.0 v hello portál pro vývojáře
Jakmile jste nakonfigurovali vašeho serveru ověřování OAuth 2.0 a nakonfigurované vaše rozhraní API toouse tento server, jej můžete otestovat tak, že budete toohello portál pro vývojáře a volání rozhraní API.  Klikněte na tlačítko **portál pro vývojáře** v pravé horní nabídce hello.

![Portál pro vývojáře][api-management-developer-portal-menu]

Klikněte na tlačítko **rozhraní API** v horní nabídce hello a vyberte **Echo API**.

![Echo API][api-management-apis-echo-api]

> [!NOTE]
> Pokud máte pouze jedno rozhraní API nakonfigurovaný nebo viditelné tooyour účet, klikněte na rozhraní API přejdete přímo toohello operací pro toto rozhraní API.
> 
> 

Vyberte hello **GET Resource** operace, klikněte na tlačítko **otevřít konzolu**a potom vyberte **autorizační kód** z rozevíracího seznamu hello.

![Otevření konzoly][api-management-open-console]

Když **autorizační kód** je vybrána, automaticky otevírané okno se zobrazí s hello přihlašovací formulář zprostředkovatele hello OAuth 2.0. V tomto příkladu je hello přihlašovací formulář poskytovaný Azure Active Directory.

> [!NOTE]
> Pokud máte automaticky otevíraná okna zakázán, bude vyzván tooenable je hello prohlížeč. Když je povolit, vyberte **autorizační kód** znovu a hello zobrazí se přihlašovací formulář.
> 
> 

![Přihlášení][api-management-oauth2-signin]

Po přihlášení, hello **hlavičky požadavku** se naplní `Authorization : Bearer` záhlaví, který opravňuje hello požadavku.

![Token hlavičky požadavku][api-management-request-header-token]

V tomto okamžiku můžete konfigurovat hello potřeby hodnoty pro hello zbývající parametry a odeslat žádost o hello. 

## <a name="next-steps"></a>Další kroky
Další informace o používání OAuth 2.0 a API Management najdete v tématu hello následující videa a doprovodné [článku](api-management-howto-protect-backend-with-aad.md).

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

