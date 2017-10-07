---
title: "Rychlý Start - aaaAzure vytvořit CLI virtuálních počítačů Windows | Microsoft Docs"
description: "Naučte se rychle toocreate virtuální počítače s Windows s hello rozhraní příkazového řádku Azure."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 029bdecec219b12b80b958ceeedda214f1b13149
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-hello-azure-cli"></a>Vytvoření virtuálního počítače s Windows pomocí hello rozhraní příkazového řádku Azure

Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech. Tento průvodce údaje, pomocí rozhraní příkazového řádku Azure toodeploy hello virtuálního počítače se systémem Windows Server 2016. Po dokončení nasazení jsme připojení toohello serveru a instalace služby IIS.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). 

Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp*. Tento příklad používá *azureuser* pro název správce a *myPassword12* jako hello heslo. Aktualizujte tyto hodnoty toosomething odpovídající tooyour prostředí. Tyto hodnoty jsou potřeba při vytváření připojení pomocí hello virtuálního počítače.

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --admin-username azureuser --admin-password myPassword12
```

Po vytvoření hello virtuálních počítačů, hello rozhraní příkazového řádku Azure znázorňuje následující ukázka podobné toohello informace. Poznamenejte si hello `publicIpAaddress`. Tato adresa je použité tooaccess hello virtuálních počítačů.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a>Otevření portu 80 pro webový provoz 

Ve výchozím nastavení jsou povoleny pouze připojení RDP v tooWindows virtuálních počítačů nasazených v Azure. Pokud tento virtuální počítač bude toobe webovém serveru, je třeba tooopen port 80 z hello Internetu. Použití hello [az virtuálních počítačů open-port](/cli/azure/vm#open-port) příkaz tooopen hello požadovaného portu.  
 
 ```azurecli-interactive  
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```


## <a name="connect-toovirtual-machine"></a>Připojte počítač toovirtual

Použití hello následující příkaz toocreate a relace vzdálené plochy s hello virtuálního počítače. Nahraďte IP adresu hello hello veřejnou IP adresu virtuálního počítače. Po zobrazení výzvy zadejte přihlašovací údaje hello použité při vytváření hello virtuálního počítače.

```bash 
mstsc /v:<Public IP Address>
```

## <a name="install-iis-using-powershell"></a>Instalace služby IIS pomocí PowerShellu

Teď, když jste byli přihlášeni toohello virtuálního počítače Azure, můžete použít jeden řádek tooinstall prostředí PowerShell služby IIS a povolit hello místní brány firewall pravidla tooallow webový provoz. Otevřete příkazovém řádku prostředí PowerShell a spusťte následující příkaz hello:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-hello-iis-welcome-page"></a>Zobrazení hello úvodní stránka služby IIS

S nainstalovanou službu IIS a port 80 nyní otevřete na vašem virtuálním počítači z hello Internetu můžete použít webový prohlížeč choice tooview hello výchozí IIS uvítací stránky. Být, že toouse hello veřejnou IP adresu, kterou popsané výše toovisit hello výchozí stránky. 

![Výchozí web služby IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove, virtuálních počítačů a všechny související prostředky.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto Rychlém startu jste nasadili jednoduchý virtuální počítač a pravidlo skupiny zabezpečení sítě a nainstalovali jste webový server. toolearn Další informace o virtuálních počítačích Azure, pokračovat v kurzu toohello pro virtuální počítače Windows.

> [!div class="nextstepaction"]
> [Kurzy pro virtuální počítače Azure s Windows](./tutorial-manage-vm.md)
