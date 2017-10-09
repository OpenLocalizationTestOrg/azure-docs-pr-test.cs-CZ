---
title: "iOS v2 aaaAzure AD Začínáme - použití | Microsoft Docs"
description: "Jak aplikace pro iOS (Swift) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 22e67850e2e0b14b6d68815d8f23e18ce2e878ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a>Použít Microsoft ověřování knihovny (MSAL) tooget hello token pro hello Microsoft Graph API

Otevřete `ViewController.swift` a nahraďte hello kódu pomocí:

```swift
import UIKit
import MSAL

class ViewController: UIViewController, UITextFieldDelegate, URLSessionDelegate {
    
    let kClientID = "Your_Application_Id_Here"
    let kAuthority = "https://login.microsoftonline.com/common/v2.0"

    let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
    let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
    
    var accessToken = String()
    var applicationContext = MSALPublicClientApplication.init()

    @IBOutlet weak var loggingText: UITextView!
    @IBOutlet weak var signoutButton: UIButton!

     // This button will invoke hello call toohello Microsoft Graph API. It uses the
     // built in Swift libraries toocreate a connection.
    
    @IBAction func callGraphButton(_ sender: UIButton) {
        
        
        do {
            
            // We check toosee if we have a current logged in user. If we don't, then we need toosign someone in.
            // We throw an interactionRequired so that we trigger hello interactive signin.
            
            if  try self.applicationContext.users().isEmpty {
                throw NSError.init(domain: "MSALErrorDomain", code: MSALErrorCode.interactionRequired.rawValue, userInfo: nil)
            } else {
            
            // Acquire a token for an existing user silently
            
            try self.applicationContext.acquireTokenSilent(forScopes: self.kScopes, user: applicationContext.users().first) { (result, error) in
    
                    if error == nil {
                        self.accessToken = (result?.accessToken)!
                        self.loggingText.text = "Refreshing token silently)"
                        self.loggingText.text = "Refreshed Access token is \(self.accessToken)"
                        
                        self.signoutButton.isEnabled = true;
                        self.getContentWithToken()
    
                    } else {
                        self.loggingText.text = "Could not acquire token silently: \(error ?? "No error information" as! Error)"
    
                    }
                }
            }
        }  catch let error as NSError {
            
            // interactionRequired means we need tooask hello user toosign-in. This usually happens
            // when hello user's Refresh Token is expired or if hello user has changed their password
            // among other possible reasons.
            
            if error.code == MSALErrorCode.interactionRequired.rawValue {
                
                self.applicationContext.acquireToken(forScopes: self.kScopes) { (result, error) in
                        if error == nil {
                            self.accessToken = (result?.accessToken)!
                            self.loggingText.text = "Access token is \(self.accessToken)"
                            self.signoutButton.isEnabled = true;
                            self.getContentWithToken()
                            
                        } else  {
                            self.loggingText.text = "Could not acquire token: \(error ?? "No error information" as! Error)"
                        }
                }
                
            }
            
        } catch {
            
            // This is hello catch all error.
            
            self.loggingText.text = "Unable tooacquire token. Got error: \(error)"
            
        }
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        
        do {
             // Initialize a MSALPublicClientApplication with a given clientID and authority
            self.applicationContext = try MSALPublicClientApplication.init(clientId: kClientID, authority: kAuthority)
        } catch {
            self.loggingText.text = "Unable toocreate Application Context. Error: \(error)"
        }
    }


    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    override func viewWillAppear(_ animated: Bool) {
        
        if self.accessToken.isEmpty {
            signoutButton.isEnabled = false; 
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Další informace
#### <a name="getting-a-user-token-interactively"></a>Získání tokenu uživatele interaktivně
Volání hello `acquireToken` metoda výsledky v okně prohlížeče výzvy hello toosign uživatele v. Aplikace obvykle vyžadují toosign uživatele v interaktivním hello poprvé potřebují tooaccess chráněného prostředku nebo při bezobslužné operace tooacquire token selže (například hello uživatele platnost hesla).

#### <a name="getting-a-user-token-silently"></a>Získání tokenu uživatele bez upozornění
Hello `acquireTokenSilent` metoda zpracovává tokenu pořízení a obnovení bez nutnosti zásahu uživatele. Po `acquireToken` je spouštěna hello poprvé, `acquireTokenSilent` je hello metoda běžně používané tooobtain použití tokenů tooaccess chráněné zdroje pro následující volání - jako toorequest volání nebo tokeny obnovení jsou vytvářeny bezobslužně.

Nakonec `acquireTokenSilent` selže – např. uživatel hello má odhlášení, nebo došlo ke změně hesla na jiném zařízení. Když MSAL zjistí, že hello problém lze vyřešit tím, že vyžaduje interaktivní akce, aktivuje se ho `MSALErrorCode.interactionRequired` výjimka. Aplikace může zpracovat výjimku dvěma způsoby:

1.  Volání proti `acquireToken` okamžitě, výsledkem je zobrazení výzvy hello toosign uživatele v. Tento vzor se obvykle používá v online aplikace tam, kde není žádná offline obsah v aplikaci hello k dispozici pro uživatele hello. Hello ukázková aplikace generované touto s průvodcem instalací používá tento vzor: Zobrazí se v akce hello prvním spuštění aplikace hello. Protože žádný uživatel nikdy používá aplikace hello `applicationContext.users().first` bude obsahovat hodnotu null a ` MSALErrorCode.interactionRequired ` dojde k výjimce. Hello kód v ukázce hello pak obslužných rutin výjimek hello voláním `acquireToken` výsledkem výzvy hello toosign uživatele v.

2.  Aplikace lze také nastavit vizuální označení toohello uživatele, který interaktivní přihlášení je povinné, takže hello uživatele můžete vybrat hello správný čas toosign v, nebo můžete zkusit aplikace hello `acquireTokenSilent` později. To se obvykle používá při hello uživatele může používat další funkce aplikace hello bez narušení – například je offline obsah k dispozici v aplikaci hello. V takovém případě se můžete rozhodnout hello uživatele, když chtějí toosign v tooaccess hello chráněný prostředek, nebo toorefresh hello zastaralé informace nebo aplikace se můžete rozhodnout tooretry `acquireTokenSilent` obnovení po síti poté, co byla dočasně nedostupná.

<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a>Volání rozhraní API Microsoft Graph hello pomocí hello tokenu, který jste obdrželi

Přidat hello nová metoda příliš`ViewController.swift`. Tato metoda je použité toomake `GET` požadavku vůči hello pomocí Microsoft Graph API *HTTP autorizační hlavičky*:

```swift
func getContentWithToken() {
    
    let sessionConfig = URLSessionConfiguration.default
    
    // Specify hello Graph API endpoint
    let url = URL(string: kGraphURI)
    var request = URLRequest(url: url!)
    
    // Set hello Authorization header for hello request. We use Bearer tokens, so we specify Bearer + hello token we got from hello result
    request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
    let urlSession = URLSession(configuration: sessionConfig, delegate: self, delegateQueue: OperationQueue.main)
    
    urlSession.dataTask(with: request) { data, response, error in
        let result = try? JSONSerialization.jsonObject(with: data!, options: [])
                    if result != nil {
                
                self.loggingText.text = result.debugDescription
            }
        }.resume()
}
```

<!--start-collapse-->
### <a name="making-a-rest-call-against-a-protected-api"></a>Volání REST chráněné rozhraní API

V této ukázkové aplikaci hello `getContentWithToken()` metoda je použité toomake HTTP `GET` požadavku vůči chráněný prostředek, který vyžaduje volající obsahu toohello tokenu a potom se vraťte hello. Tato metoda přidá hello získat token v hello *HTTP autorizační hlavičky*. Tato ukázka je hello prostředků hello Microsoft Graph API *mi* koncového bodu – zobrazí informace o profilu uživatele hello.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Nastavení odhlášení

Přidejte následující metodu příliš hello`ViewController.swift` toosign out hello uživatele:

```swift 
@IBAction func signoutButton(_ sender: UIButton) {

    do {
        
        // Removes all tokens from hello cache for this application for hello provided user
        // first parameter:   hello user tooremove from hello cache
        
        try self.applicationContext.remove(self.applicationContext.users().first)
        self.signoutButton.isEnabled = false;
        
    } catch let error {
        self.loggingText.text = "Received error signing user out: \(error)"
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Další informace o odhlášení

Hello `signoutButton` metoda odebere hello mezipaměti uživatele MSAL – hello uživatele se efektivně tak dozví, MSAL tooforget hello aktuální uživatel tak tooacquire budoucí žádosti o token bude úspěšné, pouze pokud je k toobe interaktivní.

I když aplikace hello v této ukázce podporuje jenom jednoho konkrétního uživatele, MSAL podporuje scénáře, kde můžete více účtů přihlášení v hello současně – příklad je e-mailové aplikace kde uživatel, který má více účtů.
<!--end-collapse-->

## <a name="register-hello-callback"></a>Zaregistrovat hello zpětného volání

Po ověření uživatele hello hello prohlížeč přesměruje hello uživatelská back toohello aplikace. Postupujte podle kroků hello níže tooregister tento zpětného volání:

1.  Otevřete `AppDelegate.swift` a import MSAL:

```swift
import MSAL
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Přidejte následující metodu tooyour hello <code>AppDelegate</code> třídy toohandle zpětná volání:
</li>
</ol>

```swift
// @brief Handles inbound URLs. Checks if hello URL matches hello redirect URI for a pending AppAuth
// authorization request and if so, will look for hello code in hello response.

func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    
    print("Received callback!")
    
    MSALPublicClientApplication.handleMSALResponse(url)
    
    return true
}
```

