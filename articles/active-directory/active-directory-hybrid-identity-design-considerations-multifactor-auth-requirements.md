---
title: "Azure Active Directory hybridní identity aspekty návrhu - stanovit požadavky na služby Multi-Factor authentication"
description: "Azure Active Directory pomocí podmíněného řízení přístupu, zkontroluje konkrétní podmínky, kterou vyberete při ověřování uživatele a před povolením přístupu k aplikaci. Po splnění těchto podmínek je uživatel ověřený a přistupovat k aplikaci."
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
ms.openlocfilehash: 5b3a8ce6e4203dfb3700f324e32687dd910118af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="4c911-104">Stanovení požadavků na službu Multi-Factor authentication pro vaše řešení hybridní identity</span><span class="sxs-lookup"><span data-stu-id="4c911-104">Determine multi-factor authentication requirements for your hybrid identity solution</span></span>
<span data-ttu-id="4c911-105">V tomto světě mobility s uživateli přístup k datům a aplikacím v cloudu a z jakéhokoli zařízení se stává zabezpečení tyto informace prvořadým.</span><span class="sxs-lookup"><span data-stu-id="4c911-105">In this world of mobility, with users accessing data and applications in the cloud and from any device, securing this information has become paramount.</span></span>  <span data-ttu-id="4c911-106">Každý den je nové titulku o porušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="4c911-106">Every day there is a new headline about a security breach.</span></span>  <span data-ttu-id="4c911-107">I když není zaručeno, proti takové narušení, služba Multi-Factor authentication, poskytuje další úroveň zabezpečení, aby se zabránilo tyto narušení.</span><span class="sxs-lookup"><span data-stu-id="4c911-107">Although, there is no guarantee against such breaches, multi-factor authentication, provides an additional layer of security to help prevent these breaches.</span></span>
<span data-ttu-id="4c911-108">Začít vyhodnocování požadavků organizace pro službu Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="4c911-108">Start by evaluating the organizations requirements for multi-factor authentication.</span></span> <span data-ttu-id="4c911-109">To znamená co je organizace pokusu o zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="4c911-109">That is, what is the organization trying to secure.</span></span>  <span data-ttu-id="4c911-110">Toto testování je důležité určit technické požadavky pro nastavení a povolení organizace uživatele u služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="4c911-110">This evaluation is important to define the technical requirements for setting up and enabling the organizations users for multi-factor authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="4c911-111">Pokud nejste obeznámeni s MFA a jakým způsobem, důrazně doporučujeme, abyste si přečetli článek [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) před pokračujte ve čtení v této části.</span><span class="sxs-lookup"><span data-stu-id="4c911-111">If you are not familiar with MFA and what it does, it is strongly recommended that you read the article [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) prior to continue reading this section.</span></span>
> 
> 

<span data-ttu-id="4c911-112">Nezapomeňte odpovědět následující:</span><span class="sxs-lookup"><span data-stu-id="4c911-112">Make sure to answer the following:</span></span>

* <span data-ttu-id="4c911-113">Vaše společnost pokouší zabezpečit aplikace společnosti Microsoft?</span><span class="sxs-lookup"><span data-stu-id="4c911-113">Is your company trying to secure Microsoft apps?</span></span> 
* <span data-ttu-id="4c911-114">Jak jsou publikovány těchto aplikací?</span><span class="sxs-lookup"><span data-stu-id="4c911-114">How these apps are published?</span></span>
* <span data-ttu-id="4c911-115">Poskytuje vaše společnost vzdáleným zaměstnancům přístup k místní aplikace?</span><span class="sxs-lookup"><span data-stu-id="4c911-115">Does your company provide remote access to allow employees to access on-premises apps?</span></span>

<span data-ttu-id="4c911-116">Pokud ano, jaké typy vzdáleného přístupu? Budete také muset vyhodnotit, kde budou umístěné uživatelů, kteří používají tyto aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c911-116">If yes, what type of remote access?You also need to evaluate where the users who are accessing these applications will be located.</span></span> <span data-ttu-id="4c911-117">Toto testování je jiný důležitým krokem při definování strategie správné služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="4c911-117">This evaluation is another important step to define the proper multi-factor authentication strategy.</span></span> <span data-ttu-id="4c911-118">Ujistěte se, že odpovězte si na následující otázky:</span><span class="sxs-lookup"><span data-stu-id="4c911-118">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="4c911-119">Kde budou uživatelé k nalezen?</span><span class="sxs-lookup"><span data-stu-id="4c911-119">Where are the users going to be located?</span></span>
* <span data-ttu-id="4c911-120">Může se být umístěna kdekoli?</span><span class="sxs-lookup"><span data-stu-id="4c911-120">Can they be located anywhere?</span></span>
* <span data-ttu-id="4c911-121">Vaše společnost chce vytvořit omezení podle umístění uživatele?</span><span class="sxs-lookup"><span data-stu-id="4c911-121">Does your company want to establish restrictions according to the user’s location?</span></span>

<span data-ttu-id="4c911-122">Jakmile porozumíte tyto požadavky, je potřeba také měli zvážit požadavky na uživatele služby Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="4c911-122">Once you understand these requirements, it is important to also evaluate the user’s requirements for multi-factor authentication.</span></span> <span data-ttu-id="4c911-123">Toto testování je důležité, protože ho definují požadavky na nasazení služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="4c911-123">This evaluation is important because it will define the requirements for rolling out multi-factor authentication.</span></span> <span data-ttu-id="4c911-124">Ujistěte se, že odpovězte si na následující otázky:</span><span class="sxs-lookup"><span data-stu-id="4c911-124">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="4c911-125">Uživatelé se seznámíte s službou Multi-Factor authentication?</span><span class="sxs-lookup"><span data-stu-id="4c911-125">Are the users familiar with multi-factor authentication?</span></span>
* <span data-ttu-id="4c911-126">Některé používá, se budou muset poskytnout další ověřování?</span><span class="sxs-lookup"><span data-stu-id="4c911-126">Will some uses be required to provide additional authentication?</span></span>  
  * <span data-ttu-id="4c911-127">Pokud ano, vždy, když pocházející z externí sítě nebo přístupem ke konkrétní aplikace, nebo v jiných podmínkách?</span><span class="sxs-lookup"><span data-stu-id="4c911-127">If yes, all the time, when coming from external networks, or accessing specific applications, or under other conditions?</span></span>
* <span data-ttu-id="4c911-128">Uživatelé budou vyžadovat školení o tom, jak nastavit a implementovat vícefaktorové ověřování?</span><span class="sxs-lookup"><span data-stu-id="4c911-128">Will the users require training on how to setup and implement multi-factor authentication?</span></span>
* <span data-ttu-id="4c911-129">Jaké jsou klíčové scénáře, které chce vaše společnost povolit službu Multi-Factor authentication pro své uživatele?</span><span class="sxs-lookup"><span data-stu-id="4c911-129">What are the key scenarios that your company wants to enable multi-factor authentication for their users?</span></span>

<span data-ttu-id="4c911-130">Po zodpovězení otázek předchozí, budete moct porozumění, zda jsou již implementovaná službu Multi-Factor authentication na místě.</span><span class="sxs-lookup"><span data-stu-id="4c911-130">After answering the previous questions, you will be able to understand if there are multi-factor authentication already implemented on-premises.</span></span> <span data-ttu-id="4c911-131">Toto testování je důležité určit technické požadavky pro nastavení a povolení organizace uživatele u služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="4c911-131">This evaluation is important to define the technical requirements for setting up and enabling the organizations users for multi-factor authentication.</span></span> <span data-ttu-id="4c911-132">Ujistěte se, že odpovězte si na následující otázky:</span><span class="sxs-lookup"><span data-stu-id="4c911-132">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="4c911-133">Potřebuje vaše společnost pro ochranu privilegovaných účtů s MFA?</span><span class="sxs-lookup"><span data-stu-id="4c911-133">Does your company need to protect privileged accounts with MFA?</span></span>
* <span data-ttu-id="4c911-134">Potřebuje vaše společnost povolit MFA pro určité aplikace kvůli dodržování předpisů?</span><span class="sxs-lookup"><span data-stu-id="4c911-134">Does your company need to enable MFA for certain application for compliance reasons?</span></span>
* <span data-ttu-id="4c911-135">Potřebuje vaše společnost povolit MFA pro všechny oprávněné uživatele těchto aplikací nebo pouze správci?</span><span class="sxs-lookup"><span data-stu-id="4c911-135">Does your company need to enable MFA for all eligible users of these application or only administrators?</span></span>
* <span data-ttu-id="4c911-136">Třeba máte vždy povolené ověřování MFA nebo pouze při přihlášení uživatele mimo podnikovou síť?</span><span class="sxs-lookup"><span data-stu-id="4c911-136">Do you need have MFA always enabled or only when the users are logged outside of your corporate network?</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c911-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4c911-137">Next steps</span></span>
[<span data-ttu-id="4c911-138">Definování strategie přijetí hybridní identity</span><span class="sxs-lookup"><span data-stu-id="4c911-138">Define a hybrid identity adoption strategy</span></span>](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a><span data-ttu-id="4c911-139">Viz také</span><span class="sxs-lookup"><span data-stu-id="4c911-139">See also</span></span>
[<span data-ttu-id="4c911-140">Přehled aspektů návrhu</span><span class="sxs-lookup"><span data-stu-id="4c911-140">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)

