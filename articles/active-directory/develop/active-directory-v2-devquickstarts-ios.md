---
title: "aaaAdd přihlášení tooan iOS aplikace pomocí hello koncového bodu v2.0 Azure AD | Microsoft Docs"
description: "Jak toobuild aplikaci pro iOS, přihlášení uživatele pomocí obou osobního účtu Microsoft a pracovní nebo školní účty pomocí knihovny třetích stran."
services: active-directory
documentationcenter: 
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: fd3603c0-42f7-438c-87b5-a52d20d6344b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: a384062e6e4bd398a2b12318800728e627e05c32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-ios-app-using-a-third-party-library-with-graph-api-using-hello-v20-endpoint"></a>Přidat aplikaci iOS tooan přihlášení pomocí rozhraní Graph API pomocí koncového bodu v2.0 hello knihovně třetích stran
Platforma identity Microsoft Hello používá otevřete standardy, jako je OAuth2 a OpenID Connect. Vývojáři mohou použít žádnou knihovnu chtějí toointegrate s našich služeb. Vývojáři toohelp používat naše platforma s další knihovny, jsme jste zapsána několik návody jako tento jeden toodemonstrate jak tooconfigure třetích stran knihovny tooconnect toohello Microsoft identity platformy. Většina knihovny, které implementují [specifikace hello RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) můžete připojení toohello Microsoft identity platformy.

Hello aplikaci, která vytváří Tento názorný postup mohou uživatelé přihlásit tootheir organizace a poté vyhledejte ostatními v organizaci pomocí hello rozhraní Graph API.

Pokud jste nový tooOAuth2 nebo OpenID Connect, mnohem této ukázkové konfigurace nemusí mít smysl tooyou. Doporučujeme, abyste si přečetli [v2.0 protokoly - toku OAuth 2.0 autorizační kód](active-directory-v2-protocols-oauth-code.md) pozadí.

> [!NOTE]
> Některé funkce naše platformy, které mají výraz v hello OAuth2 nebo OpenID Connect standardy, jako je například podmíněný přístup a správu zásad Intune, vyžadují jste toouse naše s otevřeným zdrojem Identity pro knihovny Microsoft Azure.
> 
> 

koncový bod v2.0 Hello nepodporuje všechny scénáře Azure Active Directory a funkce.

> [!NOTE]
> toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 
> 

## <a name="download-code-from-github"></a>Stáhněte si kód z Githubu
Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).  toofollow společně, můžete [stáhnout kostru aplikace hello jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) nebo hello kostru klonovat:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Můžete také právě stažení ukázky hello a rovnou začít:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Registrace aplikace
Vytvoření nové aplikace v hello [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle hello podrobné kroky v [jak tooregister aplikace s koncovým bodem v2.0 hello](active-directory-v2-app-registration.md).  Zkontrolujte, že:

* Kopírování hello **Id aplikace** aplikace přiřazené tooyour důvodem je, že ho budete potřebovat brzy k dispozici.
* Přidat hello **Mobile** platformu pro vaši aplikaci.
* Kopírování hello **identifikátor URI pro přesměrování** z portálu hello. Musíte použít výchozí hodnotu hello `urn:ietf:wg:oauth:2.0:oob`.

## <a name="download-hello-third-party-nxoauth2-library-and-create-a-workspace"></a>Stažení hello třetích stran NXOAuth2 knihovny a vytvořit pracovní prostor
V tomto návodu budete používat hello OAuth2Client z Githubu, která je již OAuth2 knihovny pro Mac OS X a iOS (kakao a kakao touch). Tato knihovna je založena na koncept 10 specifikace hello OAuth2. Profil nativní aplikace hello implementuje a podporuje koncový bod autorizace hello hello uživatele. To je vše, co hello budete potřebovat toointegrate s platformou identity Microsoft hello.

### <a name="add-hello-library-tooyour-project-by-using-cocoapods"></a>Přidejte hello knihovně tooyour projekt pomocí CocoaPods
CocoaPods je správce závislostí pro projekty Xcode. Automaticky spravuje hello předchozí kroky instalace.

```
$ vi Podfile
```
1. Přidejte následující toothis podfile hello:
   
    ```
     platform :ios, '8.0'
   
     target 'QuickStart' do
   
     pod 'NXOAuth2Client'
   
     end
    ```
2. Načtěte hello podfile pomocí CocoaPods. Tím se vytvoří nový pracovní prostor Xcode, který načtete.
   
    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-hello-structure-of-hello-project"></a>Prozkoumejte hello struktura hello projektu
pro naše projekt v kostře hello je nastavit Hello strukturu:

* Hlavní zobrazení s UPN vyhledávání
* Zobrazení podrobností pro hello data o hello vybraného uživatele
* Přihlášení zobrazení, kde uživatel může přihlásit toohello aplikace tooquery hello grafu

Přesune jsme toovarious soubory v hello kostru tooadd ověřování. Ostatní části hello kódu, třeba hello visual kódu, se netýkají tooidentity ale k dispozici máte.

## <a name="set-up-hello-settingsplst-file-in-hello-library"></a>Nastavení souboru settings.plst hello v hello knihovně
* V projektu typu rychlý start hello, otevřete hello `settings.plist` souboru. Nahraďte hodnoty hello hello elementů v hello části tooreflect hello hodnoty, které jste použili v hello portálu Azure. Váš kód bude odkazovat tyto hodnoty vždy, když používá hello knihovna ověřování Active Directory.
  * Hello `clientId` je hello ID klienta aplikace, který jste zkopírovali z portálu hello.
  * Hello `redirectUri` je adresa URL přesměrování hello tohoto hello portálu zadat.

## <a name="set-up-hello-nxoauth2client-library-in-your-loginviewcontroller"></a>Nastavit hello knihovně NXOAuth2Client ve vašem LoginViewController
Knihovna NXOAuth2Client Hello vyžaduje tooget některé hodnoty nastavení. Po dokončení této úlohy můžete použít hello získal token toocall hello rozhraní Graph API. Protože `LoginView` bude volána kdykoli potřebujeme tooauthenticate, hodnoty konfigurace tooput dává smysl v souboru toothat.

* Přidejme některé hodnoty toohello `LoginViewController.m` souboru tooset hello kontext pro ověřování a autorizaci. Podrobnosti o hodnotách hello podle kódu hello.
  
    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Podívejme se na podrobnosti o hello kódu.

První řetězec Hello je pro `scopes`.  Hello `User.Read` hodnota vám umožní tooread hello základní profil hello přihlášení uživatele.

Další informace o všech dostupných oborů hello v [obory oprávnění Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes).

Pro `authURL`, `loginURL`, `bhh`, a `tokenURL`, měli byste použít hodnoty hello uvedeny dříve. Pokud používáte hello knihovny Identity Microsoft Azure s otevřeným zdrojem, jsme stahují tato data můžete pomocí našeho koncový bod metadat. Máme jste Hotovo hello náročné práce extrahování tyto hodnoty pro vás.

Hello `keychain` hodnota je hello kontejner, který hello knihovně NXOAuth2Client použije toocreate toostore řetězce klíčů vašich tokenů. Pokud chcete tooget jednotné přihlašování (SSO) napříč více aplikací, můžete zadat hello stejné řetězce klíčů v každé z vašich aplikací a požádat o použití tohoto řetězce klíčů ve vašem prostředí Xcode nárocích hello. To je vysvětleno v hello dokumentaci od společnosti Apple.

Hello zbytek tyto hodnoty jsou požadované toouse hello knihovně a vytvořit míst pro vás toocarry hodnoty toohello kontext.

### <a name="create-a-url-cache"></a>Vytvoření mezipaměti se adresa URL
Uvnitř `(void)viewDidLoad()`, která je volána vždy po načtení hello zobrazení, hello následující kód primes mezipaměti pro naše používání.

Přidejte následující kód hello:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>Vytvořit webové zobrazení pro přihlášení
Webové zobrazení můžete hello uživatele vyzve k dalších faktorů, jako je SMS textová zpráva (Pokud je nakonfigurovaná), nebo může vracet chybové zprávy toohello uživatele. Tady budete nastavíte hello webové zobrazení a potom novější hello zápisu zpětná kód toohandle hello volání, které bude provedena v hello webové zobrazení z hello identity služby.

```objc
-(void)requestOAuth2Access {
    //toosign in tooMicrosoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate toohello URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-hello-webview-methods-toohandle-authentication"></a>Přepsání hello webové zobrazení metody toohandle ověřování
tootell hello webového zobrazení, co se stane, když uživatel potřebuje toosign v, jak je popsáno dříve, můžete vložit hello následující kód.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get hello auth token from a redirect so we need toohandle that in hello webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // hello webview is where all hello communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if hello UIWebView is showing our authorization URL or consent URL, show hello UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide hello UIWebView, we've left hello authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide hello UIWebView, we've left hello authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read hello Location from hello UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by hello redirect URL we chose toouse from Microsoft APIs
        //continue hello OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-toohandle-hello-result-of-hello-oauth2-request"></a>Zápis kódu toohandle hello výsledek požadavku OAuth2 hello
Hello následující kód bude zpracovávat hello redirectURL, který vrací z hello webové zobrazení. Pokud ověření nebylo úspěšné, pokusí znovu hello kódu. Mezitím hello knihovně poskytne hello chybu, která můžete zobrazit v konzole hello nebo asynchronní zpracování.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse hello response for success or failure
     if (accessResult)
    //if success, complete hello OAuth2 flow by handling hello redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-hello-oauth-context-called-account-store"></a>Nastavit hello OAuth kontextu (označovanou jako účet úložiště)
Zde můžete volat `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` na hello sdíleného účtu úložiště pro každou službu, které chcete mít tooaccess toobe hello aplikace. Typ účtu Hello je řetězec, který se používá jako identifikátor pro určité služby. Protože přistupujete hello rozhraní Graph API, kód hello odkazuje tooit jako `"myGraphService"`. Potom nastavíte pozorovatele při nic změnu s tokenem hello. Po získání tokenu hello vrátíte zpět toohello hello uživatele `masterView`.

```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-hello-master-view-toosearch-and-display-hello-users-from-hello-graph-api"></a>Nastavení hello toosearch hlavní zobrazení a zobrazení hello uživatele z hello rozhraní Graph API
Aplikaci Master-View-Controller (MVC), která zobrazuje hello vrátil data v mřížce hello je nad rámec tohoto návodu hello a mnoho online kurzy vysvětlují, jak toobuild jeden. Tento kód je v souboru kostru hello. Je však třeba toodeal s pár věcí v této aplikaci MVC:

* Zachycení, pokud uživatel zadá něco v poli vyhledávání hello
* Zadání objekt dat back toohello MasterView, aby mohla zobrazovat výsledky hello v mřížce hello

Provedeme ty níže.

### <a name="add-a-check-toosee-if-youre-logged-in"></a>Pokud jste přihlášeni, přidejte toosee kontrola
aplikace Hello nemá málo Pokud hello uživatel není přihlášený, takže je inteligentní toocheck, pokud v mezipaměti hello je již token. Pokud ne, přesměrování toohello LoginView pro uživatele toosign hello v. Pokud si Vzpomínáte, hello nejlepší způsob, jak toodo akce při načtení zobrazení se toouse hello `viewDidLoad()` metoda, která poskytuje Apple nás.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-hello-table-view-when-data-is-received"></a>Aktualizace hello zobrazení tabulky při přijetí dat
Pokud hello rozhraní Graph API vrátí data, je potřeba toodisplay hello data. Pro jednoduchost zde je všechny hello kód tooupdate hello tabulky. Můžete vložit pouze hello správné hodnoty v kódu standardní MVC.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure hello cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-toocall-hello-graph-api-when-someone-types-in-hello-search-field"></a>Zadejte hello toocall způsob, jak rozhraní Graph API, když někdo typy, které do pole hledání hello
Pokud uživatel zadá vyhledávání hello pole, je potřeba tooshove, přes toohello rozhraní Graph API. Hello `GraphAPICaller` třídy, které vytvoříte v hello následující kód, odděluje od hello prezentace hello vyhledávací funkce. Teď můžete napsat hello kód, který kanály žádné hledání znaků toohello rozhraní Graph API. Provedeme to tím, že poskytuje metodu s názvem `lookupInGraph`, což trvá hello řetězec, který má být toosearch pro.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-tooaccess-hello-graph-api"></a>Zápis pomocná třída tooaccess hello rozhraní Graph API
Toto je základní hello naše aplikace. Zatímco hello rest byl vložení kódu v vzor MVC výchozí hello od společnosti Apple, zde psát kód tooquery hello grafu hello uživatel typy a pak vrátit tato data. Tady je kód hello a podrobné vysvětlení následujícího.

### <a name="create-a-new-objective-c-header-file"></a>Vytvořit nový soubor záhlaví Objective C
Název souboru hello `GraphAPICaller.h`a přidejte následující kód hello.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Tady vidíte, že zadaná metoda přebírá řetězec a vrátí completionBlock. Tato completionBlock, jak vám může mít uhádnout, bude aktualizovat hello tabulku a poskytovat objekt vyplněná data v reálném čase jako hello hledání uživatele.

### <a name="create-a-new-objective-c-file"></a>Vytvoření nového souboru Objective C
Název souboru hello `GraphAPICaller.m`a přidejte následující metodu hello.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process hello response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab hello top most JSON node tooget our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by hello key name being "value". It really is hello name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


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

```

Prostřednictvím této metody podrobně přejděte.

základní Hello tohoto kódu je v hello `NXOAuth2Request`, metoda, která přebírá hello parametry, které jste si již definována v hello settings.plist souboru.

prvním krokem Hello je tooconstruct hello správné volání rozhraní Graph API. Protože jsou volání `/users`, můžete určit, že přidáním prostředků rozhraní Graph API toohello společně s verzí hello. Má smysl tooput v souboru s nastavením externí vzhledem k tomu, že tyto můžete změnit podle zpracovaní hello rozhraní API.

```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Dále je nutné zadat parametry toospecify, budete také poskytovat toohello volání rozhraní Graph API. Je *velmi důležité* neumisťujte hello parametry v koncovém bodu hello prostředků, vzhledem k tomu, který se očistí pro všechny znaky vyhovující – identifikátor URI za běhu. Všechny kód dotazu je třeba zadat v parametrech hello.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Možná jste si všimli volá `convertParamsToDictionary` metoda, která nebyly dosud zapsána. Pojďme se proto nyní na konci hello hello souboru:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
V dalším kroku použijeme hello `NXOAuth2Request` metoda tooget data zpět z hello rozhraní API ve formátu JSON.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process hello response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab hello top most JSON node tooget our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Nakonec se podíváme, jak vrátit hello data toohello MasterViewController. Vrátí jako serializovat Hello dat a je třeba toobe deserializovat a načíst v objektu, že hello MainViewController spotřebovat. Pro tento účel hello kostru má `User.m/h` soubor, který vytvoří objekt uživatele. Můžete naplnit tohoto objektu uživatele s informacemi z hello grafu.

```objc
                           // We can grab hello top most JSON node tooget our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by hello key name being "value". It really is hello name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-hello-sample"></a>Spuštění ukázkové hello
Pokud jste používá hello kostru nebo a potom společně s hello návod měli spustit nyní vaší aplikace. Spusťte hello simulátoru a klikněte na **přihlášení** toouse hello aplikace.

## <a name="get-security-updates-for-our-product"></a>Získejte bezpečnostní aktualizace našich produktů
Doporučujeme vám tooget oznámení o bezpečnostních incidentech návštěvou hello [Web Security TechCenter](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.

