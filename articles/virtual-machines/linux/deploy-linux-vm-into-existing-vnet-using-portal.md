---
title: "aaaDeploy virtuální počítače s Linuxem do stávající sítě pomocí portálu Azure | Microsoft Docs"
description: "Nasazení virtuálního počítače s Linuxem do existující virtuální síť Azure pomocí portálu hello."
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
ms.openlocfilehash: 51272b8cdb1dc7f3fe768d2ebbf4ebe0d754cf19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-portal"></a>Jak toodeploy virtuální počítač s Linuxem do existující virtuální síť Azure s hello portálu Azure

Tento článek ukazuje, jak toodeploy virtuální počítač (VM) do existující virtuální síť (VNet). Azure prostředky jako skupin zabezpečení virtuální sítě a sítě by měl být statické a dlouhodobě prostředky, které jsou nasazeny zřídka. Po nasazený virtuální síť, můžete použít znovu konstantní redeployments bez jakékoli infrastruktury toohello má negativní vliv. Přemýšlíte o virtuální sítě, že je tradiční hardwaru síťový přepínač - nebude potřeba tooconfigure zcela nový hardware přepínač s každého nasazení.  

Pomocí správně nakonfigurovaných virtuální síť můžete pokračovat toodeploy nových serverů do ní opakovaně v několika, pokud existuje, změny během hello doby trvání hello virtuální sítě.

## <a name="create-hello-resource-group"></a>Vytvořte skupinu prostředků hello

Nejprve vytvořte tooorganize skupiny prostředků všechny objekty, které vytvoříte v tomto návodu. Další informace o skupinách prostředků Azure najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md)

![createResourceGroup](./media/deploy-linux-vm-into-existing-vnet-using-portal/createResourceGroup.png)


## <a name="create-hello-vnet"></a>Vytvoření hello virtuální sítě

V dalším kroku sestavení hello toolaunch virtuální sítě virtuálních počítačů do. Hello virtuální síť obsahuje jednu podsíť a je přidružená ke skupině zabezpečení sítě hello k této podsíti v pozdější fázi.

![createVNet](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNet.png)

## <a name="add-a-vnic-toohello-subnet"></a>Přidat podsíť toohello VNic

Virtuální síťové karty (VNics) jsou důležité, jako je můžete připojit toodifferent virtuálních počítačů. Tento přístup zajišťuje hello VNic jako statické prostředek hello virtuální počítače mohou být dočasné. Vytvoření adaptéru VNic a přidružte ji k hello podsíť vytvořená v předchozím kroku hello.

![createVNic](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNic.png)

## <a name="create-hello-network-security-group"></a>Vytvořit skupinu zabezpečení sítě hello

Skupiny zabezpečení Azure sítě jsou ekvivalentní tooa brány firewall hello síťové vrstvy. Další informace o skupinách zabezpečení sítě Azure, najdete v části [co je skupina zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md).

![createNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/createNSG.png)

## <a name="add-an-inbound-ssh-allow-rule"></a>Přidat pravidlo povolit příchozí SSH

Hello virtuálního počítače potřebuje přístup z hello Internetu, aby pravidlo povolující příchozí port 22 provoz toobe předána hello sítě tooport 22 na hello vytvoří virtuální počítač.

![createInboundSSH](./media/deploy-linux-vm-into-existing-vnet-using-portal/createInboundSSH.png)

## <a name="associate-hello-nsg-with-hello-subnet"></a>Přidružit hello NSG k podsíti hello

Hello virtuální síť a podsíť hello vytvořená přidružte skupinu zabezpečení sítě hello hello podsítě. Skupiny zabezpečení sítě lze přidružit celou podsíť nebo jednotlivé VNic. Brána firewall hello filtrování přenosů dat na úrovni podsítě hello všechny VNics a hello virtuálních počítačů v rámci podsítě hello jsou chráněny hello skupinu zabezpečení sítě. Hello jiných přístup je hello se skupina zabezpečení sítě spojené s pouze jednoho adaptéru VNic a chrání pouze jeden virtuální počítač.

![associateNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/associateNSG.png)


## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a>Nasazení hello virtuálních počítačů do hello virtuální sítě a NSG

Hello virtuálního počítače s Linuxem pomocí hello portálu Azure, je nasazené toohello existující skupiny prostředků Azure, virtuální síť, podsíť a VNic.

![createVM](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVM.png)

Pomocí hello portálu toochoose stávajícími prostředky, dáte pokyn Azure toodeploy hello uvnitř hello stávající sítě virtuálních počítačů. Jakmile nasazených virtuálních sítí a podsítí, může být ponecháno jako statické nebo trvalé prostředky uvnitř vaší oblasti Azure.  

## <a name="next-steps"></a>Další kroky

* [Použijte toocreate šablony Azure Resource Manager konkrétní nasazení](../windows/cli-deploy-templates.md)
* [Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md).
* [Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablony](create-ssh-secured-vm-from-template.md)
