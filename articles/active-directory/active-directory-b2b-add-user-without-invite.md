---
title: "aaaAdd B2B spolupráce uživatele tooAzure služby Active Directory bez pozvánku | Microsoft Docs"
description: "Můžete je nechat uživatele guest, přidejte další tooyour hosta uživatele Azure AD bez uplatňuje Pozvánka v Azure Active Directory s B2B spolupráce."
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
ms.openlocfilehash: 459d99b9f856a35973d1b2cbfabdc23fe40c8f44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a><span data-ttu-id="9709f-103">Přidat uživatele typu Host spolupráce B2B bez Pozvánka</span><span class="sxs-lookup"><span data-stu-id="9709f-103">Add B2B collaboration guest users without an invitation</span></span>

<span data-ttu-id="9709f-104">Můžete povolit uživateli, jako jsou uživatelé partnera reprezentativní, tooadd od tooyour organizace partnera hello bez nutnosti pozvánek toobe uplatněn.</span><span class="sxs-lookup"><span data-stu-id="9709f-104">You can allow a user, such as a partner representative, tooadd users from hello partner tooyour organization without needing invitations toobe redeemed.</span></span> <span data-ttu-id="9709f-105">Jediné, co musíte udělat je uživateli udělit oprávnění výčet v hello adresář, který používáte pro org. partnera hello</span><span class="sxs-lookup"><span data-stu-id="9709f-105">All you must do is grant that user enumeration privileges in hello directory you're using for hello partner org.</span></span> 

<span data-ttu-id="9709f-106">Udělení těchto oprávnění, když:</span><span class="sxs-lookup"><span data-stu-id="9709f-106">Grant these privileges when:</span></span>

1. <span data-ttu-id="9709f-107">Jeden uživatel od organizace partnera hello vyzývá uživatele v organizaci hello hostitele (například WoodGrove) (například Sam@litware.com) jako hosta.</span><span class="sxs-lookup"><span data-stu-id="9709f-107">A user in hello host organization (for example, WoodGrove) invites one user from hello partner organization (for example, Sam@litware.com) as Guest.</span></span>
2. <span data-ttu-id="9709f-108">Dobrý den, správce v organizaci hostitele hello nastavuje zásady, které umožňují Sam tooidentify a přidat další uživatele od organizace partnera hello (Litware).</span><span class="sxs-lookup"><span data-stu-id="9709f-108">hello admin in hello host organization sets up policies that allow Sam tooidentify and add other users from hello partner organization (Litware).</span></span>
3. <span data-ttu-id="9709f-109">Nyní můžete Sam přidat jiných uživatelů z adresáře WoodGrove toohello Litware, skupiny nebo aplikace bez nutnosti pozvánek toobe uplatněn.</span><span class="sxs-lookup"><span data-stu-id="9709f-109">Now Sam can add other users from Litware toohello WoodGrove directory, groups, or applications without needing invitations toobe redeemed.</span></span> <span data-ttu-id="9709f-110">Pokud Sam hello odpovídající výčtu oprávnění Litware, probíhá automaticky.</span><span class="sxs-lookup"><span data-stu-id="9709f-110">If Sam has hello appropriate enumeration privileges in Litware, it happens automatically.</span></span>

### <a name="next-steps"></a><span data-ttu-id="9709f-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9709f-111">Next steps</span></span>

<span data-ttu-id="9709f-112">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="9709f-112">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="9709f-113">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="9709f-113">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="9709f-114">Jak Azure Active Directory správci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="9709f-114">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="9709f-115">Jak informační pracovníci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="9709f-115">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="9709f-116">elementy Hello hello e-mailová pozvánka pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="9709f-116">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="9709f-117">Uplatnění pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="9709f-117">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="9709f-118">Licencování Azure AD s B2B spolupráce</span><span class="sxs-lookup"><span data-stu-id="9709f-118">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="9709f-119">Řešení potíží s spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="9709f-119">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="9709f-120">Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)</span><span class="sxs-lookup"><span data-stu-id="9709f-120">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="9709f-121">Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="9709f-121">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="9709f-122">Vícefaktorové ověřování pro uživatele pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="9709f-122">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="9709f-123">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9709f-123">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)