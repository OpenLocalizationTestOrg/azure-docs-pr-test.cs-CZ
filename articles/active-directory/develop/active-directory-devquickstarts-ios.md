---
title: aaaIntegrate Azure AD do aplikace pro iOS | Microsoft Docs
description: "Tom, jak toobuild aplikace pro iOS, který se integruje s Azure AD pro přihlášení a volání služby Azure AD chráněný rozhraní API pomocí OAuth."
services: active-directory
documentationcenter: ios
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: 6e05745b2b2b122995dcba896ab0f2ed32509e3a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-an-ios-app"></a>Integrace Azure AD do aplikace pro iOS
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> Vyzkoušení verze preview hello naší nové [portál pro vývojáře](https://identity.microsoft.com/Docs/iOS) který vám pomůže spuštěná v Azure Active Directory v několika málo minut!  portál pro vývojáře Hello vás provede procesem hello registrace aplikace a integraci služby Azure AD do vašeho kódu.  Jakmile budete hotovi, budete mít jednoduchou aplikaci, která může ověřit uživatele v klientovi a back-end, které mohou přijímat tokeny a provést ověření. 
> 
> 

Azure Active Directory (Azure AD) poskytuje hello knihovna ověřování Active Directory nebo ADAL pro iOS klienti, kteří potřebují tooaccess chráněné zdroje. ADAL zjednodušuje proces hello, že vaše aplikace používá tooobtain přístupových tokenů. toodemonstrate jak snadné je v tomto článku jsme sestavit seznam úkolů Objective C aplikaci, která:

* Získá přístup k tokeny pro volání rozhraní API Azure AD Graph hello pomocí hello [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Vyhledá adresář pro uživatele s danou alias.

toobuild hello dokončení pracovní aplikace, budete muset:

1. Registrace vaší aplikace s Azure AD.
2. Nainstalujte a nakonfigurujte ADAL.
3. Pomocí ADAL tooget tokeny z Azure AD.

spuštění, tooget [stáhnout kostru aplikace hello](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) nebo [stažení ukázky hello Dokončit](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip). Musíte taky klient služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci. Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).


> [!TIP]
> Vyzkoušení verze preview hello naší nové [portál pro vývojáře](https://identity.microsoft.com/Docs/iOS) , umožňuje zprovoznění s Azure AD za několik minut. portál pro vývojáře Hello vás provede procesem hello registrace aplikace a integraci služby Azure AD do vašeho kódu. Až budete hotoví, budete mít jednoduchou aplikaci, která může ověřit uživatele ve vašem klientovi a back-end, můžete přijímat tokeny a provést ověření. 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a>1. Určit, jaké vaše přesměrování je identifikátor URI pro iOS
toosecurely spuštění aplikace v některých scénářích jednotné přihlašování, musíte vytvořit *identifikátor URI pro přesměrování* v určitém formátu. Přesměrování identifikátor URI je použité tooensure, který hello návratový toohello tokeny správné se aplikace, která je žádali.


Formát iOS Hello přesměrování je identifikátor URI:

```
<app-scheme>://<bundle-id>
```

* **aplikace – schéma** – to je zaregistrován ve vašem projektu XCode. Je, jak jiné aplikace může volat. Můžete najít to pod Info.plist -> adresa URL typy -> identifikátoru adresy URL. Pokud ještě nemáte jeden nebo více nakonfigurované byste měli vytvořit jednu.
* **id sady** -Toto je identifikátor svazku v části "identity" hello zrušit nastavení projektu v XCode.

Příklad pro tento kód rychlý start: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-hello-directorysearcher-application"></a>2. Registrace aplikace DirectorySearcher hello
tooset až tokeny tooget vaší aplikace, musíte nejprve tooregister ji ve službě Azure AD klienta a udělit mu oprávnění tooaccess hello Azure AD Graph API:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na horním panelu hello klikněte na váš účet. V části hello **Directory** vyberte místo, kam chcete tooregister klienta služby Active Directory hello vaší aplikace.
3. Klikněte na tlačítko **více služeb** v hello levém navigačním podokně a pak vyberte **Azure Active Directory**.
4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.
5. Postupujte podle hello vyzve toocreate nový **nativní klientská aplikace**.
  * Hello **název** z hello aplikace popisuje tooend uživatelů vaší aplikace.
  * Hello **identifikátor Uri pro přesměrování** schématu a řetězec kombinací, Azure AD používá tooreturn odpovědi tokenu.  Zadejte hodnotu, která je konkrétní tooyour aplikace která je založena na hello předchozí informace o identifikátor URI přesměrování.
6. Po dokončení registrace hello, Azure AD přiřadí vaší aplikace ID jedinečný aplikace.  Tuto hodnotu budete potřebovat v dalších částech hello, takže zkopírujte jej z karty aplikace hello.
7. Z hello **nastavení** vyberte **požadovaných oprávnění** a pak vyberte **přidat**. Vyberte **Microsoft Graph** jako hello rozhraní API a poté přidejte hello **čtení dat adresáře** oprávnění v rámci **delegovaná oprávnění**.  Toto nastaví vaší hello tooquery aplikace Azure AD Graph API pro uživatele.

## <a name="3-install-and-configure-adal"></a>3. Instalace a konfigurace ADAL
Teď, když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.  ADAL toocommunicate s Azure AD, musíte tooprovide její některé informace o registraci vaší aplikace.

1. Začněte tím, že přidání ADAL toohello DirectorySearcher projektu pomocí CocoaPods.

    ```
    $ vi Podfile
    ```
2. Přidejte následující toothis podfile hello:

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. Nyní načtěte hello podfile pomocí CocoaPods. Tento krok vytvoří nový pracovní prostor XCode, který můžete načíst.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. V projektu typu rychlý start hello, otevřete soubor plist hello `settings.plist`.  Nahraďte hodnoty hello hello elementů v hello části tooreflect hello hodnoty, které jste zadali v hello portálu Azure. Váš kód odkazuje na tyto hodnoty vždy, když ho využívá ADAL.
  * Hello `tenant` je hello domény klienta služby Azure AD, například contoso.onmicrosoft.com.
  * Hello `clientId` je hello ID klienta aplikace, který jste zkopírovali z portálu hello.
  * Hello `redirectUri` je hello přesměrování URL, která jste zaregistrovali hello portálu.

## <a name="4----use-adal-tooget-tokens-from-azure-ad"></a>4.    Použití ADAL tooget tokeny z Azure AD
Hello základní princip za ADAL je, že vždy, když aplikace potřebuje přístupový token, jednoduše volá completionBlock `+(void) getToken : `, a ADAL hello rest.  

1. V hello `QuickStart` projekt, otevřete `GraphAPICaller.m` a vyhledejte hello `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` komentář v horní hello.  Toto je, kde můžete předat ADAL hello souřadnice prostřednictvím CompletionBlock, toocommunicate s Azure AD a určit, jak toocache tokeny.

    ```ObjC
    +(void) getToken : (BOOL) clearCache
               parent:(UIViewController*) parent
    completionHandler:(void (^) (NSString*, NSError*))completionBlock;
    {
        AppData* data = [AppData getInstance];
        if(data.userItem){
            completionBlock(data.userItem.accessToken, nil);
            return;
        }

        ADAuthenticationError *error;
        authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
        authContext.parentController = parent;
        NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:data.resourceId
                                     clientId:data.clientId
                                  redirectUri:redirectUri
                               promptBehavior:AD_PROMPT_AUTO
                                       userId:data.userItem.userInformation.userId
                        extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy toodisplay hello correct mobile UX. You most likely won't need it in your code.
                             completionBlock:^(ADAuthenticationResult *result) {

                                  if (result.status != AD_SUCCEEDED)
                                  {
                                     completionBlock(nil, result.error);
                                  }
                                  else
                                  {
                                      data.userItem = result.tokenCacheStoreItem;
                                      completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                                  }
                             }];
    }

    ```

2. Nyní potřebujeme toouse tento token toosearch pro uživatele v grafu hello. Najde hello `// TODO: implement SearchUsersList` komentář. Tato metoda vytváří tooquery toohello Azure AD Graph API požadavek GET pro uživatele, jehož UPN začíná hello zadaný hledaný termín.  tooquery hello Azure AD Graph API, musíte tooinclude access_token v hello `Authorization` hlavičky požadavku hello. Toto je, kde odeslán ADAL.

    ```ObjC
    +(void) searchUserList:(NSString*)searchString
                    parent:(UIViewController*) parent
          completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
    {
        if (!loadedApplicationSettings)
       {
            [self readApplicationSettings];
        }
        
        AppData* data = [AppData getInstance];

        NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];

        [self craftRequest:[self.class trimString:graphURL]
                    parent:parent
         completionHandler:^(NSMutableURLRequest *request, NSError *error) {

             if (error != nil)
             {
                 completionBlock(nil, error);
             }
             else
             {

                 NSOperationQueue *queue = [[NSOperationQueue alloc]init];

                 [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                     if (error == nil && data != nil){

                         NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                         // We can grab hello JSON node at hello top tooget our graph data.
                         NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                         // Don't be thrown off by hello key name being "value". It really is hello name of the
                         // first node. :-)

                         // Each object is a key value pair
                         NSDictionary *keyValuePairs;
                         NSMutableArray* Users = [[NSMutableArray alloc]init];

                         for(int i =0; i < graphDataArray.count; i++)
                         {
                             keyValuePairs = [graphDataArray objectAtIndex:i];

                             User *s = [[User alloc]init];
                             s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                             s.name =[keyValuePairs valueForKey:@"givenName"];

                             [Users addObject:s];
                         }

                         completionBlock(Users, nil);
                     }
                     else
                     {
                         completionBlock(nil, error);
                     }

                }];
             }
         }];

    }

    ```


3. Když vaše aplikace vyžaduje token voláním `getToken(...)`, ADAL pokusí tooreturn token bez nutnosti hello uživatelské přihlašovací údaje.  Pokud ADAL zjistí, že tento uživatel hello je toosign v tooget token, bude zobrazit dialogové okno pro přihlášení, shromažďování přihlašovacích údajů uživatele hello a vrátíte se po úspěšném ověření tokenu.  Pokud ADAL není možné tooreturn token z jakéhokoli důvodu, že nastane `AdalException`.

> [!Note] 
> Hello `AuthenticationResult` objekt obsahuje `tokenCacheStoreItem` objekt, který lze použít toocollect hello informace, které může být nutné vaší aplikace. V hello rychlý Start `tokenCacheStoreItem` je použité toodetermine Pokud ověřování již probíhá.
>
>

## <a name="5-build-and-run-hello-application"></a>5. Sestavení a spuštění aplikace hello
Blahopřejeme! Teď máte funkční aplikaci iOS, která můžete ověřovat uživatele, bezpečně volání webového rozhraní API pomocí standardu OAuth 2.0 a získat základní informace o uživateli hello.  Pokud jste to ještě neudělali, nyní je čas toopopulate hello vašeho klienta s některými uživateli.  Spusťte aplikaci rychlý start a pak se přihlaste pomocí jeden z těchto uživatelů.  Hledání jiných uživatelů podle jejich UPN.  Zavření aplikace hello a pak spusťte znovu.  Všimněte si, že hello uživatelské relace zůstává beze změn.

ADAL umožňuje snadno tooincorporate všechny tyto běžné funkce identity do aplikace.  Se postará všechny pracovní dirty hello, jako je Správa mezipaměti podpora protokolu OAuth, prezentuje toosign uživatelského rozhraní v hello uživatele a aktualizaci vypršení platnosti tokenů.  Všechny skutečně potřebujete tooknow je jednoho volání rozhraní API `getToken`.

Pro odkaz, hello dokončit ukázka (bez vašich hodnot nastavení) zajišťuje na [Githubu](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="next-steps"></a>Další kroky
Nyní se můžete přesunout na tooadditional scénáře.  Může být vhodné tootry:

* [Zabezpečení webové aplikace Node.JS API s Azure AD](active-directory-devquickstarts-webapi-nodejs.md)
* Další informace [jak tooenable jednotného přihlašování napříč aplikacemi v systému iOS pomocí ADAL](active-directory-sso-ios.md)  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

