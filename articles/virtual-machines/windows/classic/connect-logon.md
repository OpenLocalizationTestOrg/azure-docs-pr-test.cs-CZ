---
title: "aaaLog na tooa classic virtuálního počítače Azure | Microsoft Docs"
description: "Použijte hello Azure portálu toolog na virtuální počítač Windows tooa vytvořené pomocí modelu nasazení classic hello."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 3c1239ed-07dc-48b8-8b3d-dc8c5f0ab20e
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 2e32b7036c2538e73b46580e0f5f8f4979e8a685
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-on-tooa-windows-virtual-machine-using-hello-azure-portal"></a>Přihlaste se tooa Windows virtuálního počítače pomocí hello portálu Azure
V hello portál Azure používáte hello **Connect** tlačítko toostart relaci vzdálené plochy a přihlaste se tooa virtuální počítač s Windows.

Chcete, aby tooconnect tooa virtuálního počítače s Linuxem? V tématu [jak toolog na tooa virtuální počítač se systémem Linux](../../linux/mac-create-ssh-keys.md).

<!--
Deleting, but not 100% sure
Learn how too[perform these steps using new Azure portal](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Informace o tom, jak toolog na virtuální počítač pomocí tooa hello Resource Manager modelu najdete v tématu [zde](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="connect-toohello-virtual-machine"></a>Připojit toohello virtuálního počítače
1. Přihlaste se toohello portálu Azure.
2. Klikněte na hello virtuálního počítače, které chcete tooaccess. Název Hello je uvedený v hello **všechny prostředky** podokně.

    ![Virtuální počítač – umístění](./media/connect-logon/azureportaldashboard.png)

3. Klikněte na tlačítko **Connect** na panelu příkazů hello na řídicí panel hello virtuálního počítače.

    ![Ikona pro virtuální počítač hello připojení](./media/connect-logon/virtualmachine_dashboard_connect.png)

<!-- Don't know if this still applies
     I think we can zap this.
> [!TIP]
> If hello **Connect** button isn't available, see hello troubleshooting tips at hello end of this article.
>
>
-->

## <a name="log-on-toohello-virtual-machine"></a>Přihlaste se toohello virtuálního počítače
[!INCLUDE [virtual-machines-log-on-win-server](../../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Další kroky
* Pokud hello **Connect** tlačítko je neaktivní nebo máte další problémy s hello připojení vzdálené plochy, opakujte pokus o obnovení konfigurace hello. Klikněte na tlačítko **obnovte vzdálený přístup** z řídicího panelu hello virtuálního počítače.

    ![Resetování vzdálený přístup](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* Řešení problémů s heslo zkuste resetovat ho. Klikněte na tlačítko **resetovat heslo** podél hello levé hrany řídicího panelu virtuální počítač, v části **podporu + Poradce při potížích s**.

    ![Resetování hesla](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

Pokud tyto tipy nefungují nebo nejsou, co potřebujete, najdete v části [tooa připojení řešení Vzdálená plocha systému Windows virtuálního počítače Azure](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Tento článek vás provede diagnostikou a řešením běžných problémů.
