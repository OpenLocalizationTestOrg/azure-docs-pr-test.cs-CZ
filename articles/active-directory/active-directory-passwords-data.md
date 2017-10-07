---
title: "Požadavky na Azure AD SSPR dat | Microsoft Docs"
description: "Obnovení dat požadavky pro hesla pomocí samoobslužné služby Azure AD a jak toosatisfy je"
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
ms.openlocfilehash: b68a1d7914dcd0bb4509d0e94914dc4309f4463a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a><span data-ttu-id="ecff8-103">Nasazení bez nutnosti registrace koncového uživatele pro vytvoření nového hesla</span><span class="sxs-lookup"><span data-stu-id="ecff8-103">Deploy password reset without requiring end-user registration</span></span>

<span data-ttu-id="ecff8-104">Nasazení samoobslužného resetování hesla (SSPR) vyžaduje ověřování dat toobe přítomen.</span><span class="sxs-lookup"><span data-stu-id="ecff8-104">Deploying Self-Service Password Reset (SSPR) requires authentication data toobe present.</span></span> <span data-ttu-id="ecff8-105">Některé organizace mají své uživatele, zadejte svoje data ověřování sami, ale celá řada organizací preferuje toosynchronize s existujícími daty ve službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ecff8-105">Some organizations have their users enter their authentication data themselves, but many organizations prefer toosynchronize with existing data in Active Directory.</span></span> <span data-ttu-id="ecff8-106">Pokud jste správně formátovaných dat ve vašem místním adresáři a nakonfigurovat [Azure AD Connect pomocí expresního nastavení](./connect/active-directory-aadconnect-get-started-express.md), že data budou k dispozici tooAzure AD a SSPR se nevyžaduje zásah uživatele.</span><span class="sxs-lookup"><span data-stu-id="ecff8-106">If you have properly formatted data in your on-premises directory, and configure [Azure AD Connect using express settings](./connect/active-directory-aadconnect-get-started-express.md), that data is made available tooAzure AD and SSPR with no user interaction required.</span></span>

<span data-ttu-id="ecff8-107">Žádné telefonní číslo musí být ve formátu hello + CountryCode PhoneNumber příklad: + 1 4255551234 toowork správně.</span><span class="sxs-lookup"><span data-stu-id="ecff8-107">Any phone numbers must be in hello format +CountryCode PhoneNumber Example: +1 4255551234 toowork properly.</span></span>

> [!NOTE]
> <span data-ttu-id="ecff8-108">Resetování hesla nepodporuje telefonní klapky.</span><span class="sxs-lookup"><span data-stu-id="ecff8-108">Password reset does not support phone extensions.</span></span> <span data-ttu-id="ecff8-109">I v hello 12345 formátu 4255551234 + 1 X předtím, než je uskutečněn hovor hello odebrané rozšíření.</span><span class="sxs-lookup"><span data-stu-id="ecff8-109">Even in hello +1 4255551234X12345 format, extensions are removed before hello call is placed.</span></span>

## <a name="fields-populated"></a><span data-ttu-id="ecff8-110">Pole se vyplní</span><span class="sxs-lookup"><span data-stu-id="ecff8-110">Fields populated</span></span>

<span data-ttu-id="ecff8-111">Pokud použijete výchozí nastavení hello v Azure AD Connect hello následující mapování jsou vytvářeny.</span><span class="sxs-lookup"><span data-stu-id="ecff8-111">If you use hello default settings in Azure AD Connect hello following mappings are made.</span></span>

| <span data-ttu-id="ecff8-112">Místní AD</span><span class="sxs-lookup"><span data-stu-id="ecff8-112">On-premises AD</span></span> | <span data-ttu-id="ecff8-113">Azure AD</span><span class="sxs-lookup"><span data-stu-id="ecff8-113">Azure AD</span></span> | <span data-ttu-id="ecff8-114">Azure AD Authentication kontaktní údaje</span><span class="sxs-lookup"><span data-stu-id="ecff8-114">Azure AD Authentication contact info</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ecff8-115">telephoneNumber</span><span class="sxs-lookup"><span data-stu-id="ecff8-115">telephoneNumber</span></span> | <span data-ttu-id="ecff8-116">Telefon do kanceláře</span><span class="sxs-lookup"><span data-stu-id="ecff8-116">Office phone</span></span> | <span data-ttu-id="ecff8-117">Alternativní telefonní</span><span class="sxs-lookup"><span data-stu-id="ecff8-117">Alternate phone</span></span> |
| <span data-ttu-id="ecff8-118">mobilní</span><span class="sxs-lookup"><span data-stu-id="ecff8-118">mobile</span></span> | <span data-ttu-id="ecff8-119">Mobilní telefon</span><span class="sxs-lookup"><span data-stu-id="ecff8-119">Mobile phone</span></span> | <span data-ttu-id="ecff8-120">Telefon</span><span class="sxs-lookup"><span data-stu-id="ecff8-120">Phone</span></span> |


## <a name="security-questions-and-answers"></a><span data-ttu-id="ecff8-121">Bezpečnostní otázky a odpovědi</span><span class="sxs-lookup"><span data-stu-id="ecff8-121">Security questions and answers</span></span>

<span data-ttu-id="ecff8-122">Bezpečnostní otázky a odpovědi jsou bezpečně uloženy v klientovi služby Azure AD a jsou pouze přístupné toousers prostřednictvím hello [portálu pro registraci SSPR](https://aka.ms/ssprsetup).</span><span class="sxs-lookup"><span data-stu-id="ecff8-122">Security questions and answers are stored securely in your Azure AD tenant and are only accessible toousers via hello [SSPR registration portal](https://aka.ms/ssprsetup).</span></span> <span data-ttu-id="ecff8-123">Správci nelze zobrazit ani upravit obsah hello jiného uživatele otázek a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="ecff8-123">Administrators can't see or modify hello contents of another users questions and answers.</span></span>

### <a name="what-happens-when-a-user-registers"></a><span data-ttu-id="ecff8-124">Co se stane, když se uživatel zaregistruje</span><span class="sxs-lookup"><span data-stu-id="ecff8-124">What happens when a user registers</span></span>

<span data-ttu-id="ecff8-125">Když se uživatel zaregistruje, nastaví hello registrační stránku hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="ecff8-125">When a user registers, hello registration page sets hello following fields:</span></span>

* <span data-ttu-id="ecff8-126">Telefon pro ověření</span><span class="sxs-lookup"><span data-stu-id="ecff8-126">Authentication Phone</span></span>
* <span data-ttu-id="ecff8-127">Ověření e-mailu</span><span class="sxs-lookup"><span data-stu-id="ecff8-127">Authentication Email</span></span>
* <span data-ttu-id="ecff8-128">Bezpečnostní otázky a odpovědi</span><span class="sxs-lookup"><span data-stu-id="ecff8-128">Security Questions and Answers</span></span>

<span data-ttu-id="ecff8-129">Pokud jste zadali hodnotu **mobilní telefon** nebo **alternativní e-mailovou**, uživatelé mohou okamžitě používat tyto hodnoty tooreset jejich hesla, i v případě, že nebyly registrovány pro službu hello.</span><span class="sxs-lookup"><span data-stu-id="ecff8-129">If you have provided a value for **Mobile Phone** or **Alternate Email**, users can immediately use those values tooreset their passwords, even if they haven't registered for hello service.</span></span> <span data-ttu-id="ecff8-130">Kromě toho uživatele při registraci pro hello poprvé, najdete v části tyto hodnoty a je upravit, pokud si přejí.</span><span class="sxs-lookup"><span data-stu-id="ecff8-130">In addition, users see those values when registering for hello first time, and modify them if they wish.</span></span> <span data-ttu-id="ecff8-131">Po jejich úspěšně zaregistrovat, budou tyto hodnoty zachovat v hello **telefon pro ověření** a **ověřování e-mailu** pole, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="ecff8-131">After they successfully register, these values will be persisted in hello **Authentication Phone** and **Authentication Email** fields, respectively.</span></span>

## <a name="set-and-read-authentication-data-using-powershell"></a><span data-ttu-id="ecff8-132">Nastavit a číst data ověřování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ecff8-132">Set and read authentication data using PowerShell</span></span>

<span data-ttu-id="ecff8-133">Následující pole Hello se dá nastavit pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ecff8-133">hello following fields can be set using PowerShell</span></span>

* <span data-ttu-id="ecff8-134">Alternativní e-mailu</span><span class="sxs-lookup"><span data-stu-id="ecff8-134">Alternate Email</span></span>
* <span data-ttu-id="ecff8-135">Mobilní telefon</span><span class="sxs-lookup"><span data-stu-id="ecff8-135">Mobile Phone</span></span>
* <span data-ttu-id="ecff8-136">Telefon do kanceláře - lze nastavit pouze v případě není synchronizaci s místním adresářem</span><span class="sxs-lookup"><span data-stu-id="ecff8-136">Office Phone - Can only be set if not synchronizing with an on-premises directory</span></span>

### <a name="using-powershell-v1"></a><span data-ttu-id="ecff8-137">Pomocí prostředí PowerShell V1</span><span class="sxs-lookup"><span data-stu-id="ecff8-137">Using PowerShell V1</span></span>

<span data-ttu-id="ecff8-138">tooget spuštění, budete potřebovat příliš[stáhněte a nainstalujte modul Azure AD PowerShell hello](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).</span><span class="sxs-lookup"><span data-stu-id="ecff8-138">tooget started, you need too[download and install hello Azure AD PowerShell module](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).</span></span> <span data-ttu-id="ecff8-139">Až budete mít nainstalováno, můžete provést hello kroky, které následují tooconfigure každé pole.</span><span class="sxs-lookup"><span data-stu-id="ecff8-139">Once you have it installed, you can follow hello steps that follow tooconfigure each field.</span></span>

#### <a name="set-authentication-data-with-powershell-v1"></a><span data-ttu-id="ecff8-140">Nastavení ověření dat pomocí prostředí PowerShell V1</span><span class="sxs-lookup"><span data-stu-id="ecff8-140">Set Authentication Data with PowerShell V1</span></span>

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-authentication-data-with-powershellpowershell-v1"></a><span data-ttu-id="ecff8-141">Čtení dat ověřování pomocí PowerShellPowerShell V1</span><span class="sxs-lookup"><span data-stu-id="ecff8-141">Read Authentication Data with PowerShellPowerShell V1</span></span>

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="authentication-phone-and-authentication-email-can-only-be-read-using-powershell-v1-using-hello-commands-that-follow"></a><span data-ttu-id="ecff8-142">Telefon pro ověření a e-mailu ověřování lze číst pouze pomocí prostředí Powershell V1 hello pomocí následujících příkazů</span><span class="sxs-lookup"><span data-stu-id="ecff8-142">Authentication Phone and Authentication Email can only be read using Powershell V1 using hello commands that follow</span></span>

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="using-powershell-v2"></a><span data-ttu-id="ecff8-143">Pomocí prostředí PowerShell V2</span><span class="sxs-lookup"><span data-stu-id="ecff8-143">Using PowerShell V2</span></span>

<span data-ttu-id="ecff8-144">tooget spuštění, budete potřebovat příliš[stáhněte a nainstalujte modul prostředí PowerShell hello Azure AD V2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md).</span><span class="sxs-lookup"><span data-stu-id="ecff8-144">tooget started, you need too[download and install hello Azure AD V2 PowerShell module](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md).</span></span> <span data-ttu-id="ecff8-145">Až budete mít nainstalováno, můžete provést hello kroky, které následují tooconfigure každé pole.</span><span class="sxs-lookup"><span data-stu-id="ecff8-145">Once you have it installed, you can follow hello steps that follow tooconfigure each field.</span></span>

<span data-ttu-id="ecff8-146">tooinstall rychle z nejnovější verze prostředí PowerShell, které podporují instalaci modulu, spuštěním těchto příkazů (první řádek hello jednoduše kontroluje toosee Pokud je už nainstalovaná):</span><span class="sxs-lookup"><span data-stu-id="ecff8-146">tooinstall quickly from recent versions of PowerShell that support Install-Module, run these commands (hello first line simply checks toosee if it's installed already):</span></span>

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-authentication-data-with-powershell-v2"></a><span data-ttu-id="ecff8-147">Nastavení ověření dat pomocí prostředí PowerShell V2</span><span class="sxs-lookup"><span data-stu-id="ecff8-147">Set Authentication Data with PowerShell V2</span></span>

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

### <a name="read-authentication-data-with-powershell-v2"></a><span data-ttu-id="ecff8-148">Čtení dat ověřování pomocí prostředí PowerShell V2</span><span class="sxs-lookup"><span data-stu-id="ecff8-148">Read Authentication Data with PowerShell V2</span></span>

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a><span data-ttu-id="ecff8-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ecff8-149">Next steps</span></span>

<span data-ttu-id="ecff8-150">Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ecff8-150">hello following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="ecff8-151">[**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ecff8-151">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="ecff8-152">[**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ecff8-152">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="ecff8-153">[**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden</span><span class="sxs-lookup"><span data-stu-id="ecff8-153">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR tooyour users using hello guidance found here</span></span>
* <span data-ttu-id="ecff8-154">[**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.</span><span class="sxs-lookup"><span data-stu-id="ecff8-154">[**Customize**](active-directory-passwords-customize.md) - Customize hello look and feel of hello SSPR experience for your company.</span></span>
* <span data-ttu-id="ecff8-155">[**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ecff8-155">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="ecff8-156">[**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="ecff8-156">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="ecff8-157">[**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje</span><span class="sxs-lookup"><span data-stu-id="ecff8-157">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind hello curtain toounderstand how it works</span></span>
* <span data-ttu-id="ecff8-158">[**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak?</span><span class="sxs-lookup"><span data-stu-id="ecff8-158">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="ecff8-159">Proč?</span><span class="sxs-lookup"><span data-stu-id="ecff8-159">Why?</span></span> <span data-ttu-id="ecff8-160">Co?</span><span class="sxs-lookup"><span data-stu-id="ecff8-160">What?</span></span> <span data-ttu-id="ecff8-161">Kde?</span><span class="sxs-lookup"><span data-stu-id="ecff8-161">Where?</span></span> <span data-ttu-id="ecff8-162">Kdo?</span><span class="sxs-lookup"><span data-stu-id="ecff8-162">Who?</span></span> <span data-ttu-id="ecff8-163">Kdy?</span><span class="sxs-lookup"><span data-stu-id="ecff8-163">When?</span></span> <span data-ttu-id="ecff8-164">-Odpovědi tooquestions vždy chtěli tooask</span><span class="sxs-lookup"><span data-stu-id="ecff8-164">- Answers tooquestions you always wanted tooask</span></span>
* <span data-ttu-id="ecff8-165">[**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR</span><span class="sxs-lookup"><span data-stu-id="ecff8-165">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how tooresolve common issues that we see with SSPR</span></span>
