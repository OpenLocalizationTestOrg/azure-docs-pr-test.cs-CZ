---
title: "aaaHow tooreset síťové rozhraní pro virtuální počítač s Windows Azure | Microsoft Docs"
description: "Ukazuje, jak tooreset síťové rozhraní pro virtuální počítač s Windows Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: genli
ms.openlocfilehash: 1b653820927ef4c3bb8f384a7e752846a8be3da9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-network-interface-for-azure-windows-vm"></a>Jak tooreset síťové rozhraní pro virtuální počítač s Windows Azure 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Po zakázání hello výchozí síťové rozhraní (NIC) se nemůže připojit tooMicrosoft Azure Windows virtuální počítač (VM) nebo ručně nastaví statickou IP adresu pro síťový adaptér hello. Tento článek ukazuje, jak tooreset hello síťové rozhraní pro virtuální počítač Windows Azure, který bude vyřešen problém hello vzdáleného připojení.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a>Resetování síťové rozhraní

### <a name="for-classic-vms"></a>Pro klasické virtuální počítače

tooreset síťové rozhraní, postupujte takto:

1.  Přejděte toohello [portál Azure]( https://ms.portal.azure.com).
2.  Vyberte **virtuálních počítačů (klasické)**.
3.  Vyberte hello vliv virtuálního počítače.
4.  Vyberte **IP adresy**.
5.  Pokud hello **privátní IP přiřazení** není **statické**, změňte jej příliš**statické**.
6.  Změna hello **IP adresu** tooanother IP adresu, která je k dispozici v hello podsítě.
7.  Vyberte Uložit.
8.  Hello virtuální počítač se restartuje tooinitialize hello nový síťový adaptér toohello systém.
9.  Zkuste počítač tooyour tooRDP. V případě úspěchu, pokud chcete, můžete změnit hello privátní IP adresu zpět toohello původní. Jinak můžete ponechat ji. 

### <a name="for-vms-deployed-in-resource-group-model"></a>Pro virtuální počítače nasazené v modelu skupiny prostředků

1.  Přejděte toohello [portál Azure]( https://ms.portal.azure.com).
2.  Vyberte hello vliv virtuálního počítače.
3.  Vyberte **síťových rozhraní**.
4.  Vyberte síťové rozhraní přidružené k počítači hello
5.  Vyberte **konfigurace protokolu IP**.
6.  Vyberte hello IP. 
7.  Pokud hello **privátní IP přiřazení** není **statické**, změňte jej příliš**statické**.
8.  Změna hello **IP adresu** tooanother IP adresu, která je k dispozici v hello podsítě.
9. Hello virtuální počítač se restartuje tooinitialize hello nový síťový adaptér toohello systém.
10. Zkuste počítač tooyour tooRDP. V případě úspěchu, pokud chcete, můžete změnit hello privátní IP adresu zpět toohello původní. Jinak můžete ponechat ji. 

## <a name="delete-hello-unavailable-nics"></a>Odstranit hello není k dispozici síťové karty
Poté, co je to možné vzdálené plochy toohello počítače, je nutné odstranit hello staré síťové adaptéry tooavoid hello potenciální problém:

1.  Otevřete Správce zařízení.
2.  Vyberte **zobrazení** > **zobrazit skrytá zařízení**.
3.  Vyberte **síťové adaptéry**. 
4.  Zkontrolujte hello adaptérů s názvem jako "Microsoft Hyper-V síťový adaptér".
5.  Může se zobrazit není k dispozici adaptér, který je zobrazena šedě. Klikněte pravým tlačítkem hello adaptér a potom vyberte možnost odinstalovat.

    ![Obrázek Hello hello síťový adaptér](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Pouze odinstalujte hello není k dispozici adaptéry, které mají hello název "Microsoft Hyper-V síťový adaptér". Pokud odinstalujete žádné hello ostatní skrytá adaptéry, může způsobit další problémy.
    >
    >

6.  Nyní všechny adaptér není k dispozici je nutné vymazat z vašeho systému.