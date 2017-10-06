---
title: "aaaBest postupy pro podmíněný přístup v Azure Active Directory | Microsoft Docs"
description: "Další informace o věcí, které byste měli vědět, a co je, že byste neměli dělat při konfiguraci zásad podmíněného přístupu."
services: active-directory
keywords: "podmíněný přístup tooapps, podmíněného přístupu s Azure AD, zabezpečení přístupu k prostředkům toocompany, zásady podmíněného přístupu"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4952f8746a2e583380b3bb99cfe2fbdae1c07b08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Osvědčené postupy pro podmíněný přístup v Azure Active Directory

Toto téma poskytuje informace o věcí, které byste měli vědět, a co je, že byste neměli dělat při konfiguraci zásad podmíněného přístupu. Než si přečtete v tomto tématu, měli seznámit se s koncepty hello a terminologie hello uvedených v [podmíněný přístup v Azure Active Directory](active-directory-conditional-access-azure-portal.md)

## <a name="what-you-should-know"></a>Důležité informace

### <a name="whats-required-toomake-a-policy-work"></a>Jaké jsou požadavky toomake pracovní zásady?

Když vytvoříte novou zásadu, neexistují žádné uživatele, skupiny, aplikace nebo vybrané řízení přístupu.

![Cloudové aplikace](./media/active-directory-conditional-access-best-practices/02.png)


toomake zásady vaší práci, je nutné nakonfigurovat hello následující:


|Co           | Postupy                                  | Proč|
|:--            | :--                                  | :-- |
|**Cloudové aplikace** |Je nutné tooselect jednu nebo více aplikací.  | Hello cílem zásad podmíněného přístupu je tooenable toofine tune jak Autorizovaní uživatelé můžou používat vaše aplikace.|
| **Uživatelé a skupiny** | Je třeba tooselect alespoň jeden uživatel nebo skupina, která je autorizovaný tooaccess hello cloudových aplikací jste vybrali. | Zásady podmíněného přístupu, který nemá žádné uživatele a skupiny přiřazené, je neaktivní. |
| **Řízení přístupu** | Je třeba tooselect alespoň jeden řízení přístupu. | Procesor zásady musí tooknow, jaké toodo Pokud splnění podmínek.|


V přidání toothese základní požadavky, v řadě případů byste měli nakonfigurovat také podmínku. Zatímco zásady by taky měly fungovat bez nakonfigurovaného podmínky, jsou podmínky hello řízení faktor pro optimalizaci přístupu tooyour aplikace.


![Cloudové aplikace](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a>Jak se vyhodnocují přiřazení?

Všechna přiřazení jsou logicky **spojeny**. Pokud máte více než jeden přiřazení nakonfigurované, tootrigger zásady, musí být splněny všechna přiřazení.  

Pokud potřebujete tooconfigure umístění podmínku, která používá tooall připojení z mimo síti vaší organizace, můžete to provést pomocí:

- Včetně **všech umístění**
- S výjimkou **všechny důvěryhodné IP adresy**

### <a name="what-happens-if-you-have-policies-in-hello-azure-classic-portal-and-azure-portal-configured"></a>Co se stane, pokud máte zásady v hello portál Azure classic a nakonfigurovat portál Azure?  
Obě zásady se vynucují službou Azure Active Directory a hello uživatel získá přístup jenom v případě, že jsou splněny všechny požadavky.

### <a name="what-happens-if-you-have-policies-in-hello-intune-silverlight-portal-and-hello-azure-portal"></a>Co se stane, pokud máte zásady v hello portál Intune Silverlight a hello portálu Azure?
Obě zásady se vynucují službou Azure Active Directory a hello uživatel získá přístup jenom v případě, že jsou splněny všechny požadavky.

### <a name="what-happens-if-i-have-multiple-policies-for-hello-same-user-configured"></a>Co se stane, když mám několik zásad pro hello nakonfigurované stejného uživatele?  
Pro každé přihlášení Azure Active Directory vyhodnotí všechny zásady a zajistí, že jsou splněny všechny požadavky předtím, než uživatel toohello udělí přístup.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Podmíněný přístup funguje s Exchange ActiveSync?

Ano, pomocí protokolu Exchange ActiveSync v zásadách podmíněného přístupu.


## <a name="what-you-should-avoid-doing"></a>Co byste neměli dělat

framework Hello podmíněného přístupu vám poskytne skvělé konfigurace flexibilitu. Flexibilitu však také znamená, pečlivě zkontrolujte to každé konfiguraci zásad před tooreleasing ho tooavoid nežádoucí výsledky. V tomto kontextu, měli byste věnovat zvláštní pozornost tooassignments, jako by to ovlivnilo kompletní sady **všichni uživatelé / skupiny / cloudových aplikací**.

Ve vašem prostředí neměli byste hello následující konfigurace:


**Pro všechny uživatele všech cloudových aplikací:**

- **Blokovat přístup** – tato konfigurace zablokuje celé organizace, která není výborný vhodné.

- **Vyžaduje kompatibilní zařízení** – pro uživatele, který nebudete mít zaregistrovali svá zařízení ještě tato zásada blokuje veškerý přístup, včetně portálu Intune toohello přístup. Pokud jste správce bez registrovaných zařízení, zablokuje vás tyto zásady z získali zpět do hello Azure portálu toochange hello zásady.

- **Vyžadovat připojení k doméně** – tento blok zásad přístupu má taky hello potenciální tooblock přístup pro všechny uživatele ve vaší organizaci Pokud ještě nemáte zařízení připojených k doméně.


**Pro všechny uživatele, všechny cloudové aplikace, všechny platformy zařízení:**

- **Blokovat přístup** – tato konfigurace zablokuje celé organizace, která není výborný vhodné.


## <a name="common-scenarios"></a>Obvyklé scénáře

### <a name="requiring-multi-factor-authentication-for-apps"></a>Vyžadování vícefaktorového ověřování pro aplikace

Mnoho prostředí mít aplikace, které vyžadují vyšší úroveň ochrany než hello, ostatní.
To je třeba hello případ aplikací, které mají přístup k datům toosensitive.
Pokud chcete tooadd další vrstvu ochrany toothese aplikací, můžete nakonfigurovat zásady podmíněného přístupu, který vyžaduje službu Multi-Factor authentication, když uživatelé přistupují těchto aplikací.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Vyžadování vícefaktorového ověřování pro přístup ze sítě, které nejsou důvěryhodné

Tento scénář je podobný předchozímu scénáři toohello, protože přidá požadavek pro službu Multi-Factor authentication.
Hlavní rozdíl hello je však hello podmínky pro tento požadavek.  
Během hello fokus hello předchozí situaci je pro aplikace s daty toosensitve přístup, je aktivní hello tohoto scénáře důvěryhodného umístění.  
Jinými slovy může mít požadavek pro službu Multi-Factor authentication, pokud uživatel ze sítě, kterým nedůvěřujete přístupu k aplikaci.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Jenom důvěryhodné zařízení mají přístup ke službám Office 365

Pokud ve svém prostředí používáte Intune, můžete okamžitě začít používat rozhraní zásad podmíněného přístupu hello v hello konzoly Azure.

Mnoho zákazníků Intune používáte tooensure podmíněného přístupu, jenom důvěryhodné zařízení mají přístup ke službám Office 365. To znamená, že mobilní zařízení jsou zaregistrovaná v Intune a splňovat požadavky zásad dodržování předpisů, a že jsou počítače s Windows tooan připojené k místní doméně. Klíče zlepšování je, že nemáte tooset hello stejné zásady pro jednotlivé služby hello Office 365.  Když vytvoříte novou zásadu, nakonfigurujte hello cloudové aplikace tooinclude každý hello O365 aplikací, které chcete tooprotect s pomocí podmíněného přístupu.

## <a name="next-steps"></a>Další kroky

Pokud chcete, aby tooknow jak tooconfigure zásady podmíněného přístupu, najdete v části [Začínáme s podmíněným přístupem v Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).
