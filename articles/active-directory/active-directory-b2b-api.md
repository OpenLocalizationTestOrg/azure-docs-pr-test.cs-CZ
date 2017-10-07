---
title: "aaaAzure Active Directory s B2B spolupráce rozhraní API a přizpůsobení | Microsoft Docs"
description: "Spolupráce Azure Active Directory s B2B podporuje vaše vztahy povolením obchodní partnery tooselectively přístup k podnikovým aplikacím"
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
ms.openlocfilehash: 2609971ffa5d2ebc9466c61f4e4af11f5b045ecb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a><span data-ttu-id="29408-103">Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="29408-103">Azure Active Directory B2B collaboration API and customization</span></span>

<span data-ttu-id="29408-104">Narazili jsme mnoho zákazníků, řekněte nám, jestli chtějí toocustomize hello pozvánku proces způsobem, který funguje nejlépe ve svých organizací.</span><span class="sxs-lookup"><span data-stu-id="29408-104">We've had many customers tell us that they want toocustomize hello invitation process in a way that works best for their organizations.</span></span> <span data-ttu-id="29408-105">Naše rozhraním API můžete to udělat jen.</span><span class="sxs-lookup"><span data-stu-id="29408-105">With our API, you can do just that.</span></span> [<span data-ttu-id="29408-106">https://Developer.microsoft.com/Graph/Docs/API-Reference/V1.0/Resources/invitation</span><span class="sxs-lookup"><span data-stu-id="29408-106">https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation</span></span>](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-hello-invitation-api"></a><span data-ttu-id="29408-107">Možnosti hello pozvánku rozhraní API</span><span class="sxs-lookup"><span data-stu-id="29408-107">Capabilities of hello invitation API</span></span>
<span data-ttu-id="29408-108">Hello rozhraní API nabízí hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="29408-108">hello API offers hello following capabilities:</span></span>

1. <span data-ttu-id="29408-109">Pozvěte externího uživatele s *žádné* e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="29408-109">Invite an external user with *any* email address.</span></span>

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. <span data-ttu-id="29408-110">Přizpůsobte, kam chcete vaši uživatelé tooland po přijetí jejich pozvánku.</span><span class="sxs-lookup"><span data-stu-id="29408-110">Customize where you want your users tooland after they accept their invitation.</span></span>

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. <span data-ttu-id="29408-111">Zvolte toosend hello standardní pozvánku pošty přes nám</span><span class="sxs-lookup"><span data-stu-id="29408-111">Choose toosend hello standard invitation mail through us</span></span>

    ```
    "sendInvitationMessage": true
    ```

  <span data-ttu-id="29408-112">s toohello příjemce zprávu, kterou si můžete přizpůsobit</span><span class="sxs-lookup"><span data-stu-id="29408-112">with a message toohello recipient that you can customize</span></span>

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. <span data-ttu-id="29408-113">A zvolte toocc: osob, které mají tookeep v hello cykly o tento spolupracovník pozvání.</span><span class="sxs-lookup"><span data-stu-id="29408-113">And choose toocc: people you want tookeep in hello loop about your inviting this collaborator.</span></span>

5. <span data-ttu-id="29408-114">Nebo zcela přizpůsobit pozvánky a pracovní postup registrace výběrem není toosend oznámení prostřednictvím služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29408-114">Or completely customize your invitation and onboarding workflow by choosing not toosend notifications through Azure AD.</span></span>

    ```
    "sendInvitationMessage": false
    ```

  <span data-ttu-id="29408-115">V takovém případě můžete adresu URL se od získat hello rozhraní API, které můžete vložit šablonu e-mailu, zasílání rychlých zpráv nebo jiné metody distribuce podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="29408-115">In this case, you get back a redemption URL from hello API that you can embed in an email template, IM, or other distribution method of your choice.</span></span>

6. <span data-ttu-id="29408-116">Nakonec pokud jste správce, můžete tooinvite hello uživatel jako člen.</span><span class="sxs-lookup"><span data-stu-id="29408-116">Finally, if you are an admin, you can choose tooinvite hello user as member.</span></span>

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a><span data-ttu-id="29408-117">Modelu autorizace</span><span class="sxs-lookup"><span data-stu-id="29408-117">Authorization model</span></span>
<span data-ttu-id="29408-118">Hello rozhraní API se může spouštět ve hello následující režimy ověřování:</span><span class="sxs-lookup"><span data-stu-id="29408-118">hello API can be run in hello following authorization modes:</span></span>

### <a name="app--user-mode"></a><span data-ttu-id="29408-119">Aplikace + uživatelského režimu</span><span class="sxs-lookup"><span data-stu-id="29408-119">App + User mode</span></span>
<span data-ttu-id="29408-120">V tomto režimu, kdo používá potřebám hello rozhraní API toohave hello oprávnění toobe vytvoření pozvánek B2B.</span><span class="sxs-lookup"><span data-stu-id="29408-120">In this mode, whoever is using hello API needs toohave hello permissions toobe create B2B invitations.</span></span>

### <a name="app-only-mode"></a><span data-ttu-id="29408-121">Jenom režim aplikace</span><span class="sxs-lookup"><span data-stu-id="29408-121">App only mode</span></span>
<span data-ttu-id="29408-122">V kontextu pouze aplikace musí aplikace hello hello User.ReadWrite.All nebo Directory.ReadWrite.All oborů pro toosucceed pozvánku hello.</span><span class="sxs-lookup"><span data-stu-id="29408-122">In app only context, hello app needs hello User.ReadWrite.All or Directory.ReadWrite.All scopes for hello invitation toosucceed.</span></span>

<span data-ttu-id="29408-123">Další informace najdete v části: https://graph.microsoft.io/docs/authorization/permission_scopes</span><span class="sxs-lookup"><span data-stu-id="29408-123">For more information, refer to: https://graph.microsoft.io/docs/authorization/permission_scopes</span></span>


## <a name="powershell"></a><span data-ttu-id="29408-124">PowerShell</span><span class="sxs-lookup"><span data-stu-id="29408-124">PowerShell</span></span>
<span data-ttu-id="29408-125">Je nyní možné toouse prostředí PowerShell tooadd a pozvání externí uživatelé tooan organizace snadno.</span><span class="sxs-lookup"><span data-stu-id="29408-125">It is now possible toouse PowerShell tooadd and invite external users tooan organization easily.</span></span> <span data-ttu-id="29408-126">Vytvoření pozvánky pomocí rutiny hello:</span><span class="sxs-lookup"><span data-stu-id="29408-126">Create an invitation using hello cmdlet:</span></span>

```
New-AzureADMSInvitation
```

<span data-ttu-id="29408-127">Můžete použít hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="29408-127">You can use hello following options:</span></span>

* <span data-ttu-id="29408-128">-InvitedUserDisplayName</span><span class="sxs-lookup"><span data-stu-id="29408-128">-InvitedUserDisplayName</span></span>
* <span data-ttu-id="29408-129">-InvitedUserEmailAddress</span><span class="sxs-lookup"><span data-stu-id="29408-129">-InvitedUserEmailAddress</span></span>
* <span data-ttu-id="29408-130">-SendInvitationMessage</span><span class="sxs-lookup"><span data-stu-id="29408-130">-SendInvitationMessage</span></span>
* <span data-ttu-id="29408-131">-InvitedUserMessageInfo</span><span class="sxs-lookup"><span data-stu-id="29408-131">-InvitedUserMessageInfo</span></span>

<span data-ttu-id="29408-132">Můžete se taky podívat na odkaz hello pozvánku k rozhraní API v [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)</span><span class="sxs-lookup"><span data-stu-id="29408-132">You can also check out hello invitation API reference in [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)</span></span>

## <a name="next-steps"></a><span data-ttu-id="29408-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29408-133">Next steps</span></span>

<span data-ttu-id="29408-134">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="29408-134">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="29408-135">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="29408-135">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="29408-136">Jak Azure Active Directory správci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="29408-136">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="29408-137">Jak informační pracovníci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="29408-137">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="29408-138">elementy Hello hello e-mailová pozvánka pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="29408-138">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="29408-139">Uplatnění pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="29408-139">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="29408-140">Licencování Azure AD s B2B spolupráce</span><span class="sxs-lookup"><span data-stu-id="29408-140">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="29408-141">Řešení potíží s spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="29408-141">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="29408-142">Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)</span><span class="sxs-lookup"><span data-stu-id="29408-142">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="29408-143">Vícefaktorové ověřování pro uživatele pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="29408-143">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="29408-144">Přidání uživatelů spolupráce B2B bez Pozvánka</span><span class="sxs-lookup"><span data-stu-id="29408-144">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="29408-145">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="29408-145">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
