---
title: "aaaAzure monitorování a virtuální počítače s Windows | Microsoft Docs"
description: "Kurz – monitorování virtuálního počítače s Windows pomocí Azure Powershellu"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 191dc5a30d41c25a9e38f8ec2a32efdc05e03015
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-windows-virtual-machine-with-azure-powershell"></a>Monitorování virtuálního počítače s Windows pomocí Azure Powershellu

Azure monitorování používá spouštěcí toocollect agentů a údaje o výkonu z virtuálních počítačích Azure, uložení dat této v úložišti Azure a zpřístupněte ho prostřednictvím portálu, modul Azure PowerShell hello a hello rozhraní příkazového řádku Azure. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Povolit Diagnostika spouštění na virtuálním počítači
> * Zobrazení diagnostiky spouštění
> * Zobrazit metriky hostitele virtuálního počítače
> * Instalace rozšíření diagnostiky hello
> * Zobrazit metriky virtuálního počítače
> * Vytvořit výstrahu
> * Nastavit pokročilé monitorování

Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější. Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

Příklad hello toocomplete v tomto kurzu, musí mít existující virtuální počítač. V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) můžete vytvořit za vás. Při práci prostřednictvím hello kurzu, nahraďte hello skupinu prostředků, název virtuálního počítače a umístění tam, kde je potřeba.

## <a name="view-boot-diagnostics"></a>Zobrazení diagnostiky spouštění

Jak spustit virtuální počítače s Windows, zaznamená agenta pro diagnostiku hello spouštěcí obrazovky výstupu, který lze použít pro účely odstraňování potíží. Tato funkce je ve výchozím nastavení povolené. Hello zachycení obrazovky, které snímky jsou uložené v účtu úložiště Azure, který se také vytvoří ve výchozím nastavení. 

Můžete získat hello spouštěcí diagnostických dat s hello [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) příkaz. V následujícím příkladu hello, Diagnostika spouštění jsou stažené toohello kořenovém hello * c:\* jednotky. 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Zobrazit hostitele metriky

Virtuální počítač s Windows má vyhrazený virtuální počítač hostitele v Azure, který komunikuje se službou. Metriky jsou automaticky shromažďovat pro hello hostitele a lze zobrazit v hello portálu Azure.

1. V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků hello.
2. Klikněte na tlačítko **metriky** na hello okno virtuálních počítačů a potom vyberte libovolné hello metrik hostitele pod **dostupné metriky** toosee jak provádí hello hostitele virtuálního počítače.

    ![Zobrazit hostitele metriky](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Instalace rozšíření diagnostiky

metriky základní hostitele Hello jsou k dispozici, ale toosee podrobnější a metriky specifické pro virtuální počítač, můžete tooneed tooinstall hello Azure rozšíření diagnostiky na hello virtuálních počítačů. Hello rozšíření diagnostiky Azure umožňuje další funkce monitorování a Diagnostika toobe data načíst z hello virtuálních počítačů. Můžete zobrazit tyto metriky výkonu a vytvářet výstrahy založené na tom, jak se provádí hello virtuálních počítačů. rozšíření diagnostiky Hello se instaluje prostřednictvím portálu Azure hello takto:

1. V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků hello.
2. Klikněte na tlačítko **diagnostiku nastavení**. seznam Hello ukazuje, že *spouštění diagnostiky* jsou již povolené z předchozí části hello. Klikněte na zaškrtávací pole hello *základní metriky*.
3. Klikněte na tlačítko hello **povolit sledování na úrovni hosta** tlačítko.

    ![Zobrazit diagnostické metriky](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>Zobrazit metriky virtuálního počítače

Hello virtuálních počítačů metriky lze zobrazit v hello stejným způsobem, že jste si zobrazili hello hostitele virtuálních počítačů metriky:

1. V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků hello.
2. toosee jak provádí hello virtuálních počítačů, klikněte na tlačítko **metriky** na hello okno virtuálních počítačů a potom vyberte libovolné hello diagnostiky metrik pod **dostupné metriky**.

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

Až budete mít přístup k portálu OMS toohello, najdete v okně Nastavení hello hello identifikátor klíče a pracovního prostoru pracovní prostor. Použití hello [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) zadanými tootooadd hello OMS rozšíření toohello virtuálních počítačů. Proměnná hello aktualizace hodnoty ve hello níže ukázka tooreflect můžete klíč pracovního prostoru OMS a prostoru ID.  

```powershell
$omsId = "<Replace with your OMS Id>"
$omsKey = "<Replace with your OMS key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $omsId} `
  -ProtectedSettings @{"workspaceKey" = $omsKey} `
  -Location eastus
```

Po několika minutách, měli byste vidět hello nový virtuální počítač v pracovní prostor OMS hello. 

![Okno OMS](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali a zkontrolovat virtuálních počítačů pomocí Azure Security Center. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření virtuální sítě
> * Vytvoření skupiny prostředků a virtuálních počítačů 
> * Povolit Diagnostika spouštění na hello virtuálních počítačů
> * Zobrazení diagnostiky spouštění
> * Zobrazit hostitele metriky
> * Instalace rozšíření diagnostiky hello
> * Zobrazit metriky virtuálního počítače
> * Vytvořit výstrahu
> * Nastavit pokročilé monitorování

Posunutí další kurz toolearn toohello o službě Azure security center.

> [!div class="nextstepaction"]
> [Správa zabezpečení virtuálních počítačů](./tutorial-azure-security.md)