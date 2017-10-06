---
title: "Úprava předpon adres IP brány místní sítě hello a adresa IP brány VPN hello | Azure | ROZHRANÍ PŘÍKAZOVÉHO ŘÁDKU | Microsoft Docs"
description: "Tento článek vás provede změna předpony IP adresy pro bránu místní sítě pomocí rozhraní příkazového řádku Azure hello."
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
ms.openlocfilehash: 4b8bbf3e9d3d42ac2d12f87360fa0a4134c57a21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="modify-local-network-gateway-settings-using-hello-azure-cli"></a>Upravit nastavení brány místní sítě pomocí hello rozhraní příkazového řádku Azure

Někdy hello nastavení pro bránu místní sítě předpony adres nebo IP adresu brány změnit. Tento článek ukazuje, jak toomodify nastavení brány místní sítě. Můžete také upravit tato nastavení pomocí různých metod výběrem jinou možnost z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before"></a>Než začnete

Nainstalujte nejnovější verzi hello hello rozhraní příkazového řádku (2.0 nebo novější). Informace o instalaci hello rozhraní příkazového řádku najdete v tématu [nainstalovat Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="ipaddprefix"></a>Úprava předpon adres IP

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <a name="gwip"></a>Upravit IP adresu brány hello

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a>Další kroky

Můžete ověřit připojení brány. V tématu [ověření připojení brány](vpn-gateway-verify-connection-resource-manager.md).

