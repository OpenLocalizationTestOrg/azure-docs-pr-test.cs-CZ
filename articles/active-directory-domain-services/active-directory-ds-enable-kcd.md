---
title: "Azure Active Directory Domain Services: Povolení vynuceného delegování protokolu kerberos | Microsoft Docs"
description: "Povolení vynuceného delegování protokolu kerberos na spravované domény Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: d32547c8018dd1f99c992e95a01d2711d460a261
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-kerberos-constrained-delegation-kcd-on-a-managed-domain"></a>Nakonfigurujte omezené delegování protokolu kerberos (použitím KCD) na spravované doméně
Mnoho aplikací potřebovat tooaccess prostředky v kontextu hello hello uživatele. Služba Active Directory podporuje mechanismus delegování protokolu Kerberos, která umožňuje tento případ použití volat. Navíc můžete omezit delegování tak, aby se dalo přistupovat pouze konkrétní prostředky v kontextu hello hello uživatele. Spravované domény služby Azure AD Domain Services se liší od tradiční domén služby Active Directory, vzhledem k tomu, že jsou zamčené bezpečněji dolů.

Tento článek ukazuje, jak pomocí protokolu kerberos tooconfigure omezeného delegování na spravované doméně služby Azure AD Domain Services.

## <a name="kerberos-constrained-delegation-kcd"></a>Omezené delegování protokolu Kerberos (použitím KCD)
Delegování protokolu Kerberos umožňuje účtu tooimpersonate jiné prostředky (například uživatel) hlavní tooaccess zabezpečení. Vezměte v úvahu webové aplikace, který přistupuje k back endové webové rozhraní API v kontextu hello uživatele. V tomto příkladu hello webové aplikace (spuštěn v kontextu účtu služby nebo účet počítače hello) zosobňuje hello uživatele při přístupu k prostředku hello (back endové webové rozhraní API). Delegování protokolu Kerberos je nezabezpečené, protože se neomezuje hello přístup k prostředkům hello zosobnění účtu v kontextu hello hello uživatele.

Omezené delegování protokolu Kerberos (použitím KCD) omezuje hello služby nebo prostředky toowhich hello může server reagovat hello jménem uživatele. Tradiční použitím KCD vyžaduje tooconfigure oprávnění správce domény účet domény pro službu a omezuje hello účet tooa jednu doménu.

Tradiční použitím KCD má také několik problémů, které jsou s ním spojená. V dřívějších operačních systémech kde správce domény hello nakonfigurované hello služby neměl Správce služeb hello žádné užitečný způsob, jak tooknow, které front-endové služby delegované toohello zdrojů služby vlastnil. A všechny front-endová služba, která mohla delegovat služba prostředků tooa reprezentována potenciální místo pro útok. Pokud server, který hostoval front-endové služby došlo k ohrožení, a bylo nakonfigurované toodelegate tooresource služby, služby prostředků hello může také dojít k ohrožení.

> [!NOTE]
> Na spravované doméně služby Azure AD Domain Services nemáte oprávnění správce domény. Proto **tradiční použitím KCD nelze konfigurovat ve spravované doméně**. Použijte použitím KCD založené na prostředcích, jak je popsáno v tomto článku. Tento mechanismus je také bezpečnější.
>
>

## <a name="resource-based-kerberos-constrained-delegation"></a>Na základě prostředků omezené delegování protokolu kerberos
V systému Windows Server 2012 R2 a Windows Server 2012 hello možnost tooconfigure omezené delegování pro hello službu přesunula ze Správce služby toohello správce domény hello. Správce služeb back-end hello tímto způsobem můžete povolit nebo odepřít front-endové služby. Tento model se označuje jako **omezeného delegování kerberos založené na prostředcích**.

Na základě prostředků použitím KCD je nakonfigurován pomocí prostředí PowerShell. Používáte hello Set-ADComputer nebo rutiny Set-ADUser, v závislosti na tom, jestli je hello zosobnění účtu účtu počítače nebo uživatelský účet služby nebo účet.

### <a name="configure-resource-based-kcd-for-a-computer-account-on-a-managed-domain"></a>Konfigurace založené na prostředcích použitím KCD pro účet počítače na spravované doméně
Předpokládejme, máte webovou aplikaci na počítači hello "contoso100-webapp.contoso100.com'. Potřebuje tooaccess hello prostředků (webového rozhraní API systémem ' contoso100-api.contoso100.com') v kontextu hello domain Users. Zde je, jak by nastavit založené na prostředcích použitím KCD pro tento scénář.

```
$ImpersonatingAccount = Get-ADComputer -Identity contoso100-webapp.contoso100.com
Set-ADComputer contoso100-api.contoso100.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

### <a name="configure-resource-based-kcd-for-a-user-account-on-a-managed-domain"></a>Konfigurace založené na prostředcích použitím KCD pro uživatelský účet na spravované doméně
Předpokládat, budete mít webovou aplikaci spuštěna jako účet služby, appsvc, a (web API spuštěna jako účet služby - 'backendsvc') musí tooaccess hello prostředků v kontextu hello domain Users. Zde je, jak by nastavit založené na prostředcích použitím KCD pro tento scénář.

```
$ImpersonatingAccount = Get-ADUser -Identity appsvc
Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Omezené delegování přehled protokolu Kerberos](https://technet.microsoft.com/library/jj553400.aspx)
