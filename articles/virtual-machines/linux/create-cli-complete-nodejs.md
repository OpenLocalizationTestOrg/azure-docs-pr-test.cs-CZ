---
title: "aaaCreate dokončení prostředí Linux s hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvoření úložiště, virtuální počítač s Linuxem, virtuální síť a podsíť, nástroj pro vyrovnávání zatížení, seskupování, veřejnou IP adresu a skupinu zabezpečení sítě z hello pozadí pomocí hello Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 7fe00e138704fe9c9a1c9b87a7dd1afd6174e527
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-complete-linux-environment-with-hello-azure-cli-10"></a>Vytvořte prostředí dokončení Linux s hello Azure CLI 1.0
V tomto článku jsme sestavení jednoduchá síť se nástroj pro vyrovnávání zatížení a dvojice virtuálních počítačů, které jsou užitečné pro vývoj a jednoduchý computing. Jsme provede postupem hello příkazu command, dokud nebudete mít dvě pracovní, zabezpečené toowhich virtuální počítače s Linuxem, se můžete připojit z kdekoliv na Internetu hello. Potom můžete přesunout na toomore složité sítě a prostředí.

Společně hello způsob můžete další informace o hierarchii závislostí hello hello modelu nasazení Resource Manager nabízí, a o tom, kolik power poskytuje. Jakmile uvidíte, jak se sestaví systém hello, můžete znovu sestavit je mnohem rychlejší pomocí [šablon Azure Resource Manageru](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Kromě toho po zjistíte, jak jsou navzájem propojené hello části vašeho prostředí, vytváření šablony tooautomate je jednodušší.

Hello prostředí obsahuje:

* Dva virtuální počítače uvnitř skupiny dostupnosti.
* Vyrovnávání zatížení s pravidlem Vyrovnávání zatížení na portu 80.
* Skupina zabezpečení sítě (NSG) pravidla tooprotect virtuálního počítače z nevyžádaný provoz.

toocreate tento vlastní prostředí, je nutné hello nejnovější [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) v režimu Resource Manager (`azure config mode arm`). Musíte taky analýza nástroj JSON. Tento příklad používá [jq](https://stedolan.github.io/jq/).


## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello


## <a name="quick-commands"></a>Rychlé příkazy
Pokud je třeba tooquickly dosáhnout hello, následující části Podrobnosti hello hello základní příkazy tooupload tooAzure virtuálních počítačů. Podrobnější informace a kontext pro každý krok lze nalézt v hello zbytek dokumentu hello od [zde](#detailed-walkthrough).

Ujistěte se, že máte [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) přihlášení a použití režimu Resource Manager:

```azurecli
azure config mode arm
```

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.

Vytvořte skupinu prostředků hello. Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westeurope` umístění:

```azurecli
azure group create -n myResourceGroup -l westeurope
```

Skupina prostředků hello ověřte pomocí analyzátoru hello JSON:

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Vytvořte účet úložiště hello. Hello následující příklad vytvoří účet úložiště s názvem `mystorageaccount`. (hello název účtu úložiště musí být jedinečný, takže zadejte svůj vlastní jedinečný název.)

```azurecli
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Účet úložiště hello ověřte pomocí analyzátoru hello JSON:

```azurecli
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Vytvoření virtuální sítě hello. Hello následující příklad vytvoří virtuální síť s názvem `myVnet`:

```azurecli
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Vytvoření podsítě. Hello následující příklad vytvoří podsíť s názvem `mySubnet`:

```azurecli
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Ověřte hello virtuální síť a podsíť pomocí analyzátoru hello JSON:

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Vytvořte veřejnou IP adresu. Hello následující příklad vytvoří veřejnou IP adresu s názvem `myPublicIP` s názvem DNS hello `mypublicdns`. (název DNS hello musí být jedinečný, takže zadejte svůj vlastní jedinečný název.)

```azurecli
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Vytvořte nástroj pro vyrovnávání zatížení hello. Hello následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem `myLoadBalancer`:

```azurecli
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Vytvořte fond IP front-endu nástroje pro vyrovnávání zatížení hello a přidružte veřejnou IP adresu hello. Hello následující příklad vytvoří front-end IP fond s názvem `mySubnetPool`:

```azurecli
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool
```

Vytvořte fond IP hello back-end pro vyrovnávání zatížení hello. Hello následující příklad vytvoří fond back-end IP s názvem `myBackEndPool`:

```azurecli
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Vytvoření pravidla překladu adres nástroje pro vyrovnávání zatížení hello příchozích síťových SSH. Hello následující příklad vytvoří dvě pravidla nástroje pro vyrovnávání zatížení, `myLoadBalancerRuleSSH1` a `myLoadBalancerRuleSSH2`:

```azurecli
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Vytvoření webové hello příchozích pravidel NAT pro hello nástroj pro vyrovnávání zatížení. Hello následující příklad vytvoří pravidlo Vyrovnávání zatížení s názvem `myLoadBalancerRuleWeb`:

```azurecli
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Vytvořte test stavu nástroje pro vyrovnávání zatížení hello. Hello následující příklad vytvoří sondou TCP s názvem `myHealthProbe`:

```azurecli
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Ověřte hello nástroj pro vyrovnávání zatížení, fondy IP adres a pravidla NAT pomocí analyzátoru hello JSON:

```azurecli
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Vytvořte hello první síťová karta (NIC). Nahraďte hello `#####-###-###` oddíly s vlastními ID předplatného Azure. Vaše předplatné ID je uvedeno v výstup hello **jq** při kontrole hello prostředků, kterou vytváříte. Můžete také zobrazit svoje ID předplatného s `azure account list`.

Hello následující příklad vytvoří síťový adaptér s názvem `myNic1`:

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Vytvoření hello druhý síťový adaptér. Hello následující příklad vytvoří síťový adaptér s názvem `myNic2`:

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Ověřte hello dva síťové adaptéry pomocí analyzátoru hello JSON:

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Vytvořte skupinu zabezpečení sítě hello. Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:

```azurecli
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Přidejte dva příchozích pravidel pro skupinu zabezpečení sítě hello. Hello následující příklad vytvoří dvě pravidla `myNetworkSecurityGroupRuleSSH` a `myNetworkSecurityGroupRuleHTTP`:

```azurecli
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Ověřte hello skupinu zabezpečení sítě a příchozích pravidel pomocí analyzátoru hello JSON:

```azurecli
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

Vazby zabezpečení sítě hello skupiny toohello dva síťové adaptéry:

```azurecli
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Vytvořte skupinu dostupnosti hello. Hello následující příklad vytvoří sadu s názvem dostupnosti `myAvailabilitySet`:

```azurecli
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

Vytvoření hello první virtuální počítač s Linuxem. Hello následující příklad vytvoří virtuální počítač s názvem `myVM1`:

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

Vytvoření hello druhé virtuálního počítače s Linuxem. Hello následující příklad vytvoří virtuální počítač s názvem `myVM2`:

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

Použijte hello JSON analyzátor tooverify, že vše, co byl vytvořený:

```azurecli
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Exportujte vaší nové prostředí tooa šablony tooquickly znovu vytvořit nové instance služby:

```azurecli
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Podrobný postup
Hello podrobné kroky, které následují popisují, co každý příkaz probíhá při sestavování na vašem prostředí. Tyto koncepty jsou užitečné, když vytváříte vlastní vlastní prostředí pro vývoj nebo produkčního prostředí.

Ujistěte se, že máte [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) přihlášení a použití režimu Resource Manager:

```azurecli
azure config mode arm
```

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Vytvoření skupiny prostředků a vyberte umístění nasazení
Skupiny prostředků Azure jsou logické nasazení entity, které obsahují konfigurační informace metadat tooenable hello logické správu a nasazení prostředků. Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westeurope` umístění:

```azurecli
azure group create --name myResourceGroup --location westeurope
```

Výstup:

```azurecli                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>vytvořit účet úložiště
Je třeba účty úložiště pro disky virtuálních počítačů a jakýchkoli dalších datových disků, které chcete tooadd. Můžete vytvořit účty úložiště téměř okamžitě po vytvoření skupiny prostředků.

Tady používáme hello `azure storage account create` příkazu předávání hello umístění skupiny prostředků hello hello účtu, který řídí a typ úložiště s podporou chcete hello. Hello následující příklad vytvoří účet úložiště s názvem `mystorageaccount`:

```azurecli
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Výstup:

```azurecli
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Skupina naše prostředků pomocí hello tooexamine `azure group show` příkaz, použijeme hello [jq](https://stedolan.github.io/jq/) nástroj společně s hello `--json` možnost příkazového řádku Azure CLI. (Můžete použít **jsawk** nebo žádnou knihovnu jazyk dáváte přednost tooparse hello JSON.)

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Výstup:

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

účet úložiště tooinvestigate hello pomocí hello rozhraní příkazového řádku, musíte nejprve názvy účtů hello tooset a klíče. Nahraďte hello název účtu úložiště hello v hello následující ukázka s názvem, který zvolíte:

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Potom můžete zobrazit informace z vašeho úložiště snadno:

```azurecli
azure storage container list
```

Výstup:

```azurecli
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Vytvoření virtuální sítě a podsítě
Dále budete tooneed toocreate virtuální síti se spuštěnou ve službě Azure a podsíť, ve kterém můžete vytvořit virtuální počítače. Hello následující příklad vytvoří virtuální síť s názvem `myVnet` s hello `192.168.0.0/16` předpona adresy:

```azurecli
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16
```

Výstup:

```azurecli
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Znovu, můžeme použít možnost--json hello `azure group show` a `jq` toosee jak vytváříme naše prostředky. Nyní je k dispozici `storageAccounts` prostředků a `virtualNetworks` prostředků.  

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Výstup:

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Nyní Pojďme vytvořit podsíť v hello `myVnet` virtuální sítě, do které hello nasazených virtuálních počítačů. Používáme hello `azure network vnet subnet create` příkazu společně s hello prostředky, které jste už vytvořili: hello `myResourceGroup` skupinu prostředků a hello `myVnet` virtuální sítě. V následujícím příkladu hello, přidáme hello podsíť s názvem `mySubnet` s předponu adresy podsítě hello `192.168.1.0/24`:

```azurecli
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Výstup:

```azurecli
info:    Executing command network vnet subnet create
+ Looking up hello subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up hello subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Protože je ve virtuální síti hello logicky hello podsíť, podíváme pro informace o podsíti hello mírně odlišné příkazem. příkaz Hello používáme je `azure network vnet show`, abychom mohli pokračovat výstup JSON hello tooexamine pomocí, ale `jq`.

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Výstup:

```json
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address"></a>Vytvoření veřejné IP adresy
Nyní vytvoříme hello veřejná IP adresa (PIP), jsme přiřadíte tooyour nástroj pro vyrovnávání zatížení. Umožňuje tooconnect tooyour virtuální počítače z hello Internetu pomocí hello `azure network public-ip create` příkaz. Protože hello výchozí adresa je dynamická, se nám vytvořit položku s názvem DNS v hello **cloudapp.azure.com** domény pomocí hello `--domain-name-label` možnost. Hello následující příklad vytvoří veřejnou IP adresu s názvem `myPublicIP` s názvem DNS hello `mypublicdns`. Vzhledem k tomu, že název DNS hello musí být jedinečný, zadáte svůj vlastní jedinečný název DNS:

```azurecli
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Výstup:

```azurecli
info:    Executing command network public-ip create
+ Looking up hello public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up hello public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

Hello veřejná IP adresa je také prostředek nejvyšší úrovně, zobrazí se s `azure group show`.

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Výstup:

```json
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

Můžete prozkoumat další podrobnosti prostředků, včetně hello plně kvalifikovaný název domény (FQDN) hello poddomény pomocí hello dokončení `azure network public-ip show` příkaz. logicky byl přidělen prostředek Hello veřejné IP adresy, ale ještě nebyly přiřazeny konkrétní adresu. tooobtain IP adresu, budete tooneed Vyrovnávání zatížení, které jste dosud nevytvořili.

```azurecli
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Výstup:

```json
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Vytvořit nástroj pro vyrovnávání zatížení a fondy IP adres
Když vytvoříte Vyrovnávání zatížení, umožní vám toodistribute provoz napříč více virtuálními počítači. Také poskytuje redundanci tooyour aplikace s spuštění několika virtuálních počítačů, které reagují toouser požadavky v případě hello údržby nebo velkým zatížením. Hello následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem `myLoadBalancer`:

```azurecli
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Výstup:

```azurecli
info:    Executing command network lb create
+ Looking up hello load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Naše nástroj pro vyrovnávání zatížení je docela prázdná, takže vytvoříme některé fondy IP adres. Chceme toocreate dvěma fondy IP adres pro naše nástroj pro vyrovnávání zatížení, jednu pro hello front-endu a jeden pro hello back-end. fond IP front-endu Hello je veřejně viditelný. Je také toowhich hello umístění, které jsme přiřadit hello PIP, kterou jsme vytvořili předtím. Potom používáme fond back-end hello jako umístění pro naše tooconnect virtuálních počítačů k. Tímto způsobem můžete hello provoz procházet skrz toohello nástroje pro vyrovnávání zatížení hello virtuálních počítačů.

Nejdříve vytvoříme naše front-end fond IP adres. Hello následující příklad vytvoří front-end fond s názvem `myFrontEndPool`:

```azurecli
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool
```

Výstup:

```azurecli
info:    Executing command network lb frontend-ip create
+ Looking up hello load balancer "myLoadBalancer"
+ Looking up hello public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Všimněte si, jak jsme použili hello `--public-ip-name` přepínač toopass ve hello `myPublicIP` kterou jsme vytvořili předtím. Přiřazení hello veřejnou IP adresu pro vyrovnávání zatížení toohello vám umožní tooreach virtuální počítače napříč hello Internetu.

V dalším kroku vytvoříme naše druhý fond IP této doby provozu našich back-end. Hello následující příklad vytvoří fond back-end, s názvem `myBackEndPool`:

```azurecli
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Výstup:

```azurecli
info:    Executing command network lb address-pool create
+ Looking up hello load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Vidíme úspěšnost naše nástroj pro vyrovnávání zatížení tak, že vyhledá s `azure network lb show` a prozkoumání výstup hello JSON:

```azurecli
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Výstup:

```json
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Vytvoření pravidla NAT nástroje pro vyrovnávání zatížení
tooget provoz v našich Vyrovnávání zatížení, potřebujeme toocreate síťových adres překlad (NAT) pravidla určující příchozí nebo odchozí akce. Můžete zadat toouse hello protokol a pak mapování portů toointernal externí porty podle potřeby. Pro naše prostředí vytvoříme některá pravidla, které umožňují SSH pomocí našich tooour nástroje pro vyrovnávání zatížení virtuálních počítačů. Nastaví port TCP porty 4222 a 4223 toodirect tooTCP 22 na našem virtuálních počítačích (které vytvoříme později). Hello následující příklad vytvoří pravidlo s názvem `myLoadBalancerRuleSSH1` toomap TCP port 4222 tooport 22:

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Výstup:

```azurecli
info:    Executing command network lb inbound-nat-rule create
+ Looking up hello load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Opakujte postup hello druhého pravidla NAT u SSH. Hello následující příklad vytvoří pravidlo s názvem `myLoadBalancerRuleSSH2` toomap TCP port 4223 tooport 22:

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Umožňuje také pokračujte a vytvořte tak pravidlo NAT pro port TCP 80 webových přenosů, pravidlo hello zapojování tooour fondy IP adres. Pokud jsme propojte hello pravidlo tooan fondu IP, namísto zapojování hello pravidlo tooour virtuálních počítačů jednotlivě, jsme můžete přidat nebo odebrat z fondu IP adres hello virtuálních počítačů. Nástroj pro vyrovnávání zatížení Hello automaticky upraví hello tok provozu. Hello následující příklad vytvoří pravidlo s názvem `myLoadBalancerRuleWeb` toomap TCP port 80 tooport 80:

```azurecli
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Výstup:

```azurecli
info:    Executing command network lb rule create
+ Looking up hello load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.
Stavu testů pravidelně kontroly na hello virtuálních počítačů, které jsou za naše toomake nástroje pro vyrovnávání zatížení se svém operační a odpovídá toorequests, jak jsou definovány. Pokud ne, jste odebrali z tooensure operaci, která nejsou uživatelé se přesměruje toothem. Můžete definovat vlastní kontroly pro test stavu hello, spolu s intervalech a hodnoty časového limitu. Další informace o sondy stavu najdete v tématu [nástroj pro vyrovnávání zatížení sondy](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Hello následující příklad vytvoří TCP stavu zjištěný pojmenované `myHealthProbe`:

```azurecli
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Výstup:

```azurecli
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up hello load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Zde jsme zadali interval 15 sekund pro naše kontroly stavu. Jsme můžete neproběhly maximálně čtyři sondy (jedné minuty) předtím, než nástroj pro vyrovnávání zatížení hello domnívá, že tohoto hostitele hello již není funkční.

## <a name="verify-hello-load-balancer"></a>Ověřte hello nástroj pro vyrovnávání zatížení
Nyní se provádí konfigurace služby Vyrovnávání zatížení hello. Zde jsou hello kroky, které jste pořídili:

1. Vytvořili jste nástroj pro vyrovnávání zatížení.
2. Vytvořili front-end IP fond a přiřazení veřejné tooit IP.
3. Můžete vytvořit fond back-end IP, které se mohou připojit virtuální počítače.
4. Můžete vytvořit pravidla NAT, které umožňují SSH toohello virtuálních počítačů pro správu, společně s pravidlo, které umožní port TCP 80 pro webovou aplikaci.
5. Jste přidali stav testu tooperiodically kontrola hello virtuálních počítačů. Tento test stavu zajistí, že si uživatelé pokusí tooaccess virtuální počítač, který je už funguje nebo poskytování obsahu.

Pojďme si shrnout, co nástroj pro vyrovnávání zatížení vypadá jako nyní:

```azurecli
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Výstup:

```json
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-toouse-with-hello-linux-vm"></a>Vytvoření toouse síťový adaptér s hello virtuálního počítače s Linuxem
Síťové adaptéry jsou prostřednictvím kódu programu k dispozici, protože můžete použít pravidla tootheir použití. Také můžete mít více než jednu. V následující hello `azure network nic create` příkaz spojit fond hello zatížení toohello síťový adaptér back-end IP adres a přidružte ji k hello NAT pravidlo toopermit SSH provoz.

Nahraďte hello `#####-###-###` oddíly s vlastními ID předplatného Azure. Vaše předplatné ID je uvedeno v výstup hello `jq` při kontrole hello prostředků, kterou vytváříte. Můžete také zobrazit svoje ID předplatného s `azure account list`.

Hello následující příklad vytvoří síťový adaptér s názvem `myNic1`:

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Výstup:

```azurecli
info:    Executing command network nic create
+ Looking up hello subnet "mySubnet"
+ Looking up hello network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Zobrazí se podrobnosti hello prověřením přímo hello prostředků. Zkontrolujte hello prostředků pomocí hello `azure network nic show` příkaz:

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Výstup:

```json
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Teď vytvoříme hello druhý síťový adaptér, zapojování ve fondu back-end IP tooour znovu. Tento čas hello druhé pravidlo NAT povolí provoz SSH. Hello následující příklad vytvoří síťový adaptér s názvem `myNic2`:

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Vytvořte skupinu zabezpečení sítě a pravidla
Vytvořit skupinu zabezpečení sítě a hello příchozí teď pravidla, která řídí přístup k toohello síťový adaptér. Skupina zabezpečení sítě může být použité tooa síťového adaptéru nebo podsítě. Můžete definovat pravidla toocontrol hello tok přenosů dat do aplikace a z virtuálních počítačů. Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:

```azurecli
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Přidejme hello příchozí pravidlo pro hello NSG tooallow příchozí připojení na portu 22 (toosupport SSH). Hello následující příklad vytvoří pravidlo s názvem `myNetworkSecurityGroupRuleSSH` tooallow TCP na portu 22:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Nyní Pojďme přidat příchozí pravidlo hello hello NSG tooallow příchozí připojení na portu 80 (toosupport webový provoz). Hello následující příklad vytvoří pravidlo s názvem `myNetworkSecurityGroupRuleHTTP` tooallow TCP na portu 80:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [!NOTE]
> Příchozí pravidlo Hello je filtr pro příchozí síťová připojení. V tomto příkladu jsme vazbu hello NSG toohello virtuální počítače virtuální síťovou kartu, což znamená, že všechny žádosti o tooport 22 předána toohello síťový adaptér na našem virtuálních počítačů. Toto pravidlo příchozí je o připojení k síti a není o koncový bod, který je co by bylo o v nasazení classic. tooopen port, musí zůstat hello `--source-port-range` nastavit příliš '\*' tooaccept (hello výchozí hodnota) příchozí požadavky od **žádné** požaduje portu. Porty jsou obvykle dynamická.
>
>

## <a name="bind-toohello-nic"></a>Vytvoření vazby toohello síťový adaptér
Vytvoření vazby hello NSG toohello síťových karet. Je nutné tooconnect naše síťové adaptéry s naše skupinu zabezpečení sítě. Spuštění obou příkazů, toohook nahoru i naše síťových karet:

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Vytvoření skupiny dostupnosti
Dostupnost nastaví nápovědu šíření virtuální počítače napříč domén selhání a domén upgradu. Umožňuje vytvořit sadu dostupnosti pro virtuální počítače. Hello následující příklad vytvoří sadu s názvem dostupnosti `myAvailabilitySet`:

```azurecli
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Domén selhání definovat seskupování virtuálních počítačů, které sdílejí společné přepínač zdroje a sítě power. Ve výchozím nastavení hello virtuálních počítačů, které jsou nakonfigurované v rámci vaší sady dostupnosti jsou oddělené v rámci až toothree domén selhání. Rada Hello je, že problémem hardwaru v jednom z těchto domén selhání nemá vliv na každý virtuální počítač, který běží vaše aplikace. Virtuální počítače Azure automaticky distribuuje mezi doménami selhání hello, když umístění do nastavení dostupnosti.

Domén upgradu označují skupiny virtuálních počítačů a základní fyzický hardware, který může být restartován v hello stejnou dobu. Hello pořadí, ve kterém se restartují upgradu domény nemusí být po sobě jdoucích během plánované údržby, ale jenom jeden upgradu po restartu najednou. Znovu Azure automaticky distribuuje virtuální počítače napříč doménami upgradu, když umístění do stránku dostupnosti.

Další informace o [Správa hello dostupnosti virtuálních počítačů](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="create-hello-linux-vms"></a>Vytvořit virtuální počítače s Linuxem hello
Úložiště a síťové prostředky hello jste vytvořili virtuální počítače toosupport přístupné z Internetu. Nyní Pojďme vytvořit ty virtuální počítače a zabezpečit protokolem klíč SSH, který nemá heslo. V tomto případě vytvoříme toocreate virtuálního počítače s Ubuntu podle hello nejnovější LTS. Nemůžeme najít informace o tomto obrázku pomocí `azure vm image list`, jak je popsáno v [hledání Image virtuálního počítače Azure](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Jsme vybrali bitovou kopii pomocí příkazu hello `azure vm image list westeurope canonical | grep LTS`. V tomto případě používáme `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. V poli poslední hello jsme předat `latest` tak, aby v budoucnu hello nám vždycky získat nejnovější sestavení hello. (hello řetězec používáme je `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Tento další krok je známé tooanyone, který již vytvořen ssh veřejného a privátního klíče rsa spárujte na systému Linux nebo Mac. pomocí **ssh-keygen - t rsa -b 2048**. Pokud nemáte žádné páry klíčů certifikátů vaší `~/.ssh` adresáře, můžete je vytvořit:

* Automaticky pomocí hello `azure vm create --generate-ssh-keys` možnost.
* Ručně pomocí [toocreate pokyny hello je sami](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Alternativně můžete použít hello `--admin-password` tooauthenticate metoda vytvoření připojení SSH po hello virtuálních počítačů. Tato metoda je obvykle méně bezpečné.

Vytvoříme hello virtuálních počítačů tak, že převedou všechny naše prostředky a informace o společně s hello `azure vm create` příkaz:

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

Výstup:

```azurecli
info:    Executing command vm create
+ Looking up hello VM "myVM1"
info:    Verifying hello public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using hello VM Size "Standard_DS1"
info:    hello [OS, Data] Disk or image configuration requires storage account
+ Looking up hello storage account mystorageaccount
+ Looking up hello availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up hello NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in hello NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    hello storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by hello parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Tooyour virtuálních počítačů můžete okamžitě připojit pomocí klíče SSH výchozí. Ujistěte se, zadejte odpovídající port hello, protože jsme se prošla hello nástroj pro vyrovnávání zatížení. (Pro naše první virtuální počítač, nastavíme hello NAT pravidlo tooforward port 4222 tooour virtuálního počítače.)

```bash
ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Výstup:

```bash
hello authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want toocontinue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) toohello list of known hosts.
Welcome tooUbuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Pokračujte a vytvořit druhý virtuální počítač v hello stejným způsobem:

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

A teď můžete použít hello `azure vm show myResourceGroup myVM1` příkaz tooexamine, co jste vytvořili. V tomto okamžiku používáte Ubuntu virtuálních počítačů za službou Vyrovnávání zatížení v Azure, který můžete se přihlaste pouze s dvojici klíčů SSH (protože hesla jsou zakázané). Můžete nainstalovat nginx nebo httpd, nasazení webové aplikace a zobrazit provoz hello toku prostřednictvím tooboth nástroje pro vyrovnávání zatížení hello hello virtuálních počítačů.

```azurecli
azure vm show --resource-group myResourceGroup --name myVM1
```

Výstup:

```azurecli
info:    Executing command vm show
+ Looking up hello VM "TestVM1"
+ Looking up hello NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-hello-environment-as-a-template"></a>Export hello prostředí jako šablonu
Teď, který jste vytvořili na tomto prostředí, jak postupovat, pokud chcete, aby toocreate další vývoj prostředí s hello stejnými parametry nebo produkčním prostředí, který odpovídá jeho? Správce prostředků používá šablony JSON, které definují všech hello parametrů pro vaše prostředí. Můžete vytvořit na celé prostředí podle odkazující na tuto šablonu JSON. Můžete [vytvořit šablony JSON ručně](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo exportovat stávající šablonu JSON toocreate hello prostředí pro vás:

```azurecli
azure group export --name myResourceGroup
```

Tento příkaz vytvoří hello `myResourceGroup.json` souboru v aktuální pracovní adresář. Při vytváření prostředí z této šablony, zobrazí se výzva pro všechny názvy prostředků hello, včetně hello názvy pro nástroj pro vyrovnávání zatížení hello síťových rozhraní a virtuální počítače. Tyto názvy v souboru šablony může vyplnit přidáním hello `-p` nebo `--includeParameterDefaultValue` parametr toohello `azure group export` příkaz, který byl dříve vidět. Upravit vaše JSON šablony toospecify hello názvy prostředků, nebo [vytvoření souboru parameters.JSON tímto kódem](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) určující názvy prostředků hello.

toocreate prostředí z šablony:

```azurecli
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Můžete chtít tooread [více o toodeploy ze šablon](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Další informace o tom, jak použít soubor parametrů hello tooincrementally aktualizace prostředí a přístup k šablony jedno umístění úložiště.

## <a name="next-steps"></a>Další kroky
Nyní jste připravené toobegin práce s více síťovými součástmi a virtuálních počítačů. Tato ukázka toobuild prostředí můžete použít se vaše aplikace pomocí sem zavedl hello základní součásti služby.
