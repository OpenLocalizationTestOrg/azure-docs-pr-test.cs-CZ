---
title: "aaaGet práce s hello knihovně CDN Azure pro .NET | Microsoft Docs"
description: "Zjistěte, jak toowrite .NET aplikací toomanage CDN Azure pomocí sady Visual Studio."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 63cf4101-92e7-49dd-a155-a90e54a792ca
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 9753e48c7469072cef6b2ac728e18c78121c97f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-cdn-development"></a>Začínáme s vývojem pro Azure CDN
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

Můžete použít hello [knihovny CDN Azure pro .NET](https://msdn.microsoft.com/library/mt657769.aspx) tooautomate vytváření a Správa profilů CDN a koncové body.  Tento kurz vás provede hello vytvoření jednoduché konzolové aplikace .NET, které ukazuje několik operací hello k dispozici.  V tomto kurzu není určený toodescribe všechny aspekty hello knihovně CDN Azure pro .NET podrobně.

Je nutné v tomto kurzu toocomplete Visual Studio 2015.  [Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) je dostupná volně ke stažení.

> [!TIP]
> Hello [dokončený projekt z tohoto kurzu](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) je k dispozici ke stažení na webu MSDN.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Vytvoření projektu a přidání balíčků Nuget
Teď, když jsme vytvořili skupinu prostředků pro naše profily CDN a daného naše profilů CDN toomanage oprávnění aplikace Azure AD a koncové body v rámci dané skupiny, můžeme začít vytváření aplikace.

Z v rámci sady Visual Studio 2015, klikněte na **soubor**, **nový**, **projektu...**  tooopen hello dialogové okno Nový projekt.  Rozbalte položku **Visual C#**, pak vyberte **Windows** v podokně hello na levé straně hello.  Klikněte na tlačítko **konzolové aplikace** v prostředním podokně hello.  Název projektu a pak klikněte na **OK**.  

![Nový projekt](./media/cdn-app-dev-net/cdn-new-project.png)

Naše projektu přechází toouse některé knihovny Azure, které jsou součástí balíčků Nuget.  Umožňuje přidat tyto toohello projekt.

1. Klikněte na tlačítko hello **nástroje** nabídce **Správce balíčků Nuget**, pak **Konzola správce balíčků**.
   
    ![Správa balíčků Nuget](./media/cdn-app-dev-net/cdn-manage-nuget.png)
2. V hello Konzola správce balíčků, spustit následující příkaz tooinstall hello hello **Active Directory Authentication Library (ADAL)**:
   
    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`
3. Provést následující tooinstall hello hello **Knihovna správy Azure CDN**:
   
    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Direktivy, konstanty, hlavní metoda a pomocné metody
Pojďme hello základní struktura naše programu zapsána.

1. Zpět na kartě Program.cs hello, nahraďte hello `using` direktivy v horní části hello s hello následující:
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
2. Potřebujeme toodefine některé konstanty, který bude používat naše metody.  V hello `Program` třídy, ale před hello `Main` metoda, přidejte následující hello.  Být jisti tooreplace hello zástupné symboly, včetně hello  **&lt;lomené závorky&gt;**, s vlastní hodnoty, podle potřeby.
   
    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";
   
    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```
3. Také na úrovni třídy hello, definujte tyto dvě proměnné.  Použijeme tyto novější toodetermine naše profilu a koncového bodu už existuje.
   
    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```
4. Nahraďte hello `Main` metoda následujícím způsobem:
   
   ```csharp
   static void Main(string[] args)
   {
       //Get a token
       AuthenticationResult authResult = GetAccessToken();
   
       // Create CDN client
       CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
           { SubscriptionId = subscriptionId };
   
       ListProfilesAndEndpoints(cdn);
   
       // Create CDN Profile
       CreateCdnProfile(cdn);
   
       // Create CDN Endpoint
       CreateCdnEndpoint(cdn);
   
       Console.WriteLine();
   
       // Purge CDN Endpoint
       PromptPurgeCdnEndpoint(cdn);
   
       // Delete CDN Endpoint
       PromptDeleteCdnEndpoint(cdn);
   
       // Delete CDN Profile
       PromptDeleteCdnProfile(cdn);
   
       Console.WriteLine("Press Enter tooend program.");
       Console.ReadLine();
   }
   ```
5. Některé z našich dalších metod budou tooprompt hello uživatele s dotazy "Ano".  Přidejte následující metodu toomake hello který trochu snazší:
   
    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Teď, když je zapsán hello základní struktura naše programu, by měl vytvoříme hello metody volány hello `Main` metoda.

## <a name="authentication"></a>Authentication
Před použitím jsme hello Knihovna správy Azure CDN, budeme potřebovat tooauthenticate naši službu hlavní a získat ověřovací token.  Tato metoda používá ADAL tooretrieve hello token.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Pokud používáte ověřování jednotlivých uživatelů, hello `GetAccessToken` metoda bude vypadat mírně lišit.

> [!IMPORTANT]
> Tato ukázka kódu použijte, pouze pokud zvolíte ověřování jednotlivých uživatelů toohave místo objekt služby.
> 
> 

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Být jisti tooreplace `<redirect URI>` s hello jste zadali při registraci aplikace hello ve službě Azure AD identifikátor URI pro přesměrování.

## <a name="list-cdn-profiles-and-endpoints"></a>Koncové body a seznam profilů CDN
Nyní jsme připravené tooperform CDN operace.  Hello první věc, kterou naše metoda nemá je seznam všech profilů hello a koncových bodů v naší skupiny prostředků a pokud najde shoda pro hello profilu a koncového bodu názvy zadané v našem konstanty zajišťuje Poznámka této pro později tak jsme nepokoušejte toocreate duplikáty.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all hello CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's hello name of hello CDN profile we want toocreate!
            profileAlreadyExists = true;
        }

        //List all hello CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // hello unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Vytvoření profilů CDN a koncové body
V dalším kroku vytvoříme profil.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Po vytvoření profilu hello vytvoříme koncový bod.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

> [!NOTE]
> výše uvedený příklad Hello přiřadí hello koncový bod počátek s názvem *Contoso* s hostitele `www.contoso.com`.  Měli byste změnit toto toopoint tooyour vlastní název počátečního na hostitele.
> 
> 

## <a name="purge-an-endpoint"></a>Vyprázdnění koncového bodu
Za předpokladu, že byl vytvořen koncový bod hello, běžných úloh, může chceme tooperform v našem programu je vyprazdňování hello obsah v našem koncový bod.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

> [!NOTE]
> V předchozím příkladu hello, hello řetězec `/*` označuje, že chcete toopurge všechno hello kořenové hello cesta ke koncovému bodu.  Toto je ekvivalentní toochecking **vyprázdnit všechny** v hello portál Azure "mazání" dialogové okno. V hello `CreateCdnProfile` metoda, vytvořené naše profilu jako **Azure CDN společnosti Verizon** profil pomocí kódu hello `Sku = new Sku(SkuName.StandardVerizon)`, takže to bude úspěšné.  Ale **Azure CDN společnosti Akamai** profily nepodporují **vyprázdnit všechny**, takže pokud použití profilem Akamai pro účely tohoto kurzu, by potřebuji toopurge tooinclude konkrétní cesty.
> 
> 

## <a name="delete-cdn-profiles-and-endpoints"></a>Odstranit profily CDN a koncové body
poslední metody Hello odstraní naše koncový bod a profil.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-hello-program"></a>Spuštění programu hello
Jsme teď zkompilování a spuštění programu hello kliknutím hello **spustit** tlačítko v sadě Visual Studio.

![Spuštění programu](./media/cdn-app-dev-net/cdn-program-running-1.png)

Když hello program dosáhne hello výše řádku, by měl být schopný tooreturn tooyour skupiny prostředků v hello portál Azure a najdete v části vytvořený profil hello.

![Úspěšné!](./media/cdn-app-dev-net/cdn-success.png)

Potom může potvrdit hello výzvy toorun hello rest programu hello.

![Dokončení programu](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Další kroky
Projekt hello dokončit toosee z tohoto návodu [stažení ukázky hello](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

Další dokumentaci toofind na hello Knihovna správy CDN Azure pro platformu .NET, zobrazení hello [odkaz na webu MSDN](https://msdn.microsoft.com/library/mt657769.aspx).

Spravovat prostředky CDN s [prostředí PowerShell](cdn-manage-powershell.md).

