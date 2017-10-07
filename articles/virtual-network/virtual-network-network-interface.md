---
title: "aaaCreate, měnit a odstraňovat rozhraní sítě Azure | Microsoft Docs"
description: "Zjistěte, co je síťové rozhraní a jak změnit nastavení pro toocreate a odstraňte jednu."
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
ms.openlocfilehash: dcb6cdbd73bc0d0ca4efb9d972d370cf2d54abb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-change-or-delete-a-network-interface"></a>Vytvoření, změnit nebo odstranit síťové rozhraní

Zjistěte, jak změnit nastavení pro toocreate a odstranit síťové rozhraní. Síťové rozhraní umožňuje toocommunicate virtuálnímu počítači Azure s Internetem, Azure a místních prostředků. Při vytváření virtuálního počítače pomocí hello portálu Azure, portál hello vytvoří jedno síťové rozhraní s výchozím nastavením pro vás. Místo toho může zvolte toocreate síťových rozhraní s vlastním nastavením a přidejte jeden nebo více síťových rozhraní tooa virtuální počítač při jeho vytvoření. Můžete také toochange výchozí síťové rozhraní nastavení pro existující rozhraní sítě. Tento článek vysvětluje, jak toocreate síťové rozhraní s vlastním nastavením, změňte existující nastavení, jako je například přiřazení sítě filtru (skupina zabezpečení sítě), přiřazení podsítě, nastavení serveru DNS a předávání IP a odstranit síťové rozhraní.

Pokud potřebujete tooadd, změnit nebo odebrat IP adresy pro síťové rozhraní, přečtěte si hello [Správa IP adres](virtual-network-network-interface-addresses.md) článku. Pokud potřebujete tooadd síťových rozhraní k, nebo odeberte síťová rozhraní z virtuálních počítačů, přečtěte si hello [přidat nebo odebrat síťových rozhraní](virtual-network-network-interface-vm.md) článku.


## <a name="before-you-begin"></a>Než začnete

Dokončení hello následující úkoly před dokončením všechny kroky v žádné části tohoto článku:

- Zkontrolujte hello [Azure omezuje](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) toolearn článek o omezeních pro síťová rozhraní.
- Přihlaste se toohello Azure [portál](https://portal.azure.com), rozhraní příkazového řádku Azure (CLI) nebo Azure PowerShell s účet Azure. Pokud nemáte účet Azure, si zaregistrovat [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud pomocí prostředí PowerShell příkazy toocomplete úlohy v tomto článku [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Zajistěte, že abyste měli nejnovější verzi rutin prostředí Azure PowerShell hello nainstalován hello. Zadejte tooget nápovědy pro příkazy prostředí PowerShell s příklady, `get-help <command> -full`.
- Pokud pomocí rozhraní příkazového řádku Azure (CLI) příkazy toocomplete úlohy v tomto článku [instalace a konfigurace rozhraní příkazového řádku Azure hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi hello hello nainstalované rozhraní příkazového řádku Azure. tooget nápovědy pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Namísto instalaci hello rozhraní příkazového řádku a jeho požadavky můžete použít hello prostředí cloudu Azure. Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure. Má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem. toouse hello cloudové prostředí, klikněte na tlačítko hello cloudové prostředí **> _** tlačítko hello horní části hello [portál](https://portal.azure.com).

## <a name="create-a-network-interface"></a>Vytvořit rozhraní sítě

Při vytváření virtuálního počítače pomocí hello portálu Azure, portál hello vytvoří rozhraní sítě s výchozím nastavením pro vás. Pokud byste místo zadat všechna nastavení síťového rozhraní, můžete vytvořit síťové rozhraní s vlastním nastavením a připojte hello síťové rozhraní tooa virtuálního počítače při vytváření virtuálního počítače hello (pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure hello). Můžete také vytvořit rozhraní sítě a přidat jej tooan existující virtuální počítač (pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure hello). toolearn jak toocreate virtuální počítač s existujícím síťových rozhraní nebo tooadd na nebo odeberte síťová rozhraní z existujících virtuálních počítačů, přečtěte si hello [přidat nebo odebrat síťových rozhraní](virtual-network-network-interface-vm.md) článku. Před vytvořením síťové rozhraní, musíte mít existující [virtuální sítě](virtual-networks-create-vnet-arm-pportal.md) v hello stejné umístění a předplatné vytvoříte síťové rozhraní na.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě hello pro vaše předplatné. Čtení hello [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku toolearn informace o přiřazení role a oprávnění tooaccounts.
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *síťových rozhraní*. Když **síťových rozhraní** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **síťových rozhraní** okno, které se zobrazí, klikněte na tlačítko **+ přidat**.
4. V hello **vytvořit síťové rozhraní** okno, které se zobrazí, zadejte, nebo vyberte hodnoty pro hello následující nastavení a potom klikněte na tlačítko **vytvořit**:

    |Nastavení|Povinné?|Podrobnosti|
    |---|---|---|
    |Name (Název)|Ano|Název Hello musí být jedinečný v rámci skupiny prostředků hello, kterou vyberete. V čase budete mít pravděpodobně několik síťových rozhraní ve vašem předplatném Azure. Čtení hello [konvence vytváření názvů](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions) článek pro návrhy při vytváření zásady vytváření názvů toomake Správa několik síťových rozhraní jednodušší. Název Hello nelze změnit po vytvoření hello síťové rozhraní.|
    |Virtuální síť|Ano|Vyberte hello virtuální sítě pro síťové rozhraní hello. Síťové rozhraní tooa virtuální síť, která existuje v hello stejné lze přiřadit pouze předplatném a umístění jako hello síťové rozhraní. Po vytvoření rozhraní sítě, nelze změnit hello virtuální sítě, které je přiřazen. Hello virtuálního počítače přidejte hello síťové rozhraní toomust existovat také ve hello stejné umístění a předplatné jako hello síťové rozhraní.|
    |Podsíť|Ano|Vyberte podsíť v rámci hello virtuální sítě, které jste vybrali. Můžete změnit podsíť hello hello síťové rozhraní je přiřazen tooafter jeho vytvoření.|
    |Přidělení privátní IP adresy|Ano| V tomto nastavení zvolíte metodu přiřazení hello hello adresu IPv4. Vyberte z následujících metod přiřazení hello: **dynamické:** když vyberete tuto možnost, Azure automaticky přiřadí dostupnou adresu z adresního prostoru hello hello podsítě, které jste vybrali. Azure může přiřadit jinou adresu tooa síťové rozhraní, když hello virtuálního počítače, které se spustí po co bylo v hello (deallocated) stavu Zastaveno. Hello adresa zůstane stejný hello, pokud restartování virtuálního počítače hello bez nutnosti v hello zastavilo (deallocated) stavu. **Statické:** při výběru této možnosti je nutné ručně přiřadit dostupnou IP adresu z v adresním prostoru hello hello podsítě, které jste vybrali. Statické adresy se nezmění, dokud je změnit nebo hello síťové rozhraní se odstraní. Hello přiřazení metodu můžete změnit po vytvoření hello síťové rozhraní. server Azure DHCP Hello přiřadí tato adresa toohello síťové rozhraní operačního systému hello hello virtuálního počítače.|
    |Skupina zabezpečení sítě|Ne| Ponechejte nastaven příliš**žádné**, vyberte existující [skupinu zabezpečení sítě](virtual-networks-nsg.md), nebo [vytvořit skupinu zabezpečení sítě](virtual-networks-create-nsg-arm-pportal.md). Skupiny zabezpečení sítě povolte toofilter síťový provoz směřující síťové rozhraní. Můžete použít žádnou nebo jednu zabezpečení skupiny tooa síťové rozhraní sítě. Můžete také použít žádnou nebo jednu skupinu zabezpečení sítě je přiřazen toohello podsíť hello síťové rozhraní. Pokud skupina zabezpečení sítě je síťové rozhraní použité tooa a hello podsíť hello síťové rozhraní je přiřazen, dojít k někdy neočekávaným výsledkům. skupiny zabezpečení sítě tootroubleshoot rozhraní použité toonetwork a podsítě, přečtěte si hello [odstraňování skupin zabezpečení sítě](virtual-network-nsg-troubleshoot-portal.md#nsg) článku.|
    |Předplatné|Ano|Vyberte jednu z vaší Azure [odběry](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). virtuální počítač Hello připojit síťové rozhraní tooand hello virtuální síť připojení toomust existovat v hello stejné předplatné.|
    |Privátní IP adresa (IPv6)|Ne| Pokud zaškrtnete toto políčko, adresa IPv6 je přiřazené toohello síťové rozhraní, kromě toohello adresu IPv4 přiřazen toohello síťové rozhraní. V tématu hello [IPv6](#IPv6) části tohoto článku důležité informace o použití protokolu IPv6 s síťových rozhraní. Nelze vybrat metodu přiřazení pro hello adresu IPv6. Pokud si zvolíte tooassign adresu IPv6, je přiřazen pomocí metody dynamického hello.
    |Název protokolu IPv6 (jenom se zobrazí, když hello **privátní IP adresa (IPv6)** je zaškrtnuté políčko) |Ano, pokud hello **privátní IP adresa (IPv6)** je zaškrtnuté políčko.| Tento název je přiřazen tooa sekundární konfiguraci IP adresy pro síťové rozhraní hello. Další informace o konfigurace protokolu IP v hello [zobrazení nastavení síťového rozhraní](#view-network-interface-settings) tohoto článku.|
    |Skupina prostředků|Ano|Vyberte existující [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) nebo ji vytvořte. Rozhraní sítě může existovat v hello stejné nebo jiné skupině prostředků, než hello virtuálního počítače, připojení, nebo hello virtuální sítě, kterou propojte jej s.|
    |Umístění|Ano|Hello virtuální počítač připojit síťové rozhraní tooand hello virtuální síť připojení toomust existovat v hello stejné [umístění](https://azure.microsoft.com/regions), nazývaná také jen tooas oblast.|

portál Hello neposkytuje hello možnost tooassign veřejnou IP adresu toohello síťové rozhraní, při vytváření, i když portál hello vytvoření veřejné IP adresy a přiřaďte ho tooa síťové rozhraní, při vytváření virtuálního počítače pomocí portálu hello. toolearn jak rozhraní tooadd veřejnou IP adresu toohello síť po vytvoření, čtení hello [Správa IP adres](virtual-network-network-interface-addresses.md) článku. Pokud chcete toocreate síťové rozhraní s veřejnou IP adresu, musíte použít hello rozhraní CLI nebo Powershellu toocreate hello síťové rozhraní.

>[!Note]
> Azure přiřadí MAC adres toohello síťové rozhraní až po hello síťové rozhraní je připojený tooa virtuální počítač a hello virtuálního počítače je hello prvním spuštění. Nelze zadat adresu MAC hello, že Azure přiřadí toohello síťové rozhraní. Hello MAC adres zůstane přiřazená toohello síťové rozhraní, dokud je neodstraní hello síťové rozhraní nebo hello privátní IP adresa přiřazené toohello primární konfiguraci IP hello primární síťové rozhraní se změní. Další informace o IP adresy a konfigurace protokolu IP, přečtěte si hello toolearn [Správa IP adres](virtual-network-network-interface-addresses.md) článku.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Vytvoření az síťových adaptérů sítě](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nové AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|

## <a name="view-network-interface-settings"></a>Zobrazení nastavení síťového rozhraní

Můžete zobrazit a po jejím vytvoření změnit většinu nastavení pro síťové rozhraní.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě hello pro vaše předplatné. Čtení hello [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku toolearn informace o přiřazení role a oprávnění tooaccounts.
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *síťových rozhraní*. Když **síťových rozhraní** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **síťových rozhraní** hello síťové rozhraní mají být tooview nebo změnit nastavení pro okno, které se zobrazí, klikněte na tlačítko.
4. Hello následující nastavení jsou uvedené v okně hello, který se zobrazí pro hello síťové rozhraní, které jste vybrali:
    - **Přehled:** poskytuje informace o hello síťového rozhraní, například tooit přiřazené hello IP adresy, hello virtuální sítě a podsítě hello síťové rozhraní je přiřazena k a hello virtuálního počítače hello síťové rozhraní je připojeno příliš (Pokud je připojen tooone). Hello následující obrázek ukazuje hello přehled nastavení pro síťové rozhraní s názvem **mywebserver256**: ![přehled rozhraní sítě](./media/virtual-network-network-interface/nic-overview.png) můžete přesunout síťové rozhraní tooa jiné skupině prostředků nebo předplatné kliknutím (**změnit**) další toohello **skupiny prostředků** nebo **název odběru**. Pokud přesunete hello síťové rozhraní, musíte přesunout všechny prostředky související toohello síťové rozhraní s ním. Pokud hello síťové rozhraní je připojený tooa virtuální počítač, například musíte také přesunout hello virtuálního počítače a další zdroje související s virtuální počítač. toomove síťového rozhraní, přečtěte si hello [přesunutí prostředku tooa novou skupinu prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal) článku. Hello článek uvádí předpoklady, a způsob toomove prostředků pomocí portálu Azure, prostředí PowerShell text hello a hello rozhraní příkazového řádku Azure.
    - **Konfigurace protokolu IP:** veřejných a privátních IPv4 a IPv6 adresy přiřazené tooIP konfigurace jsou zde uvedeny. Pokud adresu IPv6 je přiřazen tooan konfigurace protokolu IP, adresa hello se nezobrazí. Další informace o konfigurace protokolu IP a jak tooadd a odebrat IP adres v hello [konfigurace IP adresy pro síť Azure rozhraní](virtual-network-network-interface-addresses.md) článku. V této části jsou také nakonfigurované předávání IP a přiřazení podsítě. Další informace o těchto nastavení, přečtěte si hello toolearn [povolení nebo zakázání předávání IP](#enable-or-disable-ip-forwarding) a [změnit přiřazení podsítě](#change-subnet-assignment) části tohoto článku.
    - **Servery DNS:** můžete určit, který server DNS se síťové rozhraní je přiřazena službou hello servery Azure DHCP. Hello rozhraní sítě může dědit vlastnosti hello nastavení z hello je přiřazena k síťové rozhraní hello virtuální sítě, nebo máte vlastní nastavení, který přepíše nastavení hello hello virtuální sítě je přiřazena k. toomodify, co se zobrazí, dokončení hello kroky hello [servery DNS změnu](#change-dns-servers) tohoto článku.
    - **Skupina zabezpečení sítě (NSG):** zobrazí, které je skupina NSG přidružené toohello síťové rozhraní (pokud existuje). Skupina NSG obsahuje pravidla pro příchozí a odchozí toofilter síťových přenosů pro hello síťové rozhraní. Pokud skupina NSG přidružená toohello síťového rozhraní, hello název hello přidružené NSG se zobrazí. toomodify, co se zobrazí, dokončení hello kroky hello [spravovat přidružení skupiny zabezpečení sítě](virtual-network-manage-nsg-arm-portal.md#manage-associations) článku.
    - **Vlastnosti:** zobrazí klíč nastavení o hello síťového rozhraní, včetně jeho adresy MAC (prázdný, pokud hello síťového rozhraní není připojené tooa virtuálního počítače) a hello předplatného existuje v.
    - **Pravidla efektivní zabezpečení:** pravidla zabezpečení jsou uvedeny, pokud hello síťové rozhraní je připojený tooa spuštění virtuálního počítače a skupiny NSG přidružené toohello síťové rozhraní, je přiřazena k podsíti hello nebo obojí. toolearn Další informace o co se zobrazí, přečtěte si hello [odstraňování skupin zabezpečení sítě](virtual-network-nsg-troubleshoot-portal.md#nsg) článku. Další informace o skupin Nsg, přečtěte si hello toolearn [skupin zabezpečení sítě](virtual-networks-nsg.md) článku.
    - **Efektivní trasy:** jsou uvedeny trasy, pokud hello síťové rozhraní je připojený tooa spuštění virtuálního počítače. Hello trasy představují kombinaci hello Azure výchozích tras, všechny trasy definované uživatelem (UDR) a všechny trasy protokolu BGP, které mohou existovat pro rozhraní sítě hello podsíť hello přiřazené. toolearn Další informace o co se zobrazí, přečtěte si hello [řešení potíží s trasy](virtual-network-routes-troubleshoot-portal.md#view-effective-routes-for-a-network-interface) článku. Další informace o Azure výchozí a udr, přečtěte si hello toolearn [trasy definované uživatelem](virtual-networks-udr-overview.md) článku.
    - **Obecná nastavení Azure Resource Manager:** toolearn více informací o obecná nastavení Azure Resource Manager, přečtěte si hello [protokol aktivit](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [přístup k ovládacímu prvku (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [značky ](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [Zamkne](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json), a [skriptu pro automatizaci](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group) články.

**Příkazy**

Pokud adresu IPv6 je přiřazen tooa síťové rozhraní, hello výstup prostředí PowerShell vrátí hello fakt, že je přiřazená adresa hello, ale nevrací hello přiřazená adresa. Podobně hello CLI vrátí hello skutečnost, že hello adresa je přiřazena, ale vrátí *null* v jeho výstup hello adresou.

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[seznam seskupování sítě az](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#list) tooview síťová rozhraní hello předplatného; [az sítě seskupování zobrazit](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#show) tooview nastavení pro síťové rozhraní|
|PowerShell|[Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) tooview síťová rozhraní hello předplatné nebo zobrazení nastavení síťového rozhraní|

## <a name="change-dns-servers"></a>Změnit servery DNS

Hello DNS server je přiřazena službou hello Azure DHCP server toohello síťového rozhraní v rámci operačního systému virtuálního počítače hello. server DNS Hello přiřazené je, ať hello serveru DNS je pro síťové rozhraní. toolearn Další informace o nastavení překladu název pro rozhraní sítě, najdete v části [překlad názvů pro virtuální počítače](virtual-networks-name-resolution-for-vms-and-role-instances.md). Hello síťové rozhraní můžete dědí nastavení hello hello virtuální sítě, nebo použít svůj vlastní jedinečný nastavení, která přepsat nastavení hello hello virtuální sítě.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě hello pro vaše předplatné. Čtení hello [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku toolearn informace o přiřazení role a oprávnění tooaccounts.
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *síťových rozhraní*. Když **síťových rozhraní** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **síťových rozhraní** hello síťové rozhraní mají být tooview nebo změnit nastavení pro okno, které se zobrazí, klikněte na tlačítko.
4. V okně hello pro síťové rozhraní hello jste vybrali, klikněte na tlačítko **servery DNS** pod **nastavení**.
5. Klikněte na možnost:
    - **Dědění z virtuální sítě (výchozí)**: Toto nastavení vyberte, možnost tooinherit hello DNS serveru pro síťové rozhraní hello virtuální sítě hello je přiřazena k definována. Na úrovni hello virtuální sítě je definována buď vlastního serveru DNS nebo serveru DNS poskytnutých platformou Azure hello. Hello Azure DNS server lze přeložit názvy hostitelů pro toohello prostředků přiřazené stejné virtuální síti. Plně kvalifikovaný název domény musí být použité tooresolve pro prostředky přiřazené toodifferent virtuální sítě.
    - **Vlastní**: můžete konfigurovat vlastní názvy tooresolve server DNS v rámci více virtuálních sítí. Zadejte IP adresu hello hello serveru chcete toouse jako DNS server. Hello adresu serveru DNS, které určíte je přiřadit pouze toothis síťové rozhraní a přepsání, která je přiřazená všechna nastavení DNS pro síťové rozhraní hello virtuální sítě hello.
6. Klikněte na **Uložit**.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[aktualizace az sítě nic](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="enable-or-disable-ip-forwarding"></a>Povolení nebo zakázání předávání IP

Předávání IP umožňuje hello virtuálního počítače, které síťové rozhraní je připojen k:
- Zobrazí síťový provoz, není určené pro jednu z hello IP adresy přiřazené tooany konfigurace protokolu IP hello přiřazené toohello síťové rozhraní.
- Odesílat přenos v síti s jinou zdrojovou IP adresou než hello jeden přiřazené tooone konfigurace protokolu IP síťového rozhraní.

pro každé síťové rozhraní, která je připojená toohello virtuální počítač, který přijímá provoz, který hello tooforward potřeby virtuální počítač musí být povolené nastavení Hello. Virtuální počítač může předat provoz, zda obsahuje více síťových rozhraní nebo tooit jedné síťové rozhraní, které jsou připojené. Při předávání IP je Azure nastavení, musí také hello virtuální počítač spuštěn aplikací mít tooforward hello přenosů, například Brána firewall, optimalizace sítě WAN a aplikace pro vyrovnávání zatížení. Když virtuální počítač běží síťových aplikací, hello virtuální počítač je často označují tooas virtuální síťové zařízení. Zobrazí se seznam síťových virtuální zařízení připravené toodeploy v hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Předávání IP se obvykle používá s trasy definované uživatelem. Další informace o trasy definované uživatelem, přečtěte si hello toolearn [trasy definované uživatelem](virtual-networks-udr-overview.md) článku.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě hello pro vaše předplatné. Čtení hello [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku toolearn informace o přiřazení role a oprávnění tooaccounts.
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *síťových rozhraní*. Když **síťových rozhraní** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **síťových rozhraní** hello síťové rozhraní mají být tooenable nebo zakázat předávání protokolu IP pro okno, které se zobrazí, klikněte na tlačítko.
4. V okně hello pro síťové rozhraní hello jste vybrali, klikněte na tlačítko **konfigurace protokolu IP** v hello **nastavení** části.
5. Klikněte na tlačítko **povoleno** nebo **zakázané** nastavení hello toochange (výchozí nastavení).
6. Klikněte na **Uložit**.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[aktualizace az sítě nic](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet-assignment"></a>Změnit přiřazení podsítě

Hello podsíť, ale není hello virtuální sítě, přiřazený síťové rozhraní, můžete změnit.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě hello pro vaše předplatné. Čtení hello [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku toolearn informace o přiřazení role a oprávnění tooaccounts.
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *síťových rozhraní*. Když **síťových rozhraní** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **síťových rozhraní** hello síťové rozhraní mají být tooview nebo změnit nastavení pro okno, které se zobrazí, klikněte na tlačítko.
4. Klikněte na tlačítko **konfigurace protokolu IP** pod **nastavení** v okně hello pro síťové rozhraní hello jste vybrali. Pokud uvedené všechny privátní IP adresy pro všechny konfigurace IP **(statické)** další toothem, musíte změnit hello IP adresu přiřazení metoda toodynamic pomocí hello kroků, které následují. Všechny privátní IP adresy musí být přiřazeno hello dynamické přiřazení metoda toochange hello podsíť přiřazení pro hello síťové rozhraní. Pokud hello adresy přiřazené pomocí metody dynamického hello, pokračujte toostep pět. Pokud se metoda statického přidělování hello přiřazené žádné adresy IPv4, proveďte následující kroky toochange hello přiřazení metoda toodynamic hello:
    - Klikněte na možnost konfigurace protokolu IP hello chcete toochange hello metoda na přiřazení adres IPv4 pro hello seznamu konfigurace protokolu IP.
    - V hello zobrazeném okně pro konfiguraci protokolu IP hello, klikněte na **dynamické** pro hello **přiřazení** metoda. Nelze přiřadit adresu IPv6 s metodou hello statické přiřazení.
    - Klikněte na **Uložit**.
5. Vyberte podsíť hello chcete tooconnect hello síťové rozhraní toofrom hello **podsíť** rozevíracího seznamu.
6. Klikněte na **Uložit**. Nové dynamické adresy přiřazené z hello rozsah adres podsítě pro podsíť nové hello. Po přiřazení nové podsítě hello sítě rozhraní tooa, můžete přiřadit statická adresa IPv4 z hello nový rozsah adres podsítě, pokud si zvolíte. Další informace o přidání, změně a odebrání IP adresy pro síťové rozhraní, přečtěte si hello toolearn [Správa IP adres](virtual-network-network-interface-addresses.md) článku.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[aktualizace ip-config seskupování az sítě](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-a-network-interface"></a>Odstranit síťové rozhraní

Síťové rozhraní můžete odstranit, dokud není připojené tooa virtuálního počítače. Pokud je připojené tooa virtuálního počítače, musíte první místní hello virtuálního počítače v hello zastavena (nepřiřazeném) stavu, pak odpojit hello síťové rozhraní z hello virtuálního počítače, než budete moct odstranit hello síťové rozhraní. toodetach síťové rozhraní z virtuálního počítače, dokončení hello kroky hello [Odpojit síťové rozhraní z virtuálního počítače](virtual-network-network-interface-vm.md#vm-remove-nic) části hello [přidat nebo odebrat síťových rozhraní](virtual-network-network-interface-vm.md) článku. Odstranění virtuálního počítače umožňuje odpojit všechny připojené tooit rozhraní sítě, ale neodstraní hello síťových rozhraní.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě hello pro vaše předplatné. Čtení hello [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku toolearn informace o přiřazení role a oprávnění tooaccounts.
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *síťových rozhraní*. Když **síťových rozhraní** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. Klikněte pravým tlačítkem na hello síťové rozhraní toodelete a klikněte na **odstranit**.
4. Klikněte na tlačítko **Ano** tooconfirm odstranění hello síťového rozhraní.

Když odstraníte síťového rozhraní, všechny MAC nebo tooit přiřazené IP adresy vydávají.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[AZ síť seskupování odstranit](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Odebrat AzureRmNetworkInterface](/powershell/module/azurerm.network/remove-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Další kroky
toocreate virtuální počítač s více síťových rozhraní nebo IP adresy, přečtěte si hello následující články:

**Příkazy**

|Úkol|Nástroj|
|---|---|
|Vytvoření virtuálního počítače s několika síťovými kartami|[Rozhraní příkazového řádku](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [prostředí PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Vytvoření jednoho virtuálního počítače síťový adaptér s více adresami IPv4|[Rozhraní příkazového řádku](virtual-network-multiple-ip-addresses-cli.md), [prostředí PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Vytvoření jednoho virtuálního počítače síťový adaptér s privátní adresou IPv6 (za pro vyrovnávání zatížení Azure)|[Rozhraní příkazového řádku](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [prostředí PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [šablony Azure Resource Manageru](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
