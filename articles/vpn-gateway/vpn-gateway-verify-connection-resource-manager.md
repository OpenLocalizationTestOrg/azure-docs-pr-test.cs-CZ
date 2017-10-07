---
title: "aaaVerify připojení VPN Gateway | Microsoft Docs"
description: "Tento článek ukazuje, jak tooverify a virtuální síťové připojení brány VPN."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 7e3d1043-caa9-4472-96d3-832f4e2c91ee
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2017
ms.author: cherylmc
ms.openlocfilehash: 0d3da94a76b36251d629f82b1575328c7ac10b26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="verify-a-vpn-gateway-connection"></a>Ověření připojení brány sítě VPN

Tento článek ukazuje, jak tooverify připojení k bráně VPN pro hello classic a modelech nasazení Resource Manager.

## <a name="azure-portal"></a>portál Azure

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

model pomocí prostředí PowerShell tooverify připojení k bráně VPN pro hello nasazení Resource Manager, nainstalujte nejnovější verzi hello hello [rutiny Powershellu pro Azure Resource Manager](/powershell/azure/overview).

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure CLI

tooverify připojení k bráně VPN pro hello nasazení Resource Manager modelu pomocí rozhraní příkazového řádku Azure, nainstalujte nejnovější verzi hello hello [rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 nebo novější).

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a>Portál Azure (klasický)

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell (Classic)

tooverify připojení brány VPN pro nasazení classic hello modelu pomocí prostředí PowerShell, nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello. Se, zda text hello toodownload a nainstalujte [Service Management](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) modulu. V modelu nasazení classic toohello použijte toolog 'Add-AzureAccount'.

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Další kroky

* Můžete přidat virtuální počítače tooyour virtuální sítě. Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
