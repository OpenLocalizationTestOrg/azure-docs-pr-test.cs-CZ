---
title: "Mapování v Azure Active Directory deklarace identity uživatele spolupráce B2B | Microsoft Docs"
description: "referenční dokumentace pro Azure Active Directory s B2B spolupráce mapování deklarace identity"
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
ms.openlocfilehash: 5f8559450b24effd40a38879aeae3a8dd03944a3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a><span data-ttu-id="d292e-103">Mapování v Azure Active Directory deklarace identity uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="d292e-103">B2B collaboration user claims mapping in Azure Active Directory</span></span>

<span data-ttu-id="d292e-104">Přizpůsobení deklarace identity vystavené v tokenu SAML pro uživatele spolupráce B2B Azure podporuje služby Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d292e-104">Azure Active Directory (Azure AD) supports customizing the claims issued in the SAML token for B2B collaboration users.</span></span> <span data-ttu-id="d292e-105">Když se uživatel přihlásí k aplikaci, Azure AD vydá SAML token na aplikaci, která obsahuje informace (nebo deklarace identity) o uživatele, který jednoznačně identifikuje je.</span><span class="sxs-lookup"><span data-stu-id="d292e-105">When a user authenticates to the application, Azure AD issues a SAML token to the app that contains information (or claims) about the user that uniquely identifies them.</span></span> <span data-ttu-id="d292e-106">Ve výchozím nastavení jedná se o uživatelské jméno, e-mailová adresa, jméno a příjmení uživatele.</span><span class="sxs-lookup"><span data-stu-id="d292e-106">By default, this includes the user's user name, email address, first name, and last name.</span></span> <span data-ttu-id="d292e-107">Můžete zobrazit nebo upravit deklarace identity odeslat v tokenu SAML, aby aplikace na kartě atributy.</span><span class="sxs-lookup"><span data-stu-id="d292e-107">You can view or edit the claims sent in the SAML token to the application under the Attributes tab.</span></span>

<span data-ttu-id="d292e-108">Existují dvě možné důvody, proč je potřeba upravit deklarace identity vystavené v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="d292e-108">There are two possible reasons why you might need to edit the claims issued in the SAML token.</span></span>

1. <span data-ttu-id="d292e-109">Aplikace byla zapsána na vyžadují jinou sadu deklarací identity identifikátory URI nebo hodnot deklarací identity</span><span class="sxs-lookup"><span data-stu-id="d292e-109">The application has been written to require a different set of claim URIs or claim values</span></span>

2. <span data-ttu-id="d292e-110">Vaše aplikace vyžaduje NameIdentifier deklarace identity na něco jiného než hlavní název uživatele uložené v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d292e-110">Your application requires the NameIdentifier claim to be something other than the user principal name stored in Azure Active Directory.</span></span>

  ![Zobrazení deklarace identity v tokenu SAML](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

<span data-ttu-id="d292e-112">Informace o tom, jak přidávat a upravovat deklarace identity, podívejte se na tento článek na přizpůsobení deklarace identity, [přizpůsobení deklarace identity vystavené v tokenu SAML pro předběžně integrované aplikace v Azure Active Directory](develop/active-directory-saml-claims-customization.md).</span><span class="sxs-lookup"><span data-stu-id="d292e-112">For information on how to add and edit claims, check out this article on claims customization, [Customizing claims issued in the SAML token for pre-integrated apps in Azure Active Directory](develop/active-directory-saml-claims-customization.md).</span></span> <span data-ttu-id="d292e-113">Pro spolupráci B2B uživatelů, mapování NameID a UPN mezi klienta nelze z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="d292e-113">For B2B collaboration users, mapping NameID and UPN cross-tenant are prevented for security reasons.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d292e-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d292e-114">Next steps</span></span>

<span data-ttu-id="d292e-115">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="d292e-115">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="d292e-116">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="d292e-116">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="d292e-117">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="d292e-117">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="d292e-118">Přidání uživatele spolupráce B2B k roli</span><span class="sxs-lookup"><span data-stu-id="d292e-118">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="d292e-119">Delegovat pozvánek B2bB spolupráce</span><span class="sxs-lookup"><span data-stu-id="d292e-119">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="d292e-120">Dynamické skupiny a spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="d292e-120">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="d292e-121">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d292e-121">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="d292e-122">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="d292e-122">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="d292e-123">Externí sdílení Office 365</span><span class="sxs-lookup"><span data-stu-id="d292e-123">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="d292e-124">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="d292e-124">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="d292e-125">Aktuální omezení spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="d292e-125">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
