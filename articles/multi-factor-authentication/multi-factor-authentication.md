---
title: "Další informace o dvoustupňovém ověřování v Azure MFA | Microsoft Docs"
description: "Co je Azure Multi-Factor Authentication, proč použít vícefaktorové ověřování, další informace o vícefaktorového ověřování klienta a různé metody a verze, které jsou k dispozici. "
keywords: "Úvod do MFA, mfa přehled, co je mfa"
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
ms.openlocfilehash: 7334ab5b278c3339fdbc2e363fd5c609604d3e14
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-multi-factor-authentication"></a><span data-ttu-id="cac30-104">Co je Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="cac30-104">What is Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="cac30-105">Dvoustupňové ověření je metoda ověřování, který vyžaduje více než jednu metodu ověřování a přidá velmi důležitou druhou vrstvu zabezpečení uživatelská přihlášení a transakce.</span><span class="sxs-lookup"><span data-stu-id="cac30-105">Two-step verification is a method of authentication that requires more than one verification method and adds a critical second layer of security to user sign-ins and transactions.</span></span> <span data-ttu-id="cac30-106">Funguje tím, že jakékoliv dva nebo více z následujících metod ověřování:</span><span class="sxs-lookup"><span data-stu-id="cac30-106">It works by requiring any two or more of the following verification methods:</span></span>

* <span data-ttu-id="cac30-107">Něco znáte (obvykle heslo)</span><span class="sxs-lookup"><span data-stu-id="cac30-107">Something you know (typically a password)</span></span>
* <span data-ttu-id="cac30-108">Něco co uživatel má (důvěryhodné zařízení, která není duplikovaná snadno, například telefon)</span><span class="sxs-lookup"><span data-stu-id="cac30-108">Something you have (a trusted device that is not easily duplicated, like a phone)</span></span>
* <span data-ttu-id="cac30-109">Něco že se (biometrika)</span><span class="sxs-lookup"><span data-stu-id="cac30-109">Something you are (biometrics)</span></span>

<span data-ttu-id="cac30-110"><center>![Uživatelské jméno a heslo](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![certifikáty](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Inteligentní Phone](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![čipové karty](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Virtuální čipové karty](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![uživatelské jméno a Heslo](./media/multi-factor-authentication/cert.png)</center></span><span class="sxs-lookup"><span data-stu-id="cac30-110"><center>![Username and Password](./media/multi-factor-authentication/pword.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Certificates](./media/multi-factor-authentication/phone.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Phone](./media/multi-factor-authentication/hware.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Card](./media/multi-factor-authentication/smart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Virtual Smart Card](./media/multi-factor-authentication/vsmart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Username and Password](./media/multi-factor-authentication/cert.png)</center></span></span>

<span data-ttu-id="cac30-111">Azure Multi-Factor Authentication (MFA) je řešení dvoustupňového ověřování od Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="cac30-111">Azure Multi-Factor Authentication (MFA) is Microsoft's two-step verification solution.</span></span> <span data-ttu-id="cac30-112">Azure MFA pomáhá chránit přístup k datům a aplikacím a současně plní požadavky uživatelů na jednoduchý proces přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cac30-112">Azure MFA helps safeguard access to data and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="cac30-113">Zajišťuje silné ověřování prostřednictvím celé řady metod ověřování, včetně ověřování pomocí telefonních hovorů, textových zpráv nebo mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="cac30-113">It delivers strong authentication via a range of verification methods, including phone call, text message, or mobile app verification.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/WA-MFA-Overview/player]
>
>

## <a name="why-use-azure-multi-factor-authentication"></a><span data-ttu-id="cac30-114">Proč používat Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="cac30-114">Why use Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="cac30-115">Dnes, více než někdy uživatelé stále připojeni.</span><span class="sxs-lookup"><span data-stu-id="cac30-115">Today, more than ever, people are increasingly connected.</span></span> <span data-ttu-id="cac30-116">Chytré telefony, tablety, přenosné počítače a počítače uživatelé mají několik možností na tom, jak se bude k připojení a zachovat připojení kdykoli.</span><span class="sxs-lookup"><span data-stu-id="cac30-116">With smart phones, tablets, laptops, and PCs, people have several different options on how they are going to connect and stay connected at any time.</span></span> <span data-ttu-id="cac30-117">Lidé mají přístup k jejich účty a aplikace odkudkoli, to znamená, že můžete zvládnout větší objem práce a poskytovat svým zákazníkům lépe.</span><span class="sxs-lookup"><span data-stu-id="cac30-117">People can access their accounts and applications from anywhere, which means that they can get more work done and serve their customers better.</span></span>

<span data-ttu-id="cac30-118">Azure Multi-Factor Authentication je snadno použitelný, škálovatelného a spolehlivého řešení poskytující druhé metody ověřování, aby vaši uživatelé jsou vždycky chráněné.</span><span class="sxs-lookup"><span data-stu-id="cac30-118">Azure Multi-Factor Authentication is an easy to use, scalable, and reliable solution that provides a second method of authentication so your users are always protected.</span></span>

| ![Snadné použití](./media/multi-factor-authentication/simple.png) | ![Škálovatelné](./media/multi-factor-authentication/scalable.png) | ![Vždycky chráněné](./media/multi-factor-authentication/protected.png) | ![Spolehlivost](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| <span data-ttu-id="cac30-123">**Jednoduchost použití**</span><span class="sxs-lookup"><span data-stu-id="cac30-123">**Easy to use**</span></span> |<span data-ttu-id="cac30-124">**Škálovatelné**</span><span class="sxs-lookup"><span data-stu-id="cac30-124">**Scalable**</span></span> |<span data-ttu-id="cac30-125">**Vždycky chráněné**</span><span class="sxs-lookup"><span data-stu-id="cac30-125">**Always Protected**</span></span> |<span data-ttu-id="cac30-126">**Spolehlivé**</span><span class="sxs-lookup"><span data-stu-id="cac30-126">**Reliable**</span></span> |

* <span data-ttu-id="cac30-127">**Snadno použitelný** -Azure Multi-Factor Authentication se snadno nastavit a používat.</span><span class="sxs-lookup"><span data-stu-id="cac30-127">**Easy to Use** - Azure Multi-Factor Authentication is simple to set up and use.</span></span> <span data-ttu-id="cac30-128">Zvláštní ochranu, která se dodává s Azure Multi-Factor Authentication umožňuje uživatelům spravovat svá zařízení.</span><span class="sxs-lookup"><span data-stu-id="cac30-128">The extra protection that comes with Azure Multi-Factor Authentication allows users to manage their own devices.</span></span> <span data-ttu-id="cac30-129">Nejlepší všech v mnoha případech je lze ji nastavit pomocí několika jednoduchých kliknutí.</span><span class="sxs-lookup"><span data-stu-id="cac30-129">Best of all, in many instances it can be set up with just a few simple clicks.</span></span>
* <span data-ttu-id="cac30-130">**Škálovatelné** -Azure Multi-Factor Authentication využívá kapacitu cloudu a integruje s místní AD a vlastních aplikací.</span><span class="sxs-lookup"><span data-stu-id="cac30-130">**Scalable** - Azure Multi-Factor Authentication uses the power of the cloud and integrates with your on-premises AD and custom apps.</span></span> <span data-ttu-id="cac30-131">Tato ochrana je i rozšířit na vaše vysoký počet, klíčové scénáře.</span><span class="sxs-lookup"><span data-stu-id="cac30-131">This protection is even extended to your high-volume, mission-critical scenarios.</span></span>
* <span data-ttu-id="cac30-132">**Vždycky chráněné** -Azure Multi-Factor Authentication poskytuje silné ověřování s použitím nejvyšší oborových standardů.</span><span class="sxs-lookup"><span data-stu-id="cac30-132">**Always Protected** - Azure Multi-Factor Authentication provides strong authentication using the highest industry standards.</span></span>
* <span data-ttu-id="cac30-133">**Spolehlivé** -Zaručujeme 99,9 % dostupnost služby Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="cac30-133">**Reliable** - We guarantee 99.9% availability of Azure Multi-Factor Authentication.</span></span> <span data-ttu-id="cac30-134">Služba se považuje za není k dispozici, pokud nemůže přijímat nebo zpracovat žádosti o ověření pro dvoustupňové ověření.</span><span class="sxs-lookup"><span data-stu-id="cac30-134">The service is considered unavailable when it is unable to receive or process verification requests for the two-step verification.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a><span data-ttu-id="cac30-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cac30-135">Next steps</span></span>

- <span data-ttu-id="cac30-136">Další informace o [jak funguje Azure Multi-Factor Authentication](multi-factor-authentication-how-it-works.md)</span><span class="sxs-lookup"><span data-stu-id="cac30-136">Learn about [how Azure Multi-Factor Authentication works](multi-factor-authentication-how-it-works.md)</span></span>

- <span data-ttu-id="cac30-137">Přečtěte si informace o různých [verze a spotřeba metody pro ověřování Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span><span class="sxs-lookup"><span data-stu-id="cac30-137">Read about the different [versions and consumption methods for Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span></span>
