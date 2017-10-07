---
title: "odběry Identity Management aaaPrivileged - Azure | Microsoft Docs"
description: "Vysvětluje hello předplatné a licenční požadavky pro správu a použití ve vašem klientovi Azure AD Privileged Identity Management"
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: mwahl
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: 2639d13c250a582fdcf0b277c9bab37fdfcabcb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-privileged-identity-management-subscription-requirements"></a>Požadavky předplatné služby Azure Active Directory Privileged Identity Management

Azure AD Privileged Identity Management je k dispozici jako součást hello Premium P2 edice Azure AD. Další informace o hello jiné funkce P2 a jak porovná tooPremium P1, najdete v části [edice služby Azure Active Directory](../active-directory-editions.md).

>[!NOTE]
Když Azure Active Directory (Azure AD) Privileged Identity Management bylo ve verzi preview, nebyly k dispozici žádné licence kontroly pro službu klienta tootry hello.  Teď, když Azure AD Privileged Identity Management bylo dosaženo obecné dostupnosti, musí být k dispozici pro toocontinue hello klienta pomocí Privileged Identity Management po prosinci 2016 zkušební nebo placené předplatné.
  

## <a name="confirm-your-trial-or-paid-subscription"></a>Potvrďte zkušebnímu nebo placenému odběru

Pokud nejste jistí, jestli má vaše organizace zkušební verzi nebo zakoupit předplatné, můžete zkontrolovat, zda je odběr ve vašem klientovi pomocí příkazů hello součástí Azure Active Directory modulu pro Windows PowerShell V1. 
1. Otevřete okno PowerShell.
2. Zadejte `Connect-MsolService` tooauthenticate jako uživatel ve vašem klientovi.
3. Zadejte `Get-MsolSubscription | ft SkuPartNumber,IsTrial,Status`.

Tento příkaz načte seznam předplatných hello ve vašem klientovi. Pokud se, že neexistují že žádné řádky nevrátí, že budete potřebovat tooobtain Azure AD Premium P2 zkušební verze, koupíte Azure AD Premium P2 předplatné nebo EMS E5 toouse předplatné Azure AD Privileged Identity Management.  tooget zkušební verzi a začít používat Azure AD Privileged Identity Management, přečtěte si [Začínáme s Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md).

Pokud tento příkaz vrátí řádek, ve které SkuPartNumber je "AAD_PREMIUM_P2" nebo "EMSPREMIUM" a IsTrial hodnotu "True", to znamená, že je k dispozici v klientovi hello zkušební verzi Azure AD Premium P2.  Pokud stav předplatného hello není povoleno a není nutné nákupu předplatného Azure AD Premium P2 nebo EMS E5, pak musíte koupit Azure AD Premium P2 předplatné nebo EMS E5 předplatné toocontinue pomocí Azure AD Privileged Identity Management.

Je k dispozici prostřednictvím Azure AD Premium P2 [Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), hello [otevřete multilicenčního programu](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx)a hello [programu poskytovatele cloudových řešení](https://partner.microsoft.com/en-US/cloud-solution-provider). Odběratelé Azure a Office 365 může taky koupit Azure AD Premium P2 online.  Další informace o cenách Azure AD Premium a jak tooorder online naleznete na adrese [Azure Active Directory ceny](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

## <a name="azure-ad-privileged-identity-management-is-not-available-in-tenant"></a>Není k dispozici v klientovi Azure AD Privileged Identity Management

Azure AD Privileged Identity Management už nebude k dispozici ve vašem klientovi pokud:
- Při byl ve verzi preview a není zakoupit předplatné Azure AD Premium P2 nebo EMS E5 předplatné, byla vaše organizace používá Azure AD Privileged Identity Management.
- Vaše organizace měla služby Azure AD Premium P2 nebo EMS E5 zkušební verzi, jehož platnost vypršela.
- Vaše organizace měla zakoupené předplatné, jehož platnost vypršela.

Když Azure AD Premium P2 předplatné nebo EMS E5 předplatné vyprší platnost, nebo organizaci, která se pomocí Azure AD Privileged Identity Management ve verzi preview není získat předplatné Azure AD Premium P2 nebo EMS E5:

- Trvalá role přiřazení tooAzure AD role zůstanou beze změn.
- Hello rozšíření Azure AD Privileged Identity Management ve hello portál Azure, jakož i rutiny rozhraní Graph API hello a prostředí PowerShell rozhraní Azure AD Privileged Identity Management, budou již k dispozici pro uživatele tooactivate privilegované role, spravovat privilegovaný přístup, nebo provádět přístup recenze privilegovaných rolí.
- Role vhodné přiřazení rolí Azure AD bude odebrat, jelikož uživatelé už nebude možné tooactivate privilegované role.
- Všechny probíhající přístup recenze rolí Azure AD se ukončí a nastavení konfigurace Azure AD Privileged Identity Management se odeberou.
- Azure AD Privileged Identity Management už pošle e-mailů na změny v přiřazení role.

## <a name="next-steps"></a>Další kroky

- [Začínáme s Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md)
- [Role v Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-roles.md)
