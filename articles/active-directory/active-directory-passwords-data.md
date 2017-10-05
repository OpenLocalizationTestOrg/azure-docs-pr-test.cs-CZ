---
title: "Požadavky na Azure AD SSPR dat | Microsoft Docs"
description: "Požadavky na data pro hesla pomocí samoobslužné služby Azure AD pro resetování a postup jejich splnění"
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
ms.openlocfilehash: 2d1afd2d1265b371e0d311ed70fffbc55874b0a7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a><span data-ttu-id="4e9f9-103">Nasazení bez nutnosti registrace koncového uživatele pro vytvoření nového hesla</span><span class="sxs-lookup"><span data-stu-id="4e9f9-103">Deploy password reset without requiring end-user registration</span></span>

<span data-ttu-id="4e9f9-104">Nasazení samoobslužného resetování hesla (SSPR) vyžaduje ověřování dat být k dispozici.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-104">Deploying Self-Service Password Reset (SSPR) requires authentication data to be present.</span></span> <span data-ttu-id="4e9f9-105">Některé organizace mají své uživatele, zadejte svoje data ověřování sami, ale v mnoha organizacích je synchronizovat s existujícími daty ve službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-105">Some organizations have their users enter their authentication data themselves, but many organizations prefer to synchronize with existing data in Active Directory.</span></span> <span data-ttu-id="4e9f9-106">Pokud jste správně formátovaných dat ve vašem místním adresáři a nakonfigurovat [Azure AD Connect pomocí expresního nastavení](./connect/active-directory-aadconnect-get-started-express.md), že dat je k dispozici pro Azure AD a SSPR se nevyžaduje zásah uživatele.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-106">If you have properly formatted data in your on-premises directory, and configure [Azure AD Connect using express settings](./connect/active-directory-aadconnect-get-started-express.md), that data is made available to Azure AD and SSPR with no user interaction required.</span></span>

<span data-ttu-id="4e9f9-107">Žádné telefonní číslo musí být ve formátu + CountryCode PhoneNumber příklad: + 1 4255551234 správně fungovat.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-107">Any phone numbers must be in the format +CountryCode PhoneNumber Example: +1 4255551234 to work properly.</span></span>

> [!NOTE]
> <span data-ttu-id="4e9f9-108">Resetování hesla nepodporuje telefonní klapky.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-108">Password reset does not support phone extensions.</span></span> <span data-ttu-id="4e9f9-109">I ve formátu 12345 4255551234 + 1 X jsou rozšíření odebrat před uvedením volání.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-109">Even in the +1 4255551234X12345 format, extensions are removed before the call is placed.</span></span>

## <a name="fields-populated"></a><span data-ttu-id="4e9f9-110">Pole se vyplní</span><span class="sxs-lookup"><span data-stu-id="4e9f9-110">Fields populated</span></span>

<span data-ttu-id="4e9f9-111">Pokud použijete výchozí nastavení v Azure AD Connect jsou provedeny následující mapování.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-111">If you use the default settings in Azure AD Connect the following mappings are made.</span></span>

| <span data-ttu-id="4e9f9-112">Místní AD</span><span class="sxs-lookup"><span data-stu-id="4e9f9-112">On-premises AD</span></span> | <span data-ttu-id="4e9f9-113">Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e9f9-113">Azure AD</span></span> | <span data-ttu-id="4e9f9-114">Azure AD Authentication kontaktní údaje</span><span class="sxs-lookup"><span data-stu-id="4e9f9-114">Azure AD Authentication contact info</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4e9f9-115">telephoneNumber</span><span class="sxs-lookup"><span data-stu-id="4e9f9-115">telephoneNumber</span></span> | <span data-ttu-id="4e9f9-116">Telefon do kanceláře</span><span class="sxs-lookup"><span data-stu-id="4e9f9-116">Office phone</span></span> | <span data-ttu-id="4e9f9-117">Alternativní telefonní</span><span class="sxs-lookup"><span data-stu-id="4e9f9-117">Alternate phone</span></span> |
| <span data-ttu-id="4e9f9-118">mobilní</span><span class="sxs-lookup"><span data-stu-id="4e9f9-118">mobile</span></span> | <span data-ttu-id="4e9f9-119">Mobilní telefon</span><span class="sxs-lookup"><span data-stu-id="4e9f9-119">Mobile phone</span></span> | <span data-ttu-id="4e9f9-120">Telefon</span><span class="sxs-lookup"><span data-stu-id="4e9f9-120">Phone</span></span> |


## <a name="security-questions-and-answers"></a><span data-ttu-id="4e9f9-121">Bezpečnostní otázky a odpovědi</span><span class="sxs-lookup"><span data-stu-id="4e9f9-121">Security questions and answers</span></span>

<span data-ttu-id="4e9f9-122">Bezpečnostní otázky a odpovědi jsou bezpečně uloženy v klientovi služby Azure AD a jsou přístupné prostřednictvím jenom [portálu pro registraci SSPR](https://aka.ms/ssprsetup).</span><span class="sxs-lookup"><span data-stu-id="4e9f9-122">Security questions and answers are stored securely in your Azure AD tenant and are only accessible to users via the [SSPR registration portal](https://aka.ms/ssprsetup).</span></span> <span data-ttu-id="4e9f9-123">Správci nelze zobrazit ani upravovat obsah jiného uživatele otázky a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-123">Administrators can't see or modify the contents of another users questions and answers.</span></span>

### <a name="what-happens-when-a-user-registers"></a><span data-ttu-id="4e9f9-124">Co se stane, když se uživatel zaregistruje</span><span class="sxs-lookup"><span data-stu-id="4e9f9-124">What happens when a user registers</span></span>

<span data-ttu-id="4e9f9-125">Když se uživatel zaregistruje, nastavuje registrační stránce následující pole:</span><span class="sxs-lookup"><span data-stu-id="4e9f9-125">When a user registers, the registration page sets the following fields:</span></span>

* <span data-ttu-id="4e9f9-126">Telefon pro ověření</span><span class="sxs-lookup"><span data-stu-id="4e9f9-126">Authentication Phone</span></span>
* <span data-ttu-id="4e9f9-127">Ověření e-mailu</span><span class="sxs-lookup"><span data-stu-id="4e9f9-127">Authentication Email</span></span>
* <span data-ttu-id="4e9f9-128">Bezpečnostní otázky a odpovědi</span><span class="sxs-lookup"><span data-stu-id="4e9f9-128">Security Questions and Answers</span></span>

<span data-ttu-id="4e9f9-129">Pokud jste zadali hodnotu **mobilní telefon** nebo **alternativní e-mailovou**, uživatelé mohou okamžitě pomocí tyto hodnoty resetovat vlastní hesla i v případě, že nebyly registrovány pro službu.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-129">If you have provided a value for **Mobile Phone** or **Alternate Email**, users can immediately use those values to reset their passwords, even if they haven't registered for the service.</span></span> <span data-ttu-id="4e9f9-130">Kromě toho uživatelé zobrazit tyto hodnoty při první registraci a je upravit, pokud si přejí.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-130">In addition, users see those values when registering for the first time, and modify them if they wish.</span></span> <span data-ttu-id="4e9f9-131">Po jejich úspěšně zaregistrovat, budou tyto hodnoty v jako trvalý **telefon pro ověření** a **ověřování e-mailu** pole, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-131">After they successfully register, these values will be persisted in the **Authentication Phone** and **Authentication Email** fields, respectively.</span></span>

## <a name="set-and-read-authentication-data-using-powershell"></a><span data-ttu-id="4e9f9-132">Nastavit a číst data ověřování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4e9f9-132">Set and read authentication data using PowerShell</span></span>

<span data-ttu-id="4e9f9-133">Následující pole se dá nastavit pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4e9f9-133">The following fields can be set using PowerShell</span></span>

* <span data-ttu-id="4e9f9-134">Alternativní e-mailu</span><span class="sxs-lookup"><span data-stu-id="4e9f9-134">Alternate Email</span></span>
* <span data-ttu-id="4e9f9-135">Mobilní telefon</span><span class="sxs-lookup"><span data-stu-id="4e9f9-135">Mobile Phone</span></span>
* <span data-ttu-id="4e9f9-136">Telefon do kanceláře - lze nastavit pouze v případě není synchronizaci s místním adresářem</span><span class="sxs-lookup"><span data-stu-id="4e9f9-136">Office Phone - Can only be set if not synchronizing with an on-premises directory</span></span>

### <a name="using-powershell-v1"></a><span data-ttu-id="4e9f9-137">Pomocí prostředí PowerShell V1</span><span class="sxs-lookup"><span data-stu-id="4e9f9-137">Using PowerShell V1</span></span>

<span data-ttu-id="4e9f9-138">Chcete-li začít pracovat, je potřeba [stáhněte a nainstalujte modul Azure AD PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).</span><span class="sxs-lookup"><span data-stu-id="4e9f9-138">To get started, you need to [download and install the Azure AD PowerShell module](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).</span></span> <span data-ttu-id="4e9f9-139">Až budete mít nainstalováno, můžete provést kroky, které provést při konfiguraci každé pole.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-139">Once you have it installed, you can follow the steps that follow to configure each field.</span></span>

#### <a name="set-authentication-data-with-powershell-v1"></a><span data-ttu-id="4e9f9-140">Nastavení ověření dat pomocí prostředí PowerShell V1</span><span class="sxs-lookup"><span data-stu-id="4e9f9-140">Set Authentication Data with PowerShell V1</span></span>

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-authentication-data-with-powershellpowershell-v1"></a><span data-ttu-id="4e9f9-141">Čtení dat ověřování pomocí PowerShellPowerShell V1</span><span class="sxs-lookup"><span data-stu-id="4e9f9-141">Read Authentication Data with PowerShellPowerShell V1</span></span>

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="authentication-phone-and-authentication-email-can-only-be-read-using-powershell-v1-using-the-commands-that-follow"></a><span data-ttu-id="4e9f9-142">Telefon pro ověření a e-mailu ověřování lze číst pouze pomocí prostředí Powershell V1 pomocí příkazů, které následují</span><span class="sxs-lookup"><span data-stu-id="4e9f9-142">Authentication Phone and Authentication Email can only be read using Powershell V1 using the commands that follow</span></span>

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="using-powershell-v2"></a><span data-ttu-id="4e9f9-143">Pomocí prostředí PowerShell V2</span><span class="sxs-lookup"><span data-stu-id="4e9f9-143">Using PowerShell V2</span></span>

<span data-ttu-id="4e9f9-144">Chcete-li začít pracovat, je potřeba [stáhněte a nainstalujte modul prostředí PowerShell Azure AD V2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md).</span><span class="sxs-lookup"><span data-stu-id="4e9f9-144">To get started, you need to [download and install the Azure AD V2 PowerShell module](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md).</span></span> <span data-ttu-id="4e9f9-145">Až budete mít nainstalováno, můžete provést kroky, které provést při konfiguraci každé pole.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-145">Once you have it installed, you can follow the steps that follow to configure each field.</span></span>

<span data-ttu-id="4e9f9-146">K instalaci rychle z nejnovější verze prostředí PowerShell, které podporují instalaci modulu, spuštěním těchto příkazů (první řádek jednoduše kontroluje, zda je již nainstalována):</span><span class="sxs-lookup"><span data-stu-id="4e9f9-146">To install quickly from recent versions of PowerShell that support Install-Module, run these commands (the first line simply checks to see if it's installed already):</span></span>

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-authentication-data-with-powershell-v2"></a><span data-ttu-id="4e9f9-147">Nastavení ověření dat pomocí prostředí PowerShell V2</span><span class="sxs-lookup"><span data-stu-id="4e9f9-147">Set Authentication Data with PowerShell V2</span></span>

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

### <a name="read-authentication-data-with-powershell-v2"></a><span data-ttu-id="4e9f9-148">Čtení dat ověřování pomocí prostředí PowerShell V2</span><span class="sxs-lookup"><span data-stu-id="4e9f9-148">Read Authentication Data with PowerShell V2</span></span>

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a><span data-ttu-id="4e9f9-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e9f9-149">Next steps</span></span>

<span data-ttu-id="4e9f9-150">Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e9f9-150">The following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="4e9f9-151">[**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-151">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="4e9f9-152">[**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-152">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="4e9f9-153">[**Uvedení**](active-directory-passwords-best-practices.md) – Naplánujte a nasaďte pro své uživatele samoobslužné resetování hesla pomocí zde uvedených pokynů.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-153">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR to your users using the guidance found here</span></span>
* <span data-ttu-id="4e9f9-154">[**Přizpůsobení**](active-directory-passwords-customize.md) – Přizpůsobte vzhled a chování samoobslužného resetování hesla pro vaši společnost.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-154">[**Customize**](active-directory-passwords-customize.md) - Customize the look and feel of the SSPR experience for your company.</span></span>
* <span data-ttu-id="4e9f9-155">[**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-155">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="4e9f9-156">[**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-156">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="4e9f9-157">[**Podrobné technické informace**](active-directory-passwords-how-it-works.md) – Nahlédněte za oponu a pochopte, jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-157">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind the curtain to understand how it works</span></span>
* <span data-ttu-id="4e9f9-158">[**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak?</span><span class="sxs-lookup"><span data-stu-id="4e9f9-158">[**Frequently Asked Questions**](active-directory-passwords-faq.md) - How?</span></span> <span data-ttu-id="4e9f9-159">Proč?</span><span class="sxs-lookup"><span data-stu-id="4e9f9-159">Why?</span></span> <span data-ttu-id="4e9f9-160">Co?</span><span class="sxs-lookup"><span data-stu-id="4e9f9-160">What?</span></span> <span data-ttu-id="4e9f9-161">Kde?</span><span class="sxs-lookup"><span data-stu-id="4e9f9-161">Where?</span></span> <span data-ttu-id="4e9f9-162">Kdo?</span><span class="sxs-lookup"><span data-stu-id="4e9f9-162">Who?</span></span> <span data-ttu-id="4e9f9-163">Kdy?</span><span class="sxs-lookup"><span data-stu-id="4e9f9-163">When?</span></span> <span data-ttu-id="4e9f9-164">– Odpovědi na otázky, na které jste se vždy chtěli zeptat.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-164">- Answers to questions you always wanted to ask</span></span>
* <span data-ttu-id="4e9f9-165">[**Řešení potíží**](active-directory-passwords-troubleshoot.md) – Zjistěte, jak řešit běžné problémy, ke kterým dochází u samoobslužného resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="4e9f9-165">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how to resolve common issues that we see with SSPR</span></span>
