---
title: "aaaWorking s velké sady škálování virtuálního počítače Azure | Microsoft Docs"
description: "Co je třeba sad škálování tooknow toouse velký virtuální počítač Azure"
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
ms.date: 2/7/2017
ms.author: guybo
ms.openlocfilehash: a39aab25925d7fc50763f0a20148b1f2213b492f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-large-virtual-machine-scale-sets"></a>Práce s velkými škálovacími sadami virtuálních počítačů
Nyní můžete vytvořit Azure [sady škálování virtuálního počítače](/azure/virtual-machine-scale-sets/) s kapacitou až too1 000 virtuálních počítačů. V tomto dokumentu _škálovací sadu virtuálních počítačů velké_ je definován jako škálování nastavit podporující škálování toogreater než 100 virtuálních počítačů. Tato funkce se nastavuje pomocí vlastnosti škálovací sady (_singlePlacementGroup=False_). 

Nastaví konkrétních aspektů velkém měřítku, například zatížení vyrovnávání a selhání domény chovat jinak tooa standardní škálovací sadu. Tento dokument popisuje vlastnosti hello sad velkém měřítku a popisuje, co je třeba tooknow toosuccessfully používat ve svých aplikacích. 

Běžný postup nasazení infrastruktury cloudu ve velkém měřítku je toocreate sadu _jednotek škálování_, třeba tak, že vytvoříte víc virtuálních počítačů škálování sad napříč více virtuálních sítí a účty úložiště. Tento přístup poskytuje snazší správa ve srovnání toosingle virtuální počítače a víc jednotek škálování jsou užitečné pro mnoho aplikací, zejména ty, které vyžadují další stohovatelných komponenty, jako jsou více virtuálních sítí a koncových bodů. Pokud vaše aplikace vyžaduje ale jeden cluster velké, může být více přehledné toodeploy jeden škálování, z nastavení too1 000 virtuálních počítačů. Ukázkové scénáře zahrnují centralizovaná nasazení pro velké objemy dat nebo výpočetní sítě, které vyžadují jednoduchou správu velkého fondu pracovních uzlů. V kombinaci s sadu škálování virtuálního počítače [připojené datových disků](virtual-machine-scale-sets-attached-disks.md), nastaví velkém měřítku umožňují toodeploy škálovatelné infrastruktury sestávající z tisíce jader a petabajty úložiště, jako jednu operaci.

## <a name="placement-groups"></a>Skupiny umístění 
Díky _velké_ škálování nastavit zvláštní není hello počet virtuálních počítačů, ale počet hello _umístění skupiny_ obsahuje. Umístění skupiny je konstrukce podobné tooan Azure dostupnosti sada, s vlastní domén selhání a domén upgradu. Ve výchozím nastavení škálovací sada obsahuje jedinou skupinu umístění s maximální velikostí 100 virtuálních počítačů. Pokud škálování nastavena vlastnost s názvem _singlePlacementGroup_ nastavena too_false_, hello škálovací sadu můžete skládá z více skupin pro umístění a má rozsah 0-1 000 virtuálních počítačů. Pokud nastavíte výchozí hodnotu toohello _true_, sada škálování se skládá z jednoho umístění skupiny a má rozsah 0-100 virtuálních počítačů.

## <a name="checklist-for-using-large-scale-sets"></a>Kontrolní seznam pro používání velkých škálovací sad
jestli vaše aplikace provádět efektivní použití sad velkém měřítku, zvažte toodecide hello následující požadavky:

- Velké škálovací sady vyžadují službu Azure Managed Disks. Škálovací sady vytvořené bez disků služby Managed Disks vyžadují více účtů úložiště (jeden na každých 20 virtuálních počítačů). Nastavuje velkém měřítku jsou navrženou toowork výhradně pomocí tooreduce spravované disky omezuje riziko úložiště režii správy a tooavoid hello spuštěných do předplatného pro účty úložiště. Pokud nepoužijete spravované disků, je škálovací sadu virtuálních počítačů omezené too100.
- Sady škálování, které jsou vytvořené z imagů z Azure Marketplace můžete postupně škálovat too1 000 virtuálních počítačů.
- Škálovací sady vytvořené z vlastních bitových kopií (Image virtuálních počítačů můžete vytvořit a odeslat sami) můžete škálovat aktuálně too100 virtuálních počítačů.
- Sady škálování skládá z několika skupin umístění ještě nepodporuje vrstvy 4 Vyrovnávání zatížení s hello Vyrovnávání zatížení Azure. Pokud potřebujete toouse hello nástroj pro vyrovnávání zatížení Azure Ujistěte se, že sad škálování hello je nakonfigurované toouse umístění jednu skupinu, která se hello výchozí nastavení.
- S hello Azure Application Gateway Vyrovnávání zatížení vrstvy 7 je podporován pro všechny škálovací sady.
- Sada škálování je definován s jedinou podsítí – Ujistěte se, že podsíť pro všechny virtuální počítače hello potřebujete nemá dostatečně velký adresní prostor. Ve výchozím nastavení škálování hodnotu overprovisions (vytvoří navíc za virtuální počítače v době nasazení nebo škálování, které vám není účtován) tooimprove nasazení spolehlivost a výkon. Povolit pro větší než hello počet virtuálních počítačů, které máte v plánu tooscale na adresu % 20 místa.
- Pokud plánujete toodeploy hodně virtuálních počítačů, může být nutné toobe vyšší kvótami vaše výpočetní jádra.
- Domény selhání a upgradovací domény jsou konzistentní pouze v rámci skupiny umístění. Tato architektura nezmění hello celkové dostupnosti škálování nastavení, jako virtuální počítače jsou rovnoměrně rozdělené mezi odlišné fyzický hardware, se ale znamená, že pokud budete potřebovat tooguarantee jsou dva virtuální počítače na jiný hardware, ujistěte se, jsou v jiné chyby hello domény ve stejné skupině umístění. ID skupiny chyb doménu a umístění se zobrazují v hello _instanci zobrazení_ rozsahu nastavení virtuálního počítače. Hello zobrazení instance škálovací sady virtuálních počítačů si můžete prohlédnout v hello [Průzkumníka prostředků Azure](https://resources.azure.com/).


## <a name="creating-a-large-scale-set"></a>Vytvoření velké škálovací sady
Když vytvoříte škálování nastavit v hello portálu Azure, můžete povolit jeho tooscale toomultiple umístění skupiny podle nastavení hello _Limit tooa jeden umístění skupiny_ too_False_ možnost v hello _Základy_ okno. Pomocí sady too_False_ tuto možnost, můžete určit _Instance počet_ hodnotu až too1, 000.

![](./media/virtual-machine-scale-sets-placement-groups/portal-large-scale.png)

Můžete vytvořit s velkou škálou virtuálních počítačů, nastavit pomocí hello [rozhraní příkazového řádku Azure](https://github.com/Azure/azure-cli) _vytvořit az vmss_ příkaz. Tento příkaz nastaví inteligentního výchozí nastavení, jako je například velikost podsítě podle hello _počet instancí_ argument:

```bash
az group create -l southcentralus -n biginfra
az vmss create -g biginfra -n bigvmss --image ubuntults --instance-count 1000
```
Všimněte si, že hello _vmss vytvořit_ příkaz výchozí určité hodnoty konfigurace, pokud nezadáte je. hello toosee dostupné se možnosti, které můžete přepsat, zkuste:
```bash
az vmss create --help
```

Pokud vytváříte velkém měřítku, která nastavuje skládání šablonu Azure Resource Manager, zkontrolujte, zda šablona hello vytvoří sadu škálování založenou na discích spravovaných Azure. Můžete nastavit hello _singlePlacementGroup_ vlastnost too_false_ v hello _vlastnosti_ části hello _Microsoft.Compute/virtualMAchineScaleSets_ prostředků. Hello následující JSON fragment ukazuje hello začátku šablonu sady škálování, včetně kapacity hello 1 000 virtuálních počítačů a hello _"singlePlacementGroup": false_ nastavení:
```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "location": "australiaeast",
  "name": "bigvmss",
  "sku": {
    "name": "Standard_DS1_v2",
    "tier": "Standard",
    "capacity": 1000
  },
  "properties": {
    "singlePlacementGroup": false,
    "upgradePolicy": {
      "mode": "Automatic"
    }
```
Úplný příklad velkém měřítku nastavení šablony, získáte příliš[https://github.com/gbowerman/azure-myriad/blob/master/bigtest/bigbottle.json](https://github.com/gbowerman/azure-myriad/blob/master/bigtest/bigbottle.json).

## <a name="converting-an-existing-scale-set-toospan-multiple-placement-groups"></a>Převod existující škálovací sady toospan více skupin umístění
toomake existující sady škálování virtuálního počítače podporující škálování toomore než 100 virtuálních počítačů, je nutné toochange hello _singplePlacementGroup_ too_false_ vlastnost hello rozsahu nastavit modelu. Změna této vlastnosti se hello můžete otestovat [Průzkumníka prostředků Azure](https://resources.azure.com/). Najít existující sady škálování, vyberte _upravit_ a změňte hello _singlePlacementGroup_ vlastnost. Pokud nevidíte tuto vlastnost, může být zobrazení sad s starší verzi rozhraní API Microsoft.Compute hello škálování hello.

>[!NOTE] 
Můžete změnit škálování nastavit od podpora jednom umístění skupiny pouze (hello výchozí chování) tooa podpora více skupin umístění, ale nelze převést hello opačným způsobem. Proto ujistěte se, že rozumíte hello vlastnosti sad velkém měřítku před převodem. Konkrétně zkontrolujte, zda že není nutné vrstvy 4 Vyrovnávání zatížení s hello Vyrovnávání zatížení Azure.


