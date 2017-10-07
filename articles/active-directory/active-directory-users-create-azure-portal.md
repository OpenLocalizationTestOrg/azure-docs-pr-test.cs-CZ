---
title: "aaaAdd tooAzure nového uživatele služby Active Directory | Microsoft Docs"
description: "Vysvětluje, jak tooadd nové uživatele nebo změnit informace o uživateli v Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand;jeffsta
ms.reviewer: jeffsta
ms.openlocfilehash: c4a156ea31b81202bb0d0ac224afbfc3f1785532
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-new-users-tooazure-active-directory"></a><span data-ttu-id="c605e-103">Přidat nové uživatele tooAzure služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="c605e-103">Add new users tooAzure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c605e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c605e-104">Azure portal</span></span>](active-directory-users-create-azure-portal.md)
> * [<span data-ttu-id="c605e-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="c605e-105">Azure classic portal</span></span>](active-directory-create-users.md)
>
>

<span data-ttu-id="c605e-106">Tento článek vysvětluje, jak hello tooadd noví uživatelé ve vaší organizaci v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c605e-106">This article explains how tooadd new users in your organization in hello Azure Active Directory (Azure AD).</span></span> 

1. <span data-ttu-id="c605e-107">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="c605e-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="c605e-108">Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="c605e-108">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Otevírání uživatelů a skupin](./media/active-directory-users-create-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="c605e-110">Na hello **uživatelů a skupin** vyberte **všichni uživatelé**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c605e-110">On hello **Users and groups** blade, select **All users**, and then select **Add**.</span></span>

   ![Výběrem příkazu přidat hello](./media/active-directory-users-create-azure-portal/create-users-add-command.png)
4. <span data-ttu-id="c605e-112">Zadejte podrobnosti pro hello uživatele, jako například **název** a **uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="c605e-112">Enter details for hello user, such as **Name** and **User name**.</span></span> <span data-ttu-id="c605e-113">část názvu domény Hello hello uživatelské jméno musí být buď hello počáteční výchozí domény název "foo.onmicrosoft.com" název domény nebo název domény ověřené, nefederovaných například "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="c605e-113">hello domain name portion of hello user name must either be hello initial default domain name "foo.onmicrosoft.com" domain name, or a verified, non-federated domain name such as "contoso.com."</span></span>
5. <span data-ttu-id="c605e-114">Kopírování nebo jinak hello Poznámka vygenerované heslo uživatele tak, aby můžete pro jeho toohello uživatele po dokončení tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="c605e-114">Copy or otherwise note hello generated user password so that you can provide it toohello user after this process is complete.</span></span>
6. <span data-ttu-id="c605e-115">Volitelně můžete otevřít a vyplňte informace hello v hello **profil** okno, hello **skupiny** okno nebo hello **Directory role** okno pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="c605e-115">Optionally, you can open and fill out hello information in hello **Profile** blade, hello **Groups** blade, or hello **Directory role** blade for hello user.</span></span> <span data-ttu-id="c605e-116">Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="c605e-116">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>
7. <span data-ttu-id="c605e-117">Na hello **uživatele** vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c605e-117">On hello **User** blade, select **Create**.</span></span>
8. <span data-ttu-id="c605e-118">Bezpečně distribuujte hello vygenerované heslo toohello nového uživatele tak, aby hello uživatel může přihlásit.</span><span class="sxs-lookup"><span data-stu-id="c605e-118">Securely distribute hello generated password toohello new user so that hello user can sign in.</span></span>

### <a name="next-steps"></a><span data-ttu-id="c605e-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c605e-119">Next steps</span></span>
* [<span data-ttu-id="c605e-120">Přidat externího uživatele</span><span class="sxs-lookup"><span data-stu-id="c605e-120">Add an external user</span></span>](active-directory-users-create-external-azure-portal.md)
* [<span data-ttu-id="c605e-121">Resetovat heslo uživatele v hello nový portál Azure</span><span class="sxs-lookup"><span data-stu-id="c605e-121">Reset a user's password in hello new Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="c605e-122">Změnit informace o práci uživatele</span><span class="sxs-lookup"><span data-stu-id="c605e-122">Change a user's work information</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="c605e-123">Správa uživatelských profilů</span><span class="sxs-lookup"><span data-stu-id="c605e-123">Manage user profiles</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="c605e-124">Odstranění uživatele ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="c605e-124">Delete a user in your Azure AD</span></span>](active-directory-users-delete-user-azure-portal.md)
* [<span data-ttu-id="c605e-125">Přiřadit roli tooa uživatele ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="c605e-125">Assign a user tooa role in your Azure AD</span></span>](active-directory-users-assign-role-azure-portal.md)
