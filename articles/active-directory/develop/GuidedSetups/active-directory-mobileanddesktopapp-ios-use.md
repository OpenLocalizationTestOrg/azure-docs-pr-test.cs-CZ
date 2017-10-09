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
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a><span data-ttu-id="fc927-103">Použít Microsoft ověřování knihovny (MSAL) tooget hello token pro hello Microsoft Graph API</span><span class="sxs-lookup"><span data-stu-id="fc927-103">Use hello Microsoft Authentication Library (MSAL) tooget a token for hello Microsoft Graph API</span></span>

<span data-ttu-id="fc927-104">Otevřete `ViewController.swift` a nahraďte hello kódu pomocí:</span><span class="sxs-lookup"><span data-stu-id="fc927-104">Open `ViewController.swift` and replace hello code with:</span></span>

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
### <a name="more-information"></a><span data-ttu-id="fc927-105">Další informace</span><span class="sxs-lookup"><span data-stu-id="fc927-105">More Information</span></span>
#### <a name="getting-a-user-token-interactively"></a><span data-ttu-id="fc927-106">Získání tokenu uživatele interaktivně</span><span class="sxs-lookup"><span data-stu-id="fc927-106">Getting a user token interactively</span></span>
<span data-ttu-id="fc927-107">Volání hello `acquireToken` metoda výsledky v okně prohlížeče výzvy hello toosign uživatele v.</span><span class="sxs-lookup"><span data-stu-id="fc927-107">Calling hello `acquireToken` method results in a browser window prompting hello user toosign in.</span></span> <span data-ttu-id="fc927-108">Aplikace obvykle vyžadují toosign uživatele v interaktivním hello poprvé potřebují tooaccess chráněného prostředku nebo při bezobslužné operace tooacquire token selže (například hello uživatele platnost hesla).</span><span class="sxs-lookup"><span data-stu-id="fc927-108">Applications usually require a user toosign in interactively hello first time they need tooaccess a protected resource, or when a silent operation tooacquire a token fails (e.g. hello user’s password expired).</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="fc927-109">Získání tokenu uživatele bez upozornění</span><span class="sxs-lookup"><span data-stu-id="fc927-109">Getting a user token silently</span></span>
<span data-ttu-id="fc927-110">Hello `acquireTokenSilent` metoda zpracovává tokenu pořízení a obnovení bez nutnosti zásahu uživatele.</span><span class="sxs-lookup"><span data-stu-id="fc927-110">hello `acquireTokenSilent` method handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="fc927-111">Po `acquireToken` je spouštěna hello poprvé, `acquireTokenSilent` je hello metoda běžně používané tooobtain použití tokenů tooaccess chráněné zdroje pro následující volání - jako toorequest volání nebo tokeny obnovení jsou vytvářeny bezobslužně.</span><span class="sxs-lookup"><span data-stu-id="fc927-111">After `acquireToken` is executed for hello first time, `acquireTokenSilent` is hello method commonly used tooobtain tokens used tooaccess protected resources for subsequent calls - as calls toorequest or renew tokens are made silently.</span></span>

<span data-ttu-id="fc927-112">Nakonec `acquireTokenSilent` selže – např. uživatel hello má odhlášení, nebo došlo ke změně hesla na jiném zařízení.</span><span class="sxs-lookup"><span data-stu-id="fc927-112">Eventually, `acquireTokenSilent` will fail – e.g. hello user has signed out, or has changed their password on another device.</span></span> <span data-ttu-id="fc927-113">Když MSAL zjistí, že hello problém lze vyřešit tím, že vyžaduje interaktivní akce, aktivuje se ho `MSALErrorCode.interactionRequired` výjimka.</span><span class="sxs-lookup"><span data-stu-id="fc927-113">When MSAL detects that hello issue can be resolved by requiring an interactive action, it fires an `MSALErrorCode.interactionRequired` exception.</span></span> <span data-ttu-id="fc927-114">Aplikace může zpracovat výjimku dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="fc927-114">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="fc927-115">Volání proti `acquireToken` okamžitě, výsledkem je zobrazení výzvy hello toosign uživatele v.</span><span class="sxs-lookup"><span data-stu-id="fc927-115">Make a call against `acquireToken` immediately, which results in prompting hello user toosign in.</span></span> <span data-ttu-id="fc927-116">Tento vzor se obvykle používá v online aplikace tam, kde není žádná offline obsah v aplikaci hello k dispozici pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="fc927-116">This pattern is usually used in online applications where there is no offline content in hello application available for hello user.</span></span> <span data-ttu-id="fc927-117">Hello ukázková aplikace generované touto s průvodcem instalací používá tento vzor: Zobrazí se v akce hello prvním spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fc927-117">hello sample application generated by this guided setup uses this pattern: you can see it in action hello first time you execute hello application.</span></span> <span data-ttu-id="fc927-118">Protože žádný uživatel nikdy používá aplikace hello `applicationContext.users().first` bude obsahovat hodnotu null a ` MSALErrorCode.interactionRequired ` dojde k výjimce.</span><span class="sxs-lookup"><span data-stu-id="fc927-118">Because no user ever used hello application, `applicationContext.users().first` will contain a null value, and an ` MSALErrorCode.interactionRequired ` exception will be thrown.</span></span> <span data-ttu-id="fc927-119">Hello kód v ukázce hello pak obslužných rutin výjimek hello voláním `acquireToken` výsledkem výzvy hello toosign uživatele v.</span><span class="sxs-lookup"><span data-stu-id="fc927-119">hello code in hello sample then handles hello exception by calling `acquireToken` resulting in prompting hello user toosign in.</span></span>

2.  <span data-ttu-id="fc927-120">Aplikace lze také nastavit vizuální označení toohello uživatele, který interaktivní přihlášení je povinné, takže hello uživatele můžete vybrat hello správný čas toosign v, nebo můžete zkusit aplikace hello `acquireTokenSilent` později.</span><span class="sxs-lookup"><span data-stu-id="fc927-120">Applications can also make a visual indication toohello user that an interactive sign-in is required, so hello user can select hello right time toosign in, or hello application can retry `acquireTokenSilent` at a later time.</span></span> <span data-ttu-id="fc927-121">To se obvykle používá při hello uživatele může používat další funkce aplikace hello bez narušení – například je offline obsah k dispozici v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="fc927-121">This is usually used when hello user can use other functionality of hello application without being disrupted - for example, there is offline content available in hello application.</span></span> <span data-ttu-id="fc927-122">V takovém případě se můžete rozhodnout hello uživatele, když chtějí toosign v tooaccess hello chráněný prostředek, nebo toorefresh hello zastaralé informace nebo aplikace se můžete rozhodnout tooretry `acquireTokenSilent` obnovení po síti poté, co byla dočasně nedostupná.</span><span class="sxs-lookup"><span data-stu-id="fc927-122">In this case, hello user can decide when they want toosign in tooaccess hello protected resource, or toorefresh hello outdated information, or your application can decide tooretry `acquireTokenSilent` when network is restored after being unavailable temporarily.</span></span>

<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a><span data-ttu-id="fc927-123">Volání rozhraní API Microsoft Graph hello pomocí hello tokenu, který jste obdrželi</span><span class="sxs-lookup"><span data-stu-id="fc927-123">Call hello Microsoft Graph API using hello token you just obtained</span></span>

<span data-ttu-id="fc927-124">Přidat hello nová metoda příliš`ViewController.swift`.</span><span class="sxs-lookup"><span data-stu-id="fc927-124">Add hello new method below too`ViewController.swift`.</span></span> <span data-ttu-id="fc927-125">Tato metoda je použité toomake `GET` požadavku vůči hello pomocí Microsoft Graph API *HTTP autorizační hlavičky*:</span><span class="sxs-lookup"><span data-stu-id="fc927-125">This method is used toomake a `GET` request against hello Microsoft Graph API using an *HTTP Authorization header*:</span></span>

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
### <a name="making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="fc927-126">Volání REST chráněné rozhraní API</span><span class="sxs-lookup"><span data-stu-id="fc927-126">Making a REST call against a protected API</span></span>

<span data-ttu-id="fc927-127">V této ukázkové aplikaci hello `getContentWithToken()` metoda je použité toomake HTTP `GET` požadavku vůči chráněný prostředek, který vyžaduje volající obsahu toohello tokenu a potom se vraťte hello.</span><span class="sxs-lookup"><span data-stu-id="fc927-127">In this sample application, hello `getContentWithToken()` method is used toomake an HTTP `GET` request against a protected resource that requires a token and then return hello content toohello caller.</span></span> <span data-ttu-id="fc927-128">Tato metoda přidá hello získat token v hello *HTTP autorizační hlavičky*.</span><span class="sxs-lookup"><span data-stu-id="fc927-128">This method adds hello acquired token in hello *HTTP Authorization header*.</span></span> <span data-ttu-id="fc927-129">Tato ukázka je hello prostředků hello Microsoft Graph API *mi* koncového bodu – zobrazí informace o profilu uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="fc927-129">For this sample, hello resource is hello Microsoft Graph API *me* endpoint – which displays hello user's profile information.</span></span>
<!--end-collapse-->

## <a name="set-up-sign-out"></a><span data-ttu-id="fc927-130">Nastavení odhlášení</span><span class="sxs-lookup"><span data-stu-id="fc927-130">Set up sign-out</span></span>

<span data-ttu-id="fc927-131">Přidejte následující metodu příliš hello`ViewController.swift` toosign out hello uživatele:</span><span class="sxs-lookup"><span data-stu-id="fc927-131">Add hello following method too`ViewController.swift` toosign out hello user:</span></span>

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
### <a name="more-info-on-sign-out"></a><span data-ttu-id="fc927-132">Další informace o odhlášení</span><span class="sxs-lookup"><span data-stu-id="fc927-132">More info on sign-out</span></span>

<span data-ttu-id="fc927-133">Hello `signoutButton` metoda odebere hello mezipaměti uživatele MSAL – hello uživatele se efektivně tak dozví, MSAL tooforget hello aktuální uživatel tak tooacquire budoucí žádosti o token bude úspěšné, pouze pokud je k toobe interaktivní.</span><span class="sxs-lookup"><span data-stu-id="fc927-133">hello `signoutButton` method removes hello user from hello MSAL user cache – this will effectively tell MSAL tooforget hello current user so a future request tooacquire a token will only succeed if it is made toobe interactive.</span></span>

<span data-ttu-id="fc927-134">I když aplikace hello v této ukázce podporuje jenom jednoho konkrétního uživatele, MSAL podporuje scénáře, kde můžete více účtů přihlášení v hello současně – příklad je e-mailové aplikace kde uživatel, který má více účtů.</span><span class="sxs-lookup"><span data-stu-id="fc927-134">Although hello application in this sample supports a single user, MSAL supports scenarios where multiple accounts can be signed in at hello same time – an example is an email application where a user has multiple accounts.</span></span>
<!--end-collapse-->

## <a name="register-hello-callback"></a><span data-ttu-id="fc927-135">Zaregistrovat hello zpětného volání</span><span class="sxs-lookup"><span data-stu-id="fc927-135">Register hello callback</span></span>

<span data-ttu-id="fc927-136">Po ověření uživatele hello hello prohlížeč přesměruje hello uživatelská back toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="fc927-136">Once hello user authenticates, hello browser redirects hello user back toohello application.</span></span> <span data-ttu-id="fc927-137">Postupujte podle kroků hello níže tooregister tento zpětného volání:</span><span class="sxs-lookup"><span data-stu-id="fc927-137">Follow hello steps below tooregister this callback:</span></span>

1.  <span data-ttu-id="fc927-138">Otevřete `AppDelegate.swift` a import MSAL:</span><span class="sxs-lookup"><span data-stu-id="fc927-138">Open `AppDelegate.swift` and import MSAL:</span></span>

```swift
import MSAL
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="fc927-139">Přidejte následující metodu tooyour hello <code>AppDelegate</code> třídy toohandle zpětná volání:</span><span class="sxs-lookup"><span data-stu-id="fc927-139">Add hello following method tooyour <code>AppDelegate</code> class toohandle callbacks:</span></span>
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

