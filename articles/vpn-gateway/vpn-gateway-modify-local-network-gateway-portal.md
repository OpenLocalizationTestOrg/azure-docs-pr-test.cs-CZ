---
title: "Úprava předpon adres IP brány místní sítě a adresa brány VPN | Azure | Portál | Microsoft Docs"
description: "Tento článek vás provede změna předpony IP adresy pro bránu místní sítě pomocí portálu Azure."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: bdd6f90fe97408bd45a72adf58bfdc190207de46
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a><span data-ttu-id="6438d-103">Upravit nastavení brány místní sítě pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6438d-103">Modify local network gateway settings using the Azure portal</span></span>

<span data-ttu-id="6438d-104">Někdy se změnit nastavení pro bránu místní sítě AddressPrefix nebo GatewayIPAddress.</span><span class="sxs-lookup"><span data-stu-id="6438d-104">Sometimes the settings for your local network gateway AddressPrefix or GatewayIPAddress change.</span></span> <span data-ttu-id="6438d-105">Tento článek ukazuje, jak upravit nastavení brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="6438d-105">This article shows you how to modify your local network gateway settings.</span></span> <span data-ttu-id="6438d-106">Můžete také upravit tato nastavení pomocí různých metod výběrem jinou možnost z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="6438d-106">You can also modify these settings using a different method by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6438d-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6438d-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="6438d-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6438d-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="6438d-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6438d-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <span data-ttu-id="6438d-110"><a name="ipaddprefix"></a>Úprava předpon adres IP</span><span class="sxs-lookup"><span data-stu-id="6438d-110"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

<span data-ttu-id="6438d-111">Když upravíte předpony IP adres, kroky, které budete postupovat podle závisí na tom, jestli má bránu místní sítě připojení.</span><span class="sxs-lookup"><span data-stu-id="6438d-111">When you modify IP address prefixes, the steps you follow depend on whether your local network gateway has a connection.</span></span>

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <span data-ttu-id="6438d-112"><a name="gwip"></a>Upravit IP adresu brány</span><span class="sxs-lookup"><span data-stu-id="6438d-112"><a name="gwip"></a>Modify the gateway IP address</span></span>

<span data-ttu-id="6438d-113">Pokud zařízení VPN, ke kterému se chcete připojit, změnilo svou veřejnou IP adresu, musíte upravit bránu místní sítě, aby odrážela tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="6438d-113">If the VPN device that you want to connect to has changed its public IP address, you need to modify the local network gateway to reflect that change.</span></span> <span data-ttu-id="6438d-114">Když změníte veřejnou IP adresu, kroky, které budete postupovat podle závisí na tom, jestli má bránu místní sítě připojení.</span><span class="sxs-lookup"><span data-stu-id="6438d-114">When you change the public IP address, the steps you follow depend on whether your local network gateway has a connection.</span></span>

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a><span data-ttu-id="6438d-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6438d-115">Next steps</span></span>

<span data-ttu-id="6438d-116">Můžete ověřit připojení brány.</span><span class="sxs-lookup"><span data-stu-id="6438d-116">You can verify your gateway connection.</span></span> <span data-ttu-id="6438d-117">V tématu [ověření připojení brány](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="6438d-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>