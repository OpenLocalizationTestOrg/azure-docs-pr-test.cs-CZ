---
title: "prvky aaaThe e-mailová pozvánka hello Azure Active Directory s B2B spolupráce | Microsoft Docs"
description: "Azure Active Directory s B2B spolupráce pozvánku e-mailové šablony"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: f4908014d71a63442bbdca2182f54c7a79675a82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hello-elements-of-hello-b2b-collaboration-invitation-email"></a><span data-ttu-id="638b0-103">elementy Hello hello e-mailová pozvánka pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="638b0-103">hello elements of hello B2B collaboration invitation email</span></span>

<span data-ttu-id="638b0-104">E-mailů pozvánku jsou partnery toobring zásadní součástí na palubě jako uživatelé spolupráce B2B ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="638b0-104">Invitation emails are a critical component toobring partners on board as B2B collaboration users in Azure AD.</span></span> <span data-ttu-id="638b0-105">Můžete je používat důvěryhodnosti tooincrease hello příjemce.</span><span class="sxs-lookup"><span data-stu-id="638b0-105">You can use them tooincrease hello recipient's trust.</span></span> <span data-ttu-id="638b0-106">můžete přidat legitimitu a sociálních doklad toohello e-mailu, zda text hello příjemce toomake funguje celý výběr hello **Začínáme** tlačítko tooaccept hello pozvánku.</span><span class="sxs-lookup"><span data-stu-id="638b0-106">you can add legitimacy and social proof toohello email, toomake sure hello recipient feels comfortable with selecting hello **Get Started** button tooaccept hello invitation.</span></span> <span data-ttu-id="638b0-107">Tento vztah důvěryhodnosti je, že klíč znamená tooreduce sdílení tření.</span><span class="sxs-lookup"><span data-stu-id="638b0-107">This trust is a key means tooreduce sharing friction.</span></span> <span data-ttu-id="638b0-108">A je také potřeba toomake hello e-mailu vzhledu skvělé!</span><span class="sxs-lookup"><span data-stu-id="638b0-108">And you also want toomake hello email look great!</span></span>

![E-mailová pozvánka Azure AD s B2b](media/active-directory-b2b-invitation-email/invitation-email.png)

## <a name="explaining-hello-email"></a><span data-ttu-id="638b0-110">Vysvětlení hello e-mailu</span><span class="sxs-lookup"><span data-stu-id="638b0-110">Explaining hello email</span></span>
<span data-ttu-id="638b0-111">Podívejme na několik elementy hello e-mailů, abyste věděli, jak nejlepší toouse jejich možnosti.</span><span class="sxs-lookup"><span data-stu-id="638b0-111">Let's look at a few elements of hello email so you know how best toouse their capabilities.</span></span>

### <a name="subject"></a><span data-ttu-id="638b0-112">Předmět</span><span class="sxs-lookup"><span data-stu-id="638b0-112">Subject</span></span>
<span data-ttu-id="638b0-113">Hello předmět e-mailu hello následuje hello následující vzor: přijměte naše pozvání toohello &lt;tenantname&gt; organizace</span><span class="sxs-lookup"><span data-stu-id="638b0-113">hello subject of hello email follows hello following pattern: You're invited toohello &lt;tenantname&gt; organization</span></span>

### <a name="from-address"></a><span data-ttu-id="638b0-114">Z adresy</span><span class="sxs-lookup"><span data-stu-id="638b0-114">From address</span></span>
<span data-ttu-id="638b0-115">Používáme LinkedIn jako vzor pro hello z adresy.</span><span class="sxs-lookup"><span data-stu-id="638b0-115">We use a LinkedIn-like pattern for hello From address.</span></span>  <span data-ttu-id="638b0-116">Měli byste být zrušte kdo je hello pozvánky a ze společnosti a také vysvětlení, že e-mailové hello pochází z e-mailovou adresu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="638b0-116">You should be clear who hello inviter is and from which company, and also clarify that hello email is coming from a Microsoft email address.</span></span> <span data-ttu-id="638b0-117">Formát Hello: &lt;zobrazovaný název pozvánky&gt; z &lt;tenantname&gt; (přes Microsoft) <invites@microsoft.com&gt;</span><span class="sxs-lookup"><span data-stu-id="638b0-117">hello format is: &lt;Display name of inviter&gt; from &lt;tenantname&gt; (via Microsoft) <invites@microsoft.com&gt;</span></span>

### <a name="reply-to"></a><span data-ttu-id="638b0-118">Odpovědět</span><span class="sxs-lookup"><span data-stu-id="638b0-118">Reply To</span></span>
<span data-ttu-id="638b0-119">Hello odpovědi tooemail nastavena e-mailu pozval toohello vás, pokud je k dispozici, tak, aby odeslání odpovědi, že odešle e-mailu toohello back toohello pozvánky e-mailu.</span><span class="sxs-lookup"><span data-stu-id="638b0-119">hello reply-tooemail is set toohello inviter's email when available, so that replying toohello email sends an email back toohello inviter.</span></span>

### <a name="branding"></a><span data-ttu-id="638b0-120">Branding</span><span class="sxs-lookup"><span data-stu-id="638b0-120">Branding</span></span>
<span data-ttu-id="638b0-121">Hello pozvánku e-mailů z vašeho klienta použít hello firemní branding, který může mít nastavíte pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="638b0-121">hello invitation emails from your tenant use hello company branding that you may have set up for your tenant.</span></span> <span data-ttu-id="638b0-122">Pokud chcete tuto funkci využít tootake [sem](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) jsou hello podrobnosti o tom, tooconfigure ho.</span><span class="sxs-lookup"><span data-stu-id="638b0-122">If you want tootake advantage of this capability, [here](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) are hello details on how tooconfigure it.</span></span> <span data-ttu-id="638b0-123">Hello banner s logem se zobrazí v e-mailu hello.</span><span class="sxs-lookup"><span data-stu-id="638b0-123">hello banner logo appears in hello email.</span></span> <span data-ttu-id="638b0-124">Postupujte podle hello velikost bitové kopie kvality pokyny a [sem](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) pro dosažení co nejlepších výsledků.</span><span class="sxs-lookup"><span data-stu-id="638b0-124">Follow hello image size and quality instructions [here](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) for best results.</span></span> <span data-ttu-id="638b0-125">Kromě toho název společnosti hello taky se zobrazí v tooaction volání hello.</span><span class="sxs-lookup"><span data-stu-id="638b0-125">In addition, hello company name also shows up in hello call tooaction.</span></span>

### <a name="call-tooaction"></a><span data-ttu-id="638b0-126">Volání tooaction</span><span class="sxs-lookup"><span data-stu-id="638b0-126">Call tooaction</span></span>
<span data-ttu-id="638b0-127">Hello volání tooaction se skládá ze dvou částí: vysvětlením, proč hello příjemce přijal hello e-mailu a jaké příjemce hello je požadováno toodo o něm.</span><span class="sxs-lookup"><span data-stu-id="638b0-127">hello call tooaction consists of two parts: explaining why hello recipient has received hello mail and what hello recipient is being asked toodo about it.</span></span>
- <span data-ttu-id="638b0-128">Hello "Proč" části se dají řešit pomocí hello následující vzor: jste byla pozvané tooaccess aplikace v hello &lt;tenantname&gt; organizace</span><span class="sxs-lookup"><span data-stu-id="638b0-128">hello "why" section can be addressed using hello following pattern: You've been invited tooaccess applications in hello &lt;tenantname&gt; organization</span></span>

- <span data-ttu-id="638b0-129">A hello "co jste se se zobrazí dotaz, toodo" část je indikován hello přítomnost hello **Začínáme** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="638b0-129">And hello "what you're being asked toodo" section is indicated by hello presence of hello **Get Started** button.</span></span> <span data-ttu-id="638b0-130">Pokud příjemce hello přidala bez nutnosti hello pozvánky, toto tlačítko nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="638b0-130">When hello recipient has been added without hello need for invitations, this button doesn't show up.</span></span>

### <a name="inviters-information"></a><span data-ttu-id="638b0-131">Pozval vás na informace</span><span class="sxs-lookup"><span data-stu-id="638b0-131">Inviter's information</span></span>
<span data-ttu-id="638b0-132">Odesílatel Hello pozvánky zobrazovaný název je součástí hello e-mailu.</span><span class="sxs-lookup"><span data-stu-id="638b0-132">hello inviter's display name is included in hello email.</span></span> <span data-ttu-id="638b0-133">A kromě toho, pokud jste nastavili profilový obrázek pro váš účet Azure AD, hello pozvání e-mailu budou obsahovat tento obrázek také.</span><span class="sxs-lookup"><span data-stu-id="638b0-133">And in addition, if you've set up a profile picture for your Azure AD account, hello inviting email will include that picture as well.</span></span> <span data-ttu-id="638b0-134">Obě jsou určený tooincrease vaše příjemce důvěru hello e-mailu.</span><span class="sxs-lookup"><span data-stu-id="638b0-134">Both are intended tooincrease your recipient's confidence in hello email.</span></span>

<span data-ttu-id="638b0-135">Pokud jste ještě profilový obrázek, se zobrazí ikona s pozval hello vás iniciály místo hello obrázek:</span><span class="sxs-lookup"><span data-stu-id="638b0-135">If you haven't yet set up your profile picture, an icon with hello inviter's initials in place of hello picture is shown:</span></span>

  ![zobrazení iniciály pozvánky hello](media/active-directory-b2b-invitation-email/inviters-initials.png)

### <a name="body"></a><span data-ttu-id="638b0-137">Tělo</span><span class="sxs-lookup"><span data-stu-id="638b0-137">Body</span></span>
<span data-ttu-id="638b0-138">Hello text obsahuje uvítací zprávu této pozvánky hello vytvoří nebo předána hello pozvánku rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="638b0-138">hello body contains hello message that hello inviter composes or is passed through hello invitation API.</span></span> <span data-ttu-id="638b0-139">Je textová oblast, takže nezpracovává značky HTML z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="638b0-139">It is a text area, so it does not process HTML tags for security reasons.</span></span>

### <a name="footer-section"></a><span data-ttu-id="638b0-140">Sekce zápatí</span><span class="sxs-lookup"><span data-stu-id="638b0-140">Footer section</span></span>
<span data-ttu-id="638b0-141">zápatí Hello obsahuje hello značky společnosti Microsoft a umožňuje hello příjemce vědět, pokud e-mailu hello byla odeslaná z Nesledované alias.</span><span class="sxs-lookup"><span data-stu-id="638b0-141">hello footer contains hello Microsoft company brand and lets hello recipient know if hello email was sent from an unmonitored alias.</span></span> <span data-ttu-id="638b0-142">Zvláštní případy:</span><span class="sxs-lookup"><span data-stu-id="638b0-142">Special cases:</span></span>

- <span data-ttu-id="638b0-143">Odesílatel Hello pozvánky nemá e-mailovou adresu v hello pozvání klientů</span><span class="sxs-lookup"><span data-stu-id="638b0-143">hello inviter doesn't have an email address in hello inviting tenancy</span></span>

  ![Obrázek pozval vás nemá e-mailovou adresu v hello pozvání klientů](media/active-directory-b2b-invitation-email/inviter-no-email.png)


- <span data-ttu-id="638b0-145">příjemce Hello nepotřebuje tooredeem hello Pozvánka</span><span class="sxs-lookup"><span data-stu-id="638b0-145">hello recipient doesn't need tooredeem hello invitation</span></span>

  ![Pokud příjemce nepotřebuje tooredeem Pozvánka](media/active-directory-b2b-invitation-email/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a><span data-ttu-id="638b0-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="638b0-147">Next steps</span></span>

<span data-ttu-id="638b0-148">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="638b0-148">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="638b0-149">Co je spolupráce Azure AD B2B</span><span class="sxs-lookup"><span data-stu-id="638b0-149">What is Azure AD B2B collaboration</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="638b0-150">Jak Azure Active Directory správci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="638b0-150">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="638b0-151">Jak informační pracovníci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="638b0-151">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="638b0-152">Uplatnění pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="638b0-152">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="638b0-153">Licencování Azure AD s B2B spolupráce</span><span class="sxs-lookup"><span data-stu-id="638b0-153">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="638b0-154">Řešení potíží s spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="638b0-154">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="638b0-155">Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)</span><span class="sxs-lookup"><span data-stu-id="638b0-155">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="638b0-156">Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="638b0-156">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="638b0-157">Vícefaktorové ověřování pro uživatele pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="638b0-157">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="638b0-158">Přidání uživatelů spolupráce B2B bez Pozvánka</span><span class="sxs-lookup"><span data-stu-id="638b0-158">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="638b0-159">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="638b0-159">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
