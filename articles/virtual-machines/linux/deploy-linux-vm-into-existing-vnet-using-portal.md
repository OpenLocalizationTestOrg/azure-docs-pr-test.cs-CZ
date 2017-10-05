---
title: "Nasadit virtuální počítače s Linuxem do stávající sítě pomocí portálu Azure | Microsoft Docs"
description: "Nasazení virtuálního počítače s Linuxem do existující virtuální síť Azure pomocí portálu."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 964c0fc41773b50a9fbe476df47460484c2ada66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-the-azure-portal"></a>Jak nasadit virtuální počítač s Linuxem do existující virtuální síť Azure pomocí portálu Azure

Tento článek ukazuje, jak nasadit virtuální počítač (VM) do existující virtuální síť (VNet). Azure prostředky jako skupin zabezpečení virtuální sítě a sítě by měl být statické a dlouhodobě prostředky, které jsou nasazeny zřídka. Po nasazený virtuální síť, můžete použít znovu konstantní redeployments bez žádné negativní ovlivňuje infrastruktury. Přemýšlíte o virtuální sítě, že je tradiční hardwaru síťový přepínač - nebude potřeba ke konfiguraci nové hardwarové přepínače s každého nasazení.  

Pomocí správně nakonfigurovaných virtuální síť můžete nasadit nové servery do ní opakovaně s několika, pokud existuje, změny požadované za dobu životnosti virtuální sítě.

## <a name="create-the-resource-group"></a>Vytvoření skupiny prostředků

Nejprve vytvořte skupinu prostředků pro všechny objekty, které vytvoříte v tomto návodu uspořádání. Další informace o skupinách prostředků Azure najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md)

![createResourceGroup](./media/deploy-linux-vm-into-existing-vnet-using-portal/createResourceGroup.png)


## <a name="create-the-vnet"></a>Vytvoření sítě VNet

V dalším kroku sestavení ke spuštění virtuálních počítačů do virtuální sítě. Virtuální sítě obsahuje jednu podsíť a je přidružen k této podsíti v pozdější fázi skupinu zabezpečení sítě.

![createVNet](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNet.png)

## <a name="add-a-vnic-to-the-subnet"></a>Přidání adaptéru VNic k podsíti

Virtuální síťové karty (VNics) jsou důležité, protože je můžete připojit k jiné virtuální počítače. Tento přístup zajišťuje VNic jako statické prostředek při virtuálních počítačů může být dočasné. Vytvoření adaptéru VNic a přidružte ji k podsíť vytvořená v předchozím kroku.

![createVNic](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNic.png)

## <a name="create-the-network-security-group"></a>Vytvořit skupinu zabezpečení sítě

Skupiny zabezpečení Azure sítě jsou ekvivalentní brány firewall síťové vrstvy. Další informace o skupinách zabezpečení sítě Azure, najdete v části [co je skupina zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md).

![createNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/createNSG.png)

## <a name="add-an-inbound-ssh-allow-rule"></a>Přidat pravidlo povolit příchozí SSH

Virtuální počítač potřebuje přístup z Internetu, takže se vytvoří pravidlo povolující příchozí port 22 provoz předávané port 22 přes síť, ve virtuálním počítači.

![createInboundSSH](./media/deploy-linux-vm-into-existing-vnet-using-portal/createInboundSSH.png)

## <a name="associate-the-nsg-with-the-subnet"></a>Přidružení NSG k podsíti

Síť VNet a podsíť vytvořená přidružte skupinu zabezpečení sítě s podsítí. Skupiny zabezpečení sítě lze přidružit celou podsíť nebo jednotlivé VNic. Brána firewall, filtrování přenosů dat na úrovni podsítě všechny VNics a virtuální počítače v rámci podsítě jsou chráněny skupinu zabezpečení sítě. Ostatní přístup je skupina zabezpečení sítě bylo možné přidružit jen jedné adaptéru VNic a ochranu právě jeden virtuální počítač.

![associateNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/associateNSG.png)


## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a>Virtuální počítač nasaďte do virtuální sítě a NSG

Pomocí portálu Azure, virtuálního počítače s Linuxem se nasadí do existující skupiny prostředků Azure, virtuální síť, podsíť a VNic.

![createVM](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVM.png)

Zvolit existující prostředky prostřednictvím portálu, požádejte Azure k nasazení virtuálních počítačů uvnitř existující síť. Jakmile nasazených virtuálních sítí a podsítí, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.  

## <a name="next-steps"></a>Další kroky

* [Vytvoření konkrétního nasazení pomocí šablony Azure Resource Manageru](../windows/cli-deploy-templates.md)
* [Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md).
* [Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony](create-ssh-secured-vm-from-template.md)
