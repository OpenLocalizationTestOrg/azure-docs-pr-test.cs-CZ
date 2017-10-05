---
title: "Řešení potíží s dynamické členství ve skupinách | Microsoft Docs"
description: "Tipy k řešení potíží pro dynamické členství ve skupinách v Azure AD."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 89bb04b6-a379-49c2-8465-fe386641816a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: 32947e8cc69c9a48d9a285bf0a37ab3398571f86
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a><span data-ttu-id="1696d-103">Řešení potíží s dynamickým členstvím ve skupinách</span><span class="sxs-lookup"><span data-stu-id="1696d-103">Troubleshooting dynamic memberships for groups</span></span>
<span data-ttu-id="1696d-104">**I nakonfigurovali pravidlo na skupinu, ale aktualizovat žádné členství ve skupině**</span><span class="sxs-lookup"><span data-stu-id="1696d-104">**I configured a rule on a group but no memberships get updated in the group**</span></span><br/><span data-ttu-id="1696d-105">Ověřte, zda **povolit delegovaná Správa skupin** nastavení je **Ano** v **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="1696d-105">Verify that the **Enable delegated group management** setting is set to **Yes** in the **Configure** tab.</span></span> <span data-ttu-id="1696d-106">Toto nastavení se zobrazí jenom v případě, že jste přihlášeni jako uživatel, kterému je přiřazena licenci Azure Active Directory Premium.</span><span class="sxs-lookup"><span data-stu-id="1696d-106">You will see this setting only if you are signed in as a user to whom an Azure Active Directory Premium license is assigned.</span></span> <span data-ttu-id="1696d-107">Ověřte hodnoty atributů uživatele v pravidle: existují uživatelé, kteří splňovat pravidla?</span><span class="sxs-lookup"><span data-stu-id="1696d-107">Verify the values for user attributes on the rule: are there users that satisfy the rule?</span></span>

<span data-ttu-id="1696d-108">**I nakonfigurovali pravidlo, ale teď se odebrat členy existující pravidla**</span><span class="sxs-lookup"><span data-stu-id="1696d-108">**I configured a rule, but now the existing members of the rule are removed**</span></span><br/><span data-ttu-id="1696d-109">Toto je očekávané chování.</span><span class="sxs-lookup"><span data-stu-id="1696d-109">This is expected behavior.</span></span> <span data-ttu-id="1696d-110">Stávající členy skupiny se odeberou, když je pravidlem povolené nebo změnit.</span><span class="sxs-lookup"><span data-stu-id="1696d-110">Existing members of the group are removed when a rule is enabled or changed.</span></span> <span data-ttu-id="1696d-111">Uživatelé vrácená z vyhodnocení pravidla jsou přidány jako členy do skupiny.</span><span class="sxs-lookup"><span data-stu-id="1696d-111">The users returned from evaluation of the rule are added as members to the group.</span></span>     

<span data-ttu-id="1696d-112">**Není zobrazen, že změní okamžitě při přidání nebo změna pravidla, proč ne?**</span><span class="sxs-lookup"><span data-stu-id="1696d-112">**I don’t see membership changes instantly when I add or change a rule, why not?**</span></span><br/><span data-ttu-id="1696d-113">Vyhodnocování členství vyhrazené se provádí pravidelně v procesu asynchronní pozadí.</span><span class="sxs-lookup"><span data-stu-id="1696d-113">Dedicated membership evaluation is done periodically in an asynchronous background process.</span></span> <span data-ttu-id="1696d-114">Jak dlouho trvá proces je určen podle počet uživatelů ve vašem adresáři a velikost skupiny vytvořeny v důsledku pravidlo.</span><span class="sxs-lookup"><span data-stu-id="1696d-114">How long the process takes is determined by the number of users in your directory and the size of the group created as a result of the rule.</span></span> <span data-ttu-id="1696d-115">Obvykle adresáře s malý počet uživatelů se zobrazí změn členství ve skupinách za méně než několik minut.</span><span class="sxs-lookup"><span data-stu-id="1696d-115">Typically, directories with small numbers of users will see the group membership changes in less than a few minutes.</span></span> <span data-ttu-id="1696d-116">Adresáře s velký počet uživatelů, může trvat 30 minut nebo déle, než se naplnění.</span><span class="sxs-lookup"><span data-stu-id="1696d-116">Directories with a large number of users can take 30 minutes or longer to populate.</span></span>

### <a name="next-steps"></a><span data-ttu-id="1696d-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1696d-117">Next steps</span></span>
<span data-ttu-id="1696d-118">Následující články poskytují další informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1696d-118">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="1696d-119">Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1696d-119">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="1696d-120">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1696d-120">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="1696d-121">Představení služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1696d-121">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="1696d-122">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1696d-122">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
