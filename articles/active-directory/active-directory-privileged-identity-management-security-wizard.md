---
title: "Průvodce Azure AD Privileged Identity Management zabezpečení"
description: "Při prvním použití Azure Active Directory Privileged Identity Management rozšíření, zobrazí se Průvodce zabezpečení. Tento článek popisuje kroky pro pomocí průvodce."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: a53a3719-8cc7-4fc7-8164-aafca192871b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 260d178f3d8158411b3ad266e3b0d15edbebc722
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-security-wizard-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="b6a9c-104">Pomocí Průvodce zabezpečení v Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="b6a9c-104">Using the security wizard in Azure AD Privileged Identity Management</span></span> 
<span data-ttu-id="b6a9c-105">Pokud jste první osobou ke spuštění Azure Privileged Identity Management (PIM) pro vaši organizaci, zobrazí se průvodce.</span><span class="sxs-lookup"><span data-stu-id="b6a9c-105">If you're the first person to run Azure Privileged Identity Management (PIM) for your organization, you will be presented with a wizard.</span></span> <span data-ttu-id="b6a9c-106">Průvodce vám pomůže pochopit bezpečnostní rizika privilegované identity a jak používat PIM k omezení těchto rizik.</span><span class="sxs-lookup"><span data-stu-id="b6a9c-106">The wizard helps you understand the security risks of privileged identities and how to use PIM to reduce those risks.</span></span> <span data-ttu-id="b6a9c-107">Nepotřebujete žádné změny existujících přiřazení role v průvodci, pokud chcete provést později.</span><span class="sxs-lookup"><span data-stu-id="b6a9c-107">You don't need to make any changes to existing role assignments in the wizard, if you prefer to do it later.</span></span>

## <a name="what-to-expect"></a><span data-ttu-id="b6a9c-108">Co můžete očekávat</span><span class="sxs-lookup"><span data-stu-id="b6a9c-108">What to expect</span></span>
<span data-ttu-id="b6a9c-109">Předtím, než vaše organizace zahájí pomocí PIM, všechna přiřazení rolí jsou trvalé: uživatelé se vždycky nacházejí v těchto rolí i v případě, že v současné době nepotřebují svá oprávnění.</span><span class="sxs-lookup"><span data-stu-id="b6a9c-109">Before your organization starts using PIM, all role assignments are permanent: the users are always in these roles even if they do not presently need their privileges.</span></span>  <span data-ttu-id="b6a9c-110">Prvním krokem v Průvodci se dozvíte, seznam rolí s vysokými oprávněními a počet uživatelů, kteří jsou aktuálně v těchto rolí.</span><span class="sxs-lookup"><span data-stu-id="b6a9c-110">The first step of the wizard shows you a list of high-privileged roles and how many users are currently in those roles.</span></span> <span data-ttu-id="b6a9c-111">Můžete zobrazit další podrobnosti konkrétní roli Další informace o uživatelích, pokud jeden nebo více těchto nejste obeznámeni.</span><span class="sxs-lookup"><span data-stu-id="b6a9c-111">You can drill in to a particular role to learn more about users if one or more of them are unfamiliar.</span></span>

<span data-ttu-id="b6a9c-112">Druhý krok průvodce vám dává příležitost, chcete-li změnit přiřazení rolí správce.</span><span class="sxs-lookup"><span data-stu-id="b6a9c-112">The second step of the wizard gives you an opportunity to change administrator's role assignments.</span></span>  

> [!WARNING]
> <span data-ttu-id="b6a9c-113">Je důležité, že máte minimálně jeden globální správce a více než jeden správce privilegovaných rolí s účtem organizace (ne účet Microsoft).</span><span class="sxs-lookup"><span data-stu-id="b6a9c-113">It is important that you have at least one global administrator, and more than one privileged role administrator with an organizational account (not a Microsoft account).</span></span> <span data-ttu-id="b6a9c-114">Pokud je jen jeden správce privilegovaných rolí, organizace nebude možné spravovat PIM, pokud tento účet je odstraněný.</span><span class="sxs-lookup"><span data-stu-id="b6a9c-114">If there is only one privileged role administrator, the organization will not be able to manage PIM if that account is deleted.</span></span>
> <span data-ttu-id="b6a9c-115">Navíc zachovat přiřazení rolí trvalé, pokud má uživatel účet Microsoft (účet, které používají pro přihlášení ke službám Microsoft, jako je Skype a Outlook.com).</span><span class="sxs-lookup"><span data-stu-id="b6a9c-115">Also, keep role assignments permanent if a user has a Microsoft account (An account they use to sign in to Microsoft services like Skype and Outlook.com).</span></span> <span data-ttu-id="b6a9c-116">Pokud budete chtít vyžadovat vícefaktorové ověřování pro aktivaci pro tuto roli, tento uživatel bude uzamčena na.</span><span class="sxs-lookup"><span data-stu-id="b6a9c-116">If you plan to require MFA for activation for that role, that user will be locked out.</span></span>
> 
> 

<span data-ttu-id="b6a9c-117">Po provedení změn, Průvodce zobrazí.</span><span class="sxs-lookup"><span data-stu-id="b6a9c-117">After you have made changes, the wizard will no longer show up.</span></span> <span data-ttu-id="b6a9c-118">Při příštím vy nebo jiný správce privilegovaných rolí používat PIM, se zobrazí řídicí panel PIM.</span><span class="sxs-lookup"><span data-stu-id="b6a9c-118">The next time you or another privileged role administrator use PIM, you will see the PIM dashboard.</span></span>  

* <span data-ttu-id="b6a9c-119">Pokud chcete přidat nebo odebrat uživatele z role nebo změnit přiřazení z trvalé na oprávněné, přečtěte si informace v [jak přidat nebo odebrat roli uživatele](active-directory-privileged-identity-management-how-to-add-role-to-user.md).</span><span class="sxs-lookup"><span data-stu-id="b6a9c-119">If you would like to add or remove users from roles or change assignments from permanent to eligible, read more at [how to add or remove a user's role](active-directory-privileged-identity-management-how-to-add-role-to-user.md).</span></span>
* <span data-ttu-id="b6a9c-120">Pokud chcete poskytnout přístup ke správě PIM více uživatelů, přečtěte si informace v [poskytnout přístup ke správě v PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="b6a9c-120">If you would like to give more users access to manage PIM, read more at [how to give access to manage in PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6a9c-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6a9c-121">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

