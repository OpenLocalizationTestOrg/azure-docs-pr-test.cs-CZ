---
title: "Auditování a vytváření sestav služby Azure Active Directory s B2B spolupráce uživatele | Microsoft Docs"
description: "Vlastnosti uživatele Guest se dají konfigurovat v spolupráce Azure Active Directory s B2B"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/12/2017
ms.author: sasubram
ms.openlocfilehash: ba782270f3280e52235bc13148d232284b55762a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="auditing-and-reporting-a-b2b-collaboration-user"></a><span data-ttu-id="9231c-103">Auditování a vytváření sestav uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="9231c-103">Auditing and reporting a B2B collaboration user</span></span>
<span data-ttu-id="9231c-104">Uživatele typu Host máte auditování podobné možnosti jako s uživateli člen.</span><span class="sxs-lookup"><span data-stu-id="9231c-104">With guest users, you have auditing capabilities similar to with member users.</span></span> <span data-ttu-id="9231c-105">Tady je příklad pozvánky a uplatnění historii pozvaný Sam Oogle:</span><span class="sxs-lookup"><span data-stu-id="9231c-105">Here's an example of the invitation and redemption history of invitee Sam Oogle:</span></span>

![Protokol auditování](./media/active-directory-b2b-auditing-and-reporting/audit-log.png)

<span data-ttu-id="9231c-107">Můžete podrobně každou z těchto událostí získat podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9231c-107">You can dive into each of these events to get the details.</span></span> <span data-ttu-id="9231c-108">Například podíváme se na podrobnosti o přijetí.</span><span class="sxs-lookup"><span data-stu-id="9231c-108">For example, let's look at the acceptance details.</span></span>

![Podrobnosti o aktivitě](./media/active-directory-b2b-auditing-and-reporting/activity-details.png)

<span data-ttu-id="9231c-110">Můžete také exportovat tyto protokoly z Azure AD a používat nástroj pro vytváření sestav podle svého výběru k získání přizpůsobených sestav.</span><span class="sxs-lookup"><span data-stu-id="9231c-110">You can also export these logs from Azure AD and use the reporting tool of your choice to get customized reports.</span></span>

### <a name="next-steps"></a><span data-ttu-id="9231c-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9231c-111">Next steps</span></span>

<span data-ttu-id="9231c-112">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="9231c-112">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="9231c-113">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="9231c-113">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="9231c-114">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="9231c-114">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="9231c-115">Přidání uživatele spolupráce B2B k roli</span><span class="sxs-lookup"><span data-stu-id="9231c-115">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="9231c-116">Delegovat pozvánek B2bB spolupráce</span><span class="sxs-lookup"><span data-stu-id="9231c-116">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="9231c-117">Dynamické skupiny a spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="9231c-117">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="9231c-118">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9231c-118">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="9231c-119">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="9231c-119">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="9231c-120">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="9231c-120">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="9231c-121">Deklarace uživatele spolupráce B2B mapování</span><span class="sxs-lookup"><span data-stu-id="9231c-121">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="9231c-122">Externí sdílení Office 365</span><span class="sxs-lookup"><span data-stu-id="9231c-122">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="9231c-123">Aktuální omezení spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="9231c-123">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
