---
title: "Přiřadit uživatele k vlastní doméně v Azure Active Directory | Microsoft Docs"
description: "Postup naplnění vlastní doménu v Azure Active Directory s uživatelskými účty."
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
ms.openlocfilehash: 39cb54a6637088c35c6aef864a804c24803f48ba
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="assign-users-to-a-custom-domain"></a><span data-ttu-id="cc2e7-103">Přiřazení uživatelů k vlastní doméně</span><span class="sxs-lookup"><span data-stu-id="cc2e7-103">Assign users to a custom domain</span></span>
<span data-ttu-id="cc2e7-104">Po jste přidali vlastní domény do Azure Active Directory, je nutné přidat uživatelské účty pro tuto doménu tak, abyste ji mohli začít, je ověřování.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-104">After you have added your custom domain to Azure Active Directory, you must add the user accounts for this domain so that you can begin authenticating them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cc2e7-105">Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-105">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> <span data-ttu-id="cc2e7-106">Jak spravovat názvy domén v Centru správy služby Azure AD, najdete v článku [Správa vlastních názvů domén v Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cc2e7-106">For how to manage your domain names in the Azure AD admin center, see [Managing custom domain names in your Azure Active Directory](active-directory-domains-manage-azure-portal.md).</span></span>

## <a name="users-synced-from-a-on-premises-directory"></a><span data-ttu-id="cc2e7-107">Synchronizované z místního adresáře uživatelů</span><span class="sxs-lookup"><span data-stu-id="cc2e7-107">Users synced from a on-premises directory</span></span>
<span data-ttu-id="cc2e7-108">Pokud jste již vytvořili připojení mezi místní služby Active Directory a Azure Active Directory, může synchronizace vyplnit účty.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-108">If you have already set up a connection between your on-premises Active Directory and Azure Active Directory, synchronization can populate the accounts.</span></span> <span data-ttu-id="cc2e7-109">Další informace o tom, jak synchronizovat Azure Active Directory s vaší místní Active Directory najdete v tématu [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="cc2e7-109">For more information on how to synchronize Azure Active Directory with your on-premises Active Directory, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

## <a name="users-added-and-managed-in-the-cloud"></a><span data-ttu-id="cc2e7-110">Uživatelé přidat a spravovat v cloudu</span><span class="sxs-lookup"><span data-stu-id="cc2e7-110">Users added and managed in the cloud</span></span>
<span data-ttu-id="cc2e7-111">Chcete-li změnit doménu pro existující účet uživatele:</span><span class="sxs-lookup"><span data-stu-id="cc2e7-111">To change the domain for an existing user account:</span></span>

1. <span data-ttu-id="cc2e7-112">Otevřete portál Azure classic pomocí účtu, který je globální správce nebo správce uživatele.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-112">Open the Azure classic portal using an account that is a global admin or a user admin.</span></span>
2. <span data-ttu-id="cc2e7-113">Otevřete svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-113">Open your directory.</span></span>
3. <span data-ttu-id="cc2e7-114">Vyberte kartu **Uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-114">Select the **Users** tab.</span></span>
4. <span data-ttu-id="cc2e7-115">Vyberte uživatele ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-115">Select the user from the list.</span></span>
5. <span data-ttu-id="cc2e7-116">Změnit doménu pro uživatele a pak vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-116">Change the domain for the user, and then select **Save**.</span></span>

<span data-ttu-id="cc2e7-117">To můžete také provést pomocí [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) nebo [rozhraní Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).</span><span class="sxs-lookup"><span data-stu-id="cc2e7-117">This can also be done using [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) or the [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).</span></span>

## <a name="select-a-custom-domain-when-creating-a-new-user"></a><span data-ttu-id="cc2e7-118">Vyberte vlastní domény při vytváření nového uživatele</span><span class="sxs-lookup"><span data-stu-id="cc2e7-118">Select a custom domain when creating a new user</span></span>
1. <span data-ttu-id="cc2e7-119">Otevřete portál Azure classic pomocí účtu, který je globální správce nebo správce uživatele.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-119">Open the Azure classic portal using an account that is a global admin or a user admin.</span></span>
2. <span data-ttu-id="cc2e7-120">Otevřete svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-120">Open your directory.</span></span>
3. <span data-ttu-id="cc2e7-121">Vyberte kartu **Uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-121">Select the **Users** tab.</span></span>
4. <span data-ttu-id="cc2e7-122">Na panelu příkazů vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-122">In the command bar, select **Add**.</span></span>
5. <span data-ttu-id="cc2e7-123">Když přidáte uživatelské jméno, zvolte ze seznamu domény vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-123">When you add the user name, choose the custom domain from the domain list.</span></span>
6. <span data-ttu-id="cc2e7-124">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cc2e7-124">Select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc2e7-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cc2e7-125">Next steps</span></span>
* [<span data-ttu-id="cc2e7-126">Pomocí vlastních názvů domén ke zjednodušení přihlašování pro vaše uživatele</span><span class="sxs-lookup"><span data-stu-id="cc2e7-126">Using custom domain names to simplify the sign-in experience for your users</span></span>](active-directory-add-domain.md)
* [<span data-ttu-id="cc2e7-127">Správa vlastních názvů domén</span><span class="sxs-lookup"><span data-stu-id="cc2e7-127">Manage custom domain names</span></span>](active-directory-add-manage-domain-names.md)
* [<span data-ttu-id="cc2e7-128">Další informace o konceptech správy domén ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc2e7-128">Learn about domain management concepts in Azure AD</span></span>](active-directory-add-domain-concepts.md)

