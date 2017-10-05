---
title: "Přizpůsobení: Azure AD SSPR | Microsoft Docs"
description: "Možnosti přizpůsobení pro Azure AD samoobslužném resetování hesla služby"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 8b9c120815473b25140b8717f8fdd539c97ebb04
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a><span data-ttu-id="a1dcb-103">Přizpůsobení funkce služby Azure AD pro samoobslužného resetování hesel</span><span class="sxs-lookup"><span data-stu-id="a1dcb-103">Customize Azure AD functionality for Self-Service Password Reset</span></span>

<span data-ttu-id="a1dcb-104">Odborníci v oblasti IT chtějí nasadit samoobslužné resetování hesla můžete přizpůsobit prostředí tak, aby odpovídaly uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-104">IT Professionals looking to deploy self-service password reset can customize the experience to match their users.</span></span>

## <a name="customize-the-contact-your-administrator-link"></a><span data-ttu-id="a1dcb-105">Přizpůsobení kontakt odkaz na vaši správce</span><span class="sxs-lookup"><span data-stu-id="a1dcb-105">Customize the contact your administrator link</span></span>

<span data-ttu-id="a1dcb-106">I když není povolená SSPR uživatelé stále "obraťte se na správce" odkaz na heslo resetovat portálu.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-106">Even if SSPR is not enabled users still a "contact your administrator" link on the password reset portal.</span></span>  <span data-ttu-id="a1dcb-107">Kliknutím na tento odkaz e-maily správce žádostí o pomoc při změně hesla uživatele.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-107">Clicking this link emails your administrators asking for assistance in changing the user's password.</span></span> <span data-ttu-id="a1dcb-108">Tento e-mail je odeslán následujícím příjemcům v následujícím pořadí:</span><span class="sxs-lookup"><span data-stu-id="a1dcb-108">This email is sent to the following recipients in the following order:</span></span>

1. <span data-ttu-id="a1dcb-109">Pokud **heslo správce** role je přiřazená, správci k této roli jsou upozorněni.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-109">If the **Password administrator** role is assigned, administrators with this role are notified</span></span>
2. <span data-ttu-id="a1dcb-110">Pokud žádní správci heslo přiřazené, pak správcům, kteří mají **správce uživatele** role jsou upozorněni.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-110">If no Password administrators are assigned, then administrators with the **User administrator** role are notified</span></span>
3. <span data-ttu-id="a1dcb-111">Pokud žádná z předchozích role byly přiřazeny, pak **globální správci** jsou oznámení</span><span class="sxs-lookup"><span data-stu-id="a1dcb-111">If neither of the previous roles were assigned, then **Global administrators** are notified</span></span>

<span data-ttu-id="a1dcb-112">Ve všech případech jsou maximálně 100 příjemce oznámení.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-112">In all cases, a maximum of 100 recipients are notified.</span></span>

<span data-ttu-id="a1dcb-113">Chcete-li získat další informace o různých správce rolí a jejich přiřazení naleznete v dokumentu [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles.md)</span><span class="sxs-lookup"><span data-stu-id="a1dcb-113">To find out more about the different administrator roles and how to assign them see the document [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md)</span></span>

### <a name="disable-contact-your-administrator-emails"></a><span data-ttu-id="a1dcb-114">Obraťte se na vaši e-mailů správce zakázat</span><span class="sxs-lookup"><span data-stu-id="a1dcb-114">Disable contact your administrator emails</span></span>

<span data-ttu-id="a1dcb-115">Pokud vaše organizace nechce, že správci oznámení o heslo resetovat požadavky, můžete povolit následující konfigurace</span><span class="sxs-lookup"><span data-stu-id="a1dcb-115">If your organization does not want administrators notified about password reset requests, the following configuration can be enabled</span></span>

* <span data-ttu-id="a1dcb-116">Povolte samoobslužné resetování hesla pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-116">Enable self-service password reset for all end users.</span></span> <span data-ttu-id="a1dcb-117">Tato možnost je v části **resetování hesla > vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-117">This option is under **Password Reset > Properties**.</span></span>
    * <span data-ttu-id="a1dcb-118">Pokud nechcete, aby uživatelům resetovat vlastní hesla, můžete určit obor přístupu do prázdné skupiny **nedoporučujeme tuto možnost**.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-118">If you do not wish users to reset their own passwords, you can scope access to an empty group **we do not recommend this option**.</span></span>
* <span data-ttu-id="a1dcb-119">Přizpůsobení odkaz helpdesk poskytnout adresu URL webového nebo mailto: adresa, kterou mohou uživatelé získat pomoc.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-119">Customize the helpdesk link to provide a web URL or mailto: address that users can use to get assistance.</span></span> <span data-ttu-id="a1dcb-120">Tato možnost je v části **resetování hesla > Přizpůsobení > vlastní technické podpory e-mailu nebo adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-120">This option is under **Password Reset > Customization > Custom helpdesk email or URL**.</span></span>

## <a name="customize-adfs-sign-in-page-for-sspr"></a><span data-ttu-id="a1dcb-121">Přizpůsobit přihlašovací stránku služby AD FS pro SSPR</span><span class="sxs-lookup"><span data-stu-id="a1dcb-121">Customize ADFS sign-in page for SSPR</span></span>

<span data-ttu-id="a1dcb-122">Správci služby AD FS můžete přidat odkaz na jejich přihlašovací stránku pomocí pokyny naleznete v článku [přidat přihlašovací stránku popis](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).</span><span class="sxs-lookup"><span data-stu-id="a1dcb-122">ADFS Administrators can add a link to their sign-in page using the guidance found in the article [Add sign-in page description](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).</span></span>

<span data-ttu-id="a1dcb-123">Pomocí příkazu, který odpovídá na serveru AD FS přidá odkaz na přihlašovací stránku služby AD FS umožňuje uživatelům zadat heslo samoobslužné služby pracovního postupu pro obnovení přímo.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-123">Using the command that follows on your ADFS server adds a link to the ADFS login page allowing users to enter the self-service password reset workflow directly.</span></span>

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-the-sign-in-and-access-panel-look-and-feel"></a><span data-ttu-id="a1dcb-124">Přizpůsobení přihlašování a přístup k panelu vzhledu a chování</span><span class="sxs-lookup"><span data-stu-id="a1dcb-124">Customize the sign-in and access panel look and feel</span></span>

<span data-ttu-id="a1dcb-125">Pokud vaši uživatelé přístup k přihlašovací stránky, můžete přizpůsobit logo, které se zobrazí spolu s bitovou kopií přihlašovací stránce podle značky společnosti.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-125">When your users access the login page, you can customize the logo that appears along with the sign-in page image to fit your company branding.</span></span>

<span data-ttu-id="a1dcb-126">Tato grafika jsou uvedeny v následujících případech:</span><span class="sxs-lookup"><span data-stu-id="a1dcb-126">These graphics are shown in the following circumstances:</span></span>

* <span data-ttu-id="a1dcb-127">Poté, co uživatel zadá své uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="a1dcb-127">After a user types their username</span></span>
* <span data-ttu-id="a1dcb-128">Uživatel získá přístup k vlastní adresu url</span><span class="sxs-lookup"><span data-stu-id="a1dcb-128">User accesses customized url</span></span>
    * <span data-ttu-id="a1dcb-129">Předáním "Wh" parametru heslo resetovat stránky, jako je "https://login.microsoftonline.com/?whr=contoso.com"</span><span class="sxs-lookup"><span data-stu-id="a1dcb-129">By passing the "whr" parameter to the password reset page, like "https://login.microsoftonline.com/?whr=contoso.com"</span></span>
    * <span data-ttu-id="a1dcb-130">Předáním "username" parametru heslo resetovat stránky, jako je třeba "https://login.microsoftonline.com/?username=admin@contoso.com"</span><span class="sxs-lookup"><span data-stu-id="a1dcb-130">By passing the "username" parameter to the password reset page, like "https://login.microsoftonline.com/?username=admin@contoso.com"</span></span>

### <a name="graphics-details"></a><span data-ttu-id="a1dcb-131">Podrobnosti o grafiky</span><span class="sxs-lookup"><span data-stu-id="a1dcb-131">Graphics details</span></span>

<span data-ttu-id="a1dcb-132">Tato nastavení umožňují změnit vizuální vlastnosti na přihlašovací stránku a najdete v části **Azure Active Directory**, **firemní branding**, **upravit firemního brandingu**</span><span class="sxs-lookup"><span data-stu-id="a1dcb-132">The following settings allow you to change the visual characteristics of the sign-in page and can be found under **Azure Active Directory**, **Company branding**, **Edit company branding**</span></span>

* <span data-ttu-id="a1dcb-133">Obrázek přihlašovací stránky by měl být PNG nebo JPG souboru 1420 × 1200 pixelů a ne větší než 500KB.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-133">Sign-in page image should be a PNG or JPG file 1420x1200 pixels and no larger than 500KB.</span></span> <span data-ttu-id="a1dcb-134">Doporučujeme, abyste ho na přibližně 200 KB pro dosažení co nejlepších výsledků.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-134">We recommend it to be around 200 KB for best results.</span></span>
* <span data-ttu-id="a1dcb-135">Barva pozadí přihlašovací stránky se používá v s vysokou latencí připojení a musí být v šestnáctkovém formátu RGB.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-135">Sign-in page background color is used when on high-latency connections and must be in the RGB hex format.</span></span>
* <span data-ttu-id="a1dcb-136">Obrázek hlavičky by měl být PNG nebo JPG souboru 60 x 280 pixelů a ne větší než 10 KB.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-136">Banner image should be a PNG or JPG file 60x280 pixels and no larger than 10 KB.</span></span>
* <span data-ttu-id="a1dcb-137">Odmocnina logo (normální a tmavým motivem) PNG nebo JPG 240 x 240 (s možností změny velikosti) nesmí být větší než 10 KB.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-137">Square logo (normal and dark theme) PNG or JPG 240x240 (resizable) no larger than 10 KB.</span></span>

### <a name="sign-in-text-options"></a><span data-ttu-id="a1dcb-138">Možnosti přihlášení textu</span><span class="sxs-lookup"><span data-stu-id="a1dcb-138">Sign-in text options</span></span>

<span data-ttu-id="a1dcb-139">Tato nastavení umožňují přidat text na přihlašovací stránku pro vaši organizaci relevantní.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-139">The following settings allow you to add text to the sign-in page relevant to your organization.</span></span> <span data-ttu-id="a1dcb-140">Tato nastavení najdete v části **Azure Active Directory**, **firemní branding**, **upravit firemního brandingu**</span><span class="sxs-lookup"><span data-stu-id="a1dcb-140">These settings can be found under **Azure Active Directory**, **Company branding**, **Edit company branding**</span></span>

* <span data-ttu-id="a1dcb-141">**Pomocný parametr název uživatele** nahradí text příklad someone@example.com s něco vhodnější pro vaše uživatele, doporučuje ponechat výchozí při podpoře interních a externích uživatelů</span><span class="sxs-lookup"><span data-stu-id="a1dcb-141">**User name hint** replaces the example text of someone@example.com with something more appropriate for your users, recommended to be left default when supporting internal and external users</span></span>
* <span data-ttu-id="a1dcb-142">**Text přihlašovací stránky** je maximálně 256 znaků.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-142">**Sign-in page text** is a maximum of 256 characters in length.</span></span> <span data-ttu-id="a1dcb-143">Tento text se zobrazí odkudkoli přihlašování uživatelů online a v prostředí Azure AD Join ve Windows 10.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-143">This text appears anywhere your users login online, and in the Azure AD Join experience on Windows 10.</span></span> <span data-ttu-id="a1dcb-144">Použijte tento text pro podmínky použití, pokyny a tipy pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-144">Use this text for terms of use, instructions, and tips for your users.</span></span> <span data-ttu-id="a1dcb-145">**Kdo, zobrazit stránku přihlášení, neposkytují žádné citlivé informace v tomto poli.**</span><span class="sxs-lookup"><span data-stu-id="a1dcb-145">**Anyone can see your sign-in page so do not provide any sensitive information here.**</span></span>

### <a name="keep-me-signed-in-disabled"></a><span data-ttu-id="a1dcb-146">Možnost zůstat přihlášení je vypnutá.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-146">Keep me signed in disabled</span></span>

<span data-ttu-id="a1dcb-147">Možnost "Keep mi přihlášení zakázáno" umožňuje uživatelům zůstanou přihlášeného při jejich zavřete a znovu otevřete okno jejich prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-147">The option "Keep me signed in disabled" allows users to remain signed in when they close and reopen their browser window.</span></span> <span data-ttu-id="a1dcb-148">Tato možnost neměla vliv trvání relace.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-148">This option does not impact session lifetimes.</span></span> <span data-ttu-id="a1dcb-149">Toto nastavení je v části **Azure Active Directory > firemní branding > Upravit firemního brandingu**.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-149">This setting is found under **Azure Active Directory > Company branding > Edit company branding**.</span></span>

<span data-ttu-id="a1dcb-150">Některé funkce služby SharePoint Online a Office 2010 jsou závislé na uživatelé moct toto políčko zaškrtněte.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-150">Some features of SharePoint Online and Office 2010 have a dependency on users being able to check this box.</span></span> <span data-ttu-id="a1dcb-151">Pokud je skrýt tuto možnost, mohou uživatelé získat další a neočekávané přihlášení výzvy.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-151">If you hide this option, users may get additional and unexpected sign-in prompts.</span></span>

### <a name="directory-name"></a><span data-ttu-id="a1dcb-152">Název adresáře</span><span class="sxs-lookup"><span data-stu-id="a1dcb-152">Directory name</span></span>

<span data-ttu-id="a1dcb-153">Můžete změnit atribut názvu pod **Azure Active Directory > vlastnosti** zobrazíte organizace popisný název vidět v portálu a automatizované komunikaci.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-153">You can change the name attribute under **Azure Active Directory > Properties** to show a friendly organization name seen in the portal and automated communications.</span></span> <span data-ttu-id="a1dcb-154">Tato možnost je nejvíce viditelné ve formě automatické e-maily ve formulářích, které následují</span><span class="sxs-lookup"><span data-stu-id="a1dcb-154">This option is most visible in the form of automated emails in the forms that follow</span></span>

* <span data-ttu-id="a1dcb-155">Popisný název v e-mailu "Microsoft jménem ukázkové společnosti CONTOSO"</span><span class="sxs-lookup"><span data-stu-id="a1dcb-155">Friendly name in email “Microsoft on behalf of CONTOSO demo”</span></span>
* <span data-ttu-id="a1dcb-156">Řádek předmětu e-mailem "CONTOSO ukázkový e-mailu ověřovací kód účtu"</span><span class="sxs-lookup"><span data-stu-id="a1dcb-156">Subject line in email “CONTOSO demo account email verification code”</span></span>

## <a name="next-steps"></a><span data-ttu-id="a1dcb-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a1dcb-157">Next steps</span></span>

<span data-ttu-id="a1dcb-158">Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD</span><span class="sxs-lookup"><span data-stu-id="a1dcb-158">The following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="a1dcb-159">[**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-159">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="a1dcb-160">[**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-160">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="a1dcb-161">[**Data**](active-directory-passwords-data.md) – Pochopte požadovaná data a jejich použití pro správu hesel.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-161">[**Data**](active-directory-passwords-data.md) - Understand the data that is required and how it is used for password management</span></span>
* <span data-ttu-id="a1dcb-162">[**Uvedení**](active-directory-passwords-best-practices.md) – Naplánujte a nasaďte pro své uživatele samoobslužné resetování hesla pomocí zde uvedených pokynů.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-162">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR to your users using the guidance found here</span></span>
* <span data-ttu-id="a1dcb-163">[**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-163">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="a1dcb-164">[**Zpětný zápis hesla**](active-directory-passwords-writeback.md) – Způsob fungování zpětného zápisu hesla v místním adresáři.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-164">[**Password Writeback**](active-directory-passwords-writeback.md) - How does password writeback work with your on-premises directory</span></span>
* <span data-ttu-id="a1dcb-165">[**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-165">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="a1dcb-166">[**Podrobné technické informace**](active-directory-passwords-how-it-works.md) – Nahlédněte za oponu a pochopte, jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-166">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind the curtain to understand how it works</span></span>
* <span data-ttu-id="a1dcb-167">[**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak?</span><span class="sxs-lookup"><span data-stu-id="a1dcb-167">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="a1dcb-168">Proč?</span><span class="sxs-lookup"><span data-stu-id="a1dcb-168">Why?</span></span> <span data-ttu-id="a1dcb-169">Co?</span><span class="sxs-lookup"><span data-stu-id="a1dcb-169">What?</span></span> <span data-ttu-id="a1dcb-170">Kde?</span><span class="sxs-lookup"><span data-stu-id="a1dcb-170">Where?</span></span> <span data-ttu-id="a1dcb-171">Kdo?</span><span class="sxs-lookup"><span data-stu-id="a1dcb-171">Who?</span></span> <span data-ttu-id="a1dcb-172">Kdy?</span><span class="sxs-lookup"><span data-stu-id="a1dcb-172">When?</span></span> <span data-ttu-id="a1dcb-173">– Odpovědi na otázky, na které jste se vždy chtěli zeptat.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-173">- Answers to questions you always wanted to ask</span></span>
* <span data-ttu-id="a1dcb-174">[**Řešení potíží**](active-directory-passwords-troubleshoot.md) – Zjistěte, jak řešit běžné problémy, ke kterým dochází u samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="a1dcb-174">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how to resolve common issues that we see with SSPR</span></span>

