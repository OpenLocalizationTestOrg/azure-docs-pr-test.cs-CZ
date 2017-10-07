---
title: "aaaSet až koncových bodů na klasické virtuální počítač s Windows | Microsoft Docs"
description: "Přečtěte si informace tooset až koncové body pro virtuální počítač Windows v hello Azure classic portálu tooallow komunikaci s virtuálního počítače s Windows v Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 8afc21c2-d3fb-43a3-acce-aa06be448bb6
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: e817076f16d3a245a8d19add7b2f2cf5e3baa17e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Jak tooset až koncových bodů na klasické virtuální počítač Windows v Azure
Všechny systémy Windows, které virtuální počítače vytvořené v Azure pomocí modelu nasazení classic hello automaticky komunikovat přes kanál s jinými virtuálními počítači v privátní síti hello stejné cloudové služby nebo virtuální sítě. Počítače na hello internetové nebo jiné virtuální sítě však vyžadují koncové body toodirect hello příchozích síťových přenosů tooa virtuálního počítače. Tento článek je také k dispozici pro [virtuální počítače s Linuxem](../../linux/classic/setup-endpoints.md).

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

V hello **Resource Manager** modelu nasazení koncové body jsou konfigurováni pomocí **skupiny zabezpečení sítě (Nsg)**. Další informace najdete v tématu [povolit externí přístup tooyour virtuální počítač pomocí portálu Azure hello](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Při vytváření virtuálního počítače s Windows v hello portálu Azure, běžné koncové body, jako jsou ty, které pro vzdálenou plochu a vzdálenou komunikaci Windows PowerShell jsou obvykle vám vytvoří automaticky. Při vytváření hello virtuálního počítače nebo později podle potřeby můžete nakonfigurovat další koncové body.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Další kroky
* toouse tooset rutiny prostředí Azure PowerShell až koncový bod virtuálního počítače najdete v části [přidat AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).
* toouse toomanage rutiny prostředí Azure PowerShell ACL na koncový bod, najdete v části [seznamy Správa řízení přístupu (ACL) pro koncové body pomocí prostředí PowerShell](../../../virtual-network/virtual-networks-acl-powershell.md).
* Pokud jste vytvořili virtuální počítač v modelu nasazení Resource Manager hello, můžete použít Azure PowerShell příliš[vytvoření skupin zabezpečení sítě](../../../virtual-network/virtual-networks-create-nsg-arm-ps.md) toohello toocontrol přenosy virtuálních počítačů.
