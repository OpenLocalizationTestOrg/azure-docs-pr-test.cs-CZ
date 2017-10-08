---
title: "aaaInstall MongoDB na virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Zjistěte, jak vytvořit tooinstall MongoDB ve virtuálním počítači Azure s modelem nasazení classic hello systémem Windows Server."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4095df41-bb69-4bbe-9c1c-70923b0d84ba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: iainfou
ms.openlocfilehash: aafd86b4c49c6a97ae8a1a2191ae375b6c47483a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-mongodb-on-a-windows-vm-in-azure"></a>Nainstalujte MongoDB v systému Windows virtuálního počítače v Azure
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).  Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. tooinstall a nakonfigurovat pomocí modelu nasazení Resource Manager hello MongoDB, najdete v části [v tomto článku](../install-mongodb.md).

[MongoDB] [ MongoDB] je Oblíbené databáze NoSQL open source a vysoce výkonné. Tento článek vás provede vytvořením virtuálního počítače (VM) Windows serveru pomocí hello [portál Azure][AzurePortal]. Pak vytvořte a připojte disk toohello data virtuálních počítačů před instalací a konfigurací MongoDB. Pokud máte existující virtuální počítač v Azure, které chcete toouse, můžete přejít rovnou příliš[instalace a konfigurace MongoDB](#install-and-run-mongodb-on-the-virtual-machine).

## <a name="create-a-virtual-machine-running-windows-server"></a>Vytvoření virtuálního počítače se systémem Windows Server
Postupujte podle těchto pokynů toocreate virtuálního počítače.

[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> Přidání koncového bodu pro MongoDB při vytváření hello virtuálního počítače a nakonfigurovat následujícím způsobem: pojmenujte ji jako **Mongo**, použijte **TCP** jako hello protokolu a nastavte oba hello veřejné a privátní porty příliš**27017**.
>
>

## <a name="attach-a-data-disk"></a>Připojení datového disku
tooprovide úložiště pro virtuální počítač hello, připojit datový disk a potom inicializujte tak, aby Windows můžete používat. Pokud již máte datový disk, můžete připojit tento stávající disk, nebo můžete připojte prázdný disk.

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-windows-linux.md)]

Pokyny k inicializaci hello disku, najdete v části "postup: inicializovat nový datový disk ve Windows serveru" v [jak tooattach datový disk virtuálního počítače Windows tooa](attach-disk.md).

## <a name="install-and-run-mongodb-on-hello-virtual-machine"></a>Nainstalujte a spusťte MongoDB hello virtuálního počítače
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Souhrn
V tomto kurzu jste se dozvěděli, jak toocreate virtuálního počítače se systémem Windows Server vzdáleně připojit tooit a připojit datový disk.  Také jste se naučili jak tooinstall a konfigurace virtuálního počítače založené na Windows hello MongoDB. Nyní můžete k MongoDB hello systému Windows virtuálního počítače pomocí následující hello advanced témata v hello [dokumentace k MongoDB][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: https://portal.azure.com/

