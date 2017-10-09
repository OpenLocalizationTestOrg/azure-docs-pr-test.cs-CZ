---
title: "Klientská knihovna pro spravované aaaWorking s hello App Service Mobile Apps (Windows | Microsoft Docs"
description: "Zjistěte, jak toouse klient .NET pro Azure App Service Mobile Apps s aplikacemi pro Windows a Xamarin."
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
ms.openlocfilehash: b056e606b19406398f5b6faabb0931ad651125e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-managed-client-for-azure-mobile-apps"></a><span data-ttu-id="cb42c-103">Jak toouse hello spravovat klienta pro Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="cb42c-103">How toouse hello managed client for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a><span data-ttu-id="cb42c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="cb42c-104">Overview</span></span>
<span data-ttu-id="cb42c-105">Tento průvodce vám ukáže, jak tooperform běžné scénáře s využitím hello spravované klientské knihovny pro aplikace Azure App Service mobilní aplikace pro Windows a Xamarin.</span><span class="sxs-lookup"><span data-stu-id="cb42c-105">This guide shows you how tooperform common scenarios using hello managed client library for Azure App Service Mobile Apps for Windows and Xamarin apps.</span></span> <span data-ttu-id="cb42c-106">Pokud jste nové aplikace tooMobile, měli byste zvážit první dokončuje hello [rychlý start Azure Mobile Apps] [ 1] kurzu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-106">If you are new tooMobile Apps, you should consider first completing hello [Azure Mobile Apps quickstart][1] tutorial.</span></span> <span data-ttu-id="cb42c-107">V této příručce se zaměříme na hello spravované klientské sady SDK.</span><span class="sxs-lookup"><span data-stu-id="cb42c-107">In this guide, we focus on hello client-side managed SDK.</span></span> <span data-ttu-id="cb42c-108">Další informace o hello serverové sady SDK pro Mobile Apps, naleznete v dokumentaci k hello hello toolearn [.NET serveru SDK] [ 2] nebo [Node.js Server SDK] [ 3].</span><span class="sxs-lookup"><span data-stu-id="cb42c-108">toolearn more about hello server-side SDKs for Mobile Apps, see hello documentation for hello [.NET Server SDK][2] or the [Node.js Server SDK][3].</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="cb42c-109">Referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="cb42c-109">Reference documentation</span></span>
<span data-ttu-id="cb42c-110">Hello referenční dokumentaci k nástroji pro hello klienta SDK se nachází zde: [Azure Mobile Apps .NET – referenční informace klienta][4].</span><span class="sxs-lookup"><span data-stu-id="cb42c-110">hello reference documentation for hello client SDK is located here: [Azure Mobile Apps .NET client reference][4].</span></span>
<span data-ttu-id="cb42c-111">Můžete také získat několik ukázky klienta v hello [úložiště GitHub Azure-Samples][5].</span><span class="sxs-lookup"><span data-stu-id="cb42c-111">You can also find several client samples in hello [Azure-Samples GitHub repository][5].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="cb42c-112">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="cb42c-112">Supported Platforms</span></span>
<span data-ttu-id="cb42c-113">Hello platformy .NET podporuje hello následující platformy:</span><span class="sxs-lookup"><span data-stu-id="cb42c-113">hello .NET Platform supports hello following platforms:</span></span>

* <span data-ttu-id="cb42c-114">Xamarin Android verze pro rozhraní API 19 až 24 (KitKat prostřednictvím cukrovinkách typu nugát)</span><span class="sxs-lookup"><span data-stu-id="cb42c-114">Xamarin Android releases for API 19 through 24 (KitKat through Nougat)</span></span>
* <span data-ttu-id="cb42c-115">Xamarin iOS verze pro iOS verze 8.0 a novější</span><span class="sxs-lookup"><span data-stu-id="cb42c-115">Xamarin iOS releases for iOS versions 8.0 and later</span></span>
* <span data-ttu-id="cb42c-116">Univerzální platforma pro Windows</span><span class="sxs-lookup"><span data-stu-id="cb42c-116">Universal Windows Platform</span></span>
* <span data-ttu-id="cb42c-117">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="cb42c-117">Windows Phone 8.1</span></span>
* <span data-ttu-id="cb42c-118">Windows Phone 8.0, s výjimkou aplikace Silverlight</span><span class="sxs-lookup"><span data-stu-id="cb42c-118">Windows Phone 8.0 except for Silverlight applications</span></span>

<span data-ttu-id="cb42c-119">ověřování "serveru pohybu" Hello používá webové zobrazení pro hello uvedené uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cb42c-119">hello "server-flow" authentication uses a WebView for hello presented UI.</span></span>  <span data-ttu-id="cb42c-120">Pokud hello zařízení není možné toopresent webové zobrazení uživatelského rozhraní, jsou potřeba jiných metod ověřování.</span><span class="sxs-lookup"><span data-stu-id="cb42c-120">If hello device is not able toopresent a WebView UI, then other methods of authentication are needed.</span></span>  <span data-ttu-id="cb42c-121">Tato sada SDK není proto vhodný pro sledování type nebo podobně jako zařízení s omezeným přístupem.</span><span class="sxs-lookup"><span data-stu-id="cb42c-121">This SDK is thus not suitable for Watch-type or similarly restricted devices.</span></span>

## <span data-ttu-id="cb42c-122"><a name="setup"></a>Instalační program a požadavky</span><span class="sxs-lookup"><span data-stu-id="cb42c-122"><a name="setup"></a>Setup and Prerequisites</span></span>
<span data-ttu-id="cb42c-123">Předpokládáme, že jste již vytvořili a publikování projektu back-end mobilní aplikace, která zahrnuje nejméně jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="cb42c-123">We assume that you have already created and published your Mobile App backend project, which includes at least one table.</span></span>  <span data-ttu-id="cb42c-124">V kódu hello použitým v tomto tématu, je název tabulky hello `TodoItem` a má hello následující sloupce: `Id`, `Text`, a `Complete`.</span><span class="sxs-lookup"><span data-stu-id="cb42c-124">In hello code used in this topic, hello table is named `TodoItem` and it has hello following columns: `Id`, `Text`, and `Complete`.</span></span> <span data-ttu-id="cb42c-125">Tato tabulka je hello stejné tabulky vytvořit při dokončení [rychlý start Azure Mobile Apps][1].</span><span class="sxs-lookup"><span data-stu-id="cb42c-125">This table is hello same table created when you complete the [Azure Mobile Apps quickstart][1].</span></span>

<span data-ttu-id="cb42c-126">Hello odpovídající typu klienta v jazyce C# je typ hello následující třídy:</span><span class="sxs-lookup"><span data-stu-id="cb42c-126">hello corresponding typed client-side type in C# is hello following class:</span></span>

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

<span data-ttu-id="cb42c-127">Hello [JsonPropertyAttribute] [ 6] je použité toodefine hello *PropertyName* mapování mezi hello klienta pole a pole tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-127">hello [JsonPropertyAttribute][6] is used toodefine hello *PropertyName* mapping between hello client field and hello table field.</span></span>

<span data-ttu-id="cb42c-128">toolearn jak toocreate tabulky v váš back-end mobilní aplikace, najdete v části hello [.NET serveru SDK tématu] [ 7] nebo hello [Node.js Server SDK tématu] [ 8] .</span><span class="sxs-lookup"><span data-stu-id="cb42c-128">toolearn how toocreate tables in your Mobile Apps backend, see hello [.NET Server SDK topic][7] or hello [Node.js Server SDK topic][8].</span></span> <span data-ttu-id="cb42c-129">Pokud jste vytvořili váš back-end mobilní aplikace v hello Azure pomocí portálu hello rychlý start, můžete také použít hello **snadno tabulky** nastavení v hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="cb42c-129">If you created your Mobile App backend in hello Azure portal using hello QuickStart, you can also use hello **Easy tables** setting in hello [Azure portal].</span></span>

### <a name="how-to-install-hello-managed-client-sdk-package"></a><span data-ttu-id="cb42c-130">Postupy: instalace hello spravované balíček klienta SDK</span><span class="sxs-lookup"><span data-stu-id="cb42c-130">How to: Install hello managed client SDK package</span></span>
<span data-ttu-id="cb42c-131">Použijte jednu z následujících metod tooinstall hello hello spravovaná balíček klienta SDK pro Mobile Apps z [NuGet][9]:</span><span class="sxs-lookup"><span data-stu-id="cb42c-131">Use one of hello following methods tooinstall hello managed client SDK package for Mobile Apps from [NuGet][9]:</span></span>

* <span data-ttu-id="cb42c-132">**Visual Studio** klikněte pravým tlačítkem na projekt, klikněte na tlačítko **spravovat balíčky NuGet**, vyhledejte hello `Microsoft.Azure.Mobile.Client` balíček a potom klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="cb42c-132">**Visual Studio** Right-click your project, click **Manage NuGet Packages**, search for hello `Microsoft.Azure.Mobile.Client` package, then click **Install**.</span></span>
* <span data-ttu-id="cb42c-133">**Xamarin Studio** klikněte pravým tlačítkem na projekt, klikněte na tlačítko **přidat** > **přidání balíčků NuGet**, vyhledejte hello `Microsoft.Azure.Mobile.Client `balíček a potom klikněte na tlačítko **přidat Balíček**.</span><span class="sxs-lookup"><span data-stu-id="cb42c-133">**Xamarin Studio** Right-click your project, click **Add** > **Add NuGet Packages**, search for hello `Microsoft.Azure.Mobile.Client `package, and then click **Add Package**.</span></span>

<span data-ttu-id="cb42c-134">V souboru hlavní aktivitu, mějte na paměti následující hello tooadd **pomocí** příkaz:</span><span class="sxs-lookup"><span data-stu-id="cb42c-134">In your main activity file, remember tooadd hello following **using** statement:</span></span>

```
using Microsoft.WindowsAzure.MobileServices;
```

### <span data-ttu-id="cb42c-135"><a name="symbolsource"></a>Postupy: práce s ladicími symboly v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cb42c-135"><a name="symbolsource"></a>How to: Work with debug symbols in Visual Studio</span></span>
<span data-ttu-id="cb42c-136">Hello symboly pro obor názvů Microsoft.Azure.Mobile hello jsou k dispozici na [SymbolSource][10].</span><span class="sxs-lookup"><span data-stu-id="cb42c-136">hello symbols for hello Microsoft.Azure.Mobile namespace are available on [SymbolSource][10].</span></span>  <span data-ttu-id="cb42c-137">Odkazovat toothe [SymbolSource pokyny] [ 11] toointegrate SymbolSource pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cb42c-137">Refer toothe [SymbolSource instructions][11] toointegrate SymbolSource with Visual Studio.</span></span>

## <span data-ttu-id="cb42c-138"><a name="create-client"></a>Vytvoření klienta Mobile Apps hello</span><span class="sxs-lookup"><span data-stu-id="cb42c-138"><a name="create-client"></a>Create hello Mobile Apps client</span></span>
<span data-ttu-id="cb42c-139">Hello následující kód vytvoří hello [MobileServiceClient] [ 12] objekt, který je použité tooaccess váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb42c-139">hello following code creates hello [MobileServiceClient][12] object that is used tooaccess your Mobile App backend.</span></span>

```
var client = new MobileServiceClient("MOBILE_APP_URL");
```

<span data-ttu-id="cb42c-140">V předchozích kód hello, nahraďte `MOBILE_APP_URL` s adresou URL hello back-end mobilní aplikace hello, který se vyskytuje v okně pro váš back-end mobilní aplikace v hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="cb42c-140">In hello preceding code, replace `MOBILE_APP_URL` with hello URL of hello Mobile App backend, which is found in the blade for your Mobile App backend in hello [Azure portal].</span></span> <span data-ttu-id="cb42c-141">objekt MobileServiceClient Hello by měl být typu singleton.</span><span class="sxs-lookup"><span data-stu-id="cb42c-141">hello MobileServiceClient object should be a singleton.</span></span>

## <a name="work-with-tables"></a><span data-ttu-id="cb42c-142">Práce s tabulkami</span><span class="sxs-lookup"><span data-stu-id="cb42c-142">Work with Tables</span></span>
<span data-ttu-id="cb42c-143">Následující část podrobně popisuje, jak Hello toosearch a načtení záznamů a úprava hello dat v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-143">hello following section details how toosearch and retrieve records and modify hello data within hello table.</span></span>  <span data-ttu-id="cb42c-144">jsou popsané Hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="cb42c-144">hello following topics are covered:</span></span>

* [<span data-ttu-id="cb42c-145">Vytvořit odkaz na tabulku</span><span class="sxs-lookup"><span data-stu-id="cb42c-145">Create a table reference</span></span>](#instantiating)
* [<span data-ttu-id="cb42c-146">Dotaz na data</span><span class="sxs-lookup"><span data-stu-id="cb42c-146">Query data</span></span>](#querying)
* [<span data-ttu-id="cb42c-147">Filtr vrátil data</span><span class="sxs-lookup"><span data-stu-id="cb42c-147">Filter returned data</span></span>](#filtering)
* [<span data-ttu-id="cb42c-148">Řazení vrátil data</span><span class="sxs-lookup"><span data-stu-id="cb42c-148">Sort returned data</span></span>](#sorting)
* [<span data-ttu-id="cb42c-149">Vrátit data na stránkách</span><span class="sxs-lookup"><span data-stu-id="cb42c-149">Return data in pages</span></span>](#paging)
* [<span data-ttu-id="cb42c-150">Vyberte konkrétní sloupce</span><span class="sxs-lookup"><span data-stu-id="cb42c-150">Select specific columns</span></span>](#selecting)
* [<span data-ttu-id="cb42c-151">Vyhledat záznam podle Id</span><span class="sxs-lookup"><span data-stu-id="cb42c-151">Look up a record by Id</span></span>](#lookingup)
* [<span data-ttu-id="cb42c-152">Práci s netypové dotazy</span><span class="sxs-lookup"><span data-stu-id="cb42c-152">Dealing with untyped queries</span></span>](#untypedqueries)
* [<span data-ttu-id="cb42c-153">Vkládání dat</span><span class="sxs-lookup"><span data-stu-id="cb42c-153">Inserting data</span></span>](#inserting)
* [<span data-ttu-id="cb42c-154">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="cb42c-154">Updating data</span></span>](#updating)
* [<span data-ttu-id="cb42c-155">Odstraňování dat</span><span class="sxs-lookup"><span data-stu-id="cb42c-155">Deleting data</span></span>](#deleting)
* [<span data-ttu-id="cb42c-156">Řešení konfliktů a optimistickou metodu souběžného zpracování</span><span class="sxs-lookup"><span data-stu-id="cb42c-156">Conflict Resolution and Optimistic Concurrency</span></span>](#optimisticconcurrency)
* [<span data-ttu-id="cb42c-157">Vazba tooa uživatelské rozhraní Windows</span><span class="sxs-lookup"><span data-stu-id="cb42c-157">Binding tooa Windows User Interface</span></span>](#binding)
* [<span data-ttu-id="cb42c-158">Změna hello velikost stránky</span><span class="sxs-lookup"><span data-stu-id="cb42c-158">Changing hello Page Size</span></span>](#pagesize)

### <span data-ttu-id="cb42c-159"><a name="instantiating"></a>Postupy: vytvoření odkaz na tabulku</span><span class="sxs-lookup"><span data-stu-id="cb42c-159"><a name="instantiating"></a>How to: Create a table reference</span></span>
<span data-ttu-id="cb42c-160">Všechny hello kód, který přistupuje k nebo upravuje data v tabulce back-end volání funkce na hello `MobileServiceTable` objektu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-160">All hello code that accesses or modifies data in a backend table calls functions on hello `MobileServiceTable` object.</span></span> <span data-ttu-id="cb42c-161">Získejte odkaz na tabulku toohello volání hello [Funkce GetTable] metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cb42c-161">Obtain a reference toohello table by calling hello [GetTable] method, as follows:</span></span>

```
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

<span data-ttu-id="cb42c-162">Hello vrátit objekt používá model typovaná serializace hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-162">hello returned object uses hello typed serialization model.</span></span> <span data-ttu-id="cb42c-163">Model netypové serializace je také podporována.</span><span class="sxs-lookup"><span data-stu-id="cb42c-163">An untyped serialization model is also supported.</span></span> <span data-ttu-id="cb42c-164">Následující příklad [vytvoří odkaz na tabulku netypové tooan]:</span><span class="sxs-lookup"><span data-stu-id="cb42c-164">The following example [creates a reference tooan untyped table]:</span></span>

```
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

<span data-ttu-id="cb42c-165">V netypové dotazy je nutné zadat hello základní řetězec dotazu OData.</span><span class="sxs-lookup"><span data-stu-id="cb42c-165">In untyped queries, you must specify hello underlying OData query string.</span></span>

### <span data-ttu-id="cb42c-166"><a name="querying"></a>Postupy: dotaz na data z mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="cb42c-166"><a name="querying"></a>How to: Query data from your Mobile App</span></span>
<span data-ttu-id="cb42c-167">Tato část popisuje, jak tooissue dotazuje toohello mobilní back-end aplikace, která zahrnuje hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="cb42c-167">This section describes how tooissue queries toohello Mobile App backend, which includes hello following functionality:</span></span>

* [<span data-ttu-id="cb42c-168">Filtr vrátil data</span><span class="sxs-lookup"><span data-stu-id="cb42c-168">Filter returned data</span></span>](#filtering)
* [<span data-ttu-id="cb42c-169">Řazení vrátil data</span><span class="sxs-lookup"><span data-stu-id="cb42c-169">Sort returned data</span></span>](#sorting)
* [<span data-ttu-id="cb42c-170">Vrátit data na stránkách</span><span class="sxs-lookup"><span data-stu-id="cb42c-170">Return data in pages</span></span>](#paging)
* [<span data-ttu-id="cb42c-171">Vyberte konkrétní sloupce</span><span class="sxs-lookup"><span data-stu-id="cb42c-171">Select specific columns</span></span>](#selecting)
* [<span data-ttu-id="cb42c-172">Vyhledávání dat podle ID</span><span class="sxs-lookup"><span data-stu-id="cb42c-172">Look up data by ID</span></span>](#lookingup)

> [!NOTE]
> <span data-ttu-id="cb42c-173">Velikost stránky řízené serveru je vynucené tooprevent všechny řádky nebyl vrácen.</span><span class="sxs-lookup"><span data-stu-id="cb42c-173">A server-driven page size is enforced tooprevent all rows from being returned.</span></span>  <span data-ttu-id="cb42c-174">Stránkování zachová výchozí požadavky pro velké sady dat z mělo negativní dopad na hello služby.</span><span class="sxs-lookup"><span data-stu-id="cb42c-174">Paging keeps default requests for large data sets from negatively impacting hello service.</span></span>  <span data-ttu-id="cb42c-175">tooreturn více než 50 řádky, použijte hello `Skip` a `Take` metoda, jak je popsáno v [vracet data na stránkách](#paging).</span><span class="sxs-lookup"><span data-stu-id="cb42c-175">tooreturn more than 50 rows, use hello `Skip` and `Take` method, as described in [Return data in pages](#paging).</span></span>

### <span data-ttu-id="cb42c-176"><a name="filtering"></a>Postupy: Filtr nevrátil data</span><span class="sxs-lookup"><span data-stu-id="cb42c-176"><a name="filtering"></a>How to: Filter returned data</span></span>
<span data-ttu-id="cb42c-177">Hello následující kódu ukazuje, jak toofilter dat včetně `Where` klauzuli v dotazu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-177">hello following code illustrates how toofilter data by including a `Where` clause in a query.</span></span> <span data-ttu-id="cb42c-178">Vrátí všechny položky z `todoTable` jejichž `Complete` vlastnost je rovna příliš`false`.</span><span class="sxs-lookup"><span data-stu-id="cb42c-178">It returns all items from `todoTable` whose `Complete` property is equal too`false`.</span></span> <span data-ttu-id="cb42c-179">Hello [kde] funkce se vztahuje řádek filtrování predikátu k hello dotaz hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="cb42c-179">hello [Where] function applies a row filtering predicate to hello query against hello table.</span></span>

```
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

<span data-ttu-id="cb42c-180">Můžete zobrazit hello URI hello požadavek odeslaný toohello back-end pomocí zpráv kontroly software, například nástroje pro vývojáře prohlížeče nebo [Fiddler].</span><span class="sxs-lookup"><span data-stu-id="cb42c-180">You can view hello URI of hello request sent toohello backend by using message inspection software, such as browser developer tools or [Fiddler].</span></span> <span data-ttu-id="cb42c-181">Pokud se podíváte na URI požadavku hello, Všimněte si, že je řetězec dotazu hello upravit:</span><span class="sxs-lookup"><span data-stu-id="cb42c-181">If you look at hello request URI, notice that hello query string is modified:</span></span>

```
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

<span data-ttu-id="cb42c-182">Tento požadavek OData je přeložen do dotazu SQL hello Server SDK:</span><span class="sxs-lookup"><span data-stu-id="cb42c-182">This OData request is translated into an SQL query by hello Server SDK:</span></span>

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

<span data-ttu-id="cb42c-183">Hello funkce, která je předána toohello `Where` metoda může mít libovolný počet podmínky.</span><span class="sxs-lookup"><span data-stu-id="cb42c-183">hello function that is passed toohello `Where` method can have an arbitrary number of conditions.</span></span>

```
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

<span data-ttu-id="cb42c-184">V tomto příkladu by přeložen hello Server SDK do dotazu SQL:</span><span class="sxs-lookup"><span data-stu-id="cb42c-184">This example would be translated into an SQL query by hello Server SDK:</span></span>

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

<span data-ttu-id="cb42c-185">Tento dotaz taky dají rozdělit do více klauzulí:</span><span class="sxs-lookup"><span data-stu-id="cb42c-185">This query can also be split into multiple clauses:</span></span>

```
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

<span data-ttu-id="cb42c-186">Hello dvě metody jsou ekvivalentní a může je použít zcela zaměnitelným významem.</span><span class="sxs-lookup"><span data-stu-id="cb42c-186">hello two methods are equivalent and may be used interchangeably.</span></span>  <span data-ttu-id="cb42c-187">Hello bývalé možnost&mdash;z zřetězení více predikáty v jednom dotazu&mdash;je kompaktnější a doporučené.</span><span class="sxs-lookup"><span data-stu-id="cb42c-187">hello former option&mdash;of concatenating multiple predicates in one query&mdash;is more compact and recommended.</span></span>

<span data-ttu-id="cb42c-188">Hello `Where` klauzule podporuje operace, které se přeložit na podmnožinu hello OData.</span><span class="sxs-lookup"><span data-stu-id="cb42c-188">hello `Where` clause supports operations that be translated into hello OData subset.</span></span> <span data-ttu-id="cb42c-189">Operace zahrnují:</span><span class="sxs-lookup"><span data-stu-id="cb42c-189">Operations include:</span></span>

* <span data-ttu-id="cb42c-190">Relační operátory (==,! =, <, < =, >, > =),</span><span class="sxs-lookup"><span data-stu-id="cb42c-190">Relational operators (==, !=, <, <=, >, >=),</span></span>
* <span data-ttu-id="cb42c-191">Aritmetické operátory (+, -, /, *, %),</span><span class="sxs-lookup"><span data-stu-id="cb42c-191">Arithmetic operators (+, -, /, *, %),</span></span>
* <span data-ttu-id="cb42c-192">Číslo přesnost (Math.Floor, Math.Ceiling)</span><span class="sxs-lookup"><span data-stu-id="cb42c-192">Number precision (Math.Floor, Math.Ceiling),</span></span>
* <span data-ttu-id="cb42c-193">Funkce řetězce (délka, Substring, nahraďte, IndexOf, StartsWith, EndsWith),</span><span class="sxs-lookup"><span data-stu-id="cb42c-193">String functions (Length, Substring, Replace, IndexOf, StartsWith, EndsWith),</span></span>
* <span data-ttu-id="cb42c-194">Vlastnosti kalendářních dat (rok, měsíc, den, hodinu, minutu, sekundu),</span><span class="sxs-lookup"><span data-stu-id="cb42c-194">Date properties (Year, Month, Day, Hour, Minute, Second),</span></span>
* <span data-ttu-id="cb42c-195">Přístup k vlastnosti objektu, a</span><span class="sxs-lookup"><span data-stu-id="cb42c-195">Access properties of an object, and</span></span>
* <span data-ttu-id="cb42c-196">Výrazy kombinování kteroukoli z těchto operací.</span><span class="sxs-lookup"><span data-stu-id="cb42c-196">Expressions combining any of these operations.</span></span>

<span data-ttu-id="cb42c-197">Pokud vzhledem k tomu, co hello serveru SDK podporuje, můžete zvážit hello [OData v3 dokumentaci].</span><span class="sxs-lookup"><span data-stu-id="cb42c-197">When considering what hello Server SDK supports, you can consider hello [OData v3 Documentation].</span></span>

### <span data-ttu-id="cb42c-198"><a name="sorting"></a>Postupy: řazení vrátil data</span><span class="sxs-lookup"><span data-stu-id="cb42c-198"><a name="sorting"></a>How to: Sort returned data</span></span>
<span data-ttu-id="cb42c-199">Hello následující kódu ukazuje, jak toosort dat včetně [OrderBy] nebo [OrderByDescending] funkce v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-199">hello following code illustrates how toosort data by including an [OrderBy] or [OrderByDescending] function in hello query.</span></span> <span data-ttu-id="cb42c-200">Vrátí položky z `todoTable` seřadit vzestupně podle hello `Text` pole.</span><span class="sxs-lookup"><span data-stu-id="cb42c-200">It returns items from `todoTable` sorted ascending by hello `Text` field.</span></span>

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

### <span data-ttu-id="cb42c-201"><a name="paging"></a>Postupy: vracet data na stránkách</span><span class="sxs-lookup"><span data-stu-id="cb42c-201"><a name="paging"></a>How to: Return data in pages</span></span>
<span data-ttu-id="cb42c-202">Ve výchozím nastavení back-end hello vrátí hello pouze prvních 50 řádky.</span><span class="sxs-lookup"><span data-stu-id="cb42c-202">By default, hello backend returns only hello first 50 rows.</span></span> <span data-ttu-id="cb42c-203">Můžete zvýšit hello počet vrácených řádků ve volání hello [trvat] metoda.</span><span class="sxs-lookup"><span data-stu-id="cb42c-203">You can increase hello number of returned rows by calling hello [Take] method.</span></span> <span data-ttu-id="cb42c-204">Použití `Take` společně s hello [přeskočit] metoda toorequest konkrétní "stránky" hello celkový sady dat vrácených dotazem hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-204">Use `Take` along with hello [Skip] method toorequest a specific "page" of hello total dataset returned by hello query.</span></span> <span data-ttu-id="cb42c-205">Hello následující dotaz, při spuštění, vrátí hello první tři položky v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-205">hello following query, when executed, returns hello top three items in hello table.</span></span>

```
// Define a filtered query that returns hello top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

<span data-ttu-id="cb42c-206">Hello následující přeskočí revidovaný dotazu hello první tři výsledky a vrátí hello následující tři výsledky.</span><span class="sxs-lookup"><span data-stu-id="cb42c-206">hello following revised query skips hello first three results and returns hello next three results.</span></span> <span data-ttu-id="cb42c-207">Tento dotaz vytvoří hello druhý "stránka" dat, kde je velikost stránky hello tři položky.</span><span class="sxs-lookup"><span data-stu-id="cb42c-207">This query produces hello second "page" of data, where hello page size is three items.</span></span>

```
// Define a filtered query that skips hello top 3 items and returns hello next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

<span data-ttu-id="cb42c-208">Hello [IncludeTotalCount] metoda požadavky hello celkový počet pro *všechny* hello záznamy, které by byly vráceny, ignoruje všechny klauzuli stránkování/limit zadat:</span><span class="sxs-lookup"><span data-stu-id="cb42c-208">hello [IncludeTotalCount] method requests hello total count for *all* hello records that would have been returned, ignoring any paging/limit clause specified:</span></span>

```
query = query.IncludeTotalCount();
```

<span data-ttu-id="cb42c-209">Ve skutečných aplikaci můžete použít dotazy podobné toohello předcházející příklad s na pager ovládací prvek nebo srovnatelný uživatelského rozhraní navigace mezi stránkami.</span><span class="sxs-lookup"><span data-stu-id="cb42c-209">In a real world app, you can use queries similar toohello preceding example with a pager control or comparable UI to navigate between pages.</span></span>

> [!NOTE]
> <span data-ttu-id="cb42c-210">limit 50 řádek toooverride hello v back-end mobilní aplikace, musíte také použít hello [EnableQueryAttribute] toohello veřejných GET – metoda a zadejte hello stránkování.</span><span class="sxs-lookup"><span data-stu-id="cb42c-210">toooverride hello 50-row limit in a Mobile App backend, you must also apply hello [EnableQueryAttribute] toohello public GET method and specify hello paging behavior.</span></span> <span data-ttu-id="cb42c-211">Když metoda použitá toohello, hello následující nastaví too1000 maximální počet vrácených řádků:</span><span class="sxs-lookup"><span data-stu-id="cb42c-211">When applied toohello method, hello following sets the maximum returned rows too1000:</span></span>
>
> `[EnableQuery(MaxTop=1000)]`


### <span data-ttu-id="cb42c-212"><a name="selecting"></a>Postupy: výběr konkrétní sloupců</span><span class="sxs-lookup"><span data-stu-id="cb42c-212"><a name="selecting"></a>How to: Select specific columns</span></span>
<span data-ttu-id="cb42c-213">Můžete zadat, která sada tooinclude vlastnosti v hello výsledků přidáním [vyberte] klauzule tooyour dotazu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-213">You can specify which set of properties tooinclude in hello results by adding a [Select] clause tooyour query.</span></span> <span data-ttu-id="cb42c-214">Například hello následující kód ukazuje, jak tooselect právě jedno pole a také jak tooselect a formátování více polí:</span><span class="sxs-lookup"><span data-stu-id="cb42c-214">For example, hello following code shows how tooselect just one field and also how tooselect and format multiple fields:</span></span>

```
// Select one field -- just hello Text
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

<span data-ttu-id="cb42c-215">Všechny hello funkce popsané, pokud jsou sčítání, takže jsme můžete zachovat řetězení je.</span><span class="sxs-lookup"><span data-stu-id="cb42c-215">All hello functions described so far are additive, so we can keep chaining them.</span></span> <span data-ttu-id="cb42c-216">Každý zřetězené volání ovlivňuje více hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-216">Each chained call affects more of hello query.</span></span> <span data-ttu-id="cb42c-217">Další příklad:</span><span class="sxs-lookup"><span data-stu-id="cb42c-217">One more example:</span></span>

```
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <span data-ttu-id="cb42c-218"><a name="lookingup"></a>Postupy: vyhledávání dat podle ID</span><span class="sxs-lookup"><span data-stu-id="cb42c-218"><a name="lookingup"></a>How to: Look up data by ID</span></span>
<span data-ttu-id="cb42c-219">Hello [LookupAsync] funkce lze použít toolook objektů z databáze hello s konkrétním ID.</span><span class="sxs-lookup"><span data-stu-id="cb42c-219">hello [LookupAsync] function can be used toolook up objects from hello database with a particular ID.</span></span>

```
// This query filters out hello item with hello ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <span data-ttu-id="cb42c-220"><a name="untypedqueries"></a>Postupy: provádění dotazů bez typu</span><span class="sxs-lookup"><span data-stu-id="cb42c-220"><a name="untypedqueries"></a>How to: Execute untyped queries</span></span>
<span data-ttu-id="cb42c-221">Při provádění dotazu pomocí objektu bez typu tabulky, je nutné explicitně zadat řetězec dotazu OData hello voláním [ReadAsync], jako v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="cb42c-221">When executing a query using an untyped table object, you must explicitly specify hello OData query string by calling [ReadAsync], as in hello following example:</span></span>

```
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

<span data-ttu-id="cb42c-222">Zobrazí zpět JSON hodnoty, které lze používat jako kontejner objektů.</span><span class="sxs-lookup"><span data-stu-id="cb42c-222">You get back JSON values that you can use like a property bag.</span></span> <span data-ttu-id="cb42c-223">Další informace o JToken a Newtonsoft Json.NET, najdete v části hello [Json.NET] lokality.</span><span class="sxs-lookup"><span data-stu-id="cb42c-223">For more information on JToken and Newtonsoft Json.NET, see hello [Json.NET] site.</span></span>

### <span data-ttu-id="cb42c-224"><a name="inserting"></a>Postupy: vložení dat do back-end mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="cb42c-224"><a name="inserting"></a>How to: Insert data into a Mobile App backend</span></span>
<span data-ttu-id="cb42c-225">Všechny typy klientů musí obsahovat člen s názvem **Id**, což je ve výchozím nastavení řetězec.</span><span class="sxs-lookup"><span data-stu-id="cb42c-225">All client types must contain a member named **Id**, which is by default a string.</span></span> <span data-ttu-id="cb42c-226">To **Id** je potřebná k provedení operace CRUD a pro offline synchronizace. hello následující kód ukazuje, jak toouse hello [InsertAsync] metoda tooinsert nové řádky do tabulky.</span><span class="sxs-lookup"><span data-stu-id="cb42c-226">This **Id** is required to perform CRUD operations and for offline sync. hello following code illustrates how toouse hello [InsertAsync] method tooinsert new rows into a table.</span></span> <span data-ttu-id="cb42c-227">Parametr Hello obsahuje toobe hello data vložit jako objekt .NET.</span><span class="sxs-lookup"><span data-stu-id="cb42c-227">hello parameter contains hello data toobe inserted as a .NET object.</span></span>

```
await todoTable.InsertAsync(todoItem);
```

<span data-ttu-id="cb42c-228">Pokud hodnotu jedinečné ID vlastní není součástí hello `todoItem` při vložení, je identifikátor GUID generovaný serverem hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-228">If a unique custom ID value is not included in hello `todoItem` during an insert, a GUID is generated by hello server.</span></span>
<span data-ttu-id="cb42c-229">Můžete načíst hello vygeneruje Id zkontrolováním hello objektu po volání hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-229">You can retrieve hello generated Id by inspecting hello object after hello call returns.</span></span>

<span data-ttu-id="cb42c-230">tooinsert netypová dat, může využít výhod Json.NET:</span><span class="sxs-lookup"><span data-stu-id="cb42c-230">tooinsert untyped data, you may take advantage of Json.NET:</span></span>

```
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

<span data-ttu-id="cb42c-231">Tady je příklad pomocí e-mailovou adresu jako id jedinečného řetězce:</span><span class="sxs-lookup"><span data-stu-id="cb42c-231">Here is an example using an email address as a unique string id:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a><span data-ttu-id="cb42c-232">Práce s ID hodnoty</span><span class="sxs-lookup"><span data-stu-id="cb42c-232">Working with ID values</span></span>
<span data-ttu-id="cb42c-233">Mobilní aplikace podporuje vlastní řetězec jedinečné hodnoty pro tabulku hello **id** sloupce.</span><span class="sxs-lookup"><span data-stu-id="cb42c-233">Mobile Apps supports unique custom string values for hello table's **id** column.</span></span> <span data-ttu-id="cb42c-234">Hodnotu řetězce umožňuje aplikacím toouse vlastní hodnoty, jako je například e-mailové adresy nebo uživatelské jméno pro ID hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-234">A string value allows applications toouse custom values such as email addresses or user names for hello ID.</span></span>  <span data-ttu-id="cb42c-235">Řetězec ID poskytnout hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cb42c-235">String IDs provide you with hello following benefits:</span></span>

* <span data-ttu-id="cb42c-236">ID jsou generovány bez provedení databáze toohello odezvy.</span><span class="sxs-lookup"><span data-stu-id="cb42c-236">IDs are generated without making a round trip toohello database.</span></span>
* <span data-ttu-id="cb42c-237">Záznamy jsou jednodušší toomerge z různých tabulek nebo databází.</span><span class="sxs-lookup"><span data-stu-id="cb42c-237">Records are easier toomerge from different tables or databases.</span></span>
* <span data-ttu-id="cb42c-238">Hodnoty ID umožňuje lepší integraci s logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb42c-238">IDs values can integrate better with an application's logic.</span></span>

<span data-ttu-id="cb42c-239">Pokud řetězcová hodnota ID není nastavená na vloženého záznamu, back-end mobilní aplikace hello generuje jedinečnou hodnotu pro ID.</span><span class="sxs-lookup"><span data-stu-id="cb42c-239">When a string ID value is not set on an inserted record, hello Mobile App backend generates a unique value for the ID.</span></span> <span data-ttu-id="cb42c-240">Můžete použít hello [Guid.NewGuid] toogenerate metoda vlastní ID hodnoty na hello klienta nebo v back-end hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-240">You can use hello [Guid.NewGuid] method toogenerate your own ID values, either on hello client or in hello backend.</span></span>

```
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <span data-ttu-id="cb42c-241"><a name="modifying"></a>Postupy: Změna dat v back-end mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="cb42c-241"><a name="modifying"></a>How to: Modify data in a Mobile App backend</span></span>
<span data-ttu-id="cb42c-242">Hello následující kódu ukazuje, jak toouse hello [metod UpdateAsync] tooupdate metoda existujícího záznamu s hello stejné ID se novými informacemi.</span><span class="sxs-lookup"><span data-stu-id="cb42c-242">hello following code illustrates how toouse hello [UpdateAsync] method tooupdate an existing record with hello same ID with new information.</span></span> <span data-ttu-id="cb42c-243">Parametr Hello obsahuje toobe data hello aktualizovat jako objekt .NET.</span><span class="sxs-lookup"><span data-stu-id="cb42c-243">hello parameter contains hello data toobe updated as a .NET object.</span></span>

```
await todoTable.UpdateAsync(todoItem);
```

<span data-ttu-id="cb42c-244">tooupdate netypová dat, může využít výhod [Json.NET] následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cb42c-244">tooupdate untyped data, you may take advantage of [Json.NET] as follows:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

<span data-ttu-id="cb42c-245">`id` Je nutné zadat při aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="cb42c-245">An `id` field must be specified when making an update.</span></span> <span data-ttu-id="cb42c-246">back-end Hello používá hello `id` tooidentify pole, která řádek tooupdate.</span><span class="sxs-lookup"><span data-stu-id="cb42c-246">hello backend uses hello `id` field tooidentify which row tooupdate.</span></span> <span data-ttu-id="cb42c-247">Hello `id` pole lze získat z výsledku hello hello `InsertAsync` volání.</span><span class="sxs-lookup"><span data-stu-id="cb42c-247">hello `id` field can be obtained from hello result of hello `InsertAsync` call.</span></span> <span data-ttu-id="cb42c-248">`ArgumentException` Se vyvolá, pokud se pokusíte tooupdate položky bez zadání hello `id` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-248">An `ArgumentException` is raised if you try tooupdate an item without providing hello `id` value.</span></span>

### <span data-ttu-id="cb42c-249"><a name="deleting"></a>Postupy: odstranění dat v back-end mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="cb42c-249"><a name="deleting"></a>How to: Delete data in a Mobile App backend</span></span>
<span data-ttu-id="cb42c-250">Hello následující kódu ukazuje, jak toouse hello [DeleteAsync] metoda toodelete existující instanci.</span><span class="sxs-lookup"><span data-stu-id="cb42c-250">hello following code illustrates how toouse hello [DeleteAsync] method toodelete an existing instance.</span></span> <span data-ttu-id="cb42c-251">Hello instance je identifikována hello `id` pole je nastaven na hello `todoItem`.</span><span class="sxs-lookup"><span data-stu-id="cb42c-251">hello instance is identified by hello `id` field set on hello `todoItem`.</span></span>

```
await todoTable.DeleteAsync(todoItem);
```

<span data-ttu-id="cb42c-252">toodelete netypová dat, může využít výhod Json.NET následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cb42c-252">toodelete untyped data, you may take advantage of Json.NET as follows:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

<span data-ttu-id="cb42c-253">Pokud zadáte požadavek delete, musí být zadaná ID.</span><span class="sxs-lookup"><span data-stu-id="cb42c-253">When you make a delete request, an ID must be specified.</span></span> <span data-ttu-id="cb42c-254">Ostatní vlastnosti nejsou předán toohello služby nebo jsou ignorovány u služby hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-254">Other properties are not passed toohello service or are ignored at hello service.</span></span> <span data-ttu-id="cb42c-255">Hello výsledek `DeleteAsync` volání je obvykle `null`.</span><span class="sxs-lookup"><span data-stu-id="cb42c-255">hello result of a `DeleteAsync` call is usually `null`.</span></span> <span data-ttu-id="cb42c-256">ID toopass Hello v lze získat z výsledku hello hello `InsertAsync` volání.</span><span class="sxs-lookup"><span data-stu-id="cb42c-256">hello ID toopass in can be obtained from hello result of hello `InsertAsync` call.</span></span> <span data-ttu-id="cb42c-257">A `MobileServiceInvalidOperationException` se vyvolá, když zkusíte toodelete položky bez zadání hello `id` pole.</span><span class="sxs-lookup"><span data-stu-id="cb42c-257">A `MobileServiceInvalidOperationException` is thrown when you try toodelete an item without specifying hello `id` field.</span></span>

### <span data-ttu-id="cb42c-258"><a name="optimisticconcurrency"></a>Postupy: použití optimistickou metodu souběžného pro řešení konfliktů.</span><span class="sxs-lookup"><span data-stu-id="cb42c-258"><a name="optimisticconcurrency"></a>How to: Use Optimistic Concurrency for conflict resolution</span></span>
<span data-ttu-id="cb42c-259">Dvě nebo více klientů může zapisovat změny toohello stejné položky v hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="cb42c-259">Two or more clients may write changes toohello same item at hello same time.</span></span> <span data-ttu-id="cb42c-260">Bez zjišťování konfliktů by poslední zápis hello přepsat všechny předchozí aktualizace.</span><span class="sxs-lookup"><span data-stu-id="cb42c-260">Without conflict detection, hello last write would overwrite any previous updates.</span></span> <span data-ttu-id="cb42c-261">**Optimistické řízení souběžného** předpokládá, že každou transakci můžete potvrdit a proto nepoužívá žádné uzamčení prostředků.</span><span class="sxs-lookup"><span data-stu-id="cb42c-261">**Optimistic concurrency control** assumes that each transaction can commit and therefore does not use any resource locking.</span></span>  <span data-ttu-id="cb42c-262">Před potvrzením transakce, optimistické řízení souběžného ověřuje, že neexistují žádné další transakce změnil hello data.</span><span class="sxs-lookup"><span data-stu-id="cb42c-262">Before committing a transaction, optimistic concurrency control verifies that no other transaction has modified hello data.</span></span> <span data-ttu-id="cb42c-263">Pokud hello data byl upraven, hello potvrzování transakce bude vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="cb42c-263">If hello data has been modified, hello committing transaction is rolled back.</span></span>

<span data-ttu-id="cb42c-264">Mobilní aplikace podporuje optimistické řízení souběžného pomocí funkce sledování změn tooeach položky pomocí hello `version` sloupec vlastností systému, která je definována pro každou tabulku v váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb42c-264">Mobile Apps supports optimistic concurrency control by tracking changes tooeach item using hello `version` system property column that is defined for each table in your Mobile App backend.</span></span> <span data-ttu-id="cb42c-265">Pokaždé, když aktualizaci záznamu Mobile Apps nastaví hello `version` vlastnost pro tento záznam tooa novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-265">Each time a record is updated, Mobile Apps sets hello `version` property for that record tooa new value.</span></span> <span data-ttu-id="cb42c-266">Při každé žádosti o aktualizaci hello `version` vlastnost záznamu hello součástí požadavku hello je porovnání toohello stejnou vlastnost pro hello záznam na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-266">During each update request, hello `version` property of hello record included with hello request is compared toohello same property for hello record on hello server.</span></span> <span data-ttu-id="cb42c-267">Pokud verze byla dokončena s hello žádosti neodpovídá hello back-end, pak vyvolá hello klientské knihovny `MobileServicePreconditionFailedException<T>` výjimka.</span><span class="sxs-lookup"><span data-stu-id="cb42c-267">If the version passed with hello request does not match hello backend, then hello client library raises a `MobileServicePreconditionFailedException<T>` exception.</span></span> <span data-ttu-id="cb42c-268">Typ Hello součástí hello výjimka je záznam hello z hello back-end obsahující hello verze servery hello záznamu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-268">hello type included with hello exception is hello record from hello backend containing hello servers version of hello record.</span></span> <span data-ttu-id="cb42c-269">Hello aplikace pak může použít tento toodecide informace o tom, jestli tooexecute hello aktualizaci požadavků zopakovat se správnou hello `version` hodnotu z hello back-end toocommit změny.</span><span class="sxs-lookup"><span data-stu-id="cb42c-269">hello application can then use this information toodecide whether tooexecute hello update request again with hello correct `version` value from hello backend toocommit changes.</span></span>

<span data-ttu-id="cb42c-270">Definovat sloupec na třídu hello tabulky pro hello `version` optimistickou metodu souběžného tooenable systému vlastnost.</span><span class="sxs-lookup"><span data-stu-id="cb42c-270">Define a column on hello table class for hello `version` system property tooenable optimistic concurrency.</span></span> <span data-ttu-id="cb42c-271">Například:</span><span class="sxs-lookup"><span data-stu-id="cb42c-271">For example:</span></span>

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

<span data-ttu-id="cb42c-272">Aplikace, které používají bez typu tabulky povolte optimistickou metodu souběžného tak, že nastavení hello `Version` příznak na `SystemProperties` z hello tabulky následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="cb42c-272">Applications using untyped tables enable optimistic concurrency by setting hello `Version` flag on the `SystemProperties` of hello table as follows.</span></span>

```
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

<span data-ttu-id="cb42c-273">V přidání tooenabling optimistickou metodu souběžného, musí také catch hello `MobileServicePreconditionFailedException<T>` výjimka ve vašem kódu při volání metody [metod UpdateAsync].</span><span class="sxs-lookup"><span data-stu-id="cb42c-273">In addition tooenabling optimistic concurrency, you must also catch hello `MobileServicePreconditionFailedException<T>` exception in your code when calling [UpdateAsync].</span></span>  <span data-ttu-id="cb42c-274">Hello konflikt vyřešit použitím hello správné `version` toothe aktualizovat záznam a volání [metod UpdateAsync] s hello vyřešil záznam.</span><span class="sxs-lookup"><span data-stu-id="cb42c-274">Resolve hello conflict by applying hello correct `version` toothe updated record and call [UpdateAsync] with hello resolved record.</span></span> <span data-ttu-id="cb42c-275">Hello následující kód ukazuje, jak tooresolve zápisu jednou zjištěn konflikt:</span><span class="sxs-lookup"><span data-stu-id="cb42c-275">hello following code shows how tooresolve a write conflict once detected:</span></span>

```
private async void UpdateToDoItem(TodoItem item)
{
    MobileServicePreconditionFailedException<TodoItem> exception = null;

    try
    {
        //update at hello remote table
        await todoTable.UpdateAsync(item);
    }
    catch (MobileServicePreconditionFailedException<TodoItem> writeException)
    {
        exception = writeException;
    }

    if (exception != null)
    {
        // Conflict detected, hello item has changed since hello last query
        // Resolve hello conflict between hello local and server item
        await ResolveConflict(item, exception.Item);
    }
}


private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
{
    //Ask user toochoose hello resoltion between versions
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
        // tooresolve hello conflict, update hello version of hello item being committed. Otherwise, you will keep
        // catching a MobileServicePreConditionFailedException.
        localItem.Version = serverItem.Version;

        // Updating recursively here just in case another change happened while hello user was making a decision
        UpdateToDoItem(localItem);
    };

    ServerBtn.Invoked = async (IUICommand command) =>
    {
        RefreshTodoItems();
    };

    await msgDialog.ShowAsync();
}
```

<span data-ttu-id="cb42c-276">Další informace najdete v tématu hello [Offline synchronizací dat v Azure Mobile Apps] tématu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-276">For more information, see hello [Offline Data Sync in Azure Mobile Apps] topic.</span></span>

### <span data-ttu-id="cb42c-277"><a name="binding"></a>Postupy: uživatelské rozhraní tooa Windows Mobile Apps vazby dat</span><span class="sxs-lookup"><span data-stu-id="cb42c-277"><a name="binding"></a>How to: Bind Mobile Apps data tooa Windows user interface</span></span>
<span data-ttu-id="cb42c-278">Tato část uvádí, jak toodisplay vrácená data objektů s použitím prvky uživatelského rozhraní v aplikaci pro Windows.</span><span class="sxs-lookup"><span data-stu-id="cb42c-278">This section shows how toodisplay returned data objects using UI elements in a Windows app.</span></span>  <span data-ttu-id="cb42c-279">Následující příklad kódu vytvoří vazbu toohello zdroj seznamu hello pomocí dotazu pro neúplné položky.</span><span class="sxs-lookup"><span data-stu-id="cb42c-279">The following example code binds toohello source of hello list with a query for incomplete items.</span></span> <span data-ttu-id="cb42c-280">[MobileServiceCollection] vytvoří kolekci mobilní aplikace využívající technologii vazby.</span><span class="sxs-lookup"><span data-stu-id="cb42c-280">The [MobileServiceCollection] creates a Mobile Apps-aware binding collection.</span></span>

```
// This query filters out completed TodoItems.
MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToCollectionAsync();

// itemsControl is an IEnumerable that could be bound tooa UI list control
IEnumerable itemsControl  = items;

// Bind this tooa ListBox
ListBox lb = new ListBox();
lb.ItemsSource = items;
```

<span data-ttu-id="cb42c-281">Podpora modulu runtime rozhraní nazývá spravovaného některé ovládací prvky v hello [ISupportIncrementalLoading].</span><span class="sxs-lookup"><span data-stu-id="cb42c-281">Some controls in hello managed runtime support an interface called [ISupportIncrementalLoading].</span></span> <span data-ttu-id="cb42c-282">Toto rozhraní umožňuje ovládací prvky toorequest doplňující data, když uživatel hello posune.</span><span class="sxs-lookup"><span data-stu-id="cb42c-282">This interface allows controls toorequest extra data when hello user scrolls.</span></span> <span data-ttu-id="cb42c-283">Je integrovanou podporu pro toto rozhraní pro univerzální aplikace pro Windows prostřednictvím [MobileServiceIncrementalLoadingCollection], který automaticky zpracovává volání z ovládacích prvků hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-283">There is built-in support for this interface for universal Windows apps via [MobileServiceIncrementalLoadingCollection], which automatically handles the calls from hello controls.</span></span> <span data-ttu-id="cb42c-284">Použití `MobileServiceIncrementalLoadingCollection` v aplikacích Windows následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cb42c-284">Use `MobileServiceIncrementalLoadingCollection` in Windows apps as follows:</span></span>

```
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

<span data-ttu-id="cb42c-285">Nová kolekce v aplikacích Windows Phone 8 a "Silverlight", použijte hello hello toouse `ToCollection` rozšiřující metody na `IMobileServiceTableQuery<T>` a `IMobileServiceTable<T>`.</span><span class="sxs-lookup"><span data-stu-id="cb42c-285">toouse hello new collection on Windows Phone 8 and "Silverlight" apps, use hello `ToCollection` extension methods on `IMobileServiceTableQuery<T>` and `IMobileServiceTable<T>`.</span></span> <span data-ttu-id="cb42c-286">volání tooload data `LoadMoreItemsAsync()`.</span><span class="sxs-lookup"><span data-stu-id="cb42c-286">tooload data, call `LoadMoreItemsAsync()`.</span></span>

```
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

<span data-ttu-id="cb42c-287">Při použití hello kolekce vytvořená voláním `ToCollectionAsync` nebo `ToCollection`, získání kolekce, která může být ovládací prvky vázané tooUI.</span><span class="sxs-lookup"><span data-stu-id="cb42c-287">When you use hello collection created by calling `ToCollectionAsync` or `ToCollection`, you get a collection that can be bound tooUI controls.</span></span>  <span data-ttu-id="cb42c-288">Tato kolekce je deklaracemi stránkování.</span><span class="sxs-lookup"><span data-stu-id="cb42c-288">This collection is paging-aware.</span></span>  <span data-ttu-id="cb42c-289">Vzhledem k tomu, že kolekce hello je načítání dat ze sítě, se někdy načítání nezdaří.</span><span class="sxs-lookup"><span data-stu-id="cb42c-289">Since hello collection is loading data from the network, loading sometimes fails.</span></span> <span data-ttu-id="cb42c-290">toohandle takových selhání přepsat hello `OnException` metodu `MobileServiceIncrementalLoadingCollection` toohandle výjimky vyplývající z volání příliš`LoadMoreItemsAsync`.</span><span class="sxs-lookup"><span data-stu-id="cb42c-290">toohandle such failures, override hello `OnException` method on `MobileServiceIncrementalLoadingCollection` toohandle exceptions resulting from calls too`LoadMoreItemsAsync`.</span></span>

<span data-ttu-id="cb42c-291">Zvažte, pokud tabulka obsahuje mnoho polí, ale chcete jenom toodisplay některé z nich vlastního ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="cb42c-291">Consider if your table has many fields but you only want toodisplay some of them in your control.</span></span> <span data-ttu-id="cb42c-292">Můžete použít pokyny hello v předcházející části hello "[vyberte konkrétní sloupce](#selecting)" tooselect toodisplay určité sloupce v hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cb42c-292">You may use hello guidance in hello preceding section "[Select specific columns](#selecting)" tooselect specific columns toodisplay in hello UI.</span></span>

### <span data-ttu-id="cb42c-293"><a name="pagesize"></a>Hello změnit velikost stránky</span><span class="sxs-lookup"><span data-stu-id="cb42c-293"><a name="pagesize"></a>Change hello Page size</span></span>
<span data-ttu-id="cb42c-294">Azure Mobile Apps vrací maximálně 50 položek na požadavek ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="cb42c-294">Azure Mobile Apps returns a maximum of 50 items per request by default.</span></span>  <span data-ttu-id="cb42c-295">Zvýšením hello maximální velikost stránky hello klienta i serveru, můžete změnit velikost stránkovacího hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-295">You can change hello paging size by increasing hello maximum page size on both hello client and server.</span></span>  <span data-ttu-id="cb42c-296">tooincrease hello velikost požadované stránce, zadejte `PullOptions` při použití `PullAsync()`:</span><span class="sxs-lookup"><span data-stu-id="cb42c-296">tooincrease hello requested page size, specify `PullOptions` when using `PullAsync()`:</span></span>

```
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

<span data-ttu-id="cb42c-297">Za předpokladu, že jste udělali hello `PageSize` rovna tooor vyšší než 100 v rámci serveru hello, žádost o vrátí až 100 položek.</span><span class="sxs-lookup"><span data-stu-id="cb42c-297">Assuming you have made hello `PageSize` equal tooor greater than 100 within hello server, a request returns up to 100 items.</span></span>

## <span data-ttu-id="cb42c-298"><a name="#offlinesync"></a>Práce s tabulkami Offline</span><span class="sxs-lookup"><span data-stu-id="cb42c-298"><a name="#offlinesync"></a>Work with Offline Tables</span></span>
<span data-ttu-id="cb42c-299">Offline tabulky použít data toostore místní úložiště SQLite pro použití v režimu offline.</span><span class="sxs-lookup"><span data-stu-id="cb42c-299">Offline tables use a local SQLite store toostore data for use when offline.</span></span>  <span data-ttu-id="cb42c-300">Všechny tabulky, které se provádějí operace proti hello místní SQLite ukládat místo hello vzdáleného serveru úložiště.</span><span class="sxs-lookup"><span data-stu-id="cb42c-300">All table operations are done against hello local SQLite store instead of hello remote server store.</span></span>  <span data-ttu-id="cb42c-301">toocreate offline tabulky, nejdřív připravit projektu:</span><span class="sxs-lookup"><span data-stu-id="cb42c-301">toocreate an offline table, first prepare your project:</span></span>

1. <span data-ttu-id="cb42c-302">V sadě Visual Studio, klikněte pravým tlačítkem na řešení hello > **spravovat balíčky NuGet pro řešení...** , vyhledejte a nainstalujte **Microsoft.Azure.Mobile.Client.SQLiteStore** balíček NuGet pro všechny projekty v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-302">In Visual Studio, right-click hello solution > **Manage NuGet Packages for Solution...**, then search for and install the **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet package for all projects in hello solution.</span></span>
2. <span data-ttu-id="cb42c-303">(Volitelné) toosupport zařízení se systémem Windows, instalovat jeden z následujících balíčků runtime SQLite hello:</span><span class="sxs-lookup"><span data-stu-id="cb42c-303">(Optional) toosupport Windows devices, install one of hello following SQLite runtime packages:</span></span>

   * <span data-ttu-id="cb42c-304">**Modul Runtime pro Windows 8.1:** nainstalovat [SQLite pro Windows 8.1][3].</span><span class="sxs-lookup"><span data-stu-id="cb42c-304">**Windows 8.1 Runtime:** Install [SQLite for Windows 8.1][3].</span></span>
   * <span data-ttu-id="cb42c-305">**Windows Phone 8.1:** nainstalovat [SQLite pro Windows Phone 8.1][4].</span><span class="sxs-lookup"><span data-stu-id="cb42c-305">**Windows Phone 8.1:** Install [SQLite for Windows Phone 8.1][4].</span></span>
   * <span data-ttu-id="cb42c-306">**Univerzální platformu Windows** nainstalovat [SQLite pro Universal Windows hello][5].</span><span class="sxs-lookup"><span data-stu-id="cb42c-306">**Universal Windows Platform** Install [SQLite for hello Universal Windows][5].</span></span>
3. <span data-ttu-id="cb42c-307">(Volitelné).</span><span class="sxs-lookup"><span data-stu-id="cb42c-307">(Optional).</span></span> <span data-ttu-id="cb42c-308">Zařízení se systémem Windows, klikněte na tlačítko **odkazy** > **přidat odkaz na...** , rozbalte položku hello **Windows** složky > **rozšíření**, povolíte hello odpovídající **SQLite pro systém Windows** SDK společně s hello  **Visual C++ Runtime 2013 pro Windows** SDK.</span><span class="sxs-lookup"><span data-stu-id="cb42c-308">For Windows devices, click **References** > **Add Reference...**, expand hello **Windows** folder > **Extensions**, then enable hello appropriate **SQLite for Windows** SDK along with hello **Visual C++ 2013 Runtime for Windows** SDK.</span></span>
    <span data-ttu-id="cb42c-309">Hello SQLite SDK názvy jsou mírně odlišné s každou platformu Windows.</span><span class="sxs-lookup"><span data-stu-id="cb42c-309">hello SQLite SDK names vary slightly with each Windows platform.</span></span>

<span data-ttu-id="cb42c-310">Aby bylo možné vytvořit odkaz na tabulku, musí být připraveno hello místního úložiště:</span><span class="sxs-lookup"><span data-stu-id="cb42c-310">Before a table reference can be created, hello local store must be prepared:</span></span>

```
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes hello SyncContext using hello default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

<span data-ttu-id="cb42c-311">Inicializace úložiště se obvykle provádí ihned po vytvoření klienta hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-311">Store initialization is normally done immediately after hello client is created.</span></span>  <span data-ttu-id="cb42c-312">Hello **OfflineDbPath** by měla být název souboru vhodné pro použití na všech platformách, které podporujete.</span><span class="sxs-lookup"><span data-stu-id="cb42c-312">hello **OfflineDbPath** should be a filename suitable for use on all platforms that you support.</span></span>  <span data-ttu-id="cb42c-313">Pokud se cesta hello plně kvalifikovanou cestu (tedy začíná lomítko), je použita v této cestě.</span><span class="sxs-lookup"><span data-stu-id="cb42c-313">If hello path is a fully qualified path (that is, it starts with a slash), then that path is used.</span></span>  <span data-ttu-id="cb42c-314">Pokud hello cesta není úplná, hello soubor je umístěn v umístění specifické pro platformu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-314">If hello path is not fully qualified, hello file is placed in a platform-specific location.</span></span>

* <span data-ttu-id="cb42c-315">Pro zařízení s iOS a Android hello výchozí cesta je složka "Osobní soubory" hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-315">For iOS and Android devices, hello default path is hello "Personal Files" folder.</span></span>
* <span data-ttu-id="cb42c-316">U zařízení s Windows hello výchozí cesta je složka "data aplikací" hello specifické pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cb42c-316">For Windows devices, hello default path is hello application-specific "AppData" folder.</span></span>

<span data-ttu-id="cb42c-317">Odkaz na tabulku můžete získat pomocí hello `GetSyncTable<>` metoda:</span><span class="sxs-lookup"><span data-stu-id="cb42c-317">A table reference can be obtained using hello `GetSyncTable<>` method:</span></span>

```
var table = client.GetSyncTable<TodoItem>();
```

<span data-ttu-id="cb42c-318">Není nutné tooauthenticate toouse offline tabulky.</span><span class="sxs-lookup"><span data-stu-id="cb42c-318">You do not need tooauthenticate toouse an offline table.</span></span>  <span data-ttu-id="cb42c-319">Potřebujete jenom tooauthenticate při jsou komunikaci s hello back-end službu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-319">You only need tooauthenticate when you are communicating with hello backend service.</span></span>

### <span data-ttu-id="cb42c-320"><a name="syncoffline"></a>Synchronizace Offline tabulky</span><span class="sxs-lookup"><span data-stu-id="cb42c-320"><a name="syncoffline"></a>Syncing an Offline Table</span></span>
<span data-ttu-id="cb42c-321">Offline tabulky nejsou synchronizované s back-end hello ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="cb42c-321">Offline tables are not synchronized with hello backend by default.</span></span>  <span data-ttu-id="cb42c-322">Synchronizace je rozdělit na dvě části.</span><span class="sxs-lookup"><span data-stu-id="cb42c-322">Synchronization is split into two pieces.</span></span>  <span data-ttu-id="cb42c-323">Doručte změny můžete samostatně stahování nové položky.</span><span class="sxs-lookup"><span data-stu-id="cb42c-323">You can push changes separately from downloading new items.</span></span>  <span data-ttu-id="cb42c-324">Tady je metoda typické synchronizace:</span><span class="sxs-lookup"><span data-stu-id="cb42c-324">Here is a typical sync method:</span></span>

```
public async Task SyncAsync()
{
    ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

    try
    {
        await this.client.SyncContext.PushAsync();

        await this.todoTable.PullAsync(
            //hello first parameter is a query name that is used internally by hello client SDK tooimplement incremental sync.
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

    // Simple error/conflict handling. A real application would handle hello various errors like network conditions,
    // server conflicts and others via hello IMobileServiceSyncHandler.
    if (syncErrors != null)
    {
        foreach (var error in syncErrors)
        {
            if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
            {
                //Update failed, reverting tooserver's copy.
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

<span data-ttu-id="cb42c-325">Pokud hello první argument příliš`PullAsync` má hodnotu null, není použita přírůstkové synchronizace.</span><span class="sxs-lookup"><span data-stu-id="cb42c-325">If hello first argument too`PullAsync` is null, then incremental sync is not used.</span></span>  <span data-ttu-id="cb42c-326">Každé operace synchronizace načte všechny záznamy.</span><span class="sxs-lookup"><span data-stu-id="cb42c-326">Each sync operation retrieves all records.</span></span>

<span data-ttu-id="cb42c-327">provede Hello SDK implicitní `PushAsync()` než vytažení záznamů.</span><span class="sxs-lookup"><span data-stu-id="cb42c-327">hello SDK performs an implicit `PushAsync()` before pulling records.</span></span>

<span data-ttu-id="cb42c-328">Zpracování konfliktů dochází `PullAsync()` metoda.</span><span class="sxs-lookup"><span data-stu-id="cb42c-328">Conflict handling happens on a `PullAsync()` method.</span></span>  <span data-ttu-id="cb42c-329">Můžete můžete řešit konflikty v hello stejný způsob jako online tabulky.</span><span class="sxs-lookup"><span data-stu-id="cb42c-329">You can deal with conflicts in hello same way as online tables.</span></span>  <span data-ttu-id="cb42c-330">vytváří Hello konflikt při `PullAsync()` je volána místo v průběhu hello insert, update nebo delete.</span><span class="sxs-lookup"><span data-stu-id="cb42c-330">hello conflict is produced when `PullAsync()` is called instead of during hello insert, update, or delete.</span></span> <span data-ttu-id="cb42c-331">Pokud je v konfliktu více, jsou seskupeny do jednoho MobileServicePushFailedException.</span><span class="sxs-lookup"><span data-stu-id="cb42c-331">If multiple conflicts happen, they are bundled into a single MobileServicePushFailedException.</span></span>  <span data-ttu-id="cb42c-332">Zpracování každé selhání samostatně.</span><span class="sxs-lookup"><span data-stu-id="cb42c-332">Handle each failure separately.</span></span>

## <span data-ttu-id="cb42c-333"><a name="#customapi"></a>Práce s vlastní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cb42c-333"><a name="#customapi"></a>Work with a custom API</span></span>
<span data-ttu-id="cb42c-334">Vlastní rozhraní API umožňuje toodefine vlastní koncové body, které zveřejňují funkce serveru které není mapování tooan vložení, aktualizaci, odstranění nebo operace čtení.</span><span class="sxs-lookup"><span data-stu-id="cb42c-334">A custom API enables you toodefine custom endpoints that expose server functionality that does not map tooan insert, update, delete, or read operation.</span></span> <span data-ttu-id="cb42c-335">Pomocí vlastního rozhraní API, může mít větší kontrolu nad zasílání zpráv, včetně čtení a nastavení hlavičky protokolu HTTP zpráv a definování formátu textu zprávy kromě formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="cb42c-335">By using a custom API, you can have more control over messaging, including reading and setting HTTP message headers and defining a message body format other than JSON.</span></span>

<span data-ttu-id="cb42c-336">Volání vlastní rozhraní API pomocí volání mezi hello [InvokeApiAsync] metody na klientovi hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-336">You call a custom API by calling one of hello [InvokeApiAsync] methods on hello client.</span></span> <span data-ttu-id="cb42c-337">Například následující řádek kódu hello odešle toohello požadavek POST **completeAll** rozhraní API na back-end hello:</span><span class="sxs-lookup"><span data-stu-id="cb42c-337">For example, hello following line of code sends a POST request toohello **completeAll** API on hello backend:</span></span>

```
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

<span data-ttu-id="cb42c-338">Tento formulář je typu metoda volání a vyžaduje, že hello **MarkAllResult** vrátí typ je definován.</span><span class="sxs-lookup"><span data-stu-id="cb42c-338">This form is a typed method call and requires that hello **MarkAllResult** return type is defined.</span></span> <span data-ttu-id="cb42c-339">Typové i bez typu metody jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="cb42c-339">Both typed and untyped methods are supported.</span></span>

<span data-ttu-id="cb42c-340">Hello InvokeApiAsync() metoda přidá/api/toohello rozhraní API chcete toocall, pokud začíná hello rozhraní API lomítkem (/).</span><span class="sxs-lookup"><span data-stu-id="cb42c-340">hello InvokeApiAsync() method prepends '/api/' toohello API that you wish toocall unless hello API starts with a '/'.</span></span>
<span data-ttu-id="cb42c-341">Například:</span><span class="sxs-lookup"><span data-stu-id="cb42c-341">For example:</span></span>

* <span data-ttu-id="cb42c-342">`InvokeApiAsync("completeAll",...)`volání /api/completeAll na back-end hello</span><span class="sxs-lookup"><span data-stu-id="cb42c-342">`InvokeApiAsync("completeAll",...)` calls /api/completeAll on hello backend</span></span>
* <span data-ttu-id="cb42c-343">`InvokeApiAsync("/.auth/me",...)`volání /.auth/me na back-end hello</span><span class="sxs-lookup"><span data-stu-id="cb42c-343">`InvokeApiAsync("/.auth/me",...)` calls /.auth/me on hello backend</span></span>

<span data-ttu-id="cb42c-344">Můžete vytvořit InvokeApiAsync toocall žádné WebAPI, včetně těchto WebAPIs, které nejsou definovány s Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="cb42c-344">You can use InvokeApiAsync toocall any WebAPI, including those WebAPIs that are not defined with Azure Mobile Apps.</span></span>  <span data-ttu-id="cb42c-345">Při použití InvokeApiAsync() hello odpovídající hlavičky, včetně ověřování hlavičky, odešlou hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="cb42c-345">When you use InvokeApiAsync(), hello appropriate headers, including authentication headers, are sent with hello request.</span></span>

## <span data-ttu-id="cb42c-346"><a name="authentication"></a>Ověřování uživatelů</span><span class="sxs-lookup"><span data-stu-id="cb42c-346"><a name="authentication"></a>Authenticate users</span></span>
<span data-ttu-id="cb42c-347">Mobilní aplikace podporuje ověřování a autorizaci uživatelů aplikace pomocí různých zprostředkovatelů externí identity: Facebook, Google, Microsoft Account, Twitter a Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cb42c-347">Mobile Apps supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, Twitter, and Azure Active Directory.</span></span> <span data-ttu-id="cb42c-348">Oprávnění můžete nastavit na tabulky toorestrict přístup pro určité operace tooonly ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="cb42c-348">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="cb42c-349">Můžete taky hello identity ověřené uživatele tooimplement autorizačních pravidel ve skriptech serveru.</span><span class="sxs-lookup"><span data-stu-id="cb42c-349">You can also use hello identity of authenticated users tooimplement authorization rules in server scripts.</span></span> <span data-ttu-id="cb42c-350">Další informace najdete v tématu hello kurzu [přidat ověřování tooyour aplikace].</span><span class="sxs-lookup"><span data-stu-id="cb42c-350">For more information, see hello tutorial [Add authentication tooyour app].</span></span>

<span data-ttu-id="cb42c-351">Jsou podporovány dva ověřování toky: *klienta spravovat* a *server spravovaný* toku.</span><span class="sxs-lookup"><span data-stu-id="cb42c-351">Two authentication flows are supported: *client-managed* and *server-managed* flow.</span></span> <span data-ttu-id="cb42c-352">tok spravovaných server Hello poskytuje hello nejjednodušší ověřování, jako je závislé na hello zprostředkovatele webového ověření rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cb42c-352">hello server-managed flow provides hello simplest authentication experience, as it relies on hello provider's web authentication interface.</span></span> <span data-ttu-id="cb42c-353">tok spravovaných klienta Hello umožňuje hlubší integrace s funkcemi konkrétní zařízení jako přitom spoléhá na konkrétní zařízení specifické pro poskytovatele sady SDK.</span><span class="sxs-lookup"><span data-stu-id="cb42c-353">hello client-managed flow allows for deeper integration with device-specific capabilities as it relies on provider-specific device-specific SDKs.</span></span>

> [!NOTE]
> <span data-ttu-id="cb42c-354">Doporučujeme používat k klienta spravovat toku ve svých aplikacích produkční.</span><span class="sxs-lookup"><span data-stu-id="cb42c-354">We recommend using a client-managed flow in your production apps.</span></span>

<span data-ttu-id="cb42c-355">tooset až ověřování, je nutné zaregistrovat aplikaci s jeden nebo více poskytovatelů identity.</span><span class="sxs-lookup"><span data-stu-id="cb42c-355">tooset up authentication, you must register your app with one or more identity providers.</span></span>  <span data-ttu-id="cb42c-356">zprostředkovatele identity Hello vygeneruje ID klienta a tajný klíč klienta pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cb42c-356">hello identity provider generates a client ID and a client secret for your app.</span></span>  <span data-ttu-id="cb42c-357">Tyto hodnoty jsou pak nastavené ve vašem back-end tooenable ověřování/autorizace služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="cb42c-357">These values are then set in your backend tooenable Azure App Service authentication/authorization.</span></span>  <span data-ttu-id="cb42c-358">Další informace, postupujte podle hello podrobné pokyny v tomto kurzu [přidat ověřování tooyour aplikace].</span><span class="sxs-lookup"><span data-stu-id="cb42c-358">For more information, follow hello detailed instructions in the tutorial [Add authentication tooyour app].</span></span>

<span data-ttu-id="cb42c-359">v této části jsou popsané Hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="cb42c-359">hello following topics are covered in this section:</span></span>

* [<span data-ttu-id="cb42c-360">Klienta spravovat ověřování</span><span class="sxs-lookup"><span data-stu-id="cb42c-360">Client-managed authentication</span></span>](#clientflow)
* [<span data-ttu-id="cb42c-361">Spravovaný server ověřování</span><span class="sxs-lookup"><span data-stu-id="cb42c-361">Server-managed authentication</span></span>](#serverflow)
* [<span data-ttu-id="cb42c-362">Ukládání do mezipaměti hello ověřovací token</span><span class="sxs-lookup"><span data-stu-id="cb42c-362">Caching hello authentication token</span></span>](#caching)

### <span data-ttu-id="cb42c-363"><a name="clientflow"></a>Klienta spravovat ověřování</span><span class="sxs-lookup"><span data-stu-id="cb42c-363"><a name="clientflow"></a>Client-managed authentication</span></span>
<span data-ttu-id="cb42c-364">Aplikace můžete nezávisle obraťte se na zprostředkovatele identity hello a pak zadejte token vrácený hello během přihlašování s váš back-end.</span><span class="sxs-lookup"><span data-stu-id="cb42c-364">Your app can independently contact hello identity provider and then provide hello returned token during login with your backend.</span></span> <span data-ttu-id="cb42c-365">Tento tok klienta umožňuje tooprovide jednom přihlášení pro uživatele nebo tooretrieve další uživatelské dat od poskytovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-365">This client flow enables you tooprovide a single sign-on experience for users or tooretrieve additional user data from hello identity provider.</span></span> <span data-ttu-id="cb42c-366">Tok ověřování klientů je upřednostňovaný toousing toku serveru jako zprostředkovatele identity hello SDK poskytuje více nativní UX chování a umožňuje další přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="cb42c-366">Client flow authentication is preferred toousing a server flow as hello identity provider SDK provides a more native UX feel and allows for additional customization.</span></span>

<span data-ttu-id="cb42c-367">Příklady jsou k dispozici pro hello vzory klienta tok ověřování následující:</span><span class="sxs-lookup"><span data-stu-id="cb42c-367">Examples are provided for hello following client-flow authentication patterns:</span></span>

* [<span data-ttu-id="cb42c-368">Knihovna ověřování Active Directory</span><span class="sxs-lookup"><span data-stu-id="cb42c-368">Active Directory Authentication Library</span></span>](#adal)
* [<span data-ttu-id="cb42c-369">Facebook nebo Google</span><span class="sxs-lookup"><span data-stu-id="cb42c-369">Facebook or Google</span></span>](#client-facebook)
* [<span data-ttu-id="cb42c-370">Live SDK</span><span class="sxs-lookup"><span data-stu-id="cb42c-370">Live SDK</span></span>](#client-livesdk)

#### <span data-ttu-id="cb42c-371"><a name="adal"></a>Ověřuje uživatele pomocí hello knihovna ověřování Active Directory</span><span class="sxs-lookup"><span data-stu-id="cb42c-371"><a name="adal"></a>Authenticate users with hello Active Directory Authentication Library</span></span>
<span data-ttu-id="cb42c-372">Můžete použít hello Active Directory Authentication Library (ADAL) tooinitiate ověřování uživatelů z klienta hello pomocí ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cb42c-372">You can use hello Active Directory Authentication Library (ADAL) tooinitiate user authentication from hello client using Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="cb42c-373">Nakonfigurujte následující hello váš back-end mobilní aplikace pro přihlašování AAD [jak tooconfigure aplikaci služby pro služby Active Directory přihlášení] kurzu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-373">Configure your mobile app backend for AAD sign-on by following hello [How tooconfigure App Service for Active Directory login] tutorial.</span></span> <span data-ttu-id="cb42c-374">Ujistěte se, že toocomplete hello volitelný krok registrace nativní klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cb42c-374">Make sure toocomplete hello optional step of registering a native client application.</span></span>
2. <span data-ttu-id="cb42c-375">V sadě Visual Studio nebo Xamarin Studio, otevřete projekt a přidejte odkaz na toothe `Microsoft.IdentityModel.CLients.ActiveDirectory` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="cb42c-375">In Visual Studio or Xamarin Studio, open your project and add a reference toothe `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet package.</span></span> <span data-ttu-id="cb42c-376">Při hledání, zahrňte předběžné verze.</span><span class="sxs-lookup"><span data-stu-id="cb42c-376">When searching, include pre-release versions.</span></span>
3. <span data-ttu-id="cb42c-377">Přidejte hello následující aplikaci tooyour kódu, podle toohello platformu, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="cb42c-377">Add hello following code tooyour application, according toohello platform you are using.</span></span> <span data-ttu-id="cb42c-378">V každé zkontrolujte následující náhrady hello:</span><span class="sxs-lookup"><span data-stu-id="cb42c-378">In each, make hello following replacements:</span></span>

   * <span data-ttu-id="cb42c-379">Nahraďte **INSERT. AUTORITY zde** s názvem hello hello klienta, ve kterém jste zřídili vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb42c-379">Replace **INSERT-AUTHORITY-HERE** with hello name of hello tenant in which you provisioned your application.</span></span> <span data-ttu-id="cb42c-380">Formát by měl být https://login.microsoftonline.com/contoso.onmicrosoft.com. Tuto hodnotu lze kopírovat z karty hello domény v Azure Active Directory v hello [portál Azure classic].</span><span class="sxs-lookup"><span data-stu-id="cb42c-380">The format should be https://login.microsoftonline.com/contoso.onmicrosoft.com. This value can be copied from hello Domain tab in your Azure Active Directory in hello [Azure classic portal].</span></span>
   * <span data-ttu-id="cb42c-381">Nahraďte **INSERT-RESOURCE-ID-zde** s ID klienta hello back-endu mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb42c-381">Replace **INSERT-RESOURCE-ID-HERE** with hello client ID for your mobile app backend.</span></span> <span data-ttu-id="cb42c-382">ID klienta hello můžete získat z hello **Upřesnit** v části **nastavení Azure Active Directory** hello portálu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-382">You can obtain hello client ID from hello **Advanced** tab under **Azure Active Directory Settings** in hello portal.</span></span>
   * <span data-ttu-id="cb42c-383">Nahraďte **INSERT klienta ID zde** s ID klienta hello jste zkopírovali ze hello nativní klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cb42c-383">Replace **INSERT-CLIENT-ID-HERE** with hello client ID you copied from hello native client application.</span></span>
   * <span data-ttu-id="cb42c-384">Nahraďte **vložení PŘESMĚROVÁNÍ URI zde** s vaší lokality */.auth/login/done* koncový bod, pomocí hello schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cb42c-384">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using hello HTTPS scheme.</span></span> <span data-ttu-id="cb42c-385">Tato hodnota by mělo být podobné příliš*https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="cb42c-385">This value should be similar too*https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

     <span data-ttu-id="cb42c-386">Následuje Hello kód potřebný pro každou platformu:</span><span class="sxs-lookup"><span data-stu-id="cb42c-386">hello code needed for each platform follows:</span></span>

     <span data-ttu-id="cb42c-387">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="cb42c-387">**Windows:**</span></span>

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

     <span data-ttu-id="cb42c-388">**Xamarin.iOS**</span><span class="sxs-lookup"><span data-stu-id="cb42c-388">**Xamarin.iOS**</span></span>

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

     <span data-ttu-id="cb42c-389">**Xamarin.Android**</span><span class="sxs-lookup"><span data-stu-id="cb42c-389">**Xamarin.Android**</span></span>

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

#### <span data-ttu-id="cb42c-390"><a name="client-facebook"></a>Jednotné přihlašování pomocí tokenu z Facebook nebo Google</span><span class="sxs-lookup"><span data-stu-id="cb42c-390"><a name="client-facebook"></a>Single Sign-On using a token from Facebook or Google</span></span>
<span data-ttu-id="cb42c-391">Tok hello klienta můžete použít, jak je uvedeno v této fragmentu kódu pro Facebook nebo Google.</span><span class="sxs-lookup"><span data-stu-id="cb42c-391">You can use hello client flow as shown in this snippet for Facebook or Google.</span></span>

```
var token = new JObject();
// Replace access_token_value with actual value of your access token obtained
// using hello Facebook or Google SDK.
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
            // tooMobileServiceAuthenticationProvider.Google if using Google auth.
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

#### <span data-ttu-id="cb42c-392"><a name="client-livesdk"></a>Jednotné přihlašování pomocí Account Microsoft hello Live SDK</span><span class="sxs-lookup"><span data-stu-id="cb42c-392"><a name="client-livesdk"></a>Single Sign On using Microsoft Account with hello Live SDK</span></span>
<span data-ttu-id="cb42c-393">tooauthenticate uživatele, je nutné zaregistrovat aplikaci v hello účtu Microsoft Developer Center.</span><span class="sxs-lookup"><span data-stu-id="cb42c-393">tooauthenticate users, you must register your app at hello Microsoft account Developer Center.</span></span> <span data-ttu-id="cb42c-394">Nakonfigurujte podrobnosti registrace na váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb42c-394">Configure the registration details on your Mobile App backend.</span></span> <span data-ttu-id="cb42c-395">toocreate Microsoft registrace účtu a připojte ho back-end mobilní aplikace tooyour, dokončení hello kroky [registraci vaší aplikace toouse přihlašovací údaje účtu Microsoft].</span><span class="sxs-lookup"><span data-stu-id="cb42c-395">toocreate a Microsoft account registration and connect it tooyour Mobile App backend, complete hello steps in [Register your app toouse a Microsoft account login].</span></span> <span data-ttu-id="cb42c-396">Pokud máte Windows Store a Windows Phone 8 nebo Silverlight verze aplikace, nejprve zaregistrujte hello verze Windows Store.</span><span class="sxs-lookup"><span data-stu-id="cb42c-396">If you have both Windows Store and Windows Phone 8/Silverlight versions of your app, register hello Windows Store version first.</span></span>

<span data-ttu-id="cb42c-397">Hello následující kód se ověří pomocí Live SDK a používá token toosign, vrátí se v back-end mobilní aplikace tooyour hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-397">hello following code authenticates using Live SDK and uses hello returned token toosign in tooyour Mobile App backend.</span></span>

```
private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
private async System.Threading.Tasks.Task AuthenticateAsync()
{

    // Get hello URL hello Mobile App backend.
    var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

    // Create hello authentication client for Windows Store using hello service URL.
    LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
    //// Create hello authentication client for Windows Phone using hello client ID of hello registration.
    //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

    while (session == null)
    {
        // Request hello authentication token from hello Live authentication service.
        // hello wl.basic scope should always be requested.  Other scopes can be added
        LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
        if (result.Status == LiveConnectSessionStatus.Connected)
        {
            session = result.Session;

            // Get information about hello logged-in user.
            LiveConnectClient client = new LiveConnectClient(session);
            LiveOperationResult meResult = await client.GetAsync("me");

            // Use hello Microsoft account auth token toosign in tooApp Service.
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

<span data-ttu-id="cb42c-398">Další informace najdete v tématu hello [Windows Live SDK] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="cb42c-398">For more information, see hello [Windows Live SDK] documentation.</span></span>

### <span data-ttu-id="cb42c-399"><a name="serverflow"></a>Spravovaný server ověřování</span><span class="sxs-lookup"><span data-stu-id="cb42c-399"><a name="serverflow"></a>Server-managed authentication</span></span>
<span data-ttu-id="cb42c-400">Po registraci zprostředkovatele identity volání hello [LoginAsync] metodu hello [MobileServiceClient] s hello [MobileServiceAuthenticationProvider] hodnotu svého poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="cb42c-400">Once you have registered your identity provider, call hello [LoginAsync] method on hello [MobileServiceClient] with hello [MobileServiceAuthenticationProvider] value of your provider.</span></span> <span data-ttu-id="cb42c-401">Například hello následující kód spustí serveru toku přihlášení pomocí Facebooku.</span><span class="sxs-lookup"><span data-stu-id="cb42c-401">For example, hello following code initiates a server flow sign-in by using Facebook.</span></span>

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

<span data-ttu-id="cb42c-402">Pokud používáte zprostředkovatele identity než Facebook, změňte hodnotu hello [MobileServiceAuthenticationProvider] toohello hodnota pro zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="cb42c-402">If you are using an identity provider other than Facebook, change hello value of [MobileServiceAuthenticationProvider] toohello value for your provider.</span></span>

<span data-ttu-id="cb42c-403">V toku server Azure App Service spravuje tok ověřování OAuth hello zobrazením hello přihlašovací stránku hello vybraného zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="cb42c-403">In a server flow, Azure App Service manages hello OAuth authentication flow by displaying hello sign-in page of hello selected provider.</span></span>  <span data-ttu-id="cb42c-404">Vrátí zprostředkovatele hello identity, služby Azure App Service generuje jednou ověřovací token služby App Service.</span><span class="sxs-lookup"><span data-stu-id="cb42c-404">Once hello identity provider returns, Azure App Service generates an App Service authentication token.</span></span> <span data-ttu-id="cb42c-405">Hello [LoginAsync] metoda vrátí [MobileServiceUser], který poskytuje i hello [UserId] z hello ověřený uživatel a hello [ MobileServiceAuthenticationToken], jako webového tokenu JSON (JWT).</span><span class="sxs-lookup"><span data-stu-id="cb42c-405">hello [LoginAsync] method returns a [MobileServiceUser], which provides both hello [UserId] of hello authenticated user and hello [MobileServiceAuthenticationToken], as a JSON web token (JWT).</span></span> <span data-ttu-id="cb42c-406">Tento token se může uložit do mezipaměti a znovu požívat do vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="cb42c-406">This token can be cached and reused until it expires.</span></span> <span data-ttu-id="cb42c-407">Další informace najdete v tématu [ukládání do mezipaměti hello ověřovací token](#caching).</span><span class="sxs-lookup"><span data-stu-id="cb42c-407">For more information, see [Caching hello authentication token](#caching).</span></span>

### <span data-ttu-id="cb42c-408"><a name="caching"></a>Ukládání do mezipaměti hello ověřovací token</span><span class="sxs-lookup"><span data-stu-id="cb42c-408"><a name="caching"></a>Caching hello authentication token</span></span>
<span data-ttu-id="cb42c-409">V některých případech hello volání toohello přihlášení metoda můžete zabránit po prvním úspěšném ověření hello ukládání hello ověřovací token od zprostředkovatele hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-409">In some cases, hello call toohello login method can be avoided after hello first successful authentication by storing hello authentication token from hello provider.</span></span>  <span data-ttu-id="cb42c-410">Aplikace Windows Store a UWP můžete použít [PasswordVault] toocache aktuální ověřování tokenu po úspěšného přihlášení, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cb42c-410">Windows Store and UWP apps can use [PasswordVault] toocache the current authentication token after a successful sign-in, as follows:</span></span>

```
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

<span data-ttu-id="cb42c-411">Hello UserId hodnota je uložena jako hello uživatelské jméno pověření hello a hello token je hello uložené jako hello heslo.</span><span class="sxs-lookup"><span data-stu-id="cb42c-411">hello UserId value is stored as hello UserName of hello credential and hello token is hello stored as hello Password.</span></span> <span data-ttu-id="cb42c-412">Na následujících podniky, můžete zkontrolovat hello **PasswordVault** pro přihlašovací údaje v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="cb42c-412">On subsequent start-ups, you can check hello **PasswordVault** for cached credentials.</span></span> <span data-ttu-id="cb42c-413">Hello následující příklad používá pověření uložená v mezipaměti při jejich zjištění a v opačném případě se pokusí tooauthenticate znovu s back-end hello:</span><span class="sxs-lookup"><span data-stu-id="cb42c-413">hello following example uses cached credentials when they are found, and otherwise attempts tooauthenticate again with hello backend:</span></span>

```
// Try tooretrieve stored credentials.
var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
if (creds != null)
{
    // Create hello current user from hello stored credentials.
    client.currentUser = new MobileServiceUser(creds.UserName);
    client.currentUser.MobileServiceAuthenticationToken =
        vault.Retrieve("Facebook", creds.UserName).Password;
}
else
{
    // Regular login flow and cache hello token as shown above.
}
```

<span data-ttu-id="cb42c-414">Při odhlášení uživatele, musíte také odebrat hello uložené přihlašovací údaje, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cb42c-414">When you sign out a user, you must also remove hello stored credential, as follows:</span></span>

```
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

<span data-ttu-id="cb42c-415">Pomocí aplikace Xamarin hello [Xamarin.Auth] údaje k úložišti toosecurely rozhraní API v **účet** objektu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-415">Xamarin    apps use hello [Xamarin.Auth] APIs toosecurely store credentials in an **Account** object.</span></span> <span data-ttu-id="cb42c-416">Příklad použití tato rozhraní API, naleznete v části hello [AuthStore.cs] souboru kódu v hello [ContosoMoments fotografií sdílení ukázka](https://github.com/azure-appservice-samples/ContosoMoments).</span><span class="sxs-lookup"><span data-stu-id="cb42c-416">For an example of using these APIs, see hello [AuthStore.cs] code file in hello [ContosoMoments photo sharing sample](https://github.com/azure-appservice-samples/ContosoMoments).</span></span>

<span data-ttu-id="cb42c-417">Pokud používáte klienta spravovat ověřování, můžete také mezipaměti hello přístupový token zajišťuje poskytovatel například Facebook nebo Twitter.</span><span class="sxs-lookup"><span data-stu-id="cb42c-417">When you use client-managed authentication, you can also cache hello access token obtained from your provider such as Facebook or Twitter.</span></span> <span data-ttu-id="cb42c-418">Tento token může být zadaný toorequest nový token ověřování z back-end hello, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cb42c-418">This token can be supplied toorequest a new authentication token from hello backend, as follows:</span></span>

```
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using hello access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <span data-ttu-id="cb42c-419"><a name="pushnotifications"></a>Nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="cb42c-419"><a name="pushnotifications"></a>Push Notifications</span></span>
<span data-ttu-id="cb42c-420">Hello následujících tématech najdete nabízených oznámení:</span><span class="sxs-lookup"><span data-stu-id="cb42c-420">hello following topics cover Push Notifications:</span></span>

* [<span data-ttu-id="cb42c-421">Registrace pro nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="cb42c-421">Register for Push Notifications</span></span>](#register-for-push)
* [<span data-ttu-id="cb42c-422">Získat identifikátor SID balíčku Windows Store</span><span class="sxs-lookup"><span data-stu-id="cb42c-422">Obtain a Windows Store package SID</span></span>](#package-sid)
* [<span data-ttu-id="cb42c-423">Zaregistrovat se šablonami a platformy</span><span class="sxs-lookup"><span data-stu-id="cb42c-423">Register with Cross-platform templates</span></span>](#register-xplat)

### <span data-ttu-id="cb42c-424"><a name="register-for-push"></a>Postupy: registrace pro nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="cb42c-424"><a name="register-for-push"></a>How to: Register for Push Notifications</span></span>
<span data-ttu-id="cb42c-425">Hello Mobile Apps client umožňuje tooregister pro nabízená oznámení pomocí Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="cb42c-425">hello Mobile Apps client enables you tooregister for push notifications with Azure Notification Hubs.</span></span> <span data-ttu-id="cb42c-426">Při registraci, můžete získat popisovač, které jste získali od hello specifické platformy služby nabízených oznámení (PNS).</span><span class="sxs-lookup"><span data-stu-id="cb42c-426">When registering, you obtain a handle that you obtain from hello platform-specific Push Notification Service (PNS).</span></span> <span data-ttu-id="cb42c-427">Pak poskytnete tuto hodnotu společně s všechny značky, když vytvoříte hello registrace.</span><span class="sxs-lookup"><span data-stu-id="cb42c-427">You then provide this value along with any tags when you create hello registration.</span></span> <span data-ttu-id="cb42c-428">Hello následující kód zaregistruje aplikace pro Windows pro nabízená oznámení se hello služby oznámení Windows (WNS):</span><span class="sxs-lookup"><span data-stu-id="cb42c-428">hello following code registers your Windows app for push notifications with hello Windows Notification Service (WNS):</span></span>

```
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using hello new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

<span data-ttu-id="cb42c-429">Pokud nabízíte tooWNS, pak musíte [získat identifikátor SID balíčku Windows Store](#package-sid).</span><span class="sxs-lookup"><span data-stu-id="cb42c-429">If you are pushing tooWNS, then you MUST [obtain a Windows Store package SID](#package-sid).</span></span>  <span data-ttu-id="cb42c-430">Další informace o aplikace pro Windows včetně toho, jak zjistit, tooregister pro šablony registrace [přidat nabízená oznámení tooyour aplikace].</span><span class="sxs-lookup"><span data-stu-id="cb42c-430">For more information on Windows apps, including how tooregister for template registrations, see [Add push notifications tooyour app].</span></span>

<span data-ttu-id="cb42c-431">Požaduje značky z hello klienta není podporována.</span><span class="sxs-lookup"><span data-stu-id="cb42c-431">Requesting tags from hello client is not supported.</span></span>  <span data-ttu-id="cb42c-432">Značka požadavky jsou bezobslužně vyřadit z registrace.</span><span class="sxs-lookup"><span data-stu-id="cb42c-432">Tag Requests are silently dropped from registration.</span></span>
<span data-ttu-id="cb42c-433">Pokud chcete tooregister zařízení pomocí značek, vytvořte vlastní rozhraní API, které používá hello API centra oznámení tooperform hello registrace vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="cb42c-433">If you wish tooregister your device with tags, create a Custom API that uses hello Notification Hubs API tooperform hello registration on your behalf.</span></span>  <span data-ttu-id="cb42c-434">[Volání rozhraní API vlastní hello](#customapi) místo hello `RegisterNativeAsync()` metoda.</span><span class="sxs-lookup"><span data-stu-id="cb42c-434">[Call hello Custom API](#customapi) instead of hello `RegisterNativeAsync()` method.</span></span>

### <span data-ttu-id="cb42c-435"><a name="package-sid"></a>Postupy: získání SID balíčku Windows Store</span><span class="sxs-lookup"><span data-stu-id="cb42c-435"><a name="package-sid"></a>How to: Obtain a Windows Store package SID</span></span>
<span data-ttu-id="cb42c-436">SID balíčku je nutné pro povolení nabízených oznámení v aplikacích pro Windows Store.</span><span class="sxs-lookup"><span data-stu-id="cb42c-436">A package SID is needed for enabling push notifications in Windows Store apps.</span></span>  <span data-ttu-id="cb42c-437">tooreceive balíček SID, registrace vaší aplikace s hello Windows Store.</span><span class="sxs-lookup"><span data-stu-id="cb42c-437">tooreceive a package SID, register your application with hello Windows Store.</span></span>

<span data-ttu-id="cb42c-438">tooobtain tuto hodnotu:</span><span class="sxs-lookup"><span data-stu-id="cb42c-438">tooobtain this value:</span></span>

1. <span data-ttu-id="cb42c-439">V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na projekt aplikace Windows Store hello, klikněte na tlačítko **úložiště** > **propojit aplikaci se hello úložiště...** .</span><span class="sxs-lookup"><span data-stu-id="cb42c-439">In Visual Studio Solution Explorer, right-click hello Windows Store app project, click **Store** > **Associate App with hello Store...**.</span></span>
2. <span data-ttu-id="cb42c-440">V Průvodci hello, klikněte na tlačítko **Další**, přihlaste se pomocí účtu Microsoft, zadejte název aplikace v rámci **rezervovat nový název aplikace**, pak klikněte na tlačítko **rezervy**.</span><span class="sxs-lookup"><span data-stu-id="cb42c-440">In hello wizard, click **Next**, sign in with your Microsoft account, type a name for your app in **Reserve a new app name**, then click **Reserve**.</span></span>
3. <span data-ttu-id="cb42c-441">Po registraci aplikace hello je hello úspěšně vytvořil, vyberte název aplikace, klikněte na tlačítko **Další**a potom klikněte na **přidružit**.</span><span class="sxs-lookup"><span data-stu-id="cb42c-441">After hello app registration is successfully created, select hello app name, click **Next**, and then click **Associate**.</span></span>
4. <span data-ttu-id="cb42c-442">Přihlaste se toohello [Windows Dev Center] pomocí Account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cb42c-442">Log in toohello [Windows Dev Center] using your Microsoft Account.</span></span> <span data-ttu-id="cb42c-443">V části **Moje aplikace**, klikněte na možnost registrace aplikace hello jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="cb42c-443">Under **My apps**, click hello app registration you created.</span></span>
5. <span data-ttu-id="cb42c-444">Klikněte na tlačítko **správy aplikací** > **identity aplikace**a potom přejděte dolů na toofind vaše **SID balíčku**.</span><span class="sxs-lookup"><span data-stu-id="cb42c-444">Click **App management** > **App identity**, and then scroll down toofind your **Package SID**.</span></span>

<span data-ttu-id="cb42c-445">Mnoho používá SID balíčku hello s nimi zacházet jako identifikátor URI, v takovém případě musíte toouse *ms aplikace: / /* hello schéma.</span><span class="sxs-lookup"><span data-stu-id="cb42c-445">Many uses of hello package SID treat it as a URI, in which case you need toouse *ms-app://* as hello scheme.</span></span> <span data-ttu-id="cb42c-446">Poznamenejte si hello verzi vašeho balíčku SID tvořena zřetězením tuto hodnotu jako předponu.</span><span class="sxs-lookup"><span data-stu-id="cb42c-446">Make note of hello version of your package SID formed by concatenating this value as a prefix.</span></span>

<span data-ttu-id="cb42c-447">Aplikace Xamarin vyžadovat některé další kód toobe možné tooregister aplikace spuštěné na hello iOS nebo Android platformy.</span><span class="sxs-lookup"><span data-stu-id="cb42c-447">Xamarin apps require some additional code toobe able tooregister an app running on hello iOS or Android platforms.</span></span> <span data-ttu-id="cb42c-448">Další informace najdete v tématu hello pro vaši platformu:</span><span class="sxs-lookup"><span data-stu-id="cb42c-448">For more information, see hello topic for your platform:</span></span>

* [<span data-ttu-id="cb42c-449">Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="cb42c-449">Xamarin.Android</span></span>](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [<span data-ttu-id="cb42c-450">Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="cb42c-450">Xamarin.iOS</span></span>](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <span data-ttu-id="cb42c-451"><a name="register-xplat"></a>Postupy: registrace nabízených šablony toosend napříč platformami oznámení</span><span class="sxs-lookup"><span data-stu-id="cb42c-451"><a name="register-xplat"></a>How to: Register push templates toosend cross-platform notifications</span></span>
<span data-ttu-id="cb42c-452">tooregister šablony, použijte hello `RegisterAsync()` metoda šablonami hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cb42c-452">tooregister templates, use hello `RegisterAsync()` method with hello templates, as follows:</span></span>

```
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

<span data-ttu-id="cb42c-453">Šablony by měla být `JObject` typy a může obsahovat několik šablon v hello formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="cb42c-453">Your templates should be `JObject` types and can contain multiple templates in hello following JSON format:</span></span>

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

<span data-ttu-id="cb42c-454">Hello metoda **RegisterAsync()** přijme také sekundární dlaždice:</span><span class="sxs-lookup"><span data-stu-id="cb42c-454">hello method **RegisterAsync()** also accepts Secondary Tiles:</span></span>

```
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

<span data-ttu-id="cb42c-455">Všechny značky se odstraní a rychle během registrace pro zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="cb42c-455">All tags are stripped away during registration for security.</span></span> <span data-ttu-id="cb42c-456">tooadd značky tooinstallations nebo šablony v rámci instalace naleznete v tématu [práci s hello .NET back-end serveru SDK pro Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="cb42c-456">tooadd tags tooinstallations or templates within installations, see [Work with hello .NET backend server SDK for Azure Mobile Apps].</span></span>

<span data-ttu-id="cb42c-457">použití těchto registrovaných šablon oznámení toosend odkazovat toohello [rozhraní API centra oznámení].</span><span class="sxs-lookup"><span data-stu-id="cb42c-457">toosend notifications utilizing these registered templates, refer toohello [Notification Hubs APIs].</span></span>

## <span data-ttu-id="cb42c-458"><a name="misc"></a>Ostatní témata</span><span class="sxs-lookup"><span data-stu-id="cb42c-458"><a name="misc"></a>Miscellaneous Topics</span></span>
### <span data-ttu-id="cb42c-459"><a name="errors"></a>Postupy: zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="cb42c-459"><a name="errors"></a>How to: Handle errors</span></span>
<span data-ttu-id="cb42c-460">V případě chyby v back-end hello hello klienta SDK vyvolá `MobileServiceInvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="cb42c-460">When an error occurs in hello backend, hello client SDK raises a `MobileServiceInvalidOperationException`.</span></span>  <span data-ttu-id="cb42c-461">Následující příklad ukazuje způsob toohandle výjimka, která je vrácený back-end hello:</span><span class="sxs-lookup"><span data-stu-id="cb42c-461">The following example shows how toohandle an exception that is returned by hello backend:</span></span>

```
private async void InsertTodoItem(TodoItem todoItem)
{
    // This code inserts a new TodoItem into hello database. When hello operation completes
    // and App Service has assigned an Id, hello item is added toohello CollectionView
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

<span data-ttu-id="cb42c-462">Další příklad práci s chybové stavy lze nalézt v hello [ukázkové soubory aplikací Mobile].</span><span class="sxs-lookup"><span data-stu-id="cb42c-462">Another example of dealing with error conditions can be found in hello [Mobile Apps Files Sample].</span></span> <span data-ttu-id="cb42c-463">[LoggingHandler] příklad poskytuje delegáta protokolování obslužná rutina toolog hello požadavky prováděné toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="cb42c-463">The [LoggingHandler] example provides a logging delegate handler toolog hello requests being made toohello backend.</span></span>

### <span data-ttu-id="cb42c-464"><a name="headers"></a>Postupy: přizpůsobení hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="cb42c-464"><a name="headers"></a>How to: Customize request headers</span></span>
<span data-ttu-id="cb42c-465">toosupport váš scénář konkrétní aplikaci, bude pravděpodobně nutné toocustomize komunikaci s back-end mobilní aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="cb42c-465">toosupport your specific app scenario, you might need toocustomize communication with hello Mobile App backend.</span></span> <span data-ttu-id="cb42c-466">Může například chcete tooadd žádost odchozí tooevery vlastní hlavičky nebo dokonce změnit kódy stavu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="cb42c-466">For example, you may want tooadd a custom header tooevery outgoing request or even change responses status codes.</span></span> <span data-ttu-id="cb42c-467">Můžete použít vlastní [DelegatingHandler], jako v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="cb42c-467">You can use a custom [DelegatingHandler], as in hello following example:</span></span>

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
        // Change hello request-side here based on hello HttpRequestMessage
        request.Headers.Add("x-my-header", "my value");

        // Do hello request
        var response = await base.SendAsync(request, cancellationToken);

        // Change hello response-side here based on hello HttpResponseMessage

        // Return hello modified response
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

[přidat ověřování tooyour aplikace]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Offline synchronizací dat v Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[přidat nabízená oznámení tooyour aplikace]: app-service-mobile-windows-store-dotnet-get-started-push.md
[registraci vaší aplikace toouse přihlašovací údaje účtu Microsoft]: app-service-mobile-how-to-configure-microsoft-authentication.md
[jak tooconfigure aplikaci služby pro služby Active Directory přihlášení]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[ MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[Funkce GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[vytvoří odkaz na tabulku netypové tooan]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[trvat]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[vyberte]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[přeskočit]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[metod UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[ID uživatele]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[kde]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[portál Azure]: https://portal.azure.com/
[portál Azure classic]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windows Dev Center]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[rozhraní API centra oznámení]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[ukázkové soubory aplikací Mobile]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3 dokumentaci]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments
[ContosoMoments photo sharing sample]: https://github.com/azure-appservice-samples/ContosoMoments
