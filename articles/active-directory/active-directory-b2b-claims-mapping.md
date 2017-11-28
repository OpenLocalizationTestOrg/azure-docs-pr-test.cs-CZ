---
title: "mapování v Azure Active Directory deklarace identity uživatele spolupráce aaaB2B | Microsoft Docs"
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
ms.openlocfilehash: 9e26085e91a6004b2f11286ae9c1df133bd47341
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a><span data-ttu-id="1f7cb-103">Mapování v Azure Active Directory deklarace identity uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="1f7cb-103">B2B collaboration user claims mapping in Azure Active Directory</span></span>

<span data-ttu-id="1f7cb-104">Přizpůsobení hello deklarace identity vystavené v tokenu SAML hello uživatelům spolupráce B2B Azure podporuje služby Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1f7cb-104">Azure Active Directory (Azure AD) supports customizing hello claims issued in hello SAML token for B2B collaboration users.</span></span> <span data-ttu-id="1f7cb-105">Když se uživatel ověřuje toohello aplikace, Azure AD vydá token toohello aplikace SAML, který obsahuje informace (nebo deklarace identity) o hello uživateli, který jedinečně identifikuje je.</span><span class="sxs-lookup"><span data-stu-id="1f7cb-105">When a user authenticates toohello application, Azure AD issues a SAML token toohello app that contains information (or claims) about hello user that uniquely identifies them.</span></span> <span data-ttu-id="1f7cb-106">Ve výchozím nastavení jedná se o uživatelské jméno, e-mailová adresa, jméno a příjmení hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="1f7cb-106">By default, this includes hello user's user name, email address, first name, and last name.</span></span> <span data-ttu-id="1f7cb-107">Můžete zobrazit nebo upravit hello deklarace identity odeslat v hello aplikace toohello tokenu SAML kartě atributy hello.</span><span class="sxs-lookup"><span data-stu-id="1f7cb-107">You can view or edit hello claims sent in hello SAML token toohello application under hello Attributes tab.</span></span>

<span data-ttu-id="1f7cb-108">Existují dvě možné důvody, proč může být nutné tooedit hello deklarace identity vystavené v tokenu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="1f7cb-108">There are two possible reasons why you might need tooedit hello claims issued in hello SAML token.</span></span>

1. <span data-ttu-id="1f7cb-109">aplikace Hello byla zapsána toorequire jinou sadu deklarací identity identifikátory URI nebo hodnot deklarací identity</span><span class="sxs-lookup"><span data-stu-id="1f7cb-109">hello application has been written toorequire a different set of claim URIs or claim values</span></span>

2. <span data-ttu-id="1f7cb-110">Vaše aplikace vyžaduje hello NameIdentifier deklarace identity toobe něco jiného než hello hlavní uživatelské jméno uložené ve službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1f7cb-110">Your application requires hello NameIdentifier claim toobe something other than hello user principal name stored in Azure Active Directory.</span></span>

  ![Zobrazení deklarace identity v tokenu SAML](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

<span data-ttu-id="1f7cb-112">Informace o tom, jak tooadd a úpravy deklarací, projděte si tento článek na přizpůsobení deklarace identity, [přizpůsobení deklarace identity vystavené v tokenu hello SAML pro předběžně integrované aplikace v Azure Active Directory](develop/active-directory-saml-claims-customization.md).</span><span class="sxs-lookup"><span data-stu-id="1f7cb-112">For information on how tooadd and edit claims, check out this article on claims customization, [Customizing claims issued in hello SAML token for pre-integrated apps in Azure Active Directory](develop/active-directory-saml-claims-customization.md).</span></span> <span data-ttu-id="1f7cb-113">Pro spolupráci B2B uživatelů, mapování NameID a UPN mezi klienta nelze z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="1f7cb-113">For B2B collaboration users, mapping NameID and UPN cross-tenant are prevented for security reasons.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1f7cb-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f7cb-114">Next steps</span></span>

<span data-ttu-id="1f7cb-115">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="1f7cb-115">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="1f7cb-116">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="1f7cb-116">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="1f7cb-117">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="1f7cb-117">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="1f7cb-118">Přidání role uživatele tooa spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="1f7cb-118">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="1f7cb-119">Delegovat pozvánek B2bB spolupráce</span><span class="sxs-lookup"><span data-stu-id="1f7cb-119">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="1f7cb-120">Dynamické skupiny a spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="1f7cb-120">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="1f7cb-121">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1f7cb-121">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="1f7cb-122">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="1f7cb-122">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="1f7cb-123">Externí sdílení Office 365</span><span class="sxs-lookup"><span data-stu-id="1f7cb-123">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="1f7cb-124">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="1f7cb-124">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="1f7cb-125">Aktuální omezení spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="1f7cb-125">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
