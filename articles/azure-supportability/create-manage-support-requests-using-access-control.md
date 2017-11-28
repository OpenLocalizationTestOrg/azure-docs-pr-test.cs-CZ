---
title: "Řízení přístupu na základě Role (RBAC) toocontrol aaaAzure přístup toocreate práva a spravovat žádosti o podporu | Microsoft Docs"
description: "Azure toocontrol řízení přístupu na základě rolí (RBAC) přístup k toocreate práva a správa žádostí o podporu"
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: c68a699ac870fa6bf371deb8ed0424848f39acf0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-role-based-access-control-rbac-toocontrol-access-rights-toocreate-and-manage-support-requests"></a><span data-ttu-id="443b1-103">Azure toocontrol řízení přístupu na základě rolí (RBAC) přístup k toocreate práva a správa žádostí o podporu</span><span class="sxs-lookup"><span data-stu-id="443b1-103">Azure Role-Based Access Control (RBAC) toocontrol access rights toocreate and manage support requests</span></span>

<span data-ttu-id="443b1-104">[Řízení přístupu na základě rolí (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) umožňuje vyladění správy přístupu pro Azure.</span><span class="sxs-lookup"><span data-stu-id="443b1-104">[Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) enables fine-grained access management for Azure.</span></span>
<span data-ttu-id="443b1-105">Při vytváření žádosti o podporu v hello portál Azure, [portal.azure.com](https://portal.azure.com), používá toodefine Azure RBAC modelu, který můžete vytvořit a spravovat žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="443b1-105">Support request creation in hello Azure portal, [portal.azure.com](https://portal.azure.com), uses Azure’s RBAC model toodefine who can create and manage support requests.</span></span>
<span data-ttu-id="443b1-106">Přístup uděluje přiřazením toousers hello odpovídající RBAC role, skupiny a aplikace v určité obor, což může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="443b1-106">Access is granted by assigning hello appropriate RBAC role toousers, groups, and applications at a certain scope, which can be a subscription, resource group or a resource.</span></span>

<span data-ttu-id="443b1-107">Podívejme příklad: jako vlastník skupiny prostředků s oprávněními ke čtení v oboru hello předplatné, můžete spravovat všechny prostředky hello pod hello skupinu prostředků, jako jsou weby, virtuální počítače a podsítě.</span><span class="sxs-lookup"><span data-stu-id="443b1-107">Let’s take an example: As a resource group owner with read permissions at hello subscription scope, you can manage all hello resources under hello resource group, like websites, virtual machines, and subnets.</span></span>
<span data-ttu-id="443b1-108">Ale když zkusíte toocreate žádost o podporu pro prostředek virtuálního počítače hello, narazíte hello následující chyba</span><span class="sxs-lookup"><span data-stu-id="443b1-108">However, when you try toocreate a support request against hello virtual machine resource, you encounter hello following error</span></span>

![Chyba odběru](./media/create-manage-support-requests-using-access-control/subscription-error.png)

<span data-ttu-id="443b1-110">Ve správě žádosti o podporu potřebujete oprávnění zapisovat nebo role, kterou má hello podporují akci Microsoft.Support/* v hello předplatné oboru toobe možné toocreate a spravovat žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="443b1-110">In support request management, you need write permission or a role that has hello Support action Microsoft.Support/* at hello Subscription scope toobe able toocreate and manage support requests.</span></span>

<span data-ttu-id="443b1-111">Hello následující článek vysvětluje, jak můžete použít vlastní toocreate řízení přístupu na základě Role (RBAC) Azure a spravovat žádosti o podporu v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="443b1-111">hello following article explains how you can use Azure’s custom Role-Based Access Control (RBAC) toocreate and manage support requests in hello Azure portal.</span></span>

## <a name="getting-started"></a><span data-ttu-id="443b1-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="443b1-112">Getting Started</span></span>

<span data-ttu-id="443b1-113">Pomocí hello příkladu výše, by byl schopný toocreate žádosti o podporu pro prostředek Pokud vlastní role RBAC v předplatném hello se přiřadila hello vlastník předplatného.</span><span class="sxs-lookup"><span data-stu-id="443b1-113">Using hello example above, you would be able toocreate a support request for your resource if you were assigned a custom RBAC role on hello subscription by hello subscription owner.</span></span>
<span data-ttu-id="443b1-114">[Vlastní role RBAC](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) lze vytvořit pomocí Azure PowerShell, rozhraní příkazového řádku Azure (CLI) a hello REST API.</span><span class="sxs-lookup"><span data-stu-id="443b1-114">[Custom RBAC roles](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) can be created using Azure PowerShell, Azure Command-Line Interface (CLI), and hello REST API.</span></span>

<span data-ttu-id="443b1-115">Vlastnost akce Hello vlastní role určuje, zda text hello Azure operations toowhich hello role uděluje přístup.</span><span class="sxs-lookup"><span data-stu-id="443b1-115">hello actions property of a custom role specifies hello Azure operations toowhich hello role grants access.</span></span>
<span data-ttu-id="443b1-116">toocreate vlastní role pro správu žádosti o podporu, musí mít role, hello hello akce Microsoft.Support/*</span><span class="sxs-lookup"><span data-stu-id="443b1-116">toocreate a custom role for support request management, hello role must have hello action Microsoft.Support/*</span></span>

<span data-ttu-id="443b1-117">Tady je příklad vlastní roli, můžete použít toocreate a spravovat žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="443b1-117">Here’s an example of a custom role you can use toocreate and manage support requests.</span></span>
<span data-ttu-id="443b1-118">Jsme jste s názvem "Přispěvatel žádosti o podporu" této role, kterým je, jak označujeme toohello vlastní role v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="443b1-118">We’ve named this role “Support Request Contributor” and that’s how we refer toohello custom role in this article.</span></span>

``` Json
{
    "Name":  "Support Request Contributor",
    "Id":  "1f2aad59-39b0-41da-b052-2fb070bd7942",
    "IsCustom":  true,
    "Description":  "Lets you create and manage support tickets.",
    "Actions":  [
                    "Microsoft.Support/*"
                ],
    "NotActions":  [
                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

<span data-ttu-id="443b1-119">Postupujte podle kroků hello uvedených v [toto video](https://www.youtube.com/watch?v=-PaBaDmfwKI) toolearn jak toocreate vlastní role pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="443b1-119">Follow hello steps outlined in [this video](https://www.youtube.com/watch?v=-PaBaDmfwKI) toolearn how toocreate a custom role for your subscription.</span></span>

## <a name="create-and-manage-support-requests-in-hello-azure-portal"></a><span data-ttu-id="443b1-120">Vytvoření a správa žádostí o podporu v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="443b1-120">Create and manage support requests in hello Azure portal</span></span>

<span data-ttu-id="443b1-121">Podívejme příklad – jsou hello vlastníka předplatného "Visual Studio předplatné MSDN."</span><span class="sxs-lookup"><span data-stu-id="443b1-121">Let’s take an example – you are hello owner of Subscription "Visual Studio MSDN Subscription."</span></span>
<span data-ttu-id="443b1-122">Jan je vaše partnera, který je toosome vlastníka prostředku hello skupin prostředků v tomto předplatném a má předplatné toohello oprávnění ke čtení.</span><span class="sxs-lookup"><span data-stu-id="443b1-122">Joe is your peer who is a resource owner toosome of hello resource groups in this subscription and has read permission toohello subscription.</span></span>
<span data-ttu-id="443b1-123">Chcete toogive přístup tooyour rovnocenný, Jan, hello možnost toocreate a spravovat lístky žádostí o podporu pro hello prostředky pod tímto předplatným.</span><span class="sxs-lookup"><span data-stu-id="443b1-123">You wish toogive access tooyour peer, Joe, hello ability toocreate and manage support tickets for hello resources under this subscription.</span></span>

1. <span data-ttu-id="443b1-124">prvním krokem Hello je toogo toohello předplatného a v části "Nastavení" zobrazí seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="443b1-124">hello first step is toogo toohello subscription and under "Settings" you see a list of users.</span></span> <span data-ttu-id="443b1-125">Klikněte na uživatele hello Jan, kdo má přístup čtečky na hello předplatného a umožňuje přiřadit nové toohim vlastní role.</span><span class="sxs-lookup"><span data-stu-id="443b1-125">Click hello user Joe who has reader access on hello Subscription and let’s assign a new custom role toohim.</span></span>

    ![Přidání role](./media/create-manage-support-requests-using-access-control/add-role.png)

2. <span data-ttu-id="443b1-127">V okně "Uživatelé" hello, klikněte na tlačítko "Přidat".</span><span class="sxs-lookup"><span data-stu-id="443b1-127">Click "Add" under hello "Users" blade.</span></span> <span data-ttu-id="443b1-128">Vyberte vlastní roli hello "Přispěvatel žádosti o podporu" hello seznamu rolí</span><span class="sxs-lookup"><span data-stu-id="443b1-128">Select hello custom role "Support Request Contributor" from hello list of roles</span></span>

    ![Přidání role Přispěvatel podpory](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. <span data-ttu-id="443b1-130">Po výběru hello název role, klikněte na tlačítko "Přidat uživatele" a zadejte přihlašovací údaje hello Jan e-mailu.</span><span class="sxs-lookup"><span data-stu-id="443b1-130">After selecting hello role name, click "Add users" and enter hello Joe's email credentials.</span></span> <span data-ttu-id="443b1-131">Klikněte na tlačítko "Vyberte"</span><span class="sxs-lookup"><span data-stu-id="443b1-131">Click "Select"</span></span>

    ![Přidání uživatelů](./media/create-manage-support-requests-using-access-control/add-users.png)

4. <span data-ttu-id="443b1-133">Klikněte na tlačítko "Ok" tooproceed</span><span class="sxs-lookup"><span data-stu-id="443b1-133">Click "Ok" tooproceed</span></span>

    ![Přidat přístup](./media/create-manage-support-requests-using-access-control/add-access.png)

5. <span data-ttu-id="443b1-135">Nyní se zobrazí hello uživatele s hello nově přidané vlastní role "Podporu požadavků Přispěvatel" v rámci předplatného hello, pro který jste vlastník hello</span><span class="sxs-lookup"><span data-stu-id="443b1-135">Now you see hello user with hello newly added custom role "Support Request Contributor" under hello Subscription for which you are hello owner</span></span>

    ![Přidat uživatele](./media/create-manage-support-requests-using-access-control/user-added.png)

    <span data-ttu-id="443b1-137">Jan přihlásí hello portál, zjistí toowhich hello předplatné, které mu byla přidána.</span><span class="sxs-lookup"><span data-stu-id="443b1-137">When Joe logs in hello portal, he sees hello subscription toowhich he was added.</span></span>

7. <span data-ttu-id="443b1-138">Jan klikne na možnost "Nová žádost o podporu" v okně "Nápověda a podpora" hello a můžete vytvořit žádosti o podporu pro "Visual Studio Ultimate s MSDN"</span><span class="sxs-lookup"><span data-stu-id="443b1-138">Joe clicks "New Support request" from hello "Help and Support" blade and can create support requests for "Visual Studio Ultimate with MSDN"</span></span>

    ![Nová žádost o podporu](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. <span data-ttu-id="443b1-140">Kliknutím na tlačítko "Podporují všechny požadavky" Jan uvidí hello seznam žádostí o podporu pro toto předplatné vytvořit ![případ zobrazení podrobností](./media/create-manage-support-requests-using-access-control/case-details-view.png)</span><span class="sxs-lookup"><span data-stu-id="443b1-140">Clicking "All support requests" Joe can see hello list of support requests created for this Subscription  ![Case details view](./media/create-manage-support-requests-using-access-control/case-details-view.png)</span></span>

## <a name="remove-support-request-access-in-hello-azure-portal"></a><span data-ttu-id="443b1-141">Odebrat podporu požádat o přístup v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="443b1-141">Remove support request access in hello Azure portal</span></span>

<span data-ttu-id="443b1-142">Stejně, jako je možné toogrant přístup tooa uživatele toocreate a spravovat žádosti o podporu, je možné tooremove přístupu pro uživatele hello také.</span><span class="sxs-lookup"><span data-stu-id="443b1-142">Just as it is possible toogrant access tooa user toocreate and manage support requests, it's possible tooremove access for hello user as well.</span></span>
<span data-ttu-id="443b1-143">tooremove hello možnost toocreate a spravovat žádosti o podporu, přejděte toohello předplatné, klikněte na tlačítko "Nastavení" a klikněte na hello uživateli (v tomto případě Jan).</span><span class="sxs-lookup"><span data-stu-id="443b1-143">tooremove hello ability toocreate and manage support requests, go toohello Subscription, click "Settings" and click hello user (in this case, Joe).</span></span>
<span data-ttu-id="443b1-144">Klikněte pravým tlačítkem na název role hello, "Přispěvatel žádosti o podporu" a klikněte na tlačítko "Odebrat"</span><span class="sxs-lookup"><span data-stu-id="443b1-144">Right-click hello role name, "Support Request Contributor" and click "Remove"</span></span>

![Odebrat přístup žádosti o podporu](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

<span data-ttu-id="443b1-146">Když Jan protokoly portálu toohello a pokusí toocreate žádosti o podporu, mohl zaznamená hello následující chyba</span><span class="sxs-lookup"><span data-stu-id="443b1-146">When Joe logs in toohello portal and tries toocreate a support request, he encounters hello following error</span></span>

![Předplatné chyba-2](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

<span data-ttu-id="443b1-148">Jan nelze zobrazit žádné podporovat požadavky, pokud klikne "Podporují všechny požadavky"</span><span class="sxs-lookup"><span data-stu-id="443b1-148">Joe cannot see any support requests when he clicks "All support requests"</span></span>

![zobrazení podrobností případu-2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
