---
title: "Přehled sadách škálování virtuálních počítačů aaaAzure | Microsoft Docs"
description: "Přečtěte si podrobnosti o škálovacích sadách virtuálních počítačů Azure."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 788b2d1636e0bf4ef3fbf94aed9b3303c5fafa82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Co jsou škálovací sady virtuálních počítačů v Azure?
Sady škálování virtuálního počítače jsou prostředek výpočtů Azure, můžete použít toodeploy a spravovat sadu identické virtuální počítače. S všechny virtuální počítače nakonfigurované hello stejné, škálovatelné sady jsou true škálování navrženou toosupport a žádné předem zřizování virtuálních počítačů je požadována. Proto je snazší rozsáhlých služeb toobuild, které se zaměřují velkých výpočetních, velké objemy dat a kontejnerizované úlohy.

Pro aplikace, které potřebují tooscale výpočetní prostředky v škálování a operace jsou implicitně rovnoměrně rozdělen mezi doménách selhání a aktualizace. Nastaví další tooscale úvod, naleznete toohello [Azure blog oznámení](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Více se o škálovacích sadách dozvíte také v těchto videích:

* [Mark Russinovich hovoří o škálovacích sadách Azure](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Guy Bowerman provádí škálovacími sadami virtuálních počítačů](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-scale-sets"></a>Vytváření a správa škálovacích sad
Můžete vytvořit škálování nastavit v hello [portál Azure](https://portal.azure.com) výběrem **nové** a zadáním příkazu **škálování** na panelu Hledat hello. **Škálovací sadu virtuálních počítačů** je uvedena ve výsledcích hello. Odtud můžete vyplnit hello požadované pole toocustomize a nasadit škálovací sadu. Máte také možnosti tooset základní škálování pravidla na základě využití procesoru v aplikaci portál hello.

Škálovací sady můžete definovat a nasazovat pomocí šablon JSON a [rozhraní REST API](https://msdn.microsoft.com/library/mt589023.aspx), podobně jako jednotlivé virtuální počítače v Azure Resource Manageru. Proto můžete použít všechny standardní metody nasazení v Azure Resource Manageru. Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).

Můžete najít nastaví sadu příklad šablony pro škálování virtuálních počítačů v hello [úložiště GitHub šablony Azure Quickstart](https://github.com/Azure/azure-quickstart-templates). (Vyhledejte šablon s **vmss** v hlavě hello.)

Příklady šablony rychlý start hello propojí tlačítko "Nasadit tooAzure" v souboru readme hello ke každé šabloně funkce toohello portálu nasazení. škálování hello toodeploy nastavení, klikněte na tlačítko hello a pak zadejte parametry, které jsou požadovány hello portálu. 

## <a name="scaling-a-scale-set-out-and-in"></a>Horizontální navyšování a snižování kapacity škálovací sady
Můžete změnit hello kapacity měřítka, nastavte v hello portál Azure kliknutím hello **škálování** oddílu pod **nastavení**. 

na příkazovém řádku hello nastavená kapacita škály toochange, použijte hello **škálování** v [rozhraní příkazového řádku Azure](https://github.com/Azure/azure-cli). Například použijte tento příkaz tooset tooa kapacitou 10 virtuálních počítačů sady škálování:

```bash
az vmss scale -g resourcegroupname -n scalesetname --new-capacity 10 
```

tooset hello počet virtuálních počítačů v škálování nastavit pomocí prostředí PowerShell, použijte hello **aktualizace AzureRmVmss** příkaz:

```PowerShell
$vmss = Get-AzureRmVmss -ResourceGroupName resourcegroupname -VMScaleSetName scalesetname  
$vmss.Sku.Capacity = 10
Update-AzureRmVmss -ResourceGroupName resourcegroupname -Name scalesetname -VirtualMachineScaleSet $vmss
```

tooincrease nebo snižte počet hello virtuálních počítačů v škálování nastavení pomocí šablony Azure Resource Manager, změna hello **kapacity** vlastnosti a znovu ho zaveďte hello šablony. Tato jednoduchost umožňuje snadno toointegrate sady škálování s Azure škálování nebo toowrite vlastní škálování vrstvě Pokud potřebujete vlastní měřítko události toodefine této škálování Azure nepodporuje. 

Pokud jsou opětovného nasazení Azure Resource Manager šablony toochange hello kapacitu, můžete definovat mnohem menší šablonu, která obsahuje jenom hello **SKU** vlastnost paketu hello aktualizovat kapacity. [Tady je příklad](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing).

## <a name="autoscale"></a>Automatické škálování

Sada škálování může být volitelně nakonfigurovaný s nastavením škálování při jeho vytváření v hello portálu Azure. Hello počet virtuálních počítačů, můžete pak zvýší nebo sníží podle využití procesoru průměr. 

Řadu hello škálování sada šablony v hello [šablony Azure Quickstart](https://github.com/Azure/azure-quickstart-templates) definovat nastavení automatického škálování. Můžete také přidat tooan nastavení automatického škálování stávající sadě škálování. Tento skript prostředí Azure PowerShell například přidá sadě škálování tooa škálování na základě využití procesoru:

```PowerShell

$subid = "yoursubscriptionid"
$rgname = "yourresourcegroup"
$vmssname = "yourscalesetname"
$location = "yourlocation" # e.g. southcentralus

$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Increase -ScaleActionValue 1
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator LessThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Decrease -ScaleActionValue 1
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "autoprofile1"
Add-AzureRmAutoscaleSetting -Location $location -Name "autosetting1" -ResourceGroup $rgname -TargetResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -AutoscaleProfiles $profile1
```

Seznam tooscale platný metriky můžete najít na v [podporované metriky s Azure monitorování](../monitoring-and-diagnostics/monitoring-supported-metrics.md) pod hello záhlaví "Microsoft.Compute/virtualMachineScaleSets." Pokročilejší možnosti automatického škálování jsou také k dispozici, včetně na základě plánu škálování a pomocí webhooků toointegrate výstrahy systémy.

## <a name="monitoring-your-scale-set"></a>Monitorování škálovací sady
Hello [portál Azure](https://portal.azure.com) seznamy škálování nastaví a zobrazí jejich vlastnosti. portál Hello také podporuje operace správy. Operace správy můžete provádět se škálovacími sadami nebo s jednotlivými virtuálními počítači v rámci škálovací sady. portál Hello poskytuje také přizpůsobitelné prostředků graf využití. 

Pokud potřebujete toosee nebo upravit hello základní definici JSON prostředek služby Azure, můžete také použít [Průzkumníka prostředků Azure](https://resources.azure.com). Sady škálování jsou prostředek hello zprostředkovatele prostředků Microsoft.Compute Azure. Z tohoto webu můžete zjistit rozšířením hello následující odkazy:

**Subscriptions (Předplatná)** > **vaše předplatné** > **resourceGroups (skupiny prostředků)** > **providers (poskytovatelé)** > **Microsoft.Compute** > **virtualMachineScaleSets (škálovací sady virtuálních počítačů)** > **vaše škálovací sada** &gt; atd.

## <a name="scale-set-scenarios"></a>Scénáře použití škálovacích sad
Tato část uvádí některé typické scénáře použití škálovacích sad. Tyto scénáře využívají některé služby Azure vyšších úrovní (třeba Batch, Service Fabric nebo Container Service).

* **Pomocí protokolu RDP nebo SSH tooconnect tooscale nastavte instancí**: Sada škálování se vytvoří ve virtuální síti, a jednotlivé virtuální počítače ve škálovací sadě hello nejsou veřejné IP adresy přidělené ve výchozím nastavení. Tato zásada zabraňuje hello náklady a správu režie přidělování samostatné veřejných IP adres tooall hello uzly v mřížce vaše výpočetní. Pokud potřebujete přímé připojení k externím tooscale nastavit virtuální počítače, můžete nakonfigurovat škálování sadu tooautomatically přiřazení veřejné IP adresy toonew virtuálních počítačů. Případně můžete připojit tooVMs z jiné prostředky ve virtuální síti, kterou lze přidělit veřejné IP adresy, například nástroje pro vyrovnávání zatížení a samostatné virtuální počítače. 
* **Připojit tooVMs pomocí pravidel NAT**: můžete vytvořit veřejnou IP adresu, přiřadit ho tooa nástroj pro vyrovnávání zatížení a definovat příchozí fond NAT. Tyto akce se mapují porty hello IP adresa tooa port na virtuálním počítači v sadě škálování hello. Například:
  
  | Zdroj | Zdrojový port | Cíl | Cílový port |
  | --- | --- | --- | --- |
  |  Veřejná IP adresa |Port 50000 |vmss\_0 |Port 22 |
  |  Veřejná IP adresa |Port 50001 |vmss\_1 |Port 22 |
  |  Veřejná IP adresa |Port 50002 |vmss\_2 |Port 22 |
  
   V [v tomto příkladu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)pravidla NAT jsou definované tooenable tooevery připojení SSH virtuálního počítače ve škálovací sadě, pomocí jedné veřejné IP adresy.
  
   [Tento příklad](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat) stejné hello s RDP a systému Windows.
* **Připojit tooVMs pomocí "jumpbox"**: Pokud vytvoříte sadu škálování a samostatný virtuální počítač ve stejné virtuální síti hello hello samostatný virtuální počítač a hello škálovací sadu virtuálních počítačů můžete připojit tooone jiné pomocí jejich interních IP adres, podle definice virtuálním hello síť nebo podsíť. Pokud chcete vytvořit veřejnou IP adresu a přiřadit ji toohello samostatných virtuálních počítačů, můžete použít protokolu RDP nebo SSH tooconnect toohello samostatných virtuálních počítačů. Pak můžete připojit z že tooyour škálovací sady počítačů instancí. V tomto bodě si můžete všimnout, že jednoduchá škálovací sada je ze své podstaty bezpečnější než jednoduchý samostatný virtuální počítač s veřejnou IP adresou ve své výchozí konfiguraci.
  
   Například [tato šablona](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox) nasadí jednoduchou škálovací sadu se samostatným virtuálním počítačem. 
* **Instance sady tooscale Vyrovnávání zatížení**: Pokud chcete toodeliver pracovní tooa výpočetní cluster virtuálních počítačů pomocí kruhového dotazování, můžete nakonfigurovat k nástroji pro vyrovnávání zatížení Azure pomocí pravidel Vyrovnávání zatížení vrstvy 4 odpovídajícím způsobem. Můžete definovat sondy tooverify, aplikace se službou a otestováním porty zadaný protokol, intervalu a cesta k požadavku. [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) podporuje jak škálovací sady, tak úroveň 7 i sofistikovanější scénáře vyrovnávání zatížení.
  
   [Tento příklad](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) vytvoří sada škálování, které běží Apache webové servery a používá zatížení hello toobalance nástroje pro vyrovnávání zatížení, která přijímá každý virtuální počítač. (Podívejte se na typ prostředku Microsoft.Network/loadBalancers hello a networkProfile a extensionProfile v sada škálování virtuálních počítačů.)

   [Tento příklad pro Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway) a [tento příklad pro Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway) používají Application Gateway.  

* **Nasazení škálovací sady jako výpočetního clusteru ve správci clusteru PaaS:** Škálovací sady se někdy označují jako role pracovních procesů nové generace. I když platný popis ho spustit hello riziko matoucí škálování sadu funkcí s funkcemi Azure Cloud Services. V jistém smyslu poskytují škálovací sady skutečné pracovní role pro pracovní prostředky. Jsou generalizovaným výpočetním prostředkem, který je nezávislý na platformě nebo modulu runtime, umožňuje přizpůsobení a integruje se s IaaS s nástrojem Azure Resource Manager.
  
   Pracovní role cloudové služby je omezena z hlediska podpory platformy/runtime (platí jen pro image platformy Windows). Zahrnuje ale také služby jako záměnu virtuální IP adresy, konfigurovatelné nastavení upgradu a nastavení pro konkrétní nasazení runtime nebo aplikace. Tyto služby *ještě* nejsou dostupné jako škálovací sady ani nejsou doručované jinými vysokoúrovňovými službami PaaS jako Azure Service Fabric. Na škálovací sady se můžete dívat jako na infrastrukturu, která podporuje model PaaS. Na této infrastruktuře jsou postavena řešení PaaS, jako je [Service Fabric](https://azure.microsoft.com/services/service-fabric/).
  
   V [tomto příkladu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos) takového přístupu nasadí [Azure Container Service](https://azure.microsoft.com/services/container-service/) cluster na základě škálovacích sad pomocí orchestrátoru kontejneru.

## <a name="scale-set-performance-and-scale-guidance"></a>Pokyny týkající se výkonu a škálování u škálovacích sad
* Nastavit škálování podporuje až too1 000 virtuálních počítačů. Pokud chcete vytvořit a nahrát vlastní vlastní Image virtuálních počítačů, hello limit je 100. Důležité informace o používání velkých škálovacích sad najdete v tématu [Práce s velkými škálovacími sadami virtuálních počítačů](virtual-machine-scale-sets-placement-groups.md).
* Nemáte toopre – vytvoření sady škálování toouse účty úložiště Azure. Podpora sad škálování Azure spravovat disky, které negate výkonu pochybnostmi hello počet disků na účet úložiště. Další informace najdete v tématu [Škálovací sady virtuálních počítačů Azure a spravované disky](virtual-machine-scale-sets-managed-disks.md).
* Zvažte použití služby Azure Storage úrovně Premium namísto úrovně Standard pro rychlejší a předvídatelnější zřizování virtuálních počítačů a vylepšení výkonu vstupně-výstupních operací.
* kvóta na jádra Hello v hello oblast, ve které nasazujete omezuje hello počet virtuálních počítačů můžete vytvořit. Můžete potřebovat toocontact zákaznickou podporu tooincrease limit kvóty vaší výpočetní, i v případě, že máte vysokou maximální počet jader pro použití se službou Azure Cloud Services ještě dnes. tooquery vaší kvóty, spusťte tento příkaz příkazového řádku Azure CLI: `azure vm list-usage`. Nebo pomocí následujícího příkazu PowerShellu: `Get-AzureRmVMUsage`.

## <a name="frequently-asked-questions-for-scale-sets"></a>Nejčastější dotazy ke škálovacím sadám
**Otázka:** Kolik virtuálních počítačů může obsahovat škálovací sada?

**Odpověď:** Sada škálování může mít 0 too1, 000 virtuálních počítačů založené na imagích platformy, nebo 0 too100 virtuální počítače založeno na vlastních bitových kopií. 

**Otázka:** Podporují se ve škálovacích sadách datové disky?

**Odpověď:** Ano. Sada škálování můžete definovat konfigurace disků připojená data, která se použije tooall virtuální počítače v sadě hello. Další informace najdete v tématu [Škálovací sady Azure a připojené datové disky](virtual-machine-scale-sets-attached-disks.md). Další možnosti ukládání dat zahrnují:

* Soubory Azure (sdílené jednotky SMB)
* Jednotka operačního systému
* Dočasné jednotky (místní úložiště, nezálohované pomocí Azure Storage)
* Datová služba Azure (např. tabulky Azure, objekty blob Azure)
* Externí datová služba (např. vzdálená databáze)

**Otázka:** Které oblasti Azure podporují škálovací sady?

**Odpověď:** Všechny oblasti podporují škálovací sady.

**Otázka:** Jak se vytváří škálovací sada s použitím vlastní image?

**Odpověď:** Na základě virtuálního pevného disku vlastní image vytvoříte spravovaný disk, na který budete odkazovat v šabloně škálovací sady. [Tady je příklad](https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os).

**Otázka:** Pokud lze omezit Moje škálování nastavená kapacita z 20 too15, které jsou odebrány virtuální počítače?

**Odpověď:** Virtuální počítače jsou odebrány ze sad rovnoměrně napříč domény update a dostupnosti toomaximize domén selhání hello škálování. Virtuální počítače s hello, které jsou nejvyšší ID nejdřív odstranit.

**Otázka:** Co když poté zvýšit kapacitu hello z 15 too18?

**Odpověď:** Pokud zvýšíte kapacitu too18, vytvoří se 3 nové virtuální počítače. Pokaždé, když, hello ID instance virtuálního počítače se zvýší z hello předchozí nejvyšší hodnotu (například 20, 21, 22). Virtuální počítače se vyvažují mezi doménami selhání a aktualizačními doménami.

**Otázka:** Pokud ve škálovací sadě používám několik rozšíření, je možné vynucovat určitou posloupnost provádění?

**Odpověď:** Ne přímo, ale pro rozšíření customScript hello váš skript můžete čekat na jiné toofinish rozšíření. Další pokyny můžete získat na rozšíření sekvencování v hello příspěvku na blogu [rozšíření sekvencování v škálovatelné sady virtuálních počítačů Azure](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**Otázka:** Spolupracují škálovací sady se skupinami dostupnosti Azure?

**Odpověď:** Ano. Škálovací sada je implicitní skupina dostupnosti s 5 doménami selhání a 5 aktualizačními doménami. Škálování sady virtuálních počítačů více než 100 span více *umístění skupiny*, která jsou ekvivalentní toomultiple skupiny dostupnosti. Další informace o skupinách umístění najdete v tématu [Práce s velkými škálovacími sadami virtuálních počítačů](virtual-machine-scale-sets-placement-groups.md). Skupina dostupnosti virtuálních počítačů může existovat v hello stejné virtuální síti jako škálovací sadu virtuálních počítačů. Obvyklé konfigurace je tooput řídicí uzel virtuální počítače (které vyžadují často jedinečné konfigurace) ve skupině dostupnosti nastavena a umístí datové uzly v sadě škálování hello.

Můžete najít další odpovědi tooquestions o škálování nastaví v hello [sadách škálování virtuálních počítačů Azure – nejčastější dotazy](virtual-machine-scale-sets-faq.md).
