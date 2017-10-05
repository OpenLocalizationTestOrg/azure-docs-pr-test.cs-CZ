---
title: "Jak zabezpečit přístup k Azure Data Catalog | Microsoft Docs"
description: "Tento článek vysvětlují, jak zabezpečit katalog data catalog a jeho datové prostředky."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/17/2017
ms.author: maroche
ms.openlocfilehash: 9664a7bc8493b08c8e0797ac6f1b212079829833
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-secure-access-to-data-catalog-and-data-assets"></a><span data-ttu-id="0d206-103">Jak zabezpečit přístup do katalogu data catalog a datových prostředků</span><span class="sxs-lookup"><span data-stu-id="0d206-103">How to secure access to data catalog and data assets</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0d206-104">Tato funkce je k dispozici pouze v edici standard systému Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="0d206-104">This feature is available only in the standard edition of Azure Data Catalog.</span></span>

<span data-ttu-id="0d206-105">Azure Data Catalog umožňuje určit, kdo má přístup k katalogu dat a jaké operace (zaregistrovat, opatřit poznámkami, převzít vlastnictví) můžete provádět na metadata v katalogu.</span><span class="sxs-lookup"><span data-stu-id="0d206-105">Azure Data Catalog allows you to specify who can access the data catalog and what operations (register, annotate, take ownership) they can perform on metadata in the catalog.</span></span> 

## <a name="catalog-users-and-permissions"></a><span data-ttu-id="0d206-106">Uživatelé katalogu a oprávnění</span><span class="sxs-lookup"><span data-stu-id="0d206-106">Catalog users and permissions</span></span>
<span data-ttu-id="0d206-107">Zadejte uživatele nebo skupinu přístup do katalogu data catalog a nastavit oprávnění:</span><span class="sxs-lookup"><span data-stu-id="0d206-107">To give a user or a group the access to a data catalog and set permissions:</span></span>

1. <span data-ttu-id="0d206-108">Na [domovskou stránku katalogu dat](http://www.azuredatacatalog.com), klikněte na tlačítko **nastavení** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="0d206-108">On the [home page of your data catalog](http://www.azuredatacatalog.com),  click **Settings** on the toolbar.</span></span>

    ![data catalog – nastavení](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. <span data-ttu-id="0d206-110">Na stránce nastavení, rozbalte **uživatelé katalogu** části.</span><span class="sxs-lookup"><span data-stu-id="0d206-110">In the settings page, expand the **Catalog Users** section.</span></span>
    <span data-ttu-id="0d206-111">![Katalogu uživatelé – přidat](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)</span><span class="sxs-lookup"><span data-stu-id="0d206-111">![Catalog Users - Add](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)</span></span>
3. <span data-ttu-id="0d206-112">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="0d206-112">Click **Add**.</span></span>
4. <span data-ttu-id="0d206-113">Zadejte plně kvalifikovaný **uživatelské jméno** nebo název **skupiny zabezpečení** v Azure Active Directory (AAD) přidružené k katalogu.</span><span class="sxs-lookup"><span data-stu-id="0d206-113">Enter the fully qualified **user name** or name of the **security group** in the Azure Active Directory (AAD) associated with the catalog.</span></span> <span data-ttu-id="0d206-114">Čárkou (', ') jako oddělovač, chcete-li přidat více než jeden uživatel nebo skupina.</span><span class="sxs-lookup"><span data-stu-id="0d206-114">Use comma (\`,’) as a separator if you are adding more than one user or group.</span></span>
    <span data-ttu-id="0d206-115">![Uživatelé katalogu - uživatele nebo skupiny](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)</span><span class="sxs-lookup"><span data-stu-id="0d206-115">![Catalog Users - users or groups](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)</span></span>
5. <span data-ttu-id="0d206-116">Stiskněte klávesu **ENTER** nebo **KARTĚ** mimo textové pole.</span><span class="sxs-lookup"><span data-stu-id="0d206-116">Press **ENTER** or **TAB** out of the text box.</span></span> 
6.  <span data-ttu-id="0d206-117">Potvrďte, že všechna oprávnění (**Poznámka**, **zaregistrovat**, a **převzít vlastnictví**) jsou přiřazeny pro tyto uživatele nebo skupiny ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="0d206-117">Confirm that all permissions (**Annotate**, **Register**, and **Take Ownership**) are assigned to these users or groups by default.</span></span> <span data-ttu-id="0d206-118">To znamená, může uživatel nebo skupina [zaregistrovat datových prostředků]( data-catalog-how-to-register.md), [opatřit poznámkami datové prostředky]( data-catalog-how-to-annotate.md), a [převzetí vlastnictví datových prostředků]( data-catalog-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="0d206-118">That is, the user or group can [register data assets]( data-catalog-how-to-register.md), [annotate data assets]( data-catalog-how-to-annotate.md), and [take ownership of data assets]( data-catalog-how-to-manage.md).</span></span> 
    <span data-ttu-id="0d206-119">![Uživatelé katalogu – výchozí oprávnění](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)</span><span class="sxs-lookup"><span data-stu-id="0d206-119">![Catalog Users - default permissions](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)</span></span>
7.  <span data-ttu-id="0d206-120">Chcete-li poskytnout uživatele nebo skupinu pouze přístup pro čtení do katalogu, zrušte **opatřit poznámkami** možnost pro tohoto uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="0d206-120">To give a user or group only the read access to the catalog, clear the **annotate** option for that user or group.</span></span> <span data-ttu-id="0d206-121">Pokud tak učiníte, uživatele nebo skupinu nelze opatřit poznámkami datové prostředky v katalogu, ale je mohou zobrazit.</span><span class="sxs-lookup"><span data-stu-id="0d206-121">When you do so, the user or group cannot annotate data assets in the catalog but they can view them.</span></span> 
8.  <span data-ttu-id="0d206-122">Chcete-li odepřít uživatele nebo skupinu ze registrace datových prostředků, zrušte **zaregistrovat** možnost pro tohoto uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="0d206-122">To deny a user or group from registering data assets, clear the **register** option for that user or group.</span></span>
9.  <span data-ttu-id="0d206-123">Chcete-li odepřít uživatele přebírat vlastnictví datový prostředek, zrušte **převzít vlastnictví** možnost pro tohoto uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="0d206-123">To deny a user from taking ownership of a data asset, clear the **take ownership** option for that user or group.</span></span> 
10. <span data-ttu-id="0d206-124">Pokud chcete odstranit uživatele nebo skupiny z uživatelé katalogu, klikněte na tlačítko **x** pro uživatele nebo skupiny v dolní části seznamu.</span><span class="sxs-lookup"><span data-stu-id="0d206-124">To delete a user/group from catalog users, click **x** for the user/group at the bottom of the list.</span></span> 
    <span data-ttu-id="0d206-125">![Uživatelé katalogu – odstranit uživatele](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)</span><span class="sxs-lookup"><span data-stu-id="0d206-125">![Catalog Users - delete user](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="0d206-126">Doporučujeme přidat skupiny zabezpečení přímo katalogu uživatelů místo přidání uživatelů a přiřaďte oprávnění.</span><span class="sxs-lookup"><span data-stu-id="0d206-126">We recommend that you add security groups to catalog users rather than adding users directly and assign permissions.</span></span> <span data-ttu-id="0d206-127">Potom přidání uživatelů do skupiny zabezpečení, které odpovídají jejich rolí a jejich požadovaný přístup do katalogu.</span><span class="sxs-lookup"><span data-stu-id="0d206-127">Then, add users to the security groups that match their roles and their required access to the catalog.</span></span>

## <a name="special-considerations"></a><span data-ttu-id="0d206-128">Zvláštní upozornění</span><span class="sxs-lookup"><span data-stu-id="0d206-128">Special considerations</span></span>

- <span data-ttu-id="0d206-129">Oprávnění přiřazená uživateli skupiny zabezpečení jsou aditivní.</span><span class="sxs-lookup"><span data-stu-id="0d206-129">The permissions assigned to security groups are additive.</span></span> <span data-ttu-id="0d206-130">Vyslovte uživatel je ve dvou skupinách.</span><span class="sxs-lookup"><span data-stu-id="0d206-130">Say, a user is in two groups.</span></span> <span data-ttu-id="0d206-131">Jedna skupina má opatřit poznámkami oprávnění a jiné skupiny není mají opatřit poznámkami oprávnění.</span><span class="sxs-lookup"><span data-stu-id="0d206-131">One group has annotate permissions and other group does not have annotate permissions.</span></span> <span data-ttu-id="0d206-132">Potom uživatel má opatřit poznámkami oprávnění.</span><span class="sxs-lookup"><span data-stu-id="0d206-132">Then, user has annotate permissions.</span></span> 
- <span data-ttu-id="0d206-133">Oprávnění přiřazená uživateli explicitně přepsat oprávnění přiřazená do skupin, do kterých uživatel patří.</span><span class="sxs-lookup"><span data-stu-id="0d206-133">The permissions assigned explicitly to a user override the permissions assigned to groups to which the user belongs.</span></span> <span data-ttu-id="0d206-134">V předchozím příkladu Řekněme, můžete explicitně přidat uživatele katalogu uživatelů a provádět není přiřazení opatřit poznámkami oprávnění.</span><span class="sxs-lookup"><span data-stu-id="0d206-134">In the previous example, say, you explicitly added the user to catalog users and do not assign annotate permissions.</span></span> <span data-ttu-id="0d206-135">Uživatele nelze opatřit poznámkami datové prostředky, i když je uživatel členem skupiny, která nemá opatřit poznámkami oprávnění.</span><span class="sxs-lookup"><span data-stu-id="0d206-135">The user cannot annotate data assets even though the user is a member of a group that does have annotate permissions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d206-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d206-136">Next steps</span></span>
- [<span data-ttu-id="0d206-137">Začínáme s Azure Data Catalogem</span><span class="sxs-lookup"><span data-stu-id="0d206-137">Get started with Azure Data Catalog</span></span>](data-catalog-get-started.md)

