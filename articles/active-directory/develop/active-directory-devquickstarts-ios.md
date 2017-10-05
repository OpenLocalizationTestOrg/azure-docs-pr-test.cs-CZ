---
title: Integrace Azure AD do aplikace pro iOS | Microsoft Docs
description: "Jak vytvářet aplikace pro iOS, který se integruje s Azure AD pro přihlášení a volání služby Azure AD chráněný rozhraní API pomocí OAuth."
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
ms.openlocfilehash: 57f465df99ac234466459b8031f61805d8334b59
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-azure-ad-into-an-ios-app"></a><span data-ttu-id="69bcb-103">Integrace Azure AD do aplikace pro iOS</span><span class="sxs-lookup"><span data-stu-id="69bcb-103">Integrate Azure AD into an iOS app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> <span data-ttu-id="69bcb-104">Vyzkoušejte verzi preview našeho nového [portál pro vývojáře](https://identity.microsoft.com/Docs/iOS) který vám pomůže spuštěná v Azure Active Directory v několika málo minut!</span><span class="sxs-lookup"><span data-stu-id="69bcb-104">Try the preview of our new [developer portal](https://identity.microsoft.com/Docs/iOS) that helps you get up and running with Azure Active Directory in just a few minutes!</span></span>  <span data-ttu-id="69bcb-105">Portál pro vývojáře vás provede procesem registrace aplikace a integraci služby Azure AD do vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="69bcb-105">The developer portal guides you through the process of registering an app and integrating Azure AD into your code.</span></span>  <span data-ttu-id="69bcb-106">Jakmile budete hotovi, budete mít jednoduchou aplikaci, která může ověřit uživatele v klientovi a back-end, které mohou přijímat tokeny a provést ověření.</span><span class="sxs-lookup"><span data-stu-id="69bcb-106">When you’re finished, you'll have a simple application that can authenticate users in your tenant and a backend that can accept tokens and perform validation.</span></span> 
> 
> 

<span data-ttu-id="69bcb-107">Azure Active Directory (Azure AD) poskytuje knihovna ověřování Active Directory nebo ADAL pro iOS klienti, kteří potřebují přístup k chráněným prostředkům.</span><span class="sxs-lookup"><span data-stu-id="69bcb-107">Azure Active Directory (Azure AD) provides the Active Directory Authentication Library, or ADAL, for iOS clients that need to access protected resources.</span></span> <span data-ttu-id="69bcb-108">ADAL zjednodušuje proces, který vaše aplikace používá k získání přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="69bcb-108">ADAL simplifies the process that your app uses to obtain access tokens.</span></span> <span data-ttu-id="69bcb-109">K předvedení toho, jak je snadné, v tomto článku jsme sestavit seznam úkolů Objective C aplikaci, která:</span><span class="sxs-lookup"><span data-stu-id="69bcb-109">To demonstrate how easy it is, in this article we build an Objective C To-Do List application that:</span></span>

* <span data-ttu-id="69bcb-110">Získá přístup k tokeny pro volání rozhraní API služby Azure AD Graph pomocí [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="69bcb-110">Gets access tokens for calling the Azure AD Graph API by using the [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="69bcb-111">Vyhledá adresář pro uživatele s danou alias.</span><span class="sxs-lookup"><span data-stu-id="69bcb-111">Searches a directory for users with a given alias.</span></span>

<span data-ttu-id="69bcb-112">Chcete-li vytvořit úplný funkční aplikaci, je potřeba:</span><span class="sxs-lookup"><span data-stu-id="69bcb-112">To build the complete working application, you need to:</span></span>

1. <span data-ttu-id="69bcb-113">Registrace vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="69bcb-113">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="69bcb-114">Nainstalujte a nakonfigurujte ADAL.</span><span class="sxs-lookup"><span data-stu-id="69bcb-114">Install and configure ADAL.</span></span>
3. <span data-ttu-id="69bcb-115">Pomocí ADAL získat tokeny z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="69bcb-115">Use ADAL to get tokens from Azure AD.</span></span>

<span data-ttu-id="69bcb-116">Abyste mohli začít, [stáhnout kostru aplikace](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) nebo [stažení je hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="69bcb-116">To get started, [download the app skeleton](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) or [download the completed sample](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span></span> <span data-ttu-id="69bcb-117">Musíte taky klient služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="69bcb-117">You also need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="69bcb-118">Pokud ještě nemáte klienta, [zjistěte, jak získat](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="69bcb-118">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>


> [!TIP]
> <span data-ttu-id="69bcb-119">Vyzkoušejte verzi preview našeho nového [portál pro vývojáře](https://identity.microsoft.com/Docs/iOS) , umožňuje zprovoznění s Azure AD za několik minut.</span><span class="sxs-lookup"><span data-stu-id="69bcb-119">Try the preview of our new [developer portal](https://identity.microsoft.com/Docs/iOS) that helps you get up and running with Azure AD in just a few minutes.</span></span> <span data-ttu-id="69bcb-120">Portál pro vývojáře vás provede procesem registrace aplikace a integraci služby Azure AD do vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="69bcb-120">The developer portal guides you through the process of registering an app and integrating Azure AD into your code.</span></span> <span data-ttu-id="69bcb-121">Až budete hotoví, budete mít jednoduchou aplikaci, která může ověřit uživatele ve vašem klientovi a back-end, můžete přijímat tokeny a provést ověření.</span><span class="sxs-lookup"><span data-stu-id="69bcb-121">When you’re finished, you'll have a simple application that can authenticate users in your tenant, and a back end that can accept tokens and perform validation.</span></span> 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a><span data-ttu-id="69bcb-122">1. Určit, jaké vaše přesměrování je identifikátor URI pro iOS</span><span class="sxs-lookup"><span data-stu-id="69bcb-122">1. Determine what your redirect URI is for iOS</span></span>
<span data-ttu-id="69bcb-123">Bezpečně spuštění aplikace v některých scénářích jednotné přihlašování, musíte vytvořit *identifikátor URI pro přesměrování* v určitém formátu.</span><span class="sxs-lookup"><span data-stu-id="69bcb-123">To securely start your applications in certain SSO scenarios, you must create a *redirect URI* in a particular format.</span></span> <span data-ttu-id="69bcb-124">Identifikátor URI pro přesměrování slouží k zajištění, že tokeny vrátit k správné aplikaci, která je žádali.</span><span class="sxs-lookup"><span data-stu-id="69bcb-124">A redirect URI is used to ensure that the tokens return to the correct application that asked for them.</span></span>


<span data-ttu-id="69bcb-125">Formát iOS pro přesměrování je identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="69bcb-125">The iOS format for a redirect URI is:</span></span>

```
<app-scheme>://<bundle-id>
```

* <span data-ttu-id="69bcb-126">**aplikace – schéma** – to je zaregistrován ve vašem projektu XCode.</span><span class="sxs-lookup"><span data-stu-id="69bcb-126">**app-scheme** - This is registered in your XCode project.</span></span> <span data-ttu-id="69bcb-127">Je, jak jiné aplikace může volat.</span><span class="sxs-lookup"><span data-stu-id="69bcb-127">It is how other applications can call you.</span></span> <span data-ttu-id="69bcb-128">Můžete najít to pod Info.plist -> adresa URL typy -> identifikátoru adresy URL.</span><span class="sxs-lookup"><span data-stu-id="69bcb-128">You can find this under Info.plist -> URL types -> URL Identifier.</span></span> <span data-ttu-id="69bcb-129">Pokud ještě nemáte jeden nebo více nakonfigurované byste měli vytvořit jednu.</span><span class="sxs-lookup"><span data-stu-id="69bcb-129">You should create one if you don't already have one or more configured.</span></span>
* <span data-ttu-id="69bcb-130">**id sady** -Toto je identifikátor svazku v části "identity" zrušení nastavení projektu v XCode.</span><span class="sxs-lookup"><span data-stu-id="69bcb-130">**bundle-id** - This is the Bundle Identifier found under "identity" un your project settings in XCode.</span></span>

<span data-ttu-id="69bcb-131">Příklad pro tento kód rychlý start: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***</span><span class="sxs-lookup"><span data-stu-id="69bcb-131">An example for this QuickStart code: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***</span></span>

## <a name="2-register-the-directorysearcher-application"></a><span data-ttu-id="69bcb-132">2. Registrace aplikace DirectorySearcher</span><span class="sxs-lookup"><span data-stu-id="69bcb-132">2. Register the DirectorySearcher application</span></span>
<span data-ttu-id="69bcb-133">Chcete-li nastavit aplikaci získat tokeny, musíte nejprve zaregistrovat v klientovi služby Azure AD a udělit mu oprávnění k přístupu k Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="69bcb-133">To set up your app to get tokens, you first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API:</span></span>

1. <span data-ttu-id="69bcb-134">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="69bcb-134">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="69bcb-135">Na horním panelu klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="69bcb-135">On the top bar, click your account.</span></span> <span data-ttu-id="69bcb-136">V části **Directory** vyberte klienta služby Active Directory, kde chcete registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="69bcb-136">Under the **Directory** list, choose the Active Directory tenant where you want to register your application.</span></span>
3. <span data-ttu-id="69bcb-137">Klikněte na tlačítko **více služeb** v levém navigačním podokně a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="69bcb-137">Click **More Services** in the leftmost navigation pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="69bcb-138">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="69bcb-138">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="69bcb-139">Postupujte podle výzev a vytvořte novou **nativní klientská aplikace**.</span><span class="sxs-lookup"><span data-stu-id="69bcb-139">Follow the prompts to create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="69bcb-140">**Název** aplikace popisuje vaší aplikace pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="69bcb-140">The **Name** of the application describes your application to end users.</span></span>
  * <span data-ttu-id="69bcb-141">**Identifikátor Uri pro přesměrování** je kombinace schématu a řetězec, Azure AD se používá k vrácení odpovědi tokenu.</span><span class="sxs-lookup"><span data-stu-id="69bcb-141">The **Redirect Uri** is a scheme and string combination that Azure AD uses to return token responses.</span></span>  <span data-ttu-id="69bcb-142">Zadejte hodnotu, která je specifický pro vaši aplikaci a je založena na předchozí informace o identifikátor URI přesměrování.</span><span class="sxs-lookup"><span data-stu-id="69bcb-142">Enter a value that is specific to your application and is based on the previous redirect URI information.</span></span>
6. <span data-ttu-id="69bcb-143">Po dokončení registrace Azure AD přiřadí vaší aplikace ID jedinečný aplikace.</span><span class="sxs-lookup"><span data-stu-id="69bcb-143">After you've completed the registration, Azure AD assigns your app a unique application ID.</span></span>  <span data-ttu-id="69bcb-144">Tuto hodnotu budete potřebovat v další části, zkopírujte jej na kartě aplikace.</span><span class="sxs-lookup"><span data-stu-id="69bcb-144">You'll need this value in the next sections, so copy it from the application tab.</span></span>
7. <span data-ttu-id="69bcb-145">Z **nastavení** vyberte **požadovaných oprávnění** a pak vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="69bcb-145">From the **Settings** page, select **Required Permissions** and then select **Add**.</span></span> <span data-ttu-id="69bcb-146">Vyberte **Microsoft Graph** jako rozhraní API a poté přidejte **čtení dat adresáře** oprávnění v rámci **delegovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="69bcb-146">Select **Microsoft Graph** as the API, and then add the **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="69bcb-147">Toto nastaví aplikace zpracovat dotaz rozhraní Azure AD Graph API pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="69bcb-147">This sets up your application to query the Azure AD Graph API for users.</span></span>

## <a name="3-install-and-configure-adal"></a><span data-ttu-id="69bcb-148">3. Instalace a konfigurace ADAL</span><span class="sxs-lookup"><span data-stu-id="69bcb-148">3. Install and configure ADAL</span></span>
<span data-ttu-id="69bcb-149">Teď, když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.</span><span class="sxs-lookup"><span data-stu-id="69bcb-149">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="69bcb-150">Pro ADAL ke komunikaci s Azure AD budete muset poskytnout některé informace o registraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="69bcb-150">For ADAL to communicate with Azure AD, you need to provide it with some information about your app registration.</span></span>

1. <span data-ttu-id="69bcb-151">Začněte tím, že přidání ADAL do projektu DirectorySearcher pomocí CocoaPods.</span><span class="sxs-lookup"><span data-stu-id="69bcb-151">Begin by adding ADAL to the DirectorySearcher project by using CocoaPods.</span></span>

    ```
    $ vi Podfile
    ```
2. <span data-ttu-id="69bcb-152">Do tohoto souboru podfile přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="69bcb-152">Add the following to this podfile:</span></span>

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. <span data-ttu-id="69bcb-153">Nyní načtěte podfile pomocí CocoaPods.</span><span class="sxs-lookup"><span data-stu-id="69bcb-153">Now load the podfile by using CocoaPods.</span></span> <span data-ttu-id="69bcb-154">Tento krok vytvoří nový pracovní prostor XCode, který můžete načíst.</span><span class="sxs-lookup"><span data-stu-id="69bcb-154">This step creates a new XCode workspace that you load.</span></span>

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. <span data-ttu-id="69bcb-155">V projektu pro rychlý start, otevřete soubor plist `settings.plist`.</span><span class="sxs-lookup"><span data-stu-id="69bcb-155">In the QuickStart project, open the plist file `settings.plist`.</span></span>  <span data-ttu-id="69bcb-156">Nahraďte hodnoty elementů v části tak, aby odrážela hodnoty, které jste zadali v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="69bcb-156">Replace the values of the elements in the section to reflect the values that you entered in the Azure portal.</span></span> <span data-ttu-id="69bcb-157">Váš kód odkazuje na tyto hodnoty vždy, když ho využívá ADAL.</span><span class="sxs-lookup"><span data-stu-id="69bcb-157">Your code references these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="69bcb-158">`tenant` Je doména klienta služby Azure AD, například contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="69bcb-158">The `tenant` is the domain of your Azure AD tenant, for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="69bcb-159">`clientId` Je ID klienta aplikace, který jste zkopírovali z portálu.</span><span class="sxs-lookup"><span data-stu-id="69bcb-159">The `clientId` is the client ID of your application that you copied from the portal.</span></span>
  * <span data-ttu-id="69bcb-160">`redirectUri` Je adresa URL přesměrování, který je zaregistrovaný v portálu.</span><span class="sxs-lookup"><span data-stu-id="69bcb-160">The `redirectUri` is the redirect URL that you registered in the portal.</span></span>

## <a name="4----use-adal-to-get-tokens-from-azure-ad"></a><span data-ttu-id="69bcb-161">4.    Získat tokeny z Azure AD pomocí ADAL</span><span class="sxs-lookup"><span data-stu-id="69bcb-161">4.    Use ADAL to get tokens from Azure AD</span></span>
<span data-ttu-id="69bcb-162">Základní princip za ADAL je, že vždy, když aplikace potřebuje přístupový token, jednoduše volá completionBlock `+(void) getToken : `, a zbývající ADAL.</span><span class="sxs-lookup"><span data-stu-id="69bcb-162">The basic principle behind ADAL is that whenever your app needs an access token, it simply calls a completionBlock `+(void) getToken : `, and ADAL does the rest.</span></span>  

1. <span data-ttu-id="69bcb-163">V `QuickStart` projekt, otevřete `GraphAPICaller.m` a najděte `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` komentář horní části.</span><span class="sxs-lookup"><span data-stu-id="69bcb-163">In the `QuickStart` project, open `GraphAPICaller.m` and locate the `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` comment near the top.</span></span>  <span data-ttu-id="69bcb-164">Toto je, kde je předat ADAL souřadnice prostřednictvím CompletionBlock, komunikovat s Azure AD, a určit, jak pro ukládání do mezipaměti tokenů.</span><span class="sxs-lookup"><span data-stu-id="69bcb-164">This is where you pass ADAL the coordinates through a CompletionBlock, to communicate with Azure AD, and tell it how to cache tokens.</span></span>

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
                        extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
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

2. <span data-ttu-id="69bcb-165">Teď je potřeba tento token slouží k vyhledání uživatele v grafu.</span><span class="sxs-lookup"><span data-stu-id="69bcb-165">Now we need to use this token to search for users in the graph.</span></span> <span data-ttu-id="69bcb-166">Najít `// TODO: implement SearchUsersList` komentář.</span><span class="sxs-lookup"><span data-stu-id="69bcb-166">Find the `// TODO: implement SearchUsersList` comment.</span></span> <span data-ttu-id="69bcb-167">Tato metoda vytváří požadavek GET na Azure AD Graph API k dotazu pro uživatele, jehož UPN začíná zadaný hledaný termín.</span><span class="sxs-lookup"><span data-stu-id="69bcb-167">This method makes a GET request to the Azure AD Graph API to query for users whose UPN begins with the given search term.</span></span>  <span data-ttu-id="69bcb-168">Zpracovat dotaz rozhraní Azure AD Graph API, je nutné zahrnout access_token v `Authorization` hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="69bcb-168">To query the Azure AD Graph API, you need to include an access_token in the `Authorization` header of the request.</span></span> <span data-ttu-id="69bcb-169">Toto je, kde odeslán ADAL.</span><span class="sxs-lookup"><span data-stu-id="69bcb-169">This is where ADAL comes in.</span></span>

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

                         // We can grab the JSON node at the top to get our graph data.
                         NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                         // Don't be thrown off by the key name being "value". It really is the name of the
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


3. <span data-ttu-id="69bcb-170">Když vaše aplikace vyžaduje token voláním `getToken(...)`, ADAL pokusí vrátit token bez požadavku uživatele na přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="69bcb-170">When your app requests a token by calling `getToken(...)`, ADAL attempts to return a token without asking the user for credentials.</span></span>  <span data-ttu-id="69bcb-171">Pokud ADAL zjistí, že uživatel musí pro přihlášení k získání tokenu, bude zobrazit dialogové okno pro přihlášení, shromažďování přihlašovacích údajů uživatele a pak se vraťte token po úspěšném ověření.</span><span class="sxs-lookup"><span data-stu-id="69bcb-171">If ADAL determines that the user needs to sign in to get a token, it will display a dialog box for sign-in, collect the user's credentials, and then return a token after successful authentication.</span></span>  <span data-ttu-id="69bcb-172">Pokud není možné vrátit token z jakéhokoli důvodu ADAL, vyvolá `AdalException`.</span><span class="sxs-lookup"><span data-stu-id="69bcb-172">If ADAL is not able to return a token for any reason, it throws an `AdalException`.</span></span>

> [!Note] 
> <span data-ttu-id="69bcb-173">`AuthenticationResult` Objekt obsahuje `tokenCacheStoreItem` objekt, který můžete použít ke shromažďování informací, které vaše aplikace může být nutné.</span><span class="sxs-lookup"><span data-stu-id="69bcb-173">The `AuthenticationResult` object contains a `tokenCacheStoreItem` object that can be used to collect the information that your app may need.</span></span> <span data-ttu-id="69bcb-174">V rychlé spuštění `tokenCacheStoreItem` slouží k určení, pokud je už hotové ověřování.</span><span class="sxs-lookup"><span data-stu-id="69bcb-174">In the QuickStart, `tokenCacheStoreItem` is used to determine if authentication is already done.</span></span>
>
>

## <a name="5-build-and-run-the-application"></a><span data-ttu-id="69bcb-175">5. Sestavení a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="69bcb-175">5. Build and run the application</span></span>
<span data-ttu-id="69bcb-176">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="69bcb-176">Congratulations!</span></span> <span data-ttu-id="69bcb-177">Teď máte funkční aplikaci iOS, která můžete ověřovat uživatele, bezpečně volání webového rozhraní API pomocí standardu OAuth 2.0 a získat základní informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="69bcb-177">You now have a working iOS application that can authenticate users, securely call Web APIs by using OAuth 2.0, and get basic information about the user.</span></span>  <span data-ttu-id="69bcb-178">Pokud jste to ještě neudělali, nyní je čas k naplnění vašeho klienta s některými uživateli.</span><span class="sxs-lookup"><span data-stu-id="69bcb-178">If you haven't already, now is the time to populate your tenant with some users.</span></span>  <span data-ttu-id="69bcb-179">Spusťte aplikaci rychlý start a pak se přihlaste pomocí jeden z těchto uživatelů.</span><span class="sxs-lookup"><span data-stu-id="69bcb-179">Start your QuickStart app, and then sign in with one of those users.</span></span>  <span data-ttu-id="69bcb-180">Hledání jiných uživatelů podle jejich UPN.</span><span class="sxs-lookup"><span data-stu-id="69bcb-180">Search for other users based on their UPN.</span></span>  <span data-ttu-id="69bcb-181">Zavřete aplikaci a pak spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="69bcb-181">Close the app, and then start it again.</span></span>  <span data-ttu-id="69bcb-182">Všimněte si, že uživatelské relace zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="69bcb-182">Notice that the user's session remains intact.</span></span>

<span data-ttu-id="69bcb-183">ADAL usnadňuje všechny tyto běžné funkce identity začlenit do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="69bcb-183">ADAL makes it easy to incorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="69bcb-184">Se postará všechnu práci dirty, jako je Správa mezipaměti podpora protokolu OAuth, představuje uživatele pomocí uživatelského rozhraní pro přihlášení, a aktualizovat platnost tokenů.</span><span class="sxs-lookup"><span data-stu-id="69bcb-184">It takes care of all the dirty work for you, like cache management, OAuth protocol support, presenting the user with a UI to sign in, and refreshing expired tokens.</span></span>  <span data-ttu-id="69bcb-185">Všechny skutečně potřebujete vědět, je jednoho volání rozhraní API `getToken`.</span><span class="sxs-lookup"><span data-stu-id="69bcb-185">All you really need to know is a single API call, `getToken`.</span></span>

<span data-ttu-id="69bcb-186">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) zajišťuje na [Githubu](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="69bcb-186">For reference, the completed sample (without your configuration values) is provided on [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span></span>  

## <a name="next-steps"></a><span data-ttu-id="69bcb-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69bcb-187">Next steps</span></span>
<span data-ttu-id="69bcb-188">Nyní se můžete přesunout dalších scénářů.</span><span class="sxs-lookup"><span data-stu-id="69bcb-188">You can now move on to additional scenarios.</span></span>  <span data-ttu-id="69bcb-189">Můžete se pokusit:</span><span class="sxs-lookup"><span data-stu-id="69bcb-189">You may want to try:</span></span>

* [<span data-ttu-id="69bcb-190">Zabezpečení webové aplikace Node.JS API s Azure AD</span><span class="sxs-lookup"><span data-stu-id="69bcb-190">Secure a Node.JS Web API with Azure AD</span></span>](active-directory-devquickstarts-webapi-nodejs.md)
* <span data-ttu-id="69bcb-191">Další informace [postup povolení jednotného přihlašování napříč aplikacemi v systému iOS pomocí ADAL](active-directory-sso-ios.md)</span><span class="sxs-lookup"><span data-stu-id="69bcb-191">Learn [how to enable cross-app SSO on iOS using ADAL](active-directory-sso-ios.md)</span></span>  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

