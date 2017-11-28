---
title: "Správa licencí: Azure AD SSPR | Microsoft Docs"
description: "Azure AD samoobslužného resetování hesel licenční požadavky"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 9cecaaac429165346f7082f1965dc8a21063fe7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a><span data-ttu-id="7fb73-103">Resetování licenční požadavky pro hesla pomocí samoobslužné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7fb73-103">Licensing requirements for Azure AD self-service password reset</span></span>

<span data-ttu-id="7fb73-104">V pořadí pro resetování hesla v Azure AD toofunction jste **musí mít alespoň jednu licenci přiřazené ve vaší organizaci**.</span><span class="sxs-lookup"><span data-stu-id="7fb73-104">In order for Azure AD Password Reset toofunction, you **must have at least one license assigned in your organization**.</span></span> <span data-ttu-id="7fb73-105">Nevynucovat jsme licencování v prostředí resetování hesla hello jednotlivé uživatele.</span><span class="sxs-lookup"><span data-stu-id="7fb73-105">We do not enforce per-user licensing on hello password reset experience.</span></span> <span data-ttu-id="7fb73-106">toomaintain shody s multilicenční smlouvu vaší společnosti Microsoft, budete potřebovat tooassign licence tooany uživatelů, kteří používají prémiových funkcí.</span><span class="sxs-lookup"><span data-stu-id="7fb73-106">toomaintain compliance with your Microsoft licensing agreement, you need tooassign licenses tooany users that use premium features.</span></span>

* <span data-ttu-id="7fb73-107">**Jenom uživatelé v cloudu** -Office 365 (O365) žádné placené SKU nebo Azure AD Basic</span><span class="sxs-lookup"><span data-stu-id="7fb73-107">**Cloud only users** - Office 365 (O365) any paid SKU, or Azure AD Basic</span></span>
* <span data-ttu-id="7fb73-108">**Cloud** nebo **místních uživatelů** – Azure AD Premium P1 nebo P2, Enterprise Mobility + Security (EMS) nebo zabezpečení produktivní Enterprise (ZAD)</span><span class="sxs-lookup"><span data-stu-id="7fb73-108">**Cloud** and/or **on-premises users** - Azure AD Premium P1 or P2, Enterprise Mobility + Security (EMS), or Secure Productive Enterprise (SPE)</span></span>

## <a name="licenses-required-for-password-writeback"></a><span data-ttu-id="7fb73-109">Licence potřebné pro zpětný zápis hesla</span><span class="sxs-lookup"><span data-stu-id="7fb73-109">Licenses required for password writeback</span></span>

<span data-ttu-id="7fb73-110">zpětný zápis hesla toouse, musíte mít jednu z následujících přiřazené ve vašem klientovi licence hello.</span><span class="sxs-lookup"><span data-stu-id="7fb73-110">toouse password writeback, you must have one of hello following licenses assigned in your tenant.</span></span>

* <span data-ttu-id="7fb73-111">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="7fb73-111">Azure AD Premium P1</span></span>
* <span data-ttu-id="7fb73-112">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="7fb73-112">Azure AD Premium P2</span></span>
* <span data-ttu-id="7fb73-113">Enterprise Mobility + Security E3</span><span class="sxs-lookup"><span data-stu-id="7fb73-113">Enterprise Mobility + Security E3</span></span>
* <span data-ttu-id="7fb73-114">Enterprise Mobility + Security E5</span><span class="sxs-lookup"><span data-stu-id="7fb73-114">Enterprise Mobility + Security E5</span></span>
* <span data-ttu-id="7fb73-115">Secure Productive Enterprise E3</span><span class="sxs-lookup"><span data-stu-id="7fb73-115">Secure Productive Enterprise E3</span></span>
* <span data-ttu-id="7fb73-116">Secure Productive Enterprise E5</span><span class="sxs-lookup"><span data-stu-id="7fb73-116">Secure Productive Enterprise E5</span></span>

> [!NOTE]
> <span data-ttu-id="7fb73-117">Samostatné Office 365 licenční plány **nepodporují zpětný zápis hesla** a vyžaduje jedna z předchozích plány pro tuto funkci toowork hello.</span><span class="sxs-lookup"><span data-stu-id="7fb73-117">Standalone Office 365 licensing plans **do not support password writeback** and require one of hello preceding plans for this functionality toowork.</span></span>

<span data-ttu-id="7fb73-118">Další informace o licencování včetně náklady naleznete na následující stránky hello</span><span class="sxs-lookup"><span data-stu-id="7fb73-118">Additional licensing info including costs can be found on hello following pages</span></span>

* [<span data-ttu-id="7fb73-119">Azure Active Directory ceny lokality</span><span class="sxs-lookup"><span data-stu-id="7fb73-119">Azure Active Directory Pricing site</span></span>](https://azure.microsoft.com/pricing/details/active-directory/)
* [<span data-ttu-id="7fb73-120">Enterprise Mobility + Security</span><span class="sxs-lookup"><span data-stu-id="7fb73-120">Enterprise Mobility + Security</span></span>](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [<span data-ttu-id="7fb73-121">Zabezpečené produktivní Enterprise</span><span class="sxs-lookup"><span data-stu-id="7fb73-121">Secure Productive Enterprise</span></span>](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

## <a name="enable-group-or-user-based-licensing"></a><span data-ttu-id="7fb73-122">Povolit skupinu nebo uživatele na základě licencí</span><span class="sxs-lookup"><span data-stu-id="7fb73-122">Enable group or user-based licensing</span></span>

<span data-ttu-id="7fb73-123">Azure AD nyní podporuje na základě skupiny licencování povolení licence tooassign správci v hromadné tooa skupinu uživatelů, nikoli po jednom přiřazení.</span><span class="sxs-lookup"><span data-stu-id="7fb73-123">Azure AD now supports group-based licensing allowing administrators tooassign licenses in bulk tooa group of users, rather than assigning them one at a time.</span></span> [<span data-ttu-id="7fb73-124">Přiřadit zkontrolujte a vyřešte problémy s licencí</span><span class="sxs-lookup"><span data-stu-id="7fb73-124">Assign, verify, and resolve problems with licenses</span></span>](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

<span data-ttu-id="7fb73-125">Některé služby společnosti Microsoft nejsou k dispozici ve všech umístěních.</span><span class="sxs-lookup"><span data-stu-id="7fb73-125">Some Microsoft services are not available in all locations.</span></span> <span data-ttu-id="7fb73-126">Před tooa uživatele lze přiřadit licenci, musí správce hello určit vlastnost "Využití umístění" hello hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="7fb73-126">Before a license can be assigned tooa user, hello administrator must specify hello “Usage location” property on hello user.</span></span> <span data-ttu-id="7fb73-127">Přiřazení licencí, lze provést v rámci uživatele > Profil > část nastavení v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7fb73-127">Assignment of licenses can be done under User > Profile > Settings section in hello Azure portal.</span></span> <span data-ttu-id="7fb73-128">**Při použití přiřazení skupiny licencí, zdědí všechny uživatele bez zadané umístění využití hello umístění adresáře hello.**</span><span class="sxs-lookup"><span data-stu-id="7fb73-128">**When using group license assignment, any users without a usage location specified inherit hello location of hello directory.**</span></span>

## <a name="next-steps"></a><span data-ttu-id="7fb73-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7fb73-129">Next steps</span></span>

<span data-ttu-id="7fb73-130">Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7fb73-130">hello following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="7fb73-131">[**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7fb73-131">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="7fb73-132">[**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel</span><span class="sxs-lookup"><span data-stu-id="7fb73-132">[**Data**](active-directory-passwords-data.md) - Understand hello data that is required and how it is used for password management</span></span>
* <span data-ttu-id="7fb73-133">[**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden</span><span class="sxs-lookup"><span data-stu-id="7fb73-133">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR tooyour users using hello guidance found here</span></span>
* <span data-ttu-id="7fb73-134">[**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.</span><span class="sxs-lookup"><span data-stu-id="7fb73-134">[**Customize**](active-directory-passwords-customize.md) - Customize hello look and feel of hello SSPR experience for your company.</span></span>
* <span data-ttu-id="7fb73-135">[**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="7fb73-135">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="7fb73-136">[**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje</span><span class="sxs-lookup"><span data-stu-id="7fb73-136">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind hello curtain toounderstand how it works</span></span>
* <span data-ttu-id="7fb73-137">[**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak?</span><span class="sxs-lookup"><span data-stu-id="7fb73-137">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="7fb73-138">Proč?</span><span class="sxs-lookup"><span data-stu-id="7fb73-138">Why?</span></span> <span data-ttu-id="7fb73-139">Co?</span><span class="sxs-lookup"><span data-stu-id="7fb73-139">What?</span></span> <span data-ttu-id="7fb73-140">Kde?</span><span class="sxs-lookup"><span data-stu-id="7fb73-140">Where?</span></span> <span data-ttu-id="7fb73-141">Kdo?</span><span class="sxs-lookup"><span data-stu-id="7fb73-141">Who?</span></span> <span data-ttu-id="7fb73-142">Kdy?</span><span class="sxs-lookup"><span data-stu-id="7fb73-142">When?</span></span> <span data-ttu-id="7fb73-143">-Odpovědi tooquestions vždy chtěli tooask</span><span class="sxs-lookup"><span data-stu-id="7fb73-143">- Answers tooquestions you always wanted tooask</span></span>
* <span data-ttu-id="7fb73-144">[**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR</span><span class="sxs-lookup"><span data-stu-id="7fb73-144">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how tooresolve common issues that we see with SSPR</span></span>

