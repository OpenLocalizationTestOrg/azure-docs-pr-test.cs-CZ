---
title: "Zvládat údržby pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Zvládat údržby pro virtuální počítače s Windows."
services: virtual-machines-windows
documentationcenter: 
author: zivr
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: zivr
ms.openlocfilehash: 75cd4d567deb98e5d2498dc607b43dae483f1c94
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="impactful-maintenance-for-virtual-machines"></a>Zvládat údržby pro virtuální počítače

Existuje několik případů, kdy jsou virtuální počítače restartovat z důvodu plánované údržby základní infrastruktury. Probíhá zvládat na dostupnost virtuální počítače hostované v Azure, tady jsou nyní k dispozici pro použití:

-   Minimálně 30 dnů před dopad odesláno oznámení.

-   Přehled a viditelnost pro údržbu na každý virtuální počítač.

-   Flexibilita a řízení v nastavení přesný čas údržby měly týkat virtuálních počítačů.

Údržby v Microsoft Azure je naplánován v iterací. Počáteční iterací mít menší obor snižte riziko při zavádění nové opravy a funkce. Novější iterací může span více oblastí (nikdy ze stejného páru oblasti). Virtuální počítač je součástí jedné údržby iterací. Pokud byl přerušen iterace, zbývající virtuální počítače jsou součástí jiného, budoucí, iterací.

Iterace plánované údržby má dvě fáze: Pre-emptive údržby a naplánovaná údržby.

**Pre-emptive údržby** vám poskytuje flexibilitu při zahájení údržby na virtuálních počítačů. Díky tomu můžete určit, kdy jsou dopad na virtuální počítače, pořadí aktualizace a čas mezi jednotlivé virtuální počítače, které neudržují. Se můžete dotazovat každý virtuální počítač, který chcete zobrazit, zda je plánovaná pro údržbu a zkontrolujte výsledek vaší poslední žádosti initiated údržby.

**Naplánované údržby** je Azure má plánu pro údržbu virtuálních počítačů. Během této doby čas, který následuje preemptivní údržby, můžete stále dotazu pro okna údržby, ale již nebude možné k orchestraci údržby.

## <a name="availability-considerations-during-planned-maintenance"></a>Důležité informace o dostupnosti během plánované údržby 

### <a name="paired-regions"></a>Spárované oblastí

Každé oblasti Azure je spárován s jinou oblast v rámci stejné geography, společně provedení pár místní. Při provádění údržby, bude Azure aktualizovat pouze instance virtuálních počítačů v jedné oblasti jeho dvojic. Například při aktualizaci virtuálních počítačů v oblasti Střed USA – sever nebude Azure zároveň aktualizovat žádné virtuální počítače v oblasti Střed USA – jih. To se naplánuje na jindy – díky tomu je umožněno převzetí služeb při selhání nebo vyrovnávání zatížení mezi oblastmi. V ostatních oblastech, jako je Severní Evropa, však může údržba probíhat ve stejnou dobu jako v oblasti Východní USA.
Další informace o [oblast Azure páry](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

### <a name="single-instance-vms-vs-availability-set-or-vm-scale-set"></a>Vs jedné Instance virtuálních počítačů. Sadu škálování virtuálního počítače nebo sady dostupnosti.

Pokud nasazujete zatížení používání virtuálních počítačů v Azure, můžete vytvořit virtuální počítače v rámci dostupnosti nastavit k zajištění vysoké dostupnosti do vaší aplikace. Tato konfigurace zajistí, že během výpadku nebo údržby událostí, je k dispozici alespoň jeden virtuální počítač.

V rámci skupiny dostupnosti jednotlivé virtuální počítače jsou rozloženy až 20 aktualizaci domény. Během plánované údržby je v každém okamžiku dopad pouze jednu aktualizaci domény. Pořadí aktualizace domén ovlivněný nemusí pokračovat postupně během plánované údržby. Pro virtuální počítače jediné instance (není součástí skupiny dostupnosti) neexistuje žádný způsob, jak předpovědi nebo určit, který a společně dopad na tom, kolik virtuálních počítačů.

Sady škálování virtuálního počítače se Azure výpočtový prostředek, který umožňuje nasadit a spravovat sadu identické virtuální počítače jako jeden prostředek.
Škálovací sada poskytuje podobné záruky na dostupnosti nastavit ve formě aktualizaci domény. 

Další informace o konfiguraci virtuálních počítačů pro zajištění vysoké dostupnosti najdete v tématu [ *Správa dostupnosti virtuálních počítačů Windows*](../linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="scheduled-events"></a>Naplánované události

Služba Azure Metadata umožňuje zjišťovat informace o virtuální počítač hostovaný v Azure. Naplánované události, některou z vystaveného kategorií, povrchy informace týkající se nadcházející události (například restartování) tak, aby vaše aplikace můžete připravit pro ně a omezit přerušení.

Další informace o naplánované události, najdete v části [Azure Metadata Service - naplánované události](../virtual-machines-scheduled-events.md).

## <a name="maintenance-discovery-and-notifications"></a>Zjišťování údržby a oznámení

Plán údržby je viditelná pro zákazníky na úrovni jednotlivých virtuálních počítačů. Můžete portál Azure, rozhraní API, Powershellu nebo rozhraní příkazového řádku k dotazu pro systém windows preemptivní a plánované údržby. Kromě toho očekávají pro příjem oznámení (e-mailu) v případě, že tam, kde jsou během procesu dopad (minimálně) z virtuálních počítačů.

Preemptivní údržby a plánované údržby fáze začínat oznámení. Očekáváte jeden oznámení za předplatné Azure. Oznámení se posílá správce a spolusprávce odběru ve výchozím nastavení. Můžete také nakonfigurovat cílovou skupinu pro oznámení údržby.

### <a name="view-the-maintenance-window-in-the-portal"></a>Zobrazit na portálu pro správu a údržbu 

Můžete použít portál Azure a vyhledejte naplánována údržba virtuálních počítačů.

1.  Přihlaste se k portálu Azure.

2.  Klikněte na tlačítko a otevřete **virtuální počítače** okno.

3.  Klikněte **sloupce** tlačítko otevřete seznam dostupných sloupců lze vybírat

4.  Vyberte a přidejte **údržby** sloupce. Virtuální počítače, které jsou naplánovány pro údržbu mít prezentované údržby windows. Jakmile je byla dokončena nebo přerušena z důvodu údržby a, časové období údržby se už nebude zobrazovat.

### <a name="query-maintenance-details-using-the-azure-api"></a>Podrobnosti o údržby dotazu pomocí rozhraní API služby Azure

Použití [získat informace o virtuálních počítačů API](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-get) a vyhledejte zobrazení instance k zjištění podrobností údržby na jednotlivých virtuálních počítačů. Odpověď obsahuje následující prvky:

  - isCustomerInitiatedMaintenanceAllowed: Určuje, zda nyní můžete zahájit preemptivní znovu ho zaveďte ve virtuálním počítači.

  - preMaintenanceWindowStartTime: čas zahájení preemptivní údržby.

  - preMaintenanceWindowEndTime: koncový čas pro správu a údržbu preemptivní. Po této době jste už nebude moct iniciovat údržby na tomto virtuálním počítači.
    
  - maintenanceWindowStartTime: čas zahájení okno plánované údržby při dopad na virtuální počítač.

  - maintenanceWindowEndTime: koncový čas okno plánované údržby.
  
  - lastOperationResultCode: výsledek vaší poslední operace údržby-znovu ho zaveďte.
 
  - lastOperationMessage: zpráva popisující výsledek vaší poslední operace údržby-znovu ho zaveďte.

## <a name="pre-emptive-redeploy"></a>Preemptivní znovu ho zaveďte

Preemptivní znovu ho zaveďte akce poskytuje flexibilní možnost určit čas, kdy se použije údržby pro virtuální počítače v Azure. Plánovaná údržba začíná preemptivní údržby kde se můžete rozhodnout na přesný čas pro jednotlivé virtuální počítače restartovat. Tady jsou případy použití, kde je užitečné tyto funkce:

-   Údržby musí být zajistí koordinaci se koncového zákazníka.

-   Okno plánované údržby, které nabízí Azure není dostatečná.
    Je možné, že okno se stane, jako na nejvytíženější času v týdnu nebo je příliš dlouhá.

-   Pro více instancemi nebo vícevrstvé aplikace budete potřebovat dostatečnou dobu mezi dva virtuální počítače nebo určité pořadí podle.

Při volání pro preemptivní znovu ho zaveďte na virtuálním počítači, přesune virtuální počítač již aktualizovaný uzel a potom zapne ji zpět, zachování všech možností konfigurace a přidružených prostředků. Když to uděláte, dočasným diskovým dojde ke ztrátě a dynamické IP adresy přidružené k rozhraní virtuální sítě se aktualizují.

Preemptivní znovu ho zaveďte je povoleno jednou na virtuální počítač. Pokud dojde k chybě během procesu, operace byla zrušena, virtuální počítač není ovlivněná a je vyloučen z iterace plánované údržby. Můžete se kontaktovat v později pomocí nového plánu a nabízí nové příležitosti k plánování a pořadí dopad na virtuální počítače.