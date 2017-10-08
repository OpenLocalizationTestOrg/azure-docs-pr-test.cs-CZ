---
title: "aaaCreate, měnit a odstraňovat virtuální sítě Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate, měnit a odstraňovat virtuální sítě v Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.openlocfilehash: 7dfe6632753182eae2a13bb0327f03f75e03d057
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-change-or-delete-a-virtual-network"></a>Vytvoření, změnit nebo odstranit virtuální síť

Zjistěte, jak toocreate a odstranit virtuální síť a změnit nastavení, jako třeba servery DNS a IP adres prostorů pro existující virtuální síť.

Virtuální síť je reprezentace vlastní sítě v cloudu hello. Virtuální síť je to logická izolace cloudu Azure, který je vyhrazený tooyour předplatného Azure hello. Pro každou virtuální síť, kterou vytvoříte můžete:
- Vyberte tooassign prostor adres. Adresní prostor se skládá z jedné nebo více rozsahů adres, které jsou definovány pomocí notace CIDR (Classless Inter-Domain směrování) zápisu, jako je 10.0.0.0/16.
- Zvolte toouse hello Azure DNS server, nebo pomocí serveru DNS. Všechny prostředky, které jsou připojené toohello virtuální sítě jsou přiřazeny této názvy tooresolve server DNS v rámci virtuální sítě hello.
- Segment hello virtuální sítě do podsítí, každou s vlastní rozsah adres, v rámci hello adresní prostor virtuální sítě hello. jak toocreate, změn a odstranění podsítě, najdete v části toolearn [přidat, změnit nebo odstranit podsítě](virtual-network-manage-subnet.md).

Tento článek vysvětluje, jak změnit toocreate a odstranění virtuální sítě pomocí modelu nasazení Azure Resource Manager hello.

## <a name="before"></a>Než začnete

Než začnete hello úlohy, které jsou popsané v tomto článku, dokončete hello následující požadavky:

- Pokud jste nový tooworking s virtuálními sítěmi, doporučujeme, abyste si prošli hello cvičení v [vytvoření vaší první virtuální síť Azure](virtual-network-get-started-vnet-subnet.md). Tento postup může pomoci při seznámení s virtuálními sítěmi.
- toolearn o omezeních pro virtuální sítě, zkontrolujte [Azure omezení](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Přihlaste se toohello portálu Azure, hello nástroj příkazového řádku Azure (Azure CLI) nebo Azure PowerShell pomocí účtu Azure. Pokud nemáte účet Azure, zaregistrujte si [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud máte v plánu toouse příkazy prostředí PowerShell toocomplete hello úkoly popsané v tomto článku, musíte nejdřív [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi rutin prostředí Azure PowerShell nainstalovaný hello hello. Zadejte tooget nápovědy pro příkazy prostředí PowerShell v příkladech hello `get-help <command> -full`.
- Pokud máte v plánu toouse toocomplete hello úkoly popsané v tomto článku příkazy příkazového řádku Azure CLI, je nutné nejprve [instalace a konfigurace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi hello nainstalované rozhraní příkazového řádku Azure. tooget pomoc s příkazy rozhraní příkazového řádku Azure, zadejte `az <command> --help`.


## <a name="create-vnet"></a>Vytvoření virtuální sítě

toocreate virtuální sítě:

1. Přihlaste se toohello [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě hello (minimálně) pro vaše předplatné. najdete v části Další informace o přiřazení role a oprávnění tooaccounts toolearn [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Klikněte na tlačítko **nový** > **sítě** > **virtuální síť**.
3. Na hello **virtuální síť** okno, v hello **vybrat model nasazení** pole, ponechte **Resource Manager** vybrané a pak klikněte na tlačítko **vytvořit**.
4. Na hello **vytvořit virtuální síť** okno, zadejte nebo vyberte hodnoty pro hello následující nastavení a potom klikněte na tlačítko **vytvořit**:
    - **Název**: název hello musí být jedinečný v hello [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) vybrat toocreate hello virtuální sítě v. Název hello nelze změnit po vytvoření virtuální sítě hello. Můžete vytvořit několik virtuálních sítí v čase. Pojmenování návrhy, najdete v části [konvence vytváření názvů](/azure/architecture/best-practices/naming-conventions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). Následující zásady vytváření názvů může pomoci zajistit je snazší toomanage více virtuálních sítí.
    - **Adresní prostor**: Zadejte hello adresní prostor v notaci CIDR. Hello adresního prostoru, které definujete může být veřejné nebo privátní (RFC 1918). Jestli jako veřejný nebo privátní definujete hello adresní prostor, hello adresní prostor je dostupný jenom z v rámci virtuální sítě hello, od vzájemně propojena virtuální sítě a od žádné místní sítě, že jste připojení toohello virtuální sítě. Nelze přidat hello následujících adresních prostorů:
        - 224.0.0.0/4 (vícesměrové vysílání)
        - 255.255.255.255/32 (vysílání)
        - 127.0.0.0/8 (zpětné smyčky)
        - 169.254.0.0/16 (Link-local)
        - 168.63.129.16/32 (interní DNS)

      I když při vytvoření virtuální sítě hello lze definovat pouze jeden adresní prostor, po vytvoření virtuální sítě hello můžete přidat další adresní prostory. toolearn způsobu tooadd adresu místo tooan existující virtuální sítě, najdete v [přidat nebo odebrat adresní prostor](#add-address-spaces) v tomto článku.

      >[!WARNING]
      >Pokud virtuální síť obsahuje adresní prostory, které se překrývají s jinou virtuální síť nebo místní sítí, nemůže být připojen hello dvě sítě. Před definujete adresní prostor, zvažte, zda je vhodné tooconnect hello virtuální sítě tooother virtuální sítě nebo místními sítěmi v hello budoucí.
      >
      >

    - **Název podsítě**: název podsítě hello musí být jedinečný v rámci virtuální sítě hello. Název podsítě hello nelze změnit po vytvoření podsítě hello. portál Hello vyžaduje definování jednu podsíť při vytváření virtuální sítě, i když virtuální sítě není požadovaná toohave žádné podsítě. Hello portálu můžete definovat pouze jednu podsíť při vytváření virtuální sítě. Další podsítě toohello virtuální sítě můžete přidat později, po vytvoření virtuální sítě hello. tooadd tooa podsíť virtuální sítě, najdete v části [vytvořit podsíť](virtual-network-manage-subnet.md#create-subnet) v [Create, změnit nebo odstranit podsítě](virtual-network-manage-subnet.md). Můžete vytvořit virtuální síť, která má několik podsítí pomocí příkazového řádku Azure CLI nebo prostředí PowerShell.

      >[!TIP]
      >V některých případech admins vytvořit různých podsítích směrování mezi podsítěmi hello toofilter nebo řízení provozu. Před definujete podsítě, zvažte, jak chcete toofilter a směrovat provoz mezi podsítě. toolearn Další informace o filtrování přenosů mezi podsítěmi, najdete v části [skupin zabezpečení sítě](virtual-networks-nsg.md). Azure automaticky směrování provozu mezi podsítěmi, ale můžete přepsat Azure výchozí trasy. toolearn způsobu toooverride Azure výchozí podsíť směrování provozu, najdete v [trasy definované uživatelem](virtual-networks-udr-overview.md).
      >

    - **Rozsah adres podsítě**: hello rozsah musí být v rámci adresního prostoru hello jste zadali pro hello virtuální sítě. Hello nejmenší rozsah, které můžete zadat je /29, který poskytuje osm IP adresy pro podsíť hello. Azure rezervy hello první a poslední adres v každé podsíti pro protokol shoda. Tři další adresy jsou vyhrazené pro použití služby Azure. V důsledku toho virtuální síť s rozsahem adres podsítě /29 má jenom tři použitelných IP adresách. Pokud máte v plánu tooconnect bránu virtuální sítě tooa sítě VPN, musíte vytvořit podsíť brány. Další informace o [aspekty rozsah konkrétní adresu podsítě brány](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Rozsah adres hello můžete změnit po vytvoření hello podsítě pro určité podmínky. jak zjistit, toochange rozsah adres podsítě, toolearn [změnit nastavení podsítě](#change-subnet) v [přidat, změnit nebo odstranit podsítě](virtual-network-manage-subnet.md).
    - **Předplatné**: vyberte [předplatné](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). Nemůžete použít hello stejné virtuální síti ve více než jedno předplatné. Můžete však připojit virtuální síť v jedné sítě toovirtual předplatného v jiných předplatných. tooconnect virtuální sítě v různých předplatných, použít bránu VPN Azure nebo partnerského vztahu virtuální sítě. Připojení virtuální sítě toohello jakýmikoli prostředky Azure musí být v hello stejného předplatného jako virtuální síť hello.
    - **Skupina prostředků**: Vyberte existující [skupiny prostředků](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) nebo vytvořte novou. Prostředek služby Azure, když připojíte toohello virtuální sítě může být v hello stejné skupině prostředků jako virtuální síť hello nebo v jiné skupině prostředků.
    - **Umístění**: Vyberte Azure [umístění](https://azure.microsoft.com/regions/), označované také jako oblast. Virtuální síť může být pouze v jednom umístění Azure. Můžete však připojit virtuální síť v jedné umístění tooa virtuální síti v jiném umístění s použitím brány VPN. Připojení virtuální sítě toohello jakýmikoli prostředky Azure musí být v hello stejné umístění jako virtuální síť hello.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[Vytvoření sítě vnet az](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nový AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name = "view-vnet"></a>Zobrazit virtuální sítě a nastavení

tooview virtuální sítě a nastavení:

1. Přihlaste se toohello [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě hello (minimálně) pro vaše předplatné. najdete v části Další informace o přiřazení role a oprávnění tooaccounts toolearn [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portálu vyhledávacího pole zadejte **virtuální sítě**. Ve výsledcích hledání hello, klikněte na tlačítko **virtuální sítě**.
3. Na hello **virtuální sítě** okně klikněte na tlačítko hello virtuální síť, kterou chcete tooview nastavení pro.
4. Hello následující nastavení jsou uvedené v okně hello hello virtuální sítě, které jste vybrali:
    - **Přehled**: poskytuje informace o hello virtuální síť, včetně adresní prostor a servery DNS. Hello následující snímek obrazovky ukazuje hello přehled nastavení pro virtuální síť s názvem **MyVNet**:

        ![Přehled rozhraní sítě](./media/virtual-network-manage-network/vnet-overview.png)

      Na hello **přehled** okno, můžete přesunout virtuální sítě tooa jiné předplatné nebo prostředek skupiny. toolearn jak toomove virtuální sítě, najdete v části [přesunout prostředky tooa jiné skupině prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Hello článek uvádí předpoklady, a jak hello toomove prostředky pomocí portálu Azure, prostředí PowerShell a rozhraní příkazového řádku Azure. Všechny prostředky, které jsou připojené toohello virtuální síť, musíte přesunout s hello virtuální sítě.
    - **Adresní prostor**: hello adresní prostory, které jsou přiřazeny toohello virtuální sítě jsou uvedené. hello toolearn jak tooadd a odebrat adresní prostor, dokončete kroky v [přidat nebo odebrat adresní prostor](#address-spaces) v tomto článku.
    - **Připojená zařízení**: jsou uvedeny všechny prostředky, které jsou připojené toohello virtuální sítě. V předchozím snímku obrazovky hello tři síťových rozhraní a jedna služba Vyrovnávání zatížení jsou připojené toohello virtuální sítě. Nové zdroje, musíte vytvořit a připojit virtuální síť toohello jsou zobrazeny. Pokud odstraníte prostředek, který byl toohello připojené virtuální sítě, již nadále nebude zobrazovat v seznamu hello.
    - **Podsítě**: se zobrazí seznam podsítí, které existují v rámci virtuální sítě hello. jak tooadd a odebrat podsíť, najdete v části toolearn [vytvořit podsíť](virtual-network-manage-subnet.md#create-subnet) a [odstranit podsíť](virtual-network-manage-subnet.md#delete-subnet) v [přidat, změnit nebo odstranit podsítě](virtual-network-manage-subnet.md).
    - **Servery DNS**: můžete určit, zda hello Azure interní server DNS nebo vlastního serveru DNS poskytuje překlad názvů pro zařízení, které jsou připojené toohello virtuální sítě. Když vytvoříte virtuální síť pomocí hello portálu Azure, serverů Azure DNS se používají pro překlad názvu virtuální sítě, ve výchozím nastavení. servery DNS hello toomodify, kroky dokončení hello [přidat, změnit nebo odebrat DNS server](#dns-servers) v tomto článku.
    - **Partnerské vztahy**: Pokud jsou v rámci předplatného hello existující partnerských vztahů, jsou zde uvedeny. Můžete zobrazit nastavení pro existující partnerských vztahů, nebo vytvořit, změnit nebo odstranění partnerských vztahů. toolearn Další informace o partnerských vztahů, najdete v části [partnerský vztah virtuální sítě](virtual-network-peering-overview.md).
    - **Vlastnosti**: nastavení zobrazí o hello virtuální síť, včetně ID prostředku hello virtuální sítě a hello je v předplatném Azure.
    - **Diagram**: hello diagram poskytuje vizuální reprezentace všechna zařízení, které jsou připojené toohello virtuální sítě. Hello diagram má některé klíčové informace o zařízení hello. Klikněte na tlačítko toomanage zařízení v tomto zobrazení v diagramu hello hello zařízení.
    - **Obecná nastavení Azure**: toolearn Další informace o běžných nastavení Azure, najdete v části hello následující informace:
        *   [Protokol aktivit](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
        *   [Řízení přístupu (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
        *   [Značky](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags)
        *   [Zámky.](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        *   [Skriptu pro automatizaci](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[Zobrazit virtuální síť az sítě](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork/?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="add-address-spaces"></a>Přidat nebo odebrat adresní prostor.

Můžete přidávat a odebírat adresní prostory virtuální sítě. Adresní prostor je třeba zadat v notaci CIDR a se nesmí překrývat s další adresní prostory v rámci hello stejné virtuální síti. Hello adresní prostory, které definujete může být veřejné nebo privátní (RFC 1918). Jestli jako veřejný nebo privátní definujete hello adresní prostor, hello adresní prostor je dostupný jenom z v rámci virtuální sítě hello, od vzájemně propojena virtuální sítě a od žádné místní sítě, že jste připojení toohello virtuální sítě. Nelze přidat hello následujících adresních prostorů:

- 224.0.0.0/4 (vícesměrové vysílání)
- 255.255.255.255/32 (vysílání)
- 127.0.0.0/8 (zpětné smyčky)
- 169.254.0.0/16 (Link-local)
- 168.63.129.16/32 (interní DNS)

tooadd nebo odeberte adresní prostor:

1. Přihlaste se toohello [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě hello (minimálně) pro vaše předplatné. najdete v části Další informace o přiřazení role a oprávnění tooaccounts toolearn [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portálu vyhledávacího pole zadejte **virtuální sítě**. Ve výsledcích hledání hello, vyberte **virtuální sítě**.
3. Na hello **virtuální sítě** okně klikněte na tlačítko hello virtuální sítě, pro kterou chcete tooadd nebo odeberte adresní prostor.
4. Na hello virtuální sítě okno, v části **nastavení**, klikněte na tlačítko **adresní prostor**.
5. V okně hello hello adresního prostoru použijte některý z hello následující možnosti:
    - **Přidání adresního prostoru**: Zadejte hello nový adresní prostor. Hello adresní prostor se nesmí překrývat s existující adresní prostor, který je definován hello virtuální sítě.
    - **Odebrat adresní prostor**: klikněte pravým tlačítkem na adresní prostor a pak klikněte na **odebrat**. Pokud existuje podsíť hello adresního prostoru, nelze odebrat hello adresní prostor. tooremove adresní prostor, musíte nejprve odstranit všechny podsítě (a všechny prostředky, které jsou připojené toohello podsítě), neexistuje v adresním prostoru hello.
6. Klikněte na **Uložit**.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|Pouze správce prostředků|[Aktualizace virtuální sítě az sítě](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="dns-servers"></a>Přidat, změnit nebo odebrat DNS server

Všechny virtuální počítače, které jsou připojené toohello virtuální sítě se hello servery DNS, které zadáte pro virtuální síť hello zaregistrovat. Také používají hello zadaný server DNS pro rozlišení názvu. Každé síťové rozhraní (NIC) ve virtuálním počítači může mít svůj vlastní nastavení serveru DNS. Pokud síťový adaptér má svou vlastní nastavení serveru DNS, budou přepsat nastavení serveru DNS hello hello virtuální sítě. toolearn Další informace o nastavení DNS pro síťový adaptér, najdete v části [síťové rozhraní úlohy a nastavení](virtual-network-network-interface.md#change-dns-servers). toolearn Další informace o překladu názvů u virtuálních počítačů a instancí rolí ve službě Azure Cloud Services, najdete v části [překlad názvů pro virtuální počítače a instance rolí](virtual-networks-name-resolution-for-vms-and-role-instances.md). tooadd, změnit nebo odebrat DNS server:

1. Přihlaste se toohello [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě hello (minimálně) pro vaše předplatné. najdete v části Další informace o přiřazení role a oprávnění tooaccounts toolearn [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portálu vyhledávacího pole zadejte **virtuální sítě**. Ve výsledcích hledání hello, vyberte **virtuální sítě**.
3. Na hello **virtuální sítě** okně klikněte na tlačítko hello chcete toochange nastavení DNS pro virtuální sítě.
4. Na hello virtuální sítě okno, v části **nastavení**, klikněte na tlačítko **servery DNS**.
5. Vyberte jednu z hello následující možnosti v okně hello, který obsahuje seznam serverů DNS:
    - **Výchozí (zadaný Azure)**: všechny názvy prostředků a privátní IP adresy jsou automaticky registrované toohello servery Azure DNS. Abyste mohli vyřešit názvy mezi všechny prostředky, které jsou připojené toohello stejné virtuální síti. Názvy tooresolve tuto možnost nelze použít ve virtuální sítě. tooresolve názvy virtuálních sítí, musíte použít vlastního serveru DNS.
    - **Vlastní**: můžete přidat jeden nebo více serverů, až toohello Azure omezení pro virtuální síť. toolearn Další informace o limity serveru DNS, najdete v části [Azure omezení](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). Máte hello následující možnosti:
        - **Přidat adresu**: Přidá hello serveru tooyour seznamu serverů DNS virtuální sítě. Tato možnost také zaregistruje hello DNS server s Azure. Pokud již jste registrováni DNS server s Azure, můžete vybrat tento server DNS v seznamu hello.
        - **Odebrat adresu**: klikněte na tlačítko Další toohello serveru, které chcete tooremove, **X**. Odstraňování server hello odebere hello server pouze z tohoto seznamu virtuální sítě. Hello DNS server zůstane registrované v Azure pro vaše další toouse virtuální sítě.
        - **Změna pořadí adresy serverů DNS**: je důležité, můžete seznam serverů DNS do hello tooverify opravte pořadí pro vaše prostředí. Seznamy server DNS se používají v pořadí hello, že jsou uvedené. Nepracují jako instalace s kruhovým dotazováním. Pokud hello první server DNS v seznamu hello dostupný, klient hello používá tento server DNS, bez ohledu na to, zda je hello DNS server fungovat správně. Odeberte všechny servery DNS hello, které jsou uvedeny a poté je přidejte zpět v pořadí hello, který chcete.
        - **Změnit adresu**: zvýrazněte hello server DNS v seznamu hello a potom zadejte název nové hello.
6. Klikněte na **Uložit**.
7. Restartujte hello virtuálních počítačů, které jsou připojené toohello virtuální sítě, takže jsou přiřazeny hello nové nastavení serveru DNS. Virtuální počítače pokračovat toouse dokud nebudou restartovány jejich aktuální nastavení služby DNS.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[Aktualizace virtuální sítě az sítě](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="delete-vnet"></a>Odstranění virtuální sítě

Virtuální síť můžete odstranit pouze v případě, že neexistují žádné připojené tooit prostředky. Pokud jsou prostředky připojené tooany podsíti virtuální sítě hello, musíte nejprve odstranit hello prostředky, které jsou připojené tooall podsítí v rámci virtuální sítě hello. kroky Hello trvat toodelete prostředek lišit v závislosti na hello prostředků. jak toodelete prostředky, které jsou připojené toosubnets číst toolearn hello dokumentace pro každý typ prostředku má toodelete. toodelete virtuální sítě:

1. Přihlaste se toohello [portál](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě hello pro vaše předplatné. najdete v části Další informace o přiřazení role a oprávnění tooaccounts toolearn [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portálu vyhledávacího pole zadejte **virtuální sítě**. Ve výsledcích hledání hello, klikněte na tlačítko **virtuální sítě**.
3. Na hello **virtuální sítě** okně, vyberte hello chcete toodelete virtuální sítě.
4. V okně hello virtuální sítě, tooconfirm, že neexistují žádná zařízení připojené toohello virtuální sítě, v části **nastavení**, klikněte na tlačítko **připojená zařízení**. Připojená zařízení je nutné před odstraněním hello virtuální sítě odstranit. Pokud nejsou připojené zařízení, klikněte na tlačítko **přehled**.
5. V horní části hello hello okna, klikněte na tlačítko hello **odstranit** ikonu.
6. odstranění hello tooconfirm hello virtuální sítě, klikněte na tlačítko **Ano**.


**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[odstranění sítě Azure vnet](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetwork](/powershell/module/azurerm.network/remove-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="next-steps"></a>Další kroky

- toocreate virtuální počítač a připojte ho tooa virtuální sítě, najdete v části [vytvoření virtuální sítě a připojení virtuálních počítačů](virtual-network-get-started-vnet-subnet.md#create-vms).
- toofilter síťový provoz mezi podsítěmi v rámci virtuální sítě, najdete v části [vytvoření skupin zabezpečení sítě](virtual-networks-create-nsg-arm-pportal.md).
- toopeer tooanother virtuální sítě virtuální sítě, najdete v části [vytvoření virtuální sítě partnerského vztahu](virtual-network-create-peering.md#portal).
- toolearn o možnosti připojení virtuální sítě do místní sítě tooan, najdete v části [o službě VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).
