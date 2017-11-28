---
title: aaaUpgrading toohello Azure Search .NET SDK verze 1.1 | Microsoft Docs
description: Upgrade toohello Azure Search .NET SDK verze 1.1
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 66f89958-a320-4a24-87f9-69315848909f
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/11/2017
ms.author: brjohnst
ms.openlocfilehash: 291ae5731546e47b3c22c721d3552a79bdea80c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrading-toohello-azure-search-net-sdk-version-3"></a><span data-ttu-id="fc5d6-103">Upgrade toohello Azure Search .NET SDK verze 3</span><span class="sxs-lookup"><span data-stu-id="fc5d6-103">Upgrading toohello Azure Search .NET SDK version 3</span></span>
<span data-ttu-id="fc5d6-104">Pokud používáte verzi 2.0 preview nebo starší z hello [Azure Search .NET SDK](https://aka.ms/search-sdk), tento článek vám pomůže při upgradu vaší aplikace toouse verze 3.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-104">If you're using version 2.0-preview or older of hello [Azure Search .NET SDK](https://aka.ms/search-sdk), this article will help you upgrade your application toouse version 3.</span></span>

<span data-ttu-id="fc5d6-105">Další obecné návod hello SDK včetně příkladů, najdete v části [jak toouse Azure vyhledávání z aplikace .NET](search-howto-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="fc5d6-105">For a more general walkthrough of hello SDK including examples, see [How toouse Azure Search from a .NET Application](search-howto-dotnet-sdk.md).</span></span>

<span data-ttu-id="fc5d6-106">Verze 3 hello Azure Search .NET SDK obsahuje některé změny z předchozích verzí.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-106">Version 3 of hello Azure Search .NET SDK contains some changes from earlier versions.</span></span> <span data-ttu-id="fc5d6-107">Tyto jsou většinou dílčí, takže měnili kód měli vyžadovat jen minimální úsilí.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-107">These are mostly minor, so changing your code should require only minimal effort.</span></span> <span data-ttu-id="fc5d6-108">V tématu [tooupgrade kroky](#UpgradeSteps) pokyny toochange váš kód toouse hello nové verze sady SDK.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-108">See [Steps tooupgrade](#UpgradeSteps) for instructions on how toochange your code toouse hello new SDK version.</span></span>

> [!NOTE]
> <span data-ttu-id="fc5d6-109">Pokud používáte verzi 1.0.2-preview nebo starší, doporučujeme nejprve upgradovat tooversion 1.1 a pak upgradovat tooversion 3.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-109">If you're using version 1.0.2-preview or older, you should upgrade tooversion 1.1 first, and then upgrade tooversion 3.</span></span> <span data-ttu-id="fc5d6-110">V tématu [příloha: kroky tooupgrade tooversion 1.1](#UpgradeStepsV1) pokyny.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-110">See [Appendix: Steps tooupgrade tooversion 1.1](#UpgradeStepsV1) for instructions.</span></span>
>
> <span data-ttu-id="fc5d6-111">Instanci služby Azure Search podporuje několik verzí rozhraní REST API, včetně hello nejnovějšího.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-111">Your Azure Search service instance supports several REST API versions, including hello latest one.</span></span> <span data-ttu-id="fc5d6-112">Toouse verze můžete pokračovat v případě, že je už hello nejnovějšího, ale doporučujeme migraci nejnovější verze vašeho kódu toouse hello.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-112">You can continue toouse a version when it is no longer hello latest one, but we recommend that you migrate your code toouse hello newest version.</span></span> <span data-ttu-id="fc5d6-113">Při použití hello REST API, musíte zadat hello verze rozhraní API v každé žádosti o prostřednictvím parametr api-version hello.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-113">When using hello REST API, you must specify hello API version in every request via hello api-version parameter.</span></span> <span data-ttu-id="fc5d6-114">Při použití hello .NET SDK, určuje hello verzi hello SDK používáte hello odpovídající verzi systému hello REST API.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-114">When using hello .NET SDK, hello version of hello SDK you’re using determines hello corresponding version of hello REST API.</span></span> <span data-ttu-id="fc5d6-115">Pokud používáte starší SDK, můžete dál toorun tento kód se žádné změny i v případě, že je služba hello upgradovaný toosupport novější rozhraní API verze.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-115">If you are using an older SDK, you can continue toorun that code with no changes even if hello service is upgraded toosupport a newer API version.</span></span>

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a><span data-ttu-id="fc5d6-116">Co je nového v verze 3</span><span class="sxs-lookup"><span data-stu-id="fc5d6-116">What's new in version 3</span></span>
<span data-ttu-id="fc5d6-117">Verze 3 hello Azure Search .NET SDK cíle hello nejnovější obecně dostupná verze hello REST API služby Azure Search, konkrétně 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-117">Version 3 of hello Azure Search .NET SDK targets hello latest generally available version of hello Azure Search REST API, specifically 2016-09-01.</span></span> <span data-ttu-id="fc5d6-118">Díky tomu je možné toouse mnoha nových funkcí služby Azure Search z aplikace .NET, včetně hello následující:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-118">This makes it possible toouse many new features of Azure Search from a .NET application, including hello following:</span></span>

* [<span data-ttu-id="fc5d6-119">Vlastní analyzátory</span><span class="sxs-lookup"><span data-stu-id="fc5d6-119">Custom analyzers</span></span>](https://aka.ms/customanalyzers)
* <span data-ttu-id="fc5d6-120">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) a [Azure Table Storage](search-howto-indexing-azure-tables.md) podpora indexeru</span><span class="sxs-lookup"><span data-stu-id="fc5d6-120">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) and [Azure Table Storage](search-howto-indexing-azure-tables.md) indexer support</span></span>
* <span data-ttu-id="fc5d6-121">Přizpůsobení indexeru prostřednictvím [pole mapování](search-indexer-field-mappings.md)</span><span class="sxs-lookup"><span data-stu-id="fc5d6-121">Indexer customization via [field mappings](search-indexer-field-mappings.md)</span></span>
* <span data-ttu-id="fc5d6-122">Značky etag binárním rozsáhlým podporují tooenable bezpečné souběžných aktualizaci indexu definice, indexery a zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-122">ETags support tooenable safe concurrent updating of index definitions, indexers, and data sources</span></span>
* <span data-ttu-id="fc5d6-123">Podporu pro vytváření indexu pole definice deklarativně architekturu třídě modelu a použitím hello nové `FieldBuilder` třídy.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-123">Support for building index field definitions declaratively by decorating your model class and using hello new `FieldBuilder` class.</span></span>
* <span data-ttu-id="fc5d6-124">Podpora pro .NET Core a přenosné profil .NET 111</span><span class="sxs-lookup"><span data-stu-id="fc5d6-124">Support for .NET Core and .NET Portable Profile 111</span></span>

<a name="UpgradeSteps"></a>

## <a name="steps-tooupgrade"></a><span data-ttu-id="fc5d6-125">Kroky tooupgrade</span><span class="sxs-lookup"><span data-stu-id="fc5d6-125">Steps tooupgrade</span></span>
<span data-ttu-id="fc5d6-126">Nejdřív aktualizovat vaše NuGet odkaz pro `Microsoft.Azure.Search` pomocí buď hello Konzola správce balíčků NuGet nebo nástrojem pravým tlačítkem myši na vaše odkazy na projekt a výběrem "Správa NuGet balíčky..." v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-126">First, update your NuGet reference for `Microsoft.Azure.Search` using either hello NuGet Package Manager Console or by right-clicking on your project references and selecting "Manage NuGet Packages..." in Visual Studio.</span></span>

<span data-ttu-id="fc5d6-127">Jakmile NuGet stáhl hello nové balíčky a jejich závislosti, znovu sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-127">Once NuGet has downloaded hello new packages and their dependencies, rebuild your project.</span></span> <span data-ttu-id="fc5d6-128">V závislosti na tom, jak je strukturovaná kódu se může znovu sestavit úspěšně.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-128">Depending on how your code is structured, it may rebuild successfully.</span></span> <span data-ttu-id="fc5d6-129">Pokud ano, chystáte připravené toogo!</span><span class="sxs-lookup"><span data-stu-id="fc5d6-129">If so, you're ready toogo!</span></span>

<span data-ttu-id="fc5d6-130">Pokud vaše sestavení selže, měli byste vidět chyby sestavení jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-130">If your build fails, you should see a build error like hello following:</span></span>

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' too'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

<span data-ttu-id="fc5d6-131">dalším krokem Hello je toofix této chyby sestavení.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-131">hello next step is toofix this build error.</span></span> <span data-ttu-id="fc5d6-132">V tématu [nejnovější změny ve verzi 3](#ListOfChanges) podrobnosti o co způsobí, že chyba hello a jak toofix ho.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-132">See [Breaking changes in version 3](#ListOfChanges) for details on what causes hello error and how toofix it.</span></span>

<span data-ttu-id="fc5d6-133">Může se zobrazit další sestavení tooobsolete metody nebo vlastnosti související s upozorněními.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-133">You may see additional build warnings related tooobsolete methods or properties.</span></span> <span data-ttu-id="fc5d6-134">Hello upozornění bude obsahovat pokyny, které toouse místo z hello zastaralé funkce.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-134">hello warnings will include instructions on what toouse instead of hello deprecated feature.</span></span> <span data-ttu-id="fc5d6-135">Například, pokud vaše aplikace používá hello `IndexingParameters.Base64EncodeKeys` vlastnost, měli byste obdržet upozornění, která uvádí, že`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`</span><span class="sxs-lookup"><span data-stu-id="fc5d6-135">For example, if your application uses hello `IndexingParameters.Base64EncodeKeys` property, you should get a warning that says `"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`</span></span>

<span data-ttu-id="fc5d6-136">Jakmile jste vyřešili všechny chyby sestavení, můžete provádět změny tooyour aplikace tootake využívat nové funkce nechcete-li.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-136">Once you've fixed any build errors, you can make changes tooyour application tootake advantage of new functionality if you wish.</span></span> <span data-ttu-id="fc5d6-137">Nové funkce v hello SDK jsou podrobně popsané na [co je nového ve verzi 3](#WhatsNew).</span><span class="sxs-lookup"><span data-stu-id="fc5d6-137">New features in hello SDK are detailed in [What's new in version 3](#WhatsNew).</span></span>

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a><span data-ttu-id="fc5d6-138">Nejnovější změny ve verze 3</span><span class="sxs-lookup"><span data-stu-id="fc5d6-138">Breaking changes in version 3</span></span>
<span data-ttu-id="fc5d6-139">Existuje malý počet nejnovější změny ve verzi 3, které mohou vyžadovat kód změní kromě toorebuilding vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-139">There a small number of breaking changes in version 3 that may require code changes in addition toorebuilding your application.</span></span>

### <a name="indexesgetclient-return-type"></a><span data-ttu-id="fc5d6-140">Návratový typ Indexes.GetClient</span><span class="sxs-lookup"><span data-stu-id="fc5d6-140">Indexes.GetClient return type</span></span>
<span data-ttu-id="fc5d6-141">Hello `Indexes.GetClient` má nové návratový typ. metoda.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-141">hello `Indexes.GetClient` method has a new return type.</span></span> <span data-ttu-id="fc5d6-142">Dříve, vrátí `SearchIndexClient`, ale to byl změněn příliš`ISearchIndexClient` v verze 2.0-preview a že se změny přenesou tooversion 3.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-142">Previously, it returned `SearchIndexClient`, but this was changed too`ISearchIndexClient` in version 2.0-preview, and that change carries over tooversion 3.</span></span> <span data-ttu-id="fc5d6-143">Toto je toosupport zákazníků, které chcete toomock hello `GetClient` metodu pro testování částí vrácením imitované implementace `ISearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-143">This is toosupport customers that wish toomock hello `GetClient` method for unit tests by returning a mock implementation of `ISearchIndexClient`.</span></span>

#### <a name="example"></a><span data-ttu-id="fc5d6-144">Příklad</span><span class="sxs-lookup"><span data-stu-id="fc5d6-144">Example</span></span>
<span data-ttu-id="fc5d6-145">Pokud váš kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-145">If your code looks like this:</span></span>

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

<span data-ttu-id="fc5d6-146">Můžete ji změnit toothis toofix žádné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-146">You can change it toothis toofix any build errors:</span></span>

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-toostrings"></a><span data-ttu-id="fc5d6-147">AnalyzerName, datový typ a další jsou už implicitně převést toostrings</span><span class="sxs-lookup"><span data-stu-id="fc5d6-147">AnalyzerName, DataType, and others are no longer implicitly convertible toostrings</span></span>
<span data-ttu-id="fc5d6-148">Existuje mnoho typů v hello Azure Search .NET SDK, které pocházejí z `ExtensibleEnum`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-148">There are many types in hello Azure Search .NET SDK that derive from `ExtensibleEnum`.</span></span> <span data-ttu-id="fc5d6-149">Dříve byly tyto typy všechny implicitně převést tootype `string`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-149">Previously these types were all implicitly convertible tootype `string`.</span></span> <span data-ttu-id="fc5d6-150">Ale byly zjištěny chyby v hello `Object.Equals` implementace pro tyto třídy a uchycení hello chyb vyžaduje zakázání této implicitní převod.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-150">However, a bug was discovered in hello `Object.Equals` implementation for these classes, and fixing hello bug required disabling this implicit conversion.</span></span> <span data-ttu-id="fc5d6-151">Explicitní převod příliš`string` je stále povolený.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-151">Explicit conversion too`string` is still allowed.</span></span>

#### <a name="example"></a><span data-ttu-id="fc5d6-152">Příklad</span><span class="sxs-lookup"><span data-stu-id="fc5d6-152">Example</span></span>
<span data-ttu-id="fc5d6-153">Pokud váš kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-153">If your code looks like this:</span></span>

```csharp
var customTokenizerName = TokenizerName.Create("my_tokenizer"); 
var customTokenFilterName = TokenFilterName.Create("my_tokenfilter"); 
var customCharFilterName = CharFilterName.Create("my_charfilter"); 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        customTokenizerName,  
        new[] { customTokenFilterName },  
        new[] { customCharFilterName }), 
}; 
```

<span data-ttu-id="fc5d6-154">Můžete ji změnit toothis toofix žádné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-154">You can change it toothis toofix any build errors:</span></span>

```csharp
const string CustomTokenizerName = "my_tokenizer"; 
const string CustomTokenFilterName = "my_tokenfilter"; 
const string CustomCharFilterName = "my_charfilter"; 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        CustomTokenizerName,  
        new TokenFilterName[] { CustomTokenFilterName },  
        new CharFilterName[] { CustomCharFilterName })
}; 
```

### <a name="removed-obsolete-members"></a><span data-ttu-id="fc5d6-155">Odebrat zastaralé členy</span><span class="sxs-lookup"><span data-stu-id="fc5d6-155">Removed obsolete members</span></span>

<span data-ttu-id="fc5d6-156">Může se zobrazit vlastnosti, které byly označeny jako zastaralé v verze 2.0-preview a následně odstraněny ve verzi 3 nebo toomethods související chyby sestavení.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-156">You may see build errors related toomethods or properties that were marked as obsolete in version 2.0-preview and subsequently removed in version 3.</span></span> <span data-ttu-id="fc5d6-157">Pokud narazíte na takové chyby, zde je způsob tooresolve je:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-157">If you encounter such errors, here is how tooresolve them:</span></span>

- <span data-ttu-id="fc5d6-158">Pokud používáte tento konstruktor: `ScoringParameter(string name, string value)`, použijte místo toho tato:`ScoringParameter(string name, IEnumerable<string> values)`</span><span class="sxs-lookup"><span data-stu-id="fc5d6-158">If you were using this constructor: `ScoringParameter(string name, string value)`, use this one instead: `ScoringParameter(string name, IEnumerable<string> values)`</span></span>
- <span data-ttu-id="fc5d6-159">Pokud jste používali hello `ScoringParameter.Value` vlastnost, použijte hello `ScoringParameter.Values` vlastnost nebo hello `ToString` metoda místo.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-159">If you were using hello `ScoringParameter.Value` property, use hello `ScoringParameter.Values` property or hello `ToString` method instead.</span></span>
- <span data-ttu-id="fc5d6-160">Pokud jste používali hello `SearchRequestOptions.RequestId` vlastnost, použijte hello `ClientRequestId` vlastnost místo.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-160">If you were using hello `SearchRequestOptions.RequestId` property, use hello `ClientRequestId` property instead.</span></span>

### <a name="removed-preview-features"></a><span data-ttu-id="fc5d6-161">Funkce odebrané preview</span><span class="sxs-lookup"><span data-stu-id="fc5d6-161">Removed preview features</span></span>

<span data-ttu-id="fc5d6-162">Pokud provádíte upgrade z verze 2.0 preview tooversion 3, mějte na paměti, JSON a CSV analýza podporu pro objekt Blob indexery byl odebrán vzhledem k tomu, že tyto funkce jsou stále ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-162">If you are upgrading from version 2.0-preview tooversion 3, be aware that JSON and CSV parsing support for Blob Indexers has been removed since these features are still in preview.</span></span> <span data-ttu-id="fc5d6-163">Konkrétně hello následující metody hello `IndexingParametersExtensions` třídy byly odebrány:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-163">Specifically, hello following methods of hello `IndexingParametersExtensions` class have been removed:</span></span>

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

<span data-ttu-id="fc5d6-164">Pokud vaše aplikace obsahuje pevné závislost na těchto funkcích, nebudete moct tooupgrade tooversion 3 z hello Azure Search .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-164">If your application has a hard dependency on these features, you will not be able tooupgrade tooversion 3 of hello Azure Search .NET SDK.</span></span> <span data-ttu-id="fc5d6-165">Můžete pokračovat v toouse verze 2.0-preview.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-165">You can continue toouse version 2.0-preview.</span></span> <span data-ttu-id="fc5d6-166">Ale prosím mějte na paměti, **nedoporučujeme používání náhled sady SDK v produkční aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-166">However, please keep in mind that **we do not recommend using preview SDKs in production applications**.</span></span> <span data-ttu-id="fc5d6-167">Funkce Preview jsou pouze zkušební verze a může změnit.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-167">Preview features are for evaluation only and may change.</span></span>

## <a name="conclusion"></a><span data-ttu-id="fc5d6-168">Závěr</span><span class="sxs-lookup"><span data-stu-id="fc5d6-168">Conclusion</span></span>
<span data-ttu-id="fc5d6-169">Pokud potřebujete další podrobnosti o použití hello .NET SDK služby Azure Search, najdete v našich nedávno aktualizovaného [postupy](search-howto-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="fc5d6-169">If you need more details on using hello Azure Search .NET SDK, see our recently updated [How-to](search-howto-dotnet-sdk.md).</span></span>

<span data-ttu-id="fc5d6-170">Uvítáme vaše názory na hello SDK.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-170">We welcome your feedback on hello SDK.</span></span> <span data-ttu-id="fc5d6-171">Pokud narazíte na potíže, myslíte, že volné tooask nám nápovědu k hello [fórum Azure Search MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch).</span><span class="sxs-lookup"><span data-stu-id="fc5d6-171">If you encounter problems, feel free tooask us for help on hello [Azure Search MSDN forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch).</span></span> <span data-ttu-id="fc5d6-172">Pokud narazíte na chyby, můžete soubor problém v hello [úložiště Azure .NET SDK GitHub](https://github.com/Azure/azure-sdk-for-net/issues).</span><span class="sxs-lookup"><span data-stu-id="fc5d6-172">If you find a bug, you can file an issue in hello [Azure .NET SDK GitHub repository](https://github.com/Azure/azure-sdk-for-net/issues).</span></span> <span data-ttu-id="fc5d6-173">Ujistěte se, že tooprefix název vašeho problému s "Search SDK:".</span><span class="sxs-lookup"><span data-stu-id="fc5d6-173">Make sure tooprefix your issue title with "Search SDK: ".</span></span>

<span data-ttu-id="fc5d6-174">Děkujeme za používání služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-174">Thank you for using Azure Search!</span></span>

<a name="UpgradeStepsV1"></a>

## <a name="appendix-steps-tooupgrade-tooversion-11"></a><span data-ttu-id="fc5d6-175">Dodatek: Kroky tooupgrade tooversion 1.1</span><span class="sxs-lookup"><span data-stu-id="fc5d6-175">Appendix: Steps tooupgrade tooversion 1.1</span></span>
> [!NOTE]
> <span data-ttu-id="fc5d6-176">Tato část se týká pouze toousers hello Azure Search .NET SDK verze 1.0.2-preview a starší.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-176">This section applies only toousers of hello Azure Search .NET SDK version 1.0.2-preview and older.</span></span>
> 
> 

<span data-ttu-id="fc5d6-177">Nejdřív aktualizovat vaše NuGet odkaz pro `Microsoft.Azure.Search` pomocí buď hello Konzola správce balíčků NuGet nebo nástrojem pravým tlačítkem myši na vaše odkazy na projekt a výběrem "Správa NuGet balíčky..." v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-177">First, update your NuGet reference for `Microsoft.Azure.Search` using either hello NuGet Package Manager Console or by right-clicking on your project references and selecting "Manage NuGet Packages..." in Visual Studio.</span></span>

<span data-ttu-id="fc5d6-178">Jakmile NuGet stáhl hello nové balíčky a jejich závislosti, znovu sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-178">Once NuGet has downloaded hello new packages and their dependencies, rebuild your project.</span></span>

<span data-ttu-id="fc5d6-179">Pokud jste dříve pomocí sestavení hello verze 1.0.0-preview, 1.0.1-preview nebo 1.0.2-preview, uspěli a vy budete připravené toogo!</span><span class="sxs-lookup"><span data-stu-id="fc5d6-179">If you were previously using version 1.0.0-preview, 1.0.1-preview, or 1.0.2-preview, hello build should succeed and you're ready toogo!</span></span>

<span data-ttu-id="fc5d6-180">Pokud jste dříve používali verze 0.13.0-preview nebo starší, měli byste vidět chyby jako hello následující sestavení:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-180">If you were previously using version 0.13.0-preview or older, you should see build errors like hello following:</span></span>

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: hello type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

<span data-ttu-id="fc5d6-181">dalším krokem Hello je chyby sestavení hello toofix po jednom.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-181">hello next step is toofix hello build errors one by one.</span></span> <span data-ttu-id="fc5d6-182">Většina bude vyžadovat změna názvů některé třídy a metody, které byly přejmenovány v hello SDK.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-182">Most will require changing some class and method names that have been renamed in hello SDK.</span></span> <span data-ttu-id="fc5d6-183">[Seznam nejnovější změny ve verzi 1.1](#ListOfChangesV1) obsahuje seznam tyto změny názvu.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-183">[List of breaking changes in version 1.1](#ListOfChangesV1) contains a list of these name changes.</span></span>

<span data-ttu-id="fc5d6-184">Pokud používáte vlastní třídy toomodel dokumentů a tyto třídy mají vlastnosti použití hodnot Null primitivní typy (například `int` nebo `bool` v jazyce C#), je oprava chyb v hello 1.1 verzi hello SDK, které byste měli vědět.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-184">If you're using custom classes toomodel your documents, and those classes have properties of non-nullable primitive types (for example, `int` or `bool` in C#), there is a bug fix in hello 1.1 version of hello SDK of which you should be aware.</span></span> <span data-ttu-id="fc5d6-185">V tématu [opravy chyb ve verzi 1.1](#BugFixesV1) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-185">See [Bug fixes in version 1.1](#BugFixesV1) for more details.</span></span>

<span data-ttu-id="fc5d6-186">Nakonec po vyřešili všechny chyby sestavení, můžete provádět změny tooyour aplikace tootake využívat nové funkce nechcete-li.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-186">Finally, once you've fixed any build errors, you can make changes tooyour application tootake advantage of new functionality if you wish.</span></span>

<a name="ListOfChangesV1"></a>

### <a name="list-of-breaking-changes-in-version-11"></a><span data-ttu-id="fc5d6-187">Seznam nejnovější změny ve verze 1.1</span><span class="sxs-lookup"><span data-stu-id="fc5d6-187">List of breaking changes in version 1.1</span></span>
<span data-ttu-id="fc5d6-188">Hello následujícím seznamu je seřazené podle hello pravděpodobnost, že změna hello ovlivní kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-188">hello following list is ordered by hello likelihood that hello change will affect your application code.</span></span>

#### <a name="indexbatch-and-indexaction-changes"></a><span data-ttu-id="fc5d6-189">IndexBatch a IndexAction změny</span><span class="sxs-lookup"><span data-stu-id="fc5d6-189">IndexBatch and IndexAction changes</span></span>
<span data-ttu-id="fc5d6-190">`IndexBatch.Create`byl přejmenován příliš`IndexBatch.New` a již `params` argument.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-190">`IndexBatch.Create` has been renamed too`IndexBatch.New` and no longer has a `params` argument.</span></span> <span data-ttu-id="fc5d6-191">Můžete použít `IndexBatch.New` pro balíků, které kombinovat různé typy akcí (sloučení, odstranění atd.).</span><span class="sxs-lookup"><span data-stu-id="fc5d6-191">You can use `IndexBatch.New` for batches that mix different types of actions (merges, deletes, etc.).</span></span> <span data-ttu-id="fc5d6-192">Kromě toho existují nové statické metody pro vytvoření dávky kde jsou všechny akce hello hello stejné: `Delete`, `Merge`, `MergeOrUpload`, a `Upload`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-192">In addition, there are new static methods for creating batches where all hello actions are hello same: `Delete`, `Merge`, `MergeOrUpload`, and `Upload`.</span></span>

<span data-ttu-id="fc5d6-193">`IndexAction`již má veřejné konstruktory a jeho vlastnosti jsou nyní neměnné.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-193">`IndexAction` no longer has public constructors and its properties are now immutable.</span></span> <span data-ttu-id="fc5d6-194">Používejte hello nové statické metody pro vytvoření akce pro jiné účely: `Delete`, `Merge`, `MergeOrUpload`, a `Upload`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-194">You should use hello new static methods for creating actions for different purposes: `Delete`, `Merge`, `MergeOrUpload`, and `Upload`.</span></span> <span data-ttu-id="fc5d6-195">`IndexAction.Create`byla odebrána.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-195">`IndexAction.Create` has been removed.</span></span> <span data-ttu-id="fc5d6-196">Pokud jste použili hello přetížení, které přijímá pouze dokumentu, ujistěte se, že toouse `Upload` místo.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-196">If you used hello overload that takes only a document, make sure toouse `Upload` instead.</span></span>

##### <a name="example"></a><span data-ttu-id="fc5d6-197">Příklad</span><span class="sxs-lookup"><span data-stu-id="fc5d6-197">Example</span></span>
<span data-ttu-id="fc5d6-198">Pokud váš kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-198">If your code looks like this:</span></span>

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

<span data-ttu-id="fc5d6-199">Můžete ji změnit toothis toofix žádné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-199">You can change it toothis toofix any build errors:</span></span>

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

<span data-ttu-id="fc5d6-200">Pokud chcete, můžete další zjednodušit ho toothis:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-200">If you want, you can further simplify it toothis:</span></span>

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a><span data-ttu-id="fc5d6-201">IndexBatchException změny</span><span class="sxs-lookup"><span data-stu-id="fc5d6-201">IndexBatchException changes</span></span>
<span data-ttu-id="fc5d6-202">Hello `IndexBatchException.IndexResponse` vlastnost byla přejmenována příliš`IndexingResults`, a je její typ `IList<IndexingResult>`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-202">hello `IndexBatchException.IndexResponse` property has been renamed too`IndexingResults`, and its type is now `IList<IndexingResult>`.</span></span>

##### <a name="example"></a><span data-ttu-id="fc5d6-203">Příklad</span><span class="sxs-lookup"><span data-stu-id="fc5d6-203">Example</span></span>
<span data-ttu-id="fc5d6-204">Pokud váš kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-204">If your code looks like this:</span></span>

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed tooindex some of hello documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<span data-ttu-id="fc5d6-205">Můžete ji změnit toothis toofix žádné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-205">You can change it toothis toofix any build errors:</span></span>

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed tooindex some of hello documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

#### <a name="operation-method-changes"></a><span data-ttu-id="fc5d6-206">Operace změny – metoda</span><span class="sxs-lookup"><span data-stu-id="fc5d6-206">Operation method changes</span></span>
<span data-ttu-id="fc5d6-207">Jednotlivých operací ve hello Azure Search .NET SDK je zpřístupněná jako sadu přetížení metody pro synchronní a asynchronní volající.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-207">Each operation in hello Azure Search .NET SDK is exposed as a set of method overloads for synchronous and asynchronous callers.</span></span> <span data-ttu-id="fc5d6-208">Hello podpisů a řešení z těchto přetížení metody se změnilo v verze 1.1.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-208">hello signatures and factoring of these method overloads has changed in version 1.1.</span></span>

<span data-ttu-id="fc5d6-209">Například hello "Získat Index statistika" operace ve starších verzích hello SDK zveřejněné tyto podpisy:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-209">For example, hello "Get Index Statistics" operation in older versions of hello SDK exposed these signatures:</span></span>

<span data-ttu-id="fc5d6-210">V `IIndexOperations`:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-210">In `IIndexOperations`:</span></span>

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

<span data-ttu-id="fc5d6-211">V `IndexOperationsExtensions`:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-211">In `IndexOperationsExtensions`:</span></span>

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

<span data-ttu-id="fc5d6-212">podpisy Hello metoda pro hello stejné operace v vypadají verze 1.1:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-212">hello method signatures for hello same operation in version 1.1 look like this:</span></span>

<span data-ttu-id="fc5d6-213">V `IIndexesOperations`:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-213">In `IIndexesOperations`:</span></span>

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

<span data-ttu-id="fc5d6-214">V `IndexesOperationsExtensions`:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-214">In `IndexesOperationsExtensions`:</span></span>

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

<span data-ttu-id="fc5d6-215">Od verze 1.1, hello .NET SDK služby Azure Search slouží k uspořádání operaci metody jinak:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-215">Starting with version 1.1, hello Azure Search .NET SDK organizes operation methods differently:</span></span>

* <span data-ttu-id="fc5d6-216">Volitelné parametry jsou nyní modelován jako výchozí parametry spíš než přetížení další metody.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-216">Optional parameters are now modeled as default parameters rather than additional method overloads.</span></span> <span data-ttu-id="fc5d6-217">To někdy výrazně snižuje počet hello přetížení metody.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-217">This reduces hello number of method overloads, sometimes dramatically.</span></span>
* <span data-ttu-id="fc5d6-218">metody rozšíření Hello teď skrýt spoustu hello nadbytečné podrobnosti HTTP z volající hello.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-218">hello extension methods now hide a lot of hello extraneous details of HTTP from hello caller.</span></span> <span data-ttu-id="fc5d6-219">Například starší verze sady SDK vrátil objekt odpověď se stavovým kódem HTTP, které můžete často hello nemám potřebu toocheck, protože operaci metody throw `CloudException` pro stavový kód, který označuje chybu.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-219">For example, older versions of hello SDK returned a response object with an HTTP status code, which you often didn't need toocheck because operation methods throw `CloudException` for any status code that indicates an error.</span></span> <span data-ttu-id="fc5d6-220">Dobrý den nové objekty vrácena model metody rozšíření, ukládání můžete hello potíže při použití toounwrap je ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-220">hello new extension methods just return model objects, saving you hello trouble of having toounwrap them in your code.</span></span>
* <span data-ttu-id="fc5d6-221">Naopak hello základní rozhraní nyní vystavit metody, které získáte větší kontrolu na úrovni hello HTTP pokud ho potřebujete.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-221">Conversely, hello core interfaces now expose methods that give you more control at hello HTTP level if you need it.</span></span> <span data-ttu-id="fc5d6-222">Nyní můžete předat ve vlastní toobe hlavičky protokolu HTTP součástí požadavků a hello nové `AzureOperationResponse<T>` vrátí typ poskytuje přímý přístup toohello `HttpRequestMessage` a `HttpResponseMessage` pro operaci hello.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-222">You can now pass in custom HTTP headers toobe included in requests, and hello new `AzureOperationResponse<T>` return type gives you direct access toohello `HttpRequestMessage` and `HttpResponseMessage` for hello operation.</span></span> <span data-ttu-id="fc5d6-223">`AzureOperationResponse`je definována v hello `Microsoft.Rest.Azure` obor názvů a nahradí `Hyak.Common.OperationResponse`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-223">`AzureOperationResponse` is defined in hello `Microsoft.Rest.Azure` namespace and replaces `Hyak.Common.OperationResponse`.</span></span>

#### <a name="scoringparameters-changes"></a><span data-ttu-id="fc5d6-224">ScoringParameters změny</span><span class="sxs-lookup"><span data-stu-id="fc5d6-224">ScoringParameters changes</span></span>
<span data-ttu-id="fc5d6-225">Novou třídu s názvem `ScoringParameter` byl v hello nejnovější SDK toomake jeho přidání jednodušší tooprovide parametry tooscoring profily ve vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-225">A new class named `ScoringParameter` has been added in hello latest SDK toomake it easier tooprovide parameters tooscoring profiles in a search query.</span></span> <span data-ttu-id="fc5d6-226">Dříve hello `ScoringProfiles` vlastnost hello `SearchParameters` třída byl zadán jako `IList<string>`; Teď je zadán jako `IList<ScoringParameter>`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-226">Previously hello `ScoringProfiles` property of hello `SearchParameters` class was typed as `IList<string>`; Now it is typed as `IList<ScoringParameter>`.</span></span>

##### <a name="example"></a><span data-ttu-id="fc5d6-227">Příklad</span><span class="sxs-lookup"><span data-stu-id="fc5d6-227">Example</span></span>
<span data-ttu-id="fc5d6-228">Pokud váš kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-228">If your code looks like this:</span></span>

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

<span data-ttu-id="fc5d6-229">Můžete ji změnit toothis toofix žádné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-229">You can change it toothis toofix any build errors:</span></span> 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a><span data-ttu-id="fc5d6-230">Změny modelu – třída</span><span class="sxs-lookup"><span data-stu-id="fc5d6-230">Model class changes</span></span>
<span data-ttu-id="fc5d6-231">Z důvodu změn podpis toohello popsaných v [operaci metoda změny](#OperationMethodChanges), mnoho tříd v hello `Microsoft.Azure.Search.Models` bylo přejmenováno nebo odebrat obor názvů.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-231">Due toohello signature changes described in [Operation method changes](#OperationMethodChanges), many classes in hello `Microsoft.Azure.Search.Models` namespace have been renamed or removed.</span></span> <span data-ttu-id="fc5d6-232">Například:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-232">For example:</span></span>

* <span data-ttu-id="fc5d6-233">`IndexDefinitionResponse`nahradila`AzureOperationResponse<Index>`</span><span class="sxs-lookup"><span data-stu-id="fc5d6-233">`IndexDefinitionResponse` has been replaced by `AzureOperationResponse<Index>`</span></span>
* <span data-ttu-id="fc5d6-234">`DocumentSearchResponse`byl přejmenován příliš`DocumentSearchResult`</span><span class="sxs-lookup"><span data-stu-id="fc5d6-234">`DocumentSearchResponse` has been renamed too`DocumentSearchResult`</span></span>
* <span data-ttu-id="fc5d6-235">`IndexResult`byl přejmenován příliš`IndexingResult`</span><span class="sxs-lookup"><span data-stu-id="fc5d6-235">`IndexResult` has been renamed too`IndexingResult`</span></span>
* <span data-ttu-id="fc5d6-236">`Documents.Count()`nyní vrátí `long` s počtem dokumentů hello místo`DocumentCountResponse`</span><span class="sxs-lookup"><span data-stu-id="fc5d6-236">`Documents.Count()` now returns a `long` with hello document count instead of a `DocumentCountResponse`</span></span>
* <span data-ttu-id="fc5d6-237">`IndexGetStatisticsResponse`byl přejmenován příliš`IndexGetStatisticsResult`</span><span class="sxs-lookup"><span data-stu-id="fc5d6-237">`IndexGetStatisticsResponse` has been renamed too`IndexGetStatisticsResult`</span></span>
* <span data-ttu-id="fc5d6-238">`IndexListResponse`byl přejmenován příliš`IndexListResult`</span><span class="sxs-lookup"><span data-stu-id="fc5d6-238">`IndexListResponse` has been renamed too`IndexListResult`</span></span>

<span data-ttu-id="fc5d6-239">toosummarize, `OperationResponse`-odvozených tříd, které existovaly pouze toowrap objekt modelu se odebraly.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-239">toosummarize, `OperationResponse`-derived classes that existed only toowrap a model object have been removed.</span></span> <span data-ttu-id="fc5d6-240">Hello zbývající třídy předtím jejich přípona se změnila z `Response` příliš`Result`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-240">hello remaining classes have had their suffix changed from `Response` too`Result`.</span></span>

##### <a name="example"></a><span data-ttu-id="fc5d6-241">Příklad</span><span class="sxs-lookup"><span data-stu-id="fc5d6-241">Example</span></span>
<span data-ttu-id="fc5d6-242">Pokud váš kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-242">If your code looks like this:</span></span>

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

<span data-ttu-id="fc5d6-243">Můžete ji změnit toothis toofix žádné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-243">You can change it toothis toofix any build errors:</span></span>

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a><span data-ttu-id="fc5d6-244">Odpověď třídy a rozhraní IEnumerable</span><span class="sxs-lookup"><span data-stu-id="fc5d6-244">Response classes and IEnumerable</span></span>
<span data-ttu-id="fc5d6-245">Další změny, které mohou ovlivnit váš kód je už implementaci třídy odpovědi, které uložení kolekce `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-245">An additional change that may affect your code is that response classes that hold collections no longer implement `IEnumerable<T>`.</span></span> <span data-ttu-id="fc5d6-246">Vlastnost kolekce hello místo toho můžete přistupovat přímo.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-246">Instead, you can access hello collection property directly.</span></span> <span data-ttu-id="fc5d6-247">Například, pokud kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-247">For example, if your code looks like this:</span></span>

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

<span data-ttu-id="fc5d6-248">Můžete ji změnit toothis toofix žádné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-248">You can change it toothis toofix any build errors:</span></span>

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="special-case-for-web-applications"></a><span data-ttu-id="fc5d6-249">Zvláštním případem pro webové aplikace</span><span class="sxs-lookup"><span data-stu-id="fc5d6-249">Special case for web applications</span></span>
<span data-ttu-id="fc5d6-250">Pokud máte webové aplikace, který serializuje `DocumentSearchResponse` přímo výsledky hledání toosend toohello prohlížeče, budete potřebovat toochange kódu nebo hello výsledky nebude správně serializovat.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-250">If you have a web application that serializes `DocumentSearchResponse` directly toosend search results toohello browser, you will need toochange your code or hello results will not serialize correctly.</span></span> <span data-ttu-id="fc5d6-251">Například, pokud kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-251">For example, if your code looks like this:</span></span>

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want toosearch everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

<span data-ttu-id="fc5d6-252">Můžete ji změnit získáním hello `.Results` vlastnost vykreslování výsledků vyhledávání odpovědi toofix na hello vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-252">You can change it by getting hello `.Results` property of hello search response toofix search result rendering:</span></span>

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want toosearch everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

<span data-ttu-id="fc5d6-253">Budete mít toolook takových případech ve vašem kódu sami. **hello kompilátoru nebude varovat** protože `JsonResult.Data` je typu `object`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-253">You will have toolook for such cases in your code yourself; **hello compiler will not warn you** because `JsonResult.Data` is of type `object`.</span></span>

#### <a name="cloudexception-changes"></a><span data-ttu-id="fc5d6-254">CloudException změny</span><span class="sxs-lookup"><span data-stu-id="fc5d6-254">CloudException changes</span></span>
<span data-ttu-id="fc5d6-255">Hello `CloudException` třída přesunula z hello `Hyak.Common` toohello obor názvů `Microsoft.Rest.Azure` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-255">hello `CloudException` class has moved from hello `Hyak.Common` namespace toohello `Microsoft.Rest.Azure` namespace.</span></span> <span data-ttu-id="fc5d6-256">Také jeho `Error` vlastnost byla přejmenována příliš`Body`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-256">Also, its `Error` property has been renamed too`Body`.</span></span>

#### <a name="searchserviceclient-and-searchindexclient-changes"></a><span data-ttu-id="fc5d6-257">Změny SearchServiceClient a SearchIndexClient</span><span class="sxs-lookup"><span data-stu-id="fc5d6-257">SearchServiceClient and SearchIndexClient changes</span></span>
<span data-ttu-id="fc5d6-258">Hello typ hello `Credentials` vlastnost bylo změněno z `SearchCredentials` tooits základní třídy, `ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-258">hello type of hello `Credentials` property has changed from `SearchCredentials` tooits base class, `ServiceClientCredentials`.</span></span> <span data-ttu-id="fc5d6-259">Pokud potřebujete tooaccess hello `SearchCredentials` z `SearchIndexClient` nebo `SearchServiceClient`, použijte nové hello `SearchCredentials` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-259">If you need tooaccess hello `SearchCredentials` of a `SearchIndexClient` or `SearchServiceClient`, please use hello new `SearchCredentials` property.</span></span>

<span data-ttu-id="fc5d6-260">Ve starších verzích hello SDK `SearchServiceClient` a `SearchIndexClient` měl konstruktory, které trvalo `HttpClient` parametr.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-260">In older versions of hello SDK, `SearchServiceClient` and `SearchIndexClient` had constructors that took an `HttpClient` parameter.</span></span> <span data-ttu-id="fc5d6-261">Tyto nahradil konstruktory, které provést `HttpClientHandler` a pole `DelegatingHandler` objekty.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-261">These have been replaced with constructors that take an `HttpClientHandler` and an array of `DelegatingHandler` objects.</span></span> <span data-ttu-id="fc5d6-262">Díky tomu je snazší tooinstall vlastní obslužné rutiny toopre zpracování požadavků HTTP v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-262">This makes it easier tooinstall custom handlers toopre-process HTTP requests if necessary.</span></span>

<span data-ttu-id="fc5d6-263">Nakonec hello konstruktory, které trvalo `Uri` a `SearchCredentials` změnily.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-263">Finally, hello constructors that took a `Uri` and `SearchCredentials` have changed.</span></span> <span data-ttu-id="fc5d6-264">Například pokud máte kód, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-264">For example, if you have code that looks like this:</span></span>

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

<span data-ttu-id="fc5d6-265">Můžete ji změnit toothis toofix žádné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-265">You can change it toothis toofix any build errors:</span></span>

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

<span data-ttu-id="fc5d6-266">Všimněte si, že typ hello hello pověření parametr se změní také příliš`ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-266">Also note that hello type of hello credentials parameter has changed too`ServiceClientCredentials`.</span></span> <span data-ttu-id="fc5d6-267">Toto je pravděpodobně tooaffect kódu od `SearchCredentials` je odvozený od `ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-267">This is unlikely tooaffect your code since `SearchCredentials` is derived from `ServiceClientCredentials`.</span></span>

#### <a name="passing-a-request-id"></a><span data-ttu-id="fc5d6-268">Předání ID žádosti</span><span class="sxs-lookup"><span data-stu-id="fc5d6-268">Passing a request ID</span></span>
<span data-ttu-id="fc5d6-269">Ve starších verzích hello SDK, můžete nastavit ID požadavku na hello `SearchServiceClient` nebo `SearchIndexClient` a by být součástí každé toohello požadavku REST API.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-269">In older versions of hello SDK, you could set a request ID on hello `SearchServiceClient` or `SearchIndexClient` and it would be included in every request toohello REST API.</span></span> <span data-ttu-id="fc5d6-270">To je užitečné pro odstraňování potíží s služby search, pokud potřebujete podporu toocontact.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-270">This is useful for troubleshooting issues with your search service if you need toocontact support.</span></span> <span data-ttu-id="fc5d6-271">Je však tooset užitečnější požadavku Jedinečný ID pro každou operaci místo toouse hello stejným ID pro všechny operace.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-271">However, it is more useful tooset a unique request ID for every operation rather than toouse hello same ID for all operations.</span></span> <span data-ttu-id="fc5d6-272">Z tohoto důvodu hello `SetClientRequestId` metody `SearchServiceClient` a `SearchIndexClient` byly odebrány.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-272">For this reason, hello `SetClientRequestId` methods of `SearchServiceClient` and `SearchIndexClient` have been removed.</span></span> <span data-ttu-id="fc5d6-273">Místo toho můžete předat metodu požadavku tooeach ID operace prostřednictvím hello volitelné `SearchRequestOptions` parametr.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-273">Instead, you can pass a request ID tooeach operation method via hello optional `SearchRequestOptions` parameter.</span></span>

> [!NOTE]
> <span data-ttu-id="fc5d6-274">V budoucí verzi hello SDK přidáme nové mechanismus pro ID požadavku globální nastavení na klientovi hello objektů, který je konzistentní s přístupem hello používá jiné sady Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-274">In a future release of hello SDK, we will add a new mechanism for setting a request ID globally on hello client objects that is consistent with hello approach used by other Azure SDKs.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="fc5d6-275">Příklad</span><span class="sxs-lookup"><span data-stu-id="fc5d6-275">Example</span></span>
<span data-ttu-id="fc5d6-276">Pokud máte kód, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-276">If you have code that looks like this:</span></span>

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

<span data-ttu-id="fc5d6-277">Můžete ji změnit toothis toofix žádné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-277">You can change it toothis toofix any build errors:</span></span>

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a><span data-ttu-id="fc5d6-278">Změny názvu rozhraní</span><span class="sxs-lookup"><span data-stu-id="fc5d6-278">Interface name changes</span></span>
<span data-ttu-id="fc5d6-279">Hello operaci skupiny rozhraní názvy mají všechny změněné toobe konzistentní s jejich odpovídající názvy vlastností:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-279">hello operation group interface names have all changed toobe consistent with their corresponding property names:</span></span>

* <span data-ttu-id="fc5d6-280">Hello typ `ISearchServiceClient.Indexes` byla přejmenována z `IIndexOperations` příliš`IIndexesOperations`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-280">hello type of `ISearchServiceClient.Indexes` has been renamed from `IIndexOperations` too`IIndexesOperations`.</span></span>
* <span data-ttu-id="fc5d6-281">Hello typ `ISearchServiceClient.Indexers` byla přejmenována z `IIndexerOperations` příliš`IIndexersOperations`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-281">hello type of `ISearchServiceClient.Indexers` has been renamed from `IIndexerOperations` too`IIndexersOperations`.</span></span>
* <span data-ttu-id="fc5d6-282">Hello typ `ISearchServiceClient.DataSources` byla přejmenována z `IDataSourceOperations` příliš`IDataSourcesOperations`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-282">hello type of `ISearchServiceClient.DataSources` has been renamed from `IDataSourceOperations` too`IDataSourcesOperations`.</span></span>
* <span data-ttu-id="fc5d6-283">Hello typ `ISearchIndexClient.Documents` byla přejmenována z `IDocumentOperations` příliš`IDocumentsOperations`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-283">hello type of `ISearchIndexClient.Documents` has been renamed from `IDocumentOperations` too`IDocumentsOperations`.</span></span>

<span data-ttu-id="fc5d6-284">Tuto změnu je pravděpodobně tooaffect kódu, pokud jste vytvořili mocks tato rozhraní pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-284">This change is unlikely tooaffect your code unless you created mocks of these interfaces for test purposes.</span></span>

<a name="BugFixesV1"></a>

### <a name="bug-fixes-in-version-11"></a><span data-ttu-id="fc5d6-285">Opravy chyb v verze 1.1</span><span class="sxs-lookup"><span data-stu-id="fc5d6-285">Bug fixes in version 1.1</span></span>
<span data-ttu-id="fc5d6-286">Starší verze hello Azure Search .NET SDK týkající se tooserialization třídy vlastní model obsahoval chyby.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-286">There was a bug in older versions of hello Azure Search .NET SDK relating tooserialization of custom model classes.</span></span> <span data-ttu-id="fc5d6-287">Hello chyb mohlo dojít, pokud jste vytvořili třídu vlastní modelu s vlastností typu hodnot neumožňující hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-287">hello bug could occur if you created a custom model class with a property of a non-nullable value type.</span></span>

#### <a name="steps-tooreproduce"></a><span data-ttu-id="fc5d6-288">Kroky tooreproduce</span><span class="sxs-lookup"><span data-stu-id="fc5d6-288">Steps tooreproduce</span></span>
<span data-ttu-id="fc5d6-289">Vytvořte třídu, vlastní modelu s vlastností typu hodnot neumožňující hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-289">Create a custom model class with a property of non-nullable value type.</span></span> <span data-ttu-id="fc5d6-290">Například přidejte veřejné `UnitCount` vlastnost typu `int` místo `int?`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-290">For example, add a public `UnitCount` property of type `int` instead of `int?`.</span></span>

<span data-ttu-id="fc5d6-291">Pokud indexu dokument s hello výchozí hodnota tohoto typu (například 0 pro `int`), hello pole bude mít hodnotu null ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-291">If you index a document with hello default value of that type (for example, 0 for `int`), hello field will be null in Azure Search.</span></span> <span data-ttu-id="fc5d6-292">Pokud následně vyhledávání pro tento dokument hello `Search` volání vyvolá výjimku `JsonSerializationException` nesouhlasících, kterou nelze převést `null` příliš`int`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-292">If you subsequently search for that document, hello `Search` call will throw `JsonSerializationException` complaining that it can't convert `null` too`int`.</span></span>

<span data-ttu-id="fc5d6-293">Navíc filtry nemusí fungovat podle očekávání vzhledem k tomu, že null byla zapsána toohello index namísto hello určené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-293">Also, filters may not work as expected since null was written toohello index instead of hello intended value.</span></span>

#### <a name="fix-details"></a><span data-ttu-id="fc5d6-294">Opravte podrobnosti</span><span class="sxs-lookup"><span data-stu-id="fc5d6-294">Fix details</span></span>
<span data-ttu-id="fc5d6-295">Vyřešili jsme tento problém v verze 1.1 hello SDK.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-295">We have fixed this issue in version 1.1 of hello SDK.</span></span> <span data-ttu-id="fc5d6-296">Nyní, pokud máte třídu modelu takto:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-296">Now, if you have a model class like this:</span></span>

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

<span data-ttu-id="fc5d6-297">a nastavíte `IntValue` too0, že hodnota je nyní správně serializovanou jako 0 na hello přenosu a uložené jako 0 v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-297">and you set `IntValue` too0, that value is now correctly serialized as 0 on hello wire and stored as 0 in hello index.</span></span> <span data-ttu-id="fc5d6-298">Zaokrouhlí vypínání také funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-298">Round tripping also works as expected.</span></span>

<span data-ttu-id="fc5d6-299">Je vědoma s tímto přístupem jeden potenciální problém toobe: Pokud chcete použít typ modelu s hodnotou Null vlastností, můžete mít příliš**zaručit** aby žádné dokumenty v indexu neobsahovaly pro odpovídající pole hello hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-299">There is one potential issue toobe aware of with this approach: If you use a model type with a non-nullable property, you have too**guarantee** that no documents in your index contain a null value for hello corresponding field.</span></span> <span data-ttu-id="fc5d6-300">Hello SDK ani hello REST API služby Azure Search vám pomůže tooenforce vám to.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-300">Neither hello SDK nor hello Azure Search REST API will help you tooenforce this.</span></span>

<span data-ttu-id="fc5d6-301">Nejedná se pouze o hypotetický problém: Představte si situaci, kdy přidáte nový pole tooan existující index typu `Edm.Int32`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-301">This is not just a hypothetical concern: Imagine a scenario where you add a new field tooan existing index that is of type `Edm.Int32`.</span></span> <span data-ttu-id="fc5d6-302">Po aktualizaci definice indexu hello, budou všechny dokumenty mít pro toto nové pole hodnotu null (protože jsou všechny typy s možnou hodnotou Null ve službě Azure Search).</span><span class="sxs-lookup"><span data-stu-id="fc5d6-302">After updating hello index definition, all documents will have a null value for that new field (since all types are nullable in Azure Search).</span></span> <span data-ttu-id="fc5d6-303">Pokud pak použijete třídu modelu s hodnotou Null `int` vlastnosti pro toto pole, zobrazí se `JsonSerializationException` při pokusu o tooretrieve dokumenty podobné výjimky:</span><span class="sxs-lookup"><span data-stu-id="fc5d6-303">If you then use a model class with a non-nullable `int` property for that field, you will get a `JsonSerializationException` like this when trying tooretrieve documents:</span></span>

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

<span data-ttu-id="fc5d6-304">Z tohoto důvodu doporučujeme jako osvědčený postup použijte typy s možnou hodnotou Null ve třídách modelu.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-304">For this reason, we still recommend that you use nullable types in your model classes as a best practice.</span></span>

<span data-ttu-id="fc5d6-305">Další informace o tato oprava chyb a hello, najdete v tématu [potíže na Githubu](https://github.com/Azure/azure-sdk-for-net/issues/1063).</span><span class="sxs-lookup"><span data-stu-id="fc5d6-305">For more details on this bug and hello fix, please see [this issue on GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).</span></span>

