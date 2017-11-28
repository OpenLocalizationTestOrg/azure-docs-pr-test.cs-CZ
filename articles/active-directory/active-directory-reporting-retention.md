---
title: "Zásady uchování sestav služby Active Directory aaaAzure | Microsoft Docs"
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
ms.openlocfilehash: c46a68198cb34e9c92662b2f8461010745392c04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-report-retention-policies"></a><span data-ttu-id="750ce-103">Zásady uchování sestav Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="750ce-103">Azure Active Directory report retention policies</span></span>


<span data-ttu-id="750ce-104">Toto téma vám poskytne odpovědi toohello nejčastější dotazy ve spojení s hello uchovávání dat pro sestavy hello jiné aktivitě v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="750ce-104">This topic provides you with answers toohello most common questions in conjunction with hello data retention for hello different activity reports in Azure Active Directory.</span></span> 

<span data-ttu-id="750ce-105">**Otázka: jak můžete získat hello shromažďování dat aktivity spustit?**</span><span class="sxs-lookup"><span data-stu-id="750ce-105">**Q: How can you get hello collection of activity data started?**</span></span>

<span data-ttu-id="750ce-106">**ODPOVĚĎ:**</span><span class="sxs-lookup"><span data-stu-id="750ce-106">**A:**</span></span>

| <span data-ttu-id="750ce-107">Edice Azure AD</span><span class="sxs-lookup"><span data-stu-id="750ce-107">Azure AD Edition</span></span> | <span data-ttu-id="750ce-108">Počáteční kolekce</span><span class="sxs-lookup"><span data-stu-id="750ce-108">Collection Start</span></span> |
| :--              | :--   |
| <span data-ttu-id="750ce-109">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="750ce-109">Azure AD Premium P1</span></span> <br /> <span data-ttu-id="750ce-110">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="750ce-110">Azure AD Premium P2</span></span> | <span data-ttu-id="750ce-111">Při registraci pro předplatné</span><span class="sxs-lookup"><span data-stu-id="750ce-111">When you sign-up for a subscription</span></span> |
| <span data-ttu-id="750ce-112">Azure AD Free</span><span class="sxs-lookup"><span data-stu-id="750ce-112">Azure AD Free</span></span> | <span data-ttu-id="750ce-113">Při prvním spuštění hello Hello [okno Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) nebo použijte hello [reporting rozhraní API](https://aka.ms/aadreports)</span><span class="sxs-lookup"><span data-stu-id="750ce-113">hello first time you open hello [Azure Active Directory blade](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) or use hello [reporting APIs](https://aka.ms/aadreports)</span></span>  |

---
<span data-ttu-id="750ce-114">**Otázka: Pokud je vaše data aktivity k dispozici v hello portál Azure?**</span><span class="sxs-lookup"><span data-stu-id="750ce-114">**Q: When is your activity data available in hello Azure portal?**</span></span>

<span data-ttu-id="750ce-115">**ODPOVĚĎ:**</span><span class="sxs-lookup"><span data-stu-id="750ce-115">**A:**</span></span>

- <span data-ttu-id="750ce-116">**Okamžitě** – Pokud již pracujete se sestavami v hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="750ce-116">**Immediately** - If you have already been working with reports in hello Azure classic portal</span></span>
- <span data-ttu-id="750ce-117">**V rámci 2 hodiny** – Pokud jste nezapnuli vytváření sestav v hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="750ce-117">**Within 2 hours** - If you haven’t turned reporting on  in hello Azure classic portal</span></span>

---
<span data-ttu-id="750ce-118">**Otázka: jak můžete získat hello kolekce signálů zabezpečení spustit?**</span><span class="sxs-lookup"><span data-stu-id="750ce-118">**Q: How can you get hello collection of security signals started?**</span></span>  

<span data-ttu-id="750ce-119">**Odpověď:** signálů zabezpečení, se proces shromažďování hello spustí, když jste souhlas toouse hello Identity Protection Center.</span><span class="sxs-lookup"><span data-stu-id="750ce-119">**A:** For security signals, hello collection process starts when you opt-in toouse hello Identity Protection Center.</span></span> 


---
<span data-ttu-id="750ce-120">**Otázka: jak dlouho hello shromážděná data ukládají?**</span><span class="sxs-lookup"><span data-stu-id="750ce-120">**Q: For how long is hello collected data stored?**</span></span>

<span data-ttu-id="750ce-121">**ODPOVĚĎ:**</span><span class="sxs-lookup"><span data-stu-id="750ce-121">**A:**</span></span>

<span data-ttu-id="750ce-122">**Sestavy aktivit**</span><span class="sxs-lookup"><span data-stu-id="750ce-122">**Activity reports**</span></span>    

| <span data-ttu-id="750ce-123">Sestava</span><span class="sxs-lookup"><span data-stu-id="750ce-123">Report</span></span>                 | <span data-ttu-id="750ce-124">Azure AD Free</span><span class="sxs-lookup"><span data-stu-id="750ce-124">Azure AD Free</span></span> | <span data-ttu-id="750ce-125">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="750ce-125">Azure AD Premium P1</span></span> | <span data-ttu-id="750ce-126">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="750ce-126">Azure AD Premium P2</span></span> |
| :--                    | :--           | :--                 | :--                 |
| <span data-ttu-id="750ce-127">Audit adresáře</span><span class="sxs-lookup"><span data-stu-id="750ce-127">Directory Audit</span></span>        | <span data-ttu-id="750ce-128">7 dní</span><span class="sxs-lookup"><span data-stu-id="750ce-128">7 days</span></span>        | <span data-ttu-id="750ce-129">30 dní</span><span class="sxs-lookup"><span data-stu-id="750ce-129">30 days</span></span>             | <span data-ttu-id="750ce-130">30 dní</span><span class="sxs-lookup"><span data-stu-id="750ce-130">30 days</span></span>             |
| <span data-ttu-id="750ce-131">Přihlašovací aktivita</span><span class="sxs-lookup"><span data-stu-id="750ce-131">Sign-in Activity</span></span>       | <span data-ttu-id="750ce-132">7 dní</span><span class="sxs-lookup"><span data-stu-id="750ce-132">7 days</span></span>        | <span data-ttu-id="750ce-133">30 dní</span><span class="sxs-lookup"><span data-stu-id="750ce-133">30 days</span></span>             | <span data-ttu-id="750ce-134">30 dní</span><span class="sxs-lookup"><span data-stu-id="750ce-134">30 days</span></span>             |

<span data-ttu-id="750ce-135">**Signály zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="750ce-135">**Security Signals**</span></span>

| <span data-ttu-id="750ce-136">Sestava</span><span class="sxs-lookup"><span data-stu-id="750ce-136">Report</span></span>         | <span data-ttu-id="750ce-137">Azure AD Free</span><span class="sxs-lookup"><span data-stu-id="750ce-137">Azure AD Free</span></span> | <span data-ttu-id="750ce-138">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="750ce-138">Azure AD Premium P1</span></span> | <span data-ttu-id="750ce-139">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="750ce-139">Azure AD Premium P2</span></span> |
| :--            | :--           | :--                 | :--                 |
| <span data-ttu-id="750ce-140">Ohrožení uživatelé</span><span class="sxs-lookup"><span data-stu-id="750ce-140">Users at risk</span></span>  | <span data-ttu-id="750ce-141">7 dní</span><span class="sxs-lookup"><span data-stu-id="750ce-141">7 days</span></span>        | <span data-ttu-id="750ce-142">30 dní</span><span class="sxs-lookup"><span data-stu-id="750ce-142">30 days</span></span>             | <span data-ttu-id="750ce-143">90 dnů</span><span class="sxs-lookup"><span data-stu-id="750ce-143">90 days</span></span>             |
| <span data-ttu-id="750ce-144">Rizikové přihlášení</span><span class="sxs-lookup"><span data-stu-id="750ce-144">Risky sign-ins</span></span> | <span data-ttu-id="750ce-145">7 dní</span><span class="sxs-lookup"><span data-stu-id="750ce-145">7 days</span></span>        | <span data-ttu-id="750ce-146">30 dní</span><span class="sxs-lookup"><span data-stu-id="750ce-146">30 days</span></span>             | <span data-ttu-id="750ce-147">90 dnů</span><span class="sxs-lookup"><span data-stu-id="750ce-147">90 days</span></span>             |

---