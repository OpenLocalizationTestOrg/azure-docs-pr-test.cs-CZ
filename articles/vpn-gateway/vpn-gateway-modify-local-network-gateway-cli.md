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
# <a name="modify-local-network-gateway-settings-using-hello-azure-cli"></a><span data-ttu-id="50af9-103">Upravit nastavení brány místní sítě pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="50af9-103">Modify local network gateway settings using hello Azure CLI</span></span>

<span data-ttu-id="50af9-104">Někdy hello nastavení pro bránu místní sítě předpony adres nebo IP adresu brány změnit.</span><span class="sxs-lookup"><span data-stu-id="50af9-104">Sometimes hello settings for your local network gateway Address Prefix or Gateway IP Address change.</span></span> <span data-ttu-id="50af9-105">Tento článek ukazuje, jak toomodify nastavení brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="50af9-105">This article shows you how toomodify your local network gateway settings.</span></span> <span data-ttu-id="50af9-106">Můžete také upravit tato nastavení pomocí různých metod výběrem jinou možnost z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="50af9-106">You can also modify these settings using a different method by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="50af9-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="50af9-107">Azure portal</span></span>](vpn-gateway-modify-local-network-gateway-portal.md)
> * [<span data-ttu-id="50af9-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="50af9-108">PowerShell</span></span>](vpn-gateway-modify-local-network-gateway.md)
> * [<span data-ttu-id="50af9-109">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="50af9-109">Azure CLI</span></span>](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <span data-ttu-id="50af9-110"><a name="before"></a>Než začnete</span><span class="sxs-lookup"><span data-stu-id="50af9-110"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="50af9-111">Nainstalujte nejnovější verzi hello hello rozhraní příkazového řádku (2.0 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="50af9-111">Install hello latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="50af9-112">Informace o instalaci hello rozhraní příkazového řádku najdete v tématu [nainstalovat Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="50af9-112">For information about installing hello CLI commands, see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <span data-ttu-id="50af9-113"><a name="ipaddprefix"></a>Úprava předpon adres IP</span><span class="sxs-lookup"><span data-stu-id="50af9-113"><a name="ipaddprefix"></a>Modify IP address prefixes</span></span>

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <span data-ttu-id="50af9-114"><a name="gwip"></a>Upravit IP adresu brány hello</span><span class="sxs-lookup"><span data-stu-id="50af9-114"><a name="gwip"></a>Modify hello gateway IP address</span></span>

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a><span data-ttu-id="50af9-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="50af9-115">Next steps</span></span>

<span data-ttu-id="50af9-116">Můžete ověřit připojení brány.</span><span class="sxs-lookup"><span data-stu-id="50af9-116">You can verify your gateway connection.</span></span> <span data-ttu-id="50af9-117">V tématu [ověření připojení brány](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="50af9-117">See [Verify a gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>

