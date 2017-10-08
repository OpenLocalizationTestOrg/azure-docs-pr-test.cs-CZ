---
title: "aaaConfigure IP adresy pro rozhraní sítě Azure | Microsoft Docs"
description: "Zjistěte, jak změnit tooadd a odebírat privátní a veřejné IP adresy pro síťové rozhraní."
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
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: 1e5ea6c65d93be9b1fda5d807500a0823c94c89c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-change-or-remove-ip-addresses-for-an-azure-network-interface"></a>Přidat, změnit nebo odebrat IP adresy pro rozhraní sítě Azure

Zjistěte, jak změnit tooadd a odebírat veřejné a privátní IP adresy pro síťové rozhraní. Privátní IP adresy přiřazené tooa síťové rozhraní povolit toocommunicate virtuální počítač s další prostředky ve virtuální sítě Azure a připojených sítí. Privátní IP adresy, taky umožňuje odchozí komunikaci toohello Internetu pomocí nepředvídatelným IP adresu. A [veřejnou IP adresu](virtual-network-public-ip-address.md) přiřazené tooa síťové rozhraní povolí příchozí komunikaci tooa virtuální počítač z hello Internetu. Hello adres taky umožňuje odchozí komunikaci z virtuálního počítače toohello hello Internetu pomocí předvídatelný IP adresu. Podrobnosti najdete v tématu [pochopení odchozí připojení v Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Pokud potřebujete toocreate, měnit a odstraňovat síťového rozhraní, přečtěte si hello [spravovat síťové rozhraní](virtual-network-network-interface.md) článku. Potřebujete-li tooadd síťové rozhraní tooor odebrat síťová rozhraní z virtuálního počítače, přečtěte si hello [přidat nebo odebrat síťových rozhraní](virtual-network-network-interface-vm.md) článku. 


## <a name="before-you-begin"></a>Než začnete

Dokončení hello následující úkoly před dokončením všechny kroky v žádné části tohoto článku:

- Zkontrolujte hello [Azure omezuje](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) toolearn článek o omezeních pro veřejné a privátní IP adresy.
- Přihlaste se toohello Azure [portál](https://portal.azure.com), rozhraní příkazového řádku Azure (CLI) nebo Azure PowerShell s účet Azure. Pokud nemáte účet Azure, si zaregistrovat [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud pomocí prostředí PowerShell příkazy toocomplete úlohy v tomto článku [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Zajistěte, že abyste měli nejnovější verzi rutin prostředí Azure PowerShell hello nainstalován hello. Zadejte tooget nápovědy pro příkazy prostředí PowerShell s příklady, `get-help <command> -full`.
- Pokud pomocí rozhraní příkazového řádku Azure (CLI) příkazy toocomplete úlohy v tomto článku [instalace a konfigurace rozhraní příkazového řádku Azure hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi hello hello nainstalované rozhraní příkazového řádku Azure. tooget nápovědy pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Namísto instalaci hello rozhraní příkazového řádku a jeho požadavky můžete použít hello prostředí cloudu Azure. Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure. Má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem. toouse hello cloudové prostředí, klikněte na tlačítko hello cloudové prostředí **> _** tlačítko hello horní části hello [portál](https://portal.azure.com).

## <a name="add-ip-addresses"></a>Přidání IP adres

Můžete přidat jako mnoho [privátní](#private) a [veřejné](#public) [IPv4](#ipv4) adresy jako nezbytné tooa síťového rozhraní, v rámci omezení hello uvedené v hello [Azure omezení ](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) článku. Nelze použít hello portálu tooadd IPv6 adres tooan existující rozhraní sítě (i když při vytváření hello síťové rozhraní, můžete použít hello portálu tooadd privátní tooa síťové rozhraní adresy IPv6). Můžete použít PowerShell nebo rozhraní příkazového řádku tooadd privátní IPv6 adres tooone hello [sekundární konfiguraci IP adresy](#secondary) (za předpokladu, nejsou žádné existující sekundární konfigurace IP) pro existující síť rozhraní, které není připojené tooa virtuální počítač. Nelze použít žádné nástroj tooadd veřejné tooa síťové rozhraní adresu IPv6. V tématu [IPv6](#ipv6) podrobnosti o použití adresy IPv6. 

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě hello pro vaše předplatné. Čtení hello [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku toolearn informace o přiřazení role a oprávnění tooaccounts.
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *síťových rozhraní*. Když **síťových rozhraní** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **síťových rozhraní** okno, které se zobrazí, klikněte na tlačítko hello síťové rozhraní chcete tooadd IPv4 adresu.
4. Klikněte na tlačítko **konfigurace protokolu IP** v hello **nastavení** části hello okna pro síťové rozhraní hello jste vybrali.
5. Klikněte na tlačítko **+ přidat** v okně hello, které se otevře pro konfigurace protokolu IP.
6. Zadejte následující hello a pak klikněte na **OK** tooclose hello **přidat IP konfigurace** okno:

    |Nastavení|Povinné?|Podrobnosti|
    |---|---|---|
    |Name (Název)|Ano|Musí být jedinečný pro síťové rozhraní hello|
    |Typ|Ano|Vzhledem k tomu, že chcete přidat IP konfigurace tooan existující rozhraní sítě, a musí mít každé síťové rozhraní [primární](#primary) je jedinou možností konfigurace protokolu IP, **sekundární**.|
    |Způsob přiřazení privátní IP adresy|Ano|[**Dynamické** ](#dynamic) adresy se mohou změnit, pokud restartování hello virtuálního počítače, po které byly v hello zastavení (deallocated) stavu. Azure přiřadí dostupnou adresu z adresního prostoru hello hello podsíť hello síťového rozhraní je připojeno k. [**Statické** ](#static) adresy nejsou vydané až do odstranění hello síťové rozhraní. Zadejte IP adresu z hello podsíť adres místo rozsah, který aktuálně není používán jinou konfiguraci protokolu IP.|
    |Veřejná IP adresa|Ne|**Zakázáno:** žádný prostředek veřejné IP adresy je aktuálně přidružené toohello konfigurace protokolu IP. **Povoleno:** vyberte existující veřejnou IP adresu IPv4 adresu, nebo vytvořte novou. jak toocreate veřejnou IP adresu, číst toolearn hello [veřejné IP adresy](virtual-network-public-ip-address.md#create-a-public-ip-address) článku.|
7. Ručně přidat sekundární privátní IP adresy toohello operačního systému virtuálního počítače provedením hello pokyny v hello [přiřadit více IP adres toovirtual počítač operační systémy](virtual-network-multiple-ip-addresses-portal.md#os-config) článku. V tématu [privátní](#private) IP adresy pro zvláštní situace před ručně přidáním operačního systému tooa virtuálního počítače IP adresy. Nepřidávejte všechny veřejné IP adresy toohello operačního systému virtuálního počítače.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Seskupování síťových az ip-config vytvořit](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Přidat AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/add-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-ip-address-settings"></a>Změňte nastavení IP adresy

Může být nutné toochange hello přiřazení metoda adresy IPv4, změna hello statická adresa IPv4, nebo změnu hello veřejnou IP adresu přiřadit tooa síťové rozhraní. Pokud chcete změnit hello privátní IPv4 adresu sekundární konfiguraci IP adresy přidružené k sekundární síťové rozhraní ve virtuálním počítači (Další informace o [primární a sekundární síťová rozhraní](virtual-network-network-interface-vm.md#about)), virtuální místní hello (deallocated) stavu zastavení počítače do hello před dokončením hello následující kroky: 

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě hello pro vaše předplatné. Čtení hello [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku toolearn informace o přiřazení role a oprávnění tooaccounts.
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *síťových rozhraní*. Když **síťových rozhraní** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **síťových rozhraní** hello síťové rozhraní mají být tooview nebo změnit nastavení IP adresy pro okno, které se zobrazí, klikněte na tlačítko.
4. Klikněte na tlačítko **konfigurace protokolu IP** v hello **nastavení** části hello okna pro síťové rozhraní hello jste vybrali.
5. Klikněte na možnost konfigurace protokolu IP hello chcete toomodify hello seznamu hello okno, které se otevře pro konfigurace protokolu IP.
6. Dobrý den, podle potřeby změňte nastavení, pomocí hello informace o nastavení hello v kroku 6 hello [Přidat konfiguraci IP adres](#create-ip-config) tohoto článku. Klikněte na tlačítko **Uložit** tooclose hello okna pro konfiguraci protokolu IP hello jste změnili.

>[!NOTE]
>Pokud hello primární síťové rozhraní má víc konfigurací IP adres a změňte hello privátní IP adresu hello primární konfiguraci IP adresy, je třeba přiřadit ručně hello primární a sekundární IP adresy toohello rozhraní sítě v systému Windows (ne vyžaduje se pro Linux). toomanually přiřadit IP adresy tooa síťového rozhraní v rámci operačního systému, přečtěte si hello [přiřadit více IP adres počítačů toovirtual](virtual-network-multiple-ip-addresses-portal.md#os-config) článku. V tématu [privátní](#private) IP adresy pro zvláštní situace před ručně přidáním operačního systému tooa virtuálního počítače IP adresy. Nepřidávejte všechny veřejné IP adresy toohello operačního systému virtuálního počítače.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[aktualizace ip-config seskupování az sítě](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRMNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="remove-ip-addresses"></a>Odebírat IP adresy

Můžete odebrat [privátní](#private) a [veřejné](#public) IP adres ze síťového rozhraní, ale síťové rozhraní musí být vždy alespoň jeden privátní IPv4 adresu přiřazenou tooit.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě hello pro vaše předplatné. Čtení hello [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku toolearn informace o přiřazení role a oprávnění tooaccounts.
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *síťových rozhraní*. Když **síťových rozhraní** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **síťových rozhraní** okno, které se zobrazí, klikněte na tlačítko hello síťové rozhraní chcete tooremove z IP adres.
4. Klikněte na tlačítko **konfigurace protokolu IP** v hello **nastavení** části hello okna pro síťové rozhraní hello jste vybrali.
5. Klikněte pravým tlačítkem myši [sekundární](#secondary) konfigurace protokolu IP (nelze odstranit hello [primární](#primary) konfigurace) má toodelete, klikněte na tlačítko **odstranit**, pak klikněte na tlačítko **Ano**  tooconfirm hello odstranění. Pokud konfigurace hello obsahovala prostředek veřejné IP adresy přidružené tooit, hello prostředků je oddělen od hello konfiguraci IP adresy, ale neodstraní hello prostředků.
6. Zavřít hello **konfigurace protokolu IP** okno.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[AZ síť seskupování ip-config odstranit](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Odebrat AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/remove-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="ip-configurations"></a>Konfigurace protokolu IP

[Privátní](#private) a (volitelně) [veřejné](#public) tooone budou přiřazovat IP adresy nebo víc konfigurací IP adres přiřazené tooa síťové rozhraní. Existují dva typy konfigurace protokolu IP:

### <a name="primary"></a>Primární

Každé síťové rozhraní je přiřazen jednu primární konfiguraci IP adresy. Primární konfigurace IP adresy:

- Má [privátní](#private) [IPv4](#ipv4) tooit adresu přiřazenou. Nelze přiřadit privátního [IPv6](#ipv6) primární konfiguraci IP adresy tooa.
- Mohou mít i [veřejné](#public) tooit přiřazená adresa IPv4. Nelze přiřadit na veřejné IPv6 adres tooa primární nebo sekundární konfiguraci protokolu IP. Můžete ale, přiřazení veřejné IPv6 adres pro vyrovnávání zatížení Azure tooan, které můžete načíst vyvážit privátní adresa IPv6 provoz tooa virtuální počítače. Další informace najdete v tématu [podrobnosti a omezení pro protokol IPv6](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations).

### <a name="secondary"></a>Sekundární

Kromě toho tooa primární konfiguraci IP adresy, rozhraní sítě může obsahovat nula nebo více sekundární IP konfigurace přiřazené tooit. Sekundární konfiguraci IP adresy:

- Musí mít privátní tooit přiřazená adresa IPv4 nebo IPv6. Pokud hello adresu IPv6, hello síťové rozhraní může mít pouze jednu sekundární konfiguraci IP adresy. Pokud hello adresa IPv4, hello síťové rozhraní může mít víc sekundární konfigurací IP adres přiřazené tooit. toolearn Další informace o tom, kolik privátní a veřejné IPv4 adresy lze přiřadit tooa síťového rozhraní, najdete v části hello [Azure omezuje](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) článku.  
- Může také mít veřejnou IPv4 adresu přiřazenou tooit, pokud hello privátní IP adresa IPv4. Pokud hello privátní IP adresu IPv6, nelze přiřadit na veřejnou IPv4 nebo IPv6 adresa toohello konfiguraci protokolu IP. Přiřazení více IP adres tooa síťové rozhraní je užitečné v situacích, jako:
    - Hostovat několik webů nebo služeb s různými IP adresami a certifikáty SSL na jednom serveru.
    - Virtuální počítač, který slouží jako virtuální zařízení sítě, jako je například brány firewall nebo službu Vyrovnávání zatížení.
    - Hello možnost tooadd žádné hello privátní IPv4 adresy pro některý z fondu hello síťových rozhraní tooan nástroj pro vyrovnávání zatížení Azure back-end. V posledních hello nebylo možné pouze hello primární adresu IPv4 pro hello primární síťové rozhraní přidat tooa fond back-end. toolearn Další informace o tom, jak tooload vyrovnávat víc konfigurací protokolu IPv4, najdete v části hello [víc konfigurací IP adres služby Vyrovnávání zatížení](../load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku. 
    - možnost tooload Hello vyrovnávat jedno síťové rozhraní přiřazené tooa IPv6 adresu. toolearn Další informace o tom, jak tooload vyvážit tooa privátní adresu IPv6, najdete v části hello [IPv6 adresy Vyrovnávání zatížení](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.


## <a name="address-types"></a>Typy adres

Můžete přiřadit hello následující typy IP adresy tooan [konfigurace protokolu IP](#ip-configurations):

### <a name="private"></a>Privátní

Privátní [IPv4](#ipv4) adresy povolit toocommunicate virtuální počítač s další prostředky ve virtuální síti nebo jiné připojených sítích. Virtuální počítač nelze oznamovat příchozí k ani může hello virtuální počítač komunikovat s privátního odchozí [IPv6](#ipv6) adresu s jednou výjimkou. Virtuální počítač může komunikovat s nástroje pro vyrovnávání zatížení Azure hello pomocí adresy IPv6. Další informace najdete v tématu [podrobnosti a omezení pro protokol IPv6](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations). 

Ve výchozím nastavení, servery Azure DHCP hello přiřadit hello privátní IPv4 adresu pro hello [primární konfiguraci IP adresy](#primary) hello síťové rozhraní toohello síťového rozhraní v rámci operačního systému virtuálního počítače hello. Pokud není nezbytné, měli byste nastavit nikdy ručně hello IP adresa síťového rozhraní v rámci operačního systému hello virtuálního počítače. 

> [!WARNING]
> Pokud sada hello IPv4 adres jako hello primární IP adresu síťového rozhraní v rámci operačního systému virtuálního počítače je někdy jiné než hello privátní IPv4 adresu přiřazenou toohello primární konfiguraci IP hello primární síťové rozhraní připojené tooa virtuální počítač v rámci Azure, ztratíte připojení k toohello virtuálního počítače.

Existují scénáře, kdy je nutné toomanually sadu hello IP adresu síťového rozhraní v rámci operačního systému hello virtuálního počítače. Při přidávání více IP adres tooan virtuální počítač Azure například musíte nastavit ručně hello primární a sekundární IP adresy operačního systému Windows. Pro virtuální počítač s Linuxem musíte pouze toomanually sadu hello sekundární IP adresy. V tématu [přidání IP adres pro operační systém virtuálního počítače tooa](virtual-network-multiple-ip-addresses-portal.md#os-config) podrobnosti. Když nastavíte ručně hello IP adres v rámci hello operačního systému, doporučujeme vždy přiřadit konfigurace protokolu IP toohello hello adresy pro síťové rozhraní, pomocí metody hello přiřazení statických (nikoli dynamické). Přiřazení adresy hello pomocí hello statickou metodu zajistí, že hello adres nezmění v rámci Azure. Pokud byste někdy potřebovali toochange hello adresu přiřazenou tooan konfigurace protokolu IP, se doporučuje, můžete:

1. tooensure hello virtuálního počítače je přijetí adresy ze serverů Azure DHCP hello, změňte hello přiřazení back tooDHCP hello IP adres v rámci hello operačního systému a restartování hello virtuálního počítače.
2. Zastavit (zrušit přidělení) hello virtuálního počítače.
3. Změna hello IP adresy pro konfiguraci protokolu IP hello v rámci Azure.
4. Spusťte virtuální počítač hello.
5. [Ruční konfigurace](virtual-network-multiple-ip-addresses-portal.md#os-config) hello sekundární toomatch IP adresy v rámci hello operačního systému (a také hello primární IP adresu v systému Windows), nastavte v rámci Azure.
 
Podle předchozích kroků hello hello privátní IP adresu přiřazenou toohello síťové rozhraní v rámci Azure a v rámci operačního systému virtuálního počítače, zůstávají stejné hello. které virtuální počítače v rámci vašeho předplatného, které jste nastavili ručně IP adresy v rámci operačního systému, zvažte přidání Azure sledovat tookeep [značka](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags) toohello virtuálních počítačů. Můžete použít "přiřazení IP adresy: statické", např. Tímto způsobem, budete moci snadno najít hello virtuálních počítačů v rámci vašeho předplatného, které jste nastavili ručně hello IP adresu pro hello operačního systému.

Kromě tooenabling toocommunicate virtuální počítač s jiným prostředkům v rámci hello stejné, nebo připojené virtuální sítě, privátní IP adres taky umožňuje odchozí toohello virtuální počítač toocommunicate Internetu. Odchozí připojení jsou zdroj síťová adresa přeložen Azure tooan nepředvídatelným veřejnou IP adresu. Další informace o Azure odchozí připojení k Internetu, přečtěte si hello toolearn [Azure odchozí připojení k Internetu](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku. Nelze komunikovat příchozí tooa virtuálního počítače privátní IP adresu z hello Internetu.

### <a name="public"></a>Veřejné

Veřejné IP adresy povolit příchozí připojení tooa virtuální počítač z hello Internetu. Odchozí připojení toohello Internetu používat předvídatelný IP adresu. V tématu [pochopení odchozí připojení v Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) podrobnosti. Může přiřadit na veřejnou IP adresu tooan konfiguraci protokolu IP, ale nejsou potřeba. Pokud nepřiřadíte veřejnou IP adresu tooa virtuálního počítače, může stále komunikovat odchozí toohello Internetu pomocí privátní IP adresu. Další informace o veřejné IP adresy, přečtěte si hello toolearn [veřejnou IP adresu](virtual-network-public-ip-address.md) článku.

Existují omezení počtu toohello privátní a veřejné IP adresy, které můžete přiřazovat tooa síťové rozhraní. Podrobnosti najdete v tématu hello [Azure omezuje](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) článku.

> [!NOTE]
> Azure znamená, že virtuální počítač privátní IP adresu tooa veřejnou IP adresu. V důsledku toho ne všechny veřejné IP adresy přiřazené tooit hello operačního systému, takže není žádná tooever nutné ručně přiřadit veřejnou IP adresu v rámci hello operačního systému.

## <a name="assignment-methods"></a>Přiřazení metody

Veřejné a privátní IP adresy se přiřazují pomocí hello následující metody přiřazení:

### <a name="dynamic"></a>Dynamická

Dynamické privátní IPv4 a IPv6 (volitelně) adresy přiřazené ve výchozím nastavení. Dynamické adresy se mohou změnit, pokud hello virtuální počítač uveden do hello (deallocated) stavu Zastaveno a začít. Pokud nechcete, aby toochange adresy IPv4 dobu životnosti hello hello virtuálního počítače, přiřadíte adresy hello pomocí statickou metodu hello. Lze přiřadit pouze s privátní adresou IPv6 pomocí metody dynamického přidělování hello. Nelze přiřadit na veřejné IPv6 adres tooan konfiguraci protokolu IP pomocí obou těchto metod.

### <a name="static"></a>Statická

Adresy přiřazené statické metodou hello neměňte až do odstranění virtuálního počítače. Ručně zařadíte privátních IPv4 adres tooan konfiguraci statické IP adresy z hello adresního prostoru pro hello podsíť hello síťové rozhraní. (Volitelně) můžete přiřadit veřejných nebo privátních IPv4 adres tooan konfiguraci statické IP adresy. Nelze přiřadit veřejných nebo privátních IPv6 adres tooan konfiguraci statické IP adresy. toolearn Další informace o tom, jak Azure přiřadí statické veřejné adresy IPv4, najdete v části hello [veřejnou IP adresu](virtual-network-public-ip-address.md) článku.

## <a name="ip-address-versions"></a>Verze IP adres

Můžete určit hello při přiřazování adresy následující verze:

### <a name="ipv4"></a>IPv4

Každé síťové rozhraní musí mít jeden [primární](#primary) konfigurace protokolu IP s přiřazeným [privátní](#private) [IPv4](#ipv4) adresu. Můžete přidat jeden nebo víc [sekundární](#secondary) konfigurace protokolu IP nichž každá má privátní IPv4 a (volitelně) IPv4 [veřejné](#public) IP adresu.

### <a name="ipv6"></a>IPv6

Můžete přiřadit žádnou nebo jednu privátní [IPv6](#ipv6) adresu tooone sekundární konfiguraci IP adresy síťového rozhraní. Hello síťového rozhraní nemůže mít všechny existující sekundární konfigurace IP. Nelze přidat konfiguraci IP adres s IPv6 adresu pomocí portálu hello. Pomocí prostředí PowerShell nebo hello rozhraní příkazového řádku tooadd konfiguraci IP adres s privátní IPv6 adres tooan existující rozhraní sítě. Hello síťového rozhraní nemůže být připojené tooan existující virtuální počítač.

> [!NOTE]
> Když vytvoříte síťového rozhraní IPv6 adresu pomocí portálu hello nelze přidat existující síťové rozhraní tooa nový nebo existující virtuální počítač, pomocí portálu hello. Pomocí prostředí PowerShell nebo hello Azure CLI 2.0 toocreate síťové rozhraní s privátní adresou IPv6 a potom připojit hello síťové rozhraní, při vytváření virtuálního počítače. Nelze připojit síťové rozhraní s privátní adresou IPv6 přiřazená tooit tooan existující virtuální počítač. Nelze přidat na privátní IPv6 adres tooan konfiguraci protokolu IP pro všechny síťové rozhraní, které jsou připojené tooa virtuálního počítače pomocí žádné nástroje (portálu, rozhraní příkazového řádku nebo prostředí PowerShell).

Nelze přiřadit na veřejné IPv6 adres tooa primární nebo sekundární konfiguraci protokolu IP.

## <a name="next-steps"></a>Další kroky
toocreate virtuálního počítače s různými konfiguracemi protokolu IP, přečtěte si hello následující články:

|Úkol|Nástroj|
|---|---|
|Vytvoření virtuálního počítače s několika síťovými kartami|[Rozhraní příkazového řádku](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [prostředí PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Vytvoření jednoho virtuálního počítače síťový adaptér s více adresami IPv4|[Rozhraní příkazového řádku](virtual-network-multiple-ip-addresses-cli.md), [prostředí PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Vytvoření jednoho virtuálního počítače síťový adaptér s privátní adresou IPv6 (za pro vyrovnávání zatížení Azure)|[Rozhraní příkazového řádku](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [prostředí PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [šablony Azure Resource Manageru](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
