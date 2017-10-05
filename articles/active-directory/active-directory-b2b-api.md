---
title: "Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení | Microsoft Docs"
description: "Spolupráce B2B ve službě Azure Active Directory podporuje vaše vztahy s ostatními společnostmi tím, že vašim obchodním partnerům umožní selektivní přístup ke podnikovým aplikacím"
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
ms.date: 04/11/2017
ms.author: sasubram
ms.openlocfilehash: c85e05b38b4a9525e13ec510a17b7ef4841198d7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a><span data-ttu-id="bd0f4-103">Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="bd0f4-103">Azure Active Directory B2B collaboration API and customization</span></span>

<span data-ttu-id="bd0f4-104">Narazili jsme mnoho zákazníků, řekněte nám, chtějí-li k přizpůsobení procesu pozvánku způsobem, který je nejvhodnější pro jejich organizace.</span><span class="sxs-lookup"><span data-stu-id="bd0f4-104">We've had many customers tell us that they want to customize the invitation process in a way that works best for their organizations.</span></span> <span data-ttu-id="bd0f4-105">Naše rozhraním API můžete to udělat jen.</span><span class="sxs-lookup"><span data-stu-id="bd0f4-105">With our API, you can do just that.</span></span> [<span data-ttu-id="bd0f4-106">https://Developer.microsoft.com/Graph/Docs/API-Reference/V1.0/Resources/invitation</span><span class="sxs-lookup"><span data-stu-id="bd0f4-106">https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation</span></span>](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-the-invitation-api"></a><span data-ttu-id="bd0f4-107">Možnosti pozvánku rozhraní API</span><span class="sxs-lookup"><span data-stu-id="bd0f4-107">Capabilities of the invitation API</span></span>
<span data-ttu-id="bd0f4-108">Rozhraní API nabízí tyto možnosti:</span><span class="sxs-lookup"><span data-stu-id="bd0f4-108">The API offers the following capabilities:</span></span>

1. <span data-ttu-id="bd0f4-109">Pozvěte externího uživatele s *žádné* e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="bd0f4-109">Invite an external user with *any* email address.</span></span>

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. <span data-ttu-id="bd0f4-110">Přizpůsobte, kam chcete uživatelům zobrazovat po přijetí jejich pozvánku.</span><span class="sxs-lookup"><span data-stu-id="bd0f4-110">Customize where you want your users to land after they accept their invitation.</span></span>

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. <span data-ttu-id="bd0f4-111">Vyberte možnost odesílat e-mailu standardní pozvánku prostřednictvím nám</span><span class="sxs-lookup"><span data-stu-id="bd0f4-111">Choose to send the standard invitation mail through us</span></span>

    ```
    "sendInvitationMessage": true
    ```

  <span data-ttu-id="bd0f4-112">zpráva pro příjemce, který můžete přizpůsobit</span><span class="sxs-lookup"><span data-stu-id="bd0f4-112">with a message to the recipient that you can customize</span></span>

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. <span data-ttu-id="bd0f4-113">A zvolte na kopii: osoby, které chcete zachovat ve smyčce o tento spolupracovník pozvání.</span><span class="sxs-lookup"><span data-stu-id="bd0f4-113">And choose to cc: people you want to keep in the loop about your inviting this collaborator.</span></span>

5. <span data-ttu-id="bd0f4-114">Nebo zcela přizpůsobit pozvánky a pracovní postup registrace výběrem nechcete odeslat oznámení prostřednictvím služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bd0f4-114">Or completely customize your invitation and onboarding workflow by choosing not to send notifications through Azure AD.</span></span>

    ```
    "sendInvitationMessage": false
    ```

  <span data-ttu-id="bd0f4-115">V takovém případě můžete adresu URL se od získat rozhraní API, které můžete vložit šablonu e-mailu, zasílání rychlých zpráv nebo jiné metody distribuce podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="bd0f4-115">In this case, you get back a redemption URL from the API that you can embed in an email template, IM, or other distribution method of your choice.</span></span>

6. <span data-ttu-id="bd0f4-116">Nakonec pokud jste správce, můžete pozvat uživatele jako člen.</span><span class="sxs-lookup"><span data-stu-id="bd0f4-116">Finally, if you are an admin, you can choose to invite the user as member.</span></span>

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a><span data-ttu-id="bd0f4-117">Modelu autorizace</span><span class="sxs-lookup"><span data-stu-id="bd0f4-117">Authorization model</span></span>
<span data-ttu-id="bd0f4-118">Rozhraní API můžete spustit v následujících režimech ověřování:</span><span class="sxs-lookup"><span data-stu-id="bd0f4-118">The API can be run in the following authorization modes:</span></span>

### <a name="app--user-mode"></a><span data-ttu-id="bd0f4-119">Aplikace + uživatelského režimu</span><span class="sxs-lookup"><span data-stu-id="bd0f4-119">App + User mode</span></span>
<span data-ttu-id="bd0f4-120">V tomto režimu kdo používá rozhraní API musí mít oprávnění pro být vytvoření pozvánky B2B.</span><span class="sxs-lookup"><span data-stu-id="bd0f4-120">In this mode, whoever is using the API needs to have the permissions to be create B2B invitations.</span></span>

### <a name="app-only-mode"></a><span data-ttu-id="bd0f4-121">Jenom režim aplikace</span><span class="sxs-lookup"><span data-stu-id="bd0f4-121">App only mode</span></span>
<span data-ttu-id="bd0f4-122">V kontextu pouze aplikace musí aplikace User.ReadWrite.All nebo Directory.ReadWrite.All oborů této pozvánky proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="bd0f4-122">In app only context, the app needs the User.ReadWrite.All or Directory.ReadWrite.All scopes for the invitation to succeed.</span></span>

<span data-ttu-id="bd0f4-123">Další informace najdete v části: https://graph.microsoft.io/docs/authorization/permission_scopes</span><span class="sxs-lookup"><span data-stu-id="bd0f4-123">For more information, refer to: https://graph.microsoft.io/docs/authorization/permission_scopes</span></span>


## <a name="powershell"></a><span data-ttu-id="bd0f4-124">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bd0f4-124">PowerShell</span></span>
<span data-ttu-id="bd0f4-125">Nyní je možné použít PowerShell k přidání a snadno pozvat externí uživatele organizaci.</span><span class="sxs-lookup"><span data-stu-id="bd0f4-125">It is now possible to use PowerShell to add and invite external users to an organization easily.</span></span> <span data-ttu-id="bd0f4-126">Vytvoření pozvánky pomocí rutiny:</span><span class="sxs-lookup"><span data-stu-id="bd0f4-126">Create an invitation using the cmdlet:</span></span>

```
New-AzureADMSInvitation
```

<span data-ttu-id="bd0f4-127">Můžete použít následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="bd0f4-127">You can use the following options:</span></span>

* <span data-ttu-id="bd0f4-128">-InvitedUserDisplayName</span><span class="sxs-lookup"><span data-stu-id="bd0f4-128">-InvitedUserDisplayName</span></span>
* <span data-ttu-id="bd0f4-129">-InvitedUserEmailAddress</span><span class="sxs-lookup"><span data-stu-id="bd0f4-129">-InvitedUserEmailAddress</span></span>
* <span data-ttu-id="bd0f4-130">-SendInvitationMessage</span><span class="sxs-lookup"><span data-stu-id="bd0f4-130">-SendInvitationMessage</span></span>
* <span data-ttu-id="bd0f4-131">-InvitedUserMessageInfo</span><span class="sxs-lookup"><span data-stu-id="bd0f4-131">-InvitedUserMessageInfo</span></span>

<span data-ttu-id="bd0f4-132">Můžete se taky podívat na odkaz na pozvánku k rozhraní API v [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)</span><span class="sxs-lookup"><span data-stu-id="bd0f4-132">You can also check out the invitation API reference in [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd0f4-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd0f4-133">Next steps</span></span>

<span data-ttu-id="bd0f4-134">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="bd0f4-134">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="bd0f4-135">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="bd0f4-135">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="bd0f4-136">Jak Azure Active Directory správci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="bd0f4-136">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="bd0f4-137">Jak informační pracovníci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="bd0f4-137">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="bd0f4-138">Elementy e-mail pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="bd0f4-138">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="bd0f4-139">Uplatnění pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="bd0f4-139">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="bd0f4-140">Licencování Azure AD s B2B spolupráce</span><span class="sxs-lookup"><span data-stu-id="bd0f4-140">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="bd0f4-141">Řešení potíží s spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="bd0f4-141">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="bd0f4-142">Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)</span><span class="sxs-lookup"><span data-stu-id="bd0f4-142">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="bd0f4-143">Vícefaktorové ověřování pro uživatele pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="bd0f4-143">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="bd0f4-144">Přidání uživatelů spolupráce B2B bez Pozvánka</span><span class="sxs-lookup"><span data-stu-id="bd0f4-144">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="bd0f4-145">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bd0f4-145">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
