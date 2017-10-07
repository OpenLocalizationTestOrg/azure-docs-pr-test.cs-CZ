---
title: "aaaCreate vlastních artefaktů pro virtuální počítač DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak tooauthor vlastních artefaktů pro použití s DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 32dcdc61-ec23-4a01-b731-78c029ea5316
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: 2bd603bc1241ca6b669a3a276a677729514f0df2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a>Vytvoření vlastních artefaktů pro virtuální počítač DevTest Labs
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a>Přehled
**Artefakty** jsou použité toodeploy a nakonfigurovat svoji aplikaci po zřízení virtuálního počítače. Artefakt se skládá z soubor definice artefaktů a další soubory skriptu, které jsou uložené ve složce v úložišti git. Soubory definice artefaktů skládá z JSON a výrazy, které můžete použít toospecify co chcete tooinstall na virtuálním počítači. Například můžete definovat název hello artefakt, příkaz toorun a parametry, které jsou k dispozici, když se spustí příkaz hello. Soubory skriptu tooother v souboru definice artefaktů hello mohou odkazovat podle názvu.

## <a name="artifact-definition-file-format"></a>Formát souboru definice artefaktů
Hello následující příklad ukazuje hello oddíly, které tvoří základní strukturu hello definičního souboru:

    {
      "$schema": "https://raw.githubusercontent.com/Azure/azure-devtestlab/master/schemas/2016-11-28/dtlArtifacts.json",
      "title": "",
      "description": "",
      "iconUri": "",
      "targetOsType": "",
      "parameters": {
        "<parameterName>": {
          "type": "",
          "displayName": "",
          "description": ""
        }
      },
      "runCommand": {
        "commandToExecute": ""
      }
    }

| Název elementu | Povinné? | Popis |
| --- | --- | --- |
| $schema |Ne |Umístění souboru schématu JSON hello, který pomáhá při testování platnosti hello hello definičního souboru. |
| Název |Ano |Název hello artefaktů zobrazí v testovacím prostředí hello. |
| description |Ano |Popis hello artefaktů zobrazí v testovacím prostředí hello. |
| iconUri |Ne |Identifikátor URI ikony hello zobrazí v testovacím prostředí hello. |
| targetOsType |Ano |Operační systém virtuálního počítače, kde je nainstalován artefaktů hello. Podporované možnosti jsou: Windows a Linux. |
| parameters |Ne |Hodnoty, které jsou k dispozici při spuštění příkazu instalace artefaktů na počítači. Tato funkce umožňuje přizpůsobení vaší artefaktů. |
| SpustitPříkaz |Ano |Artefaktů instalační příkaz, který se spouští na virtuální počítač. |

### <a name="artifact-parameters"></a>Parametry artefaktů
V části Parametry hello hello definiční soubor zadejte hodnoty, které uživatel můžete zadat při instalaci artefakt. Je možné odkazovat toothese hodnoty v příkazu instalace artefaktů hello.

Definujte parametry s hello strukturu:

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| Název elementu | Povinné? | Popis |
| --- | --- | --- |
| type |Ano |Typ hodnoty parametru. V tématu hello následující seznam pro hello povolené typy: |
| displayName |Ano |Název parametru hello, který je zobrazený tooa uživatele v testovacím prostředí hello. | |
| description |Ano |Popis hello parametr, který se zobrazí v testovacím prostředí hello. |

Hello povolená, typy jsou:

* řetězec – libovolný platný řetězec formátu JSON
* int – libovolné platné celé číslo JSON
* BOOL – všechny platné JSON logické
* pole – žádné platné pole JSON

## <a name="artifact-expressions-and-functions"></a>Výrazy artefaktů a funkce
Můžete použít výraz a příkaz instalovat funkce tooconstruct hello artefaktů.
Výrazy jsou uzavřené do závorek ([a]) a vyhodnocují se při instalaci hello artefaktů. Výrazy může vyskytovat kdekoli v hodnotu řetězce formátu JSON a vždy vrátí jinou hodnotu JSON. Pokud potřebujete toouse řetězcový literál, který začíná závorky [, musíte použít dva závorky [[.
Výrazy se obvykle používá s funkcí tooconstruct hodnotu. Stejně jako v jazyce JavaScript, volání funkce jsou formátovány jako functionName(arg1,arg2,arg3).

Hello následujícím seznamu jsou uvedeny běžné funkce:

* Parameters(parameterName) – vrátí hodnotu parametru, který je zadán při spuštění příkazu artefaktů hello.
* concat (arg1, arg2 arg3,...) - kombinuje více řetězcové hodnoty. Tato funkce může trvat libovolný počet argumentů.

Následující příklad ukazuje, jak Hello toouse výraz a funkce tooconstruct hodnotu:

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a>Vytvoření vlastních artefaktů
Vytvořte vaše vlastní artefaktů pomocí následujících kroků:

1. Nainstalovat JSON editor - je nutné toowork editor JSON s artefaktů definiční soubory. Doporučujeme používat [Visual Studio Code](https://code.visualstudio.com/), která je dostupná pro Windows, Linux a OS X.
2. Get artifactfile.json ukázka - rezervaci hello artefakty vytvořené týmu Azure DevTest Labs na našem [úložiště GitHub](https://github.com/Azure/azure-devtestlab), kde jsme vytvořili bohaté knihovnu artefaktů, které vám pomůžou vytvořit vlastní artefakty. Stáhněte si soubor definice artefaktů a proveďte změny tooit toocreate vlastní artefakty.
3. Ujistěte se, můžete použít technologie IntelliSense - toosee využívají IntelliSense platný se elementy, které se dají použít tooconstruct soubor definice artefaktů. Zobrazí se také hello různé možnosti pro hodnoty elementu. Například zobrazit IntelliSense hello dvě možnosti systému Windows nebo Linux při úpravě hello **targetOsType** element.
4. Úložiště artefaktů hello v [úložiště git](devtest-lab-add-artifact-repo.md).
   
   1. Vytvořte samostatný adresář pro každý artefaktů, kde název adresáře hello je hello stejný jako název artefaktů hello.
   2. Uložte soubor definice artefaktů hello (artifactfile.json) hello adresář, který jste vytvořili.
   3. Příkaz instalovat úložiště hello skripty, které jsou odkazované z hello artefaktů.
      
      Tady je příklad, jak může vypadat na artefaktů složku:
      
      ![Příklad úložišti git artefaktů](./media/devtest-lab-artifact-author/git-repo.png)
5. Přidat hello artefakty úložiště toohello testovacím – najdete v článku toohello [přidejte úložiště Git pro artefakty a šablony](devtest-lab-add-artifact-repo.md).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a>Související články
* [Jak toodiagnose artefaktů selhání v DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md)
* [Připojení k tooexisting virtuálního počítače domény AD pomocí šablony správce prostředků v Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[přidání testovacího prostředí tooa úložiště artefaktů Git](devtest-lab-add-artifact-repo.md).

