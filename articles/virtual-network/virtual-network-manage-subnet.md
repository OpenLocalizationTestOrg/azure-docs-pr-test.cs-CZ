---
title: "aaaAdd, změnit nebo odstranit podsíť virtuální sítě Azure | Microsoft Docs"
description: "Zjistěte, jak tooadd, změnit nebo odstranit podsíť virtuální sítě v Azure."
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
ms.openlocfilehash: 0d6d813b7e2fc52a00a87f6c6714ab5b7ca589ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Přidat, změnit nebo odstranit podsíť virtuální sítě

Zjistěte, jak tooadd, změnit nebo odstranit podsíť virtuální sítě. 

Pokud nejste obeznámení s virtuálními sítěmi, před přidat, změnit nebo odstranit podsíť, doporučujeme, abyste si přečetli [Přehled virtuálních sítí Azure](virtual-networks-overview.md) a [vytvoření, změny a odstranění virtuální sítě](virtual-network-manage-network.md). Všechny prostředky Azure, které jsou nasazeny do virtuální sítě se nasadí do podsítě virtuální sítě. Několik podsítí jsou obvykle vytvořeny v rámci virtuální sítě pro:
- **Filtrovat provoz mezi podsítěmi**. Můžete použít sítě zabezpečení skupiny toosubnets toofilter příchozí a odchozí síťový provoz pro všechny prostředky (třeba virtuální počítače), které jsou ve virtuální síti hello. toolearn Další informace o tom, jak toocreate skupinu zabezpečení sítě najdete v části [vytvoření skupin zabezpečení sítě](virtual-networks-create-nsg-arm-pportal.md).
- **Řídit směrování mezi podsítěmi**. Azure vytvoří výchozí trasy, aby automaticky směrovat přenosy mezi podsítěmi. Azure výchozí trasy můžete přepsat tak, že vytvoříte trasy definované uživatelem. toolearn Další informace o trasy definované uživatelem, najdete v části [vytvořit trasy definované uživatelem](virtual-network-create-udr-arm-ps.md). 

Tento článek vysvětluje, jak změnit tooadd a odstranit podsíť pro virtuální sítě, které byly vytvořeny pomocí modelu nasazení Azure Resource Manager hello.
 
## <a name="before"></a>Než začnete

Než začnete hello úlohy, které jsou popsané v tomto článku, dokončete hello následující požadavky:

- Pokud jste nový tooworking s virtuálními sítěmi, doporučujeme, abyste si prošli hello cvičení v [vytvoření vaší první virtuální síť Azure](virtual-network-get-started-vnet-subnet.md). Tento postup může pomoci při seznámení s virtuálními sítěmi.
- toolearn o omezeních pro virtuální sítě, zkontrolujte [Azure omezení](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Přihlaste se toohello portálu Azure, hello nástroj příkazového řádku Azure (Azure CLI) nebo Azure PowerShell pomocí účtu Azure. Pokud nemáte účet Azure, zaregistrujte si [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud máte v plánu toouse příkazy prostředí PowerShell toocomplete hello úkoly popsané v tomto článku, musíte nejdřív [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi rutin prostředí Azure PowerShell nainstalovaný hello hello. Zadejte tooget nápovědy pro příkazy prostředí PowerShell v příkladech hello `get-help <command> -full`.
- Pokud máte v plánu toouse toocomplete hello úkoly popsané v tomto článku příkazy rozhraní příkazového řádku Azure, které je nutné buď:
    - [Instalace a konfigurace rozhraní příkazového řádku Azure hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi hello nainstalované rozhraní příkazového řádku Azure.
    - Použijte hello prostředí cloudu Azure. Místo instalace hello rozhraní příkazového řádku a jeho závislé součásti, můžete použít hello prostředí cloudu Azure. Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure. Má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem. toouse hello cloudové prostředí, klikněte na tlačítko hello prostředí cloudu (**> _**) ikona hello horní části hello portálu Azure. 

  tooget pomoc s příkazy rozhraní příkazového řádku Azure, zadejte `az <command> --help`.

## <a name="create-subnet"></a>Přidat podsíť

tooadd podsítě:

1. Přihlaste se toohello [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě hello (minimálně) pro vaše předplatné. najdete v části Další informace o přiřazení role a oprávnění tooaccounts toolearn [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portálu vyhledávacího pole zadejte **virtuální sítě**. Ve výsledcích hledání hello, klikněte na tlačítko **virtuální sítě**.
3. Na hello **virtuální sítě** okně klikněte na tlačítko hello chcete tooadd do podsítě virtuální sítě.
4. V okně hello virtuální sítě, klikněte na tlačítko **podsítě**.
5. Klikněte na tlačítko **+ podsítě**.
6. Na hello **přidat podsíť** okno, zadejte hodnoty pro hello následující parametry:
    - **Název**: název hello musí být jedinečný v rámci virtuální sítě hello.
    - **Rozsah adres**: hello rozsahu musí být jedinečný v rámci hello adresního prostoru virtuální sítě hello. rozsah Hello se nesmí překrývat s další rozsahy adres podsítě v rámci virtuální sítě hello. Hello adresní prostor musí být určena pomocí notace CIDR (Classless Inter-Domain směrování) zápisu. Ve virtuální síti s 10.0.0.0/16 prostoru adres, můžete třeba definovat adresního prostoru podsítě 10.0.0.0/24. Hello nejmenší rozsah, které můžete zadat je /29, který poskytuje osm IP adresy pro podsíť hello. Azure rezervy hello první a poslední adres v každé podsíti pro protokol shoda. Tři další adresy jsou vyhrazené pro použití služby Azure. V důsledku toho definování podsíť s/29 adres rozsah výsledků v tři použitelných IP adresách v podsíti hello. Pokud máte v plánu tooconnect bránu virtuální sítě tooa sítě VPN, musíte vytvořit podsíť brány. Další informace o [aspekty rozsah konkrétní adresu podsítě brány](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Po přidání hello podsítě pro určité podmínky, můžete změnit rozsah adres hello. jak zjistit, toochange rozsah adres podsítě, toolearn [změnit nastavení podsítě](#change-subnet) v tomto článku.
    - **Skupina zabezpečení sítě**: Volitelně můžete přidružit existující skupinu zabezpečení sítě hello podsíť toocontrol příchozí a odchozí síťový provoz filtrování pro podsíť hello. Hello skupinu zabezpečení sítě, musí existovat v hello stejné předplatné a umístění jako virtuální síť hello. Je také nutné vytvořit pomocí modelu nasazení Resource Manager hello. toolearn Další informace o tom, jak toocreate skupinu zabezpečení sítě najdete v části [skupin zabezpečení sítě](virtual-networks-create-nsg-arm-pportal.md).
    - **Směrovací tabulka**: Volitelně můžete přidružit existující směrovací tabulku hello podsíť toocontrol síťový provoz směrování tooother sítě. Hello směrovací tabulka musí existovat v hello stejné předplatné a umístění jako virtuální síť hello. Je také nutné vytvořit pomocí modelu nasazení Resource Manager hello. Další informace o toolearn, jak zjistit, toocreate směrovací tabulky, [trasy definované uživatelem](virtual-network-create-udr-arm-ps.md).
    - **Uživatelé**: podsíť toohello přístup můžete řídit pomocí předdefinované role nebo vlastní role. toolearn Další informace o přiřazení role a uživatelé tooaccess hello podsíť, najdete v části [použít roli přiřazení toomanage přístup tooyour Azure prostředky](../active-directory/role-based-access-control-configure.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
7. Klikněte na tlačítko tooadd hello podsíť toohello virtuální síť, kterou jste vybrali, **OK**.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[Vytvoření az podsíti virtuální sítě](/cli/azure/network/vnet/subnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nové AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json), [přidat AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet"></a>Změňte nastavení podsítě

Skupiny zabezpečení sítě, směrovací tabulky a podsíť tooa přístupu uživatele můžete změnit pomocí správy prostředků, které jsou v podsíti. toolearn o těchto nastaveních v [přidat podsíť](#create-subnet), podívejte se na krok 6. Pokud chcete toochange hello adresní prostor podsítě, musíte nejprve odstranit všechny prostředky, které jsou v podsíti hello. kroky Hello trvat toodelete prostředek lišit v závislosti na hello prostředků. jak toodelete prostředky, které jsou v podsítích, čtení hello dokumentace pro každý prostředek, zadejte, které chcete toodelete toolearn. toochange hello rozsah adres podsítě:

1. Přihlaste se toohello [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě hello (minimálně) pro vaše předplatné. najdete v části Další informace o přiřazení role a oprávnění tooaccounts toolearn [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portálu vyhledávacího pole zadejte **virtuální sítě**. Ve výsledcích hledání hello, klikněte na tlačítko **virtuální sítě**.
3. Na hello **virtuální sítě** okně klikněte na tlačítko hello virtuální sítě, pro které chcete toochange rozsah adres podsítě.
4. Klikněte na tlačítko hello podsítě, pro které chcete rozsah adres toochange hello.
5. V okně podsítě hello, v hello **rozsahu adres** zadejte nový rozsah adres hello. Hello rozsahu musí být jedinečný v rámci hello adresního prostoru virtuální sítě hello. rozsah Hello se nesmí překrývat s další rozsahy adres podsítě v rámci virtuální sítě hello. Hello adresní prostor musí být určena pomocí zápisu směrování CIDR. Ve virtuální síti s 10.0.0.0/16 prostoru adres, můžete třeba definovat adresního prostoru podsítě 10.0.0.0/24. Hello nejmenší rozsah, které můžete zadat je /29, který poskytuje osm IP adresy pro podsíť hello. Azure rezervy hello první a poslední adres v každé podsíti pro protokol shoda. Tři další adresy jsou vyhrazené pro použití služby Azure. V důsledku toho podsíť s/29 rozsah adres má tři použitelných IP adresách. Pokud máte v plánu tooconnect bránu virtuální sítě tooa sítě VPN, musíte vytvořit podsíť brány. Další informace o [aspekty rozsah konkrétní adresu podsítě brány](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Rozsah adres hello můžete změnit po vytvoření hello podsítě pro určité podmínky. jak zjistit, toochange rozsah adres podsítě, toolearn [změnit nastavení podsítě](#change-subnet) v tomto článku.
6. Klikněte na **Uložit**.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[aktualizace az sítě vnet podsíť](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-subnet"></a>Odstranit podsíť

Podsíť může odstranit pouze v případě, že neexistují žádné prostředky v podsíti hello. Pokud v podsíti hello zdrojů, je nutné odstranit hello prostředky, které jsou v podsíti hello před odstraněním hello podsítě. kroky Hello trvat toodelete prostředek lišit v závislosti na hello prostředků. jak toodelete prostředky, které jsou v podsítích, čtení hello dokumentace pro každý prostředek, zadejte, které chcete toodelete toolearn. toodelete podsítě:

1. Přihlaste se toohello [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě hello (minimálně) pro vaše předplatné. najdete v části Další informace o přiřazení role a oprávnění tooaccounts toolearn [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Hello portálu vyhledávacího pole zadejte **virtuální sítě**. Ve výsledcích hledání hello, klikněte na tlačítko **virtuální sítě**.
3. Na hello **virtuální sítě** okně klikněte na tlačítko hello chcete toodelete podsíť z virtuální sítě.
4. Na hello virtuální sítě okno, v části **nastavení**, klikněte na tlačítko **podsítě**.
5. V seznamu hello podsítí, který se zobrazí v okně hello podsítě, klikněte pravým tlačítkem na hello podsítě můžete má toodelete, klikněte na tlačítko **odstranit**a potom klikněte na **Ano** toodelete hello podsítě.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[AZ síť vnet odstranit](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Odebrat AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Další kroky

toocreate virtuálního počítače v podsíti, najdete v části [vytvořit virtuální síť a nasazovat virtuální počítače v podsíti hello](virtual-network-get-started-vnet-subnet.md#create-vms).
