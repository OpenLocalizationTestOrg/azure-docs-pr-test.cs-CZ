---
title: "Azure Cosmos DB: Úvodní kurz k rozhraní DocumentDB API | Dokumentace Microsoftu"
description: "Kurz, v rámci kterého se vytvoří online databáze a konzolová aplikace v jazyce C# pomocí rozhraní DocumentDB API."
keywords: "kurz nosql, online databáze konzolová aplikace jazyka c#"
services: cosmos-db
documentationcenter: .net
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: bf08e031-718a-4a2a-89d6-91e12ff8797d
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2017
ms.author: anhoh
ms.openlocfilehash: 72f66081a6409f980ec6bca5188f585489245a36
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-documentdb-api-getting-started-tutorial"></a><span data-ttu-id="70400-104">Azure Cosmos DB: Úvodní kurz k rozhraní DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="70400-104">Azure Cosmos DB: DocumentDB API getting started tutorial</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="70400-105">.NET</span><span class="sxs-lookup"><span data-stu-id="70400-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="70400-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="70400-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="70400-107">Node.js pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="70400-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="70400-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="70400-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="70400-109">Java</span><span class="sxs-lookup"><span data-stu-id="70400-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="70400-110">C++</span><span class="sxs-lookup"><span data-stu-id="70400-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="70400-111">Vítejte v úvodním kurzu k rozhraní DocumentDB API služby Azure Cosmos DB!</span><span class="sxs-lookup"><span data-stu-id="70400-111">Welcome to the Azure Cosmos DB DocumentDB API getting started tutorial!</span></span> <span data-ttu-id="70400-112">Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky Azure Cosmos DB a dotazuje se na ně.</span><span class="sxs-lookup"><span data-stu-id="70400-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="70400-113">Budeme se zabývat těmito tématy:</span><span class="sxs-lookup"><span data-stu-id="70400-113">We'll cover:</span></span>

* <span data-ttu-id="70400-114">Vytvoření účtu služby Azure Cosmos DB a připojení k němu</span><span class="sxs-lookup"><span data-stu-id="70400-114">Creating and connecting to an Azure Cosmos DB account</span></span>
* <span data-ttu-id="70400-115">Konfigurace řešení v nástroji Visual Studio</span><span class="sxs-lookup"><span data-stu-id="70400-115">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="70400-116">Vytvoření online databáze</span><span class="sxs-lookup"><span data-stu-id="70400-116">Creating an online database</span></span>
* <span data-ttu-id="70400-117">Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="70400-117">Creating a collection</span></span>
* <span data-ttu-id="70400-118">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="70400-118">Creating JSON documents</span></span>
* <span data-ttu-id="70400-119">Dotazování na kolekci</span><span class="sxs-lookup"><span data-stu-id="70400-119">Querying the collection</span></span>
* <span data-ttu-id="70400-120">Nahrazení dokumentu</span><span class="sxs-lookup"><span data-stu-id="70400-120">Replacing a document</span></span>
* <span data-ttu-id="70400-121">Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="70400-121">Deleting a document</span></span>
* <span data-ttu-id="70400-122">Odstranění databáze</span><span class="sxs-lookup"><span data-stu-id="70400-122">Deleting the database</span></span>

<span data-ttu-id="70400-123">Nemáte čas?</span><span class="sxs-lookup"><span data-stu-id="70400-123">Don't have time?</span></span> <span data-ttu-id="70400-124">Nevadí!</span><span class="sxs-lookup"><span data-stu-id="70400-124">Don't worry!</span></span> <span data-ttu-id="70400-125">Úplné řešení je k dispozici na [GitHubu](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="70400-125">The complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span></span> <span data-ttu-id="70400-126">Pro rychlé pokyny přeskočte na [oddíl Získání úplného řešení kurzu NoSQL](#GetSolution).</span><span class="sxs-lookup"><span data-stu-id="70400-126">Jump to the [Get the complete NoSQL tutorial solution section](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="70400-127">Potom prosím použijte hlasovací tlačítka v horní nebo dolní části stránky, abychom získali zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="70400-127">Afterwards, please use the voting buttons at the top or bottom of this page to give us feedback.</span></span> <span data-ttu-id="70400-128">Pokud chcete, abychom vás kontaktovali přímo, můžete nám nechat e-mailovou adresu v komentářích.</span><span class="sxs-lookup"><span data-stu-id="70400-128">If you'd like us to contact you directly, feel free to include your email address in your comments.</span></span>

<span data-ttu-id="70400-129">Můžeme začít!</span><span class="sxs-lookup"><span data-stu-id="70400-129">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70400-130">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="70400-130">Prerequisites</span></span>
<span data-ttu-id="70400-131">Ujistěte se prosím, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="70400-131">Please make sure you have the following:</span></span>

* <span data-ttu-id="70400-132">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="70400-132">An active Azure account.</span></span> <span data-ttu-id="70400-133">Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="70400-133">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="70400-134">Alternativně můžete pro tento kurz použít [emulátor služby Azure Cosmos DB](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="70400-134">Alternatively, you can use the [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="70400-135">[Visual Studio Community 2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="70400-135">[Visual Studio Community 2017](http://www.visualstudio.com/).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="70400-136">Krok 1: Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="70400-136">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="70400-137">Vytvořme účet služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="70400-137">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="70400-138">Pokud již máte účet, který chcete použít, můžete přeskočit na [Nastavení řešení v nástroji Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="70400-138">If you already have an account you want to use, you can skip ahead to [Setup your Visual Studio Solution](#SetupVS).</span></span> <span data-ttu-id="70400-139">Pokud používáte emulátor služby Azure Cosmos DB, nastavte emulátor pomocí postupu v tématu [Emulátor služby Azure Cosmos DB](local-emulator.md) a přeskočte k části [Nastavení řešení v sadě Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="70400-139">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Setup your Visual Studio Solution](#SetupVS).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="70400-140"><a id="SetupVS"></a>Krok 2: Nastavení řešení v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="70400-140"><a id="SetupVS"></a>Step 2: Setup your Visual Studio solution</span></span>
1. <span data-ttu-id="70400-141">Otevřete v počítači **Visual Studio 2017**.</span><span class="sxs-lookup"><span data-stu-id="70400-141">Open **Visual Studio 2017** on your computer.</span></span>
2. <span data-ttu-id="70400-142">V nabídce **Soubor** vyberte **Nový** a zvolte **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="70400-142">On the **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="70400-143">V dialogovém okně **Nový projekt** vyberte **Šablony** / **Visual C#** / **Konzolová aplikace**, pojmenujte svůj projekt a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="70400-143">In the **New Project** dialog, select **Templates** / **Visual C#** / **Console Application**, name your project, and then click **OK**.</span></span>
   <span data-ttu-id="70400-144">![Snímek obrazovky okna Nový projekt](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)</span><span class="sxs-lookup"><span data-stu-id="70400-144">![Screen shot of the New Project window](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)</span></span>
4. <span data-ttu-id="70400-145">V **Průzkumníku řešení** klikněte pravým tlačítkem na novou konzolovou aplikaci v rámci řešení sady Visual Studio a pak klikněte na **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="70400-145">In the **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
    
    ![Snímek obrazovky místní nabídky projektu](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges.png)
5. <span data-ttu-id="70400-147">Na kartě **NuGet** klikněte na **Procházet** a do vyhledávacího pole zadejte **azure documentdb**.</span><span class="sxs-lookup"><span data-stu-id="70400-147">In the **Nuget** tab, click **Browse**, and type **azure documentdb** in the search box.</span></span>
6. <span data-ttu-id="70400-148">Najděte ve výsledcích **Microsoft.Azure.DocumentDB** a klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="70400-148">Within the results, find **Microsoft.Azure.DocumentDB** and click **Install**.</span></span>
   <span data-ttu-id="70400-149">Je třeba ID balíčku klientské knihovny Azure Cosmos databáze DocumentDB rozhraní API [Microsoft Azure DocumentDB Client Library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).</span><span class="sxs-lookup"><span data-stu-id="70400-149">The package ID for the Azure Cosmos DB DocumentDB API Client Library is [Microsoft Azure DocumentDB Client Library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).</span></span>
   <span data-ttu-id="70400-150">![Snímek obrazovky nabídky Nuget pro vyhledání sady Azure Cosmos DB Client SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)</span><span class="sxs-lookup"><span data-stu-id="70400-150">![Screen shot of the Nuget Menu for finding Azure Cosmos DB Client SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)</span></span>

    <span data-ttu-id="70400-151">Pokud se vám zobrazí zpráva týkající se kontroly změn řešení, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="70400-151">If you get a messages about reviewing changes to the solution, click **OK**.</span></span> <span data-ttu-id="70400-152">Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.</span><span class="sxs-lookup"><span data-stu-id="70400-152">If you get a message about license acceptance, click **I accept**.</span></span>

<span data-ttu-id="70400-153">Výborně!</span><span class="sxs-lookup"><span data-stu-id="70400-153">Great!</span></span> <span data-ttu-id="70400-154">Teď když jsme dokončili nastavování, napišme nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="70400-154">Now that we finished the setup, let's start writing some code.</span></span> <span data-ttu-id="70400-155">Úplný projekt s kódem pro tento kurz najdete na [GitHubu](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="70400-155">You can find a completed code project of this tutorial at [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).</span></span>

## <span data-ttu-id="70400-156"><a id="Connect"></a>Krok 3: Připojení k účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="70400-156"><a id="Connect"></a>Step 3: Connect to an Azure Cosmos DB account</span></span>
<span data-ttu-id="70400-157">Nejprve přidejte na začátek aplikace C# do souboru Program.cs tyto reference:</span><span class="sxs-lookup"><span data-stu-id="70400-157">First, add these references to the beginning of your C# application, in the Program.cs file:</span></span>

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART TO YOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [!IMPORTANT]
> <span data-ttu-id="70400-158">Přidání výše uvedených závislostí je nezbytné pro dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="70400-158">In order to complete the tutorial, make sure you add the dependencies above.</span></span>
> 
> 

<span data-ttu-id="70400-159">Nyní přidejte tyto dvě konstanty a proměnnou *client* pod veřejnou třídu *Program*.</span><span class="sxs-lookup"><span data-stu-id="70400-159">Now, add these two constants and your *client* variable underneath your public class *Program*.</span></span>

    public class Program
    {
        // ADD THIS PART TO YOUR CODE
        private const string EndpointUrl = "<your endpoint URL>";
        private const string PrimaryKey = "<your primary key>";
        private DocumentClient client;

<span data-ttu-id="70400-160">Dále přejděte zpět na [Azure Portal](https://portal.azure.com) a získejte adresu URL koncového bodu a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="70400-160">Next, head back to the [Azure Portal](https://portal.azure.com) to retrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="70400-161">Adresa URL koncového bodu a primární klíč jsou potřeba k tomu, aby aplikace věděla, kam se má připojit, a aby služba Azure Cosmos DB důvěřovala připojení aplikace.</span><span class="sxs-lookup"><span data-stu-id="70400-161">The endpoint URL and primary key are necessary for your application to understand where to connect to, and for Azure Cosmos DB to trust your application's connection.</span></span>

<span data-ttu-id="70400-162">Na webu Azure Portal přejděte do účtu služby Azure Cosmos DB a klikněte na **Klíče**.</span><span class="sxs-lookup"><span data-stu-id="70400-162">In the Azure Portal, navigate to your Azure Cosmos DB account, and then click **Keys**.</span></span>

<span data-ttu-id="70400-163">Zkopírujte identifikátor URI z portálu a vložte ho do `<your endpoint URL>` v souboru program.cs.</span><span class="sxs-lookup"><span data-stu-id="70400-163">Copy the URI from the portal and paste it into `<your endpoint URL>` in the program.cs file.</span></span> <span data-ttu-id="70400-164">Poté zkopírujte PRIMÁRNÍ KLÍČ z portálu a vložte ho do `<your primary key>`.</span><span class="sxs-lookup"><span data-stu-id="70400-164">Then copy the PRIMARY KEY from the portal and paste it into `<your primary key>`.</span></span>

![Snímek obrazovky Portálu Azure, který se v kurzu NoSQL používá k vytvoření konzolové aplikace v C#.][keys]

<span data-ttu-id="70400-167">Potom vytvořením nové instance **DocumentClient** spustíme aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70400-167">Next, we'll start the application by creating a new instance of the **DocumentClient**.</span></span>

<span data-ttu-id="70400-168">Pod metodu **Main** přidejte tento nový asynchronní úkol pojmenovaný **GetStartedDemo**, který vytvoří instanci našeho nového klienta **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="70400-168">Below the **Main** method, add this new asynchronous task called **GetStartedDemo**, which will instantiate our new **DocumentClient**.</span></span>

    static void Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
    }

<span data-ttu-id="70400-169">Přidejte následující kód, aby se asynchronní úkol spustil z metody **Main**.</span><span class="sxs-lookup"><span data-stu-id="70400-169">Add the following code to run your asynchronous task from your **Main** method.</span></span> <span data-ttu-id="70400-170">Metoda **Main** zachytí výjimky a vypíše je do konzoly.</span><span class="sxs-lookup"><span data-stu-id="70400-170">The **Main** method will catch exceptions and write them to the console.</span></span>

    static void Main(string[] args)
    {
            // ADD THIS PART TO YOUR CODE
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
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
            }

<span data-ttu-id="70400-171">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70400-171">Press **F5** to run your application.</span></span> <span data-ttu-id="70400-172">Výstup okna konzoly zobrazuje zprávu `End of demo, press any key to exit.`, která potvrzuje vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="70400-172">The console window output displays the message `End of demo, press any key to exit.` confirming that the connection was made.</span></span>  <span data-ttu-id="70400-173">Potom můžete okno konzoly zavřít.</span><span class="sxs-lookup"><span data-stu-id="70400-173">You can then close the console window.</span></span> 

<span data-ttu-id="70400-174">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="70400-174">Congratulations!</span></span> <span data-ttu-id="70400-175">Úspěšně jste se připojili k účtu služby Azure Cosmos DB. Nyní se podívejme, jak se pracuje s prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="70400-175">You have successfully connected to an Azure Cosmos DB account, let's now take a look at working with Azure Cosmos DB resources.</span></span>  

## <a name="step-4-create-a-database"></a><span data-ttu-id="70400-176">Krok 4: Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="70400-176">Step 4: Create a database</span></span>
<span data-ttu-id="70400-177">Než přidáte kód pro vytvoření databáze, přidejte pomocnou metodu pro výpis do konzoly.</span><span class="sxs-lookup"><span data-stu-id="70400-177">Before you add the code for creating a database, add a helper method for writing to the console.</span></span>

<span data-ttu-id="70400-178">Zkopírujte a vložte metodu **WriteToConsoleAndPromptToContinue** za metodu **GetStartedDemo**.</span><span class="sxs-lookup"><span data-stu-id="70400-178">Copy and paste the **WriteToConsoleAndPromptToContinue** method after the **GetStartedDemo** method.</span></span>

    // ADD THIS PART TO YOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

<span data-ttu-id="70400-179">[Databázi](documentdb-resources.md#databases) Azure Cosmos DB je možné vytvořit pomocí metody [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) třídy **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="70400-179">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using the [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="70400-180">Databáze je logický kontejner úložiště dokumentů JSON rozděleného mezi kolekcemi.</span><span class="sxs-lookup"><span data-stu-id="70400-180">A database is the logical container of JSON document storage partitioned across collections.</span></span>

<span data-ttu-id="70400-181">Zkopírujte a vložte následující kód do metody **GetStartedDemo** za vytvoření klienta.</span><span class="sxs-lookup"><span data-stu-id="70400-181">Copy and paste the following code to your **GetStartedDemo** method after the client creation.</span></span> <span data-ttu-id="70400-182">Tím se vytvoří databáze s názvem *FamilyDB*.</span><span class="sxs-lookup"><span data-stu-id="70400-182">This will create a database named *FamilyDB*.</span></span>

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        // ADD THIS PART TO YOUR CODE
        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

<span data-ttu-id="70400-183">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70400-183">Press **F5** to run your application.</span></span>

<span data-ttu-id="70400-184">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="70400-184">Congratulations!</span></span> <span data-ttu-id="70400-185">Úspěšně jste vytvořili databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="70400-185">You have successfully created an Azure Cosmos DB database.</span></span>  

## <span data-ttu-id="70400-186"><a id="CreateColl"></a>Krok 5: Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="70400-186"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="70400-187">**CreateDocumentCollectionIfNotExistsAsync** vytvoří novou kolekci s vyhrazenou propustností, za kterou se hradí poplatky.</span><span class="sxs-lookup"><span data-stu-id="70400-187">**CreateDocumentCollectionIfNotExistsAsync** will create a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="70400-188">Další podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="70400-188">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="70400-189">[Kolekci](documentdb-resources.md#collections) je možné vytvořit pomocí metody [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) třídy **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="70400-189">A [collection](documentdb-resources.md#collections) can be created by using the [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="70400-190">Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.</span><span class="sxs-lookup"><span data-stu-id="70400-190">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="70400-191">Zkopírujte a vložte následující kód do metody **GetStartedDemo** za vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="70400-191">Copy and paste the following code to your **GetStartedDemo** method after the database creation.</span></span> <span data-ttu-id="70400-192">Tím se vytvoří kolekce dokumentů s názvem *FamilyCollection*.</span><span class="sxs-lookup"><span data-stu-id="70400-192">This will create a document collection named *FamilyCollection*.</span></span>

        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

        // ADD THIS PART TO YOUR CODE
         await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });

<span data-ttu-id="70400-193">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70400-193">Press **F5** to run your application.</span></span>

<span data-ttu-id="70400-194">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="70400-194">Congratulations!</span></span> <span data-ttu-id="70400-195">Úspěšně jste vytvořili kolekci dokumentů Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="70400-195">You have successfully created an Azure Cosmos DB document collection.</span></span>  

## <span data-ttu-id="70400-196"><a id="CreateDoc"></a>Krok 6: Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="70400-196"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="70400-197">[Dokument](documentdb-resources.md#documents) je možné vytvořit pomocí metody [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) třídy **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="70400-197">A [document](documentdb-resources.md#documents) can be created by using the [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="70400-198">Dokumenty představují uživatelem definovaný (libovolný) obsah JSON.</span><span class="sxs-lookup"><span data-stu-id="70400-198">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="70400-199">Nyní můžete vložit jeden nebo více dokumentů.</span><span class="sxs-lookup"><span data-stu-id="70400-199">We can now insert one or more documents.</span></span> <span data-ttu-id="70400-200">Pokud již máte data, která chcete uložit do databáze, můžete použít Azure Cosmos DB [nástroj pro migraci dat](import-data.md) pro import dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="70400-200">If you already have data you'd like to store in your database, you can use the Azure Cosmos DB [Data Migration tool](import-data.md) to import the data into a database.</span></span>

<span data-ttu-id="70400-201">Nejprve musíme vytvořit třídu **Family**, která bude v této ukázce představovat objekty uložené ve službě Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="70400-201">First, we need to create a **Family** class that will represent objects stored within Azure Cosmos DB in this sample.</span></span> <span data-ttu-id="70400-202">Kromě toho vytvoříme i podtřídy **Parent**, **Child**, **Pet** a **Address**, které se použijí v rámci **Family**.</span><span class="sxs-lookup"><span data-stu-id="70400-202">We will also create **Parent**, **Child**, **Pet**, **Address** subclasses that are used within **Family**.</span></span> <span data-ttu-id="70400-203">Povšimněte si, že dokumenty musí mít vlastnost **Id** serializovanou jako **id** ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="70400-203">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span> <span data-ttu-id="70400-204">Vytvořte tyto třídy tak, že za metodu **GetStartedDemo** přidáte následující vnitřní podtřídy.</span><span class="sxs-lookup"><span data-stu-id="70400-204">Create these classes by adding the following internal sub-classes after the **GetStartedDemo** method.</span></span>

<span data-ttu-id="70400-205">Zkopírujte a vložte třídy **Family**, **Parent**, **Child**, **Pet** a **Address** za metodu **WriteToConsoleAndPromptToContinue**.</span><span class="sxs-lookup"><span data-stu-id="70400-205">Copy and paste the **Family**, **Parent**, **Child**, **Pet**, and **Address** classes after the **WriteToConsoleAndPromptToContinue** method.</span></span>

    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="70400-206">Zkopírujte a vložte metodu **CreateFamilyDocumentIfNotExists** pod třídu **Address**.</span><span class="sxs-lookup"><span data-stu-id="70400-206">Copy and paste the **CreateFamilyDocumentIfNotExists** method underneath your **Address** class.</span></span>

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="70400-207">Vložte dva dokumenty, jeden pro rodinu Andersenů a druhý pro rodinu Wakefieldů.</span><span class="sxs-lookup"><span data-stu-id="70400-207">And insert two documents, one each for the Andersen Family and the Wakefield Family.</span></span>

<span data-ttu-id="70400-208">Zkopírujte a vložte následující kód do metody **GetStartedDemo** za vytvoření kolekce dokumentů.</span><span class="sxs-lookup"><span data-stu-id="70400-208">Copy and paste the following code to your **GetStartedDemo** method after the document collection creation.</span></span>

    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });
    
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });


    // ADD THIS PART TO YOUR CODE
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

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", andersenFamily);

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

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

<span data-ttu-id="70400-209">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70400-209">Press **F5** to run your application.</span></span>

<span data-ttu-id="70400-210">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="70400-210">Congratulations!</span></span> <span data-ttu-id="70400-211">Úspěšně jste vytvořili dva dokumenty Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="70400-211">You have successfully created two Azure Cosmos DB documents.</span></span>  

![Diagram ilustrující hierarchický vztah mezi účtem, online databází, kolekcí a dokumenty používanými v kurzu NoSQL k vytvoření konzolové aplikace v jazyce C#](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="70400-213"><a id="Query"></a>Krok 7: Dotazování prostředků Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="70400-213"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="70400-214">Azure Cosmos DB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí.</span><span class="sxs-lookup"><span data-stu-id="70400-214">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="70400-215">Následující ukázkový kód ukazuje různé dotazy – používající jak syntaxi SQL služby Azure Cosmos DB, tak LINQ – které můžeme spouštět na dokumenty vložené v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="70400-215">The following sample code shows various queries - using both Azure Cosmos DB SQL syntax as well as LINQ - that we can run against the documents we inserted in the previous step.</span></span>

<span data-ttu-id="70400-216">Zkopírujte a vložte metodu **ExecuteSimpleQuery** za metodu **CreateFamilyDocumentIfNotExists**.</span><span class="sxs-lookup"><span data-stu-id="70400-216">Copy and paste the **ExecuteSimpleQuery** method after your **CreateFamilyDocumentIfNotExists** method.</span></span>

    // ADD THIS PART TO YOUR CODE
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

            // Here we find the Andersen family via its LastName
            IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                    .Where(f => f.LastName == "Andersen");

            // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
            Console.WriteLine("Running LINQ query...");
            foreach (Family family in familyQuery)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            // Now execute the same query via direct SQL
            IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                    "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                    queryOptions);

            Console.WriteLine("Running direct SQL query...");
            foreach (Family family in familyQueryInSql)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

<span data-ttu-id="70400-217">Zkopírujte a vložte následující kód do metody **GetStartedDemo** za vytvoření druhého dokumentu.</span><span class="sxs-lookup"><span data-stu-id="70400-217">Copy and paste the following code to your **GetStartedDemo** method after the second document creation.</span></span>

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    // ADD THIS PART TO YOUR CODE
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

<span data-ttu-id="70400-218">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70400-218">Press **F5** to run your application.</span></span>

<span data-ttu-id="70400-219">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="70400-219">Congratulations!</span></span> <span data-ttu-id="70400-220">Úspěšně jste provedli dotaz na kolekci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="70400-220">You have successfully queried against an Azure Cosmos DB collection.</span></span>

<span data-ttu-id="70400-221">Následující diagram ilustruje volání syntaxe příkazu jazyka SQL služby Azure Cosmos DB na kolekci, kterou jste vytvořili. Stejná logika platí také pro dotaz LINQ.</span><span class="sxs-lookup"><span data-stu-id="70400-221">The following diagram illustrates how the Azure Cosmos DB SQL query syntax is called against the collection you created, and the same logic applies to the LINQ query as well.</span></span>

![Diagram ilustrující obor a význam dotazu použitého v kurzu NoSQL k vytvoření konzolové aplikace v jazyce C#](./media/documentdb-get-started/nosql-tutorial-collection-documents.png)

<span data-ttu-id="70400-223">[FROM](documentdb-sql-query.md#FromClause) – klíčové slovo je v dotazu volitelné, protože Azure Cosmos DB dotazy již mají obor nastaven na jedinou kolekci.</span><span class="sxs-lookup"><span data-stu-id="70400-223">The [FROM](documentdb-sql-query.md#FromClause) keyword is optional in the query because Azure Cosmos DB queries are already scoped to a single collection.</span></span> <span data-ttu-id="70400-224">Proto je možné příkaz „FROM Families f“ vyměnit za „FROM root r“ nebo jakoukoli jinou proměnnou, kterou si zvolíte.</span><span class="sxs-lookup"><span data-stu-id="70400-224">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="70400-225">Azure Cosmos DB bude odvození že Families, root nebo název proměnné, které jste zvolili, odkazují na aktuální kolekci ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="70400-225">Azure Cosmos DB will infer that Families, root, or the variable name you chose, reference the current collection by default.</span></span>

## <span data-ttu-id="70400-226"><a id="ReplaceDocument"></a>Krok 8: Nahrazení dokumentu JSON</span><span class="sxs-lookup"><span data-stu-id="70400-226"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="70400-227">Azure Cosmos DB podporuje nahrazování dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="70400-227">Azure Cosmos DB supports replacing JSON documents.</span></span>  

<span data-ttu-id="70400-228">Zkopírujte a vložte metodu **ReplaceFamilyDocument** za metodu **ExecuteSimpleQuery**.</span><span class="sxs-lookup"><span data-stu-id="70400-228">Copy and paste the **ReplaceFamilyDocument** method after your **ExecuteSimpleQuery** method.</span></span>

    // ADD THIS PART TO YOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
         await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
         this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }

<span data-ttu-id="70400-229">Zkopírujte a vložte následující kód do metody **GetStartedDemo** za spuštění dotazu na konec metody.</span><span class="sxs-lookup"><span data-stu-id="70400-229">Copy and paste the following code to your **GetStartedDemo** method after the query execution, at the end of the method.</span></span> <span data-ttu-id="70400-230">Po nahrazení dokumentu tento kód spustí stejný dotaz znovu, aby se zobrazil změněný dokument.</span><span class="sxs-lookup"><span data-stu-id="70400-230">After replacing the document, this will run the same query again to view the changed document.</span></span>

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    // ADD THIS PART TO YOUR CODE
    // Update the Grade of the Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

<span data-ttu-id="70400-231">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70400-231">Press **F5** to run your application.</span></span>

<span data-ttu-id="70400-232">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="70400-232">Congratulations!</span></span> <span data-ttu-id="70400-233">Úspěšně jste nahradili dokument Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="70400-233">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="70400-234"><a id="DeleteDocument"></a>Krok 9: Odstranění dokumentu JSON</span><span class="sxs-lookup"><span data-stu-id="70400-234"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="70400-235">Azure Cosmos DB podporuje odstraňování dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="70400-235">Azure Cosmos DB supports deleting JSON documents.</span></span>  

<span data-ttu-id="70400-236">Zkopírujte a vložte metodu **DeleteFamilyDocument** za metodu **ReplaceFamilyDocument**.</span><span class="sxs-lookup"><span data-stu-id="70400-236">Copy and paste the **DeleteFamilyDocument** method after your **ReplaceFamilyDocument** method.</span></span>

    // ADD THIS PART TO YOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
         await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
         Console.WriteLine("Deleted Family {0}", documentName);
    }

<span data-ttu-id="70400-237">Zkopírujte a vložte následující kód do metody **GetStartedDemo** za spuštění druhého dotazu na konec metody.</span><span class="sxs-lookup"><span data-stu-id="70400-237">Copy and paste the following code to your **GetStartedDemo** method after the second query execution, at the end of the method.</span></span>

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);
    
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
    
    // ADD THIS PART TO CODE
    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

<span data-ttu-id="70400-238">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70400-238">Press **F5** to run your application.</span></span>

<span data-ttu-id="70400-239">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="70400-239">Congratulations!</span></span> <span data-ttu-id="70400-240">Úspěšně jste odstranili dokument Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="70400-240">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="70400-241"><a id="DeleteDatabase"></a>Krok 10: Odstranění databáze</span><span class="sxs-lookup"><span data-stu-id="70400-241"><a id="DeleteDatabase"></a>Step 10: Delete the database</span></span>
<span data-ttu-id="70400-242">Odstraněním vytvořené databáze dojde k odstranění databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).</span><span class="sxs-lookup"><span data-stu-id="70400-242">Deleting the created database will remove the database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="70400-243">Pokud chcete odstranit celou databázi a její podřízené prostředky, zkopírujte a vložte následující kód do metody **GetStartedDemo** za odstranění dokumentu.</span><span class="sxs-lookup"><span data-stu-id="70400-243">Copy and paste the following code to your **GetStartedDemo** method after the document delete to delete the entire database and all children resources.</span></span>

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

    // ADD THIS PART TO CODE
    // Clean up/delete the database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));

<span data-ttu-id="70400-244">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="70400-244">Press **F5** to run your application.</span></span>

<span data-ttu-id="70400-245">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="70400-245">Congratulations!</span></span> <span data-ttu-id="70400-246">Úspěšně jste odstranili databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="70400-246">You have successfully deleted an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="70400-247"><a id="Run"></a>Krok 11: Spuštění celé konzolové aplikace jazyka C#</span><span class="sxs-lookup"><span data-stu-id="70400-247"><a id="Run"></a>Step 11: Run your C# console application all together!</span></span>
<span data-ttu-id="70400-248">Stiskněte v nástroji Visual Studio klávesu F5 – aplikace se sestaví v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="70400-248">Hit F5 in Visual Studio to build the application in debug mode.</span></span>

<span data-ttu-id="70400-249">Měli byste vidět výstup počáteční aplikace v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="70400-249">You should see the output of your get started app in a console window.</span></span> <span data-ttu-id="70400-250">Výstup bude zobrazovat výsledky dotazů, které jsme přidali, a měl by odpovídat ukázkovému textu níže.</span><span class="sxs-lookup"><span data-stu-id="70400-250">The output will show the results of the queries we added and should match the example text below.</span></span>

    Created FamilyDB
    Press any key to continue ...
    Created FamilyCollection
    Press any key to continue ...
    Created Family Andersen.1
    Press any key to continue ...
    Created Family Wakefield.7
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Replaced Family Andersen.1
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Deleted Family Andersen.1
    End of demo, press any key to exit.

<span data-ttu-id="70400-251">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="70400-251">Congratulations!</span></span> <span data-ttu-id="70400-252">Dokončili jste tento kurz a máte funkční konzolovou aplikaci jazyka C#!</span><span class="sxs-lookup"><span data-stu-id="70400-252">You've completed the tutorial and have a working C# console application!</span></span>

## <span data-ttu-id="70400-253"><a id="GetSolution"></a>Získání úplného řešení kurzu</span><span class="sxs-lookup"><span data-stu-id="70400-253"><a id="GetSolution"></a> Get the complete tutorial solution</span></span>
<span data-ttu-id="70400-254">Pokud jste neměli dostatek času k dokončení kroků v tomto kurzu nebo si jen chcete stáhnout ukázky kódu, můžete je získat z [GitHubu](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="70400-254">If you didn't have time to complete the steps in this tutorial, or just want to download the code samples, you can get it from [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span></span> 

<span data-ttu-id="70400-255">K vytvoření řešení GetStarted budete potřebovat toto:</span><span class="sxs-lookup"><span data-stu-id="70400-255">To build the GetStarted solution, you will need the following:</span></span>

* <span data-ttu-id="70400-256">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="70400-256">An active Azure account.</span></span> <span data-ttu-id="70400-257">Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="70400-257">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="70400-258">[Účet Azure Cosmos DB][cosmos-db-create-account].</span><span class="sxs-lookup"><span data-stu-id="70400-258">An [Azure Cosmos DB account][cosmos-db-create-account].</span></span>
* <span data-ttu-id="70400-259">Řešení [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) dostupné na GitHubu</span><span class="sxs-lookup"><span data-stu-id="70400-259">The [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="70400-260">Chcete-li obnovit odkazy na Cosmos DB .NET SDK služby Azure v sadě Visual Studio, klikněte pravým tlačítkem **GetStarted** řešení v Průzkumníku řešení a pak klikněte na tlačítko **povolit obnovení balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="70400-260">To restore the references to the Azure Cosmos DB .NET SDK in Visual Studio, right-click the **GetStarted** solution in Solution Explorer, and then click **Enable NuGet Package Restore**.</span></span> <span data-ttu-id="70400-261">Dále v souboru App.config aktualizujte hodnoty EndpointUrl a AuthorizationKey tak, jak je popsáno v části [Připojení k účtu služby Azure Cosmos DB](#Connect).</span><span class="sxs-lookup"><span data-stu-id="70400-261">Next, in the App.config file, update the EndpointUrl and AuthorizationKey values as described in [Connect to an Azure Cosmos DB account](#Connect).</span></span>

<span data-ttu-id="70400-262">A to je vše, stačí sestavit a máte hotovo.</span><span class="sxs-lookup"><span data-stu-id="70400-262">That's it, build it and you're on your way!</span></span>


## <a name="next-steps"></a><span data-ttu-id="70400-263">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70400-263">Next steps</span></span>
* <span data-ttu-id="70400-264">Chcete komplexnější kurz pro ASP.NET MVC?</span><span class="sxs-lookup"><span data-stu-id="70400-264">Want a more complex ASP.NET MVC tutorial?</span></span> <span data-ttu-id="70400-265">V tématu [kurz k ASP.NET MVC: vývoj webových aplikací s Azure Cosmos DB](documentdb-dotnet-application.md).</span><span class="sxs-lookup"><span data-stu-id="70400-265">See [ASP.NET MVC Tutorial: Web application development with Azure Cosmos DB](documentdb-dotnet-application.md).</span></span>
* <span data-ttu-id="70400-266">Chcete testovat škálování a výkon pomocí služby Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="70400-266">Want to perform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="70400-267">V tématu [výkonu a možností škálování testování pomocí Azure Cosmos DB](performance-testing.md)</span><span class="sxs-lookup"><span data-stu-id="70400-267">See [Performance and scale testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="70400-268">Zjistěte, jak [sledování požadavků, využití a úložiště Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="70400-268">Learn how to [monitor Azure Cosmos DB requests, usage, and storage](monitor-accounts.md).</span></span>
* <span data-ttu-id="70400-269">Spouštějte dotazy proti ukázkovým datovým sadám v [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="70400-269">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="70400-270">Další informace o databázi Cosmos Azure najdete v tématu [Vítá vás Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="70400-270">To learn more about Azure Cosmos DB, see [Welcome to Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span></span>

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
[cosmos-db-create-account]: create-documentdb-dotnet.md#create-account
