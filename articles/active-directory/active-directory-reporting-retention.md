---
title: "Zásady uchování sestav Azure Active Directory | Microsoft Docs"
description: "Zásady uchovávání informací na data sestavy ve vašem Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: ffeba8a6c32a21c75af21f948bbd6ea88dd9278c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-report-retention-policies"></a><span data-ttu-id="968bb-103">Zásady uchování sestav Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="968bb-103">Azure Active Directory report retention policies</span></span>


<span data-ttu-id="968bb-104">Toto téma poskytuje odpovědi na nejčastější dotazy ve spojení s uchovávání dat pro sestavy jinou aktivitu v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="968bb-104">This topic provides you with answers to the most common questions in conjunction with the data retention for the different activity reports in Azure Active Directory.</span></span> 

<span data-ttu-id="968bb-105">**Otázka: jak můžete získat shromažďování dat aktivity spustit?**</span><span class="sxs-lookup"><span data-stu-id="968bb-105">**Q: How can you get the collection of activity data started?**</span></span>

<span data-ttu-id="968bb-106">**ODPOVĚĎ:**</span><span class="sxs-lookup"><span data-stu-id="968bb-106">**A:**</span></span>

| <span data-ttu-id="968bb-107">Edice Azure AD</span><span class="sxs-lookup"><span data-stu-id="968bb-107">Azure AD Edition</span></span> | <span data-ttu-id="968bb-108">Počáteční kolekce</span><span class="sxs-lookup"><span data-stu-id="968bb-108">Collection Start</span></span> |
| :--              | :--   |
| <span data-ttu-id="968bb-109">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="968bb-109">Azure AD Premium P1</span></span> <br /> <span data-ttu-id="968bb-110">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="968bb-110">Azure AD Premium P2</span></span> | <span data-ttu-id="968bb-111">Při registraci pro předplatné</span><span class="sxs-lookup"><span data-stu-id="968bb-111">When you sign-up for a subscription</span></span> |
| <span data-ttu-id="968bb-112">Azure AD Free</span><span class="sxs-lookup"><span data-stu-id="968bb-112">Azure AD Free</span></span> | <span data-ttu-id="968bb-113">Při prvním spuštění [okno Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) nebo použít [reporting rozhraní API](https://aka.ms/aadreports)</span><span class="sxs-lookup"><span data-stu-id="968bb-113">The first time you open the [Azure Active Directory blade](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) or use the [reporting APIs](https://aka.ms/aadreports)</span></span>  |

---
<span data-ttu-id="968bb-114">**Otázka: Pokud je vaše data aktivity k dispozici na portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="968bb-114">**Q: When is your activity data available in the Azure portal?**</span></span>

<span data-ttu-id="968bb-115">**ODPOVĚĎ:**</span><span class="sxs-lookup"><span data-stu-id="968bb-115">**A:**</span></span>

- <span data-ttu-id="968bb-116">**Okamžitě** – Pokud již pracujete s sestav na portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="968bb-116">**Immediately** - If you have already been working with reports in the Azure classic portal</span></span>
- <span data-ttu-id="968bb-117">**V rámci 2 hodiny** – Pokud jste nezapnuli generování sestav na portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="968bb-117">**Within 2 hours** - If you haven’t turned reporting on  in the Azure classic portal</span></span>

---
<span data-ttu-id="968bb-118">**Otázka: jak můžete získat kolekce signálů zabezpečení spustit?**</span><span class="sxs-lookup"><span data-stu-id="968bb-118">**Q: How can you get the collection of security signals started?**</span></span>  

<span data-ttu-id="968bb-119">**Odpověď:** signálů zabezpečení, se proces shromažďování spustí, když jste přihlásit k centru pro ochranu Identity použít.</span><span class="sxs-lookup"><span data-stu-id="968bb-119">**A:** For security signals, the collection process starts when you opt-in to use the Identity Protection Center.</span></span> 


---
<span data-ttu-id="968bb-120">**Otázka: jak dlouho shromážděná data ukládají?**</span><span class="sxs-lookup"><span data-stu-id="968bb-120">**Q: For how long is the collected data stored?**</span></span>

<span data-ttu-id="968bb-121">**ODPOVĚĎ:**</span><span class="sxs-lookup"><span data-stu-id="968bb-121">**A:**</span></span>

<span data-ttu-id="968bb-122">**Sestavy aktivit**</span><span class="sxs-lookup"><span data-stu-id="968bb-122">**Activity reports**</span></span>    

| <span data-ttu-id="968bb-123">Sestava</span><span class="sxs-lookup"><span data-stu-id="968bb-123">Report</span></span>                 | <span data-ttu-id="968bb-124">Azure AD Free</span><span class="sxs-lookup"><span data-stu-id="968bb-124">Azure AD Free</span></span> | <span data-ttu-id="968bb-125">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="968bb-125">Azure AD Premium P1</span></span> | <span data-ttu-id="968bb-126">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="968bb-126">Azure AD Premium P2</span></span> |
| :--                    | :--           | :--                 | :--                 |
| <span data-ttu-id="968bb-127">Audit adresáře</span><span class="sxs-lookup"><span data-stu-id="968bb-127">Directory Audit</span></span>        | <span data-ttu-id="968bb-128">7 dní</span><span class="sxs-lookup"><span data-stu-id="968bb-128">7 days</span></span>        | <span data-ttu-id="968bb-129">30 dní</span><span class="sxs-lookup"><span data-stu-id="968bb-129">30 days</span></span>             | <span data-ttu-id="968bb-130">30 dní</span><span class="sxs-lookup"><span data-stu-id="968bb-130">30 days</span></span>             |
| <span data-ttu-id="968bb-131">Přihlašovací aktivita</span><span class="sxs-lookup"><span data-stu-id="968bb-131">Sign-in Activity</span></span>       | <span data-ttu-id="968bb-132">7 dní</span><span class="sxs-lookup"><span data-stu-id="968bb-132">7 days</span></span>        | <span data-ttu-id="968bb-133">30 dní</span><span class="sxs-lookup"><span data-stu-id="968bb-133">30 days</span></span>             | <span data-ttu-id="968bb-134">30 dní</span><span class="sxs-lookup"><span data-stu-id="968bb-134">30 days</span></span>             |

<span data-ttu-id="968bb-135">**Signály zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="968bb-135">**Security Signals**</span></span>

| <span data-ttu-id="968bb-136">Sestava</span><span class="sxs-lookup"><span data-stu-id="968bb-136">Report</span></span>         | <span data-ttu-id="968bb-137">Azure AD Free</span><span class="sxs-lookup"><span data-stu-id="968bb-137">Azure AD Free</span></span> | <span data-ttu-id="968bb-138">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="968bb-138">Azure AD Premium P1</span></span> | <span data-ttu-id="968bb-139">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="968bb-139">Azure AD Premium P2</span></span> |
| :--            | :--           | :--                 | :--                 |
| <span data-ttu-id="968bb-140">Ohrožení uživatelé</span><span class="sxs-lookup"><span data-stu-id="968bb-140">Users at risk</span></span>  | <span data-ttu-id="968bb-141">7 dní</span><span class="sxs-lookup"><span data-stu-id="968bb-141">7 days</span></span>        | <span data-ttu-id="968bb-142">30 dní</span><span class="sxs-lookup"><span data-stu-id="968bb-142">30 days</span></span>             | <span data-ttu-id="968bb-143">90 dnů</span><span class="sxs-lookup"><span data-stu-id="968bb-143">90 days</span></span>             |
| <span data-ttu-id="968bb-144">Rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="968bb-144">Risky sign-ins</span></span> | <span data-ttu-id="968bb-145">7 dní</span><span class="sxs-lookup"><span data-stu-id="968bb-145">7 days</span></span>        | <span data-ttu-id="968bb-146">30 dní</span><span class="sxs-lookup"><span data-stu-id="968bb-146">30 days</span></span>             | <span data-ttu-id="968bb-147">90 dnů</span><span class="sxs-lookup"><span data-stu-id="968bb-147">90 days</span></span>             |

---