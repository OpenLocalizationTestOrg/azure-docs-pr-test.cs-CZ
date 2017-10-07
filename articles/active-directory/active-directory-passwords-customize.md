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
ms.openlocfilehash: 4762fffef040f9b409355f9ee0e8cc593e3eea0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a><span data-ttu-id="f35c4-103">Přizpůsobení funkce služby Azure AD pro samoobslužného resetování hesel</span><span class="sxs-lookup"><span data-stu-id="f35c4-103">Customize Azure AD functionality for Self-Service Password Reset</span></span>

<span data-ttu-id="f35c4-104">Odborníci v oblasti IT vyhledávání toodeploy samoobslužné resetování hesla můžete přizpůsobit toomatch hello zkušeností uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f35c4-104">IT Professionals looking toodeploy self-service password reset can customize hello experience toomatch their users.</span></span>

## <a name="customize-hello-contact-your-administrator-link"></a><span data-ttu-id="f35c4-105">Přizpůsobení hello obraťte se na odkaz na vaši správce</span><span class="sxs-lookup"><span data-stu-id="f35c4-105">Customize hello contact your administrator link</span></span>

<span data-ttu-id="f35c4-106">I když SSPR nejsou povolení uživatelé stále "obraťte se na správce" odkaz na hello heslo resetovat portálu.</span><span class="sxs-lookup"><span data-stu-id="f35c4-106">Even if SSPR is not enabled users still a "contact your administrator" link on hello password reset portal.</span></span>  <span data-ttu-id="f35c4-107">Kliknutím na tento odkaz e-maily správce žádostí o pomoc při změně hesla uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="f35c4-107">Clicking this link emails your administrators asking for assistance in changing hello user's password.</span></span> <span data-ttu-id="f35c4-108">Tento e-mail je odeslán toohello následující příjemci hello následující pořadí:</span><span class="sxs-lookup"><span data-stu-id="f35c4-108">This email is sent toohello following recipients in hello following order:</span></span>

1. <span data-ttu-id="f35c4-109">Pokud hello **heslo správce** role je přiřazená, správci k této roli jsou upozorněni.</span><span class="sxs-lookup"><span data-stu-id="f35c4-109">If hello **Password administrator** role is assigned, administrators with this role are notified</span></span>
2. <span data-ttu-id="f35c4-110">Pokud žádní správci heslo přiřazené, pak správcům hello **správce uživatele** role jsou upozorněni.</span><span class="sxs-lookup"><span data-stu-id="f35c4-110">If no Password administrators are assigned, then administrators with hello **User administrator** role are notified</span></span>
3. <span data-ttu-id="f35c4-111">Pokud žádná z předchozích role hello byly přiřazeny, pak **globální správci** jsou oznámení</span><span class="sxs-lookup"><span data-stu-id="f35c4-111">If neither of hello previous roles were assigned, then **Global administrators** are notified</span></span>

<span data-ttu-id="f35c4-112">Ve všech případech jsou maximálně 100 příjemce oznámení.</span><span class="sxs-lookup"><span data-stu-id="f35c4-112">In all cases, a maximum of 100 recipients are notified.</span></span>

<span data-ttu-id="f35c4-113">Další informace o rolích jiný správce hello a jak hello tooassign je najdete v dokumentu toofind [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles.md)</span><span class="sxs-lookup"><span data-stu-id="f35c4-113">toofind out more about hello different administrator roles and how tooassign them see hello document [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md)</span></span>

### <a name="disable-contact-your-administrator-emails"></a><span data-ttu-id="f35c4-114">Obraťte se na vaši e-mailů správce zakázat</span><span class="sxs-lookup"><span data-stu-id="f35c4-114">Disable contact your administrator emails</span></span>

<span data-ttu-id="f35c4-115">Pokud vaše organizace nechce, že správci oznámení o heslo resetovat požadavky, hello následující konfiguraci můžete povolit.</span><span class="sxs-lookup"><span data-stu-id="f35c4-115">If your organization does not want administrators notified about password reset requests, hello following configuration can be enabled</span></span>

* <span data-ttu-id="f35c4-116">Povolte samoobslužné resetování hesla pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="f35c4-116">Enable self-service password reset for all end users.</span></span> <span data-ttu-id="f35c4-117">Tato možnost je v části **resetování hesla > vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="f35c4-117">This option is under **Password Reset > Properties**.</span></span>
    * <span data-ttu-id="f35c4-118">Pokud nechcete, aby uživatelé tooreset vlastní hesla, můžete určit obor přístupu tooan prázdné skupiny **nedoporučujeme tuto možnost**.</span><span class="sxs-lookup"><span data-stu-id="f35c4-118">If you do not wish users tooreset their own passwords, you can scope access tooan empty group **we do not recommend this option**.</span></span>
* <span data-ttu-id="f35c4-119">Přizpůsobení hello helpdesk odkaz tooprovide adresa URL webové nebo mailto: adresy, které uživatelé mohou používat tooget pomoc.</span><span class="sxs-lookup"><span data-stu-id="f35c4-119">Customize hello helpdesk link tooprovide a web URL or mailto: address that users can use tooget assistance.</span></span> <span data-ttu-id="f35c4-120">Tato možnost je v části **resetování hesla > Přizpůsobení > vlastní technické podpory e-mailu nebo adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="f35c4-120">This option is under **Password Reset > Customization > Custom helpdesk email or URL**.</span></span>

## <a name="customize-adfs-sign-in-page-for-sspr"></a><span data-ttu-id="f35c4-121">Přizpůsobit přihlašovací stránku služby AD FS pro SSPR</span><span class="sxs-lookup"><span data-stu-id="f35c4-121">Customize ADFS sign-in page for SSPR</span></span>

<span data-ttu-id="f35c4-122">Správci služby AD FS můžete přidat odkaz tootheir přihlašovací stránku podle pokynů hello najít v článku hello [přidat přihlašovací stránku popis](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).</span><span class="sxs-lookup"><span data-stu-id="f35c4-122">ADFS Administrators can add a link tootheir sign-in page using hello guidance found in hello article [Add sign-in page description](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).</span></span>

<span data-ttu-id="f35c4-123">Pomocí příkazu hello, který odpovídá na serveru AD FS přidá odkaz toohello služby AD FS přihlašovací stránky umožňuje uživatelům tooenter hello hesla pomocí samoobslužné služby pracovního postupu resetování přímo.</span><span class="sxs-lookup"><span data-stu-id="f35c4-123">Using hello command that follows on your ADFS server adds a link toohello ADFS login page allowing users tooenter hello self-service password reset workflow directly.</span></span>

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-hello-sign-in-and-access-panel-look-and-feel"></a><span data-ttu-id="f35c4-124">Přizpůsobení hello přihlášení a přístup k panelu Vzhled a chování</span><span class="sxs-lookup"><span data-stu-id="f35c4-124">Customize hello sign-in and access panel look and feel</span></span>

<span data-ttu-id="f35c4-125">Když uživatelé přistoupí k hello přihlašovací stránky, můžete přizpůsobit logo hello, který se zobrazí spolu s toofit image přihlašovací stránku hello značky společnosti.</span><span class="sxs-lookup"><span data-stu-id="f35c4-125">When your users access hello login page, you can customize hello logo that appears along with hello sign-in page image toofit your company branding.</span></span>

<span data-ttu-id="f35c4-126">Tato grafika jsou zobrazeny v hello následující okolnosti:</span><span class="sxs-lookup"><span data-stu-id="f35c4-126">These graphics are shown in hello following circumstances:</span></span>

* <span data-ttu-id="f35c4-127">Poté, co uživatel zadá své uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="f35c4-127">After a user types their username</span></span>
* <span data-ttu-id="f35c4-128">Uživatel získá přístup k vlastní adresu url</span><span class="sxs-lookup"><span data-stu-id="f35c4-128">User accesses customized url</span></span>
    * <span data-ttu-id="f35c4-129">Pomocí předání hello resetování hesla toohello parametr "Wh" stránky, jako je "https://login.microsoftonline.com/?whr=contoso.com"</span><span class="sxs-lookup"><span data-stu-id="f35c4-129">By passing hello "whr" parameter toohello password reset page, like "https://login.microsoftonline.com/?whr=contoso.com"</span></span>
    * <span data-ttu-id="f35c4-130">Pomocí předání hello "username" parametr toohello resetování hesla stránky, jako je třeba "https://login.microsoftonline.com/?username=admin@contoso.com"</span><span class="sxs-lookup"><span data-stu-id="f35c4-130">By passing hello "username" parameter toohello password reset page, like "https://login.microsoftonline.com/?username=admin@contoso.com"</span></span>

### <a name="graphics-details"></a><span data-ttu-id="f35c4-131">Podrobnosti o grafiky</span><span class="sxs-lookup"><span data-stu-id="f35c4-131">Graphics details</span></span>

<span data-ttu-id="f35c4-132">Hello následující nastavení umožňují toochange hello vizuální vlastnosti přihlašovací stránku hello a najdete v části **Azure Active Directory**, **firemní branding**, **upravit společnosti Branding**</span><span class="sxs-lookup"><span data-stu-id="f35c4-132">hello following settings allow you toochange hello visual characteristics of hello sign-in page and can be found under **Azure Active Directory**, **Company branding**, **Edit company branding**</span></span>

* <span data-ttu-id="f35c4-133">Obrázek přihlašovací stránky by měl být PNG nebo JPG souboru 1420 × 1200 pixelů a ne větší než 500KB.</span><span class="sxs-lookup"><span data-stu-id="f35c4-133">Sign-in page image should be a PNG or JPG file 1420x1200 pixels and no larger than 500KB.</span></span> <span data-ttu-id="f35c4-134">Doporučujeme, abyste ho toobe přibližně 200 KB pro dosažení co nejlepších výsledků.</span><span class="sxs-lookup"><span data-stu-id="f35c4-134">We recommend it toobe around 200 KB for best results.</span></span>
* <span data-ttu-id="f35c4-135">Barva pozadí přihlašovací stránky se používá v s vysokou latencí připojení a musí být v šestnáctkovém formátu RGB hello.</span><span class="sxs-lookup"><span data-stu-id="f35c4-135">Sign-in page background color is used when on high-latency connections and must be in hello RGB hex format.</span></span>
* <span data-ttu-id="f35c4-136">Obrázek hlavičky by měl být PNG nebo JPG souboru 60 x 280 pixelů a ne větší než 10 KB.</span><span class="sxs-lookup"><span data-stu-id="f35c4-136">Banner image should be a PNG or JPG file 60x280 pixels and no larger than 10 KB.</span></span>
* <span data-ttu-id="f35c4-137">Odmocnina logo (normální a tmavým motivem) PNG nebo JPG 240 x 240 (s možností změny velikosti) nesmí být větší než 10 KB.</span><span class="sxs-lookup"><span data-stu-id="f35c4-137">Square logo (normal and dark theme) PNG or JPG 240x240 (resizable) no larger than 10 KB.</span></span>

### <a name="sign-in-text-options"></a><span data-ttu-id="f35c4-138">Možnosti přihlášení textu</span><span class="sxs-lookup"><span data-stu-id="f35c4-138">Sign-in text options</span></span>

<span data-ttu-id="f35c4-139">Hello následující nastavení povolí tooadd text toohello přihlašovací stránce relevantní tooyour organizace.</span><span class="sxs-lookup"><span data-stu-id="f35c4-139">hello following settings allow you tooadd text toohello sign-in page relevant tooyour organization.</span></span> <span data-ttu-id="f35c4-140">Tato nastavení najdete v části **Azure Active Directory**, **firemní branding**, **upravit firemního brandingu**</span><span class="sxs-lookup"><span data-stu-id="f35c4-140">These settings can be found under **Azure Active Directory**, **Company branding**, **Edit company branding**</span></span>

* <span data-ttu-id="f35c4-141">**Pomocný parametr název uživatele** nahradí hello příklad text someone@example.com s něco vhodnější pro vaše uživatele, doporučujeme levém výchozí toobe při podpoře interních a externích uživatelů</span><span class="sxs-lookup"><span data-stu-id="f35c4-141">**User name hint** replaces hello example text of someone@example.com with something more appropriate for your users, recommended toobe left default when supporting internal and external users</span></span>
* <span data-ttu-id="f35c4-142">**Text přihlašovací stránky** je maximálně 256 znaků.</span><span class="sxs-lookup"><span data-stu-id="f35c4-142">**Sign-in page text** is a maximum of 256 characters in length.</span></span> <span data-ttu-id="f35c4-143">Tento text se zobrazí kdekoli přihlašování uživatelů online a v hello Azure AD Join prostředí ve Windows 10.</span><span class="sxs-lookup"><span data-stu-id="f35c4-143">This text appears anywhere your users login online, and in hello Azure AD Join experience on Windows 10.</span></span> <span data-ttu-id="f35c4-144">Použijte tento text pro podmínky použití, pokyny a tipy pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="f35c4-144">Use this text for terms of use, instructions, and tips for your users.</span></span> <span data-ttu-id="f35c4-145">**Kdo, zobrazit stránku přihlášení, neposkytují žádné citlivé informace v tomto poli.**</span><span class="sxs-lookup"><span data-stu-id="f35c4-145">**Anyone can see your sign-in page so do not provide any sensitive information here.**</span></span>

### <a name="keep-me-signed-in-disabled"></a><span data-ttu-id="f35c4-146">Možnost zůstat přihlášení je vypnutá.</span><span class="sxs-lookup"><span data-stu-id="f35c4-146">Keep me signed in disabled</span></span>

<span data-ttu-id="f35c4-147">možnost Hello "zůstat přihlášeni zakázáno" umožňuje uživatelům tooremain při jejich zavřete a znovu otevřete okno jejich prohlížeče přihlášený.</span><span class="sxs-lookup"><span data-stu-id="f35c4-147">hello option "Keep me signed in disabled" allows users tooremain signed in when they close and reopen their browser window.</span></span> <span data-ttu-id="f35c4-148">Tato možnost neměla vliv trvání relace.</span><span class="sxs-lookup"><span data-stu-id="f35c4-148">This option does not impact session lifetimes.</span></span> <span data-ttu-id="f35c4-149">Toto nastavení je v části **Azure Active Directory > firemní branding > Upravit firemního brandingu**.</span><span class="sxs-lookup"><span data-stu-id="f35c4-149">This setting is found under **Azure Active Directory > Company branding > Edit company branding**.</span></span>

<span data-ttu-id="f35c4-150">Některé funkce služby SharePoint Online a Office 2010 jsou závislé na uživatele, je možné toocheck toto políčko.</span><span class="sxs-lookup"><span data-stu-id="f35c4-150">Some features of SharePoint Online and Office 2010 have a dependency on users being able toocheck this box.</span></span> <span data-ttu-id="f35c4-151">Pokud je skrýt tuto možnost, mohou uživatelé získat další a neočekávané přihlášení výzvy.</span><span class="sxs-lookup"><span data-stu-id="f35c4-151">If you hide this option, users may get additional and unexpected sign-in prompts.</span></span>

### <a name="directory-name"></a><span data-ttu-id="f35c4-152">Název adresáře</span><span class="sxs-lookup"><span data-stu-id="f35c4-152">Directory name</span></span>

<span data-ttu-id="f35c4-153">Můžete změnit atribut name hello pod **Azure Active Directory > vlastnosti** tooshow organizace popisný název vidět v portálu hello a automatizovat komunikace.</span><span class="sxs-lookup"><span data-stu-id="f35c4-153">You can change hello name attribute under **Azure Active Directory > Properties** tooshow a friendly organization name seen in hello portal and automated communications.</span></span> <span data-ttu-id="f35c4-154">Tato možnost je nejvíce viditelné v podobě hello automatizovaných e-mailů ve formulářích hello, které následují</span><span class="sxs-lookup"><span data-stu-id="f35c4-154">This option is most visible in hello form of automated emails in hello forms that follow</span></span>

* <span data-ttu-id="f35c4-155">Popisný název v e-mailu "Microsoft jménem ukázkové společnosti CONTOSO"</span><span class="sxs-lookup"><span data-stu-id="f35c4-155">Friendly name in email “Microsoft on behalf of CONTOSO demo”</span></span>
* <span data-ttu-id="f35c4-156">Řádek předmětu e-mailem "CONTOSO ukázkový e-mailu ověřovací kód účtu"</span><span class="sxs-lookup"><span data-stu-id="f35c4-156">Subject line in email “CONTOSO demo account email verification code”</span></span>

## <a name="next-steps"></a><span data-ttu-id="f35c4-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f35c4-157">Next steps</span></span>

<span data-ttu-id="f35c4-158">Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f35c4-158">hello following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="f35c4-159">[**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f35c4-159">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="f35c4-160">[**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f35c4-160">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="f35c4-161">[**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel</span><span class="sxs-lookup"><span data-stu-id="f35c4-161">[**Data**](active-directory-passwords-data.md) - Understand hello data that is required and how it is used for password management</span></span>
* <span data-ttu-id="f35c4-162">[**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden</span><span class="sxs-lookup"><span data-stu-id="f35c4-162">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR tooyour users using hello guidance found here</span></span>
* <span data-ttu-id="f35c4-163">[**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f35c4-163">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="f35c4-164">[**Zpětný zápis hesla**](active-directory-passwords-writeback.md) – Způsob fungování zpětného zápisu hesla v místním adresáři.</span><span class="sxs-lookup"><span data-stu-id="f35c4-164">[**Password Writeback**](active-directory-passwords-writeback.md) - How does password writeback work with your on-premises directory</span></span>
* <span data-ttu-id="f35c4-165">[**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="f35c4-165">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="f35c4-166">[**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje</span><span class="sxs-lookup"><span data-stu-id="f35c4-166">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind hello curtain toounderstand how it works</span></span>
* <span data-ttu-id="f35c4-167">[**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak?</span><span class="sxs-lookup"><span data-stu-id="f35c4-167">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="f35c4-168">Proč?</span><span class="sxs-lookup"><span data-stu-id="f35c4-168">Why?</span></span> <span data-ttu-id="f35c4-169">Co?</span><span class="sxs-lookup"><span data-stu-id="f35c4-169">What?</span></span> <span data-ttu-id="f35c4-170">Kde?</span><span class="sxs-lookup"><span data-stu-id="f35c4-170">Where?</span></span> <span data-ttu-id="f35c4-171">Kdo?</span><span class="sxs-lookup"><span data-stu-id="f35c4-171">Who?</span></span> <span data-ttu-id="f35c4-172">Kdy?</span><span class="sxs-lookup"><span data-stu-id="f35c4-172">When?</span></span> <span data-ttu-id="f35c4-173">-Odpovědi tooquestions vždy chtěli tooask</span><span class="sxs-lookup"><span data-stu-id="f35c4-173">- Answers tooquestions you always wanted tooask</span></span>
* <span data-ttu-id="f35c4-174">[**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR</span><span class="sxs-lookup"><span data-stu-id="f35c4-174">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how tooresolve common issues that we see with SSPR</span></span>

