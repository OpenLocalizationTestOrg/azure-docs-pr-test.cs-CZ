---
title: aaaTroubleshoot Azure RBAC | Microsoft Docs
description: "Získáte pomoc s problémy nebo dotazy týkající se řízení přístupu na základě Role prostředky."
services: azure-portal
documentationcenter: na
author: andredm7
manager: femila
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 15feced32d8459d90c4c246d335932f90e1fc91f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="role-based-access-control-troubleshooting"></a>Na základě rolí řešení potíží s řízení přístupu

Tento dokument článku naleznete odpovědi na časté otázky týkající se hello konkrétní přístupová práva, kterým je uděleno oprávnění s rolemi, tak, aby víte, jaké tooexpect při použití hello rolí v hello portál Azure a můžete řešit problémy přístup. Tyto tři role popisuje všechny typy prostředků:

* Vlastník  
* Přispěvatel  
* Čtenář  

Vlastníci a přispěvatelé mají plný přístup toohello správu prostředí, ale Přispěvatel nelze udělit přístup tooother uživatele nebo skupiny. Věcí získat něco zajímavějšího s rolí čtečky hello, tak, aby se, kde budete Věnujte nějaký čas. V tématu hello [řízení přístupu na základě Role get-started článku](role-based-access-control-configure.md) podrobnosti o tom, jak toogrant přístup.

## <a name="app-service-workloads"></a>Úlohy služby aplikace
### <a name="write-access-capabilities"></a>Možnosti přístup pro zápis
Pokud byste udělit uživatelské oprávnění jen pro čtení tooa jedné webové aplikace, některé funkce jsou vypnuté, nemusí očekáváte. Hello následující možnosti správy vyžadují **zápisu** přístup k webové aplikaci tooa (Přispěvatel nebo vlastníka) a nejsou k dispozici v žádném scénáři jen pro čtení.

* Příkazy (například spuštění, zastavení, atd.)
* Změna nastavení, jako je obecná konfigurace, nastavení škálování, nastavení zálohování a nastavení monitorování
* Přístup k pověření k publikování a jiné tajné klíče, jako je nastavení aplikace a připojovacích řetězců
* Protokoly streamování
* Konfigurace diagnostických protokolů
* Konzole (příkazového řádku)
* Aktivní a poslední nasazení (pro místní git průběžné nasazování)
* Odhadované tráví
* Testy webu
* Virtuální sítě (jenom viditelné tooa čtečky Pokud uživatel s přístup pro zápis byl dříve nakonfigurován virtuální sítě).

Pokud nemůžete použít žádnou z těchto dlaždicích, je třeba tooask správce pro přispěvatele přístup toohello webovou aplikaci.

### <a name="dealing-with-related-resources"></a>Práci s související informační zdroje
Webové aplikace jsou ztěžuje hello přítomnost několik různých prostředků, které vztahu. Tady je skupina prostředků typické se několik webů:

![Skupina prostředků webové aplikace](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Výsledkem je pokud někdo udělíte přístup toojust hello webové aplikace, většinu funkcí hello v okně hello webu v hello portál Azure je zakázané.

Tyto položky vyžadují **zápisu** přístup toohello **plán služby App Service** odpovídající tooyour webu:  

* Zobrazení hello webové aplikace je cenová úroveň (volné nebo Standard)  
* Škálování konfigurace (počet instancí, velikost virtuálního počítače, nastavení automatického škálování)  
* Kvóty (úložiště, šířku pásma, procesoru)  

Tyto položky vyžadují **zápisu** přístup toohello celou **skupiny prostředků** svůj web, který obsahuje:  

* Certifikáty SSL a vazeb (certifikáty protokolu SSL může být sdílena mezi lokalitami v hello stejnou skupinu prostředků a geografického umístění)  
* Pravidla výstrah  
* nastavení automatického škálování  
* Přehled součásti aplikace  
* Testy webu  

## <a name="virtual-machine-workloads"></a>Úlohy virtuálních počítačů
Mnohem jako s webovými aplikacemi, některé funkce v okně hello virtuálního počítače vyžadují přístup pro zápis toohello virtuálního počítače nebo tooother prostředky ve skupině prostředků hello.

Virtuální počítače jsou názvy související tooDomain, virtuálních sítí, účty úložiště a pravidla výstrah.

Tyto položky vyžadují **zápisu** přístup toohello **virtuální počítač**:

* Koncové body  
* IP adresy  
* Disky  
* Rozšíření  

Vyžadují **zápisu** přístup tooboth hello **virtuálního počítače**a hello **skupiny prostředků** (spolu s názvem domény hello) je to v:  

* Skupina dostupnosti  
* Skupině s vyrovnáváním zatížení  
* Pravidla výstrah  

Pokud nemůžete použít žádnou z těchto dlaždicích, požádejte správce pro skupinu prostředků toohello Přispěvatel přístup.

## <a name="see-more"></a>Přečíst si k tomu víc
* [Řízení přístupu na základě role](role-based-access-control-configure.md): Začínáme s RBAC v hello portálu Azure.
* [Předdefinované role](role-based-access-built-in-roles.md): získání podrobností o hello rolí, které jsou v RBAC standardní.
* [Vlastní role v Azure RBAC](role-based-access-control-custom-roles.md): Zjistěte, jak se toocreate vlastní role toofit přístup musí.
* [Vytvoření sestavy historie změn přístupu](role-based-access-control-access-change-history-report.md): udržování přehledu o změně přiřazení rolí v RBAC.

