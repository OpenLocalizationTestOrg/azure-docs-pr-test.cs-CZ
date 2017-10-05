---
title: "Připojení k virtuálnímu počítači s Windows Serverem | Dokumentace Microsoftu"
description: "Zjistěte, jak se připojit a přihlásit k virtuálnímu počítači s Windows pomocí portálu Azure a modelu nasazení s využitím Resource Manageru."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/01/2017
ms.author: cynthn
ms.openlocfilehash: 88431377a36d5bc36220c630f0c8d4a46ab4a434
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a><span data-ttu-id="95924-103">Jak se připojit a přihlásit k virtuálnímu počítači s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="95924-103">How to connect and log on to an Azure virtual machine running Windows</span></span>
<span data-ttu-id="95924-104">Pomocí tlačítka **Připojit** na webu Azure Portal spustíte z počítače s Windows relaci Vzdálené plochy (protokol RDP).</span><span class="sxs-lookup"><span data-stu-id="95924-104">You'll use the **Connect** button in the Azure portal to start a Remote Desktop (RDP) session from a Windows desktop.</span></span> <span data-ttu-id="95924-105">Nejdřív se k virtuálnímu počítači připojíte a pak se k němu přihlásíte.</span><span class="sxs-lookup"><span data-stu-id="95924-105">First you connect to the virtual machine, then you log on.</span></span>

<span data-ttu-id="95924-106">Pokud se pokoušíte připojit k virtuálnímu počítači s Windows z Macu, budete muset nainstalovat klienta protokolu RDP pro Mac jako [Vzdálená plocha Microsoft](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).</span><span class="sxs-lookup"><span data-stu-id="95924-106">If you are trying to connect to a Windows VM from a Mac, you need to install an RDP client for Mac like [Microsoft Remote Desktop](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).</span></span>

## <a name="connect-to-the-virtual-machine"></a><span data-ttu-id="95924-107">Připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="95924-107">Connect to the virtual machine</span></span>
1. <span data-ttu-id="95924-108">Pokud jste to ještě neudělali, přihlaste se k [Portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="95924-108">If you haven't already done so, sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="95924-109">V nabídce centra klikněte na **Virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="95924-109">On the Hub menu, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="95924-110">Ze seznamu vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="95924-110">Select the virtual machine from the list.</span></span>
4. <span data-ttu-id="95924-111">V okně pro virtuální počítač klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="95924-111">On the blade for the virtual machine, click **Connect**.</span></span>
   
    ![Snímek obrazovky Portálu Azure znázorňující způsob připojení k virtuálnímu počítači](./media/connect-logon/connect.png)
   
   > [!TIP]
   > <span data-ttu-id="95924-113">Pokud je tlačítko **Připojit** na portálu zašedlé a nejste připojení k Azure přes [Express Route](../../expressroute/expressroute-introduction.md) nebo připojení [S2S (Site-to-site) VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), musíte před použitím protokolu RDP vytvořit veřejnou IP adresu a přiřadit ji svému virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="95924-113">If the **Connect** button in the portal is greyed out and you are not connected to Azure via an [Express Route](../../expressroute/expressroute-introduction.md) or [Site-to-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) connection, you need to create and assign your VM a public IP address before you can use RDP.</span></span> <span data-ttu-id="95924-114">Podle potřeby si můžete přečíst další informace o [veřejných IP adresách v Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span><span class="sxs-lookup"><span data-stu-id="95924-114">You can read more about [public IP addresses in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span></span>
   > 
   > 

## <a name="log-on-to-the-virtual-machine"></a><span data-ttu-id="95924-115">Přihlášení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="95924-115">Log on to the virtual machine</span></span>
[!INCLUDE [virtual-machines-log-on-win-server](../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a><span data-ttu-id="95924-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95924-116">Next steps</span></span>
<span data-ttu-id="95924-117">Pokud při připojování nastanou problémy, přečtěte si článek [Řešení problémů s připojením ke Vzdálené ploše](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="95924-117">If you run into trouble when you try to connect, see [Troubleshoot Remote Desktop connections](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="95924-118">Tento článek vás provede diagnostikou a řešením běžných problémů.</span><span class="sxs-lookup"><span data-stu-id="95924-118">This article walks you through diagnosing and resolving common problems.</span></span>

