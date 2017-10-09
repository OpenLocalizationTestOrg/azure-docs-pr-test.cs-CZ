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
# <a name="how-toouse-hello-managed-client-for-azure-mobile-apps"></a>Jak toouse hello spravovat klienta pro Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak tooperform běžné scénáře s využitím hello spravované klientské knihovny pro aplikace Azure App Service mobilní aplikace pro Windows a Xamarin. Pokud jste nové aplikace tooMobile, měli byste zvážit první dokončuje hello [rychlý start Azure Mobile Apps] [ 1] kurzu. V této příručce se zaměříme na hello spravované klientské sady SDK. Další informace o hello serverové sady SDK pro Mobile Apps, naleznete v dokumentaci k hello hello toolearn [.NET serveru SDK] [ 2] nebo [Node.js Server SDK] [ 3].

## <a name="reference-documentation"></a>Referenční dokumentace
Hello referenční dokumentaci k nástroji pro hello klienta SDK se nachází zde: [Azure Mobile Apps .NET – referenční informace klienta][4].
Můžete také získat několik ukázky klienta v hello [úložiště GitHub Azure-Samples][5].

## <a name="supported-platforms"></a>Podporované platformy
Hello platformy .NET podporuje hello následující platformy:

* Xamarin Android verze pro rozhraní API 19 až 24 (KitKat prostřednictvím cukrovinkách typu nugát)
* Xamarin iOS verze pro iOS verze 8.0 a novější
* Univerzální platforma pro Windows
* Windows Phone 8.1
* Windows Phone 8.0, s výjimkou aplikace Silverlight

ověřování "serveru pohybu" Hello používá webové zobrazení pro hello uvedené uživatelského rozhraní.  Pokud hello zařízení není možné toopresent webové zobrazení uživatelského rozhraní, jsou potřeba jiných metod ověřování.  Tato sada SDK není proto vhodný pro sledování type nebo podobně jako zařízení s omezeným přístupem.

## <a name="setup"></a>Instalační program a požadavky
Předpokládáme, že jste již vytvořili a publikování projektu back-end mobilní aplikace, která zahrnuje nejméně jednu tabulku.  V kódu hello použitým v tomto tématu, je název tabulky hello `TodoItem` a má hello následující sloupce: `Id`, `Text`, a `Complete`. Tato tabulka je hello stejné tabulky vytvořit při dokončení [rychlý start Azure Mobile Apps][1].

Hello odpovídající typu klienta v jazyce C# je typ hello následující třídy:

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

Hello [JsonPropertyAttribute] [ 6] je použité toodefine hello *PropertyName* mapování mezi hello klienta pole a pole tabulky hello.

toolearn jak toocreate tabulky v váš back-end mobilní aplikace, najdete v části hello [.NET serveru SDK tématu] [ 7] nebo hello [Node.js Server SDK tématu] [ 8] . Pokud jste vytvořili váš back-end mobilní aplikace v hello Azure pomocí portálu hello rychlý start, můžete také použít hello **snadno tabulky** nastavení v hello [portál Azure].

### <a name="how-to-install-hello-managed-client-sdk-package"></a>Postupy: instalace hello spravované balíček klienta SDK
Použijte jednu z následujících metod tooinstall hello hello spravovaná balíček klienta SDK pro Mobile Apps z [NuGet][9]:

* **Visual Studio** klikněte pravým tlačítkem na projekt, klikněte na tlačítko **spravovat balíčky NuGet**, vyhledejte hello `Microsoft.Azure.Mobile.Client` balíček a potom klikněte na **nainstalovat**.
* **Xamarin Studio** klikněte pravým tlačítkem na projekt, klikněte na tlačítko **přidat** > **přidání balíčků NuGet**, vyhledejte hello `Microsoft.Azure.Mobile.Client `balíček a potom klikněte na tlačítko **přidat Balíček**.

V souboru hlavní aktivitu, mějte na paměti následující hello tooadd **pomocí** příkaz:

```
using Microsoft.WindowsAzure.MobileServices;
```

### <a name="symbolsource"></a>Postupy: práce s ladicími symboly v sadě Visual Studio
Hello symboly pro obor názvů Microsoft.Azure.Mobile hello jsou k dispozici na [SymbolSource][10].  Odkazovat toothe [SymbolSource pokyny] [ 11] toointegrate SymbolSource pomocí sady Visual Studio.

## <a name="create-client"></a>Vytvoření klienta Mobile Apps hello
Hello následující kód vytvoří hello [MobileServiceClient] [ 12] objekt, který je použité tooaccess váš back-end mobilní aplikace.

```
var client = new MobileServiceClient("MOBILE_APP_URL");
```

V předchozích kód hello, nahraďte `MOBILE_APP_URL` s adresou URL hello back-end mobilní aplikace hello, který se vyskytuje v okně pro váš back-end mobilní aplikace v hello [portál Azure]. objekt MobileServiceClient Hello by měl být typu singleton.

## <a name="work-with-tables"></a>Práce s tabulkami
Následující část podrobně popisuje, jak Hello toosearch a načtení záznamů a úprava hello dat v tabulce hello.  jsou popsané Hello následující témata:

* [Vytvořit odkaz na tabulku](#instantiating)
* [Dotaz na data](#querying)
* [Filtr vrátil data](#filtering)
* [Řazení vrátil data](#sorting)
* [Vrátit data na stránkách](#paging)
* [Vyberte konkrétní sloupce](#selecting)
* [Vyhledat záznam podle Id](#lookingup)
* [Práci s netypové dotazy](#untypedqueries)
* [Vkládání dat](#inserting)
* [Aktualizace dat](#updating)
* [Odstraňování dat](#deleting)
* [Řešení konfliktů a optimistickou metodu souběžného zpracování](#optimisticconcurrency)
* [Vazba tooa uživatelské rozhraní Windows](#binding)
* [Změna hello velikost stránky](#pagesize)

### <a name="instantiating"></a>Postupy: vytvoření odkaz na tabulku
Všechny hello kód, který přistupuje k nebo upravuje data v tabulce back-end volání funkce na hello `MobileServiceTable` objektu. Získejte odkaz na tabulku toohello volání hello [Funkce GetTable] metoda následujícím způsobem:

```
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

Hello vrátit objekt používá model typovaná serializace hello. Model netypové serializace je také podporována. Následující příklad [vytvoří odkaz na tabulku netypové tooan]:

```
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

V netypové dotazy je nutné zadat hello základní řetězec dotazu OData.

### <a name="querying"></a>Postupy: dotaz na data z mobilní aplikace
Tato část popisuje, jak tooissue dotazuje toohello mobilní back-end aplikace, která zahrnuje hello následující funkce:

* [Filtr vrátil data](#filtering)
* [Řazení vrátil data](#sorting)
* [Vrátit data na stránkách](#paging)
* [Vyberte konkrétní sloupce](#selecting)
* [Vyhledávání dat podle ID](#lookingup)

> [!NOTE]
> Velikost stránky řízené serveru je vynucené tooprevent všechny řádky nebyl vrácen.  Stránkování zachová výchozí požadavky pro velké sady dat z mělo negativní dopad na hello služby.  tooreturn více než 50 řádky, použijte hello `Skip` a `Take` metoda, jak je popsáno v [vracet data na stránkách](#paging).

### <a name="filtering"></a>Postupy: Filtr nevrátil data
Hello následující kódu ukazuje, jak toofilter dat včetně `Where` klauzuli v dotazu. Vrátí všechny položky z `todoTable` jejichž `Complete` vlastnost je rovna příliš`false`. Hello [kde] funkce se vztahuje řádek filtrování predikátu k hello dotaz hello tabulky.

```
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

Můžete zobrazit hello URI hello požadavek odeslaný toohello back-end pomocí zpráv kontroly software, například nástroje pro vývojáře prohlížeče nebo [Fiddler]. Pokud se podíváte na URI požadavku hello, Všimněte si, že je řetězec dotazu hello upravit:

```
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

Tento požadavek OData je přeložen do dotazu SQL hello Server SDK:

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

Hello funkce, která je předána toohello `Where` metoda může mít libovolný počet podmínky.

```
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

V tomto příkladu by přeložen hello Server SDK do dotazu SQL:

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

Tento dotaz taky dají rozdělit do více klauzulí:

```
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

Hello dvě metody jsou ekvivalentní a může je použít zcela zaměnitelným významem.  Hello bývalé možnost&mdash;z zřetězení více predikáty v jednom dotazu&mdash;je kompaktnější a doporučené.

Hello `Where` klauzule podporuje operace, které se přeložit na podmnožinu hello OData. Operace zahrnují:

* Relační operátory (==,! =, <, < =, >, > =),
* Aritmetické operátory (+, -, /, *, %),
* Číslo přesnost (Math.Floor, Math.Ceiling)
* Funkce řetězce (délka, Substring, nahraďte, IndexOf, StartsWith, EndsWith),
* Vlastnosti kalendářních dat (rok, měsíc, den, hodinu, minutu, sekundu),
* Přístup k vlastnosti objektu, a
* Výrazy kombinování kteroukoli z těchto operací.

Pokud vzhledem k tomu, co hello serveru SDK podporuje, můžete zvážit hello [OData v3 dokumentaci].

### <a name="sorting"></a>Postupy: řazení vrátil data
Hello následující kódu ukazuje, jak toosort dat včetně [OrderBy] nebo [OrderByDescending] funkce v dotazu hello. Vrátí položky z `todoTable` seřadit vzestupně podle hello `Text` pole.

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

### <a name="paging"></a>Postupy: vracet data na stránkách
Ve výchozím nastavení back-end hello vrátí hello pouze prvních 50 řádky. Můžete zvýšit hello počet vrácených řádků ve volání hello [trvat] metoda. Použití `Take` společně s hello [přeskočit] metoda toorequest konkrétní "stránky" hello celkový sady dat vrácených dotazem hello. Hello následující dotaz, při spuštění, vrátí hello první tři položky v tabulce hello.

```
// Define a filtered query that returns hello top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

Hello následující přeskočí revidovaný dotazu hello první tři výsledky a vrátí hello následující tři výsledky. Tento dotaz vytvoří hello druhý "stránka" dat, kde je velikost stránky hello tři položky.

```
// Define a filtered query that skips hello top 3 items and returns hello next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

Hello [IncludeTotalCount] metoda požadavky hello celkový počet pro *všechny* hello záznamy, které by byly vráceny, ignoruje všechny klauzuli stránkování/limit zadat:

```
query = query.IncludeTotalCount();
```

Ve skutečných aplikaci můžete použít dotazy podobné toohello předcházející příklad s na pager ovládací prvek nebo srovnatelný uživatelského rozhraní navigace mezi stránkami.

> [!NOTE]
> limit 50 řádek toooverride hello v back-end mobilní aplikace, musíte také použít hello [EnableQueryAttribute] toohello veřejných GET – metoda a zadejte hello stránkování. Když metoda použitá toohello, hello následující nastaví too1000 maximální počet vrácených řádků:
>
> `[EnableQuery(MaxTop=1000)]`


### <a name="selecting"></a>Postupy: výběr konkrétní sloupců
Můžete zadat, která sada tooinclude vlastnosti v hello výsledků přidáním [vyberte] klauzule tooyour dotazu. Například hello následující kód ukazuje, jak tooselect právě jedno pole a také jak tooselect a formátování více polí:

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

Všechny hello funkce popsané, pokud jsou sčítání, takže jsme můžete zachovat řetězení je. Každý zřetězené volání ovlivňuje více hello dotazu. Další příklad:

```
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <a name="lookingup"></a>Postupy: vyhledávání dat podle ID
Hello [LookupAsync] funkce lze použít toolook objektů z databáze hello s konkrétním ID.

```
// This query filters out hello item with hello ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <a name="untypedqueries"></a>Postupy: provádění dotazů bez typu
Při provádění dotazu pomocí objektu bez typu tabulky, je nutné explicitně zadat řetězec dotazu OData hello voláním [ReadAsync], jako v hello následující ukázka:

```
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

Zobrazí zpět JSON hodnoty, které lze používat jako kontejner objektů. Další informace o JToken a Newtonsoft Json.NET, najdete v části hello [Json.NET] lokality.

### <a name="inserting"></a>Postupy: vložení dat do back-end mobilní aplikace
Všechny typy klientů musí obsahovat člen s názvem **Id**, což je ve výchozím nastavení řetězec. To **Id** je potřebná k provedení operace CRUD a pro offline synchronizace. hello následující kód ukazuje, jak toouse hello [InsertAsync] metoda tooinsert nové řádky do tabulky. Parametr Hello obsahuje toobe hello data vložit jako objekt .NET.

```
await todoTable.InsertAsync(todoItem);
```

Pokud hodnotu jedinečné ID vlastní není součástí hello `todoItem` při vložení, je identifikátor GUID generovaný serverem hello.
Můžete načíst hello vygeneruje Id zkontrolováním hello objektu po volání hello.

tooinsert netypová dat, může využít výhod Json.NET:

```
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

Tady je příklad pomocí e-mailovou adresu jako id jedinečného řetězce:

```
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a>Práce s ID hodnoty
Mobilní aplikace podporuje vlastní řetězec jedinečné hodnoty pro tabulku hello **id** sloupce. Hodnotu řetězce umožňuje aplikacím toouse vlastní hodnoty, jako je například e-mailové adresy nebo uživatelské jméno pro ID hello.  Řetězec ID poskytnout hello následující výhody:

* ID jsou generovány bez provedení databáze toohello odezvy.
* Záznamy jsou jednodušší toomerge z různých tabulek nebo databází.
* Hodnoty ID umožňuje lepší integraci s logiku aplikace.

Pokud řetězcová hodnota ID není nastavená na vloženého záznamu, back-end mobilní aplikace hello generuje jedinečnou hodnotu pro ID. Můžete použít hello [Guid.NewGuid] toogenerate metoda vlastní ID hodnoty na hello klienta nebo v back-end hello.

```
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <a name="modifying"></a>Postupy: Změna dat v back-end mobilní aplikace
Hello následující kódu ukazuje, jak toouse hello [metod UpdateAsync] tooupdate metoda existujícího záznamu s hello stejné ID se novými informacemi. Parametr Hello obsahuje toobe data hello aktualizovat jako objekt .NET.

```
await todoTable.UpdateAsync(todoItem);
```

tooupdate netypová dat, může využít výhod [Json.NET] následujícím způsobem:

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

`id` Je nutné zadat při aktualizaci. back-end Hello používá hello `id` tooidentify pole, která řádek tooupdate. Hello `id` pole lze získat z výsledku hello hello `InsertAsync` volání. `ArgumentException` Se vyvolá, pokud se pokusíte tooupdate položky bez zadání hello `id` hodnotu.

### <a name="deleting"></a>Postupy: odstranění dat v back-end mobilní aplikace
Hello následující kódu ukazuje, jak toouse hello [DeleteAsync] metoda toodelete existující instanci. Hello instance je identifikována hello `id` pole je nastaven na hello `todoItem`.

```
await todoTable.DeleteAsync(todoItem);
```

toodelete netypová dat, může využít výhod Json.NET následujícím způsobem:

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

Pokud zadáte požadavek delete, musí být zadaná ID. Ostatní vlastnosti nejsou předán toohello služby nebo jsou ignorovány u služby hello. Hello výsledek `DeleteAsync` volání je obvykle `null`. ID toopass Hello v lze získat z výsledku hello hello `InsertAsync` volání. A `MobileServiceInvalidOperationException` se vyvolá, když zkusíte toodelete položky bez zadání hello `id` pole.

### <a name="optimisticconcurrency"></a>Postupy: použití optimistickou metodu souběžného pro řešení konfliktů.
Dvě nebo více klientů může zapisovat změny toohello stejné položky v hello stejný čas. Bez zjišťování konfliktů by poslední zápis hello přepsat všechny předchozí aktualizace. **Optimistické řízení souběžného** předpokládá, že každou transakci můžete potvrdit a proto nepoužívá žádné uzamčení prostředků.  Před potvrzením transakce, optimistické řízení souběžného ověřuje, že neexistují žádné další transakce změnil hello data. Pokud hello data byl upraven, hello potvrzování transakce bude vrácena zpět.

Mobilní aplikace podporuje optimistické řízení souběžného pomocí funkce sledování změn tooeach položky pomocí hello `version` sloupec vlastností systému, která je definována pro každou tabulku v váš back-end mobilní aplikace. Pokaždé, když aktualizaci záznamu Mobile Apps nastaví hello `version` vlastnost pro tento záznam tooa novou hodnotu. Při každé žádosti o aktualizaci hello `version` vlastnost záznamu hello součástí požadavku hello je porovnání toohello stejnou vlastnost pro hello záznam na serveru hello. Pokud verze byla dokončena s hello žádosti neodpovídá hello back-end, pak vyvolá hello klientské knihovny `MobileServicePreconditionFailedException<T>` výjimka. Typ Hello součástí hello výjimka je záznam hello z hello back-end obsahující hello verze servery hello záznamu. Hello aplikace pak může použít tento toodecide informace o tom, jestli tooexecute hello aktualizaci požadavků zopakovat se správnou hello `version` hodnotu z hello back-end toocommit změny.

Definovat sloupec na třídu hello tabulky pro hello `version` optimistickou metodu souběžného tooenable systému vlastnost. Například:

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

Aplikace, které používají bez typu tabulky povolte optimistickou metodu souběžného tak, že nastavení hello `Version` příznak na `SystemProperties` z hello tabulky následujícím způsobem.

```
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

V přidání tooenabling optimistickou metodu souběžného, musí také catch hello `MobileServicePreconditionFailedException<T>` výjimka ve vašem kódu při volání metody [metod UpdateAsync].  Hello konflikt vyřešit použitím hello správné `version` toothe aktualizovat záznam a volání [metod UpdateAsync] s hello vyřešil záznam. Hello následující kód ukazuje, jak tooresolve zápisu jednou zjištěn konflikt:

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

Další informace najdete v tématu hello [Offline synchronizací dat v Azure Mobile Apps] tématu.

### <a name="binding"></a>Postupy: uživatelské rozhraní tooa Windows Mobile Apps vazby dat
Tato část uvádí, jak toodisplay vrácená data objektů s použitím prvky uživatelského rozhraní v aplikaci pro Windows.  Následující příklad kódu vytvoří vazbu toohello zdroj seznamu hello pomocí dotazu pro neúplné položky. [MobileServiceCollection] vytvoří kolekci mobilní aplikace využívající technologii vazby.

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

Podpora modulu runtime rozhraní nazývá spravovaného některé ovládací prvky v hello [ISupportIncrementalLoading]. Toto rozhraní umožňuje ovládací prvky toorequest doplňující data, když uživatel hello posune. Je integrovanou podporu pro toto rozhraní pro univerzální aplikace pro Windows prostřednictvím [MobileServiceIncrementalLoadingCollection], který automaticky zpracovává volání z ovládacích prvků hello. Použití `MobileServiceIncrementalLoadingCollection` v aplikacích Windows následujícím způsobem:

```
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

Nová kolekce v aplikacích Windows Phone 8 a "Silverlight", použijte hello hello toouse `ToCollection` rozšiřující metody na `IMobileServiceTableQuery<T>` a `IMobileServiceTable<T>`. volání tooload data `LoadMoreItemsAsync()`.

```
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

Při použití hello kolekce vytvořená voláním `ToCollectionAsync` nebo `ToCollection`, získání kolekce, která může být ovládací prvky vázané tooUI.  Tato kolekce je deklaracemi stránkování.  Vzhledem k tomu, že kolekce hello je načítání dat ze sítě, se někdy načítání nezdaří. toohandle takových selhání přepsat hello `OnException` metodu `MobileServiceIncrementalLoadingCollection` toohandle výjimky vyplývající z volání příliš`LoadMoreItemsAsync`.

Zvažte, pokud tabulka obsahuje mnoho polí, ale chcete jenom toodisplay některé z nich vlastního ovládacího prvku. Můžete použít pokyny hello v předcházející části hello "[vyberte konkrétní sloupce](#selecting)" tooselect toodisplay určité sloupce v hello uživatelského rozhraní.

### <a name="pagesize"></a>Hello změnit velikost stránky
Azure Mobile Apps vrací maximálně 50 položek na požadavek ve výchozím nastavení.  Zvýšením hello maximální velikost stránky hello klienta i serveru, můžete změnit velikost stránkovacího hello.  tooincrease hello velikost požadované stránce, zadejte `PullOptions` při použití `PullAsync()`:

```
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

Za předpokladu, že jste udělali hello `PageSize` rovna tooor vyšší než 100 v rámci serveru hello, žádost o vrátí až 100 položek.

## <a name="#offlinesync"></a>Práce s tabulkami Offline
Offline tabulky použít data toostore místní úložiště SQLite pro použití v režimu offline.  Všechny tabulky, které se provádějí operace proti hello místní SQLite ukládat místo hello vzdáleného serveru úložiště.  toocreate offline tabulky, nejdřív připravit projektu:

1. V sadě Visual Studio, klikněte pravým tlačítkem na řešení hello > **spravovat balíčky NuGet pro řešení...** , vyhledejte a nainstalujte **Microsoft.Azure.Mobile.Client.SQLiteStore** balíček NuGet pro všechny projekty v řešení hello.
2. (Volitelné) toosupport zařízení se systémem Windows, instalovat jeden z následujících balíčků runtime SQLite hello:

   * **Modul Runtime pro Windows 8.1:** nainstalovat [SQLite pro Windows 8.1][3].
   * **Windows Phone 8.1:** nainstalovat [SQLite pro Windows Phone 8.1][4].
   * **Univerzální platformu Windows** nainstalovat [SQLite pro Universal Windows hello][5].
3. (Volitelné). Zařízení se systémem Windows, klikněte na tlačítko **odkazy** > **přidat odkaz na...** , rozbalte položku hello **Windows** složky > **rozšíření**, povolíte hello odpovídající **SQLite pro systém Windows** SDK společně s hello  **Visual C++ Runtime 2013 pro Windows** SDK.
    Hello SQLite SDK názvy jsou mírně odlišné s každou platformu Windows.

Aby bylo možné vytvořit odkaz na tabulku, musí být připraveno hello místního úložiště:

```
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes hello SyncContext using hello default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

Inicializace úložiště se obvykle provádí ihned po vytvoření klienta hello.  Hello **OfflineDbPath** by měla být název souboru vhodné pro použití na všech platformách, které podporujete.  Pokud se cesta hello plně kvalifikovanou cestu (tedy začíná lomítko), je použita v této cestě.  Pokud hello cesta není úplná, hello soubor je umístěn v umístění specifické pro platformu.

* Pro zařízení s iOS a Android hello výchozí cesta je složka "Osobní soubory" hello.
* U zařízení s Windows hello výchozí cesta je složka "data aplikací" hello specifické pro aplikaci.

Odkaz na tabulku můžete získat pomocí hello `GetSyncTable<>` metoda:

```
var table = client.GetSyncTable<TodoItem>();
```

Není nutné tooauthenticate toouse offline tabulky.  Potřebujete jenom tooauthenticate při jsou komunikaci s hello back-end službu.

### <a name="syncoffline"></a>Synchronizace Offline tabulky
Offline tabulky nejsou synchronizované s back-end hello ve výchozím nastavení.  Synchronizace je rozdělit na dvě části.  Doručte změny můžete samostatně stahování nové položky.  Tady je metoda typické synchronizace:

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

Pokud hello první argument příliš`PullAsync` má hodnotu null, není použita přírůstkové synchronizace.  Každé operace synchronizace načte všechny záznamy.

provede Hello SDK implicitní `PushAsync()` než vytažení záznamů.

Zpracování konfliktů dochází `PullAsync()` metoda.  Můžete můžete řešit konflikty v hello stejný způsob jako online tabulky.  vytváří Hello konflikt při `PullAsync()` je volána místo v průběhu hello insert, update nebo delete. Pokud je v konfliktu více, jsou seskupeny do jednoho MobileServicePushFailedException.  Zpracování každé selhání samostatně.

## <a name="#customapi"></a>Práce s vlastní rozhraní API
Vlastní rozhraní API umožňuje toodefine vlastní koncové body, které zveřejňují funkce serveru které není mapování tooan vložení, aktualizaci, odstranění nebo operace čtení. Pomocí vlastního rozhraní API, může mít větší kontrolu nad zasílání zpráv, včetně čtení a nastavení hlavičky protokolu HTTP zpráv a definování formátu textu zprávy kromě formátu JSON.

Volání vlastní rozhraní API pomocí volání mezi hello [InvokeApiAsync] metody na klientovi hello. Například následující řádek kódu hello odešle toohello požadavek POST **completeAll** rozhraní API na back-end hello:

```
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

Tento formulář je typu metoda volání a vyžaduje, že hello **MarkAllResult** vrátí typ je definován. Typové i bez typu metody jsou podporovány.

Hello InvokeApiAsync() metoda přidá/api/toohello rozhraní API chcete toocall, pokud začíná hello rozhraní API lomítkem (/).
Například:

* `InvokeApiAsync("completeAll",...)`volání /api/completeAll na back-end hello
* `InvokeApiAsync("/.auth/me",...)`volání /.auth/me na back-end hello

Můžete vytvořit InvokeApiAsync toocall žádné WebAPI, včetně těchto WebAPIs, které nejsou definovány s Azure Mobile Apps.  Při použití InvokeApiAsync() hello odpovídající hlavičky, včetně ověřování hlavičky, odešlou hello požadavku.

## <a name="authentication"></a>Ověřování uživatelů
Mobilní aplikace podporuje ověřování a autorizaci uživatelů aplikace pomocí různých zprostředkovatelů externí identity: Facebook, Google, Microsoft Account, Twitter a Azure Active Directory. Oprávnění můžete nastavit na tabulky toorestrict přístup pro určité operace tooonly ověřeného uživatele. Můžete taky hello identity ověřené uživatele tooimplement autorizačních pravidel ve skriptech serveru. Další informace najdete v tématu hello kurzu [přidat ověřování tooyour aplikace].

Jsou podporovány dva ověřování toky: *klienta spravovat* a *server spravovaný* toku. tok spravovaných server Hello poskytuje hello nejjednodušší ověřování, jako je závislé na hello zprostředkovatele webového ověření rozhraní. tok spravovaných klienta Hello umožňuje hlubší integrace s funkcemi konkrétní zařízení jako přitom spoléhá na konkrétní zařízení specifické pro poskytovatele sady SDK.

> [!NOTE]
> Doporučujeme používat k klienta spravovat toku ve svých aplikacích produkční.

tooset až ověřování, je nutné zaregistrovat aplikaci s jeden nebo více poskytovatelů identity.  zprostředkovatele identity Hello vygeneruje ID klienta a tajný klíč klienta pro vaši aplikaci.  Tyto hodnoty jsou pak nastavené ve vašem back-end tooenable ověřování/autorizace služby Azure App Service.  Další informace, postupujte podle hello podrobné pokyny v tomto kurzu [přidat ověřování tooyour aplikace].

v této části jsou popsané Hello následující témata:

* [Klienta spravovat ověřování](#clientflow)
* [Spravovaný server ověřování](#serverflow)
* [Ukládání do mezipaměti hello ověřovací token](#caching)

### <a name="clientflow"></a>Klienta spravovat ověřování
Aplikace můžete nezávisle obraťte se na zprostředkovatele identity hello a pak zadejte token vrácený hello během přihlašování s váš back-end. Tento tok klienta umožňuje tooprovide jednom přihlášení pro uživatele nebo tooretrieve další uživatelské dat od poskytovatele identity hello. Tok ověřování klientů je upřednostňovaný toousing toku serveru jako zprostředkovatele identity hello SDK poskytuje více nativní UX chování a umožňuje další přizpůsobení.

Příklady jsou k dispozici pro hello vzory klienta tok ověřování následující:

* [Knihovna ověřování Active Directory](#adal)
* [Facebook nebo Google](#client-facebook)
* [Live SDK](#client-livesdk)

#### <a name="adal"></a>Ověřuje uživatele pomocí hello knihovna ověřování Active Directory
Můžete použít hello Active Directory Authentication Library (ADAL) tooinitiate ověřování uživatelů z klienta hello pomocí ověřování Azure Active Directory.

1. Nakonfigurujte následující hello váš back-end mobilní aplikace pro přihlašování AAD [jak tooconfigure aplikaci služby pro služby Active Directory přihlášení] kurzu. Ujistěte se, že toocomplete hello volitelný krok registrace nativní klientskou aplikaci.
2. V sadě Visual Studio nebo Xamarin Studio, otevřete projekt a přidejte odkaz na toothe `Microsoft.IdentityModel.CLients.ActiveDirectory` balíček NuGet. Při hledání, zahrňte předběžné verze.
3. Přidejte hello následující aplikaci tooyour kódu, podle toohello platformu, kterou používáte. V každé zkontrolujte následující náhrady hello:

   * Nahraďte **INSERT. AUTORITY zde** s názvem hello hello klienta, ve kterém jste zřídili vaší aplikace. Formát by měl být https://login.microsoftonline.com/contoso.onmicrosoft.com. Tuto hodnotu lze kopírovat z karty hello domény v Azure Active Directory v hello [portál Azure classic].
   * Nahraďte **INSERT-RESOURCE-ID-zde** s ID klienta hello back-endu mobilní aplikace. ID klienta hello můžete získat z hello **Upřesnit** v části **nastavení Azure Active Directory** hello portálu.
   * Nahraďte **INSERT klienta ID zde** s ID klienta hello jste zkopírovali ze hello nativní klientskou aplikaci.
   * Nahraďte **vložení PŘESMĚROVÁNÍ URI zde** s vaší lokality */.auth/login/done* koncový bod, pomocí hello schéma HTTPS. Tato hodnota by mělo být podobné příliš*https://contoso.azurewebsites.net/.auth/login/done*.

     Následuje Hello kód potřebný pro každou platformu:

     **Windows:**

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

     **Xamarin.iOS**

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

     **Xamarin.Android**

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

#### <a name="client-facebook"></a>Jednotné přihlašování pomocí tokenu z Facebook nebo Google
Tok hello klienta můžete použít, jak je uvedeno v této fragmentu kódu pro Facebook nebo Google.

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

#### <a name="client-livesdk"></a>Jednotné přihlašování pomocí Account Microsoft hello Live SDK
tooauthenticate uživatele, je nutné zaregistrovat aplikaci v hello účtu Microsoft Developer Center. Nakonfigurujte podrobnosti registrace na váš back-end mobilní aplikace. toocreate Microsoft registrace účtu a připojte ho back-end mobilní aplikace tooyour, dokončení hello kroky [registraci vaší aplikace toouse přihlašovací údaje účtu Microsoft]. Pokud máte Windows Store a Windows Phone 8 nebo Silverlight verze aplikace, nejprve zaregistrujte hello verze Windows Store.

Hello následující kód se ověří pomocí Live SDK a používá token toosign, vrátí se v back-end mobilní aplikace tooyour hello.

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

Další informace najdete v tématu hello [Windows Live SDK] dokumentaci.

### <a name="serverflow"></a>Spravovaný server ověřování
Po registraci zprostředkovatele identity volání hello [LoginAsync] metodu hello [MobileServiceClient] s hello [MobileServiceAuthenticationProvider] hodnotu svého poskytovatele. Například hello následující kód spustí serveru toku přihlášení pomocí Facebooku.

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

Pokud používáte zprostředkovatele identity než Facebook, změňte hodnotu hello [MobileServiceAuthenticationProvider] toohello hodnota pro zprostředkovatele.

V toku server Azure App Service spravuje tok ověřování OAuth hello zobrazením hello přihlašovací stránku hello vybraného zprostředkovatele.  Vrátí zprostředkovatele hello identity, služby Azure App Service generuje jednou ověřovací token služby App Service. Hello [LoginAsync] metoda vrátí [MobileServiceUser], který poskytuje i hello [UserId] z hello ověřený uživatel a hello [ MobileServiceAuthenticationToken], jako webového tokenu JSON (JWT). Tento token se může uložit do mezipaměti a znovu požívat do vypršení platnosti. Další informace najdete v tématu [ukládání do mezipaměti hello ověřovací token](#caching).

### <a name="caching"></a>Ukládání do mezipaměti hello ověřovací token
V některých případech hello volání toohello přihlášení metoda můžete zabránit po prvním úspěšném ověření hello ukládání hello ověřovací token od zprostředkovatele hello.  Aplikace Windows Store a UWP můžete použít [PasswordVault] toocache aktuální ověřování tokenu po úspěšného přihlášení, následujícím způsobem:

```
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

Hello UserId hodnota je uložena jako hello uživatelské jméno pověření hello a hello token je hello uložené jako hello heslo. Na následujících podniky, můžete zkontrolovat hello **PasswordVault** pro přihlašovací údaje v mezipaměti. Hello následující příklad používá pověření uložená v mezipaměti při jejich zjištění a v opačném případě se pokusí tooauthenticate znovu s back-end hello:

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

Při odhlášení uživatele, musíte také odebrat hello uložené přihlašovací údaje, následujícím způsobem:

```
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

Pomocí aplikace Xamarin hello [Xamarin.Auth] údaje k úložišti toosecurely rozhraní API v **účet** objektu. Příklad použití tato rozhraní API, naleznete v části hello [AuthStore.cs] souboru kódu v hello [ContosoMoments fotografií sdílení ukázka](https://github.com/azure-appservice-samples/ContosoMoments).

Pokud používáte klienta spravovat ověřování, můžete také mezipaměti hello přístupový token zajišťuje poskytovatel například Facebook nebo Twitter. Tento token může být zadaný toorequest nový token ověřování z back-end hello, následujícím způsobem:

```
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using hello access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <a name="pushnotifications"></a>Nabízená oznámení
Hello následujících tématech najdete nabízených oznámení:

* [Registrace pro nabízená oznámení](#register-for-push)
* [Získat identifikátor SID balíčku Windows Store](#package-sid)
* [Zaregistrovat se šablonami a platformy](#register-xplat)

### <a name="register-for-push"></a>Postupy: registrace pro nabízená oznámení
Hello Mobile Apps client umožňuje tooregister pro nabízená oznámení pomocí Azure Notification Hubs. Při registraci, můžete získat popisovač, které jste získali od hello specifické platformy služby nabízených oznámení (PNS). Pak poskytnete tuto hodnotu společně s všechny značky, když vytvoříte hello registrace. Hello následující kód zaregistruje aplikace pro Windows pro nabízená oznámení se hello služby oznámení Windows (WNS):

```
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using hello new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

Pokud nabízíte tooWNS, pak musíte [získat identifikátor SID balíčku Windows Store](#package-sid).  Další informace o aplikace pro Windows včetně toho, jak zjistit, tooregister pro šablony registrace [přidat nabízená oznámení tooyour aplikace].

Požaduje značky z hello klienta není podporována.  Značka požadavky jsou bezobslužně vyřadit z registrace.
Pokud chcete tooregister zařízení pomocí značek, vytvořte vlastní rozhraní API, které používá hello API centra oznámení tooperform hello registrace vaším jménem.  [Volání rozhraní API vlastní hello](#customapi) místo hello `RegisterNativeAsync()` metoda.

### <a name="package-sid"></a>Postupy: získání SID balíčku Windows Store
SID balíčku je nutné pro povolení nabízených oznámení v aplikacích pro Windows Store.  tooreceive balíček SID, registrace vaší aplikace s hello Windows Store.

tooobtain tuto hodnotu:

1. V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na projekt aplikace Windows Store hello, klikněte na tlačítko **úložiště** > **propojit aplikaci se hello úložiště...** .
2. V Průvodci hello, klikněte na tlačítko **Další**, přihlaste se pomocí účtu Microsoft, zadejte název aplikace v rámci **rezervovat nový název aplikace**, pak klikněte na tlačítko **rezervy**.
3. Po registraci aplikace hello je hello úspěšně vytvořil, vyberte název aplikace, klikněte na tlačítko **Další**a potom klikněte na **přidružit**.
4. Přihlaste se toohello [Windows Dev Center] pomocí Account Microsoft. V části **Moje aplikace**, klikněte na možnost registrace aplikace hello jste vytvořili.
5. Klikněte na tlačítko **správy aplikací** > **identity aplikace**a potom přejděte dolů na toofind vaše **SID balíčku**.

Mnoho používá SID balíčku hello s nimi zacházet jako identifikátor URI, v takovém případě musíte toouse *ms aplikace: / /* hello schéma. Poznamenejte si hello verzi vašeho balíčku SID tvořena zřetězením tuto hodnotu jako předponu.

Aplikace Xamarin vyžadovat některé další kód toobe možné tooregister aplikace spuštěné na hello iOS nebo Android platformy. Další informace najdete v tématu hello pro vaši platformu:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <a name="register-xplat"></a>Postupy: registrace nabízených šablony toosend napříč platformami oznámení
tooregister šablony, použijte hello `RegisterAsync()` metoda šablonami hello následujícím způsobem:

```
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

Šablony by měla být `JObject` typy a může obsahovat několik šablon v hello formátu JSON:

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

Hello metoda **RegisterAsync()** přijme také sekundární dlaždice:

```
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

Všechny značky se odstraní a rychle během registrace pro zabezpečení. tooadd značky tooinstallations nebo šablony v rámci instalace naleznete v tématu [práci s hello .NET back-end serveru SDK pro Azure Mobile Apps].

použití těchto registrovaných šablon oznámení toosend odkazovat toohello [rozhraní API centra oznámení].

## <a name="misc"></a>Ostatní témata
### <a name="errors"></a>Postupy: zpracování chyb
V případě chyby v back-end hello hello klienta SDK vyvolá `MobileServiceInvalidOperationException`.  Následující příklad ukazuje způsob toohandle výjimka, která je vrácený back-end hello:

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

Další příklad práci s chybové stavy lze nalézt v hello [ukázkové soubory aplikací Mobile]. [LoggingHandler] příklad poskytuje delegáta protokolování obslužná rutina toolog hello požadavky prováděné toohello back-end.

### <a name="headers"></a>Postupy: přizpůsobení hlavičky požadavku
toosupport váš scénář konkrétní aplikaci, bude pravděpodobně nutné toocustomize komunikaci s back-end mobilní aplikace hello. Může například chcete tooadd žádost odchozí tooevery vlastní hlavičky nebo dokonce změnit kódy stavu odpovědi. Můžete použít vlastní [DelegatingHandler], jako v hello následující ukázka:

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
