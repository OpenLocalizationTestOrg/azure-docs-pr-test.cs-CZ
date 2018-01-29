---
title: "Rozšíření virtuálního počítače Azure výkon diagnostiky pro Windows | Microsoft Docs"
description: "Představuje rozšíření virtuálního počítače Azure výkon diagnostiky pro systém Windows."
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/29/2017
ms.author: genli
ms.openlocfilehash: 5a7dc313f1d6453562e4d5a11ceca03e4459b043
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="azure-performance-diagnostics-vm-extension-for-windows"></a>Rozšíření virtuálního počítače Azure výkon diagnostiky pro Windows

Rozšíření virtuálního počítače Azure výkon diagnostiky pomáhá shromažďování diagnostických dat výkonu z virtuálních počítačů Windows. Rozšíření provede analýzu a poskytuje sestavu výsledků zjištění a doporučení pro identifikovat a řešit problémy s výkonem na virtuálním počítači. Toto rozšíření nainstaluje nástroj řešení potíží s názvem [PerfInsights](http://aka.ms/perfinsights).

## <a name="prerequisites"></a>Požadavky

Toto rozšíření můžete nainstalovat na Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 a Windows Server 2016. Lze ji také nainstalovat na Windows 8.1 a Windows 10.

## <a name="extension-schema"></a>Rozšíření schématu
Následujícím kódu JSON znázorňuje schéma pro rozšíření virtuálního počítače diagnostiky výkonu Azure. Toto rozšíření vyžaduje název a klíč pro účet úložiště pro uložení výstup diagnostiky a sestavy. Tyto hodnoty jsou citlivé a by měly být uložené v chráněném nastavení konfigurace. Azure nastavení rozšíření chráněná data virtuálního počítače je šifrovaný a dešifruje se pouze na cílový virtuální počítač. Všimněte si, že **storageAccountName** a **storageAccountKey** malých a velkých písmen. V následující části jsou uvedeny další požadované parametry.

```JSON
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
```

### <a name="property-values"></a>Hodnoty vlastností

|   **Název**   |**Hodnota nebo příklad**|       **Popis**      |
|--------------|-------------------|----------------------------|
|apiVersion|2015-06-15|Verze rozhraní API.
|Vydavatele|Microsoft.Azure.Performance.Diagnostics|Obor názvů vydavatele pro rozšíření.
|type|AzurePerformanceDiagnostics|Typ rozšíření virtuálního počítače.
|typeHandlerVersion|1.0|Verze rozšíření obslužné rutiny.
|performanceScenario|Základní|Výkon scénář, pro které chcete zaznamenat data. Platné hodnoty jsou: **základní**, **vmslow**, **azurefiles**, a **vlastní**.
|traceDurationInSeconds|300|Platnost trasování, pokud nejsou vybrány žádné možnosti trasování.
|perfCounterTrace|P|Možnost povolit trasování čítače výkonu. Platné hodnoty jsou **p** nebo prázdnou hodnotu. Pokud nechcete zaznamenat trasování, ponechte hodnotu jako prázdný.
|networkTrace|n|Možnost povolit trasování sítě. Platné hodnoty jsou  **n**  nebo prázdnou hodnotu. Pokud nechcete zaznamenat trasování, ponechte hodnotu jako prázdný.
|xperfTrace|x|Možnost povolit XPerf trasování. Platné hodnoty jsou **x** nebo prázdnou hodnotu. Pokud nechcete zaznamenat trasování, ponechte hodnotu jako prázdný.
|storPortTrace|s|Možnost povolit StorPort trasování. Platné hodnoty jsou **s** nebo prázdnou hodnotu. Pokud nechcete zaznamenat trasování, ponechte hodnotu jako prázdný.
|srNumber|123452016365929|Podpora číslo lístku, pokud je k dispozici. Ponechte hodnotu jako prázdné, pokud ji nemáte.
|storageAccountName|můj_účet_úložiště|Název účtu úložiště k ukládání protokolů diagnostiky a výsledky.
|storageAccountKey|lDuVvxuZB28NNP... hAiRF3voADxLBTcc ==|Klíč pro účet úložiště.

## <a name="install-the-extension"></a>Instalace rozšíření

Postupujte podle těchto kroků nainstalujte rozšíření na virtuálních počítačích Windows:

1. Přihlaste se k webu [Azure Portal](http://portal.azure.com).
2. Vyberte virtuální počítač, ve které chcete nainstalovat tato rozšíření.

    ![Snímek obrazovky Azure portal se zvýrazněnou virtuální počítače](media/performance-diagnostics-vm-extension/select-the-virtual-machine.png)
3. Vyberte **rozšíření** a vyberte **přidat**.

    ![Snímek obrazovky rozšíření okno se zvýrazněnou přidat](media/performance-diagnostics-vm-extension/select-extensions.png)
4. Vyberte **Azure Diagnostics výkonu**, přečtěte si podmínky a ujednání a vyberte **vytvořit**.

    ![Snímek obrazovky nové obrazovce prostředků s Azure Diagnostics výkonu zvýrazněných](media/performance-diagnostics-vm-extension/create-azure-performance-diagnostics-extension.png)
5. Zadejte hodnoty parametrů pro instalaci a vyberte **OK** k instalaci rozšíření. Další informace o podporovaných scénářích najdete v tématu [použití PerfInsights](how-to-use-perfInsights.md#supported-troubleshooting-scenarios). 

    ![Dialogové okno snímek obrazovky Instalace rozšíření](media/performance-diagnostics-vm-extension/install-the-extension.png)
6. Po úspěšné instalaci, zobrazí se zpráva s oznámením tento stav.

    ![Snímek obrazovky zřízení úspěšné zpráv](media/performance-diagnostics-vm-extension/provisioning-succeeded-message.png)

    > [!NOTE]
    > Rozšíření se spustí při zajišťování proběhla úspěšně. Jak dlouho trvá dvě minuty nebo méně dokončete pro základní scénáře. Další scénáře spustí se prostřednictvím dobu zadanou během instalace.

## <a name="remove-the-extension"></a>Odeberte rozšíření
Chcete-li odebrat rozšíření z virtuálního počítače, postupujte takto:

1. Přihlaste se k [portál Azure](http://portal.azure.com), vyberte virtuální počítač, ze kterého chcete odebrat toto rozšíření a pak vyberte **rozšíření** okno. 
2. Vyberte (**...** ) pro rozšíření diagnostiky výkonu položku ze seznamu a vyberte **odinstalovat**.

    ![Snímek obrazovky rozšíření okno se zvýrazněnou odinstalace](media/performance-diagnostics-vm-extension/uninstall-the-extension.png)

    > [!NOTE]
    > Můžete také vybrat položku rozšíření a vyberte **odinstalovat** možnost.

## <a name="template-deployment"></a>Nasazení šablon
Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager. Schéma JSON, které jsou popsané v předchozí části lze použít v šablonu Azure Resource Manager. Toto rozšíření virtuálního počítače diagnostiky výkonu Azure spouští při nasazení šablony Azure Resource Manager. Zde je ukázka šablony:

````
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "defaultValue": "yourVMName"
    },
    "location": {
      "type": "string",
      "defaultValue": "southcentralus"
    },
    "storageAccountName": {
      "type": "securestring"
      "defaultValue": "yourStorageAccount"
    },
    "storageAccountKey": {
      "type": "securestring"
      "defaultValue": "yourStorageAccountKey"
    },
    "performanceScenario": {
      "type": "string",
      "defaultValue": "basic"
    },
    "srNumber": {
      "type": "string",
      "defaultValue": ""
    },
    "traceDurationInSeconds": {
      "type": "int",
    "defaultValue": 300
    },
    "perfCounterTrace": {
      "type": "string",
      "defaultValue": "p"
    },
    "networkTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "xperfTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "storPortTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "requestTimeUtc": {
      "type": "string",
      "defaultValue": "10/2/2017 11:06:00 PM"
    }       
  },
  "resources": [
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
  ]
}
````

## <a name="powershell-deployment"></a>Nasazení prostředí PowerShell
`Set-AzureRmVMExtension` Příkaz lze použít k nasazení rozšíření Azure výkon diagnostiky virtuálního počítače do existujícího virtuálního počítače. Před spuštěním příkazu, uložte konfiguraci veřejné a privátní zatřiďovací tabulku prostředí PowerShell.

PowerShell

````
$PublicSettings = @{ "performanceScenario":"basic","traceDurationInSeconds":300,"perfCounterTrace":"p","networkTrace":"","xperfTrace":"","storPortTrace":"","srNumber":"","requestTimeUtc":"2017-09-28T22:08:53.736Z" }
$ProtectedSettings = @{"storageAccountName":"mystorageaccount","storageAccountKey":"mystoragekey"}

Set-AzureRmVMExtension -ExtensionName "AzurePerformanceDiagnostics" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Performance.Diagnostics" `
    -ExtensionType "AzurePerformanceDiagnostics" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS `
````

## <a name="information-on-the-data-captured"></a>Informace o data zaznamenaná
Nástroj PerfInsights shromažďuje různé protokoly, konfiguraci a diagnostických dat, v závislosti na vybrané scénáři. Další informace najdete v tématu [PerfInsights dokumentaci](http://aka.ms/perfinsights).

## <a name="view-and-share-the-results"></a>Zobrazovat a sdílet výsledky

Výstup z rozšíření je uložena ve složce. Složka jmenuje log_collection a najdete v části dočasné jednotce (obvykle D:\log_collection) ve výchozím nastavení. V této složce uvidíte soubory zip obsahující diagnostické protokoly a sestavy s výsledky a doporučení.

Můžete také získat soubor zip v účet úložiště zadaný během instalace. Je sdílen po dobu 30 dnů pomocí [podpisy sdíleného přístupu (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md). Textový soubor s názvem *zipfilename*_saslink.txt se také vytvoří ve složce log_collection. Tento soubor obsahuje odkaz SAS vytvořili ke stažení souboru zip. Každý, kdo má tento odkaz je možné stáhnout soubor zip.

Jako pomoc pracovníka podpory práce v lístku podpory, může společnost Microsoft používat tento odkaz SAS ke stahování dat diagnostiky.

Chcete-li zobrazit sestavu, extrahujte soubor zip a otevřete **PerfInsights Report.html** souboru.

Také je možné stáhnout soubor zip přímo z portálu výběrem rozšíření.

![Snímek obrazovky výkon diagnostiky podrobný stav](media/performance-diagnostics-vm-extension/view-detailed-status.png)

> [!NOTE]
> Odkaz SAS, které se zobrazí na portálu nemusí fungovat. To může být způsobeno poškozená adresa URL během operace kódování a dekódování. Místo toho můžete získat na odkaz přímo ze souboru *_saslink.txt z virtuálního počítače.

## <a name="troubleshoot-and-support"></a>Řešení potíží a podpora

- Stav nasazení rozšíření (v oznamovací oblasti) může zobrazit "Nasazení v průběhu", i když je úspěšně zřízený rozšíření.

    Tento problém můžete ignorovat, tak dlouho, dokud rozšíření stav označuje, že je rozšíření úspěšně zřízený.
- Některé problémy můžete vyřešit během instalace pomocí protokolů rozšíření. Výstupu spuštění rozšíření se zaznamenává soubory, které jsou v následujícím adresáři:

        C:\Packages\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte na [podporu Azure lokality](https://azure.microsoft.com/support/options/)a vyberte **získat podporu**. Informace o používání podpory Azure najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/support/faq/).
