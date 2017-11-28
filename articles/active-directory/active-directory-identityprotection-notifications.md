---
title: "oznámení ochrany identit služby Active Directory aaaAzure | Microsoft Docs"
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
ms.openlocfilehash: 19e62374873f034591c658a779f97d935766c612
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a><span data-ttu-id="35295-104">Azure oznámení ochrany identit služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="35295-104">Azure Active Directory Identity Protection notifications</span></span>
<span data-ttu-id="35295-105">Azure AD Identity Protection odešle dva typy oznámení automatizované e-maily toohelp, které můžete spravovat uživatele rizika a rizikových událostí:</span><span class="sxs-lookup"><span data-stu-id="35295-105">Azure AD Identity Protection sends two types of automated notification emails toohelp you manage user risk and risk events:</span></span>

* <span data-ttu-id="35295-106">Uživatel, dojde k ohrožení výstrahy e-mailu</span><span class="sxs-lookup"><span data-stu-id="35295-106">User compromised alert email</span></span>
* <span data-ttu-id="35295-107">Týdenní digest e-mailu</span><span class="sxs-lookup"><span data-stu-id="35295-107">Weekly digest email</span></span>

## <a name="user-compromised-alert-email"></a><span data-ttu-id="35295-108">Uživatel, dojde k ohrožení výstrahy e-mailu</span><span class="sxs-lookup"><span data-stu-id="35295-108">User compromised alert email</span></span>
<span data-ttu-id="35295-109">Ohrožené e-mailových upozornění na uživatele se vygeneruje, když Azure AD Identity Protection identifikuje účet jako ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="35295-109">A user compromised email alert is generated when Azure AD Identity Protection identifies an account as compromised.</span></span> <span data-ttu-id="35295-110">e-mailu Hello zahrnuje odkaz toohello, které uživatelé označení příznakem pro sestavu riziko hello Identity Protection řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="35295-110">hello email includes a link toohello Users flagged for risk report in hello Identity Protection dashboard.</span></span> <span data-ttu-id="35295-111">Doporučujeme prozkoumat okamžitě oznámení o ohrožených účty.</span><span class="sxs-lookup"><span data-stu-id="35295-111">We recommend that you immediately investigate notifications of compromised accounts.</span></span>

## <a name="weekly-digest-email"></a><span data-ttu-id="35295-112">Týdenní digest e-mailu</span><span class="sxs-lookup"><span data-stu-id="35295-112">Weekly digest email</span></span>
<span data-ttu-id="35295-113">Hello týdenní ověřování algoritmem digest, e-mailu obsahuje souhrn nových událostí riziko.</span><span class="sxs-lookup"><span data-stu-id="35295-113">hello weekly digest email contains a summary of new risk events.</span></span><br>
<span data-ttu-id="35295-114">Obsahuje:</span><span class="sxs-lookup"><span data-stu-id="35295-114">It includes:</span></span>

* <span data-ttu-id="35295-115">Ohrožení uživatelé</span><span class="sxs-lookup"><span data-stu-id="35295-115">Users at risk</span></span>
* <span data-ttu-id="35295-116">Podezřelé aktivity</span><span class="sxs-lookup"><span data-stu-id="35295-116">Suspicious activities</span></span>
* <span data-ttu-id="35295-117">Zjištěné ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="35295-117">Detected vulnerabilities</span></span>
* <span data-ttu-id="35295-118">Odkazy toohello související sestavy v Identity Protection</span><span class="sxs-lookup"><span data-stu-id="35295-118">Links toohello related reports in Identity Protection</span></span>

<br><span data-ttu-id="35295-119">
![Náprava](./media/active-directory-identityprotection-notifications/400.png "nápravy")
</span><span class="sxs-lookup"><span data-stu-id="35295-119">
![Remediation](./media/active-directory-identityprotection-notifications/400.png "Remediation")
</span></span><br>

<span data-ttu-id="35295-120">Můžete přepnout odesílání týdenní ověřování algoritmem digest, e-mailu.</span><span class="sxs-lookup"><span data-stu-id="35295-120">You can switch sending a weekly digest email off.</span></span>
<br><br><span data-ttu-id="35295-121">
![Uživatel rizika](./media/active-directory-identityprotection-notifications/62.png "rizika uživatele")
</span><span class="sxs-lookup"><span data-stu-id="35295-121">
![User risks](./media/active-directory-identityprotection-notifications/62.png "User risks")
</span></span><br>

<span data-ttu-id="35295-122">**Dialogové okno související konfigurace hello tooopen**:</span><span class="sxs-lookup"><span data-stu-id="35295-122">**tooopen hello related configuration dialog**:</span></span>

1. <span data-ttu-id="35295-123">Na hello **Azure AD Identity Protection** okně klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="35295-123">On hello **Azure AD Identity Protection** blade, click **Settings**.</span></span>
   <br><br><span data-ttu-id="35295-124">
   ![Zásady uživatele riziko](./media/active-directory-identityprotection-notifications/401.png "riziko zásady uživatele")
   </span><span class="sxs-lookup"><span data-stu-id="35295-124">
![User risk policy](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
</span></span><br>
2. <span data-ttu-id="35295-125">V hello **Obecné** klikněte na tlačítko **oznámení**.</span><span class="sxs-lookup"><span data-stu-id="35295-125">In hello **General** section, click **Notifications**.</span></span>
   <br><br><span data-ttu-id="35295-126">
   ![Zásady uživatele riziko](./media/active-directory-identityprotection-notifications/405.png "riziko zásady uživatele")
   </span><span class="sxs-lookup"><span data-stu-id="35295-126">
![User risk policy](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
</span></span><br>

## <a name="see-also"></a><span data-ttu-id="35295-127">Viz také</span><span class="sxs-lookup"><span data-stu-id="35295-127">See also</span></span>
* [<span data-ttu-id="35295-128">Ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="35295-128">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)
