---
title: "Spravovat nastavení dvoustupňového ověřování | Microsoft Docs"
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
ms.openlocfilehash: f752fb19e4ff2f831d50104e9c7d5f42cc3486d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-your-settings-for-two-step-verification"></a><span data-ttu-id="daff4-104">Spravovat nastavení pro dvoustupňové ověření</span><span class="sxs-lookup"><span data-stu-id="daff4-104">Manage your settings for two-step verification</span></span>
<span data-ttu-id="daff4-105">Tento článek obsahuje odpovědi na dotazy týkající se aktualizovat nastavení pro dvoustupňové ověření nebo vícefaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="daff4-105">This article answers questions about how to update settings for two-step verification or multi-factor authentication.</span></span> <span data-ttu-id="daff4-106">Pokud máte problémy s přihlášení ke svému účtu, podívejte se na [došlo k potížím s dvoustupnovym overovanim](multi-factor-authentication-end-user-troubleshoot.md) Poradce při potížích.</span><span class="sxs-lookup"><span data-stu-id="daff4-106">If you are having issues signing in to your account, refer to [Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md) for troubleshooting help.</span></span>

## <a name="where-to-find-the-settings-page"></a><span data-ttu-id="daff4-107">Kde najít nastavení stránky</span><span class="sxs-lookup"><span data-stu-id="daff4-107">Where to find the settings page</span></span>
<span data-ttu-id="daff4-108">V závislosti na tom, jak vaše společnost nastavit ověřování Azure Multi-Factor Authentication jsou na několika místech, kde můžete změnit nastavení jako vaše telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="daff4-108">Depending on how your company set up Azure Multi-Factor Authentication, there are a few places where you can change your settings like your phone number.</span></span>

<span data-ttu-id="daff4-109">Pokud váš správce IT odeslat na konkrétní adresy URL nebo kroky, které slouží ke správě dvoustupňové ověření, postupujte podle těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="daff4-109">If your IT admin sent out a specific URL or steps to manage two-step verification, follow those instructions.</span></span> <span data-ttu-id="daff4-110">Následující pokyny, jinak hodnota by měla fungovat pro ostatní uživatelé.</span><span class="sxs-lookup"><span data-stu-id="daff4-110">Otherwise, the following instructions should work for everybody else.</span></span> <span data-ttu-id="daff4-111">Pokud tyto pokyny, ale nevidíte stejné možnosti, to znamená, že vaše škola nebo přizpůsobit svoje vlastní portál.</span><span class="sxs-lookup"><span data-stu-id="daff4-111">If you follow these steps but don't see the same options, that means that your work or school customized their own portal.</span></span> <span data-ttu-id="daff4-112">Požádejte správce pro odkaz na portál Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="daff4-112">Ask your admin for the link to your Azure Multi-Factor Authentication portal.</span></span>

1. <span data-ttu-id="daff4-113">Přihlaste se k [https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="daff4-113">Sign in to [https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>  
2. <span data-ttu-id="daff4-114">V pravém horním rohu vyberte název účtu a pak vyberte **profil**.</span><span class="sxs-lookup"><span data-stu-id="daff4-114">Select your account name in the top right, then select **profile**.</span></span>  
3. <span data-ttu-id="daff4-115">Vyberte **dalšího ověření zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="daff4-115">Select **Additional security verification**.</span></span>  

    ![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. <span data-ttu-id="daff4-117">Další bezpečnostní ověření stránka načte s nastavením.</span><span class="sxs-lookup"><span data-stu-id="daff4-117">The Additional security verification page loads with your settings.</span></span>

    ![Ověření](./media/multi-factor-authentication-end-user-manage/proofup.png)

## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a><span data-ttu-id="daff4-119">Chcete změnit své telefonní číslo nebo přidat sekundární číslo</span><span class="sxs-lookup"><span data-stu-id="daff4-119">I want to change my phone number, or add a secondary number</span></span>
<span data-ttu-id="daff4-120">Je potřeba nakonfigurovat telefonní číslo sekundární ověřování.</span><span class="sxs-lookup"><span data-stu-id="daff4-120">It is important to configure a secondary authentication phone number.</span></span>  <span data-ttu-id="daff4-121">Protože vaše primární telefonní číslo a mobilní aplikace jsou pravděpodobně na stejné telefonu, sekundární telefonní číslo je jediným způsobem, jak bude moct vrátit ke svému účtu, pokud dojde ke ztrátě nebo odcizení vašeho telefonu.</span><span class="sxs-lookup"><span data-stu-id="daff4-121">Because your primary phone number and your mobile app are probably on the same phone, the secondary phone number is the only way you will be able to get back into your account if your phone is lost or stolen.</span></span>

> [!NOTE]
> <span data-ttu-id="daff4-122">Pokud nebudete mít přístup k vaší primární telefonní číslo a potřebujete pomoc, získávání ve ke svému účtu, najdete v našich témata nápovědy v [došlo k potížím s dvoustupnovym overovanim](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="daff4-122">If you don't have access to your primary phone number, and need help getting in to your account, see our help topics in [Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md).</span></span>  

<span data-ttu-id="daff4-123">**Chcete-li změnit primární telefonní číslo:**</span><span class="sxs-lookup"><span data-stu-id="daff4-123">**To change your primary phone number:**</span></span>  

1. <span data-ttu-id="daff4-124">Na stránce další bezpečnostní ověření vyberte textové pole s vaší aktuální telefonní číslo a upravit ho pomocí nové telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="daff4-124">On the Additional security verification page, select the text box with your current phone number and edit it with your new phone number.</span></span>  
2. <span data-ttu-id="daff4-125">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="daff4-125">Select **Save**.</span></span>  
3. <span data-ttu-id="daff4-126">Pokud je to počet, který používáte pro vaše upřednostňovanou možnost ověřování, je nutné ověřit nové číslo předtím, než jste jej uložili.</span><span class="sxs-lookup"><span data-stu-id="daff4-126">If this is the number that you use for your preferred verification option, you have to verify the new number before you can save it.</span></span>  

<span data-ttu-id="daff4-127">**Chcete-li přidat sekundární telefonní číslo:**</span><span class="sxs-lookup"><span data-stu-id="daff4-127">**To add a secondary phone number:**</span></span>  

1. <span data-ttu-id="daff4-128">Na stránce další bezpečnostní ověření, zaškrtněte políčko vedle **telefon pro alternativní ověření.**</span><span class="sxs-lookup"><span data-stu-id="daff4-128">On the Additional security verification page, check the box next to **Alternate authentication phone.**</span></span>  
2. <span data-ttu-id="daff4-129">Do textového pole zadejte sekundární telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="daff4-129">Enter your secondary phone number in the text box.</span></span>  
3. <span data-ttu-id="daff4-130">Vyberte **Uložit** a dokončení změny.</span><span class="sxs-lookup"><span data-stu-id="daff4-130">Select **Save** and your changes are finished.</span></span>  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a><span data-ttu-id="daff4-131">Vyžadovat dvoustupňové ověření znovu v zařízení, které jste označili jako důvěryhodný</span><span class="sxs-lookup"><span data-stu-id="daff4-131">Require two-step verification again on a device you've marked as trusted</span></span>

<span data-ttu-id="daff4-132">V závislosti na nastavení organizace může mít zaškrtávací políčko, která uvádí, že "nezobrazovat dotaz dalších **X** dní" při provádění dvoustupňové ověření v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="daff4-132">Depending on your organization settings, you may have a checkbox that says "Don't ask again for **X** days" when you perform two-step verification on your browser.</span></span> <span data-ttu-id="daff4-133">Pokud toto políčko zaškrtněte a pak ztráty zařízení nebo myslíte, že je váš účet ohrožená, obnovíte dvoustupňové ověřování pro všechna vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="daff4-133">If you check this box and then lose your device or think that your account is compromised, you should restore two-step verification to all your devices.</span></span> 

1. <span data-ttu-id="daff4-134">Na stránce další bezpečnostní ověření, vyberte **obnovení služby Multi-Factor authentication u dřív důvěryhodných zařízení**.</span><span class="sxs-lookup"><span data-stu-id="daff4-134">On the Additional security verification page, select **Restore multi-factor authentication on previously trusted devices**.</span></span>
2. <span data-ttu-id="daff4-135">Při příštím přihlášení na libovolném zařízení, budete vyzváni k provedení dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="daff4-135">The next time you sign in on any device, you'll be prompted to perform two-step verification.</span></span> 

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a><span data-ttu-id="daff4-136">Jak vyčistit Microsoft Authenticator z původního zařízení a přesunout do nového?</span><span class="sxs-lookup"><span data-stu-id="daff4-136">How do I clean up Microsoft Authenticator from my old device and move to a new one?</span></span>
<span data-ttu-id="daff4-137">Po odinstalaci aplikace ze zařízení nebo se zařízení resetovat, neodebere aktivace na back-end.</span><span class="sxs-lookup"><span data-stu-id="daff4-137">When you uninstall the app from your device or reset the device, it does not remove the activation on the back end.</span></span> <span data-ttu-id="daff4-138">Další informace najdete v tématu [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="daff4-138">For more information, see [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="daff4-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="daff4-139">Next steps</span></span>
* <span data-ttu-id="daff4-140">Získat tipy pro odstraňování potíží a nápovědy na [došlo k potížím s dvoustupňové ověření](multi-factor-authentication-end-user-troubleshoot.md)</span><span class="sxs-lookup"><span data-stu-id="daff4-140">Get troubleshooting tips and help on [Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md)</span></span>
* <span data-ttu-id="daff4-141">Nastavit [hesla aplikací](multi-factor-authentication-end-user-app-passwords.md) pro všechny aplikace, které nepodporují dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="daff4-141">Set up [app passwords](multi-factor-authentication-end-user-app-passwords.md) for any apps that don't support two-step verification.</span></span>
