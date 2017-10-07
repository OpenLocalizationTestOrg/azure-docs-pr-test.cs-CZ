---
title: aaaHow tooUse iOS SDK pro Azure Mobile Apps
description: Jak tooUse iOS SDK pro Azure Mobile Apps
services: app-service\mobile
documentationcenter: ios
author: ysxu
manager: yochayk
editor: 
ms.assetid: 4e8e45df-c36a-4a60-9ad4-393ec10b7eb9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: yuaxu
ms.openlocfilehash: fa299ab3f152bad12d821832fa9fb5495d1fa296
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-ios-client-library-for-azure-mobile-apps"></a>Jak tooUse iOS Klientská knihovna pro Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Tato příručka je určena tooperform běžné scénáře s využitím hello nejnovější [Azure Mobile Apps iOS SDK][1]. Pokud jste nový tooAzure Mobile Apps, nejprve dokončit [Azure Mobile Apps rychlý Start] toocreate back-end, vytvořit tabulku a stažení projektu Xcode předdefinovaných iOS. V této příručce se zaměříme na hello klienta iOS SDK. toolearn Další informace o hello serverové SDK pro back-end hello najdete v tématu hello Server SDK HOWTOs.

## <a name="reference-documentation"></a>Referenční dokumentace
Hello referenční dokumentaci k nástroji pro hello iOS klienta SDK se nachází zde: [Azure Mobile Apps iOS odkaz klienta][2].

## <a name="supported-platforms"></a>Podporované platformy
Hello iOS SDK podporuje jazyka Objective-C projekty – projekty Swift 2.2 a Swift 2.3 projekty pro systém iOS verze 8.0 nebo novější.

ověřování "serveru pohybu" Hello používá webové zobrazení pro hello uvedené uživatelského rozhraní.  Pokud hello zařízení není možné toopresent webové zobrazení uživatelského rozhraní, se vyžaduje jinou metodu ověřování, který je mimo rozsah hello hello produktu.  
Tato sada SDK není proto vhodný pro sledování type nebo podobně jako zařízení s omezeným přístupem.

## <a name="Setup"></a>Instalační program a požadavky
Tato příručka předpokládá, že jste vytvořili back-end s tabulkou. Tato příručka předpokládá, že tato tabulka hello má stejné schéma jako hello tabulky v těchto kurzech. Tento průvodce také předpokládá, že ve vašem kódu odkazujete `MicrosoftAzureMobile.framework` a import `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

## <a name="create-client"></a>Postupy: vytvoření klienta
Vytvoření tooaccess back-end Azure Mobile Apps ve vašem projektu, `MSClient`. Nahraďte `AppUrl` se adresa URL aplikace hello. Můžete ponechat `gatewayURLString` a `applicationKey` prázdný. Pokud jste nastavili bránu pro ověřování, naplnit `gatewayURLString` s adresou URL hello brány.

**Jazyka Objective-C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Kód SWIFT**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


## <a name="table-reference"></a>Postupy: vytvoření odkaz na tabulku
tooaccess nebo aktualizace dat, vytvořit tabulku odkaz toohello back-end. Nahraďte `TodoItem` s názvem hello tabulky

**Jazyka Objective-C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Kód SWIFT**:

```
let table = client.tableWithName("TodoItem")
```


## <a name="querying"></a>Postupy: dotaz na Data
toocreate dotaz na databázi dotazu hello `MSTable` objektu. Hello následující dotaz načte všechny položky hello ve `TodoItem` a protokoly hello text jednotlivých položek.

**Jazyka Objective-C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Kód SWIFT**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="filtering"></a>Postupy: Filtr nevrátil Data
výsledky toofilter celá řada možností k dispozici.

toofilter pomocí predikátu, použijte `NSPredicate` a `readWithPredicate`. Následující Hello filtrování položek Todo pouze nedokončené toofind vrácená data.

**Jazyka Objective-C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query hello TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Kód SWIFT**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query hello TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="query-object"></a>Postupy: použití MSQuery
tooperform vytvoření složitých dotazů (včetně řazení a stránkování), `MSQuery` objektu, přímo nebo pomocí predikátu:

**Jazyka Objective-C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Kód SWIFT**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`můžete nastavit různé chování dotazu.

* Určení pořadí výsledků
* Omezení, která pole tooreturn
* Omezit, kolik zaznamenává tooreturn
* Zadejte celkový počet v odpovědi
* Zadejte parametry řetězec vlastního dotazu v žádosti
* Použít další funkce

Spuštění `MSQuery` dotazu voláním `readWithCompletion` hello objektu.

## <a name="sorting"></a>Postupy: řazení dat s MSQuery
výsledky toosort, podíváme se na příklad. vyvolání toosort ve vzestupném pole 'text' a pak podle 'dokončení' sestupném `MSQuery` takto:

**Jazyka Objective-C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Kód SWIFT**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>Postupy: omezení polí a parametrů řetězce dotazu s MSQuery rozbalte
toobe pole toolimit vrácených dotazem, zadejte hello názvy polí hello v hello **selectFields** vlastnost. Tento příklad vrátí jenom hello text a dokončené pole:

**Jazyka Objective-C**:

```
query.selectFields = @[@"text", @"complete"];
```

**Kód SWIFT**:

```
query.selectFields = ["text", "complete"]
```

parametrů řetězce dotazu další tooinclude hello serveru požadavku (například, protože je používá vlastní skript na straně serveru), naplnit `query.parameters` takto:

**Jazyka Objective-C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Kód SWIFT**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Postupy: Konfigurace velikost stránky
S Azure Mobile Apps ovládací prvky velikost stránky hello hello počet záznamů, které jsou vyžádány současně z tabulek hello back-end. A volání příliš`pull` dat by pak dávky dat, podle této velikosti stránky, dokud nejsou žádné další toopull záznamy.

Je možné tooconfigure velikost stránky pomocí **MSPullSettings** jak je uvedeno níže. Výchozí velikost stránky Hello je 50 a hello příklad změny too3.

Můžete nakonfigurovat velikost různé stránky z důvodů výkonu. Pokud máte velké množství malých datových záznamů, snižuje velikost vysoké stránky hello dobu odezvy serveru.

Toto nastavení řídí pouze velikost hello stránky na straně klienta hello. Hello klient požaduje větší velikost stránky než back-end mobilní aplikace hello podporuje, je velikost stránky hello limitován hello maximální hello back-end je nakonfigurované toosupport.

Toto nastavení je také hello *číslo* datových záznamů, není hello *velikost v bajtech*.

Pokud zvýšíte velikost stránky hello klienta, měli také zvýšit velikost stránky hello na serveru hello. V tématu ["postup: Upravit velikost stránkování tabulky hello"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) pro hello kroky toodo to.

**Jazyka Objective-C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```


**Kód SWIFT**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <a name="inserting"></a>Postupy: vkládání dat
Vytvoření tooinsert řádku nové tabulky `NSDictionary` a vyvolání `table insert`. Pokud [dynamické schématu] je povoleno, hello Azure App Service mobilního back-endu automaticky vygeneruje nové sloupce podle hello `NSDictionary`.

Pokud `id` není uvedeno, back-end hello generuje automaticky nový jedinečný identifikátor. Zadejte vlastní `id` toouse e-mailové adresy, uživatelská jména nebo vlastní hodnoty jako ID. Poskytnutí vlastní ID může usnadňují spojení a logiku orientovanými na firmu databáze.

Hello `result` obsahuje hello novou položku, která byla vložena. V závislosti na logice server může mít další nebo upravených dat porovnání toowhat byl předán toohello serveru.

**Jazyka Objective-C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Kód SWIFT**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

## <a name="modifying"></a>Postupy: Změna dat
Upravit tooupdate stávající řádek, položku a volání `update`:

**Jazyka Objective-C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Kód SWIFT**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Můžete taky zadejte ID řádku hello a hello aktualizovat pole:

**Jazyka Objective-C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Kód SWIFT**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Minimálně, hello `id` musí být nastaven při provádění aktualizací.

## <a name="deleting"></a>Postupy: odstranění dat
vyvolání toodelete některou položku, `delete` s položkou hello:

**Jazyka Objective-C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Kód SWIFT**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Můžete taky odstraňte tím, že poskytuje ID řádku:

**Jazyka Objective-C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Kód SWIFT**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Minimálně, hello `id` musí být nastaven při provádění odstraní.

## <a name="customapi"></a>Postupy: volání vlastního rozhraní API
Pomocí vlastního rozhraní API můžou zpřístupnit žádné funkce back-end. Operace toomap tooa tabulka nemá. Nejen získáte větší kontrolu nad zasílání zpráv, můžete dokonce i pro čtení nebo nastavení hlavičky a změňte formát textu hello odpovědi. jak toocreate vlastní rozhraní API na back-end hello, číst toolearn [vlastní rozhraní API](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

volání toocall vlastní rozhraní API, `MSClient.invokeAPI`. Hello žádosti a odpovědi obsahu jsou považovány za JSON. toouse jiné typy médií [hello použijte jiné přetížení `invokeAPI` ] [ 5].  toomake `GET` žádosti o místo `POST` požádat, parametr sady `HTTPMethod` příliš`"GET"` a parametr `body` příliš`nil` (protože požadavky GET ještě nemá obsah zpráv.) Pokud vaše vlastní rozhraní API podporuje další příkazy HTTP, změňte `HTTPMethod` správně.

**Jazyka Objective-C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Kód SWIFT**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

## <a name="templates"></a>Postupy: registrace nabízených šablony toosend napříč platformami oznámení
šablony tooregister předat šablon s vaší **client.push registerDeviceToken** metoda ve vaší klientské aplikace.

**Jazyka Objective-C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Kód SWIFT**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Vaše šablony typu NSDictionary a může obsahovat několik šablon v hello následující formát:

**Jazyka Objective-C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Kód SWIFT**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Všechny značky se odstraní z hello požadavku pro zabezpečení.  tooadd značky tooinstallations nebo šablony v rámci instalace najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps][4].  Pomocí těchto registrovaných šablon oznámení toosend pracovat s [rozhraní API centra oznámení][3].

## <a name="errors"></a>Postupy: zpracování chyb
Při volání Azure App Service mobilního back-endu blok dokončení hello obsahuje `NSError` parametr. Když dojde k chybě, tento parametr je nemá hodnotu nil. V kódu zkontrolujte tento parametr a zpracování chyb hello podle potřeby, jak je předvedeno v hello předchozích fragmentů kódu.

soubor Hello [ `<WindowsAzureMobileServices/MSError.h>` ] [ 6] definuje hello konstanty `MSErrorResponseKey`, `MSErrorRequestKey`, a `MSErrorServerItemKey`. tooget další data související s toohello Chyba:

**Jazyka Objective-C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Kód SWIFT**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Kromě toho hello soubor definuje konstanty pro každý kód chyby:

**Jazyka Objective-C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**Kód SWIFT**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Postupy: ověřuje uživatele pomocí hello knihovna ověřování Active Directory
Můžete vytvořit hello Active Directory Authentication Library (ADAL) toosign uživatelů do vaší aplikace pomocí Azure Active Directory. Tok ověřování klientů pomocí zprostředkovatele identity SDK je vhodnější toousing hello `loginWithProvider:completion:` metoda.  Tok ověření klienta poskytuje více nativní UX chování a umožňuje další přizpůsobení.

1. Nakonfigurujte následující hello váš back-end mobilní aplikace při přihlášení AAD [jak tooconfigure aplikaci služby pro služby Active Directory přihlášení] [ 7] kurzu. Ujistěte se, že toocomplete hello volitelný krok registrace nativní klientskou aplikaci. Pro iOS, doporučujeme, abyste tento hello přesměrování je identifikátor URI hello formuláře `<app-scheme>://<bundle-id>`. Další informace najdete v tématu hello [rychlý start ADAL iOS][8].
2. Nainstalujte ADAL pomocí Cocoapods. Upravit vaše hello tooinclude Podfile následující definice, nahraďte **YOUR PROJECT** s názvem projektu Xcode hello:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   a hello Pod:

        pod 'ADALiOS'
3. Pomocí hello terminálu spustit `pod install` z adresáře hello obsahující projekt a pak otevřete pracovní prostor Xcode hello generované (ne hello projekt).
4. Přidejte následující kód tooyour aplikace, podle toohello jazyk, který používáte hello. V každé udělejte tyto náhrady:

   * Nahraďte **INSERT. AUTORITY zde** s názvem hello hello klienta, ve kterém jste zřídili vaší aplikace. Formát by měl být https://login.microsoftonline.com/contoso.onmicrosoft.com. Tuto hodnotu můžete zkopírovat z karty hello domény v Azure Active Directory v hello [portál Azure classic].
   * Nahraďte **INSERT-RESOURCE-ID-zde** s ID klienta hello back-endu mobilní aplikace. ID klienta můžete získat z hello **Upřesnit** v části **nastavení Azure Active Directory** hello portálu.
   * Nahraďte **INSERT klienta ID zde** s ID klienta hello jste zkopírovali ze hello nativní klientskou aplikaci.
   * Nahraďte **vložení PŘESMĚROVÁNÍ URI zde** s vaší lokality */.auth/login/done* koncový bod, pomocí hello schéma HTTPS. Tato hodnota by mělo být podobné příliš*https://contoso.azurewebsites.net/.auth/login/done*.

**Jazyka Objective-C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Kód SWIFT**:

    // add hello following imports tooyour bridging header:
    //        #import <ADALiOS/ADAuthenticationContext.h>
    //        #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>Postupy: ověřuje uživatele pomocí hello Facebook SDK pro iOS
Hello Facebook SDK můžete použít pro uživatele iOS toosign do vaší aplikace pomocí služby Facebook.  Použití tok ověření klienta, je vhodnější toousing hello `loginWithProvider:completion:` metoda.  Hello klienta tok ověřování poskytuje více nativní UX chování a umožňuje další přizpůsobení.

1. Konfigurace váš back-end mobilní aplikace pro Facebook přihlásit pomocí následujících [jak tooconfigure služby aplikace pro Facebook přihlášení] [ 9] kurzu.
2. Nainstalujte hello Facebook SDK pro iOS pomocí následující hello [Facebook SDK pro iOS – Začínáme] [ 10] dokumentaci. Místo vytváření aplikace, můžete přidat hello iOS platformy tooyour existující registraci.
3. Dokumentace pro Facebook obsahuje nějaký kód jazyka Objective-C v hello delegáta aplikace. Pokud používáte **Swift**, můžete použít následující překladů pro AppDelegate.swift hello:

        // Add hello following import tooyour bridging header:
        //        #import <FBSDKCoreKit/FBSDKCoreKit.h>

        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }
4. V přidání tooadding `FBSDKCoreKit.framework` tooyour projektu, také přidat odkaz příliš`FBSDKLoginKit.framework` v hello stejným způsobem.
5. Přidejte následující kód tooyour aplikace, podle toohello jazyk, který používáte hello.

**Jazyka Objective-C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {        
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Kód SWIFT**:

    // Add hello following imports tooyour bridging header:
    //        #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //        #import <FBSDKCoreKit/FBSDKAccessToken.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>Postupy: ověřuje uživatele pomocí služby Twitter prostředků infrastruktury pro iOS
Prostředky infrastruktury můžete použít pro uživatele iOS toosign do vaší aplikace pomocí služby Twitter. Tok ověření klienta je vhodnější toousing hello `loginWithProvider:completion:` metoda, protože poskytuje více nativní UX chování a umožňuje další přizpůsobení.

1. Nakonfigurujte následující hello váš back-end mobilní aplikace při přihlášení Twitter [jak tooconfigure aplikaci služby pro přihlášení služby Twitter](app-service-mobile-how-to-configure-twitter-authentication.md) kurzu.
2. Projekt tooyour prostředků infrastruktury můžete přidat následující hello [prostředků infrastruktury pro iOS – Začínáme] dokumentace a nastavení TwitterKit.

   > [!NOTE]
   > Ve výchozím nastavení prostředků infrastruktury pro vás vytvoří aplikaci služby Twitter. Vytváření aplikací tak, že zaregistrujete hello uživatelský klíč a tajný klíč příjemce, které jste vytvořili dříve pomocí hello následující fragmenty kódu se můžete vyhnout.    Alternativně můžete nahradit hello uživatelský klíč a zadejte tooApp služby s hello hodnoty můžete hodnoty uživatelský tajný klíč, najdete v části v hello [řídicí panel infrastruktury]. Pokud zvolíte tuto možnost, být jisti tooset hello zpětného volání adresy URL tooa hodnotu zástupného symbolu, jako třeba `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.
   >
   >

    Pokud si zvolíte toouse hello tajné klíče, které jste vytvořili dříve, přidejte následující kód tooyour delegáta aplikace hello:

    **Jazyka Objective-C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }

    **Kód SWIFT**:

        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
3. Přidejte následující kód tooyour aplikace, podle toohello jazyk, který používáte hello.

**Jazyka Objective-C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Kód SWIFT**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>Postupy: ověřuje uživatele pomocí hello Google přihlášení SDK pro iOS
Hello Google přihlášení SDK můžete použít pro uživatele iOS toosign do vaší aplikace pomocí účtu Google.  Google nedávno vydal zásady zabezpečení OAuth tootheir změny.  Tyto změny zásad bude vyžadovat použití hello Google SDK v budoucnu hello.

1. Nakonfigurujte následující hello váš back-end mobilní aplikace pro Google přihlásit [jak tooconfigure aplikaci služby pro Google přihlášení](app-service-mobile-how-to-configure-google-authentication.md) kurzu.
2. Nainstalujte hello Google SDK pro iOS pomocí následující hello [přihlášení Google pro iOS – spuštění integrace](https://developers.google.com/identity/sign-in/ios/start-integrating) dokumentaci. Část "Ověřit s back-end Server" hello, můžete vynechat.
3. Přidejte následující tooyour delegáta hello `signIn:didSignInForUser:withError:` metoda, podle toohello jazyk používáte.

**Jazyka Objective-C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };

        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Kód SWIFT**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

1. Ujistěte se také přidat hello následující příliš`application:didFinishLaunchingWithOptions:` ve vaší aplikaci delegovat, nahraďte "SERVER_CLIENT_ID" hello stejné ID této jste použili tooconfigure služby App Service v kroku 1.

**Jazyka Objective-C**:

         [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";

 **Kód SWIFT**:

        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"


1. Přidejte následující kód tooyour aplikace v UIViewController, který implementuje hello hello `GIDSignInUIDelegate` protokol, podle toohello jazyk používáte.  Jste se odhlásili před se přihlášení znovu a i když není nutné tooenter vaše přihlašovací údaje znovu, se zobrazí dialogové okno souhlasu.  Tuto metodu volejte pouze hello relace tokenu vypršela.

   **Jazyka Objective-C**:

       #import <Google/SignIn.h>
       // ...
       - (void)authenticate
       {
               [GIDSignIn sharedInstance].uiDelegate = self;
               [[GIDSignIn sharedInstance] signOut];
               [[GIDSignIn sharedInstance] signIn];
        }

   **Kód SWIFT**:

       // ...
       func authenticate() {
           GIDSignIn.sharedInstance().uiDelegate = self
           GIDSignIn.sharedInstance().signOut()
           GIDSignIn.sharedInstance().signIn()
       }

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create hello Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data toohello user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize hello client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure Mobile Apps rychlý Start]: app-service-mobile-ios-get-started.md

[Add Mobile Services tooExisting App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts tooauthorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[dynamické schématu]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI toomanage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[řídicí panel infrastruktury]: https://www.fabric.io/home
[prostředků infrastruktury pro iOS – Začínáme]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
