---
title: "AAA \"Vytvoření indexu (rozhraní .NET API - Azure Search) | Microsoft Docs\""
description: "Vytvořte index v kódu pomocí hello Azure Search .NET SDK."
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 3a851647-fc7b-4fb6-8506-6aaa519e77cd
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/22/2017
ms.author: brjohnst
ms.openlocfilehash: 7fa4030b8c3565bc02b1d6bb4426331657cf3a5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-index-using-hello-net-sdk"></a>Vytvoření indexu Azure Search pomocí .NET SDK hello
> [!div class="op_single_selector"]
> * [Přehled](search-what-is-an-index.md)
> * [Azure Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Tento článek vás provede procesem vytvoření Azure Search hello [index](https://docs.microsoft.com/rest/api/searchservice/Create-Index) pomocí hello [Azure Search .NET SDK](https://aka.ms/search-sdk).

Předtím, než podle těchto pokynů vytvoříte index, byste už měli mít [vytvořenou službu Azure Search](search-create-service-portal.md).

> [!NOTE]
> Ukázkový kód v tomto článku je napsán v jazyce C#. Hello úplný zdrojový kód najdete [na Githubu](http://aka.ms/search-dotnet-howto). Můžete si také přečíst o hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) pro podrobnější procházení prostřednictvím hello ukázek kódu.


## <a name="identify-your-azure-search-services-admin-api-key"></a>Identifikace klíče rozhraní API správce služby Azure Search
Teď, když máte zřízenou službu Azure Search, jste skoro hotový tooissue požadavky na váš koncový bod služby, pomocí hello .NET SDK. Nejprve budete potřebovat tooobtain mezi hello správce klíče api Key vytvořených pro hello vyhledávací službu, kterou jste zřídili. Hello .NET SDK odešle tento klíč rozhraní api služby tooyour každý požadavek. Platný klíč vytváří na základě žádosti mezi hello aplikace odesílání hello požadavku a hello služby, která ji zpracovává vztah důvěryhodnosti.

1. toofind klíče služby api Key, přihlaste se toohello [portálu Azure](https://portal.azure.com/)
2. Okno služby Azure Search přejděte tooyour
3. Klikněte na hello ikonu klíčů."

Vaše služba bude mít *klíče správce* a *klíče dotazů*.

* Primární a sekundární *klíče správce* udělit úplná práva tooall operace, včetně hello možnost toomanage hello služby, vytvářet a odstraňovat indexy, indexery a zdroje dat.. Existují dva klíče, aby mohl pokračovat toouse hello sekundární klíč, pokud se rozhodnete tooregenerate hello primární klíč a naopak.
* Vaše *klíče dotazů* udělit oprávnění jen pro čtení tooindexes a dokumentům a obvykle distribuované tooclient aplikace, které vydávají požadavky hledání.

Pro účely vytvoření indexu hello, můžete použít buď vaší primární nebo sekundární klíč správce.

<a name="CreateSearchServiceClient"></a>

## <a name="create-an-instance-of-hello-searchserviceclient-class"></a>Vytvoření instance třídy SearchServiceClient hello
pomocí toostart hello .NET SDK služby Azure Search, budete potřebovat toocreate instanci hello `SearchServiceClient` třídy. Tato třída obsahuje několik konstruktorů. Dobrý den, ten, který chcete přebírá název vaší vyhledávací služby a `SearchCredentials` jako parametry. `SearchCredentials` zabalí váš klíč api-key.

Hello následující kód vytvoří novou `SearchServiceClient` pomocí hodnot pro název vyhledávací služby hello a rozhraní api-key, které jsou uložené v konfiguračním souboru aplikace hello (`appsettings.json` v případě hello hello [ukázkové aplikace](http://aka.ms/search-dotnet-howto)):

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

`Indexes` má vlastnost `SearchServiceClient`. Tato vlastnost poskytuje všechny metody hello potřebovat toocreate, seznam, aktualizace nebo odstranění indexů Azure Search.

> [!NOTE]
> Hello `SearchServiceClient` třída spravuje připojení tooyour vyhledávací službu. V pořadí tooavoid otevírání příliš mnoha připojení, že byste měli zkusit tooshare jednu instanci `SearchServiceClient` ve vaší aplikaci pokud je to možné. Její metody jsou bezpečné pro přístup z více vláken tooenable takové sdílení.
> 
> 

<a name="DefineIndex"></a>

## <a name="define-your-azure-search-index"></a>Definování indexu Azure Search
Jednoho volání toohello `Indexes.Create` metoda vytvoří váš index. Tato metoda přebírá jako parametr objekt `Index`, který definuje index Azure Search. Je třeba toocreate `Index` objektu a provést jeho inicializaci následujícím způsobem:

1. Sada hello `Name` vlastnost hello `Index` objekt toohello název indexu.
2. Sada hello `Fields` vlastnost hello `Index` pole tooan objektu `Field` objekty. Hello nejjednodušší způsob, jak toocreate hello `Field` objekty, je volání hello `FieldBuilder.BuildForType` metodu předáním třídu modelu pro parametr typu hello. Třídu modelu má vlastnosti, které mapování polí toohello indexu. To vám umožní toobind dokumenty z vaší tooinstances indexu vyhledávání vaší třídy modelu.

> [!NOTE]
> Pokud neplánujete toouse třídu modelu, stále můžete definovat indexu vytvořením `Field` objekty přímo. Můžete zadat název hello hello pole toohello konstruktoru, společně s datovým typem hello (nebo analyzátor pro pole řetězce). Můžete také nastavit další vlastnosti, například `IsSearchable`, `IsFilterable` atd.
>
>

Je důležité, aby byl vyhledávání uživatelské prostředí a obchodní potřeby v úvahu při navrhování indexu, protože každé pole musí mít přiřazen hello [příslušné vlastnosti](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Tyto vlastnosti určují, které funkce vyhledávání (filtrování, používání faset, řazení fulltextového vyhledávání atd.) použít toowhich pole. Pro vlastnost, kterou nenastavíte explicitně, hello `Field` třída výchozí toodisabling hello odpovídající funkce hledání, pokud ji specificky nepovolíte.

V našem příkladu jsme nazvali index „hotels“ a pole jsme definovali pomocí třídy modelu. Každá vlastnost třídy modelu hello má atributy, které určují chování související s vyhledávání hello hello odpovídající index pole. třídy modelu Hello je definován následujícím způsobem:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// hello SerializePropertyNamesAsCamelCase attribute is defined in hello Azure Search .NET SDK.
// It ensures that Pascal-case property names in hello model class are mapped toocamel-case
// field names in hello index.
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }
}
```

Pečlivě zvolili hello atributy pro každou vlastnost závislosti na tom, jak myslíme si, že se pravděpodobně použijí v aplikaci. Například je pravděpodobné, že lidé hledající hotely se budou zajímat klíčového slova v hello `description` pole, takže jsme povolit fulltextové vyhledávání pro toto pole přidáním hello `IsSearchable` atribut toohello `Description` vlastnost.

Upozorňujeme, že právě jedno pole v indexu typu `string` musí být hello určeny jako hello *klíč* pole přidáním hello `Key` atribut (viz `HotelId` v hello výše příklad).

výše uvedená definice indexu Hello používá analyzátor jazyka pro hello `description_fr` pole, protože je určený toostore francouzského textu. V tématu [tématu jazykové podpory hello](https://docs.microsoft.com/rest/api/searchservice/Language-support) a také odpovídající hello [příspěvku na blogu](https://azure.microsoft.com/blog/language-support-in-azure-search/) Další informace o analyzátorech jazyka.

> [!NOTE]
> Ve výchozím nastavení je hello název každé vlastnosti ve třídě modelu slouží jako název hello hello odpovídající pole v indexu hello. Pokud chcete toomap vlastnost názvy toocamel případ názvy všech polí, označte hello třídu s hello `SerializePropertyNamesAsCamelCase` atribut. Pokud chcete, aby toomap tooa jiný název, můžete použít hello `JsonProperty` atribut jako hello `DescriptionFr` vlastnost výše. Hello `JsonProperty` atribut má přednost před hello `SerializePropertyNamesAsCamelCase` atribut.
> 
> 

Teď, když jsme nadefinovali třídu modelu, můžeme velmi snadno vytvořit definici indexu:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = FieldBuilder.BuildForType<Hotel>()
};
```

## <a name="create-hello-index"></a>Vytvořte hello index
Teď, když jste inicializovali `Index` objekt, můžete vytvořit hello index jednoduchým voláním `Indexes.Create` na vaše `SearchServiceClient` objektu:

```csharp
serviceClient.Indexes.Create(definition);
```

Pro úspěšné žádosti hello metoda vrátí normálně. Pokud dojde k problému s hello žádostí, jako je například neplatný parametr, vyvolá výjimku hello metoda `CloudException`.

Když jste hotovi s toodelete indexu a chcete ho, stačí zavolat hello `Indexes.Delete` metoda na vaše `SearchServiceClient`. Jedná se například jak jsme odstranili index "hotels" hello:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> Hello ukázkový kód v tomto článku používá pro jednoduchost synchronní metody hello hello Azure Search .NET SDK. Doporučujeme použít hello asynchronních metod v tookeep vlastní aplikace je škálovatelné a dobře reagovaly. Například v hello příklady výše můžete použít `CreateAsync` a `DeleteAsync` místo `Create` a `Delete`.
> 
> 

## <a name="next-steps"></a>Další kroky
Po vytvoření indexu Azure Search, budete moci příliš[nahrát obsah do indexu hello](search-what-is-data-import.md) , abyste mohli začít prohledávat data.

