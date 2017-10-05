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
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a><span data-ttu-id="02dd5-103">Vytvoření vlastních artefaktů pro virtuální počítač DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="02dd5-103">Create custom artifacts for your DevTest Labs VM</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a><span data-ttu-id="02dd5-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="02dd5-104">Overview</span></span>
<span data-ttu-id="02dd5-105">**Artefakty** slouží k nasazení a konfiguraci aplikace po zřízení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="02dd5-105">**Artifacts** are used to deploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="02dd5-106">Artefakt se skládá z soubor definice artefaktů a další soubory skriptu, které jsou uložené ve složce v úložišti git.</span><span class="sxs-lookup"><span data-stu-id="02dd5-106">An artifact consists of an artifact definition file and other script files that are stored in a folder in a git repository.</span></span> <span data-ttu-id="02dd5-107">Soubory definice artefaktů se skládají z JSON a výrazů, které můžete použít k určení, co chcete nainstalovat na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="02dd5-107">Artifact definition files consist of JSON and expressions that you can use to specify what you want to install on a VM.</span></span> <span data-ttu-id="02dd5-108">Například můžete definovat název artefakt, příkaz ke spuštění a parametry, které jsou k dispozici, pokud se příkaz spustí.</span><span class="sxs-lookup"><span data-stu-id="02dd5-108">For example, you can define the name of an artifact, command to run, and parameters that are made available when the command is run.</span></span> <span data-ttu-id="02dd5-109">Podle názvu se může odkazovat na jiné soubory skriptu v souboru definice artefaktů.</span><span class="sxs-lookup"><span data-stu-id="02dd5-109">You can refer to other script files within the artifact definition file by name.</span></span>

## <a name="artifact-definition-file-format"></a><span data-ttu-id="02dd5-110">Formát souboru definice artefaktů</span><span class="sxs-lookup"><span data-stu-id="02dd5-110">Artifact definition file format</span></span>
<span data-ttu-id="02dd5-111">Následující příklad ukazuje oddíly, které tvoří základní strukturu definičního souboru:</span><span class="sxs-lookup"><span data-stu-id="02dd5-111">The following example shows the sections that make up the basic structure of a definition file:</span></span>

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

| <span data-ttu-id="02dd5-112">Název elementu</span><span class="sxs-lookup"><span data-stu-id="02dd5-112">Element name</span></span> | <span data-ttu-id="02dd5-113">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="02dd5-113">Required?</span></span> | <span data-ttu-id="02dd5-114">Popis</span><span class="sxs-lookup"><span data-stu-id="02dd5-114">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="02dd5-115">$schema</span><span class="sxs-lookup"><span data-stu-id="02dd5-115">$schema</span></span> |<span data-ttu-id="02dd5-116">Ne</span><span class="sxs-lookup"><span data-stu-id="02dd5-116">No</span></span> |<span data-ttu-id="02dd5-117">Umístění souboru schématu JSON, který pomáhá při testování platnosti definiční soubor.</span><span class="sxs-lookup"><span data-stu-id="02dd5-117">Location of the JSON schema file that helps in testing the validity of the definition file.</span></span> |
| <span data-ttu-id="02dd5-118">Název</span><span class="sxs-lookup"><span data-stu-id="02dd5-118">title</span></span> |<span data-ttu-id="02dd5-119">Ano</span><span class="sxs-lookup"><span data-stu-id="02dd5-119">Yes</span></span> |<span data-ttu-id="02dd5-120">Název artefaktů zobrazí v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="02dd5-120">Name of the artifact displayed in the lab.</span></span> |
| <span data-ttu-id="02dd5-121">Popis</span><span class="sxs-lookup"><span data-stu-id="02dd5-121">description</span></span> |<span data-ttu-id="02dd5-122">Ano</span><span class="sxs-lookup"><span data-stu-id="02dd5-122">Yes</span></span> |<span data-ttu-id="02dd5-123">Popis artefaktu zobrazí v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="02dd5-123">Description of the artifact displayed in the lab.</span></span> |
| <span data-ttu-id="02dd5-124">iconUri</span><span class="sxs-lookup"><span data-stu-id="02dd5-124">iconUri</span></span> |<span data-ttu-id="02dd5-125">Ne</span><span class="sxs-lookup"><span data-stu-id="02dd5-125">No</span></span> |<span data-ttu-id="02dd5-126">Identifikátor URI na ikonu se zobrazí v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="02dd5-126">Uri of the icon displayed in the lab.</span></span> |
| <span data-ttu-id="02dd5-127">targetOsType</span><span class="sxs-lookup"><span data-stu-id="02dd5-127">targetOsType</span></span> |<span data-ttu-id="02dd5-128">Ano</span><span class="sxs-lookup"><span data-stu-id="02dd5-128">Yes</span></span> |<span data-ttu-id="02dd5-129">Operační systém virtuálního počítače, kde je nainstalován artefaktů.</span><span class="sxs-lookup"><span data-stu-id="02dd5-129">Operating system of the VM where artifact is installed.</span></span> <span data-ttu-id="02dd5-130">Podporované možnosti jsou: Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="02dd5-130">Supported options are: Windows and Linux.</span></span> |
| <span data-ttu-id="02dd5-131">Parametry</span><span class="sxs-lookup"><span data-stu-id="02dd5-131">parameters</span></span> |<span data-ttu-id="02dd5-132">Ne</span><span class="sxs-lookup"><span data-stu-id="02dd5-132">No</span></span> |<span data-ttu-id="02dd5-133">Hodnoty, které jsou k dispozici při spuštění příkazu instalace artefaktů na počítači.</span><span class="sxs-lookup"><span data-stu-id="02dd5-133">Values that are provided when artifact install command is run on a machine.</span></span> <span data-ttu-id="02dd5-134">Tato funkce umožňuje přizpůsobení vaší artefaktů.</span><span class="sxs-lookup"><span data-stu-id="02dd5-134">This helps in customizing your artifact.</span></span> |
| <span data-ttu-id="02dd5-135">SpustitPříkaz</span><span class="sxs-lookup"><span data-stu-id="02dd5-135">runCommand</span></span> |<span data-ttu-id="02dd5-136">Ano</span><span class="sxs-lookup"><span data-stu-id="02dd5-136">Yes</span></span> |<span data-ttu-id="02dd5-137">Artefaktů instalační příkaz, který se spouští na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="02dd5-137">Artifact install command that is executed on a VM.</span></span> |

### <a name="artifact-parameters"></a><span data-ttu-id="02dd5-138">Parametry artefaktů</span><span class="sxs-lookup"><span data-stu-id="02dd5-138">Artifact parameters</span></span>
<span data-ttu-id="02dd5-139">V sekci parametrů souboru definice zadejte hodnoty, které uživatel můžete zadat při instalaci artefakt.</span><span class="sxs-lookup"><span data-stu-id="02dd5-139">In the parameters section of the definition file, you specify which values a user can input when installing an artifact.</span></span> <span data-ttu-id="02dd5-140">Může být tyto hodnoty v příkazu instalace artefaktů.</span><span class="sxs-lookup"><span data-stu-id="02dd5-140">You can refer to these values in the artifact install command.</span></span>

<span data-ttu-id="02dd5-141">Můžete definovat parametry s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="02dd5-141">You define parameters with the following structure:</span></span>

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| <span data-ttu-id="02dd5-142">Název elementu</span><span class="sxs-lookup"><span data-stu-id="02dd5-142">Element name</span></span> | <span data-ttu-id="02dd5-143">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="02dd5-143">Required?</span></span> | <span data-ttu-id="02dd5-144">Popis</span><span class="sxs-lookup"><span data-stu-id="02dd5-144">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="02dd5-145">type</span><span class="sxs-lookup"><span data-stu-id="02dd5-145">type</span></span> |<span data-ttu-id="02dd5-146">Ano</span><span class="sxs-lookup"><span data-stu-id="02dd5-146">Yes</span></span> |<span data-ttu-id="02dd5-147">Typ hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="02dd5-147">Type of parameter value.</span></span> <span data-ttu-id="02dd5-148">Najdete v následujícím seznamu Povolené typy:</span><span class="sxs-lookup"><span data-stu-id="02dd5-148">See the following list for the allowed types:</span></span> |
| <span data-ttu-id="02dd5-149">displayName</span><span class="sxs-lookup"><span data-stu-id="02dd5-149">displayName</span></span> |<span data-ttu-id="02dd5-150">Ano</span><span class="sxs-lookup"><span data-stu-id="02dd5-150">Yes</span></span> |<span data-ttu-id="02dd5-151">Název parametru, který se zobrazí uživateli v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="02dd5-151">Name of the parameter that is displayed to a user in the lab.</span></span> | |
| <span data-ttu-id="02dd5-152">Popis</span><span class="sxs-lookup"><span data-stu-id="02dd5-152">description</span></span> |<span data-ttu-id="02dd5-153">Ano</span><span class="sxs-lookup"><span data-stu-id="02dd5-153">Yes</span></span> |<span data-ttu-id="02dd5-154">Popis parametru, který se zobrazí v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="02dd5-154">Description of the parameter that is displayed in the lab.</span></span> |

<span data-ttu-id="02dd5-155">Povolené typy jsou:</span><span class="sxs-lookup"><span data-stu-id="02dd5-155">The allowed types are:</span></span>

* <span data-ttu-id="02dd5-156">řetězec – libovolný platný řetězec formátu JSON</span><span class="sxs-lookup"><span data-stu-id="02dd5-156">string – any valid JSON string</span></span>
* <span data-ttu-id="02dd5-157">int – libovolné platné celé číslo JSON</span><span class="sxs-lookup"><span data-stu-id="02dd5-157">int – any valid JSON integer</span></span>
* <span data-ttu-id="02dd5-158">BOOL – všechny platné JSON logické</span><span class="sxs-lookup"><span data-stu-id="02dd5-158">bool – any valid JSON Boolean</span></span>
* <span data-ttu-id="02dd5-159">pole – žádné platné pole JSON</span><span class="sxs-lookup"><span data-stu-id="02dd5-159">array – any valid JSON array</span></span>

## <a name="artifact-expressions-and-functions"></a><span data-ttu-id="02dd5-160">Výrazy artefaktů a funkce</span><span class="sxs-lookup"><span data-stu-id="02dd5-160">Artifact expressions and functions</span></span>
<span data-ttu-id="02dd5-161">Můžete použít výraz a funkce Vytvořit artefakt příkaz instalovat.</span><span class="sxs-lookup"><span data-stu-id="02dd5-161">You can use expression and functions to construct the artifact install command.</span></span>
<span data-ttu-id="02dd5-162">Výrazy jsou uzavřené do závorek ([a]) a vyhodnocují se při instalaci artefakt.</span><span class="sxs-lookup"><span data-stu-id="02dd5-162">Expressions are enclosed with brackets ([ and ]), and are evaluated when the artifact is installed.</span></span> <span data-ttu-id="02dd5-163">Výrazy může vyskytovat kdekoli v hodnotu řetězce formátu JSON a vždy vrátí jinou hodnotu JSON.</span><span class="sxs-lookup"><span data-stu-id="02dd5-163">Expressions can appear anywhere in a JSON string value and always return another JSON value.</span></span> <span data-ttu-id="02dd5-164">Pokud budete muset použít řetězcový literál, který začíná závorky [, musíte použít dva závorky [[.</span><span class="sxs-lookup"><span data-stu-id="02dd5-164">If you need to use a literal string that starts with a bracket [, you must use two brackets [[.</span></span>
<span data-ttu-id="02dd5-165">Obvykle použijete výrazy s funkcemi, vytvořit hodnotu.</span><span class="sxs-lookup"><span data-stu-id="02dd5-165">Typically, you use expressions with functions to construct a value.</span></span> <span data-ttu-id="02dd5-166">Stejně jako v jazyce JavaScript, volání funkce jsou formátovány jako functionName(arg1,arg2,arg3).</span><span class="sxs-lookup"><span data-stu-id="02dd5-166">Just like in JavaScript, function calls are formatted as functionName(arg1,arg2,arg3).</span></span>

<span data-ttu-id="02dd5-167">V následujícím seznamu jsou uvedeny běžné funkce:</span><span class="sxs-lookup"><span data-stu-id="02dd5-167">The following list shows common functions:</span></span>

* <span data-ttu-id="02dd5-168">Parameters(parameterName) – vrátí hodnotu parametru, který je zadán při spuštění příkazu artefaktů.</span><span class="sxs-lookup"><span data-stu-id="02dd5-168">parameters(parameterName) - Returns a parameter value that is provided when the artifact command is run.</span></span>
* <span data-ttu-id="02dd5-169">concat (arg1, arg2 arg3,...) - kombinuje více řetězcové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="02dd5-169">concat(arg1,arg2,arg3, …..) -     Combines multiple string values.</span></span> <span data-ttu-id="02dd5-170">Tato funkce může trvat libovolný počet argumentů.</span><span class="sxs-lookup"><span data-stu-id="02dd5-170">This function can take any number of arguments.</span></span>

<span data-ttu-id="02dd5-171">Následující příklad ukazuje, jak vytvořit hodnotu pomocí výrazu a funkce:</span><span class="sxs-lookup"><span data-stu-id="02dd5-171">The following example shows how to use expression and functions to construct a value:</span></span>

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a><span data-ttu-id="02dd5-172">Vytvoření vlastních artefaktů</span><span class="sxs-lookup"><span data-stu-id="02dd5-172">Create a custom artifact</span></span>
<span data-ttu-id="02dd5-173">Vytvořte vaše vlastní artefaktů pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="02dd5-173">Create your custom artifact by following these steps:</span></span>

1. <span data-ttu-id="02dd5-174">Nainstalovat JSON editor - potřebujete editor JSON pro práci s artefaktů definiční soubory.</span><span class="sxs-lookup"><span data-stu-id="02dd5-174">Install a JSON editor - You need a JSON editor to work with artifact definition files.</span></span> <span data-ttu-id="02dd5-175">Doporučujeme používat [Visual Studio Code](https://code.visualstudio.com/), která je dostupná pro Windows, Linux a OS X.</span><span class="sxs-lookup"><span data-stu-id="02dd5-175">We recommend using [Visual Studio Code](https://code.visualstudio.com/), which is available for Windows, Linux and OS X.</span></span>
2. <span data-ttu-id="02dd5-176">Get artifactfile.json ukázka - rezervaci artefakty vytvořené týmu Azure DevTest Labs na našem [úložiště GitHub](https://github.com/Azure/azure-devtestlab), kde jsme vytvořili bohaté knihovnu artefaktů, které vám pomůžou vytvořit vlastní artefakty.</span><span class="sxs-lookup"><span data-stu-id="02dd5-176">Get a sample artifactfile.json - Check out the artifacts created by Azure DevTest Labs team at our [GitHub repository](https://github.com/Azure/azure-devtestlab), where we have created a rich library of artifacts that help you create your own artifacts.</span></span> <span data-ttu-id="02dd5-177">Stáhněte soubor definice artefaktů a změnit jej k vytvoření vlastních artefaktů.</span><span class="sxs-lookup"><span data-stu-id="02dd5-177">Download an artifact definition file and make changes to it to create your own artifacts.</span></span>
3. <span data-ttu-id="02dd5-178">Využívat technologie IntelliSense - využívají IntelliSense zobrazíte platné prvky, které lze použít k vytvoření definice soubor artefaktů.</span><span class="sxs-lookup"><span data-stu-id="02dd5-178">Make use of IntelliSense - Leverage IntelliSense to see valid elements that can be used to construct an artifact definition file.</span></span> <span data-ttu-id="02dd5-179">Zobrazí se také různé možnosti pro hodnoty elementu.</span><span class="sxs-lookup"><span data-stu-id="02dd5-179">You can also see the different options for values of an element.</span></span> <span data-ttu-id="02dd5-180">Například IntelliSense ukazují dvě možnosti systému Windows nebo Linux při úpravě **targetOsType** element.</span><span class="sxs-lookup"><span data-stu-id="02dd5-180">For example, IntelliSense show you the two choices of Windows or Linux when editing the **targetOsType** element.</span></span>
4. <span data-ttu-id="02dd5-181">Ukládání artefaktů v [úložiště git](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="02dd5-181">Store the artifact in a [git repository](devtest-lab-add-artifact-repo.md).</span></span>
   
   1. <span data-ttu-id="02dd5-182">Vytvořte samostatný adresář pro každý artefaktů, kde název adresáře je stejný jako název artefaktů.</span><span class="sxs-lookup"><span data-stu-id="02dd5-182">Create a separate directory for each artifact where the directory name is the same as the artifact name.</span></span>
   2. <span data-ttu-id="02dd5-183">Uložte soubor definice artefaktů (artifactfile.json) v adresáři, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="02dd5-183">Store the artifact definition file (artifactfile.json) in the directory you created.</span></span>
   3. <span data-ttu-id="02dd5-184">Úložiště skriptů, které jsou odkazované z instalace příkazu artefaktů.</span><span class="sxs-lookup"><span data-stu-id="02dd5-184">Store the scripts that are referenced from the artifact install command.</span></span>
      
      <span data-ttu-id="02dd5-185">Tady je příklad, jak může vypadat na artefaktů složku:</span><span class="sxs-lookup"><span data-stu-id="02dd5-185">Here is an example of how an artifact folder might look:</span></span>
      
      ![Příklad úložišti git artefaktů](./media/devtest-lab-artifact-author/git-repo.png)
5. <span data-ttu-id="02dd5-187">Přidejte úložiště artefakty do testovacího prostředí - naleznete v článku [přidejte úložiště Git pro artefakty a šablony](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="02dd5-187">Add the artifacts repository to the lab - Refer to the article, [Add a Git repository for artifacts and templates](devtest-lab-add-artifact-repo.md).</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a><span data-ttu-id="02dd5-188">Související články</span><span class="sxs-lookup"><span data-stu-id="02dd5-188">Related articles</span></span>
* [<span data-ttu-id="02dd5-189">Diagnostika chyb artefaktů v DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="02dd5-189">How to diagnose artifact failures in DevTest Labs</span></span>](devtest-lab-troubleshoot-artifact-failure.md)
* [<span data-ttu-id="02dd5-190">Připojení virtuálního počítače do existující domény AD pomocí šablony správce prostředků v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="02dd5-190">Join a VM to existing AD Domain using a resource manager template in Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a><span data-ttu-id="02dd5-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="02dd5-191">Next steps</span></span>
* <span data-ttu-id="02dd5-192">Zjistěte, jak [přidat artefaktů úložiště Git do testovacího prostředí](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="02dd5-192">Learn how to [add a Git artifact repository to a lab](devtest-lab-add-artifact-repo.md).</span></span>

