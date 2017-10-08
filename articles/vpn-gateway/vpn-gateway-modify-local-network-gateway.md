---
title: "Úprava předpon adres IP brány místní sítě hello a adresa IP brány VPN hello | Azure | Prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede procesem Změna předpony IP adresy pro bránu místní sítě pomocí prostředí PowerShell"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c7db48f-d09a-44e7-836f-1fb6930389df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: 1353598b39a97fae9bdb424505a5ae2560482654
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="modify-local-network-gateway-settings-using-powershell"></a>Úprava nastavení místní síťové brány pomocí PowerShellu

Někdy hello nastavení pro bránu místní sítě AddressPrefix nebo GatewayIPAddress změnit. Tento článek ukazuje, jak toomodify nastavení brány místní sítě. Můžete také upravit tato nastavení pomocí různých metod výběrem jinou možnost z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before"></a>Než začnete

Nainstalujte nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) Další informace o instalaci rutin prostředí PowerShell hello.

## <a name="ipaddprefix"></a>Úprava předpon adres IP

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="gwip"></a>Upravit IP adresu brány hello

[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Další kroky

Můžete ověřit připojení brány. V tématu [ověření připojení brány](vpn-gateway-verify-connection-resource-manager.md).