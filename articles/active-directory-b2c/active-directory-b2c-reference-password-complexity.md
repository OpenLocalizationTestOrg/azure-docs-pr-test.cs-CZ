---
title: "Složitost hesla – Azure AD B2C | Microsoft Docs"
description: "Postup konfigurace požadavky na složitost hesel poskytl příjemce v Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: 53ef86c4-1586-45dc-9952-dbbd62f68afc
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeeda
ms.openlocfilehash: 3906c9fa1def206a8f0a7e155949097242728c2f
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a><span data-ttu-id="1a813-103">Azure AD B2C: Konfigurujte požadavky na složitost hesel</span><span class="sxs-lookup"><span data-stu-id="1a813-103">Azure AD B2C: Configure complexity requirements for passwords</span></span>

> [!NOTE]
> <span data-ttu-id="1a813-104">Tato funkce je ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="1a813-104">**This feature is in preview.**</span></span>  <span data-ttu-id="1a813-105">Obraťte se na [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com) tak, aby měl váš klient testovací povolit tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="1a813-105">Contact [AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com) to have your test tenant enabled with this feature.</span></span>  <span data-ttu-id="1a813-106">Neprovádějte testování to na produkční klienty.</span><span class="sxs-lookup"><span data-stu-id="1a813-106">Do not test this on production tenants.</span></span>

<span data-ttu-id="1a813-107">Azure Active Directory B2C (Azure AD B2C) podporuje změna požadavky na složitost hesla poskytl koncového uživatele při vytváření účtu.</span><span class="sxs-lookup"><span data-stu-id="1a813-107">Azure Active Directory B2C (Azure AD B2C) supports changing the complexity requirements for passwords supplied by an end user when creating an account.</span></span>  <span data-ttu-id="1a813-108">Ve výchozím nastavení používá Azure AD B2C `Strong` hesla.</span><span class="sxs-lookup"><span data-stu-id="1a813-108">By default, Azure AD B2C uses `Strong` passwords.</span></span>  <span data-ttu-id="1a813-109">Azure AD B2C podporuje také konfigurační možnosti pro řízení složitosti hesla, které zákazníci mohou používat.</span><span class="sxs-lookup"><span data-stu-id="1a813-109">Azure AD B2C also supports configuration options to control the complexity of passwords that customers can use.</span></span>

## <a name="when-password-rules-are-enforced"></a><span data-ttu-id="1a813-110">Když se vynucují pravidla pro hesla</span><span class="sxs-lookup"><span data-stu-id="1a813-110">When password rules are enforced</span></span>

<span data-ttu-id="1a813-111">Během registrace nebo resetování hesla, koncový uživatel musí zadat heslo, které splňuje pravidla složitosti.</span><span class="sxs-lookup"><span data-stu-id="1a813-111">During sign-up or password reset, an end user must supply a password that meets the complexity rules.</span></span>  <span data-ttu-id="1a813-112">Podle zásad se vynucují pravidla složitosti hesla.</span><span class="sxs-lookup"><span data-stu-id="1a813-112">Password complexity rules are enforced per policy.</span></span>  <span data-ttu-id="1a813-113">Je možné, že jeden zásad vyžadovat kód pin čtyřciferné během registrace chvíli vyžaduje jinou zásadou řetězec osm znaků během registrace.</span><span class="sxs-lookup"><span data-stu-id="1a813-113">It is possible to have one policy require a four-digit pin during sign-up while another policy requires a eight character string during sign-up.</span></span>  <span data-ttu-id="1a813-114">Může například pomocí zásad se složitost jiné heslo pro dospělé než pro děti.</span><span class="sxs-lookup"><span data-stu-id="1a813-114">For example, you may use a policy with different password complexity for adults than for children.</span></span>

<span data-ttu-id="1a813-115">Složitost hesla se nikdy vynucují během přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1a813-115">Password complexity is never enforced during sign-in.</span></span>  <span data-ttu-id="1a813-116">Uživatelé nikdy vyzváni během přihlášení změnit své heslo, protože nesplňuje požadavek na aktuální složitost.</span><span class="sxs-lookup"><span data-stu-id="1a813-116">Users are never prompted during sign-in to change their password because it doesn't meet the current complexity requirement.</span></span>

<span data-ttu-id="1a813-117">Toto jsou typy zásad konfigurovaným složitost hesla:</span><span class="sxs-lookup"><span data-stu-id="1a813-117">Here are the types of policies where password complexity can be configured:</span></span>

* <span data-ttu-id="1a813-118">Zásady registrace nebo přihlášení</span><span class="sxs-lookup"><span data-stu-id="1a813-118">Sign-up or Sign-in Policy</span></span>
* <span data-ttu-id="1a813-119">Zásady resetování hesel</span><span class="sxs-lookup"><span data-stu-id="1a813-119">Password Reset Policy</span></span>
* <span data-ttu-id="1a813-120">Vlastní zásady ([nakonfigurujte složitost hesel ve vlastních zásadách pro](active-directory-b2c-reference-password-complexity-custom.md))</span><span class="sxs-lookup"><span data-stu-id="1a813-120">Custom Policy ([Configure password complexity in custom policy](active-directory-b2c-reference-password-complexity-custom.md))</span></span>

## <a name="how-to-configure-password-complexity"></a><span data-ttu-id="1a813-121">Postup konfigurace složitost hesla</span><span class="sxs-lookup"><span data-stu-id="1a813-121">How to configure password complexity</span></span>

1. <span data-ttu-id="1a813-122">Postupujte podle těchto kroků [přejděte do nastavení Azure AD B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="1a813-122">Follow these steps to [navigate to Azure AD B2C settings](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
1. <span data-ttu-id="1a813-123">Otevřete **registrace nebo přihlášení zásady**.</span><span class="sxs-lookup"><span data-stu-id="1a813-123">Open **Sign-up or sign-in polices**.</span></span>
1. <span data-ttu-id="1a813-124">Vyberte zásadu a klikněte na **upravit**.</span><span class="sxs-lookup"><span data-stu-id="1a813-124">Select a policy, and click **Edit**.</span></span>
1. <span data-ttu-id="1a813-125">Otevřete **složitost hesla**.</span><span class="sxs-lookup"><span data-stu-id="1a813-125">Open **Password complexity**.</span></span>
1. <span data-ttu-id="1a813-126">Změnit složitost hesla pro tuto zásadu **jednoduché**, **silným**, nebo **vlastní**.</span><span class="sxs-lookup"><span data-stu-id="1a813-126">Change the password complexity for this policy to **Simple**, **Strong**, or **Custom**.</span></span>

### <a name="comparison-chart"></a><span data-ttu-id="1a813-127">Porovnávací graf</span><span class="sxs-lookup"><span data-stu-id="1a813-127">Comparison Chart</span></span>

| <span data-ttu-id="1a813-128">Složitost</span><span class="sxs-lookup"><span data-stu-id="1a813-128">Complexity</span></span> | <span data-ttu-id="1a813-129">Popis</span><span class="sxs-lookup"><span data-stu-id="1a813-129">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1a813-130">Jednoduchý</span><span class="sxs-lookup"><span data-stu-id="1a813-130">Simple</span></span> | <span data-ttu-id="1a813-131">Heslo, které je alespoň 8 až 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="1a813-131">A password that is at least 8 to 64 characters.</span></span> |
| <span data-ttu-id="1a813-132">Silné</span><span class="sxs-lookup"><span data-stu-id="1a813-132">Strong</span></span> | <span data-ttu-id="1a813-133">Heslo, které je alespoň 8 až 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="1a813-133">A password that is at least 8 to 64 characters.</span></span> <span data-ttu-id="1a813-134">To vyžaduje 3 ze 4 malá písmena, velká písmena, číslice a symboly.</span><span class="sxs-lookup"><span data-stu-id="1a813-134">It requires 3 out of 4 of lowercase, uppercase, numbers, or symbols.</span></span> |
| <span data-ttu-id="1a813-135">Vlastní</span><span class="sxs-lookup"><span data-stu-id="1a813-135">Custom</span></span> | <span data-ttu-id="1a813-136">Tato možnost poskytuje maximální kontrolu nad pravidla složitosti hesla.</span><span class="sxs-lookup"><span data-stu-id="1a813-136">This option provides the most control over password complexity rules.</span></span>  <span data-ttu-id="1a813-137">Umožňuje konfiguraci vlastní délka.</span><span class="sxs-lookup"><span data-stu-id="1a813-137">It allows configuring a custom length.</span></span>  <span data-ttu-id="1a813-138">Umožňuje také přijetí hesla jenom číslo (PIN).</span><span class="sxs-lookup"><span data-stu-id="1a813-138">It also allows accepting number-only passwords (pins).</span></span> |

## <a name="options-available-under-custom"></a><span data-ttu-id="1a813-139">Možnosti, které jsou k dispozici v části vlastní</span><span class="sxs-lookup"><span data-stu-id="1a813-139">Options available under custom</span></span>

### <a name="character-set"></a><span data-ttu-id="1a813-140">Znakové sady</span><span class="sxs-lookup"><span data-stu-id="1a813-140">Character Set</span></span>

<span data-ttu-id="1a813-141">Umožňuje přijímat pouze číslice (PIN) nebo celý znaková sada.</span><span class="sxs-lookup"><span data-stu-id="1a813-141">Allows you to accept digits only (pins) or the full character set.</span></span>

* <span data-ttu-id="1a813-142">**Pouze čísla** umožňuje číslic pouze (0-9) při zadávání hesla.</span><span class="sxs-lookup"><span data-stu-id="1a813-142">**Numbers only** allows digits only (0-9) while entering a password.</span></span>
* <span data-ttu-id="1a813-143">**Všechny** umožňuje jakékoli písmeno, číslo nebo symbol.</span><span class="sxs-lookup"><span data-stu-id="1a813-143">**All** allows any letter, number, or symbol.</span></span>

### <a name="length"></a><span data-ttu-id="1a813-144">Délka</span><span class="sxs-lookup"><span data-stu-id="1a813-144">Length</span></span>

<span data-ttu-id="1a813-145">Umožňuje řídit požadavky na délku hesla.</span><span class="sxs-lookup"><span data-stu-id="1a813-145">Allows you to control the length requirements of the password.</span></span>

* <span data-ttu-id="1a813-146">**Minimální délka** musí být aspoň na 4.</span><span class="sxs-lookup"><span data-stu-id="1a813-146">**Minimum Length** must be at least 4.</span></span>
* <span data-ttu-id="1a813-147">**Maximální délka** musí být větší nebo rovna minimální délku a může obsahovat maximálně 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="1a813-147">**Maximum Length** must be greater or equal to minimum length and at most can be 64 characters.</span></span>

### <a name="character-classes"></a><span data-ttu-id="1a813-148">Třídy znaků</span><span class="sxs-lookup"><span data-stu-id="1a813-148">Character classes</span></span>

<span data-ttu-id="1a813-149">Umožňuje řídit typy různých znakových použít heslo.</span><span class="sxs-lookup"><span data-stu-id="1a813-149">Allows you to control the different character types used in the password.</span></span>

* <span data-ttu-id="1a813-150">**2 4: malé písmeno, velké písmeno, číslici (0-9), Symbol** zajišťuje heslo obsahuje alespoň dva typy znaků.</span><span class="sxs-lookup"><span data-stu-id="1a813-150">**2 of 4: Lowercase character, Uppercase character, Number (0-9), Symbol** ensures the password contains at least two character types.</span></span> <span data-ttu-id="1a813-151">Například číslo a malé písmeno.</span><span class="sxs-lookup"><span data-stu-id="1a813-151">For example, a number and a lowercase character.</span></span>
* <span data-ttu-id="1a813-152">**3 4: malé písmeno, velké písmeno, číslici (0-9), Symbol** zajišťuje heslo obsahuje alespoň dva typy znaků.</span><span class="sxs-lookup"><span data-stu-id="1a813-152">**3 of 4: Lowercase character, Uppercase character, Number (0-9), Symbol** ensures the password contains at least two character types.</span></span> <span data-ttu-id="1a813-153">Například: číslo, malé písmeno a velké písmeno.</span><span class="sxs-lookup"><span data-stu-id="1a813-153">For example, a number, a lowercase character and an uppercase character.</span></span>
* <span data-ttu-id="1a813-154">**4 4: malé písmeno, velké písmeno, číslici (0-9), Symbol** zajišťuje heslo obsahuje všechny pro znakové typy.</span><span class="sxs-lookup"><span data-stu-id="1a813-154">**4 of 4: Lowercase character, Uppercase character, Number (0-9), Symbol** ensures the password contains all for character types.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1a813-155">Vyžadování **4 4** může mít za následek před koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="1a813-155">Requiring **4 of 4** can result in end-user frustration.</span></span> <span data-ttu-id="1a813-156">Některé ze studií vyplývá, že tento požadavek není zlepšit heslo šifrování.</span><span class="sxs-lookup"><span data-stu-id="1a813-156">Some studies have shown that this requirement does not improve password entropy.</span></span> <span data-ttu-id="1a813-157">V tématu [pokynů NIST heslo](https://pages.nist.gov/800-63-3/sp800-63b.html#appA)</span><span class="sxs-lookup"><span data-stu-id="1a813-157">See [NIST Password Guidelines](https://pages.nist.gov/800-63-3/sp800-63b.html#appA)</span></span>
