---
title: "aaaAzure služby Active Directory ověřování a Resource Manager | Microsoft Docs"
description: "Pro vývojáře průvodce tooauthentication s hello rozhraní API Správce prostředků Azure a Azure Active Directory pro integraci aplikace s jiných předplatných Azure."
services: azure-resource-manager,active-directory
documentationcenter: na
author: dushyantgill
manager: timlt
editor: tysonn
ms.assetid: 17b2b40d-bf42-4c7d-9a88-9938409c5088
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/27/2016
ms.author: dugill;tomfitz
ms.openlocfilehash: 757e45fdb28488b45de70647746461888bf35a56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-resource-manager-authentication-api-tooaccess-subscriptions"></a>Pomocí Správce prostředků ověřování rozhraní API tooaccess odběrů
## <a name="introduction"></a>Úvod
Pokud jste vývojář softwaru, který potřebuje toocreate aplikaci, která spravuje prostředky Azure zákazníka, v tomto tématu se dozvíte, jak tooauthenticate s hello rozhraní API Správce Azure Resource Manager a získání přístupu tooresources v jiných předplatných.

Aplikace se můžete dostat hello rozhraní API Resource Manager v několika způsoby:

1. **Uživatel + přístup k aplikaci**: pro aplikace, která přistupují k prostředkům jménem přihlášeného uživatele. Tento postup funguje pro aplikace, jako třeba webové aplikace a nástroje příkazového řádku, které pracují s pouze "interaktivní Správa" prostředků Azure.
2. **Přístup jen aplikace**: pro aplikace, které běží démon služeb a naplánované úlohy. identity aplikace Hello se poskytuje přímý přístup k prostředkům toohello. Tento postup funguje pro aplikace, které potřebují dlouhodobé tooAzure bezobslužných (bezobslužná instalace) přístup.

Toto téma obsahuje podrobné pokyny toocreate aplikaci, která aktivuje obě tyto metody ověřování. Ukazuje, jak tooperform každý krok REST API nebo C#. Kompletní aplikace ASP.NET MVC Hello je k dispozici na [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

## <a name="what-hello-web-app-does"></a>Nemá jaké hello webové aplikace
Hello webové aplikace:

1. Přihlásí uživatele služby Azure.
2. Požádá uživatele toogrant hello webové aplikace přístup tooResource správce.
3. Získá uživatele + přístupový token aplikace pro přístup k Resource Manager.
4. Používá tokenu (z kroku 3) toocall Resource Manager a aplikace hello přiřazení role hlavní tooa služby v rámci předplatného hello, která umožňuje hello aplikace dlouhodobé přístup toohello předplatné.
5. Získá jen aplikace přístupový token.
6. Používá tokenu (z kroku 5) toomanage prostředků v předplatném hello prostřednictvím Resource Manager.

Zde je hello začátku do konce toku hello webové aplikace.

![Tok ověření správce prostředků](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Jako uživatel, zadejte id předplatného hello pro předplatné hello chcete toouse:

![Zadejte id předplatného](./media/resource-manager-api-authentication/sample-ux-1.png)

Vyberte hello toouse účet pro přihlášení.

![Vyberte účet](./media/resource-manager-api-authentication/sample-ux-2.png)

Zadejte svoje přihlašovací údaje.

![Zadejte přihlašovací údaje](./media/resource-manager-api-authentication/sample-ux-3.png)

Udělte tooyour přístupu aplikace hello předplatná Azure:

![Udělení přístupu](./media/resource-manager-api-authentication/sample-ux-4.png)

Správa odběrů připojené:

![Připojení odběru](./media/resource-manager-api-authentication/sample-ux-7.png)

## <a name="register-application"></a>Registrace aplikace
Než začnete, kódování, zaregistrujte vaší webové aplikace s Azure Active Directory (AD). registrace aplikace Hello vytvoří centrální identitu pro vaši aplikaci ve službě Azure AD. Že obsahuje základní informace o vaší aplikaci jako ID klienta OAuth, adresy URL odpovědí a pověření, kterou vaše aplikace používá tooauthenticate a přístup k rozhraní API Správce Azure Resource Manager. registrace aplikace Hello taky zaznamenává hello různé delegovaná oprávnění, které aplikace potřebuje při přístupu k Microsoft APIs jménem uživatele hello.

Vzhledem k tomu, že vaše aplikace přístup k jiné předplatné, musíte ho nakonfigurovat jako víceklientské aplikace. ověření toopass, poskytovat domény přidružené k vaší služby Azure Active Directory. toosee hello domény přidružené k Azure Active Directory, přihlášení toohello [portálu classic](https://manage.windowsazure.com). Vyberte služby Azure Active Directory a pak vyberte **domény**.

Hello následující příklad ukazuje, jak tooregister hello aplikace pomocí Azure PowerShell. Musíte mít hello nejnovější verzi (srpna 2016) prostředí Azure PowerShell pro tento příkaz toowork.

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true

toolog v jako hello AD aplikace, musíte hello aplikace id a heslo. id aplikace hello toosee vrácená hello předchozí příkaz, použijte:

    $app.ApplicationId

Hello následující příklad ukazuje, jak tooregister hello aplikace pomocí rozhraní příkazového řádku Azure.

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

Hello výsledky obsahovat hello AppId, který je třeba při ověřování jako aplikace hello.

### <a name="optional-configuration---certificate-credential"></a>Volitelné konfigurace – certifikát přihlašovacích údajů
Azure AD podporuje přihlašovací údaje certifikátu pro aplikace: vytvoření certifikátu podepsaného svým držitelem, zachovat hello privátní klíč a přidejte registrace aplikace hello veřejného klíče tooyour Azure AD. Pro ověřování vaše aplikace odešle malé datové části tooAzure AD podepsaná pomocí soukromého klíče a Azure AD ověří podpis hello pomocí hello veřejného klíče, který je zaregistrovaný.

Informace o vytvoření aplikace AD pomocí certifikátu najdete v tématu [toocreate použití Azure PowerShell objekt služby prostředků tooaccess](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority) nebo [použití Azure CLI toocreate objekt služby tooaccess prostředky](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate).

## <a name="get-tenant-id-from-subscription-id"></a>Získání id klienta z id předplatného
toorequest token, který lze použít toocall Resource Manager, aplikace musí ID klienta hello tooknow klienta hello Azure AD, který je hostitelem hello předplatného Azure. S největší pravděpodobností vaši uživatelé věděli, jejich ID předplatného, ale nemusí věděli, jejich klienta ID pro Azure Active Directory. tooget hello id uživatele klienta, požádejte uživatele hello pro id předplatného hello. Při odesílání žádosti o předplatném hello, zadejte toto id předplatného:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

Hello požadavek selže, protože hello uživatel nebyl ještě přihlášen, ale můžete načíst id klienta hello z hello odpovědi. V této výjimky načíst id klienta hello z hello hodnotu hlavičky odpovědi pro **WWW-Authenticate**. Zobrazí tuto implementaci v hello [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) metoda.

## <a name="get-user--app-access-token"></a>Získat uživatele + přístupový token aplikace
Vaše aplikace přesměruje tooAzure hello uživatele AD s OAuth 2.0 autorizace požadavku - přihlašovacích údajů tooauthenticate hello uživatele a získat zpátky autorizační kód. Vaše aplikace používá hello autorizační kód tooget přístupový token pro Resource Manager. Hello [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) metoda vytvoří žádost o ověření hello.

Toto téma ukazuje hello REST API požadavky tooauthenticate hello uživatele. Můžete také pomocné knihovny tooperform ověřování ve vašem kódu. Další informace o tyto knihovny najdete v tématu [knihovny Azure Active Directory Authentication](../active-directory/active-directory-authentication-libraries.md). Pokyny k integraci správy identit v aplikaci najdete v tématu [Příručka pro vývojáře Azure Active Directory](../active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Žádost o ověření (OAuth 2.0)
Vydejte koncový Open ID Connect/OAuth2.0 autorizace požadavku Authorize toohello Azure AD:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

Hello parametrů řetězce dotazu, které jsou k dispozici pro tento požadavek, jsou popsané v hello [autorizační kód požadavku](../active-directory/develop/active-directory-protocols-oauth-code.md#request-an-authorization-code) tématu.

Následující příklad ukazuje, jak Hello toorequest OAuth2.0 autorizace:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD ověřuje uživatele hello a v případě potřeby požádá hello uživatele toogrant oprávnění toohello aplikace. Vrátí hello autorizační kód toohello adresa URL odpovědi vaší aplikace. V závislosti na hello požadovaný response_mode, Azure AD buď odesílají zpět hello data v řetězci dotazu nebo jako post data.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Žádost o ověření (Open ID Connect)
Pokud nejen chcete tooaccess Azure Resource Manager jménem uživatele hello, ale také povolit hello uživatele toosign v tooyour aplikaci pomocí svého účtu Azure AD, vydejte Open ID připojení autorizaci požadavků. Aplikace s Open ID Connect také přijímá požadavku id_token z Azure AD, vaše aplikace můžete použít toosign v hello uživatele.

Hello parametrů řetězce dotazu, které jsou k dispozici pro tento požadavek, jsou popsané v hello [hello přihlašovací požadavek na odeslání](../active-directory/develop/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) tématu.

Jedná se o příklad Open ID Connect žádost:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD ověřuje uživatele hello a v případě potřeby požádá hello uživatele toogrant oprávnění toohello aplikace. Vrátí hello autorizační kód toohello adresa URL odpovědi vaší aplikace. V závislosti na hello požadovaný response_mode, Azure AD buď odesílají zpět hello data v řetězci dotazu nebo jako post data.

Příklad Open ID Connect odpověď je:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Žádosti o token (OAuth2.0 kód Grant toku)
Teď, když vaše aplikace přijal hello autorizační kód z Azure AD, je čas tooget hello přístupový token pro Azure Resource Manager.  POST OAuth2.0 kód Grant tokenu žádosti o toohello Azure AD tokenu koncový bod:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Hello parametrů řetězce dotazu, které jsou k dispozici pro tento požadavek, jsou popsané v hello [použít hello autorizační kód](../active-directory/develop/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) tématu.

Hello následující příklad ukazuje žádost o token grant kód s pověřením heslo:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Při práci s přihlašovací údaje certifikátu, vytvořte Token pro webové JSON (JWT) a přihlašovací (RSA SHA256) pomocí hello privátní klíč certifikátu pověření vaší aplikace. Hello typy deklarací identity pro hello token se zobrazují v [deklarace tokenů JWT](../active-directory/develop/active-directory-protocols-oauth-code.md#jwt-token-claims). Odkaz, najdete v části hello [knihovna ověřování Active Directory (.NET) kód](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) toosign klienta Assertion Kompaktních tokenů jwt.

V tématu hello [Open ID Connect specifikace](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) podrobnosti o ověření klienta.

Hello následující příklad ukazuje žádost o token grant kód s certifikát přihlašovacích údajů:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Odpověď příklad pro kód udělit token:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Zpracování odpovědi tokenu grant kódu
Úspěšné odpovědi tokenu obsahuje přístupový token hello (uživatele + aplikace) pro Azure Resource Manager. Vaše aplikace používá tento přístup tokenu tooaccess Resource Manager jménem uživatele hello. Doba platnosti Hello přístupové tokeny vydané službou Azure AD je jedna hodina. Není pravděpodobné, že vaše webová aplikace musí toorenew hello (uživatele + aplikace) přístupový token. Pokud je toorenew hello přístupový token, použijte hello token obnovení, které aplikace přijímá v odpovědi tokenu hello. Vystavení tokenu požadavku OAuth2.0 toohello Azure AD tokenu koncový bod:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Hello toouse parametry s požadavek refresh hello jsou popsány v [aktualizace hello přístupový token](../active-directory/develop/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens).

Hello následující příklad ukazuje, jak toouse hello aktualizovat token:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

I když tokeny obnovení může být použité tooget nové přístupové tokeny pro Azure Resource Manager, nejsou vhodné pro offline přístup k vaší aplikací. Doba platnosti tokenů aktualizace Hello je omezený a obnovovacích tokenů jsou vázané toohello uživatele. Pokud uživatel hello opustí organizaci hello, ztrátě hello aplikace pomocí hello obnovovací token přístupu. Tento přístup není vhodný pro aplikace, které jsou používané týmy toomanage svých prostředků Azure.

## <a name="check-if-user-can-assign-access-toosubscription"></a>Zkontrolujte, pokud uživatele lze přiřadit toosubscription přístup
Aplikace nyní obsahuje token tooaccess Azure Resource Manager jménem uživatele hello. dalším krokem Hello je tooconnect předplatné toohello aplikace. Po připojení, vaše aplikace může spravovat tyto odběry i v případě, že hello uživatel není přítomen (dlouhodobé offline přístup).

Pro každé předplatné tooconnect volání hello [seznamu oprávnění správce prostředků](https://docs.microsoft.com/rest/api/authorization/permissions) rozhraní API toodetermine jestli hello uživatel nemá přístupová práva správy pro předplatné hello.

Hello [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) metoda hello ukázkovou aplikaci ASP.NET MVC implementuje toto volání.

Následující příklad ukazuje, jak Hello toorequest oprávnění uživatele na předplatném. 83cfe939-2402-4581-b761-4f59b0a041e4 je hello id předplatného hello.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Příkladem hello odpovědi tooget oprávnění uživatele na základě předplatného je:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

oprávnění Hello rozhraní API vrátí více oprávnění. Každé oprávnění se skládá z povolených akcí (akce) a zakázané akce (notactions). Pokud je součástí hello povolené akce seznam všech oprávnění a není přítomný v seznamu notactions hello tohoto oprávnění, hello uživatele je povoleno tooperform této akce. **Microsoft.Authorization/RoleAssignments/Write** je hello akce, který uděluje přístup práva pro správu. Aplikace musí analyzovat hello oprávnění výsledek toolook regex shodu se tento řetězec akce v hello akce a notactions jednotlivých oprávnění.

## <a name="get-app-only-access-token"></a>Získání tokenu přístupu jen aplikace
Teď víte-li hello uživatele můžete přiřadit přístup toohello předplatného Azure. Další kroky Hello jsou:

1. Přiřaďte hello odpovídající RBAC role tooyour identitu aplikace v předplatném hello.
2. Ověření přiřazení přístupu hello pomocí dotazů na oprávnění hello aplikace v předplatném hello nebo přímým přístupem Resource Manager pomocí tokenu jen aplikace.
3. Záznamů hello připojení ve struktuře vaší aplikace "připojené odběry" data - uložením hello id předplatného hello.

Podíváme blíže v prvním kroku hello. tooassign hello odpovídající RBAC role toohello identitu aplikace, je třeba určit:

* id objektu Hello identity aplikace v hello uživatele Azure Active Directory
* identifikátor Hello role RBAC hello, který vaše aplikace vyžaduje v předplatném hello

Když se aplikace ověřuje uživatele z Azure AD, vytvoří objekt služby zabezpečení pro vaši aplikaci v této službě Azure AD. Azure umožňuje toobe role RBAC přiřazené objekty zabezpečení tooservice toogrant přímý přístup toocorresponding aplikací na prostředky Azure. Tato akce je přesně co jsme chcete toodo. Dotaz hello Azure AD Graph API toodetermine hello identifikátor objektu služby hello vaší aplikace v hello přihlášeného uživatele je Azure AD.

Můžete mít pouze přístupový token pro Azure Resource Manager – budete potřebovat nové hello tokenu toocall přístup k Azure AD Graph API. Každá aplikace ve službě Azure AD má oprávnění tooquery vlastní objekt zabezpečení služby, stačí jen aplikace přístupový token.

<a id="app-azure-ad-graph" />

### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Získání tokenu přístupu jen pro aplikace pro Azure AD Graph API
tooauthenticate vaší aplikace a získání tokenu tooAzure AD Graph API, vydávání OAuth2.0 udělení pověření klienta tok požadavku na token tooAzure AD koncový bod tokenu (**https://login.microsoftonline.com/ {directory_domain_name} / OAuth2/Token**).

Hello [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) metoda hello ukázkovou aplikaci ASP.net MVC získá token pro používání rozhraní Graph API přístup jen aplikace hello Active Directory Authentication Library pro .NET.

Hello parametrů řetězce dotazu, které jsou k dispozici pro tento požadavek, jsou popsané v hello [žádosti Token přístupu](../active-directory/develop/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) tématu.

Požadavek příklad token udělení pověření klienta:

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Odpověď příklad token udělení pověření klienta:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Získejte ObjectId aplikace instanční objekt v uživatele Azure AD
Nyní pomocí hello tokenu tooquery přístup jen aplikace hello [objekty služby Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) hello toodetermine rozhraní API Id objektu aplikace hello instančního objektu v adresáři hello.

Hello [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) metoda hello ukázkovou aplikaci ASP.net MVC implementuje toto volání.

Následující příklad ukazuje, jak Hello toorequest aplikace instanční objekt. a0448380-c346-4f9f-b897-c18733de9394 je hello id klienta aplikace hello.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Hello následující příklad ukazuje žádost toohello odpovědi pro službu aplikace hlavní

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow hello application tooaccess CloudSense on behalf of hello signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow hello application tooaccess CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Získat identifikátor role Azure RBAC
tooassign hello odpovídající RBAC role tooyour instančního objektu, musíte určit hello identifikátor role Azure RBAC hello.

Dobrý den správné role RBAC pro aplikaci:

* Pokud vaše aplikace sleduje pouze hello předplatného, beze změn, vyžaduje pouze oprávnění čtečky u předplatného hello. Přiřadit hello **čtečky** role.
* Pokud vaše aplikace spravuje předplatného Azure hello, vytváření, úpravy nebo odstranění entity, vyžaduje jednu z oprávnění přispěvatele hello.
  * toomanage konkrétní typ prostředku, přiřazení role Přispěvatel konkrétní prostředky hello (Přispěvatel virtuálních počítačů, Přispěvatel sítě, Přispěvatel účet úložiště, atd.)
  * toomanage libovolný typ prostředku, přiřaďte hello **Přispěvatel** role.

Hello přiřazení role pro vaši aplikaci je viditelné toousers, takže vyberte hello nejmenší vyžaduje oprávnění.

Volání hello [definice role správce prostředků rozhraní API](https://docs.microsoft.com/rest/api/authorization/roledefinitions) toolist všechny role Azure RBAC a hledání pak iterace hello výsledek toofind hello potřeby definice role podle názvu.

Hello [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) metoda hello ukázkovou aplikaci ASP.net MVC implementuje toto volání.

Následující Hello požadavku příklad ukazuje, jak tooget identifikátor role Azure RBAC. 09cbd307-aa71-4aca-b346-5f253e6e3ebb je hello id předplatného hello.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

odpověď Hello je ve formátu hello:

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access toothem.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access toothem.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Není nutné toocall toto rozhraní API průběžně. Jakmile jste zjistili, dobře známé GUID definice role hello Dobrý den, můžete vytvořit id definice role hello jako:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Toto jsou známé identifikátory GUID hello běžně používané předdefinovaných rolí:

| Role | Identifikátor GUID |
| --- | --- |
| Čtenář |acdd72a7-3385-48EF-bd42-f606fba81ae7 |
| Přispěvatel |b24988ac-6180-42A0-ab88-20f7382dd24c |
| Přispěvatel virtuálních počítačů |d73bb868-a0df-4d4d-bd69-98a00b01fccb |
| Přispěvatel virtuální sítě |b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
| Přispěvatel účtu úložiště |86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
| Přispěvatel webu |de139f84-1756-47ae-9be6-808fbbe84772 |
| Plán přispěvatelů webu |2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
| Přispěvatel serveru SQL |6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
| Přispěvatel databází SQL |9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |

### <a name="assign-rbac-role-tooapplication"></a>Přiřadit RBAC role tooapplication
Máte všechno, co potřebujete tooassign hello odpovídající RBAC role tooyour instanční objekt pomocí hello [Resource Manager vytvořit přiřazení role](https://docs.microsoft.com/rest/api/authorization/roleassignments) rozhraní API.

Hello [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) metoda hello ukázkovou aplikaci ASP.net MVC implementuje toto volání.

Na příkladu požadavek tooassign RBAC role tooapplication:

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

V žádosti o hello se používají hello následující hodnoty:

| Identifikátor GUID | Popis |
| --- | --- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb |Hello id předplatného hello |
| c3097b31-7309-4C59-b4e3-770f8406bad2 |id objektu Hello hello instanční objekt aplikace hello |
| acdd72a7-3385-48EF-bd42-f606fba81ae7 |Hello id role Čtenář hello |
| 4f87261d-2816-465D-8311-70a27558df4c |nový identifikátor guid pro nové přiřazení role hello vytvořit |

odpověď Hello je ve formátu hello:

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Získání jen aplikace tokenu přístupu pro Azure Resource Manager
toovalidate aplikaci má hello požadovaného přístup na základě předplatného hello, provést úlohu test na hello předplatného pomocí tokenu jen aplikace.

tooget jen aplikace přístupový token, postupujte podle pokynů z oddílu [získání tokenu přístupu jen pro aplikace pro Azure AD Graph API](#app-azure-ad-graph), s jinou hodnotu pro parametr prostředku hello:

    https://management.core.windows.net/

Hello [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) metoda hello ukázkovou aplikaci ASP.NET MVC získá token pro používání Azure Resource Manager přístup jen aplikace hello Active Directory Authentication Library pro .net.

#### <a name="get-applications-permissions-on-subscription"></a>Získat oprávnění aplikace v předplatném
toocheck, který má aplikace hello požadovaného přístupu na základě předplatného služby Azure, můžete také zavolat hello [oprávnění správce prostředků](https://docs.microsoft.com/rest/api/authorization/permissions) rozhraní API. Tento přístup je podobné toohow můžete určit, zda má uživatel hello práva pro správu přístupu pro předplatné hello. Tentokrát však volejte hello oprávnění rozhraní API pomocí hello jen aplikace přístupový token, který jste obdrželi v předchozím kroku hello.

Hello [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) metoda hello ukázkovou aplikaci ASP.NET MVC implementuje toto volání.

## <a name="manage-connected-subscriptions"></a>Správa připojené odběrů
Jakmile příslušné role RBAC hello je přiřazena instanční objekt tooyour aplikace v předplatném hello, vaše aplikace můžete zachovat monitorování nebo správa pomocí jen aplikace přístupových tokenů pro Azure Resource Manager.

Pokud vlastník předplatného Odebere přiřazení role vaší aplikace pomocí portálu classic hello nebo nástroje příkazového řádku, vaše aplikace je již nebude možné tooaccess toto předplatné. V takovém případě by měl upozornit hello uživatele, aby bylo porušeno hello připojení s předplatným hello z mimo aplikace hello a udělit jim možnost příliš "Opravit" hello připojení. "Opravit" by jednoduše znovu vytvořit přiřazení role hello, který byl odstraněn do offline režimu.

Stejně jako jste povolili hello uživatelská tooconnect odběry tooyour aplikace, musíte povolit hello uživatele toodisconnect odběry příliš. Z přístup správu hlediska odpojte znamená odebrání přiřazení role hello, který má aplikace hello instanční objekt na předplatné hello. Volitelně může být jakýkoli stav v hello aplikaci pro předplatné hello odstraněna příliš.
Pouze uživatelé s oprávněním správy přístupu na základě předplatného hello jsou možné toodisconnect hello předplatného.

Hello [RevokeRoleFromServicePrincipalOnSubscription metoda](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) z hello ASP.net MVC implementuje ukázkovou aplikaci toto volání.

Je to – uživatelé teď můžete snadno připojit a spravovat svá předplatná Azure s vaší aplikací.
