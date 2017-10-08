---
title: "aaaConfigure privátních IP adres pro virtuální počítače (klasické) - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello tooconfigure privátních IP adres pro virtuální počítače (klasické) pomocí rozhraní příkazového řádku Azure (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17386acf-c708-4103-9b22-ff9bf04b778d
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 417a57181bcf5c2e6101bf3bdf63fc94ebc99df5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-hello-azure-cli-10"></a>Nakonfigurovat privátní IP adresy pro virtuální počítač (klasický) pomocí hello Azure CLI 1.0

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení classic hello. Můžete také [spravovat statickou privátní IP adresou v modelu nasazení Resource Manager hello](virtual-networks-static-private-ip-arm-cli.md).

níže uvedené příkazy rozhraní příkazového řádku Azure ukázkové Hello očekávat jednoduché prostředí již vytvořeny. Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a>Jak toospecify statickou privátní IP adresy při vytvoření virtuálního počítače
toocreate nový virtuální počítač s názvem *DNS01* v novou cloudovou službu s názvem *TestService* podle hello scénář výše, postupujte takto:

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.
2. Spustit hello **vytvoření azure služba** příkaz toocreate hello cloudové služby.
   
        azure service create TestService --location uscentral
   
    Očekávaný výstup:
   
        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
3. Spustit hello **azure vytvořit virtuální počítač** příkaz toocreate hello virtuálních počítačů. Všimněte si, hodnota hello se statickou privátní IP adresou. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.
   
        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd
   
    Očekávaný výstup:
   
        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting too"Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK
   
   * **-l (nebo --location)**. Oblast Azure, kde bude vytvořen hello virtuálních počítačů. V našem scénáři je to *centralus*.
   * **-n (nebo název virtuálního počítače –)**. Název toobe hello virtuálních počítačů vytvořena.
   * **-w (nebo--virtuální síťový název)**. Název hello virtuální síť, kde bude vytvořen hello virtuálních počítačů. 
   * **-S (nebo--statické ip)**. Statickou privátní IP adresou pro hello virtuálních počítačů.
   * **TestService**. Název hello cloudové služby, kde bude vytvořen hello virtuálních počítačů.
   * **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**. Bitová kopie používaná toocreate hello virtuálních počítačů.
   * **AdminUser**. Místní správce pro virtuální počítač s Windows hello.
   * **AdminP@ssw0rd**. Heslo místního správce pro virtuální počítač s Windows hello.

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a>Jak tooretrieve statickou privátní IP adresu pro virtuální počítač
tooview hello statickou privátní IP adresu pro hello virtuální počítač vytvořen pomocí skriptu hello výše, spusťte následující příkaz rozhraní příkazového řádku Azure hello a sledovat hello hodnotu *sítě StaticIP*:

    azure vm static-ip show DNS01

Očekávaný výstup:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a>Jak tooremove statickou privátní IP adresu z virtuálního počítače
tooremove hello statickou privátní IP adresou přidána toohello virtuálních počítačů ve skriptu hello nad spuštění hello následující příkaz rozhraní příkazového řádku Azure:

    azure vm static-ip remove DNS01

Očekávaný výstup:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-tooadd-a-static-private-ip-tooan-existing-vm"></a>Jak tooadd statickou privátní tooan IP existující virtuální počítač
tooadd statickou privátní IP adresu toohello virtuálních počítačů, které jsou vytvořené pomocí skriptu hello výše, o následující příkaz:

    azure vm static-ip set DNS01 192.168.1.101

Očekávaný výstup:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Další kroky
* Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.
* Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.
* Poraďte se hello [vyhrazené IP rozhraní REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).

