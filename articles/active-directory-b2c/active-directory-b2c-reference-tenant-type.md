---
title: "Azure Active Directory B2C: Dostupnost & data sídle oblast | Microsoft Docs"
description: "Téma na typech hello klientům Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: gsacavdm
manager: krassk
editor: bryanla
ms.assetid: 8a0644da-b825-4edc-8ce9-541c3c976afb
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: gsacavdm
ms.openlocfilehash: d382921fe9d62183b6d52c47d62f952dabd116c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a><span data-ttu-id="ab5e7-103">Azure Active Directory B2C: Dostupnost & data sídle oblast</span><span class="sxs-lookup"><span data-stu-id="ab5e7-103">Azure Active Directory B2C: Region availability & data residency</span></span>
<span data-ttu-id="ab5e7-104">Dostupnost v oblastech a sídla dat jsou dvě příliš neliší koncepty, které jinak použít tooAzure AD B2C z hello rest Azure.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-104">Region availability and data residency are two very different concepts that apply differently tooAzure AD B2C from hello rest of Azure.</span></span> <span data-ttu-id="ab5e7-105">Tento článek se popisují hello rozdíly mezi těmito dvěma konceptů a porovnání, jak se vztahují tooAzure a Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-105">This article will explain hello differences between these two concepts and compare how they apply tooAzure versus Azure AD B2C.</span></span>

## <a name="summary"></a><span data-ttu-id="ab5e7-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ab5e7-106">Summary</span></span>
<span data-ttu-id="ab5e7-107">Azure AD B2C je **všeobecně dostupná po celém světě** s hello možností pro **sídle data ve Spojených státech amerických nebo Evropa**.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-107">Azure AD B2C is **generally available worldwide** with hello option for **data residency in United States or Europe**.</span></span>

## <a name="concepts"></a><span data-ttu-id="ab5e7-108">Koncepty</span><span class="sxs-lookup"><span data-stu-id="ab5e7-108">Concepts</span></span>
* <span data-ttu-id="ab5e7-109">**Dostupnost v oblastech** odkazuje toowhere je k dispozici pro použití služby.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-109">**Region availability** refers toowhere a service is available for use.</span></span>
* <span data-ttu-id="ab5e7-110">**Data sídle** odkazuje toowhere uživatele jsou uložena data.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-110">**Data residency** refers toowhere user data is stored.</span></span>

## <a name="region-availability"></a><span data-ttu-id="ab5e7-111">Dostupnost v oblastech</span><span class="sxs-lookup"><span data-stu-id="ab5e7-111">Region availability</span></span>
<span data-ttu-id="ab5e7-112">Azure AD B2C je k dispozici po celém světě prostřednictvím hello veřejného cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-112">Azure AD B2C is available worldwide via hello Azure public cloud.</span></span> 

<span data-ttu-id="ab5e7-113">To se liší od hello modelu většině ostatních služeb Azure postupujte podle kroků, které spojte dostupnosti s sídle data.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-113">This differs from hello model most other Azure services follow which couple availability with data residency.</span></span> <span data-ttu-id="ab5e7-114">Vidíte příklady tohoto v obou Azure [produkty dostupné podle oblasti](https://azure.microsoft.com/regions/services/) stránku a hello [Active Directory B2C cenové kalkulačky](https://azure.microsoft.com/pricing/details/active-directory-b2c/).</span><span class="sxs-lookup"><span data-stu-id="ab5e7-114">You can see examples of this in both Azure's [Products Available By Region](https://azure.microsoft.com/regions/services/) page and hello [Active Directory B2C pricing calculator](https://azure.microsoft.com/pricing/details/active-directory-b2c/).</span></span>

## <a name="data-residency"></a><span data-ttu-id="ab5e7-115">Rezidence dat</span><span class="sxs-lookup"><span data-stu-id="ab5e7-115">Data residency</span></span>
<span data-ttu-id="ab5e7-116">Azure AD B2C data uživatelů uloží ve Spojených státech amerických nebo Evropa.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-116">Azure AD B2C stores user data in either United States or Europe.</span></span>

<span data-ttu-id="ab5e7-117">Sídlo dat je určen podle zemi nebo region je vybraná při [vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ab5e7-117">Data residency is determined based on which country/region is selected when [creating an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

![Snímek obrazovky preview klienta](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

<span data-ttu-id="ab5e7-119">Data se nachází v hello Spojených států pro následující zemích hello:</span><span class="sxs-lookup"><span data-stu-id="ab5e7-119">Data resides in hello United States for hello following countries/regions:</span></span>

> <span data-ttu-id="ab5e7-120">Spojené státy, Kanada, Kostarika, Dominikánská republika, El Salvador, Kostarika, Mexico, Panama, Portoriko a Trinidad a Tobago</span><span class="sxs-lookup"><span data-stu-id="ab5e7-120">United States, Canada, Costa Rica, Dominican Republic, El Salvador, Guatemala, Mexico, Panama, Puerto Rico and Trinidad & Tobago</span></span>

<span data-ttu-id="ab5e7-121">Data se nachází v Evropě pro hello následující zemích nebo oblastech:</span><span class="sxs-lookup"><span data-stu-id="ab5e7-121">Data resides in Europe for hello following countries/regions:</span></span>

> <span data-ttu-id="ab5e7-122">Alžírsko, Rakousko, Ázerbájdžán, Bahrajn, Běloruska, Belgie, Bulharsko, Chorvatsko, Kypru, Česká republika, Dánsko, Egypta, Estonska, Finsko, Francie, Německo, Řecko, Maďarska, Island, Irská, Izrael, Itálie, Jordánsko, Kazachstán, Keni, Kuvajt, Lotyšska, Libanon, Lichtenštejnsko, Lituania, Lucembursko, Makedonie FYRO, Malty, Černá Hora, Maroko, Nizozemsko, Nigérie, Norsko, Omán, Pákistánu, Polsko, Portugalsko, Katar, Rumunska, Ruska, Saúdská Arábie, Srbsko, Slovenska, Slovinsko, Jihoafrická republika, Španělsko, Švédsko, Švýcarska, Tunisko, Turecko, Ukrajina, Spojené arabské emiráty a Spojené království.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-122">Algeria, Austria, Azerbaijan, Bahrain, Belarus, Belgium, Bulgaria, Croatia, Cyprus, Czech Republic, Denmark, Egypt, Estonia, Finland, France, Germany, Greece, Hungary, Iceland, Ireland, Israel, Italy, Jordan, Kazakhstan, Kenya, Kuwait, Lativa, Lebanon, Liechtenstein, Lituania, Luxembourg, Macedonia FYRO, Malta, Montenegro, Morocco, Netherlands, Nigeria, Norway, Oman, Pakistan, Poland, Portugal, Qatar, Romania, Russia, Saudi Arabia, Serbia, Slovakia, Slovenia, South Africa, Spain, Sweden, Switzerland, Tunisia, Turkey, Ukraine, United Arab Emirates and United Kingdom.</span></span>

<span data-ttu-id="ab5e7-123">Hello zbývající zemích jsou v procesu hello přidávané toohello seznamu.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-123">hello remaining countries/regions are in hello process of being added toohello list.</span></span>  <span data-ttu-id="ab5e7-124">Teď stále můžete Azure AD B2C výběrem žádné hello zemích výše.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-124">For now, you can still use Azure AD B2C by picking any of hello countries/regions above.</span></span>

> <span data-ttu-id="ab5e7-125">Afghánistán, Argentiny, Austrálie, Brazílie, základě, Kolumbie, Ekvádor, Hongkong – zvláštní správní oblast ČLR, Indie, Indonésie, Irák, Japonsko, Korejská, Malajsie, Nový Zéland, Paraguay, Peru, Filipíny, Singapur, Srí Lanka, Tchaj-wan, Thajska, Uruguayského a Venezuela.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-125">Afghanistan, Argentina, Australia, Brazil, Chile, Colombia, Ecuador, Hong Kong SAR, India, Indonesia, Iraq, Japan, Korea, Malaysia, New Zealand, Paraguay, Peru, Philippines, Singapore, Sri Lanka, Taiwan, Thailand, Uruguay and Venezuela.</span></span>

## <a name="preview-tenant"></a><span data-ttu-id="ab5e7-126">Náhled klienta</span><span class="sxs-lookup"><span data-stu-id="ab5e7-126">Preview tenant</span></span>
<span data-ttu-id="ab5e7-127">Pokud jste vytvořili klienta B2C během období preview Azure AD B2C, je pravděpodobné, který vaše **klienta typu** uvádí **Preview klienta**.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-127">If you had created a B2C tenant during Azure AD B2C's preview period, it is likely that your **Tenant type** says **Preview tenant**.</span></span> <span data-ttu-id="ab5e7-128">Pokud se jedná o případ hello, používejte pouze pro účely vývoje a testování a ne pro produkční aplikace vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-128">If this is hello case, you MUST use your tenant only for development and testing purposes, and NOT for production apps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ab5e7-129">Neexistuje cesta migrace z náhled klienta B2C produkční škálování tooa klienta B2C.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-129">There is no migration path from a preview B2C tenant tooa production-scale B2C tenant.</span></span> <span data-ttu-id="ab5e7-130">Všimněte si, že existují známé problémy při odstranění preview B2C klienta a znovu vytvořit klienta B2C produkční škálování s hello se stejným názvem domény.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-130">Note that there are known issues when you delete a preview B2C tenant and re-create a production-scale B2C tenant with hello same domain name.</span></span> <span data-ttu-id="ab5e7-131">Máte toocreate klienta B2C produkční škálování s jiným názvem domény.</span><span class="sxs-lookup"><span data-stu-id="ab5e7-131">You have toocreate a production-scale B2C tenant with a different domain name.</span></span>


![Snímek obrazovky preview klienta](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
