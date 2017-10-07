---
title: "virtuální počítače s Linuxem aaaMonitor v Azure | Microsoft Docs"
description: "Zjistěte, jak toomonitor spouštění diagnostiky a metriky výkonu na virtuální počítač s Linuxem v Azure"
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
ms.openlocfilehash: 282da0f03ab0bf37bd44750c22813ca8d1c89560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-a-linux-virtual-machine-in-azure"></a>Jak toomonitor virtuální počítač s Linuxem v Azure

tooensure, virtuální počítače (VM) v Azure běží správně, můžete zkontrolovat Diagnostika spouštění a metriky výkonu. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Povolit Diagnostika spouštění na hello virtuálních počítačů
> * Zobrazení diagnostiky spouštění
> * Zobrazit hostitele metriky
> * Povolit rozšíření diagnostiky na hello virtuálních počítačů
> * Zobrazit metriky virtuálního počítače
> * Vytvářet výstrahy na základě diagnostiky metriky
> * Nastavit pokročilé monitorování


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-vm"></a>Vytvoření virtuálního počítače

toosee diagnostiky a metriky v akci, je nutné virtuální počítač. Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupMonitor* v hello *eastus* umístění.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](https://docs.microsoft.com/cli/azure/vm#create). Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Povolit spuštění diagnostiky

Jak spustit virtuální počítače s Linuxem, rozšíření diagnostiky spouštěcí hello zaznamená spouštěcí výstup a uloží je v úložišti Azure. Tato data mohou být problémy spouštěcí použité tootroubleshoot virtuálních počítačů. Diagnostika spouštění nepovolí automaticky při vytváření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure hello.

Před povolením Diagnostika spouštění, musí účet úložiště toobe vytvořené pro ukládání spouštěcí protokoly. Účty úložiště musí mít globálně jedinečného názvu, být v rozmezí 3 až 24 znaků a musí obsahovat pouze čísla a malá písmena. Vytvořit účet úložiště s hello [vytvořit účet úložiště az](/cli/azure/storage/account#create) příkaz. V tomto příkladu je náhodný řetězec použité toocreate název účtu úložiště jedinečný. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Při povolování Diagnostika spouštění, je potřeba kontejner úložiště objektů blob toohello URI hello. Hello následující dotazy příkaz hello tooreturn účet úložiště tento identifikátor URI. Hello hodnota identifikátoru URI je uložený v názvech proměnných *bloburi*, která je použita v dalším kroku hello.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Teď povolit Diagnostika spouštění s [povolit az virtuálního počítače – Diagnostika spouštění](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable). Hello `--storage` hodnota je hello blob URI shromážděných v předchozím kroku hello.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Zobrazení diagnostiky spouštění

Pokud jsou povolené Diagnostika spouštění, pokaždé, když zastavit a spustit hello virtuálních počítačů, informace o procesu spouštění hello se zapíše tooa souboru protokolu. V tomto příkladu nejprve zrušit přidělení hello virtuálních počítačů s hello [az OM deallocate](/cli/azure/vm#deallocate) příkaz takto:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Nyní spustit hello virtuálních počítačů s hello [spuštění virtuálního počítače az]( /cli/azure/vm#stop) příkaz takto:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

Můžete získat hello spouštěcí diagnostických dat pro *Můjvp* s hello [Diagnostika spouštění virtuálních počítačů az – get spouštěcí protokolu](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) příkaz takto:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Zobrazit hostitele metriky

Virtuální počítač s Linuxem má vyhrazený hostitel v Azure, který komunikuje se službou. Metriky se pro hostitele hello automaticky shromažďovat a lze zobrazit v portálu Azure hello následujícím způsobem:

1. V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroupMonitor**a potom vyberte **Můjvp** v seznamu prostředků hello.
1. toosee jak provádí hello hostitele virtuálních počítačů, klikněte na tlačítko **metriky** v okně hello virtuální počítač, pak vyberte některé z hello *[hostitel]* metriky v části **dostupné metriky**.

    ![Zobrazit hostitele metriky](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Instalace rozšíření diagnostiky

> [!IMPORTANT]
> Tento dokument popisuje 2.3 verzi hello rozšíření diagnostiky Linux, která se už nepoužívá. Verze 2.3 bude až do 30. června 2018 podporována.
>
> Místo toho lze povolit verze 3.0 hello rozšíření diagnostiky Linux. Další informace najdete v tématu [hello dokumentaci](./diagnostic-extension.md).

metriky základní hostitele Hello jsou k dispozici, ale toosee podrobnější a metriky specifické pro virtuální počítač, můžete tooneed tooinstall hello Azure rozšíření diagnostiky na hello virtuálních počítačů. Hello rozšíření diagnostiky Azure umožňuje další funkce monitorování a Diagnostika toobe data načíst z hello virtuálních počítačů. Můžete zobrazit tyto metriky výkonu a vytvářet výstrahy založené na tom, jak se provádí hello virtuálních počítačů. rozšíření diagnostiky Hello se instaluje prostřednictvím portálu Azure hello takto:

1. V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků hello.
1. Klikněte na tlačítko **diagnostiku nastavení**. seznam Hello ukazuje, že *spouštění diagnostiky* jsou již povolené z předchozí části hello. Klikněte na zaškrtávací pole hello *základní metriky*.
1. V hello *účet úložiště* vyhledejte vyberte hello tooand *mydiagdata [1234]* účet vytvořený v předchozí části hello.
1. Klikněte na tlačítko hello **Uložit** tlačítko.

    ![Zobrazit diagnostické metriky](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>Zobrazit metriky virtuálního počítače

Hello virtuálních počítačů metriky lze zobrazit v hello stejným způsobem, že jste si zobrazili hello hostitele virtuálních počítačů metriky:

1. V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků hello.
1. toosee jak provádí hello virtuálních počítačů, klikněte na tlačítko **metriky** na hello okno virtuálních počítačů a potom vyberte libovolné hello diagnostiky metrik pod **dostupné metriky**.

    ![Zobrazit metriky virtuálního počítače](./media/tutorial-monitoring/monitor-vm-metrics.png)


## <a name="create-alerts"></a>Vytváření upozornění

Můžete vytvořit na základě metriky výkonu specifických výstrah. Výstrahy můžou být použité toonotify, které jste při průměrné využití procesoru překračuje prahovou hodnotu nebo volné místo na disku klesne pod určitou částku, například. Výstrahy se zobrazují v hello portál Azure nebo lze odeslat e-mailem. V odpovědi tooalerts generován můžete spustit taky runbooků služeb automatizace Azure nebo Azure Logic Apps.

Hello následující příklad vytvoří výstrahu při průměrné využití procesoru.

1. V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků hello.
2. Klikněte na tlačítko **výstrah pravidla** v okně hello virtuálních počítačů, klikněte **přidat metriky upozornění** napříč hello horní části okna hello výstrahy.
4. Zadejte **název** výstrahy, jako například *myAlertRule*
5. tootrigger výstrahu, pokud procento využití procesoru překračuje 1.0 pro pět minut, ponechte hello všechny ostatní výchozí nastavení vybrané.
6. V případě potřeby zaškrtněte políčko hello pro *e-mailu vlastníci, přispěvatelé a čtenáři* toosend e-mailové oznámení. výchozí akce Hello je toopresent oznámení portálu hello.
7. Klikněte na tlačítko hello **OK** tlačítko.


## <a name="advanced-monitoring"></a>Pokročilé sledování 

Můžete provést rozšířené monitorování vašeho virtuálního počítače pomocí [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Pokud jste tak již neučinili, můžete si zaregistrovat [bezplatnou zkušební verzi](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) služby Operations Management Suite.

Až budete mít přístup k portálu OMS toohello, najdete v okně Nastavení hello hello identifikátor klíče a pracovního prostoru pracovní prostor. Nahraďte < klíč pracovního prostoru > a < id pracovního prostoru > hello hodnotami pro z vaší OMS prostoru a pak můžete použít **nastavení rozšíření virtuálního az** tooadd hello OMS rozšíření toohello virtuálních počítačů:

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

V okně hledání protokolů hello portálu OMS hello, měli byste vidět *Můjvp* například informace zobrazené v hello následujícím obrázku:

![Okno OMS](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nakonfigurovali a zkontrolovat virtuálních počítačů pomocí Azure Security Center. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Povolit Diagnostika spouštění na hello virtuálních počítačů
> * Zobrazení diagnostiky spouštění
> * Zobrazit hostitele metriky
> * Povolit rozšíření diagnostiky na hello virtuálních počítačů
> * Zobrazit metriky virtuálního počítače
> * Vytvářet výstrahy na základě diagnostiky metriky
> * Nastavit pokročilé monitorování

Posunutí další kurz toolearn toohello o službě Azure security center.

> [!div class="nextstepaction"]
> [Správa zabezpečení virtuálních počítačů](./tutorial-azure-security.md)