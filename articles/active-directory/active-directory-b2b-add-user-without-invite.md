---
title: "Přidání uživatelů spolupráce B2B do Azure Active Directory bez pozvánku | Microsoft Docs"
description: "Můžete je nechat uživatele guest, přidat další uživatele typu Host do služby Azure AD bez uplatňuje Pozvánka v Azure Active Directory s B2B spolupráce."
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
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: 91b9477cdb679851e7d8d2942c06999a05f64e46
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a><span data-ttu-id="a76f3-103">Přidat uživatele typu Host spolupráce B2B bez Pozvánka</span><span class="sxs-lookup"><span data-stu-id="a76f3-103">Add B2B collaboration guest users without an invitation</span></span>

<span data-ttu-id="a76f3-104">Můžete povolit uživateli, jako je například zástupce partnera, přidat uživatele z partnera pro vaši organizaci bez nutnosti pozvánek k uplatnit.</span><span class="sxs-lookup"><span data-stu-id="a76f3-104">You can allow a user, such as a partner representative, to add users from the partner to your organization without needing invitations to be redeemed.</span></span> <span data-ttu-id="a76f3-105">Jediné, co musíte udělat je uživateli udělit oprávnění výčet v adresáři, kterou používáte pro org. partnera</span><span class="sxs-lookup"><span data-stu-id="a76f3-105">All you must do is grant that user enumeration privileges in the directory you're using for the partner org.</span></span> 

<span data-ttu-id="a76f3-106">Udělení těchto oprávnění, když:</span><span class="sxs-lookup"><span data-stu-id="a76f3-106">Grant these privileges when:</span></span>

1. <span data-ttu-id="a76f3-107">Uživatel v organizaci hostitele (například WoodGrove) vyzývá jeden uživatel od partnerské organizace (například Sam@litware.com) jako hosta.</span><span class="sxs-lookup"><span data-stu-id="a76f3-107">A user in the host organization (for example, WoodGrove) invites one user from the partner organization (for example, Sam@litware.com) as Guest.</span></span>
2. <span data-ttu-id="a76f3-108">Správce v organizaci hostitele nastavuje zásady, které umožňují Sam identifikovat a přidat další uživatele od organizace partnera (Litware).</span><span class="sxs-lookup"><span data-stu-id="a76f3-108">The admin in the host organization sets up policies that allow Sam to identify and add other users from the partner organization (Litware).</span></span>
3. <span data-ttu-id="a76f3-109">Nyní Sam můžete přidat další uživatele z Litware do adresáře WoodGrove, skupiny nebo aplikace bez nutnosti pozvánek k uplatnit.</span><span class="sxs-lookup"><span data-stu-id="a76f3-109">Now Sam can add other users from Litware to the WoodGrove directory, groups, or applications without needing invitations to be redeemed.</span></span> <span data-ttu-id="a76f3-110">Pokud Sam má odpovídající výčtu oprávnění v Litware, probíhá automaticky.</span><span class="sxs-lookup"><span data-stu-id="a76f3-110">If Sam has the appropriate enumeration privileges in Litware, it happens automatically.</span></span>

### <a name="next-steps"></a><span data-ttu-id="a76f3-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a76f3-111">Next steps</span></span>

<span data-ttu-id="a76f3-112">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="a76f3-112">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="a76f3-113">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="a76f3-113">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="a76f3-114">Jak Azure Active Directory správci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="a76f3-114">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="a76f3-115">Jak informační pracovníci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="a76f3-115">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="a76f3-116">Elementy e-mail pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="a76f3-116">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="a76f3-117">Uplatnění pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="a76f3-117">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="a76f3-118">Licencování Azure AD s B2B spolupráce</span><span class="sxs-lookup"><span data-stu-id="a76f3-118">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="a76f3-119">Řešení potíží s spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="a76f3-119">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="a76f3-120">Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)</span><span class="sxs-lookup"><span data-stu-id="a76f3-120">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="a76f3-121">Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="a76f3-121">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="a76f3-122">Vícefaktorové ověřování pro uživatele pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="a76f3-122">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="a76f3-123">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a76f3-123">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)