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
# <a name="add-sign-in-tooan-ios-app-using-a-third-party-library-with-graph-api-using-hello-v20-endpoint"></a><span data-ttu-id="2fdd8-103">Přidat aplikaci iOS tooan přihlášení pomocí rozhraní Graph API pomocí koncového bodu v2.0 hello knihovně třetích stran</span><span class="sxs-lookup"><span data-stu-id="2fdd8-103">Add sign-in tooan iOS app using a third-party library with Graph API using hello v2.0 endpoint</span></span>
<span data-ttu-id="2fdd8-104">Platforma identity Microsoft Hello používá otevřete standardy, jako je OAuth2 a OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-104">hello Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="2fdd8-105">Vývojáři mohou použít žádnou knihovnu chtějí toointegrate s našich služeb.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-105">Developers can use any library they want toointegrate with our services.</span></span> <span data-ttu-id="2fdd8-106">Vývojáři toohelp používat naše platforma s další knihovny, jsme jste zapsána několik návody jako tento jeden toodemonstrate jak tooconfigure třetích stran knihovny tooconnect toohello Microsoft identity platformy.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-106">toohelp developers use our platform with other libraries, we've written a few walkthroughs like this one toodemonstrate how tooconfigure third-party libraries tooconnect toohello Microsoft identity platform.</span></span> <span data-ttu-id="2fdd8-107">Většina knihovny, které implementují [specifikace hello RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) můžete připojení toohello Microsoft identity platformy.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-107">Most libraries that implement [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) can connect toohello Microsoft identity platform.</span></span>

<span data-ttu-id="2fdd8-108">Hello aplikaci, která vytváří Tento názorný postup mohou uživatelé přihlásit tootheir organizace a poté vyhledejte ostatními v organizaci pomocí hello rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-108">With hello application that this walkthrough creates, users can sign in tootheir organization and then search for others in their organization by using hello Graph API.</span></span>

<span data-ttu-id="2fdd8-109">Pokud jste nový tooOAuth2 nebo OpenID Connect, mnohem této ukázkové konfigurace nemusí mít smysl tooyou.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-109">If you're new tooOAuth2 or OpenID Connect, much of this sample configuration may not make sense tooyou.</span></span> <span data-ttu-id="2fdd8-110">Doporučujeme, abyste si přečetli [v2.0 protokoly - toku OAuth 2.0 autorizační kód](active-directory-v2-protocols-oauth-code.md) pozadí.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-110">We recommend that you read  [v2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md) for background.</span></span>

> [!NOTE]
> <span data-ttu-id="2fdd8-111">Některé funkce naše platformy, které mají výraz v hello OAuth2 nebo OpenID Connect standardy, jako je například podmíněný přístup a správu zásad Intune, vyžadují jste toouse naše s otevřeným zdrojem Identity pro knihovny Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-111">Some features of our platform that do have an expression in hello OAuth2 or OpenID Connect standards, such as Conditional Access and Intune policy management, require you toouse our open source Microsoft Azure Identity Libraries.</span></span>
> 
> 

<span data-ttu-id="2fdd8-112">koncový bod v2.0 Hello nepodporuje všechny scénáře Azure Active Directory a funkce.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-112">hello v2.0 endpoint does not support all Azure Active Directory scenarios and features.</span></span>

> [!NOTE]
> <span data-ttu-id="2fdd8-113">toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="2fdd8-113">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-code-from-github"></a><span data-ttu-id="2fdd8-114">Stáhněte si kód z Githubu</span><span class="sxs-lookup"><span data-stu-id="2fdd8-114">Download code from GitHub</span></span>
<span data-ttu-id="2fdd8-115">Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).</span><span class="sxs-lookup"><span data-stu-id="2fdd8-115">hello code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).</span></span>  <span data-ttu-id="2fdd8-116">toofollow společně, můžete [stáhnout kostru aplikace hello jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) nebo hello kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="2fdd8-116">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

<span data-ttu-id="2fdd8-117">Můžete také právě stažení ukázky hello a rovnou začít:</span><span class="sxs-lookup"><span data-stu-id="2fdd8-117">You can also just download hello sample and get started right away:</span></span>

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a><span data-ttu-id="2fdd8-118">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="2fdd8-118">Register an app</span></span>
<span data-ttu-id="2fdd8-119">Vytvoření nové aplikace v hello [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle hello podrobné kroky v [jak tooregister aplikace s koncovým bodem v2.0 hello](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="2fdd8-119">Create a new app at hello [Application registration portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow hello detailed steps at  [How tooregister an app with hello v2.0 endpoint](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="2fdd8-120">Zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="2fdd8-120">Make sure to:</span></span>

* <span data-ttu-id="2fdd8-121">Kopírování hello **Id aplikace** aplikace přiřazené tooyour důvodem je, že ho budete potřebovat brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-121">Copy hello **Application Id** that's assigned tooyour app because you'll need it soon.</span></span>
* <span data-ttu-id="2fdd8-122">Přidat hello **Mobile** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-122">Add hello **Mobile** platform for your app.</span></span>
* <span data-ttu-id="2fdd8-123">Kopírování hello **identifikátor URI pro přesměrování** z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-123">Copy hello **Redirect URI** from hello portal.</span></span> <span data-ttu-id="2fdd8-124">Musíte použít výchozí hodnotu hello `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-124">You must use hello default value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="download-hello-third-party-nxoauth2-library-and-create-a-workspace"></a><span data-ttu-id="2fdd8-125">Stažení hello třetích stran NXOAuth2 knihovny a vytvořit pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="2fdd8-125">Download hello third-party NXOAuth2 library and create a workspace</span></span>
<span data-ttu-id="2fdd8-126">V tomto návodu budete používat hello OAuth2Client z Githubu, která je již OAuth2 knihovny pro Mac OS X a iOS (kakao a kakao touch).</span><span class="sxs-lookup"><span data-stu-id="2fdd8-126">For this walkthrough, you will use hello OAuth2Client from GitHub, which is an OAuth2 library for Mac OS X and iOS (Cocoa and Cocoa touch).</span></span> <span data-ttu-id="2fdd8-127">Tato knihovna je založena na koncept 10 specifikace hello OAuth2. Profil nativní aplikace hello implementuje a podporuje koncový bod autorizace hello hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-127">This library is based on draft 10 of hello OAuth2 spec. It implements hello native application profile and supports hello authorization endpoint of hello user.</span></span> <span data-ttu-id="2fdd8-128">To je vše, co hello budete potřebovat toointegrate s platformou identity Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-128">These are all hello things you'll need toointegrate with hello Microsoft identity platform.</span></span>

### <a name="add-hello-library-tooyour-project-by-using-cocoapods"></a><span data-ttu-id="2fdd8-129">Přidejte hello knihovně tooyour projekt pomocí CocoaPods</span><span class="sxs-lookup"><span data-stu-id="2fdd8-129">Add hello library tooyour project by using CocoaPods</span></span>
<span data-ttu-id="2fdd8-130">CocoaPods je správce závislostí pro projekty Xcode.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-130">CocoaPods is a dependency manager for Xcode projects.</span></span> <span data-ttu-id="2fdd8-131">Automaticky spravuje hello předchozí kroky instalace.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-131">It manages hello previous installation steps automatically.</span></span>

```
$ vi Podfile
```
1. <span data-ttu-id="2fdd8-132">Přidejte následující toothis podfile hello:</span><span class="sxs-lookup"><span data-stu-id="2fdd8-132">Add hello following toothis podfile:</span></span>
   
    ```
     platform :ios, '8.0'
   
     target 'QuickStart' do
   
     pod 'NXOAuth2Client'
   
     end
    ```
2. <span data-ttu-id="2fdd8-133">Načtěte hello podfile pomocí CocoaPods.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-133">Load hello podfile by using CocoaPods.</span></span> <span data-ttu-id="2fdd8-134">Tím se vytvoří nový pracovní prostor Xcode, který načtete.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-134">This will create a new Xcode workspace that you will load.</span></span>
   
    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-hello-structure-of-hello-project"></a><span data-ttu-id="2fdd8-135">Prozkoumejte hello struktura hello projektu</span><span class="sxs-lookup"><span data-stu-id="2fdd8-135">Explore hello structure of hello project</span></span>
<span data-ttu-id="2fdd8-136">pro naše projekt v kostře hello je nastavit Hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="2fdd8-136">hello following structure is set up for our project in hello skeleton:</span></span>

* <span data-ttu-id="2fdd8-137">Hlavní zobrazení s UPN vyhledávání</span><span class="sxs-lookup"><span data-stu-id="2fdd8-137">A Master View with a UPN Search</span></span>
* <span data-ttu-id="2fdd8-138">Zobrazení podrobností pro hello data o hello vybraného uživatele</span><span class="sxs-lookup"><span data-stu-id="2fdd8-138">A Detail View for hello data about hello selected user</span></span>
* <span data-ttu-id="2fdd8-139">Přihlášení zobrazení, kde uživatel může přihlásit toohello aplikace tooquery hello grafu</span><span class="sxs-lookup"><span data-stu-id="2fdd8-139">A Login View where a user can sign in toohello app tooquery hello graph</span></span>

<span data-ttu-id="2fdd8-140">Přesune jsme toovarious soubory v hello kostru tooadd ověřování.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-140">We will move toovarious files in hello skeleton tooadd authentication.</span></span> <span data-ttu-id="2fdd8-141">Ostatní části hello kódu, třeba hello visual kódu, se netýkají tooidentity ale k dispozici máte.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-141">Other parts of hello code, such as hello visual code, do not pertain tooidentity but are provided for you.</span></span>

## <a name="set-up-hello-settingsplst-file-in-hello-library"></a><span data-ttu-id="2fdd8-142">Nastavení souboru settings.plst hello v hello knihovně</span><span class="sxs-lookup"><span data-stu-id="2fdd8-142">Set up hello settings.plst file in hello library</span></span>
* <span data-ttu-id="2fdd8-143">V projektu typu rychlý start hello, otevřete hello `settings.plist` souboru.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-143">In hello QuickStart project, open hello `settings.plist` file.</span></span> <span data-ttu-id="2fdd8-144">Nahraďte hodnoty hello hello elementů v hello části tooreflect hello hodnoty, které jste použili v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-144">Replace hello values of hello elements in hello section tooreflect hello values that you used in hello Azure portal.</span></span> <span data-ttu-id="2fdd8-145">Váš kód bude odkazovat tyto hodnoty vždy, když používá hello knihovna ověřování Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-145">Your code will reference these values whenever it uses hello Active Directory Authentication Library.</span></span>
  * <span data-ttu-id="2fdd8-146">Hello `clientId` je hello ID klienta aplikace, který jste zkopírovali z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-146">hello `clientId` is hello client ID of your application that you copied from hello portal.</span></span>
  * <span data-ttu-id="2fdd8-147">Hello `redirectUri` je adresa URL přesměrování hello tohoto hello portálu zadat.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-147">hello `redirectUri` is hello redirect URL that hello portal provided.</span></span>

## <a name="set-up-hello-nxoauth2client-library-in-your-loginviewcontroller"></a><span data-ttu-id="2fdd8-148">Nastavit hello knihovně NXOAuth2Client ve vašem LoginViewController</span><span class="sxs-lookup"><span data-stu-id="2fdd8-148">Set up hello NXOAuth2Client library in your LoginViewController</span></span>
<span data-ttu-id="2fdd8-149">Knihovna NXOAuth2Client Hello vyžaduje tooget některé hodnoty nastavení.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-149">hello NXOAuth2Client library requires some values tooget set up.</span></span> <span data-ttu-id="2fdd8-150">Po dokončení této úlohy můžete použít hello získal token toocall hello rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-150">After you complete that task, you can use hello acquired token toocall hello Graph API.</span></span> <span data-ttu-id="2fdd8-151">Protože `LoginView` bude volána kdykoli potřebujeme tooauthenticate, hodnoty konfigurace tooput dává smysl v souboru toothat.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-151">Because `LoginView` will be called any time we need tooauthenticate, it makes sense tooput configuration values in toothat file.</span></span>

* <span data-ttu-id="2fdd8-152">Přidejme některé hodnoty toohello `LoginViewController.m` souboru tooset hello kontext pro ověřování a autorizaci.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-152">Let's add some values toohello  `LoginViewController.m` file tooset hello context for authentication and authorization.</span></span> <span data-ttu-id="2fdd8-153">Podrobnosti o hodnotách hello podle kódu hello.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-153">Details about hello values follow hello code.</span></span>
  
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

<span data-ttu-id="2fdd8-154">Podívejme se na podrobnosti o hello kódu.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-154">Let's look at details about hello code.</span></span>

<span data-ttu-id="2fdd8-155">První řetězec Hello je pro `scopes`.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-155">hello first string is for `scopes`.</span></span>  <span data-ttu-id="2fdd8-156">Hello `User.Read` hodnota vám umožní tooread hello základní profil hello přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-156">hello `User.Read` value allows you tooread hello basic profile of hello signed in user.</span></span>

<span data-ttu-id="2fdd8-157">Další informace o všech dostupných oborů hello v [obory oprávnění Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes).</span><span class="sxs-lookup"><span data-stu-id="2fdd8-157">You can learn more about all hello available scopes at [Microsoft Graph permission scopes](https://graph.microsoft.io/docs/authorization/permission_scopes).</span></span>

<span data-ttu-id="2fdd8-158">Pro `authURL`, `loginURL`, `bhh`, a `tokenURL`, měli byste použít hodnoty hello uvedeny dříve.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-158">For `authURL`, `loginURL`, `bhh`, and `tokenURL`, you should use hello values provided previously.</span></span> <span data-ttu-id="2fdd8-159">Pokud používáte hello knihovny Identity Microsoft Azure s otevřeným zdrojem, jsme stahují tato data můžete pomocí našeho koncový bod metadat.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-159">If you use hello open source Microsoft Azure Identity Libraries, we pull this data down for you by using our metadata endpoint.</span></span> <span data-ttu-id="2fdd8-160">Máme jste Hotovo hello náročné práce extrahování tyto hodnoty pro vás.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-160">We've done hello hard work of extracting these values for you.</span></span>

<span data-ttu-id="2fdd8-161">Hello `keychain` hodnota je hello kontejner, který hello knihovně NXOAuth2Client použije toocreate toostore řetězce klíčů vašich tokenů.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-161">hello `keychain` value is hello container that hello NXOAuth2Client library will use toocreate a keychain toostore your tokens.</span></span> <span data-ttu-id="2fdd8-162">Pokud chcete tooget jednotné přihlašování (SSO) napříč více aplikací, můžete zadat hello stejné řetězce klíčů v každé z vašich aplikací a požádat o použití tohoto řetězce klíčů ve vašem prostředí Xcode nárocích hello.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-162">If you'd like tooget single sign-on (SSO) across numerous apps, you can specify hello same keychain in each of your applications and request hello use of that keychain in your Xcode entitlements.</span></span> <span data-ttu-id="2fdd8-163">To je vysvětleno v hello dokumentaci od společnosti Apple.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-163">This is explained in hello Apple documentation.</span></span>

<span data-ttu-id="2fdd8-164">Hello zbytek tyto hodnoty jsou požadované toouse hello knihovně a vytvořit míst pro vás toocarry hodnoty toohello kontext.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-164">hello rest of these values are required toouse hello library and create places for you toocarry values toohello context.</span></span>

### <a name="create-a-url-cache"></a><span data-ttu-id="2fdd8-165">Vytvoření mezipaměti se adresa URL</span><span class="sxs-lookup"><span data-stu-id="2fdd8-165">Create a URL cache</span></span>
<span data-ttu-id="2fdd8-166">Uvnitř `(void)viewDidLoad()`, která je volána vždy po načtení hello zobrazení, hello následující kód primes mezipaměti pro naše používání.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-166">Inside `(void)viewDidLoad()`, which is always called after hello view is loaded, hello following code primes a cache for our use.</span></span>

<span data-ttu-id="2fdd8-167">Přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="2fdd8-167">Add hello following code:</span></span>

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

### <a name="create-a-webview-for-sign-in"></a><span data-ttu-id="2fdd8-168">Vytvořit webové zobrazení pro přihlášení</span><span class="sxs-lookup"><span data-stu-id="2fdd8-168">Create a WebView for sign-in</span></span>
<span data-ttu-id="2fdd8-169">Webové zobrazení můžete hello uživatele vyzve k dalších faktorů, jako je SMS textová zpráva (Pokud je nakonfigurovaná), nebo může vracet chybové zprávy toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-169">A WebView can prompt hello user for additional factors like SMS text message (if configured) or return error messages toohello user.</span></span> <span data-ttu-id="2fdd8-170">Tady budete nastavíte hello webové zobrazení a potom novější hello zápisu zpětná kód toohandle hello volání, které bude provedena v hello webové zobrazení z hello identity služby.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-170">Here you'll set up hello WebView and then later write hello code toohandle hello callbacks that will happen in hello WebView from hello identity services.</span></span>

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

### <a name="override-hello-webview-methods-toohandle-authentication"></a><span data-ttu-id="2fdd8-171">Přepsání hello webové zobrazení metody toohandle ověřování</span><span class="sxs-lookup"><span data-stu-id="2fdd8-171">Override hello WebView methods toohandle authentication</span></span>
<span data-ttu-id="2fdd8-172">tootell hello webového zobrazení, co se stane, když uživatel potřebuje toosign v, jak je popsáno dříve, můžete vložit hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-172">tootell hello WebView what happens when a user needs toosign in as discussed previously, you can paste hello following code.</span></span>

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

### <a name="write-code-toohandle-hello-result-of-hello-oauth2-request"></a><span data-ttu-id="2fdd8-173">Zápis kódu toohandle hello výsledek požadavku OAuth2 hello</span><span class="sxs-lookup"><span data-stu-id="2fdd8-173">Write code toohandle hello result of hello OAuth2 request</span></span>
<span data-ttu-id="2fdd8-174">Hello následující kód bude zpracovávat hello redirectURL, který vrací z hello webové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-174">hello following code will handle hello redirectURL that returns from hello WebView.</span></span> <span data-ttu-id="2fdd8-175">Pokud ověření nebylo úspěšné, pokusí znovu hello kódu.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-175">If authentication wasn't successful, hello code will try again.</span></span> <span data-ttu-id="2fdd8-176">Mezitím hello knihovně poskytne hello chybu, která můžete zobrazit v konzole hello nebo asynchronní zpracování.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-176">Meanwhile, hello library will provide hello error that you can see in hello console or handle asynchronously.</span></span>

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

### <a name="set-up-hello-oauth-context-called-account-store"></a><span data-ttu-id="2fdd8-177">Nastavit hello OAuth kontextu (označovanou jako účet úložiště)</span><span class="sxs-lookup"><span data-stu-id="2fdd8-177">Set up hello OAuth Context (called account store)</span></span>
<span data-ttu-id="2fdd8-178">Zde můžete volat `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` na hello sdíleného účtu úložiště pro každou službu, které chcete mít tooaccess toobe hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-178">Here you can call `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` on hello shared account store for each service that you want hello application toobe able tooaccess.</span></span> <span data-ttu-id="2fdd8-179">Typ účtu Hello je řetězec, který se používá jako identifikátor pro určité služby.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-179">hello account type is a string that is used as an identifier for a certain service.</span></span> <span data-ttu-id="2fdd8-180">Protože přistupujete hello rozhraní Graph API, kód hello odkazuje tooit jako `"myGraphService"`.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-180">Because you are accessing hello Graph API, hello code refers tooit as `"myGraphService"`.</span></span> <span data-ttu-id="2fdd8-181">Potom nastavíte pozorovatele při nic změnu s tokenem hello.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-181">You then set up an observer that will tell you when anything changes with hello token.</span></span> <span data-ttu-id="2fdd8-182">Po získání tokenu hello vrátíte zpět toohello hello uživatele `masterView`.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-182">After you get hello token, you return hello user back toohello `masterView`.</span></span>

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

## <a name="set-up-hello-master-view-toosearch-and-display-hello-users-from-hello-graph-api"></a><span data-ttu-id="2fdd8-183">Nastavení hello toosearch hlavní zobrazení a zobrazení hello uživatele z hello rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="2fdd8-183">Set up hello Master View toosearch and display hello users from hello Graph API</span></span>
<span data-ttu-id="2fdd8-184">Aplikaci Master-View-Controller (MVC), která zobrazuje hello vrátil data v mřížce hello je nad rámec tohoto návodu hello a mnoho online kurzy vysvětlují, jak toobuild jeden.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-184">A Master-View-Controller (MVC) app that displays hello returned data in hello grid is beyond hello scope of this walkthrough, and many online tutorials explain how toobuild one.</span></span> <span data-ttu-id="2fdd8-185">Tento kód je v souboru kostru hello.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-185">All this code is in hello skeleton file.</span></span> <span data-ttu-id="2fdd8-186">Je však třeba toodeal s pár věcí v této aplikaci MVC:</span><span class="sxs-lookup"><span data-stu-id="2fdd8-186">However, you do need toodeal with a few things in this MVC application:</span></span>

* <span data-ttu-id="2fdd8-187">Zachycení, pokud uživatel zadá něco v poli vyhledávání hello</span><span class="sxs-lookup"><span data-stu-id="2fdd8-187">Intercept when a user types something in hello search field</span></span>
* <span data-ttu-id="2fdd8-188">Zadání objekt dat back toohello MasterView, aby mohla zobrazovat výsledky hello v mřížce hello</span><span class="sxs-lookup"><span data-stu-id="2fdd8-188">Provide an object of data back toohello MasterView so it can display hello results in hello grid</span></span>

<span data-ttu-id="2fdd8-189">Provedeme ty níže.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-189">We'll do those below.</span></span>

### <a name="add-a-check-toosee-if-youre-logged-in"></a><span data-ttu-id="2fdd8-190">Pokud jste přihlášeni, přidejte toosee kontrola</span><span class="sxs-lookup"><span data-stu-id="2fdd8-190">Add a check toosee if you're logged in</span></span>
<span data-ttu-id="2fdd8-191">aplikace Hello nemá málo Pokud hello uživatel není přihlášený, takže je inteligentní toocheck, pokud v mezipaměti hello je již token.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-191">hello application does little if hello user is not signed in, so it's smart toocheck if there is already a token in hello cache.</span></span> <span data-ttu-id="2fdd8-192">Pokud ne, přesměrování toohello LoginView pro uživatele toosign hello v.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-192">If not, you redirect toohello LoginView for hello user toosign in.</span></span> <span data-ttu-id="2fdd8-193">Pokud si Vzpomínáte, hello nejlepší způsob, jak toodo akce při načtení zobrazení se toouse hello `viewDidLoad()` metoda, která poskytuje Apple nás.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-193">If you recall, hello best way toodo actions when a view loads is toouse hello `viewDidLoad()` method that Apple provides us.</span></span>

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

### <a name="update-hello-table-view-when-data-is-received"></a><span data-ttu-id="2fdd8-194">Aktualizace hello zobrazení tabulky při přijetí dat</span><span class="sxs-lookup"><span data-stu-id="2fdd8-194">Update hello Table View when data is received</span></span>
<span data-ttu-id="2fdd8-195">Pokud hello rozhraní Graph API vrátí data, je potřeba toodisplay hello data.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-195">When hello Graph API returns data, you need toodisplay hello data.</span></span> <span data-ttu-id="2fdd8-196">Pro jednoduchost zde je všechny hello kód tooupdate hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-196">For simplicity, here is all hello code tooupdate hello table.</span></span> <span data-ttu-id="2fdd8-197">Můžete vložit pouze hello správné hodnoty v kódu standardní MVC.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-197">You can just paste hello right values in your MVC boilerplate code.</span></span>

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

### <a name="provide-a-way-toocall-hello-graph-api-when-someone-types-in-hello-search-field"></a><span data-ttu-id="2fdd8-198">Zadejte hello toocall způsob, jak rozhraní Graph API, když někdo typy, které do pole hledání hello</span><span class="sxs-lookup"><span data-stu-id="2fdd8-198">Provide a way toocall hello Graph API when someone types in hello search field</span></span>
<span data-ttu-id="2fdd8-199">Pokud uživatel zadá vyhledávání hello pole, je potřeba tooshove, přes toohello rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-199">When a user types a search in hello box, you need tooshove that over toohello Graph API.</span></span> <span data-ttu-id="2fdd8-200">Hello `GraphAPICaller` třídy, které vytvoříte v hello následující kód, odděluje od hello prezentace hello vyhledávací funkce.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-200">hello `GraphAPICaller` class, which you will build in hello following code, separates hello lookup functionality from hello presentation.</span></span> <span data-ttu-id="2fdd8-201">Teď můžete napsat hello kód, který kanály žádné hledání znaků toohello rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-201">For now, let's write hello code that feeds any search characters toohello Graph API.</span></span> <span data-ttu-id="2fdd8-202">Provedeme to tím, že poskytuje metodu s názvem `lookupInGraph`, což trvá hello řetězec, který má být toosearch pro.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-202">We do this by providing a method called `lookupInGraph`, which takes hello string that we want toosearch for.</span></span>

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

## <a name="write-a-helper-class-tooaccess-hello-graph-api"></a><span data-ttu-id="2fdd8-203">Zápis pomocná třída tooaccess hello rozhraní Graph API</span><span class="sxs-lookup"><span data-stu-id="2fdd8-203">Write a Helper class tooaccess hello Graph API</span></span>
<span data-ttu-id="2fdd8-204">Toto je základní hello naše aplikace.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-204">This is hello core of our application.</span></span> <span data-ttu-id="2fdd8-205">Zatímco hello rest byl vložení kódu v vzor MVC výchozí hello od společnosti Apple, zde psát kód tooquery hello grafu hello uživatel typy a pak vrátit tato data.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-205">Whereas hello rest was inserting code in hello default MVC pattern from Apple, here you write code tooquery hello graph as hello user types and then return that data.</span></span> <span data-ttu-id="2fdd8-206">Tady je kód hello a podrobné vysvětlení následujícího.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-206">Here's hello code, and a detailed explanation follows it.</span></span>

### <a name="create-a-new-objective-c-header-file"></a><span data-ttu-id="2fdd8-207">Vytvořit nový soubor záhlaví Objective C</span><span class="sxs-lookup"><span data-stu-id="2fdd8-207">Create a new Objective C header file</span></span>
<span data-ttu-id="2fdd8-208">Název souboru hello `GraphAPICaller.h`a přidejte následující kód hello.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-208">Name hello file `GraphAPICaller.h`, and add hello following code.</span></span>

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

<span data-ttu-id="2fdd8-209">Tady vidíte, že zadaná metoda přebírá řetězec a vrátí completionBlock.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-209">Here you see that a specified method takes a string and returns a completionBlock.</span></span> <span data-ttu-id="2fdd8-210">Tato completionBlock, jak vám může mít uhádnout, bude aktualizovat hello tabulku a poskytovat objekt vyplněná data v reálném čase jako hello hledání uživatele.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-210">This completionBlock, as you may have guessed, will update hello table by providing an object with populated data in real time as hello user searches.</span></span>

### <a name="create-a-new-objective-c-file"></a><span data-ttu-id="2fdd8-211">Vytvoření nového souboru Objective C</span><span class="sxs-lookup"><span data-stu-id="2fdd8-211">Create a new Objective C file</span></span>
<span data-ttu-id="2fdd8-212">Název souboru hello `GraphAPICaller.m`a přidejte následující metodu hello.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-212">Name hello file `GraphAPICaller.m`, and add hello following method.</span></span>

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

<span data-ttu-id="2fdd8-213">Prostřednictvím této metody podrobně přejděte.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-213">Let's go through this method in detail.</span></span>

<span data-ttu-id="2fdd8-214">základní Hello tohoto kódu je v hello `NXOAuth2Request`, metoda, která přebírá hello parametry, které jste si již definována v hello settings.plist souboru.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-214">hello core of this code is in hello `NXOAuth2Request`, method which takes hello parameters that you've already defined in hello settings.plist file.</span></span>

<span data-ttu-id="2fdd8-215">prvním krokem Hello je tooconstruct hello správné volání rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-215">hello first step is tooconstruct hello right Graph API call.</span></span> <span data-ttu-id="2fdd8-216">Protože jsou volání `/users`, můžete určit, že přidáním prostředků rozhraní Graph API toohello společně s verzí hello.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-216">Because you are calling `/users`, you specify that by appending it toohello Graph API resource along with hello version.</span></span> <span data-ttu-id="2fdd8-217">Má smysl tooput v souboru s nastavením externí vzhledem k tomu, že tyto můžete změnit podle zpracovaní hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-217">It makes sense tooput these in an external settings file because these can change as hello API evolves.</span></span>

```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

<span data-ttu-id="2fdd8-218">Dále je nutné zadat parametry toospecify, budete také poskytovat toohello volání rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-218">Next, you need toospecify parameters that you will also provide toohello Graph API call.</span></span> <span data-ttu-id="2fdd8-219">Je *velmi důležité* neumisťujte hello parametry v koncovém bodu hello prostředků, vzhledem k tomu, který se očistí pro všechny znaky vyhovující – identifikátor URI za běhu.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-219">It is *very important* that you do not put hello parameters in hello resource endpoint because that is scrubbed for all non-URI conforming characters at runtime.</span></span> <span data-ttu-id="2fdd8-220">Všechny kód dotazu je třeba zadat v parametrech hello.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-220">All query code must be provided in hello parameters.</span></span>

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

<span data-ttu-id="2fdd8-221">Možná jste si všimli volá `convertParamsToDictionary` metoda, která nebyly dosud zapsána.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-221">You might notice this calls a `convertParamsToDictionary` method that you haven't written yet.</span></span> <span data-ttu-id="2fdd8-222">Pojďme se proto nyní na konci hello hello souboru:</span><span class="sxs-lookup"><span data-stu-id="2fdd8-222">Let's do so now at hello end of hello file:</span></span>

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
<span data-ttu-id="2fdd8-223">V dalším kroku použijeme hello `NXOAuth2Request` metoda tooget data zpět z hello rozhraní API ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-223">Next, let's use hello `NXOAuth2Request` method tooget data back from hello API in JSON format.</span></span>

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

<span data-ttu-id="2fdd8-224">Nakonec se podíváme, jak vrátit hello data toohello MasterViewController.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-224">Finally, let's look at how you return hello data toohello MasterViewController.</span></span> <span data-ttu-id="2fdd8-225">Vrátí jako serializovat Hello dat a je třeba toobe deserializovat a načíst v objektu, že hello MainViewController spotřebovat.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-225">hello data returns as serialized and needs toobe deserialized and loaded in an object that hello MainViewController can consume.</span></span> <span data-ttu-id="2fdd8-226">Pro tento účel hello kostru má `User.m/h` soubor, který vytvoří objekt uživatele.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-226">For this purpose, hello skeleton has a `User.m/h` file that creates a User object.</span></span> <span data-ttu-id="2fdd8-227">Můžete naplnit tohoto objektu uživatele s informacemi z hello grafu.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-227">You populate that User object with information from hello graph.</span></span>

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


## <a name="run-hello-sample"></a><span data-ttu-id="2fdd8-228">Spuštění ukázkové hello</span><span class="sxs-lookup"><span data-stu-id="2fdd8-228">Run hello sample</span></span>
<span data-ttu-id="2fdd8-229">Pokud jste používá hello kostru nebo a potom společně s hello návod měli spustit nyní vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-229">If you've used hello skeleton or followed along with hello walkthrough your application should now run.</span></span> <span data-ttu-id="2fdd8-230">Spusťte hello simulátoru a klikněte na **přihlášení** toouse hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-230">Start hello simulator and click **Sign in** toouse hello application.</span></span>

## <a name="get-security-updates-for-our-product"></a><span data-ttu-id="2fdd8-231">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="2fdd8-231">Get security updates for our product</span></span>
<span data-ttu-id="2fdd8-232">Doporučujeme vám tooget oznámení o bezpečnostních incidentech návštěvou hello [Web Security TechCenter](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="2fdd8-232">We encourage you tooget notifications of when security incidents occur by visiting hello [Security TechCenter](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

