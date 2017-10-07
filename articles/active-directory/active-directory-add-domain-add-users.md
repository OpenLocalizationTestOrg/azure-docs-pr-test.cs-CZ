---
title: "vlastní domény tooa aaaAssign uživatele v Azure Active Directory | Microsoft Docs"
description: "Jak toopopulate vlastní doménu v Azure Active Directory s uživatelskými účty."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 717b5a7c-7bc3-4ab1-98b5-4740b53338fe
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 23c338a361a90fddf42d4df90db94c9774305886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-users-tooa-custom-domain"></a><span data-ttu-id="7b4de-103">Přiřazení uživatelů tooa vlastní domény</span><span class="sxs-lookup"><span data-stu-id="7b4de-103">Assign users tooa custom domain</span></span>
<span data-ttu-id="7b4de-104">Po přidání tooAzure vaši vlastní doménu služby Active Directory, je nutné přidat hello uživatelské účty pro tuto doménu tak, abyste ji mohli začít, je ověřování.</span><span class="sxs-lookup"><span data-stu-id="7b4de-104">After you have added your custom domain tooAzure Active Directory, you must add hello user accounts for this domain so that you can begin authenticating them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7b4de-105">Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="7b4de-105">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> <span data-ttu-id="7b4de-106">Jak toomanage vaše názvů domén v Centru pro správu hello Azure AD, najdete v části [Správa vlastních názvů domén v Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7b4de-106">For how toomanage your domain names in hello Azure AD admin center, see [Managing custom domain names in your Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span></span>

## <a name="users-synced-from-a-on-premises-directory"></a><span data-ttu-id="7b4de-107">Synchronizované z místního adresáře uživatelů</span><span class="sxs-lookup"><span data-stu-id="7b4de-107">Users synced from a on-premises directory</span></span>
<span data-ttu-id="7b4de-108">Pokud jste již vytvořili připojení mezi místní služby Active Directory a Azure Active Directory, může synchronizace vyplnit hello účty.</span><span class="sxs-lookup"><span data-stu-id="7b4de-108">If you have already set up a connection between your on-premises Active Directory and Azure Active Directory, synchronization can populate hello accounts.</span></span> <span data-ttu-id="7b4de-109">Další informace o tom, jak toosynchronize Azure Active Directory s vaší místní služby Active Directory, najdete v části [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="7b4de-109">For more information on how toosynchronize Azure Active Directory with your on-premises Active Directory, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

## <a name="users-added-and-managed-in-hello-cloud"></a><span data-ttu-id="7b4de-110">Uživatelé přidat a spravovat v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="7b4de-110">Users added and managed in hello cloud</span></span>
<span data-ttu-id="7b4de-111">doména hello toochange pro existující účet uživatele:</span><span class="sxs-lookup"><span data-stu-id="7b4de-111">toochange hello domain for an existing user account:</span></span>

1. <span data-ttu-id="7b4de-112">Otevřete hello portál Azure classic pomocí účtu, který je globální správce nebo správce uživatele.</span><span class="sxs-lookup"><span data-stu-id="7b4de-112">Open hello Azure classic portal using an account that is a global admin or a user admin.</span></span>
2. <span data-ttu-id="7b4de-113">Otevřete svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="7b4de-113">Open your directory.</span></span>
3. <span data-ttu-id="7b4de-114">Vyberte hello **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="7b4de-114">Select hello **Users** tab.</span></span>
4. <span data-ttu-id="7b4de-115">Vyberte hello uživatele ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="7b4de-115">Select hello user from hello list.</span></span>
5. <span data-ttu-id="7b4de-116">Změnit doménu hello hello uživatele a pak vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7b4de-116">Change hello domain for hello user, and then select **Save**.</span></span>

<span data-ttu-id="7b4de-117">To můžete také provést pomocí [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) nebo hello [rozhraní Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).</span><span class="sxs-lookup"><span data-stu-id="7b4de-117">This can also be done using [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) or hello [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).</span></span>

## <a name="select-a-custom-domain-when-creating-a-new-user"></a><span data-ttu-id="7b4de-118">Vyberte vlastní domény při vytváření nového uživatele</span><span class="sxs-lookup"><span data-stu-id="7b4de-118">Select a custom domain when creating a new user</span></span>
1. <span data-ttu-id="7b4de-119">Otevřete hello portál Azure classic pomocí účtu, který je globální správce nebo správce uživatele.</span><span class="sxs-lookup"><span data-stu-id="7b4de-119">Open hello Azure classic portal using an account that is a global admin or a user admin.</span></span>
2. <span data-ttu-id="7b4de-120">Otevřete svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="7b4de-120">Open your directory.</span></span>
3. <span data-ttu-id="7b4de-121">Vyberte hello **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="7b4de-121">Select hello **Users** tab.</span></span>
4. <span data-ttu-id="7b4de-122">V řádku nabídek hello vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="7b4de-122">In hello command bar, select **Add**.</span></span>
5. <span data-ttu-id="7b4de-123">Když přidáte hello uživatelské jméno, zvolte vlastní domény hello hello domény seznamu.</span><span class="sxs-lookup"><span data-stu-id="7b4de-123">When you add hello user name, choose hello custom domain from hello domain list.</span></span>
6. <span data-ttu-id="7b4de-124">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7b4de-124">Select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7b4de-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7b4de-125">Next steps</span></span>
* [<span data-ttu-id="7b4de-126">Použití vlastní doménu názvy toosimplify hello přihlašování pro vaše uživatele</span><span class="sxs-lookup"><span data-stu-id="7b4de-126">Using custom domain names toosimplify hello sign-in experience for your users</span></span>](active-directory-add-domain.md)
* [<span data-ttu-id="7b4de-127">Správa vlastních názvů domén</span><span class="sxs-lookup"><span data-stu-id="7b4de-127">Manage custom domain names</span></span>](active-directory-add-manage-domain-names.md)
* [<span data-ttu-id="7b4de-128">Další informace o konceptech správy domén ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="7b4de-128">Learn about domain management concepts in Azure AD</span></span>](active-directory-add-domain-concepts.md)

