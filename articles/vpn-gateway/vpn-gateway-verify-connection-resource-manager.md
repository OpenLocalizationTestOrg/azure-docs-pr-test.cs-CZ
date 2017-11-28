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
# <a name="verify-a-vpn-gateway-connection"></a><span data-ttu-id="67e13-103">Ověření připojení brány sítě VPN</span><span class="sxs-lookup"><span data-stu-id="67e13-103">Verify a VPN Gateway connection</span></span>

<span data-ttu-id="67e13-104">Tento článek ukazuje, jak tooverify připojení k bráně VPN pro hello classic a modelech nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="67e13-104">This article shows you how tooverify a VPN gateway connection for both hello classic and Resource Manager deployment models.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="67e13-105">portál Azure</span><span class="sxs-lookup"><span data-stu-id="67e13-105">Azure portal</span></span>

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a><span data-ttu-id="67e13-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="67e13-106">PowerShell</span></span>

<span data-ttu-id="67e13-107">model pomocí prostředí PowerShell tooverify připojení k bráně VPN pro hello nasazení Resource Manager, nainstalujte nejnovější verzi hello hello [rutiny Powershellu pro Azure Resource Manager](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="67e13-107">tooverify a VPN gateway connection for hello Resource Manager deployment model using PowerShell, install hello latest version of hello [Azure Resource Manager PowerShell cmdlets](/powershell/azure/overview).</span></span>

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a><span data-ttu-id="67e13-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="67e13-108">Azure CLI</span></span>

<span data-ttu-id="67e13-109">tooverify připojení k bráně VPN pro hello nasazení Resource Manager modelu pomocí rozhraní příkazového řádku Azure, nainstalujte nejnovější verzi hello hello [rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="67e13-109">tooverify a VPN gateway connection for hello Resource Manager deployment model using Azure CLI, install hello latest version of hello [CLI commands](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 or later).</span></span>

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a><span data-ttu-id="67e13-110">Portál Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="67e13-110">Azure portal (classic)</span></span>

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a><span data-ttu-id="67e13-111">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="67e13-111">PowerShell (classic)</span></span>

<span data-ttu-id="67e13-112">tooverify připojení brány VPN pro nasazení classic hello modelu pomocí prostředí PowerShell, nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello.</span><span class="sxs-lookup"><span data-stu-id="67e13-112">tooverify your VPN gateway connection for hello classic deployment model using PowerShell, install hello latest versions of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="67e13-113">Se, zda text hello toodownload a nainstalujte [Service Management](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) modulu.</span><span class="sxs-lookup"><span data-stu-id="67e13-113">Be sure toodownload and install hello [Service Management](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) module.</span></span> <span data-ttu-id="67e13-114">V modelu nasazení classic toohello použijte toolog 'Add-AzureAccount'.</span><span class="sxs-lookup"><span data-stu-id="67e13-114">Use 'Add-AzureAccount' toolog in toohello classic deployment model.</span></span>

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a><span data-ttu-id="67e13-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67e13-115">Next steps</span></span>

* <span data-ttu-id="67e13-116">Můžete přidat virtuální počítače tooyour virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="67e13-116">You can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="67e13-117">Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="67e13-117">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>
