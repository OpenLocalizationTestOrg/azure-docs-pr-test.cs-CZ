---
title: "Elementy Azure Active Directory s B2B spolupráce e-mailová pozvánka | Microsoft Docs"
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
ms.openlocfilehash: 458a2cab13b7e83f120e0926a95d454070181dfb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email"></a><span data-ttu-id="d02bb-103">Elementy e-mail pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="d02bb-103">The elements of the B2B collaboration invitation email</span></span>

<span data-ttu-id="d02bb-104">E-mailů pozvánku je zásadní součástí a převeďte partnery na palubě jako uživatelé spolupráce B2B ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d02bb-104">Invitation emails are a critical component to bring partners on board as B2B collaboration users in Azure AD.</span></span> <span data-ttu-id="d02bb-105">Můžete je používat k příjemce důvěryhodnosti zvýšit.</span><span class="sxs-lookup"><span data-stu-id="d02bb-105">You can use them to increase the recipient's trust.</span></span> <span data-ttu-id="d02bb-106">můžete přidat legitimitu a sociálních ověření k e-mailu, abyste měli jistotu příjemce funguje celý výběr **Začínáme** tlačítko pro přijetí pozvánky.</span><span class="sxs-lookup"><span data-stu-id="d02bb-106">you can add legitimacy and social proof to the email, to make sure the recipient feels comfortable with selecting the **Get Started** button to accept the invitation.</span></span> <span data-ttu-id="d02bb-107">Tento vztah důvěryhodnosti je, že klíč znamená snížení sdílení tření.</span><span class="sxs-lookup"><span data-stu-id="d02bb-107">This trust is a key means to reduce sharing friction.</span></span> <span data-ttu-id="d02bb-108">A budete chtít také zajistit e-mailu vypadají skvěle!</span><span class="sxs-lookup"><span data-stu-id="d02bb-108">And you also want to make the email look great!</span></span>

![E-mailová pozvánka Azure AD s B2b](media/active-directory-b2b-invitation-email/invitation-email.png)

## <a name="explaining-the-email"></a><span data-ttu-id="d02bb-110">Vysvětlení e-mailu</span><span class="sxs-lookup"><span data-stu-id="d02bb-110">Explaining the email</span></span>
<span data-ttu-id="d02bb-111">Podívejme se na několik elementy e-mailu, abyste věděli, jak nejlépe používat jejich funkce.</span><span class="sxs-lookup"><span data-stu-id="d02bb-111">Let's look at a few elements of the email so you know how best to use their capabilities.</span></span>

### <a name="subject"></a><span data-ttu-id="d02bb-112">Předmět</span><span class="sxs-lookup"><span data-stu-id="d02bb-112">Subject</span></span>
<span data-ttu-id="d02bb-113">Předmět e-mailu se následující následující: přijměte naše pozvání &lt;tenantname&gt; organizace</span><span class="sxs-lookup"><span data-stu-id="d02bb-113">The subject of the email follows the following pattern: You're invited to the &lt;tenantname&gt; organization</span></span>

### <a name="from-address"></a><span data-ttu-id="d02bb-114">Z adresy</span><span class="sxs-lookup"><span data-stu-id="d02bb-114">From address</span></span>
<span data-ttu-id="d02bb-115">Používáme LinkedIn jako vzor pro adresa odesílatele.</span><span class="sxs-lookup"><span data-stu-id="d02bb-115">We use a LinkedIn-like pattern for the From address.</span></span>  <span data-ttu-id="d02bb-116">Musí být jasné, kdo je pozvání odeslal a ze společnosti a také vysvětlení, že e-mailu, pochází z Microsoftu e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="d02bb-116">You should be clear who the inviter is and from which company, and also clarify that the email is coming from a Microsoft email address.</span></span> <span data-ttu-id="d02bb-117">Formát je: &lt;zobrazovaný název pozvánky&gt; z &lt;tenantname&gt; (přes Microsoft) <invites@microsoft.com&gt;</span><span class="sxs-lookup"><span data-stu-id="d02bb-117">The format is: &lt;Display name of inviter&gt; from &lt;tenantname&gt; (via Microsoft) <invites@microsoft.com&gt;</span></span>

### <a name="reply-to"></a><span data-ttu-id="d02bb-118">Odpovědět</span><span class="sxs-lookup"><span data-stu-id="d02bb-118">Reply To</span></span>
<span data-ttu-id="d02bb-119">Odpověď pro e-mailu je nastavena k e-mailu pozval vás, pokud je k dispozici, takže odpovídání na e-mailu, odešle e-mailem zpátky do pozvání odeslal.</span><span class="sxs-lookup"><span data-stu-id="d02bb-119">The reply-to email is set to the inviter's email when available, so that replying to the email sends an email back to the inviter.</span></span>

### <a name="branding"></a><span data-ttu-id="d02bb-120">Branding</span><span class="sxs-lookup"><span data-stu-id="d02bb-120">Branding</span></span>
<span data-ttu-id="d02bb-121">Pozvánku e-mailů z vašeho používání klienta firemního brandingu, které může nastavili pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="d02bb-121">The invitation emails from your tenant use the company branding that you may have set up for your tenant.</span></span> <span data-ttu-id="d02bb-122">Pokud budete chtít využít tuto funkci využít [sem](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) jsou uvedeny podrobnosti o tom, jak ho nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="d02bb-122">If you want to take advantage of this capability, [here](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) are the details on how to configure it.</span></span> <span data-ttu-id="d02bb-123">Banner s logem se zobrazí v e-mailu.</span><span class="sxs-lookup"><span data-stu-id="d02bb-123">The banner logo appears in the email.</span></span> <span data-ttu-id="d02bb-124">Postupujte podle velikost bitové kopie kvality pokyny a [sem](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) pro dosažení co nejlepších výsledků.</span><span class="sxs-lookup"><span data-stu-id="d02bb-124">Follow the image size and quality instructions [here](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) for best results.</span></span> <span data-ttu-id="d02bb-125">Kromě toho název společnosti také se zobrazí v volání akce.</span><span class="sxs-lookup"><span data-stu-id="d02bb-125">In addition, the company name also shows up in the call to action.</span></span>

### <a name="call-to-action"></a><span data-ttu-id="d02bb-126">Výzva k akci</span><span class="sxs-lookup"><span data-stu-id="d02bb-126">Call to action</span></span>
<span data-ttu-id="d02bb-127">Volání akce se skládá ze dvou částí: vysvětlením, proč příjemce přijal e-mailu a co příjemce je se zobrazí dotaz, jak je řešit.</span><span class="sxs-lookup"><span data-stu-id="d02bb-127">The call to action consists of two parts: explaining why the recipient has received the mail and what the recipient is being asked to do about it.</span></span>
- <span data-ttu-id="d02bb-128">V části "Proč" lze řešit pomocí následujícího vzorce: jste byli pozváni přístup k aplikacím v &lt;tenantname&gt; organizace</span><span class="sxs-lookup"><span data-stu-id="d02bb-128">The "why" section can be addressed using the following pattern: You've been invited to access applications in the &lt;tenantname&gt; organization</span></span>

- <span data-ttu-id="d02bb-129">A "co zobrazí se výzva k provést" části je indikován přítomnost **Začínáme** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d02bb-129">And the "what you're being asked to do" section is indicated by the presence of the **Get Started** button.</span></span> <span data-ttu-id="d02bb-130">Pokud příjemce byl přidán bez nutnosti pozvánky, toto tlačítko nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="d02bb-130">When the recipient has been added without the need for invitations, this button doesn't show up.</span></span>

### <a name="inviters-information"></a><span data-ttu-id="d02bb-131">Pozval vás na informace</span><span class="sxs-lookup"><span data-stu-id="d02bb-131">Inviter's information</span></span>
<span data-ttu-id="d02bb-132">Pozvání odeslal na zobrazované jméno je obsažena v e-mailu.</span><span class="sxs-lookup"><span data-stu-id="d02bb-132">The inviter's display name is included in the email.</span></span> <span data-ttu-id="d02bb-133">A kromě toho, pokud jste nastavili profilový obrázek pro váš účet Azure AD, pozváním e-mailu, budou obsahovat tento obrázek také.</span><span class="sxs-lookup"><span data-stu-id="d02bb-133">And in addition, if you've set up a profile picture for your Azure AD account, the inviting email will include that picture as well.</span></span> <span data-ttu-id="d02bb-134">Oba mají zvýšit důvěru vaše příjemce e-mailu.</span><span class="sxs-lookup"><span data-stu-id="d02bb-134">Both are intended to increase your recipient's confidence in the email.</span></span>

<span data-ttu-id="d02bb-135">Pokud jste ještě profilový obrázek, se zobrazí ikona s iniciály pozvání odeslal na místo na obrázku:</span><span class="sxs-lookup"><span data-stu-id="d02bb-135">If you haven't yet set up your profile picture, an icon with the inviter's initials in place of the picture is shown:</span></span>

  ![Zobrazení pozvánky iniciály](media/active-directory-b2b-invitation-email/inviters-initials.png)

### <a name="body"></a><span data-ttu-id="d02bb-137">Tělo</span><span class="sxs-lookup"><span data-stu-id="d02bb-137">Body</span></span>
<span data-ttu-id="d02bb-138">Text obsahuje zprávu, která vytvoří pozvání odeslal nebo předána pozvánku rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d02bb-138">The body contains the message that the inviter composes or is passed through the invitation API.</span></span> <span data-ttu-id="d02bb-139">Je textová oblast, takže nezpracovává značky HTML z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="d02bb-139">It is a text area, so it does not process HTML tags for security reasons.</span></span>

### <a name="footer-section"></a><span data-ttu-id="d02bb-140">Sekce zápatí</span><span class="sxs-lookup"><span data-stu-id="d02bb-140">Footer section</span></span>
<span data-ttu-id="d02bb-141">Zápatí obsahuje značky společnosti Microsoft a umožňuje příjemce vědět, pokud e-mailu byla odeslaná z Nesledované alias.</span><span class="sxs-lookup"><span data-stu-id="d02bb-141">The footer contains the Microsoft company brand and lets the recipient know if the email was sent from an unmonitored alias.</span></span> <span data-ttu-id="d02bb-142">Zvláštní případy:</span><span class="sxs-lookup"><span data-stu-id="d02bb-142">Special cases:</span></span>

- <span data-ttu-id="d02bb-143">Pozvání odeslal nemá e-mailovou adresu v pozváním klientů</span><span class="sxs-lookup"><span data-stu-id="d02bb-143">The inviter doesn't have an email address in the inviting tenancy</span></span>

  ![Obrázek pozval vás nemá e-mailovou adresu v pozváním klientů](media/active-directory-b2b-invitation-email/inviter-no-email.png)


- <span data-ttu-id="d02bb-145">Příjemce nemusí uplatnit pozvánku</span><span class="sxs-lookup"><span data-stu-id="d02bb-145">The recipient doesn't need to redeem the invitation</span></span>

  ![Pokud není třeba uplatnit pozvánku k příjemce](media/active-directory-b2b-invitation-email/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a><span data-ttu-id="d02bb-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d02bb-147">Next steps</span></span>

<span data-ttu-id="d02bb-148">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="d02bb-148">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="d02bb-149">Co je spolupráce Azure AD B2B</span><span class="sxs-lookup"><span data-stu-id="d02bb-149">What is Azure AD B2B collaboration</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="d02bb-150">Jak Azure Active Directory správci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="d02bb-150">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="d02bb-151">Jak informační pracovníci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="d02bb-151">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="d02bb-152">Uplatnění pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="d02bb-152">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="d02bb-153">Licencování Azure AD s B2B spolupráce</span><span class="sxs-lookup"><span data-stu-id="d02bb-153">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="d02bb-154">Řešení potíží s spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="d02bb-154">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="d02bb-155">Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)</span><span class="sxs-lookup"><span data-stu-id="d02bb-155">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="d02bb-156">Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="d02bb-156">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="d02bb-157">Vícefaktorové ověřování pro uživatele pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="d02bb-157">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="d02bb-158">Přidání uživatelů spolupráce B2B bez Pozvánka</span><span class="sxs-lookup"><span data-stu-id="d02bb-158">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="d02bb-159">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d02bb-159">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
