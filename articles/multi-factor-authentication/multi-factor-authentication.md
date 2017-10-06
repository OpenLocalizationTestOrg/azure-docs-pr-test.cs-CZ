---
title: "aaaLearn o dvoustupňovém ověřování v Azure MFA | Microsoft Docs"
description: "Co je Azure Multi-Factor Authentication, proč použít vícefaktorové ověřování, další informace o hello vícefaktorového ověřování klienta a různé metody hello a verze, které jsou k dispozici. "
keywords: "tooMFA Úvod přehled vícefaktorového ověřování, co je mfa"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: c40d7a34-1274-4496-96b0-784850c06e9b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: kgremban
ms.openlocfilehash: a91b8d6941d2b6ce72a789a97c57e10e594e7ada
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-multi-factor-authentication"></a><span data-ttu-id="9ea7c-104">Co je Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="9ea7c-104">What is Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="9ea7c-105">Dvoustupňové ověření je metoda ověřování, který vyžaduje více než jednu metodu ověřování a přidá velmi důležitou druhou vrstvu zabezpečení toouser přihlášení a transakce.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-105">Two-step verification is a method of authentication that requires more than one verification method and adds a critical second layer of security toouser sign-ins and transactions.</span></span> <span data-ttu-id="9ea7c-106">Funguje tím, že jakékoliv dva nebo více hello následující metody ověření:</span><span class="sxs-lookup"><span data-stu-id="9ea7c-106">It works by requiring any two or more of hello following verification methods:</span></span>

* <span data-ttu-id="9ea7c-107">Něco znáte (obvykle heslo)</span><span class="sxs-lookup"><span data-stu-id="9ea7c-107">Something you know (typically a password)</span></span>
* <span data-ttu-id="9ea7c-108">Něco co uživatel má (důvěryhodné zařízení, která není duplikovaná snadno, například telefon)</span><span class="sxs-lookup"><span data-stu-id="9ea7c-108">Something you have (a trusted device that is not easily duplicated, like a phone)</span></span>
* <span data-ttu-id="9ea7c-109">Něco že se (biometrika)</span><span class="sxs-lookup"><span data-stu-id="9ea7c-109">Something you are (biometrics)</span></span>

<span data-ttu-id="9ea7c-110"><center>![Uživatelské jméno a heslo](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![certifikáty](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Inteligentní Phone](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![čipové karty](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Virtuální čipové karty](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![uživatelské jméno a Heslo](./media/multi-factor-authentication/cert.png)</center></span><span class="sxs-lookup"><span data-stu-id="9ea7c-110"><center>![Username and Password](./media/multi-factor-authentication/pword.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Certificates](./media/multi-factor-authentication/phone.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Phone](./media/multi-factor-authentication/hware.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Card](./media/multi-factor-authentication/smart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Virtual Smart Card](./media/multi-factor-authentication/vsmart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Username and Password](./media/multi-factor-authentication/cert.png)</center></span></span>

<span data-ttu-id="9ea7c-111">Azure Multi-Factor Authentication (MFA) je řešení dvoustupňového ověřování od Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-111">Azure Multi-Factor Authentication (MFA) is Microsoft's two-step verification solution.</span></span> <span data-ttu-id="9ea7c-112">Azure MFA pomáhá chránit přístup toodata a aplikace při splnění požadavků uživatelů pro jednoduchý proces přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-112">Azure MFA helps safeguard access toodata and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="9ea7c-113">Zajišťuje silné ověřování prostřednictvím celé řady metod ověřování, včetně ověřování pomocí telefonních hovorů, textových zpráv nebo mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-113">It delivers strong authentication via a range of verification methods, including phone call, text message, or mobile app verification.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/WA-MFA-Overview/player]
>
>

## <a name="why-use-azure-multi-factor-authentication"></a><span data-ttu-id="9ea7c-114">Proč používat Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="9ea7c-114">Why use Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="9ea7c-115">Dnes, více než někdy uživatelé stále připojeni.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-115">Today, more than ever, people are increasingly connected.</span></span> <span data-ttu-id="9ea7c-116">Chytré telefony, tablety, přenosné počítače a počítače uživatelé mají několik možností na tom, jak se budou tooconnect zůstat připojeni a kdykoli.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-116">With smart phones, tablets, laptops, and PCs, people have several different options on how they are going tooconnect and stay connected at any time.</span></span> <span data-ttu-id="9ea7c-117">Lidé mají přístup k jejich účty a aplikace odkudkoli, to znamená, že můžete zvládnout větší objem práce a poskytovat svým zákazníkům lépe.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-117">People can access their accounts and applications from anywhere, which means that they can get more work done and serve their customers better.</span></span>

<span data-ttu-id="9ea7c-118">Azure Multi-Factor Authentication je snadno toouse, škálovatelnou, a spolehlivé řešení, který poskytuje, aby druhé metody ověřování, aby vaši uživatelé jsou vždycky chráněné.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-118">Azure Multi-Factor Authentication is an easy toouse, scalable, and reliable solution that provides a second method of authentication so your users are always protected.</span></span>

| ![Snadno tooUse](./media/multi-factor-authentication/simple.png) | ![Škálovatelné](./media/multi-factor-authentication/scalable.png) | ![Vždycky chráněné](./media/multi-factor-authentication/protected.png) | ![Spolehlivost](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| <span data-ttu-id="9ea7c-123">**Snadno toouse**</span><span class="sxs-lookup"><span data-stu-id="9ea7c-123">**Easy toouse**</span></span> |<span data-ttu-id="9ea7c-124">**Škálovatelné**</span><span class="sxs-lookup"><span data-stu-id="9ea7c-124">**Scalable**</span></span> |<span data-ttu-id="9ea7c-125">**Vždycky chráněné**</span><span class="sxs-lookup"><span data-stu-id="9ea7c-125">**Always Protected**</span></span> |<span data-ttu-id="9ea7c-126">**Spolehlivé**</span><span class="sxs-lookup"><span data-stu-id="9ea7c-126">**Reliable**</span></span> |

* <span data-ttu-id="9ea7c-127">**Snadno tooUse** -Azure Multi-Factor Authentication je jednoduchý tooset nahoru a použití.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-127">**Easy tooUse** - Azure Multi-Factor Authentication is simple tooset up and use.</span></span> <span data-ttu-id="9ea7c-128">Hello zvláštní ochranu, která se dodává s Azure Multi-Factor Authentication umožňuje uživatelům toomanage svá vlastní zařízení.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-128">hello extra protection that comes with Azure Multi-Factor Authentication allows users toomanage their own devices.</span></span> <span data-ttu-id="9ea7c-129">Nejlepší všech v mnoha případech je lze ji nastavit pomocí několika jednoduchých kliknutí.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-129">Best of all, in many instances it can be set up with just a few simple clicks.</span></span>
* <span data-ttu-id="9ea7c-130">**Škálovatelné** -Azure Multi-Factor Authentication použije hello výkonu hello cloudových a integruje s místní AD a vlastních aplikací.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-130">**Scalable** - Azure Multi-Factor Authentication uses hello power of hello cloud and integrates with your on-premises AD and custom apps.</span></span> <span data-ttu-id="9ea7c-131">Tato ochrana je i rozšířené tooyour vysoký počet, klíčové scénáře.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-131">This protection is even extended tooyour high-volume, mission-critical scenarios.</span></span>
* <span data-ttu-id="9ea7c-132">**Vždycky chráněné** -Azure Multi-Factor Authentication poskytuje silné ověřování s použitím hello nejvyšší oborových standardů.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-132">**Always Protected** - Azure Multi-Factor Authentication provides strong authentication using hello highest industry standards.</span></span>
* <span data-ttu-id="9ea7c-133">**Spolehlivé** -Zaručujeme 99,9 % dostupnost služby Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-133">**Reliable** - We guarantee 99.9% availability of Azure Multi-Factor Authentication.</span></span> <span data-ttu-id="9ea7c-134">Hello služby se považuje za není k dispozici po nelze tooreceive nebo proces žádosti o ověření pro hello dvoustupňové ověření.</span><span class="sxs-lookup"><span data-stu-id="9ea7c-134">hello service is considered unavailable when it is unable tooreceive or process verification requests for hello two-step verification.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a><span data-ttu-id="9ea7c-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ea7c-135">Next steps</span></span>

- <span data-ttu-id="9ea7c-136">Další informace o [jak funguje Azure Multi-Factor Authentication](multi-factor-authentication-how-it-works.md)</span><span class="sxs-lookup"><span data-stu-id="9ea7c-136">Learn about [how Azure Multi-Factor Authentication works](multi-factor-authentication-how-it-works.md)</span></span>

- <span data-ttu-id="9ea7c-137">Přečtěte si informace o různých hello [verze a spotřeba metody pro ověřování Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span><span class="sxs-lookup"><span data-stu-id="9ea7c-137">Read about hello different [versions and consumption methods for Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span></span>
