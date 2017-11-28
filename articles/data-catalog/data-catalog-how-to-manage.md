---
title: "aaaManage datových prostředcích v Azure Data Catalog | Microsoft Docs"
description: "článek Hello označuje, jak zaregistrovat toocontrol viditelnost a vlastnictví datových prostředků v Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 623f5ed4-8da7-48f5-943a-448d0b7cba69
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 48a634b92d7da19c32c9e551f295eec257f54f1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-data-assets-in-azure-data-catalog"></a><span data-ttu-id="3e176-103">Spravovat datové prostředky v Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="3e176-103">Manage data assets in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="3e176-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="3e176-104">Introduction</span></span>
<span data-ttu-id="3e176-105">Azure Data Catalog je určen pro zdroj dat zjišťování, takže lze snadno vyhledat a pochopit zdroje dat hello potřebujete tooperform analýzy a rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="3e176-105">Azure Data Catalog is designed for data-source discovery, so that you can easily discover and understand hello data sources you need tooperform analysis and make decisions.</span></span> <span data-ttu-id="3e176-106">Tyto funkce zjišťování zajistěte největších dopad hello při vy a ostatní uživatelé můžete najít a pochopit hello nejširší řadu dostupných zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="3e176-106">These discovery capabilities make hello biggest impact when you and other users can find and understand hello broadest range of available data sources.</span></span> <span data-ttu-id="3e176-107">S těmito prvky pamatovat hello výchozí chování katalogu Data Catalog je pro všechny registrované datové zdroje toobe viditelné tooand zjistitelný všichni uživatelé katalogu.</span><span class="sxs-lookup"><span data-stu-id="3e176-107">With these elements in mind, hello default behavior of Data Catalog is for all registered data sources toobe visible tooand discoverable by all catalog users.</span></span>

<span data-ttu-id="3e176-108">Data Catalog nezískáte přístup toohello samotná data.</span><span class="sxs-lookup"><span data-stu-id="3e176-108">Data Catalog does not give you access toohello data itself.</span></span> <span data-ttu-id="3e176-109">Vlastníkem hello hello zdroje dat se řídí přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="3e176-109">Data access is controlled by hello owner of hello data source.</span></span> <span data-ttu-id="3e176-110">Pomocí katalogu Data Catalog můžete zjistit zdroje dat a zobrazit hello metadata, která je související toohello zdroje, které jsou zaregistrovány v katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="3e176-110">With Data Catalog, you can discover data sources and view hello metadata that's related toohello sources that are registered in hello catalog.</span></span>

<span data-ttu-id="3e176-111">Můžou nastat situace, ale kde zdroje dat mají jenom viditelné toospecific uživatele nebo toomembers určitých skupin.</span><span class="sxs-lookup"><span data-stu-id="3e176-111">There might be situations, however, where data sources should only be visible toospecific users, or toomembers of specific groups.</span></span> <span data-ttu-id="3e176-112">V takových scénářů uživatelé převzít vlastnictví registrované datové prostředky v rámci katalogu hello a pak řídit viditelnost hello hello prostředků, které vlastní.</span><span class="sxs-lookup"><span data-stu-id="3e176-112">In such scenarios, users can take ownership of registered data assets within hello catalog and then control hello visibility of hello assets they own.</span></span>

> [!NOTE]
> <span data-ttu-id="3e176-113">Hello funkcí popsaných v tomto článku je k dispozici pouze v hello Azure Data Catalog, Standard Edition.</span><span class="sxs-lookup"><span data-stu-id="3e176-113">hello functionality described in this article is available only in hello Standard Edition of Azure Data Catalog.</span></span> <span data-ttu-id="3e176-114">Edice Free Hello nenabízí možnosti pro vlastnictví a omezení viditelnosti datový prostředek.</span><span class="sxs-lookup"><span data-stu-id="3e176-114">hello Free Edition does not provide capabilities for ownership and restricting data-asset visibility.</span></span>
>
>

## <a name="manage-ownership-of-data-assets"></a><span data-ttu-id="3e176-115">Spravovat vlastnictví datových prostředků</span><span class="sxs-lookup"><span data-stu-id="3e176-115">Manage ownership of data assets</span></span>
<span data-ttu-id="3e176-116">Ve výchozím nastavení jsou bez přiřazeného vlastnictví datových prostředků, které jsou zaregistrovány v katalogu Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="3e176-116">By default, data assets that are registered in Data Catalog are unowned.</span></span> <span data-ttu-id="3e176-117">Každý uživatel s oprávnění tooaccess hello katalogu můžete zjistit a opatřit poznámkami tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="3e176-117">Any user with permission tooaccess hello catalog can discover and annotate these assets.</span></span> <span data-ttu-id="3e176-118">Uživatelé mohou převzít vlastnictví bez přiřazeného vlastnictví datových prostředků a pak omezit viditelnost hello hello prostředků, které vlastní.</span><span class="sxs-lookup"><span data-stu-id="3e176-118">Users can take ownership of unowned data assets and then limit hello visibility of hello assets they own.</span></span>

<span data-ttu-id="3e176-119">Pokud vlastní datový prostředek v katalogu Data Catalog, pouze uživatelé, kteří jsou autorizovány hello vlastníky můžete zjistit hello asset a zobrazit jeho metadata, a pouze hello vlastníky můžete odstranit prostředek hello z katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="3e176-119">When a data asset in Data Catalog is owned, only users who are authorized by hello owners can discover hello asset and view its metadata, and only hello owners can delete hello asset from hello catalog.</span></span>

> [!NOTE]
> <span data-ttu-id="3e176-120">Vlastnictví v katalogu Data Catalog ovlivňuje pouze hello metadata, která je uložena v katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="3e176-120">Ownership in Data Catalog affects only hello metadata that's stored in hello catalog.</span></span> <span data-ttu-id="3e176-121">Vlastnictví neuděluje všechna oprávnění v podkladovém zdroji dat hello.</span><span class="sxs-lookup"><span data-stu-id="3e176-121">Ownership does not confer any permissions on hello underlying data source.</span></span>
>
>

### <a name="take-ownership"></a><span data-ttu-id="3e176-122">Přebírat vlastnictví</span><span class="sxs-lookup"><span data-stu-id="3e176-122">Take ownership</span></span>
<span data-ttu-id="3e176-123">Uživatelé mohou převzít vlastnictví datových prostředků tak, že vyberete hello **převzít vlastnictví** možnost portálu hello katalogu Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="3e176-123">Users can take ownership of data assets by selecting hello **Take Ownership** option in hello Data Catalog portal.</span></span> <span data-ttu-id="3e176-124">Žádná zvláštní oprávnění jsou požadované tootake vlastnictví prostředek bez přiřazeného vlastnictví data.</span><span class="sxs-lookup"><span data-stu-id="3e176-124">No special permissions are required tootake ownership of an unowned data asset.</span></span> <span data-ttu-id="3e176-125">Každý uživatel může převzít vlastnictví prostředek bez přiřazeného vlastnictví data.</span><span class="sxs-lookup"><span data-stu-id="3e176-125">Any user can take ownership of an unowned data asset.</span></span>

### <a name="add-owners-and-co-owners"></a><span data-ttu-id="3e176-126">Přidat vlastníků a spoluvlastníci</span><span class="sxs-lookup"><span data-stu-id="3e176-126">Add owners and co-owners</span></span>
<span data-ttu-id="3e176-127">Pokud je již vlastněn datový prostředek, nelze ostatní uživatelé jednoduše převzít vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="3e176-127">If a data asset is already owned, other users cannot simply take ownership.</span></span> <span data-ttu-id="3e176-128">Je nutné přidat jako spoluvlastníci existující vlastník.</span><span class="sxs-lookup"><span data-stu-id="3e176-128">They must be added as co-owners by an existing owner.</span></span> <span data-ttu-id="3e176-129">Všechny vlastníka můžete přidat další uživatele nebo skupiny zabezpečení jako spoluvlastníci.</span><span class="sxs-lookup"><span data-stu-id="3e176-129">Any owner can add additional users or security groups as co-owners.</span></span>

> [!NOTE]
> <span data-ttu-id="3e176-130">Ho je nejlepší postup toohave aspoň dva jednotlivce jako vlastníci pro všechny vlastní datový prostředek.</span><span class="sxs-lookup"><span data-stu-id="3e176-130">It is a best practice toohave at least two individuals as owners for any owned data asset.</span></span>
>
>

### <a name="remove-owners"></a><span data-ttu-id="3e176-131">Odebrat vlastníky</span><span class="sxs-lookup"><span data-stu-id="3e176-131">Remove owners</span></span>
<span data-ttu-id="3e176-132">Stejně jako všechny vlastník majetku můžete přidat spoluvlastníky, můžete odebrat všechny vlastník majetku žádné spoluvlastník.</span><span class="sxs-lookup"><span data-stu-id="3e176-132">Just as any asset owner can add co-owners, any asset owner can remove any co-owner.</span></span>

<span data-ttu-id="3e176-133">Vlastníka prostředku, který odebere mu nebo samu sebe jako vlastníka již nebude možné spravovat hello asset.</span><span class="sxs-lookup"><span data-stu-id="3e176-133">An asset owner who removes him or herself as an owner can no longer manage hello asset.</span></span> <span data-ttu-id="3e176-134">Pokud vlastník majetku hello odebere mu nebo samu sebe jako vlastníka a neexistují žádné společné vlastníci, vrátí hello asset tooan bez přiřazeného vlastníka stavu.</span><span class="sxs-lookup"><span data-stu-id="3e176-134">If hello asset owner removes him or herself as an owner and there are no other co-owners, hello asset reverts tooan unowned state.</span></span>

## <a name="control-visibility"></a><span data-ttu-id="3e176-135">Přehled ovládacího prvku</span><span class="sxs-lookup"><span data-stu-id="3e176-135">Control visibility</span></span>
<span data-ttu-id="3e176-136">Datový prostředek vlastníky můžete řídit viditelnost hello hello datových prostředků, které vlastní.</span><span class="sxs-lookup"><span data-stu-id="3e176-136">Data-asset owners can control hello visibility of hello data assets they own.</span></span> <span data-ttu-id="3e176-137">viditelnost toorestrict jako výchozí hello, kde všechny katalog Data Catalog smí uživatelé zjistit a zobrazení hello datový prostředek, vlastník majetku hello můžete přepínat hello viditelnost nastavení z **Everyone** příliš**vlastníci a tito uživatelé** v hello vlastnosti pro prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="3e176-137">toorestrict visibility as hello default, where all Data Catalog users can discover and view hello data asset, hello asset owner can toggle hello visibility setting from **Everyone** too**Owners & These Users** in hello properties for hello asset.</span></span> <span data-ttu-id="3e176-138">Vlastníci poté můžete přidat konkrétní uživatele a skupiny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3e176-138">Owners can then add specific users and security groups.</span></span>

> [!NOTE]
> <span data-ttu-id="3e176-139">Kdykoli je to možné, měli oprávnění vlastnictví a viditelnost asset přiřadit není tooindividual uživatelé a skupiny toosecurity.</span><span class="sxs-lookup"><span data-stu-id="3e176-139">Whenever possible, asset ownership and visibility permissions should be assigned toosecurity groups and not tooindividual users.</span></span>
>
>

## <a name="catalog-administrators"></a><span data-ttu-id="3e176-140">Správci katalogu</span><span class="sxs-lookup"><span data-stu-id="3e176-140">Catalog administrators</span></span>
<span data-ttu-id="3e176-141">Správci katalogu dat se implicitně společné vlastníci všechny prostředky v katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="3e176-141">Data Catalog administrators are implicitly co-owners of all assets in hello catalog.</span></span> <span data-ttu-id="3e176-142">Vlastníci Asset nelze odebrat viditelnost od správců, a můžou správci spravovat vlastnictví a viditelnost pro všechny datové prostředky v katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="3e176-142">Asset owners cannot remove visibility from administrators, and administrators can manage ownership and visibility for all data assets in hello catalog.</span></span>

## <a name="summary"></a><span data-ttu-id="3e176-143">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3e176-143">Summary</span></span>
<span data-ttu-id="3e176-144">Hello katalogu Data Catalog crowdsourcingový model toometadata zjišťování a dat asset umožňuje všechny toocontribute uživatelé katalogu a zjišťování.</span><span class="sxs-lookup"><span data-stu-id="3e176-144">hello Data Catalog crowdsourcing model toometadata and data asset discovery allows all catalog users toocontribute and discover.</span></span> <span data-ttu-id="3e176-145">Hello Data Catalog, Standard Edition je určená pro vlastnictví a správu toolimit hello viditelnost a použít konkrétní datových prostředků.</span><span class="sxs-lookup"><span data-stu-id="3e176-145">hello Standard Edition of Data Catalog is designed for ownership and management toolimit hello visibility and use of specific data assets.</span></span>
