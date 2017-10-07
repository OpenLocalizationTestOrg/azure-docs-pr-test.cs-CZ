---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity - stanovit požadavky na služby Multi-Factor authentication"
description: "Pomocí podmíněného řízení přístupu Azure Active Directory kontroluje hello konkrétní podmínky, kterou vyberete při ověřování uživatele hello a před povolením přístupu toohello aplikace. Po splnění těchto podmínek hello uživatel ověří a povolená přístup toohello aplikace."
documentationcenter: 
services: active-directory
author: femila
manager: billmath
editor: 
ms.assetid: 9c59fda9-47d0-4c7e-b3e7-3575c29beabe
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 49fa7b43772fb3a2d6664747477c60a34cddde2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="9e8c2-104">Stanovení požadavků na službu Multi-Factor authentication pro vaše řešení hybridní identity</span><span class="sxs-lookup"><span data-stu-id="9e8c2-104">Determine multi-factor authentication requirements for your hybrid identity solution</span></span>
<span data-ttu-id="9e8c2-105">V tomto světě mobility s uživateli přístup k datům a aplikacím v hello cloudu a z jakéhokoli zařízení se stává zabezpečení tyto informace prvořadým.</span><span class="sxs-lookup"><span data-stu-id="9e8c2-105">In this world of mobility, with users accessing data and applications in hello cloud and from any device, securing this information has become paramount.</span></span>  <span data-ttu-id="9e8c2-106">Každý den je nové titulku o porušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9e8c2-106">Every day there is a new headline about a security breach.</span></span>  <span data-ttu-id="9e8c2-107">I když není zaručeno, proti takové narušení, služba Multi-Factor authentication, poskytuje další úroveň zabezpečení toohelp brání tyto narušení.</span><span class="sxs-lookup"><span data-stu-id="9e8c2-107">Although, there is no guarantee against such breaches, multi-factor authentication, provides an additional layer of security toohelp prevent these breaches.</span></span>
<span data-ttu-id="9e8c2-108">Začít vyhodnocování požadavků organizace hello u služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="9e8c2-108">Start by evaluating hello organizations requirements for multi-factor authentication.</span></span> <span data-ttu-id="9e8c2-109">To znamená co je snažíme toosecure hello organizace.</span><span class="sxs-lookup"><span data-stu-id="9e8c2-109">That is, what is hello organization trying toosecure.</span></span>  <span data-ttu-id="9e8c2-110">Toto testování je důležité toodefine hello technické požadavky pro nastavení a povolení hello organizace uživatele u služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="9e8c2-110">This evaluation is important toodefine hello technical requirements for setting up and enabling hello organizations users for multi-factor authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="9e8c2-111">Pokud nejste obeznámeni s MFA a jakým způsobem, důrazně doporučujeme, abyste si přečetli hello článku [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) předchozí toocontinue čtení v této části.</span><span class="sxs-lookup"><span data-stu-id="9e8c2-111">If you are not familiar with MFA and what it does, it is strongly recommended that you read hello article [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) prior toocontinue reading this section.</span></span>
> 
> 

<span data-ttu-id="9e8c2-112">Ujistěte se, že tooanswer hello následující:</span><span class="sxs-lookup"><span data-stu-id="9e8c2-112">Make sure tooanswer hello following:</span></span>

* <span data-ttu-id="9e8c2-113">Aplikace Microsoft toosecure zkouší vaší společnosti?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-113">Is your company trying toosecure Microsoft apps?</span></span> 
* <span data-ttu-id="9e8c2-114">Jak jsou publikovány těchto aplikací?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-114">How these apps are published?</span></span>
* <span data-ttu-id="9e8c2-115">Vaše společnost poskytuje vzdáleného přístupu tooallow zaměstnanci tooaccess místní aplikace?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-115">Does your company provide remote access tooallow employees tooaccess on-premises apps?</span></span>

<span data-ttu-id="9e8c2-116">Pokud ano, jaké typy vzdáleného přístupu? Budete také potřebovat tooevaluate, kde budou umístěné hello uživatelů, kteří používají tyto aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e8c2-116">If yes, what type of remote access?You also need tooevaluate where hello users who are accessing these applications will be located.</span></span> <span data-ttu-id="9e8c2-117">Další důležitý krok toodefine hello správné služby Multi-Factor authentication strategie je toto testování.</span><span class="sxs-lookup"><span data-stu-id="9e8c2-117">This evaluation is another important step toodefine hello proper multi-factor authentication strategy.</span></span> <span data-ttu-id="9e8c2-118">Ujistěte se, zda text hello tooanswer následující otázky:</span><span class="sxs-lookup"><span data-stu-id="9e8c2-118">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="9e8c2-119">Kde se nachází uživatelé hello budete toobe?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-119">Where are hello users going toobe located?</span></span>
* <span data-ttu-id="9e8c2-120">Může se být umístěna kdekoli?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-120">Can they be located anywhere?</span></span>
* <span data-ttu-id="9e8c2-121">Vaše společnost chcete tooestablish omezení podle umístění uživatele toohello?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-121">Does your company want tooestablish restrictions according toohello user’s location?</span></span>

<span data-ttu-id="9e8c2-122">Jakmile porozumíte tyto požadavky, je důležité vyhodnotit tooalso hello uživatelských požadavků u služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="9e8c2-122">Once you understand these requirements, it is important tooalso evaluate hello user’s requirements for multi-factor authentication.</span></span> <span data-ttu-id="9e8c2-123">Toto testování je důležité, protože ji definovat hello požadavky pro nasazení služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="9e8c2-123">This evaluation is important because it will define hello requirements for rolling out multi-factor authentication.</span></span> <span data-ttu-id="9e8c2-124">Ujistěte se, zda text hello tooanswer následující otázky:</span><span class="sxs-lookup"><span data-stu-id="9e8c2-124">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="9e8c2-125">Znáte hello uživatelů pomocí služby Multi-Factor authentication?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-125">Are hello users familiar with multi-factor authentication?</span></span>
* <span data-ttu-id="9e8c2-126">Budou některé používá požadované tooprovide další ověřování?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-126">Will some uses be required tooprovide additional authentication?</span></span>  
  * <span data-ttu-id="9e8c2-127">Pokud ano, všechny hello čas, při pocházející z externí sítě nebo přístupem ke konkrétní aplikace, nebo v jiných podmínkách?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-127">If yes, all hello time, when coming from external networks, or accessing specific applications, or under other conditions?</span></span>
* <span data-ttu-id="9e8c2-128">Budou uživatelé hello vyžadovat školení na postupy toosetup a implementace služby Multi-Factor authentication?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-128">Will hello users require training on how toosetup and implement multi-factor authentication?</span></span>
* <span data-ttu-id="9e8c2-129">Jaké jsou hello klíčových scénářů, že vaše společnost chce tooenable služby Multi-Factor authentication pro své uživatele?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-129">What are hello key scenarios that your company wants tooenable multi-factor authentication for their users?</span></span>

<span data-ttu-id="9e8c2-130">Po zodpovězení otázek předchozí hello, budete mít toounderstand Pokud jsou již implementovaná službu Multi-Factor authentication na místě.</span><span class="sxs-lookup"><span data-stu-id="9e8c2-130">After answering hello previous questions, you will be able toounderstand if there are multi-factor authentication already implemented on-premises.</span></span> <span data-ttu-id="9e8c2-131">Toto testování je důležité toodefine hello technické požadavky pro nastavení a povolení hello organizace uživatele u služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="9e8c2-131">This evaluation is important toodefine hello technical requirements for setting up and enabling hello organizations users for multi-factor authentication.</span></span> <span data-ttu-id="9e8c2-132">Ujistěte se, zda text hello tooanswer následující otázky:</span><span class="sxs-lookup"><span data-stu-id="9e8c2-132">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="9e8c2-133">Potřebuje vaše společnost tooprotect privilegované účty s MFA?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-133">Does your company need tooprotect privileged accounts with MFA?</span></span>
* <span data-ttu-id="9e8c2-134">Potřebuje vaše společnost tooenable MFA pro určité aplikace kvůli dodržování předpisů?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-134">Does your company need tooenable MFA for certain application for compliance reasons?</span></span>
* <span data-ttu-id="9e8c2-135">Potřebuje vaše společnost pro všechny oprávněné uživatele těchto aplikací nebo pouze správci tooenable MFA?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-135">Does your company need tooenable MFA for all eligible users of these application or only administrators?</span></span>
* <span data-ttu-id="9e8c2-136">Třeba máte vždy povolené ověřování MFA nebo pouze při protokolování hello uživatelé mimo podnikovou síť?</span><span class="sxs-lookup"><span data-stu-id="9e8c2-136">Do you need have MFA always enabled or only when hello users are logged outside of your corporate network?</span></span>

## <a name="next-steps"></a><span data-ttu-id="9e8c2-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9e8c2-137">Next steps</span></span>
[<span data-ttu-id="9e8c2-138">Definování strategie přijetí hybridní identity</span><span class="sxs-lookup"><span data-stu-id="9e8c2-138">Define a hybrid identity adoption strategy</span></span>](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a><span data-ttu-id="9e8c2-139">Viz také</span><span class="sxs-lookup"><span data-stu-id="9e8c2-139">See also</span></span>
[<span data-ttu-id="9e8c2-140">Přehled aspektů návrhu</span><span class="sxs-lookup"><span data-stu-id="9e8c2-140">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)

