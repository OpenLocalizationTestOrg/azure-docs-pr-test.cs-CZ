---
title: "Úprava předpon adres IP brány místní sítě a adresa brány VPN | Azure | ROZHRANÍ PŘÍKAZOVÉHO ŘÁDKU | Microsoft Docs"
description: "Tento článek vás provede změna předpony IP adresy pro bránu místní sítě pomocí rozhraní příkazového řádku Azure."
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
ms.openlocfilehash: 7db1ad970ebb93d46d5a861f9a9b27bf121531a3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="modify-local-network-gateway-settings-using-the-azure-cli"></a><span data-ttu-id="534c4-103">Upravit nastavení brány místní sítě pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="534c4-103">Modify local network gateway settings using the Azure CLI</span></span>

<span data-ttu-id="534c4-104">Někdy se změnit nastavení pro bránu místní sítě předpony adres nebo IP adresu brány.</span><span class="sxs-lookup"><span data-stu-id="534c4-104">Sometimes the settings for your local network gateway Address Prefix or Gateway IP Address change.</span></span> <span data-ttu-id="534c4-105">Tento článek ukazuje, jak upravit nastavení brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="534c4-105">This article shows you how to modify your local network gateway settings.</span></span> <span data-ttu-id="534c4-106">Můžete také upravit tato nastavení pomocí různých metod výběrem jinou možnost z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="534c4-106">You can also modify these settings using a different method by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="534c4-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="534c4-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="534c4-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="534c4-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="534c4-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="534c4-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <span data-ttu-id="534c4-110"><a name="before"></a>Než začnete</span><span class="sxs-lookup"><span data-stu-id="534c4-110"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="534c4-111">Nainstalujte nejnovější verzi rozhraní příkazového řádku příkazy (2.0 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="534c4-111">Install the latest version of the CLI commands (2.0 or later).</span></span> <span data-ttu-id="534c4-112">Informace o instalaci příkazů rozhraní příkazového řádku najdete v tématu [Instalace Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="534c4-112">For information about installing the CLI commands, see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <span data-ttu-id="534c4-113"><a name="ipaddprefix"></a>Úprava předpon adres IP</span><span class="sxs-lookup"><span data-stu-id="534c4-113"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <span data-ttu-id="534c4-114"><a name="gwip"></a>Upravit IP adresu brány</span><span class="sxs-lookup"><span data-stu-id="534c4-114"><a name="gwip"></a>Modify the gateway IP address</span></span>

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a><span data-ttu-id="534c4-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="534c4-115">Next steps</span></span>

<span data-ttu-id="534c4-116">Můžete ověřit připojení brány.</span><span class="sxs-lookup"><span data-stu-id="534c4-116">You can verify your gateway connection.</span></span> <span data-ttu-id="534c4-117">V tématu [ověření připojení brány](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="534c4-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>

