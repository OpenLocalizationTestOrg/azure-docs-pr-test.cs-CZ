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
ms.openlocfilehash: 4fed3a1c690fd6423ee5d3e5baef690d8896fbe9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-azure-ad-self-service-password-reset"></a><span data-ttu-id="c688c-103">Rychlý start: Samoobslužné resetování hesla Azure AD</span><span class="sxs-lookup"><span data-stu-id="c688c-103">Quickstart: Azure AD self-service password reset</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c688c-104">**Jste tady, protože máte potíže s přihlášením?**</span><span class="sxs-lookup"><span data-stu-id="c688c-104">**Are you here because you're having problems signing in?**</span></span> <span data-ttu-id="c688c-105">Pokud ano, [přečtěte si informace o tom, jak můžete změnit a resetovat vlastní heslo](active-directory-passwords-update-your-own-password.md).</span><span class="sxs-lookup"><span data-stu-id="c688c-105">If so, [here's how you can change and reset your own password](active-directory-passwords-update-your-own-password.md).</span></span>

## <a name="rapidly-deploy-self-service-password-reset"></a><span data-ttu-id="c688c-106">Rychlé nasazené samoobslužného resetování hesla</span><span class="sxs-lookup"><span data-stu-id="c688c-106">Rapidly deploy self-service password reset</span></span>

<span data-ttu-id="c688c-107">Samoobslužného obnovení hesla (SSPR) nabízí jednoduchý znamená, že pro tooreset uživatelé tooempower správci IT nebo odemknutí jejich hesla nebo účty.</span><span class="sxs-lookup"><span data-stu-id="c688c-107">Self-service password reset (SSPR) offers a simple means for IT administrators tooempower users tooreset or unlock their passwords or accounts.</span></span> <span data-ttu-id="c688c-108">Hello systému zahrnuje podrobné sestavy tootrack uživatelům při používání systému hello společně s tooalert oznámení můžete toomisuse nebo zneužití.</span><span class="sxs-lookup"><span data-stu-id="c688c-108">hello system includes detailed reporting tootrack when users use hello system along with notifications tooalert you toomisuse or abuse.</span></span>

<span data-ttu-id="c688c-109">Tento průvodce předpokládá, že již máte funkčního tenanta Azure AD ve zkušební verzi nebo s licencí.</span><span class="sxs-lookup"><span data-stu-id="c688c-109">This guide assumes you already have a working trial or licensed Azure AD tenant.</span></span> <span data-ttu-id="c688c-110">Pokud potřebujete pomoc, nastavení služby Azure AD, najdete v článku hello [Začínáme s Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/).</span><span class="sxs-lookup"><span data-stu-id="c688c-110">If you need help setting up Azure AD, see hello article [Getting Started with Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/).</span></span>

1. <span data-ttu-id="c688c-111">Ve svém existujícím tenantovi Azure AD vyberte **Resetování hesla**.</span><span class="sxs-lookup"><span data-stu-id="c688c-111">From your existing Azure AD tenant, select **"Password reset"**</span></span>

2. <span data-ttu-id="c688c-112">Z hello **"Vlastnosti"** obrazovky v části "Samoobslužné služby heslo resetovat povolené" vyberte jednu z následujících hello možnost hello</span><span class="sxs-lookup"><span data-stu-id="c688c-112">From hello **"Properties"** screen, under hello option "Self Service Password Reset Enabled" choose one of hello following</span></span>
    * <span data-ttu-id="c688c-113">Nikdo - žádné jeden je možné toouse SSPR funkce</span><span class="sxs-lookup"><span data-stu-id="c688c-113">Nobody - No one is able toouse SSPR functionality</span></span>
    * <span data-ttu-id="c688c-114">Skupina – konkrétní Azure AD pouze členové skupiny, abyste zvolili jsou možné toouse SSPR funkce</span><span class="sxs-lookup"><span data-stu-id="c688c-114">A group - Only members of a specific Azure AD group that you choose are able toouse SSPR functionality</span></span>
    * <span data-ttu-id="c688c-115">Každý uživatel - všichni uživatelé s účty v klientovi služby Azure AD jsou možné toouse SSPR funkce</span><span class="sxs-lookup"><span data-stu-id="c688c-115">Everybody - All users with accounts in your Azure AD tenant are able toouse SSPR functionality</span></span>

3. <span data-ttu-id="c688c-116">Z hello **"Metody ověřování"** zvolte obrazovky</span><span class="sxs-lookup"><span data-stu-id="c688c-116">From hello **"Authentication methods"** screen choose</span></span>
    * <span data-ttu-id="c688c-117">Počet metody požadováno tooreset – podporujeme alespoň jednu nebo dvě maximálně</span><span class="sxs-lookup"><span data-stu-id="c688c-117">Number of methods required tooreset - We support a minimum of one or a maximum of two</span></span>
    * <span data-ttu-id="c688c-118">Metody dostupné toousers - potřebujeme alespoň jeden ale ho nikdy škodí jak toohave se navíc řešením, které jsou k dispozici</span><span class="sxs-lookup"><span data-stu-id="c688c-118">Methods available toousers - We need at least one but it never hurts toohave an extra choice available</span></span>
        * <span data-ttu-id="c688c-119">**E-mailu** odešle e-mail s uživatelem toohello kód je nakonfigurované ověřování e-mailová adresa</span><span class="sxs-lookup"><span data-stu-id="c688c-119">**Email** sends an email with a code toohello user's configured authentication email address</span></span>
        * <span data-ttu-id="c688c-120">**Mobilní telefon** poskytuje hello uživatele hello volba tooreceive volání nebo textu pomocí kódu tootheir nakonfigurovaná mobilní telefonní číslo</span><span class="sxs-lookup"><span data-stu-id="c688c-120">**Mobile Phone** gives hello user hello choice tooreceive a call or text with a code tootheir configured mobile phone number</span></span>
        * <span data-ttu-id="c688c-121">**Telefon do kanceláře** volání hello uživatele s tootheir kód nakonfigurované office telefonní číslo</span><span class="sxs-lookup"><span data-stu-id="c688c-121">**Office Phone** calls hello user with a code tootheir configured office phone number</span></span>
        * <span data-ttu-id="c688c-122">**Bezpečnostní otázky** vyžaduje, abyste toochoose</span><span class="sxs-lookup"><span data-stu-id="c688c-122">**Security Questions** requires you toochoose</span></span>
            * <span data-ttu-id="c688c-123">Počet otázek vyžadovaných tooregister - hello minimální pro úspěšnou registraci, což znamená, uživatel může vybrat tooanswer další toocreate otázky, které toopull z fondu.</span><span class="sxs-lookup"><span data-stu-id="c688c-123">Number of questions required tooregister - hello minimum for successful registration, meaning a user can choose tooanswer more toocreate a pool of questions toopull from.</span></span> <span data-ttu-id="c688c-124">Tuto možnost můžete nastavit od 3 až 5 a musí být větší než nebo roven hodnotě toohello počet tooreset požadované otázky.</span><span class="sxs-lookup"><span data-stu-id="c688c-124">This option can be set from 3-5 and must be greater than or equal toohello number of questions required tooreset.</span></span>
                * <span data-ttu-id="c688c-125">Když kliknete na tlačítko "Vlastní" hello, při výběru bezpečnostní otázky lze přidat vlastní dotazy</span><span class="sxs-lookup"><span data-stu-id="c688c-125">Custom questions can be added by clicking hello "Custom" button when selecting security questions</span></span>
            * <span data-ttu-id="c688c-126">Počet otázek vyžadovaných tooreset – můžete nastavit od 3 až 5 otázky toobe správně zodpovězena, před povolením heslo toobe uživatelů, resetování nebo odemknout.</span><span class="sxs-lookup"><span data-stu-id="c688c-126">Number of questions required tooreset - Can be set from 3-5 questions toobe answered correctly before allowing a users password toobe reset or unlocked.</span></span>

4. <span data-ttu-id="c688c-127">Doporučené: **"Vlastní"** vám umožní toochange hello "Obraťte se na správce" odkaz toopoint tooa stránky nebo e-mailovou adresu definujete</span><span class="sxs-lookup"><span data-stu-id="c688c-127">RECOMMENDED: **"Customization"** allows you toochange hello "Contact your administrator" link toopoint tooa page or email address you define</span></span>

5. <span data-ttu-id="c688c-128">Volitelné: hello **"Registrace"** obrazovky poskytuje správcům hello možnosti:</span><span class="sxs-lookup"><span data-stu-id="c688c-128">OPTIONAL: hello **"Registration"** screen provides administrators hello options for:</span></span>
    * <span data-ttu-id="c688c-129">Vyžadovat tooregister uživatele při přihlášení</span><span class="sxs-lookup"><span data-stu-id="c688c-129">Require users tooregister when signing in</span></span>
    * <span data-ttu-id="c688c-130">Počet dní, než budou uživatelé vyzváni tooreconfirm své informace o ověřování</span><span class="sxs-lookup"><span data-stu-id="c688c-130">Number of days before users are asked tooreconfirm their authentication information</span></span>

6. <span data-ttu-id="c688c-131">Volitelné: hello **"Oznámení"** obrazovky poskytuje správcům možnost hello:</span><span class="sxs-lookup"><span data-stu-id="c688c-131">OPTIONAL: hello **"Notification"** screen provides administrators hello options to:</span></span>
    * <span data-ttu-id="c688c-132">Upozornit uživatele na resetování hesla</span><span class="sxs-lookup"><span data-stu-id="c688c-132">Notify users on password resets</span></span>
    * <span data-ttu-id="c688c-133">Upozornit všechny správce na resetování hesla jiného správce</span><span class="sxs-lookup"><span data-stu-id="c688c-133">Notify all admins when other admins reset their password</span></span>

<span data-ttu-id="c688c-134">**Právě jste pro svého tenanta Azure AD nakonfigurovali samoobslužné resetování hesla**.</span><span class="sxs-lookup"><span data-stu-id="c688c-134">**At this point, you have configured SSPR for your Azure AD tenant**.</span></span> <span data-ttu-id="c688c-135">Můžete zde zastavit nebo pokračovat v tooconfigure synchronizace tooan hesla místní domény služby AD.</span><span class="sxs-lookup"><span data-stu-id="c688c-135">You can stop here or continue on tooconfigure synchronization of passwords tooan on-premises AD domain.</span></span>

> [!NOTE]
> <span data-ttu-id="c688c-136">Otestujte samoobslužné resetování hesla pomocí uživatele, a ne správce, protože Microsoft pro účty typu správce Azure vynucuje požadavky na silné ověřování.</span><span class="sxs-lookup"><span data-stu-id="c688c-136">Test SSPR with a user and not an administrator as Microsoft enforces strong authentication requirements for Azure administrator type accounts.</span></span> <span data-ttu-id="c688c-137">Další informace týkající se zásad hesla správce hello najdete v tématu naše [článek zásady hesla](active-directory-passwords-policy.md#administrator-password-policy-differences).</span><span class="sxs-lookup"><span data-stu-id="c688c-137">For more information regarding hello administrator password policy, see our [password policy article](active-directory-passwords-policy.md#administrator-password-policy-differences).</span></span>

## <a name="configure-synchronization-tooexisting-identity-source"></a><span data-ttu-id="c688c-138">Nastavit zdroj synchronizací tooexisting identity</span><span class="sxs-lookup"><span data-stu-id="c688c-138">Configure synchronization tooexisting identity source</span></span>

<span data-ttu-id="c688c-139">tooenable místní tooAzure synchronizaci identit AD, je nutné tooinstall a nakonfigurovat [Azure AD Connect](./connect/active-directory-aadconnect.md) na serveru ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="c688c-139">tooenable on-premises identity synchronization tooAzure AD, you need tooinstall and configure [Azure AD Connect](./connect/active-directory-aadconnect.md) on a server in your organization.</span></span> <span data-ttu-id="c688c-140">Tato aplikace zpracovává synchronizaci uživatelů a skupin ze stávající klientovi Azure AD zdroj tooyour identity.</span><span class="sxs-lookup"><span data-stu-id="c688c-140">This application handles synchronizing users and groups from your existing identity source tooyour Azure AD tenant.</span></span>

* [<span data-ttu-id="c688c-141">Upgrade z nástroje DirSync nebo Azure AD Sync tooAzure AD Connect</span><span class="sxs-lookup"><span data-stu-id="c688c-141">Upgrade from DirSync or Azure AD Sync tooAzure AD Connect</span></span>](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [<span data-ttu-id="c688c-142">Začínáme se službou Azure AD Connect s použitím expresního nastavení</span><span class="sxs-lookup"><span data-stu-id="c688c-142">Getting started with Azure AD Connect using express settings</span></span>](./connect/active-directory-aadconnect-get-started-express.md)
* <span data-ttu-id="c688c-143">[Nakonfigurovat zpětný zápis hesla](active-directory-passwords-writeback.md#configuring-password-writeback) toowrite hesla z Azure AD zpátky tooyour místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="c688c-143">[Configure password writeback](active-directory-passwords-writeback.md#configuring-password-writeback) toowrite passwords from Azure AD back tooyour on-premises directory.</span></span>

## <a name="disabling-self-service-password-reset"></a><span data-ttu-id="c688c-144">Zakázání samoobslužného resetování hesla</span><span class="sxs-lookup"><span data-stu-id="c688c-144">Disabling self-service password reset</span></span>

<span data-ttu-id="c688c-145">Zakázání samoobslužné resetování hesla je jednoduché, otevřete klientovi Azure AD a budete příliš**resetování hesla > vlastnosti** > vyberte **žádné** pod **samoobslužného resetování hesla Povoleno**</span><span class="sxs-lookup"><span data-stu-id="c688c-145">Disabling self-service password reset is as simple as opening your Azure AD tenant and going too**Password Reset > Properties** > choose **None** under **Self Service Password Reset Enabled**</span></span>

### <a name="learn-more"></a><span data-ttu-id="c688c-146">Další informace</span><span class="sxs-lookup"><span data-stu-id="c688c-146">Learn more</span></span>
<span data-ttu-id="c688c-147">Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c688c-147">hello following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="c688c-148">[**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c688c-148">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="c688c-149">[**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel</span><span class="sxs-lookup"><span data-stu-id="c688c-149">[**Data**](active-directory-passwords-data.md) - Understand hello data that is required and how it is used for password management</span></span>
* <span data-ttu-id="c688c-150">[**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden</span><span class="sxs-lookup"><span data-stu-id="c688c-150">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR tooyour users using hello guidance found here</span></span>
* <span data-ttu-id="c688c-151">[**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.</span><span class="sxs-lookup"><span data-stu-id="c688c-151">[**Customize**](active-directory-passwords-customize.md) - Customize hello look and feel of hello SSPR experience for your company.</span></span>
* <span data-ttu-id="c688c-152">[**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c688c-152">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="c688c-153">[**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="c688c-153">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="c688c-154">[**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje</span><span class="sxs-lookup"><span data-stu-id="c688c-154">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind hello curtain toounderstand how it works</span></span>
* <span data-ttu-id="c688c-155">[**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak?</span><span class="sxs-lookup"><span data-stu-id="c688c-155">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="c688c-156">Proč?</span><span class="sxs-lookup"><span data-stu-id="c688c-156">Why?</span></span> <span data-ttu-id="c688c-157">Co?</span><span class="sxs-lookup"><span data-stu-id="c688c-157">What?</span></span> <span data-ttu-id="c688c-158">Kde?</span><span class="sxs-lookup"><span data-stu-id="c688c-158">Where?</span></span> <span data-ttu-id="c688c-159">Kdo?</span><span class="sxs-lookup"><span data-stu-id="c688c-159">Who?</span></span> <span data-ttu-id="c688c-160">Kdy?</span><span class="sxs-lookup"><span data-stu-id="c688c-160">When?</span></span> <span data-ttu-id="c688c-161">-Odpovědi tooquestions vždy chtěli tooask</span><span class="sxs-lookup"><span data-stu-id="c688c-161">- Answers tooquestions you always wanted tooask</span></span>
* <span data-ttu-id="c688c-162">[**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR</span><span class="sxs-lookup"><span data-stu-id="c688c-162">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how tooresolve common issues that we see with SSPR</span></span>

## <a name="next-steps"></a><span data-ttu-id="c688c-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c688c-163">Next steps</span></span>

<span data-ttu-id="c688c-164">V tento rychlý start když jste se naučili jak resetování hesla pomocí samoobslužné služby tooconfigure pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="c688c-164">In this quickstart, you’ve learned how tooconfigure self-service password reset for your users.</span></span> <span data-ttu-id="c688c-165">toocontinue toohello Azure portálu toocomplete, postupovat podle těchto kroků hello odkaz níže toohello portálu.</span><span class="sxs-lookup"><span data-stu-id="c688c-165">toocontinue toohello Azure portal toocomplete these steps follow hello link below toohello portal.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c688c-166">Povolení samoobslužného resetování hesla</span><span class="sxs-lookup"><span data-stu-id="c688c-166">Enable self-service password reset</span></span>](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

