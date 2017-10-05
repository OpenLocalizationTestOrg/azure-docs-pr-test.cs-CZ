---
title: "Vytvoření sady škálování virtuálního počítače pomocí portálu Azure | Microsoft Docs"
description: "Nasazení sad škálování pomocí portálu Azure."
keywords: "Sady škálování virtuálního počítače"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7157a429829974b45dad29ac53fb5fb46c71f821
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-the-azure-portal"></a>Jak vytvořit sadu škálování virtuálního počítače pomocí portálu Azure
Tento kurz ukazuje, jak je snadné vytvořit sadu škálování virtuálního počítače v několika málo minut pomocí portálu Azure. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Volba image virtuálního počítače z Marketplace
Z portálu můžete snadno nasadit škálování s CentOS, CoreOS, Debian, otevřete Suse, Red Hat Enterprise Linux, SUSE Linux Enterprise Server, Ubuntu Server nebo bitové kopie systému Windows Server.

První, přejděte na [portál Azure](https://portal.azure.com) ve webovém prohlížeči. Klikněte na tlačítko `New`, vyhledejte `scale set`a pak vyberte `Virtual machine scale set` položku:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-the-scale-set"></a>Vytvoření škálovací sady
Teď můžete použít výchozí nastavení a rychle vytvořit byly sadou škálování.

* Na `Basics` okno, zadejte název pro sadu škálování. Tento název bude základem plně kvalifikovaný název domény služby Vyrovnávání zatížení před byly sadou škálování, proto se ujistěte, že název je jedinečný všechny Azure.
* Vyberte požadovanou operačním systému zadejte, zadejte své požadované uživatelské jméno a vyberte typ ověřování, které dávají přednost. Pokud si zvolíte heslo, musí být nejméně 12 znaků dlouhé a splňovat tři ze čtyř následujících složitost: jedno malé písmeno, jedno velké písmeno, jedna číslice a jeden speciální znak. Přečtěte si další informace o [požadavcích na uživatelské jméno a heslo](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm). Pokud se rozhodnete `SSH public key`, nezapomeňte pouze vložení v veřejný klíč, ne privátní klíč:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Zvolte, zda chcete omezit sad do jednoho umístění skupiny škálování nebo jestli pokrývat více skupin pro umístění. Což sad rozložit umístění skupiny škálování umožňuje škálování nastaví více než 100 virtuálních počítačů v kapacity (až 1 000) s některými omezeními. Další informace najdete v tématu [této dokumentace](./virtual-machine-scale-sets-placement-groups.md).
* Zadejte název skupiny požadovaných prostředků a umístění a pak klikněte na tlačítko `OK`.
* Na `Virtual machine scale set service settings` okno: Zadejte vaše Popisek názvu domény požadované (základ plně kvalifikovaný název domény pro vyrovnávání zatížení před byly sadou škálování). Tento popisek musí být jedinečný v rámci všech Azure.
* Zvolte si image disku požadovaný operační systém, počet instancí a velikost počítače.
* Vyberte typ požadovaný disk: spravované nebo nespravované. Další informace najdete v tématu [této dokumentace](./virtual-machine-scale-sets-managed-disks.md). Pokud jste vybrali možnost sad škálování rozmístěny v několika skupinách pro umístění, tato možnost nebude dostupná, protože je vyžadován pro sady škálování rozložit umístění skupin spravovaných disků.
* Povolit nebo zakázat automatické škálování a nakonfigurujte, pokud je povoleno:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* Na `Summary` okno, pokud se provádí ověřování, klikněte na tlačítko `OK` zahájíte měřítka nastavit nasazení.


## <a name="connect-to-a-vm-in-the-scale-set"></a>Připojit k virtuálnímu počítači v sadě škálování
Pokud jste zvolili pro omezení vaší škálování nastavit do jednoho umístění skupiny, pak byly sadou škálování se nasadí s nakonfigurovaná na umožňují připojit se ke stupnici snadno nastavit (v opačném případě se připojit k virtuálním počítačům v sadě škálování, je pravděpodobně potřeba vytvořit jumpbox ve stejné pravidla NAT  virtuální síť jako sada škálování). Chcete-li vidět, přejděte na `Inbound NAT Rules` kartě Nástroje pro vyrovnávání zatížení pro škálovací sadu:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

Pro každý virtuální počítač ve škálovací nastavit pomocí těchto pravidel NAT můžete připojit. Pro instanci pro sadu škálování systému Windows, pokud je pravidlo NAT na příchozí port 50000, můžete se připojit k tomuto počítači pomocí protokolu RDP na `<load-balancer-ip-address>:50000`. Pro sadu škálování Linux byste připojili pomocí příkazu `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Další kroky
Dokumentace k nasazení škálování nastaví z rozhraní příkazového řádku, najdete v části [této dokumentace](virtual-machine-scale-sets-cli-quick-create.md).

Dokumentace k nasazení škálování nastaví z prostředí PowerShell, najdete v části [této dokumentace](virtual-machine-scale-sets-windows-create.md).

Dokumentace k nasazení škálování nastaví ze sady Visual Studio, najdete v části [této dokumentace](virtual-machine-scale-sets-vs-create.md).

Pro obecnou dokumentací, podívejte se [stránky přehled dokumentace pro sady škálování](virtual-machine-scale-sets-overview.md).

Obecné informace, podívejte se [hlavní cílová stránka pro sady škálování](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

