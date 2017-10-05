---
title: "Vytvoření vlastních artefaktů pro virtuální počítač DevTest Labs | Microsoft Docs"
description: "Naučte se vytvářet vlastní artefaktů pro použití s DevTest Labs"
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
ms.openlocfilehash: 2412033daa1d97860dd9f380178622b1ddc590c0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a>Vytvoření vlastních artefaktů pro virtuální počítač DevTest Labs
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a>Přehled
**Artefakty** slouží k nasazení a konfiguraci aplikace po zřízení virtuálního počítače. Artefakt se skládá z soubor definice artefaktů a další soubory skriptu, které jsou uložené ve složce v úložišti git. Soubory definice artefaktů se skládají z JSON a výrazů, které můžete použít k určení, co chcete nainstalovat na virtuální počítač. Například můžete definovat název artefakt, příkaz ke spuštění a parametry, které jsou k dispozici, pokud se příkaz spustí. Podle názvu se může odkazovat na jiné soubory skriptu v souboru definice artefaktů.

## <a name="artifact-definition-file-format"></a>Formát souboru definice artefaktů
Následující příklad ukazuje oddíly, které tvoří základní strukturu definičního souboru:

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

| Název elementu | Vyžaduje? | Popis |
| --- | --- | --- |
| $schema |Ne |Umístění souboru schématu JSON, který pomáhá při testování platnosti definiční soubor. |
| Název |Ano |Název artefaktů zobrazí v testovacím prostředí. |
| Popis |Ano |Popis artefaktu zobrazí v testovacím prostředí. |
| iconUri |Ne |Identifikátor URI na ikonu se zobrazí v testovacím prostředí. |
| targetOsType |Ano |Operační systém virtuálního počítače, kde je nainstalován artefaktů. Podporované možnosti jsou: Windows a Linux. |
| Parametry |Ne |Hodnoty, které jsou k dispozici při spuštění příkazu instalace artefaktů na počítači. Tato funkce umožňuje přizpůsobení vaší artefaktů. |
| SpustitPříkaz |Ano |Artefaktů instalační příkaz, který se spouští na virtuální počítač. |

### <a name="artifact-parameters"></a>Parametry artefaktů
V sekci parametrů souboru definice zadejte hodnoty, které uživatel můžete zadat při instalaci artefakt. Může být tyto hodnoty v příkazu instalace artefaktů.

Můžete definovat parametry s následující strukturou:

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| Název elementu | Vyžaduje? | Popis |
| --- | --- | --- |
| type |Ano |Typ hodnoty parametru. Najdete v následujícím seznamu Povolené typy: |
| displayName |Ano |Název parametru, který se zobrazí uživateli v testovacím prostředí. | |
| Popis |Ano |Popis parametru, který se zobrazí v testovacím prostředí. |

Povolené typy jsou:

* řetězec – libovolný platný řetězec formátu JSON
* int – libovolné platné celé číslo JSON
* BOOL – všechny platné JSON logické
* pole – žádné platné pole JSON

## <a name="artifact-expressions-and-functions"></a>Výrazy artefaktů a funkce
Můžete použít výraz a funkce Vytvořit artefakt příkaz instalovat.
Výrazy jsou uzavřené do závorek ([a]) a vyhodnocují se při instalaci artefakt. Výrazy může vyskytovat kdekoli v hodnotu řetězce formátu JSON a vždy vrátí jinou hodnotu JSON. Pokud budete muset použít řetězcový literál, který začíná závorky [, musíte použít dva závorky [[.
Obvykle použijete výrazy s funkcemi, vytvořit hodnotu. Stejně jako v jazyce JavaScript, volání funkce jsou formátovány jako functionName(arg1,arg2,arg3).

V následujícím seznamu jsou uvedeny běžné funkce:

* Parameters(parameterName) – vrátí hodnotu parametru, který je zadán při spuštění příkazu artefaktů.
* concat (arg1, arg2 arg3,...) - kombinuje více řetězcové hodnoty. Tato funkce může trvat libovolný počet argumentů.

Následující příklad ukazuje, jak vytvořit hodnotu pomocí výrazu a funkce:

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a>Vytvoření vlastních artefaktů
Vytvořte vaše vlastní artefaktů pomocí následujících kroků:

1. Nainstalovat JSON editor - potřebujete editor JSON pro práci s artefaktů definiční soubory. Doporučujeme používat [Visual Studio Code](https://code.visualstudio.com/), která je dostupná pro Windows, Linux a OS X.
2. Get artifactfile.json ukázka - rezervaci artefakty vytvořené týmu Azure DevTest Labs na našem [úložiště GitHub](https://github.com/Azure/azure-devtestlab), kde jsme vytvořili bohaté knihovnu artefaktů, které vám pomůžou vytvořit vlastní artefakty. Stáhněte soubor definice artefaktů a změnit jej k vytvoření vlastních artefaktů.
3. Využívat technologie IntelliSense - využívají IntelliSense zobrazíte platné prvky, které lze použít k vytvoření definice soubor artefaktů. Zobrazí se také různé možnosti pro hodnoty elementu. Například IntelliSense ukazují dvě možnosti systému Windows nebo Linux při úpravě **targetOsType** element.
4. Ukládání artefaktů v [úložiště git](devtest-lab-add-artifact-repo.md).
   
   1. Vytvořte samostatný adresář pro každý artefaktů, kde název adresáře je stejný jako název artefaktů.
   2. Uložte soubor definice artefaktů (artifactfile.json) v adresáři, kterou jste vytvořili.
   3. Úložiště skriptů, které jsou odkazované z instalace příkazu artefaktů.
      
      Tady je příklad, jak může vypadat na artefaktů složku:
      
      ![Příklad úložišti git artefaktů](./media/devtest-lab-artifact-author/git-repo.png)
5. Přidejte úložiště artefakty do testovacího prostředí - naleznete v článku [přidejte úložiště Git pro artefakty a šablony](devtest-lab-add-artifact-repo.md).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a>Související články
* [Diagnostika chyb artefaktů v DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md)
* [Připojení virtuálního počítače do existující domény AD pomocí šablony správce prostředků v Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [přidat artefaktů úložiště Git do testovacího prostředí](devtest-lab-add-artifact-repo.md).

