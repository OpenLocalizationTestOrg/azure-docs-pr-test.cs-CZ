---
title: "aaaAdd síťové rozhraní tooor odebrat z virtuálních počítačů Azure | Microsoft Docs"
description: "Zjistěte, jak odebrat tooadd síťové rozhraní tooor síťová rozhraní z virtuálních počítačů."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: jdial
ms.openlocfilehash: eb564b94932b9242f29bc23234b08b0c76ac23a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-network-interfaces-tooor-remove-from-virtual-machines"></a>Přidání síťových rozhraní tooor odebrat z virtuálních počítačů

Zjistěte, jak tooadd existující síť rozhraní při vytváření virtuálního počítače nebo přidat nebo odebrat síťová rozhraní ze stávajícího virtuálního počítače v hello (deallocated) stavu Zastaveno. Síťové rozhraní umožňuje toocommunicate Azure virtuálního počítače (VM) s Internetem, Azure a místních prostředků. Virtuální počítač může mít jeden nebo víc síťových rozhraní. 

Pokud potřebujete tooadd, změnit nebo odebrat IP adresy pro síťové rozhraní, přečtěte si hello [Správa IP adres rozhraní sítě](virtual-network-network-interface-addresses.md) článku. Pokud potřebujete toocreate, měnit a odstraňovat síťová rozhraní, přečtěte si hello [Spravovat síťová rozhraní](virtual-network-network-interface.md) článku.

## <a name="before"></a>Než začnete

Dokončení hello následující úkoly před dokončením všechny kroky v žádné části tohoto článku:

- Další informace o tom, kolik síťových rozhraní velikost pro všechny systémy Linux a virtuální počítač s Windows podporuje kontrolou hello [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálních počítačů.
- Přihlaste se toohello Azure [portál](https://portal.azure.com), rozhraní příkazového řádku Azure (CLI) nebo Azure PowerShell s účet Azure. Pokud nemáte účet Azure, si zaregistrovat [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud pomocí prostředí PowerShell příkazy toocomplete úlohy v tomto článku [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Zajistěte, že abyste měli nejnovější verzi rutin prostředí Azure PowerShell hello nainstalován hello. Zadejte tooget nápovědy pro příkazy prostředí PowerShell s příklady, `get-help <command> -full`.
- Pokud pomocí rozhraní příkazového řádku Azure (CLI) příkazy toocomplete úlohy v tomto článku [instalace a konfigurace rozhraní příkazového řádku Azure hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi hello hello nainstalované rozhraní příkazového řádku Azure. tooget nápovědy pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Namísto instalaci hello rozhraní příkazového řádku a jeho požadavky můžete použít hello prostředí cloudu Azure. Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure. Má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem. toouse hello cloudové prostředí, klikněte na tlačítko hello cloudové prostředí **> _** tlačítko hello horní části hello [portál](https://portal.azure.com).

## <a name="about"></a>O rozhraní sítě a virtuální počítače

Můžete přidat (připojení) existující tooa rozhraní sítě virtuálních počítačů při vytváření hello virtuálních počítačů, zadaný hello síťového rozhraní není aktuálně připojený tooanother virtuálních počítačů. Můžete přidat síťové rozhraní k nebo odebrat (odpojit) k síti rozhraní toofrom existující virtuální počítač, zadaný hello virtuálních počítačů v hello zastavena (deallocated) stavu. Pokud vytvoříte virtuální počítač pomocí hello portálu Azure, portál hello vytvoří síťové rozhraní s výchozím nastavením. portál Hello neumožňuje vám:

- Při vytváření hello virtuálního počítače zadat existující tooadd rozhraní sítě
- Vytvoření virtuálního počítače s několika síťovými rozhraními
- Zadejte název pro rozhraní sítě hello (hello portál vytvoří hello síťové rozhraní s výchozím názvem)

Prostředí Azure PowerShell nebo rozhraní příkazového řádku toocreate hello rozhraní sítě nebo virtuálního počítače můžete použít s všechny atributy předchozí hello, které nelze použít hello portálu. Před dokončením hello úkoly v následující části hello, zvažte následující hello omezení a chování:

- Všechny velikosti virtuálních počítačů podpory aspoň dvě rozhraní sítě, avšak některé velikosti virtuálních počítačů podporovat více než dvě síťová rozhraní. V hello za některých virtuálních počítačů podporovány velikosti pouze jedno síťové rozhraní. toolearn kolik síťových rozhraní, které podporuje každý velikost virtuálního počítače, přečtěte si hello [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálních počítačů. 
- V posledních hello může být síťová rozhraní pouze přidané tooVMs, který podporovat více síťovými rozhraními a byly vytvořeny s aspoň dvě rozhraní sítě. Tooa rozhraní sítě virtuálního počítače, který byl vytvořen s jedním síťovým rozhraním, nelze přidat, i když hello velikost virtuálního počítače podporované více síťových rozhraní. Naopak síťových rozhraní může odebrat pouze z virtuálního počítače s aspoň tři síťových rozhraní, protože virtuální počítače vytvořené pomocí aspoň dvě rozhraní sítě vždy měli toohave alespoň dva síťové rozhraní. Ani jeden z těchto omezení platí už. Můžete nyní vytvořte virtuální počítač s libovolný počet síťových rozhraní (telefonní číslo toohello nepodporuje hello velikost virtuálního počítače) a přidat nebo odebrat libovolný počet síťových rozhraní (z virtuálních počítačů v hello zastavena (nepřiřazeném) stavu), tak dlouho, dokud hello virtuálních počítačů má vždy alespoň jedno síťové rozhraní.
- Ve výchozím nastavení, hello první síťové rozhraní na virtuální počítač je definován jako hello *primární* síťové rozhraní. Všechny ostatní síťová rozhraní hello virtuálních počítačů jsou *sekundární* síťových rozhraní.
- Primární síťová rozhraní jsou servery Azure DHCP hello přiřazené výchozí bránu, ale nejsou sekundární síťová rozhraní. Vzhledem k tomu, že sekundární síťová rozhraní nejsou přiřazené výchozí bránu, nemohou komunikovat s prostředky mimo jejich podsíť, ve výchozím nastavení. tooenable sekundární síťová rozhraní toocommunicate virtuální počítač s Windows s prostředky mimo jejich podsíť, přidejte trasy toohello operačního systému pomocí hello `route add` příkazu z příkazového řádku systému Windows. Pro virtuální počítače s Linuxem protože výchozí chování hello používá slabé hostitele směrování, doporučujeme omezení provozu pro sekundární síťová rozhraní tooa jedné podsíti. Pokud požadujete připojení mimo hello podsítě pro sekundární síťová rozhraní, povolit na základě zásad směrování tooensure této příchozí a odchozí provoz používá hello stejné síťové rozhraní.
- Ve výchozím nastavení všechny odchozí přenosy z hello hello IP adresu odeslání virtuálních počítačů přiřazen toohello primární konfiguraci IP hello primární síťové rozhraní. Můžete řídit, která IP adresa se používá pro odchozí provoz v rámci operačního systému hello Virtuálního počítače, ale ve výchozím nastavení, je prostřednictvím hello primární síťové rozhraní.
- V hello po všech virtuálních počítačů v rámci hello byly stejné skupině dostupnosti vyžaduje toohave, které jeden nebo víc síťových rozhraní. Hello virtuálních počítačů s libovolný počet síťová rozhraní můžete nyní existují ve stejné skupině dostupnosti, telefonní číslo toohello nepodporuje hello velikost virtuálního počítače. Můžete přidat pouze s dostupností tooan počítač nastavit, pokud je vytvořen, když. Další informace o nastavení dostupnosti, přečtěte si hello toolearn [spravovat hello dostupnost virtuálních počítačů v Azure](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) článku.
- Při síťová rozhraní v hello stejného virtuálního počítače může být připojené toodifferent podsítí v rámci virtuální sítě, všechny hello síťová rozhraní musí být připojené toohello stejnou virtuální síť.
- Přidáním jakékoli IP adresy pro všechny IP konfiguraci jakékoli primární nebo sekundární síťové rozhraní tooan nástroj pro vyrovnávání zatížení Azure back-end fondu. V posledních hello nebylo možné pouze hello primární IP adresu hello primární síťové rozhraní přidat tooa fond back-end. Další informace o IP adresy a konfigurace, přečtěte si hello toolearn [přidat, změnit nebo odebrat IP adres](virtual-network-network-interface-addresses.md) článku.
- Odstranění virtuálního počítače hello síťových rozhraní, které jsou připojené tooit neodstraní. Při odstranění virtuálního počítače hello síťových rozhraní se odpojit od hello virtuálních počítačů. Můžete přidat hello síťové rozhraní toodifferent virtuální počítače, nebo je odstranit.
- Pokud síťové rozhraní má privátní tooit přiřazenu adresu IPv6, můžete ji připojit tooa virtuálních počítačů při vytváření hello virtuálních počítačů. Síťové rozhraní s přiřazenou tooa adresu IPv6 virtuálních počítačů nelze připojit, po vytvoření hello virtuálních počítačů. Pokud je při vytváření virtuálního počítače připojit síťové rozhraní s přiřazenou privátní adresu IPv6, je možné ještě připojit této síťové rozhraní toohello virtuálního počítače, bez ohledu na to, kolik síťových rozhraní, které podporuje hello velikost virtuálního počítače. V tématu [sítě IP adresy rozhraní](virtual-network-network-interface-addresses.md) toolearn Další informace o přiřazování IP adres toonetwork rozhraní.

## <a name="vm-create"></a>Přidat existující tooa rozhraní sítě nového virtuálního počítače

Při vytváření virtuálního počítače přes portál hello hello portál vytvoří rozhraní sítě s výchozím nastavením a připojí jej pro vás toohello virtuálních počítačů. Nelze přidat stávající síťové rozhraní tooa nového virtuálního počítače, nebo vytvořte virtuální počítač s více síťovými rozhraními pomocí hello portálu Azure. Můžete provést i pomocí hello rozhraní CLI nebo Powershellu. Můžete přidat jako mnoho sítě tooa rozhraní virtuálního počítače jako hello velikost virtuálního počítače, kterou vytváříte, podporuje. toolearn Další informace o tom, kolik síťových rozhraní každý podporuje velikost virtuálního počítače, přečíst hello [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálních počítačů. Síťová rozhraní Hello přidáte tooa virtuálního počítače nemůže být aktuálně připojené tooanother virtuálních počítačů. Další informace o vytvoření síťová rozhraní, přečtěte si hello toolearn [Spravovat síťová rozhraní](virtual-network-network-interface.md#create-a-network-interface) článku.

> [!WARNING]
> Pokud síťové rozhraní má přiřazenu tooit privátní adresu IPv6, můžete hello síťové rozhraní toohello virtuálního počítače přidat pouze při vytváření hello virtuálního počítače. Více než jedno síťové rozhraní toohello virtuálního počítače nelze připojit při vytváření hello virtuálního počítače nebo virtuální počítač je vytvořen po hello, tak dlouho, dokud adresu IPv6 je přiřazen tooa síťových rozhraní připojených tooa virtuálního počítače. V tématu [sítě IP adresy rozhraní](virtual-network-network-interface-addresses.md) toolearn Další informace o přiřazování IP adres toonetwork rozhraní.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Vytvoření virtuálního počítače az](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nový AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Přidat existující tooan rozhraní sítě existující virtuální počítač

Můžete přidat jako mnoho sítě tooa rozhraní virtuálního počítače jako hello velikost virtuálního počítače, které přidáváte toosupports rozhraní sítě. toolearn kolik síťových rozhraní, které podporuje každý velikost virtuálního počítače, přečtěte si hello [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálních počítačů. Hello chcete tooadd podporu hello počet síťových rozhraní má tooadd a musí být v hello zastavena toomust rozhraní sítě virtuálních počítačů (nepřiřazeném) stavu. Síťová rozhraní Hello chcete tooadd nemůže být aktuálně připojené tooanother virtuálních počítačů. Nelze přidat tooan rozhraní sítě existující virtuální počítač pomocí hello portálu Azure. tooadd síťová rozhraní tooan existující virtuální počítač, je nutné použít hello rozhraní příkazového řádku nebo prostředí PowerShell. 

> [!WARNING]
> Pokud síťové rozhraní má přiřazenu tooit privátní adresu IPv6, nelze ji přidat tooan existující virtuální počítač. Síťové rozhraní s se přiřazené privátní IPv6 adres tooa virtuální počítač můžete přidat pouze při vytváření virtuálního počítače. V tématu [sítě IP adresy rozhraní](virtual-network-network-interface-addresses.md) toolearn Další informace o přiřazování IP adres toonetwork rozhraní.

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Přidejte síťový adaptér virtuálního počítače az](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add) (referenční dokumentace) nebo [podrobné kroky](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (referenční dokumentace) nebo [podrobné kroky](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>Zobrazení síťových rozhraní pro virtuální počítač

Můžete zobrazit hello síťové rozhraní tooa aktuálně připojených virtuálních počítačů toolearn o konfiguraci každé síťové rozhraní a hello IP adres přiřazených tooeach síťové rozhraní. 

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazen hello rolí vlastník, Přispěvatel nebo Přispěvatel sítě pro vaše předplatné. najdete v části Další informace o přiřazení role tooaccounts toolearn [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *virtuální počítače*. Když **virtuální počítače** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **virtuální počítače** okno, které se zobrazí, klikněte na název hello hello chcete tooview síťová rozhraní pro virtuální počítač.
4. V hello **nastavení** části hello virtuálního počítače zobrazeném okně pro hello virtuálního počítače jste vybrali, klikněte na tlačítko **síťových rozhraní**. toolearn o nastavení síťového rozhraní a jak je, přečtěte si hello toochange [Spravovat síťová rozhraní](virtual-network-network-interface.md) článku. toolearn o přidání, změně nebo odebrání IP adresy přiřazené tooa síťového rozhraní, najdete v části [Správa IP adres](virtual-network-network-interface-addresses.md).

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Zobrazit az virtuálních počítačů](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>Odstranit síťové rozhraní z virtuálního počítače

Hello virtuálních počítačů má tooremove (nebo odpojit) síťové rozhraní z musí být v hello zastaveném stavu (deallocated) a musí v současnosti mít alespoň dvě síťová rozhraní připojená tooit. Můžete odebrat všechny síťové rozhraní, ale hello virtuálního počítače musí mít vždy alespoň jeden tooit rozhraní připojené sítě. Pokud odeberete primární síťové rozhraní, Azure přiřadí hello primární atribut toohello síťové rozhraní, které hello virtuálních počítačů připojených toohello nejdelší bylo. 

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazen hello rolí vlastník, Přispěvatel nebo Přispěvatel sítě pro vaše předplatné. najdete v části Další informace o přiřazení role tooaccounts toolearn [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *virtuální počítače*. Když **virtuální počítače** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **virtuální počítače** okno, které se zobrazí, klikněte na název hello hello chcete tooremove rozhraní sítě pro virtuální počítač.
4. V hello **nastavení** části hello virtuálního počítače zobrazeném okně pro hello virtuálního počítače jste vybrali, klikněte na tlačítko **síťových rozhraní**. toolearn o nastavení síťového rozhraní a jak je, přečtěte si hello toochange [Spravovat síťová rozhraní](virtual-network-network-interface.md) článku. toolearn o přidání, změně nebo odebrání IP adresy přiřazené tooa síťového rozhraní, najdete v části [Správa IP adres](virtual-network-network-interface-addresses.md).
5. V okně rozhraní sítě hello který se zobrazí, klikněte na tlačítko hello **...**  toohello napravo od hello síťové rozhraní, které chcete toodetach.
6. Klikněte na tlačítko **odpojit**. Pokud existuje jenom jeden síťových rozhraní připojených toohello virtuální počítač, hello **odpojení** možnost není dostupná. Klikněte na tlačítko **Ano** v zobrazeném poli hello potvrzení.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[síťový adaptér virtuálního počítače az odebrat](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#remove) (referenční dokumentace) nebo [podrobné kroky](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Odebrat AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (referenční dokumentace) nebo [podrobné kroky](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>Další kroky
toocreate virtuálního počítače s více síťovými rozhraními nebo IP adresy, přečtěte si hello následující články:

**Příkazy**

|Úkol|Nástroj|
|---|---|
|Vytvoření virtuálního počítače s několika síťovými kartami|[Rozhraní příkazového řádku](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [prostředí PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Vytvoření jednoho virtuálního počítače síťový adaptér s více adresami IPv4|[Rozhraní příkazového řádku](virtual-network-multiple-ip-addresses-cli.md), [prostředí PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Vytvoření jednoho virtuálního počítače síťový adaptér s privátní adresou IPv6 (za pro vyrovnávání zatížení Azure)|[Rozhraní příkazového řádku](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [prostředí PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [šablony Azure Resource Manageru](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
