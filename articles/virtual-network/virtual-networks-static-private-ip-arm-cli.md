---
title: "aaaConfigure privátních IP adres pro virtuální počítače - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello tooconfigure privátních IP adres pro virtuální počítače pomocí rozhraní příkazového řádku Azure (CLI) 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 40b03a1a-ea00-454c-b716-7574cea49ac0
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0e278e6ac63c0cda061cf70ab0edfaff5491c03b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-cli-20"></a>Nakonfigurovat privátní IP adresy pro virtuální počítač pomocí hello 2.0 rozhraní příkazového řádku Azure

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]


## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku 

Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku: 

- [Azure CLI 1.0](virtual-networks-static-private-ip-cli-nodejs.md) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely 
- [Azure CLI 2.0](#specify-a-static-private-ip-address-when-creating-a-vm) -naší nové generace rozhraní příkazového řádku pro model nasazení prostředků správu hello (v tomto článku)

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Resource Manager hello. Můžete také [spravovat statickou privátní IP adresou v modelu nasazení classic hello](virtual-networks-static-private-ip-classic-cli.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

> [!NOTE]
> Hello vzorové Azure CLI 2.0 příkazy níže očekávat jednoduché prostředí již vytvořeny. Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-arm-cli.md).

## <a name="specify-a-static-private-ip-address-when-creating-a-vm"></a>Při vytváření virtuálního počítače zadat statickou privátní IP adresu

virtuální počítač s názvem toocreate *DNS01* v hello *front-endu* podsíť virtuální sítě s názvem *TestVNet* se statickou privátní IP z *192.168.1.101*, postupujte podle následujících kroků hello:

1. Pokud nebyly dosud, nainstalujete a nakonfigurujete hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login). 

2. Vytvoření veřejné IP adresy pro hello virtuálních počítačů s hello [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create) příkaz. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.

    > [!NOTE]
    > Můžete chtít nebo potřebovat toouse různé hodnoty pro vaše argumenty v tomto a dalších krocích v závislosti na vašem prostředí.
   
    ```azurecli
    az network public-ip create \
    --name TestPIP \
    --resource-group TestRG \
    --location centralus \
    --allocation-method Static
    ```

    Očekávaný výstup:
   
   ```json
   {
        "publicIp": {
            "idleTimeoutInMinutes": 4,
            "ipAddress": "52.176.43.167",
            "provisioningState": "Succeeded",
            "publicIPAllocationMethod": "Static",
            "resourceGuid": "79e8baa3-33ce-466a-846c-37af3c721ce1"
        }
    }
    ```

   * `--resource-group`: Název skupiny prostředků hello toocreate veřejné IP hello.
   * `--name`: Název veřejné IP adresy hello.
   * `--location`: Oblast azure, ve které toocreate hello veřejnou IP adresu.

3. Spustit hello [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create) příkaz toocreate síťový adaptér se statickou privátní IP. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello. 
   
    ```azurecli
    az network nic create \
    --resource-group TestRG \
    --name TestNIC \
    --location centralus \
    --subnet FrontEnd \
    --private-ip-address 192.168.1.101 \
    --vnet-name TestVNet
    ```

    Očekávaný výstup:
   
    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.101",
                "privateIPAllocationMethod": "Static",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>"
        }
    }
    ```
    
    Parametry:

    * `--private-ip-address`: Statickou privátní IP adresou pro hello síťový adaptér.
    * `--vnet-name`: Název hello sítě vnet, ve které toocreate hello síťový adaptér.
    * `--subnet`: Název hello podsítě, ve které toocreate hello síťový adaptér.

4. Spustit hello [vytvoření virtuálního počítače azure](/cli/azure/vm/nic#create) hello toocreate příkaz virtuálního počítače pomocí hello veřejná IP adresa a síťovou kartu vytvořili výše. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.
   
    ```azurecli
    az vm create \
    --resource-group TestRG \
    --name DNS01 \
    --location centralus \
    --image Debian \
    --admin-username adminuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics TestNIC
    ```

    Očekávaný výstup:
   
    ```json
    {
        "fqdns": "",
        "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01",
        "location": "centralus",
        "macAddress": "00-0D-3A-92-C1-66",
        "powerState": "VM running",
        "privateIpAddress": "192.168.1.101",
        "publicIpAddress": "",
        "resourceGroup": "TestRG"
    }
    ```
   
   Parametry než hello základní [vytvořit virtuální počítač az](/cli/azure/vm#create) parametry.

   * `--nics`: Název hello seskupování toowhich hello virtuální počítač je připojený.
   

## <a name="retrieve-static-private-ip-address-information-for-a-vm"></a>Načíst statickou privátní IP adresu informace pro virtuální počítač

Spusťte následující příkaz rozhraní příkazového řádku Azure hello tooview hello statickou privátní IP adresu, která jste vytvořili, a sledovat hello hodnoty pro *alokační metody privátní IP* a *privátní IP adresa*:

```azurecli
az vm show -g TestRG -n DNS01 --show-details --query 'privateIps'
```

Očekávaný výstup:

```json
"192.168.1.101"
```

toodisplay hello konkrétně pro konkrétní IP informace hello síťovou kartu pro tento virtuální počítač, dotaz hello síťovou kartu:

```azurecli
az network nic show \
-g testrg \
-n testnic \
--query 'ipConfigurations[0].{PrivateAddress:privateIpAddress,IPVer:privateIpAddressVersion,IpAllocMethod:p
rivateIpAllocationMethod,PublicAddress:publicIpAddress}'
```

výstup Hello se něco podobného jako:

```json
{
    "IPVer": "IPv4",
    "IpAllocMethod": "Static",
    "PrivateAddress": "192.168.1.101",
    "PublicAddress": null
}
```

## <a name="remove-a-static-private-ip-address-from-a-vm"></a>Odeberte statickou privátní IP adresu z virtuálního počítače

Statickou privátní IP adresu nelze odebrat z síťový adaptér v Azure CLI pro nasazení resource manager. Postupujte takto:
- Vytvořte novou síťovou kartu, která používá dynamický IP
- Nastavit hello síťový adaptér na hello hello virtuálních počítačů nově vytvořený síťový adaptér. 

toochange hello síťovou kartu pro hello virtuálních počítačů použita v hello příkazů výše, postupujte podle kroků hello níže.

1. Spustit hello **síťových adaptérů sítě azure vytvořit** příkaz toocreate nový síťový adaptér pomocí dynamické přidělování IP adres s novou IP adresu. Všimněte si, protože není zadána žádná IP adresa, metoda přidělení hello **dynamické**.

    ```azurecli
    az network nic create     \
    --resource-group TestRG     \
    --name TestNIC2     \
    --location centralus     \
    --subnet FrontEnd    \
    --vnet-name TestVNet
    ```        
   
    Očekávaný výstup:

    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.4",
                "privateIPAllocationMethod": "Dynamic",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "0808a61c-476f-4d08-98ee-0fa83671b010"
        }
    }
    ```

2. Spustit hello **sada virtuálních počítačů azure** příkaz toochange hello síťový adaptér používá hello virtuálních počítačů.
   
    ```azurecli
    azure vm set -g TestRG -n DNS01 -N TestNIC2
    ```

    Očekávaný výstup:
   
    ```json
    [
        {
            "id": "/subscriptions/0e220bf6-5caa-4e9f-8383-51f16b6c109f/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC3",
            "primary": true,
            "resourceGroup": "TestRG"
        }
    ]
    ```

    > [!NOTE]
    > Pokud hello virtuálního počítače je dostatečně velké na to toohave více než jeden síťový adaptér, spusťte hello **odstranit síťových adaptérů sítě azure** příkaz toodelete hello staré síťový adaptér.
   
## <a name="next-steps"></a>Další kroky
* Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.
* Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.
* Poraďte se hello [vyhrazené IP rozhraní REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).

