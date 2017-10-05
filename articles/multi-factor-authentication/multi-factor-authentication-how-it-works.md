---
title: "Azure Multi-Factor Authentication – jak to funguje"
description: "Azure Multi-Factor Authentication pomáhá chránit přístup k datům a aplikacím a současně plní požadavky uživatelů na jednoduchý proces přihlašování. Poskytuje dodatečné zabezpečení vyžadováním druhou podobu ověřování a zajišťuje silné ověřování přes celou řadu možností snadno ověření."
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
ms.openlocfilehash: 6fee02885cc76b3a4fdad11e8702f623d6fe6597
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-azure-multi-factor-authentication-works"></a><span data-ttu-id="2e398-104">Jak funguje Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="2e398-104">How Azure Multi-Factor Authentication works</span></span>
<span data-ttu-id="2e398-105">Zabezpečení dvoustupňové ověření spočívá v jeho vrstveného přístupu.</span><span class="sxs-lookup"><span data-stu-id="2e398-105">The security of two-step verification lies in its layered approach.</span></span> <span data-ttu-id="2e398-106">Porušení zabezpečení několika faktory ověřování uvede významné výzvu pro útočníky.</span><span class="sxs-lookup"><span data-stu-id="2e398-106">Compromising multiple authentication factors presents a significant challenge for attackers.</span></span> <span data-ttu-id="2e398-107">I v případě, že útočník dokázal další heslo uživatele, je zbytečné bez nutnosti důvěryhodné zařízení u sebe.</span><span class="sxs-lookup"><span data-stu-id="2e398-107">Even if an attacker manages to learn the user's password, it is useless without also having possession of the trusted device.</span></span> 

![Ověření](./media/multi-factor-authentication-how-it-works/howitworks.png)

<span data-ttu-id="2e398-109">Azure Multi-Factor Authentication pomáhá chránit přístup k datům a aplikacím a současně plní požadavky uživatelů na jednoduchý proces přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2e398-109">Azure Multi-Factor Authentication helps safeguard access to data and applications while meeting user demand for a simple sign-in process.</span></span>  <span data-ttu-id="2e398-110">Poskytuje dodatečné zabezpečení vyžadováním druhou podobu ověřování a zajišťuje silné ověřování přes celou řadu možností snadno ověření.</span><span class="sxs-lookup"><span data-stu-id="2e398-110">It provides additional security by requiring a second form of authentication and delivers strong authentication via a range of easy verification options.</span></span>


## <a name="methods-available-for-two-step-verification"></a><span data-ttu-id="2e398-111">Dostupné metody pro dvoustupňové ověření</span><span class="sxs-lookup"><span data-stu-id="2e398-111">Methods available for two-step verification</span></span>
<span data-ttu-id="2e398-112">Když se uživatel přihlásí, je uživateli odeslána dalšího ověření.</span><span class="sxs-lookup"><span data-stu-id="2e398-112">When a user signs in, an additional verification is sent to the user.</span></span>  <span data-ttu-id="2e398-113">Následuje seznam metod, které lze použít pro tento druhý ověření.</span><span class="sxs-lookup"><span data-stu-id="2e398-113">The following are a list of methods that can be used for this second verification.</span></span>

| <span data-ttu-id="2e398-114">Metoda ověření</span><span class="sxs-lookup"><span data-stu-id="2e398-114">Verification Method</span></span> | <span data-ttu-id="2e398-115">Popis</span><span class="sxs-lookup"><span data-stu-id="2e398-115">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2e398-116">Telefonní hovor</span><span class="sxs-lookup"><span data-stu-id="2e398-116">Phone call</span></span> |<span data-ttu-id="2e398-117">Volání je umístěn na registrované Telefon uživatele.</span><span class="sxs-lookup"><span data-stu-id="2e398-117">A call is placed to a user’s registered phone.</span></span> <span data-ttu-id="2e398-118">Uživatel zadá kód PIN, pokud potřeby pak stiskne klávesu #.</span><span class="sxs-lookup"><span data-stu-id="2e398-118">The user enters a PIN if necessary then presses the # key.</span></span> |
| <span data-ttu-id="2e398-119">Textová zpráva</span><span class="sxs-lookup"><span data-stu-id="2e398-119">Text message</span></span> |<span data-ttu-id="2e398-120">Odeslána textová zpráva na mobilní telefon uživatele s šestimístný kód.</span><span class="sxs-lookup"><span data-stu-id="2e398-120">A text message is sent to a user’s mobile phone with a six-digit code.</span></span> <span data-ttu-id="2e398-121">Uživatel zadá tento kód na stránce přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2e398-121">The user enters this code on the sign-in page.</span></span> |
| <span data-ttu-id="2e398-122">Oznámení mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="2e398-122">Mobile app notification</span></span> |<span data-ttu-id="2e398-123">Žádost o ověření posílá Smartphone uživatele.</span><span class="sxs-lookup"><span data-stu-id="2e398-123">A verification request is sent to a user’s smart phone.</span></span> <span data-ttu-id="2e398-124">Uživatel zadá kód PIN, v případě potřeby pak vybere **ověřte** na mobilní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2e398-124">The user enters a PIN if necessary then selects **Verify** on the mobile app.</span></span> |
| <span data-ttu-id="2e398-125">Kód ověření mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="2e398-125">Mobile app verification code</span></span> |<span data-ttu-id="2e398-126">Mobilní aplikace, která běží na uživatele Smartphone, zobrazí ověřovací kód, který změní každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="2e398-126">The mobile app, which is running on a user’s smart phone, displays a verification code that changes every 30 seconds.</span></span> <span data-ttu-id="2e398-127">Najde poslední kód a přejde na stránku přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="2e398-127">The user finds the most recent code and enters it on the sign-in page.</span></span> |
| <span data-ttu-id="2e398-128">Tokeny OATH třetích stran</span><span class="sxs-lookup"><span data-stu-id="2e398-128">Third-party OATH tokens</span></span> | <span data-ttu-id="2e398-129">Azure Multi-Factor Authentication Server může být nastaven na přijímání metody ověřování třetích stran.</span><span class="sxs-lookup"><span data-stu-id="2e398-129">Azure Multi-Factor Authentication Server can be configured to accept third-party verification methods.</span></span> |

<span data-ttu-id="2e398-130">Azure Multi-Factor Authentication poskytuje metody volitelný ověření pro cloud a serveru.</span><span class="sxs-lookup"><span data-stu-id="2e398-130">Azure Multi-Factor Authentication provides selectable verification methods for both cloud and server.</span></span> <span data-ttu-id="2e398-131">Můžete zvolit, které metody jsou k dispozici pro vaše uživatele: telefonní hovor, text, oznámení aplikaci nebo aplikaci kódy.</span><span class="sxs-lookup"><span data-stu-id="2e398-131">You can choose which methods are available for your users: phone call, text, app notification, or app codes.</span></span> <span data-ttu-id="2e398-132">Další informace najdete v tématu [volitelný ověření metody](multi-factor-authentication-whats-next.md#selectable-verification-methods).</span><span class="sxs-lookup"><span data-stu-id="2e398-132">For more information, see [selectable verification methods](multi-factor-authentication-whats-next.md#selectable-verification-methods).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e398-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e398-133">Next steps</span></span>

- <span data-ttu-id="2e398-134">Přečtěte si informace o různých [verze a spotřeba metody pro ověřování Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span><span class="sxs-lookup"><span data-stu-id="2e398-134">Read about the different [versions and consumption methods for Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span></span>

- <span data-ttu-id="2e398-135">Vyberte, jestli chcete nasadit Azure MFA [v cloudu nebo místně](multi-factor-authentication-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="2e398-135">Choose whether to deploy Azure MFA [in the cloud or on-premises](multi-factor-authentication-get-started.md)</span></span>

- <span data-ttu-id="2e398-136">Přečtěte si odpovědi pro [nejčastější dotazy](multi-factor-authentication-faq.md)</span><span class="sxs-lookup"><span data-stu-id="2e398-136">Read answers for [Frequently asked questions](multi-factor-authentication-faq.md)</span></span>