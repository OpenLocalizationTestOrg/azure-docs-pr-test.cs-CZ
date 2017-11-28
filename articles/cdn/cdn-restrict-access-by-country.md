---
title: "aaaRestrict obsahu Azure CDN podle země | Microsoft Docs"
description: "Zjistěte, jak toorestrict přístupu k tooyour Azure CDN obsahu pomocí funkce filtrování geograficky hello."
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
ms.openlocfilehash: ffdd994612b6c9cfbf1a6e29d260709b4afa86e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restrict-azure-cdn-content-by-country"></a><span data-ttu-id="2784a-103">Omezit přístup k obsahu Azure CDN podle země</span><span class="sxs-lookup"><span data-stu-id="2784a-103">Restrict Azure CDN content by country</span></span>

## <a name="overview"></a><span data-ttu-id="2784a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="2784a-104">Overview</span></span>
<span data-ttu-id="2784a-105">Když si uživatel vyžádá obsah, ve výchozím nastavení, hello obsahu obsluhovaného bez ohledu na to, kde uživatel hello provedené této žádosti z.</span><span class="sxs-lookup"><span data-stu-id="2784a-105">When a user requests your content, by default, hello content is served regardless of where hello user made this request from.</span></span> <span data-ttu-id="2784a-106">V některých případech může být vhodné toorestrict přístup k obsahu tooyour podle země.</span><span class="sxs-lookup"><span data-stu-id="2784a-106">In some cases, you may want toorestrict access tooyour content by country.</span></span> <span data-ttu-id="2784a-107">Toto téma vysvětluje, jak toouse hello **filtrování geograficky** funkce v tooconfigure pořadí hello služby tooallow nebo blokování přístupu podle země.</span><span class="sxs-lookup"><span data-stu-id="2784a-107">This topic explains how toouse hello **Geo-Filtering** feature in order tooconfigure hello service tooallow or block access by country.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2784a-108">Verizon a Akamai produkty Hello poskytují hello stejné funkce filtrování geograficky, ale mají malý rozdíl v kódy zemí te podporují.</span><span class="sxs-lookup"><span data-stu-id="2784a-108">hello Verizon and Akamai products provide hello same geo-filtering functionality but have a small difference in te country codes they support.</span></span> <span data-ttu-id="2784a-109">Najdete v kroku 3 rozdíly toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="2784a-109">See Step 3 for a link toohello differences.</span></span>


<span data-ttu-id="2784a-110">Informace o aspektech, které se vztahují tooconfiguring tento typ omezení, najdete v části hello [aspekty](cdn-restrict-access-by-country.md#considerations) části na konci hello hello tématu.</span><span class="sxs-lookup"><span data-stu-id="2784a-110">For information about considerations that apply tooconfiguring this type of restriction, see hello [Considerations](cdn-restrict-access-by-country.md#considerations) section at hello end of hello topic.</span></span>  

![Filtrování podle země](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-hello-directory-path"></a><span data-ttu-id="2784a-112">Krok 1: Definování cesta k adresáři hello</span><span class="sxs-lookup"><span data-stu-id="2784a-112">Step 1: Define hello directory path</span></span>
<span data-ttu-id="2784a-113">Vyberte váš koncový bod hello portálu a najít hello filtrování geograficky karty v levé navigační toofind hello tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="2784a-113">Select your endpoint within hello portal, and find hello Geo-Filtering tab on hello left-hand navigation toofind this feature.</span></span>

<span data-ttu-id="2784a-114">Při konfiguraci země filtru, musíte zadat hello relativní cestu toohello umístění toowhich uživatelé se povoluje nebo odepírá přístup.</span><span class="sxs-lookup"><span data-stu-id="2784a-114">When configuring a country filter, you must specify hello relative path toohello location toowhich users will be allowed or denied access.</span></span> <span data-ttu-id="2784a-115">Filtrování geograficky můžete použít pro všechny soubory s "/" nebo vybrané složky zadáním cesty k adresáři "/ obrázky /".</span><span class="sxs-lookup"><span data-stu-id="2784a-115">You can apply geo-filtering for all your files with "/" or selected folders by specifying directory paths "/pictures/".</span></span> <span data-ttu-id="2784a-116">Můžete taky použít geograficky filtrování tooa jedním souborem zadáním hello souboru a vynechala hello koncové lomítko "/ obrazky/Mesto.png".</span><span class="sxs-lookup"><span data-stu-id="2784a-116">You can also apply geo-filtering tooa single file by specifying hello file, and leaving out hello trailing slash "/pictures/city.png".</span></span>

<span data-ttu-id="2784a-117">Příklad filtru cesta adresáře:</span><span class="sxs-lookup"><span data-stu-id="2784a-117">Example directory path filter:</span></span>

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-hello-action-block-or-allow"></a><span data-ttu-id="2784a-118">Krok 2: Definování akce hello: blokování nebo povolení</span><span class="sxs-lookup"><span data-stu-id="2784a-118">Step 2: Define hello action: block or allow</span></span>
<span data-ttu-id="2784a-119">**Blokování:** uživatele z hello zadaný požadovaný z této cesty rekurzivní tooassets přístup bude odepřen zemích.</span><span class="sxs-lookup"><span data-stu-id="2784a-119">**Block:** Users from hello specified countries will be denied access tooassets requested from that recursive path.</span></span> <span data-ttu-id="2784a-120">Pokud pro toto umístění nebyly nakonfigurovány žádné jiné země možnosti filtrování, pak všichni ostatní uživatelé budou mít povolený přístup.</span><span class="sxs-lookup"><span data-stu-id="2784a-120">If no other country filtering options have been configured for that location, then all other users will be allowed access.</span></span>

<span data-ttu-id="2784a-121">**Povolit:** jen uživatelé z hello zadaný zemích budou mít povolený přístup tooassets vyžádaný z této cestě rekurzivní.</span><span class="sxs-lookup"><span data-stu-id="2784a-121">**Allow:** Only users from hello specified countries will be allowed access tooassets requested from that recursive path.</span></span>

## <a name="step-3-define-hello-countries"></a><span data-ttu-id="2784a-122">Krok 3: Definování zemích hello</span><span class="sxs-lookup"><span data-stu-id="2784a-122">Step 3: Define hello countries</span></span>
<span data-ttu-id="2784a-123">Vyberte hello zemích, mají být tooblock nebo povolit pro cestu hello.</span><span class="sxs-lookup"><span data-stu-id="2784a-123">Select hello countries that you want tooblock or allow for hello path.</span></span> 

<span data-ttu-id="2784a-124">Například hello pravidlo blokování /Photos/Štrasburku/bude filtrovat soubory včetně:</span><span class="sxs-lookup"><span data-stu-id="2784a-124">For example, hello rule of blocking /Photos/Strasbourg/ will filter files including:</span></span>

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a><span data-ttu-id="2784a-125">Kódy zemí</span><span class="sxs-lookup"><span data-stu-id="2784a-125">Country codes</span></span>
<span data-ttu-id="2784a-126">Hello **filtrování geograficky** funkce používá země kódy toodefine hello zemích ze kterých žádost povolené nebo blokované pro zabezpečené adresář.</span><span class="sxs-lookup"><span data-stu-id="2784a-126">hello **Geo-Filtering** feature uses country codes toodefine hello countries from which a request will be allowed or blocked for a secured directory.</span></span> <span data-ttu-id="2784a-127">Zjistíte hello kódy zemí v [kódy zemí CDN Azure](https://msdn.microsoft.com/library/mt761717.aspx).</span><span class="sxs-lookup"><span data-stu-id="2784a-127">You will find hello country codes in [Azure CDN  Country Codes](https://msdn.microsoft.com/library/mt761717.aspx).</span></span> 

## <span data-ttu-id="2784a-128"><a id="considerations"></a>Důležité informace</span><span class="sxs-lookup"><span data-stu-id="2784a-128"><a id="considerations"></a>Considerations</span></span>
* <span data-ttu-id="2784a-129">To může trvat až minut too90 Verizon nebo několik minut s Akamai, změny tooyour země filtrování konfigurace tootake vliv.</span><span class="sxs-lookup"><span data-stu-id="2784a-129">It may take up too90 minutes for Verizon, or a couple minutes with Akamai, for changes tooyour country filtering configuration tootake effect.</span></span>
* <span data-ttu-id="2784a-130">Tato funkce nepodporuje zástupné znaky (například ' *').</span><span class="sxs-lookup"><span data-stu-id="2784a-130">This feature does not support wildcard characters (for example, ‘*’).</span></span>
* <span data-ttu-id="2784a-131">Hello filtrování geograficky konfigurace související s relativní cestou hello bude rekurzivně toothat cesta.</span><span class="sxs-lookup"><span data-stu-id="2784a-131">hello geo-filtering configuration associated with hello relative path will be applied recursively toothat path.</span></span>
* <span data-ttu-id="2784a-132">Pouze jedno pravidlo může být použité toohello stejné relativní cestě (nelze vytvořit více filtrů země tento bod toohello stejné relativní cestě.</span><span class="sxs-lookup"><span data-stu-id="2784a-132">Only one rule can be applied toohello same relative path (you cannot create multiple country filters that point toohello same relative path.</span></span> <span data-ttu-id="2784a-133">Do složky, ale může mít více filtrů země.</span><span class="sxs-lookup"><span data-stu-id="2784a-133">However, a folder may have multiple country filters.</span></span> <span data-ttu-id="2784a-134">Toto je z důvodu toohello rekurzivní povaha země filtry.</span><span class="sxs-lookup"><span data-stu-id="2784a-134">This is due toohello recursive nature of country filters.</span></span> <span data-ttu-id="2784a-135">Jinými slovy podsložkou dříve nakonfigurované složky lze přiřadit jiné zemi filtru.</span><span class="sxs-lookup"><span data-stu-id="2784a-135">In other words, a subfolder of a previously configured folder can be assigned a different country filter.</span></span>

