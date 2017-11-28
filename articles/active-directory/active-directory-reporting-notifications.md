---
title: "aaaAzure oznámení vytváření sestav Active Directory"
description: "Jak toouse hello oznámení vytváření sestav Azure Active Directory pro podezřelé přihlášení in."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ae6d4b0e-5931-4cb3-98bf-9be91b381c92
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.custom: oldportal
ms.reviewer: dhanyahk
ms.openlocfilehash: 3843c45eaf9d68e671943bfdbc7ab68933f38fbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-notifications"></a><span data-ttu-id="0937c-103">Oznámení vytváření sestav Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0937c-103">Azure Active Directory Reporting Notifications</span></span>
## <a name="what-reports-generate-email-notifications"></a><span data-ttu-id="0937c-104">Jaké sestavy generovat e-mailových oznámení</span><span class="sxs-lookup"><span data-stu-id="0937c-104">What reports generate email notifications</span></span>
<span data-ttu-id="0937c-105">V tomto okamžiku pouze hello nestandardní přihlašovací v aktivační události sestavy aktivity e-mailová oznámení.</span><span class="sxs-lookup"><span data-stu-id="0937c-105">At this time, only hello Irregular Sign in Activity report triggers email notifications.</span></span>

## <a name="what-is-an-irregular-sign-in"></a><span data-ttu-id="0937c-106">Co je "nestandardní přihlašovací"?</span><span class="sxs-lookup"><span data-stu-id="0937c-106">What is an "Irregular Sign in"?</span></span>
<span data-ttu-id="0937c-107">Nestandardní přihlášení jsou ty, které byly identifikovány naše algoritmů strojového učení, na základě hello podmínku "neuskutečnitelná cesta" v kombinaci s neobvyklá přihlášení, umístění a zařízení.</span><span class="sxs-lookup"><span data-stu-id="0937c-107">Irregular sign-ins are those that have been identified by our machine learning algorithms, on hello basis of an "impossible travel" condition combined with an anomalous sign-in location and device.</span></span> <span data-ttu-id="0937c-108">To může znamenat, že se hacker byla při toosign pomocí tohoto účtu.</span><span class="sxs-lookup"><span data-stu-id="0937c-108">This may indicate that a hacker has been trying toosign in using this account.</span></span>

## <a name="who-receives-hello-email-notifications"></a><span data-ttu-id="0937c-109">Kdo obdrží hello e-mailová oznámení?</span><span class="sxs-lookup"><span data-stu-id="0937c-109">Who receives hello email notifications?</span></span>
<span data-ttu-id="0937c-110">Hello e-mail je odeslán tooall globální správci, kteří mají přiřazený licenci služby Active Directory Premium.</span><span class="sxs-lookup"><span data-stu-id="0937c-110">hello email is sent tooall global admins who have been assigned an Active Directory Premium license.</span></span> <span data-ttu-id="0937c-111">tooensure, které se doručí, odešleme ho také toohello admins alternativní e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="0937c-111">tooensure it is delivered, we send it toohello admins Alternate Email Address as well.</span></span> <span data-ttu-id="0937c-112">Správci by měla obsahovat aad-alerts-noreply@mail.windowsazure.com v jejich seznamu bezpečných odesílatelů, nemusíte promeškají hello e-mailu.</span><span class="sxs-lookup"><span data-stu-id="0937c-112">Admins should include aad-alerts-noreply@mail.windowsazure.com in their safe senders list so they don’t miss hello email.</span></span>

## <a name="how-often-are-these-emails-sent"></a><span data-ttu-id="0937c-113">Jak často jsou tyto e-maily odesílané?</span><span class="sxs-lookup"><span data-stu-id="0937c-113">How often are these emails sent?</span></span>
<span data-ttu-id="0937c-114">Hello e-mail je odeslán, pokud dojít 10 nové nestandardní přihlašovací aktivity v hello posledních 30 dnů nebo od poslední e-mailu hello odeslání, podle toho, která je menší.</span><span class="sxs-lookup"><span data-stu-id="0937c-114">hello email is sent if 10 new irregular sign-in activities occur in hello last 30 days, or since hello last email was sent, whichever is less.</span></span>

## <a name="how-do-i-access-hello-report-mentioned-in-hello-email"></a><span data-ttu-id="0937c-115">Přístupu hello sestavy uvedených v e-mailu hello</span><span class="sxs-lookup"><span data-stu-id="0937c-115">How do I access hello report mentioned in hello email?</span></span>
<span data-ttu-id="0937c-116">Když kliknete na odkaz hello, bude stránka přesměrovaného toohello sestavy v rámci hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="0937c-116">When you click on hello link, you will be redirected toohello report page within hello Azure classic portal.</span></span> <span data-ttu-id="0937c-117">V pořadí tooaccess hello sestavy, musíte toobe obou:</span><span class="sxs-lookup"><span data-stu-id="0937c-117">In order tooaccess hello report, you need toobe both:</span></span>

* <span data-ttu-id="0937c-118">Na správce nebo spolusprávce předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="0937c-118">An admin or co-admin of your Azure subscription</span></span>
* <span data-ttu-id="0937c-119">Globální správce adresáře hello a přiřadit licenci služby Active Directory Premium.</span><span class="sxs-lookup"><span data-stu-id="0937c-119">A global administrator in hello directory, and assigned an Active Directory Premium license.</span></span> <span data-ttu-id="0937c-120">Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).</span><span class="sxs-lookup"><span data-stu-id="0937c-120">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

## <a name="can-i-turn-off-these-emails"></a><span data-ttu-id="0937c-121">Můžete vypnout těchto e-mailů?</span><span class="sxs-lookup"><span data-stu-id="0937c-121">Can I turn off these emails?</span></span>
<span data-ttu-id="0937c-122">Ano, tooturn zakázat upozornění související s tooanomalous přihlášení v rámci hello portál Azure classic, klikněte na tlačítko **konfigurace**a potom vyberte **zakázané** pod hello **oznámení**části.</span><span class="sxs-lookup"><span data-stu-id="0937c-122">Yes, tooturn off notifications related tooanomalous sign-ins within hello Azure classic portal, click **Configure**, and then select **Disabled** under hello **Notifications** section.</span></span>

## <a name="whats-next"></a><span data-ttu-id="0937c-123">Kam dál</span><span class="sxs-lookup"><span data-stu-id="0937c-123">What's next</span></span>
* <span data-ttu-id="0937c-124">Chcete zjistit, jaké sestavy zabezpečení, auditování a aktivity jsou k dispozici?</span><span class="sxs-lookup"><span data-stu-id="0937c-124">Curious about what security, audit, and activity reports are available?</span></span> <span data-ttu-id="0937c-125">Podívejte se na [zabezpečení Azure AD, auditování a protokolování aktivit](active-directory-view-access-usage-reports.md)</span><span class="sxs-lookup"><span data-stu-id="0937c-125">Check out [Azure AD Security, Audit, and Activity Reports](active-directory-view-access-usage-reports.md)</span></span>
* [<span data-ttu-id="0937c-126">Začínáme se službou Azure Active Directory Premium</span><span class="sxs-lookup"><span data-stu-id="0937c-126">Getting started with Azure Active Directory Premium</span></span>](active-directory-get-started-premium.md)
* [<span data-ttu-id="0937c-127">Přidání firemního brandingu tooyour stránky přihlásit a přístupového panelu</span><span class="sxs-lookup"><span data-stu-id="0937c-127">Add company branding tooyour Sign In and Access Panel pages</span></span>](active-directory-add-company-branding.md)

