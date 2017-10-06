---
title: "aaaScenarios a příklady pro řízení předplatné | Microsoft Docs"
description: "Poskytuje příklady, jak tooimplement předplatného Azure zásad správného řízení pro běžné scénáře."
services: azure-resource-manager
documentationcenter: na
author: rdendtler
manager: timlt
editor: tysonn
ms.assetid: e8fbeeb8-d7a1-48af-804f-6fe1a6024bcb
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: rodend;karlku;tomfitz
ms.openlocfilehash: f750e834519c8e64f57f87e2067801feb38b5c29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Příklady implementace Azure enterprise vygenerované uživatelské rozhraní
Toto téma obsahuje příklady, jak organizace může implementovat hello doporučení pro [vygenerované uživatelské rozhraní Azure enterprise](resource-manager-subscription-governance.md). Používá fiktivní společnost s názvem Contoso tooillustrate osvědčené postupy pro běžné scénáře.

## <a name="background"></a>Pozadí
Contoso je, že po celém světě společnost, která poskytuje zákazníků v všechno z modelu zabalené tooa modelu "Softwaru jako službu" zásobovací řetězec řešení nasadit místně.  Vyvíjejí softwaru v celém světě hello s centrech významné vývoj v Indie, hello Spojené státy a Kanadu.

část ISV Hello hello společnosti je rozdělena na několik nezávislých organizační jednotky, které spravovat produkty ve významné podniku. Jednotlivé obchodní jednotky má svou vlastní vývojáře, produktu správci a architekti.

Hello Enterprise technologie služby (ETS) organizační jednotka poskytuje funkce centralizované IT a spravuje několik datových center, kterých obchodních jednotek hostovat svých aplikací. Společně s správy hello datových center, hello ETS organizace obsahuje a spravuje centralizované spolupráce (například e-mailu a weby) a sítě nebo telefonní služby. Pro menší organizační jednotky, kteří nemají provozní pracovníci také spravují zákazníkem úlohy.

v tomto tématu se používají následující osoby Hello:

* Dave je správce ETS Azure hello.
* Alice je společnosti Contoso ředitel vývoj v hello zásobovací řetězec organizační jednotce.

Contoso je potřeba toobuild řádek obchodní aplikace a aplikace na straně zákazníka. Rozhodla toorun hello aplikace v Azure. Dave čte hello [zásad správného řízení doporučený předplatné](resource-manager-subscription-governance.md) tématu, a je nyní připraven tooimplement hello doporučení.

## <a name="scenario-1-line-of-business-application"></a>Scénář 1:-obchodní aplikace
Contoso je sestavení zdrojový kód správu systému (BitBucket) toobe používají vývojáři napříč hello, world.  aplikace Hello používá infrastrukturu jako službu (IaaS) pro hostování a se skládá z webových serverů a databázový server. Vývojáři přístup k serverům ve svých prostředích vývoj, ale nebudete potřebovat přístup toohello servery v Azure. Contoso ETS, které tooallow hello aplikace vlastníka a aplikace hello toomanage týmu. aplikace Hello je dostupná pouze při na podnikové síti společnosti Contoso. Pro tuto aplikaci potřebuje Dave tooset si předplatné hello. Hello předplatné bude hostovat další software vývojáře v budoucnu hello.  

### <a name="naming-standards--resource-groups"></a>Standardy pro vytváření názvů & skupiny prostředků
Dave vytvoří odběr toosupport nástroje pro vývojáře, které jsou společné pro všechny hello organizační jednotky. Potřebuje toocreate smysluplný názvy hello předplatného a skupiny prostředků (pro aplikace hello a hello sítě). Vytvoří hello následující předplatného a prostředků skupiny:

| Položka | Name (Název) | Popis |
| --- | --- | --- |
| Předplatné |Produkční DeveloperTools contoso ETS |Podporuje běžné nástroje pro vývojáře |
| Skupina prostředků |rgBitBucket |Obsahuje hello aplikace webového serveru a databázového serveru |
| Skupina prostředků |rgCoreNetworks |Obsahuje hello virtuální sítě a připojení brány site-to-site |

### <a name="role-based-access-control"></a>Řízení přístupu na základě role
Po vytvoření svého předplatného, Dave chce tooensure, který hello příslušné týmy a vlastníci aplikace mají přístup k jejich prostředkům. Dave rozpozná, že každý tým má jiné požadavky. Zadá využívá hello skupin, které byly synchronizovány ze společnosti Contoso místní služby Active Directory (AD) tooAzure služby Active Directory a poskytuje hello správnou úroveň přístupu toohello týmů.

Dave přiřadí hello následující role pro předplatné hello:

| Role | Přiřazené příliš| Popis |
| --- | --- | --- |
| [Vlastník](../active-directory/role-based-access-built-in-roles.md#owner) |Spravované ID ze společnosti Contoso AD |Toto ID je řízen pomocí těsně přístupu čas (JIT) pomocí nástroje Správa identit společnosti Contoso a zajistí, že je plně auditován přístup vlastníka předplatného. |
| [Správce zabezpečení](../active-directory/role-based-access-built-in-roles.md#security-manager) |Zabezpečení a rizika oddělení správy |Tato role umožňuje toolook uživatelé v hello Azure Security Center a stav hello hello prostředků. |
| [Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md#network-contributor) |Tým síťových |Tato role umožňuje společnosti Contoso sítě team toomanage hello lokality tooSite VPN a hello virtuální sítě. |
| *Vlastní role* |Majitel aplikace |Dave vytvoří roli, která uděluje hello možnost toomodify prostředky v rámci skupiny prostředků hello. Další informace najdete v tématu [vlastní role v Azure RBAC](../active-directory/role-based-access-control-custom-roles.md) |

### <a name="policies"></a>Zásady
Dave má následující požadavky pro správu prostředků v předplatném hello hello:

* Nástroje pro vývoj hello nepodporují vývojáři napříč hello, world, mohl nechce tooblock uživatelům ve vytváření prostředků v libovolné oblasti. Však mohl musí tooknow, kde jsou vytvářeny prostředky.
* Se zajímají náklady. Proto chce tooprevent vlastníci aplikace z vytváření zbytečně nákladné virtuálních počítačů.  
* Protože tato aplikace obsluhuje vývojáři v mnoha obchodních jednotek, chce tootag každého prostředku se hello obchodní jednotky a aplikace vlastníka. Pomocí těchto značek můžete ETS účtovat hello příslušné týmy.

Vytvoří následující hello [Resource Manager zásady](resource-manager-policy.md):

| Pole | Efekt | Popis |
| --- | --- | --- |
| location |Audit |Vytvoření hello auditu hello prostředky v libovolné oblasti |
| type |Odepřít |Odepřít vytváření virtuálních počítačů G-Series |
| tags |Odepřít |Vyžadují aplikace vlastníka značky |
| tags |Odepřít |Vyžadovat náklady center značky |
| tags |Připojit |Append – název značky **organizační jednotce** a hodnota značky **ETS** tooall prostředky |

### <a name="resource-tags"></a>Značky prostředku
Dave jste srozuměni s tím mohl musí toohave konkrétní informace o hello faktury tooidentify hello nákladové středisko pro implementaci BitBucket hello. Kromě toho chce Dave tooknow všechny prostředky, které vlastní ETS hello.

Přidá následující hello [značky](resource-group-using-tags.md) toohello prostředků a jejich skupin.

| Název značky | Hodnota značky |
| --- | --- |
| ApplicationOwner |Název Hello hello osobě, která spravuje této aplikace. |
| CostCenter |Nákladové středisko Hello hello skupiny, která platí za hello využití platformy Azure. |
| Organizační jednotku |**ETS** (hello organizační jednotce přidružené předplatné hello) |

### <a name="core-network"></a>Základní sítě
Hello Contoso ETS informace o zabezpečení a rizika tým správy kontroluje společnosti navrhované naplánujte tooAzure aplikace hello toomove. Chtějí tooensure, který aplikace hello není vystavený toohello Internetu.  Dave má také vývojáře aplikací, které v budoucích hello bude přesunutý tooAzure. Tyto aplikace vyžadují veřejné rozhraní.  toomeet tyto požadavky poskytuje interní a externí virtuální sítě a přístup k toorestrict skupiny zabezpečení sítě.

Vytvoří hello následující prostředky:

| Typ prostředku | Name (Název) | Popis |
| --- | --- | --- |
| Virtual Network |vnInternal |Použít s hello BitBucket aplikace a je připojená přes ExpressRoute tooContoso podnikové síti.  Podsíť (sbBitBucket) poskytuje aplikace hello s konkrétní prostor IP adres. |
| Virtual Network |vnExternal |Chcete-li k dispozici pro budoucí aplikace, které vyžadují veřejné koncové body. |
| Skupina zabezpečení sítě |nsgBitBucket |Zajišťuje, že hello prostor pro útoky této úlohy je minimalizován tím, že se připojení pouze na portu 443 pro podsíť hello tam, kde aplikace hello platný (sbBitBucket). |

### <a name="resource-locks"></a>Uzamčení prostředků
Dave rozpozná, že připojení hello ze společnosti Contoso podnikové síti toohello interní virtuální síť musí být chráněny v jakémkoli wayward skriptu nebo náhodného odstranění.

Vytvoří následující hello [prostředek zámku](resource-group-lock-resources.md):

| Typ zámku | Prostředek | Popis |
| --- | --- | --- |
| **CanNotDelete** |vnInternal |Zabraňuje uživatelům odstraňování hello virtuální sítě a podsítě, ale nezabrání hello přidání nových podsítí. |

### <a name="azure-automation"></a>Azure Automation
Dave nijak nesouvisí tooautomate pro tuto aplikaci. I když mu vytvořili účet Azure Automation, mu zpočátku je používat.

### <a name="azure-security-center"></a>Azure Security Center
Správy služeb contoso IT musí tooquickly identifikovat a zpracování hrozeb. Také kvůli toounderstand jaké problémy můžou existovat.  

toofulfill tyto požadavky, Dave umožňuje hello [Azure Security Center](../security-center/security-center-intro.md)a poskytuje role správce zabezpečení toohello přístup.

## <a name="scenario-2-customer-facing-app"></a>Scénář 2: zákazníka směřujících aplikací
vedení Hello firmy v hello zásobovací řetězec organizační jednotce označila různé možnosti tooincrease zapojení uživatelů zákazníků společnosti Contoso pomocí karty věrného. Alice tým musí vytvořit tuto aplikaci a rozhodne, že Azure zvyšuje jejich možnost toomeet hello obchodních potřeb. Alice pracuje s Dave z ETS tooconfigure obě předplatná pro vývoj a provoz této aplikace.

### <a name="azure-subscriptions"></a>Předplatná Azure
Dave přihlásí toohello podnikový portál Azure a uvidí, že oddělení hello zásobovací řetězec již existuje.  Ale tento projekt je hello první vývojového projektu pro team řetězu hello napájení v Azure, Dave rozpozná hello potřebu nového účtu pro vývojový tým od Alice.  Vytvoří hello "R & D" účet pro svůj tým a přiřadí tooAlice přístup. Alice přihlásí prostřednictvím hello portál Azure a vytvoří dvě předplatná: jeden toohold hello vývoj servery a jeden toohold hello produkční servery.  Jana řídí standardy pro vytváření názvů hello dříve vytvořeno, při vytváření hello následující odběry:

| Použití předplatného | Name (Název) |
| --- | --- |
| Vývoj |Vývoj LoyaltyCard SupplyChain ResearchDevelopment |
| Produkční |Produkční LoyaltyCard SupplyChain operace |

### <a name="policies"></a>Zásady
Dave a Alice popisují hello aplikace a identifikaci této aplikace jenom slouží zákazníků v oblasti Severní Ameriky hello.  Alice a její tým naplánujte toouse Azure prostředí aplikace služby a aplikace hello toocreate Azure SQL. Během vývoje, které mohou potřebovat toocreate virtuálních počítačů.  Alice chce tooensure, který svůj vývojáři mají prostředky hello potřebují tooexplore a prozkoumat problémy bez vyžádání v ETS.

Pro hello **vývoj předplatné**, vytvoří hello následující zásady:

| Pole | Efekt | Popis |
| --- | --- | --- |
| location |Audit |Audit hello vytvoření hello prostředky v libovolné oblasti. |

Neomezovat hello typ sku, které uživatel může vytvořit při vývoji a nevyžaduje žádné značky pro všechny skupiny prostředků nebo prostředky.

Pro hello **produkční předplatné**, vytvoří hello následující zásady:

| Pole | Efekt | Popis |
| --- | --- | --- |
| location |Odepřít |Odepřete hello vytvoření všechny prostředky mimo hello USA datových centrech. |
| tags |Odepřít |Vyžadují aplikace vlastníka značky |
| tags |Odepřít |Vyžadovat oddělení značky. |
| tags |Připojit |Připojte skupinu prostředků tooeach značku, která označuje produkčního prostředí. |

Neomezovat hello typ sku, které uživatel může vytvořit v produkčním prostředí.

### <a name="resource-tags"></a>Značky prostředku
Dave jste srozuměni s tím mu potřebuje toohave konkrétní informace tooidentify hello správné obchodních skupin pro fakturaci a vlastnictví. Definuje značky prostředku pro skupiny prostředků a prostředky.

| Název značky | Hodnota značky |
| --- | --- |
| ApplicationOwner |Název Hello hello osobě, která spravuje této aplikace. |
| Oddělení |Nákladové středisko Hello hello skupiny, která platí za hello využití platformy Azure. |
| EnvironmentType |**Produkční** (i když hello předplatné zahrnuje **produkční** v názvu text hello, včetně tato značka umožňuje snadnou identifikaci při prohlížení prostředky hello portálu nebo na faktuře hello.) |

### <a name="core-networks"></a>Základní sítě
Hello Contoso ETS informace o zabezpečení a rizika tým správy kontroluje společnosti navrhované naplánujte tooAzure aplikace hello toomove. Chtějí tooensure, který hello aplikaci věrného karta správně izolované a chráněné v síti hraniční sítě.  toofulfill tento požadavek Dave a Alice vytvořit externí virtuální sítě a sítě zabezpečení skupiny tooisolate hello aplikaci věrného karta z podnikové sítě hello Contoso.  

Pro hello **vývoj předplatné**, vytvoří:

| Typ prostředku | Name (Název) | Popis |
| --- | --- | --- |
| Virtual Network |vnInternal |Slouží hello Contoso věrného karty vývojového prostředí a je připojená přes ExpressRoute tooContoso podnikové síti. |

Pro hello **produkční předplatné**, vytvoří:

| Typ prostředku | Name (Název) | Popis |
| --- | --- | --- |
| Virtual Network |vnExternal |Hostuje aplikaci věrného karta hello a není připojený přímo tooContoso je ExpressRoute. Kód se instaluje prostřednictvím jejich systému zdrojového kódu přímo toohello PaaS služby. |
| Skupina zabezpečení sítě |nsgBitBucket |Zajišťuje, že hello prostor pro útoky této úlohy je minimalizován povolením jenom v vázané na komunikaci na portu TCP 443.  Contoso je také příčin pro další ochranu pomocí brány Firewall webových aplikací. |

### <a name="resource-locks"></a>Uzamčení prostředků
Dave a Alice udělit a rozhodnout, uzamčení prostředků tooadd u některých hello klíče prostředky v hello prostředí tooprevent náhodného odstranění během nabízené vyvolání chybové kódu.

Uživatel vytvořit hello následující uzamčení:

| Typ zámku | Prostředek | Popis |
| --- | --- | --- |
| **CanNotDelete** |vnExternal |tooprevent osoby z odstranění hello virtuální sítě nebo podsítě. Hello zámku nezabrání hello přidání nových podsítí. |

### <a name="azure-automation"></a>Azure Automation
Alice a její tým vývoj mají rozsáhlé sady runbook toomanage hello prostředí pro tuto aplikaci. sady runbook Hello umožňují hello přidávání a odstraňování uzlů pro aplikaci hello a dalších úloh DevOps.

toouse tyto sady runbook umožňují [automatizace](../automation/automation-intro.md).

### <a name="azure-security-center"></a>Azure Security Center
Správy služeb contoso IT musí tooquickly identifikovat a zpracování hrozeb. Také kvůli toounderstand jaké problémy můžou existovat.  

toofulfill tyto požadavky Dave umožňuje Azure Security Center. Mu zajišťuje, že je monitorování prostředků hello hello Azure Security Center a poskytuje přístup týmy toohello DevOps a zabezpečení.

## <a name="next-steps"></a>Další kroky
* toolearn o vytváření šablon Resource Manageru, najdete v části [osvědčené postupy pro vytváření šablon Azure Resource Manager](resource-manager-template-best-practices.md).

