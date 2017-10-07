---
title: "aaaAzure virtuálních sítí a virtuální počítače s Linuxem | Microsoft Docs"
description: "Kurz – Správa virtuálních sítí Azure a virtuální počítače Linux s hello rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: 57e6bd4de16f0e31d53dc67bf50dc5730d43712b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-hello-azure-cli"></a>Správa virtuálních sítí Azure a virtuální počítače Linux s hello rozhraní příkazového řádku Azure

Virtuální počítače Azure pomocí Azure sítě pro vnitřní a vnější síťovou komunikaci. Tento kurz vás provede nasazením dva virtuální počítače a konfigurace Azure sítě pro tyto virtuální počítače. Hello příklady v tomto kurzu předpokládají, že virtuální počítače hello hostují webové aplikace s databáze back-end, ale aplikace není nasazena v kurzu hello. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Nasazení virtuální sítě
> * Vytvořte podsíť virtuální sítě.
> * Připojte tooa podsíť virtuálních počítačů
> * Spravovat veřejné IP adresy virtuálních počítačů
> * Zabezpečené příchozí přenosy z Internetu
> * Zabezpečení přenosu tooVM virtuálních počítačů


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="vm-networking-overview"></a>Přehled sítě virtuálních počítačů

Virtuální sítě Azure povolit zabezpečené sítě připojení mezi virtuálními počítači, hello Internetu a jinými službami Azure, jako je například Azure SQL database. Virtuální sítě jsou rozdělit do logických segmenty označují jako podsítě. Podsítě se používají toocontrol tok sítě a jako hranice zabezpečení. Při nasazování virtuálního počítače, obvykle zahrnuje rozhraní virtuální sítě, které je připojené tooa podsítě.

## <a name="deploy-virtual-network"></a>Nasazení virtuální sítě

V tomto kurzu se vytvoří jedné virtuální sítě se dvěma podsítěmi. Podsíť front-end pro hostování webové aplikace a podsíť back-end pro hostování databázový server.

Než bude možné vytvořit virtuální síť, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myRGNetwork* v umístění eastus hello.

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a>Vytvoření virtuální sítě

Nám hello [vytvoření sítě vnet az](/cli/azure/network/vnet#create) příkaz toocreate virtuální sítě. V tomto příkladu je název sítě hello *mvVnet* a předponu adresy z *10.0.0.0/16*. Podsíť je vytvořen také s názvem *mySubnetFrontEnd* a předponu *10.0.1.0/24*. Později v tomto kurzu front-end virtuální počítač je připojený toothis podsíť. 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnetFrontEnd \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a>Vytvoření podsítě

Přidá novou podsíť toohello virtuální sítě pomocí hello [az sítě vnet podsíť vytváření](/cli/azure/network/vnet/subnet#create) příkaz. V tomto příkladu je hello podsíť s názvem *mySubnetBackEnd* a předponu adresy z *10.0.2.0/24*. Tato podsíť se používá u všech služeb back-end.

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --address-prefix 10.0.2.0/24
```

V tomto okamžiku síť byla vytvořil a rozdělena na dvě podsítě, jeden pro front-endové služby a jinou pro back endové služby. V další části hello virtuální počítače jsou vytvořené a připojené toothese podsítě.

## <a name="understand-public-ip-address"></a>Pochopení veřejnou IP adresu

Veřejná IP adresa umožňuje toobe prostředky Azure dostupné na hello Internetu. V této části kurzu hello je virtuální počítač vytvořený toodemonstrate jak toowork s veřejné IP adresy.

### <a name="allocation-method"></a>Metoda přidělování

Veřejné IP adresy mohou být přiděleny jako dynamická nebo statická. Ve výchozím nastavení veřejnou IP adresu přidělí dynamicky. Dynamické IP adresy vydávají, když virtuální počítač je navrácena. To způsobí, že hello IP adresu toochange během všechny operace, která zahrnuje navrácení virtuálních počítačů.

může být nastavena metoda přidělení Hello toostatic, což zajistí, že zůstanou hello IP adresy přiřazené tooa virtuální počítač, i během deallocated stavu. Při použití staticky přidělená adresa IP, nelze zadat IP adresu hello sám sebe. Místo toho je přidělen z fondu adres k dispozici.

### <a name="dynamic-allocation"></a>Dynamické přidělení

Při vytváření virtuálního počítače pomocí hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz, je hello výchozí veřejnou IP adresu metoda přidělení dynamické. V následujícím příkladu hello se vytvoří virtuální počítač s dynamickou IP adresu. 

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

Při vytváření virtuálního počítače pomocí hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz, zahrnují hello `--public-ip-address-allocation static` argument tooassign statickou veřejnou IP adresu. Tato operace není ukázáno v tomto kurzu, ale v další části hello dynamicky přidělené IP adresy je změněné tooa staticky přidělenou adresu. 

### <a name="change-allocation-method"></a>Změnit přidělení – metoda

Metoda přidělování adres IP Hello se dá změnit pomocí hello [aktualizace veřejné ip sítě az](/cli/azure/network/public-ip#update) příkaz. V tomto příkladu hello metoda přidělení IP adres hello front-end virtuálního počítače se změní toostatic.

Nejprve zrušit přidělení hello virtuálních počítačů.

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontEndVM
```

Použití hello [aktualizace veřejné ip sítě az](/cli/azure/network/public-ip#update) příkaz metoda přidělení tooupdate hello. V takovém případě hello `--allocation-method` byl nastaven příliš*statické*.

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myFrontEndIP --allocation-method static
```

Spusťte hello virtuálních počítačů.

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontEndVM --no-wait
```

### <a name="no-public-ip-address"></a>Žádné veřejnou IP adresu

Často, virtuální počítač nemusí toobe přístupné prostřednictvím Internetu hello. toocreate virtuální počítač bez veřejnou IP adresu, použijte hello `--public-ip-address ""` argument s prázdnou sadu dvojité uvozovky. Tato konfigurace je ukázán později v tomto kurzu

## <a name="secure-network-traffic"></a>Zabezpečení provozu sítě

Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel zabezpečení, které povolí nebo zakáže přenášení tooresources provoz sítě připojené tooAzure virtuálních sítí (VNet). Skupiny Nsg můžou být přidružené toosubnets nebo jednotlivých síťových rozhraní. Je-li skupinu NSG je spojen s síťovým rozhraním, platí pouze hello přidružené virtuálních počítačů. Pokud skupinu NSG přidružená tooa podsíť, hello pravidla použít tooall prostředky připojené toohello podsítě. 

### <a name="network-security-group-rules"></a>Pravidla skupiny zabezpečení sítě

Pravidla NSG definovat síťové porty, přes které provoz povolený nebo zakázaný. Hello pravidla může obsahovat zdrojové a cílové rozsahy IP adres, tak, aby je řízen provoz mezi konkrétní systémy nebo podsítě. Pravidla NSG také zahrnovat prioritu (mezi 1 – a 4096). Pravidla se vyhodnocují v hello pořadí podle priority. Pravidlo s prioritou 100 vyhodnotí před pravidlo s prioritou 200.

Všechny skupiny NSG obsahují sadu výchozích pravidel. nelze odstranit Hello výchozí pravidla, ale protože je jim přiřazená nejnižší priorita hello, dají se přepsat pravidly hello, které vytvoříte.

- **Virtuální síť** – provoz pocházející a ukončování ve virtuální síti je povolena v příchozí a odchozí.
- **Internet** – odchozí provoz je povolený, ale jsou blokovány příchozí přenosy.
- **Nástroj pro vyrovnávání zatížení** -povolit Azure zatížení vyrovnávání tooprobe hello stavu virtuálních počítačů a instancí rolí. Pokud nepoužíváte skupinu s vyrovnáváním zatížení, můžete přepsat toto pravidlo.

### <a name="create-network-security-groups"></a>Vytvoření skupiny zabezpečení sítě

Skupina zabezpečení sítě lze vytvořit v hello stejný čas jako virtuálního počítače pomocí hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz. Když to uděláte, je spojen s rozhraní sítě virtuálních počítačů hello hello NSG a automaticky vytvořená tooallow přenosy na portu je pravidlo NSG *22* z jakéhokoli zdroje. V tomto kurzu hello hello front-end NSG se automaticky vytvořené pomocí virtuálních počítačů front-end. Pravidlo NSG se také automaticky vytvořenou pro port 22. 

V některých případech může být užitečné toopre-vytvořit skupinu NSG, například když by neměly být vytvářeny výchozí pravidla pro SSH, nebo když hello NSG by měly být připojené tooa podsítě. 

Použití hello [vytvořit az sítě nsg](/cli/azure/network/nsg#create) příkaz toocreate skupinu zabezpečení sítě.

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myNSGBackEnd
```

Místo přidružení hello NSG tooa síťové rozhraní, je přidružená k podsíti. V této konfiguraci dědí všechny virtuální počítač, který je připojený toohello podsíť hello pravidla NSG.

Aktualizovat hello existující podsíť s názvem *mySubnetBackEnd* s hello nová skupina NSG.

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --network-security-group myNSGBackEnd
```

Nyní vytvořit virtuální počítač, který je připojený toohello *mySubnetBackEnd*. Všimněte si, že hello `--nsg` argument má hodnotu prázdný dvojité uvozovky. Skupina NSG nemusí toobe vytvořené pomocí hello virtuálních počítačů. Hello virtuální počítač je připojený toohello back-end podsítě, který je chráněn s hello předem vytvořené NSG back-end. Tato skupina NSG použije toohello virtuálních počítačů. Také si zde všimnout, že hello `--public-ip-address` argument má hodnotu prázdný dvojité uvozovky. Tato konfigurace vytvoří virtuální počítač bez veřejnou IP adresu. 

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

Hello front-end virtuálních počítačů v okamžiku vytvoření pravidlo NSG byl vytvořen tooallow příchozí přenosy na portu 22. Toto pravidlo umožňuje toohello připojení SSH virtuálních počítačů. V tomto příkladu má být povoleno přenosy na portu *80*. Tato konfigurace umožňuje toobe webové aplikace na hello virtuálních počítačů.

Použití hello [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) příkaz toocreate pravidlo pro port *80*.

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

Hello front-end virtuálního počítače je teď pouze přístupné na portu *22* a port *80*. Všechny ostatní příchozí přenosy je blokováno v hello skupinu zabezpečení sítě. To může být užitečné toovisualize hello NSG pravidlo konfigurace. Konfigurace pravidla NSG návratový hello s hello [az sítě pravidlo seznamu](/cli/azure/network/nsg/rule#list) příkaz. 

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

### <a name="secure-vm-toovm-traffic"></a>Zabezpečení přenosu tooVM virtuálních počítačů

Pravidla skupiny zabezpečení sítě můžete také použít mezi virtuálními počítači. V tomto příkladu hello hello front-end virtuálních počítačů musí toocommunicate s back-end virtuálních počítačů na portu *22* a *3306*. Tato konfigurace umožňuje připojení SSH z hello front-end virtuálních počítačů a taky umožnit aplikaci na hello front-end toocommunicate virtuálních počítačů s databází MySQL back-end. Všechny ostatní přenosy by se zablokovat mezi virtuálními počítači hello front-end a back-end.

Použití hello [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) příkaz toocreate pravidlo pro port 22. Všimněte si, že hello `--source-address-prefix` argument určuje hodnotu *10.0.1.0/24*. Tato konfigurace zajistí, že jsou povoleny pouze přenosy z podsítě front-endu hello prostřednictvím hello NSG.

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

Nakonec, protože skupiny Nsg výchozí pravidlo, které povoluje všechny přenosy mezi virtuálními počítači v hello stejné virtuální síti, pravidlo lze vytvořit pro hello back-end skupiny Nsg tooblock veškerý provoz. Si zde všimnout, že hello `--priority` je zadána hodnota *300*, což je nižší, že oba hello pravidla NSG a MySQL. Tato konfigurace zajistí, že provoz SSH a MySQL stále může prostřednictvím hello NSG.

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

Hello back-end virtuální počítač nyní pouze dostupný na portu *22* a port *3306* z podsítě front-endu hello. Všechny ostatní příchozí přenosy je blokováno v hello skupinu zabezpečení sítě. To může být užitečné toovisualize hello NSG pravidlo konfigurace. Konfigurace pravidla NSG návratový hello s hello [az sítě pravidlo seznamu](/cli/azure/network/nsg/rule#list) příkaz. 

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

V tomto kurzu jste vytvořili a zabezpečené sítě Azure jako související toovirtual počítače. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Nasazení virtuální sítě
> * Vytvořte podsíť virtuální sítě.
> * Připojte tooa podsíť virtuálních počítačů
> * Spravovat veřejné IP adresy virtuálních počítačů
> * Zabezpečené příchozí přenosy z Internetu
> * Zabezpečení přenosu tooVM virtuálních počítačů

Posunutí další kurz toolearn toohello o zabezpečení dat na virtuální počítače pomocí zálohování Azure. 

> [!div class="nextstepaction"]
> [Zálohovat virtuální počítače s Linuxem v Azure](./tutorial-backup-vms.md)
