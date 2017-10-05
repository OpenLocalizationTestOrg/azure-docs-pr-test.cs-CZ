---
title: "Azure Active Directory Identity Protection oznámení | Microsoft Docs"
description: "Zjistěte, jak oznámení podporují vaše aktivity šetření."
services: active-directory
keywords: "ochrany identit Azure active directory, cloud app discovery,. Správa aplikací, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 079d16bbf75cd2b3b94269d684e1ae1a0e6aa967
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a><span data-ttu-id="75888-104">Azure oznámení ochrany identit služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="75888-104">Azure Active Directory Identity Protection notifications</span></span>
<span data-ttu-id="75888-105">Azure AD Identity Protection odešle dva typy automatizované oznámení e-mailů vám pomůžou spravovat riziko pro uživatele a rizikových událostí:</span><span class="sxs-lookup"><span data-stu-id="75888-105">Azure AD Identity Protection sends two types of automated notification emails to help you manage user risk and risk events:</span></span>

* <span data-ttu-id="75888-106">Uživatel, dojde k ohrožení výstrahy e-mailu</span><span class="sxs-lookup"><span data-stu-id="75888-106">User compromised alert email</span></span>
* <span data-ttu-id="75888-107">Týdenní digest e-mailu</span><span class="sxs-lookup"><span data-stu-id="75888-107">Weekly digest email</span></span>

## <a name="user-compromised-alert-email"></a><span data-ttu-id="75888-108">Uživatel, dojde k ohrožení výstrahy e-mailu</span><span class="sxs-lookup"><span data-stu-id="75888-108">User compromised alert email</span></span>
<span data-ttu-id="75888-109">Ohrožené e-mailových upozornění na uživatele se vygeneruje, když Azure AD Identity Protection identifikuje účet jako ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="75888-109">A user compromised email alert is generated when Azure AD Identity Protection identifies an account as compromised.</span></span> <span data-ttu-id="75888-110">E-mailu obsahuje odkaz na uživatelé označení příznakem pro sestavu riziko v řídicím panelu ochrany identit.</span><span class="sxs-lookup"><span data-stu-id="75888-110">The email includes a link to the Users flagged for risk report in the Identity Protection dashboard.</span></span> <span data-ttu-id="75888-111">Doporučujeme prozkoumat okamžitě oznámení o ohrožených účty.</span><span class="sxs-lookup"><span data-stu-id="75888-111">We recommend that you immediately investigate notifications of compromised accounts.</span></span>

## <a name="weekly-digest-email"></a><span data-ttu-id="75888-112">Týdenní digest e-mailu</span><span class="sxs-lookup"><span data-stu-id="75888-112">Weekly digest email</span></span>
<span data-ttu-id="75888-113">Týdenní digest e-mailu obsahuje souhrn nových událostí riziko.</span><span class="sxs-lookup"><span data-stu-id="75888-113">The weekly digest email contains a summary of new risk events.</span></span><br>
<span data-ttu-id="75888-114">Obsahuje:</span><span class="sxs-lookup"><span data-stu-id="75888-114">It includes:</span></span>

* <span data-ttu-id="75888-115">Ohrožení uživatelé</span><span class="sxs-lookup"><span data-stu-id="75888-115">Users at risk</span></span>
* <span data-ttu-id="75888-116">Podezřelé aktivity</span><span class="sxs-lookup"><span data-stu-id="75888-116">Suspicious activities</span></span>
* <span data-ttu-id="75888-117">Zjištěné ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="75888-117">Detected vulnerabilities</span></span>
* <span data-ttu-id="75888-118">Odkazy na související sestavy v Identity Protection</span><span class="sxs-lookup"><span data-stu-id="75888-118">Links to the related reports in Identity Protection</span></span>

<br><span data-ttu-id="75888-119">
![Náprava](./media/active-directory-identityprotection-notifications/400.png "nápravy")
</span><span class="sxs-lookup"><span data-stu-id="75888-119">
![Remediation](./media/active-directory-identityprotection-notifications/400.png "Remediation")
</span></span><br>

<span data-ttu-id="75888-120">Můžete přepnout odesílání týdenní ověřování algoritmem digest, e-mailu.</span><span class="sxs-lookup"><span data-stu-id="75888-120">You can switch sending a weekly digest email off.</span></span>
<br><br><span data-ttu-id="75888-121">
![Uživatel rizika](./media/active-directory-identityprotection-notifications/62.png "rizika uživatele")
</span><span class="sxs-lookup"><span data-stu-id="75888-121">
![User risks](./media/active-directory-identityprotection-notifications/62.png "User risks")
</span></span><br>

<span data-ttu-id="75888-122">**Tím otevřete dialogové okno Konfigurace související**:</span><span class="sxs-lookup"><span data-stu-id="75888-122">**To open the related configuration dialog**:</span></span>

1. <span data-ttu-id="75888-123">Na **Azure AD Identity Protection** okně klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="75888-123">On the **Azure AD Identity Protection** blade, click **Settings**.</span></span>
   <br><br><span data-ttu-id="75888-124">
   ![Zásady uživatele riziko](./media/active-directory-identityprotection-notifications/401.png "riziko zásady uživatele")
   </span><span class="sxs-lookup"><span data-stu-id="75888-124">
![User risk policy](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
</span></span><br>
2. <span data-ttu-id="75888-125">V **Obecné** klikněte na tlačítko **oznámení**.</span><span class="sxs-lookup"><span data-stu-id="75888-125">In the **General** section, click **Notifications**.</span></span>
   <br><br><span data-ttu-id="75888-126">
   ![Zásady uživatele riziko](./media/active-directory-identityprotection-notifications/405.png "riziko zásady uživatele")
   </span><span class="sxs-lookup"><span data-stu-id="75888-126">
![User risk policy](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
</span></span><br>

## <a name="see-also"></a><span data-ttu-id="75888-127">Viz také</span><span class="sxs-lookup"><span data-stu-id="75888-127">See also</span></span>
* [<span data-ttu-id="75888-128">Ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="75888-128">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)
