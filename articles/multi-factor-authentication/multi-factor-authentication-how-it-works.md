---
title: "aaaAzure Multi-Factor Authentication – jak to funguje"
description: "Azure Multi-Factor Authentication pomáhá chránit přístup toodata a aplikace při splnění požadavků uživatelů pro jednoduchý proces přihlášení. Poskytuje dodatečné zabezpečení vyžadováním druhou podobu ověřování a zajišťuje silné ověřování přes celou řadu možností snadno ověření."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d14db902-9afe-4fca-b3a5-4bd54b3d8ec5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 82f234fb86f145c42e8e56b8bdd2d61720c9ff2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-azure-multi-factor-authentication-works"></a><span data-ttu-id="ae833-104">Jak funguje Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="ae833-104">How Azure Multi-Factor Authentication works</span></span>
<span data-ttu-id="ae833-105">Hello zabezpečení dvoustupňové ověření spočívá v jeho vrstveného přístupu.</span><span class="sxs-lookup"><span data-stu-id="ae833-105">hello security of two-step verification lies in its layered approach.</span></span> <span data-ttu-id="ae833-106">Porušení zabezpečení několika faktory ověřování uvede významné výzvu pro útočníky.</span><span class="sxs-lookup"><span data-stu-id="ae833-106">Compromising multiple authentication factors presents a significant challenge for attackers.</span></span> <span data-ttu-id="ae833-107">I v případě, že útočník dokázal toolearn hello uživatelské heslo, je zbytečné bez nutnosti hello důvěryhodné zařízení u sebe.</span><span class="sxs-lookup"><span data-stu-id="ae833-107">Even if an attacker manages toolearn hello user's password, it is useless without also having possession of hello trusted device.</span></span> 

![Ověření](./media/multi-factor-authentication-how-it-works/howitworks.png)

<span data-ttu-id="ae833-109">Azure Multi-Factor Authentication pomáhá chránit přístup toodata a aplikace při splnění požadavků uživatelů pro jednoduchý proces přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ae833-109">Azure Multi-Factor Authentication helps safeguard access toodata and applications while meeting user demand for a simple sign-in process.</span></span>  <span data-ttu-id="ae833-110">Poskytuje dodatečné zabezpečení vyžadováním druhou podobu ověřování a zajišťuje silné ověřování přes celou řadu možností snadno ověření.</span><span class="sxs-lookup"><span data-stu-id="ae833-110">It provides additional security by requiring a second form of authentication and delivers strong authentication via a range of easy verification options.</span></span>


## <a name="methods-available-for-two-step-verification"></a><span data-ttu-id="ae833-111">Dostupné metody pro dvoustupňové ověření</span><span class="sxs-lookup"><span data-stu-id="ae833-111">Methods available for two-step verification</span></span>
<span data-ttu-id="ae833-112">Když se uživatel přihlásí, je odeslána další ověření toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="ae833-112">When a user signs in, an additional verification is sent toohello user.</span></span>  <span data-ttu-id="ae833-113">Hello následují seznam metod, které lze použít pro tento druhý ověření.</span><span class="sxs-lookup"><span data-stu-id="ae833-113">hello following are a list of methods that can be used for this second verification.</span></span>

| <span data-ttu-id="ae833-114">Metoda ověření</span><span class="sxs-lookup"><span data-stu-id="ae833-114">Verification Method</span></span> | <span data-ttu-id="ae833-115">Popis</span><span class="sxs-lookup"><span data-stu-id="ae833-115">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ae833-116">Telefonní hovor</span><span class="sxs-lookup"><span data-stu-id="ae833-116">Phone call</span></span> |<span data-ttu-id="ae833-117">Volání je umístěna do registrované telefon tooa uživatele.</span><span class="sxs-lookup"><span data-stu-id="ae833-117">A call is placed tooa user’s registered phone.</span></span> <span data-ttu-id="ae833-118">Hello uživatel zadá kód PIN, v případě potřeby pak stiskne klávesu # hello.</span><span class="sxs-lookup"><span data-stu-id="ae833-118">hello user enters a PIN if necessary then presses hello # key.</span></span> |
| <span data-ttu-id="ae833-119">Textová zpráva</span><span class="sxs-lookup"><span data-stu-id="ae833-119">Text message</span></span> |<span data-ttu-id="ae833-120">Mobilní telefon tooa uživatele s šestimístný kód je odeslána textová zpráva.</span><span class="sxs-lookup"><span data-stu-id="ae833-120">A text message is sent tooa user’s mobile phone with a six-digit code.</span></span> <span data-ttu-id="ae833-121">Hello uživatel zadá tento kód na přihlašovací stránku hello.</span><span class="sxs-lookup"><span data-stu-id="ae833-121">hello user enters this code on hello sign-in page.</span></span> |
| <span data-ttu-id="ae833-122">Oznámení mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="ae833-122">Mobile app notification</span></span> |<span data-ttu-id="ae833-123">Smartphone tooa uživatele je odeslána žádost o ověření.</span><span class="sxs-lookup"><span data-stu-id="ae833-123">A verification request is sent tooa user’s smart phone.</span></span> <span data-ttu-id="ae833-124">Hello uživatel zadá kód PIN, v případě potřeby pak vybere **ověřte** na mobilní aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ae833-124">hello user enters a PIN if necessary then selects **Verify** on hello mobile app.</span></span> |
| <span data-ttu-id="ae833-125">Kód ověření mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="ae833-125">Mobile app verification code</span></span> |<span data-ttu-id="ae833-126">Hello mobilní aplikaci, která běží na uživatele Smartphone, zobrazí ověřovací kód, který změní každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="ae833-126">hello mobile app, which is running on a user’s smart phone, displays a verification code that changes every 30 seconds.</span></span> <span data-ttu-id="ae833-127">uživatel Hello nejnovější kód hello vyhledá a přejde na přihlašovací stránku hello.</span><span class="sxs-lookup"><span data-stu-id="ae833-127">hello user finds hello most recent code and enters it on hello sign-in page.</span></span> |
| <span data-ttu-id="ae833-128">Tokeny OATH třetích stran</span><span class="sxs-lookup"><span data-stu-id="ae833-128">Third-party OATH tokens</span></span> | <span data-ttu-id="ae833-129">Azure Multi-Factor Authentication Server může být nakonfigurované tooaccept metody ověření dat třetí stranou.</span><span class="sxs-lookup"><span data-stu-id="ae833-129">Azure Multi-Factor Authentication Server can be configured tooaccept third-party verification methods.</span></span> |

<span data-ttu-id="ae833-130">Azure Multi-Factor Authentication poskytuje metody volitelný ověření pro cloud a serveru.</span><span class="sxs-lookup"><span data-stu-id="ae833-130">Azure Multi-Factor Authentication provides selectable verification methods for both cloud and server.</span></span> <span data-ttu-id="ae833-131">Můžete zvolit, které metody jsou k dispozici pro vaše uživatele: telefonní hovor, text, oznámení aplikaci nebo aplikaci kódy.</span><span class="sxs-lookup"><span data-stu-id="ae833-131">You can choose which methods are available for your users: phone call, text, app notification, or app codes.</span></span> <span data-ttu-id="ae833-132">Další informace najdete v tématu [volitelný ověření metody](multi-factor-authentication-whats-next.md#selectable-verification-methods).</span><span class="sxs-lookup"><span data-stu-id="ae833-132">For more information, see [selectable verification methods](multi-factor-authentication-whats-next.md#selectable-verification-methods).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae833-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ae833-133">Next steps</span></span>

- <span data-ttu-id="ae833-134">Přečtěte si informace o různých hello [verze a spotřeba metody pro ověřování Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span><span class="sxs-lookup"><span data-stu-id="ae833-134">Read about hello different [versions and consumption methods for Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span></span>

- <span data-ttu-id="ae833-135">Vyberte, zda toodeploy Azure MFA [v hello cloudu nebo místně](multi-factor-authentication-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="ae833-135">Choose whether toodeploy Azure MFA [in hello cloud or on-premises](multi-factor-authentication-get-started.md)</span></span>

- <span data-ttu-id="ae833-136">Přečtěte si odpovědi pro [nejčastější dotazy](multi-factor-authentication-faq.md)</span><span class="sxs-lookup"><span data-stu-id="ae833-136">Read answers for [Frequently asked questions](multi-factor-authentication-faq.md)</span></span>