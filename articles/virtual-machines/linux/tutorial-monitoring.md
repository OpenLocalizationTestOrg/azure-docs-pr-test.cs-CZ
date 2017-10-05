---
title: "Monitorovat virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Naučte se monitorovat Diagnostika spouštění a metriky výkonu pro virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 3fe8390e88e609b57a462e066f972346f8e8730e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-monitor-a-linux-virtual-machine-in-azure"></a>Postup sledování virtuální počítač s Linuxem v Azure

K zajištění, že virtuální počítače (VM) v Azure běží správně, můžete zkontrolovat Diagnostika spouštění a metriky výkonu. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Povolit Diagnostika spouštění ve virtuálním počítači
> * Zobrazení diagnostiky spouštění
> * Zobrazit hostitele metriky
> * Povolit rozšíření diagnostiky na virtuálním počítači
> * Zobrazit metriky virtuálního počítače
> * Vytvářet výstrahy na základě diagnostiky metriky
> * Nastavit pokročilé monitorování


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-vm"></a>Vytvoření virtuálního počítače

Pokud chcete zobrazit diagnostiku a metriky v akci, je nutné virtuální počítač. Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupMonitor* v *eastus* umístění.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](https://docs.microsoft.com/cli/azure/vm#create). Následující příklad vytvoří virtuální počítač s názvem *Můjvp*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Povolit spuštění diagnostiky

Jak spustit virtuální počítače s Linuxem, rozšíření diagnostiky spouštěcí zaznamená spouštěcí výstup a uloží je v úložišti Azure. Tato data můžete použít k odstraňování problémů spouštění virtuálních počítačů. Diagnostika spouštění nepovolí automaticky při vytváření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure.

Před povolením Diagnostika spouštění, musí být vytvořen pro ukládání protokolů spouštěcí účet úložiště. Účty úložiště musí mít globálně jedinečného názvu, být v rozmezí 3 až 24 znaků a musí obsahovat pouze čísla a malá písmena. Vytvořit účet úložiště s [vytvořit účet úložiště az](/cli/azure/storage/account#create) příkaz. V tomto příkladu je náhodný řetězec použít k vytvoření název účtu úložiště jedinečný. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Při povolování Diagnostika spouštění, je potřeba identifikátor URI pro kontejner úložiště objektů blob. Následující příkaz dotazuje účet úložiště, který chcete vrátit tento identifikátor URI. Hodnota identifikátoru URI je uložen v názvech proměnných *bloburi*, která je použita v dalším kroku.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Teď povolit Diagnostika spouštění s [povolit az virtuálního počítače – Diagnostika spouštění](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable). `--storage` Hodnota je objekt blob URI shromážděných v předchozím kroku.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Zobrazení diagnostiky spouštění

Pokud jsou povolené Diagnostika spouštění, pokaždé, když zastavení a spuštění virtuálního počítače, informace o procesu spouštění je zapsán do souboru protokolu. V tomto příkladu nejprve zrušit přidělení virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate) příkaz takto:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Nyní spusťte virtuální počítač s [spuštění virtuálního počítače az]( /cli/azure/vm#stop) příkaz takto:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

Můžete získat data diagnostiky spouštění pro *Můjvp* s [Diagnostika spouštění virtuálních počítačů az – get spouštěcí protokolu](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) příkaz takto:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Zobrazit hostitele metriky

Virtuální počítač s Linuxem má vyhrazený hostitel v Azure, který komunikuje se službou. Metriky jsou automaticky shromažďovat pro hostitele a lze je zobrazit na portálu Azure následujícím způsobem:

1. Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroupMonitor**a potom vyberte **Můjvp** v seznamu prostředků.
1. Chcete-li zjistit, jaký je výkon hostitelů virtuálních počítačů, klikněte na tlačítko **metriky** v okně virtuálního počítače, pak vyberte některé z *[hostitel]* metriky v části **dostupné metriky**.

    ![Zobrazit hostitele metriky](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Instalace rozšíření diagnostiky

> [!IMPORTANT]
> Tento dokument popisuje 2.3 verzi rozšíření diagnostiky Linux, která se už nepoužívá. Verze 2.3 bude až do 30. června 2018 podporována.
>
> Místo toho lze povolit rozšíření diagnostiky Linux verze 3.0. Další informace najdete v tématu [dokumentaci](./diagnostic-extension.md).

Metriky základní hostitele jsou dostupné, ale podrobnější a metriky specifické pro virtuální počítač, budete muset nainstalovat rozšíření diagnostiky Azure ve virtuálním počítači. Rozšíření diagnostiky Azure umožňuje další funkce monitorování a diagnostická data mají být načteny z virtuálního počítače. Můžete zobrazit tyto metriky výkonu a vytvářet výstrahy založené na tom, jak se provádí virtuálního počítače. Diagnostické rozšíření nainstalovaný prostřednictvím portálu Azure následujícím způsobem:

1. Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků.
1. Klikněte na tlačítko **diagnostiku nastavení**. V seznamu uvedena, který *spouštění diagnostiky* jsou již povolené z předchozí části. Klikněte na zaškrtávací políčko pro *základní metriky*.
1. V *účet úložiště* , vyhledejte a vyberte *mydiagdata [1234]* účet vytvořený v předchozí části.
1. Klikněte na tlačítko **Uložit**.

    ![Zobrazit diagnostické metriky](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>Zobrazit metriky virtuálního počítače

Metriky virtuálního počítače lze zobrazit stejným způsobem, že jste si zobrazili hostitele metriky virtuálního počítače:

1. Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků.
1. Chcete-li zjistit, jaký je výkon virtuálního počítače, klikněte na tlačítko **metriky** v okně virtuálního počítače a potom vyberte některé z metriky diagnostiky v části **dostupné metriky**.

    ![Zobrazit metriky virtuálního počítače](./media/tutorial-monitoring/monitor-vm-metrics.png)


## <a name="create-alerts"></a>Vytváření upozornění

Můžete vytvořit na základě metriky výkonu specifických výstrah. Výstrahy lze oznámí, že jste při průměrné využití procesoru překračuje prahovou hodnotu nebo volné místo na disku klesne pod určitou částku, například. Výstrahy jsou zobrazeny v portálu Azure nebo lze odeslat e-mailem. Runbooky služby automatizace Azure nebo Azure Logic Apps můžete také aktivovat v reakci na výstrahy generován.

Následující příklad vytvoří výstrahu při průměrné využití procesoru.

1. Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků.
2. Klikněte na tlačítko **výstrah pravidla** v okně virtuálního počítače klikněte **přidat metriky upozornění** v horní části okna výstrahy.
4. Zadejte **název** výstrahy, jako například *myAlertRule*
5. Spustí výstrahu, pokud procento využití procesoru překročí 1.0 pět minut, ponechte všechny ostatní výchozí nastavení vybrané.
6. Volitelně můžete zaškrtnout políčko pro *e-mailu vlastníci, přispěvatelé a čtenáři* k odesílání e-mailové oznámení. Výchozí akce je k dispozici oznámení na portálu.
7. Klikněte na tlačítko **OK**.


## <a name="advanced-monitoring"></a>Pokročilé sledování 

Můžete provést rozšířené monitorování vašeho virtuálního počítače pomocí [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Pokud jste tak již neučinili, můžete si zaregistrovat [bezplatnou zkušební verzi](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) služby Operations Management Suite.

Až budete mít přístup k portálu OMS, můžete najít klíč pracovního prostoru a identifikátor prostoru v okně nastavení. Nahrazení < klíč pracovního prostoru > a < id pracovního prostoru > hodnotami pro z vaší OMS prostoru a pak můžete použít **nastavení rozšíření virtuálního az** přidat příponu OMS na virtuální počítač:

```azurecli-interactive 
az vm extension set \
  --resource-group myResourceGroupMonitor \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.3 \
  --protected-settings '{"workspaceKey": "<workspace-key>"}' \
  --settings '{"workspaceId": "<workspace-id>"}'
```

V okně hledání protokolů na portálu OMS byste měli vidět *Můjvp* například informace zobrazené na následujícím obrázku:

![Okno OMS](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nakonfigurovali a zkontrolovat virtuálních počítačů pomocí Azure Security Center. Jste se dozvěděli, jak na:

> [!div class="checklist"]
> * Povolit Diagnostika spouštění ve virtuálním počítači
> * Zobrazení diagnostiky spouštění
> * Zobrazit hostitele metriky
> * Povolit rozšíření diagnostiky na virtuálním počítači
> * Zobrazit metriky virtuálního počítače
> * Vytvářet výstrahy na základě diagnostiky metriky
> * Nastavit pokročilé monitorování

Přechodu na v dalším kurzu se dozvíte o službě Azure security center.

> [!div class="nextstepaction"]
> [Správa zabezpečení virtuálních počítačů](./tutorial-azure-security.md)