---
title: "Přidání nových uživatelů do služby Azure Active Directory | Dokumentace Microsoftu"
description: "Vysvětluje, jak přidávat nové uživatele ve službě Azure Active Directory."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 13a7d2d3b991206c45e66872b590bc27a224eead
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="quickstart-add-new-users-to-azure-active-directory"></a><span data-ttu-id="cc56f-103">Rychlé spuštění: Přidání nových uživatelů do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cc56f-103">Quickstart: Add new users to Azure Active Directory</span></span>
<span data-ttu-id="cc56f-104">Tento článek vysvětluje, jak přidat nové uživatele ve vaší organizaci ve službě Azure Active Directory (Azure AD) jednoho současně pomocí portálu Azure nebo synchronizace účtu místního systému Windows Server AD uživatelská data.</span><span class="sxs-lookup"><span data-stu-id="cc56f-104">This article explains how to add new users in your organization in the Azure Active Directory (Azure AD) one at a time using the Azure portal or by synchronizing your on-premises Windows Server AD user account data.</span></span> 

## <a name="add-cloud-based-users"></a><span data-ttu-id="cc56f-105">Přidání cloudových uživatelů</span><span class="sxs-lookup"><span data-stu-id="cc56f-105">Add cloud-based users</span></span>
1. <span data-ttu-id="cc56f-106">Přihlaste se k [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="cc56f-106">Sign in to the [Azure Active Directory admin center](https://aad.portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="cc56f-107">Vyberte **Azure Active Directory** a potom **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cc56f-107">Select **Azure Active Directory** and then **Users and groups**.</span></span>
3. <span data-ttu-id="cc56f-108">Na **uživatelů a skupin** vyberte **všichni uživatelé**a potom vyberte **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="cc56f-108">On the **Users and groups** blade, select **All users**, and then select **New user**.</span></span>
   <span data-ttu-id="cc56f-109">![Použití příkazu Přidat](./media/add-users-azure-active-directory/add-user.png)</span><span class="sxs-lookup"><span data-stu-id="cc56f-109">![Selecting the Add command](./media/add-users-azure-active-directory/add-user.png)</span></span>
4. <span data-ttu-id="cc56f-110">Zadejte podrobnosti pro uživatele, jako například **název** a **uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="cc56f-110">Enter details for the user, such as **Name** and **User name**.</span></span> <span data-ttu-id="cc56f-111">Část názvu domény uživatelského jména musí buď musí být počáteční výchozí domény název "[název domény].onmicrosoft.com" nebo ověřené, nefederovaných [vlastní název domény](add-custom-domain.md) například "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="cc56f-111">The domain name portion of the user name must either be the initial default domain name "[domain name].onmicrosoft.com" or a verified, non-federated [custom domain name](add-custom-domain.md) such as "contoso.com."</span></span>
5. <span data-ttu-id="cc56f-112">Zkopírujte nebo jinak Poznámka: hesla generovaného uživatele tak, aby ji mohli nabídnout uživatele po dokončení tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="cc56f-112">Copy or otherwise note the generated user password so that you can provide it to the user after this process is complete.</span></span>
6. <span data-ttu-id="cc56f-113">Volitelně můžete otevřít a vyplňte informace v **profil** okně **skupiny** okno, nebo **Directory role** okno pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="cc56f-113">Optionally, you can open and fill out the information in the **Profile** blade, the **Groups** blade, or the **Directory role** blade for the user.</span></span> <span data-ttu-id="cc56f-114">Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="cc56f-114">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>
7. <span data-ttu-id="cc56f-115">Na **uživatele** vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cc56f-115">On the **User** blade, select **Create**.</span></span>
8. <span data-ttu-id="cc56f-116">Bezpečně distribuujte vygenerované heslo pro nového uživatele tak, aby uživatel moct přihlásit.</span><span class="sxs-lookup"><span data-stu-id="cc56f-116">Securely distribute the generated password to the new user so that the user can sign in.</span></span>

> [!TIP]
> <span data-ttu-id="cc56f-117">Můžete také synchronizovat data uživatelských účtů z místního systému Windows Server AD.</span><span class="sxs-lookup"><span data-stu-id="cc56f-117">You can also synchronize user account data from on-premises Windows Server AD.</span></span> <span data-ttu-id="cc56f-118">Řešení společnosti Microsoft identity span místní a cloudové schopnosti, vytváření identitu jednoho uživatele pro ověřování a autorizaci pro všechny prostředky, bez ohledu na umístění.</span><span class="sxs-lookup"><span data-stu-id="cc56f-118">Microsoft’s identity solutions span on-premises and cloud-based capabilities, creating a single user identity for authentication and authorization to all resources, regardless of location.</span></span> <span data-ttu-id="cc56f-119">Říkáme toto hybridní identita.</span><span class="sxs-lookup"><span data-stu-id="cc56f-119">We call this Hybrid Identity.</span></span> <span data-ttu-id="cc56f-120">[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) slouží k integraci místních adresářů se službou Azure Active Directory pro hybridní identity scénáře.</span><span class="sxs-lookup"><span data-stu-id="cc56f-120">[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) can be used to integrate your on-premises directories with Azure Active Directory for hybrid identity scenarios.</span></span> <span data-ttu-id="cc56f-121">To umožní poskytovat společnou identitu pro uživatele pro aplikace Office 365, Azure a SaaS integrované s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc56f-121">This allows you to provide a common identity for your users for Office 365, Azure, and SaaS applications integrated with Azure AD.</span></span> 

## <a name="delete-users-from-azure-ad"></a><span data-ttu-id="cc56f-122">Odstranit uživatele z Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc56f-122">Delete users from Azure AD</span></span>
1. <span data-ttu-id="cc56f-123">Přihlaste se k [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="cc56f-123">Sign in to the [Azure Active Directory admin center](https://aad.portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="cc56f-124">Vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cc56f-124">Select **Users and groups**.</span></span>
3. <span data-ttu-id="cc56f-125">Na **uživatelů a skupin** okně vyberte uživatele, kterého chcete odstranit ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="cc56f-125">On the **Users and groups** blade, select the user to delete from the list.</span></span> 
4. <span data-ttu-id="cc56f-126">V okně pro vybraného uživatele, vyberte **přehled**a potom na panelu příkazů vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="cc56f-126">On the blade for the selected user, select **Overview**, and then in the command bar, select **Delete**.</span></span>
   <span data-ttu-id="cc56f-127">![Použití příkazu Přidat](./media/add-users-azure-active-directory/delete-user.png)</span><span class="sxs-lookup"><span data-stu-id="cc56f-127">![Selecting the Add command](./media/add-users-azure-active-directory/delete-user.png)</span></span>


### <a name="learn-more"></a><span data-ttu-id="cc56f-128">Další informace</span><span class="sxs-lookup"><span data-stu-id="cc56f-128">Learn more</span></span> 
* [<span data-ttu-id="cc56f-129">Přidat externího uživatele</span><span class="sxs-lookup"><span data-stu-id="cc56f-129">Add an external user</span></span>](active-directory-users-create-external-azure-portal.md)

* [<span data-ttu-id="cc56f-130">Přiřazení uživatele k roli ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc56f-130">Assign a user to a role in your Azure AD</span></span>](active-directory-users-assign-role-azure-portal.md)

## <a name="next-steps"></a><span data-ttu-id="cc56f-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cc56f-131">Next steps</span></span>
<span data-ttu-id="cc56f-132">V tento rychlý start když jste se naučili postup přidání nových uživatelů do Azure AD Premium.</span><span class="sxs-lookup"><span data-stu-id="cc56f-132">In this quickstart, you’ve learned how to add new users to Azure AD Premium.</span></span> 

<span data-ttu-id="cc56f-133">Na následující odkaz můžete použít k vytvoření nového uživatele ve službě Azure AD na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cc56f-133">You can use the following link to create a new user in Azure AD from the Azure portal.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="cc56f-134">Přidání uživatelů do Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc56f-134">Add users to Azure AD</span></span>](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) 