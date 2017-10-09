---
title: aaaAuthenticate s Mobile Engagement REST API
description: Popisuje, jak tooauthenticate s Azure Mobile Engagement REST API
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: da82cb36-957a-4e19-a805-b44733cf6597
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 9b54aa5ec3da4bcf55ffe5b7e8d1759095d0c486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Ověření s Mobile Engagementem rozhraní REST API
## <a name="overview"></a>Přehled
Tento dokument popisuje, jak tooget platný Oauth AAD token tooauthenticate s hello Mobile Engagement REST API. 

Předpokládá se, že máte platné předplatné Azure a jste vytvořili aplikaci Mobile Engagement, pomocí jednoho z našich [vývojáře kurzy](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Authentication
Microsoft Azure Active Directory na základě tokenu se používá k ověřování OAuth. 

V žádosti o pořadí tooauthentication rozhraní API musí hlavičku autorizace přidány tooevery požadavek, který je hello následující formulář:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> Azure Active Directory tokeny vyprší za 1 hodinu.
> 
> 

Existuje několik způsobů tooget token. Od hello rozhraní API jsou obecně volat z cloudové služby chcete toouse klíč rozhraní API. Klíč rozhraní API v Azure terminologii nazývá hlavní heslo služby. Hello následující postup popisuje jedním ze způsobů toosetting ho až ručně.

### <a name="one-time-setup-using-script"></a>Jednorázové instalace (pomocí skriptu.)
Postupujte podle hello sadu pokynů níže tooperform hello instalaci pomocí skriptu prostředí PowerShell, který přebírá hello minimální doba pro instalační program, ale používá hello nejvíce povolenou výchozí hodnoty. Volitelně můžete také podle pokynů hello v hello [ruční instalaci](mobile-engagement-api-authentication-manual.md) to chcete provést z hello přímo portál Azure a proveďte jemnějšího konfigurace. 

1. Získat nejnovější verzi prostředí Azure PowerShell z hello [zde](http://aka.ms/webpi-azps). Další informace o pokyny ke stažení hello uvidíte to [odkaz](/powershell/azure/overview).  
2. Po instalaci prostředí Azure PowerShell, použijte hello následující příkazy, abyste měli hello tooensure **Azure modulu** nainstalován:
   
    a. Zkontrolujte, zda je k dispozici v seznamu dostupných modulů hello modul Azure PowerShell hello. 
   
        Get-Module –ListAvailable 
   
    ![K dispozici moduly Azure][1]
   
    b. Pokud nenajdete modul Azure PowerShell hello hello výše seznamu musíte toorun hello následující:
   
        Import-Module Azure 
3. Přihlášení toohello Azure Resource Manageru z prostředí PowerShell spuštěním hello následující příkaz a poskytuje uživatelské jméno a heslo pro účet Azure: 
   
        Login-AzureRmAccount
4. Pokud máte více předplatných, pak byste měli spustit hello následující:
   
    a. Získáte seznam všech předplatných a kopírování hello SubscriptionId hello odběr, který chcete toouse. Zajistěte, aby toto předplatné je stejný jako ten, který má hello aplikace Mobile Engagementu, který se bude hello hello toointeract s použitím rozhraní API. 
   
        Get-AzureRmSubscription
   
    b. Hello spusťte následující příkaz, který poskytnete hello SubscriptionId tooconfigure hello předplatné toobe použít.
   
        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. Zkopírujte hello text hello [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) skriptu tooyour místního počítače a uložte ho jako rutiny prostředí PowerShell (např `APIAuth.ps1`) a spusťte ho `.\APIAuth.ps1`. 
6. Hello skriptu se zeptá, tooprovide na vstup pro **principalName**. Zadejte vhodný název má toouse toocreate aplikace služby Active Directory (např. APIAuth). 
7. Po dokončení skriptu hello zobrazí hello následující čtyři hodnoty, které budete potřebovat tooauthenticate prostřednictvím kódu programu s AD, ujistěte se, že toocopy je. 
   
    **TenantId**, **SubscriptionId**, **ApplicationId**, a **tajný klíč**.
   
    Budete používat TenantId jako `{TENANT_ID}`, ApplicationId jako `{CLIENT_ID}` a tajný jako `{CLIENT_SECRET}`.
   
   > [!NOTE]
   > Výchozí zásady zabezpečení mohou blokovat spouštění skriptů prostředí PowerShell. Pokud ano, můžete dočasně konfiguraci vašeho provádění zásad tooallow spuštění skriptu pomocí hello následující příkaz:
   > 
   > Set-ExecutionPolicy RemoteSigned
   > 
   > 
8. Zde je, jak bude vypadat hello sadu rutin PS. 
   
    ![][3]
9. Na portálu pro správu Azure hello zkontrolujte, zda novou aplikaci AD byla vytvořena s názvem hello je zadaný skript toohello nazývá **principalName** pod **zobrazit aplikace Moje společnost vlastní**.
   
    ![][4]

#### <a name="steps-tooget-a-valid-token"></a>Kroky tooget platný token
1. Volání rozhraní API hello s hello následující parametry a ujistěte se, zda text hello tooreplace klienta\_ID, klient\_ID a klienta\_tajný klíč:
   
   * **Adresa URL požadavku** jako *https://login.microsoftonline.com/ {klienta\_ID} / oauth2/token*
   * **Hlavičku HTTP Content-Type** jako *application/x--www-form-urlencoded*
   * **Text žádosti HTTP** jako *udělit\_typ = klient\_přihlašovací údaje & client_id = {klienta\_ID} & tajný klíč client_secret = {klienta\_tajný klíč} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*
     
     Následující Hello je požadavek příklad:
     
       POST / {TENANT_ID} / oauth2/token hostitele HTTP/1.1: login.microsoftonline.com Content-Type: grant_type application/x--www-form-urlencoded = client_credentials & client_id = {CLIENT_ID} & tajný klíč client_secret = {tajný klíč CLIENT_SECRET} & Zdroj Př napětí = https % 3A % 2f%2Fmanagement.Core.Windows.NET%2f
     
     Tady je příklad odpověď:
     
       Protokol HTTP/1.1 200 OK Content-Type: application/json; charset = utf-8 Content-Length: 1234
     
       {"token_type": "Nosiče", "expires_in": "3599", "expires_on": "1445395811", "not_before": "144 5391911 prostředků","": "https://management.core.windows.net/", "access_token": {ACCESS_TOKEN}}
     
     Tento příklad zahrnuté kódování URL hello parametry POST, `resource` hodnota je ve skutečnosti `https://management.core.windows.net/`. Dávejte pozor, kódování tooalso URL `{CLIENT_SECRET}` jako může obsahovat speciální znaky.
     
     > [!NOTE]
     > Pro testování, můžete použít nástroj pro klienta na HTTP jako [Fiddler](http://www.telerik.com/fiddler) nebo [rozšíření Chrome Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 
     > 
     > 
2. Každé volání rozhraní API nyní obsahují hlavička požadavku hello autorizace:
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    Pokud se zobrazí kód stavu 401 vrátí, zkontrolujte hello odpovědi, ho vás může upozornit, že hello tokenu vypršela platnost. V takovém případě získáte nový token.

## <a name="using-hello-apis"></a>Pomocí hello rozhraní API
Teď, když máte platný token, se volání připravené toomake hello rozhraní API.

1. V každé žádosti o rozhraní API budete potřebovat toopass token platný, neprošlé, který jste získali v předchozí části hello.
2. Budete potřebovat tooplug v některé parametry do hello požadavek URI, který identifikuje vaši aplikaci. identifikátor URI požadavku Hello vypadá jako následující hello
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    Parametry hello tooget, klikněte na název vaší aplikace a klikněte na řídicí panel a na stránce se zobrazí, jako jsou následující hello se všemi hello 3 parametry.
   
   * **1** `{subscription-id}`
   * **2** `{app-collection}`
   * **3** `{app-resource-name}`
   * **4** název vaše skupiny prostředků bude toobe **MobileEngagement** Pokud jste vytvořili novou. 
     
     ![Mobile Engagement rozhraní API URI parametry][2]

> [!NOTE]
> <br/>
> 
> 1. Ignorovat hello kořenové adresy rozhraní API, jako to byla pro hello předchozí rozhraní API.<br/>
> 2. Pokud jste vytvořili aplikaci hello pomocí portálu Azure Classic budete potřebovat toouse hello prostředku aplikační název, který je jiný než název aplikace hello, sám sebe. Pokud jste vytvořili aplikaci hello v hello portálu Azure doporučujeme, abyste používali hello samotný (není žádný rozdíl mezi název prostředku aplikace a název aplikace pro aplikace vytvořené v nový portál hello) název aplikace.  
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



