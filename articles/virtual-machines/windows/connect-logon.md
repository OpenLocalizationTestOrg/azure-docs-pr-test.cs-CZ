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
# <a name="how-tooconnect-and-log-on-tooan-azure-virtual-machine-running-windows"></a>Jak se tooconnect a protokol na tooan Azure virtuální počítač, systémem Windows
Budete používat hello **Connect** tlačítka na hello Azure portálu toostart relaci vzdálené plochy (RDP) z plochy Windows. Nejprve připojit toohello virtuálního počítače a potom přihlášení.

Pokud se pokoušíte tooconnect tooa Windows virtuální počítač z algoritmu Mac, je třeba tooinstall klientem RDP pro Mac, například [Vzdálená plocha od Microsoftu](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).

## <a name="connect-toohello-virtual-machine"></a>Připojit toohello virtuálního počítače
1. Pokud jste tak již neučinili, přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. V nabídce centra hello, klikněte na tlačítko **virtuální počítače**.
3. Vyberte virtuální počítač hello hello seznamu.
4. V okně hello hello virtuálního počítače, klikněte na tlačítko **Connect**.
   
    ![Snímek obrazovky portálu Azure znázorňující hello jak tooconnect tooyour virtuálních počítačů.](./media/connect-logon/connect.png)
   
   > [!TIP]
   > Pokud hello **připojit** tlačítko hello portálu šedé a nejste připojené tooAzure prostřednictvím [Express Route](../../expressroute/expressroute-introduction.md) nebo [Site-to-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) připojení, je nutné toocreate a Přiřazení virtuálního počítače na veřejnou IP adresu před použitím protokolu RDP. Podle potřeby si můžete přečíst další informace o [veřejných IP adresách v Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 

## <a name="log-on-toohello-virtual-machine"></a>Přihlaste se toohello virtuálního počítače
[!INCLUDE [virtual-machines-log-on-win-server](../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Další kroky
Pokud narazíte na potíže při pokusu o tooconnect, přečtěte si téma [připojení ke vzdálené ploše řešení potíží s](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Tento článek vás provede diagnostikou a řešením běžných problémů.

