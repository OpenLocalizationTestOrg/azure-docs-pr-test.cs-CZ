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
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a><span data-ttu-id="a5582-103">Vytvoření vlastních artefaktů pro virtuální počítač DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="a5582-103">Create custom artifacts for your DevTest Labs VM</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a><span data-ttu-id="a5582-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="a5582-104">Overview</span></span>
<span data-ttu-id="a5582-105">**Artefakty** jsou použité toodeploy a nakonfigurovat svoji aplikaci po zřízení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a5582-105">**Artifacts** are used toodeploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="a5582-106">Artefakt se skládá z soubor definice artefaktů a další soubory skriptu, které jsou uložené ve složce v úložišti git.</span><span class="sxs-lookup"><span data-stu-id="a5582-106">An artifact consists of an artifact definition file and other script files that are stored in a folder in a git repository.</span></span> <span data-ttu-id="a5582-107">Soubory definice artefaktů skládá z JSON a výrazy, které můžete použít toospecify co chcete tooinstall na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="a5582-107">Artifact definition files consist of JSON and expressions that you can use toospecify what you want tooinstall on a VM.</span></span> <span data-ttu-id="a5582-108">Například můžete definovat název hello artefakt, příkaz toorun a parametry, které jsou k dispozici, když se spustí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="a5582-108">For example, you can define hello name of an artifact, command toorun, and parameters that are made available when hello command is run.</span></span> <span data-ttu-id="a5582-109">Soubory skriptu tooother v souboru definice artefaktů hello mohou odkazovat podle názvu.</span><span class="sxs-lookup"><span data-stu-id="a5582-109">You can refer tooother script files within hello artifact definition file by name.</span></span>

## <a name="artifact-definition-file-format"></a><span data-ttu-id="a5582-110">Formát souboru definice artefaktů</span><span class="sxs-lookup"><span data-stu-id="a5582-110">Artifact definition file format</span></span>
<span data-ttu-id="a5582-111">Hello následující příklad ukazuje hello oddíly, které tvoří základní strukturu hello definičního souboru:</span><span class="sxs-lookup"><span data-stu-id="a5582-111">hello following example shows hello sections that make up hello basic structure of a definition file:</span></span>

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

| <span data-ttu-id="a5582-112">Název elementu</span><span class="sxs-lookup"><span data-stu-id="a5582-112">Element name</span></span> | <span data-ttu-id="a5582-113">Povinné?</span><span class="sxs-lookup"><span data-stu-id="a5582-113">Required?</span></span> | <span data-ttu-id="a5582-114">Popis</span><span class="sxs-lookup"><span data-stu-id="a5582-114">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a5582-115">$schema</span><span class="sxs-lookup"><span data-stu-id="a5582-115">$schema</span></span> |<span data-ttu-id="a5582-116">Ne</span><span class="sxs-lookup"><span data-stu-id="a5582-116">No</span></span> |<span data-ttu-id="a5582-117">Umístění souboru schématu JSON hello, který pomáhá při testování platnosti hello hello definičního souboru.</span><span class="sxs-lookup"><span data-stu-id="a5582-117">Location of hello JSON schema file that helps in testing hello validity of hello definition file.</span></span> |
| <span data-ttu-id="a5582-118">Název</span><span class="sxs-lookup"><span data-stu-id="a5582-118">title</span></span> |<span data-ttu-id="a5582-119">Ano</span><span class="sxs-lookup"><span data-stu-id="a5582-119">Yes</span></span> |<span data-ttu-id="a5582-120">Název hello artefaktů zobrazí v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="a5582-120">Name of hello artifact displayed in hello lab.</span></span> |
| <span data-ttu-id="a5582-121">description</span><span class="sxs-lookup"><span data-stu-id="a5582-121">description</span></span> |<span data-ttu-id="a5582-122">Ano</span><span class="sxs-lookup"><span data-stu-id="a5582-122">Yes</span></span> |<span data-ttu-id="a5582-123">Popis hello artefaktů zobrazí v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="a5582-123">Description of hello artifact displayed in hello lab.</span></span> |
| <span data-ttu-id="a5582-124">iconUri</span><span class="sxs-lookup"><span data-stu-id="a5582-124">iconUri</span></span> |<span data-ttu-id="a5582-125">Ne</span><span class="sxs-lookup"><span data-stu-id="a5582-125">No</span></span> |<span data-ttu-id="a5582-126">Identifikátor URI ikony hello zobrazí v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="a5582-126">Uri of hello icon displayed in hello lab.</span></span> |
| <span data-ttu-id="a5582-127">targetOsType</span><span class="sxs-lookup"><span data-stu-id="a5582-127">targetOsType</span></span> |<span data-ttu-id="a5582-128">Ano</span><span class="sxs-lookup"><span data-stu-id="a5582-128">Yes</span></span> |<span data-ttu-id="a5582-129">Operační systém virtuálního počítače, kde je nainstalován artefaktů hello.</span><span class="sxs-lookup"><span data-stu-id="a5582-129">Operating system of hello VM where artifact is installed.</span></span> <span data-ttu-id="a5582-130">Podporované možnosti jsou: Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="a5582-130">Supported options are: Windows and Linux.</span></span> |
| <span data-ttu-id="a5582-131">parameters</span><span class="sxs-lookup"><span data-stu-id="a5582-131">parameters</span></span> |<span data-ttu-id="a5582-132">Ne</span><span class="sxs-lookup"><span data-stu-id="a5582-132">No</span></span> |<span data-ttu-id="a5582-133">Hodnoty, které jsou k dispozici při spuštění příkazu instalace artefaktů na počítači.</span><span class="sxs-lookup"><span data-stu-id="a5582-133">Values that are provided when artifact install command is run on a machine.</span></span> <span data-ttu-id="a5582-134">Tato funkce umožňuje přizpůsobení vaší artefaktů.</span><span class="sxs-lookup"><span data-stu-id="a5582-134">This helps in customizing your artifact.</span></span> |
| <span data-ttu-id="a5582-135">SpustitPříkaz</span><span class="sxs-lookup"><span data-stu-id="a5582-135">runCommand</span></span> |<span data-ttu-id="a5582-136">Ano</span><span class="sxs-lookup"><span data-stu-id="a5582-136">Yes</span></span> |<span data-ttu-id="a5582-137">Artefaktů instalační příkaz, který se spouští na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a5582-137">Artifact install command that is executed on a VM.</span></span> |

### <a name="artifact-parameters"></a><span data-ttu-id="a5582-138">Parametry artefaktů</span><span class="sxs-lookup"><span data-stu-id="a5582-138">Artifact parameters</span></span>
<span data-ttu-id="a5582-139">V části Parametry hello hello definiční soubor zadejte hodnoty, které uživatel můžete zadat při instalaci artefakt.</span><span class="sxs-lookup"><span data-stu-id="a5582-139">In hello parameters section of hello definition file, you specify which values a user can input when installing an artifact.</span></span> <span data-ttu-id="a5582-140">Je možné odkazovat toothese hodnoty v příkazu instalace artefaktů hello.</span><span class="sxs-lookup"><span data-stu-id="a5582-140">You can refer toothese values in hello artifact install command.</span></span>

<span data-ttu-id="a5582-141">Definujte parametry s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="a5582-141">You define parameters with hello following structure:</span></span>

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| <span data-ttu-id="a5582-142">Název elementu</span><span class="sxs-lookup"><span data-stu-id="a5582-142">Element name</span></span> | <span data-ttu-id="a5582-143">Povinné?</span><span class="sxs-lookup"><span data-stu-id="a5582-143">Required?</span></span> | <span data-ttu-id="a5582-144">Popis</span><span class="sxs-lookup"><span data-stu-id="a5582-144">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a5582-145">type</span><span class="sxs-lookup"><span data-stu-id="a5582-145">type</span></span> |<span data-ttu-id="a5582-146">Ano</span><span class="sxs-lookup"><span data-stu-id="a5582-146">Yes</span></span> |<span data-ttu-id="a5582-147">Typ hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="a5582-147">Type of parameter value.</span></span> <span data-ttu-id="a5582-148">V tématu hello následující seznam pro hello povolené typy:</span><span class="sxs-lookup"><span data-stu-id="a5582-148">See hello following list for hello allowed types:</span></span> |
| <span data-ttu-id="a5582-149">displayName</span><span class="sxs-lookup"><span data-stu-id="a5582-149">displayName</span></span> |<span data-ttu-id="a5582-150">Ano</span><span class="sxs-lookup"><span data-stu-id="a5582-150">Yes</span></span> |<span data-ttu-id="a5582-151">Název parametru hello, který je zobrazený tooa uživatele v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="a5582-151">Name of hello parameter that is displayed tooa user in hello lab.</span></span> | |
| <span data-ttu-id="a5582-152">description</span><span class="sxs-lookup"><span data-stu-id="a5582-152">description</span></span> |<span data-ttu-id="a5582-153">Ano</span><span class="sxs-lookup"><span data-stu-id="a5582-153">Yes</span></span> |<span data-ttu-id="a5582-154">Popis hello parametr, který se zobrazí v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="a5582-154">Description of hello parameter that is displayed in hello lab.</span></span> |

<span data-ttu-id="a5582-155">Hello povolená, typy jsou:</span><span class="sxs-lookup"><span data-stu-id="a5582-155">hello allowed types are:</span></span>

* <span data-ttu-id="a5582-156">řetězec – libovolný platný řetězec formátu JSON</span><span class="sxs-lookup"><span data-stu-id="a5582-156">string – any valid JSON string</span></span>
* <span data-ttu-id="a5582-157">int – libovolné platné celé číslo JSON</span><span class="sxs-lookup"><span data-stu-id="a5582-157">int – any valid JSON integer</span></span>
* <span data-ttu-id="a5582-158">BOOL – všechny platné JSON logické</span><span class="sxs-lookup"><span data-stu-id="a5582-158">bool – any valid JSON Boolean</span></span>
* <span data-ttu-id="a5582-159">pole – žádné platné pole JSON</span><span class="sxs-lookup"><span data-stu-id="a5582-159">array – any valid JSON array</span></span>

## <a name="artifact-expressions-and-functions"></a><span data-ttu-id="a5582-160">Výrazy artefaktů a funkce</span><span class="sxs-lookup"><span data-stu-id="a5582-160">Artifact expressions and functions</span></span>
<span data-ttu-id="a5582-161">Můžete použít výraz a příkaz instalovat funkce tooconstruct hello artefaktů.</span><span class="sxs-lookup"><span data-stu-id="a5582-161">You can use expression and functions tooconstruct hello artifact install command.</span></span>
<span data-ttu-id="a5582-162">Výrazy jsou uzavřené do závorek ([a]) a vyhodnocují se při instalaci hello artefaktů.</span><span class="sxs-lookup"><span data-stu-id="a5582-162">Expressions are enclosed with brackets ([ and ]), and are evaluated when hello artifact is installed.</span></span> <span data-ttu-id="a5582-163">Výrazy může vyskytovat kdekoli v hodnotu řetězce formátu JSON a vždy vrátí jinou hodnotu JSON.</span><span class="sxs-lookup"><span data-stu-id="a5582-163">Expressions can appear anywhere in a JSON string value and always return another JSON value.</span></span> <span data-ttu-id="a5582-164">Pokud potřebujete toouse řetězcový literál, který začíná závorky [, musíte použít dva závorky [[.</span><span class="sxs-lookup"><span data-stu-id="a5582-164">If you need toouse a literal string that starts with a bracket [, you must use two brackets [[.</span></span>
<span data-ttu-id="a5582-165">Výrazy se obvykle používá s funkcí tooconstruct hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a5582-165">Typically, you use expressions with functions tooconstruct a value.</span></span> <span data-ttu-id="a5582-166">Stejně jako v jazyce JavaScript, volání funkce jsou formátovány jako functionName(arg1,arg2,arg3).</span><span class="sxs-lookup"><span data-stu-id="a5582-166">Just like in JavaScript, function calls are formatted as functionName(arg1,arg2,arg3).</span></span>

<span data-ttu-id="a5582-167">Hello následujícím seznamu jsou uvedeny běžné funkce:</span><span class="sxs-lookup"><span data-stu-id="a5582-167">hello following list shows common functions:</span></span>

* <span data-ttu-id="a5582-168">Parameters(parameterName) – vrátí hodnotu parametru, který je zadán při spuštění příkazu artefaktů hello.</span><span class="sxs-lookup"><span data-stu-id="a5582-168">parameters(parameterName) - Returns a parameter value that is provided when hello artifact command is run.</span></span>
* <span data-ttu-id="a5582-169">concat (arg1, arg2 arg3,...) - kombinuje více řetězcové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a5582-169">concat(arg1,arg2,arg3, …..) -     Combines multiple string values.</span></span> <span data-ttu-id="a5582-170">Tato funkce může trvat libovolný počet argumentů.</span><span class="sxs-lookup"><span data-stu-id="a5582-170">This function can take any number of arguments.</span></span>

<span data-ttu-id="a5582-171">Následující příklad ukazuje, jak Hello toouse výraz a funkce tooconstruct hodnotu:</span><span class="sxs-lookup"><span data-stu-id="a5582-171">hello following example shows how toouse expression and functions tooconstruct a value:</span></span>

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a><span data-ttu-id="a5582-172">Vytvoření vlastních artefaktů</span><span class="sxs-lookup"><span data-stu-id="a5582-172">Create a custom artifact</span></span>
<span data-ttu-id="a5582-173">Vytvořte vaše vlastní artefaktů pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="a5582-173">Create your custom artifact by following these steps:</span></span>

1. <span data-ttu-id="a5582-174">Nainstalovat JSON editor - je nutné toowork editor JSON s artefaktů definiční soubory.</span><span class="sxs-lookup"><span data-stu-id="a5582-174">Install a JSON editor - You need a JSON editor toowork with artifact definition files.</span></span> <span data-ttu-id="a5582-175">Doporučujeme používat [Visual Studio Code](https://code.visualstudio.com/), která je dostupná pro Windows, Linux a OS X.</span><span class="sxs-lookup"><span data-stu-id="a5582-175">We recommend using [Visual Studio Code](https://code.visualstudio.com/), which is available for Windows, Linux and OS X.</span></span>
2. <span data-ttu-id="a5582-176">Get artifactfile.json ukázka - rezervaci hello artefakty vytvořené týmu Azure DevTest Labs na našem [úložiště GitHub](https://github.com/Azure/azure-devtestlab), kde jsme vytvořili bohaté knihovnu artefaktů, které vám pomůžou vytvořit vlastní artefakty.</span><span class="sxs-lookup"><span data-stu-id="a5582-176">Get a sample artifactfile.json - Check out hello artifacts created by Azure DevTest Labs team at our [GitHub repository](https://github.com/Azure/azure-devtestlab), where we have created a rich library of artifacts that help you create your own artifacts.</span></span> <span data-ttu-id="a5582-177">Stáhněte si soubor definice artefaktů a proveďte změny tooit toocreate vlastní artefakty.</span><span class="sxs-lookup"><span data-stu-id="a5582-177">Download an artifact definition file and make changes tooit toocreate your own artifacts.</span></span>
3. <span data-ttu-id="a5582-178">Ujistěte se, můžete použít technologie IntelliSense - toosee využívají IntelliSense platný se elementy, které se dají použít tooconstruct soubor definice artefaktů.</span><span class="sxs-lookup"><span data-stu-id="a5582-178">Make use of IntelliSense - Leverage IntelliSense toosee valid elements that can be used tooconstruct an artifact definition file.</span></span> <span data-ttu-id="a5582-179">Zobrazí se také hello různé možnosti pro hodnoty elementu.</span><span class="sxs-lookup"><span data-stu-id="a5582-179">You can also see hello different options for values of an element.</span></span> <span data-ttu-id="a5582-180">Například zobrazit IntelliSense hello dvě možnosti systému Windows nebo Linux při úpravě hello **targetOsType** element.</span><span class="sxs-lookup"><span data-stu-id="a5582-180">For example, IntelliSense show you hello two choices of Windows or Linux when editing hello **targetOsType** element.</span></span>
4. <span data-ttu-id="a5582-181">Úložiště artefaktů hello v [úložiště git](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="a5582-181">Store hello artifact in a [git repository](devtest-lab-add-artifact-repo.md).</span></span>
   
   1. <span data-ttu-id="a5582-182">Vytvořte samostatný adresář pro každý artefaktů, kde název adresáře hello je hello stejný jako název artefaktů hello.</span><span class="sxs-lookup"><span data-stu-id="a5582-182">Create a separate directory for each artifact where hello directory name is hello same as hello artifact name.</span></span>
   2. <span data-ttu-id="a5582-183">Uložte soubor definice artefaktů hello (artifactfile.json) hello adresář, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a5582-183">Store hello artifact definition file (artifactfile.json) in hello directory you created.</span></span>
   3. <span data-ttu-id="a5582-184">Příkaz instalovat úložiště hello skripty, které jsou odkazované z hello artefaktů.</span><span class="sxs-lookup"><span data-stu-id="a5582-184">Store hello scripts that are referenced from hello artifact install command.</span></span>
      
      <span data-ttu-id="a5582-185">Tady je příklad, jak může vypadat na artefaktů složku:</span><span class="sxs-lookup"><span data-stu-id="a5582-185">Here is an example of how an artifact folder might look:</span></span>
      
      ![Příklad úložišti git artefaktů](./media/devtest-lab-artifact-author/git-repo.png)
5. <span data-ttu-id="a5582-187">Přidat hello artefakty úložiště toohello testovacím – najdete v článku toohello [přidejte úložiště Git pro artefakty a šablony](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="a5582-187">Add hello artifacts repository toohello lab - Refer toohello article, [Add a Git repository for artifacts and templates](devtest-lab-add-artifact-repo.md).</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a><span data-ttu-id="a5582-188">Související články</span><span class="sxs-lookup"><span data-stu-id="a5582-188">Related articles</span></span>
* [<span data-ttu-id="a5582-189">Jak toodiagnose artefaktů selhání v DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="a5582-189">How toodiagnose artifact failures in DevTest Labs</span></span>](devtest-lab-troubleshoot-artifact-failure.md)
* [<span data-ttu-id="a5582-190">Připojení k tooexisting virtuálního počítače domény AD pomocí šablony správce prostředků v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="a5582-190">Join a VM tooexisting AD Domain using a resource manager template in Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a><span data-ttu-id="a5582-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5582-191">Next steps</span></span>
* <span data-ttu-id="a5582-192">Zjistěte, jak příliš[přidání testovacího prostředí tooa úložiště artefaktů Git](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="a5582-192">Learn how too[add a Git artifact repository tooa lab](devtest-lab-add-artifact-repo.md).</span></span>

