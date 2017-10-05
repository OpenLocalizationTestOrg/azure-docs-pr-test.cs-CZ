---
title: "Oznámení vytváření sestav Azure Active Directory"
description: "Jak používat Azure Active Directory, vytváření sestav oznámení pro podezřelé přihlášení in."
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
ms.openlocfilehash: f4632bd2af802b10c8c64972e8c605d7ad7c0eaf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-reporting-notifications"></a><span data-ttu-id="e46d8-103">Oznámení vytváření sestav Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e46d8-103">Azure Active Directory Reporting Notifications</span></span>
## <a name="what-reports-generate-email-notifications"></a><span data-ttu-id="e46d8-104">Jaké sestavy generovat e-mailových oznámení</span><span class="sxs-lookup"><span data-stu-id="e46d8-104">What reports generate email notifications</span></span>
<span data-ttu-id="e46d8-105">V tomto okamžiku pouze nestandardní přihlašovací aktivita sestavy aktivační události e-mailová oznámení.</span><span class="sxs-lookup"><span data-stu-id="e46d8-105">At this time, only the Irregular Sign in Activity report triggers email notifications.</span></span>

## <a name="what-is-an-irregular-sign-in"></a><span data-ttu-id="e46d8-106">Co je "nestandardní přihlašovací"?</span><span class="sxs-lookup"><span data-stu-id="e46d8-106">What is an "Irregular Sign in"?</span></span>
<span data-ttu-id="e46d8-107">Nestandardní přihlášení jsou ty, které byly identifikovány naše algoritmů strojového učení, na základě podmínku "neuskutečnitelná cesta" v kombinaci s neobvyklá přihlášení, umístění a zařízení.</span><span class="sxs-lookup"><span data-stu-id="e46d8-107">Irregular sign-ins are those that have been identified by our machine learning algorithms, on the basis of an "impossible travel" condition combined with an anomalous sign-in location and device.</span></span> <span data-ttu-id="e46d8-108">To může znamenat, že se hacker se se pokoušel přihlásit pomocí tohoto účtu.</span><span class="sxs-lookup"><span data-stu-id="e46d8-108">This may indicate that a hacker has been trying to sign in using this account.</span></span>

## <a name="who-receives-the-email-notifications"></a><span data-ttu-id="e46d8-109">Kdo obdrží e-mailová oznámení?</span><span class="sxs-lookup"><span data-stu-id="e46d8-109">Who receives the email notifications?</span></span>
<span data-ttu-id="e46d8-110">E-mail je odeslán do všichni globální správci, kteří mají přiřazený licenci služby Active Directory Premium.</span><span class="sxs-lookup"><span data-stu-id="e46d8-110">The email is sent to all global admins who have been assigned an Active Directory Premium license.</span></span> <span data-ttu-id="e46d8-111">Zajistěte, aby že se doručí, jsme poslat ho správcům alternativní e-mailovou adresu i.</span><span class="sxs-lookup"><span data-stu-id="e46d8-111">To ensure it is delivered, we send it to the admins Alternate Email Address as well.</span></span> <span data-ttu-id="e46d8-112">Správci by měla obsahovat aad-alerts-noreply@mail.windowsazure.com v jejich seznamu bezpečných odesílatelů, takže nemáte promeškají e-mailu.</span><span class="sxs-lookup"><span data-stu-id="e46d8-112">Admins should include aad-alerts-noreply@mail.windowsazure.com in their safe senders list so they don’t miss the email.</span></span>

## <a name="how-often-are-these-emails-sent"></a><span data-ttu-id="e46d8-113">Jak často jsou tyto e-maily odesílané?</span><span class="sxs-lookup"><span data-stu-id="e46d8-113">How often are these emails sent?</span></span>
<span data-ttu-id="e46d8-114">E-mail je odeslán, pokud 10 nové nestandardní přihlašovací aktivity, ke kterým došlo v posledních 30 dnů nebo od odeslání poslední e-mailu, podle toho, která je menší.</span><span class="sxs-lookup"><span data-stu-id="e46d8-114">The email is sent if 10 new irregular sign-in activities occur in the last 30 days, or since the last email was sent, whichever is less.</span></span>

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a><span data-ttu-id="e46d8-115">Přístupu zprávu podle e-mailu</span><span class="sxs-lookup"><span data-stu-id="e46d8-115">How do I access the report mentioned in the email?</span></span>
<span data-ttu-id="e46d8-116">Když kliknete na odkaz, budete přesměrováni na stránku sestavy v rámci portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="e46d8-116">When you click on the link, you will be redirected to the report page within the Azure classic portal.</span></span> <span data-ttu-id="e46d8-117">Chcete-li získat přístup k sestavě, musíte nastavit na obojí:</span><span class="sxs-lookup"><span data-stu-id="e46d8-117">In order to access the report, you need to be both:</span></span>

* <span data-ttu-id="e46d8-118">Na správce nebo spolusprávce předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="e46d8-118">An admin or co-admin of your Azure subscription</span></span>
* <span data-ttu-id="e46d8-119">Globální správce v adresáři a přiřadit licenci služby Active Directory Premium.</span><span class="sxs-lookup"><span data-stu-id="e46d8-119">A global administrator in the directory, and assigned an Active Directory Premium license.</span></span> <span data-ttu-id="e46d8-120">Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).</span><span class="sxs-lookup"><span data-stu-id="e46d8-120">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

## <a name="can-i-turn-off-these-emails"></a><span data-ttu-id="e46d8-121">Můžete vypnout těchto e-mailů?</span><span class="sxs-lookup"><span data-stu-id="e46d8-121">Can I turn off these emails?</span></span>
<span data-ttu-id="e46d8-122">Ano, chcete-li vypnout oznámení související s neobvyklých přihlášení v rámci portálu Azure classic, klikněte na tlačítko **konfigurace**a potom vyberte **zakázané** pod **oznámení** části.</span><span class="sxs-lookup"><span data-stu-id="e46d8-122">Yes, to turn off notifications related to anomalous sign-ins within the Azure classic portal, click **Configure**, and then select **Disabled** under the **Notifications** section.</span></span>

## <a name="whats-next"></a><span data-ttu-id="e46d8-123">Kam dál</span><span class="sxs-lookup"><span data-stu-id="e46d8-123">What's next</span></span>
* <span data-ttu-id="e46d8-124">Chcete zjistit, jaké sestavy zabezpečení, auditování a aktivity jsou k dispozici?</span><span class="sxs-lookup"><span data-stu-id="e46d8-124">Curious about what security, audit, and activity reports are available?</span></span> <span data-ttu-id="e46d8-125">Podívejte se na [zabezpečení Azure AD, auditování a protokolování aktivit](active-directory-view-access-usage-reports.md)</span><span class="sxs-lookup"><span data-stu-id="e46d8-125">Check out [Azure AD Security, Audit, and Activity Reports](active-directory-view-access-usage-reports.md)</span></span>
* [<span data-ttu-id="e46d8-126">Začínáme se službou Azure Active Directory Premium</span><span class="sxs-lookup"><span data-stu-id="e46d8-126">Getting started with Azure Active Directory Premium</span></span>](active-directory-get-started-premium.md)
* [<span data-ttu-id="e46d8-127">Přidání firemního brandingu na přihlašovací stránku a na stránku přístupového panelu</span><span class="sxs-lookup"><span data-stu-id="e46d8-127">Add company branding to your Sign In and Access Panel pages</span></span>](active-directory-add-company-branding.md)

