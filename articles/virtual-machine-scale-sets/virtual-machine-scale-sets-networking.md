---
title: "aaaNetworking pro sady škálování virtuálního počítače Azure | Microsoft Docs"
description: "Konfigurace síťových vlastností pro škálovací sady virtuálních počítačů Azure."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: guybo
ms.openlocfilehash: ef3f0cfe648d2195c051a73987e654f0e15d13bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="networking-for-azure-virtual-machine-scale-sets"></a>Síťové služby pro škálovací sady virtuálních počítačů Azure

Při nasazení sad prostřednictvím portálu hello škálování virtuálního počítače Azure, jsou uvedena určité vlastnosti sítě, například Vyrovnávání zatížení Azure příchozí pravidla NAT. Tento článek popisuje, jak nastaví toouse některé hello pokročilejší síťových funkcí, které můžete nakonfigurovat s měřítkem.

Můžete nakonfigurovat všechny funkce hello popsaná v tomto článku pomocí šablony Azure Resource Manager. Pro vybrané funkce jsou zahrnuté také příklady Azure CLI a PowerShellu. Použijte CLI 2.10 a PowerShell 4.2.0 nebo novější.

## <a name="accelerated-networking"></a>Akcelerované síťové služby
Azure [Accelerated sítě](../virtual-network/virtual-network-create-vm-accelerated-networking.md) zlepšuje výkon sítě povolením jeden kořenový vstupně-výstupních operací virtualizace (SR-IOV) tooa virtuálního počítače. toouse accelerated práce se sítěmi pomocí sady škálování, nastavte enableAcceleratedNetworking příliš**true** v sadě škálování Networkinterfaceconfiguration nastavení. Například:
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
      "name": "niconfig1",
      "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
          ...
        ]
      }
    }
   ]
}
```

## <a name="create-a-scale-set-that-references-an-existing-azure-load-balancer"></a>Vytvoření škálovací sady odkazující na existující Azure Load Balancer
Když sadu škálování je vytvořený pomocí hello portálu Azure, je většina možností konfigurace vytvoří nové služby Vyrovnávání zatížení. Pokud vytvoříte sadu škálování, který se musí tooreference existující pro vyrovnávání zatížení, můžete k tomu pomocí rozhraní příkazového řádku. Následující ukázkový skript Hello vytváří nástroj pro vyrovnávání zatížení a potom vytvoří sada škálování, které na ni odkazuje:
```bash
az network lb create -g lbtest -n mylb --vnet-name myvnet --subnet mysubnet --public-ip-address-allocation Static --backend-pool-name mybackendpool

az vmss create -g lbtest -n myvmss --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username negat --ssh-key-value /home/myuser/.ssh/id_rsa.pub --upgrade-policy-mode Automatic --instance-count 3 --vnet-name myvnet --subnet mysubnet --lb mylb --backend-pool-name mybackendpool

```

## <a name="configurable-dns-settings"></a>Konfigurovatelná nastavení DNS
Ve výchozím nastavení na konkrétní nastavení DNS hello hello virtuální síť a podsíť, které byly vytvořeny v trvat sady škálování. Ale můžete nakonfigurovat nastavení DNS hello horizontálního nastavovat přímo.
~
### <a name="creating-a-scale-set-with-configurable-dns-servers"></a>Vytvoření škálovací sady s konfigurovatelnými servery DNS
toocreate škálování nastavit pomocí vlastní konfigurace DNS pomocí rozhraní příkazového řádku 2.0, přidejte hello **servery – dns** argument toohello **vmss vytvořit** příkazu, za nímž následuje místo oddělené ip adres serveru. Například:
```bash
--dns-servers 10.0.0.6 10.0.0.5
```
tooconfigure vlastní servery DNS v šablony Azure, přidejte škálování dnsSettings vlastnost toohello nastavení Networkinterfaceconfiguration sekce. Například:
```json
"dnsSettings":{
    "dnsServers":["10.0.0.6", "10.0.0.5"]
}
```

### <a name="creating-a-scale-set-with-configurable-virtual-machine-domain-names"></a>Vytvoření škálovací sady s konfigurovatelnými názvy domén virtuálních počítačů
toocreate škálování nastavit vlastní název DNS pro virtuální počítače pomocí rozhraní příkazového řádku 2.0, přidejte hello **název domény virtuálního počítače –** argument toohello **vmss vytvořit** příkazu, za nímž následuje řetězec představující název domény hello.

Přidejte název domény hello tooset šablony Azure, **dnsSettings** vlastnost toohello škálovací sadu **Networkinterfaceconfiguration** části. Například:

```json
"networkProfile": {
  "networkInterfaceConfigurations": [
    {
    "name": "nic1",
    "properties": {
      "primary": "true",
      "ipConfigurations": [
      {
        "name": "ip1",
        "properties": {
          "subnet": {
            "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
          },
          "publicIPAddressconfiguration": {
            "name": "publicip",
            "properties": {
            "idleTimeoutInMinutes": 10,
              "dnsSettings": {
                "domainNameLabel": "[parameters('vmssDnsName')]"
              }
            }
          }
        }
      }
    ]
    }
}
```

výstup Hello pro virtuální počítač zvlášť název dns by byl v hello následující formulář: 
```
<vm><vmindex>.<specifiedVmssDomainNameLabel>
```

## <a name="public-ipv4-per-virtual-machine"></a>Veřejná IPv4 adresa na virtuální počítač
Obecně platí, že virtuální počítače Azure ve škálovací sadě nevyžadují vlastní veřejné IP adresy. Pro většinu scénářů je tooassociate veřejnou IP adresu tooa zatížení vyrovnávání nebo tooan jednotlivé virtuální počítač (neboli jumpbox), který pak směruje příchozí připojení tooscale sada virtuálních počítačů podle potřeby (například prostřednictvím více ekonomické a zabezpečení Příchozí pravidla NAT).

Ale některé scénáře vyžadují sady škálování virtuálních počítačů toohave svá vlastní veřejná IP adresy. Hry je příklad, kdy konzoly je toomake přímé připojení tooa cloudu virtuálního počítače, který provádí herní fyziky zpracování. Dalším příkladem je, kde virtuální počítače musí toomake externí připojení tooone jiné v oblastech v distribuované databáze.

### <a name="creating-a-scale-set-with-public-ip-per-virtual-machine"></a>Vytvoření škálovací sady s veřejnou IP adresou na virtuální počítač
toocreate sadu škálování, který přiřazuje veřejnou IP adresu tooeach virtuální počítač s 2.0 rozhraní příkazového řádku, přidejte hello **– veřejné ip na počítač** parametr toohello **vmss vytvořit** příkaz. 

toocreate škálování nastavit pomocí šablony Azure, zkontrolujte, zda hello rozhraní API verze hello Microsoft.Compute/virtualMachineScaleSets prostředků je alespoň **2017-03-30**a přidejte **publicIpAddressConfiguration**JSON vlastnost toohello škálovací sady část konfigurace IP adresy. Například:

```json
"publicIpAddressConfiguration": {
    "name": "pub1",
    "properties": {
      "idleTimeoutInMinutes": 15
    }
}
```
Ukázková šablona: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)

### <a name="querying-hello-public-ip-addresses-of-hello-virtual-machines-in-a-scale-set"></a>Dotazu hello veřejné IP adresy hello virtuálních počítačů v škálování sadu
toolist hello veřejné IP adresy přiřazené tooscale sada virtuálních počítačů pomocí rozhraní příkazového řádku 2.0, použijte hello **az vmss seznamu instance veřejný-IP adresy** příkaz.

sady škálování toolist veřejné IP adresy pomocí prostředí PowerShell, použijte hello _Get-AzureRmPublicIpAddress_ příkaz. Například:
```PowerShell
PS C:\> Get-AzureRmPublicIpAddress -ResourceGroupName myrg -VirtualMachineScaleSetName myvmss
```

Můžete také dotazu hello veřejné IP adresy pomocí přímo odkazující na id prostředku hello konfiguraci hello veřejné IP adresy. Například:
```PowerShell
PS C:\> Get-AzureRmPublicIpAddress -ResourceGroupName myrg -Name myvmsspip
```

tooquery hello veřejné IP adresy přiřazené tooscale sada virtuálních počítačů pomocí hello [Průzkumníka prostředků Azure](https://resources.azure.com), nebo hello REST API služby Azure s verzí **2017-03-30** nebo vyšší.

tooview veřejné IP adresy pro škálování nastavit pomocí hello Průzkumníkem prostředků, podívejte se na hello **publicipaddresses, na které** oddílu v části škálovací sadu. Například: https://resources.azure.com/subscriptions/_id_vašeho_předplatného_/resourceGroups/_vaše_skupina_prostředků_/providers/Microsoft.Compute/virtualMachineScaleSets/_vaše_škálovací_sada_virtuálních_počítačů_/publicipaddresses

```
GET https://management.azure.com/subscriptions/{your sub ID}/resourceGroups/{RG name}/providers/Microsoft.Compute/virtualMachineScaleSets/{scale set name}/publicipaddresses?api-version=2017-03-30
```

Příklad výstupu:
```json
{
  "value": [
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/pipvmss/virtualMachines/0/networkInterfaces/pipvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"a64060d5-4dea-4379-a11d-b23cd49a3c8d\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "ee8cb20f-af8e-4cd6-892f-441ae2bf701f",
        "ipAddress": "13.84.190.11",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/0/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    },
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"5f6ff30c-a24c-4818-883c-61ebd5f9eee8\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "036ce266-403f-41bd-8578-d446d7397c2f",
        "ipAddress": "13.84.159.176",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    }
```

## <a name="multiple-ip-addresses-per-nic"></a>Několik IP adres na síťové rozhraní
Každý síťový adaptér připojený tooa virtuální počítač ve škálovací sadě může mít jeden nebo více konfigurací IP adres, které jsou s ním spojená. Každá konfigurace má přiřazenu jednu privátní IP adresu. Každá konfigurace také může mít přiřazen jeden prostředek veřejné IP adresy. toounderstand kolik IP adresy můžete přiřadit tooa síťového adaptéru, a kolik veřejné IP adresy v předplatné Azure, můžete použít příliš[Azure omezení](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="multiple-nics-per-virtual-machine"></a>Několik síťových rozhraní na virtuální počítač
Může obsahovat maximálně too8 síťové adaptéry na virtuálním počítači, v závislosti na velikosti počítače. Hello maximální počet síťových adaptérů na počítač je k dispozici v hello [článku velikost virtuálního počítače](../virtual-machines/windows/sizes.md). Hello následující příklad je že profil sítě s více položek síťový adaptér a víc veřejných IP adres na virtuální počítač sady škálování:
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
        "name": "nic1",
        "properties": {
            "primary": "true",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        },
        {
        "name": "nic2",
        "properties": {
            "primary": "false",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        }
    ]
}
```

## <a name="nsg-per-scale-set"></a>NSG na škálovací sadu
Skupiny zabezpečení sítě je možné použít přímo tooa škálovací sadu, přidáním odkazu toohello síťové rozhraní konfigurační sekce hello měřítka nastavit vlastnosti virtuálního počítače.

Například: 
```
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Další kroky
Další informace o virtuálních sítí Azure, najdete v části příliš[této dokumentace](../virtual-network/virtual-networks-overview.md).
