---
title: "Virtuální sítě Azure a virtuální počítače s Linuxem | Microsoft Docs"
description: "Kurz – Správa virtuálních sítí Azure a virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2366905b8160675f77cbc41ba97540af70be8c01
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-the-azure-cli"></a>Správa virtuálních sítí Azure a virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure CLI

Virtuální počítače Azure pomocí Azure sítě pro vnitřní a vnější síťovou komunikaci. Tento kurz vás provede nasazením dva virtuální počítače a konfigurace Azure sítě pro tyto virtuální počítače. V příkladech v tomto kurzu předpokládá, že virtuální počítače hostují webové aplikace s databáze back-end, ale není v tomto kurzu nasazené aplikace. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Nasazení virtuální sítě
> * Vytvořte podsíť virtuální sítě.
> * Připojení k podsíti virtuálních počítačů
> * Spravovat veřejné IP adresy virtuálních počítačů
> * Zabezpečené příchozí přenosy z Internetu
> * Zabezpečený virtuální počítač k přenosy virtuálních počítačů


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="vm-networking-overview"></a>Přehled sítě virtuálních počítačů

Virtuální sítě Azure povolit zabezpečené sítě připojení mezi virtuálními počítači, internet a jinými službami Azure, jako je například Azure SQL database. Virtuální sítě jsou rozdělit do logických segmenty označují jako podsítě. Podsítě se používají k řízení toku sítě a jako hranice zabezpečení. Při nasazování virtuálního počítače, obvykle zahrnuje rozhraní virtuální sítě, který je připojen k podsíti.

## <a name="deploy-virtual-network"></a>Nasazení virtuální sítě

V tomto kurzu se vytvoří jedné virtuální sítě se dvěma podsítěmi. Podsíť front-end pro hostování webové aplikace a podsíť back-end pro hostování databázový server.

Než bude možné vytvořit virtuální síť, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myRGNetwork* v eastus umístění.

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a>Vytvoření virtuální sítě

Nám [vytvoření sítě vnet az](/cli/azure/network/vnet#create) příkaz pro vytvoření virtuální sítě. V tomto příkladu je název sítě *mvVnet* a předponu adresy z *10.0.0.0/16*. Podsíť je vytvořen také s názvem *mySubnetFrontEnd* a předponu *10.0.1.0/24*. Později v tomto kurzu je front-end virtuálního počítače připojené k této podsíti. 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnetFrontEnd \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a>Vytvoření podsítě

Novou podsíť je přidaný do virtuální sítě pomocí [az sítě vnet podsíť vytváření](/cli/azure/network/vnet/subnet#create) příkaz. V tomto příkladu je název podsítě *mySubnetBackEnd* a předponu adresy z *10.0.2.0/24*. Tato podsíť se používá u všech služeb back-end.

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --address-prefix 10.0.2.0/24
```

V tomto okamžiku síť byla vytvořil a rozdělena na dvě podsítě, jeden pro front-endové služby a jinou pro back endové služby. V další části jsou virtuální počítače vytvořené a připojené k tyto podsítě.

## <a name="understand-public-ip-address"></a>Pochopení veřejnou IP adresu

Veřejná IP adresa umožňuje prostředků Azure bude přístupný na Internetu. V této části kurzu je virtuální počítač vytvořený na ukazují, jak pracovat s veřejné IP adresy.

### <a name="allocation-method"></a>Metoda přidělování

Veřejné IP adresy mohou být přiděleny jako dynamická nebo statická. Ve výchozím nastavení veřejnou IP adresu přidělí dynamicky. Dynamické IP adresy vydávají, když virtuální počítač je navrácena. To způsobí, že IP adresa, chcete-li změnit během všechny operace, která zahrnuje navrácení virtuálních počítačů.

Metoda přidělení lze nastavit na static, který zajišťuje, že IP adresa zůstanou zařazené do virtuálního počítače, i během deallocated stavu. Pokud používáte staticky přidělená adresa IP, nelze zadat IP adresu. Místo toho je přidělen z fondu adres k dispozici.

### <a name="dynamic-allocation"></a>Dynamické přidělení

Při vytváření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz, je dynamický metodu přidělení výchozí veřejnou IP adresu. V následujícím příkladu se vytvoří virtuální počítač s dynamickou IP adresu. 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myFrontEndVM \
  --vnet-name myVnet \
  --subnet mySubnetFrontEnd \
  --nsg myNSGFrontEnd \
  --public-ip-address myFrontEndIP \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="static-allocation"></a>Statického přidělování

Při vytváření virtuálního počítače pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz, zahrnout `--public-ip-address-allocation static` argument přiřadit statickou veřejnou IP adresu. Tato operace není ukázáno v tomto kurzu, ale v další části se změní dynamicky přidělené IP adresy na staticky přidělené adresy. 

### <a name="change-allocation-method"></a>Změnit přidělení – metoda

Metoda přidělení IP adres se dá změnit pomocí [aktualizace veřejné ip sítě az](/cli/azure/network/public-ip#update) příkaz. V tomto příkladu metoda přidělení IP adresy front-endu virtuálního počítače se změní na statické.

Nejprve zrušit přidělení virtuálního počítače.

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontEndVM
```

Použití [aktualizace veřejné ip sítě az](/cli/azure/network/public-ip#update) příkaz k aktualizaci metoda přidělení. V takovém případě `--allocation-method` je nastavena na *statické*.

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myFrontEndIP --allocation-method static
```

Spusťte virtuální počítač.

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontEndVM --no-wait
```

### <a name="no-public-ip-address"></a>Žádné veřejnou IP adresu

Virtuální počítač se často, nemusí být přístupné přes internet. Chcete-li vytvořit virtuální počítač bez veřejnou IP adresu, použijte `--public-ip-address ""` argument s prázdnou sadu dvojité uvozovky. Tato konfigurace je ukázán později v tomto kurzu

## <a name="secure-network-traffic"></a>Zabezpečení provozu sítě

Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel zabezpečení, která prostředkům připojeným k virtuálním sítím Azure povolují nebo odpírají síťový provoz. Skupiny Nsg můžou být přidružena k podsítě nebo jednotlivých síťových rozhraní. Když skupinu NSG je spojen s síťovým rozhraním, bude se vztahovat jenom přidružený virtuální počítač. Pokud je skupina zabezpečení sítě přidružená k podsíti, pravidla se vztahují na všechny prostředky, které jsou připojené k příslušné podsíti. 

### <a name="network-security-group-rules"></a>Pravidla skupiny zabezpečení sítě

Pravidla NSG definovat síťové porty, přes které provoz povolený nebo zakázaný. Pravidla mohou obsahovat zdrojové a cílové rozsahy IP adres, tak, aby je řízen provoz mezi konkrétní systémy nebo podsítě. Pravidla NSG také zahrnovat prioritu (mezi 1 – a 4096). Pravidla jsou vyhodnocovány v pořadí podle priority. Pravidlo s prioritou 100 vyhodnotí před pravidlo s prioritou 200.

Všechny skupiny NSG obsahují sadu výchozích pravidel. Výchozí pravidla se nedají odstranit, ale protože je jim přiřazená nejnižší priorita, dají se přepsat pravidly, která vytvoříte.

- **Virtuální síť** – provoz pocházející a ukončování ve virtuální síti je povolena v příchozí a odchozí.
- **Internet** – odchozí provoz je povolený, ale jsou blokovány příchozí přenosy.
- **Nástroj pro vyrovnávání zatížení** – nástroj pro vyrovnávání zatížení povolit Azure testovat stav virtuálních počítačů a instancí rolí. Pokud nepoužíváte skupinu s vyrovnáváním zatížení, můžete přepsat toto pravidlo.

### <a name="create-network-security-groups"></a>Vytvoření skupiny zabezpečení sítě

Lze vytvořit skupinu zabezpečení sítě ve stejnou dobu jako virtuální počítač pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz. Když to uděláte, se NSG k rozhraní sítě virtuálních počítačů a automaticky vytvořit, aby povolovala přenosy na portu je pravidlo NSG *22* z jakéhokoli zdroje. V tomto kurzu front-end NSG se automaticky vytvořené s front-endu virtuálního počítače. Pravidlo NSG se také automaticky vytvořenou pro port 22. 

V některých případech může být užitečné předem vytvořit skupinu NSG, například když by neměly být vytvářeny výchozí pravidla pro SSH, nebo když by mělo být připojeno NSG k podsíti. 

Použití [vytvořit az sítě nsg](/cli/azure/network/nsg#create) příkazu vytvořte skupinu zabezpečení sítě.

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myNSGBackEnd
```

Místo přidružení skupiny NSG k síťové rozhraní, je přidružená k podsíti. V této konfiguraci dědí žádné virtuální počítače, který je připojen k podsíti pravidla NSG.

Aktualizovat existující podsíť s názvem *mySubnetBackEnd* se nová skupina NSG.

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --network-security-group myNSGBackEnd
```

Teď vytvořte virtuální počítač, který je připojen k *mySubnetBackEnd*. Všimněte si, že `--nsg` argument má hodnotu prázdný dvojité uvozovky. Skupina NSG nemusí být vytvořen s virtuálním Počítačem. Virtuální počítač je připojený k podsíti back-end, který je chráněn s předem vytvořené NSG back-end. Tato skupina NSG se vztahuje na virtuální počítač. Také si zde všimnout, který `--public-ip-address` argument má hodnotu prázdný dvojité uvozovky. Tato konfigurace vytvoří virtuální počítač bez veřejnou IP adresu. 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myBackEndVM \
  --vnet-name myVnet \
  --subnet mySubnetBackEnd \
  --public-ip-address "" \
  --nsg "" \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="secure-incoming-traffic"></a>Zabezpečené příchozí provoz

V okamžiku vytvoření front-endu virtuálního počítače bylo pravidlo NSG vytvořeno povolit příchozí přenosy na portu 22. Toto pravidlo umožňuje připojení SSH pro virtuální počítač. V tomto příkladu má být povoleno přenosy na portu *80*. Tato konfigurace umožňuje webovou aplikaci nelze přistupovat ve virtuálním počítači.

Použití [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) příkaz k vytvoření pravidla pro port *80*.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGFrontEnd \
  --name http \
  --access allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80
```

Front-endu virtuální počítač je nyní pouze přístupné na portu *22* a port *80*. Všechny ostatní příchozí přenosy je blokováno v skupinu zabezpečení sítě. Může být užitečné k vizualizaci konfigurace pravidel NSG. Konfigurace pravidla NSG se vraťte [az sítě pravidlo seznamu](/cli/azure/network/nsg/rule#list) příkaz. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGFrontEnd --output table
```

Výstup:

```azurecli-interactive 
Access    DestinationAddressPrefix      DestinationPortRange  Direction    Name                 Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -----------------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                                               22  Inbound      default-allow-ssh        1000  Tcp         Succeeded            myRGNetwork      *                      *
Allow     *                                               80  Inbound      http                      200  Tcp         Succeeded            myRGNetwork      *                      *
```

### <a name="secure-vm-to-vm-traffic"></a>Zabezpečený virtuální počítač k přenosy virtuálních počítačů

Pravidla skupiny zabezpečení sítě můžete také použít mezi virtuálními počítači. V tomto příkladu musí front-end virtuálních počítačů komunikovat s back-end virtuálního počítače na portu *22* a *3306*. Tato konfigurace umožňuje připojení SSH z front-endu virtuálního počítače a také povolit aplikace front-endu virtuálního počítače ke komunikaci s databází MySQL back-end. Všechny ostatní přenosy by se zablokovat mezi virtuálními počítači front-end a back-end.

Použití [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) příkaz k vytvoření pravidla pro port 22. Všimněte si, že `--source-address-prefix` argument určuje hodnotu *10.0.1.0/24*. Tato konfigurace zajistí, že jsou povoleny pouze přenosy z podsítě front-endu prostřednictvím NSG.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name SSH \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "22"
```

Teď můžete přidáte pravidlo pro provoz MySQL na portu 3306.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name MySQL \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "3306"
```

Navíc vzhledem k tomu, že skupiny Nsg výchozí pravidlo povolující veškerý provoz mezi virtuálními počítači ve stejné virtuální síti, lze vytvořit pravidlo pro skupiny Nsg back-end zablokovat veškerý provoz. Všimněte si zde, že `--priority` je zadána hodnota *300*, která je nižší tohoto pravidla NSG i MySQL. Tato konfigurace zajistí, že provoz SSH a MySQL stále může prostřednictvím NSG.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name denyAll \
  --access Deny \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "*"
```

Je back-end virtuální počítač nyní pouze dostupný na portu *22* a port *3306* z podsítě front-endu. Všechny ostatní příchozí přenosy je blokováno v skupinu zabezpečení sítě. Může být užitečné k vizualizaci konfigurace pravidel NSG. Konfigurace pravidla NSG se vraťte [az sítě pravidlo seznamu](/cli/azure/network/nsg/rule#list) příkaz. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGBackEnd --output table
```

Výstup:

```azurecli-interactive 
Access    DestinationAddressPrefix    DestinationPortRange    Direction    Name       Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                           22                      Inbound      SSH             100  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Allow     *                           3306                    Inbound      MySQL           200  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Deny      *                           *                       Inbound      denyAll         300  Tcp         Succeeded            myRGNetwork      *                      *
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili a zabezpečené sítě v souvislosti s virtuálními počítači Azure. Jste se dozvěděli, jak na:

> [!div class="checklist"]
> * Nasazení virtuální sítě
> * Vytvořte podsíť virtuální sítě.
> * Připojení k podsíti virtuálních počítačů
> * Spravovat veřejné IP adresy virtuálních počítačů
> * Zabezpečené příchozí přenosy z Internetu
> * Zabezpečený virtuální počítač k přenosy virtuálních počítačů

Přechodu na v dalším kurzu se dozvíte o zabezpečení dat na virtuální počítače pomocí zálohování Azure. 

> [!div class="nextstepaction"]
> [Zálohovat virtuální počítače s Linuxem v Azure](./tutorial-backup-vms.md)
