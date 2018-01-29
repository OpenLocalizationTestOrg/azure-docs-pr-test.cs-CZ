---
title: "Konfigurace IP adres pro rozhraní sítě Azure | Microsoft Docs"
description: "Naučte se přidávat, měnit a odebírat privátní a veřejné IP adresy pro síťové rozhraní."
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
ms.openlocfilehash: 637b380dacc91e4ad55044c1d92936be2435138d
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="add-change-or-remove-ip-addresses-for-an-azure-network-interface"></a>Přidat, změnit nebo odebrat IP adresy pro rozhraní sítě Azure

Naučte se přidávat, měnit a odebírat veřejné a privátní IP adresy pro síťové rozhraní. Povolit privátní IP adresy přiřazené k síťovému rozhraní virtuálního počítače ke komunikaci s další prostředky ve virtuální sítě Azure a připojených sítí. Privátní IP adresy, taky umožňuje odchozí komunikaci k Internetu pomocí nepředvídatelným IP adresy. A [veřejnou IP adresu](virtual-network-public-ip-address.md) přiřazené na síťové rozhraní povolí příchozí komunikaci na virtuální počítač z Internetu. Adresu taky umožňuje odchozí komunikaci z virtuálního počítače k Internetu pomocí předvídatelný IP adresy. Podrobnosti najdete v tématu [pochopení odchozí připojení v Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Pokud třeba chcete vytvořit, změnit nebo odstranit síťové rozhraní, přečtěte si [spravovat síťové rozhraní](virtual-network-network-interface.md) článku. Pokud potřebujete síťová rozhraní pro přidání nebo odebrání síťová rozhraní z virtuálního počítače, přečtěte si [přidat nebo odebrat síťových rozhraní](virtual-network-network-interface-vm.md) článku. 


## <a name="before-you-begin"></a>Než začnete

Před dokončením všech kroků v žádné části tohoto článku dokončete následující úlohy:

- Zkontrolujte [Azure omezuje](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) článku se dozvíte o omezeních pro veřejné a privátní IP adresy.
- Přihlaste se k Azure [portál](https://portal.azure.com), rozhraní příkazového řádku Azure (CLI) nebo Azure PowerShell s účet Azure. Pokud nemáte účet Azure, si zaregistrovat [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud pomocí příkazů prostředí PowerShell k dokončení úloh v tomto článku [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi rutin prostředí Azure PowerShell nainstalovaný. Chcete-li získat nápovědu pro příkazy prostředí PowerShell s příklady, zadejte `get-help <command> -full`.
- Pokud používáte rozhraní příkazového řádku Azure (CLI) příkazy k dokončení úloh v tomto článku [instalace a konfigurace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zajistěte, že abyste měli nejnovější verzi rozhraní příkazového řádku Azure, který je nainstalovaný. Chcete-li získat nápovědu pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Místo instalace rozhraní příkazového řádku a jeho požadavky, můžete použít prostředí cloudové služby Azure. Služba Azure Cloud Shell je volně dostupné prostředí Bash, které můžete spustit přímo z portálu Azure Portal. Má předinstalované rozhraní Azure CLI, které je nakonfigurované pro použití s vaším účtem. Chcete-li použít cloudové prostředí, klikněte na tlačítko prostředí cloudu **> _** tlačítka v horní části [portál](https://portal.azure.com).

## <a name="add-ip-addresses"></a>Přidání IP adres

Můžete přidat jako mnoho [privátní](#private) a [veřejné](#public) [IPv4](#ipv4) adres podle potřeby k síťovému rozhraní, v rámci omezení uvedené v [Azure omezuje](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) článku. Přidat adresu IPv6 do existujícího síťového rozhraní (i když na portálu můžete přidat k síťovému rozhraní s privátní adresou IPv6, při vytváření rozhraní sítě) nelze použít na portálu. Můžete použít PowerShell nebo rozhraní příkazového řádku přidání s privátní adresou IPv6 do jedné [sekundární konfiguraci IP adresy](#secondary) (za předpokladu, nejsou žádné existující sekundární konfigurace IP) pro síťové rozhraní, který není připojen k virtuálnímu počítači. Jakýkoli nástroj nelze použít pro přidání veřejnou adresu IPv6 k síťovému rozhraní. V tématu [IPv6](#ipv6) podrobnosti o použití adresy IPv6. 

1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě pro vaše předplatné. Pro čtení [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku Další informace o přiřazování rolí a oprávnění k účtům.
2. Do pole, která obsahuje text *vyhledávání prostředků* v horní části portálu Azure, zadejte *síťových rozhraní*. Když **síťových rozhraní** se zobrazí ve výsledcích hledání klikněte na něj.
3. V **síťových rozhraní** okno, které se zobrazí, klikněte na síťové rozhraní, které chcete přidat adresu IPv4 pro.
4. Klikněte na tlačítko **konfigurace protokolu IP** v **nastavení** části okna pro síťové rozhraní, které jste vybrali.
5. Klikněte na tlačítko **+ přidat** v okně, které se otevře pro konfigurace protokolu IP.
6. Zadejte následující a pak klikněte na **OK** zavřete **přidat IP konfigurace** okno:

    |Nastavení|Povinné?|Podrobnosti|
    |---|---|---|
    |Název|Ano|Musí být jedinečný pro síťové rozhraní|
    |Typ|Ano|Vzhledem k tomu, že přidáváte konfiguraci IP adres do existujícího síťového rozhraní, a musí mít každé síťové rozhraní [primární](#primary) je jedinou možností konfigurace protokolu IP, **sekundární**.|
    |Způsob přiřazení privátní IP adresy|Ano|[**Dynamické**](#dynamic): Azure přiřadí další dostupnou adresu pro síťové rozhraní je nasazena v rozsah adres podsítě. [**Statické**](#static): přiřaďte nepoužívané adresy pro síťové rozhraní je nasazena v rozsah adres podsítě.|
    |Veřejná IP adresa|Ne|**Zakázáno:** žádný prostředek veřejné IP adresy je aktuálně přidružené ke konfiguraci IP adresy. **Povoleno:** vyberte existující veřejnou IP adresu IPv4 adresu, nebo vytvořte novou. Naučte se vytvořit veřejnou IP adresu, přečtěte si téma [veřejné IP adresy](virtual-network-public-ip-address.md#create-a-public-ip-address) článku.|
7. Ručně pomocí pokynů v přidat sekundární soukromé IP adresy do operačního systému virtuálního počítače [přiřadit více IP adres pro virtuální počítač operační systémy](virtual-network-multiple-ip-addresses-portal.md#os-config) článku. V tématu [privátní](#private) IP adresy pro zvláštní situace před ruční přidání IP adres do operačního systému virtuálního počítače. Všechny veřejné IP adresy, nepřidávejte do operačního systému virtuálního počítače.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Seskupování síťových az ip-config vytvořit](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Přidat AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/add-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-ip-address-settings"></a>Změňte nastavení IP adresy

Můžete třeba změnit metodu přiřazení adresy IPv4 změňte statickou IPv4 adresu, nebo změna veřejné IP adresy přiřazené k síťovému rozhraní. Pokud chcete změnit privátní IPv4 adresu sekundární konfiguraci IP adresy přidružené k sekundární síťové rozhraní ve virtuálním počítači (Další informace o [primární a sekundární síťová rozhraní](virtual-network-network-interface-vm.md)), umístěte virtuální počítač do stavu ukončeno (deallocated) před dokončením následujících kroků: 

1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě pro vaše předplatné. Pro čtení [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku Další informace o přiřazování rolí a oprávnění k účtům.
2. Do pole, která obsahuje text *vyhledávání prostředků* v horní části portálu Azure, zadejte *síťových rozhraní*. Když **síťových rozhraní** se zobrazí ve výsledcích hledání klikněte na něj.
3. V **síťových rozhraní** okno, které se zobrazí, klikněte na tlačítko chcete zobrazit nebo změnit nastavení IP adresy pro síťové rozhraní.
4. Klikněte na tlačítko **konfigurace protokolu IP** v **nastavení** části okna pro síťové rozhraní, které jste vybrali.
5. Klikněte na konfiguraci protokolu IP, kterou chcete upravit ze seznamu, ve které se otevře okno Konfigurace protokolu IP.
6. Změňte nastavení podle potřeby, pomocí informací o nastavení v kroku 6 [Přidat konfiguraci IP adres](#create-ip-config) tohoto článku. Klikněte na tlačítko **Uložit** zavřete okno pro konfiguraci protokolu IP můžete změnit.

>[!NOTE]
>Pokud primární síťové rozhraní má víc konfigurací IP adres a změníte privátní IP adresa primární konfiguraci IP adresy, je třeba přiřadit ručně IP adresy primární a sekundární rozhraní sítě v systému Windows (není potřeba pro Linux) . Chcete-li přiřadit ručně IP adresy síťového rozhraní v rámci operačního systému, přečtěte si [přiřadit více IP adres k virtuálním počítačům](virtual-network-multiple-ip-addresses-portal.md#os-config) článku. V tématu [privátní](#private) IP adresy pro zvláštní situace před ruční přidání IP adres do operačního systému virtuálního počítače. Všechny veřejné IP adresy, nepřidávejte do operačního systému virtuálního počítače.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[aktualizace ip-config seskupování az sítě](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRMNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="remove-ip-addresses"></a>Odebírat IP adresy

Můžete odebrat [privátní](#private) a [veřejné](#public) IP adres ze síťového rozhraní, ale síťové rozhraní musí být vždy alespoň jeden privátní IPv4 adresu přiřazen.

1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě pro vaše předplatné. Pro čtení [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku Další informace o přiřazování rolí a oprávnění k účtům.
2. Do pole, která obsahuje text *vyhledávání prostředků* v horní části portálu Azure, zadejte *síťových rozhraní*. Když **síťových rozhraní** se zobrazí ve výsledcích hledání klikněte na něj.
3. V **síťových rozhraní** okno, které se zobrazí, klikněte na rozhraní sítě, kterou chcete odebrat IP adresy z.
4. Klikněte na tlačítko **konfigurace protokolu IP** v **nastavení** části okna pro síťové rozhraní, které jste vybrali.
5. Klikněte pravým tlačítkem myši [sekundární](#secondary) konfigurace protokolu IP (nelze odstranit, [primární](#primary) konfigurace) chcete odstranit, klikněte na **odstranit**, pak klikněte na tlačítko **Ano** potvrďte odstranění. Konfigurace měli prostředek veřejné IP adresy přidružené k němu, prostředek je oddělen od konfiguraci protokolu IP, ale prostředek se neodstraní.
6. Zavřít **konfigurace protokolu IP** okno.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[AZ síť seskupování ip-config odstranit](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Odebrat AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/remove-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="ip-configurations"></a>Konfigurace protokolu IP

[Privátní](#private) a (volitelně) [veřejné](#public) IP adresy přiřazené k jedné nebo více konfigurací IP adres, které jsou přiřazené k síťovému rozhraní. Existují dva typy konfigurace protokolu IP:

### <a name="primary"></a>Primární

Každé síťové rozhraní je přiřazen jednu primární konfiguraci IP adresy. Primární konfigurace IP adresy:

- Má [privátní](#private) [IPv4](#ipv4) adresu přiřazenou k němu. Nelze přiřadit privátního [IPv6](#ipv6) adresu primární konfiguraci IP adresy.
- Mohou mít i [veřejné](#public) přiřazenu adresu IPv4. Nelze přiřadit veřejnou adresu IPv6 na primární nebo sekundární konfiguraci IP. Ale můžete přiřadit veřejnou adresu IPv6 pro vyrovnávání zatížení Azure, které můžete načíst vyrovnávání přenosů virtuálního počítače privátního IPv6 adresu. Další informace najdete v tématu [podrobnosti a omezení pro protokol IPv6](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations).

### <a name="secondary"></a>Sekundární

Kromě primární konfiguraci IP adresy rozhraní sítě může obsahovat nula nebo více sekundární IP konfigurace přiřazen. Sekundární konfiguraci IP adresy:

- Musí mít privátní adresa IPv4 nebo IPv6 přiřazená k němu. Pokud je adresa IPv6, rozhraní sítě může mít pouze jednu sekundární konfiguraci IP adresy. Pokud je adresa IPv4, rozhraní sítě může mít několik sekundární konfigurace IP přiřazen. Další informace o tom, kolik privátní a veřejné IPv4 adresy lze přiřadit k síťovému rozhraní, najdete v článku [Azure omezuje](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) článku.  
- Může také veřejnou adresu IPv4 přiřadili, pokud je privátní IP adresa IPv4. Pokud je privátní IP adresu IPv6, nelze přiřadit veřejnou adresu IPv4 nebo IPv6 ke konfiguraci IP adresy. Přiřazení více IP adres k síťovému rozhraní je užitečné v situacích, jako:
    - Hostovat několik webů nebo služeb s různými IP adresami a certifikáty SSL na jednom serveru.
    - Virtuální počítač, který slouží jako virtuální zařízení sítě, jako je například brány firewall nebo službu Vyrovnávání zatížení.
    - Možnost přidat kterýkoliv privátní adresy IPv4 v žádném z rozhraní sítě na fond back-end pro vyrovnávání zatížení Azure. V minulosti pouze primární adresu IPv4 pro primární síťové rozhraní nebylo možné přidat do fondu back-end. Další informace o tom, jak Vyrovnávání zatížení konfigurací s více IPv4, najdete v článku [víc konfigurací IP adres Vyrovnávání zatížení](../load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku. 
    - Umožňuje načíst vyrovnávat jedna adresa IPv6, které jsou přiřazené k síťovému rozhraní. Další informace o tom, jak pro privátní adresa IPv6 Vyrovnávání zatížení, najdete v článku [IPv6 adresy Vyrovnávání zatížení](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.


## <a name="address-types"></a>Typy adres

Následující typy IP adresy, které můžete přiřadit [konfigurace protokolu IP](#ip-configurations):

### <a name="private"></a>Privátní

Privátní [IPv4](#ipv4) adresy povolit virtuální počítač ke komunikaci s další prostředky ve virtuální síti nebo jiné připojených sítích. Virtuální počítač nelze oznamovat příchozí k ani může virtuální počítač komunikovat s privátního odchozí [IPv6](#ipv6) adresu s jednou výjimkou. Virtuální počítač může komunikovat s nástrojem pro vyrovnávání zatížení Azure pomocí adresy IPv6. Další informace najdete v tématu [podrobnosti a omezení pro protokol IPv6](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations). 

Ve výchozím nastavení, servery Azure DHCP přiřadit privátní adresa IPv4 pro [primární konfiguraci IP adresy](#primary) Azure síťového rozhraní na rozhraní sítě v rámci operačního systému virtuálního počítače. Pokud není nezbytné, měli byste nastavit nikdy ručně IP adresu síťového rozhraní v rámci operačního systému virtuálního počítače. 

> [!WARNING]
> Pokud adresu IPv4 nastavit jako primární IP adresa síťového rozhraní v rámci operačního systému virtuálního počítače je někdy liší od privátní adresa IPv4, které jsou přiřazené k primární konfiguraci IP primární síťové rozhraní připojen k virtuálnímu počítači v rámci Azure ztratíte připojení k virtuálnímu počítači.

Existují scénáře, kdy je nutné ručně nastavit IP adresu síťového rozhraní v rámci operačního systému virtuálního počítače. Při přidávání více IP adres pro virtuální počítač Azure například musí nastavit ručně primární a sekundární IP adresy operačního systému Windows. Pro virtuální počítač s Linuxem jenom musíte ručně nastavit sekundární IP adresy. V tématu [přidat IP adresy na operační systém virtuálního počítače](virtual-network-multiple-ip-addresses-portal.md#os-config) podrobnosti. Pokud byste někdy potřebovali změnit přiřazené ke konfiguraci IP adresu, se doporučuje, můžete:

1. Ujistěte se, že virtuální počítač je přijetí adresy ze serverů Azure DHCP. Když máte, můžete změnit přiřazení IP adresy zpět na serveru DHCP v operačním systému a restartování virtuálního počítače.
2. Zastavit (zrušit přidělení) virtuálního počítače.
3. Změna IP adresy pro konfiguraci protokolu IP v rámci Azure.
4. Umožňuje spustit virtuální počítač.
5. [Ruční konfigurace](virtual-network-multiple-ip-addresses-portal.md#os-config) sekundární IP adresy v rámci operačního systému (a také primární IP adresu v systému Windows) tak, aby odpovídaly nastavení v rámci Azure.
 
Podle předchozího postupu, přiřazené k síťovému rozhraní v rámci Azure a v rámci operačního systému virtuálního počítače, privátní IP adresy zůstávají stejné. Ke sledování virtuálních počítačů v rámci vašeho předplatného, které jste nastavili ručně IP adresy v rámci operačního systému pro, zvažte přidání Azure [značky](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags) k virtuálním počítačům. Můžete použít "přiřazení IP adresy: statické", např. Tímto způsobem, budete moci snadno najít virtuální počítače v rámci vašeho předplatného, které jste ručně nastavení IP adresy v operačním systému.

Kromě povolení komunikovat s jiným prostředkům v rámci stejné, nebo připojené virtuální sítě virtuálního počítače, privátní IP adresu taky umožňuje virtuálnímu počítači komunikovat odchozí k Internetu. Odchozí připojení se zdrojovou adresu sítě přeložit v Azure nepředvídatelným veřejné IP adresy. Další informace o Azure odchozí připojení k Internetu, přečtěte si [Azure odchozí připojení k Internetu](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku. Nemůžete komunikovat příchozí privátní IP adresu virtuálního počítače z Internetu. Pokud váš odchozí připojení vyžadují předvídatelnou veřejnou IP adresu, přidružte prostředek veřejné IP adresy pro síťové rozhraní.

### <a name="public"></a>Veřejné

Veřejné IP adresy přiřazené prostřednictvím prostředek veřejné IP adresy povolit příchozí připojení k virtuálnímu počítači z Internetu. Odchozí připojení k Internetu používat předvídatelný IP adresu. V tématu [pochopení odchozí připojení v Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) podrobnosti. Může přiřadit konfiguraci IP adres veřejnou IP adresu, ale nejsou potřeba. Pokud nepřiřadíte veřejnou IP adresu do virtuálního počítače tím, že přidružíte prostředek veřejné IP adresy, virtuálního počítače může komunikovat stále odchozí k Internetu. V takovém případě privátní IP adresa je adresa sítě zdroj přeložit v Azure nepředvídatelným veřejné IP adresy. Další informace o veřejné IP adresy prostředky, najdete v části [prostředek veřejné IP adresy](virtual-network-public-ip-address.md).

Existují omezení počtu privátní a veřejné IP adresy, které můžete přiřadit k síťovému rozhraní. Informace najdete [Azure omezuje](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) článku.

> [!NOTE]
> Azure znamená, že virtuální počítač privátní IP adresu na veřejnou IP adresu. V důsledku toho je operačního systému virtuálního počítače neví, všechny veřejné IP adresy přiřazené, a proto není nutné někdy ručně přiřadit veřejnou IP adresu v operačním systému.

## <a name="assignment-methods"></a>Přiřazení metody

Veřejné a privátní IP adresy se přiřazují pomocí jedné z následujících metod přiřazení:

### <a name="dynamic"></a>Dynamická

Dynamické privátní IPv4 a IPv6 (volitelně) adresy přiřazené ve výchozím nastavení. 

- **Pouze veřejné**: Azure přiřadí adresou z rozsahu jedinečný pro každé oblasti Azure. Další informace, které rozsahy jsou přiřazeny pro každou oblast, najdete v části [rozsahy IP Datacentra Azure Microsoft](https://www.microsoft.com/download/details.aspx?id=41653). Pokud je virtuální počítač zastaven (deallocated), pak spustit znovu, můžete změnit adresu. Pro konfiguraci IP adres pomocí metody přiřazení nelze přiřadit veřejnou adresu IPv6.
- **Privátní pouze**: Azure si vyhrazuje první čtyři adres v každé rozsah adres podsítě a není přiřadit adresy. Azure přiřadí prostředku další dostupnou adresu z rozsahu adres podsítě. Pokud je například rozsah adres podsítě 10.0.0.0/16 a adresy 10.0.0.0.4–10.0.0.14 už jsou přiřazené (.0–.3 jsou vyhrazené), Azure prostředku přiřadí adresu 10.0.0.15. Dynamická metoda přidělování je výchozí metoda. Jakmile jsou dynamické IP adresy přiřazené, uvolní se pouze v případě odstranění síťového rozhraní, jeho přiřazení k jiné podsíti ve stejné virtuální síti nebo změně metody přidělování na statickou a zadání jiné IP adresy. Když změníte metodu přidělování z dynamické na statickou, Azure ve výchozím nastavení jako statickou IP adresu přiřadí dříve dynamicky přiřazenou adresu. Lze přiřadit pouze s privátní adresou IPv6 pomocí metody dynamického přiřazení.

### <a name="static"></a>Statická

Veřejné nebo soukromé statickou IPv4 adresu (volitelně) můžete přiřadit konfiguraci IP adres. Nelze přiřadit statická adresa IPv6 veřejných nebo privátních pro konfiguraci IP adres. Další informace o tom, jak Azure přiřadí statické veřejné adresy IPv4, najdete v článku [veřejnou IP adresu](virtual-network-public-ip-address.md) článku.

- **Pouze veřejné**: Azure přiřadí adresou z rozsahu jedinečný pro každé oblasti Azure. Další informace, které rozsahy jsou přiřazeny pro každou oblast, najdete v části [rozsahy IP Datacentra Azure Microsoft](https://www.microsoft.com/download/details.aspx?id=41653). Adresu nemění příliš dlouho, dokud prostředek veřejné adresy IP, které je přiřazena k odstranění nebo metodu přiřazení se změní na dynamický. Pokud prostředek veřejné IP adresy je přidružena ke konfiguraci IP adres, musí být oddělit z konfigurace IP před změnou metoda jeho přiřazení.
- **Privátní pouze**: vyberte a přiřadit adresu z rozsahu adres podsíti. Adresa, kterou přiřadíte, může být jakákoli adresa v rozsahu adres podsítě kromě prvních čtyř adres, která aktuálně není přiřazená k žádnému jinému prostředku v této podsíti. Statické adresy se uvolní pouze v případě odstranění síťového rozhraní. Pokud změníte metodu přidělování na statickou, Azure jako dynamickou adresu dynamicky přiřadí dříve přiřazenou statickou IP adresu, a to i v případě, že tato adresa není další dostupnou adresou v rozsahu adres podsítě. Adresa se změní také v případě přiřazení síťového rozhraní k jiné podsíti ve stejné virtuální síti. Pokud však chcete síťové rozhraní přiřadit k jiné podsíti, musíte nejprve změnit metodu přidělování ze statické na dynamickou. Jakmile přiřadíte síťové rozhraní k jiné podsíti, můžete metodu přidělování změnit zpět na statickou a přiřadit IP adresu z rozsahu adres nové podsítě.

## <a name="ip-address-versions"></a>Verze IP adres

Při přiřazování adresy můžete zadat následující verze:

### <a name="ipv4"></a>IPv4

Každé síťové rozhraní musí mít jeden [primární](#primary) konfigurace protokolu IP s přiřazeným [privátní](#private) [IPv4](#ipv4) adresu. Můžete přidat jeden nebo víc [sekundární](#secondary) konfigurace protokolu IP nichž každá má privátní IPv4 a (volitelně) IPv4 [veřejné](#public) IP adresu.

### <a name="ipv6"></a>IPv6

Můžete přiřadit žádnou nebo jednu privátní [IPv6](#ipv6) adresu jednu sekundární konfiguraci IP adresy síťového rozhraní. Síťové rozhraní nemůže mít všechny existující sekundární konfigurace IP. Nelze přidat konfiguraci IP adres s IPv6 adresu pomocí portálu. Pomocí prostředí PowerShell nebo rozhraní příkazového řádku přidejte konfiguraci IP adres s privátní adresou IPv6 do existujícího síťového rozhraní. Síťové rozhraní nelze připojit k existující virtuální počítač.

> [!NOTE]
> Když vytvoříte síťového rozhraní IPv6 adresu pomocí portálu nelze přidat existující rozhraní sítě do nového nebo existujícího virtuálního počítače pomocí portálu. Pomocí prostředí PowerShell nebo 2.0 rozhraní příkazového řádku Azure k vytvoření síťového rozhraní s privátní adresou IPv6 a potom připojit síťové rozhraní, při vytváření virtuálního počítače. Nelze připojit síťové rozhraní s privátní adresou IPv6 přiřazená k existující virtuální počítač. Pro konfiguraci IP adres pro všechny síťové rozhraní, který je připojen k virtuálnímu počítači pomocí libovolné nástroje (portál, rozhraní příkazového řádku nebo prostředí PowerShell) nelze přidat s privátní adresou IPv6.

Nelze přiřadit veřejnou adresu IPv6 na primární nebo sekundární konfiguraci IP.

## <a name="skus"></a>Skladové položky

Veřejná IP adresa je vytvořen s SKU basic nebo standard.  Další informace o SKU rozdíly najdete v tématu [spravovat veřejné IP adresy](virtual-network-public-ip-address.md).

> [!NOTE]
> Při přiřazování veřejné IP adresy standardní SKU k síťovému rozhraní virtuálního počítače je potřeba explicitně povolit plánovaný provoz pomocí [skupiny zabezpečení sítě](security-overview.md#network-security-groups). Komunikace s prostředkem nebude možná, dokud nevytvoříte a nepřiřadíte skupinu zabezpečení sítě a explicitně nepovolíte požadovaný provoz.

## <a name="next-steps"></a>Další postup
Pokud chcete vytvořit virtuální počítač s různými konfiguracemi protokolu IP, přečtěte si v následujících článcích:

|Úkol|Nástroj|
|---|---|
|Vytvoření virtuálního počítače s několika síťovými kartami|[Rozhraní příkazového řádku](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [prostředí PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Vytvoření jednoho virtuálního počítače síťový adaptér s více adresami IPv4|[Rozhraní příkazového řádku](virtual-network-multiple-ip-addresses-cli.md), [prostředí PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Vytvoření jednoho virtuálního počítače síťový adaptér s privátní adresou IPv6 (za pro vyrovnávání zatížení Azure)|[Rozhraní příkazového řádku](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [prostředí PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [šablony Azure Resource Manageru](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
