---
title: "Přidání nových uživatelů do služby Azure Active Directory | Dokumentace Microsoftu"
description: "Vysvětluje, jak ve službě Azure Active Directory přidat nové uživatele nebo změnit informace o uživatelích."
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
ms.openlocfilehash: bfe0c556d94d50207a23d2e3984371fb602e9406
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-new-users-to-azure-active-directory"></a><span data-ttu-id="8a1da-103">Přidání nových uživatelů do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8a1da-103">Add new users to Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8a1da-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8a1da-104">Azure portal</span></span>](active-directory-users-create-azure-portal.md)
> * [<span data-ttu-id="8a1da-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="8a1da-105">Azure classic portal</span></span>](active-directory-create-users.md)
>
>

<span data-ttu-id="8a1da-106">Tento článek vysvětluje, jak přidat nové uživatele ve vaší organizaci ve službě Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8a1da-106">This article explains how to add new users in your organization in the Azure Active Directory (Azure AD).</span></span> 

1. <span data-ttu-id="8a1da-107">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="8a1da-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="8a1da-108">Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="8a1da-108">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Otevírání uživatelů a skupin](./media/active-directory-users-create-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="8a1da-110">Na **uživatelů a skupin** vyberte **všichni uživatelé**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8a1da-110">On the **Users and groups** blade, select **All users**, and then select **Add**.</span></span>

   ![Použití příkazu Přidat](./media/active-directory-users-create-azure-portal/create-users-add-command.png)
4. <span data-ttu-id="8a1da-112">Zadejte podrobnosti pro uživatele, jako například **název** a **uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="8a1da-112">Enter details for the user, such as **Name** and **User name**.</span></span> <span data-ttu-id="8a1da-113">Část názvu domény uživatelské jméno musí být buď počáteční výchozí název domény "foo.onmicrosoft.com" název domény nebo název domény ověřené, nefederovaných například "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="8a1da-113">The domain name portion of the user name must either be the initial default domain name "foo.onmicrosoft.com" domain name, or a verified, non-federated domain name such as "contoso.com."</span></span>
5. <span data-ttu-id="8a1da-114">Zkopírujte nebo jinak Poznámka: hesla generovaného uživatele tak, aby ji mohli nabídnout uživatele po dokončení tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="8a1da-114">Copy or otherwise note the generated user password so that you can provide it to the user after this process is complete.</span></span>
6. <span data-ttu-id="8a1da-115">Volitelně můžete otevřít a vyplňte informace v **profil** okně **skupiny** okno, nebo **Directory role** okno pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="8a1da-115">Optionally, you can open and fill out the information in the **Profile** blade, the **Groups** blade, or the **Directory role** blade for the user.</span></span> <span data-ttu-id="8a1da-116">Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="8a1da-116">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>
7. <span data-ttu-id="8a1da-117">Na **uživatele** vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8a1da-117">On the **User** blade, select **Create**.</span></span>
8. <span data-ttu-id="8a1da-118">Bezpečně distribuujte vygenerované heslo pro nového uživatele tak, aby uživatel moct přihlásit.</span><span class="sxs-lookup"><span data-stu-id="8a1da-118">Securely distribute the generated password to the new user so that the user can sign in.</span></span>

### <a name="next-steps"></a><span data-ttu-id="8a1da-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a1da-119">Next steps</span></span>
* [<span data-ttu-id="8a1da-120">Přidat externího uživatele</span><span class="sxs-lookup"><span data-stu-id="8a1da-120">Add an external user</span></span>](active-directory-users-create-external-azure-portal.md)
* [<span data-ttu-id="8a1da-121">Resetovat heslo uživatele v nový portál Azure</span><span class="sxs-lookup"><span data-stu-id="8a1da-121">Reset a user's password in the new Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="8a1da-122">Změnit informace o práci uživatele</span><span class="sxs-lookup"><span data-stu-id="8a1da-122">Change a user's work information</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="8a1da-123">Správa uživatelských profilů</span><span class="sxs-lookup"><span data-stu-id="8a1da-123">Manage user profiles</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="8a1da-124">Odstranění uživatele ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="8a1da-124">Delete a user in your Azure AD</span></span>](active-directory-users-delete-user-azure-portal.md)
* [<span data-ttu-id="8a1da-125">Přiřazení uživatele k roli ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="8a1da-125">Assign a user to a role in your Azure AD</span></span>](active-directory-users-assign-role-azure-portal.md)
