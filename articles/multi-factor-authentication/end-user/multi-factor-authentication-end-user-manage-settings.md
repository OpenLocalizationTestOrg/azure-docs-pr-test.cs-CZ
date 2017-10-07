---
title: "aaaManage nastavení dvoustupňového ověřování | Microsoft Docs"
description: "Spravujte, jak pomocí Azure Multi-Factor Authentication, včetně změnu kontaktních informací nebo konfigurace zařízení."
services: multi-factor-authentication
keywords: "vícefaktorové ověřování klienta, problém s ověřováním, ID korelace"
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 2c974b08c584943f3c5a6b9bf16497d1706e8329
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-settings-for-two-step-verification"></a><span data-ttu-id="9bf58-104">Spravovat nastavení pro dvoustupňové ověření</span><span class="sxs-lookup"><span data-stu-id="9bf58-104">Manage your settings for two-step verification</span></span>
<span data-ttu-id="9bf58-105">Tento článek obsahuje odpovědi na otázky týkající se nastavení tooupdate pro dvoustupňové ověření nebo vícefaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="9bf58-105">This article answers questions about how tooupdate settings for two-step verification or multi-factor authentication.</span></span> <span data-ttu-id="9bf58-106">Pokud máte problémy s přihlášením tooyour účet, podívejte se příliš[došlo k potížím s dvoustupnovym overovanim](multi-factor-authentication-end-user-troubleshoot.md) Poradce při potížích.</span><span class="sxs-lookup"><span data-stu-id="9bf58-106">If you are having issues signing in tooyour account, refer too[Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md) for troubleshooting help.</span></span>

## <a name="where-toofind-hello-settings-page"></a><span data-ttu-id="9bf58-107">Kde toofind hello nastavení stránky</span><span class="sxs-lookup"><span data-stu-id="9bf58-107">Where toofind hello settings page</span></span>
<span data-ttu-id="9bf58-108">V závislosti na tom, jak vaše společnost nastavit ověřování Azure Multi-Factor Authentication jsou na několika místech, kde můžete změnit nastavení jako vaše telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="9bf58-108">Depending on how your company set up Azure Multi-Factor Authentication, there are a few places where you can change your settings like your phone number.</span></span>

<span data-ttu-id="9bf58-109">Pokud váš správce IT odeslat na konkrétní adresy URL nebo kroky toomanage dvoustupňové ověření, postupujte podle těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="9bf58-109">If your IT admin sent out a specific URL or steps toomanage two-step verification, follow those instructions.</span></span> <span data-ttu-id="9bf58-110">Hello pokynů, jinak hodnota by měla fungovat pro ostatní uživatelé.</span><span class="sxs-lookup"><span data-stu-id="9bf58-110">Otherwise, hello following instructions should work for everybody else.</span></span> <span data-ttu-id="9bf58-111">Pokud tyto pokyny, ale nezobrazí hello stejné možnosti, které znamená, že vaše škola nebo přizpůsobit svoje vlastní portál.</span><span class="sxs-lookup"><span data-stu-id="9bf58-111">If you follow these steps but don't see hello same options, that means that your work or school customized their own portal.</span></span> <span data-ttu-id="9bf58-112">Požádejte správce pro portál Azure Multi-Factor Authentication tooyour odkaz hello.</span><span class="sxs-lookup"><span data-stu-id="9bf58-112">Ask your admin for hello link tooyour Azure Multi-Factor Authentication portal.</span></span>

1. <span data-ttu-id="9bf58-113">Přihlaste se příliš[https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="9bf58-113">Sign in too[https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>  
2. <span data-ttu-id="9bf58-114">V pravém horním hello vyberte název účtu a pak vyberte **profil**.</span><span class="sxs-lookup"><span data-stu-id="9bf58-114">Select your account name in hello top right, then select **profile**.</span></span>  
3. <span data-ttu-id="9bf58-115">Vyberte **dalšího ověření zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="9bf58-115">Select **Additional security verification**.</span></span>  

    ![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. <span data-ttu-id="9bf58-117">Hello další bezpečnostní ověření stránka načte s nastavením.</span><span class="sxs-lookup"><span data-stu-id="9bf58-117">hello Additional security verification page loads with your settings.</span></span>

    ![Ověření](./media/multi-factor-authentication-end-user-manage/proofup.png)

## <a name="i-want-toochange-my-phone-number-or-add-a-secondary-number"></a><span data-ttu-id="9bf58-119">Chcete toochange telefonního čísla nebo přidat sekundární číslo</span><span class="sxs-lookup"><span data-stu-id="9bf58-119">I want toochange my phone number, or add a secondary number</span></span>
<span data-ttu-id="9bf58-120">Je důležité tooconfigure telefonní číslo sekundární ověřování.</span><span class="sxs-lookup"><span data-stu-id="9bf58-120">It is important tooconfigure a secondary authentication phone number.</span></span>  <span data-ttu-id="9bf58-121">Protože vaše primární telefonní číslo a mobilní aplikace se pravděpodobně na hello stejné phone, hello sekundární telefonní číslo je jedinou možností hello, nebudou se moct tooget zpátky do vašeho účtu, pokud dojde ke ztrátě nebo odcizení vašeho telefonu.</span><span class="sxs-lookup"><span data-stu-id="9bf58-121">Because your primary phone number and your mobile app are probably on hello same phone, hello secondary phone number is hello only way you will be able tooget back into your account if your phone is lost or stolen.</span></span>

> [!NOTE]
> <span data-ttu-id="9bf58-122">Pokud nemáte přístup tooyour primární telefonní číslo a potřebujete pomoc, získávání v tooyour účtu, najdete v našich témata nápovědy v [došlo k potížím s dvoustupnovym overovanim](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="9bf58-122">If you don't have access tooyour primary phone number, and need help getting in tooyour account, see our help topics in [Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md).</span></span>  

<span data-ttu-id="9bf58-123">**toochange vaší primární telefonní číslo:**</span><span class="sxs-lookup"><span data-stu-id="9bf58-123">**toochange your primary phone number:**</span></span>  

1. <span data-ttu-id="9bf58-124">Na stránce pro hello další bezpečnostní ověření vyberte hello textové pole s vaší aktuální telefonní číslo a upravit ho pomocí nové telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="9bf58-124">On hello Additional security verification page, select hello text box with your current phone number and edit it with your new phone number.</span></span>  
2. <span data-ttu-id="9bf58-125">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9bf58-125">Select **Save**.</span></span>  
3. <span data-ttu-id="9bf58-126">Pokud je číslo hello, který používáte pro vaše upřednostňovanou možnost ověřování, musíte tooverify hello nové číslo předtím, než jste jej uložili.</span><span class="sxs-lookup"><span data-stu-id="9bf58-126">If this is hello number that you use for your preferred verification option, you have tooverify hello new number before you can save it.</span></span>  

<span data-ttu-id="9bf58-127">**tooadd záložní telefonní číslo:**</span><span class="sxs-lookup"><span data-stu-id="9bf58-127">**tooadd a secondary phone number:**</span></span>  

1. <span data-ttu-id="9bf58-128">Na stránce další bezpečnostní ověření hello políčko hello vedle příliš**telefon pro alternativní ověření.**</span><span class="sxs-lookup"><span data-stu-id="9bf58-128">On hello Additional security verification page, check hello box next too**Alternate authentication phone.**</span></span>  
2. <span data-ttu-id="9bf58-129">Zadejte do textového pole hello sekundární telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="9bf58-129">Enter your secondary phone number in hello text box.</span></span>  
3. <span data-ttu-id="9bf58-130">Vyberte **Uložit** a dokončení změny.</span><span class="sxs-lookup"><span data-stu-id="9bf58-130">Select **Save** and your changes are finished.</span></span>  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a><span data-ttu-id="9bf58-131">Vyžadovat dvoustupňové ověření znovu v zařízení, které jste označili jako důvěryhodný</span><span class="sxs-lookup"><span data-stu-id="9bf58-131">Require two-step verification again on a device you've marked as trusted</span></span>

<span data-ttu-id="9bf58-132">V závislosti na nastavení organizace může mít zaškrtávací políčko, která uvádí, že "nezobrazovat dotaz dalších **X** dní" při provádění dvoustupňové ověření v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="9bf58-132">Depending on your organization settings, you may have a checkbox that says "Don't ask again for **X** days" when you perform two-step verification on your browser.</span></span> <span data-ttu-id="9bf58-133">Pokud toto políčko zaškrtněte a pak ztráty zařízení nebo myslíte, že je váš účet ohrožená, obnovíte dvoustupňové ověření tooall zařízení.</span><span class="sxs-lookup"><span data-stu-id="9bf58-133">If you check this box and then lose your device or think that your account is compromised, you should restore two-step verification tooall your devices.</span></span> 

1. <span data-ttu-id="9bf58-134">Na stránce další bezpečnostní ověření hello vyberte **obnovení služby Multi-Factor authentication u dřív důvěryhodných zařízení**.</span><span class="sxs-lookup"><span data-stu-id="9bf58-134">On hello Additional security verification page, select **Restore multi-factor authentication on previously trusted devices**.</span></span>
2. <span data-ttu-id="9bf58-135">Hello při příštím přihlášení na libovolném zařízení, budete výzvami tooperform dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="9bf58-135">hello next time you sign in on any device, you'll be prompted tooperform two-step verification.</span></span> 

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-tooa-new-one"></a><span data-ttu-id="9bf58-136">Jak vyčistit Microsoft Authenticator z původního zařízení a přesunout tooa novým?</span><span class="sxs-lookup"><span data-stu-id="9bf58-136">How do I clean up Microsoft Authenticator from my old device and move tooa new one?</span></span>
<span data-ttu-id="9bf58-137">Když odinstalujete aplikaci hello ze zařízení nebo zařízení hello resetování, neodebere hello aktivace na back-end hello.</span><span class="sxs-lookup"><span data-stu-id="9bf58-137">When you uninstall hello app from your device or reset hello device, it does not remove hello activation on hello back end.</span></span> <span data-ttu-id="9bf58-138">Další informace najdete v tématu [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="9bf58-138">For more information, see [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9bf58-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9bf58-139">Next steps</span></span>
* <span data-ttu-id="9bf58-140">Získat tipy pro odstraňování potíží a nápovědy na [došlo k potížím s dvoustupňové ověření](multi-factor-authentication-end-user-troubleshoot.md)</span><span class="sxs-lookup"><span data-stu-id="9bf58-140">Get troubleshooting tips and help on [Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md)</span></span>
* <span data-ttu-id="9bf58-141">Nastavit [hesla aplikací](multi-factor-authentication-end-user-app-passwords.md) pro všechny aplikace, které nepodporují dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="9bf58-141">Set up [app passwords](multi-factor-authentication-end-user-app-passwords.md) for any apps that don't support two-step verification.</span></span>
