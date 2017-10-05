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
ms.openlocfilehash: 936134bddad19964f809a17f200ebbeed5aa853c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a><span data-ttu-id="c4a41-103">Resetování licenční požadavky pro hesla pomocí samoobslužné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c4a41-103">Licensing requirements for Azure AD self-service password reset</span></span>

<span data-ttu-id="c4a41-104">V pořadí pro resetování hesel služby Azure AD pro funkce které **musí mít alespoň jednu licenci přiřazené ve vaší organizaci**.</span><span class="sxs-lookup"><span data-stu-id="c4a41-104">In order for Azure AD Password Reset to function, you **must have at least one license assigned in your organization**.</span></span> <span data-ttu-id="c4a41-105">Nevynucovat jsme licencování na vlastní uživatelské prostředí resetování hesla na uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4a41-105">We do not enforce per-user licensing on the password reset experience.</span></span> <span data-ttu-id="c4a41-106">Chcete-li udržovat kompatibilitu s licenční smlouvou Microsoft, přiřadit licence pro všechny uživatele, které používají prémiových funkcí.</span><span class="sxs-lookup"><span data-stu-id="c4a41-106">To maintain compliance with your Microsoft licensing agreement, you need to assign licenses to any users that use premium features.</span></span>

* <span data-ttu-id="c4a41-107">**Jenom uživatelé v cloudu** -Office 365 (O365) žádné placené SKU nebo Azure AD Basic</span><span class="sxs-lookup"><span data-stu-id="c4a41-107">**Cloud only users** - Office 365 (O365) any paid SKU, or Azure AD Basic</span></span>
* <span data-ttu-id="c4a41-108">**Cloud** nebo **místních uživatelů** – Azure AD Premium P1 nebo P2, Enterprise Mobility + Security (EMS) nebo zabezpečení produktivní Enterprise (ZAD)</span><span class="sxs-lookup"><span data-stu-id="c4a41-108">**Cloud** and/or **on-premises users** - Azure AD Premium P1 or P2, Enterprise Mobility + Security (EMS), or Secure Productive Enterprise (SPE)</span></span>

## <a name="licenses-required-for-password-writeback"></a><span data-ttu-id="c4a41-109">Licence potřebné pro zpětný zápis hesla</span><span class="sxs-lookup"><span data-stu-id="c4a41-109">Licenses required for password writeback</span></span>

<span data-ttu-id="c4a41-110">Pokud chcete používat zpětný zápis hesla, musí mít jednu z následujících licencí přiřazených ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="c4a41-110">To use password writeback, you must have one of the following licenses assigned in your tenant.</span></span>

* <span data-ttu-id="c4a41-111">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="c4a41-111">Azure AD Premium P1</span></span>
* <span data-ttu-id="c4a41-112">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="c4a41-112">Azure AD Premium P2</span></span>
* <span data-ttu-id="c4a41-113">Enterprise Mobility + Security E3</span><span class="sxs-lookup"><span data-stu-id="c4a41-113">Enterprise Mobility + Security E3</span></span>
* <span data-ttu-id="c4a41-114">Enterprise Mobility + Security E5</span><span class="sxs-lookup"><span data-stu-id="c4a41-114">Enterprise Mobility + Security E5</span></span>
* <span data-ttu-id="c4a41-115">Secure Productive Enterprise E3</span><span class="sxs-lookup"><span data-stu-id="c4a41-115">Secure Productive Enterprise E3</span></span>
* <span data-ttu-id="c4a41-116">Secure Productive Enterprise E5</span><span class="sxs-lookup"><span data-stu-id="c4a41-116">Secure Productive Enterprise E5</span></span>

> [!NOTE]
> <span data-ttu-id="c4a41-117">Samostatné Office 365 licenční plány **nepodporují zpětný zápis hesla** a vyžaduje jedna z předchozí plány pro tato funkce fungovat.</span><span class="sxs-lookup"><span data-stu-id="c4a41-117">Standalone Office 365 licensing plans **do not support password writeback** and require one of the preceding plans for this functionality to work.</span></span>

<span data-ttu-id="c4a41-118">Další informace o licencování včetně náklady najdete na následujících stránkách</span><span class="sxs-lookup"><span data-stu-id="c4a41-118">Additional licensing info including costs can be found on the following pages</span></span>

* [<span data-ttu-id="c4a41-119">Azure Active Directory ceny lokality</span><span class="sxs-lookup"><span data-stu-id="c4a41-119">Azure Active Directory Pricing site</span></span>](https://azure.microsoft.com/pricing/details/active-directory/)
* [<span data-ttu-id="c4a41-120">Enterprise Mobility + Security</span><span class="sxs-lookup"><span data-stu-id="c4a41-120">Enterprise Mobility + Security</span></span>](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [<span data-ttu-id="c4a41-121">Zabezpečené produktivní Enterprise</span><span class="sxs-lookup"><span data-stu-id="c4a41-121">Secure Productive Enterprise</span></span>](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

## <a name="enable-group-or-user-based-licensing"></a><span data-ttu-id="c4a41-122">Povolit skupinu nebo uživatele na základě licencí</span><span class="sxs-lookup"><span data-stu-id="c4a41-122">Enable group or user-based licensing</span></span>

<span data-ttu-id="c4a41-123">Azure AD nyní podporuje na základě skupiny licencování povolení správci přiřadit hromadných licencí pro skupinu uživatelů, nikoli po jednom přiřazení.</span><span class="sxs-lookup"><span data-stu-id="c4a41-123">Azure AD now supports group-based licensing allowing administrators to assign licenses in bulk to a group of users, rather than assigning them one at a time.</span></span> [<span data-ttu-id="c4a41-124">Přiřadit zkontrolujte a vyřešte problémy s licencí</span><span class="sxs-lookup"><span data-stu-id="c4a41-124">Assign, verify, and resolve problems with licenses</span></span>](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

<span data-ttu-id="c4a41-125">Některé služby společnosti Microsoft nejsou k dispozici ve všech umístěních.</span><span class="sxs-lookup"><span data-stu-id="c4a41-125">Some Microsoft services are not available in all locations.</span></span> <span data-ttu-id="c4a41-126">Předtím, než je možné přiřadit licence pro uživatele, Správce musí určovat vlastnost "Umístění využití" na uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4a41-126">Before a license can be assigned to a user, the administrator must specify the “Usage location” property on the user.</span></span> <span data-ttu-id="c4a41-127">Přiřazení licencí, lze provést v rámci uživatele > Profil > v oddílu nastavení na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c4a41-127">Assignment of licenses can be done under User > Profile > Settings section in the Azure portal.</span></span> <span data-ttu-id="c4a41-128">**Při použití přiřazení skupiny licencí, zdědí všechny uživatele bez využití umístění zadané umístění adresáře.**</span><span class="sxs-lookup"><span data-stu-id="c4a41-128">**When using group license assignment, any users without a usage location specified inherit the location of the directory.**</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4a41-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4a41-129">Next steps</span></span>

<span data-ttu-id="c4a41-130">Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD</span><span class="sxs-lookup"><span data-stu-id="c4a41-130">The following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="c4a41-131">[**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c4a41-131">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="c4a41-132">[**Data**](active-directory-passwords-data.md) – Pochopte požadovaná data a jejich použití pro správu hesel.</span><span class="sxs-lookup"><span data-stu-id="c4a41-132">[**Data**](active-directory-passwords-data.md) - Understand the data that is required and how it is used for password management</span></span>
* <span data-ttu-id="c4a41-133">[**Uvedení**](active-directory-passwords-best-practices.md) – Naplánujte a nasaďte pro své uživatele samoobslužné resetování hesla pomocí zde uvedených pokynů.</span><span class="sxs-lookup"><span data-stu-id="c4a41-133">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR to your users using the guidance found here</span></span>
* <span data-ttu-id="c4a41-134">[**Přizpůsobení**](active-directory-passwords-customize.md) – Přizpůsobte vzhled a chování samoobslužného resetování hesla pro vaši společnost.</span><span class="sxs-lookup"><span data-stu-id="c4a41-134">[**Customize**](active-directory-passwords-customize.md) - Customize the look and feel of the SSPR experience for your company.</span></span>
* <span data-ttu-id="c4a41-135">[**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="c4a41-135">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="c4a41-136">[**Podrobné technické informace**](active-directory-passwords-how-it-works.md) – Nahlédněte za oponu a pochopte, jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="c4a41-136">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind the curtain to understand how it works</span></span>
* <span data-ttu-id="c4a41-137">[**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak?</span><span class="sxs-lookup"><span data-stu-id="c4a41-137">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="c4a41-138">Proč?</span><span class="sxs-lookup"><span data-stu-id="c4a41-138">Why?</span></span> <span data-ttu-id="c4a41-139">Co?</span><span class="sxs-lookup"><span data-stu-id="c4a41-139">What?</span></span> <span data-ttu-id="c4a41-140">Kde?</span><span class="sxs-lookup"><span data-stu-id="c4a41-140">Where?</span></span> <span data-ttu-id="c4a41-141">Kdo?</span><span class="sxs-lookup"><span data-stu-id="c4a41-141">Who?</span></span> <span data-ttu-id="c4a41-142">Kdy?</span><span class="sxs-lookup"><span data-stu-id="c4a41-142">When?</span></span> <span data-ttu-id="c4a41-143">– Odpovědi na otázky, na které jste se vždy chtěli zeptat.</span><span class="sxs-lookup"><span data-stu-id="c4a41-143">- Answers to questions you always wanted to ask</span></span>
* <span data-ttu-id="c4a41-144">[**Řešení potíží**](active-directory-passwords-troubleshoot.md) – Zjistěte, jak řešit běžné problémy, ke kterým dochází u samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="c4a41-144">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how to resolve common issues that we see with SSPR</span></span>

