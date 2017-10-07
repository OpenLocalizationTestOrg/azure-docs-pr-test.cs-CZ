---
title: "aaaDeploy, volání a ověřování webových rozhraní API a rozhraní REST API pro Azure Logic Apps | Microsoft Docs"
description: "Nasazení, ověřování a volání webového rozhraní API a rozhraní API REST v pracovních postupech pro integrací systému službou Azure Logic Apps"
keywords: "webové rozhraní API, rozhraní REST API, konektorů, pracovní postupy, integrace v rámci systému, ověření"
services: logic-apps
author: stepsic-microsoft-com
manager: anneta
editor: 
documentationcenter: 
ms.assetid: f113005d-0ba6-496b-8230-c1eadbd6dbb9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: ca1e4f28196b21f43b7c9ab94029684121e36f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-call-and-authenticate-custom-apis-as-connectors-for-logic-apps"></a>Nasazení, volání a ověření vlastní rozhraní API jako konektory pro logic apps

Po jste [vytvořte vlastní rozhraní API](./logic-apps-create-api-app.md) , zadejte akce nebo aktivační události toouse v logiku aplikace pracovní postupy, je nutné nasadit vaše rozhraní API, než bude možné volat. A i když můžete volat jakéhokoli rozhraní API z aplikace logiky, pro hello nejlepších, přidejte [Swagger metadata](http://swagger.io/specification/) operace vaše rozhraní API a parametry, který popisuje. Tento soubor Swagger pomáhá rozhraní API fungují lépe a snadněji integrovat s logic apps.

Můžete nasadit vaše rozhraní API jako [webové aplikace](../app-service-web/app-service-web-overview.md), ale zvažte nasazení vašich rozhraní API jako [aplikace API](../app-service-api/app-service-api-apps-why-best-platform.md), který může usnadnit úlohu při sestavení, hostovat a používat rozhraní API v hello cloudu a místně. Nemáte toochange žádný kód ve vašich rozhraní API – aplikace kód tooan API jen nasadit. Vaše rozhraní API můžete hostovat na [Azure App Service](../app-service/app-service-value-prop-what-is.md), platformy jako služba (PaaS) nabídka, která jedním ze způsobů nejlepší, nejjednodušší a nejvíce škálovatelným hello poskytuje pro hostování rozhraní API.

tooauthenticate volání z logiku aplikace tooyour rozhraní API, nastavením Azure Active Directory v hello portálu Azure, nemáte tooupdate vašeho kódu. Nebo můžete požadovat a vynutit ověřování prostřednictvím kódu vaše rozhraní API.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>Nasadit své rozhraní API jako webové aplikace nebo aplikace API

Než bude možné volat vlastního rozhraní API z aplikace logiky, nasaďte své rozhraní API jako webovou aplikaci nebo aplikaci tooAzure rozhraní API služby App Service. Navíc toomake váš dokument Swagger přečíst hello návrhář aplikace na základě logiky, nastavte vlastnosti definice hello rozhraní API a zapnout [(CORS) pro sdílení prostředků různého původu](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) pro webovou aplikaci nebo aplikaci API.

1. V hello portálu Azure vyberte webovou aplikaci nebo aplikaci API.

2. V okně hello, které se otevře v části **rozhraní API**, zvolte **definice rozhraní API**. Sada hello **umístění definice rozhraní API** toohello adresa URL pro váš soubor swagger.json.

   Adresa URL hello obvykle, se zobrazí v tomto formátu:`https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Soubor tooSwagger odkazu pro vaše vlastní rozhraní API](media/logic-apps-custom-hosted-api/custom-api-swagger-url.png)

3. V části **rozhraní API**, zvolte **CORS**. Nastavení zásad hello CORS pro **povolené zdroje** příliš**'*'** (povolit všechny).

   Toto nastavení umožňuje požadavky z návrháře aplikace logiky.

   ![Povolení požadavků z vlastního rozhraní API tooyour návrhář aplikace logiky](media/logic-apps-custom-hosted-api/custom-api-cors.png)

Další informace najdete v těchto článcích:

* [Přidat metadata Swagger pro rozhraní ASP.NET web API](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)
* [Nasazení technologie ASP.NET web API tooAzure služby App Service](../app-service-api/app-service-api-dotnet-get-started.md)

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Volání vlastního rozhraní API z logiku aplikace pracovních postupů

Po nastavení vlastnosti definice hello rozhraní API a CORS by měl k dispozici pro tooinclude v pracovním postupu aplikace logiky triggery a akce vlastního rozhraní API. 

*  tooview hello weby, které mají Swagger adresy URL, můžete procházet své předplatné weby v hello návrhář aplikace logiky.

*  Dostupné akce tooview a vstupy tak, že odkazuje na dokumentem Swagger pomocí hello [HTTP + Swagger akce](../connectors/connectors-native-http-swagger.md).

*  toocall jakéhokoli rozhraní API, včetně rozhraní API, která nesmí mít ani vystavit dokumentem Swagger můžete kdykoli vytvořit požadavek s hello [akce HTTP](../connectors/connectors-native-http.md).

## <a name="authenticate-calls-tooyour-custom-api"></a>Ověření vlastního rozhraní API tooyour volání

Můžete zabezpečit volání tooyour vlastního rozhraní API těmito způsoby:

* [Žádné změny kódu](#no-code): ochrana rozhraní API s [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) prostřednictvím portálu Azure hello, takže nemáte tooupdate kódu nebo znovu nasadit své rozhraní API.

  > [!NOTE]
  > Ve výchozím nastavení hello ověřování Azure AD, které zapnout v hello portál Azure neposkytuje podrobné autorizace. Toto ověřování například zamkne vaše rozhraní API toojust konkrétní klienta, není tooa konkrétního uživatele nebo aplikace. 

* [Aktualizujte kód vaše rozhraní API](#update-code): ochrana rozhraní API vynucením [ověřování pomocí certifikátu](#certificate), [základní ověřování](#basic), nebo [ověřování Azure AD](#azure-ad-code) prostřednictvím kódu.

<a name="no-code"></a>

### <a name="authenticate-calls-tooyour-api-without-changing-code"></a>Ověření volání API tooyour beze změny kódu

Tady je hello obecné kroky pro tuto metodu:

1. Vytvořte dvě [identity aplikace Azure Active Directory (Azure AD)](../app-service-api/app-service-api-dotnet-service-principal-auth.md): jeden pro svou aplikaci logiky a jeden pro webové aplikace (nebo aplikace API).

2. tooyour tooauthenticate volání rozhraní API, pomocí přihlašovacích údajů hello (ID klienta a tajný klíč) hello [instanční objekt](../app-service-api/app-service-api-dotnet-service-principal-auth.md) který je spojen s hello identity aplikací Azure AD pro svou aplikaci logiky.

3. Zahrňte aplikace hello ID svou definici. aplikaci logiky.

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a>Část 1: Vytvoření identity aplikací Azure AD pro svou aplikaci logiky

Aplikace logiky používá tento tooauthenticate Azure AD application identity služby Azure AD. Můžete mít pouze tooset si tuto identitu jednou pro váš adresář. Například můžete toouse hello stejnou identitu pro všechny aplikace logiky, i když můžete vytvořit jedinečné identity pro každou aplikaci logiky. Můžete nastavit tyto identity v hello portál Azure, [portál Azure classic](#app-identity-logic-classic), nebo použijte [prostředí PowerShell](#powershell).

**Vytvoření identity aplikace hello aplikace logiky v hello portálu Azure**

1. V hello [portál Azure](https://portal.azure.com "https://portal.azure.com"), zvolte **Azure Active Directory**. 

2. Potvrďte, že jste v hello stejného adresáře jako webovou aplikaci nebo aplikaci API.

   > [!TIP]
   > tooswitch adresáře, klikněte na váš profil a vyberte jiný adresář. Nebo zvolte **přehled** > **přepínač directory**.

3. V adresáři nabídce hello pod **spravovat**, zvolte **registrace aplikace** > **nové registrace aplikace**.

   > [!TIP]
   > Ve výchozím nastavení seznam registrace aplikace hello zobrazuje všechny registrace aplikace ve vašem adresáři. Vyberte pouze vaší aplikace registrace, další pole hledání toohello tooview **Moje aplikace**. 

   ![Vytvořit novou registraci aplikace](./media/logic-apps-custom-hosted-api/new-app-registration-azure-portal.png)

4. Zadejte název vaší identity aplikace, ponechejte **typ aplikace** nastavit příliš**webovou aplikaci nebo rozhraní API**, zadejte jedinečný řetězec formátovaný jako doménu pro **přihlašovací adresa URL**a zvolte  **Vytvoření**.

   ![Zadejte název a adresu URL pro přihlašování identita aplikace](./media/logic-apps-custom-hosted-api/logic-app-identity-azure-portal.png)

   Hello identita aplikace, kterou jste vytvořili pro svou aplikaci logiky nyní se zobrazí v seznamu registrace aplikací hello.

   ![Identita aplikace pro svou aplikaci logiky](./media/logic-apps-custom-hosted-api/logic-app-identity-created.png)

5. V seznamu aplikací registrace hello vyberte novou identitu aplikace. Zkopírujte a uložte hello **ID aplikace** toouse jako hello "ID klienta" aplikace logiky v části 3.

   ![Zkopírujte a uložte ID aplikace pro aplikaci logiky](./media/logic-apps-custom-hosted-api/logic-app-application-id.png)

6. Pokud nastavení identity aplikace nejsou zobrazeny, zvolte **nastavení** nebo **všechna nastavení**.

7. V části **přístup pomocí rozhraní API**, zvolte **klíče**. V části **popis**, zadejte název pro váš klíč. V části **Expires**, vyberte dobu trvání pro váš klíč.

   Hello klíč, který vytváříte funguje jako identita aplikace hello "tajný" nebo heslo pro svou aplikaci logiky.

   ![Vytvořte klíč pro identitu aplikace logiky](./media/logic-apps-custom-hosted-api/create-logic-app-identity-key-secret-password.png)

8. Na panelu nástrojů hello, zvolte **Uložit**. V části **hodnotu**, klíč se teď zobrazí. 
**Ujistěte se, že toocopy a uložte klíč** pro pozdější použití protože je skrytá hello klíč když necháte klíče okno hello.

   Při konfiguraci aplikace logiky v rámci 3, zadejte tento klíč jako hello "tajný" nebo heslo.

   ![Zkopírujte a uložte klíč pro pozdější](./media/logic-apps-custom-hosted-api/logic-app-copy-key-secret-password.png)

<a name="app-identity-logic-classic"></a>

**Vytvoření identity aplikace hello aplikace logiky v hello portál Azure classic**

1. V hello portál Azure classic, vyberte [ **služby Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).

2. Vyberte hello stejný adresář, který používáte pro webovou aplikaci nebo aplikaci API.

3. Na hello **aplikace** , zvolte **přidat** na hello dolní části stránky hello.

4. Zadejte název vaší identity aplikace a zvolte **Další** (šipka vpravo).

5. V části **vlastností aplikace**, poskytovat jedinečný řetězec formátovaný jako doménu pro **přihlašovací adresa URL** a **identifikátor ID URI aplikace**a zvolte **Complete** (zaškrtnutí).

6. Na hello **konfigurace** kartě, zkopírujte a uložte hello **ID klienta** pro vaše aplikace toouse logiku v části 3.

7. V části **klíče**, otevřete hello **vyberte dobu trvání** seznamu. Vyberte dobu trvání pro váš klíč.

   Hello klíč, který vytváříte funguje jako identita aplikace hello "tajný" nebo heslo pro svou aplikaci logiky.

8. V dolní části hello hello stránky, zvolte **Uložit**. Toowait může mít několik sekund.

9. V části **klíče**, ujistěte se, že toocopy a uložte hello klíč, který se teď zobrazí. 

   Při konfiguraci aplikace logiky v rámci 3, zadejte tento klíč jako hello "tajný" nebo heslo.

Další informace najdete další informace jak příliš [konfigurace vaší služby App Service aplikace toouse Azure Active Directory přihlášení](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

<a name="powershell"></a>

**Vytvoření identity aplikace hello aplikace logiky v prostředí PowerShell**

Můžete provést tuto úlohu prostřednictvím Správce Azure Resource Manager pomocí prostředí PowerShell. V prostředí PowerShell spusťte tyto příkazy:

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. Ujistěte se, zda text hello toocopy **ID klienta** hello (identifikátor GUID pro vašeho tenanta Azure AD), **ID aplikace**a hello heslo, které jste použili.

Další informace najdete další informace jak příliš [vytvořit objekt služby s prostředky tooaccess prostředí PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md).

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a>Část 2: Vytvoření identity aplikací Azure AD pro vaši webovou aplikaci nebo aplikace API

Pokud webovou aplikaci nebo aplikaci API je už nasazená, můžete zapnout ověřování a vytvoření identity aplikace hello v hello portálu Azure. Jinak můžete [zapnout ověřování při nasazení pomocí šablony Azure Resource Manager](#authen-deploy). 

**Vytvoření identity aplikace hello a zapněte ověřování v hello portál Azure pro nasazené aplikace**

1. V hello [portál Azure](https://portal.azure.com "https://portal.azure.com"), najděte a vyberte webovou aplikaci nebo aplikaci API. 

2. V části **nastavení**, zvolte **ověřování/autorizace**. V části **ověřování služby aplikace**, zapnout ověřování **na**. V části **zprostředkovatele ověřování**, zvolte **Azure Active Directory**.

   ![Zapnout ověřování](./media/logic-apps-custom-hosted-api/custom-web-api-app-authentication.png)

3. Teď vytvořte identity aplikací pro webovou aplikaci nebo aplikaci API, jak je vidět tady. Na hello **nastavení Azure Active Directory** okně nastavit **režim správy** příliš**Express**. Zvolte **vytvořit novou aplikaci AD**. Zadejte název vaší identity aplikace a zvolte **OK**. 

   ![Vytvoření identity aplikací pro webovou aplikaci nebo aplikace API](./media/logic-apps-custom-hosted-api/custom-api-application-identity.png)

4. Na hello **ověřování / autorizace okno**, zvolte **Uložit**.

Nyní je nutné vyhledat hello ID klienta a ID klienta pro identitu hello aplikací, který je spojen s webovou aplikaci nebo aplikaci API. Použijte tyto identifikátory v části 3. Proto pokračujte postupem pro hello portál Azure nebo [portál Azure classic](#find-id-classic).

**Najít ID klienta aplikace identit a ID klienta pro webovou aplikaci nebo aplikaci API hello portálu Azure**

1. V části **zprostředkovatele ověřování**, zvolte **Azure Active Directory**. 

   ![Zvolte "Azure Active Directory"](./media/logic-apps-custom-hosted-api/custom-api-app-identity-client-id-tenant-id.png)

2. Na hello **nastavení Azure Active Directory** okně nastavit **režim správy** příliš**Upřesnit**.

3. Kopírování hello **ID klienta**a uložte tento identifikátor GUID pro použití v části 3.

   > [!TIP] 
   > Pokud **ID klienta** a **Url vystavitele** nemáte objeví, zkuste aktualizovat hello portál Azure a zopakujte krok 1.

4. V části **Url vystavitele**, zkopírujte a uložte právě hello identifikátor GUID pro část 3. Můžete také použít tento identifikátor GUID ve vaší webové aplikace nebo aplikace API šablonu nasazení, v případě potřeby.

   Tento identifikátor GUID je GUID konkrétní klienta ("ID klienta") a by se zobrazit v této adresy URL:`https://sts.windows.net/{GUID}`

5. Bez uložení změn, zavřete hello **nastavení Azure Active Directory** okno.

<a name="find-id-classic"></a>

**Najít ID klienta aplikace identit a ID klienta pro webovou aplikaci nebo aplikaci API hello portál Azure classic**

1. V hello portál Azure classic, vyberte [ **služby Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).

2.  Vyberte hello adresář, který používáte pro webovou aplikaci nebo aplikaci API.

3. V hello **vyhledávání** pole, najděte a vyberte hello identity aplikací pro webovou aplikaci nebo aplikaci API.

4. Na hello **konfigurace** kartě, kopie hello **ID klienta**a uložte tento identifikátor GUID pro použití v části 3.

5. Po získání ID klienta hello, dole hello hello **konfigurace** , zvolte **zobrazit koncové body**.

6. Zkopírujte adresu URL hello **dokument federačních metadat**a vyhledejte toothat adresy URL.

7. V dokumentu metadat hello, které se otevře, vyhledejte kořenový hello **EntityDescriptor ID** element, který má **entityID** atributů v této podobě:`https://sts.windows.net/{GUID}` 

      Hello identifikátor GUID v tento atribut je GUID konkrétní klienta (ID klienta).

8. ID klienta hello zkopírovat a uložit toto ID pro použití v rámci 3 a toouse ve vaší webové aplikace nebo aplikace API šablonu nasazení, v případě potřeby.

Další informace naleznete v následujících tématech:

* [Ověřování uživatelů pro aplikace API v Azure App Service](../app-service-api/app-service-api-dotnet-user-principal-auth.md)
* [Ověřování a autorizace ve službě Azure App Service](../app-service/app-service-authentication-overview.md)

<a name="authen-deploy"></a>

**Zapnout ověřování při nasazení pomocí šablony Azure Resource Manager**

Stále je nutné vytvořit identity aplikací Azure AD pro vaši webovou aplikaci nebo aplikaci API, která se liší od identity aplikace hello pro svou aplikaci logiky. Identita aplikace toocreate hello, hello postupujte podle předchozích kroků v části 2 pro hello portálu Azure. Můžete také postupujte podle kroků hello v část 1, ale ujistěte se, že toouse vaší webové aplikace nebo aplikace API skutečné `https://{URL}` pro **přihlašovací adresa URL** a **identifikátor ID URI aplikace**. Z těchto kroků máte toosave obou client hello ID a ID klienta pro použití v šabloně nasazení vaší aplikace a také pro část 3.

> [!NOTE]
> Když vytvoříte hello Azure AD identity aplikací pro webovou aplikaci nebo aplikaci API, musíte použít hello portál Azure nebo portál Azure classic, nikoli prostředí PowerShell. Hello prostředí PowerShell nemá nastavit hello požadované oprávnění uživatelé toosign do webu.

Po získání ID klienta hello a ID klienta, patří tyto identifikátory jako subresource vaší webové aplikace nebo aplikace API v šabloně nasazení:

   ```
   "resources": [
       {
           "apiVersion": "2015-08-01",
           "name": "web",
           "type": "config",
           "dependsOn": ["[concat('Microsoft.Web/sites/','parameters('webAppName'))]"],
           "properties": {
               "siteAuthEnabled": true,
               "siteAuthSettings": {
                 "clientId": "{client-ID}",
                 "issuer": "https://sts.windows.net/{tenant-ID}/",
               }
           }
       }
   ]
   ```

tooautomatically nasazení prázdnou webovou aplikaci a aplikace logiky společně s ověřování Azure Active Directory, [zobrazení hello úplnou šablonu zde](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), nebo klikněte na tlačítko **nasazení tooAzure** tady:

[![Nasazení tooAzure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

#### <a name="part-3-populate-hello-authorization-section-in-your-logic-app"></a>Část 3: Naplnění hello části autorizace v aplikaci logiky

již existuje v této části autorizace nastavit Hello předchozí šablona, ale vytváříte-li přímo hello aplikace logiky, musí obsahovat části plnou autorizaci hello.

Otevřete svou definici. aplikaci logiky v zobrazení kódu, přejděte toohello **HTTP** část akce, najít hello **autorizace** části a zahrnovat tento řádek:

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| Element | Popis |
| ------- | ----------- |
| Klienta |Hello identifikátor GUID pro klienta hello Azure AD |
| Cílová skupina |Povinná hodnota. Hello identifikátor GUID pro hello cílový prostředek, který má tooaccess - hello ID klienta z identity hello aplikací pro webovou aplikaci nebo aplikaci API |
| clientId |Hello identifikátor GUID pro klienta hello žádají o přístup - hello ID klienta z identity aplikace hello aplikace logiky |
| tajný klíč |Povinná hodnota. Hello klíč nebo heslo z identity aplikace hello pro hello klienta, který požaduje hello přístupový token |
| type |typ ověřování Hello. Pro ověřování ActiveDirectoryOAuth hello hodnota je `ActiveDirectoryOAuth`. |

Například:

```
{
   ...
   "actions": {
      "some-action": {
         "conditions": [],
         "inputs": {
            "method": "post",
            "uri": "https://your-api-azurewebsites.net/api/your-method",
            "authentication": {
               "tenant": "tenant-ID",
               "audience": "client-ID-from-azure-ad-app-for-web-app-or-api-app",
               "clientId": "client-ID-from-azure-ad-app-for-logic-app",
               "secret": "key-from-azure-ad-app-for-logic-app",
               "type": "ActiveDirectoryOAuth"
            }
         },
      }
   }
}
```

<a name="update-code"></a>

### <a name="secure-api-calls-through-code"></a>Zabezpečená volání rozhraní API prostřednictvím kódu

<a name="certificate"></a>

#### <a name="certificate-authentication"></a>Ověřování pomocí certifikátu

hello příchozí požadavky toovalidate z logiku aplikace tooyour webové aplikace nebo aplikace API, můžete použít klientské certifikáty. Další tooset až kódu, [jak vzájemné ověřování TLS tooconfigure](../app-service-web/app-service-web-configure-tls-mutual-auth.md).

V hello **autorizace** část, zahrnují tento řádek: 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| Element | Popis |
| ------- | ----------- |
| type |Povinná hodnota. typ ověřování Hello. Pro klientské certifikáty protokolu SSL, musí být hodnota hello `ClientCertificate`. |
| heslo |Povinná hodnota. Hello heslo pro přístup k certifikátu klienta hello (soubor PFX) |
| Soubor PFX |Povinná hodnota. Kódování Base64 obsah hello klientského certifikátu (soubor PFX) |

<a name="basic"></a>

#### <a name="basic-authentication"></a>Základní ověřování

toovalidate příchozí požadavky z logiku aplikace tooyour webové aplikace nebo aplikace API, můžete základní ověřování, jako je například uživatelské jméno a heslo. Základní ověřování je běžný vzor, a můžete toto ověřování v jakékoli toobuild jazyk používaný webovou aplikaci nebo aplikaci API.

V hello **autorizace** část, zahrnují tento řádek:

`{"type": "basic", "username": "username", "password": "password"}`.

| Element | Popis |
| --- | --- |
| type |Povinná hodnota. typ ověřování Hello. Pro základní ověřování, musí být hodnota hello `Basic`. |
| uživatelské jméno |Povinná hodnota. Hello uživatelské jméno pro ověřování |
| heslo |Povinná hodnota. Hello heslo pro ověřování |

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a>Ověřování Azure Active Directory prostřednictvím kódu

Ve výchozím nastavení hello ověřování Azure AD, které zapnout v hello portál Azure neposkytuje podrobné autorizace. Toto ověřování například zamkne vaše rozhraní API toojust konkrétní klienta, není tooa konkrétního uživatele nebo aplikace. 

aplikace logiky tooyour toorestrict rozhraní API přístup prostřednictvím kódu, extrahujte hello hlavičky, která má hello JSON web token (JWT). Zkontrolujte identitu volajícího hello a odmítnout požadavky, které se neshodují.

Tooimplement budete pokračovat, toto ověřování zcela v vlastního kódu a není hello použijte portál Azure, zjistěte, jak příliš [ověření pomocí místní služby Active Directory v aplikaci Azure](../app-service-web/web-sites-authentication-authorization.md).

toocreate identity aplikací pro svou aplikaci logiky a použít tuto identitu toocall vaše rozhraní API, postupujte podle předchozích kroků hello.

## <a name="next-steps"></a>Další kroky

* [Kontrola výkonu aplikace logiky s diagnostické protokoly a výstrahy](logic-apps-monitor-your-logic-apps.md)