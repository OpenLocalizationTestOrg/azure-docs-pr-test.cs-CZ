---
title: "Ochrany identit Azure Active Directory – nejčastější dotazy | Microsoft Docs"
description: "Časté otázky k Azure AD Identity Protection"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 781f868c87cea3cd790d89c61e26eecf352babea
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection-faq"></a><span data-ttu-id="75af9-103">Ochrany identit Azure Active Directory – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="75af9-103">Azure Active Directory Identity Protection FAQ</span></span>


## <a name="why-do-some-risk-events-have-closed-system-status"></a><span data-ttu-id="75af9-104">Proč některé události riziko mají stav "Uzavřeno (systém)"?</span><span class="sxs-lookup"><span data-stu-id="75af9-104">Why do some risk events have “Closed (system)” status?</span></span>

<span data-ttu-id="75af9-105">**Odpověď:** tyto jsou události, které Azure Active Directory Identity Protection zjištěny a později ukončeno, protože události byly nelze nadále považovat za rizikové.</span><span class="sxs-lookup"><span data-stu-id="75af9-105">**A:** These are events that Azure Active Directory Identity Protection detected and later closed because the events were no longer considered risky.</span></span> <span data-ttu-id="75af9-106">Tyto události není započítávat úroveň rizika uživatele.</span><span class="sxs-lookup"><span data-stu-id="75af9-106">These events do not count towards the user’s risk level.</span></span> 

---

## <a name="do-i-need-to-be-a-global-admin-to-use-identity-protection-in-the-azure-portal"></a><span data-ttu-id="75af9-107">Je nutné být globálním správcem pro použití ochrany identit na portálu Azure?</span><span class="sxs-lookup"><span data-stu-id="75af9-107">Do I need to be a global admin to use Identity Protection in the Azure portal?</span></span>
<span data-ttu-id="75af9-108">**Odpověď:** **ne**.</span><span class="sxs-lookup"><span data-stu-id="75af9-108">**A:** **No**.</span></span> <span data-ttu-id="75af9-109">Může být buď čtečku zabezpečení, správce zabezpečení nebo globální správce pro použití ochrany Identity.</span><span class="sxs-lookup"><span data-stu-id="75af9-109">You can either be a Security Reader, a Security Admin or a Global Admin to use Identity Protection.</span></span>

---

## <a name="how-do-i-get-identity-protection"></a><span data-ttu-id="75af9-110">Jak získám Identity Protection?</span><span class="sxs-lookup"><span data-stu-id="75af9-110">How do I get Identity Protection?</span></span>
<span data-ttu-id="75af9-111">**Odpověď:** najdete v části [Začínáme s Azure Active Directory Premium](active-directory-get-started-premium.md) pro odpověď na tuto otázku.</span><span class="sxs-lookup"><span data-stu-id="75af9-111">**A:** See [Getting started with Azure Active Directory Premium](active-directory-get-started-premium.md) for an answer to this question.</span></span>

---

## <a name="what-happens-when-your-azure-active-directory-premium-2-trial-expires"></a><span data-ttu-id="75af9-112">Co se stane, když vyprší platnost zkušební období služby Azure Active Directory Premium 2?</span><span class="sxs-lookup"><span data-stu-id="75af9-112">What happens when your Azure Active Directory Premium 2 trial expires?</span></span>

<span data-ttu-id="75af9-113">**Odpověď:** Pokud vypršela platnost vaší zkušební edice Azure Active Directory Premium 2 na vaší edicí Azure Active Directory volné, Basic nebo Premium 1, musíte povolit ochranu Identity, máte přístup k němu, přestože není v souladu s licencí.</span><span class="sxs-lookup"><span data-stu-id="75af9-113">**A:** If your Azure Active Directory Premium 2 trial edition has expired on your Azure Active Directory Free, Basic or Premium 1 edition, and you had Identity Protection enabled, you still have access to it even though you are not in compliance with licensing.</span></span>

---
