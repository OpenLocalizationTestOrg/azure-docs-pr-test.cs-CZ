---
title: "Azure na základě rolí řízení přístupu (RBAC) do ovládacího prvku přístupová práva k vytváření a správě žádosti o podporu | Microsoft Docs"
description: "Azure na základě rolí řízení přístupu (RBAC) do ovládacího prvku přístupová práva k vytváření a správě žádosti o podporu"
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: 20ebd324cbf379980b43d255d468673de2b6d950
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-role-based-access-control-rbac-to-control-access-rights-to-create-and-manage-support-requests"></a><span data-ttu-id="036e1-103">Azure na základě rolí řízení přístupu (RBAC) do ovládacího prvku přístupová práva k vytváření a správě žádosti o podporu</span><span class="sxs-lookup"><span data-stu-id="036e1-103">Azure Role-Based Access Control (RBAC) to control access rights to create and manage support requests</span></span>

<span data-ttu-id="036e1-104">[Řízení přístupu na základě rolí (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) umožňuje vyladění správy přístupu pro Azure.</span><span class="sxs-lookup"><span data-stu-id="036e1-104">[Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) enables fine-grained access management for Azure.</span></span>
<span data-ttu-id="036e1-105">Při vytváření žádosti o podporu na portálu Azure [portal.azure.com](https://portal.azure.com), používá Azure RBAC model můžete definovat, kdo může vytvářet a spravovat žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="036e1-105">Support request creation in the Azure portal, [portal.azure.com](https://portal.azure.com), uses Azure’s RBAC model to define who can create and manage support requests.</span></span>
<span data-ttu-id="036e1-106">Přístup uděluje přiřazením příslušné role RBAC uživatelům, skupinám a aplikacím na určité obor, což může být předplatné, skupinu prostředků nebo prostředek.</span><span class="sxs-lookup"><span data-stu-id="036e1-106">Access is granted by assigning the appropriate RBAC role to users, groups, and applications at a certain scope, which can be a subscription, resource group or a resource.</span></span>

<span data-ttu-id="036e1-107">Podívejme příklad: jako vlastník skupiny prostředků s oprávněními ke čtení v oboru předplatné, můžete spravovat všechny prostředky ve skupině prostředků, jako jsou weby, virtuální počítače a podsítě.</span><span class="sxs-lookup"><span data-stu-id="036e1-107">Let’s take an example: As a resource group owner with read permissions at the subscription scope, you can manage all the resources under the resource group, like websites, virtual machines, and subnets.</span></span>
<span data-ttu-id="036e1-108">Ale když se pokusíte vytvořit žádost o podporu pro prostředek virtuálního počítače, dojde k následující chybě</span><span class="sxs-lookup"><span data-stu-id="036e1-108">However, when you try to create a support request against the virtual machine resource, you encounter the following error</span></span>

![Chyba odběru](./media/create-manage-support-requests-using-access-control/subscription-error.png)

<span data-ttu-id="036e1-110">Ve správě žádosti o podporu třeba zápisu oprávnění nebo role, kterou má akce podporu Microsoft.Support/* na obor předplatného, abyste mohli vytvářet a spravovat žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="036e1-110">In support request management, you need write permission or a role that has the Support action Microsoft.Support/* at the Subscription scope to be able to create and manage support requests.</span></span>

<span data-ttu-id="036e1-111">V následujícím článku vysvětluje, jak můžete vytvořit a spravovat žádosti o podporu na portálu Azure pomocí Azure vlastní na základě rolí řízení přístupu (RBAC).</span><span class="sxs-lookup"><span data-stu-id="036e1-111">The following article explains how you can use Azure’s custom Role-Based Access Control (RBAC) to create and manage support requests in the Azure portal.</span></span>

## <a name="getting-started"></a><span data-ttu-id="036e1-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="036e1-112">Getting Started</span></span>

<span data-ttu-id="036e1-113">Pomocí výše uvedeném příkladu, bude moci vytvořit žádost o podporu pro prostředek, pokud vlastní role RBAC u předplatného se přiřadila vlastník předplatného.</span><span class="sxs-lookup"><span data-stu-id="036e1-113">Using the example above, you would be able to create a support request for your resource if you were assigned a custom RBAC role on the subscription by the subscription owner.</span></span>
<span data-ttu-id="036e1-114">[Vlastní role RBAC](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) lze vytvořit pomocí Azure PowerShell, rozhraní příkazového řádku Azure (CLI) a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="036e1-114">[Custom RBAC roles](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) can be created using Azure PowerShell, Azure Command-Line Interface (CLI), and the REST API.</span></span>

<span data-ttu-id="036e1-115">Vlastnost akce vlastní role určuje činnosti Azure, ke kterým roli udělí přístup.</span><span class="sxs-lookup"><span data-stu-id="036e1-115">The actions property of a custom role specifies the Azure operations to which the role grants access.</span></span>
<span data-ttu-id="036e1-116">Pokud chcete vytvořit vlastní role pro správu žádosti o podporu, musí mít roli akce Microsoft.Support/*</span><span class="sxs-lookup"><span data-stu-id="036e1-116">To create a custom role for support request management, the role must have the action Microsoft.Support/*</span></span>

<span data-ttu-id="036e1-117">Tady je příklad vlastní roli, které můžete použít k vytvoření a správa žádostí o podporu.</span><span class="sxs-lookup"><span data-stu-id="036e1-117">Here’s an example of a custom role you can use to create and manage support requests.</span></span>
<span data-ttu-id="036e1-118">Jsme jste s názvem "Přispěvatel žádosti o podporu" této role, kterým je, jak jsme odkazují na vlastní role v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="036e1-118">We’ve named this role “Support Request Contributor” and that’s how we refer to the custom role in this article.</span></span>

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

<span data-ttu-id="036e1-119">Postupujte podle kroků uvedených v [toto video](https://www.youtube.com/watch?v=-PaBaDmfwKI) se dozvíte, jak vytvářet vlastní role pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="036e1-119">Follow the steps outlined in [this video](https://www.youtube.com/watch?v=-PaBaDmfwKI) to learn how to create a custom role for your subscription.</span></span>

## <a name="create-and-manage-support-requests-in-the-azure-portal"></a><span data-ttu-id="036e1-120">Vytvoření a správa žádostí o podporu na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="036e1-120">Create and manage support requests in the Azure portal</span></span>

<span data-ttu-id="036e1-121">Podívejme příklad – jste vlastníkem předplatného "Visual Studio předplatné MSDN."</span><span class="sxs-lookup"><span data-stu-id="036e1-121">Let’s take an example – you are the owner of Subscription "Visual Studio MSDN Subscription."</span></span>
<span data-ttu-id="036e1-122">Jan je vaše sdílená, kdo je vlastníkem prostředku pro některé skupiny prostředků v tomto předplatném a má oprávnění ke čtení pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="036e1-122">Joe is your peer who is a resource owner to some of the resource groups in this subscription and has read permission to the subscription.</span></span>
<span data-ttu-id="036e1-123">Chcete poskytnout přístup k vaší sdílené, Jan, umožňuje vytvářet a spravovat lístky žádostí o podporu pro prostředky pod tímto předplatným.</span><span class="sxs-lookup"><span data-stu-id="036e1-123">You wish to give access to your peer, Joe, the ability to create and manage support tickets for the resources under this subscription.</span></span>

1. <span data-ttu-id="036e1-124">Prvním krokem je přejděte na předplatné a v části "Nastavení" zobrazí seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="036e1-124">The first step is to go to the subscription and under "Settings" you see a list of users.</span></span> <span data-ttu-id="036e1-125">Klikněte na uživatele Jan, kdo má přístup čtečky u předplatného a umožňuje přiřadit novou vlastní roli mu.</span><span class="sxs-lookup"><span data-stu-id="036e1-125">Click the user Joe who has reader access on the Subscription and let’s assign a new custom role to him.</span></span>

    ![Přidání role](./media/create-manage-support-requests-using-access-control/add-role.png)

2. <span data-ttu-id="036e1-127">V části "Uživatelé" okna klikněte na tlačítko "Přidat".</span><span class="sxs-lookup"><span data-stu-id="036e1-127">Click "Add" under the "Users" blade.</span></span> <span data-ttu-id="036e1-128">Vyberte vlastní roli "Přispěvatel žádosti o podporu" ze seznamu rolí</span><span class="sxs-lookup"><span data-stu-id="036e1-128">Select the custom role "Support Request Contributor" from the list of roles</span></span>

    ![Přidání role Přispěvatel podpory](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. <span data-ttu-id="036e1-130">Po výběru název role, klikněte na tlačítko "Přidat uživatele" a zadejte přihlašovací údaje Michalův e-mailu.</span><span class="sxs-lookup"><span data-stu-id="036e1-130">After selecting the role name, click "Add users" and enter the Joe's email credentials.</span></span> <span data-ttu-id="036e1-131">Klikněte na tlačítko "Vyberte"</span><span class="sxs-lookup"><span data-stu-id="036e1-131">Click "Select"</span></span>

    ![Přidání uživatelů](./media/create-manage-support-requests-using-access-control/add-users.png)

4. <span data-ttu-id="036e1-133">Klikněte na tlačítko "Ok", aby bylo možné pokračovat</span><span class="sxs-lookup"><span data-stu-id="036e1-133">Click "Ok" to proceed</span></span>

    ![Přidat přístup](./media/create-manage-support-requests-using-access-control/add-access.png)

5. <span data-ttu-id="036e1-135">Nyní se zobrazí uživatele s nově přidané vlastní role "Podporu požadavků Přispěvatel" v rámci předplatného, pro který jste vlastníkem</span><span class="sxs-lookup"><span data-stu-id="036e1-135">Now you see the user with the newly added custom role "Support Request Contributor" under the Subscription for which you are the owner</span></span>

    ![Přidat uživatele](./media/create-manage-support-requests-using-access-control/user-added.png)

    <span data-ttu-id="036e1-137">Jan přihlásí na portálu, zjistí předplatné, do které byl přidán.</span><span class="sxs-lookup"><span data-stu-id="036e1-137">When Joe logs in the portal, he sees the subscription to which he was added.</span></span>

7. <span data-ttu-id="036e1-138">Jan klikne na možnost "Nová žádost o podporu" v okně "Nápověda a podpora" a můžete vytvořit žádosti o podporu pro "Visual Studio Ultimate s MSDN"</span><span class="sxs-lookup"><span data-stu-id="036e1-138">Joe clicks "New Support request" from the "Help and Support" blade and can create support requests for "Visual Studio Ultimate with MSDN"</span></span>

    ![Nová žádost o podporu](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. <span data-ttu-id="036e1-140">Kliknutím na tlačítko "Podporují všechny požadavky" Jan můžete zobrazit seznam žádostí o podporu pro toto předplatné vytvořit ![případ zobrazení podrobností](./media/create-manage-support-requests-using-access-control/case-details-view.png)</span><span class="sxs-lookup"><span data-stu-id="036e1-140">Clicking "All support requests" Joe can see the list of support requests created for this Subscription  ![Case details view](./media/create-manage-support-requests-using-access-control/case-details-view.png)</span></span>

## <a name="remove-support-request-access-in-the-azure-portal"></a><span data-ttu-id="036e1-141">Odebrat přístup žádosti o podporu na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="036e1-141">Remove support request access in the Azure portal</span></span>

<span data-ttu-id="036e1-142">Stejně, jako je možné k udělení přístupu k uživatelům vytvářet a spravovat žádosti o podporu, je možné k odebrání přístupu pro uživatele i.</span><span class="sxs-lookup"><span data-stu-id="036e1-142">Just as it is possible to grant access to a user to create and manage support requests, it's possible to remove access for the user as well.</span></span>
<span data-ttu-id="036e1-143">Chcete-li odebrat možnost vytvořit a spravovat žádosti o podporu, přejděte na předplatné, klikněte na tlačítko "Nastavení" a klikněte na uživateli (v tomto případě Jan).</span><span class="sxs-lookup"><span data-stu-id="036e1-143">To remove the ability to create and manage support requests, go to the Subscription, click "Settings" and click the user (in this case, Joe).</span></span>
<span data-ttu-id="036e1-144">Klikněte pravým tlačítkem na název role, "Přispěvatel žádosti o podporu" a klikněte na tlačítko "Odebrat"</span><span class="sxs-lookup"><span data-stu-id="036e1-144">Right-click the role name, "Support Request Contributor" and click "Remove"</span></span>

![Odebrat přístup žádosti o podporu](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

<span data-ttu-id="036e1-146">Když Jan přihlásí k portálu a pokusí se vytvořit žádost o podporu, mohl dojde k následující chybě</span><span class="sxs-lookup"><span data-stu-id="036e1-146">When Joe logs in to the portal and tries to create a support request, he encounters the following error</span></span>

![Předplatné chyba-2](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

<span data-ttu-id="036e1-148">Jan nelze zobrazit žádné podporovat požadavky, pokud klikne "Podporují všechny požadavky"</span><span class="sxs-lookup"><span data-stu-id="036e1-148">Joe cannot see any support requests when he clicks "All support requests"</span></span>

![zobrazení podrobností případu-2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
