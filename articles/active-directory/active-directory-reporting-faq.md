---
title: "aaaAzure Active Directory Reporting – nejčastější dotazy | Microsoft Docs"
description: "Nejčastější dotazy týkající se vytváření sestav Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: be65a05574ea3b5b190cd02a96b211c571ba70bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-faq"></a>Nejčastější dotazy týkající se vytváření sestav Azure Active Directory.

Tento článek obsahuje odpovědi toofrequently kladené dotazy (FAQ) o vytváření sestav Azure Active Directory.  
Další podrobnosti najdete v tématu [generování sestav Azure Active Directory](active-directory-reporting-azure-portal.md). 

**Otázka: co je uchovávání dat hello protokoly aktivity (auditu a přihlášení) v hello portál Azure?** 

**Odpověď:** poskytujeme 7 dnů dat pro naše zákazníky volné a přepnutím tooan Azure AD Premium 1 nebo Premium 2 licenci, přistupujete k datům pro až too30 dnů. Další informace o uchovávání dat v [zásady uchování sestav Azure Active Directory](active-directory-reporting-retention.md)

--- 

**Otázka: jak dlouho trvá až po dokončení Moje úloh data aktivit hello uvidí?**

**Odpověď:** protokoly auditu aktivity, jejichž latence od hodinu tooan 15 minut. Protokoly přihlašovací aktivity, jejichž latence od 15 minut pro většinu záznamů a v provozu too2 hodin pro několik záznamů.

---

**Otázka: je nutné toobe protokoly aktivitu globálního správce toosee hello hello portálu Azure nebo tooget data prostřednictvím hello rozhraní API?**

**Odpověď:** Ne. Může být buď **zabezpečení čtečky**, **správce zabezpečení** nebo **globálního správce** toosee data na portálu Azure nebo k ní přistupují prostřednictvím hello rozhraní API pro vytváření sestav.

---

**Otázka: je možné získat informace o protokolu činnosti Office 365 prostřednictvím hello portál Azure?**

**Odpověď:** i když aktivita Office 365 a Azure AD aktivity protokoly sdílení velké množství prostředků directory hello, pokud chcete, aby ucelený pohled na dobrý den protokoly aktivity Office 365, měli byste toohello Centrum pro správu Office 365 tooget Office 365 aktivity protokolu informace.

---


**Otázka: rozhraní API se který používám tooget informace o protokoly aktivity Office 365?**

**Odpověď:** použití hello rozhraní API pro správu Office 365 tooaccess hello [Office 365 aktivity protokoly prostřednictvím rozhraní API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).

---

**Otázka: počet záznamů, můžete stáhnout z portálu Azure?**

**Odpověď:** si můžete stáhnout si too120K záznamy ze hello portálu Azure. Hello záznamy jsou seřazené podle *nejnovější* a ve výchozím nastavení, získáte záznamy hello nejnovější 120 kB. 

---

**Otázka: počet záznamů, můžete dotazovat pomocí aktivity hello rozhraní API?**

**Odpověď:** až too1 mil. záznamy se můžete dotazovat (Pokud nechcete použít operátor top hello, která seřadí hello záznam většina poslední). Pokud se rozhodnete použít operátor "top" hello, se můžete dotazovat až too500K záznamy. Ukázkové dotazy na tom, jak toouse hello API Zde můžete najít [zde](active-directory-reporting-api-getting-started.md).

---

**Otázka: jak lze získat licenci premium?**

**Odpověď:** najdete v části [Začínáme s Azure Active Directory Premium](active-directory-get-started-premium.md) pro otázku toothis odpovědí.

---

**Otázka: jak brzy by měl zobrazit data aktivity po získání licence premium?**

**Odpověď:** Pokud již máte data aktivity, jako volné licenci, pak se zobrazí hello stejná data. Pokud nemáte k dispozici žádná data, pak bude trvat jeden nebo dva dny.

---

**Otázka: Po získání licenci Azure AD premium zobrazit data poslední měsíc?**

**Odpověď:** Pokud Přepnuli jste nedávno tooa verze Premium (včetně zkušební verzi), můžete zobrazit data si too7 dnů původně. Když se data hromadí, zobrazí se až too30 dnů.

---

**Otázka: je riziko událostí v ochrany identit, ale odpovídající přihlášení v hello nejsou zobrazeny všechny přihlášení. Je to očekávané?**

**Odpověď:** Ano, Identity Protection vyhodnotí riziko pro všechny toky ověřování jestli-li být interaktivní nebo neinteraktivní. Ale všechny přihlášení pouze sestava zobrazí jenom hello interaktivní přihlašování.

---

**Otázka: jak můžete stáhnout hello "Uživatelé označení příznakem rizik" sestav na portálu Azure?**

**Odpověď:** hello možnost toodownload *uživatelé označení příznakem rizik* sestavy přidá brzy.

---

**Otázka: Jak poznám, proč přihlášení, nebo pro uživatele byla příznakem rizikové v hello portál Azure?**

**Odpověď:** zákazníkům edice Premium můžete další informace o hello základní rizikových událostech kliknutím na uživatele hello v "Uživatelé označení příznakem rizik" nebo kliknutím na hello "Rizikové přihlášení". Zákazníci volné a Základní edice získání bez hello základní informace o události riziko toosee hello rizikové uživatelů a přihlášení.

---

**Otázka: jak jsou vypočítávány IP adresy v sestavě rizikové přihlášení a přihlášení hello??**

**Odpověď:** IP adresy vydávají tak, že není spolehlivý připojení mezi IP adresu a kde hello s touto adresou se fyzicky nacházejí. To ztěžuje faktorech například mobilní poskytovatelů a vydání z Centrální fondy IP adres velmi často daleko od skutečně použití hello klientské zařízení sítě VPN. Zadané hello výše, převod IP adres tooa fyzické umístění je nejlepší úsilí na základě trasování, data registru, zpětné vyhledání a další informace. 

---
