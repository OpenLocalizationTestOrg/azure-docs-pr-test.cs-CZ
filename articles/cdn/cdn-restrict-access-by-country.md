---
title: "Omezit přístup k obsahu Azure CDN podle země | Microsoft Docs"
description: "Zjistěte, jak omezit přístup k vašemu obsahu Azure CDN pomocí funkce geograficky filtrování."
services: cdn
documentationcenter: 
author: lichard
manager: akucer
editor: 
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 30160088d9c770400f342e67527e1cf1cabc4f6b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="restrict-azure-cdn-content-by-country"></a><span data-ttu-id="70232-103">Omezit přístup k obsahu Azure CDN podle země</span><span class="sxs-lookup"><span data-stu-id="70232-103">Restrict Azure CDN content by country</span></span>

## <a name="overview"></a><span data-ttu-id="70232-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="70232-104">Overview</span></span>
<span data-ttu-id="70232-105">Když si uživatel vyžádá obsah, ve výchozím nastavení, je zpracovat obsah bez ohledu na to, kde uživatel provedené této žádosti z.</span><span class="sxs-lookup"><span data-stu-id="70232-105">When a user requests your content, by default, the content is served regardless of where the user made this request from.</span></span> <span data-ttu-id="70232-106">V některých případech můžete chtít omezit přístup k vašemu obsahu podle země.</span><span class="sxs-lookup"><span data-stu-id="70232-106">In some cases, you may want to restrict access to your content by country.</span></span> <span data-ttu-id="70232-107">Toto téma vysvětluje, jak používat **filtrování geograficky** funkci chcete-li nakonfigurovat službu, kterou chcete povolit nebo blokovat přístup podle země.</span><span class="sxs-lookup"><span data-stu-id="70232-107">This topic explains how to use the **Geo-Filtering** feature in order to configure the service to allow or block access by country.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="70232-108">Verizon a Akamai produkty nabízí stejnou funkčnost geograficky filtrování, ale mají malý rozdíl v te země kódy, které podporují.</span><span class="sxs-lookup"><span data-stu-id="70232-108">The Verizon and Akamai products provide the same geo-filtering functionality but have a small difference in te country codes they support.</span></span> <span data-ttu-id="70232-109">Odkaz na rozdíly najdete v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="70232-109">See Step 3 for a link to the differences.</span></span>


<span data-ttu-id="70232-110">Informace o aspektech, které platí pro konfiguraci tohoto typu omezení najdete v tématu [aspekty](cdn-restrict-access-by-country.md#considerations) na konci tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="70232-110">For information about considerations that apply to configuring this type of restriction, see the [Considerations](cdn-restrict-access-by-country.md#considerations) section at the end of the topic.</span></span>  

![Filtrování podle země](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-the-directory-path"></a><span data-ttu-id="70232-112">Krok 1: Definování cesta k adresáři</span><span class="sxs-lookup"><span data-stu-id="70232-112">Step 1: Define the directory path</span></span>
<span data-ttu-id="70232-113">Vyberte koncový bod služby v rámci portálu a najít na kartě filtrování geograficky v levé navigaci k vyhledání této funkce.</span><span class="sxs-lookup"><span data-stu-id="70232-113">Select your endpoint within the portal, and find the Geo-Filtering tab on the left-hand navigation to find this feature.</span></span>

<span data-ttu-id="70232-114">Při konfiguraci filtr země, zadejte relativní cestu k umístění, do které se uživatelům povoluje nebo odepírá přístup.</span><span class="sxs-lookup"><span data-stu-id="70232-114">When configuring a country filter, you must specify the relative path to the location to which users will be allowed or denied access.</span></span> <span data-ttu-id="70232-115">Filtrování geograficky můžete použít pro všechny soubory s "/" nebo vybrané složky zadáním cesty k adresáři "/ obrázky /".</span><span class="sxs-lookup"><span data-stu-id="70232-115">You can apply geo-filtering for all your files with "/" or selected folders by specifying directory paths "/pictures/".</span></span> <span data-ttu-id="70232-116">Můžete taky použít filtrování geograficky do jednoho souboru zadáním soubor a ponechat si do adresy koncové lomítko "/ obrazky/Mesto.png".</span><span class="sxs-lookup"><span data-stu-id="70232-116">You can also apply geo-filtering to a single file by specifying the file, and leaving out the trailing slash "/pictures/city.png".</span></span>

<span data-ttu-id="70232-117">Příklad filtru cesta adresáře:</span><span class="sxs-lookup"><span data-stu-id="70232-117">Example directory path filter:</span></span>

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-the-action-block-or-allow"></a><span data-ttu-id="70232-118">Krok 2: Definování akce: blokování nebo povolení</span><span class="sxs-lookup"><span data-stu-id="70232-118">Step 2: Define the action: block or allow</span></span>
<span data-ttu-id="70232-119">**Blokování:** uživatelů ze zadané zemí bude odepřen přístup k prostředkům vyžádaný z této cestě rekurzivní.</span><span class="sxs-lookup"><span data-stu-id="70232-119">**Block:** Users from the specified countries will be denied access to assets requested from that recursive path.</span></span> <span data-ttu-id="70232-120">Pokud pro toto umístění nebyly nakonfigurovány žádné jiné země možnosti filtrování, pak všichni ostatní uživatelé budou mít povolený přístup.</span><span class="sxs-lookup"><span data-stu-id="70232-120">If no other country filtering options have been configured for that location, then all other users will be allowed access.</span></span>

<span data-ttu-id="70232-121">**Povolit:** jen uživatelé ze zemí zadaný budou mít povolený přístup k prostředkům vyžádaný z této cestě rekurzivní.</span><span class="sxs-lookup"><span data-stu-id="70232-121">**Allow:** Only users from the specified countries will be allowed access to assets requested from that recursive path.</span></span>

## <a name="step-3-define-the-countries"></a><span data-ttu-id="70232-122">Krok 3: Definování zemí</span><span class="sxs-lookup"><span data-stu-id="70232-122">Step 3: Define the countries</span></span>
<span data-ttu-id="70232-123">Vyberte země, ve které chcete blokovat nebo povolit pro danou cestu.</span><span class="sxs-lookup"><span data-stu-id="70232-123">Select the countries that you want to block or allow for the path.</span></span> 

<span data-ttu-id="70232-124">Například pravidlo blokování /Photos/Štrasburku/bude filtrovat soubory včetně:</span><span class="sxs-lookup"><span data-stu-id="70232-124">For example, the rule of blocking /Photos/Strasbourg/ will filter files including:</span></span>

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a><span data-ttu-id="70232-125">Kódy zemí</span><span class="sxs-lookup"><span data-stu-id="70232-125">Country codes</span></span>
<span data-ttu-id="70232-126">**Filtrování geograficky** funkce používá k definování zemí, ze kterých se žádost o povolené nebo blokované pro zabezpečené adresář kódy zemí.</span><span class="sxs-lookup"><span data-stu-id="70232-126">The **Geo-Filtering** feature uses country codes to define the countries from which a request will be allowed or blocked for a secured directory.</span></span> <span data-ttu-id="70232-127">Zjistíte, kódy zemí v [kódy zemí CDN Azure](https://msdn.microsoft.com/library/mt761717.aspx).</span><span class="sxs-lookup"><span data-stu-id="70232-127">You will find the country codes in [Azure CDN  Country Codes](https://msdn.microsoft.com/library/mt761717.aspx).</span></span> 

## <span data-ttu-id="70232-128"><a id="considerations"></a>Důležité informace</span><span class="sxs-lookup"><span data-stu-id="70232-128"><a id="considerations"></a>Considerations</span></span>
* <span data-ttu-id="70232-129">Verizon nebo několik minut s Akamai, změny ve vaší zemi filtrování k provedení změn konfigurace může trvat až 90 minut.</span><span class="sxs-lookup"><span data-stu-id="70232-129">It may take up to 90 minutes for Verizon, or a couple minutes with Akamai, for changes to your country filtering configuration to take effect.</span></span>
* <span data-ttu-id="70232-130">Tato funkce nepodporuje zástupné znaky (například ' *').</span><span class="sxs-lookup"><span data-stu-id="70232-130">This feature does not support wildcard characters (for example, ‘*’).</span></span>
* <span data-ttu-id="70232-131">Konfigurace filtrování geograficky spojené s relativní cestou bude rekurzivně této cesty.</span><span class="sxs-lookup"><span data-stu-id="70232-131">The geo-filtering configuration associated with the relative path will be applied recursively to that path.</span></span>
* <span data-ttu-id="70232-132">Pouze jedno pravidlo můžete použít stejné relativní cestu (nemůžete vytvořit více filtrů země, které odkazují na stejné relativní cestě.</span><span class="sxs-lookup"><span data-stu-id="70232-132">Only one rule can be applied to the same relative path (you cannot create multiple country filters that point to the same relative path.</span></span> <span data-ttu-id="70232-133">Do složky, ale může mít více filtrů země.</span><span class="sxs-lookup"><span data-stu-id="70232-133">However, a folder may have multiple country filters.</span></span> <span data-ttu-id="70232-134">Toto je vzhledem k povaze rekurzivní země filtrů.</span><span class="sxs-lookup"><span data-stu-id="70232-134">This is due to the recursive nature of country filters.</span></span> <span data-ttu-id="70232-135">Jinými slovy podsložkou dříve nakonfigurované složky lze přiřadit jiné zemi filtru.</span><span class="sxs-lookup"><span data-stu-id="70232-135">In other words, a subfolder of a previously configured folder can be assigned a different country filter.</span></span>

