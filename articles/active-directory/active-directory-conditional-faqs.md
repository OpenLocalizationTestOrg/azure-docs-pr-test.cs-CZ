---
title: "aaaAzure podmíněného přístupu služby Active Directory nejčastější dotazy | Microsoft Docs"
description: "Získejte odpovědi toofrequently kladené dotazy týkající se podmíněného přístupu v Azure Active Directory."
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
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: d23acbb01217d7e9717d1a43de1b46a929404118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-conditional-access-faqs"></a><span data-ttu-id="b686b-103">Azure Active Directory podmíněného přístupu nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="b686b-103">Azure Active Directory conditional access FAQs</span></span>

## <a name="which-applications-work-with-conditional-access-policies"></a><span data-ttu-id="b686b-104">Aplikace, které podporují zásady podmíněného přístupu?</span><span class="sxs-lookup"><span data-stu-id="b686b-104">Which applications work with conditional access policies?</span></span>

<span data-ttu-id="b686b-105">Informace o aplikacích, které pracují se zásadami podmíněného přístupu najdete v tématu [aplikací a prohlížeče, které používají pravidla podmíněného přístupu v Azure Active Directory](active-directory-conditional-access-supported-apps.md).</span><span class="sxs-lookup"><span data-stu-id="b686b-105">For information about applications that work with conditional access policies, see [Applications and browsers that use conditional access rules in Azure Active Directory](active-directory-conditional-access-supported-apps.md).</span></span>

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a><span data-ttu-id="b686b-106">Prosazují zásady podmíněného přístupu pro spolupráci B2B a uživatele typu Host?</span><span class="sxs-lookup"><span data-stu-id="b686b-106">Are conditional access policies enforced for B2B collaboration and guest users?</span></span>

<span data-ttu-id="b686b-107">Zásady se vynucují pro business-to-business (B2B) spolupráce uživatele.</span><span class="sxs-lookup"><span data-stu-id="b686b-107">Policies are enforced for business-to-business (B2B) collaboration users.</span></span> <span data-ttu-id="b686b-108">V některých případech ale uživatel nemusí být možné toosatisfy požadavky zásad hello.</span><span class="sxs-lookup"><span data-stu-id="b686b-108">However, in some cases, a user might not be able toosatisfy hello policy requirements.</span></span> <span data-ttu-id="b686b-109">Například uživatel typu Host organizace nemusí podporovat službu Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="b686b-109">For example, a guest user's organization might not support multi-factor authentication.</span></span> 



## <a name="does-a-sharepoint-online-policy-also-apply-tooonedrive-for-business"></a><span data-ttu-id="b686b-110">Zásady Sharepointu Online také platí tooOneDrive pro firmy?</span><span class="sxs-lookup"><span data-stu-id="b686b-110">Does a SharePoint Online policy also apply tooOneDrive for Business?</span></span>

<span data-ttu-id="b686b-111">Ano.</span><span class="sxs-lookup"><span data-stu-id="b686b-111">Yes.</span></span> <span data-ttu-id="b686b-112">Zásady Sharepointu Online platí také tooOneDrive pro firmy.</span><span class="sxs-lookup"><span data-stu-id="b686b-112">A SharePoint Online policy also applies tooOneDrive for Business.</span></span>


## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a><span data-ttu-id="b686b-113">Proč nelze nastavit zásadu pro klientské aplikace, jako je Word nebo Outlook?</span><span class="sxs-lookup"><span data-stu-id="b686b-113">Why can’t I set a policy on client apps, like Word or Outlook?</span></span>

<span data-ttu-id="b686b-114">Požadavky pro přístup k službě nastavuje zásady podmíněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="b686b-114">A conditional access policy sets requirements for accessing a service.</span></span> <span data-ttu-id="b686b-115">Prosazuje se při ověřování toothat služby.</span><span class="sxs-lookup"><span data-stu-id="b686b-115">It's enforced when authentication toothat service occurs.</span></span> <span data-ttu-id="b686b-116">přímo na klientskou aplikaci nejsou nastavené zásady Hello.</span><span class="sxs-lookup"><span data-stu-id="b686b-116">hello policy is not set directly on a client application.</span></span> <span data-ttu-id="b686b-117">Místo toho se použije, když klient zavolá službu.</span><span class="sxs-lookup"><span data-stu-id="b686b-117">Instead, it is applied when a client calls a service.</span></span> <span data-ttu-id="b686b-118">Například zásadu nastavit u SharePoint platí tooclients volání služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="b686b-118">For example, a policy set on SharePoint applies tooclients calling SharePoint.</span></span> <span data-ttu-id="b686b-119">Zásadu nastavit u Exchange platí tooOutlook.</span><span class="sxs-lookup"><span data-stu-id="b686b-119">A policy set on Exchange applies tooOutlook.</span></span>

## <a name="does-a-conditional-access-policy-apply-tooservice-accounts"></a><span data-ttu-id="b686b-120">Platí zásady podmíněného přístupu tooservice účty?</span><span class="sxs-lookup"><span data-stu-id="b686b-120">Does a conditional access policy apply tooservice accounts?</span></span>

<span data-ttu-id="b686b-121">Zásady podmíněného přístupu použije tooall uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="b686b-121">Conditional access policies apply tooall user accounts.</span></span> <span data-ttu-id="b686b-122">To zahrnuje uživatelské účty, které se používají jako účty služeb.</span><span class="sxs-lookup"><span data-stu-id="b686b-122">This includes user accounts that are used as service accounts.</span></span> <span data-ttu-id="b686b-123">Účet služby, který spouští bezobslužné často nelze vyhovět požadavkům hello zásady podmíněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="b686b-123">Often, a service account that runs unattended can't satisfy hello requirements of a conditional access policy.</span></span> <span data-ttu-id="b686b-124">Služba Multi-Factor authentication může být například vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="b686b-124">For example, multi-factor authentication might be required.</span></span> <span data-ttu-id="b686b-125">Účty služby můžete vyloučit ze zásady pomocí nastavení zásad řízení podmíněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="b686b-125">Service accounts can be excluded from a policy by using conditional access policy management settings.</span></span> 

## <a name="are-graph-apis-available-for-configuring-conditional-access-policies"></a><span data-ttu-id="b686b-126">Rozhraní Graph API jsou k dispozici ke konfiguraci zásad podmíněného přístupu?</span><span class="sxs-lookup"><span data-stu-id="b686b-126">Are Graph APIs available for configuring conditional access policies?</span></span>

<span data-ttu-id="b686b-127">V současné době není.</span><span class="sxs-lookup"><span data-stu-id="b686b-127">Currently, no.</span></span> 

## <a name="what-is-hello-default-exclusion-policy-for-unsupported-device-platforms"></a><span data-ttu-id="b686b-128">Co je hello výchozí vyloučení zásady pro platformy nepodporované zařízení?</span><span class="sxs-lookup"><span data-stu-id="b686b-128">What is hello default exclusion policy for unsupported device platforms?</span></span>

<span data-ttu-id="b686b-129">V současné době se na uživatele se systémy iOS a zařízení se systémem Android selektivně vynucují zásady podmíněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="b686b-129">Currently, conditional access policies are selectively enforced on users of iOS and Android devices.</span></span> <span data-ttu-id="b686b-130">Aplikace na jiných platformách zařízení se ve výchozím nastavení, není vliv hello zásady podmíněného přístupu pro zařízení iOS a Android.</span><span class="sxs-lookup"><span data-stu-id="b686b-130">Applications on other device platforms are, by default, not affected by hello conditional access policy for iOS and Android devices.</span></span> <span data-ttu-id="b686b-131">Správce klienta můžete zvolit toooverride hello globální zásady toodisallow přístup toousers na platformách, které nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="b686b-131">A tenant admin can choose toooverride hello global policy toodisallow access toousers on platforms that are not supported.</span></span>


## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a><span data-ttu-id="b686b-132">Jak fungují zásady podmíněného přístupu pro Teams společnosti Microsoft?</span><span class="sxs-lookup"><span data-stu-id="b686b-132">How do conditional access policies work for Microsoft Teams?</span></span>  

<span data-ttu-id="b686b-133">Microsoft Teams velice spoléhá na Exchange Online a SharePoint Online pro základní produktivitu scénáře, jako je schůzek, kalendáře a sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="b686b-133">Microsoft Teams relies heavily on Exchange Online and SharePoint Online for core productivity scenarios, like meetings, calendars, and file sharing.</span></span> <span data-ttu-id="b686b-134">Zásady podmíněného přístupu, které jsou nastavené na těchto cloudových aplikací použít tooMicrosoft týmy, když se uživatel přihlásí.</span><span class="sxs-lookup"><span data-stu-id="b686b-134">Conditional access policies that are set for these cloud apps apply tooMicrosoft Teams when a user signs in.</span></span>

<span data-ttu-id="b686b-135">Teams společnosti Microsoft je podporováno také samostatně jako cloudové aplikace v Azure Active Directory zásady podmíněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="b686b-135">Microsoft Teams also is supported separately as a cloud app in Azure Active Directory conditional access policies.</span></span> <span data-ttu-id="b686b-136">Zásady certifikátu autority, které jsou nastavené pro cloudové aplikace použít tooMicrosoft týmy, když se uživatel přihlásí.</span><span class="sxs-lookup"><span data-stu-id="b686b-136">Certificate authority policies that are set for a cloud app apply tooMicrosoft Teams when a user signs in.</span></span>

<span data-ttu-id="b686b-137">Klienti plocha Teams společnosti Microsoft pro systém Windows a Mac podporují moderní ověřování.</span><span class="sxs-lookup"><span data-stu-id="b686b-137">Microsoft Teams desktop clients for Windows and Mac support modern authentication.</span></span> <span data-ttu-id="b686b-138">Moderní ověřování poskytuje přihlašování založené na hello Azure Active Directory Authentication Library (ADAL) tooMicrosoft Office klientské aplikace napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="b686b-138">Modern authentication brings sign-in based on hello Azure Active Directory Authentication Library (ADAL) tooMicrosoft Office client applications across platforms.</span></span> 
