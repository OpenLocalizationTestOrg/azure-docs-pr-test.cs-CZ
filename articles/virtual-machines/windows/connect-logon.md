---
title: "aaaConnect tooa virtuálního počítače Windows serveru | Microsoft Docs"
description: "Zjistěte, jak tooconnect a přihlášení pomocí tooa virtuální počítač s Windows hello Azure portal a hello modelu nasazení Resource Manager."
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
ms.openlocfilehash: dc397f435ef165ae5af09f1d037ad3d520bb7ac3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconnect-and-log-on-tooan-azure-virtual-machine-running-windows"></a><span data-ttu-id="92ce2-103">Jak se tooconnect a protokol na tooan Azure virtuální počítač, systémem Windows</span><span class="sxs-lookup"><span data-stu-id="92ce2-103">How tooconnect and log on tooan Azure virtual machine running Windows</span></span>
<span data-ttu-id="92ce2-104">Budete používat hello **Connect** tlačítka na hello Azure portálu toostart relaci vzdálené plochy (RDP) z plochy Windows.</span><span class="sxs-lookup"><span data-stu-id="92ce2-104">You'll use hello **Connect** button in hello Azure portal toostart a Remote Desktop (RDP) session from a Windows desktop.</span></span> <span data-ttu-id="92ce2-105">Nejprve připojit toohello virtuálního počítače a potom přihlášení.</span><span class="sxs-lookup"><span data-stu-id="92ce2-105">First you connect toohello virtual machine, then you log on.</span></span>

<span data-ttu-id="92ce2-106">Pokud se pokoušíte tooconnect tooa Windows virtuální počítač z algoritmu Mac, je třeba tooinstall klientem RDP pro Mac, například [Vzdálená plocha od Microsoftu](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).</span><span class="sxs-lookup"><span data-stu-id="92ce2-106">If you are trying tooconnect tooa Windows VM from a Mac, you need tooinstall an RDP client for Mac like [Microsoft Remote Desktop](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).</span></span>

## <a name="connect-toohello-virtual-machine"></a><span data-ttu-id="92ce2-107">Připojit toohello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="92ce2-107">Connect toohello virtual machine</span></span>
1. <span data-ttu-id="92ce2-108">Pokud jste tak již neučinili, přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="92ce2-108">If you haven't already done so, sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="92ce2-109">V nabídce centra hello, klikněte na tlačítko **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="92ce2-109">On hello Hub menu, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="92ce2-110">Vyberte virtuální počítač hello hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="92ce2-110">Select hello virtual machine from hello list.</span></span>
4. <span data-ttu-id="92ce2-111">V okně hello hello virtuálního počítače, klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="92ce2-111">On hello blade for hello virtual machine, click **Connect**.</span></span>
   
    ![Snímek obrazovky portálu Azure znázorňující hello jak tooconnect tooyour virtuálních počítačů.](./media/connect-logon/connect.png)
   
   > [!TIP]
   > <span data-ttu-id="92ce2-113">Pokud hello **připojit** tlačítko hello portálu šedé a nejste připojené tooAzure prostřednictvím [Express Route](../../expressroute/expressroute-introduction.md) nebo [Site-to-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) připojení, je nutné toocreate a Přiřazení virtuálního počítače na veřejnou IP adresu před použitím protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="92ce2-113">If hello **Connect** button in hello portal is greyed out and you are not connected tooAzure via an [Express Route](../../expressroute/expressroute-introduction.md) or [Site-to-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) connection, you need toocreate and assign your VM a public IP address before you can use RDP.</span></span> <span data-ttu-id="92ce2-114">Podle potřeby si můžete přečíst další informace o [veřejných IP adresách v Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span><span class="sxs-lookup"><span data-stu-id="92ce2-114">You can read more about [public IP addresses in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span></span>
   > 
   > 

## <a name="log-on-toohello-virtual-machine"></a><span data-ttu-id="92ce2-115">Přihlaste se toohello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="92ce2-115">Log on toohello virtual machine</span></span>
[!INCLUDE [virtual-machines-log-on-win-server](../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a><span data-ttu-id="92ce2-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="92ce2-116">Next steps</span></span>
<span data-ttu-id="92ce2-117">Pokud narazíte na potíže při pokusu o tooconnect, přečtěte si téma [připojení ke vzdálené ploše řešení potíží s](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="92ce2-117">If you run into trouble when you try tooconnect, see [Troubleshoot Remote Desktop connections](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="92ce2-118">Tento článek vás provede diagnostikou a řešením běžných problémů.</span><span class="sxs-lookup"><span data-stu-id="92ce2-118">This article walks you through diagnosing and resolving common problems.</span></span>

