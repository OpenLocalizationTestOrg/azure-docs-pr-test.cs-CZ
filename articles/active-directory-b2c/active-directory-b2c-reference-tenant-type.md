---
title: "Azure Active Directory B2C: Dostupnost & data sídle oblast | Microsoft Docs"
description: "Téma na typy klientů, které Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: gsacavdm
manager: mtillman
editor: bryanla
ms.assetid: 8a0644da-b825-4edc-8ce9-541c3c976afb
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: gsacavdm
ms.openlocfilehash: 752a98ca7f3c77c434de296461790f2cf37e2d5c
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a><span data-ttu-id="3f325-103">Azure Active Directory B2C: Dostupnost & data sídle oblast</span><span class="sxs-lookup"><span data-stu-id="3f325-103">Azure Active Directory B2C: Region availability & data residency</span></span>
<span data-ttu-id="3f325-104">Dostupnost v oblastech a sídla dat jsou dvě velmi různých koncepty, které jinak týkají Azure AD B2C, od zbytku Azure.</span><span class="sxs-lookup"><span data-stu-id="3f325-104">Region availability and data residency are two very different concepts that apply differently to Azure AD B2C from the rest of Azure.</span></span> <span data-ttu-id="3f325-105">Tento článek se popisují rozdíly mezi těmito dvěma konceptů a porovnání, jak se vztahují na Azure oproti využívání Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="3f325-105">This article will explain the differences between these two concepts and compare how they apply to Azure versus Azure AD B2C.</span></span>

## <a name="summary"></a><span data-ttu-id="3f325-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3f325-106">Summary</span></span>
<span data-ttu-id="3f325-107">Azure AD B2C je **všeobecně dostupná po celém světě** s možností pro **sídle data ve Spojených státech amerických nebo Evropa**.</span><span class="sxs-lookup"><span data-stu-id="3f325-107">Azure AD B2C is **generally available worldwide** with the option for **data residency in United States or Europe**.</span></span>

## <a name="concepts"></a><span data-ttu-id="3f325-108">Koncepty</span><span class="sxs-lookup"><span data-stu-id="3f325-108">Concepts</span></span>
* <span data-ttu-id="3f325-109">**Dostupnost v oblastech** odkazuje na to, kde je k dispozici pro použití služby.</span><span class="sxs-lookup"><span data-stu-id="3f325-109">**Region availability** refers to where a service is available for use.</span></span>
* <span data-ttu-id="3f325-110">**Data sídle** odkazuje na kterém jsou uložená data uživatele.</span><span class="sxs-lookup"><span data-stu-id="3f325-110">**Data residency** refers to where user data is stored.</span></span>

## <a name="region-availability"></a><span data-ttu-id="3f325-111">Dostupnost v oblastech</span><span class="sxs-lookup"><span data-stu-id="3f325-111">Region availability</span></span>
<span data-ttu-id="3f325-112">Azure AD B2C je k dispozici po celém světě prostřednictvím veřejného cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="3f325-112">Azure AD B2C is available worldwide via the Azure public cloud.</span></span> 

<span data-ttu-id="3f325-113">To se liší od modelu většině ostatních služeb Azure postupujte podle kroků, které spojte dostupnosti s sídle data.</span><span class="sxs-lookup"><span data-stu-id="3f325-113">This differs from the model most other Azure services follow which couple availability with data residency.</span></span> <span data-ttu-id="3f325-114">Zobrazí se tato příklady v obou Azure [produkty dostupné podle oblasti](https://azure.microsoft.com/regions/services/) stránky a [cenové kalkulačky Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).</span><span class="sxs-lookup"><span data-stu-id="3f325-114">You can see examples of this in both Azure's [Products Available By Region](https://azure.microsoft.com/regions/services/) page and the [Active Directory B2C pricing calculator](https://azure.microsoft.com/pricing/details/active-directory-b2c/).</span></span>

## <a name="data-residency"></a><span data-ttu-id="3f325-115">Rezidence dat</span><span class="sxs-lookup"><span data-stu-id="3f325-115">Data residency</span></span>
<span data-ttu-id="3f325-116">Azure AD B2C data uživatelů uloží ve Spojených státech amerických nebo Evropa.</span><span class="sxs-lookup"><span data-stu-id="3f325-116">Azure AD B2C stores user data in either United States or Europe.</span></span>

<span data-ttu-id="3f325-117">Sídlo dat je určen podle zemi nebo region je vybraná při [vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3f325-117">Data residency is determined based on which country/region is selected when [creating an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

![Snímek obrazovky preview klienta](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

<span data-ttu-id="3f325-119">Data se nachází ve Spojených státech amerických pro následující země nebo oblasti:</span><span class="sxs-lookup"><span data-stu-id="3f325-119">Data resides in the United States for the following countries/regions:</span></span>

> <span data-ttu-id="3f325-120">Spojené státy, Kanada, Kostarika, Dominikánská republika, El Salvador, Kostarika, Mexico, Panama, Portoriko a Trinidad a Tobago</span><span class="sxs-lookup"><span data-stu-id="3f325-120">United States, Canada, Costa Rica, Dominican Republic, El Salvador, Guatemala, Mexico, Panama, Puerto Rico and Trinidad & Tobago</span></span>

<span data-ttu-id="3f325-121">Data se nachází v Evropě pro následující země nebo oblasti:</span><span class="sxs-lookup"><span data-stu-id="3f325-121">Data resides in Europe for the following countries/regions:</span></span>

> <span data-ttu-id="3f325-122">Alžírsko, Rakousko, Ázerbájdžán, Bahrajn, Běloruska, Belgie, Bulharsko, Chorvatsko, Kypru, Česká republika, Dánsko, Egypta, Estonska, Finsko, Francie, Německo, Řecko, Maďarska, Island, Irská, Izrael, Itálie, Jordánsko, Kazachstán, Keni, Kuvajt, Lotyšska, Libanon, Lichtenštejnsko, Lituania, Lucembursko, Makedonie FYRO, Malty, Černá Hora, Maroko, Nizozemsko, Nigérie, Norsko, Omán, Pákistánu, Polsko, Portugalsko, Katar, Rumunska, Ruska, Saúdská Arábie, Srbsko, Slovenska, Slovinsko, Jihoafrická republika, Španělsko, Švédsko, Švýcarska, Tunisko, Turecko, Ukrajina, Spojené arabské emiráty a Spojené království.</span><span class="sxs-lookup"><span data-stu-id="3f325-122">Algeria, Austria, Azerbaijan, Bahrain, Belarus, Belgium, Bulgaria, Croatia, Cyprus, Czech Republic, Denmark, Egypt, Estonia, Finland, France, Germany, Greece, Hungary, Iceland, Ireland, Israel, Italy, Jordan, Kazakhstan, Kenya, Kuwait, Lativa, Lebanon, Liechtenstein, Lituania, Luxembourg, Macedonia FYRO, Malta, Montenegro, Morocco, Netherlands, Nigeria, Norway, Oman, Pakistan, Poland, Portugal, Qatar, Romania, Russia, Saudi Arabia, Serbia, Slovakia, Slovenia, South Africa, Spain, Sweden, Switzerland, Tunisia, Turkey, Ukraine, United Arab Emirates and United Kingdom.</span></span>

<span data-ttu-id="3f325-123">Zbývající zemích probíhá se přidává do seznamu.</span><span class="sxs-lookup"><span data-stu-id="3f325-123">The remaining countries/regions are in the process of being added to the list.</span></span>  <span data-ttu-id="3f325-124">Teď stále můžete Azure AD B2C výběrem některé z výše uvedených zemí/oblastí.</span><span class="sxs-lookup"><span data-stu-id="3f325-124">For now, you can still use Azure AD B2C by picking any of the countries/regions above.</span></span>

> <span data-ttu-id="3f325-125">Afghánistán, Argentiny, Austrálie, Brazílie, základě, Kolumbie, Ekvádor, Hongkong – zvláštní správní oblast ČLR, Indie, Indonésie, Irák, Japonsko, Korejská, Malajsie, Nový Zéland, Paraguay, Peru, Filipíny, Singapur, Srí Lanka, Tchaj-wan, Thajska, Uruguayského a Venezuela.</span><span class="sxs-lookup"><span data-stu-id="3f325-125">Afghanistan, Argentina, Australia, Brazil, Chile, Colombia, Ecuador, Hong Kong SAR, India, Indonesia, Iraq, Japan, Korea, Malaysia, New Zealand, Paraguay, Peru, Philippines, Singapore, Sri Lanka, Taiwan, Thailand, Uruguay and Venezuela.</span></span>

## <a name="preview-tenant"></a><span data-ttu-id="3f325-126">Tenant ve verzi Preview</span><span class="sxs-lookup"><span data-stu-id="3f325-126">Preview tenant</span></span>
<span data-ttu-id="3f325-127">Pokud jste vytvořili klienta B2C během období preview Azure AD B2C, je pravděpodobné, který vaše **klienta typu** uvádí **Preview klienta**.</span><span class="sxs-lookup"><span data-stu-id="3f325-127">If you had created a B2C tenant during Azure AD B2C's preview period, it is likely that your **Tenant type** says **Preview tenant**.</span></span> <span data-ttu-id="3f325-128">Pokud je to tento případ, používejte pouze pro účely vývoje a testování a ne pro produkční aplikace vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="3f325-128">If this is the case, you MUST use your tenant only for development and testing purposes, and NOT for production apps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f325-129">Neexistuje žádné cesty migrace z náhled klienta B2C na produkční škálování klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="3f325-129">There is no migration path from a preview B2C tenant to a production-scale B2C tenant.</span></span> <span data-ttu-id="3f325-130">Všimněte si, že existují známé problémy při odstranit preview B2C klienta a znovu vytvořit klienta B2C produkční škálování se stejným názvem domény.</span><span class="sxs-lookup"><span data-stu-id="3f325-130">Note that there are known issues when you delete a preview B2C tenant and re-create a production-scale B2C tenant with the same domain name.</span></span> <span data-ttu-id="3f325-131">Je nutné vytvořit klienta B2C produkční škálování s jiným názvem domény.</span><span class="sxs-lookup"><span data-stu-id="3f325-131">You have to create a production-scale B2C tenant with a different domain name.</span></span>


![Snímek obrazovky preview klienta](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
