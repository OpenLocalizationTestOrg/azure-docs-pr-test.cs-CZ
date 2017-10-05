---
title: "Rychlý start: Samoobslužné resetování hesla Azure AD | Dokumentace Microsoftu"
description: "Rychlé nasazené samoobslužného resetování hesla Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 07c7f3ad066c735054cb339f6e09aa4d7d23f23a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="quickstart-azure-ad-self-service-password-reset"></a><span data-ttu-id="fb6b5-103">Rychlý start: Samoobslužné resetování hesla Azure AD</span><span class="sxs-lookup"><span data-stu-id="fb6b5-103">Quickstart: Azure AD self-service password reset</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb6b5-104">**Jste tady, protože máte potíže s přihlášením?**</span><span class="sxs-lookup"><span data-stu-id="fb6b5-104">**Are you here because you're having problems signing in?**</span></span> <span data-ttu-id="fb6b5-105">Pokud ano, [přečtěte si informace o tom, jak můžete změnit a resetovat vlastní heslo](active-directory-passwords-update-your-own-password.md).</span><span class="sxs-lookup"><span data-stu-id="fb6b5-105">If so, [here's how you can change and reset your own password](active-directory-passwords-update-your-own-password.md).</span></span>

## <a name="rapidly-deploy-self-service-password-reset"></a><span data-ttu-id="fb6b5-106">Rychlé nasazené samoobslužného resetování hesla</span><span class="sxs-lookup"><span data-stu-id="fb6b5-106">Rapidly deploy self-service password reset</span></span>

<span data-ttu-id="fb6b5-107">Samoobslužné resetování hesla nabízí správcům IT jednoduchý způsob, jak umožnit uživatelům resetovat nebo odemknout svá hesla nebo účty.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-107">Self-service password reset (SSPR) offers a simple means for IT administrators to empower users to reset or unlock their passwords or accounts.</span></span> <span data-ttu-id="fb6b5-108">Tento systém zahrnuje vytváření podrobných sestav, pomocí kterých můžete sledovat, kdy uživatelé systém používají, spolu s oznámeními, která upozorňují na zneužití.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-108">The system includes detailed reporting to track when users use the system along with notifications to alert you to misuse or abuse.</span></span>

<span data-ttu-id="fb6b5-109">Tento průvodce předpokládá, že již máte funkčního tenanta Azure AD ve zkušební verzi nebo s licencí.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-109">This guide assumes you already have a working trial or licensed Azure AD tenant.</span></span> <span data-ttu-id="fb6b5-110">Pokud potřebujete pomoc s nastavením Azure AD, přečtěte si článek [Začínáme s Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/).</span><span class="sxs-lookup"><span data-stu-id="fb6b5-110">If you need help setting up Azure AD, see the article [Getting Started with Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/).</span></span>

1. <span data-ttu-id="fb6b5-111">Ve svém existujícím tenantovi Azure AD vyberte **Resetování hesla**.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-111">From your existing Azure AD tenant, select **"Password reset"**</span></span>

2. <span data-ttu-id="fb6b5-112">Na obrazovce **Vlastnosti** v části „Samoobslužné resetování hesla je povoleno“ zvolte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="fb6b5-112">From the **"Properties"** screen, under the option "Self Service Password Reset Enabled" choose one of the following</span></span>
    * <span data-ttu-id="fb6b5-113">Nikdo – Nikdo nemůže využívat funkci samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-113">Nobody - No one is able to use SSPR functionality</span></span>
    * <span data-ttu-id="fb6b5-114">Skupina – Pouze členové určité skupiny Azure AD, kterou zvolíte, můžou využívat funkci samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-114">A group - Only members of a specific Azure AD group that you choose are able to use SSPR functionality</span></span>
    * <span data-ttu-id="fb6b5-115">Všichni – Všichni uživatelé s účty ve vašem tenantovi Azure AD můžou využívat funkci samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-115">Everybody - All users with accounts in your Azure AD tenant are able to use SSPR functionality</span></span>

3. <span data-ttu-id="fb6b5-116">Na obrazovce **Metody ověření** zvolte</span><span class="sxs-lookup"><span data-stu-id="fb6b5-116">From the **"Authentication methods"** screen choose</span></span>
    * <span data-ttu-id="fb6b5-117">Počet metod požadovaných k resetování – Podporujeme minimálně jednu nebo maximálně dvě.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-117">Number of methods required to reset - We support a minimum of one or a maximum of two</span></span>
    * <span data-ttu-id="fb6b5-118">Metody dostupné pro uživatele – Potřebujeme alespoň jednu, ale nikdy neuškodí mít k dispozici jednu možnost navíc.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-118">Methods available to users - We need at least one but it never hurts to have an extra choice available</span></span>
        * <span data-ttu-id="fb6b5-119">**E-mail** odešle e-mail s kódem na nastavenou ověřovací e-mailovou adresu uživatele.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-119">**Email** sends an email with a code to the user's configured authentication email address</span></span>
        * <span data-ttu-id="fb6b5-120">**Mobilní telefon** umožňuje uživateli obdržet hovor nebo zprávu s kódem na nastavené číslo mobilního telefonu.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-120">**Mobile Phone** gives the user the choice to receive a call or text with a code to their configured mobile phone number</span></span>
        * <span data-ttu-id="fb6b5-121">**Telefon do kanceláře** zavolá uživateli na nastavené telefonní číslo do kanceláře a sdělí mu kód.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-121">**Office Phone** calls the user with a code to their configured office phone number</span></span>
        * <span data-ttu-id="fb6b5-122">**Bezpečnostní otázky** vyžadují, abyste zvolili:</span><span class="sxs-lookup"><span data-stu-id="fb6b5-122">**Security Questions** requires you to choose</span></span>
            * <span data-ttu-id="fb6b5-123">Počet otázek požadovaných k registraci – Minimum pro úspěšnou registraci. To znamená, že uživatel může odpovědět na více otázek a vytvořit tak fond, ze kterého se otázky budou načítat.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-123">Number of questions required to register - The minimum for successful registration, meaning a user can choose to answer more to create a pool of questions to pull from.</span></span> <span data-ttu-id="fb6b5-124">Tuto možnost můžete nastavit na 3 až 5 a tato hodnota musí být vyšší nebo rovna počtu otázek požadovaných k resetování.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-124">This option can be set from 3-5 and must be greater than or equal to the number of questions required to reset.</span></span>
                * <span data-ttu-id="fb6b5-125">Vlastní otázky můžete přidat kliknutím na tlačítko Vlastní při výběru bezpečnostních otázek.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-125">Custom questions can be added by clicking the "Custom" button when selecting security questions</span></span>
            * <span data-ttu-id="fb6b5-126">Počet otázek požadovaných k resetování – Můžete nastavit na 3 až 5 otázek, které je třeba správně zodpovědět, než bude umožněno resetování nebo odemknutí hesla uživatele.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-126">Number of questions required to reset - Can be set from 3-5 questions to be answered correctly before allowing a users password to be reset or unlocked.</span></span>

4. <span data-ttu-id="fb6b5-127">DOPORUČENÉ: **Přizpůsobení** umožňuje změnit odkaz „Kontaktujte správce“, aby odkazoval na stránku nebo e-mailovou adresu, kterou zadáte.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-127">RECOMMENDED: **"Customization"** allows you to change the "Contact your administrator" link to point to a page or email address you define</span></span>

5. <span data-ttu-id="fb6b5-128">VOLITELNÉ: Obrazovka **Registrace** poskytuje správcům tyto možnosti:</span><span class="sxs-lookup"><span data-stu-id="fb6b5-128">OPTIONAL: The **"Registration"** screen provides administrators the options for:</span></span>
    * <span data-ttu-id="fb6b5-129">Při přihlášení vyžadovat registraci uživatelů</span><span class="sxs-lookup"><span data-stu-id="fb6b5-129">Require users to register when signing in</span></span>
    * <span data-ttu-id="fb6b5-130">Počet dní před vyzváním uživatelů k potvrzení ověřovacích informací</span><span class="sxs-lookup"><span data-stu-id="fb6b5-130">Number of days before users are asked to reconfirm their authentication information</span></span>

6. <span data-ttu-id="fb6b5-131">VOLITELNÉ: Obrazovka **Oznámení** poskytuje správcům tyto možnosti:</span><span class="sxs-lookup"><span data-stu-id="fb6b5-131">OPTIONAL: The **"Notification"** screen provides administrators the options to:</span></span>
    * <span data-ttu-id="fb6b5-132">Upozornit uživatele na resetování hesla</span><span class="sxs-lookup"><span data-stu-id="fb6b5-132">Notify users on password resets</span></span>
    * <span data-ttu-id="fb6b5-133">Upozornit všechny správce na resetování hesla jiného správce</span><span class="sxs-lookup"><span data-stu-id="fb6b5-133">Notify all admins when other admins reset their password</span></span>

<span data-ttu-id="fb6b5-134">**Právě jste pro svého tenanta Azure AD nakonfigurovali samoobslužné resetování hesla**.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-134">**At this point, you have configured SSPR for your Azure AD tenant**.</span></span> <span data-ttu-id="fb6b5-135">Tady můžete skončit nebo pokračovat a nakonfigurovat synchronizaci hesel s místní doménou AD.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-135">You can stop here or continue on to configure synchronization of passwords to an on-premises AD domain.</span></span>

> [!NOTE]
> <span data-ttu-id="fb6b5-136">Otestujte samoobslužné resetování hesla pomocí uživatele, a ne správce, protože Microsoft pro účty typu správce Azure vynucuje požadavky na silné ověřování.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-136">Test SSPR with a user and not an administrator as Microsoft enforces strong authentication requirements for Azure administrator type accounts.</span></span> <span data-ttu-id="fb6b5-137">Další informace týkající se zásad hesel správců najdete v našem [článku o zásadách hesel](active-directory-passwords-policy.md#administrator-password-policy-differences).</span><span class="sxs-lookup"><span data-stu-id="fb6b5-137">For more information regarding the administrator password policy, see our [password policy article](active-directory-passwords-policy.md#administrator-password-policy-differences).</span></span>

## <a name="configure-synchronization-to-existing-identity-source"></a><span data-ttu-id="fb6b5-138">Konfigurace synchronizace s existujícím zdrojem identit</span><span class="sxs-lookup"><span data-stu-id="fb6b5-138">Configure synchronization to existing identity source</span></span>

<span data-ttu-id="fb6b5-139">Pokud chcete povolit místní synchronizaci identit s Azure AD, je nutné na serveru ve vaší organizaci nainstalovat a nakonfigurovat službu [Azure AD Connect](./connect/active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="fb6b5-139">To enable on-premises identity synchronization to Azure AD, you need to install and configure [Azure AD Connect](./connect/active-directory-aadconnect.md) on a server in your organization.</span></span> <span data-ttu-id="fb6b5-140">Tato aplikace se stará o synchronizaci uživatelů a skupin mezi vaším existujícím zdrojem identit a vaším tenantem Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-140">This application handles synchronizing users and groups from your existing identity source to your Azure AD tenant.</span></span>

* [<span data-ttu-id="fb6b5-141">Upgrade z nástroje DirSync nebo z Azure AD Sync na Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="fb6b5-141">Upgrade from DirSync or Azure AD Sync to Azure AD Connect</span></span>](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [<span data-ttu-id="fb6b5-142">Začínáme se službou Azure AD Connect s použitím expresního nastavení</span><span class="sxs-lookup"><span data-stu-id="fb6b5-142">Getting started with Azure AD Connect using express settings</span></span>](./connect/active-directory-aadconnect-get-started-express.md)
* <span data-ttu-id="fb6b5-143">[Nakonfigurujte zpětný zápis hesel](active-directory-passwords-writeback.md#configuring-password-writeback), aby se hesla z Azure AD zapsala zpátky do místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-143">[Configure password writeback](active-directory-passwords-writeback.md#configuring-password-writeback) to write passwords from Azure AD back to your on-premises directory.</span></span>

## <a name="disabling-self-service-password-reset"></a><span data-ttu-id="fb6b5-144">Zakázání samoobslužného resetování hesla</span><span class="sxs-lookup"><span data-stu-id="fb6b5-144">Disabling self-service password reset</span></span>

<span data-ttu-id="fb6b5-145">Zakázání samoobslužného resetování hesla je snadné – stačí otevřít vašeho tenanta Azure AD, přejít na **Resetování hesla > Vlastnosti** a v části **Samoobslužné resetování hesla povoleno** zvolit **Žádné**.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-145">Disabling self-service password reset is as simple as opening your Azure AD tenant and going to **Password Reset > Properties** > choose **None** under **Self Service Password Reset Enabled**</span></span>

### <a name="learn-more"></a><span data-ttu-id="fb6b5-146">Další informace</span><span class="sxs-lookup"><span data-stu-id="fb6b5-146">Learn more</span></span>
<span data-ttu-id="fb6b5-147">Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD</span><span class="sxs-lookup"><span data-stu-id="fb6b5-147">The following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="fb6b5-148">[**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-148">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="fb6b5-149">[**Data**](active-directory-passwords-data.md) – Pochopte požadovaná data a jejich použití pro správu hesel.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-149">[**Data**](active-directory-passwords-data.md) - Understand the data that is required and how it is used for password management</span></span>
* <span data-ttu-id="fb6b5-150">[**Uvedení**](active-directory-passwords-best-practices.md) – Naplánujte a nasaďte pro své uživatele samoobslužné resetování hesla pomocí zde uvedených pokynů.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-150">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR to your users using the guidance found here</span></span>
* <span data-ttu-id="fb6b5-151">[**Přizpůsobení**](active-directory-passwords-customize.md) – Přizpůsobte vzhled a chování samoobslužného resetování hesla pro vaši společnost.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-151">[**Customize**](active-directory-passwords-customize.md) - Customize the look and feel of the SSPR experience for your company.</span></span>
* <span data-ttu-id="fb6b5-152">[**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-152">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="fb6b5-153">[**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-153">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="fb6b5-154">[**Podrobné technické informace**](active-directory-passwords-how-it-works.md) – Nahlédněte za oponu a pochopte, jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-154">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind the curtain to understand how it works</span></span>
* <span data-ttu-id="fb6b5-155">[**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak?</span><span class="sxs-lookup"><span data-stu-id="fb6b5-155">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="fb6b5-156">Proč?</span><span class="sxs-lookup"><span data-stu-id="fb6b5-156">Why?</span></span> <span data-ttu-id="fb6b5-157">Co?</span><span class="sxs-lookup"><span data-stu-id="fb6b5-157">What?</span></span> <span data-ttu-id="fb6b5-158">Kde?</span><span class="sxs-lookup"><span data-stu-id="fb6b5-158">Where?</span></span> <span data-ttu-id="fb6b5-159">Kdo?</span><span class="sxs-lookup"><span data-stu-id="fb6b5-159">Who?</span></span> <span data-ttu-id="fb6b5-160">Kdy?</span><span class="sxs-lookup"><span data-stu-id="fb6b5-160">When?</span></span> <span data-ttu-id="fb6b5-161">– Odpovědi na otázky, na které jste se vždy chtěli zeptat.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-161">- Answers to questions you always wanted to ask</span></span>
* <span data-ttu-id="fb6b5-162">[**Řešení potíží**](active-directory-passwords-troubleshoot.md) – Zjistěte, jak řešit běžné problémy, ke kterým dochází u samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-162">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how to resolve common issues that we see with SSPR</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb6b5-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fb6b5-163">Next steps</span></span>

<span data-ttu-id="fb6b5-164">V tomto rychlém startu jste zjistili, jak pro vaše uživatele nakonfigurovat samoobslužné resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-164">In this quickstart, you’ve learned how to configure self-service password reset for your users.</span></span> <span data-ttu-id="fb6b5-165">Pokud chcete pokračovat na Azure Portal a dokončit tyto kroky, použijte následující odkaz na portál.</span><span class="sxs-lookup"><span data-stu-id="fb6b5-165">To continue to the Azure portal to complete these steps follow the link below to the portal.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fb6b5-166">Povolení samoobslužného resetování hesla</span><span class="sxs-lookup"><span data-stu-id="fb6b5-166">Enable self-service password reset</span></span>](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

