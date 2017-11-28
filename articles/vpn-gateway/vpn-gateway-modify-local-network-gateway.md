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
# <a name="modify-local-network-gateway-settings-using-powershell"></a><span data-ttu-id="b8c9e-103">Úprava nastavení místní síťové brány pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="b8c9e-103">Modify local network gateway settings using PowerShell</span></span>

<span data-ttu-id="b8c9e-104">Někdy hello nastavení pro bránu místní sítě AddressPrefix nebo GatewayIPAddress změnit.</span><span class="sxs-lookup"><span data-stu-id="b8c9e-104">Sometimes hello settings for your local network gateway AddressPrefix or GatewayIPAddress change.</span></span> <span data-ttu-id="b8c9e-105">Tento článek ukazuje, jak toomodify nastavení brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="b8c9e-105">This article shows you how toomodify your local network gateway settings.</span></span> <span data-ttu-id="b8c9e-106">Můžete také upravit tato nastavení pomocí různých metod výběrem jinou možnost z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="b8c9e-106">You can also modify these settings using a different method by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b8c9e-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b8c9e-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="b8c9e-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b8c9e-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="b8c9e-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b8c9e-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <span data-ttu-id="b8c9e-110"><a name="before"></a>Než začnete</span><span class="sxs-lookup"><span data-stu-id="b8c9e-110"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="b8c9e-111">Nainstalujte nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello.</span><span class="sxs-lookup"><span data-stu-id="b8c9e-111">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="b8c9e-112">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) Další informace o instalaci rutin prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="b8c9e-112">See [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for more information about installing hello PowerShell cmdlets.</span></span>

## <span data-ttu-id="b8c9e-113"><a name="ipaddprefix"></a>Úprava předpon adres IP</span><span class="sxs-lookup"><span data-stu-id="b8c9e-113"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <span data-ttu-id="b8c9e-114"><a name="gwip"></a>Upravit IP adresu brány hello</span><span class="sxs-lookup"><span data-stu-id="b8c9e-114"><a name="gwip"></a>Modify hello gateway IP address</span></span>

[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a><span data-ttu-id="b8c9e-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8c9e-115">Next steps</span></span>

<span data-ttu-id="b8c9e-116">Můžete ověřit připojení brány.</span><span class="sxs-lookup"><span data-stu-id="b8c9e-116">You can verify your gateway connection.</span></span> <span data-ttu-id="b8c9e-117">V tématu [ověření připojení brány](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b8c9e-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>