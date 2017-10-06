---
title: "aaaProtect back-endu webového rozhraní API pomocí Azure Active Directory a API Management | Microsoft Docs"
description: "Zjistěte, jak tooprotect back-endu webového rozhraní API pomocí Azure Active Directory a API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: f856ff03-64a1-4548-9ec4-c0ec4cc1600f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: f4b323034354aa09579c643bade47257fbf1e5c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprotect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Jak tooprotect back-endu webového rozhraní API pomocí Azure Active Directory a API Management
následující video ukazuje, jak Hello toobuild back-end webového rozhraní API a chránit pomocí Azure Active Directory a rozhraní API správy protokolu OAuth 2.0.  Tento článek obsahuje přehled a další informace o hello kroky hello video. Následující 24 minutu video ukazuje, jak na:

* Sestavení webového rozhraní API back-end a zabezpečte ji pomocí AAD - počínaje 1:30
* Importovat rozhraní API hello do rozhraní API Management – počínaje 7:10
* Konfigurace hello vývojáře portálu toocall hello API – počínaje 9:09
* Konfigurace desktopová aplikace toocall hello API – od 18:08
* Konfigurace JWT ověření zásad toopre-autorizaci požadavků - začínající na 20:47

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

## <a name="create-an-azure-ad-directory"></a>Vytvořte adresář služby Azure AD
toosecure zálohovaný vašeho webového rozhraní API pomocí Azure Active Directory je nutné nejdříve vytvořit klienta služby AAD. V tomto videu klienta s názvem **APIMDemo** se používá. toocreate klienta služby AAD přihlášení toohello [portálu Azure Classic](https://manage.windowsazure.com) a klikněte na tlačítko **nový**->**App Services**->**Active Adresář**->**Directory**->**vytvořit vlastní**. 

![Azure Active Directory][api-management-create-aad-menu]

V tomto příkladu adresář s názvem **APIMDemo** je vytvořena s výchozí doménu s názvem **DemoAPIM.onmicrosoft.com**. Tento adresář se používá napříč hello video.

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Vytvoření webového rozhraní API služby Zabezpečené přes Azure Active Directory
V tomto kroku se vytvoří webového rozhraní API back-end pomocí Visual Studio 2013. Tento krok hello video se spustí v 1:30. toocreate webového rozhraní API back-end projektu v sadě Visual Studio klikněte na položku **soubor**->**nový**->**projektu**a zvolte **technologie ASP.NET Aplikace** z hello **webové** seznamu šablon. V této video hello projektu jmenuje **APIMAADDemo**. Klikněte na tlačítko **OK** toocreate hello projektu. 

![Visual Studio][api-management-new-web-app]

Klikněte na tlačítko **webového rozhraní API** z hello **vyberte seznam šablony** toocreate projekt webového rozhraní API. Klikněte na tlačítko tooconfigure ověřování Azure Directory **změna ověřování**.

![Nový projekt][api-management-new-project]

Klikněte na tlačítko **účty organizace**a zadejte hello **domény** klienta služby AAD. Tento příklad hello domény je **DemoAPIM.onmicrosoft.com**. hello domény adresáře můžete získat hello **domén** svůj adresář.

![Domény][api-management-aad-domains]

Konfigurace nastavení hello potřeby v hello **změna ověřování** dialogové okno a klikněte na tlačítko **OK**.

![Změna ověřování][api-management-change-authentication]

Když kliknete na tlačítko **OK** Visual Studio se pokusí tooregister vaší aplikace pomocí svého adresáře Azure AD a může být výzvami toosign ve Visual Studio. Přihlaste se pomocí účtu správce pro váš adresář.

![Přihlaste se tooVisual Studio][api-management-sign-in-vidual-studio]

tooconfigure tohoto projektu jako hello webového rozhraní API Azure zkontrolujte pole pro **hostitel v cloudu hello** a pak klikněte na **OK**.

![Nový projekt][api-management-new-project-cloud]

Může být výzvami toosign v tooAzure, a pak můžete nakonfigurovat hello webové aplikace.

![Konfigurace][api-management-configure-web-app]

V tomto příkladu a nové **plán služby App Service** s názvem **APIMAADDemo** je zadán.

Klikněte na tlačítko **OK** tooconfigure hello webové aplikace a vytvoření projektu hello.

## <a name="add-hello-code-toohello-web-api-project"></a>Přidat projekt webového rozhraní API toohello hello kódu
Hello další krok v hello video přidá projekt webového rozhraní API toohello hello kódu. Tento krok se spustí při 4:35.

Hello webového rozhraní API v tomto příkladu implementuje základní kalkulačky služby pomocí modelu a kontroleru. Klikněte pravým tlačítkem na model hello tooadd pro službu hello, **modely** v **Průzkumníku řešení** a zvolte **přidat**, **třída**. Název třídy hello `CalcInput` a klikněte na tlačítko **přidat**.

Přidejte následující hello `using` příkaz toohello začátek hello `CalcInput.cs` souboru.

```c#
using Newtonsoft.Json;
```

Nahraďte hello generované třídy hello následující kód.

```c#
public class CalcInput
{
    [JsonProperty(PropertyName = "a")]
    public int a;

    [JsonProperty(PropertyName = "b")]
    public int b;
}
```

Klikněte pravým tlačítkem na **řadiče** v **Průzkumníku řešení** a zvolte **přidat**->**řadič**. Zvolte **webové rozhraní API 2 řadiče - prázdný** a klikněte na tlačítko **přidat**. Typ **CalcController** hello řadiče název a klikněte na tlačítko **přidat**.

![Přidání Kontroleru][api-management-add-controller]

Přidejte následující hello `using` příkaz toohello začátek hello `CalcController.cs` souboru.

```c#
using System.IO;
using System.Web;
using APIMAADDemo.Models;
```

Třída controller hello generované nahraďte hello následující kód. Tento kód implementuje hello `Add`, `Subtract`, `Multiply`, a `Divide` operace hello rozhraní API základní kalkulačky.

```c#
[Authorize]
public class CalcController : ApiController
{
    [Route("api/add")]
    [HttpGet]
    public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/sub")]
    [HttpGet]
    public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/mul")]
    [HttpGet]
    public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/div")]
    [HttpGet]
    public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }
}
```

Stiskněte klávesu **F6** toobuild a ověřte hello řešení.

## <a name="publish-hello-project-tooazure"></a>Publikování projektu tooAzure hello
Tento krok hello Visual Studio je projekt publikované tooAzure. Tento krok hello video začíná na 5:45.

toopublish hello tooAzure projektu, klikněte pravým tlačítkem na hello **APIMAADDemo** projektu v sadě Visual Studio a zvolte **publikovat**. Ponechat výchozí nastavení hello v hello **Publikovat Web** dialogové okno a klikněte na tlačítko **publikovat**.

![Publikování webu][api-management-web-publish]

## <a name="grant-permissions-toohello-azure-ad-backend-service-application"></a>Udělení oprávnění aplikace služby back-end toohello Azure AD
V adresáři služby Azure AD jako součást konfigurace hello a proces publikování projektu webového rozhraní API je vytvořena nová aplikace pro hello back-end službu. V tomto kroku hello video od 6:13 oprávnění toohello webového rozhraní API back-end.

![Aplikace][api-management-aad-backend-app]

Klikněte na název hello hello aplikace tooconfigure hello požadované oprávnění. Přejděte toohello **konfigurace** a přejděte dolů toohello **oprávnění tooother aplikace** části. Klikněte na tlačítko hello **oprávnění aplikací** rozevíracího seznamu vedle položky **Windows** **Azure Active Directory**, zaškrtněte políčko hello pro **čtení dat adresáře**a klikněte na tlačítko **Uložit**.

![Přidání oprávnění][api-management-aad-add-permissions]

> [!NOTE]
> Pokud **Windows** **Azure Active Directory** nejsou uvedené v části oprávnění tooother aplikace, klikněte na tlačítko **přidat aplikaci** a přidejte ho do seznamu hello.
> 
> 

Poznamenejte si hello **identifikátor Id URI aplikace** pro použití v následném kroku při aplikaci Azure AD je nakonfigurován pro portál pro vývojáře hello API Management.

![Id URI aplikace][api-management-aad-sso-uri]

## <a name="import-hello-web-api-into-api-management"></a>Importovat hello webového rozhraní API do rozhraní API Management
Rozhraní API se konfigurují na hello rozhraní API portálu vydavatele, který je přístupný prostřednictvím hello portálu Azure. tooreach, klikněte na tlačítko **portál vydavatele** z hello nástrojů služby API Management. Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Správa vašeho prvního rozhraní API] [ Manage your first API] kurzu.

![Portál vydavatele][api-management-management-console]

Operace jde [tooAPIs přidat ručně](api-management-howto-add-operations.md), nebo může být importován. V tomto videu se operace importují ve formátu Swagger od 6:40.

Vytvořte soubor s názvem `calcapi.json` s následující obsah a uložit ho tooyour počítače. Ujistěte se, že hello `host` atribut odkazuje tooyour webového rozhraní API back-end. V tomto příkladu `"host": "apimaaddemo.azurewebsites.net"` se používá.

```json
{
  "swagger": "2.0",
  "info": {
    "title": "Calculator",
    "description": "Arithmetics over HTTP!",
    "version": "1.0"
  },
  "host": "apimaaddemo.azurewebsites.net",
  "basePath": "/api",
  "schemes": [
    "http"
  ],
  "paths": {
    "/add?a={a}&b={b}": {
      "get": {
        "description": "Responds with a sum of two numbers.",
        "operationId": "Add two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>51</code>.",
            "required": true,
            "type": "string",
            "default": "51",
            "enum": [
              "51"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>49</code>.",
            "required": true,
            "type": "string",
            "default": "49",
            "enum": [
              "49"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/sub?a={a}&b={b}": {
      "get": {
        "description": "Responds with a difference between two numbers.",
        "operationId": "Subtract two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>50</code>.",
            "required": true,
            "type": "string",
            "default": "50",
            "enum": [
              "50"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/div?a={a}&b={b}": {
      "get": {
        "description": "Responds with a quotient of two numbers.",
        "operationId": "Divide two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/mul?a={a}&b={b}": {
      "get": {
        "description": "Responds with a product of two numbers.",
        "operationId": "Multiply two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>5</code>.",
            "required": true,
            "type": "string",
            "default": "5",
            "enum": [
              "5"
            ]
          }
        ],
        "responses": { }
      }
    }
  }
}
```

tooimport hello kalkulačky rozhraní API, klikněte na tlačítko **rozhraní API** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **rozhraní API pro Import**.

![Tlačítko Importovat rozhraní API][api-management-import-api]

Proveďte následující kroky rozhraní API kalkulačky hello tooconfigure hello.

1. Klikněte na tlačítko **ze souboru**, procházet toohello `calculator.json` soubor uložit a klikněte na tlačítko hello **Swagger** přepínač.
2. Typ **calc** do hello **přípona adresy URL webového rozhraní API** textové pole.
3. Klikněte na tlačítko v hello **produkty (volitelné)** pole a zvolte **Starter**.
4. Klikněte na tlačítko **Uložit** tooimport hello rozhraní API.

![Přidání nového rozhraní API][api-management-import-new-api]

Po importu rozhraní API hello hello souhrnná stránka rozhraní API hello zobrazí na portálu vydavatele hello.

## <a name="call-hello-api-unsuccessfully-from-hello-developer-portal"></a>Volání rozhraní API hello neúspěšně z portálu pro vývojáře hello
V tomto okamžiku hello rozhraní API byla naimportována do rozhraní API správy, ale nejde ještě volat úspěšně z portálu pro vývojáře hello protože hello back-end službu chráněné pomocí ověřování Azure AD. Tento postup je znázorněn v hello videa začínající na 7:40 pomocí hello následující kroky.

Klikněte na tlačítko **portál pro vývojáře** z hello horní pravé části portálu vydavatele hello.

![Portál pro vývojáře][api-management-developer-portal-menu]

Klikněte na tlačítko **rozhraní API** a klikněte na tlačítko hello **kalkulačky** rozhraní API.

![Portál pro vývojáře][api-management-dev-portal-apis]

Klikněte na tlačítko **vyzkoušet**.

![Vyzkoušet][api-management-dev-portal-try-it]

Klikněte na tlačítko **odeslat** a poznamenejte si stav odezvy hello **401 – Neověřeno**.

![Odeslat][api-management-dev-portal-send-401]

Hello požadavek není autorizovaný, protože hello back-end rozhraní API je chráněn službou Azure Active Directory. Před úspěšně volání rozhraní API hello portál pro vývojáře hello musí být nakonfigurované tooauthorize vývojáře, kteří používají OAuth 2.0. Tento proces je popsán v následující části hello.

## <a name="register-hello-developer-portal-as-an-aad-application"></a>Zaregistrujte se jako aplikaci AAD hello portál pro vývojáře
Hello prvním krokem při konfiguraci hello vývojáře portálu tooauthorize vývojáře, kteří používají OAuth 2.0 je portál pro vývojáře hello tooregister jako aplikaci AAD. Tento postup je znázorněn od 8:27 v hello video.

Přejděte toohello Azure AD tenant z první krok hello toto video, v tomto příkladu **APIMDemo** a přejděte toohello **aplikace** kartě.

![Nová aplikace][api-management-aad-new-application-devportal]

Klikněte na tlačítko hello **přidat** tlačítko toocreate novou aplikaci Azure Active Directory a vyberte **přidat aplikaci, kterou vyvíjí Moje organizace**.

![Nová aplikace][api-management-new-aad-application-menu]

Zvolte **webové aplikace nebo webové rozhraní API**, zadejte název a klikněte na šipku další hello. V tomto příkladu **APIMDeveloperPortal** se používá.

![Nová aplikace][api-management-aad-new-application-devportal-1]

Pro **přihlašovací adresa URL** zadejte adresu URL hello služby API Management a připojte `/signin`. V tomto příkladu `https://contoso5.portal.azure-api.net/signin` se používá.

Pro **URL Id aplikace** zadejte adresu URL hello služby API Management a připojte některé jedinečných znaků. To může být jakékoli požadované znaky a v tomto příkladu `https://contoso5.portal.azure-api.net/dp` se používá. Když hello potřeby **vlastností aplikace** jsou nakonfigurovaná, klikněte na tlačítko hello zaškrtnutí toocreate hello aplikace.

![Nová aplikace][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Konfigurace serveru autorizace OAuth 2.0 rozhraní API Management
dalším krokem Hello je tooconfigure serveru autorizace OAuth 2.0 ve službě API Management. Tento krok je znázorněn v videa začínající na 9:43 hello.

Klikněte na tlačítko **zabezpečení** hello API Management na nabídce hello vlevo, klikněte na tlačítko **OAuth 2.0**a potom klikněte na **přidat autorizační** serveru.

![Přidání serveru ověřování][api-management-add-authorization-server]

Zadejte název a volitelný popis v hello **název** a **popis** pole. Tato pole jsou použité tooidentify hello OAuth 2.0 autorizace serveru v rámci instance služby API Management hello. V tomto příkladu **ukázkový server autorizace** se používá. Později při zadávání toobe server OAuth 2.0 používat k ověřování pro rozhraní API, vyberete tento název.

Pro hello **adresa URL stránky registrace klienta** zadejte hodnotu zástupného symbolu, jako `http://localhost`.  Hello **adresa URL stránky registrace klienta** body toohello stránka, kterou můžou uživatelé použít toocreate a nakonfigurovat svoje vlastní účty zprostředkovatelů OAuth 2.0, které podporují správu uživatelské účty. V tomto příkladu uživatele není vytvořit a nakonfigurovat svoje vlastní účty, tak se používá zástupný symbol.

![Přidání serveru ověřování][api-management-add-authorization-server-1]

Potom zadejte **adresu URL koncového bodu autorizace** a **adresu URL koncového bodu Token**.

![Autorizace serveru][api-management-add-authorization-server-1a]

Tyto hodnoty lze získat z hello **koncových bodů aplikace** stránku hello AAD aplikace, které jste vytvořili pro portál pro vývojáře hello. Koncové body hello tooaccess přejděte toohello **konfigurace** hello AAD aplikace a klikněte na **zobrazit koncové body**.

![Aplikace][api-management-aad-devportal-application]

![Zobrazit koncové body][api-management-aad-view-endpoints]

Kopírování hello **koncový bod autorizace OAuth 2.0** a vložte jej do hello **adresu URL koncového bodu autorizace** textové pole.

![Přidání serveru ověřování][api-management-add-authorization-server-2]

Kopírování hello **koncový bod tokenu OAuth 2.0** a vložte jej do hello **adresu URL koncového bodu Token** textové pole.

![Přidání serveru ověřování][api-management-add-authorization-server-2a]

Kromě toho toopasting v hello koncovému bodu tokenu, přidáte další text parametr s názvem **prostředků** a pro hodnotu hello používat hello **identifikátor Id URI aplikace** z hello AAD aplikace hello back-end službu, která byla vytvoří, když byla publikována hello projektu sady Visual Studio.

![Id URI aplikace][api-management-aad-sso-uri]

Potom zadejte pověření klienta hello. Tyto jsou hello přihlašovací údaje pro prostředek hello chcete tooaccess, v takovém případě hello portál pro vývojáře.

![Pověření klienta][api-management-client-credentials]

tooget hello **Id klienta**, přejděte toohello **konfigurace** kartě hello AAD aplikace pro vývojáře hello portál a zkopírujte hello **Id klienta**.

tooget hello **tajný klíč klienta** klikněte na tlačítko hello **vyberte dobu trvání** rozevírací seznam v hello **klíče** části a zadat interval. V tomto příkladu se používá 1 rok.

![ID klienta][api-management-aad-client-id]

Klikněte na tlačítko **Uložit** toosave hello konfiguraci a zobrazení hello klíč. 

> [!IMPORTANT]
> Tento klíč si poznamenejte. Po zavření okna konfigurace Azure Active Directory hello hello klíč nelze zobrazit znovu.
> 
> 

Kopírování hello klíče toohello schránky, portál vydavatele back toohello přepínače, vložte klíč hello do hello **tajný klíč klienta** textovému poli a klikněte na tlačítko **Uložit**.

![Přidání serveru ověřování][api-management-add-authorization-server-3]

Hned za hello pověření klienta je udělení autorizačního kódu. Zkopírujte tento autorizační kód a přepínač back tooyour aplikace portálu služby Azure AD vývojáře konfigurace stránky a vložte udělení autorizace hello do hello **adresa URL odpovědi** pole a klikněte na tlačítko **Uložit** znovu.

![Adresa URL odpovědi][api-management-aad-reply-url]

dalším krokem Hello je tooconfigure hello oprávnění pro portál pro vývojáře hello AAD aplikace. Klikněte na tlačítko **oprávnění aplikací** a zaškrtněte políčko hello pro **čtení dat adresáře**. Klikněte na tlačítko **Uložit** toosave to změnit a potom klikněte na **přidat aplikaci**.

![Přidání oprávnění][api-management-add-devportal-permissions]

Klikněte na ikonu hledání hello, typ **APIM** do hello počínaje pole, vyberte **APIMAADDemo**a klikněte na tlačítko zaškrtnutí toosave hello.

![Přidání oprávnění][api-management-aad-add-app-permissions]

Klikněte na tlačítko **delegovaná oprávnění** pro **APIMAADDemo** a zaškrtněte políčko hello pro **přístup APIMAADDemo**a klikněte na tlačítko **Uložit**. To umožňuje vývojáři hello aplikace portálu tooaccess hello back-end službu.

![Přidání oprávnění][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-hello-calculator-api"></a>Povolení autorizace uživatelů OAuth 2.0 pro rozhraní API kalkulačky hello
Teď, když hello OAuth 2.0 je server nakonfigurovaný, můžete je zadat v hello nastavení zabezpečení pro vaše rozhraní API. Tento krok je znázorněn v hello videa začínající na 14:30.

Klikněte na tlačítko **rozhraní API** v levé nabídce text hello a klikněte na tlačítko **kalkulačky** tooview a konfigurovat jeho nastavení.

![API kalkulačky][api-management-calc-api]

Přejděte toohello **zabezpečení** kartě, zkontrolujte hello **OAuth 2.0** zaškrtávací políčko, vyberte hello serveru požadované ověřování z hello **serveru ověřování** rozevíracího seznamu a klikněte na tlačítko **Uložit**.

![API kalkulačky][api-management-enable-aad-calculator]

## <a name="successfully-call-hello-calculator-api-from-hello-developer-portal"></a>Úspěšně hello rozhraní API kalkulačky volejte z portálu pro vývojáře hello
Teď, když na hello rozhraní API je nakonfigurováno autorizace hello OAuth 2.0, můžete jeho operace úspěšně volat z středisku pro vývojáře hello. Tento krok je znázorněn v hello videa začínající na 15:00.

Přejděte zpět toohello **přidat dvě celá čísla** operaci hello kalkulačky služby v portálu pro vývojáře hello a klikněte na **vyzkoušet**. Poznámka: hello novou položku hello **autorizace** části odpovídající serveru ověřování toohello jste právě přidali.

![API kalkulačky][api-management-calc-authorization-server]

Vyberte **autorizační kód** z hello autorizace rozevíracího seznamu a zadejte přihlašovací údaje účtu toouse hello hello. Pokud jste již přihlášeni pomocí účtu hello nemusí zobrazí se výzva.

![API kalkulačky][api-management-devportal-authorization-code]

Klikněte na tlačítko **odeslat** a Poznámka hello **stav odpovědi** z **200 OK** a výsledky hello hello operace v obsahu odpovědi hello.

![API kalkulačky][api-management-devportal-response]

## <a name="configure-a-desktop-application-toocall-hello-api"></a>Konfigurace desktopová aplikace toocall hello rozhraní API
Další postup Hello v hello video začíná na 16:30 a nakonfiguruje hello toocall jednoduchou aplikaci plochy rozhraní API. prvním krokem Hello je tooregister hello desktopová aplikace ve službě Azure AD a poskytněte přístup toohello adresář a toohello back-end službu. Na 18:25 je ukázka aplikace pracovní plochy hello volání operace na API kalkulačky hello.

## <a name="configure-a-jwt-validation-policy-toopre-authorize-requests"></a>Konfigurace JWT ověření zásad toopre-autorizaci požadavků
Hello poslední postup v hello video začíná na 20:48 a ukazuje, jak toouse hello [ověření JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) zásad toopre-autorizaci požadavků na ověřením hello přístupové tokeny každého příchozího požadavku. Pokud hello požadavek není ověřen hello zásady ověřování tokenů JWT, žádost o hello je blokována API Management a není předají toohello back-end.

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
        </claim>
    </required-claims>
</validate-jwt>
```

Jiné předvedení konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: víc funkcí správy rozhraní API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too13:50. Rychlé převinutí vpřed too15:00 toosee hello zásady nakonfigurované v editoru zásad hello, a potom too18:50 pro předvedení volání operace z portálu pro vývojáře hello s i bez hello vyžaduje autorizační token.

## <a name="next-steps"></a>Další kroky
* Podívejte se na další [videa](https://azure.microsoft.com/documentation/videos/index/?services=api-management) o službě API Management.
* Pro jiné způsoby toosecure službě back-end, najdete v části [vzájemného ověření certifikátů](api-management-howto-mutual-certificates.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Manage your first API]: api-management-get-started.md
