---
title: "Úprava předpon adres IP brány místní sítě hello a adresa IP brány VPN hello | Azure | Portál | Microsoft Docs"
description: "Tento článek vás provede změna předpony IP adresy pro bránu místní sítě pomocí hello portálu Azure."
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
ms.openlocfilehash: 001df7b748ccc234d87aab3501a4f0e4ddfe60f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="modify-local-network-gateway-settings-using-hello-azure-portal"></a><span data-ttu-id="40c07-103">Upravit nastavení brány místní sítě pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="40c07-103">Modify local network gateway settings using hello Azure portal</span></span>

<span data-ttu-id="40c07-104">Někdy hello nastavení pro bránu místní sítě AddressPrefix nebo GatewayIPAddress změnit.</span><span class="sxs-lookup"><span data-stu-id="40c07-104">Sometimes hello settings for your local network gateway AddressPrefix or GatewayIPAddress change.</span></span> <span data-ttu-id="40c07-105">Tento článek ukazuje, jak toomodify nastavení brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="40c07-105">This article shows you how toomodify your local network gateway settings.</span></span> <span data-ttu-id="40c07-106">Můžete také upravit tato nastavení pomocí různých metod výběrem jinou možnost z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="40c07-106">You can also modify these settings using a different method by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="40c07-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="40c07-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="40c07-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="40c07-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="40c07-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="40c07-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <span data-ttu-id="40c07-110"><a name="ipaddprefix"></a>Úprava předpon adres IP</span><span class="sxs-lookup"><span data-stu-id="40c07-110"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

<span data-ttu-id="40c07-111">Když upravíte předpony IP adres, hello kroky, které budete postupovat podle závisí na tom, jestli má bránu místní sítě připojení.</span><span class="sxs-lookup"><span data-stu-id="40c07-111">When you modify IP address prefixes, hello steps you follow depend on whether your local network gateway has a connection.</span></span>

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <span data-ttu-id="40c07-112"><a name="gwip"></a>Upravit IP adresu brány hello</span><span class="sxs-lookup"><span data-stu-id="40c07-112"><a name="gwip"></a>Modify hello gateway IP address</span></span>

<span data-ttu-id="40c07-113">Pokud hello zařízení VPN, které chcete tooconnect toohas změnit jeho veřejnou IP adresu, musíte toomodify hello místní síťové brány tooreflect, který změnit.</span><span class="sxs-lookup"><span data-stu-id="40c07-113">If hello VPN device that you want tooconnect toohas changed its public IP address, you need toomodify hello local network gateway tooreflect that change.</span></span> <span data-ttu-id="40c07-114">Při změně hello veřejnou IP adresu, budete postupovat podle kroků hello závisí na tom, jestli má bránu místní sítě připojení.</span><span class="sxs-lookup"><span data-stu-id="40c07-114">When you change hello public IP address, hello steps you follow depend on whether your local network gateway has a connection.</span></span>

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a><span data-ttu-id="40c07-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="40c07-115">Next steps</span></span>

<span data-ttu-id="40c07-116">Můžete ověřit připojení brány.</span><span class="sxs-lookup"><span data-stu-id="40c07-116">You can verify your gateway connection.</span></span> <span data-ttu-id="40c07-117">V tématu [ověření připojení brány](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="40c07-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>