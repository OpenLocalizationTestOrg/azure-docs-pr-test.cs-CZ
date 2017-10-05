---
title: "Upgrade .NET SDK služby Azure Search verze 1.1 | Microsoft Docs"
description: "Upgrade .NET SDK služby Azure Search verze 1.1"
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
ms.openlocfilehash: 9782454e3bfc697b63cde8aa28a14be0c393c36b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-3"></a><span data-ttu-id="c6bfc-103">Upgrade na Azure Search .NET SDK verze 3</span><span class="sxs-lookup"><span data-stu-id="c6bfc-103">Upgrading to the Azure Search .NET SDK version 3</span></span>
<span data-ttu-id="c6bfc-104">Pokud používáte verzi 2.0 preview nebo starší z [Azure Search .NET SDK](https://aka.ms/search-sdk), tento článek vám pomůže při upgradu aplikace k používání verze 3.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-104">If you're using version 2.0-preview or older of the [Azure Search .NET SDK](https://aka.ms/search-sdk), this article will help you upgrade your application to use version 3.</span></span>

<span data-ttu-id="c6bfc-105">Další obecné návod sady SDK, včetně příkladů, najdete v tématu [jak používat Azure Search z aplikace .NET](search-howto-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="c6bfc-105">For a more general walkthrough of the SDK including examples, see [How to use Azure Search from a .NET Application](search-howto-dotnet-sdk.md).</span></span>

<span data-ttu-id="c6bfc-106">Verze 3 .NET SDK služby Azure Search obsahuje některé změny z předchozích verzí.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-106">Version 3 of the Azure Search .NET SDK contains some changes from earlier versions.</span></span> <span data-ttu-id="c6bfc-107">Tyto jsou většinou dílčí, takže měnili kód měli vyžadovat jen minimální úsilí.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-107">These are mostly minor, so changing your code should require only minimal effort.</span></span> <span data-ttu-id="c6bfc-108">V tématu [kroky pro upgrade](#UpgradeSteps) pokyny o tom, jak upravit svůj kód k použití nové verze sady SDK.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-108">See [Steps to upgrade](#UpgradeSteps) for instructions on how to change your code to use the new SDK version.</span></span>

> [!NOTE]
> <span data-ttu-id="c6bfc-109">Pokud používáte verzi 1.0.2-preview nebo starší, byste měli upgradovat na verzi 1.1 první a pak upgradovat na verze 3.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-109">If you're using version 1.0.2-preview or older, you should upgrade to version 1.1 first, and then upgrade to version 3.</span></span> <span data-ttu-id="c6bfc-110">V tématu [příloha: kroky pro upgrade na verzi 1.1](#UpgradeStepsV1) pokyny.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-110">See [Appendix: Steps to upgrade to version 1.1](#UpgradeStepsV1) for instructions.</span></span>
>
> <span data-ttu-id="c6bfc-111">Instanci služby Azure Search podporuje několik verzí rozhraní REST API, včetně nejnovější.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-111">Your Azure Search service instance supports several REST API versions, including the latest one.</span></span> <span data-ttu-id="c6bfc-112">Můžete použít verzi, když je už nejnovějšího, ale doporučujeme migraci kód a použít nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-112">You can continue to use a version when it is no longer the latest one, but we recommend that you migrate your code to use the newest version.</span></span> <span data-ttu-id="c6bfc-113">Při použití rozhraní REST API, musíte zadat verze rozhraní API v každé žádosti o prostřednictvím parametr api-version.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-113">When using the REST API, you must specify the API version in every request via the api-version parameter.</span></span> <span data-ttu-id="c6bfc-114">Když pomocí sady .NET SDK, určuje verzi sady SDK používáte odpovídající verzi rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-114">When using the .NET SDK, the version of the SDK you’re using determines the corresponding version of the REST API.</span></span> <span data-ttu-id="c6bfc-115">Pokud používáte starší SDK, můžete spustit tento kód se žádné změny, i v případě, že je služba upgradovat pro podporu na novější verzi rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-115">If you are using an older SDK, you can continue to run that code with no changes even if the service is upgraded to support a newer API version.</span></span>

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a><span data-ttu-id="c6bfc-116">Co je nového v verze 3</span><span class="sxs-lookup"><span data-stu-id="c6bfc-116">What's new in version 3</span></span>
<span data-ttu-id="c6bfc-117">Verze 3 .NET SDK služby Azure Search cílí na nejnovější verzi obecně k dispozici REST API Azure Search, konkrétně 2016-09-01.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-117">Version 3 of the Azure Search .NET SDK targets the latest generally available version of the Azure Search REST API, specifically 2016-09-01.</span></span> <span data-ttu-id="c6bfc-118">Díky tomu je možné použít mnoha nových funkcí služby Azure Search z aplikace .NET, včetně následujících:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-118">This makes it possible to use many new features of Azure Search from a .NET application, including the following:</span></span>

* [<span data-ttu-id="c6bfc-119">Vlastní analyzátory</span><span class="sxs-lookup"><span data-stu-id="c6bfc-119">Custom analyzers</span></span>](https://aka.ms/customanalyzers)
* <span data-ttu-id="c6bfc-120">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) a [Azure Table Storage](search-howto-indexing-azure-tables.md) podpora indexeru</span><span class="sxs-lookup"><span data-stu-id="c6bfc-120">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) and [Azure Table Storage](search-howto-indexing-azure-tables.md) indexer support</span></span>
* <span data-ttu-id="c6bfc-121">Přizpůsobení indexeru prostřednictvím [pole mapování](search-indexer-field-mappings.md)</span><span class="sxs-lookup"><span data-stu-id="c6bfc-121">Indexer customization via [field mappings](search-indexer-field-mappings.md)</span></span>
* <span data-ttu-id="c6bfc-122">Značky etag binárním rozsáhlým podporu pro povolení bezpečné souběžných aktualizací definic index, indexery a zdroje dat</span><span class="sxs-lookup"><span data-stu-id="c6bfc-122">ETags support to enable safe concurrent updating of index definitions, indexers, and data sources</span></span>
* <span data-ttu-id="c6bfc-123">Podporu pro vytváření indexu pole definice deklarativně architekturu třídě modelu a použitím nové `FieldBuilder` třídy.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-123">Support for building index field definitions declaratively by decorating your model class and using the new `FieldBuilder` class.</span></span>
* <span data-ttu-id="c6bfc-124">Podpora pro .NET Core a přenosné profil .NET 111</span><span class="sxs-lookup"><span data-stu-id="c6bfc-124">Support for .NET Core and .NET Portable Profile 111</span></span>

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a><span data-ttu-id="c6bfc-125">Kroky pro upgrade</span><span class="sxs-lookup"><span data-stu-id="c6bfc-125">Steps to upgrade</span></span>
<span data-ttu-id="c6bfc-126">Nejdřív aktualizovat vaše NuGet odkaz pro `Microsoft.Azure.Search` pomocí konzoly Správce balíčků NuGet nebo nástrojem pravým tlačítkem myši na vaše odkazy na projekt a výběrem "Správa NuGet balíčky..." v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-126">First, update your NuGet reference for `Microsoft.Azure.Search` using either the NuGet Package Manager Console or by right-clicking on your project references and selecting "Manage NuGet Packages..." in Visual Studio.</span></span>

<span data-ttu-id="c6bfc-127">Jakmile NuGet stáhl nové balíčky a jejich závislosti, znovu sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-127">Once NuGet has downloaded the new packages and their dependencies, rebuild your project.</span></span> <span data-ttu-id="c6bfc-128">V závislosti na tom, jak je strukturovaná kódu se může znovu sestavit úspěšně.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-128">Depending on how your code is structured, it may rebuild successfully.</span></span> <span data-ttu-id="c6bfc-129">Pokud ano, jste připravení přejít!</span><span class="sxs-lookup"><span data-stu-id="c6bfc-129">If so, you're ready to go!</span></span>

<span data-ttu-id="c6bfc-130">Pokud vaše sestavení selže, měli byste vidět chyby sestavení takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-130">If your build fails, you should see a build error like the following:</span></span>

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

<span data-ttu-id="c6bfc-131">Dalším krokem je odstranění této chyby sestavení.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-131">The next step is to fix this build error.</span></span> <span data-ttu-id="c6bfc-132">V tématu [nejnovější změny ve verzi 3](#ListOfChanges) podrobné informace o co způsobí, že chyba a jeho řešení.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-132">See [Breaking changes in version 3](#ListOfChanges) for details on what causes the error and how to fix it.</span></span>

<span data-ttu-id="c6bfc-133">Může se zobrazit další sestavení upozornění související s zastaralé metody nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-133">You may see additional build warnings related to obsolete methods or properties.</span></span> <span data-ttu-id="c6bfc-134">Upozornění bude obsahovat pokyny o tom, jak používat místo zastaralé funkce.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-134">The warnings will include instructions on what to use instead of the deprecated feature.</span></span> <span data-ttu-id="c6bfc-135">Například, pokud vaše aplikace používá `IndexingParameters.Base64EncodeKeys` vlastnost, měli byste obdržet upozornění, která uvádí, že`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`</span><span class="sxs-lookup"><span data-stu-id="c6bfc-135">For example, if your application uses the `IndexingParameters.Base64EncodeKeys` property, you should get a warning that says `"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`</span></span>

<span data-ttu-id="c6bfc-136">Jakmile vyřešili všechny chyby sestavení, vám do vaší aplikace využívat výhod nových funkcí, nechcete-li provádět změny.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-136">Once you've fixed any build errors, you can make changes to your application to take advantage of new functionality if you wish.</span></span> <span data-ttu-id="c6bfc-137">Nové funkce v sadě SDK jsou podrobně popsané na [co je nového ve verzi 3](#WhatsNew).</span><span class="sxs-lookup"><span data-stu-id="c6bfc-137">New features in the SDK are detailed in [What's new in version 3](#WhatsNew).</span></span>

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a><span data-ttu-id="c6bfc-138">Nejnovější změny ve verze 3</span><span class="sxs-lookup"><span data-stu-id="c6bfc-138">Breaking changes in version 3</span></span>
<span data-ttu-id="c6bfc-139">Existuje malý počet nejnovější změny ve verzi 3, která může vyžadovat kód změní kromě nové sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-139">There a small number of breaking changes in version 3 that may require code changes in addition to rebuilding your application.</span></span>

### <a name="indexesgetclient-return-type"></a><span data-ttu-id="c6bfc-140">Návratový typ Indexes.GetClient</span><span class="sxs-lookup"><span data-stu-id="c6bfc-140">Indexes.GetClient return type</span></span>
<span data-ttu-id="c6bfc-141">`Indexes.GetClient` Má nové návratový typ. metoda.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-141">The `Indexes.GetClient` method has a new return type.</span></span> <span data-ttu-id="c6bfc-142">Dříve, vrátí `SearchIndexClient`, ale to byl změněn na `ISearchIndexClient` v verze 2.0-preview a že se změny přenesou do verze 3.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-142">Previously, it returned `SearchIndexClient`, but this was changed to `ISearchIndexClient` in version 2.0-preview, and that change carries over to version 3.</span></span> <span data-ttu-id="c6bfc-143">To je podpora zákazníků, které chcete model `GetClient` metodu pro testování částí vrácením imitované implementace `ISearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-143">This is to support customers that wish to mock the `GetClient` method for unit tests by returning a mock implementation of `ISearchIndexClient`.</span></span>

#### <a name="example"></a><span data-ttu-id="c6bfc-144">Příklad</span><span class="sxs-lookup"><span data-stu-id="c6bfc-144">Example</span></span>
<span data-ttu-id="c6bfc-145">Pokud váš kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-145">If your code looks like this:</span></span>

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

<span data-ttu-id="c6bfc-146">Můžete ji změnit k tomuto a opravte případné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-146">You can change it to this to fix any build errors:</span></span>

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-to-strings"></a><span data-ttu-id="c6bfc-147">AnalyzerName, datový typ a další jsou už implicitně převést na řetězce</span><span class="sxs-lookup"><span data-stu-id="c6bfc-147">AnalyzerName, DataType, and others are no longer implicitly convertible to strings</span></span>
<span data-ttu-id="c6bfc-148">Existuje mnoho typů v Azure Search .NET SDK, které pocházejí z `ExtensibleEnum`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-148">There are many types in the Azure Search .NET SDK that derive from `ExtensibleEnum`.</span></span> <span data-ttu-id="c6bfc-149">Dříve byly všechny tyto typy implicitně převést na typ `string`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-149">Previously these types were all implicitly convertible to type `string`.</span></span> <span data-ttu-id="c6bfc-150">Ale byly zjištěny chyby ve `Object.Equals` implementace pro tyto třídy a oprava chyb vyžaduje zakázání této implicitní převod.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-150">However, a bug was discovered in the `Object.Equals` implementation for these classes, and fixing the bug required disabling this implicit conversion.</span></span> <span data-ttu-id="c6bfc-151">Explicitní převod na `string` je stále povolený.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-151">Explicit conversion to `string` is still allowed.</span></span>

#### <a name="example"></a><span data-ttu-id="c6bfc-152">Příklad</span><span class="sxs-lookup"><span data-stu-id="c6bfc-152">Example</span></span>
<span data-ttu-id="c6bfc-153">Pokud váš kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-153">If your code looks like this:</span></span>

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

<span data-ttu-id="c6bfc-154">Můžete ji změnit k tomuto a opravte případné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-154">You can change it to this to fix any build errors:</span></span>

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

### <a name="removed-obsolete-members"></a><span data-ttu-id="c6bfc-155">Odebrat zastaralé členy</span><span class="sxs-lookup"><span data-stu-id="c6bfc-155">Removed obsolete members</span></span>

<span data-ttu-id="c6bfc-156">Může se zobrazit chyby sestavení související s metody nebo vlastnosti, které byly označeny jako zastaralé ve verzi 2.0 preview a následně odebrat verze 3.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-156">You may see build errors related to methods or properties that were marked as obsolete in version 2.0-preview and subsequently removed in version 3.</span></span> <span data-ttu-id="c6bfc-157">Pokud narazíte na takové chyby, zde je způsob jejich řešení:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-157">If you encounter such errors, here is how to resolve them:</span></span>

- <span data-ttu-id="c6bfc-158">Pokud používáte tento konstruktor: `ScoringParameter(string name, string value)`, použijte místo toho tato:`ScoringParameter(string name, IEnumerable<string> values)`</span><span class="sxs-lookup"><span data-stu-id="c6bfc-158">If you were using this constructor: `ScoringParameter(string name, string value)`, use this one instead: `ScoringParameter(string name, IEnumerable<string> values)`</span></span>
- <span data-ttu-id="c6bfc-159">Pokud jste používali `ScoringParameter.Value` vlastnosti, použijte `ScoringParameter.Values` vlastnost nebo `ToString` metoda místo.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-159">If you were using the `ScoringParameter.Value` property, use the `ScoringParameter.Values` property or the `ToString` method instead.</span></span>
- <span data-ttu-id="c6bfc-160">Pokud jste používali `SearchRequestOptions.RequestId` vlastnosti, použijte `ClientRequestId` vlastnost místo.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-160">If you were using the `SearchRequestOptions.RequestId` property, use the `ClientRequestId` property instead.</span></span>

### <a name="removed-preview-features"></a><span data-ttu-id="c6bfc-161">Funkce odebrané preview</span><span class="sxs-lookup"><span data-stu-id="c6bfc-161">Removed preview features</span></span>

<span data-ttu-id="c6bfc-162">Pokud upgradujete z verze 2.0-preview verze 3, mějte na paměti, JSON a CSV analýza podporu pro objekt Blob indexery byl odebrán vzhledem k tomu, že tyto funkce jsou stále ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-162">If you are upgrading from version 2.0-preview to version 3, be aware that JSON and CSV parsing support for Blob Indexers has been removed since these features are still in preview.</span></span> <span data-ttu-id="c6bfc-163">Konkrétně tyto metody `IndexingParametersExtensions` třídy byly odebrány:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-163">Specifically, the following methods of the `IndexingParametersExtensions` class have been removed:</span></span>

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

<span data-ttu-id="c6bfc-164">Pokud vaše aplikace obsahuje pevné závislost na těchto funkcích, nebudete moci upgradovat na verzi 3 .NET SDK služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-164">If your application has a hard dependency on these features, you will not be able to upgrade to version 3 of the Azure Search .NET SDK.</span></span> <span data-ttu-id="c6bfc-165">Můžete nadále používat verze 2.0-preview.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-165">You can continue to use version 2.0-preview.</span></span> <span data-ttu-id="c6bfc-166">Ale prosím mějte na paměti, **nedoporučujeme používání náhled sady SDK v produkční aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-166">However, please keep in mind that **we do not recommend using preview SDKs in production applications**.</span></span> <span data-ttu-id="c6bfc-167">Funkce Preview jsou pouze zkušební verze a může změnit.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-167">Preview features are for evaluation only and may change.</span></span>

## <a name="conclusion"></a><span data-ttu-id="c6bfc-168">Závěr</span><span class="sxs-lookup"><span data-stu-id="c6bfc-168">Conclusion</span></span>
<span data-ttu-id="c6bfc-169">Pokud potřebujete další podrobnosti o použití .NET SDK služby Azure Search, najdete v našich nedávno aktualizovaného [postupy](search-howto-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="c6bfc-169">If you need more details on using the Azure Search .NET SDK, see our recently updated [How-to](search-howto-dotnet-sdk.md).</span></span>

<span data-ttu-id="c6bfc-170">Uvítáme vaše názory týkající se sady SDK.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-170">We welcome your feedback on the SDK.</span></span> <span data-ttu-id="c6bfc-171">Pokud narazíte na potíže, neváhejte nám požádat o pomoc na [fórum Azure Search MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch).</span><span class="sxs-lookup"><span data-stu-id="c6bfc-171">If you encounter problems, feel free to ask us for help on the [Azure Search MSDN forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch).</span></span> <span data-ttu-id="c6bfc-172">Pokud narazíte na chyby, můžete soubor problém v [úložiště Azure .NET SDK GitHub](https://github.com/Azure/azure-sdk-for-net/issues).</span><span class="sxs-lookup"><span data-stu-id="c6bfc-172">If you find a bug, you can file an issue in the [Azure .NET SDK GitHub repository](https://github.com/Azure/azure-sdk-for-net/issues).</span></span> <span data-ttu-id="c6bfc-173">Ujistěte se, zda jste předpony název vašeho problému s "Search SDK:".</span><span class="sxs-lookup"><span data-stu-id="c6bfc-173">Make sure to prefix your issue title with "Search SDK: ".</span></span>

<span data-ttu-id="c6bfc-174">Děkujeme za používání služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-174">Thank you for using Azure Search!</span></span>

<a name="UpgradeStepsV1"></a>

## <a name="appendix-steps-to-upgrade-to-version-11"></a><span data-ttu-id="c6bfc-175">Dodatek: Kroky pro upgrade na verzi 1.1</span><span class="sxs-lookup"><span data-stu-id="c6bfc-175">Appendix: Steps to upgrade to version 1.1</span></span>
> [!NOTE]
> <span data-ttu-id="c6bfc-176">Tato část se týká pouze uživatelé 1.0.2-preview verze .NET SDK služby Azure Search a starší.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-176">This section applies only to users of the Azure Search .NET SDK version 1.0.2-preview and older.</span></span>
> 
> 

<span data-ttu-id="c6bfc-177">Nejdřív aktualizovat vaše NuGet odkaz pro `Microsoft.Azure.Search` pomocí konzoly Správce balíčků NuGet nebo nástrojem pravým tlačítkem myši na vaše odkazy na projekt a výběrem "Správa NuGet balíčky..." v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-177">First, update your NuGet reference for `Microsoft.Azure.Search` using either the NuGet Package Manager Console or by right-clicking on your project references and selecting "Manage NuGet Packages..." in Visual Studio.</span></span>

<span data-ttu-id="c6bfc-178">Jakmile NuGet stáhl nové balíčky a jejich závislosti, znovu sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-178">Once NuGet has downloaded the new packages and their dependencies, rebuild your project.</span></span>

<span data-ttu-id="c6bfc-179">Pokud jste používali dříve verze 1.0.0-preview, 1.0.1-preview nebo 1.0.2-preview, sestavení uspěli a jste připravení přejít!</span><span class="sxs-lookup"><span data-stu-id="c6bfc-179">If you were previously using version 1.0.0-preview, 1.0.1-preview, or 1.0.2-preview, the build should succeed and you're ready to go!</span></span>

<span data-ttu-id="c6bfc-180">Pokud jste dříve používali verze 0.13.0-preview nebo starší, měli byste vidět sestavení chyby takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-180">If you were previously using version 0.13.0-preview or older, you should see build errors like the following:</span></span>

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

<span data-ttu-id="c6bfc-181">Dalším krokem je opravte chyby sestavení po jednom.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-181">The next step is to fix the build errors one by one.</span></span> <span data-ttu-id="c6bfc-182">Většina bude vyžadovat změny některé názvy třídy a metody, které byly přejmenovány v sadě SDK.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-182">Most will require changing some class and method names that have been renamed in the SDK.</span></span> <span data-ttu-id="c6bfc-183">[Seznam nejnovější změny ve verzi 1.1](#ListOfChangesV1) obsahuje seznam tyto změny názvu.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-183">[List of breaking changes in version 1.1](#ListOfChangesV1) contains a list of these name changes.</span></span>

<span data-ttu-id="c6bfc-184">Pokud používáte vlastní třídy pro modelování dokumentů a tyto třídy mají vlastnosti použití hodnot Null primitivní typy (například `int` nebo `bool` v jazyce C#), je oprava chyb v 1.1 verzi sady SDK, které byste měli vědět.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-184">If you're using custom classes to model your documents, and those classes have properties of non-nullable primitive types (for example, `int` or `bool` in C#), there is a bug fix in the 1.1 version of the SDK of which you should be aware.</span></span> <span data-ttu-id="c6bfc-185">V tématu [opravy chyb ve verzi 1.1](#BugFixesV1) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-185">See [Bug fixes in version 1.1](#BugFixesV1) for more details.</span></span>

<span data-ttu-id="c6bfc-186">Nakonec po vyřešili všechny chyby sestavení, vám provádět změny do vaší aplikace využívat výhod nových funkcí, nechcete-li.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-186">Finally, once you've fixed any build errors, you can make changes to your application to take advantage of new functionality if you wish.</span></span>

<a name="ListOfChangesV1"></a>

### <a name="list-of-breaking-changes-in-version-11"></a><span data-ttu-id="c6bfc-187">Seznam nejnovější změny ve verze 1.1</span><span class="sxs-lookup"><span data-stu-id="c6bfc-187">List of breaking changes in version 1.1</span></span>
<span data-ttu-id="c6bfc-188">V následujícím seznamu je seřazené podle pravděpodobnost, že změna ovlivní kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-188">The following list is ordered by the likelihood that the change will affect your application code.</span></span>

#### <a name="indexbatch-and-indexaction-changes"></a><span data-ttu-id="c6bfc-189">IndexBatch a IndexAction změny</span><span class="sxs-lookup"><span data-stu-id="c6bfc-189">IndexBatch and IndexAction changes</span></span>
<span data-ttu-id="c6bfc-190">`IndexBatch.Create`byl přejmenován na `IndexBatch.New` a již `params` argument.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-190">`IndexBatch.Create` has been renamed to `IndexBatch.New` and no longer has a `params` argument.</span></span> <span data-ttu-id="c6bfc-191">Můžete použít `IndexBatch.New` pro balíků, které kombinovat různé typy akcí (sloučení, odstranění atd.).</span><span class="sxs-lookup"><span data-stu-id="c6bfc-191">You can use `IndexBatch.New` for batches that mix different types of actions (merges, deletes, etc.).</span></span> <span data-ttu-id="c6bfc-192">Kromě toho existují nové statické metody pro vytvoření dávky kde všechny akce jsou stejné: `Delete`, `Merge`, `MergeOrUpload`, a `Upload`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-192">In addition, there are new static methods for creating batches where all the actions are the same: `Delete`, `Merge`, `MergeOrUpload`, and `Upload`.</span></span>

<span data-ttu-id="c6bfc-193">`IndexAction`již má veřejné konstruktory a jeho vlastnosti jsou nyní neměnné.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-193">`IndexAction` no longer has public constructors and its properties are now immutable.</span></span> <span data-ttu-id="c6bfc-194">Byste měli použít nové statické metody pro vytvoření akce pro jiné účely: `Delete`, `Merge`, `MergeOrUpload`, a `Upload`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-194">You should use the new static methods for creating actions for different purposes: `Delete`, `Merge`, `MergeOrUpload`, and `Upload`.</span></span> <span data-ttu-id="c6bfc-195">`IndexAction.Create`byla odebrána.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-195">`IndexAction.Create` has been removed.</span></span> <span data-ttu-id="c6bfc-196">Pokud jste použili přetížení, které přijímá pouze dokumentu, nezapomeňte použít `Upload` místo.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-196">If you used the overload that takes only a document, make sure to use `Upload` instead.</span></span>

##### <a name="example"></a><span data-ttu-id="c6bfc-197">Příklad</span><span class="sxs-lookup"><span data-stu-id="c6bfc-197">Example</span></span>
<span data-ttu-id="c6bfc-198">Pokud váš kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-198">If your code looks like this:</span></span>

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

<span data-ttu-id="c6bfc-199">Můžete ji změnit k tomuto a opravte případné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-199">You can change it to this to fix any build errors:</span></span>

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

<span data-ttu-id="c6bfc-200">Pokud chcete, můžete ho zjednodušit další k tomuto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-200">If you want, you can further simplify it to this:</span></span>

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a><span data-ttu-id="c6bfc-201">IndexBatchException změny</span><span class="sxs-lookup"><span data-stu-id="c6bfc-201">IndexBatchException changes</span></span>
<span data-ttu-id="c6bfc-202">`IndexBatchException.IndexResponse` Vlastnost byla přejmenována na `IndexingResults`, a je její typ `IList<IndexingResult>`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-202">The `IndexBatchException.IndexResponse` property has been renamed to `IndexingResults`, and its type is now `IList<IndexingResult>`.</span></span>

##### <a name="example"></a><span data-ttu-id="c6bfc-203">Příklad</span><span class="sxs-lookup"><span data-stu-id="c6bfc-203">Example</span></span>
<span data-ttu-id="c6bfc-204">Pokud váš kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-204">If your code looks like this:</span></span>

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<span data-ttu-id="c6bfc-205">Můžete ji změnit k tomuto a opravte případné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-205">You can change it to this to fix any build errors:</span></span>

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

#### <a name="operation-method-changes"></a><span data-ttu-id="c6bfc-206">Operace změny – metoda</span><span class="sxs-lookup"><span data-stu-id="c6bfc-206">Operation method changes</span></span>
<span data-ttu-id="c6bfc-207">Každé operace v rozhraní .NET SDK služby Azure Search je zpřístupněná jako sadu přetížení metody pro synchronní a asynchronní volající.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-207">Each operation in the Azure Search .NET SDK is exposed as a set of method overloads for synchronous and asynchronous callers.</span></span> <span data-ttu-id="c6bfc-208">Podpisů a řešení z těchto přetížení metody se změnilo v verze 1.1.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-208">The signatures and factoring of these method overloads has changed in version 1.1.</span></span>

<span data-ttu-id="c6bfc-209">Například "Získat Index statistika" operaci ve starší verze sady SDK zveřejněné tyto podpisy:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-209">For example, the "Get Index Statistics" operation in older versions of the SDK exposed these signatures:</span></span>

<span data-ttu-id="c6bfc-210">V `IIndexOperations`:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-210">In `IIndexOperations`:</span></span>

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

<span data-ttu-id="c6bfc-211">V `IndexOperationsExtensions`:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-211">In `IndexOperationsExtensions`:</span></span>

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

<span data-ttu-id="c6bfc-212">Metoda podpisy pro stejnou operaci v verze 1.1 bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-212">The method signatures for the same operation in version 1.1 look like this:</span></span>

<span data-ttu-id="c6bfc-213">V `IIndexesOperations`:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-213">In `IIndexesOperations`:</span></span>

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

<span data-ttu-id="c6bfc-214">V `IndexesOperationsExtensions`:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-214">In `IndexesOperationsExtensions`:</span></span>

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

<span data-ttu-id="c6bfc-215">Od verze 1.1, .NET SDK služby Azure Search slouží k uspořádání operaci metody jinak:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-215">Starting with version 1.1, the Azure Search .NET SDK organizes operation methods differently:</span></span>

* <span data-ttu-id="c6bfc-216">Volitelné parametry jsou nyní modelován jako výchozí parametry spíš než přetížení další metody.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-216">Optional parameters are now modeled as default parameters rather than additional method overloads.</span></span> <span data-ttu-id="c6bfc-217">To někdy výrazně snižuje počet přetížení metody.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-217">This reduces the number of method overloads, sometimes dramatically.</span></span>
* <span data-ttu-id="c6bfc-218">Rozšiřující metody teď skrýt spoustu nadbytečné podrobnosti HTTP volající.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-218">The extension methods now hide a lot of the extraneous details of HTTP from the caller.</span></span> <span data-ttu-id="c6bfc-219">Například starší verze sady SDK vrátil objekt odpověď se stavovým kódem HTTP, které často nebyla musíte zkontrolovat, protože operace metody throw `CloudException` pro stavový kód, který označuje chybu.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-219">For example, older versions of the SDK returned a response object with an HTTP status code, which you often didn't need to check because operation methods throw `CloudException` for any status code that indicates an error.</span></span> <span data-ttu-id="c6bfc-220">Nové metody rozšíření právě vrátí objekty modelu, ukládání, můžete problém toho, že je rozbalení v kódu.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-220">The new extension methods just return model objects, saving you the trouble of having to unwrap them in your code.</span></span>
* <span data-ttu-id="c6bfc-221">Naopak základní rozhraní nyní zveřejněte metody, které získáte větší kontrolu na úrovni HTTP pokud ho potřebujete.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-221">Conversely, the core interfaces now expose methods that give you more control at the HTTP level if you need it.</span></span> <span data-ttu-id="c6bfc-222">Nyní můžete předat do vlastní hlavičky protokolu HTTP, které mají být zahrnuty do požadavků a nové `AzureOperationResponse<T>` návratový typ poskytuje přímý přístup k `HttpRequestMessage` a `HttpResponseMessage` pro operaci.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-222">You can now pass in custom HTTP headers to be included in requests, and the new `AzureOperationResponse<T>` return type gives you direct access to the `HttpRequestMessage` and `HttpResponseMessage` for the operation.</span></span> <span data-ttu-id="c6bfc-223">`AzureOperationResponse`je definována v `Microsoft.Rest.Azure` obor názvů a nahradí `Hyak.Common.OperationResponse`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-223">`AzureOperationResponse` is defined in the `Microsoft.Rest.Azure` namespace and replaces `Hyak.Common.OperationResponse`.</span></span>

#### <a name="scoringparameters-changes"></a><span data-ttu-id="c6bfc-224">ScoringParameters změny</span><span class="sxs-lookup"><span data-stu-id="c6bfc-224">ScoringParameters changes</span></span>
<span data-ttu-id="c6bfc-225">Novou třídu s názvem `ScoringParameter` byla přidána do na nejnovější SDK, aby bylo snazší poskytuje parametry pro vyhodnocování profily ve vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-225">A new class named `ScoringParameter` has been added in the latest SDK to make it easier to provide parameters to scoring profiles in a search query.</span></span> <span data-ttu-id="c6bfc-226">Dříve `ScoringProfiles` vlastnost `SearchParameters` třída byl zadán jako `IList<string>`; Teď je zadán jako `IList<ScoringParameter>`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-226">Previously the `ScoringProfiles` property of the `SearchParameters` class was typed as `IList<string>`; Now it is typed as `IList<ScoringParameter>`.</span></span>

##### <a name="example"></a><span data-ttu-id="c6bfc-227">Příklad</span><span class="sxs-lookup"><span data-stu-id="c6bfc-227">Example</span></span>
<span data-ttu-id="c6bfc-228">Pokud váš kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-228">If your code looks like this:</span></span>

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

<span data-ttu-id="c6bfc-229">Můžete ji změnit k tomuto a opravte případné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-229">You can change it to this to fix any build errors:</span></span> 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a><span data-ttu-id="c6bfc-230">Změny modelu – třída</span><span class="sxs-lookup"><span data-stu-id="c6bfc-230">Model class changes</span></span>
<span data-ttu-id="c6bfc-231">Z důvodu změn podpis popsaných v [operaci metoda změny](#OperationMethodChanges), mnoho tříd v `Microsoft.Azure.Search.Models` bylo přejmenováno nebo odebrat obor názvů.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-231">Due to the signature changes described in [Operation method changes](#OperationMethodChanges), many classes in the `Microsoft.Azure.Search.Models` namespace have been renamed or removed.</span></span> <span data-ttu-id="c6bfc-232">Například:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-232">For example:</span></span>

* <span data-ttu-id="c6bfc-233">`IndexDefinitionResponse`nahradila`AzureOperationResponse<Index>`</span><span class="sxs-lookup"><span data-stu-id="c6bfc-233">`IndexDefinitionResponse` has been replaced by `AzureOperationResponse<Index>`</span></span>
* <span data-ttu-id="c6bfc-234">Přejmenování `DocumentSearchResponse` na `DocumentSearchResult`</span><span class="sxs-lookup"><span data-stu-id="c6bfc-234">`DocumentSearchResponse` has been renamed to `DocumentSearchResult`</span></span>
* <span data-ttu-id="c6bfc-235">Přejmenování `IndexResult` na `IndexingResult`</span><span class="sxs-lookup"><span data-stu-id="c6bfc-235">`IndexResult` has been renamed to `IndexingResult`</span></span>
* <span data-ttu-id="c6bfc-236">`Documents.Count()`nyní vrátí `long` s počtem dokumentů místo`DocumentCountResponse`</span><span class="sxs-lookup"><span data-stu-id="c6bfc-236">`Documents.Count()` now returns a `long` with the document count instead of a `DocumentCountResponse`</span></span>
* <span data-ttu-id="c6bfc-237">Přejmenování `IndexGetStatisticsResponse` na `IndexGetStatisticsResult`</span><span class="sxs-lookup"><span data-stu-id="c6bfc-237">`IndexGetStatisticsResponse` has been renamed to `IndexGetStatisticsResult`</span></span>
* <span data-ttu-id="c6bfc-238">Přejmenování `IndexListResponse` na `IndexListResult`</span><span class="sxs-lookup"><span data-stu-id="c6bfc-238">`IndexListResponse` has been renamed to `IndexListResult`</span></span>

<span data-ttu-id="c6bfc-239">To Shrneme, `OperationResponse`-odvozených tříd, které existovaly pouze zabalit byly odebrány objekt modelu.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-239">To summarize, `OperationResponse`-derived classes that existed only to wrap a model object have been removed.</span></span> <span data-ttu-id="c6bfc-240">Zbývající třídy předtím jejich přípona se změnila z `Response` k `Result`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-240">The remaining classes have had their suffix changed from `Response` to `Result`.</span></span>

##### <a name="example"></a><span data-ttu-id="c6bfc-241">Příklad</span><span class="sxs-lookup"><span data-stu-id="c6bfc-241">Example</span></span>
<span data-ttu-id="c6bfc-242">Pokud váš kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-242">If your code looks like this:</span></span>

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

<span data-ttu-id="c6bfc-243">Můžete ji změnit k tomuto a opravte případné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-243">You can change it to this to fix any build errors:</span></span>

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

##### <a name="response-classes-and-ienumerable"></a><span data-ttu-id="c6bfc-244">Odpověď třídy a rozhraní IEnumerable</span><span class="sxs-lookup"><span data-stu-id="c6bfc-244">Response classes and IEnumerable</span></span>
<span data-ttu-id="c6bfc-245">Další změny, které mohou ovlivnit váš kód je už implementaci třídy odpovědi, které uložení kolekce `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-245">An additional change that may affect your code is that response classes that hold collections no longer implement `IEnumerable<T>`.</span></span> <span data-ttu-id="c6bfc-246">Vlastnost kolekce místo toho můžete přistupovat přímo.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-246">Instead, you can access the collection property directly.</span></span> <span data-ttu-id="c6bfc-247">Například, pokud kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-247">For example, if your code looks like this:</span></span>

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

<span data-ttu-id="c6bfc-248">Můžete ji změnit k tomuto a opravte případné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-248">You can change it to this to fix any build errors:</span></span>

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="special-case-for-web-applications"></a><span data-ttu-id="c6bfc-249">Zvláštním případem pro webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c6bfc-249">Special case for web applications</span></span>
<span data-ttu-id="c6bfc-250">Pokud máte webové aplikace, který serializuje `DocumentSearchResponse` přímo do prohlížeče odeslat výsledky hledání, budete muset upravit svůj kód nebo výsledky nebude správně serializovat.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-250">If you have a web application that serializes `DocumentSearchResponse` directly to send search results to the browser, you will need to change your code or the results will not serialize correctly.</span></span> <span data-ttu-id="c6bfc-251">Například, pokud kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-251">For example, if your code looks like this:</span></span>

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

<span data-ttu-id="c6bfc-252">Můžete ji změnit získáním `.Results` vlastnost vyhledávání odpovědi opravit vykreslování výsledků vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-252">You can change it by getting the `.Results` property of the search response to fix search result rendering:</span></span>

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

<span data-ttu-id="c6bfc-253">Bude mít a hledat v takových případech ve vašem kódu sami. **Kompilátor nebude varovat** protože `JsonResult.Data` je typu `object`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-253">You will have to look for such cases in your code yourself; **The compiler will not warn you** because `JsonResult.Data` is of type `object`.</span></span>

#### <a name="cloudexception-changes"></a><span data-ttu-id="c6bfc-254">CloudException změny</span><span class="sxs-lookup"><span data-stu-id="c6bfc-254">CloudException changes</span></span>
<span data-ttu-id="c6bfc-255">`CloudException` Třída přesunula z `Hyak.Common` názvů `Microsoft.Rest.Azure` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-255">The `CloudException` class has moved from the `Hyak.Common` namespace to the `Microsoft.Rest.Azure` namespace.</span></span> <span data-ttu-id="c6bfc-256">Také jeho `Error` vlastnost byla přejmenována na `Body`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-256">Also, its `Error` property has been renamed to `Body`.</span></span>

#### <a name="searchserviceclient-and-searchindexclient-changes"></a><span data-ttu-id="c6bfc-257">Změny SearchServiceClient a SearchIndexClient</span><span class="sxs-lookup"><span data-stu-id="c6bfc-257">SearchServiceClient and SearchIndexClient changes</span></span>
<span data-ttu-id="c6bfc-258">Typ `Credentials` vlastnost bylo změněno z `SearchCredentials` k její základní třída `ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-258">The type of the `Credentials` property has changed from `SearchCredentials` to its base class, `ServiceClientCredentials`.</span></span> <span data-ttu-id="c6bfc-259">Pokud budete potřebovat pro přístup `SearchCredentials` z `SearchIndexClient` nebo `SearchServiceClient`, použijte prosím nový `SearchCredentials` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-259">If you need to access the `SearchCredentials` of a `SearchIndexClient` or `SearchServiceClient`, please use the new `SearchCredentials` property.</span></span>

<span data-ttu-id="c6bfc-260">Starší verze sady SDK `SearchServiceClient` a `SearchIndexClient` měl konstruktory, které trvalo `HttpClient` parametr.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-260">In older versions of the SDK, `SearchServiceClient` and `SearchIndexClient` had constructors that took an `HttpClient` parameter.</span></span> <span data-ttu-id="c6bfc-261">Tyto nahradil konstruktory, které provést `HttpClientHandler` a pole `DelegatingHandler` objekty.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-261">These have been replaced with constructors that take an `HttpClientHandler` and an array of `DelegatingHandler` objects.</span></span> <span data-ttu-id="c6bfc-262">To usnadňuje instalaci vlastní obslužné rutiny předběžné zpracování požadavků HTTP, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-262">This makes it easier to install custom handlers to pre-process HTTP requests if necessary.</span></span>

<span data-ttu-id="c6bfc-263">Nakonec konstruktory, které trvalo `Uri` a `SearchCredentials` změnily.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-263">Finally, the constructors that took a `Uri` and `SearchCredentials` have changed.</span></span> <span data-ttu-id="c6bfc-264">Například pokud máte kód, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-264">For example, if you have code that looks like this:</span></span>

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

<span data-ttu-id="c6bfc-265">Můžete ji změnit k tomuto a opravte případné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-265">You can change it to this to fix any build errors:</span></span>

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

<span data-ttu-id="c6bfc-266">Také Upozorňujeme, že typ parametru přihlašovací údaje se změnila na `ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-266">Also note that the type of the credentials parameter has changed to `ServiceClientCredentials`.</span></span> <span data-ttu-id="c6bfc-267">To se pravděpodobně ovlivnit kódu od `SearchCredentials` je odvozený od `ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-267">This is unlikely to affect your code since `SearchCredentials` is derived from `ServiceClientCredentials`.</span></span>

#### <a name="passing-a-request-id"></a><span data-ttu-id="c6bfc-268">Předání ID žádosti</span><span class="sxs-lookup"><span data-stu-id="c6bfc-268">Passing a request ID</span></span>
<span data-ttu-id="c6bfc-269">Starší verze sady SDK, můžete nastavit ID požadavku na `SearchServiceClient` nebo `SearchIndexClient` a by být součástí každého požadavku rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-269">In older versions of the SDK, you could set a request ID on the `SearchServiceClient` or `SearchIndexClient` and it would be included in every request to the REST API.</span></span> <span data-ttu-id="c6bfc-270">To je užitečné pro odstraňování potíží s služby search, pokud potřebujete obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-270">This is useful for troubleshooting issues with your search service if you need to contact support.</span></span> <span data-ttu-id="c6bfc-271">Je však užitečnější nastavit požadavek jedinečné ID pro všechny operace, a nikoli používat stejné ID pro všechny operace.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-271">However, it is more useful to set a unique request ID for every operation rather than to use the same ID for all operations.</span></span> <span data-ttu-id="c6bfc-272">Z tohoto důvodu `SetClientRequestId` metody `SearchServiceClient` a `SearchIndexClient` byly odebrány.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-272">For this reason, the `SetClientRequestId` methods of `SearchServiceClient` and `SearchIndexClient` have been removed.</span></span> <span data-ttu-id="c6bfc-273">Místo toho můžete předat ID žádosti do jednotlivých metod operace prostřednictvím nepovinný `SearchRequestOptions` parametr.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-273">Instead, you can pass a request ID to each operation method via the optional `SearchRequestOptions` parameter.</span></span>

> [!NOTE]
> <span data-ttu-id="c6bfc-274">V budoucí verzi sady SDK přidáme nové mechanismus pro ID požadavku globální nastavení na straně klienta objektů, který je konzistentní s přístupem používá jiné sady Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-274">In a future release of the SDK, we will add a new mechanism for setting a request ID globally on the client objects that is consistent with the approach used by other Azure SDKs.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="c6bfc-275">Příklad</span><span class="sxs-lookup"><span data-stu-id="c6bfc-275">Example</span></span>
<span data-ttu-id="c6bfc-276">Pokud máte kód, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-276">If you have code that looks like this:</span></span>

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

<span data-ttu-id="c6bfc-277">Můžete ji změnit k tomuto a opravte případné chyby sestavení:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-277">You can change it to this to fix any build errors:</span></span>

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a><span data-ttu-id="c6bfc-278">Změny názvu rozhraní</span><span class="sxs-lookup"><span data-stu-id="c6bfc-278">Interface name changes</span></span>
<span data-ttu-id="c6bfc-279">Názvy rozhraní skupin operaci byla změněna být konzistentní s jejich názvy vlastností, odpovídající:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-279">The operation group interface names have all changed to be consistent with their corresponding property names:</span></span>

* <span data-ttu-id="c6bfc-280">Typ `ISearchServiceClient.Indexes` byla přejmenována z `IIndexOperations` k `IIndexesOperations`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-280">The type of `ISearchServiceClient.Indexes` has been renamed from `IIndexOperations` to `IIndexesOperations`.</span></span>
* <span data-ttu-id="c6bfc-281">Typ `ISearchServiceClient.Indexers` byla přejmenována z `IIndexerOperations` k `IIndexersOperations`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-281">The type of `ISearchServiceClient.Indexers` has been renamed from `IIndexerOperations` to `IIndexersOperations`.</span></span>
* <span data-ttu-id="c6bfc-282">Typ `ISearchServiceClient.DataSources` byla přejmenována z `IDataSourceOperations` k `IDataSourcesOperations`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-282">The type of `ISearchServiceClient.DataSources` has been renamed from `IDataSourceOperations` to `IDataSourcesOperations`.</span></span>
* <span data-ttu-id="c6bfc-283">Typ `ISearchIndexClient.Documents` byla přejmenována z `IDocumentOperations` k `IDocumentsOperations`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-283">The type of `ISearchIndexClient.Documents` has been renamed from `IDocumentOperations` to `IDocumentsOperations`.</span></span>

<span data-ttu-id="c6bfc-284">Tuto změnu je nepravděpodobné, že pokud jste vytvořili mocks tato rozhraní pro testovací účely ovlivní kódu.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-284">This change is unlikely to affect your code unless you created mocks of these interfaces for test purposes.</span></span>

<a name="BugFixesV1"></a>

### <a name="bug-fixes-in-version-11"></a><span data-ttu-id="c6bfc-285">Opravy chyb v verze 1.1</span><span class="sxs-lookup"><span data-stu-id="c6bfc-285">Bug fixes in version 1.1</span></span>
<span data-ttu-id="c6bfc-286">Ve starších verzích týkající se serializace třídy vlastní modelu Azure Search .NET SDK se chyby.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-286">There was a bug in older versions of the Azure Search .NET SDK relating to serialization of custom model classes.</span></span> <span data-ttu-id="c6bfc-287">Chybě mohlo dojít, pokud jste vytvořili třídu vlastní modelu s vlastností typu hodnot neumožňující hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-287">The bug could occur if you created a custom model class with a property of a non-nullable value type.</span></span>

#### <a name="steps-to-reproduce"></a><span data-ttu-id="c6bfc-288">Kroky pro reprodukci</span><span class="sxs-lookup"><span data-stu-id="c6bfc-288">Steps to reproduce</span></span>
<span data-ttu-id="c6bfc-289">Vytvořte třídu, vlastní modelu s vlastností typu hodnot neumožňující hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-289">Create a custom model class with a property of non-nullable value type.</span></span> <span data-ttu-id="c6bfc-290">Například přidejte veřejné `UnitCount` vlastnost typu `int` místo `int?`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-290">For example, add a public `UnitCount` property of type `int` instead of `int?`.</span></span>

<span data-ttu-id="c6bfc-291">Pokud indexu dokument s výchozí hodnotou tohoto typu (například 0 pro `int`), bude toto pole hodnotu null ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-291">If you index a document with the default value of that type (for example, 0 for `int`), the field will be null in Azure Search.</span></span> <span data-ttu-id="c6bfc-292">Pokud následně vyhledávání pro tento dokument `Search` volání vyvolá výjimku `JsonSerializationException` nesouhlasících, kterou nelze převést `null` k `int`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-292">If you subsequently search for that document, the `Search` call will throw `JsonSerializationException` complaining that it can't convert `null` to `int`.</span></span>

<span data-ttu-id="c6bfc-293">Navíc filtry nemusí fungovat podle očekávání, protože hodnota null byla zapsána do indexu místo určená hodnota.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-293">Also, filters may not work as expected since null was written to the index instead of the intended value.</span></span>

#### <a name="fix-details"></a><span data-ttu-id="c6bfc-294">Opravte podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c6bfc-294">Fix details</span></span>
<span data-ttu-id="c6bfc-295">Vyřešili jsme tento problém v verze 1.1 sady SDK.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-295">We have fixed this issue in version 1.1 of the SDK.</span></span> <span data-ttu-id="c6bfc-296">Nyní, pokud máte třídu modelu takto:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-296">Now, if you have a model class like this:</span></span>

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

<span data-ttu-id="c6bfc-297">a nastavíte `IntValue` na hodnotu 0, tuto hodnotu je nyní správně serializovanou jako 0 v drátové síti a uloží jako 0 v indexu.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-297">and you set `IntValue` to 0, that value is now correctly serialized as 0 on the wire and stored as 0 in the index.</span></span> <span data-ttu-id="c6bfc-298">Zaokrouhlí vypínání také funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-298">Round tripping also works as expected.</span></span>

<span data-ttu-id="c6bfc-299">Neexistuje jeden potenciální problém s tímto přístupem znát: Pokud použijete typ modelu s hodnotou Null vlastností, budete muset **zaručit** aby žádné dokumenty v indexu neobsahovaly pro odpovídající pole hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-299">There is one potential issue to be aware of with this approach: If you use a model type with a non-nullable property, you have to **guarantee** that no documents in your index contain a null value for the corresponding field.</span></span> <span data-ttu-id="c6bfc-300">Sada SDK, ani REST API služby Azure Search vám pomůže vynutit.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-300">Neither the SDK nor the Azure Search REST API will help you to enforce this.</span></span>

<span data-ttu-id="c6bfc-301">Nejedná se pouze o hypotetický problém: představte si situaci, kdy přidáte nové pole do stávajícího indexu typu `Edm.Int32`.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-301">This is not just a hypothetical concern: Imagine a scenario where you add a new field to an existing index that is of type `Edm.Int32`.</span></span> <span data-ttu-id="c6bfc-302">Po aktualizaci definice indexu budou mít všechny dokumenty pro toto nové pole hodnotu null (protože všechny typy jsou ve službě Azure Search s možností null).</span><span class="sxs-lookup"><span data-stu-id="c6bfc-302">After updating the index definition, all documents will have a null value for that new field (since all types are nullable in Azure Search).</span></span> <span data-ttu-id="c6bfc-303">Pokud pak použijete třídu modelu s vlastností `int` se zakázanou hodnotou null, při pokusu o načtení dokumentů dojde k vyvolání podobné výjimky `JsonSerializationException`:</span><span class="sxs-lookup"><span data-stu-id="c6bfc-303">If you then use a model class with a non-nullable `int` property for that field, you will get a `JsonSerializationException` like this when trying to retrieve documents:</span></span>

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

<span data-ttu-id="c6bfc-304">Z tohoto důvodu doporučujeme jako osvědčený postup použijte typy s možnou hodnotou Null ve třídách modelu.</span><span class="sxs-lookup"><span data-stu-id="c6bfc-304">For this reason, we still recommend that you use nullable types in your model classes as a best practice.</span></span>

<span data-ttu-id="c6bfc-305">Další podrobnosti o této chyby a opravu, najdete v tématu [potíže na Githubu](https://github.com/Azure/azure-sdk-for-net/issues/1063).</span><span class="sxs-lookup"><span data-stu-id="c6bfc-305">For more details on this bug and the fix, please see [this issue on GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).</span></span>

