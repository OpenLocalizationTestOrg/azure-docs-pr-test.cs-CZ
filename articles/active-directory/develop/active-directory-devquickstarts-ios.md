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
# <a name="integrate-azure-ad-into-an-ios-app"></a><span data-ttu-id="0032e-103">Integrace Azure AD do aplikace pro iOS</span><span class="sxs-lookup"><span data-stu-id="0032e-103">Integrate Azure AD into an iOS app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> <span data-ttu-id="0032e-104">Vyzkoušení verze preview hello naší nové [portál pro vývojáře](https://identity.microsoft.com/Docs/iOS) který vám pomůže spuštěná v Azure Active Directory v několika málo minut!</span><span class="sxs-lookup"><span data-stu-id="0032e-104">Try hello preview of our new [developer portal](https://identity.microsoft.com/Docs/iOS) that helps you get up and running with Azure Active Directory in just a few minutes!</span></span>  <span data-ttu-id="0032e-105">portál pro vývojáře Hello vás provede procesem hello registrace aplikace a integraci služby Azure AD do vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="0032e-105">hello developer portal guides you through hello process of registering an app and integrating Azure AD into your code.</span></span>  <span data-ttu-id="0032e-106">Jakmile budete hotovi, budete mít jednoduchou aplikaci, která může ověřit uživatele v klientovi a back-end, které mohou přijímat tokeny a provést ověření.</span><span class="sxs-lookup"><span data-stu-id="0032e-106">When you’re finished, you'll have a simple application that can authenticate users in your tenant and a backend that can accept tokens and perform validation.</span></span> 
> 
> 

<span data-ttu-id="0032e-107">Azure Active Directory (Azure AD) poskytuje hello knihovna ověřování Active Directory nebo ADAL pro iOS klienti, kteří potřebují tooaccess chráněné zdroje.</span><span class="sxs-lookup"><span data-stu-id="0032e-107">Azure Active Directory (Azure AD) provides hello Active Directory Authentication Library, or ADAL, for iOS clients that need tooaccess protected resources.</span></span> <span data-ttu-id="0032e-108">ADAL zjednodušuje proces hello, že vaše aplikace používá tooobtain přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="0032e-108">ADAL simplifies hello process that your app uses tooobtain access tokens.</span></span> <span data-ttu-id="0032e-109">toodemonstrate jak snadné je v tomto článku jsme sestavit seznam úkolů Objective C aplikaci, která:</span><span class="sxs-lookup"><span data-stu-id="0032e-109">toodemonstrate how easy it is, in this article we build an Objective C To-Do List application that:</span></span>

* <span data-ttu-id="0032e-110">Získá přístup k tokeny pro volání rozhraní API Azure AD Graph hello pomocí hello [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="0032e-110">Gets access tokens for calling hello Azure AD Graph API by using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="0032e-111">Vyhledá adresář pro uživatele s danou alias.</span><span class="sxs-lookup"><span data-stu-id="0032e-111">Searches a directory for users with a given alias.</span></span>

<span data-ttu-id="0032e-112">toobuild hello dokončení pracovní aplikace, budete muset:</span><span class="sxs-lookup"><span data-stu-id="0032e-112">toobuild hello complete working application, you need to:</span></span>

1. <span data-ttu-id="0032e-113">Registrace vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0032e-113">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="0032e-114">Nainstalujte a nakonfigurujte ADAL.</span><span class="sxs-lookup"><span data-stu-id="0032e-114">Install and configure ADAL.</span></span>
3. <span data-ttu-id="0032e-115">Pomocí ADAL tooget tokeny z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0032e-115">Use ADAL tooget tokens from Azure AD.</span></span>

<span data-ttu-id="0032e-116">spuštění, tooget [stáhnout kostru aplikace hello](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) nebo [stažení ukázky hello Dokončit](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="0032e-116">tooget started, [download hello app skeleton](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span></span> <span data-ttu-id="0032e-117">Musíte taky klient služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0032e-117">You also need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="0032e-118">Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="0032e-118">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>


> [!TIP]
> <span data-ttu-id="0032e-119">Vyzkoušení verze preview hello naší nové [portál pro vývojáře](https://identity.microsoft.com/Docs/iOS) , umožňuje zprovoznění s Azure AD za několik minut.</span><span class="sxs-lookup"><span data-stu-id="0032e-119">Try hello preview of our new [developer portal](https://identity.microsoft.com/Docs/iOS) that helps you get up and running with Azure AD in just a few minutes.</span></span> <span data-ttu-id="0032e-120">portál pro vývojáře Hello vás provede procesem hello registrace aplikace a integraci služby Azure AD do vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="0032e-120">hello developer portal guides you through hello process of registering an app and integrating Azure AD into your code.</span></span> <span data-ttu-id="0032e-121">Až budete hotoví, budete mít jednoduchou aplikaci, která může ověřit uživatele ve vašem klientovi a back-end, můžete přijímat tokeny a provést ověření.</span><span class="sxs-lookup"><span data-stu-id="0032e-121">When you’re finished, you'll have a simple application that can authenticate users in your tenant, and a back end that can accept tokens and perform validation.</span></span> 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a><span data-ttu-id="0032e-122">1. Určit, jaké vaše přesměrování je identifikátor URI pro iOS</span><span class="sxs-lookup"><span data-stu-id="0032e-122">1. Determine what your redirect URI is for iOS</span></span>
<span data-ttu-id="0032e-123">toosecurely spuštění aplikace v některých scénářích jednotné přihlašování, musíte vytvořit *identifikátor URI pro přesměrování* v určitém formátu.</span><span class="sxs-lookup"><span data-stu-id="0032e-123">toosecurely start your applications in certain SSO scenarios, you must create a *redirect URI* in a particular format.</span></span> <span data-ttu-id="0032e-124">Přesměrování identifikátor URI je použité tooensure, který hello návratový toohello tokeny správné se aplikace, která je žádali.</span><span class="sxs-lookup"><span data-stu-id="0032e-124">A redirect URI is used tooensure that hello tokens return toohello correct application that asked for them.</span></span>


<span data-ttu-id="0032e-125">Formát iOS Hello přesměrování je identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="0032e-125">hello iOS format for a redirect URI is:</span></span>

```
<app-scheme>://<bundle-id>
```

* <span data-ttu-id="0032e-126">**aplikace – schéma** – to je zaregistrován ve vašem projektu XCode.</span><span class="sxs-lookup"><span data-stu-id="0032e-126">**app-scheme** - This is registered in your XCode project.</span></span> <span data-ttu-id="0032e-127">Je, jak jiné aplikace může volat.</span><span class="sxs-lookup"><span data-stu-id="0032e-127">It is how other applications can call you.</span></span> <span data-ttu-id="0032e-128">Můžete najít to pod Info.plist -> adresa URL typy -> identifikátoru adresy URL.</span><span class="sxs-lookup"><span data-stu-id="0032e-128">You can find this under Info.plist -> URL types -> URL Identifier.</span></span> <span data-ttu-id="0032e-129">Pokud ještě nemáte jeden nebo více nakonfigurované byste měli vytvořit jednu.</span><span class="sxs-lookup"><span data-stu-id="0032e-129">You should create one if you don't already have one or more configured.</span></span>
* <span data-ttu-id="0032e-130">**id sady** -Toto je identifikátor svazku v části "identity" hello zrušit nastavení projektu v XCode.</span><span class="sxs-lookup"><span data-stu-id="0032e-130">**bundle-id** - This is hello Bundle Identifier found under "identity" un your project settings in XCode.</span></span>

<span data-ttu-id="0032e-131">Příklad pro tento kód rychlý start: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***</span><span class="sxs-lookup"><span data-stu-id="0032e-131">An example for this QuickStart code: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***</span></span>

## <a name="2-register-hello-directorysearcher-application"></a><span data-ttu-id="0032e-132">2. Registrace aplikace DirectorySearcher hello</span><span class="sxs-lookup"><span data-stu-id="0032e-132">2. Register hello DirectorySearcher application</span></span>
<span data-ttu-id="0032e-133">tooset až tokeny tooget vaší aplikace, musíte nejprve tooregister ji ve službě Azure AD klienta a udělit mu oprávnění tooaccess hello Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="0032e-133">tooset up your app tooget tokens, you first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API:</span></span>

1. <span data-ttu-id="0032e-134">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0032e-134">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="0032e-135">Na horním panelu hello klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="0032e-135">On hello top bar, click your account.</span></span> <span data-ttu-id="0032e-136">V části hello **Directory** vyberte místo, kam chcete tooregister klienta služby Active Directory hello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0032e-136">Under hello **Directory** list, choose hello Active Directory tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="0032e-137">Klikněte na tlačítko **více služeb** v hello levém navigačním podokně a pak vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0032e-137">Click **More Services** in hello leftmost navigation pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="0032e-138">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0032e-138">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="0032e-139">Postupujte podle hello vyzve toocreate nový **nativní klientská aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0032e-139">Follow hello prompts toocreate a new **Native Client Application**.</span></span>
  * <span data-ttu-id="0032e-140">Hello **název** z hello aplikace popisuje tooend uživatelů vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0032e-140">hello **Name** of hello application describes your application tooend users.</span></span>
  * <span data-ttu-id="0032e-141">Hello **identifikátor Uri pro přesměrování** schématu a řetězec kombinací, Azure AD používá tooreturn odpovědi tokenu.</span><span class="sxs-lookup"><span data-stu-id="0032e-141">hello **Redirect Uri** is a scheme and string combination that Azure AD uses tooreturn token responses.</span></span>  <span data-ttu-id="0032e-142">Zadejte hodnotu, která je konkrétní tooyour aplikace která je založena na hello předchozí informace o identifikátor URI přesměrování.</span><span class="sxs-lookup"><span data-stu-id="0032e-142">Enter a value that is specific tooyour application and is based on hello previous redirect URI information.</span></span>
6. <span data-ttu-id="0032e-143">Po dokončení registrace hello, Azure AD přiřadí vaší aplikace ID jedinečný aplikace.</span><span class="sxs-lookup"><span data-stu-id="0032e-143">After you've completed hello registration, Azure AD assigns your app a unique application ID.</span></span>  <span data-ttu-id="0032e-144">Tuto hodnotu budete potřebovat v dalších částech hello, takže zkopírujte jej z karty aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="0032e-144">You'll need this value in hello next sections, so copy it from hello application tab.</span></span>
7. <span data-ttu-id="0032e-145">Z hello **nastavení** vyberte **požadovaných oprávnění** a pak vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="0032e-145">From hello **Settings** page, select **Required Permissions** and then select **Add**.</span></span> <span data-ttu-id="0032e-146">Vyberte **Microsoft Graph** jako hello rozhraní API a poté přidejte hello **čtení dat adresáře** oprávnění v rámci **delegovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="0032e-146">Select **Microsoft Graph** as hello API, and then add hello **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="0032e-147">Toto nastaví vaší hello tooquery aplikace Azure AD Graph API pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="0032e-147">This sets up your application tooquery hello Azure AD Graph API for users.</span></span>

## <a name="3-install-and-configure-adal"></a><span data-ttu-id="0032e-148">3. Instalace a konfigurace ADAL</span><span class="sxs-lookup"><span data-stu-id="0032e-148">3. Install and configure ADAL</span></span>
<span data-ttu-id="0032e-149">Teď, když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.</span><span class="sxs-lookup"><span data-stu-id="0032e-149">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="0032e-150">ADAL toocommunicate s Azure AD, musíte tooprovide její některé informace o registraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0032e-150">For ADAL toocommunicate with Azure AD, you need tooprovide it with some information about your app registration.</span></span>

1. <span data-ttu-id="0032e-151">Začněte tím, že přidání ADAL toohello DirectorySearcher projektu pomocí CocoaPods.</span><span class="sxs-lookup"><span data-stu-id="0032e-151">Begin by adding ADAL toohello DirectorySearcher project by using CocoaPods.</span></span>

    ```
    $ vi Podfile
    ```
2. <span data-ttu-id="0032e-152">Přidejte následující toothis podfile hello:</span><span class="sxs-lookup"><span data-stu-id="0032e-152">Add hello following toothis podfile:</span></span>

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. <span data-ttu-id="0032e-153">Nyní načtěte hello podfile pomocí CocoaPods.</span><span class="sxs-lookup"><span data-stu-id="0032e-153">Now load hello podfile by using CocoaPods.</span></span> <span data-ttu-id="0032e-154">Tento krok vytvoří nový pracovní prostor XCode, který můžete načíst.</span><span class="sxs-lookup"><span data-stu-id="0032e-154">This step creates a new XCode workspace that you load.</span></span>

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. <span data-ttu-id="0032e-155">V projektu typu rychlý start hello, otevřete soubor plist hello `settings.plist`.</span><span class="sxs-lookup"><span data-stu-id="0032e-155">In hello QuickStart project, open hello plist file `settings.plist`.</span></span>  <span data-ttu-id="0032e-156">Nahraďte hodnoty hello hello elementů v hello části tooreflect hello hodnoty, které jste zadali v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0032e-156">Replace hello values of hello elements in hello section tooreflect hello values that you entered in hello Azure portal.</span></span> <span data-ttu-id="0032e-157">Váš kód odkazuje na tyto hodnoty vždy, když ho využívá ADAL.</span><span class="sxs-lookup"><span data-stu-id="0032e-157">Your code references these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="0032e-158">Hello `tenant` je hello domény klienta služby Azure AD, například contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="0032e-158">hello `tenant` is hello domain of your Azure AD tenant, for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="0032e-159">Hello `clientId` je hello ID klienta aplikace, který jste zkopírovali z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="0032e-159">hello `clientId` is hello client ID of your application that you copied from hello portal.</span></span>
  * <span data-ttu-id="0032e-160">Hello `redirectUri` je hello přesměrování URL, která jste zaregistrovali hello portálu.</span><span class="sxs-lookup"><span data-stu-id="0032e-160">hello `redirectUri` is hello redirect URL that you registered in hello portal.</span></span>

## <a name="4----use-adal-tooget-tokens-from-azure-ad"></a><span data-ttu-id="0032e-161">4.    Použití ADAL tooget tokeny z Azure AD</span><span class="sxs-lookup"><span data-stu-id="0032e-161">4.    Use ADAL tooget tokens from Azure AD</span></span>
<span data-ttu-id="0032e-162">Hello základní princip za ADAL je, že vždy, když aplikace potřebuje přístupový token, jednoduše volá completionBlock `+(void) getToken : `, a ADAL hello rest.</span><span class="sxs-lookup"><span data-stu-id="0032e-162">hello basic principle behind ADAL is that whenever your app needs an access token, it simply calls a completionBlock `+(void) getToken : `, and ADAL does hello rest.</span></span>  

1. <span data-ttu-id="0032e-163">V hello `QuickStart` projekt, otevřete `GraphAPICaller.m` a vyhledejte hello `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` komentář v horní hello.</span><span class="sxs-lookup"><span data-stu-id="0032e-163">In hello `QuickStart` project, open `GraphAPICaller.m` and locate hello `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` comment near hello top.</span></span>  <span data-ttu-id="0032e-164">Toto je, kde můžete předat ADAL hello souřadnice prostřednictvím CompletionBlock, toocommunicate s Azure AD a určit, jak toocache tokeny.</span><span class="sxs-lookup"><span data-stu-id="0032e-164">This is where you pass ADAL hello coordinates through a CompletionBlock, toocommunicate with Azure AD, and tell it how toocache tokens.</span></span>

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

2. <span data-ttu-id="0032e-165">Nyní potřebujeme toouse tento token toosearch pro uživatele v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="0032e-165">Now we need toouse this token toosearch for users in hello graph.</span></span> <span data-ttu-id="0032e-166">Najde hello `// TODO: implement SearchUsersList` komentář.</span><span class="sxs-lookup"><span data-stu-id="0032e-166">Find hello `// TODO: implement SearchUsersList` comment.</span></span> <span data-ttu-id="0032e-167">Tato metoda vytváří tooquery toohello Azure AD Graph API požadavek GET pro uživatele, jehož UPN začíná hello zadaný hledaný termín.</span><span class="sxs-lookup"><span data-stu-id="0032e-167">This method makes a GET request toohello Azure AD Graph API tooquery for users whose UPN begins with hello given search term.</span></span>  <span data-ttu-id="0032e-168">tooquery hello Azure AD Graph API, musíte tooinclude access_token v hello `Authorization` hlavičky požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="0032e-168">tooquery hello Azure AD Graph API, you need tooinclude an access_token in hello `Authorization` header of hello request.</span></span> <span data-ttu-id="0032e-169">Toto je, kde odeslán ADAL.</span><span class="sxs-lookup"><span data-stu-id="0032e-169">This is where ADAL comes in.</span></span>

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


3. <span data-ttu-id="0032e-170">Když vaše aplikace vyžaduje token voláním `getToken(...)`, ADAL pokusí tooreturn token bez nutnosti hello uživatelské přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0032e-170">When your app requests a token by calling `getToken(...)`, ADAL attempts tooreturn a token without asking hello user for credentials.</span></span>  <span data-ttu-id="0032e-171">Pokud ADAL zjistí, že tento uživatel hello je toosign v tooget token, bude zobrazit dialogové okno pro přihlášení, shromažďování přihlašovacích údajů uživatele hello a vrátíte se po úspěšném ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="0032e-171">If ADAL determines that hello user needs toosign in tooget a token, it will display a dialog box for sign-in, collect hello user's credentials, and then return a token after successful authentication.</span></span>  <span data-ttu-id="0032e-172">Pokud ADAL není možné tooreturn token z jakéhokoli důvodu, že nastane `AdalException`.</span><span class="sxs-lookup"><span data-stu-id="0032e-172">If ADAL is not able tooreturn a token for any reason, it throws an `AdalException`.</span></span>

> [!Note] 
> <span data-ttu-id="0032e-173">Hello `AuthenticationResult` objekt obsahuje `tokenCacheStoreItem` objekt, který lze použít toocollect hello informace, které může být nutné vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0032e-173">hello `AuthenticationResult` object contains a `tokenCacheStoreItem` object that can be used toocollect hello information that your app may need.</span></span> <span data-ttu-id="0032e-174">V hello rychlý Start `tokenCacheStoreItem` je použité toodetermine Pokud ověřování již probíhá.</span><span class="sxs-lookup"><span data-stu-id="0032e-174">In hello QuickStart, `tokenCacheStoreItem` is used toodetermine if authentication is already done.</span></span>
>
>

## <a name="5-build-and-run-hello-application"></a><span data-ttu-id="0032e-175">5. Sestavení a spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="0032e-175">5. Build and run hello application</span></span>
<span data-ttu-id="0032e-176">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="0032e-176">Congratulations!</span></span> <span data-ttu-id="0032e-177">Teď máte funkční aplikaci iOS, která můžete ověřovat uživatele, bezpečně volání webového rozhraní API pomocí standardu OAuth 2.0 a získat základní informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="0032e-177">You now have a working iOS application that can authenticate users, securely call Web APIs by using OAuth 2.0, and get basic information about hello user.</span></span>  <span data-ttu-id="0032e-178">Pokud jste to ještě neudělali, nyní je čas toopopulate hello vašeho klienta s některými uživateli.</span><span class="sxs-lookup"><span data-stu-id="0032e-178">If you haven't already, now is hello time toopopulate your tenant with some users.</span></span>  <span data-ttu-id="0032e-179">Spusťte aplikaci rychlý start a pak se přihlaste pomocí jeden z těchto uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0032e-179">Start your QuickStart app, and then sign in with one of those users.</span></span>  <span data-ttu-id="0032e-180">Hledání jiných uživatelů podle jejich UPN.</span><span class="sxs-lookup"><span data-stu-id="0032e-180">Search for other users based on their UPN.</span></span>  <span data-ttu-id="0032e-181">Zavření aplikace hello a pak spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="0032e-181">Close hello app, and then start it again.</span></span>  <span data-ttu-id="0032e-182">Všimněte si, že hello uživatelské relace zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="0032e-182">Notice that hello user's session remains intact.</span></span>

<span data-ttu-id="0032e-183">ADAL umožňuje snadno tooincorporate všechny tyto běžné funkce identity do aplikace.</span><span class="sxs-lookup"><span data-stu-id="0032e-183">ADAL makes it easy tooincorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="0032e-184">Se postará všechny pracovní dirty hello, jako je Správa mezipaměti podpora protokolu OAuth, prezentuje toosign uživatelského rozhraní v hello uživatele a aktualizaci vypršení platnosti tokenů.</span><span class="sxs-lookup"><span data-stu-id="0032e-184">It takes care of all hello dirty work for you, like cache management, OAuth protocol support, presenting hello user with a UI toosign in, and refreshing expired tokens.</span></span>  <span data-ttu-id="0032e-185">Všechny skutečně potřebujete tooknow je jednoho volání rozhraní API `getToken`.</span><span class="sxs-lookup"><span data-stu-id="0032e-185">All you really need tooknow is a single API call, `getToken`.</span></span>

<span data-ttu-id="0032e-186">Pro odkaz, hello dokončit ukázka (bez vašich hodnot nastavení) zajišťuje na [Githubu](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="0032e-186">For reference, hello completed sample (without your configuration values) is provided on [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span></span>  

## <a name="next-steps"></a><span data-ttu-id="0032e-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0032e-187">Next steps</span></span>
<span data-ttu-id="0032e-188">Nyní se můžete přesunout na tooadditional scénáře.</span><span class="sxs-lookup"><span data-stu-id="0032e-188">You can now move on tooadditional scenarios.</span></span>  <span data-ttu-id="0032e-189">Může být vhodné tootry:</span><span class="sxs-lookup"><span data-stu-id="0032e-189">You may want tootry:</span></span>

* [<span data-ttu-id="0032e-190">Zabezpečení webové aplikace Node.JS API s Azure AD</span><span class="sxs-lookup"><span data-stu-id="0032e-190">Secure a Node.JS Web API with Azure AD</span></span>](active-directory-devquickstarts-webapi-nodejs.md)
* <span data-ttu-id="0032e-191">Další informace [jak tooenable jednotného přihlašování napříč aplikacemi v systému iOS pomocí ADAL](active-directory-sso-ios.md)</span><span class="sxs-lookup"><span data-stu-id="0032e-191">Learn [how tooenable cross-app SSO on iOS using ADAL](active-directory-sso-ios.md)</span></span>  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

