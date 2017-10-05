---
title: "Spravovat datové prostředky v Azure Data Catalog | Microsoft Docs"
description: "V článku se dozvíte, jak řídit viditelnost a vlastnictví datových prostředků, které jsou zaregistrované v Azure Data Catalog."
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
ms.openlocfilehash: 8b9159b7bc4f7b2dac12d9012c6c903e75a6ac16
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manage-data-assets-in-azure-data-catalog"></a><span data-ttu-id="05cbd-103">Spravovat datové prostředky v Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="05cbd-103">Manage data assets in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="05cbd-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="05cbd-104">Introduction</span></span>
<span data-ttu-id="05cbd-105">Azure Data Catalog je určená pro zdroj dat zjišťování, takže můžete snadno vyhledat a pochopit zdroje dat, které budete muset provádět analýzy a rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="05cbd-105">Azure Data Catalog is designed for data-source discovery, so that you can easily discover and understand the data sources you need to perform analysis and make decisions.</span></span> <span data-ttu-id="05cbd-106">Tyto funkce zjišťování zajistěte největších dopad při vy a ostatní uživatelé můžete najít a pochopit nejširší řadu dostupných zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="05cbd-106">These discovery capabilities make the biggest impact when you and other users can find and understand the broadest range of available data sources.</span></span> <span data-ttu-id="05cbd-107">S těmito prvky na paměti je výchozí chování katalogu Data Catalog pro všechny zdroje dat registrované být viditelné a zjistitelný všichni uživatelé katalogu.</span><span class="sxs-lookup"><span data-stu-id="05cbd-107">With these elements in mind, the default behavior of Data Catalog is for all registered data sources to be visible to and discoverable by all catalog users.</span></span>

<span data-ttu-id="05cbd-108">Data Catalog není umožňují přístup k samotná data.</span><span class="sxs-lookup"><span data-stu-id="05cbd-108">Data Catalog does not give you access to the data itself.</span></span> <span data-ttu-id="05cbd-109">Vlastníkem zdroje dat se řídí přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="05cbd-109">Data access is controlled by the owner of the data source.</span></span> <span data-ttu-id="05cbd-110">Pomocí katalogu Data Catalog můžete zjistit zdroje dat a zobrazit metadata, která souvisí s zdrojům, které jsou zaregistrovány v katalogu.</span><span class="sxs-lookup"><span data-stu-id="05cbd-110">With Data Catalog, you can discover data sources and view the metadata that's related to the sources that are registered in the catalog.</span></span>

<span data-ttu-id="05cbd-111">Můžou nastat situace, ale kde zdroje dat by měly být jenom viditelné určitým uživatelům nebo do členů určitých skupin.</span><span class="sxs-lookup"><span data-stu-id="05cbd-111">There might be situations, however, where data sources should only be visible to specific users, or to members of specific groups.</span></span> <span data-ttu-id="05cbd-112">V takových scénářů uživatelé převzít vlastnictví registrované datové prostředky v rámci katalogu a pak řídit viditelnost prostředků, které vlastní.</span><span class="sxs-lookup"><span data-stu-id="05cbd-112">In such scenarios, users can take ownership of registered data assets within the catalog and then control the visibility of the assets they own.</span></span>

> [!NOTE]
> <span data-ttu-id="05cbd-113">Funkce popsané v tomto článku je k dispozici pouze v Azure Data Catalog, Standard Edition.</span><span class="sxs-lookup"><span data-stu-id="05cbd-113">The functionality described in this article is available only in the Standard Edition of Azure Data Catalog.</span></span> <span data-ttu-id="05cbd-114">Edice Free nenabízí možnosti pro vlastnictví a omezení viditelnosti datový prostředek.</span><span class="sxs-lookup"><span data-stu-id="05cbd-114">The Free Edition does not provide capabilities for ownership and restricting data-asset visibility.</span></span>
>
>

## <a name="manage-ownership-of-data-assets"></a><span data-ttu-id="05cbd-115">Spravovat vlastnictví datových prostředků</span><span class="sxs-lookup"><span data-stu-id="05cbd-115">Manage ownership of data assets</span></span>
<span data-ttu-id="05cbd-116">Ve výchozím nastavení jsou bez přiřazeného vlastnictví datových prostředků, které jsou zaregistrovány v katalogu Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="05cbd-116">By default, data assets that are registered in Data Catalog are unowned.</span></span> <span data-ttu-id="05cbd-117">Každý uživatel s oprávněním pro přístup ke katalogu můžete zjistit a opatřit poznámkami tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="05cbd-117">Any user with permission to access the catalog can discover and annotate these assets.</span></span> <span data-ttu-id="05cbd-118">Uživatelé mohou převzít vlastnictví bez přiřazeného vlastnictví datových prostředků a pak omezit viditelnost prostředků, které vlastní.</span><span class="sxs-lookup"><span data-stu-id="05cbd-118">Users can take ownership of unowned data assets and then limit the visibility of the assets they own.</span></span>

<span data-ttu-id="05cbd-119">Pokud vlastní datový prostředek v katalogu Data Catalog, jenom uživatelé, kteří jsou autorizovaní vlastníky můžete zjistit asset a zobrazit jeho metadata, a pouze vlastníci můžete odstranit i asset z katalogu.</span><span class="sxs-lookup"><span data-stu-id="05cbd-119">When a data asset in Data Catalog is owned, only users who are authorized by the owners can discover the asset and view its metadata, and only the owners can delete the asset from the catalog.</span></span>

> [!NOTE]
> <span data-ttu-id="05cbd-120">Vlastnictví v katalogu Data Catalog ovlivňuje pouze metadata, která je uložené v katalogu.</span><span class="sxs-lookup"><span data-stu-id="05cbd-120">Ownership in Data Catalog affects only the metadata that's stored in the catalog.</span></span> <span data-ttu-id="05cbd-121">Vlastnictví neuděluje všechna oprávnění v podkladovém zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="05cbd-121">Ownership does not confer any permissions on the underlying data source.</span></span>
>
>

### <a name="take-ownership"></a><span data-ttu-id="05cbd-122">Přebírat vlastnictví</span><span class="sxs-lookup"><span data-stu-id="05cbd-122">Take ownership</span></span>
<span data-ttu-id="05cbd-123">Uživatelé mohou převzít vlastnictví datových prostředků tak, že vyberete **převzít vlastnictví** možnost na portálu katalogu Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="05cbd-123">Users can take ownership of data assets by selecting the **Take Ownership** option in the Data Catalog portal.</span></span> <span data-ttu-id="05cbd-124">Žádné speciální oprávnění jsou vyžadována k převzetí vlastnictví prostředek bez přiřazeného vlastnictví data.</span><span class="sxs-lookup"><span data-stu-id="05cbd-124">No special permissions are required to take ownership of an unowned data asset.</span></span> <span data-ttu-id="05cbd-125">Každý uživatel může převzít vlastnictví prostředek bez přiřazeného vlastnictví data.</span><span class="sxs-lookup"><span data-stu-id="05cbd-125">Any user can take ownership of an unowned data asset.</span></span>

### <a name="add-owners-and-co-owners"></a><span data-ttu-id="05cbd-126">Přidat vlastníků a spoluvlastníci</span><span class="sxs-lookup"><span data-stu-id="05cbd-126">Add owners and co-owners</span></span>
<span data-ttu-id="05cbd-127">Pokud je již vlastněn datový prostředek, nelze ostatní uživatelé jednoduše převzít vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="05cbd-127">If a data asset is already owned, other users cannot simply take ownership.</span></span> <span data-ttu-id="05cbd-128">Je nutné přidat jako spoluvlastníci existující vlastník.</span><span class="sxs-lookup"><span data-stu-id="05cbd-128">They must be added as co-owners by an existing owner.</span></span> <span data-ttu-id="05cbd-129">Všechny vlastníka můžete přidat další uživatele nebo skupiny zabezpečení jako spoluvlastníci.</span><span class="sxs-lookup"><span data-stu-id="05cbd-129">Any owner can add additional users or security groups as co-owners.</span></span>

> [!NOTE]
> <span data-ttu-id="05cbd-130">Je vhodné mít alespoň dva jednotlivce jako vlastníci pro všechny vlastní datový prostředek.</span><span class="sxs-lookup"><span data-stu-id="05cbd-130">It is a best practice to have at least two individuals as owners for any owned data asset.</span></span>
>
>

### <a name="remove-owners"></a><span data-ttu-id="05cbd-131">Odebrat vlastníky</span><span class="sxs-lookup"><span data-stu-id="05cbd-131">Remove owners</span></span>
<span data-ttu-id="05cbd-132">Stejně jako všechny vlastník majetku můžete přidat spoluvlastníky, můžete odebrat všechny vlastník majetku žádné spoluvlastník.</span><span class="sxs-lookup"><span data-stu-id="05cbd-132">Just as any asset owner can add co-owners, any asset owner can remove any co-owner.</span></span>

<span data-ttu-id="05cbd-133">Vlastníka prostředku, který odebere mu nebo samu sebe jako vlastníka již nebude možné spravovat asset.</span><span class="sxs-lookup"><span data-stu-id="05cbd-133">An asset owner who removes him or herself as an owner can no longer manage the asset.</span></span> <span data-ttu-id="05cbd-134">Pokud vlastník majetku odebere mu nebo samu sebe jako vlastníka a neexistují žádné společné vlastníci, asset přejde do stavu bez přiřazeného vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="05cbd-134">If the asset owner removes him or herself as an owner and there are no other co-owners, the asset reverts to an unowned state.</span></span>

## <a name="control-visibility"></a><span data-ttu-id="05cbd-135">Přehled ovládacího prvku</span><span class="sxs-lookup"><span data-stu-id="05cbd-135">Control visibility</span></span>
<span data-ttu-id="05cbd-136">Datový prostředek vlastníky můžete řídit viditelnost datových prostředků, které vlastní.</span><span class="sxs-lookup"><span data-stu-id="05cbd-136">Data-asset owners can control the visibility of the data assets they own.</span></span> <span data-ttu-id="05cbd-137">Chcete-li omezit viditelnost jako výchozí, kde všichni uživatelé katalogu Data Catalog můžete vyhledat a zobrazit datový prostředek, můžete vlastník majetku přepnutí viditelnost nastavení z **Everyone** k **vlastníci a tito uživatelé** v vlastnosti pro asset.</span><span class="sxs-lookup"><span data-stu-id="05cbd-137">To restrict visibility as the default, where all Data Catalog users can discover and view the data asset, the asset owner can toggle the visibility setting from **Everyone** to **Owners & These Users** in the properties for the asset.</span></span> <span data-ttu-id="05cbd-138">Vlastníci poté můžete přidat konkrétní uživatele a skupiny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="05cbd-138">Owners can then add specific users and security groups.</span></span>

> [!NOTE]
> <span data-ttu-id="05cbd-139">Kdykoli je to možné, měli oprávnění vlastnictví a viditelnost asset přiřadit do skupin zabezpečení a ne pro jednotlivé uživatele.</span><span class="sxs-lookup"><span data-stu-id="05cbd-139">Whenever possible, asset ownership and visibility permissions should be assigned to security groups and not to individual users.</span></span>
>
>

## <a name="catalog-administrators"></a><span data-ttu-id="05cbd-140">Správci katalogu</span><span class="sxs-lookup"><span data-stu-id="05cbd-140">Catalog administrators</span></span>
<span data-ttu-id="05cbd-141">Správci katalogu dat se implicitně společné vlastníci všechny prostředky v katalogu.</span><span class="sxs-lookup"><span data-stu-id="05cbd-141">Data Catalog administrators are implicitly co-owners of all assets in the catalog.</span></span> <span data-ttu-id="05cbd-142">Vlastníci Asset nelze odebrat viditelnost od správců, a můžou správci spravovat vlastnictví a viditelnost pro všechny datové prostředky v katalogu.</span><span class="sxs-lookup"><span data-stu-id="05cbd-142">Asset owners cannot remove visibility from administrators, and administrators can manage ownership and visibility for all data assets in the catalog.</span></span>

## <a name="summary"></a><span data-ttu-id="05cbd-143">Souhrn</span><span class="sxs-lookup"><span data-stu-id="05cbd-143">Summary</span></span>
<span data-ttu-id="05cbd-144">Data Catalog crowdsourcingový model metadata a data zjišťování asset umožňuje všem uživatelům katalogu přispívat a zjišťovat.</span><span class="sxs-lookup"><span data-stu-id="05cbd-144">The Data Catalog crowdsourcing model to metadata and data asset discovery allows all catalog users to contribute and discover.</span></span> <span data-ttu-id="05cbd-145">Standard Edition Data Catalog je určen pro vlastnictví a správu omezit viditelnost a použití konkrétní datové prostředky.</span><span class="sxs-lookup"><span data-stu-id="05cbd-145">The Standard Edition of Data Catalog is designed for ownership and management to limit the visibility and use of specific data assets.</span></span>
