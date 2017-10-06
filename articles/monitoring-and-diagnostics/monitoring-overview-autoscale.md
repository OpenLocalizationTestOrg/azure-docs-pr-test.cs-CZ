---
title: "aaaOverview škálování ve virtuálních počítačích Microsoft Azure, cloudové služby a webové aplikace | Microsoft Docs"
description: "Přehled automatického škálování v Microsoft Azure. Platí tooVirtual počítačů, cloudové služby a webové aplikace."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 74bf03be-e658-4239-a214-c12424b53e4c
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2016
ms.author: robb
ms.openlocfilehash: ef68b722ea22c4149a7d7a89c5855ba761db9247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Přehled automatického škálování ve virtuálních počítačích Microsoft Azure, cloudové služby a webové aplikace
Tento článek popisuje, co Microsoft Azure při automatickém škálování je, její výhody, a způsob spuštění tooget jeho použití.  

Škálování Azure monitorování platí pouze příliš[sady škálování virtuálního počítače](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [cloudové služby](https://azure.microsoft.com/services/cloud-services/), a [služby App Service – webové aplikace](https://azure.microsoft.com/services/app-service/web/).

> [!NOTE]
> Azure má dvě metody automatického škálování. Starší verze škálování platí tooVirtual počítače (skupiny dostupnosti). Tato funkce má omezenou podporu a doporučujeme migrace škálování toovirtual počítače nastaví pro podporu rychlejší a spolehlivější škálování. Odkaz na tom, jak toouse hello starší technologie je součástí tohoto článku.  
>
>

## <a name="what-is-autoscale"></a>Co je automatické škálování?
Škálování umožňuje toohave hello správného množství prostředků systémem toohandle hello zatížení vaší aplikace. Umožňuje vám tooadd prostředků toohandle nárůst zatížení a také ušetřit peníze odebráním prostředky, které jsou uložený nečinnosti. Zadejte minimální a maximální počet instancí toorun a přidat nebo odebrat virtuální počítače automaticky na základě sady pravidel. S minimální díky, že aplikace je vždy spuštěna ani v žádné zatížení. S maximální omezuje vaše náklady možné každou hodinu. Automatické škálování mezi těmito dvěma hranicemi pomocí pravidel, které vytvoříte.

 ![Škálování vysvětlené. Přidání a odebrání virtuálních počítačů](./media/monitoring-overview-autoscale/AutoscaleConcept.png)

Pokud jsou splněny podmínky pravidla, vyvolají se jeden nebo víc akcí škálování. Můžete přidat a odebrat virtuální počítače nebo provádět další akce. Hello následující koncepční diagram znázorňuje tento proces.  

 ![Vývojový Diagram škálování](./media/monitoring-overview-autoscale/Autoscale_Overview_v4.png)

Hello vysvětlení níže vztahuje toohello údaje předchozímu diagramu hello.   

## <a name="resource-metrics"></a>Metrika prostředků
Emitování metriky prostředky, tyto metriky později zpracovává pravidla. Metriky pocházet pomocí různých metod.
Sady škálování virtuálního počítače pomocí telemetrická data z Azure diagnostics agentů, zatímco telemetrie pro webové aplikace a cloudové služby pochází přímo z hello infrastruktury Azure. Běžně používané statistikami zahrnují využití procesoru, využití paměti, počet vláken, délka fronty a využití disku. Seznam co telemetrická data, můžete použít, naleznete v části [běžné metriky automatického škálování](insights-autoscale-common-metrics.md).

## <a name="custom-metrics"></a>Vlastní metriky
Můžete využít i vlastní vlastní metriky, které vaše aplikace může být generování. Pokud vaše aplikace toosend metriky tooApplication statistiky, které můžete využít tyto metriky toomake rozhodnutí na tom, zda jste nakonfigurovali tooscale nebo ne. 

## <a name="time"></a>Čas
Pravidla na základě plánu jsou založené na UTC. Časové pásmo je nutné nastavit správně při nastavování pravidel.  

## <a name="rules"></a>Pravidla
Hello diagram znázorňuje pouze jedno pravidlo automatického škálování, ale může mít mnoho z nich. Můžete vytvořit komplexní překrývající se pravidla, podle potřeby pro vaši situaci.  Zahrnout typy pravidel  

* **Na základě metrika** – například tuto akci proveďte, pokud využití CPU je vyšší než 50 %.
* **Založené na čase** – například aktivační události webhooku každých 8: 00 na sobotu v daném časovém pásmu.

Na základě metrika pravidla měření zatížení aplikace a přidat nebo odebrat podle tohoto zatížení virtuálních počítačů. Na základě plánu pravidla umožňují tooscale při najdete v části vzory čas v zatížení a chcete tooscale před možné zatížení zvýšení nebo snížení.  

## <a name="actions-and-automation"></a>Akce a automatizace
Pravidla můžete aktivovat jeden nebo více typů akcí.

* **Škálování** -škálování virtuálních počítačů příchozí nebo odchozí
* **E-mailu** -odesílat e-mailu toosubscription admins, spolusprávci nebo další e-mailovou adresu, které zadáte
* **Automatizovat pomocí webhooků** -volání webhooků, které můžete aktivovat více komplexní akcí uvnitř nebo mimo Azure. Uvnitř Azure můžete spustit runbook automatizace Azure, funkce Azure nebo Azure Logic Apps. Příklad adresy URL třetích stran mimo Azure zahrnují služby, jako je Slacku a Twilio.

## <a name="autoscale-settings"></a>Nastavení automatického škálování
Škálování použijte hello terminologie a struktura.

- **Nastavení automatického škálování** přečte hello škálování modul toodetermine jestli tooscale nahoru nebo dolů. Obsahuje jeden nebo více profilů, informace o hello cílový prostředek a nastavení oznámení.

    - **Škálování profil** je kombinací a:

        - **Nastavení kapacity**, což naznačuje hello minimální, maximální a výchozí hodnoty pro počet instancí.
        - **Sada pravidel**, z nichž každý obsahuje aktivační událost (čas nebo metrika) a akce škálování (nahoru nebo dolů).
        - **opakování**, což naznačuje, když put tento profil platit škálování.

        Může mít několik profilů, které umožňují tootake péče o různé překrývající se požadavky. Profilů automatického škálování různých může mít například pro různé časy den nebo dny v týdnu hello.

    - A **nastavení oznámení** definuje, k jaké oznámení by mělo dojít, když dojde k události škálování podle které splňují kritéria hello jeden z profilů nastavení automatického škálování hello. Škálování můžete oznámit jeden nebo více e-mailové adresy nebo zajistěte, aby tooone volání nebo další webhooky.


![Nastavení automatického škálování Azure, profil a strukturu pravidla](./media/monitoring-overview-autoscale/AzureResourceManagerRuleStructure3.png)

Hello úplný seznam konfigurovat polí a popisy je k dispozici v hello [škálování REST API](https://msdn.microsoft.com/library/dn931928.aspx).

Příklady kódu najdete v tématu

* [Upřesňující konfigurace automatického škálování pro škálovatelné sady virtuálních počítačů pomocí šablony Resource Manageru](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [Škálování REST API](https://msdn.microsoft.com/library/dn931953.aspx)

## <a name="horizontal-vs-vertical-scaling"></a>Svislé škálování vodorovné vs
Škálování pouze škáluje vodorovně, což je zvýšení ("na") nebo zmenšit ("v") v hello počet instancí virtuálního počítače.  Vodorovný je flexibilnější v situaci, cloud jako umožňuje toorun potenciálně tisíců toohandle zatížení virtuálních počítačů.

Naproti tomu svislé škálování se liší. Se udržuje hello stejný počet virtuálních počítačů, ale díky hello virtuální počítače ("nahoru") více nebo méně ("dolů") výkonné. Výkon se měří v paměť, rychlost procesoru, místo na disku atd.  Svislé škálování má další omezení. Je závislý na dostupnosti hello větší hardwaru, který rychle dotkne horní limit a můžete se liší podle oblasti. Svislý škálování také obvykle vyžaduje toostop virtuálních počítačů a znovu spusťte.

Další informace najdete v tématu [svisle škálování virtuální počítač Azure s Azure Automation](../virtual-machines/linux/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="methods-of-access"></a>Metody přístupu
Můžete nastavit automatické škálování prostřednictvím

* [Azure Portal](insights-how-to-scale.md)
* [PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)
* [Napříč platformami rozhraní příkazového řádku (CLI)](insights-cli-samples.md#autoscale)
* [Rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931953.aspx)

## <a name="supported-services-for-autoscale"></a>Podporované služby pro škálování
| Služba | Schéma & dokumentace |
| --- | --- |
| Web Apps |[Škálování webové aplikace](insights-how-to-scale.md) |
| Cloud Services |[Škálování cloudové služby](../cloud-services/cloud-services-how-to-scale.md) |
| Virtuální počítače: Classic |[Škálování sady dostupnosti Classic virtuálního počítače](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Virtuálních počítačů: Sady škálování Windows |[Škálování virtuálního počítače nastaví v systému Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) |
| Virtuálních počítačů: Nastaví Linux škálování |[Škálování virtuálního počítače nastaví v systému Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Virtuálních počítačů: Příklad Windows |[Upřesňující konfigurace automatického škálování pro škálovatelné sady virtuálních počítačů pomocí šablony Resource Manageru](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## <a name="next-steps"></a>Další kroky
toolearn Další informace o škálování, použijte hello škálování návody uvedených výše nebo prostudujte toohello následující prostředky:

* [Azure monitorování běžné metriky automatického škálování](insights-autoscale-common-metrics.md)
* [Osvědčené postupy pro monitorování Azure škálování](insights-autoscale-best-practices.md)
* [Použít automatické škálování akce toosend e-mailu a webhooku oznámení výstrah](insights-autoscale-to-webhook-email.md)
* [Škálování REST API](https://msdn.microsoft.com/library/dn931953.aspx)
* [Řešení potíží škálování sady škálování virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
