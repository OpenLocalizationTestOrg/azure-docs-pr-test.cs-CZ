---
title: "aaaImpactful údržby pro virtuální počítače Windows v Azure | Microsoft Docs"
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
ms.openlocfilehash: 98afaea0fdca796177e075b33615b03f1e7a0fdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="impactful-maintenance-for-virtual-machines"></a>Zvládat údržby pro virtuální počítače

Existuje několik případů, kde jsou virtuální počítače restartovat z důvodu toohello údržby tooplanned základní infrastruktury. Probíhá zvládat toothe dostupnost virtuální počítače hostované v Azure, následující hello jsou nyní k dispozici pro toouse můžete:

-   Minimálně 30 dnů před hello dopad odesláno oznámení.

-   Viditelnost toohello údržbu na každý virtuální počítač.

-   Flexibilita a řízení v nastavení hello přesný čas pro údržbu měly týkat virtuálních počítačů.

Údržby v Microsoft Azure je naplánován v iterací. Počáteční iterací mít menší obor v pořadí tooreduce hello riziko při zavádění nové opravy a funkce. Novější iterací může rozmístěny v několika oblastech (nikdy z hello stejného páru oblasti). Virtuální počítač je součástí jedné údržby iterací. Pokud byl přerušen iterace, zbývající virtuální počítače jsou součástí jiného, budoucí, iterací.

Hello plánované údržby iterace má dvě fáze: Pre-emptive údržby a naplánovaná údržby.

Hello **Pre-emptive údržby** vám poskytne hello flexibilitu tooinitiate hello údržby na virtuálních počítačů. Díky tomu můžete určit, kdy dopad na virtuální počítače, hello pořadí hello aktualizace a hello čas mezi jednotlivé virtuální počítače, které neudržují. Můžete dotazovat toosee každý virtuální počítač, zda je plánovaná pro údržbu a zkontrolujte výsledek hello vaší poslední žádosti initiated údržby.

Hello **naplánované údržby** je Azure má plánu virtuálních počítačů pro údržbu hello. Během této doby čas, který následuje preemptivní údržby, můžete pro hello údržby se dotazovat, ale již nebude možné tooorchestrate hello údržby.

## <a name="availability-considerations-during-planned-maintenance"></a>Důležité informace o dostupnosti během plánované údržby 

### <a name="paired-regions"></a>Spárované oblastí

Každé oblasti Azure je spárován s jinou oblast v rámci hello stejné geography, společně provedení pár místní. Při provádění údržby, bude Azure aktualizovat pouze hello instance virtuálních počítačů v jedné oblasti jeho dvojic. Například při aktualizaci hello virtuálních počítačů v Severní jihu USA, Azure nebude aktualizujte všechny virtuální počítače v jihu USA v hello stejnou dobu. To se naplánuje na jindy – díky tomu je umožněno převzetí služeb při selhání nebo vyrovnávání zatížení mezi oblastmi. Ale jiných oblastech, jako je Severní Evropa může být v rámci údržby v hello stejný čas jako východní USA.
Další informace o [oblast Azure páry](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

### <a name="single-instance-vms-vs-availability-set-or-vm-scale-set"></a>Vs jedné Instance virtuálních počítačů. Sadu škálování virtuálního počítače nebo sady dostupnosti.

Pokud nasazujete zatížení používání virtuálních počítačů v Azure, můžete vytvořit hello virtuálních počítačů v rámci dostupnosti nastavte v pořadí tooprovide vysokou dostupnost tooyour aplikaci. Tato konfigurace zajistí, že během výpadku nebo údržby událostí, je k dispozici alespoň jeden virtuální počítač.

V rámci skupiny dostupnosti jednotlivé virtuální počítače jsou rozloženy až too20 aktualizaci domény. Během plánované údržby je v každém okamžiku dopad pouze jednu aktualizaci domény. Hello pořadí aktualizace domén ovlivněný nemusí pokračovat postupně během plánované údržby. Pro jeden instance virtuálních počítačů (není součástí skupiny dostupnosti), neexistuje žádný způsob, jak toopredict nebo určit, který a společně dopad na tom, kolik virtuálních počítačů.

Sady škálování virtuálního počítače se Azure výpočtový prostředek, který vám umožní toodeploy a spravovat sadu identické virtuální počítače jako jeden prostředek.
Hello škálovací sadu poskytuje podobné záruky tooan skupinou dostupnosti ve formě aktualizaci domény. 

Další informace o konfiguraci virtuálních počítačů pro zajištění vysoké dostupnosti najdete v tématu [ *Správa dostupnosti virtuálních počítačů Windows hello*](../linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="scheduled-events"></a>Naplánované události

Služba Azure Metadata vám umožní toodiscover informace o virtuální počítač hostovaný v Azure. Naplánované události, některou z kategorií hello vystavený, povrchy informace týkající se nadcházející události (například restartování) tak, aby vaše aplikace můžete připravit pro ně a omezit přerušení.

Další informace o naplánované události, najdete v části příliš[Azure Metadata Service - naplánované události](../virtual-machines-scheduled-events.md).

## <a name="maintenance-discovery-and-notifications"></a>Zjišťování údržby a oznámení

Plán údržby je viditelná toocustomers na úrovni hello jednotlivých virtuálních počítačů. Můžete použít Azure portal, tooquery rozhraní API, Powershellu nebo rozhraní příkazového řádku pro windows preemptivní a plánované údržby. Kromě toho, na které přijde upozornění (e-mailu) v případě hello očekávat tam, kde jsou během procesu hello dopad jeden (nebo více) z virtuálních počítačů.

Preemptivní údržby a plánované údržby fáze začínat oznámení. Očekávejte tooreceive jeden oznámení za předplatné Azure. Hello je odesláno oznámení správce a spolusprávce předplatného toohello ve výchozím nastavení. Můžete také nakonfigurovat hello cílová skupina pro oznámení údržby.

### <a name="view-hello-maintenance-window-in-hello-portal"></a>Zobrazení hello údržby hello portálu 

Můžete použít hello portál Azure a vyhledejte naplánována údržba virtuálních počítačů.

1.  Přihlaste se toohello portálu Azure.

2.  Klikněte na tlačítko a otevřete hello **virtuální počítače** okno.

3.  Klikněte na tlačítko hello **sloupce** tlačítko tooopen hello seznam dostupných sloupců toochoose z

4.  Vyberte a přidejte hello **údržby** sloupce. Virtuální počítače, které jsou naplánovány pro údržbu mít hello prezentované časová období údržby. Jakmile je byla dokončena nebo přerušena z důvodu údržby a, časové období údržby se už nebude zobrazovat.

### <a name="query-maintenance-details-using-hello-azure-api"></a>Podrobnosti o údržby dotazu pomocí hello rozhraní API služby Azure

Použití hello [získat informace o virtuálních počítačů API](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-get) a vyhledejte hello instance zobrazit toodiscover hello údržby podrobnosti o jednotlivých virtuálních počítačů. Hello odpověď obsahuje hello následující prvky:

  - isCustomerInitiatedMaintenanceAllowed: Určuje, zda nyní můžete zahájit preemptivní znovu ho zaveďte na hello virtuálních počítačů.

  - preMaintenanceWindowStartTime: hello čas zahájení údržby preemptivní hello.

  - preMaintenanceWindowEndTime: hello koncový čas hello preemptivní údržby. Po této době už nebude možné tooinitiate údržby na tomto virtuálním počítači.
    
  - maintenanceWindowStartTime: hello počáteční čas období naplánované údržby hello při dopad na virtuální počítač.

  - maintenanceWindowEndTime: hello koncový čas hello plánované údržby.
  
  - lastOperationResultCode: hello výsledek vaší poslední operace údržby-znovu ho zaveďte.
 
  - lastOperationMessage: zpráva popisující hello výsledek vaší poslední operace údržby-znovu ho zaveďte.

## <a name="pre-emptive-redeploy"></a>Preemptivní znovu ho zaveďte

Preemptivní znovu ho zaveďte akce poskytuje hello flexibilitu toocontrol hello čas, kdy údržby je použité tooyour virtuálních počítačů v Azure. Plánovaná údržba začíná preemptivní údržby kde se můžete rozhodnout na hello přesný čas pro každé toobe vaše virtuální počítače restartovat. Tady jsou případy použití, kde je užitečné tyto funkce:

-   Toobe nutné údržby koordinované s hello koncového zákazníka.

-   okno plánované údržby Hello, které nabízí Azure není dostatečná.
    Je možné, že hello okno se stane toobe na hello nejvytíženější času v týdnu nebo je příliš dlouhá.

-   Pro více instancemi nebo vícevrstvé aplikace budete potřebovat dostatečnou dobu mezi dva virtuální počítače nebo určité pořadí toofollow.

Při volání pro preemptivní znovu ho zaveďte na virtuálním počítači, přesune ho tooan hello virtuálního počítače již aktualizován uzel a potom zapne ji zpět, zachování všech možností konfigurace a přidružených prostředků. Když to uděláte, dočasným diskovým hello dojde ke ztrátě a dynamické IP adresy přidružené k rozhraní virtuální sítě se aktualizují.

Preemptivní znovu ho zaveďte je povoleno jednou na virtuální počítač. Pokud dojde k chybě během procesu hello, hello operace byla přerušena, hello virtuálního počítače není ovlivněná a je vyloučen z iterace hello plánované údržby. Můžete se kontaktovat v později pomocí nového plánu a nabízí novou možnost tooschedule a pořadí hello dopadu na virtuální počítače.