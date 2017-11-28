---
title: "Práce s knihovnou klienta spravovaného App Service Mobile Apps (Windows | Microsoft Docs"
description: "Další informace o použití klient .NET pro Azure App Service Mobile Apps s aplikacemi pro Windows a Xamarin."
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 0280785c-e027-4e0d-aaf2-6f155e5a6197
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2017
ms.author: glenga
ms.openlocfilehash: 5f4cc3e97ba7adde2aaac471951a3130d79910f6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a><span data-ttu-id="e422b-103">Jak používat spravovaného klienta pro Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="e422b-103">How to use the managed client for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a><span data-ttu-id="e422b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e422b-104">Overview</span></span>
<span data-ttu-id="e422b-105">Tento průvodce vám ukáže, jak provádět běžné scénáře s využitím spravované klientské knihovny pro Azure App Service mobilní aplikace pro Windows a aplikacemi Xamarin.</span><span class="sxs-lookup"><span data-stu-id="e422b-105">This guide shows you how to perform common scenarios using the managed client library for Azure App Service Mobile Apps for Windows and Xamarin apps.</span></span> <span data-ttu-id="e422b-106">Pokud jste ještě Mobile Apps, zvažte nejdřív dokončení [rychlý start Azure Mobile Apps] [ 1] kurzu.</span><span class="sxs-lookup"><span data-stu-id="e422b-106">If you are new to Mobile Apps, you should consider first completing the [Azure Mobile Apps quickstart][1] tutorial.</span></span> <span data-ttu-id="e422b-107">V této příručce se zaměříme na spravované SDK klienta.</span><span class="sxs-lookup"><span data-stu-id="e422b-107">In this guide, we focus on the client-side managed SDK.</span></span> <span data-ttu-id="e422b-108">Další informace o na straně serveru SDK pro Mobile Apps, naleznete v dokumentaci k [.NET serveru SDK] [ 2] nebo [Node.js Server SDK][3].</span><span class="sxs-lookup"><span data-stu-id="e422b-108">To learn more about the server-side SDKs for Mobile Apps, see the documentation for the [.NET Server SDK][2] or the [Node.js Server SDK][3].</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="e422b-109">Referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="e422b-109">Reference documentation</span></span>
<span data-ttu-id="e422b-110">Referenční dokumentaci k nástroji pro klienta SDK se nachází zde: [Azure Mobile Apps .NET – referenční informace klienta][4].</span><span class="sxs-lookup"><span data-stu-id="e422b-110">The reference documentation for the client SDK is located here: [Azure Mobile Apps .NET client reference][4].</span></span>
<span data-ttu-id="e422b-111">Můžete také vyhledat několik ukázky klienta v [úložiště GitHub Azure-Samples][5].</span><span class="sxs-lookup"><span data-stu-id="e422b-111">You can also find several client samples in the [Azure-Samples GitHub repository][5].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="e422b-112">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="e422b-112">Supported Platforms</span></span>
<span data-ttu-id="e422b-113">Platformě .NET podporuje tyto platformy:</span><span class="sxs-lookup"><span data-stu-id="e422b-113">The .NET Platform supports the following platforms:</span></span>

* <span data-ttu-id="e422b-114">Xamarin Android verze pro rozhraní API 19 až 24 (KitKat prostřednictvím cukrovinkách typu nugát)</span><span class="sxs-lookup"><span data-stu-id="e422b-114">Xamarin Android releases for API 19 through 24 (KitKat through Nougat)</span></span>
* <span data-ttu-id="e422b-115">Xamarin iOS verze pro iOS verze 8.0 a novější</span><span class="sxs-lookup"><span data-stu-id="e422b-115">Xamarin iOS releases for iOS versions 8.0 and later</span></span>
* <span data-ttu-id="e422b-116">Univerzální platforma pro Windows</span><span class="sxs-lookup"><span data-stu-id="e422b-116">Universal Windows Platform</span></span>
* <span data-ttu-id="e422b-117">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="e422b-117">Windows Phone 8.1</span></span>
* <span data-ttu-id="e422b-118">Windows Phone 8.0, s výjimkou aplikace Silverlight</span><span class="sxs-lookup"><span data-stu-id="e422b-118">Windows Phone 8.0 except for Silverlight applications</span></span>

<span data-ttu-id="e422b-119">"Serveru pohybu" ověřování používá webové zobrazení pro vidění uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e422b-119">The "server-flow" authentication uses a WebView for the presented UI.</span></span>  <span data-ttu-id="e422b-120">Pokud zařízení není možné zobrazení webové zobrazení uživatelského rozhraní, je potřeba jiných metod ověřování.</span><span class="sxs-lookup"><span data-stu-id="e422b-120">If the device is not able to present a WebView UI, then other methods of authentication are needed.</span></span>  <span data-ttu-id="e422b-121">Tato sada SDK není proto vhodný pro sledování type nebo podobně jako zařízení s omezeným přístupem.</span><span class="sxs-lookup"><span data-stu-id="e422b-121">This SDK is thus not suitable for Watch-type or similarly restricted devices.</span></span>

## <span data-ttu-id="e422b-122"><a name="setup"></a>Instalační program a požadavky</span><span class="sxs-lookup"><span data-stu-id="e422b-122"><a name="setup"></a>Setup and Prerequisites</span></span>
<span data-ttu-id="e422b-123">Předpokládáme, že jste již vytvořili a publikování projektu back-end mobilní aplikace, která zahrnuje nejméně jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="e422b-123">We assume that you have already created and published your Mobile App backend project, which includes at least one table.</span></span>  <span data-ttu-id="e422b-124">V kódu aplikace v tomto tématu, je v tabulce s názvem `TodoItem` a obsahuje následující sloupce: `Id`, `Text`, a `Complete`.</span><span class="sxs-lookup"><span data-stu-id="e422b-124">In the code used in this topic, the table is named `TodoItem` and it has the following columns: `Id`, `Text`, and `Complete`.</span></span> <span data-ttu-id="e422b-125">Tato tabulka je stejné tabulky vytvořit při dokončení [rychlý start Azure Mobile Apps][1].</span><span class="sxs-lookup"><span data-stu-id="e422b-125">This table is the same table created when you complete the [Azure Mobile Apps quickstart][1].</span></span>

<span data-ttu-id="e422b-126">Odpovídající typu Typ klienta v jazyce C# je následující třídy:</span><span class="sxs-lookup"><span data-stu-id="e422b-126">The corresponding typed client-side type in C# is the following class:</span></span>

```
public class TodoItem
{
    public string Id { get; set; }

    [JsonProperty(PropertyName = "text")]
    public string Text { get; set; }

    [JsonProperty(PropertyName = "complete")]
    public bool Complete { get; set; }
}
```

<span data-ttu-id="e422b-127">[JsonPropertyAttribute] [ 6] se používá k definování *PropertyName* mapování mezi klienta pole a pole tabulky.</span><span class="sxs-lookup"><span data-stu-id="e422b-127">The [JsonPropertyAttribute][6] is used to define the *PropertyName* mapping between the client field and the table field.</span></span>

<span data-ttu-id="e422b-128">Informace o vytváření tabulek v váš back-end mobilní aplikace, najdete v článku [tématu .NET serveru SDK] [ 7] nebo [Node.js Server SDK tématu][8].</span><span class="sxs-lookup"><span data-stu-id="e422b-128">To learn how to create tables in your Mobile Apps backend, see the [.NET Server SDK topic][7] or the [Node.js Server SDK topic][8].</span></span> <span data-ttu-id="e422b-129">Pokud jste vytvořili váš back-end mobilní aplikace v portálu Azure pomocí rychlé spuštění, můžete také použít **snadno tabulky** nastavení v [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="e422b-129">If you created your Mobile App backend in the Azure portal using the QuickStart, you can also use the **Easy tables** setting in the [Azure portal].</span></span>

### <a name="how-to-install-the-managed-client-sdk-package"></a><span data-ttu-id="e422b-130">Postupy: instalace balíčku SDK spravovaného klienta</span><span class="sxs-lookup"><span data-stu-id="e422b-130">How to: Install the managed client SDK package</span></span>
<span data-ttu-id="e422b-131">Použijte jednu z následujících metod k instalaci balíčku SDK spravovaného klienta pro mobilní aplikace od [NuGet][9]:</span><span class="sxs-lookup"><span data-stu-id="e422b-131">Use one of the following methods to install the managed client SDK package for Mobile Apps from [NuGet][9]:</span></span>

* <span data-ttu-id="e422b-132">**Visual Studio** klikněte pravým tlačítkem na projekt, klikněte na tlačítko **spravovat balíčky NuGet**, vyhledejte `Microsoft.Azure.Mobile.Client` balíček a potom klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="e422b-132">**Visual Studio** Right-click your project, click **Manage NuGet Packages**, search for the `Microsoft.Azure.Mobile.Client` package, then click **Install**.</span></span>
* <span data-ttu-id="e422b-133">**Xamarin Studio** klikněte pravým tlačítkem na projekt, klikněte na tlačítko **přidat** > **přidání balíčků NuGet**, vyhledejte `Microsoft.Azure.Mobile.Client `balíček a potom klikněte na **přidat balíček**.</span><span class="sxs-lookup"><span data-stu-id="e422b-133">**Xamarin Studio** Right-click your project, click **Add** > **Add NuGet Packages**, search for the `Microsoft.Azure.Mobile.Client `package, and then click **Add Package**.</span></span>

<span data-ttu-id="e422b-134">V souboru hlavní aktivitu, nezapomeňte přidat následující **pomocí** příkaz:</span><span class="sxs-lookup"><span data-stu-id="e422b-134">In your main activity file, remember to add the following **using** statement:</span></span>

```
using Microsoft.WindowsAzure.MobileServices;
```

### <span data-ttu-id="e422b-135"><a name="symbolsource"></a>Postupy: práce s ladicími symboly v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e422b-135"><a name="symbolsource"></a>How to: Work with debug symbols in Visual Studio</span></span>
<span data-ttu-id="e422b-136">Symboly pro obor názvů Microsoft.Azure.Mobile jsou k dispozici na [SymbolSource][10].</span><span class="sxs-lookup"><span data-stu-id="e422b-136">The symbols for the Microsoft.Azure.Mobile namespace are available on [SymbolSource][10].</span></span>  <span data-ttu-id="e422b-137">Odkazovat [SymbolSource pokyny] [ 11] integrovat SymbolSource pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e422b-137">Refer to the [SymbolSource instructions][11] to integrate SymbolSource with Visual Studio.</span></span>

## <span data-ttu-id="e422b-138"><a name="create-client"></a>Vytvoření klienta Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="e422b-138"><a name="create-client"></a>Create the Mobile Apps client</span></span>
<span data-ttu-id="e422b-139">Následující kód vytvoří [MobileServiceClient] [ 12] objekt, který se používá pro přístup k vaší back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e422b-139">The following code creates the [MobileServiceClient][12] object that is used to access your Mobile App backend.</span></span>

```
var client = new MobileServiceClient("MOBILE_APP_URL");
```

<span data-ttu-id="e422b-140">V předchozím kódu, nahraďte `MOBILE_APP_URL` s adresou URL back-end mobilní aplikace, které je najít v okně v back-endu mobilní aplikace [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="e422b-140">In the preceding code, replace `MOBILE_APP_URL` with the URL of the Mobile App backend, which is found in the blade for your Mobile App backend in the [Azure portal].</span></span> <span data-ttu-id="e422b-141">Objekt MobileServiceClient by měl být typu singleton.</span><span class="sxs-lookup"><span data-stu-id="e422b-141">The MobileServiceClient object should be a singleton.</span></span>

## <a name="work-with-tables"></a><span data-ttu-id="e422b-142">Práce s tabulkami</span><span class="sxs-lookup"><span data-stu-id="e422b-142">Work with Tables</span></span>
<span data-ttu-id="e422b-143">V následující části podrobně popisují k vyhledání a načtení záznamů a úprava dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="e422b-143">The following section details how to search and retrieve records and modify the data within the table.</span></span>  <span data-ttu-id="e422b-144">Jsou pokryta následující témata:</span><span class="sxs-lookup"><span data-stu-id="e422b-144">The following topics are covered:</span></span>

* [<span data-ttu-id="e422b-145">Vytvořit odkaz na tabulku</span><span class="sxs-lookup"><span data-stu-id="e422b-145">Create a table reference</span></span>](#instantiating)
* [<span data-ttu-id="e422b-146">Dotaz na data</span><span class="sxs-lookup"><span data-stu-id="e422b-146">Query data</span></span>](#querying)
* [<span data-ttu-id="e422b-147">Filtr vrátil data</span><span class="sxs-lookup"><span data-stu-id="e422b-147">Filter returned data</span></span>](#filtering)
* [<span data-ttu-id="e422b-148">Řazení vrátil data</span><span class="sxs-lookup"><span data-stu-id="e422b-148">Sort returned data</span></span>](#sorting)
* [<span data-ttu-id="e422b-149">Vrátit data na stránkách</span><span class="sxs-lookup"><span data-stu-id="e422b-149">Return data in pages</span></span>](#paging)
* [<span data-ttu-id="e422b-150">Vyberte konkrétní sloupce</span><span class="sxs-lookup"><span data-stu-id="e422b-150">Select specific columns</span></span>](#selecting)
* [<span data-ttu-id="e422b-151">Vyhledat záznam podle Id</span><span class="sxs-lookup"><span data-stu-id="e422b-151">Look up a record by Id</span></span>](#lookingup)
* [<span data-ttu-id="e422b-152">Práci s netypové dotazy</span><span class="sxs-lookup"><span data-stu-id="e422b-152">Dealing with untyped queries</span></span>](#untypedqueries)
* [<span data-ttu-id="e422b-153">Vkládání dat</span><span class="sxs-lookup"><span data-stu-id="e422b-153">Inserting data</span></span>](#inserting)
* [<span data-ttu-id="e422b-154">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="e422b-154">Updating data</span></span>](#updating)
* [<span data-ttu-id="e422b-155">Odstraňování dat</span><span class="sxs-lookup"><span data-stu-id="e422b-155">Deleting data</span></span>](#deleting)
* [<span data-ttu-id="e422b-156">Řešení konfliktů a optimistickou metodu souběžného zpracování</span><span class="sxs-lookup"><span data-stu-id="e422b-156">Conflict Resolution and Optimistic Concurrency</span></span>](#optimisticconcurrency)
* [<span data-ttu-id="e422b-157">Vytvoření vazby na uživatelské rozhraní Windows</span><span class="sxs-lookup"><span data-stu-id="e422b-157">Binding to a Windows User Interface</span></span>](#binding)
* [<span data-ttu-id="e422b-158">Změna velikosti stránky</span><span class="sxs-lookup"><span data-stu-id="e422b-158">Changing the Page Size</span></span>](#pagesize)

### <span data-ttu-id="e422b-159"><a name="instantiating"></a>Postupy: vytvoření odkaz na tabulku</span><span class="sxs-lookup"><span data-stu-id="e422b-159"><a name="instantiating"></a>How to: Create a table reference</span></span>
<span data-ttu-id="e422b-160">Všechny kód, který přistupuje k nebo upravuje data v tabulce back-end volání funkce na `MobileServiceTable` objektu.</span><span class="sxs-lookup"><span data-stu-id="e422b-160">All the code that accesses or modifies data in a backend table calls functions on the `MobileServiceTable` object.</span></span> <span data-ttu-id="e422b-161">Získat odkaz na tabulku voláním [Funkce GetTable] metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e422b-161">Obtain a reference to the table by calling the [GetTable] method, as follows:</span></span>

```
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

<span data-ttu-id="e422b-162">Vráceného objektu používá model typovaná serializace.</span><span class="sxs-lookup"><span data-stu-id="e422b-162">The returned object uses the typed serialization model.</span></span> <span data-ttu-id="e422b-163">Model netypové serializace je také podporována.</span><span class="sxs-lookup"><span data-stu-id="e422b-163">An untyped serialization model is also supported.</span></span> <span data-ttu-id="e422b-164">Následující příklad [vytvoří odkaz na tabulku netypové]:</span><span class="sxs-lookup"><span data-stu-id="e422b-164">The following example [creates a reference to an untyped table]:</span></span>

```
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

<span data-ttu-id="e422b-165">V netypové dotazy je nutné zadat základní řetězec dotazu OData.</span><span class="sxs-lookup"><span data-stu-id="e422b-165">In untyped queries, you must specify the underlying OData query string.</span></span>

### <span data-ttu-id="e422b-166"><a name="querying"></a>Postupy: dotaz na data z mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="e422b-166"><a name="querying"></a>How to: Query data from your Mobile App</span></span>
<span data-ttu-id="e422b-167">Tato část popisuje, jak vydávat dotazy back-end mobilní aplikace, která zahrnuje následující funkce:</span><span class="sxs-lookup"><span data-stu-id="e422b-167">This section describes how to issue queries to the Mobile App backend, which includes the following functionality:</span></span>

* [<span data-ttu-id="e422b-168">Filtr vrátil data</span><span class="sxs-lookup"><span data-stu-id="e422b-168">Filter returned data</span></span>](#filtering)
* [<span data-ttu-id="e422b-169">Řazení vrátil data</span><span class="sxs-lookup"><span data-stu-id="e422b-169">Sort returned data</span></span>](#sorting)
* [<span data-ttu-id="e422b-170">Vrátit data na stránkách</span><span class="sxs-lookup"><span data-stu-id="e422b-170">Return data in pages</span></span>](#paging)
* [<span data-ttu-id="e422b-171">Vyberte konkrétní sloupce</span><span class="sxs-lookup"><span data-stu-id="e422b-171">Select specific columns</span></span>](#selecting)
* [<span data-ttu-id="e422b-172">Vyhledávání dat podle ID</span><span class="sxs-lookup"><span data-stu-id="e422b-172">Look up data by ID</span></span>](#lookingup)

> [!NOTE]
> <span data-ttu-id="e422b-173">Velikost stránky řízené serveru se vynucuje zabránit nevrátila všechny řádky.</span><span class="sxs-lookup"><span data-stu-id="e422b-173">A server-driven page size is enforced to prevent all rows from being returned.</span></span>  <span data-ttu-id="e422b-174">Stránkování zachová výchozí požadavky pro velké sady dat z mělo negativní dopad na službu.</span><span class="sxs-lookup"><span data-stu-id="e422b-174">Paging keeps default requests for large data sets from negatively impacting the service.</span></span>  <span data-ttu-id="e422b-175">Chcete-li vrátit více než 50 řádky, použijte `Skip` a `Take` metoda, jak je popsáno v [vracet data na stránkách](#paging).</span><span class="sxs-lookup"><span data-stu-id="e422b-175">To return more than 50 rows, use the `Skip` and `Take` method, as described in [Return data in pages](#paging).</span></span>

### <span data-ttu-id="e422b-176"><a name="filtering"></a>Postupy: Filtr nevrátil data</span><span class="sxs-lookup"><span data-stu-id="e422b-176"><a name="filtering"></a>How to: Filter returned data</span></span>
<span data-ttu-id="e422b-177">Následující kód ukazuje, jak k filtrování dat včetně `Where` klauzuli v dotazu.</span><span class="sxs-lookup"><span data-stu-id="e422b-177">The following code illustrates how to filter data by including a `Where` clause in a query.</span></span> <span data-ttu-id="e422b-178">Vrátí všechny položky z `todoTable` jejichž `Complete` vlastnost rovná `false`.</span><span class="sxs-lookup"><span data-stu-id="e422b-178">It returns all items from `todoTable` whose `Complete` property is equal to `false`.</span></span> <span data-ttu-id="e422b-179">[Kde] funkce se vztahuje řádek filtrování predikátu k dotazu vůči v tabulce.</span><span class="sxs-lookup"><span data-stu-id="e422b-179">The [Where] function applies a row filtering predicate to the query against the table.</span></span>

```
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

<span data-ttu-id="e422b-180">Můžete zobrazit identifikátor URI požadavek odeslaný back-end pomocí softwaru zpráva kontroly, jako jsou nástroje pro vývojáře prohlížeče nebo [Fiddler].</span><span class="sxs-lookup"><span data-stu-id="e422b-180">You can view the URI of the request sent to the backend by using message inspection software, such as browser developer tools or [Fiddler].</span></span> <span data-ttu-id="e422b-181">Pokud se podíváte na identifikátoru URI požadavku, Všimněte si, že je řetězec dotazu upravit:</span><span class="sxs-lookup"><span data-stu-id="e422b-181">If you look at the request URI, notice that the query string is modified:</span></span>

```
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

<span data-ttu-id="e422b-182">Tento požadavek OData je přeložen do dotazu SQL Server SDK:</span><span class="sxs-lookup"><span data-stu-id="e422b-182">This OData request is translated into an SQL query by the Server SDK:</span></span>

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

<span data-ttu-id="e422b-183">Funkce, která je předána `Where` metoda může mít libovolný počet podmínky.</span><span class="sxs-lookup"><span data-stu-id="e422b-183">The function that is passed to the `Where` method can have an arbitrary number of conditions.</span></span>

```
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

<span data-ttu-id="e422b-184">V tomto příkladu by byl přeložen do dotazu SQL serveru SDK:</span><span class="sxs-lookup"><span data-stu-id="e422b-184">This example would be translated into an SQL query by the Server SDK:</span></span>

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

<span data-ttu-id="e422b-185">Tento dotaz taky dají rozdělit do více klauzulí:</span><span class="sxs-lookup"><span data-stu-id="e422b-185">This query can also be split into multiple clauses:</span></span>

```
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

<span data-ttu-id="e422b-186">Tyto dvě metody jsou ekvivalentní a mohou být použity zcela zaměnitelným významem.</span><span class="sxs-lookup"><span data-stu-id="e422b-186">The two methods are equivalent and may be used interchangeably.</span></span>  <span data-ttu-id="e422b-187">Možnost bývalé&mdash;z zřetězení více predikáty v jednom dotazu&mdash;je kompaktnější a doporučené.</span><span class="sxs-lookup"><span data-stu-id="e422b-187">The former option&mdash;of concatenating multiple predicates in one query&mdash;is more compact and recommended.</span></span>

<span data-ttu-id="e422b-188">`Where` Klauzule podporuje operace, které se přeložit na podmnožinu protokolu OData.</span><span class="sxs-lookup"><span data-stu-id="e422b-188">The `Where` clause supports operations that be translated into the OData subset.</span></span> <span data-ttu-id="e422b-189">Operace zahrnují:</span><span class="sxs-lookup"><span data-stu-id="e422b-189">Operations include:</span></span>

* <span data-ttu-id="e422b-190">Relační operátory (==,! =, <, < =, >, > =),</span><span class="sxs-lookup"><span data-stu-id="e422b-190">Relational operators (==, !=, <, <=, >, >=),</span></span>
* <span data-ttu-id="e422b-191">Aritmetické operátory (+, -, /, *, %),</span><span class="sxs-lookup"><span data-stu-id="e422b-191">Arithmetic operators (+, -, /, *, %),</span></span>
* <span data-ttu-id="e422b-192">Číslo přesnost (Math.Floor, Math.Ceiling)</span><span class="sxs-lookup"><span data-stu-id="e422b-192">Number precision (Math.Floor, Math.Ceiling),</span></span>
* <span data-ttu-id="e422b-193">Funkce řetězce (délka, Substring, nahraďte, IndexOf, StartsWith, EndsWith),</span><span class="sxs-lookup"><span data-stu-id="e422b-193">String functions (Length, Substring, Replace, IndexOf, StartsWith, EndsWith),</span></span>
* <span data-ttu-id="e422b-194">Vlastnosti kalendářních dat (rok, měsíc, den, hodinu, minutu, sekundu),</span><span class="sxs-lookup"><span data-stu-id="e422b-194">Date properties (Year, Month, Day, Hour, Minute, Second),</span></span>
* <span data-ttu-id="e422b-195">Přístup k vlastnosti objektu, a</span><span class="sxs-lookup"><span data-stu-id="e422b-195">Access properties of an object, and</span></span>
* <span data-ttu-id="e422b-196">Výrazy kombinování kteroukoli z těchto operací.</span><span class="sxs-lookup"><span data-stu-id="e422b-196">Expressions combining any of these operations.</span></span>

<span data-ttu-id="e422b-197">Při určování, co Server SDK podporuje, můžete zvážit [OData v3 dokumentaci].</span><span class="sxs-lookup"><span data-stu-id="e422b-197">When considering what the Server SDK supports, you can consider the [OData v3 Documentation].</span></span>

### <span data-ttu-id="e422b-198"><a name="sorting"></a>Postupy: řazení vrátil data</span><span class="sxs-lookup"><span data-stu-id="e422b-198"><a name="sorting"></a>How to: Sort returned data</span></span>
<span data-ttu-id="e422b-199">Následující kód ukazuje, jak řadit data zahrnutím [OrderBy] nebo [OrderByDescending] funkce v dotazu.</span><span class="sxs-lookup"><span data-stu-id="e422b-199">The following code illustrates how to sort data by including an [OrderBy] or [OrderByDescending] function in the query.</span></span> <span data-ttu-id="e422b-200">Vrátí položky z `todoTable` seřadit vzestupně podle `Text` pole.</span><span class="sxs-lookup"><span data-stu-id="e422b-200">It returns items from `todoTable` sorted ascending by the `Text` field.</span></span>

```
// Sort items in ascending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderBy(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();

// Sort items in descending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderByDescending(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();
```

### <span data-ttu-id="e422b-201"><a name="paging"></a>Postupy: vracet data na stránkách</span><span class="sxs-lookup"><span data-stu-id="e422b-201"><a name="paging"></a>How to: Return data in pages</span></span>
<span data-ttu-id="e422b-202">Ve výchozím nastavení vrátí back-end pouze prvních 50 řádků.</span><span class="sxs-lookup"><span data-stu-id="e422b-202">By default, the backend returns only the first 50 rows.</span></span> <span data-ttu-id="e422b-203">Můžete zvýšit počet vrácených řádků voláním [trvat] metoda.</span><span class="sxs-lookup"><span data-stu-id="e422b-203">You can increase the number of returned rows by calling the [Take] method.</span></span> <span data-ttu-id="e422b-204">Použití `Take` spolu s [přeskočit] metoda požádat o konkrétní "stránka" celkový sady dat vrácených dotazem.</span><span class="sxs-lookup"><span data-stu-id="e422b-204">Use `Take` along with the [Skip] method to request a specific "page" of the total dataset returned by the query.</span></span> <span data-ttu-id="e422b-205">Následující dotaz, při spuštění, vrátí první tři položky v tabulce.</span><span class="sxs-lookup"><span data-stu-id="e422b-205">The following query, when executed, returns the top three items in the table.</span></span>

```
// Define a filtered query that returns the top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

<span data-ttu-id="e422b-206">Následující dotaz revidovaný přeskočí první tři výsledky a vrátí následující tři výsledky.</span><span class="sxs-lookup"><span data-stu-id="e422b-206">The following revised query skips the first three results and returns the next three results.</span></span> <span data-ttu-id="e422b-207">Tento dotaz vytvoří druhý "stránku" dat, kde je velikost stránky tři položky.</span><span class="sxs-lookup"><span data-stu-id="e422b-207">This query produces the second "page" of data, where the page size is three items.</span></span>

```
// Define a filtered query that skips the top 3 items and returns the next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

<span data-ttu-id="e422b-208">[IncludeTotalCount] metoda požadavky celkový počet pro *všechny* záznamy, které by byly vráceny, ignoruje všechny klauzuli stránkování/limit zadat:</span><span class="sxs-lookup"><span data-stu-id="e422b-208">The [IncludeTotalCount] method requests the total count for *all* the records that would have been returned, ignoring any paging/limit clause specified:</span></span>

```
query = query.IncludeTotalCount();
```

<span data-ttu-id="e422b-209">Ve skutečných aplikaci můžete použít dotazy, podobně jako v předchozím příkladu s na pager ovládací prvek nebo srovnatelný uživatelského rozhraní navigace mezi stránkami.</span><span class="sxs-lookup"><span data-stu-id="e422b-209">In a real world app, you can use queries similar to the preceding example with a pager control or comparable UI to navigate between pages.</span></span>

> [!NOTE]
> <span data-ttu-id="e422b-210">Chcete-li přepsat limit 50 řádek v back-end mobilní aplikace, musíte také použít [EnableQueryAttribute] veřejnosti GET – metoda a určují chování stránkování.</span><span class="sxs-lookup"><span data-stu-id="e422b-210">To override the 50-row limit in a Mobile App backend, you must also apply the [EnableQueryAttribute] to the public GET method and specify the paging behavior.</span></span> <span data-ttu-id="e422b-211">Při použití metody následující Nastaví maximální vráceny řádky do 1000:</span><span class="sxs-lookup"><span data-stu-id="e422b-211">When applied to the method, the following sets the maximum returned rows to 1000:</span></span>
>
> `[EnableQuery(MaxTop=1000)]`


### <span data-ttu-id="e422b-212"><a name="selecting"></a>Postupy: výběr konkrétní sloupců</span><span class="sxs-lookup"><span data-stu-id="e422b-212"><a name="selecting"></a>How to: Select specific columns</span></span>
<span data-ttu-id="e422b-213">Můžete zadat, která sada vlastností pro zahrnutí do výsledků přidáním [vyberte] klauzule do dotazu.</span><span class="sxs-lookup"><span data-stu-id="e422b-213">You can specify which set of properties to include in the results by adding a [Select] clause to your query.</span></span> <span data-ttu-id="e422b-214">Například následující kód ukazuje, jak vybrat pouze jedno pole a také jak vybrat a formátování více polí:</span><span class="sxs-lookup"><span data-stu-id="e422b-214">For example, the following code shows how to select just one field and also how to select and format multiple fields:</span></span>

```
// Select one field -- just the Text
MobileServiceTableQuery<TodoItem> query = todoTable
                .Select(todoItem => todoItem.Text);
List<string> items = await query.ToListAsync();

// Select multiple fields -- both Complete and Text info
MobileServiceTableQuery<TodoItem> query = todoTable
                .Select(todoItem => string.Format("{0} -- {1}",
                    todoItem.Text.PadRight(30), todoItem.Complete ?
                    "Now complete!" : "Incomplete!"));
List<string> items = await query.ToListAsync();
```

<span data-ttu-id="e422b-215">Všechny funkce popsané, pokud jsou aditivní, takže jsme můžete zachovat řetězení je.</span><span class="sxs-lookup"><span data-stu-id="e422b-215">All the functions described so far are additive, so we can keep chaining them.</span></span> <span data-ttu-id="e422b-216">Každý zřetězené volání ovlivňuje více dotazu.</span><span class="sxs-lookup"><span data-stu-id="e422b-216">Each chained call affects more of the query.</span></span> <span data-ttu-id="e422b-217">Další příklad:</span><span class="sxs-lookup"><span data-stu-id="e422b-217">One more example:</span></span>

```
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <span data-ttu-id="e422b-218"><a name="lookingup"></a>Postupy: vyhledávání dat podle ID</span><span class="sxs-lookup"><span data-stu-id="e422b-218"><a name="lookingup"></a>How to: Look up data by ID</span></span>
<span data-ttu-id="e422b-219">[LookupAsync] funkce lze použít k vyhledání objekty z databáze s konkrétním ID.</span><span class="sxs-lookup"><span data-stu-id="e422b-219">The [LookupAsync] function can be used to look up objects from the database with a particular ID.</span></span>

```
// This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <span data-ttu-id="e422b-220"><a name="untypedqueries"></a>Postupy: provádění dotazů bez typu</span><span class="sxs-lookup"><span data-stu-id="e422b-220"><a name="untypedqueries"></a>How to: Execute untyped queries</span></span>
<span data-ttu-id="e422b-221">Při provádění dotazu pomocí objektu bez typu tabulky, je nutné explicitně zadat řetězec dotazu OData voláním [ReadAsync], jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="e422b-221">When executing a query using an untyped table object, you must explicitly specify the OData query string by calling [ReadAsync], as in the following example:</span></span>

```
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

<span data-ttu-id="e422b-222">Zobrazí zpět JSON hodnoty, které lze používat jako kontejner objektů.</span><span class="sxs-lookup"><span data-stu-id="e422b-222">You get back JSON values that you can use like a property bag.</span></span> <span data-ttu-id="e422b-223">Další informace o JToken a Newtonsoft Json.NET, najdete v článku [Json.NET] lokality.</span><span class="sxs-lookup"><span data-stu-id="e422b-223">For more information on JToken and Newtonsoft Json.NET, see the [Json.NET] site.</span></span>

### <span data-ttu-id="e422b-224"><a name="inserting"></a>Postupy: vložení dat do back-end mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="e422b-224"><a name="inserting"></a>How to: Insert data into a Mobile App backend</span></span>
<span data-ttu-id="e422b-225">Všechny typy klientů musí obsahovat člen s názvem **Id**, což je ve výchozím nastavení řetězec.</span><span class="sxs-lookup"><span data-stu-id="e422b-225">All client types must contain a member named **Id**, which is by default a string.</span></span> <span data-ttu-id="e422b-226">To **Id** je potřebná k provedení operace CRUD a pro offline synchronizace.</span><span class="sxs-lookup"><span data-stu-id="e422b-226">This **Id** is required to perform CRUD operations and for offline sync.</span></span> <span data-ttu-id="e422b-227">Následující kód ukazuje, jak používat [InsertAsync] metodu pro vkládání nových řádků do tabulky.</span><span class="sxs-lookup"><span data-stu-id="e422b-227">The following code illustrates how to use the [InsertAsync] method to insert new rows into a table.</span></span> <span data-ttu-id="e422b-228">Parametr obsahuje data, která mají být vloženy jako objekt .NET.</span><span class="sxs-lookup"><span data-stu-id="e422b-228">The parameter contains the data to be inserted as a .NET object.</span></span>

```
await todoTable.InsertAsync(todoItem);
```

<span data-ttu-id="e422b-229">Pokud není součástí jedinečný vlastní hodnota ID `todoItem` při vložení, je identifikátor GUID generovaný serverem.</span><span class="sxs-lookup"><span data-stu-id="e422b-229">If a unique custom ID value is not included in the `todoItem` during an insert, a GUID is generated by the server.</span></span>
<span data-ttu-id="e422b-230">Vygenerovaný Id můžete načíst zkontrolováním objekt po volání vrátí.</span><span class="sxs-lookup"><span data-stu-id="e422b-230">You can retrieve the generated Id by inspecting the object after the call returns.</span></span>

<span data-ttu-id="e422b-231">Pokud chcete vložit bez typu dat, může využít výhod Json.NET:</span><span class="sxs-lookup"><span data-stu-id="e422b-231">To insert untyped data, you may take advantage of Json.NET:</span></span>

```
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

<span data-ttu-id="e422b-232">Tady je příklad pomocí e-mailovou adresu jako id jedinečného řetězce:</span><span class="sxs-lookup"><span data-stu-id="e422b-232">Here is an example using an email address as a unique string id:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a><span data-ttu-id="e422b-233">Práce s ID hodnoty</span><span class="sxs-lookup"><span data-stu-id="e422b-233">Working with ID values</span></span>
<span data-ttu-id="e422b-234">Mobilní aplikace podporuje vlastní řetězec jedinečné hodnoty pro tuto tabulku **id** sloupce.</span><span class="sxs-lookup"><span data-stu-id="e422b-234">Mobile Apps supports unique custom string values for the table's **id** column.</span></span> <span data-ttu-id="e422b-235">Hodnotu řetězce umožňuje aplikacím používat vlastní hodnoty, jako je například e-mailové adresy nebo uživatelské jméno pro ID.</span><span class="sxs-lookup"><span data-stu-id="e422b-235">A string value allows applications to use custom values such as email addresses or user names for the ID.</span></span>  <span data-ttu-id="e422b-236">Řetězec ID poskytují následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e422b-236">String IDs provide you with the following benefits:</span></span>

* <span data-ttu-id="e422b-237">ID jsou generovány bez provedení výměnu zpráv do databáze.</span><span class="sxs-lookup"><span data-stu-id="e422b-237">IDs are generated without making a round trip to the database.</span></span>
* <span data-ttu-id="e422b-238">Záznamy jsou usnadňují sloučení z různých tabulek nebo databází.</span><span class="sxs-lookup"><span data-stu-id="e422b-238">Records are easier to merge from different tables or databases.</span></span>
* <span data-ttu-id="e422b-239">Hodnoty ID umožňuje lepší integraci s logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="e422b-239">IDs values can integrate better with an application's logic.</span></span>

<span data-ttu-id="e422b-240">Pokud řetězcová hodnota ID není nastavená na vloženého záznamu, back-end mobilní aplikace generuje jedinečnou hodnotu pro ID.</span><span class="sxs-lookup"><span data-stu-id="e422b-240">When a string ID value is not set on an inserted record, the Mobile App backend generates a unique value for the ID.</span></span> <span data-ttu-id="e422b-241">Můžete použít [Guid.NewGuid] metoda ke generování vlastních ID hodnot na straně klienta nebo v back-end.</span><span class="sxs-lookup"><span data-stu-id="e422b-241">You can use the [Guid.NewGuid] method to generate your own ID values, either on the client or in the backend.</span></span>

```
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <span data-ttu-id="e422b-242"><a name="modifying"></a>Postupy: Změna dat v back-end mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="e422b-242"><a name="modifying"></a>How to: Modify data in a Mobile App backend</span></span>
<span data-ttu-id="e422b-243">Následující kód ukazuje, jak používat [metod UpdateAsync] metoda aktualizovat existující záznam se stejným ID se novými informacemi.</span><span class="sxs-lookup"><span data-stu-id="e422b-243">The following code illustrates how to use the [UpdateAsync] method to update an existing record with the same ID with new information.</span></span> <span data-ttu-id="e422b-244">Parametr obsahuje data, která mají být aktualizována jako objekt .NET.</span><span class="sxs-lookup"><span data-stu-id="e422b-244">The parameter contains the data to be updated as a .NET object.</span></span>

```
await todoTable.UpdateAsync(todoItem);
```

<span data-ttu-id="e422b-245">Pokud chcete aktualizovat bez typu dat, může využít výhod [Json.NET] následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e422b-245">To update untyped data, you may take advantage of [Json.NET] as follows:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

<span data-ttu-id="e422b-246">`id` Je nutné zadat při aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="e422b-246">An `id` field must be specified when making an update.</span></span> <span data-ttu-id="e422b-247">Back-end používá `id` pole k identifikaci řádek, který má aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="e422b-247">The backend uses the `id` field to identify which row to update.</span></span> <span data-ttu-id="e422b-248">`id` Pole lze získat z výsledku `InsertAsync` volání.</span><span class="sxs-lookup"><span data-stu-id="e422b-248">The `id` field can be obtained from the result of the `InsertAsync` call.</span></span> <span data-ttu-id="e422b-249">`ArgumentException` Se vyvolá, pokud se pokusíte aktualizovat položku bez zadání `id` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e422b-249">An `ArgumentException` is raised if you try to update an item without providing the `id` value.</span></span>

### <span data-ttu-id="e422b-250"><a name="deleting"></a>Postupy: odstranění dat v back-end mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="e422b-250"><a name="deleting"></a>How to: Delete data in a Mobile App backend</span></span>
<span data-ttu-id="e422b-251">Následující kód ukazuje, jak používat [DeleteAsync] metoda odstranit existující instanci.</span><span class="sxs-lookup"><span data-stu-id="e422b-251">The following code illustrates how to use the [DeleteAsync] method to delete an existing instance.</span></span> <span data-ttu-id="e422b-252">Instance je identifikována `id` na pole sady `todoItem`.</span><span class="sxs-lookup"><span data-stu-id="e422b-252">The instance is identified by the `id` field set on the `todoItem`.</span></span>

```
await todoTable.DeleteAsync(todoItem);
```

<span data-ttu-id="e422b-253">Pokud chcete odstranit bez typu dat, může využít výhod Json.NET následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e422b-253">To delete untyped data, you may take advantage of Json.NET as follows:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

<span data-ttu-id="e422b-254">Pokud zadáte požadavek delete, musí být zadaná ID.</span><span class="sxs-lookup"><span data-stu-id="e422b-254">When you make a delete request, an ID must be specified.</span></span> <span data-ttu-id="e422b-255">Ostatní vlastnosti nejsou předaný službu nebo se ignorují na službu.</span><span class="sxs-lookup"><span data-stu-id="e422b-255">Other properties are not passed to the service or are ignored at the service.</span></span> <span data-ttu-id="e422b-256">Výsledek `DeleteAsync` volání je obvykle `null`.</span><span class="sxs-lookup"><span data-stu-id="e422b-256">The result of a `DeleteAsync` call is usually `null`.</span></span> <span data-ttu-id="e422b-257">Nelze získat ID předávat od výsledku `InsertAsync` volání.</span><span class="sxs-lookup"><span data-stu-id="e422b-257">The ID to pass in can be obtained from the result of the `InsertAsync` call.</span></span> <span data-ttu-id="e422b-258">A `MobileServiceInvalidOperationException` se vyvolá, když se pokusíte odstranit položku bez zadání `id` pole.</span><span class="sxs-lookup"><span data-stu-id="e422b-258">A `MobileServiceInvalidOperationException` is thrown when you try to delete an item without specifying the `id` field.</span></span>

### <span data-ttu-id="e422b-259"><a name="optimisticconcurrency"></a>Postupy: použití optimistickou metodu souběžného pro řešení konfliktů.</span><span class="sxs-lookup"><span data-stu-id="e422b-259"><a name="optimisticconcurrency"></a>How to: Use Optimistic Concurrency for conflict resolution</span></span>
<span data-ttu-id="e422b-260">Dvě nebo více klientů může zápisu změn do jedné položce ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="e422b-260">Two or more clients may write changes to the same item at the same time.</span></span> <span data-ttu-id="e422b-261">Bez zjišťování konfliktů by poslední zápis přepsat všechny předchozí aktualizace.</span><span class="sxs-lookup"><span data-stu-id="e422b-261">Without conflict detection, the last write would overwrite any previous updates.</span></span> <span data-ttu-id="e422b-262">**Optimistické řízení souběžného** předpokládá, že každou transakci můžete potvrdit a proto nepoužívá žádné uzamčení prostředků.</span><span class="sxs-lookup"><span data-stu-id="e422b-262">**Optimistic concurrency control** assumes that each transaction can commit and therefore does not use any resource locking.</span></span>  <span data-ttu-id="e422b-263">Před potvrzením transakce, optimistické řízení souběžného ověřuje, že neexistují žádné další transakce změnil data.</span><span class="sxs-lookup"><span data-stu-id="e422b-263">Before committing a transaction, optimistic concurrency control verifies that no other transaction has modified the data.</span></span> <span data-ttu-id="e422b-264">Pokud data byla změněna, spouštění transakce je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="e422b-264">If the data has been modified, the committing transaction is rolled back.</span></span>

<span data-ttu-id="e422b-265">Mobilní aplikace podporuje optimistické řízení souběžného pomocí funkce sledování změn pro každou položku pomocí `version` sloupec vlastností systému, která je definována pro každou tabulku v váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e422b-265">Mobile Apps supports optimistic concurrency control by tracking changes to each item using the `version` system property column that is defined for each table in your Mobile App backend.</span></span> <span data-ttu-id="e422b-266">Pokaždé, když je aktualizovat záznam, nastaví Mobile Apps `version` vlastnost pro tento záznam na novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e422b-266">Each time a record is updated, Mobile Apps sets the `version` property for that record to a new value.</span></span> <span data-ttu-id="e422b-267">Při každém požadavku aktualizace `version` vlastnost záznamu součástí požadavku se porovnává se stejnou vlastnost pro záznam na serveru.</span><span class="sxs-lookup"><span data-stu-id="e422b-267">During each update request, the `version` property of the record included with the request is compared to the same property for the record on the server.</span></span> <span data-ttu-id="e422b-268">Pokud verze byla dokončena s požadavku se neshoduje s back-end, pak klientské knihovny vyvolá `MobileServicePreconditionFailedException<T>` výjimka.</span><span class="sxs-lookup"><span data-stu-id="e422b-268">If the version passed with the request does not match the backend, then the client library raises a `MobileServicePreconditionFailedException<T>` exception.</span></span> <span data-ttu-id="e422b-269">Typ zahrnuty s výjimkou je záznam z back-end obsahující servery verzi záznamu.</span><span class="sxs-lookup"><span data-stu-id="e422b-269">The type included with the exception is the record from the backend containing the servers version of the record.</span></span> <span data-ttu-id="e422b-270">Aplikace pak může tyto informace slouží k rozhodnout, zda chcete provést aktualizaci požadavků zopakovat se správnou `version` hodnotu z back-end potvrzení změn.</span><span class="sxs-lookup"><span data-stu-id="e422b-270">The application can then use this information to decide whether to execute the update request again with the correct `version` value from the backend to commit changes.</span></span>

<span data-ttu-id="e422b-271">U třídy tabulky pro definovat sloupec `version` systému vlastnost umožňující optimistickou metodu souběžného.</span><span class="sxs-lookup"><span data-stu-id="e422b-271">Define a column on the table class for the `version` system property to enable optimistic concurrency.</span></span> <span data-ttu-id="e422b-272">Například:</span><span class="sxs-lookup"><span data-stu-id="e422b-272">For example:</span></span>

```
public class TodoItem
{
    public string Id { get; set; }

    [JsonProperty(PropertyName = "text")]
    public string Text { get; set; }

    [JsonProperty(PropertyName = "complete")]
    public bool Complete { get; set; }

    // *** Enable Optimistic Concurrency *** //
    [JsonProperty(PropertyName = "version")]
    public string Version { set; get; }
}
```

<span data-ttu-id="e422b-273">Aplikace, které používají bez typu tabulky povolit optimistickou metodu souběžného nastavením `Version` příznak na `SystemProperties` tabulky následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e422b-273">Applications using untyped tables enable optimistic concurrency by setting the `Version` flag on the `SystemProperties` of the table as follows.</span></span>

```
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

<span data-ttu-id="e422b-274">Kromě povolení optimistickou metodu souběžného, musí také catch `MobileServicePreconditionFailedException<T>` výjimka ve vašem kódu při volání metody [metod UpdateAsync].</span><span class="sxs-lookup"><span data-stu-id="e422b-274">In addition to enabling optimistic concurrency, you must also catch the `MobileServicePreconditionFailedException<T>` exception in your code when calling [UpdateAsync].</span></span>  <span data-ttu-id="e422b-275">Vyřešte konflikt s použitím správné `version` aktualizovaný záznam a volání [metod UpdateAsync] k vyřešení záznamu.</span><span class="sxs-lookup"><span data-stu-id="e422b-275">Resolve the conflict by applying the correct `version` to the updated record and call [UpdateAsync] with the resolved record.</span></span> <span data-ttu-id="e422b-276">Následující kód ukazuje, jak vyřešit jednou zjištěn konflikt zápisu:</span><span class="sxs-lookup"><span data-stu-id="e422b-276">The following code shows how to resolve a write conflict once detected:</span></span>

```
private async void UpdateToDoItem(TodoItem item)
{
    MobileServicePreconditionFailedException<TodoItem> exception = null;

    try
    {
        //update at the remote table
        await todoTable.UpdateAsync(item);
    }
    catch (MobileServicePreconditionFailedException<TodoItem> writeException)
    {
        exception = writeException;
    }

    if (exception != null)
    {
        // Conflict detected, the item has changed since the last query
        // Resolve the conflict between the local and server item
        await ResolveConflict(item, exception.Item);
    }
}


private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
{
    //Ask user to choose the resoltion between versions
    MessageDialog msgDialog = new MessageDialog(
        String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
        serverItem.Text, localItem.Text),
        "CONFLICT DETECTED - Select a resolution:");

    UICommand localBtn = new UICommand("Commit Local Text");
    UICommand ServerBtn = new UICommand("Leave Server Text");
    msgDialog.Commands.Add(localBtn);
    msgDialog.Commands.Add(ServerBtn);

    localBtn.Invoked = async (IUICommand command) =>
    {
        // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
        // catching a MobileServicePreConditionFailedException.
        localItem.Version = serverItem.Version;

        // Updating recursively here just in case another change happened while the user was making a decision
        UpdateToDoItem(localItem);
    };

    ServerBtn.Invoked = async (IUICommand command) =>
    {
        RefreshTodoItems();
    };

    await msgDialog.ShowAsync();
}
```

<span data-ttu-id="e422b-277">Další informace najdete v tématu [Offline synchronizací dat v Azure Mobile Apps] tématu.</span><span class="sxs-lookup"><span data-stu-id="e422b-277">For more information, see the [Offline Data Sync in Azure Mobile Apps] topic.</span></span>

### <span data-ttu-id="e422b-278"><a name="binding"></a>Postupy: vázání Mobile Apps dat do uživatelského rozhraní systému Windows</span><span class="sxs-lookup"><span data-stu-id="e422b-278"><a name="binding"></a>How to: Bind Mobile Apps data to a Windows user interface</span></span>
<span data-ttu-id="e422b-279">Tato část ukazuje způsob zobrazení vrácená data objektů s použitím prvky uživatelského rozhraní v aplikaci pro Windows.</span><span class="sxs-lookup"><span data-stu-id="e422b-279">This section shows how to display returned data objects using UI elements in a Windows app.</span></span>  <span data-ttu-id="e422b-280">Následující příklad kódu vytvoří vazbu na zdroj seznamu pomocí dotazu pro neúplné položky.</span><span class="sxs-lookup"><span data-stu-id="e422b-280">The following example code binds to the source of the list with a query for incomplete items.</span></span> <span data-ttu-id="e422b-281">[MobileServiceCollection] vytvoří kolekci mobilní aplikace využívající technologii vazby.</span><span class="sxs-lookup"><span data-stu-id="e422b-281">The [MobileServiceCollection] creates a Mobile Apps-aware binding collection.</span></span>

```
// This query filters out completed TodoItems.
MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToCollectionAsync();

// itemsControl is an IEnumerable that could be bound to a UI list control
IEnumerable itemsControl  = items;

// Bind this to a ListBox
ListBox lb = new ListBox();
lb.ItemsSource = items;
```

<span data-ttu-id="e422b-282">Některé ovládací prvky v spravovaného modulu runtime podporují rozhraní nazvané [ISupportIncrementalLoading].</span><span class="sxs-lookup"><span data-stu-id="e422b-282">Some controls in the managed runtime support an interface called [ISupportIncrementalLoading].</span></span> <span data-ttu-id="e422b-283">Toto rozhraní umožňuje ovládací prvky, co uživatel posune žádost o další data.</span><span class="sxs-lookup"><span data-stu-id="e422b-283">This interface allows controls to request extra data when the user scrolls.</span></span> <span data-ttu-id="e422b-284">Je integrovanou podporu pro toto rozhraní pro univerzální aplikace pro Windows prostřednictvím [MobileServiceIncrementalLoadingCollection], který automaticky zpracovává volání z ovládacích prvků.</span><span class="sxs-lookup"><span data-stu-id="e422b-284">There is built-in support for this interface for universal Windows apps via [MobileServiceIncrementalLoadingCollection], which automatically handles the calls from the controls.</span></span> <span data-ttu-id="e422b-285">Použití `MobileServiceIncrementalLoadingCollection` v aplikacích Windows následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e422b-285">Use `MobileServiceIncrementalLoadingCollection` in Windows apps as follows:</span></span>

```
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

<span data-ttu-id="e422b-286">Pokud chcete použít nová kolekce v aplikacích Windows Phone 8 a "Silverlight", použijte `ToCollection` rozšiřující metody na `IMobileServiceTableQuery<T>` a `IMobileServiceTable<T>`.</span><span class="sxs-lookup"><span data-stu-id="e422b-286">To use the new collection on Windows Phone 8 and "Silverlight" apps, use the `ToCollection` extension methods on `IMobileServiceTableQuery<T>` and `IMobileServiceTable<T>`.</span></span> <span data-ttu-id="e422b-287">Chcete-li načíst data, volejte `LoadMoreItemsAsync()`.</span><span class="sxs-lookup"><span data-stu-id="e422b-287">To load data, call `LoadMoreItemsAsync()`.</span></span>

```
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

<span data-ttu-id="e422b-288">Při použití kolekce vytvořená voláním `ToCollectionAsync` nebo `ToCollection`, získání kolekce, která mohou být vázány na ovládacích prvků uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e422b-288">When you use the collection created by calling `ToCollectionAsync` or `ToCollection`, you get a collection that can be bound to UI controls.</span></span>  <span data-ttu-id="e422b-289">Tato kolekce je deklaracemi stránkování.</span><span class="sxs-lookup"><span data-stu-id="e422b-289">This collection is paging-aware.</span></span>  <span data-ttu-id="e422b-290">Protože kolekce je načítání dat ze sítě, se někdy načítání nezdaří.</span><span class="sxs-lookup"><span data-stu-id="e422b-290">Since the collection is loading data from the network, loading sometimes fails.</span></span> <span data-ttu-id="e422b-291">Zpracovat takové chyby, přepsat `OnException` metodu `MobileServiceIncrementalLoadingCollection` zpracování výjimek, které jsou výsledkem volání `LoadMoreItemsAsync`.</span><span class="sxs-lookup"><span data-stu-id="e422b-291">To handle such failures, override the `OnException` method on `MobileServiceIncrementalLoadingCollection` to handle exceptions resulting from calls to `LoadMoreItemsAsync`.</span></span>

<span data-ttu-id="e422b-292">Zvažte, pokud tabulka obsahuje mnoho polí, ale chcete zobrazit některá z nich v vlastního ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="e422b-292">Consider if your table has many fields but you only want to display some of them in your control.</span></span> <span data-ttu-id="e422b-293">Můžete použít pokyny v předchozí části "[vyberte konkrétní sloupce](#selecting)" a vyberte konkrétní sloupce se mají zobrazit v uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e422b-293">You may use the guidance in the preceding section "[Select specific columns](#selecting)" to select specific columns to display in the UI.</span></span>

### <span data-ttu-id="e422b-294"><a name="pagesize"></a>Změnit velikost stránky</span><span class="sxs-lookup"><span data-stu-id="e422b-294"><a name="pagesize"></a>Change the Page size</span></span>
<span data-ttu-id="e422b-295">Azure Mobile Apps vrací maximálně 50 položek na požadavek ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e422b-295">Azure Mobile Apps returns a maximum of 50 items per request by default.</span></span>  <span data-ttu-id="e422b-296">Můžete změnit velikost stránkovacího zvýšením maximální velikost stránky na klientovi a serveru.</span><span class="sxs-lookup"><span data-stu-id="e422b-296">You can change the paging size by increasing the maximum page size on both the client and server.</span></span>  <span data-ttu-id="e422b-297">Chcete-li zvýšit velikost požadované stránce, zadejte `PullOptions` při použití `PullAsync()`:</span><span class="sxs-lookup"><span data-stu-id="e422b-297">To increase the requested page size, specify `PullOptions` when using `PullAsync()`:</span></span>

```
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

<span data-ttu-id="e422b-298">Za předpokladu, že jste udělali `PageSize` rovna nebo větší než 100 v rámci serveru, žádost o vrátí až 100 položek.</span><span class="sxs-lookup"><span data-stu-id="e422b-298">Assuming you have made the `PageSize` equal to or greater than 100 within the server, a request returns up to 100 items.</span></span>

## <span data-ttu-id="e422b-299"><a name="#offlinesync"></a>Práce s tabulkami Offline</span><span class="sxs-lookup"><span data-stu-id="e422b-299"><a name="#offlinesync"></a>Work with Offline Tables</span></span>
<span data-ttu-id="e422b-300">Offline tabulky použijte místní úložiště pro ukládání dat SQLite pro použití v režimu offline.</span><span class="sxs-lookup"><span data-stu-id="e422b-300">Offline tables use a local SQLite store to store data for use when offline.</span></span>  <span data-ttu-id="e422b-301">Všechny tabulky, které se provádějí operace pro místní úložiště SQLite místo úložiště vzdáleného serveru.</span><span class="sxs-lookup"><span data-stu-id="e422b-301">All table operations are done against the local SQLite store instead of the remote server store.</span></span>  <span data-ttu-id="e422b-302">Pokud chcete vytvořit offline tabulky, nejdřív připravte projektu:</span><span class="sxs-lookup"><span data-stu-id="e422b-302">To create an offline table, first prepare your project:</span></span>

1. <span data-ttu-id="e422b-303">V sadě Visual Studio, klikněte pravým tlačítkem na řešení > **spravovat balíčky NuGet pro řešení...** , vyhledejte a nainstalujte **Microsoft.Azure.Mobile.Client.SQLiteStore** balíček NuGet pro všechny projekty v řešení.</span><span class="sxs-lookup"><span data-stu-id="e422b-303">In Visual Studio, right-click the solution > **Manage NuGet Packages for Solution...**, then search for and install the **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet package for all projects in the solution.</span></span>
2. <span data-ttu-id="e422b-304">(Volitelné) Pro podporu zařízení se systémem Windows, instalovat jeden z následujících balíčků runtime SQLite:</span><span class="sxs-lookup"><span data-stu-id="e422b-304">(Optional) To support Windows devices, install one of the following SQLite runtime packages:</span></span>

   * <span data-ttu-id="e422b-305">**Modul Runtime pro Windows 8.1:** nainstalovat [SQLite pro Windows 8.1][3].</span><span class="sxs-lookup"><span data-stu-id="e422b-305">**Windows 8.1 Runtime:** Install [SQLite for Windows 8.1][3].</span></span>
   * <span data-ttu-id="e422b-306">**Windows Phone 8.1:** nainstalovat [SQLite pro Windows Phone 8.1][4].</span><span class="sxs-lookup"><span data-stu-id="e422b-306">**Windows Phone 8.1:** Install [SQLite for Windows Phone 8.1][4].</span></span>
   * <span data-ttu-id="e422b-307">**Univerzální platformu Windows** nainstalovat [SQLite for Universal Windows][5].</span><span class="sxs-lookup"><span data-stu-id="e422b-307">**Universal Windows Platform** Install [SQLite for the Universal Windows][5].</span></span>
3. <span data-ttu-id="e422b-308">(Volitelné).</span><span class="sxs-lookup"><span data-stu-id="e422b-308">(Optional).</span></span> <span data-ttu-id="e422b-309">Zařízení se systémem Windows, klikněte na tlačítko **odkazy** > **přidat odkaz na...** , rozbalte **Windows** složky > **rozšíření**, povolíte příslušné **SQLite pro systém Windows** SDK spolu s **Visual C++ 2013 modulu Runtime pro Windows** SDK.</span><span class="sxs-lookup"><span data-stu-id="e422b-309">For Windows devices, click **References** > **Add Reference...**, expand the **Windows** folder > **Extensions**, then enable the appropriate **SQLite for Windows** SDK along with the **Visual C++ 2013 Runtime for Windows** SDK.</span></span>
    <span data-ttu-id="e422b-310">Názvy SQLite SDK mírně lišit s každou platformu Windows.</span><span class="sxs-lookup"><span data-stu-id="e422b-310">The SQLite SDK names vary slightly with each Windows platform.</span></span>

<span data-ttu-id="e422b-311">Aby bylo možné vytvořit odkaz na tabulku, musí být připraveno místní úložiště:</span><span class="sxs-lookup"><span data-stu-id="e422b-311">Before a table reference can be created, the local store must be prepared:</span></span>

```
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes the SyncContext using the default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

<span data-ttu-id="e422b-312">Inicializace úložiště se obvykle provádí ihned po vytvoření klienta.</span><span class="sxs-lookup"><span data-stu-id="e422b-312">Store initialization is normally done immediately after the client is created.</span></span>  <span data-ttu-id="e422b-313">**OfflineDbPath** by měla být název souboru vhodné pro použití na všech platformách, které podporujete.</span><span class="sxs-lookup"><span data-stu-id="e422b-313">The **OfflineDbPath** should be a filename suitable for use on all platforms that you support.</span></span>  <span data-ttu-id="e422b-314">Pokud se cesta plně kvalifikovanou cestu (tedy začíná lomítko), je použita v této cestě.</span><span class="sxs-lookup"><span data-stu-id="e422b-314">If the path is a fully qualified path (that is, it starts with a slash), then that path is used.</span></span>  <span data-ttu-id="e422b-315">Pokud cesta není úplná, soubor je umístěn v umístění specifické pro platformu.</span><span class="sxs-lookup"><span data-stu-id="e422b-315">If the path is not fully qualified, the file is placed in a platform-specific location.</span></span>

* <span data-ttu-id="e422b-316">Pro zařízení s iOS a Android výchozí cesta je složce "Osobní soubory".</span><span class="sxs-lookup"><span data-stu-id="e422b-316">For iOS and Android devices, the default path is the "Personal Files" folder.</span></span>
* <span data-ttu-id="e422b-317">Výchozí cesta pro zařízení s Windows, je složce "data aplikací" specifické pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e422b-317">For Windows devices, the default path is the application-specific "AppData" folder.</span></span>

<span data-ttu-id="e422b-318">Odkaz na tabulku můžete získat pomocí `GetSyncTable<>` metoda:</span><span class="sxs-lookup"><span data-stu-id="e422b-318">A table reference can be obtained using the `GetSyncTable<>` method:</span></span>

```
var table = client.GetSyncTable<TodoItem>();
```

<span data-ttu-id="e422b-319">Není nutné k ověření pomocí offline tabulky.</span><span class="sxs-lookup"><span data-stu-id="e422b-319">You do not need to authenticate to use an offline table.</span></span>  <span data-ttu-id="e422b-320">Potřebujete jenom ověřování, když jsou komunikaci s back-end službu.</span><span class="sxs-lookup"><span data-stu-id="e422b-320">You only need to authenticate when you are communicating with the backend service.</span></span>

### <span data-ttu-id="e422b-321"><a name="syncoffline"></a>Synchronizace Offline tabulky</span><span class="sxs-lookup"><span data-stu-id="e422b-321"><a name="syncoffline"></a>Syncing an Offline Table</span></span>
<span data-ttu-id="e422b-322">Offline tabulky nejsou synchronizované s back-end ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e422b-322">Offline tables are not synchronized with the backend by default.</span></span>  <span data-ttu-id="e422b-323">Synchronizace je rozdělit na dvě části.</span><span class="sxs-lookup"><span data-stu-id="e422b-323">Synchronization is split into two pieces.</span></span>  <span data-ttu-id="e422b-324">Doručte změny můžete samostatně stahování nové položky.</span><span class="sxs-lookup"><span data-stu-id="e422b-324">You can push changes separately from downloading new items.</span></span>  <span data-ttu-id="e422b-325">Tady je metoda typické synchronizace:</span><span class="sxs-lookup"><span data-stu-id="e422b-325">Here is a typical sync method:</span></span>

```
public async Task SyncAsync()
{
    ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

    try
    {
        await this.client.SyncContext.PushAsync();

        await this.todoTable.PullAsync(
            //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
            //Use a different query name for each unique query in your program
            "allTodoItems",
            this.todoTable.CreateQuery());
    }
    catch (MobileServicePushFailedException exc)
    {
        if (exc.PushResult != null)
        {
            syncErrors = exc.PushResult.Errors;
        }
    }

    // Simple error/conflict handling. A real application would handle the various errors like network conditions,
    // server conflicts and others via the IMobileServiceSyncHandler.
    if (syncErrors != null)
    {
        foreach (var error in syncErrors)
        {
            if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
            {
                //Update failed, reverting to server's copy.
                await error.CancelAndUpdateItemAsync(error.Result);
            }
            else
            {
                // Discard local change.
                await error.CancelAndDiscardItemAsync();
            }

            Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
        }
    }
}
```

<span data-ttu-id="e422b-326">Pokud první argument `PullAsync` má hodnotu null, není použita přírůstkové synchronizace.</span><span class="sxs-lookup"><span data-stu-id="e422b-326">If the first argument to `PullAsync` is null, then incremental sync is not used.</span></span>  <span data-ttu-id="e422b-327">Každé operace synchronizace načte všechny záznamy.</span><span class="sxs-lookup"><span data-stu-id="e422b-327">Each sync operation retrieves all records.</span></span>

<span data-ttu-id="e422b-328">Sada SDK provádí implicitní `PushAsync()` než vytažení záznamů.</span><span class="sxs-lookup"><span data-stu-id="e422b-328">The SDK performs an implicit `PushAsync()` before pulling records.</span></span>

<span data-ttu-id="e422b-329">Zpracování konfliktů dochází `PullAsync()` metoda.</span><span class="sxs-lookup"><span data-stu-id="e422b-329">Conflict handling happens on a `PullAsync()` method.</span></span>  <span data-ttu-id="e422b-330">Stejným způsobem jako online tabulky můžete řešit konflikty.</span><span class="sxs-lookup"><span data-stu-id="e422b-330">You can deal with conflicts in the same way as online tables.</span></span>  <span data-ttu-id="e422b-331">Vytváří konflikt při `PullAsync()` je volána místo v průběhu insert, update nebo delete.</span><span class="sxs-lookup"><span data-stu-id="e422b-331">The conflict is produced when `PullAsync()` is called instead of during the insert, update, or delete.</span></span> <span data-ttu-id="e422b-332">Pokud je v konfliktu více, jsou seskupeny do jednoho MobileServicePushFailedException.</span><span class="sxs-lookup"><span data-stu-id="e422b-332">If multiple conflicts happen, they are bundled into a single MobileServicePushFailedException.</span></span>  <span data-ttu-id="e422b-333">Zpracování každé selhání samostatně.</span><span class="sxs-lookup"><span data-stu-id="e422b-333">Handle each failure separately.</span></span>

## <span data-ttu-id="e422b-334"><a name="#customapi"></a>Práce s vlastní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e422b-334"><a name="#customapi"></a>Work with a custom API</span></span>
<span data-ttu-id="e422b-335">Vlastní rozhraní API umožňuje definovat vlastní koncové body, které zveřejňují funkce serveru které není mapovat k typu vložení, aktualizaci, odstranění nebo operace čtení.</span><span class="sxs-lookup"><span data-stu-id="e422b-335">A custom API enables you to define custom endpoints that expose server functionality that does not map to an insert, update, delete, or read operation.</span></span> <span data-ttu-id="e422b-336">Pomocí vlastního rozhraní API, může mít větší kontrolu nad zasílání zpráv, včetně čtení a nastavení hlavičky protokolu HTTP zpráv a definování formátu textu zprávy kromě formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="e422b-336">By using a custom API, you can have more control over messaging, including reading and setting HTTP message headers and defining a message body format other than JSON.</span></span>

<span data-ttu-id="e422b-337">Můžete pro volání jedné z volání vlastní rozhraní API [InvokeApiAsync] metody na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e422b-337">You call a custom API by calling one of the [InvokeApiAsync] methods on the client.</span></span> <span data-ttu-id="e422b-338">Například následující řádek kódu odešle požadavek POST do **completeAll** rozhraní API na back-end:</span><span class="sxs-lookup"><span data-stu-id="e422b-338">For example, the following line of code sends a POST request to the **completeAll** API on the backend:</span></span>

```
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

<span data-ttu-id="e422b-339">Tento formulář je typu metoda volání a vyžaduje, aby **MarkAllResult** vrátí typ je definován.</span><span class="sxs-lookup"><span data-stu-id="e422b-339">This form is a typed method call and requires that the **MarkAllResult** return type is defined.</span></span> <span data-ttu-id="e422b-340">Typové i bez typu metody jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="e422b-340">Both typed and untyped methods are supported.</span></span>

<span data-ttu-id="e422b-341">Metoda InvokeApiAsync() přidá '/ api /' do rozhraní API, kterou chcete volat, pokud začíná rozhraní API lomítkem (/).</span><span class="sxs-lookup"><span data-stu-id="e422b-341">The InvokeApiAsync() method prepends '/api/' to the API that you wish to call unless the API starts with a '/'.</span></span>
<span data-ttu-id="e422b-342">Například:</span><span class="sxs-lookup"><span data-stu-id="e422b-342">For example:</span></span>

* <span data-ttu-id="e422b-343">`InvokeApiAsync("completeAll",...)`volání /api/completeAll na back-end</span><span class="sxs-lookup"><span data-stu-id="e422b-343">`InvokeApiAsync("completeAll",...)` calls /api/completeAll on the backend</span></span>
* <span data-ttu-id="e422b-344">`InvokeApiAsync("/.auth/me",...)`volání /.auth/me na back-end</span><span class="sxs-lookup"><span data-stu-id="e422b-344">`InvokeApiAsync("/.auth/me",...)` calls /.auth/me on the backend</span></span>

<span data-ttu-id="e422b-345">InvokeApiAsync můžete použít k volání žádné WebAPI, včetně těchto WebAPIs, které nejsou definovány s Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="e422b-345">You can use InvokeApiAsync to call any WebAPI, including those WebAPIs that are not defined with Azure Mobile Apps.</span></span>  <span data-ttu-id="e422b-346">Při použití InvokeApiAsync() odpovídající hlavičky, včetně ověřování hlavičky, odešlou se žádostí.</span><span class="sxs-lookup"><span data-stu-id="e422b-346">When you use InvokeApiAsync(), the appropriate headers, including authentication headers, are sent with the request.</span></span>

## <span data-ttu-id="e422b-347"><a name="authentication"></a>Ověřování uživatelů</span><span class="sxs-lookup"><span data-stu-id="e422b-347"><a name="authentication"></a>Authenticate users</span></span>
<span data-ttu-id="e422b-348">Mobilní aplikace podporuje ověřování a autorizaci uživatelů aplikace pomocí různých zprostředkovatelů externí identity: Facebook, Google, Microsoft Account, Twitter a Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e422b-348">Mobile Apps supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, Twitter, and Azure Active Directory.</span></span> <span data-ttu-id="e422b-349">Můžete nastavit oprávnění pro tabulky, pokud chcete omezit přístup pro určité operace pouze ověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="e422b-349">You can set permissions on tables to restrict access for specific operations to only authenticated users.</span></span> <span data-ttu-id="e422b-350">Můžete také použít identitu ověřeného uživatele k implementaci autorizační pravidla v skripty serveru.</span><span class="sxs-lookup"><span data-stu-id="e422b-350">You can also use the identity of authenticated users to implement authorization rules in server scripts.</span></span> <span data-ttu-id="e422b-351">Další informace najdete v kurzu [přidání ověřování do aplikace].</span><span class="sxs-lookup"><span data-stu-id="e422b-351">For more information, see the tutorial [Add authentication to your app].</span></span>

<span data-ttu-id="e422b-352">Jsou podporovány dva ověřování toky: *klienta spravovat* a *server spravovaný* toku.</span><span class="sxs-lookup"><span data-stu-id="e422b-352">Two authentication flows are supported: *client-managed* and *server-managed* flow.</span></span> <span data-ttu-id="e422b-353">Tok spravovaných server poskytuje nejjednodušší zkušeností ověřování, jako je závislé na poskytovatele webové ověřování rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e422b-353">The server-managed flow provides the simplest authentication experience, as it relies on the provider's web authentication interface.</span></span> <span data-ttu-id="e422b-354">Tok spravovaných klienta umožňuje hlubší integrace s funkcemi konkrétní zařízení jako přitom spoléhá na konkrétní zařízení specifické pro poskytovatele sady SDK.</span><span class="sxs-lookup"><span data-stu-id="e422b-354">The client-managed flow allows for deeper integration with device-specific capabilities as it relies on provider-specific device-specific SDKs.</span></span>

> [!NOTE]
> <span data-ttu-id="e422b-355">Doporučujeme používat k klienta spravovat toku ve svých aplikacích produkční.</span><span class="sxs-lookup"><span data-stu-id="e422b-355">We recommend using a client-managed flow in your production apps.</span></span>

<span data-ttu-id="e422b-356">Nastavení ověřování, je nutné se jeden nebo více poskytovatelů identity zaregistrovat vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e422b-356">To set up authentication, you must register your app with one or more identity providers.</span></span>  <span data-ttu-id="e422b-357">Poskytovatel identity vygeneruje ID klienta a tajný klíč klienta pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e422b-357">The identity provider generates a client ID and a client secret for your app.</span></span>  <span data-ttu-id="e422b-358">Tyto hodnoty jsou pak uloženy v váš back-end povolit ověřování/autorizace služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e422b-358">These values are then set in your backend to enable Azure App Service authentication/authorization.</span></span>  <span data-ttu-id="e422b-359">Další informace, postupujte podle podrobné pokyny v tomto kurzu [přidání ověřování do aplikace].</span><span class="sxs-lookup"><span data-stu-id="e422b-359">For more information, follow the detailed instructions in the tutorial [Add authentication to your app].</span></span>

<span data-ttu-id="e422b-360">V této části jsou popsané v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="e422b-360">The following topics are covered in this section:</span></span>

* [<span data-ttu-id="e422b-361">Klienta spravovat ověřování</span><span class="sxs-lookup"><span data-stu-id="e422b-361">Client-managed authentication</span></span>](#clientflow)
* [<span data-ttu-id="e422b-362">Spravovaný server ověřování</span><span class="sxs-lookup"><span data-stu-id="e422b-362">Server-managed authentication</span></span>](#serverflow)
* [<span data-ttu-id="e422b-363">Ukládání do mezipaměti ověřovací token</span><span class="sxs-lookup"><span data-stu-id="e422b-363">Caching the authentication token</span></span>](#caching)

### <span data-ttu-id="e422b-364"><a name="clientflow"></a>Klienta spravovat ověřování</span><span class="sxs-lookup"><span data-stu-id="e422b-364"><a name="clientflow"></a>Client-managed authentication</span></span>
<span data-ttu-id="e422b-365">Aplikace můžete nezávisle obraťte se na poskytovatele identity a zadejte token vrácený během přihlašování s váš back-end.</span><span class="sxs-lookup"><span data-stu-id="e422b-365">Your app can independently contact the identity provider and then provide the returned token during login with your backend.</span></span> <span data-ttu-id="e422b-366">Tento tok klienta umožňuje k jednotnému přihlášení pro uživatele nebo k načtení dat další uživatele z poskytovatele identit.</span><span class="sxs-lookup"><span data-stu-id="e422b-366">This client flow enables you to provide a single sign-on experience for users or to retrieve additional user data from the identity provider.</span></span> <span data-ttu-id="e422b-367">Tok ověřování klientů je upřednostňovaný pomocí toku serveru jako zprostředkovatele identity SDK poskytuje více nativní UX chování a umožňuje pro další přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="e422b-367">Client flow authentication is preferred to using a server flow as the identity provider SDK provides a more native UX feel and allows for additional customization.</span></span>

<span data-ttu-id="e422b-368">Příklady jsou k dispozici pro následující způsoby ověřování tok klienta:</span><span class="sxs-lookup"><span data-stu-id="e422b-368">Examples are provided for the following client-flow authentication patterns:</span></span>

* [<span data-ttu-id="e422b-369">Knihovna ověřování Active Directory</span><span class="sxs-lookup"><span data-stu-id="e422b-369">Active Directory Authentication Library</span></span>](#adal)
* [<span data-ttu-id="e422b-370">Facebook nebo Google</span><span class="sxs-lookup"><span data-stu-id="e422b-370">Facebook or Google</span></span>](#client-facebook)
* [<span data-ttu-id="e422b-371">Live SDK</span><span class="sxs-lookup"><span data-stu-id="e422b-371">Live SDK</span></span>](#client-livesdk)

#### <span data-ttu-id="e422b-372"><a name="adal"></a>Ověřuje uživatele pomocí knihovny Active Directory Authentication Library</span><span class="sxs-lookup"><span data-stu-id="e422b-372"><a name="adal"></a>Authenticate users with the Active Directory Authentication Library</span></span>
<span data-ttu-id="e422b-373">Active Directory Authentication Library (ADAL) slouží k ověřování uživatelů inicializovat z klienta pomocí ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e422b-373">You can use the Active Directory Authentication Library (ADAL) to initiate user authentication from the client using Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="e422b-374">Podle konfigurace váš back-end mobilní aplikace pro přihlašování AAD [jak nakonfigurovat App Service pro přihlášení služby Active Directory] kurzu.</span><span class="sxs-lookup"><span data-stu-id="e422b-374">Configure your mobile app backend for AAD sign-on by following the [How to configure App Service for Active Directory login] tutorial.</span></span> <span data-ttu-id="e422b-375">Ujistěte se, že dokončení volitelný krok registrace nativní klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e422b-375">Make sure to complete the optional step of registering a native client application.</span></span>
2. <span data-ttu-id="e422b-376">V sadě Visual Studio nebo Xamarin Studio, otevřete projekt a přidejte odkaz na `Microsoft.IdentityModel.CLients.ActiveDirectory` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="e422b-376">In Visual Studio or Xamarin Studio, open your project and add a reference to the `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet package.</span></span> <span data-ttu-id="e422b-377">Při hledání, zahrňte předběžné verze.</span><span class="sxs-lookup"><span data-stu-id="e422b-377">When searching, include pre-release versions.</span></span>
3. <span data-ttu-id="e422b-378">Přidejte následující kód k vaší aplikaci, podle platformy, které používáte.</span><span class="sxs-lookup"><span data-stu-id="e422b-378">Add the following code to your application, according to the platform you are using.</span></span> <span data-ttu-id="e422b-379">V každé zkontrolujte následující náhrady:</span><span class="sxs-lookup"><span data-stu-id="e422b-379">In each, make the following replacements:</span></span>

   * <span data-ttu-id="e422b-380">Nahraďte **INSERT. AUTORITY zde** s názvem klienta, ve kterém jste zřídili vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e422b-380">Replace **INSERT-AUTHORITY-HERE** with the name of the tenant in which you provisioned your application.</span></span> <span data-ttu-id="e422b-381">Formát by měl být https://login.microsoftonline.com/contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="e422b-381">The format should be https://login.microsoftonline.com/contoso.onmicrosoft.com.</span></span> <span data-ttu-id="e422b-382">Tuto hodnotu lze kopírovat z karty domény v Azure Active Directory v [portál Azure classic].</span><span class="sxs-lookup"><span data-stu-id="e422b-382">This value can be copied from the Domain tab in your Azure Active Directory in the [Azure classic portal].</span></span>
   * <span data-ttu-id="e422b-383">Nahraďte **INSERT-RESOURCE-ID-zde** s ID klienta pro váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e422b-383">Replace **INSERT-RESOURCE-ID-HERE** with the client ID for your mobile app backend.</span></span> <span data-ttu-id="e422b-384">Můžete získat ID klienta z **Upřesnit** v části **nastavení Azure Active Directory** na portálu.</span><span class="sxs-lookup"><span data-stu-id="e422b-384">You can obtain the client ID from the **Advanced** tab under **Azure Active Directory Settings** in the portal.</span></span>
   * <span data-ttu-id="e422b-385">Nahraďte **INSERT klienta ID zde** s ID klienta, který jste zkopírovali z nativní klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e422b-385">Replace **INSERT-CLIENT-ID-HERE** with the client ID you copied from the native client application.</span></span>
   * <span data-ttu-id="e422b-386">Nahraďte **vložení PŘESMĚROVÁNÍ URI zde** s vaší lokality */.auth/login/done* koncový bod, pomocí schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e422b-386">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using the HTTPS scheme.</span></span> <span data-ttu-id="e422b-387">Tato hodnota by měla být podobná *https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="e422b-387">This value should be similar to *https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

     <span data-ttu-id="e422b-388">Následuje kód potřebný pro každou platformu:</span><span class="sxs-lookup"><span data-stu-id="e422b-388">The code needed for each platform follows:</span></span>

     <span data-ttu-id="e422b-389">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="e422b-389">**Windows:**</span></span>

    ```
    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        while (user == null)
        {
            string message;
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await App.MobileService.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
    ```

     <span data-ttu-id="e422b-390">**Xamarin.iOS**</span><span class="sxs-lookup"><span data-stu-id="e422b-390">**Xamarin.iOS**</span></span>

    ```
    private MobileServiceUser user;
    private async Task AuthenticateAsync(UIViewController view)
    {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        try
        {
            AuthenticationContext ac = new AuthenticationContext(authority);
            AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                new Uri(redirectUri), new PlatformParameters(view));
            JObject payload = new JObject();
            payload["access_token"] = ar.AccessToken;
            user = await client.LoginAsync(
                MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
        }
        catch (Exception ex)
        {
            Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
        }
    }
    ```

     <span data-ttu-id="e422b-391">**Xamarin.Android**</span><span class="sxs-lookup"><span data-stu-id="e422b-391">**Xamarin.Android**</span></span>

    ```
    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        try
        {
            AuthenticationContext ac = new AuthenticationContext(authority);
            AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                new Uri(redirectUri), new PlatformParameters(this));
            JObject payload = new JObject();
            payload["access_token"] = ar.AccessToken;
            user = await client.LoginAsync(
                MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
        }
        catch (Exception ex)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(ex.Message);
            builder.SetTitle("You must log in. Login Required");
            builder.Create().Show();
        }
    }
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {

        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ```

#### <span data-ttu-id="e422b-392"><a name="client-facebook"></a>Jednotné přihlašování pomocí tokenu z Facebook nebo Google</span><span class="sxs-lookup"><span data-stu-id="e422b-392"><a name="client-facebook"></a>Single Sign-On using a token from Facebook or Google</span></span>
<span data-ttu-id="e422b-393">Tok klienta můžete použít, jak je uvedeno v této fragmentu kódu pro Facebook nebo Google.</span><span class="sxs-lookup"><span data-stu-id="e422b-393">You can use the client flow as shown in this snippet for Facebook or Google.</span></span>

```
var token = new JObject();
// Replace access_token_value with actual value of your access token obtained
// using the Facebook or Google SDK.
token.Add("access_token", "access_token_value");

private MobileServiceUser user;
private async Task AuthenticateAsync()
{
    while (user == null)
    {
        string message;
        try
        {
            // Change MobileServiceAuthenticationProvider.Facebook
            // to MobileServiceAuthenticationProvider.Google if using Google auth.
            user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
            message = string.Format("You are now logged in - {0}", user.UserId);
        }
        catch (InvalidOperationException)
        {
            message = "You must log in. Login Required";
        }

        var dialog = new MessageDialog(message);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
}
```

#### <span data-ttu-id="e422b-394"><a name="client-livesdk"></a>Jednotné přihlašování pomocí Account Microsoft Live SDK</span><span class="sxs-lookup"><span data-stu-id="e422b-394"><a name="client-livesdk"></a>Single Sign On using Microsoft Account with the Live SDK</span></span>
<span data-ttu-id="e422b-395">K ověřování uživatelů, je nutné zaregistrovat aplikaci ve středisku pro vývojáře účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e422b-395">To authenticate users, you must register your app at the Microsoft account Developer Center.</span></span> <span data-ttu-id="e422b-396">Nakonfigurujte podrobnosti registrace na váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e422b-396">Configure the registration details on your Mobile App backend.</span></span> <span data-ttu-id="e422b-397">Vytvořit registrace účtu Microsoft a připojte ho k váš back-end mobilní aplikace, proveďte kroky v [svou aplikaci zaregistrovat pro používat přihlašovací údaje účtu Microsoft].</span><span class="sxs-lookup"><span data-stu-id="e422b-397">To create a Microsoft account registration and connect it to your Mobile App backend, complete the steps in [Register your app to use a Microsoft account login].</span></span> <span data-ttu-id="e422b-398">Pokud máte Windows Store a Windows Phone 8 nebo Silverlight verze aplikace, nejprve zaregistrujte verze Windows Store.</span><span class="sxs-lookup"><span data-stu-id="e422b-398">If you have both Windows Store and Windows Phone 8/Silverlight versions of your app, register the Windows Store version first.</span></span>

<span data-ttu-id="e422b-399">Následující kód se ověří pomocí sady SDK za provozu a použije token vrácený pro přihlášení k váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e422b-399">The following code authenticates using Live SDK and uses the returned token to sign in to your Mobile App backend.</span></span>

```
private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
private async System.Threading.Tasks.Task AuthenticateAsync()
{

    // Get the URL the Mobile App backend.
    var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

    // Create the authentication client for Windows Store using the service URL.
    LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
    //// Create the authentication client for Windows Phone using the client ID of the registration.
    //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

    while (session == null)
    {
        // Request the authentication token from the Live authentication service.
        // The wl.basic scope should always be requested.  Other scopes can be added
        LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
        if (result.Status == LiveConnectSessionStatus.Connected)
        {
            session = result.Session;

            // Get information about the logged-in user.
            LiveConnectClient client = new LiveConnectClient(session);
            LiveOperationResult meResult = await client.GetAsync("me");

            // Use the Microsoft account auth token to sign in to App Service.
            MobileServiceUser loginResult = await App.MobileService
                .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

            // Display a personalized sign-in greeting.
            string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
            var message = string.Format("You are now logged in - {0}", loginResult.UserId);
            var dialog = new MessageDialog(message, title);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
        else
        {
            session = null;
            var dialog = new MessageDialog("You must log in.", "Login Required");
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
}
```

<span data-ttu-id="e422b-400">Další informace najdete v tématu [Windows Live SDK] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="e422b-400">For more information, see the [Windows Live SDK] documentation.</span></span>

### <span data-ttu-id="e422b-401"><a name="serverflow"></a>Spravovaný server ověřování</span><span class="sxs-lookup"><span data-stu-id="e422b-401"><a name="serverflow"></a>Server-managed authentication</span></span>
<span data-ttu-id="e422b-402">Po registraci zprostředkovatele identity volání [LoginAsync] metodu [MobileServiceClient] s [MobileServiceAuthenticationProvider] hodnotu svého poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="e422b-402">Once you have registered your identity provider, call the [LoginAsync] method on the [MobileServiceClient] with the [MobileServiceAuthenticationProvider] value of your provider.</span></span> <span data-ttu-id="e422b-403">Například následující kód spustí serveru toku přihlášení pomocí Facebooku.</span><span class="sxs-lookup"><span data-stu-id="e422b-403">For example, the following code initiates a server flow sign-in by using Facebook.</span></span>

```
private MobileServiceUser user;
private async System.Threading.Tasks.Task Authenticate()
{
    while (user == null)
    {
        string message;
        try
        {
            user = await client
                .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
            message =
                string.Format("You are now logged in - {0}", user.UserId);
        }
        catch (InvalidOperationException)
        {
            message = "You must log in. Login Required";
        }

        var dialog = new MessageDialog(message);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
}
```

<span data-ttu-id="e422b-404">Pokud používáte zprostředkovatele identity než Facebook, změňte hodnotu [MobileServiceAuthenticationProvider] na hodnotu pro poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="e422b-404">If you are using an identity provider other than Facebook, change the value of [MobileServiceAuthenticationProvider] to the value for your provider.</span></span>

<span data-ttu-id="e422b-405">V toku server Azure App Service spravuje tok ověřování OAuth zobrazením přihlašovací stránky vybraného zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="e422b-405">In a server flow, Azure App Service manages the OAuth authentication flow by displaying the sign-in page of the selected provider.</span></span>  <span data-ttu-id="e422b-406">Jakmile se identity vrátí zprostředkovatele služby Azure App Service generuje ověřovací token služby App Service.</span><span class="sxs-lookup"><span data-stu-id="e422b-406">Once the identity provider returns, Azure App Service generates an App Service authentication token.</span></span> <span data-ttu-id="e422b-407">[LoginAsync] metoda vrátí [MobileServiceUser], který poskytuje i [UserId] ověřeného uživatele a [MobileServiceAuthenticationToken], jako webového tokenu JSON (JWT).</span><span class="sxs-lookup"><span data-stu-id="e422b-407">The [LoginAsync] method returns a [MobileServiceUser], which provides both the [UserId] of the authenticated user and the [MobileServiceAuthenticationToken], as a JSON web token (JWT).</span></span> <span data-ttu-id="e422b-408">Tento token se může uložit do mezipaměti a znovu požívat do vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="e422b-408">This token can be cached and reused until it expires.</span></span> <span data-ttu-id="e422b-409">Další informace najdete v tématu [ukládání do mezipaměti ověřovací token](#caching).</span><span class="sxs-lookup"><span data-stu-id="e422b-409">For more information, see [Caching the authentication token](#caching).</span></span>

### <span data-ttu-id="e422b-410"><a name="caching"></a>Ukládání do mezipaměti ověřovací token</span><span class="sxs-lookup"><span data-stu-id="e422b-410"><a name="caching"></a>Caching the authentication token</span></span>
<span data-ttu-id="e422b-411">V některých případech volání metody přihlášení můžete zabránit po prvním úspěšném ověření ukládání ověřovací token od zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="e422b-411">In some cases, the call to the login method can be avoided after the first successful authentication by storing the authentication token from the provider.</span></span>  <span data-ttu-id="e422b-412">Aplikace Windows Store a UWP můžete použít [PasswordVault] pro ukládání do mezipaměti aktuální ověřovací token po úspěšného přihlášení, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e422b-412">Windows Store and UWP apps can use [PasswordVault] to cache the current authentication token after a successful sign-in, as follows:</span></span>

```
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

<span data-ttu-id="e422b-413">Hodnota ID uživatele je uložena jako uživatelské jméno pověření a token je uložená jako heslo.</span><span class="sxs-lookup"><span data-stu-id="e422b-413">The UserId value is stored as the UserName of the credential and the token is the stored as the Password.</span></span> <span data-ttu-id="e422b-414">Na následujících podniky, můžete zkontrolovat **PasswordVault** pro přihlašovací údaje v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e422b-414">On subsequent start-ups, you can check the **PasswordVault** for cached credentials.</span></span> <span data-ttu-id="e422b-415">Následující příklad používá pověření uložená v mezipaměti, při jejich zjištění a v opačném případě se pokusí provést ověření znovu s back-end:</span><span class="sxs-lookup"><span data-stu-id="e422b-415">The following example uses cached credentials when they are found, and otherwise attempts to authenticate again with the backend:</span></span>

```
// Try to retrieve stored credentials.
var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
if (creds != null)
{
    // Create the current user from the stored credentials.
    client.currentUser = new MobileServiceUser(creds.UserName);
    client.currentUser.MobileServiceAuthenticationToken =
        vault.Retrieve("Facebook", creds.UserName).Password;
}
else
{
    // Regular login flow and cache the token as shown above.
}
```

<span data-ttu-id="e422b-416">Při odhlášení uživatele, musíte také odebrat uložených přihlašovacích údajů, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e422b-416">When you sign out a user, you must also remove the stored credential, as follows:</span></span>

```
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

<span data-ttu-id="e422b-417">Pomocí aplikace Xamarin [Xamarin.Auth] rozhraní API bezpečně uložit přihlašovací údaje v **účet** objektu.</span><span class="sxs-lookup"><span data-stu-id="e422b-417">Xamarin    apps use the [Xamarin.Auth] APIs to securely store credentials in an **Account** object.</span></span> <span data-ttu-id="e422b-418">Příklad použití tato rozhraní API, najdete v článku [AuthStore.cs] souboru kódu v [ContosoMoments fotografií sdílení ukázka](https://github.com/azure-appservice-samples/ContosoMoments).</span><span class="sxs-lookup"><span data-stu-id="e422b-418">For an example of using these APIs, see the [AuthStore.cs] code file in the [ContosoMoments photo sharing sample](https://github.com/azure-appservice-samples/ContosoMoments).</span></span>

<span data-ttu-id="e422b-419">Pokud používáte klienta spravovat ověřování, můžete také mezipaměti přístupový token, který zajišťuje poskytovatel například Facebook nebo Twitter.</span><span class="sxs-lookup"><span data-stu-id="e422b-419">When you use client-managed authentication, you can also cache the access token obtained from your provider such as Facebook or Twitter.</span></span> <span data-ttu-id="e422b-420">Tento token můžete zadat požádat o nový ověřovací token z back-end, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e422b-420">This token can be supplied to request a new authentication token from the backend, as follows:</span></span>

```
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using the access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <span data-ttu-id="e422b-421"><a name="pushnotifications"></a>Nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="e422b-421"><a name="pushnotifications"></a>Push Notifications</span></span>
<span data-ttu-id="e422b-422">V následujících tématech najdete nabízených oznámení:</span><span class="sxs-lookup"><span data-stu-id="e422b-422">The following topics cover Push Notifications:</span></span>

* [<span data-ttu-id="e422b-423">Registrace pro nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="e422b-423">Register for Push Notifications</span></span>](#register-for-push)
* [<span data-ttu-id="e422b-424">Získat identifikátor SID balíčku Windows Store</span><span class="sxs-lookup"><span data-stu-id="e422b-424">Obtain a Windows Store package SID</span></span>](#package-sid)
* [<span data-ttu-id="e422b-425">Zaregistrovat se šablonami a platformy</span><span class="sxs-lookup"><span data-stu-id="e422b-425">Register with Cross-platform templates</span></span>](#register-xplat)

### <span data-ttu-id="e422b-426"><a name="register-for-push"></a>Postupy: registrace pro nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="e422b-426"><a name="register-for-push"></a>How to: Register for Push Notifications</span></span>
<span data-ttu-id="e422b-427">Klient pro Mobile Apps umožňuje zaregistrovat pro nabízená oznámení pomocí Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="e422b-427">The Mobile Apps client enables you to register for push notifications with Azure Notification Hubs.</span></span> <span data-ttu-id="e422b-428">Při registraci, můžete získat popisovač, které jste získali z specifické pro platformu nabízená oznámení služby (PNS).</span><span class="sxs-lookup"><span data-stu-id="e422b-428">When registering, you obtain a handle that you obtain from the platform-specific Push Notification Service (PNS).</span></span> <span data-ttu-id="e422b-429">Pak poskytnete tuto hodnotu společně s všechny značky, když vytvoříte registrace.</span><span class="sxs-lookup"><span data-stu-id="e422b-429">You then provide this value along with any tags when you create the registration.</span></span> <span data-ttu-id="e422b-430">Následující kód registruje aplikace pro Windows pro nabízená oznámení pomocí služby oznámení Windows (WNS):</span><span class="sxs-lookup"><span data-stu-id="e422b-430">The following code registers your Windows app for push notifications with the Windows Notification Service (WNS):</span></span>

```
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using the new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

<span data-ttu-id="e422b-431">Pokud nabízíte k WNS, pak musíte [získat identifikátor SID balíčku Windows Store](#package-sid).</span><span class="sxs-lookup"><span data-stu-id="e422b-431">If you are pushing to WNS, then you MUST [obtain a Windows Store package SID](#package-sid).</span></span>  <span data-ttu-id="e422b-432">Další informace o aplikace pro Windows, včetně toho, jak zaregistrovat pro šablony registrace najdete v části [přidat nabízená oznámení do vaší aplikace].</span><span class="sxs-lookup"><span data-stu-id="e422b-432">For more information on Windows apps, including how to register for template registrations, see [Add push notifications to your app].</span></span>

<span data-ttu-id="e422b-433">Požaduje značky z klienta není podporována.</span><span class="sxs-lookup"><span data-stu-id="e422b-433">Requesting tags from the client is not supported.</span></span>  <span data-ttu-id="e422b-434">Značka požadavky jsou bezobslužně vyřadit z registrace.</span><span class="sxs-lookup"><span data-stu-id="e422b-434">Tag Requests are silently dropped from registration.</span></span>
<span data-ttu-id="e422b-435">Pokud chcete zaregistrovat zařízení s značkami, vytvořte vlastní rozhraní API, který používá rozhraní API centra oznámení k provedení registrace vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="e422b-435">If you wish to register your device with tags, create a Custom API that uses the Notification Hubs API to perform the registration on your behalf.</span></span>  <span data-ttu-id="e422b-436">[Volání rozhraní API vlastní](#customapi) místo `RegisterNativeAsync()` metoda.</span><span class="sxs-lookup"><span data-stu-id="e422b-436">[Call the Custom API](#customapi) instead of the `RegisterNativeAsync()` method.</span></span>

### <span data-ttu-id="e422b-437"><a name="package-sid"></a>Postupy: získání SID balíčku Windows Store</span><span class="sxs-lookup"><span data-stu-id="e422b-437"><a name="package-sid"></a>How to: Obtain a Windows Store package SID</span></span>
<span data-ttu-id="e422b-438">SID balíčku je nutné pro povolení nabízených oznámení v aplikacích pro Windows Store.</span><span class="sxs-lookup"><span data-stu-id="e422b-438">A package SID is needed for enabling push notifications in Windows Store apps.</span></span>  <span data-ttu-id="e422b-439">Pokud chcete získat SID balíčku, registrace vaší aplikace s Windows Store.</span><span class="sxs-lookup"><span data-stu-id="e422b-439">To receive a package SID, register your application with the Windows Store.</span></span>

<span data-ttu-id="e422b-440">Chcete-li získat tuto hodnotu:</span><span class="sxs-lookup"><span data-stu-id="e422b-440">To obtain this value:</span></span>

1. <span data-ttu-id="e422b-441">V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na projekt aplikace Windows Store, klikněte na tlačítko **úložiště** > **přidružit aplikaci ve Store...** .</span><span class="sxs-lookup"><span data-stu-id="e422b-441">In Visual Studio Solution Explorer, right-click the Windows Store app project, click **Store** > **Associate App with the Store...**.</span></span>
2. <span data-ttu-id="e422b-442">V průvodci klikněte na tlačítko **Další**, přihlaste se pomocí účtu Microsoft, zadejte název aplikace v rámci **rezervovat nový název aplikace**, pak klikněte na tlačítko **rezervy**.</span><span class="sxs-lookup"><span data-stu-id="e422b-442">In the wizard, click **Next**, sign in with your Microsoft account, type a name for your app in **Reserve a new app name**, then click **Reserve**.</span></span>
3. <span data-ttu-id="e422b-443">Po registraci aplikace je úspěšně vytvořen, vyberte název aplikace, klikněte na **Další**a potom klikněte na **přidružit**.</span><span class="sxs-lookup"><span data-stu-id="e422b-443">After the app registration is successfully created, select the app name, click **Next**, and then click **Associate**.</span></span>
4. <span data-ttu-id="e422b-444">Přihlaste se k [Windows Dev Center] pomocí Account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e422b-444">Log in to the [Windows Dev Center] using your Microsoft Account.</span></span> <span data-ttu-id="e422b-445">V části **Moje aplikace**, klikněte na možnost registrace aplikací, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="e422b-445">Under **My apps**, click the app registration you created.</span></span>
5. <span data-ttu-id="e422b-446">Klikněte na tlačítko **správy aplikací** > **identity aplikace**a přejděte k části najít váš **SID balíčku**.</span><span class="sxs-lookup"><span data-stu-id="e422b-446">Click **App management** > **App identity**, and then scroll down to find your **Package SID**.</span></span>

<span data-ttu-id="e422b-447">Mnoho používá identifikátor SID balíčku s nimi zacházet jako identifikátor URI, v takovém případě budete muset použít *ms aplikace: / /* schéma.</span><span class="sxs-lookup"><span data-stu-id="e422b-447">Many uses of the package SID treat it as a URI, in which case you need to use *ms-app://* as the scheme.</span></span> <span data-ttu-id="e422b-448">Poznamenejte si verzi vašeho balíčku SID tvořena zřetězením tuto hodnotu jako předponu.</span><span class="sxs-lookup"><span data-stu-id="e422b-448">Make note of the version of your package SID formed by concatenating this value as a prefix.</span></span>

<span data-ttu-id="e422b-449">Aplikace Xamarin vyžadovat některé další kód, abyste mohli zaregistrovat aplikaci systémem iOS nebo Android platformy.</span><span class="sxs-lookup"><span data-stu-id="e422b-449">Xamarin apps require some additional code to be able to register an app running on the iOS or Android platforms.</span></span> <span data-ttu-id="e422b-450">Další informace naleznete v tématu pro vaši platformu:</span><span class="sxs-lookup"><span data-stu-id="e422b-450">For more information, see the topic for your platform:</span></span>

* [<span data-ttu-id="e422b-451">Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="e422b-451">Xamarin.Android</span></span>](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [<span data-ttu-id="e422b-452">Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="e422b-452">Xamarin.iOS</span></span>](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <span data-ttu-id="e422b-453"><a name="register-xplat"></a>Postupy: registrace nabízených šablony k odesílání oznámení napříč platformami</span><span class="sxs-lookup"><span data-stu-id="e422b-453"><a name="register-xplat"></a>How to: Register push templates to send cross-platform notifications</span></span>
<span data-ttu-id="e422b-454">Chcete-li zaregistrovat šablony, použijte `RegisterAsync()` metoda s šablonami a následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e422b-454">To register templates, use the `RegisterAsync()` method with the templates, as follows:</span></span>

```
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

<span data-ttu-id="e422b-455">Šablony by měla být `JObject` typy a může obsahovat několik šablon ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="e422b-455">Your templates should be `JObject` types and can contain multiple templates in the following JSON format:</span></span>

```
public JObject myTemplates()
{
    // single template for Windows Notification Service toast
    var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

    var templates = new JObject
    {
        ["generic-message"] = new JObject
        {
            ["body"] = template,
            ["headers"] = new JObject
            {
                ["X-WNS-Type"] = "wns/toast"
            },
            ["tags"] = new JArray()
        },
        ["more-templates"] = new JObject {...}
    };
    return templates;
}
```

<span data-ttu-id="e422b-456">Metoda **RegisterAsync()** přijme také sekundární dlaždice:</span><span class="sxs-lookup"><span data-stu-id="e422b-456">The method **RegisterAsync()** also accepts Secondary Tiles:</span></span>

```
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

<span data-ttu-id="e422b-457">Všechny značky se odstraní a rychle během registrace pro zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e422b-457">All tags are stripped away during registration for security.</span></span> <span data-ttu-id="e422b-458">Přidání značek na zařízení nebo šablony v rámci instalace naleznete v tématu [práci s .NET back-end serveru SDK pro Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="e422b-458">To add tags to installations or templates within installations, see [Work with the .NET backend server SDK for Azure Mobile Apps].</span></span>

<span data-ttu-id="e422b-459">Odeslat oznámení využívá tyto registrované šablony, najdete v tématu [rozhraní API centra oznámení].</span><span class="sxs-lookup"><span data-stu-id="e422b-459">To send notifications utilizing these registered templates, refer to the [Notification Hubs APIs].</span></span>

## <span data-ttu-id="e422b-460"><a name="misc"></a>Ostatní témata</span><span class="sxs-lookup"><span data-stu-id="e422b-460"><a name="misc"></a>Miscellaneous Topics</span></span>
### <span data-ttu-id="e422b-461"><a name="errors"></a>Postupy: zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="e422b-461"><a name="errors"></a>How to: Handle errors</span></span>
<span data-ttu-id="e422b-462">Když dojde k chybě v back-end, klient SDK vyvolá `MobileServiceInvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="e422b-462">When an error occurs in the backend, the client SDK raises a `MobileServiceInvalidOperationException`.</span></span>  <span data-ttu-id="e422b-463">Následující příklad ukazuje, jak zpracovat výjimku, která je vrácena back-end:</span><span class="sxs-lookup"><span data-stu-id="e422b-463">The following example shows how to handle an exception that is returned by the backend:</span></span>

```
private async void InsertTodoItem(TodoItem todoItem)
{
    // This code inserts a new TodoItem into the database. When the operation completes
    // and App Service has assigned an Id, the item is added to the CollectionView
    try
    {
        await todoTable.InsertAsync(todoItem);
        items.Add(todoItem);
    }
    catch (MobileServiceInvalidOperationException e)
    {
        // Handle error
    }
}
```

<span data-ttu-id="e422b-464">Další příklad práci s chybové stavy lze nalézt v [ukázkové soubory aplikací Mobile].</span><span class="sxs-lookup"><span data-stu-id="e422b-464">Another example of dealing with error conditions can be found in the [Mobile Apps Files Sample].</span></span> <span data-ttu-id="e422b-465">[LoggingHandler] příklad poskytuje obslužnou rutinu delegáta protokolování do protokolu požadavky prováděné na back-end.</span><span class="sxs-lookup"><span data-stu-id="e422b-465">The [LoggingHandler] example provides a logging delegate handler to log the requests being made to the backend.</span></span>

### <span data-ttu-id="e422b-466"><a name="headers"></a>Postupy: přizpůsobení hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="e422b-466"><a name="headers"></a>How to: Customize request headers</span></span>
<span data-ttu-id="e422b-467">Pro podporu váš scénář konkrétní aplikaci, může být potřeba přizpůsobit komunikaci s back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e422b-467">To support your specific app scenario, you might need to customize communication with the Mobile App backend.</span></span> <span data-ttu-id="e422b-468">Můžete například chtít přidat vlastní hlavičku do každého odchozí požadavku nebo dokonce změnit stavový kód odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e422b-468">For example, you may want to add a custom header to every outgoing request or even change responses status codes.</span></span> <span data-ttu-id="e422b-469">Můžete použít vlastní [DelegatingHandler], jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="e422b-469">You can use a custom [DelegatingHandler], as in the following example:</span></span>

```
public async Task CallClientWithHandler()
{
    MobileServiceClient client = new MobileServiceClient("AppUrl", new MyHandler());
    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
    var newItem = new TodoItem { Text = "Hello world", Complete = false };
    await todoTable.InsertAsync(newItem);
}

public class MyHandler : DelegatingHandler
{
    protected override async Task<HttpResponseMessage>
        SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
    {
        // Change the request-side here based on the HttpRequestMessage
        request.Headers.Add("x-my-header", "my value");

        // Do the request
        var response = await base.SendAsync(request, cancellationToken);

        // Change the response-side here based on the HttpResponseMessage

        // Return the modified response
        return response;
    }
}
```


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

<span data-ttu-id="e422b-470">[přidání ověřování do aplikace]: app-service-mobile-windows-store-dotnet-get-started-users.md</span><span class="sxs-lookup"><span data-stu-id="e422b-470">[Add authentication to your app]: app-service-mobile-windows-store-dotnet-get-started-users.md</span></span>
<span data-ttu-id="e422b-471">[Offline synchronizací dat v Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="e422b-471">[Offline Data Sync in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span></span>
<span data-ttu-id="e422b-472">[přidat nabízená oznámení do vaší aplikace]: app-service-mobile-windows-store-dotnet-get-started-push.md</span><span class="sxs-lookup"><span data-stu-id="e422b-472">[Add push notifications to your app]: app-service-mobile-windows-store-dotnet-get-started-push.md</span></span>
<span data-ttu-id="e422b-473">[svou aplikaci zaregistrovat pro používat přihlašovací údaje účtu Microsoft]: app-service-mobile-how-to-configure-microsoft-authentication.md</span><span class="sxs-lookup"><span data-stu-id="e422b-473">[Register your app to use a Microsoft account login]: app-service-mobile-how-to-configure-microsoft-authentication.md</span></span>
<span data-ttu-id="e422b-474">[jak nakonfigurovat App Service pro přihlášení služby Active Directory]: app-service-mobile-how-to-configure-active-directory-authentication.md</span><span class="sxs-lookup"><span data-stu-id="e422b-474">[How to configure App Service for Active Directory login]: app-service-mobile-how-to-configure-active-directory-authentication.md</span></span>

<!-- Microsoft URLs. -->
<span data-ttu-id="e422b-475">[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-475">[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-476">[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-476">[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-477">[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-477">[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-478">[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-478">[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-479">[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-479">[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-480">[Funkce GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-480">[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-481">[vytvoří odkaz na tabulku netypové]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-481">[creates a reference to an untyped table]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-482">[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-482">[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-483">[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-483">[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-484">[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-484">[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-485">[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-485">[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-486">[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-486">[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-487">[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-487">[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-488">[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-488">[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-489">[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-489">[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-490">[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-490">[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-491">[trvat]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-491">[Take]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-492">[vyberte]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-492">[Select]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-493">[přeskočit]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-493">[Skip]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-494">[metod UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-494">[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx</span></span>
<span data-ttu-id="e422b-495">[ID uživatele]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-495">[UserID]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-496">[Kde]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-496">[Where]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx</span></span>
<span data-ttu-id="e422b-497">[portál Azure]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="e422b-497">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="e422b-498">[portál Azure classic]: https://manage.windowsazure.com/</span><span class="sxs-lookup"><span data-stu-id="e422b-498">[Azure classic portal]: https://manage.windowsazure.com/</span></span>
<span data-ttu-id="e422b-499">[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-499">[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx</span></span>
<span data-ttu-id="e422b-500">[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-500">[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx</span></span>
<span data-ttu-id="e422b-501">[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-501">[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx</span></span>
<span data-ttu-id="e422b-502">[Windows Dev Center]: https://dev.windows.com/en-us/overview</span><span class="sxs-lookup"><span data-stu-id="e422b-502">[Windows Dev Center]: https://dev.windows.com/en-us/overview</span></span>
<span data-ttu-id="e422b-503">[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-503">[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx</span></span>
<span data-ttu-id="e422b-504">[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-504">[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx</span></span>
<span data-ttu-id="e422b-505">[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-505">[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx</span></span>
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
<span data-ttu-id="e422b-506">[rozhraní API centra oznámení]: https://msdn.microsoft.com/library/azure/dn495101.aspx</span><span class="sxs-lookup"><span data-stu-id="e422b-506">[Notification Hubs APIs]: https://msdn.microsoft.com/library/azure/dn495101.aspx</span></span>
<span data-ttu-id="e422b-507">[ukázkové soubory aplikací Mobile]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files</span><span class="sxs-lookup"><span data-stu-id="e422b-507">[Mobile Apps Files Sample]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files</span></span>
<span data-ttu-id="e422b-508">[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63</span><span class="sxs-lookup"><span data-stu-id="e422b-508">[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63</span></span>

<!-- External URLs -->
<span data-ttu-id="e422b-509">[OData v3 dokumentaci]: http://www.odata.org/documentation/odata-version-3-0/</span><span class="sxs-lookup"><span data-stu-id="e422b-509">[OData v3 Documentation]: http://www.odata.org/documentation/odata-version-3-0/</span></span>
<span data-ttu-id="e422b-510">[Fiddler]: http://www.telerik.com/fiddler</span><span class="sxs-lookup"><span data-stu-id="e422b-510">[Fiddler]: http://www.telerik.com/fiddler</span></span>
<span data-ttu-id="e422b-511">[Json.NET]: http://www.newtonsoft.com/json</span><span class="sxs-lookup"><span data-stu-id="e422b-511">[Json.NET]: http://www.newtonsoft.com/json</span></span>
<span data-ttu-id="e422b-512">[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/</span><span class="sxs-lookup"><span data-stu-id="e422b-512">[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/</span></span>
<span data-ttu-id="e422b-513">[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments</span><span class="sxs-lookup"><span data-stu-id="e422b-513">[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments</span></span>
[ContosoMoments photo sharing sample]: https://github.com/azure-appservice-samples/ContosoMoments
