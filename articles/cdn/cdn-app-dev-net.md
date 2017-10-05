---
title: "Začínáme s knihovnou Azure CDN pro rozhraní .NET | Microsoft Docs"
description: "Zjistěte, jak psát aplikace .NET pro správu Azure CDN pomocí sady Visual Studio."
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
ms.openlocfilehash: 5379586355ece98af6295236d6cbd09cb31c742b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-cdn-development"></a><span data-ttu-id="5e1f3-103">Začínáme s vývojem pro Azure CDN</span><span class="sxs-lookup"><span data-stu-id="5e1f3-103">Get started with Azure CDN development</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5e1f3-104">Node.js</span><span class="sxs-lookup"><span data-stu-id="5e1f3-104">Node.js</span></span>](cdn-app-dev-node.md)
> * [<span data-ttu-id="5e1f3-105">.NET</span><span class="sxs-lookup"><span data-stu-id="5e1f3-105">.NET</span></span>](cdn-app-dev-net.md)
> 
> 

<span data-ttu-id="5e1f3-106">Můžete použít [knihovny CDN Azure pro .NET](https://msdn.microsoft.com/library/mt657769.aspx) k automatizaci vytváření a Správa profilů CDN a koncové body.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-106">You can use the [Azure CDN Library for .NET](https://msdn.microsoft.com/library/mt657769.aspx) to automate creation and management of CDN profiles and endpoints.</span></span>  <span data-ttu-id="5e1f3-107">Tento kurz vás provede vytvoření jednoduché konzolové aplikace .NET, která demonstruje řadu dostupné operace.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-107">This tutorial walks through the creation of a simple .NET console application that demonstrates several of the available operations.</span></span>  <span data-ttu-id="5e1f3-108">V tomto kurzu není určeno k podrobně popisují všechny aspekty Azure CDN Library pro .NET.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-108">This tutorial is not intended to describe all aspects of the Azure CDN Library for .NET in detail.</span></span>

<span data-ttu-id="5e1f3-109">Budete potřebovat k dokončení tohoto kurzu sady Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-109">You need Visual Studio 2015 to complete this tutorial.</span></span>  <span data-ttu-id="5e1f3-110">[Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) je dostupná volně ke stažení.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-110">[Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) is freely available for download.</span></span>

> [!TIP]
> <span data-ttu-id="5e1f3-111">[Dokončený projekt z tohoto kurzu](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) je k dispozici ke stažení na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-111">The [completed project from this tutorial](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) is available for download on MSDN.</span></span>
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a><span data-ttu-id="5e1f3-112">Vytvoření projektu a přidání balíčků Nuget</span><span class="sxs-lookup"><span data-stu-id="5e1f3-112">Create your project and add Nuget packages</span></span>
<span data-ttu-id="5e1f3-113">Teď, když jsme vytvořili skupinu prostředků pro naše profily CDN a získá naše Azure AD aplikace oprávnění ke správě profilů CDN a koncové body v rámci dané skupiny, můžeme začít vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-113">Now that we've created a resource group for our CDN profiles and given our Azure AD application permission to manage CDN profiles and endpoints within that group, we can start creating our application.</span></span>

<span data-ttu-id="5e1f3-114">Z v rámci sady Visual Studio 2015, klikněte na **soubor**, **nový**, **projektu...**  otevřete dialogové okno Nový projekt.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-114">From within Visual Studio 2015, click **File**, **New**, **Project...** to open the new project dialog.</span></span>  <span data-ttu-id="5e1f3-115">Rozbalte položku **Visual C#**, pak vyberte **Windows** v podokně na levé straně.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-115">Expand **Visual C#**, then select **Windows** in the pane on the left.</span></span>  <span data-ttu-id="5e1f3-116">Klikněte na tlačítko **konzolové aplikace** v prostředním podokně.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-116">Click **Console Application** in the center pane.</span></span>  <span data-ttu-id="5e1f3-117">Název projektu a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-117">Name your project, then click **OK**.</span></span>  

![Nový projekt](./media/cdn-app-dev-net/cdn-new-project.png)

<span data-ttu-id="5e1f3-119">Naše projektu bude používat některé Azure knihovny, které jsou součástí balíčků Nuget.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-119">Our project is going to use some Azure libraries contained in Nuget packages.</span></span>  <span data-ttu-id="5e1f3-120">Umožňuje přidat do projektu.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-120">Let's add those to the project.</span></span>

1. <span data-ttu-id="5e1f3-121">Klikněte **nástroje** nabídce **Správce balíčků Nuget**, pak **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-121">Click the **Tools** menu, **Nuget Package Manager**, then **Package Manager Console**.</span></span>
   
    ![Správa balíčků Nuget](./media/cdn-app-dev-net/cdn-manage-nuget.png)
2. <span data-ttu-id="5e1f3-123">V konzole Správce balíčků spustíte následující příkaz k instalaci **Active Directory Authentication Library (ADAL)**:</span><span class="sxs-lookup"><span data-stu-id="5e1f3-123">In the Package Manager Console, execute the following command to install the **Active Directory Authentication Library (ADAL)**:</span></span>
   
    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`
3. <span data-ttu-id="5e1f3-124">Spuštěním následujících příkazů nainstalujte **Knihovna správy Azure CDN**:</span><span class="sxs-lookup"><span data-stu-id="5e1f3-124">Execute the following to install the **Azure CDN Management Library**:</span></span>
   
    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a><span data-ttu-id="5e1f3-125">Direktivy, konstanty, hlavní metoda a pomocné metody</span><span class="sxs-lookup"><span data-stu-id="5e1f3-125">Directives, constants, main method, and helper methods</span></span>
<span data-ttu-id="5e1f3-126">Pojďme základní strukturu naše programu zapsána.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-126">Let's get the basic structure of our program written.</span></span>

1. <span data-ttu-id="5e1f3-127">Zpět na kartě Program.cs nahradit `using` direktivy v horní části následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="5e1f3-127">Back in the Program.cs tab, replace the `using` directives at the top with the following:</span></span>
   
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
2. <span data-ttu-id="5e1f3-128">Je potřeba definovat některé konstanty, který bude používat naše metody.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-128">We need to define some constants our methods will use.</span></span>  <span data-ttu-id="5e1f3-129">V `Program` třídy, ale předtím, než `Main` metoda, přidejte následující.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-129">In the `Program` class, but before the `Main` method, add the following.</span></span>  <span data-ttu-id="5e1f3-130">Ujistěte se, zda jste nahradili zástupné symboly, včetně  **&lt;lomené závorky&gt;**, s vlastní hodnoty, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-130">Be sure to replace the placeholders, including the **&lt;angle brackets&gt;**, with your own values as needed.</span></span>
   
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
3. <span data-ttu-id="5e1f3-131">Také na úrovni třídy definujte tyto dvě proměnné.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-131">Also at the class level, define these two variables.</span></span>  <span data-ttu-id="5e1f3-132">Použijeme tyto později k určení, zda naše profilu a koncového bodu už existují.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-132">We'll use these later to determine if our profile and endpoint already exist.</span></span>
   
    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```
4. <span data-ttu-id="5e1f3-133">Nahraďte `Main` metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5e1f3-133">Replace the `Main` method as follows:</span></span>
   
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
   
       Console.WriteLine("Press Enter to end program.");
       Console.ReadLine();
   }
   ```
5. <span data-ttu-id="5e1f3-134">Některé z našich dalších metod se chystáte Dotázat se uživatele pomocí otázek "Ano".</span><span class="sxs-lookup"><span data-stu-id="5e1f3-134">Some of our other methods are going to prompt the user with "Yes/No" questions.</span></span>  <span data-ttu-id="5e1f3-135">Přidejte následující metodu pro snazší, trochu:</span><span class="sxs-lookup"><span data-stu-id="5e1f3-135">Add the following method to make that a little easier:</span></span>
   
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

<span data-ttu-id="5e1f3-136">Teď, když je zapsán základní struktura naše program, by měl vytvoříme volá metody `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-136">Now that the basic structure of our program is written, we should create the methods called by the `Main` method.</span></span>

## <a name="authentication"></a><span data-ttu-id="5e1f3-137">Authentication</span><span class="sxs-lookup"><span data-stu-id="5e1f3-137">Authentication</span></span>
<span data-ttu-id="5e1f3-138">Knihovna správy Azure CDN jsme mohli používat, je potřeba ověřit naše instanční objekt a získat ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-138">Before we can use the Azure CDN Management Library, we need to authenticate our service principal and obtain an authentication token.</span></span>  <span data-ttu-id="5e1f3-139">Tato metoda používá ADAL pro načtení tokenu.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-139">This method uses ADAL to retrieve the token.</span></span>

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

<span data-ttu-id="5e1f3-140">Pokud používáte ověřování jednotlivých uživatelů, `GetAccessToken` metoda bude vypadat mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-140">If you are using individual user authentication, the `GetAccessToken` method will look slightly different.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5e1f3-141">Tato ukázka kódu použijte, pouze pokud zvolíte ověřování jednotlivého uživatele místo objekt služby.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-141">Only use this code sample if you are choosing to have individual user authentication instead of a service principal.</span></span>
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

<span data-ttu-id="5e1f3-142">Nezapomeňte nahradit `<redirect URI>` s přesměrováním URI, které jste zadali při registraci aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-142">Be sure to replace `<redirect URI>` with the redirect URI you entered when you registered the application in Azure AD.</span></span>

## <a name="list-cdn-profiles-and-endpoints"></a><span data-ttu-id="5e1f3-143">Koncové body a seznam profilů CDN</span><span class="sxs-lookup"><span data-stu-id="5e1f3-143">List CDN profiles and endpoints</span></span>
<span data-ttu-id="5e1f3-144">Nyní je připraven k provádění operací CDN.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-144">Now we're ready to perform CDN operations.</span></span>  <span data-ttu-id="5e1f3-145">První krok, který nemá naše metoda je seznam všech profilů a koncových bodů v naší skupiny prostředků a pokud najde shoda pro zadané v našem konstanty názvy profilu a koncového bodu usnadňuje Poznámka této později, pokusíme nemáte k vytvoření duplicitní položky.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-145">The first thing our method does is list all the profiles and endpoints in our resource group, and if it finds a match for the profile and endpoint names specified in our constants, makes a note of that for later so we don't try to create duplicates.</span></span>

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a><span data-ttu-id="5e1f3-146">Vytvoření profilů CDN a koncové body</span><span class="sxs-lookup"><span data-stu-id="5e1f3-146">Create CDN profiles and endpoints</span></span>
<span data-ttu-id="5e1f3-147">V dalším kroku vytvoříme profil.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-147">Next, we'll create a profile.</span></span>

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

<span data-ttu-id="5e1f3-148">Po vytvoření profilu vytvoříme koncový bod.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-148">Once the profile is created, we'll create an endpoint.</span></span>

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
> <span data-ttu-id="5e1f3-149">V předchozím příkladu přiřadí koncový bod počátek s názvem *Contoso* s hostitele `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-149">The example above assigns the endpoint an origin named *Contoso* with a hostname `www.contoso.com`.</span></span>  <span data-ttu-id="5e1f3-150">Měli byste změnit to tak, aby odkazoval na název vlastního počátečního hostitele.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-150">You should change this to point to your own origin's hostname.</span></span>
> 
> 

## <a name="purge-an-endpoint"></a><span data-ttu-id="5e1f3-151">Vyprázdnění koncového bodu</span><span class="sxs-lookup"><span data-stu-id="5e1f3-151">Purge an endpoint</span></span>
<span data-ttu-id="5e1f3-152">Za předpokladu, že byl vytvořen koncový bod, jeden běžný úkol, který jsme může být třeba provést v našem programu je mazání obsahu v našem koncový bod.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-152">Assuming the endpoint has been created, one common task that we might want to perform in our program is purging the content in our endpoint.</span></span>

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
> <span data-ttu-id="5e1f3-153">V příkladu výše, řetězec `/*` označuje, že chcete vymazat vše v kořenovém cestu ke koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-153">In the example above, the string `/*` denotes that I want to purge everything in the root of the endpoint path.</span></span>  <span data-ttu-id="5e1f3-154">Jde o ekvivalent kontrola **vyprázdnit všechny** v dialogovém okně "mazání" portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-154">This is equivalent to checking **Purge All** in the Azure portal's "purge" dialog.</span></span> <span data-ttu-id="5e1f3-155">V `CreateCdnProfile` metoda, vytvořené naše profilu jako **Azure CDN společnosti Verizon** profilu pomocí kódu `Sku = new Sku(SkuName.StandardVerizon)`, takže to bude úspěšné.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-155">In the `CreateCdnProfile` method, I created our profile as an **Azure CDN from Verizon** profile using the code `Sku = new Sku(SkuName.StandardVerizon)`, so this will be successful.</span></span>  <span data-ttu-id="5e1f3-156">Ale **Azure CDN společnosti Akamai** profily nepodporují **vyprázdnit všechny**, takže pokud použití profilem Akamai pro účely tohoto kurzu, by je nutné zahrnout konkrétní cesty k vyprázdnění.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-156">However, **Azure CDN from Akamai** profiles do not support **Purge All**, so if I was using an Akamai profile for this tutorial, I would need to include specific paths to purge.</span></span>
> 
> 

## <a name="delete-cdn-profiles-and-endpoints"></a><span data-ttu-id="5e1f3-157">Odstranit profily CDN a koncové body</span><span class="sxs-lookup"><span data-stu-id="5e1f3-157">Delete CDN profiles and endpoints</span></span>
<span data-ttu-id="5e1f3-158">Poslední metody odstraní naše koncový bod a profil.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-158">The last methods will delete our endpoint and profile.</span></span>

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

## <a name="running-the-program"></a><span data-ttu-id="5e1f3-159">Spuštění programu</span><span class="sxs-lookup"><span data-stu-id="5e1f3-159">Running the program</span></span>
<span data-ttu-id="5e1f3-160">Jsme teď můžete zkompilovat a program spusťte kliknutím **spustit** tlačítko v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-160">We can now compile and run the program by clicking the **Start** button in Visual Studio.</span></span>

![Spuštění programu](./media/cdn-app-dev-net/cdn-program-running-1.png)

<span data-ttu-id="5e1f3-162">Když program dosáhne řádku výše, byste měli mít se vrátíte do vaší skupiny prostředků na portálu Azure najdete v části vytvořený profil.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-162">When the program reaches the above prompt, you should be able to return to your resource group in the Azure portal and see that the profile has been created.</span></span>

![Úspěšné!](./media/cdn-app-dev-net/cdn-success.png)

<span data-ttu-id="5e1f3-164">Potom může potvrdit pokynů ke spuštění zbytek programu.</span><span class="sxs-lookup"><span data-stu-id="5e1f3-164">We can then confirm the prompts to run the rest of the program.</span></span>

![Dokončení programu](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a><span data-ttu-id="5e1f3-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e1f3-166">Next Steps</span></span>
<span data-ttu-id="5e1f3-167">Zobrazíte dokončený projekt z tohoto návodu [stáhnout vzorek](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).</span><span class="sxs-lookup"><span data-stu-id="5e1f3-167">To see the completed project from this walkthrough, [download the sample](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).</span></span>

<span data-ttu-id="5e1f3-168">Chcete-li vyhledat další dokumentaci v knihovně správy CDN Azure pro .NET, podívejte se [odkaz na webu MSDN](https://msdn.microsoft.com/library/mt657769.aspx).</span><span class="sxs-lookup"><span data-stu-id="5e1f3-168">To find additional documentation on the Azure CDN Management Library for .NET, view the [reference on MSDN](https://msdn.microsoft.com/library/mt657769.aspx).</span></span>

<span data-ttu-id="5e1f3-169">Spravovat prostředky CDN s [prostředí PowerShell](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="5e1f3-169">Manage your CDN resources with [PowerShell](cdn-manage-powershell.md).</span></span>

