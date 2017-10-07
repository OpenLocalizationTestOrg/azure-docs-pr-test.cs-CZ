---
title: "aaaAzure služby Active Directory hybridní identita aspekty návrhu - určete úlohy správy hybridní identity | Microsoft Docs"
description: "Pomocí podmíněného řízení přístupu Azure Active Directory kontroluje hello konkrétní podmínky, kterou vyberete při ověřování uživatele hello a před povolením přístupu toohello aplikace. Po splnění těchto podmínek hello uživatel ověří a povolená přístup toohello aplikace."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 65f80aea-0426-4072-83e1-faf5b76df034
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: d3c0e9b23f43127b3d8e0b3a4e8f03d4bc148c27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="plan-for-hybrid-identity-lifecycle"></a>Plánování životního cyklu hybridní Identity
Identity je jedním ze základů hello strategie enterprise mobility a aplikaci přístup. Jestli se přihlašujete na tooyour mobilní zařízení nebo aplikace SaaS, je vaší identity tooeverything přístup klíče toogaining hello. Na nejvyšší úrovni do řešení pro správu identity zahrnuje synchronizovat mezi vaší identity úložiště, které zahrnuje automatizaci a centralizuje hello proces zřizování prostředků a je sjednotit. řešení identit Hello by měl být centralizované identity v rámci místní a cloudové a určitou formu identity federation toomaintain centralizované ověřování a bezpečně sdílet a použít spolupracovat s externími uživateli a firmy. Prostředky v rozsahu od operačních systémů a aplikací toopeople v nebo spojit s organizace. Organizační struktura může být změněna tooaccommodate hello zřizování zásad a postupů.

Je také důležité toohave řešení identity s cílem tooempower uživatele a poskytovat jim s samoobslužných vyskytne tookeep jejich produktivitu. Řešení identity je robustnější, pokud umožňuje jednotné přihlašování pro uživatele mezi všechny prostředky hello potřebují přístup k správci na všech úrovních použít standardizované postupy pro správu přihlašovací údaje uživatele. Některé úrovně správy lze omezit nebo vyloučit, v závislosti na hello spektra hello zřizování řešení pro správu. Kromě toho můžete bezpečně distribuovat možnosti správy, ručně nebo automaticky, mezi různými organizacemi. Správce domény může například sloužit pouze hello osoby a prostředky v této doméně. Tento uživatel může provádět úkoly správy a zřizování, ale není autorizovaný toodo úlohy konfigurace, jako je například vytváření pracovních postupů.

## <a name="determine-hybrid-identity-management-tasks"></a>Určení úlohy správy hybridní Identity
Distribuci úloh správy ve vaší organizaci zlepšuje hello přesnost a efektivnosti správy a hello Vyrovnávání zatížení hello organizace. Následují hello pivotů, které definují systém správy robustní identity.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)

úlohy správy hybridní identity toodefine, musíte pochopit některé základní znaky hello organizace, kterou bude možné přijala hybridní identita. Je důležité toounderstand hello aktuální úložiště používá pro identitu zdroje. Musíte znát ty základní prvky, budete mít hello základní požadavky a na základě, že budete potřebovat tooask podrobnější otázky, které povede tooa lepší rozhodnutí o návrhu pro vaše řešení Identity.  

Při definování těchto požadavků, ujistěte se, který alespoň hello následující otázky jsou zodpovězeny

* Možnosti zřizování: 
  
  * Podporuje řešení hybridní identity hello robustní účet správu přístupu a zřizování systému?
  * Jak jsou uživatelé, skupiny a hesla, které budete spravovat toobe?
  * Správa životního cyklu identity hello reaguje? 
    * Jak dlouho pozastavení účtu aktualizace hesla trvá?
* Správa licencí: 
  
  * Nepodporuje hello hybridní identity řešení popisovače Správa licencí?
    * Pokud ano, jaké funkce jsou k dispozici?
* Správa licencí na základě skupiny popisovač řešení hello? 
  
      - Pokud ano, je možné tooassign tooit skupiny zabezpečení? 
       - Pokud ano, bude hello cloudového adresáře automaticky přiřadit licence tooall hello členy skupiny hello? 
        - Co se stane, pokud uživatel je následně přidány do nebo odebrat ze skupiny hello, bude licence automaticky přiřadit nebo odebrat podle potřeby? 
* Integrace s jiných poskytovatelů identit třetích stran:
* Toto hybridní řešení se dá integrovat s identity jiných poskytovatelů tooimplement jednotné přihlašování?
* Je možné toounify všechny hello zprostředkovatelů různých identity do systému získá na ucelenosti identity?
* Pokud ano, jak a které jsou jejich a funkcích, které jsou k dispozici?

## <a name="synchronization-management"></a>Synchronizace správy
Jedním z cílů hello identity manager, možné toobring toobe všechny hello poskytovatelů identit a udržovat je synchronizován. Zachovat data hello synchronizovala podle zprostředkovatele autoritativní hlavní identity. V hybridním scénáři identity s modelem synchronizované správy spravovat všechny identity uživatelů a zařízení v místním serveru a synchronizaci hello účty a volitelně hesla toohello cloudu. Hello uživatel zadá hello stejné heslo v místní stejně potvrdí v cloudu hello a při přihlášení se heslo hello je ověřit pomocí řešení identit hello. Tento model používá nástroj pro synchronizaci adresáře.

![](./media/hybrid-id-design-considerations/Directory_synchronization.png)synchronizace hello tooproper návrh vašeho řešení hybridní identity Ujistěte se, že hello následující otázky jsou zodpovězeny: •, jaké jsou k dispozici pro řešení hybridní identity hello řešení synchronizace hello?
•, Jaké jsou hello jednotného přihlašování možnosti dostupné?
•, Jaké jsou možnosti hello federaci mezi B2B a B2C?

## <a name="next-steps"></a>Další kroky
[Určení strategie přijetí správy hybridní identity](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

