---
title: "aaaHow toosecure přístup tooAzure katalogu Data Catalog | Microsoft Docs"
description: "Tento článek vysvětlují, jak toosecure katalog data catalog a jeho datové prostředky."
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
ms.openlocfilehash: d7c35fd57d8add1cdc152b75f111879288e1548f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosecure-access-toodata-catalog-and-data-assets"></a><span data-ttu-id="500ac-103">Jak toosecure přistupovat k toodata katalogu a data prostředků</span><span class="sxs-lookup"><span data-stu-id="500ac-103">How toosecure access toodata catalog and data assets</span></span>
> [!IMPORTANT]
> <span data-ttu-id="500ac-104">Tato funkce je k dispozici pouze v edici standard hello systému Azure Data Catalog.</span><span class="sxs-lookup"><span data-stu-id="500ac-104">This feature is available only in hello standard edition of Azure Data Catalog.</span></span>

<span data-ttu-id="500ac-105">Azure Data Catalog umožňuje toospecify, kdo má přístup k katalogu dat hello a jaké operace (zaregistrovat, opatřit poznámkami, převzít vlastnictví) můžete provádět na metadata v katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="500ac-105">Azure Data Catalog allows you toospecify who can access hello data catalog and what operations (register, annotate, take ownership) they can perform on metadata in hello catalog.</span></span> 

## <a name="catalog-users-and-permissions"></a><span data-ttu-id="500ac-106">Uživatelé katalogu a oprávnění</span><span class="sxs-lookup"><span data-stu-id="500ac-106">Catalog users and permissions</span></span>
<span data-ttu-id="500ac-107">toogive a uživatele nebo skupiny hello přistupovat ke katalogu dat tooa a nastavte oprávnění:</span><span class="sxs-lookup"><span data-stu-id="500ac-107">toogive a user or a group hello access tooa data catalog and set permissions:</span></span>

1. <span data-ttu-id="500ac-108">Na hello [domovskou stránku katalogu dat](http://www.azuredatacatalog.com), klikněte na tlačítko **nastavení** na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="500ac-108">On hello [home page of your data catalog](http://www.azuredatacatalog.com),  click **Settings** on hello toolbar.</span></span>

    ![data catalog – nastavení](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. <span data-ttu-id="500ac-110">Na stránce nastavení hello rozbalte hello **uživatelé katalogu** části.</span><span class="sxs-lookup"><span data-stu-id="500ac-110">In hello settings page, expand hello **Catalog Users** section.</span></span>
    <span data-ttu-id="500ac-111">![Katalogu uživatelé – přidat](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)</span><span class="sxs-lookup"><span data-stu-id="500ac-111">![Catalog Users - Add](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)</span></span>
3. <span data-ttu-id="500ac-112">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="500ac-112">Click **Add**.</span></span>
4. <span data-ttu-id="500ac-113">Zadejte plně kvalifikovaný hello **uživatelské jméno** nebo název hello **skupiny zabezpečení** v Azure Active Directory (AAD) přidružené k hello katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="500ac-113">Enter hello fully qualified **user name** or name of hello **security group** in hello Azure Active Directory (AAD) associated with hello catalog.</span></span> <span data-ttu-id="500ac-114">Čárkou (', ') jako oddělovač, chcete-li přidat více než jeden uživatel nebo skupina.</span><span class="sxs-lookup"><span data-stu-id="500ac-114">Use comma (\`,’) as a separator if you are adding more than one user or group.</span></span>
    <span data-ttu-id="500ac-115">![Uživatelé katalogu - uživatele nebo skupiny](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)</span><span class="sxs-lookup"><span data-stu-id="500ac-115">![Catalog Users - users or groups](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)</span></span>
5. <span data-ttu-id="500ac-116">Stiskněte klávesu **ENTER** nebo **KARTĚ** mimo hello textové pole.</span><span class="sxs-lookup"><span data-stu-id="500ac-116">Press **ENTER** or **TAB** out of hello text box.</span></span> 
6.  <span data-ttu-id="500ac-117">Potvrďte, že všechna oprávnění (**Poznámka**, **zaregistrovat**, a **převzít vlastnictví**) jsou přiřazeny toothese uživatele nebo skupiny ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="500ac-117">Confirm that all permissions (**Annotate**, **Register**, and **Take Ownership**) are assigned toothese users or groups by default.</span></span> <span data-ttu-id="500ac-118">To znamená, hello uživatel nebo skupina může [zaregistrovat datových prostředků]( data-catalog-how-to-register.md), [opatřit poznámkami datové prostředky]( data-catalog-how-to-annotate.md), a [převzetí vlastnictví datových prostředků]( data-catalog-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="500ac-118">That is, hello user or group can [register data assets]( data-catalog-how-to-register.md), [annotate data assets]( data-catalog-how-to-annotate.md), and [take ownership of data assets]( data-catalog-how-to-manage.md).</span></span> 
    <span data-ttu-id="500ac-119">![Uživatelé katalogu – výchozí oprávnění](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)</span><span class="sxs-lookup"><span data-stu-id="500ac-119">![Catalog Users - default permissions](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)</span></span>
7.  <span data-ttu-id="500ac-120">toogive uživatele nebo skupinu pouze ke čtení hello, přístup toohello katalogu, zrušte zaškrtnutí hello **opatřit poznámkami** možnost pro tohoto uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="500ac-120">toogive a user or group only hello read access toohello catalog, clear hello **annotate** option for that user or group.</span></span> <span data-ttu-id="500ac-121">Pokud tak učiníte, hello uživatele nebo skupinu nelze opatřit poznámkami datové prostředky v katalogu hello, ale je mohou zobrazit.</span><span class="sxs-lookup"><span data-stu-id="500ac-121">When you do so, hello user or group cannot annotate data assets in hello catalog but they can view them.</span></span> 
8.  <span data-ttu-id="500ac-122">toodeny uživatele nebo skupinu registraci datových prostředků, zrušte hello **zaregistrovat** možnost pro tohoto uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="500ac-122">toodeny a user or group from registering data assets, clear hello **register** option for that user or group.</span></span>
9.  <span data-ttu-id="500ac-123">toodeny uživatele z převzetí vlastnictví datový prostředek, zrušte hello **převzít vlastnictví** možnost pro tohoto uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="500ac-123">toodeny a user from taking ownership of a data asset, clear hello **take ownership** option for that user or group.</span></span> 
10. <span data-ttu-id="500ac-124">Klikněte na tlačítko toodelete uživateli nebo skupině od uživatelů katalogu **x** pro hello uživatele nebo skupiny v hello dolní části seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="500ac-124">toodelete a user/group from catalog users, click **x** for hello user/group at hello bottom of hello list.</span></span> 
    <span data-ttu-id="500ac-125">![Uživatelé katalogu – odstranit uživatele](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)</span><span class="sxs-lookup"><span data-stu-id="500ac-125">![Catalog Users - delete user](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="500ac-126">Doporučujeme přidat přímo uživatelů toocatalog zabezpečení skupiny místo přidání uživatelů a přiřaďte oprávnění.</span><span class="sxs-lookup"><span data-stu-id="500ac-126">We recommend that you add security groups toocatalog users rather than adding users directly and assign permissions.</span></span> <span data-ttu-id="500ac-127">Pak přidejte skupiny zabezpečení toohello uživatelé, které odpovídají jejich rolí a jejich katalogu toohello požadovaný přístup.</span><span class="sxs-lookup"><span data-stu-id="500ac-127">Then, add users toohello security groups that match their roles and their required access toohello catalog.</span></span>

## <a name="special-considerations"></a><span data-ttu-id="500ac-128">Zvláštní upozornění</span><span class="sxs-lookup"><span data-stu-id="500ac-128">Special considerations</span></span>

- <span data-ttu-id="500ac-129">oprávnění Hello přiřazeny toosecurity skupiny jsou aditivní.</span><span class="sxs-lookup"><span data-stu-id="500ac-129">hello permissions assigned toosecurity groups are additive.</span></span> <span data-ttu-id="500ac-130">Vyslovte uživatel je ve dvou skupinách.</span><span class="sxs-lookup"><span data-stu-id="500ac-130">Say, a user is in two groups.</span></span> <span data-ttu-id="500ac-131">Jedna skupina má opatřit poznámkami oprávnění a jiné skupiny není mají opatřit poznámkami oprávnění.</span><span class="sxs-lookup"><span data-stu-id="500ac-131">One group has annotate permissions and other group does not have annotate permissions.</span></span> <span data-ttu-id="500ac-132">Potom uživatel má opatřit poznámkami oprávnění.</span><span class="sxs-lookup"><span data-stu-id="500ac-132">Then, user has annotate permissions.</span></span> 
- <span data-ttu-id="500ac-133">Hello oprávnění explicitně přiřazovat tooa uživatele přepsání hello oprávnění přiřazená toogroups toowhich hello uživatel patří.</span><span class="sxs-lookup"><span data-stu-id="500ac-133">hello permissions assigned explicitly tooa user override hello permissions assigned toogroups toowhich hello user belongs.</span></span> <span data-ttu-id="500ac-134">V předchozím příkladu hello Řekněme, explicitně přidali hello uživatele toocatalog uživatele a nepřiřazujte opatřit poznámkami oprávnění.</span><span class="sxs-lookup"><span data-stu-id="500ac-134">In hello previous example, say, you explicitly added hello user toocatalog users and do not assign annotate permissions.</span></span> <span data-ttu-id="500ac-135">Hello uživatele nelze opatřit poznámkami datové prostředky, i když hello uživatel je členem skupiny, která nemá opatřit poznámkami oprávnění.</span><span class="sxs-lookup"><span data-stu-id="500ac-135">hello user cannot annotate data assets even though hello user is a member of a group that does have annotate permissions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="500ac-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="500ac-136">Next steps</span></span>
- [<span data-ttu-id="500ac-137">Začínáme s Azure Data Catalogem</span><span class="sxs-lookup"><span data-stu-id="500ac-137">Get started with Azure Data Catalog</span></span>](data-catalog-get-started.md)

