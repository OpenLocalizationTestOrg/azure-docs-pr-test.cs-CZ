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
# <a name="get-started-with-azure-cdn-development"></a><span data-ttu-id="5d3cb-103">Začínáme s vývojem pro Azure CDN</span><span class="sxs-lookup"><span data-stu-id="5d3cb-103">Get started with Azure CDN development</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5d3cb-104">Node.js</span><span class="sxs-lookup"><span data-stu-id="5d3cb-104">Node.js</span></span>](cdn-app-dev-node.md)
> * [<span data-ttu-id="5d3cb-105">.NET</span><span class="sxs-lookup"><span data-stu-id="5d3cb-105">.NET</span></span>](cdn-app-dev-net.md)
> 
> 

<span data-ttu-id="5d3cb-106">Můžete použít hello [knihovny CDN Azure pro .NET](https://msdn.microsoft.com/library/mt657769.aspx) tooautomate vytváření a Správa profilů CDN a koncové body.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-106">You can use hello [Azure CDN Library for .NET](https://msdn.microsoft.com/library/mt657769.aspx) tooautomate creation and management of CDN profiles and endpoints.</span></span>  <span data-ttu-id="5d3cb-107">Tento kurz vás provede hello vytvoření jednoduché konzolové aplikace .NET, které ukazuje několik operací hello k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-107">This tutorial walks through hello creation of a simple .NET console application that demonstrates several of hello available operations.</span></span>  <span data-ttu-id="5d3cb-108">V tomto kurzu není určený toodescribe všechny aspekty hello knihovně CDN Azure pro .NET podrobně.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-108">This tutorial is not intended toodescribe all aspects of hello Azure CDN Library for .NET in detail.</span></span>

<span data-ttu-id="5d3cb-109">Je nutné v tomto kurzu toocomplete Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-109">You need Visual Studio 2015 toocomplete this tutorial.</span></span>  <span data-ttu-id="5d3cb-110">[Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) je dostupná volně ke stažení.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-110">[Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) is freely available for download.</span></span>

> [!TIP]
> <span data-ttu-id="5d3cb-111">Hello [dokončený projekt z tohoto kurzu](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) je k dispozici ke stažení na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-111">hello [completed project from this tutorial](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) is available for download on MSDN.</span></span>
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a><span data-ttu-id="5d3cb-112">Vytvoření projektu a přidání balíčků Nuget</span><span class="sxs-lookup"><span data-stu-id="5d3cb-112">Create your project and add Nuget packages</span></span>
<span data-ttu-id="5d3cb-113">Teď, když jsme vytvořili skupinu prostředků pro naše profily CDN a daného naše profilů CDN toomanage oprávnění aplikace Azure AD a koncové body v rámci dané skupiny, můžeme začít vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-113">Now that we've created a resource group for our CDN profiles and given our Azure AD application permission toomanage CDN profiles and endpoints within that group, we can start creating our application.</span></span>

<span data-ttu-id="5d3cb-114">Z v rámci sady Visual Studio 2015, klikněte na **soubor**, **nový**, **projektu...**  tooopen hello dialogové okno Nový projekt.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-114">From within Visual Studio 2015, click **File**, **New**, **Project...** tooopen hello new project dialog.</span></span>  <span data-ttu-id="5d3cb-115">Rozbalte položku **Visual C#**, pak vyberte **Windows** v podokně hello na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-115">Expand **Visual C#**, then select **Windows** in hello pane on hello left.</span></span>  <span data-ttu-id="5d3cb-116">Klikněte na tlačítko **konzolové aplikace** v prostředním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-116">Click **Console Application** in hello center pane.</span></span>  <span data-ttu-id="5d3cb-117">Název projektu a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-117">Name your project, then click **OK**.</span></span>  

![Nový projekt](./media/cdn-app-dev-net/cdn-new-project.png)

<span data-ttu-id="5d3cb-119">Naše projektu přechází toouse některé knihovny Azure, které jsou součástí balíčků Nuget.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-119">Our project is going toouse some Azure libraries contained in Nuget packages.</span></span>  <span data-ttu-id="5d3cb-120">Umožňuje přidat tyto toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-120">Let's add those toohello project.</span></span>

1. <span data-ttu-id="5d3cb-121">Klikněte na tlačítko hello **nástroje** nabídce **Správce balíčků Nuget**, pak **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-121">Click hello **Tools** menu, **Nuget Package Manager**, then **Package Manager Console**.</span></span>
   
    ![Správa balíčků Nuget](./media/cdn-app-dev-net/cdn-manage-nuget.png)
2. <span data-ttu-id="5d3cb-123">V hello Konzola správce balíčků, spustit následující příkaz tooinstall hello hello **Active Directory Authentication Library (ADAL)**:</span><span class="sxs-lookup"><span data-stu-id="5d3cb-123">In hello Package Manager Console, execute hello following command tooinstall hello **Active Directory Authentication Library (ADAL)**:</span></span>
   
    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`
3. <span data-ttu-id="5d3cb-124">Provést následující tooinstall hello hello **Knihovna správy Azure CDN**:</span><span class="sxs-lookup"><span data-stu-id="5d3cb-124">Execute hello following tooinstall hello **Azure CDN Management Library**:</span></span>
   
    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a><span data-ttu-id="5d3cb-125">Direktivy, konstanty, hlavní metoda a pomocné metody</span><span class="sxs-lookup"><span data-stu-id="5d3cb-125">Directives, constants, main method, and helper methods</span></span>
<span data-ttu-id="5d3cb-126">Pojďme hello základní struktura naše programu zapsána.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-126">Let's get hello basic structure of our program written.</span></span>

1. <span data-ttu-id="5d3cb-127">Zpět na kartě Program.cs hello, nahraďte hello `using` direktivy v horní části hello s hello následující:</span><span class="sxs-lookup"><span data-stu-id="5d3cb-127">Back in hello Program.cs tab, replace hello `using` directives at hello top with hello following:</span></span>
   
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
2. <span data-ttu-id="5d3cb-128">Potřebujeme toodefine některé konstanty, který bude používat naše metody.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-128">We need toodefine some constants our methods will use.</span></span>  <span data-ttu-id="5d3cb-129">V hello `Program` třídy, ale před hello `Main` metoda, přidejte následující hello.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-129">In hello `Program` class, but before hello `Main` method, add hello following.</span></span>  <span data-ttu-id="5d3cb-130">Být jisti tooreplace hello zástupné symboly, včetně hello  **&lt;lomené závorky&gt;**, s vlastní hodnoty, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-130">Be sure tooreplace hello placeholders, including hello **&lt;angle brackets&gt;**, with your own values as needed.</span></span>
   
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
3. <span data-ttu-id="5d3cb-131">Také na úrovni třídy hello, definujte tyto dvě proměnné.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-131">Also at hello class level, define these two variables.</span></span>  <span data-ttu-id="5d3cb-132">Použijeme tyto novější toodetermine naše profilu a koncového bodu už existuje.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-132">We'll use these later toodetermine if our profile and endpoint already exist.</span></span>
   
    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```
4. <span data-ttu-id="5d3cb-133">Nahraďte hello `Main` metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5d3cb-133">Replace hello `Main` method as follows:</span></span>
   
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
5. <span data-ttu-id="5d3cb-134">Některé z našich dalších metod budou tooprompt hello uživatele s dotazy "Ano".</span><span class="sxs-lookup"><span data-stu-id="5d3cb-134">Some of our other methods are going tooprompt hello user with "Yes/No" questions.</span></span>  <span data-ttu-id="5d3cb-135">Přidejte následující metodu toomake hello který trochu snazší:</span><span class="sxs-lookup"><span data-stu-id="5d3cb-135">Add hello following method toomake that a little easier:</span></span>
   
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

<span data-ttu-id="5d3cb-136">Teď, když je zapsán hello základní struktura naše programu, by měl vytvoříme hello metody volány hello `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-136">Now that hello basic structure of our program is written, we should create hello methods called by hello `Main` method.</span></span>

## <a name="authentication"></a><span data-ttu-id="5d3cb-137">Authentication</span><span class="sxs-lookup"><span data-stu-id="5d3cb-137">Authentication</span></span>
<span data-ttu-id="5d3cb-138">Před použitím jsme hello Knihovna správy Azure CDN, budeme potřebovat tooauthenticate naši službu hlavní a získat ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-138">Before we can use hello Azure CDN Management Library, we need tooauthenticate our service principal and obtain an authentication token.</span></span>  <span data-ttu-id="5d3cb-139">Tato metoda používá ADAL tooretrieve hello token.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-139">This method uses ADAL tooretrieve hello token.</span></span>

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

<span data-ttu-id="5d3cb-140">Pokud používáte ověřování jednotlivých uživatelů, hello `GetAccessToken` metoda bude vypadat mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-140">If you are using individual user authentication, hello `GetAccessToken` method will look slightly different.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5d3cb-141">Tato ukázka kódu použijte, pouze pokud zvolíte ověřování jednotlivých uživatelů toohave místo objekt služby.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-141">Only use this code sample if you are choosing toohave individual user authentication instead of a service principal.</span></span>
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

<span data-ttu-id="5d3cb-142">Být jisti tooreplace `<redirect URI>` s hello jste zadali při registraci aplikace hello ve službě Azure AD identifikátor URI pro přesměrování.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-142">Be sure tooreplace `<redirect URI>` with hello redirect URI you entered when you registered hello application in Azure AD.</span></span>

## <a name="list-cdn-profiles-and-endpoints"></a><span data-ttu-id="5d3cb-143">Koncové body a seznam profilů CDN</span><span class="sxs-lookup"><span data-stu-id="5d3cb-143">List CDN profiles and endpoints</span></span>
<span data-ttu-id="5d3cb-144">Nyní jsme připravené tooperform CDN operace.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-144">Now we're ready tooperform CDN operations.</span></span>  <span data-ttu-id="5d3cb-145">Hello první věc, kterou naše metoda nemá je seznam všech profilů hello a koncových bodů v naší skupiny prostředků a pokud najde shoda pro hello profilu a koncového bodu názvy zadané v našem konstanty zajišťuje Poznámka této pro později tak jsme nepokoušejte toocreate duplikáty.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-145">hello first thing our method does is list all hello profiles and endpoints in our resource group, and if it finds a match for hello profile and endpoint names specified in our constants, makes a note of that for later so we don't try toocreate duplicates.</span></span>

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

## <a name="create-cdn-profiles-and-endpoints"></a><span data-ttu-id="5d3cb-146">Vytvoření profilů CDN a koncové body</span><span class="sxs-lookup"><span data-stu-id="5d3cb-146">Create CDN profiles and endpoints</span></span>
<span data-ttu-id="5d3cb-147">V dalším kroku vytvoříme profil.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-147">Next, we'll create a profile.</span></span>

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

<span data-ttu-id="5d3cb-148">Po vytvoření profilu hello vytvoříme koncový bod.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-148">Once hello profile is created, we'll create an endpoint.</span></span>

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
> <span data-ttu-id="5d3cb-149">výše uvedený příklad Hello přiřadí hello koncový bod počátek s názvem *Contoso* s hostitele `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-149">hello example above assigns hello endpoint an origin named *Contoso* with a hostname `www.contoso.com`.</span></span>  <span data-ttu-id="5d3cb-150">Měli byste změnit toto toopoint tooyour vlastní název počátečního na hostitele.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-150">You should change this toopoint tooyour own origin's hostname.</span></span>
> 
> 

## <a name="purge-an-endpoint"></a><span data-ttu-id="5d3cb-151">Vyprázdnění koncového bodu</span><span class="sxs-lookup"><span data-stu-id="5d3cb-151">Purge an endpoint</span></span>
<span data-ttu-id="5d3cb-152">Za předpokladu, že byl vytvořen koncový bod hello, běžných úloh, může chceme tooperform v našem programu je vyprazdňování hello obsah v našem koncový bod.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-152">Assuming hello endpoint has been created, one common task that we might want tooperform in our program is purging hello content in our endpoint.</span></span>

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
> <span data-ttu-id="5d3cb-153">V předchozím příkladu hello, hello řetězec `/*` označuje, že chcete toopurge všechno hello kořenové hello cesta ke koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-153">In hello example above, hello string `/*` denotes that I want toopurge everything in hello root of hello endpoint path.</span></span>  <span data-ttu-id="5d3cb-154">Toto je ekvivalentní toochecking **vyprázdnit všechny** v hello portál Azure "mazání" dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-154">This is equivalent toochecking **Purge All** in hello Azure portal's "purge" dialog.</span></span> <span data-ttu-id="5d3cb-155">V hello `CreateCdnProfile` metoda, vytvořené naše profilu jako **Azure CDN společnosti Verizon** profil pomocí kódu hello `Sku = new Sku(SkuName.StandardVerizon)`, takže to bude úspěšné.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-155">In hello `CreateCdnProfile` method, I created our profile as an **Azure CDN from Verizon** profile using hello code `Sku = new Sku(SkuName.StandardVerizon)`, so this will be successful.</span></span>  <span data-ttu-id="5d3cb-156">Ale **Azure CDN společnosti Akamai** profily nepodporují **vyprázdnit všechny**, takže pokud použití profilem Akamai pro účely tohoto kurzu, by potřebuji toopurge tooinclude konkrétní cesty.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-156">However, **Azure CDN from Akamai** profiles do not support **Purge All**, so if I was using an Akamai profile for this tutorial, I would need tooinclude specific paths toopurge.</span></span>
> 
> 

## <a name="delete-cdn-profiles-and-endpoints"></a><span data-ttu-id="5d3cb-157">Odstranit profily CDN a koncové body</span><span class="sxs-lookup"><span data-stu-id="5d3cb-157">Delete CDN profiles and endpoints</span></span>
<span data-ttu-id="5d3cb-158">poslední metody Hello odstraní naše koncový bod a profil.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-158">hello last methods will delete our endpoint and profile.</span></span>

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

## <a name="running-hello-program"></a><span data-ttu-id="5d3cb-159">Spuštění programu hello</span><span class="sxs-lookup"><span data-stu-id="5d3cb-159">Running hello program</span></span>
<span data-ttu-id="5d3cb-160">Jsme teď zkompilování a spuštění programu hello kliknutím hello **spustit** tlačítko v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-160">We can now compile and run hello program by clicking hello **Start** button in Visual Studio.</span></span>

![Spuštění programu](./media/cdn-app-dev-net/cdn-program-running-1.png)

<span data-ttu-id="5d3cb-162">Když hello program dosáhne hello výše řádku, by měl být schopný tooreturn tooyour skupiny prostředků v hello portál Azure a najdete v části vytvořený profil hello.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-162">When hello program reaches hello above prompt, you should be able tooreturn tooyour resource group in hello Azure portal and see that hello profile has been created.</span></span>

![Úspěšné!](./media/cdn-app-dev-net/cdn-success.png)

<span data-ttu-id="5d3cb-164">Potom může potvrdit hello výzvy toorun hello rest programu hello.</span><span class="sxs-lookup"><span data-stu-id="5d3cb-164">We can then confirm hello prompts toorun hello rest of hello program.</span></span>

![Dokončení programu](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a><span data-ttu-id="5d3cb-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d3cb-166">Next Steps</span></span>
<span data-ttu-id="5d3cb-167">Projekt hello dokončit toosee z tohoto návodu [stažení ukázky hello](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).</span><span class="sxs-lookup"><span data-stu-id="5d3cb-167">toosee hello completed project from this walkthrough, [download hello sample](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).</span></span>

<span data-ttu-id="5d3cb-168">Další dokumentaci toofind na hello Knihovna správy CDN Azure pro platformu .NET, zobrazení hello [odkaz na webu MSDN](https://msdn.microsoft.com/library/mt657769.aspx).</span><span class="sxs-lookup"><span data-stu-id="5d3cb-168">toofind additional documentation on hello Azure CDN Management Library for .NET, view hello [reference on MSDN](https://msdn.microsoft.com/library/mt657769.aspx).</span></span>

<span data-ttu-id="5d3cb-169">Spravovat prostředky CDN s [prostředí PowerShell](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="5d3cb-169">Manage your CDN resources with [PowerShell](cdn-manage-powershell.md).</span></span>

