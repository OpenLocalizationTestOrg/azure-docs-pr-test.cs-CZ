---
title: "aaaCreate prostředí Linux s hello 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvoření úložiště, virtuální počítač s Linuxem, virtuální síť a podsíť, nástroj pro vyrovnávání zatížení, seskupování, veřejnou IP adresu a skupinu zabezpečení sítě z hello pozadí pomocí hello 2.0 rozhraní příkazového řádku Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/06/2017
ms.author: iainfou
ms.openlocfilehash: 7287ea178e76001b84dade628ead04a59dc27f40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-complete-linux-virtual-machine-with-hello-azure-cli"></a>Vytvoření kompletní virtuální počítač Linux s hello rozhraní příkazového řádku Azure
tooquickly vytvoření virtuálního počítače (VM) v Azure, můžete použít jeden příkaz rozhraní příkazového řádku Azure, který používá výchozí hodnoty toocreate všech požadovaných podpora prostředky. Prostředky, jako je například virtuální sítě, veřejnou IP adresu a pravidel skupiny zabezpečení sítě se vytvářejí automaticky. Pro další ovládací prvek v provozním prostředí použít, můžete vytvořit tyto prostředky předem a pak přidat toothem vaše virtuální počítače. Tento článek vás provede jak toocreate virtuálního počítače a každý z hello doprovodné materiály po jednom.

Ujistěte se, že jste nainstalovali hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a protokolu tooan Azure účet s [az přihlášení](/cli/azure/#login).

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků
Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Před virtuální počítač a doprovodné materiály virtuální sítě je třeba vytvořit skupinu prostředků. Vytvořte skupinu prostředků hello s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:

```azurecli
az group create --name myResourceGroup --location eastus
```

Ve výchozím nastavení výstup hello rozhraní příkazového řádku Azure je ve formátu JSON (JavaScript Object Notation). toochange hello výchozí výstup tooa seznamu nebo tabulka, například pomocí [konfigurace az--výstup](/cli/azure/#configure). Můžete také přidat `--output` tooany příkaz po dobu jednoho změnit ve výstupním formátu. Hello následující příklad ukazuje výstup JSON hello hello `az group create` příkaz:

```json                       
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "location": "eastus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-a-virtual-network-and-subnet"></a>Vytvoření virtuální sítě a podsítě
Dále vytvoříte virtuální síť v Azure a podsíť v toowhich můžete vytvořit virtuální počítače. Použití [vytvoření sítě vnet az](/cli/azure/network/vnet#create) toocreate virtuální síť s názvem *myVnet* s hello *192.168.0.0/16* předponu adresy. Můžete také přidat podsíť s názvem *mySubnet* s předponou adresy hello z *192.168.1.0/24*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

výstup Hello ukazuje podsíť hello jako logicky vytvořená ve virtuální síti hello:

```json
{
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "location": "eastus",
  "name": "myVnet",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "ed62fd03-e9de-430b-84df-8a3b87cacdbb",
  "subnets": [
    {
      "addressPrefix": "192.168.1.0/24",
      "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
      "ipConfigurations": null,
      "name": "mySubnet",
      "networkSecurityGroup": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  "tags": {},
  "type": "Microsoft.Network/virtualNetworks",
  "virtualNetworkPeerings": null
}
```


## <a name="create-a-public-ip-address"></a>Vytvoření veřejné IP adresy
Nyní vytvoříme veřejnou IP adresu s [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create). Tato veřejná IP adresa umožňuje vám tooconnect tooyour virtuální počítače z hello Internetu. Protože hello výchozí adresa je dynamická, jsme také vytvořit položku DNS s názvem hello `--domain-name-label` možnost. Hello následující příklad vytvoří veřejnou IP adresu s názvem *myPublicIP* s názvem DNS hello *mypublicdns*. Vzhledem k tomu, že název DNS hello musí být jedinečný, zadejte svůj vlastní jedinečný název DNS:

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

Výstup:

```json
{
  "publicIp": {
    "dnsSettings": {
      "domainNameLabel": "mypublicdns",
      "fqdn": "mypublicdns.eastus.cloudapp.azure.com",
      "reverseFqdn": null
    },
    "etag": "W/\"2632aa72-3d2d-4529-b38e-b622b4202925\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": null,
    "ipConfiguration": null,
    "location": "eastus",
    "name": "myPublicIP",
    "provisioningState": "Succeeded",
    "publicIpAddressVersion": "IPv4",
    "publicIpAllocationMethod": "Dynamic",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "4c65de38-71f5-4684-be10-75e605b3e41f",
    "tags": null,
    "type": "Microsoft.Network/publicIPAddresses"
  }
}
```


## <a name="create-a-network-security-group"></a>Vytvořit skupinu zabezpečení sítě
toocontrol hello tok provozu do a z virtuálních počítačů, vytvořte skupinu zabezpečení sítě. Skupina zabezpečení sítě může být použité tooa síťového adaptéru nebo podsítě. Hello následující příklad používá [vytvořit az sítě nsg](/cli/azure/network/nsg#create) toocreate skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

Můžete definovat pravidla, která povolují nebo odepírají hello konkrétní přenosy. tooallow příchozí připojení na portu 22 (toosupport SSH), vytvoření příchozího pravidla pro skupinu zabezpečení sítě hello s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create). Hello následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRuleSSH*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 22 \
    --access allow
```

tooallow příchozí připojení na portu 80 (toosupport webových přenosů), přidejte další pravidla skupiny zabezpečení sítě. Hello následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRuleHTTP*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleWeb \
    --protocol tcp \
    --priority 1001 \
    --destination-port-range 80 \
    --access allow
```

Zkontrolujte hello skupinu zabezpečení sítě a pravidla s [az sítě nsg zobrazit](/cli/azure/network/nsg#show):

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

Výstup:

```json
{
  "defaultSecurityRules": [
    {
      "access": "Allow",
      "description": "Allow inbound traffic from all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetInBound",
      "name": "AllowVnetInBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow inbound traffic from azure load balancer",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowAzureLoadBalancerInBou
      "name": "AllowAzureLoadBalancerInBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "AzureLoadBalancer",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all inbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllInBound",
      "name": "DenyAllInBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs tooall VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetOutBound",
      "name": "AllowVnetOutBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs tooInternet",
      "destinationAddressPrefix": "Internet",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowInternetOutBound",
      "name": "AllowInternetOutBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all outbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllOutBound",
      "name": "DenyAllOutBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
  "location": "eastus",
  "name": "myNetworkSecurityGroup",
  "networkInterfaces": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "47a9964e-23a3-438a-a726-8d60ebbb1c3c",
  "securityRules": [
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "22",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleSSH",
      "name": "myNetworkSecurityGroupRuleSSH",
      "priority": 1000,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "80",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleWeb",
      "name": "myNetworkSecurityGroupRuleWeb",
      "priority": 1001,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "subnets": null,
  "tags": null,
  "type": "Microsoft.Network/networkSecurityGroups"
}
```

## <a name="create-a-virtual-nic"></a>Vytvořit virtuální síťovou kartu
Virtuální síťové adaptéry (NIC) jsou prostřednictvím kódu programu k dispozici, protože můžete použít pravidla tootheir použití. Také můžete mít více než jednu. V následující hello [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create) příkazu vytvoříte síťový adaptér s názvem *myNic* a přidružte ji k hello skupinu zabezpečení sítě. Hello veřejnou IP adresu *myPublicIP* je taky přiřazený hello virtuální síťový adaptér.

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

Výstup:

```json
{
  "NewNIC": {
    "dnsSettings": {
      "appliedDnsServers": [],
      "dnsServers": [],
      "internalDnsNameLabel": null,
      "internalDomainNameSuffix": "brqlt10lvoxedgkeuomc4pm5tb.bx.internal.cloudapp.net",
      "internalFqdn": null
    },
    "enableAcceleratedNetworking": false,
    "enableIpForwarding": false,
    "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
    "ipConfigurations": [
      {
        "applicationGatewayBackendAddressPools": null,
        "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic/ipConfigurations/ipconfig1",
        "loadBalancerBackendAddressPools": null,
        "loadBalancerInboundNatRules": null,
        "name": "ipconfig1",
        "primary": true,
        "privateIpAddress": "192.168.1.4",
        "privateIpAddressVersion": "IPv4",
        "privateIpAllocationMethod": "Dynamic",
        "provisioningState": "Succeeded",
        "publicIpAddress": {
          "dnsSettings": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
          "idleTimeoutInMinutes": null,
          "ipAddress": null,
          "ipConfiguration": null,
          "location": null,
          "name": null,
          "provisioningState": null,
          "publicIpAddressVersion": null,
          "publicIpAllocationMethod": null,
          "resourceGroup": "myResourceGroup",
          "resourceGuid": null,
          "tags": null,
          "type": null
        },
        "resourceGroup": "myResourceGroup",
        "subnet": {
          "addressPrefix": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
          "ipConfigurations": null,
          "name": null,
          "networkSecurityGroup": null,
          "provisioningState": null,
          "resourceGroup": "myResourceGroup",
          "resourceNavigationLinks": null,
          "routeTable": null
        }
      }
    ],
    "location": "eastus",
    "macAddress": null,
    "name": "myNic",
    "networkSecurityGroup": {
      "defaultSecurityRules": null,
      "etag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
      "location": null,
      "name": null,
      "networkInterfaces": null,
      "provisioningState": null,
      "resourceGroup": "myResourceGroup",
      "resourceGuid": null,
      "securityRules": null,
      "subnets": null,
      "tags": null,
      "type": null
    },
    "primary": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "b3dbaa0e-2cf2-43be-a814-5cc49fea3304",
    "tags": null,
    "type": "Microsoft.Network/networkInterfaces",
    "virtualMachine": null
  }
}
```


## <a name="create-an-availability-set"></a>Vytvoření skupiny dostupnosti
Dostupnost nastaví nápovědu šíření virtuální počítače napříč domén selhání a aktualizace domény. I když můžete vytvořit pouze jeden virtuální počítač nyní, je nejlepší postup toouse dostupnost sady toomake je snazší tooexpand v budoucnu hello. 

Domén selhání definovat seskupování virtuálních počítačů, které sdílejí společné přepínač zdroje a sítě power. Ve výchozím nastavení hello virtuálních počítačů, které jsou nakonfigurované v rámci vaší sady dostupnosti jsou oddělené v rámci až toothree domén selhání. Problémem hardwaru v jednom z těchto domén selhání nemá vliv na každý virtuální počítač, který běží vaše aplikace.

Aktualizace domén označují skupiny virtuálních počítačů a základní fyzický hardware, který může být restartován v hello stejnou dobu. Během plánované údržby hello pořadí, ve které aktualizace se restartují domény nemusí být po sobě jdoucích, ale po restartu pouze jednu aktualizační doménu najednou.

Virtuální počítače Azure automaticky distribuuje mezi doménami hello selhání a aktualizace, když umístění do nastavení dostupnosti. Další informace najdete v tématu [Správa hello dostupnosti virtuálních počítačů](manage-availability.md).

Vytvořit sadu dostupnosti pro virtuální počítač s [az virtuálních počítačů sady dostupnosti. vytváření](/cli/azure/vm/availability-set#create). Hello následující příklad vytvoří sadu s názvem dostupnosti *myAvailabilitySet*:

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

Hello domén selhání poznámky výstup a aktualizace domény:

```json
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet",
  "location": "eastus",
  "managed": null,
  "name": "myAvailabilitySet",
  "platformFaultDomainCount": 2,
  "platformUpdateDomainCount": 5,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": null,
    "managed": true,
    "tier": null
  },
  "statuses": null,
  "tags": {},
  "type": "Microsoft.Compute/availabilitySets",
  "virtualMachines": []
}
```


## <a name="create-hello-linux-vms"></a>Vytvořit virtuální počítače s Linuxem hello
Hello síťové prostředky toosupport jste vytvořili přístupné z Internetu virtuálních počítačů. Teď vytvořte virtuální počítač a zabezpečte ji pomocí klíče SSH. V tomto případě vytvoříme toocreate virtuálního počítače s Ubuntu podle hello nejnovější LTS. Můžete najít další Image s [seznamu obrázků virtuálních počítačů az](/cli/azure/vm/image#list), jak je popsáno v [hledání Image virtuálního počítače Azure](cli-ps-findimage.md).

Můžeme také určit klíče toouse SSH pro ověřování. Pokud nemáte páru veřejného klíče SSH, můžete [jejich vytvoření](mac-create-ssh-keys.md) nebo použijte hello `--generate-ssh-keys` toocreate parametr je pro vás. Pokud jste již pár klíčů, tento parametr používá existující klíče v `~/.ssh`.

Vytvoření hello virtuálních počítačů tak, že převedou všechny naše prostředky a informace o společně s hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz. Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp*:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --availability-set myAvailabilitySet \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH tooyour virtuálních počítačů s hello záznam DNS, které jste zadali při vytváření hello veřejnou IP adresu. To `fqdn` se zobrazí ve výstupu hello při vytváření virtuálního počítače:

```json
{
  "fqdns": "mypublicdns.eastus.cloudapp.azure.com",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-13-71-C8",
  "powerState": "VM running",
  "privateIpAddress": "192.168.1.5",
  "publicIpAddress": "13.90.94.252",
  "resourceGroup": "myResourceGroup"
}
```

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

Výstup:

```bash
hello authenticity of host 'mypublicdns.eastus.cloudapp.azure.com (13.90.94.252)' can't be established.
ECDSA key fingerprint is SHA256:SylINP80Um6XRTvWiFaNz+H+1jcrKB1IiNgCDDJRj6A.
Are you sure you want toocontinue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.eastus.cloudapp.azure.com,13.90.94.252' (ECDSA) toohello list of known hosts.
Welcome tooUbuntu 16.04.2 LTS (GNU/Linux 4.4.0-81-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


hello programs included with hello Ubuntu system are free software;
hello exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, toohello extent permitted by
applicable law.

toorun a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@myVM:~$
```

Můžete nainstalovat NGINX a najdete v části hello provoz toku toohello virtuálních počítačů. Nainstalujte NGINX následujícím způsobem:

```bash
sudo apt-get install -y nginx
```

toosee hello výchozí NGINX lokality v akci, otevřete webový prohlížeč a zadejte vaše plně kvalifikovaný název domény:

![Výchozí web NGINX na vašem virtuálním počítači](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a>Exportovat jako šablonu
Co když teď chcete toocreate další vývoj prostředí s hello stejnými parametry nebo produkčním prostředí, který odpovídá jeho? Správce prostředků používá šablony JSON, které definují všech hello parametrů pro vaše prostředí. Můžete vytvořit na celé prostředí podle odkazující na tuto šablonu JSON. Můžete [vytvořit šablony JSON ručně](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo exportovat stávající šablonu JSON toocreate hello prostředí pro vás. Použití [export skupiny az](/cli/azure/group#export) tooexport prostředku skupiny takto:

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

Tento příkaz vytvoří hello `myResourceGroup.json` souboru v aktuální pracovní adresář. Při vytváření prostředí z této šablony, zobrazí se výzva pro všechny názvy prostředků hello. Tyto názvy v souboru šablony může vyplnit přidáním hello `--include-parameter-default-value` parametr toohello `az group export` příkaz. Upravit vaše JSON šablony toospecify hello názvy prostředků, nebo [vytvoření souboru parameters.JSON tímto kódem](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) určující názvy prostředků hello.

toocreate prostředí z šablony, použijte [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) následujícím způsobem:

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

Můžete chtít tooread [více o toodeploy ze šablon](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Další informace o tom, jak použít soubor parametrů hello tooincrementally aktualizace prostředí a přístup k šablony jedno umístění úložiště.

## <a name="next-steps"></a>Další kroky
Nyní jste připravené toobegin práce s více síťovými součástmi a virtuálních počítačů. Tato ukázka toobuild prostředí můžete použít se vaše aplikace pomocí sem zavedl hello základní součásti služby.
