---
title: "aaaAdd tooAzure nového uživatele služby Active Directory | Microsoft Docs"
description: "Vysvětluje, jak tooadd nové uživatele ve službě Azure Active Directory."
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
ms.openlocfilehash: 6ca413c84a7a5238a30fd26fc751d687d827b24a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-add-new-users-tooazure-active-directory"></a><span data-ttu-id="da8ed-103">Rychlý úvod: Přidat nové uživatele tooAzure služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="da8ed-103">Quickstart: Add new users tooAzure Active Directory</span></span>
<span data-ttu-id="da8ed-104">Tento článek vysvětluje, jak tooadd noví uživatelé ve vaší organizaci v Azure Active Directory (Azure AD) hello jeden současně pomocí hello portál Azure nebo synchronizaci uživatelů systému Windows Server AD místní účet data.</span><span class="sxs-lookup"><span data-stu-id="da8ed-104">This article explains how tooadd new users in your organization in hello Azure Active Directory (Azure AD) one at a time using hello Azure portal or by synchronizing your on-premises Windows Server AD user account data.</span></span> 

## <a name="add-cloud-based-users"></a><span data-ttu-id="da8ed-105">Přidání cloudových uživatelů</span><span class="sxs-lookup"><span data-stu-id="da8ed-105">Add cloud-based users</span></span>
1. <span data-ttu-id="da8ed-106">Přihlaste se toohello [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="da8ed-106">Sign in toohello [Azure Active Directory admin center](https://aad.portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="da8ed-107">Vyberte **Azure Active Directory** a potom **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="da8ed-107">Select **Azure Active Directory** and then **Users and groups**.</span></span>
3. <span data-ttu-id="da8ed-108">Na hello **uživatelů a skupin** vyberte **všichni uživatelé**a potom vyberte **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="da8ed-108">On hello **Users and groups** blade, select **All users**, and then select **New user**.</span></span>
   <span data-ttu-id="da8ed-109">![Výběrem příkazu přidat hello](./media/add-users-azure-active-directory/add-user.png)</span><span class="sxs-lookup"><span data-stu-id="da8ed-109">![Selecting hello Add command](./media/add-users-azure-active-directory/add-user.png)</span></span>
4. <span data-ttu-id="da8ed-110">Zadejte podrobnosti pro hello uživatele, jako například **název** a **uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="da8ed-110">Enter details for hello user, such as **Name** and **User name**.</span></span> <span data-ttu-id="da8ed-111">Hello část názvu domény hello uživatelské jméno musí být buď hello počáteční výchozí domény název "[název domény].onmicrosoft.com" nebo ověřené, nefederovaných [vlastní název domény](add-custom-domain.md) například "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="da8ed-111">hello domain name portion of hello user name must either be hello initial default domain name "[domain name].onmicrosoft.com" or a verified, non-federated [custom domain name](add-custom-domain.md) such as "contoso.com."</span></span>
5. <span data-ttu-id="da8ed-112">Kopírování nebo jinak hello Poznámka vygenerované heslo uživatele tak, aby můžete pro jeho toohello uživatele po dokončení tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="da8ed-112">Copy or otherwise note hello generated user password so that you can provide it toohello user after this process is complete.</span></span>
6. <span data-ttu-id="da8ed-113">Volitelně můžete otevřít a vyplňte informace hello v hello **profil** okno, hello **skupiny** okno nebo hello **Directory role** okno pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="da8ed-113">Optionally, you can open and fill out hello information in hello **Profile** blade, hello **Groups** blade, or hello **Directory role** blade for hello user.</span></span> <span data-ttu-id="da8ed-114">Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="da8ed-114">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>
7. <span data-ttu-id="da8ed-115">Na hello **uživatele** vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="da8ed-115">On hello **User** blade, select **Create**.</span></span>
8. <span data-ttu-id="da8ed-116">Bezpečně distribuujte hello vygenerované heslo toohello nového uživatele tak, aby hello uživatel může přihlásit.</span><span class="sxs-lookup"><span data-stu-id="da8ed-116">Securely distribute hello generated password toohello new user so that hello user can sign in.</span></span>

> [!TIP]
> <span data-ttu-id="da8ed-117">Můžete také synchronizovat data uživatelských účtů z místního systému Windows Server AD.</span><span class="sxs-lookup"><span data-stu-id="da8ed-117">You can also synchronize user account data from on-premises Windows Server AD.</span></span> <span data-ttu-id="da8ed-118">Řešení společnosti Microsoft identity span místní a cloudové schopnosti, vytváření identitu jednoho uživatele pro ověřování a autorizaci tooall prostředků, bez ohledu na umístění.</span><span class="sxs-lookup"><span data-stu-id="da8ed-118">Microsoft’s identity solutions span on-premises and cloud-based capabilities, creating a single user identity for authentication and authorization tooall resources, regardless of location.</span></span> <span data-ttu-id="da8ed-119">Říkáme toto hybridní identita.</span><span class="sxs-lookup"><span data-stu-id="da8ed-119">We call this Hybrid Identity.</span></span> <span data-ttu-id="da8ed-120">[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) lze použít toointegrate místních adresářů se službou Azure Active Directory pro hybridní identity scénáře.</span><span class="sxs-lookup"><span data-stu-id="da8ed-120">[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) can be used toointegrate your on-premises directories with Azure Active Directory for hybrid identity scenarios.</span></span> <span data-ttu-id="da8ed-121">To vám umožní tooprovide společnou identitu pro uživatele pro aplikace Office 365, Azure a SaaS integrované s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="da8ed-121">This allows you tooprovide a common identity for your users for Office 365, Azure, and SaaS applications integrated with Azure AD.</span></span> 

## <a name="delete-users-from-azure-ad"></a><span data-ttu-id="da8ed-122">Odstranit uživatele z Azure AD</span><span class="sxs-lookup"><span data-stu-id="da8ed-122">Delete users from Azure AD</span></span>
1. <span data-ttu-id="da8ed-123">Přihlaste se toohello [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="da8ed-123">Sign in toohello [Azure Active Directory admin center](https://aad.portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="da8ed-124">Vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="da8ed-124">Select **Users and groups**.</span></span>
3. <span data-ttu-id="da8ed-125">Na hello **uživatelů a skupin** okně, vyberte hello toodelete uživatele ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="da8ed-125">On hello **Users and groups** blade, select hello user toodelete from hello list.</span></span> 
4. <span data-ttu-id="da8ed-126">V okně hello hello vybraného uživatele, vyberte **přehled**a pak v řádku nabídek hello vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="da8ed-126">On hello blade for hello selected user, select **Overview**, and then in hello command bar, select **Delete**.</span></span>
   <span data-ttu-id="da8ed-127">![Výběrem příkazu přidat hello](./media/add-users-azure-active-directory/delete-user.png)</span><span class="sxs-lookup"><span data-stu-id="da8ed-127">![Selecting hello Add command](./media/add-users-azure-active-directory/delete-user.png)</span></span>


### <a name="learn-more"></a><span data-ttu-id="da8ed-128">Další informace</span><span class="sxs-lookup"><span data-stu-id="da8ed-128">Learn more</span></span> 
* [<span data-ttu-id="da8ed-129">Přidat externího uživatele</span><span class="sxs-lookup"><span data-stu-id="da8ed-129">Add an external user</span></span>](active-directory-users-create-external-azure-portal.md)

* [<span data-ttu-id="da8ed-130">Přiřadit roli tooa uživatele ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="da8ed-130">Assign a user tooa role in your Azure AD</span></span>](active-directory-users-assign-role-azure-portal.md)

## <a name="next-steps"></a><span data-ttu-id="da8ed-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="da8ed-131">Next steps</span></span>
<span data-ttu-id="da8ed-132">V tento rychlý start, když jste se naučili jak tooadd nové uživatele tooAzure AD Premium.</span><span class="sxs-lookup"><span data-stu-id="da8ed-132">In this quickstart, you’ve learned how tooadd new users tooAzure AD Premium.</span></span> 

<span data-ttu-id="da8ed-133">Můžete použít hello následujících odkaz toocreate nového uživatele ve službě Azure AD z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="da8ed-133">You can use hello following link toocreate a new user in Azure AD from hello Azure portal.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="da8ed-134">Přidat uživatele tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="da8ed-134">Add users tooAzure AD</span></span>](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) 
