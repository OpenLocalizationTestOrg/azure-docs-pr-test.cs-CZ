---
title: "Ověření připojení VPN Gateway | Microsoft Docs"
description: "Tento článek ukazuje, jak ověřit připojení VPN brány virtuální sítě."
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
ms.openlocfilehash: b2d702ecdd5e1fca342e7c84c6e75339097f0bcd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="verify-a-vpn-gateway-connection"></a><span data-ttu-id="6dfbc-103">Ověření připojení brány sítě VPN</span><span class="sxs-lookup"><span data-stu-id="6dfbc-103">Verify a VPN Gateway connection</span></span>

<span data-ttu-id="6dfbc-104">Tento článek ukazuje, jak ověřit připojení k bráně VPN pro classic a modelech nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6dfbc-104">This article shows you how to verify a VPN gateway connection for both the classic and Resource Manager deployment models.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="6dfbc-105">portál Azure</span><span class="sxs-lookup"><span data-stu-id="6dfbc-105">Azure portal</span></span>

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a><span data-ttu-id="6dfbc-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6dfbc-106">PowerShell</span></span>

<span data-ttu-id="6dfbc-107">Chcete-li ověřit připojení k bráně VPN pro model nasazení Resource Manager pomocí prostředí PowerShell, nainstalujte nejnovější verzi [rutiny Powershellu pro Azure Resource Manager](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6dfbc-107">To verify a VPN gateway connection for the Resource Manager deployment model using PowerShell, install the latest version of the [Azure Resource Manager PowerShell cmdlets](/powershell/azure/overview).</span></span>

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a><span data-ttu-id="6dfbc-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6dfbc-108">Azure CLI</span></span>

<span data-ttu-id="6dfbc-109">Chcete-li ověřit připojení k bráně VPN pro model nasazení Resource Manager pomocí rozhraní příkazového řádku Azure, nainstalujte nejnovější verzi [rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="6dfbc-109">To verify a VPN gateway connection for the Resource Manager deployment model using Azure CLI, install the latest version of the [CLI commands](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 or later).</span></span>

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a><span data-ttu-id="6dfbc-110">Portál Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="6dfbc-110">Azure portal (classic)</span></span>

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a><span data-ttu-id="6dfbc-111">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="6dfbc-111">PowerShell (classic)</span></span>

<span data-ttu-id="6dfbc-112">Chcete-li ověřit připojení brány VPN pro model nasazení classic pomocí prostředí PowerShell, nainstalujte nejnovější verzi rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6dfbc-112">To verify your VPN gateway connection for the classic deployment model using PowerShell, install the latest versions of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="6dfbc-113">Nezapomeňte si stáhněte a nainstalujte [Service Management](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) modulu.</span><span class="sxs-lookup"><span data-stu-id="6dfbc-113">Be sure to download and install the [Service Management](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) module.</span></span> <span data-ttu-id="6dfbc-114">'Add-AzureAccount, použijte k přihlášení do modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="6dfbc-114">Use 'Add-AzureAccount' to log in to the classic deployment model.</span></span>

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a><span data-ttu-id="6dfbc-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6dfbc-115">Next steps</span></span>

* <span data-ttu-id="6dfbc-116">Do virtuálních sítí můžete přidat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="6dfbc-116">You can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="6dfbc-117">Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6dfbc-117">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>