---
title: "Azure Cosmos DB: Úvodní kurz k .NET Core pro rozhraní DocumentDB API | Dokumentace Microsoftu"
description: "Kurz, který vytváří online databáze a Konzolová aplikace C# pomocí hello Azure Cosmos databáze DocumentDB rozhraní API .NET Core SDK."
services: cosmos-db
documentationcenter: .net
author: arramac
manager: jhubbard
editor: 
ms.assetid: 9f93e276-9936-4efb-a534-a9889fa7c7d2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: 5e7efb327252e9e73e9b2a340820eeecc6937fa5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-getting-started-with-hello-documentdb-api-and-net-core"></a><span data-ttu-id="fe21f-103">Azure Cosmos DB: Začínáme s hello DocumentDB rozhraní API a .NET Core</span><span class="sxs-lookup"><span data-stu-id="fe21f-103">Azure Cosmos DB: Getting started with hello DocumentDB API and .NET Core</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fe21f-104">.NET</span><span class="sxs-lookup"><span data-stu-id="fe21f-104">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="fe21f-105">.NET Core</span><span class="sxs-lookup"><span data-stu-id="fe21f-105">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="fe21f-106">Node.js pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="fe21f-106">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="fe21f-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="fe21f-107">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="fe21f-108">Java</span><span class="sxs-lookup"><span data-stu-id="fe21f-108">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="fe21f-109">C++</span><span class="sxs-lookup"><span data-stu-id="fe21f-109">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="fe21f-110">Vítejte toohello DocumentDB rozhraní API pro Azure DB Cosmos Začínáme s .NET Core kurzu.</span><span class="sxs-lookup"><span data-stu-id="fe21f-110">Welcome toohello DocumentDB API for Azure Cosmos DB getting started with .NET Core tutorial!</span></span> <span data-ttu-id="fe21f-111">Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky Azure Cosmos DB a dotazuje se na ně.</span><span class="sxs-lookup"><span data-stu-id="fe21f-111">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="fe21f-112">Budeme se zabývat těmito tématy:</span><span class="sxs-lookup"><span data-stu-id="fe21f-112">We'll cover:</span></span>

* <span data-ttu-id="fe21f-113">Vytvoření a připojení účtu Azure Cosmos DB tooan</span><span class="sxs-lookup"><span data-stu-id="fe21f-113">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="fe21f-114">Konfigurace řešení v nástroji Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fe21f-114">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="fe21f-115">Vytvoření online databáze</span><span class="sxs-lookup"><span data-stu-id="fe21f-115">Creating an online database</span></span>
* <span data-ttu-id="fe21f-116">Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="fe21f-116">Creating a collection</span></span>
* <span data-ttu-id="fe21f-117">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="fe21f-117">Creating JSON documents</span></span>
* <span data-ttu-id="fe21f-118">Dotazování na kolekci hello</span><span class="sxs-lookup"><span data-stu-id="fe21f-118">Querying hello collection</span></span>
* <span data-ttu-id="fe21f-119">Nahrazení dokumentu</span><span class="sxs-lookup"><span data-stu-id="fe21f-119">Replacing a document</span></span>
* <span data-ttu-id="fe21f-120">Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="fe21f-120">Deleting a document</span></span>
* <span data-ttu-id="fe21f-121">Odstraňování databáze aplikace hello</span><span class="sxs-lookup"><span data-stu-id="fe21f-121">Deleting hello database</span></span>

<span data-ttu-id="fe21f-122">Nemáte čas?</span><span class="sxs-lookup"><span data-stu-id="fe21f-122">Don't have time?</span></span> <span data-ttu-id="fe21f-123">Nevadí!</span><span class="sxs-lookup"><span data-stu-id="fe21f-123">Don't worry!</span></span> <span data-ttu-id="fe21f-124">Hello úplné řešení je k dispozici na [Githubu](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span><span class="sxs-lookup"><span data-stu-id="fe21f-124">hello complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span></span> <span data-ttu-id="fe21f-125">Jump toohello [načtení oddílu kompletního řešení hello](#GetSolution) pro rychlé pokyny.</span><span class="sxs-lookup"><span data-stu-id="fe21f-125">Jump toohello [Get hello complete solution section](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="fe21f-126">Chcete toobuild Xamarin iOS, Android nebo formuláře aplikace pomocí hello DocumentDB rozhraní API a rozhraní .NET Core SDK?</span><span class="sxs-lookup"><span data-stu-id="fe21f-126">Want toobuild a Xamarin iOS, Android, or Forms application using hello DocumentDB API and .NET Core SDK?</span></span> <span data-ttu-id="fe21f-127">V tématu [vytváření mobilních aplikací s Xamarin a Azure Cosmos DB](mobile-apps-with-xamarin.md).</span><span class="sxs-lookup"><span data-stu-id="fe21f-127">See [Build mobile applications with Xamarin and Azure Cosmos DB](mobile-apps-with-xamarin.md).</span></span>

> [!NOTE]
> <span data-ttu-id="fe21f-128">Hello Azure Cosmos DB .NET Core SDK použili v tomto kurzu není kompatibilní s aplikací pro univerzální platformu Windows (UWP).</span><span class="sxs-lookup"><span data-stu-id="fe21f-128">hello Azure Cosmos DB .NET Core SDK used in this tutorial is not yet compatible with Universal Windows Platform (UWP) apps.</span></span> <span data-ttu-id="fe21f-129">Pro verzi preview hello .NET Core SDK, který podporuje aplikace UWP, odeslání e-mailu příliš[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="fe21f-129">For a preview version of hello .NET Core SDK that does support UWP apps, send email too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

<span data-ttu-id="fe21f-130">Můžeme začít!</span><span class="sxs-lookup"><span data-stu-id="fe21f-130">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe21f-131">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fe21f-131">Prerequisites</span></span>
<span data-ttu-id="fe21f-132">Přesvědčte se, že máte následující hello:</span><span class="sxs-lookup"><span data-stu-id="fe21f-132">Please make sure you have hello following:</span></span>

* <span data-ttu-id="fe21f-133">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="fe21f-133">An active Azure account.</span></span> <span data-ttu-id="fe21f-134">Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="fe21f-134">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="fe21f-135">Alternativně můžete použít hello [emulátoru DB Cosmos Azure](local-emulator.md) pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="fe21f-135">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* [<span data-ttu-id="fe21f-136">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="fe21f-136">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/) 
    * <span data-ttu-id="fe21f-137">Pokud pracujete v systému MacOS nebo Linux, můžete vyvíjet aplikace .NET Core z příkazového řádku hello nainstalováním hello [.NET Core SDK](https://www.microsoft.com/net/core#macos) pro platformu hello podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="fe21f-137">If you're working on MacOS or Linux, you can develop .NET Core apps from hello command-line by installing hello [.NET Core SDK](https://www.microsoft.com/net/core#macos) for hello platform of your choice.</span></span> 
    * <span data-ttu-id="fe21f-138">Pokud pracujete v systému Windows, můžete vyvíjet aplikace .NET Core z příkazového řádku hello nainstalováním hello [.NET Core SDK](https://www.microsoft.com/net/core#windows).</span><span class="sxs-lookup"><span data-stu-id="fe21f-138">If you're working on Windows, you can develop .NET Core apps from hello command-line by installing hello [.NET Core SDK](https://www.microsoft.com/net/core#windows).</span></span> 
    * <span data-ttu-id="fe21f-139">Můžete použít vlastní editor nebo si bezplatně stáhnout editor [Visual Studio Code](https://code.visualstudio.com/), který funguje na systémech Windows, Linux a MacOS.</span><span class="sxs-lookup"><span data-stu-id="fe21f-139">You can use your own editor, or download [Visual Studio Code](https://code.visualstudio.com/) which is free and works on Windows, Linux, and MacOS.</span></span> 

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="fe21f-140">Krok 1: Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fe21f-140">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="fe21f-141">Vytvořme účet služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fe21f-141">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="fe21f-142">Pokud již máte účet, který chcete toouse, můžete přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="fe21f-142">If you already have an account you want toouse, you can skip ahead too[Setup your Visual Studio Solution](#SetupVS).</span></span> <span data-ttu-id="fe21f-143">Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) toosetup hello emulátoru a přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="fe21f-143">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Setup your Visual Studio Solution](#SetupVS).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="fe21f-144"><a id="SetupVS"></a>Krok 2: Nastavení řešení v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fe21f-144"><a id="SetupVS"></a>Step 2: Setup your Visual Studio solution</span></span>
1. <span data-ttu-id="fe21f-145">Otevřete v počítači **Visual Studio 2017**.</span><span class="sxs-lookup"><span data-stu-id="fe21f-145">Open **Visual Studio 2017** on your computer.</span></span>
2. <span data-ttu-id="fe21f-146">Na hello **soubor** nabídce vyberte možnost **nový**a potom zvolte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="fe21f-146">On hello **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="fe21f-147">V hello **nový projekt** dialogovém okně, vyberte **šablony** / **Visual C#** / **.NET Core** / **Konzolové aplikace (.NET Core)**, pojmenujte svůj projekt **DocumentDBGettingStarted**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="fe21f-147">In hello **New Project** dialog, select **Templates** / **Visual C#** / **.NET Core**/**Console Application (.NET Core)**, name your project **DocumentDBGettingStarted**, and then click **OK**.</span></span>

   ![Snímek obrazovky okna Nový projekt hello](./media/documentdb-dotnetcore-get-started/nosql-tutorial-new-project-2.png)
4. <span data-ttu-id="fe21f-149">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **DocumentDBGettingStarted**.</span><span class="sxs-lookup"><span data-stu-id="fe21f-149">In hello **Solution Explorer**, right click on **DocumentDBGettingStarted**.</span></span>
5. <span data-ttu-id="fe21f-150">Aniž byste museli opustit hello nabídky, klikněte na **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="fe21f-150">Then without leaving hello menu, click on **Manage NuGet Packages...**.</span></span>

   ![Snímek obrazovky znázorňující hello právo místní nabídky pro hello projektu](./media/documentdb-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)
6. <span data-ttu-id="fe21f-152">V hello **NuGet** , klikněte na **Procházet** hello horní části okna hello a typ **azure documentdb** hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="fe21f-152">In hello **NuGet** tab, click **Browse** at hello top of hello window, and type **azure documentdb** in hello search box.</span></span>
7. <span data-ttu-id="fe21f-153">V rámci hello výsledky najít **Microsoft.Azure.DocumentDB.Core** a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="fe21f-153">Within hello results, find **Microsoft.Azure.DocumentDB.Core** and click **Install**.</span></span>
   <span data-ttu-id="fe21f-154">je třeba ID balíčku Hello pro hello DocumentDB Client Library pro .NET Core [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core).</span><span class="sxs-lookup"><span data-stu-id="fe21f-154">hello package ID for hello DocumentDB Client Library for .NET Core is [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core).</span></span> <span data-ttu-id="fe21f-155">Pokud cílíte na verzi rozhraní .NET Framework (třeba net461), kterou tento balíček .NET Core NuGet nepodporuje, použijte [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB) podporující všechny verze rozhraní .NET Framework počínaje verzí .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="fe21f-155">If you are targeting a .NET Framework version (like net461) that is not supported by this .NET Core NuGet package, then use [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB) that supports all .NET Framework versions starting .NET Framework 4.5.</span></span>
8. <span data-ttu-id="fe21f-156">Na výzvy hello přijměte instalace balíčku NuGet hello a hello licenční smlouvy.</span><span class="sxs-lookup"><span data-stu-id="fe21f-156">At hello prompts, accept hello NuGet package installations and hello license agreement.</span></span>

<span data-ttu-id="fe21f-157">Výborně!</span><span class="sxs-lookup"><span data-stu-id="fe21f-157">Great!</span></span> <span data-ttu-id="fe21f-158">Teď když jsme dokončili hello instalace, napišme nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="fe21f-158">Now that we finished hello setup, let's start writing some code.</span></span> <span data-ttu-id="fe21f-159">Úplný projekt s kódem pro tento kurz najdete na [GitHubu](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span><span class="sxs-lookup"><span data-stu-id="fe21f-159">You can find a completed code project of this tutorial at [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span></span>

## <span data-ttu-id="fe21f-160"><a id="Connect"></a>Krok 3: Připojení účtu Azure Cosmos DB tooan</span><span class="sxs-lookup"><span data-stu-id="fe21f-160"><a id="Connect"></a>Step 3: Connect tooan Azure Cosmos DB account</span></span>
<span data-ttu-id="fe21f-161">Nejprve přidejte tyto odkazuje toohello začátek aplikace C#, v souboru Program.cs hello:</span><span class="sxs-lookup"><span data-stu-id="fe21f-161">First, add these references toohello beginning of your C# application, in hello Program.cs file:</span></span>

```csharp
using System;

// ADD THIS PART tooYOUR CODE
using System.Linq;
using System.Threading.Tasks;
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

> [!IMPORTANT]
> <span data-ttu-id="fe21f-162">V pořadí toocomplete v tomto kurzu, ujistěte se, že přidáte hello závislostí uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="fe21f-162">In order toocomplete this tutorial, make sure you add hello dependencies above.</span></span>

<span data-ttu-id="fe21f-163">Nyní přidejte tyto dvě konstanty a proměnnou *client* pod veřejnou třídu *Program*.</span><span class="sxs-lookup"><span data-stu-id="fe21f-163">Now, add these two constants and your *client* variable underneath your public class *Program*.</span></span>

```csharp
class Program
{
    // ADD THIS PART tooYOUR CODE
    private const string EndpointUri = "<your endpoint URI>";
    private const string PrimaryKey = "<your key>";
    private DocumentClient client;
```

<span data-ttu-id="fe21f-164">V dalším kroku head toohello [portálu Azure](https://portal.azure.com) tooretrieve identifikátor URI a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="fe21f-164">Next, head toohello [Azure Portal](https://portal.azure.com) tooretrieve your URI and primary key.</span></span> <span data-ttu-id="fe21f-165">Hello Azure Cosmos DB URI a primary key jsou nezbytné, aby vaše aplikace toounderstand kde tooconnect a u Azure Cosmos DB tootrust připojení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe21f-165">hello Azure Cosmos DB URI and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span>

<span data-ttu-id="fe21f-166">V hello portálu Azure, přejděte tooyour Azure Cosmos DB účtu a pak klikněte na tlačítko **klíče**.</span><span class="sxs-lookup"><span data-stu-id="fe21f-166">In hello Azure Portal, navigate tooyour Azure Cosmos DB account, and then click **Keys**.</span></span>

<span data-ttu-id="fe21f-167">Zkopírujte z portálu hello hello URI a vložte ji do `<your endpoint URI>` v souboru program.cs hello.</span><span class="sxs-lookup"><span data-stu-id="fe21f-167">Copy hello URI from hello portal and paste it into `<your endpoint URI>` in hello program.cs file.</span></span> <span data-ttu-id="fe21f-168">Kopírování hello primární klíč z portálu hello a vložte ji do `<your key>`.</span><span class="sxs-lookup"><span data-stu-id="fe21f-168">Then copy hello PRIMARY KEY from hello portal and paste it into `<your key>`.</span></span> <span data-ttu-id="fe21f-169">Pokud používáte hello emulátoru DB Cosmos Azure, použijte `https://localhost:8081` jako koncový bod hello a hello dobře definovaný autorizační klíč z [jak toodevelop pomocí emulátoru DB Cosmos Azure hello](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="fe21f-169">If you are using hello Azure Cosmos DB Emulator, use `https://localhost:8081` as hello endpoint, and hello well-defined authorization key from [How toodevelop using hello Azure Cosmos DB Emulator](local-emulator.md).</span></span> <span data-ttu-id="fe21f-170">Ujistěte se, zda text hello tooremove < a >, ale ponechte hello dvojitých uvozovkách. koncový bod a klíč.</span><span class="sxs-lookup"><span data-stu-id="fe21f-170">Make sure tooremove hello < and > but leave hello double quotes around your endpoint and key.</span></span>

![Snímek obrazovky hello portálu Azure používá hello toocreate kurzu NoSQL konzolovou aplikaci C#.][keys]

<span data-ttu-id="fe21f-173">Začneme vytvořením novou instanci třídy hello aplikaci získávání hello **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="fe21f-173">We'll start hello getting started application by creating a new instance of hello **DocumentClient**.</span></span>

<span data-ttu-id="fe21f-174">Níže hello **hlavní** metoda, přidejte tento nový asynchronní úkol pojmenovaný **GetStartedDemo**, který vytvoří instanci našeho nového **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="fe21f-174">Below hello **Main** method, add this new asynchronous task called **GetStartedDemo**, which will instantiate our new **DocumentClient**.</span></span>

```csharp
static void Main(string[] args)
{
}

// ADD THIS PART tooYOUR CODE
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
}
```

<span data-ttu-id="fe21f-175">Přidejte následující hello kód toorun asynchronní úkol z vaší **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="fe21f-175">Add hello following code toorun your asynchronous task from your **Main** method.</span></span> <span data-ttu-id="fe21f-176">Hello **hlavní** metoda zachytí výjimky a jejich zápis toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="fe21f-176">hello **Main** method will catch exceptions and write them toohello console.</span></span>

```csharp
static void Main(string[] args)
{
        // ADD THIS PART tooYOUR CODE
        try
        {
                Program p = new Program();
                p.GetStartedDemo().Wait();
        }
        catch (DocumentClientException de)
        {
                Exception baseException = de.GetBaseException();
                Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
        }
        catch (Exception e)
        {
                Exception baseException = e.GetBaseException();
                Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
        }
        finally
        {
                Console.WriteLine("End of demo, press any key tooexit.");
                Console.ReadKey();
        }
```

<span data-ttu-id="fe21f-177">Stiskněte klávesu hello **DocumentDBGettingStarted** tlačítko toobuild a spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fe21f-177">Press hello **DocumentDBGettingStarted** button toobuild and run hello application.</span></span>

<span data-ttu-id="fe21f-178">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="fe21f-178">Congratulations!</span></span> <span data-ttu-id="fe21f-179">Úspěšně jste se připojili účet Azure Cosmos DB tooan, teď Podívejme se na práci s prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fe21f-179">You have successfully connected tooan Azure Cosmos DB account, let's now take a look at working with Azure Cosmos DB resources.</span></span>  

## <a name="step-4-create-a-database"></a><span data-ttu-id="fe21f-180">Krok 4: Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="fe21f-180">Step 4: Create a database</span></span>
<span data-ttu-id="fe21f-181">Než přidáte hello kód pro vytvoření databáze, přidejte pomocnou metodu pro výpis toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="fe21f-181">Before you add hello code for creating a database, add a helper method for writing toohello console.</span></span>

<span data-ttu-id="fe21f-182">Zkopírujte a vložte hello **WriteToConsoleAndPromptToContinue** pod hello **GetStartedDemo** metoda.</span><span class="sxs-lookup"><span data-stu-id="fe21f-182">Copy and paste hello **WriteToConsoleAndPromptToContinue** method underneath hello **GetStartedDemo** method.</span></span>

```csharp
// ADD THIS PART tooYOUR CODE
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key toocontinue ...");
        Console.ReadKey();
}
```

<span data-ttu-id="fe21f-183">Vaše Azure DB Cosmos [databáze](documentdb-resources.md#databases) lze vytvořit pomocí hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metoda hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="fe21f-183">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="fe21f-184">Databáze je logický kontejner úložiště dokumentů JSON rozděleného mezi kolekcemi hello.</span><span class="sxs-lookup"><span data-stu-id="fe21f-184">A database is hello logical container of JSON document storage partitioned across collections.</span></span>

<span data-ttu-id="fe21f-185">Kopírování a vložení hello následující kód tooyour **GetStartedDemo** pod vytvoření klienta hello.</span><span class="sxs-lookup"><span data-stu-id="fe21f-185">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello client creation.</span></span> <span data-ttu-id="fe21f-186">Tím se vytvoří databáze s názvem *FamilyDB*.</span><span class="sxs-lookup"><span data-stu-id="fe21f-186">This will create a database named *FamilyDB*.</span></span>

```csharp
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    // ADD THIS PART tooYOUR CODE
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });
```

<span data-ttu-id="fe21f-187">Stiskněte klávesu hello **DocumentDBGettingStarted** tlačítko toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe21f-187">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="fe21f-188">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="fe21f-188">Congratulations!</span></span> <span data-ttu-id="fe21f-189">Úspěšně jste vytvořili databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fe21f-189">You have successfully created an Azure Cosmos DB database.</span></span>  

## <span data-ttu-id="fe21f-190"><a id="CreateColl"></a>Krok 5: Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="fe21f-190"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="fe21f-191">**CreateDocumentCollectionAsync** vytvoří novou kolekci s vyhrazenou propustností, za kterou se hradí poplatky.</span><span class="sxs-lookup"><span data-stu-id="fe21f-191">**CreateDocumentCollectionAsync** will create a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="fe21f-192">Další podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="fe21f-192">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>

<span data-ttu-id="fe21f-193">A [kolekce](documentdb-resources.md#collections) lze vytvořit pomocí hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metoda hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="fe21f-193">A [collection](documentdb-resources.md#collections) can be created by using hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="fe21f-194">Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe21f-194">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="fe21f-195">Kopírování a vložení hello následující kód tooyour **GetStartedDemo** pod vytvoření databáze hello.</span><span class="sxs-lookup"><span data-stu-id="fe21f-195">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello database creation.</span></span> <span data-ttu-id="fe21f-196">Tím se vytvoří kolekce dokumentů s názvem *FamilyCollection_oa*.</span><span class="sxs-lookup"><span data-stu-id="fe21f-196">This will create a document collection named *FamilyCollection_oa*.</span></span>

```csharp
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    await this.client.CreateDatabaseIfNotExists("FamilyDB_oa");

    // ADD THIS PART tooYOUR CODE
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"), new DocumentCollection { Id = "FamilyCollection_oa" });
```

<span data-ttu-id="fe21f-197">Stiskněte klávesu hello **DocumentDBGettingStarted** tlačítko toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe21f-197">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="fe21f-198">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="fe21f-198">Congratulations!</span></span> <span data-ttu-id="fe21f-199">Úspěšně jste vytvořili kolekci dokumentů Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fe21f-199">You have successfully created an Azure Cosmos DB document collection.</span></span>  

## <span data-ttu-id="fe21f-200"><a id="CreateDoc"></a>Krok 6: Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="fe21f-200"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="fe21f-201">A [dokumentu](documentdb-resources.md#documents) lze vytvořit pomocí hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) metoda hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="fe21f-201">A [document](documentdb-resources.md#documents) can be created by using hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="fe21f-202">Dokumenty představují uživatelem definovaný (libovolný) obsah JSON.</span><span class="sxs-lookup"><span data-stu-id="fe21f-202">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="fe21f-203">Nyní můžete vložit jeden nebo více dokumentů.</span><span class="sxs-lookup"><span data-stu-id="fe21f-203">We can now insert one or more documents.</span></span> <span data-ttu-id="fe21f-204">Pokud již máte data, které byste chtěli toostore v databázi, můžete použít Azure Cosmos DB [nástroj pro migraci dat](import-data.md).</span><span class="sxs-lookup"><span data-stu-id="fe21f-204">If you already have data you'd like toostore in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md).</span></span>

<span data-ttu-id="fe21f-205">Nejdřív potřebujeme toocreate **rodiny** třídu, která bude představovat objekty uložené v Azure Cosmos DB v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="fe21f-205">First, we need toocreate a **Family** class that will represent objects stored within Azure Cosmos DB in this sample.</span></span> <span data-ttu-id="fe21f-206">Kromě toho vytvoříme i podtřídy **Parent**, **Child**, **Pet** a **Address**, které se použijí v rámci **Family**.</span><span class="sxs-lookup"><span data-stu-id="fe21f-206">We will also create **Parent**, **Child**, **Pet**, **Address** subclasses that are used within **Family**.</span></span> <span data-ttu-id="fe21f-207">Povšimněte si, že dokumenty musí mít vlastnost **Id** serializovanou jako **id** ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="fe21f-207">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span> <span data-ttu-id="fe21f-208">Vytvořte tyto třídy tak, že přidáte následující vnitřní podtřídy po hello hello **GetStartedDemo** metoda.</span><span class="sxs-lookup"><span data-stu-id="fe21f-208">Create these classes by adding hello following internal sub-classes after hello **GetStartedDemo** method.</span></span>

<span data-ttu-id="fe21f-209">Zkopírujte a vložte hello **rodiny**, **nadřazené**, **podřízené**, **Pet**, a **adresu** pod Hello **WriteToConsoleAndPromptToContinue** metoda.</span><span class="sxs-lookup"><span data-stu-id="fe21f-209">Copy and paste hello **Family**, **Parent**, **Child**, **Pet**, and **Address** classes underneath hello **WriteToConsoleAndPromptToContinue** method.</span></span>

```csharp
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
    Console.WriteLine(format, args);
    Console.WriteLine("Press any key toocontinue ...");
    Console.ReadKey();
}

// ADD THIS PART tooYOUR CODE
public class Family
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    public string LastName { get; set; }
    public Parent[] Parents { get; set; }
    public Child[] Children { get; set; }
    public Address Address { get; set; }
    public bool IsRegistered { get; set; }
    public override string ToString()
    {
            return JsonConvert.SerializeObject(this);
    }
}

public class Parent
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
}

public class Child
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
    public string Gender { get; set; }
    public int Grade { get; set; }
    public Pet[] Pets { get; set; }
}

public class Pet
{
    public string GivenName { get; set; }
}

public class Address
{
    public string State { get; set; }
    public string County { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="fe21f-210">Zkopírujte a vložte hello **CreateFamilyDocumentIfNotExists** pod vaší **CreateDocumentCollectionIfNotExists** metoda.</span><span class="sxs-lookup"><span data-stu-id="fe21f-210">Copy and paste hello **CreateFamilyDocumentIfNotExists** method underneath your **CreateDocumentCollectionIfNotExists** method.</span></span>

```csharp
// ADD THIS PART tooYOUR CODE
private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
{
    try
    {
        await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
        this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
    }
    catch (DocumentClientException de)
    {
        if (de.StatusCode == HttpStatusCode.NotFound)
        {
            await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
            this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
        }
        else
        {
            throw;
        }
    }
}
```

<span data-ttu-id="fe21f-211">A vložte dva dokumenty, jeden pro rodinu hello a hello rodinu Wakefieldů.</span><span class="sxs-lookup"><span data-stu-id="fe21f-211">And insert two documents, one each for hello Andersen Family and hello Wakefield Family.</span></span>

<span data-ttu-id="fe21f-212">Zkopírujte a vložte hello kód, který následuje `// ADD THIS PART tooYOUR CODE` tooyour **GetStartedDemo** pod vytvoření kolekce dokumentů hello.</span><span class="sxs-lookup"><span data-stu-id="fe21f-212">Copy and paste hello code that follows `// ADD THIS PART tooYOUR CODE` tooyour **GetStartedDemo** method underneath hello document collection creation.</span></span>

```csharp
await this.CreateDatabaseIfNotExists("FamilyDB_oa");

await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooYOUR CODE
Family andersenFamily = new Family
{
        Id = "Andersen.1",
        LastName = "Andersen",
        Parents = new Parent[]
        {
                new Parent { FirstName = "Thomas" },
                new Parent { FirstName = "Mary Kay" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FirstName = "Henriette Thaulow",
                        Gender = "female",
                        Grade = 5,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Fluffy" }
                        }
                }
        },
        Address = new Address { State = "WA", County = "King", City = "Seattle" },
        IsRegistered = true
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", andersenFamily);

Family wakefieldFamily = new Family
{
        Id = "Wakefield.7",
        LastName = "Wakefield",
        Parents = new Parent[]
        {
                new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                new Parent { FamilyName = "Miller", FirstName = "Ben" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FamilyName = "Merriam",
                        FirstName = "Jesse",
                        Gender = "female",
                        Grade = 8,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Goofy" },
                                new Pet { GivenName = "Shadow" }
                        }
                },
                new Child
                {
                        FamilyName = "Miller",
                        FirstName = "Lisa",
                        Gender = "female",
                        Grade = 1
                }
        },
        Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
        IsRegistered = false
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);
```

<span data-ttu-id="fe21f-213">Stiskněte klávesu hello **DocumentDBGettingStarted** tlačítko toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe21f-213">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="fe21f-214">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="fe21f-214">Congratulations!</span></span> <span data-ttu-id="fe21f-215">Úspěšně jste vytvořili dva dokumenty Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fe21f-215">You have successfully created two Azure Cosmos DB documents.</span></span>  

![Diagram ilustrující hierarchický vztah hello mezi hello účet, hello online databáze, kolekce hello a hello dokumenty používanými v kurzu NoSQL toocreate konzolovou aplikaci C# hello](./media/documentdb-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="fe21f-217"><a id="Query"></a>Krok 7: Dotazování prostředků Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fe21f-217"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="fe21f-218">Azure Cosmos DB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí.</span><span class="sxs-lookup"><span data-stu-id="fe21f-218">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="fe21f-219">Hello následující vzorový kód ukazuje různé dotazy – používání obou Azure SQL DB Cosmos syntaxe, jakož i LINQ – které jsme dají spouštět i proti hello dokumenty, které jsme v předchozím kroku hello vložit.</span><span class="sxs-lookup"><span data-stu-id="fe21f-219">hello following sample code shows various queries - using both Azure Cosmos DB SQL syntax as well as LINQ - that we can run against hello documents we inserted in hello previous step.</span></span>

<span data-ttu-id="fe21f-220">Zkopírujte a vložte hello **ExecuteSimpleQuery** pod vaší **CreateFamilyDocumentIfNotExists** metoda.</span><span class="sxs-lookup"><span data-stu-id="fe21f-220">Copy and paste hello **ExecuteSimpleQuery** method underneath your **CreateFamilyDocumentIfNotExists** method.</span></span>

```csharp
// ADD THIS PART tooYOUR CODE
private void ExecuteSimpleQuery(string databaseName, string collectionName)
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

        // Here we find hello Andersen family via its LastName
        IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                .Where(f => f.LastName == "Andersen");

        // hello query is executed synchronously here, but can also be executed asynchronously via hello IDocumentQuery<T> interface
        Console.WriteLine("Running LINQ query...");
        foreach (Family family in familyQuery)
        {
            Console.WriteLine("\tRead {0}", family);
        }

        // Now execute hello same query via direct SQL
        IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                queryOptions);

        Console.WriteLine("Running direct SQL query...");
        foreach (Family family in familyQueryInSql)
        {
                Console.WriteLine("\tRead {0}", family);
        }

        Console.WriteLine("Press any key toocontinue ...");
        Console.ReadKey();
}
```

<span data-ttu-id="fe21f-221">Kopírování a vložení hello následující kód tooyour **GetStartedDemo** pod vytvoření druhého dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="fe21f-221">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello second document creation.</span></span>

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

// ADD THIS PART tooYOUR CODE
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

<span data-ttu-id="fe21f-222">Stiskněte klávesu hello **DocumentDBGettingStarted** tlačítko toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe21f-222">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="fe21f-223">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="fe21f-223">Congratulations!</span></span> <span data-ttu-id="fe21f-224">Úspěšně jste provedli dotaz na kolekci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fe21f-224">You have successfully queried against an Azure Cosmos DB collection.</span></span>

<span data-ttu-id="fe21f-225">Hello následující diagram ilustruje, jak se hello Azure Cosmos DB SQL dotazu, že se volá syntaxe proti kolekci hello jste vytvořili, a hello stejná logika platí také dotaz LINQ toohello.</span><span class="sxs-lookup"><span data-stu-id="fe21f-225">hello following diagram illustrates how hello Azure Cosmos DB SQL query syntax is called against hello collection you created, and hello same logic applies toohello LINQ query as well.</span></span>

![Diagram ilustrující hello obor a význam dotazu hello používá toocreate kurzu NoSQL hello konzolovou aplikaci C#](./media/documentdb-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

<span data-ttu-id="fe21f-227">Hello [FROM](documentdb-sql-query.md#FromClause) – klíčové slovo je volitelný hello dotazu, protože Azure Cosmos DB dotazy jsou již vymezená tooa jedinou kolekci.</span><span class="sxs-lookup"><span data-stu-id="fe21f-227">hello [FROM](documentdb-sql-query.md#FromClause) keyword is optional in hello query because Azure Cosmos DB queries are already scoped tooa single collection.</span></span> <span data-ttu-id="fe21f-228">Proto je možné příkaz „FROM Families f“ vyměnit za „FROM root r“ nebo jakoukoli jinou proměnnou, kterou si zvolíte.</span><span class="sxs-lookup"><span data-stu-id="fe21f-228">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="fe21f-229">DocumentDB odvodí, že rodiny, root nebo název proměnné hello jste vybrali, odkaz na aktuální kolekci hello ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="fe21f-229">DocumentDB will infer that Families, root, or hello variable name you chose, reference hello current collection by default.</span></span>

## <span data-ttu-id="fe21f-230"><a id="ReplaceDocument"></a>Krok 8: Nahrazení dokumentu JSON</span><span class="sxs-lookup"><span data-stu-id="fe21f-230"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="fe21f-231">Azure Cosmos DB podporuje nahrazování dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="fe21f-231">Azure Cosmos DB supports replacing JSON documents.</span></span>  

<span data-ttu-id="fe21f-232">Zkopírujte a vložte hello **ReplaceFamilyDocument** pod vaší **ExecuteSimpleQuery** metoda.</span><span class="sxs-lookup"><span data-stu-id="fe21f-232">Copy and paste hello **ReplaceFamilyDocument** method underneath your **ExecuteSimpleQuery** method.</span></span>

```csharp
// ADD THIS PART tooYOUR CODE
private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
{
    try
    {
        await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
        this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }
    catch (DocumentClientException de)
    {
        throw;
    }
}
```

<span data-ttu-id="fe21f-233">Kopírování a vložení hello následující kód tooyour **GetStartedDemo** pod spuštění dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="fe21f-233">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello query execution.</span></span> <span data-ttu-id="fe21f-234">Po nahrazení dokumentu hello, tím se spustí hello stejný dotaz znovu tooview hello změnit dokumentu.</span><span class="sxs-lookup"><span data-stu-id="fe21f-234">After replacing hello document, this will run hello same query again tooview hello changed document.</span></span>

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooYOUR CODE
// Update hello Grade of hello Andersen Family child
andersenFamily.Children[0].Grade = 6;

await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

<span data-ttu-id="fe21f-235">Stiskněte klávesu hello **DocumentDBGettingStarted** tlačítko toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe21f-235">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="fe21f-236">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="fe21f-236">Congratulations!</span></span> <span data-ttu-id="fe21f-237">Úspěšně jste nahradili dokument Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fe21f-237">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="fe21f-238"><a id="DeleteDocument"></a>Krok 9: Odstranění dokumentu JSON</span><span class="sxs-lookup"><span data-stu-id="fe21f-238"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="fe21f-239">Azure Cosmos DB podporuje odstraňování dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="fe21f-239">Azure Cosmos DB supports deleting JSON documents.</span></span>  

<span data-ttu-id="fe21f-240">Zkopírujte a vložte hello **DeleteFamilyDocument** pod vaší **ReplaceFamilyDocument** metoda.</span><span class="sxs-lookup"><span data-stu-id="fe21f-240">Copy and paste hello **DeleteFamilyDocument** method underneath your **ReplaceFamilyDocument** method.</span></span>

```csharp
// ADD THIS PART tooYOUR CODE
private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
{
    try
    {
        await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
        Console.WriteLine("Deleted Family {0}", documentName);
    }
    catch (DocumentClientException de)
    {
        throw;
    }
}
```

<span data-ttu-id="fe21f-241">Kopírování a vložení hello následující kód tooyour **GetStartedDemo** pod spuštění druhého dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="fe21f-241">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello second query execution.</span></span>

```csharp
await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooCODE
await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");
```

<span data-ttu-id="fe21f-242">Stiskněte klávesu hello **DocumentDBGettingStarted** tlačítko toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe21f-242">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="fe21f-243">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="fe21f-243">Congratulations!</span></span> <span data-ttu-id="fe21f-244">Úspěšně jste odstranili dokument Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fe21f-244">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="fe21f-245"><a id="DeleteDatabase"></a>Krok 10: Odstranění databáze hello</span><span class="sxs-lookup"><span data-stu-id="fe21f-245"><a id="DeleteDatabase"></a>Step 10: Delete hello database</span></span>
<span data-ttu-id="fe21f-246">Odstraňování hello vytvořené databáze dojde k odebrání hello databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).</span><span class="sxs-lookup"><span data-stu-id="fe21f-246">Deleting hello created database will remove hello database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="fe21f-247">Kopírování a vložení hello následující kód tooyour **GetStartedDemo** pod hello dokumentu odstranit toodelete hello celou databázi a její podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="fe21f-247">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello document delete toodelete hello entire database and all children resources.</span></span>

```csharp
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

// ADD THIS PART tooCODE
// Clean up/delete hello database
await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));
```

<span data-ttu-id="fe21f-248">Stiskněte klávesu hello **DocumentDBGettingStarted** tlačítko toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe21f-248">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="fe21f-249">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="fe21f-249">Congratulations!</span></span> <span data-ttu-id="fe21f-250">Úspěšně jste odstranili databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fe21f-250">You have successfully deleted an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="fe21f-251"><a id="Run"></a>Krok 11: Spuštění celé konzolové aplikace jazyka C#</span><span class="sxs-lookup"><span data-stu-id="fe21f-251"><a id="Run"></a>Step 11: Run your C# console application all together!</span></span>
<span data-ttu-id="fe21f-252">Stiskněte klávesu hello **DocumentDBGettingStarted** tlačítko v sadě Visual Studio toobuild hello aplikace v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="fe21f-252">Press hello **DocumentDBGettingStarted** button in Visual Studio toobuild hello application in debug mode.</span></span>

<span data-ttu-id="fe21f-253">Měli byste vidět hello výstup počáteční aplikace v okně konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="fe21f-253">You should see hello output of your get started app in hello console window.</span></span> <span data-ttu-id="fe21f-254">Hello výstup bude zobrazovat výsledky hello hello dotazy jsme přidali a by měl odpovídat ukázkovému textu hello níže.</span><span class="sxs-lookup"><span data-stu-id="fe21f-254">hello output will show hello results of hello queries we added and should match hello example text below.</span></span>

```
Created FamilyDB_oa
Press any key toocontinue ...
Created FamilyCollection_oa
Press any key toocontinue ...
Created Family Andersen.1
Press any key toocontinue ...
Created Family Wakefield.7
Press any key toocontinue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Replaced Family Andersen.1
Press any key toocontinue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Deleted Family Andersen.1
End of demo, press any key tooexit.
```

<span data-ttu-id="fe21f-255">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="fe21f-255">Congratulations!</span></span> <span data-ttu-id="fe21f-256">Po dokončení kurzu hello a mít práci konzolovou aplikaci C#!</span><span class="sxs-lookup"><span data-stu-id="fe21f-256">You've completed hello tutorial and have a working C# console application!</span></span>

## <span data-ttu-id="fe21f-257"><a id="GetSolution"></a>Získání úplného řešení kurzu hello</span><span class="sxs-lookup"><span data-stu-id="fe21f-257"><a id="GetSolution"></a> Get hello complete tutorial solution</span></span>
<span data-ttu-id="fe21f-258">toobuild hello řešení GetStarted, které obsahuje všechny ukázky hello v tomto článku, budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="fe21f-258">toobuild hello GetStarted solution that contains all hello samples in this article, you will need hello following:</span></span>

* <span data-ttu-id="fe21f-259">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="fe21f-259">An active Azure account.</span></span> <span data-ttu-id="fe21f-260">Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="fe21f-260">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="fe21f-261">[Účet Azure Cosmos DB][create-documentdb-dotnet.md#create-account].</span><span class="sxs-lookup"><span data-stu-id="fe21f-261">An [Azure Cosmos DB account][create-documentdb-dotnet.md#create-account].</span></span>
* <span data-ttu-id="fe21f-262">Hello [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) řešení, které jsou dostupné na Githubu.</span><span class="sxs-lookup"><span data-stu-id="fe21f-262">hello [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="fe21f-263">toorestore hello odkazy toohello DocumentDB rozhraní API pro Azure Cosmos DB .NET Core SDK v sadě Visual Studio, klikněte pravým tlačítkem na hello **GetStarted** řešení v Průzkumníku řešení a pak klikněte na tlačítko **povolit obnovení balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fe21f-263">toorestore hello references toohello DocumentDB API for Azure Cosmos DB .NET Core SDK in Visual Studio, right-click hello **GetStarted** solution in Solution Explorer, and then click **Enable NuGet Package Restore**.</span></span> <span data-ttu-id="fe21f-264">Potom v souboru Program.cs hello, aktualizujte hodnoty EndpointUrl a authorizationkey tak hello jak je popsáno v [připojení účtu Azure Cosmos DB tooan](#Connect).</span><span class="sxs-lookup"><span data-stu-id="fe21f-264">Next, in hello Program.cs file, update hello EndpointUrl and AuthorizationKey values as described in [Connect tooan Azure Cosmos DB account](#Connect).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe21f-265">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe21f-265">Next steps</span></span>
* <span data-ttu-id="fe21f-266">Chcete komplexnější kurz pro ASP.NET MVC?</span><span class="sxs-lookup"><span data-stu-id="fe21f-266">Want a more complex ASP.NET MVC tutorial?</span></span> <span data-ttu-id="fe21f-267">V tématu [kurz k ASP.NET MVC: vývoj webových aplikací s Azure Cosmos DB](documentdb-dotnet-application.md).</span><span class="sxs-lookup"><span data-stu-id="fe21f-267">See [ASP.NET MVC Tutorial: Web application development with Azure Cosmos DB](documentdb-dotnet-application.md).</span></span>
* <span data-ttu-id="fe21f-268">Chcete toodevelop Xamarin iOS, Android nebo formuláře aplikace pomocí hello DocumentDB rozhraní API pro Azure Cosmos DB .NET Core SDK?</span><span class="sxs-lookup"><span data-stu-id="fe21f-268">Want toodevelop a Xamarin iOS, Android, or Forms application using hello DocumentDB API for Azure Cosmos DB .NET Core SDK?</span></span> <span data-ttu-id="fe21f-269">V tématu [vytváření mobilních aplikací s Xamarin a Azure Cosmos DB](mobile-apps-with-xamarin.md).</span><span class="sxs-lookup"><span data-stu-id="fe21f-269">See [Build mobile applications with Xamarin and Azure Cosmos DB](mobile-apps-with-xamarin.md).</span></span>
* <span data-ttu-id="fe21f-270">Chcete tooperform škálování a výkon testování pomocí Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="fe21f-270">Want tooperform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="fe21f-271">V tématu [výkonu a možností škálování testování pomocí Azure Cosmos DB](performance-testing.md)</span><span class="sxs-lookup"><span data-stu-id="fe21f-271">See [Performance and scale testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="fe21f-272">Zjistěte, jak příliš[požadavky na monitorování Azure Cosmos DB, využití a úložiště](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="fe21f-272">Learn how too[Monitor Azure Cosmos DB requests, usage, and storage](monitor-accounts.md).</span></span>
* <span data-ttu-id="fe21f-273">Spouštění dotazů na našem ukázkovou datovou sadu v hello [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="fe21f-273">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="fe21f-274">toolearn Další informace o hello programovací model, najdete v části [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="fe21f-274">toolearn more about hello programming model, see [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span>

[create-documentdb-dotnet.md#create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-dotnetcore-get-started/nosql-tutorial-keys.png
