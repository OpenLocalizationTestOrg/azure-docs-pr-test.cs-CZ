---
title: "Úprava předpon adres IP brány místní sítě a adresa brány VPN | Azure | Prostředí PowerShell | Microsoft Docs"
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
ms.openlocfilehash: 2a095b96a8c352abeca72640d37c0d629b447763
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="modify-local-network-gateway-settings-using-powershell"></a><span data-ttu-id="2ad1c-103">Úprava nastavení místní síťové brány pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="2ad1c-103">Modify local network gateway settings using PowerShell</span></span>

<span data-ttu-id="2ad1c-104">Někdy se změnit nastavení pro bránu místní sítě AddressPrefix nebo GatewayIPAddress.</span><span class="sxs-lookup"><span data-stu-id="2ad1c-104">Sometimes the settings for your local network gateway AddressPrefix or GatewayIPAddress change.</span></span> <span data-ttu-id="2ad1c-105">Tento článek ukazuje, jak upravit nastavení brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="2ad1c-105">This article shows you how to modify your local network gateway settings.</span></span> <span data-ttu-id="2ad1c-106">Můžete také upravit tato nastavení pomocí různých metod výběrem jinou možnost z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="2ad1c-106">You can also modify these settings using a different method by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2ad1c-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2ad1c-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="2ad1c-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2ad1c-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="2ad1c-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2ad1c-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <span data-ttu-id="2ad1c-110"><a name="before"></a>Než začnete</span><span class="sxs-lookup"><span data-stu-id="2ad1c-110"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="2ad1c-111">Nainstalujte nejnovější verzi rutin PowerShellu pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="2ad1c-111">Install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="2ad1c-112">Další informace o instalaci rutin prostředí PowerShell najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="2ad1c-112">See [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for more information about installing the PowerShell cmdlets.</span></span>

## <span data-ttu-id="2ad1c-113"><a name="ipaddprefix"></a>Úprava předpon adres IP</span><span class="sxs-lookup"><span data-stu-id="2ad1c-113"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <span data-ttu-id="2ad1c-114"><a name="gwip"></a>Upravit IP adresu brány</span><span class="sxs-lookup"><span data-stu-id="2ad1c-114"><a name="gwip"></a>Modify the gateway IP address</span></span>

[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a><span data-ttu-id="2ad1c-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2ad1c-115">Next steps</span></span>

<span data-ttu-id="2ad1c-116">Můžete ověřit připojení brány.</span><span class="sxs-lookup"><span data-stu-id="2ad1c-116">You can verify your gateway connection.</span></span> <span data-ttu-id="2ad1c-117">V tématu [ověření připojení brány](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="2ad1c-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>