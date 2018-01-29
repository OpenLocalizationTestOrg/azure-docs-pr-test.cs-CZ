---
title: "Osvědčené postupy pro podmíněný přístup v Azure Active Directory | Microsoft Docs"
description: "Další informace o věcí, které byste měli vědět, a co je, že byste neměli dělat při konfiguraci zásad podmíněného přístupu."
services: active-directory
keywords: "podmíněný přístup k aplikacím, podmíněného přístupu s Azure AD, zabezpečený přístup k prostředkům společnosti, zásady podmíněného přístupu"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/12/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 8c6707505a6331b53e06b1de60575dd3637ea477
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Osvědčené postupy pro podmíněný přístup v Azure Active Directory

Toto téma poskytuje informace o věcí, které byste měli vědět, a co je, že byste neměli dělat při konfiguraci zásad podmíněného přístupu. Než si přečtete v tomto tématu, měli seznámit se s koncepty a technologiím uvedených v [podmíněný přístup v Azure Active Directory](active-directory-conditional-access-azure-portal.md)

## <a name="what-you-should-know"></a>Důležité informace

### <a name="whats-required-to-make-a-policy-work"></a>Co potřebné k tomu, aby zásada fungovat?

Když vytvoříte novou zásadu, neexistují žádné uživatele, skupiny, aplikace nebo vybrané řízení přístupu.

![Cloudové aplikace](./media/active-directory-conditional-access-best-practices/02.png)


Chcete-li vaše zásady fungovat, je nutné nakonfigurovat následující:


|Co           | Postupy                                  | Proč|
|:--            | :--                                  | :-- |
|**Cloudové aplikace** |Musíte vybrat jednu nebo více aplikací.  | Cílem zásad podmíněného přístupu je umožnit a systém doladit jak Autorizovaní uživatelé můžou používat vaše aplikace.|
| **Uživatelé a skupiny** | Je nutné vybrat alespoň jeden uživatel nebo skupina, který má oprávnění pro přístup k cloudové aplikace, které jste vybrali. | Zásady podmíněného přístupu, který nemá žádné uživatele a skupiny přiřazené, je neaktivní. |
| **Řízení přístupu** | Musíte vybrat aspoň jedno přístupu řízení. | Procesor zásad je potřeba vědět, co dělat v případě splnění podmínek.|


Kromě těchto základních požadavků v řadě případů byste měli také nakonfigurovat podmínku. Zatímco zásady by taky měly fungovat bez nakonfigurovaného podmínky, jsou podmínky řízení faktor pro optimalizaci přístupu k aplikacím.


![Cloudové aplikace](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a>Jak se vyhodnocují přiřazení?

Všechna přiřazení jsou logicky **spojeny**. Pokud máte více než jeden přiřazení nakonfigurovaný pro aktivaci a zásady musí být splněny všechna přiřazení.  

Pokud potřebujete nakonfigurovat umístění podmínku, která platí pro všechna připojení z mimo síti vaší organizace, můžete to provést pomocí:

- Včetně **všech umístění**
- S výjimkou **všechny důvěryhodné IP adresy**

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Co se stane, pokud máte v portálu Azure classic a portálu Azure, které jsou nakonfigurované zásady?  
Obě zásady se vynucují službou Azure Active Directory a uživatel získá přístup jenom v případě, že jsou splněny všechny požadavky.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Co se stane, pokud máte zásady v portálu Intune Silverlight a portálu Azure?
Obě zásady se vynucují službou Azure Active Directory a uživatel získá přístup jenom v případě, že jsou splněny všechny požadavky.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Co se stane, když mám několik zásad pro stejného uživatele nakonfigurované?  
Pro každé přihlášení Azure Active Directory vyhodnotí všechny zásady a zajistí, že jsou splněny všechny požadavky před udělil přístup pro uživatele.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Podmíněný přístup funguje s Exchange ActiveSync?

Ano, pomocí protokolu Exchange ActiveSync v zásadách podmíněného přístupu.


## <a name="what-you-should-avoid-doing"></a>Co byste neměli dělat

Rozhraní framework podmíněného přístupu vám poskytne skvělé konfigurace flexibilitu. Ale flexibilitu také znamená, že byste měli pečlivě zkontrolovat každou zásadu konfigurace před uvolněním, aby se zabránilo nežádoucí výsledky. V tomto kontextu, měli byste věnovat zvláštní pozornost, jako by to ovlivnilo kompletní sady přiřazení **všichni uživatelé / skupiny / cloudových aplikací**.

Ve vašem prostředí neměli byste následující konfigurace:


**Pro všechny uživatele všech cloudových aplikací:**

- **Blokovat přístup** – tato konfigurace zablokuje celé organizace, která není výborný vhodné.

- **Vyžaduje kompatibilní zařízení** – pro uživatele, který nebudete mít zaregistrovali svá zařízení ještě tato zásada blokuje veškerý přístup, včetně přístupu k portálu služby Intune. Pokud jste správce bez registrovaných zařízení, zablokuje vás tyto zásady z získali zpět do portálu Azure, změna zásad.

- **Vyžadovat připojení k doméně** – tento blok zásad přístupu se taky může blokovat přístup pro všechny uživatele ve vaší organizaci, pokud ještě nemáte zařízení připojených k doméně.


**Pro všechny uživatele, všechny cloudové aplikace, všechny platformy zařízení:**

- **Blokovat přístup** – tato konfigurace zablokuje celé organizace, která není výborný vhodné.



## <a name="policy-migration"></a>Migrace zásad

Měli byste zvážit, zásady, které jste dosud nevytvořili na portálu Azure, protože migrace:

- Scénáře, které nelze zpracovat před můžete vyřešit.

- Můžete snížit počet zásad, které budete muset spravovat konsolidovat.   

- Můžete spravovat všechny zásady podmíněného přístupu v jednom centrálním místě.

- Portál Azure classic vyřadí.   


Další informace najdete v tématu [migraci na portálu Azure classic zásad](active-directory-conditional-access-migration.md).


## <a name="next-steps"></a>Další kroky

Pokud chcete vědět, jak konfigurovat zásadu podmíněného přístupu, najdete v článku [Začínáme s podmíněným přístupem v Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).
