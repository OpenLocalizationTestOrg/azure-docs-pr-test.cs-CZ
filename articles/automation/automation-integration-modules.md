---
title: "<span data-ttu-id=\"5916e-101\">Vytvoření modulu integrace pro Azure Automation | Dokumentace Microsoftu</span><span class=\"sxs-lookup\"><span data-stu-id=\"5916e-101\">Create an Azure Automation Integration Module | Microsoft Docs</span></span>"
description: "<span data-ttu-id=\"5916e-102\">Kurz vás provede vytvořením, otestováním a ukázkovým použitím modulů integrace ve službě Azure Automation.</span><span class=\"sxs-lookup\"><span data-stu-id=\"5916e-102\">Tutorial that walks you through the creation, testing, and example use of  integration modules in Azure Automation.</span></span>"
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 27798efb-08b9-45d9-9b41-5ad91a3df41e
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/13/2017
ms.author: magoedte
ms.openlocfilehash: aeb06276a52e5472667ae0a741fb3007a91910fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-integration-modules"></a><span data-ttu-id="5916e-103">Moduly integrace pro Azure Automation</span><span class="sxs-lookup"><span data-stu-id="5916e-103">Azure Automation Integration Modules</span></span>
<span data-ttu-id="5916e-104">PowerShell je základní technologií, která stojí za službou Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5916e-104">PowerShell is the fundamental technology behind Azure Automation.</span></span> <span data-ttu-id="5916e-105">Vzhledem k tomu, že Azure Automation je postavená na PowerShellu, jsou moduly PowerShellu klíčem k rozšiřitelnosti služby Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5916e-105">Since Azure Automation is built on PowerShell, PowerShell modules are key to the extensibility of Azure Automation.</span></span> <span data-ttu-id="5916e-106">V tomto článku vás provedeme specifiky používání modulů PowerShellu ve službě Azure Automation. Tyto moduly se nazývají „moduly integrace“. Budeme se věnovat i osvědčeným postupům pro vytváření vlastních modulů PowerShellu, abychom zajistili jejich správnou funkčnost v rámci Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5916e-106">In this article, we will guide you through the specifics of Azure Automation’s use of PowerShell modules, referred to as “Integration Modules”, and best practices for creating your own PowerShell modules to make sure they work as Integration Modules within Azure Automation.</span></span> 

## <a name="what-is-a-powershell-module"></a><span data-ttu-id="5916e-107">Co je modul PowerShellu?</span><span class="sxs-lookup"><span data-stu-id="5916e-107">What is a PowerShell Module?</span></span>
<span data-ttu-id="5916e-108">Modul PowerShellu je skupina rutin PowerShellu, například **Get-Date** nebo **Copy-Item**, které můžete používat v konzole PowerShellu, skriptech, pracovních postupech, runboocích a prostředcích DSC PowerShellu, například WindowsFeature nebo File, které můžete používat v konfiguracích DSC PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="5916e-108">A PowerShell module is a group of PowerShell cmdlets like **Get-Date** or **Copy-Item**, that can be used from the PowerShell console, scripts, workflows, runbooks, and PowerShell DSC resources like WindowsFeature or File, that can be used from PowerShell DSC configurations.</span></span> <span data-ttu-id="5916e-109">Všechny funkce PowerShellu jsou dostupné prostřednictvím rutin a prostředků DSC a každá rutina nebo prostředek DSC se zálohuje pomocí modulu PowerShellu (většina z nich se dodává přímo s PowerShellem).</span><span class="sxs-lookup"><span data-stu-id="5916e-109">All of the functionality of PowerShell is exposed through cmdlets and DSC resources, and every cmdlet/DSC resource is backed by a PowerShell module, many of which ship with PowerShell itself.</span></span> <span data-ttu-id="5916e-110">Například rutina **Get-Date** je součástí modulu PowerShellu Microsoft.PowerShell.Utility a rutina **Copy-Item** je součástí modulu PowerShellu Microsoft.PowerShell.Management. Prostředek DSC Balíček je součástí modulu PowerShellu PSDesiredStateConfiguration.</span><span class="sxs-lookup"><span data-stu-id="5916e-110">For example, the **Get-Date** cmdlet is part of the Microsoft.PowerShell.Utility PowerShell module, and **Copy-Item** cmdlet is part of the Microsoft.PowerShell.Management PowerShell module and the Package DSC resource is part of the PSDesiredStateConfiguration PowerShell module.</span></span> <span data-ttu-id="5916e-111">Oba tyto moduly se dodávají spolu s PowerShellem.</span><span class="sxs-lookup"><span data-stu-id="5916e-111">Both of these modules ship with PowerShell.</span></span> <span data-ttu-id="5916e-112">Řada modulů PowerShellu se nedodává jako součást PowerShellu, ale distribuuje se s produkty prvních nebo třetích stran, například s aplikací System Center 2012 Configuration Manager, nebo prostřednictvím rozsáhlé komunity PowerShellu na místech, jako je třeba Galerie prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5916e-112">But many PowerShell modules do not ship as part of PowerShell, and are instead distributed with first or third-party products like System Center 2012 Configuration Manager or by the vast PowerShell community on places like PowerShell Gallery.</span></span>  <span data-ttu-id="5916e-113">Moduly jsou užitečné, protože složité úlohy zjednodušují prostřednictvím zapouzdřené funkcionality.</span><span class="sxs-lookup"><span data-stu-id="5916e-113">The modules are useful because they make complex tasks simpler through encapsulated functionality.</span></span>  <span data-ttu-id="5916e-114">Další informace o [modulech PowerShellu najdete na webu MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="5916e-114">You can learn more about [PowerShell modules on MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx).</span></span> 

## <a name="what-is-an-azure-automation-integration-module"></a><span data-ttu-id="5916e-115">Co je modul integrace pro Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="5916e-115">What is an Azure Automation Integration Module?</span></span>
<span data-ttu-id="5916e-116">Modul integrace se příliš neliší od modulu PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="5916e-116">An Integration Module isn't very different from a PowerShell module.</span></span> <span data-ttu-id="5916e-117">Je to prostě modul PowerShellu, který volitelně obsahuje jeden další soubor – soubor metadat určujících typ připojení Azure Automation, který se bude používat s rutinami modulu v runboocích.</span><span class="sxs-lookup"><span data-stu-id="5916e-117">Its simply a PowerShell module that optionally contains one additional file - a metadata file specifying an Azure Automation connection type to be used with the module's cmdlets in runbooks.</span></span> <span data-ttu-id="5916e-118">Bez ohledu na volitelný soubor můžete tyto moduly PowerShellu importovat do Azure Automation, abyste jejich rutiny zpřístupnili pro použití v rámci runbooků a jejich prostředků DSC, které jsou dostupné pro použití v rámci konfigurací DSC.</span><span class="sxs-lookup"><span data-stu-id="5916e-118">Optional file or not, these PowerShell modules can be imported into Azure Automation to make their cmdlets available for use within runbooks and their DSC resources available for use within DSC configurations.</span></span> <span data-ttu-id="5916e-119">Azure Automation tyto moduly na pozadí ukládá a v čase spuštění úlohy runbooku a úlohy kompilace DSC je načte do izolovaného prostoru (sandbox) v Azure Automation, kde se runbooky spustí a konfigurace DSC se zkompilují.</span><span class="sxs-lookup"><span data-stu-id="5916e-119">Behind the scenes, Azure Automation stores these modules, and at runbook job and DSC compilation job execution time, loads them into the Azure Automation sandboxes where runbooks are executed and DSC configurations are compiled.</span></span>  <span data-ttu-id="5916e-120">Veškeré prostředky DSC v modulech se také automaticky umístí na server vyžádané replikace Automation DSC, aby si je mohly vyžádat počítače, které chtějí použít konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="5916e-120">Any DSC resources in modules are also automatically placed on the Automation DSC pull server, so that they can be pulled by machines attempting to apply DSC configurations.</span></span>  

<span data-ttu-id="5916e-121">Řadu modulů Azure PowerShellu dodáváme jako součást služby Azure Automation, abyste mohli okamžitě začít s automatizací správy Azure. Samozřejmě můžete importovat další moduly PowerShellu pro libovolný systém, službu nebo nástroj, které chcete integrovat.</span><span class="sxs-lookup"><span data-stu-id="5916e-121">We ship a number of Azure  PowerShell modules out of the box in Azure Automation for you to use so you can get started automating Azure management right away, but you can import PowerShell modules for whatever system, service, or tool you want to integrate with.</span></span> 

> [!NOTE]
> <span data-ttu-id="5916e-122">Některé moduly se dodávají jako „globální“ moduly ve službě Automation.</span><span class="sxs-lookup"><span data-stu-id="5916e-122">Certain modules are shipped as “global modules” in the Automation service.</span></span> <span data-ttu-id="5916e-123">Tyto globální moduly jsou dostupné po vytvoření účtu automatizace a my je někdy aktualizujeme, což je automaticky nasadí do vašeho účtu automatizace.</span><span class="sxs-lookup"><span data-stu-id="5916e-123">These global modules are available to you when you create an automation account, and we update them sometimes which automatically pushes them out to your automation account.</span></span> <span data-ttu-id="5916e-124">Pokud nechcete používat automatické aktualizace, můžete vždy importovat stejný modul sami a ten bude mít přednost před globální verzí modulu, dodávanou v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="5916e-124">If you don’t want them to be auto-updated, you can always import the same module yourself, and that will take precedence over the global module version of that module that we ship in the service.</span></span> 

<span data-ttu-id="5916e-125">Formát, ve kterém budete importovat balíček modulu integrace, je komprimovaný soubor se stejným názvem, jako má modul, a navíc s příponou .zip.</span><span class="sxs-lookup"><span data-stu-id="5916e-125">The format in which you import an Integration Module package is a compressed file with the same name as the module and a .zip extension.</span></span> <span data-ttu-id="5916e-126">Obsahuje modul Windows PowerShellu a všechny podpůrné soubory včetně souboru manifestu (.psd1), pokud ho modul obsahuje.</span><span class="sxs-lookup"><span data-stu-id="5916e-126">It contains the Windows PowerShell module and any supporting files, including a manifest file (.psd1) if the module has one.</span></span>

<span data-ttu-id="5916e-127">Pokud má modul obsahovat typ připojení Azure Automation, musí obsahovat také soubor s názvem `<ModuleName>-Automation.json`, který určuje vlastnosti typu připojení.</span><span class="sxs-lookup"><span data-stu-id="5916e-127">If the module should contain an Azure Automation connection type, it must also contain a file with the name `<ModuleName>-Automation.json` that specifies the connection type properties.</span></span> <span data-ttu-id="5916e-128">Toto je soubor json, který je umístěný ve složce modulu vašeho komprimovaného souboru .zip, a obsahuje všechna pole „připojení“, které je povinné pro připojení k systému nebo službě, které modul představuje.</span><span class="sxs-lookup"><span data-stu-id="5916e-128">This is a json file placed within the module folder of your compressed .zip file, and contains the fields of a “connection” that is required to connect to the system or service the module represents.</span></span> <span data-ttu-id="5916e-129">Tím dojdete k vytvoření typu připojení ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5916e-129">This will end up creating a connection type in Azure Automation.</span></span> <span data-ttu-id="5916e-130">Pomocí tohoto souboru můžete pro typ připojení modulu nastavit názvy polí, typy polí a možnosti, jestli mají být pole šifrovaná nebo volitelná.</span><span class="sxs-lookup"><span data-stu-id="5916e-130">Using this file you can set the field names, types, and whether the fields should be encrypted and / or optional, for the connection type of the module.</span></span> <span data-ttu-id="5916e-131">Níže uvádíme šablonu ve formátu json:</span><span class="sxs-lookup"><span data-stu-id="5916e-131">The following is a template in the json file format:</span></span>

```
{ 
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  false,
      "Name":  "ComputerName",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
   "TypeName":  "System.String"
   }],
   "ConnectionTypeName":  "DataProtectionManager",
   "IntegrationModuleName":  "DataProtectionManager"
}
```

<span data-ttu-id="5916e-132">Pokud máte zkušenosti s nasazením Service Management Automation a vytvářením balíčků modulů integrace pro svoje automatizační runbooky, bude to pro vás velmi povědomé.</span><span class="sxs-lookup"><span data-stu-id="5916e-132">If you have deployed Service Management Automation and created Integration Modules packages for your automation runbooks, this should look very familiar to you.</span></span> 

## <a name="authoring-best-practices"></a><span data-ttu-id="5916e-133">Osvědčené postupy pro vytváření obsahu</span><span class="sxs-lookup"><span data-stu-id="5916e-133">Authoring Best Practices</span></span>
<span data-ttu-id="5916e-134">Ačkoli jsou moduly integrace v zásadě moduly PowerShellu, stále existuje řada věcí, které vám při vytváření modulů PowerShellu doporučujeme zohlednit, abyste z nich při používání služby Azure Automation vytěžili maximum.</span><span class="sxs-lookup"><span data-stu-id="5916e-134">Even though Integration Modules are essentially PowerShell modules, there’s still a number of things we recommend you consider while authoring a PowerShell module, to make it most usable in Azure Automation.</span></span> <span data-ttu-id="5916e-135">Některé z nich se týkají konkrétně Azure Automation a jiné jsou užitečné k tomu, aby moduly dobře fungovaly i v pracovním postupu PowerShellu bez ohledu na to, jestli používáte Automation.</span><span class="sxs-lookup"><span data-stu-id="5916e-135">Some of these are Azure Automation specific, and some of them are useful just to make your modules work well in PowerShell Workflow, regardless of whether or not you’re using Automation.</span></span> 

1. <span data-ttu-id="5916e-136">Ke každé rutině v modulu přidejte i stručný obsah, popis a pomocný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="5916e-136">Include a synopsis, description, and help URI for every cmdlet in the module.</span></span> <span data-ttu-id="5916e-137">V PowerShellu můžete definovat určité informace nápovědy k rutinám, abyste uživatelům umožnili zobrazit nápovědu k jejich používání pomocí rutiny **Get-Help**.</span><span class="sxs-lookup"><span data-stu-id="5916e-137">In PowerShell, you can define certain help information for cmdlets to allow the user to receive help on using them with the **Get-Help** cmdlet.</span></span> <span data-ttu-id="5916e-138">Dáme vám příklad, jak můžete definovat stručný obsah a pomocný identifikátor URI pro modul PowerShellu, který je zapsaný v souboru .psm1.</span><span class="sxs-lookup"><span data-stu-id="5916e-138">For example, here’s how you can define a synopsis and help URI for a PowerShell module written in a .psm1 file.</span></span><br>  
   
    ```
    <#
        .SYNOPSIS
         Gets all outgoing phone numbers for this Twilio account 
    #>
    function Get-TwilioPhoneNumbers {
    [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
    HelpUri='http://www.twilio.com/docs/api/rest/outgoing-caller-ids')]
    param(
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AccountSid,
   
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AuthToken,
   
       [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [Hashtable]
       $Connection
    )
   
    $cred = CreateTwilioCredential -Connection $Connection -AccountSid $AccountSid -AuthToken $AuthToken
   
    $uri = "$TWILIO_BASE_URL/Accounts/" + $cred.UserName + "/IncomingPhoneNumbers"
   
    $response = Invoke-RestMethod -Method Get -Uri $uri -Credential $cred
   
    $response.TwilioResponse.IncomingPhoneNumbers.IncomingPhoneNumber
    }
    ```
   <br> <span data-ttu-id="5916e-139">Zadání těchto informací nezajistí zobrazení této nápovědy jenom pomocí rutiny **Get-Help** v konzole PowerShellu, ale zobrazí tuto funkci nápovědy také v rámci Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5916e-139">Providing this info will not only show this help using the **Get-Help** cmdlet in the PowerShell console, it will also expose this help functionality within Azure Automation.</span></span>  <span data-ttu-id="5916e-140">Například při vkládání aktivit během vytváření obsahu runbooku.</span><span class="sxs-lookup"><span data-stu-id="5916e-140">For example, when inserting activities during runbook authoring.</span></span> <span data-ttu-id="5916e-141">Kliknutím na „Zobrazit podrobnou nápovědu“ otevřete pomocný identifikátor URI na jiné kartě webového prohlížeče, který používáte pro přístup k Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5916e-141">Clicking “View detailed help” will open the help URI in another tab of the web browser you’re using to access Azure Automation.</span></span><br><span data-ttu-id="5916e-142">![Nápověda k modulu integrace](media/automation-integration-modules/automation-integration-module-activitydesc.png)</span><span class="sxs-lookup"><span data-stu-id="5916e-142">![Integration Module Help](media/automation-integration-modules/automation-integration-module-activitydesc.png)</span></span>
2. <span data-ttu-id="5916e-143">Pokud se modul spouští pro vzdálený systém,</span><span class="sxs-lookup"><span data-stu-id="5916e-143">If the module runs against a remote system,</span></span>

    <span data-ttu-id="5916e-144">a.</span><span class="sxs-lookup"><span data-stu-id="5916e-144">a.</span></span> <span data-ttu-id="5916e-145">Musí obsahovat soubor s metadaty modulu integrace, který definuje informace potřebné pro připojení ke vzdálenému systému (to znamená typ připojení).</span><span class="sxs-lookup"><span data-stu-id="5916e-145">It should contain an Integration Module metadata file that defines the information needed to connect to that remote system, meaning the connection type.</span></span>  
    <span data-ttu-id="5916e-146">b.</span><span class="sxs-lookup"><span data-stu-id="5916e-146">b.</span></span> <span data-ttu-id="5916e-147">Každá rutina v modulu musí být schopná přijmout objekt připojení (instanci takového typu připojení) jako parametr.</span><span class="sxs-lookup"><span data-stu-id="5916e-147">Each cmdlet in the module should be able to take in a connection object (an instance of that connection type) as a parameter.</span></span>  

    <span data-ttu-id="5916e-148">Používání rutin v modulu v Azure Automation bude snazší, když povolíte předávání objektů s poli typu připojení jako parametru pro rutinu.</span><span class="sxs-lookup"><span data-stu-id="5916e-148">Cmdlets in the module become easier to use in Azure Automation if you allow passing an object with the fields of the connection type as a parameter to the cmdlet.</span></span> <span data-ttu-id="5916e-149">Tímto způsobem uživatelé nemusejí mapovat parametry prostředku připojení na odpovídající parametry rutiny při každém volání rutiny.</span><span class="sxs-lookup"><span data-stu-id="5916e-149">This way users don’t have to map parameters of the connection asset to the cmdlet's corresponding parameters each time they call a cmdlet.</span></span> <span data-ttu-id="5916e-150">Výše uvedený příklad runbooku používá asset připojení Twilio, který se nazývá CorpTwilio, pro přístup k assetu Twilio a k vrácení všech telefonních čísel na účtu.</span><span class="sxs-lookup"><span data-stu-id="5916e-150">Based on the runbook example above, it uses a Twilio connection asset called CorpTwilio to access Twilio and return all the phone numbers in the account.</span></span>  <span data-ttu-id="5916e-151">Všimli jste si, jak mapuje pole připojení na parametry rutiny?</span><span class="sxs-lookup"><span data-stu-id="5916e-151">Notice how it is mapping the fields of the connection to the parameters of the cmdlet?</span></span><br>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    <span data-ttu-id="5916e-152">Jednodušším a lepším způsobem je předat objekt připojení přímo do rutiny –</span><span class="sxs-lookup"><span data-stu-id="5916e-152">An easier and better way to approach this is directly passing the connection object to the cmdlet -</span></span>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   
    <span data-ttu-id="5916e-153">Takové chování můžete svým rutinám povolit tak, že jim dovolíte přijímat objekt připojení přímo jako parametr místo určování parametrů prostým přijímáním polí připojení.</span><span class="sxs-lookup"><span data-stu-id="5916e-153">You can enable behavior like this for your cmdlets by allowing them to accept a connection object directly as a parameter, instead of just connection fields for parameters.</span></span> <span data-ttu-id="5916e-154">Obvykle je vhodné mít samostatné sady parametrů, aby uživatel, který Azure Automation nepoužívá, mohl vaše rutiny volat bez vytváření zatřiďovací tabulky, která by se chovala jako objekt připojení.</span><span class="sxs-lookup"><span data-stu-id="5916e-154">Usually you’ll want a parameter set for each, so that a user not using Azure Automation can call your cmdlets without constructing a hashtable to act as the connection object.</span></span> <span data-ttu-id="5916e-155">Níže uvedená sada parametrů **SpecifyConnectionFields** slouží k postupnému předávání vlastností polí připojení.</span><span class="sxs-lookup"><span data-stu-id="5916e-155">Parameter set **SpecifyConnectionFields** below is used to pass the connection field properties one by one.</span></span> <span data-ttu-id="5916e-156">**UseConnectionObject** vám umožní předat připojení přímo.</span><span class="sxs-lookup"><span data-stu-id="5916e-156">**UseConnectionObject** lets you pass the connection straight through.</span></span> <span data-ttu-id="5916e-157">Jak můžete vidět, rutina Send-TwilioSMS v [modulu PowerShellu Twilio](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) umožňuje předání oběma způsoby:</span><span class="sxs-lookup"><span data-stu-id="5916e-157">As you can see, the Send-TwilioSMS cmdlet in the [Twilio PowerShell module](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) allows passing either way:</span></span> 
   
    ```
    function Send-TwilioSMS {
      [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
      HelpUri='http://www.twilio.com/docs/api/rest/sending-sms')]
      param(
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AccountSid,
   
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AuthToken,
   
         [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [Hashtable]
         $Connection
   
       )
    }
    ```
   <br>
3. <span data-ttu-id="5916e-158">Definujte výstupní typ pro všechny rutiny v modulu.</span><span class="sxs-lookup"><span data-stu-id="5916e-158">Define output type for all cmdlets in the module.</span></span> <span data-ttu-id="5916e-159">Definování typu výstupu rutiny umožňuje technologii IntelliSense, aby vám v době návrhu pomohla zjistit výstupní vlastnosti rutiny, které použijete při vytváření obsahu.</span><span class="sxs-lookup"><span data-stu-id="5916e-159">Defining an output type for a cmdlet allows design-time IntelliSense to help you determine the output properties of the cmdlet, for use during authoring.</span></span> <span data-ttu-id="5916e-160">To se hodí zejména při vytváření grafického obsahu runbooku Automation, kde je znalost doby návrhu klíčová pro snadnou práci s modulem.</span><span class="sxs-lookup"><span data-stu-id="5916e-160">It is especially helpful during Automation runbook graphical authoring, where design time knowledge is key to an easy user experience with your module.</span></span><br><br> <span data-ttu-id="5916e-161">![Typ výstupu grafického runbooku](media/automation-integration-modules/runbook-graphical-module-output-type.png)</span><span class="sxs-lookup"><span data-stu-id="5916e-161">![Graphical Runbook Output Type](media/automation-integration-modules/runbook-graphical-module-output-type.png)</span></span><br> <span data-ttu-id="5916e-162">Podobá se funkci „našeptávání“ výstupu rutiny v integrovaném skriptovacím prostředí PowerShellu bez nutnosti jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="5916e-162">This is similar to the "type ahead" functionality of a cmdlet's output in PowerShell ISE without having to run it.</span></span><br><br> <span data-ttu-id="5916e-163">![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)</span><span class="sxs-lookup"><span data-stu-id="5916e-163">![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)</span></span><br>
4. <span data-ttu-id="5916e-164">Rutiny v modulu nesmí přijímat komplexní typy objektů jako parametry.</span><span class="sxs-lookup"><span data-stu-id="5916e-164">Cmdlets in the module should not take complex object types for parameters.</span></span> <span data-ttu-id="5916e-165">Pracovní postup PowerShellu se liší od PowerShellu tím, že komplexní typy ukládá v deserializované podobě.</span><span class="sxs-lookup"><span data-stu-id="5916e-165">PowerShell Workflow is different from PowerShell in that it stores complex types in deserialized form.</span></span> <span data-ttu-id="5916e-166">Primitivní typy zůstanou jako primitiva, ale komplexní typy budou převedené na jejich deserializované verze, které jsou v podstatě kontejnery vlastností.</span><span class="sxs-lookup"><span data-stu-id="5916e-166">Primitive types will stay as primitives, but complex types are converted to their deserialized versions, which are essentially property bags.</span></span> <span data-ttu-id="5916e-167">Pokud jste například použili rutinu **Get-Process** v runbooku (nebo v pracovním postupu PowerShellu), měla by vrátit objekt typu [Deserialized.System.Diagnostic.Process] a ne očekávaný typ [System.Diagnostic.Process].</span><span class="sxs-lookup"><span data-stu-id="5916e-167">For example, if you used the **Get-Process** cmdlet in a runbook (or a PowerShell Workflow for that matter), it would return an object of type [Deserialized.System.Diagnostic.Process], not the expected [System.Diagnostic.Process] type.</span></span> <span data-ttu-id="5916e-168">Tento typ má stejné vlastnosti jako nedeserializovaný typ, ale nemá žádné metody.</span><span class="sxs-lookup"><span data-stu-id="5916e-168">This type has all the same properties as the non-deserialized type, but none of the methods.</span></span> <span data-ttu-id="5916e-169">A pokud se pokusíte předat tuto hodnotu do rutiny jako parametr, zatímco rutina pro tento parametr očekává hodnotu [System.Diagnostic.Process], dojde k následující chybě: *Není možné zpracovat transformaci argumentu na parametru ‚proces‘. Chyba: „Hodnotu „System.Diagnostics.Process (CcmExec)“ typu „Deserialized.System.Diagnostics.Process“ není možné převést na typ „System.Diagnostics.Process“.*</span><span class="sxs-lookup"><span data-stu-id="5916e-169">And if you try to pass this value as a parameter to a cmdlet, where the cmdlet expects a [System.Diagnostic.Process] value for this parameter, you’ll receive the following error: *Cannot process argument transformation on parameter 'process'. Error: "Cannot convert the "System.Diagnostics.Process (CcmExec)" value of type  "Deserialized.System.Diagnostics.Process" to type "System.Diagnostics.Process".*</span></span>   <span data-ttu-id="5916e-170">Je to proto, že došlo k neshodě typů mezi očekávaným typem [System.Diagnostic.Process] a daným typem [Deserialized.System.Diagnostic.Process].</span><span class="sxs-lookup"><span data-stu-id="5916e-170">This is because there is a type mismatch between the expected [System.Diagnostic.Process] type and the given [Deserialized.System.Diagnostic.Process] type.</span></span> <span data-ttu-id="5916e-171">Způsobem řešení tohoto problému je zajistit, aby rutiny modulu nepřebíraly komplexní typy jako parametry.</span><span class="sxs-lookup"><span data-stu-id="5916e-171">The way around this issue is to ensure the cmdlets of your module do not take complex types for parameters.</span></span> <span data-ttu-id="5916e-172">Následuje ukázka nesprávného způsobu řešení.</span><span class="sxs-lookup"><span data-stu-id="5916e-172">Here is the wrong way to do it.</span></span>
   
    ```
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
    <br>
    <span data-ttu-id="5916e-173">A tady je správný způsob, který přijímá primitivum, které může rutina interně použít k převzetí a používání komplexního objektu.</span><span class="sxs-lookup"><span data-stu-id="5916e-173">And here is the right way, taking in a primitive that can be used internally by the cmdlet to grab the complex object and use it.</span></span> <span data-ttu-id="5916e-174">Vzhledem k tomu, že rutiny se spouštějí v kontextu PowerShellu a ne v kontextu pracovního postupu PowerShellu, $process se uvnitř rutiny stane správným typem [System.Diagnostic.Process].</span><span class="sxs-lookup"><span data-stu-id="5916e-174">Since cmdlets execute in the context of PowerShell, not PowerShell Workflow, inside the cmdlet $process becomes the correct [System.Diagnostic.Process] type.</span></span>  
   
    ```
    function Get-ProcessDescription {
      param (
            [String] $processName
      )
      $process = Get-Process -Name $processName
   
      $process.Description
    }
    ```
   <br>
   <span data-ttu-id="5916e-175">Assety připojení v runboocích představují zatřiďovací tabulky, které jsou komplexním typem, a přesto tyto zatřiďovací tabulky můžou být bezchybně předávané do rutin díky svému parametru -Connection, bez výjimky pro přetypování.</span><span class="sxs-lookup"><span data-stu-id="5916e-175">Connection assets in runbooks are hashtables, which are a complex type, and yet these hashtables seem to be able to be passed into cmdlets for their –Connection parameter perfectly, with no cast exception.</span></span> <span data-ttu-id="5916e-176">Technicky jsou některé typy PowerShellu schopné provést přetypování správně ze serializované podoby do deserializované a proto mohou být předány do rutin jako parametry, které přijímají nedeserializovaný typ.</span><span class="sxs-lookup"><span data-stu-id="5916e-176">Technically, some PowerShell types are able to cast properly from their serialized form to their deserialized form, and hence can be passed into cmdlets for parameters accepting the non-deserialized type.</span></span> <span data-ttu-id="5916e-177">Zatřiďovací tabulka je jedním z nich.</span><span class="sxs-lookup"><span data-stu-id="5916e-177">Hashtable is one of these.</span></span> <span data-ttu-id="5916e-178">Autorem definované typy modulů můžete implementovat způsobem, kterým se také můžou správně deserializovat, ale bude to za cenu určitých kompromisů.</span><span class="sxs-lookup"><span data-stu-id="5916e-178">It’s possible for a module author’s defined types to be implemented in a way that they can correctly deserialize as well, but there are some trade-offs to consider.</span></span> <span data-ttu-id="5916e-179">Tento typ musí mít výchozí konstruktor, musí mít všechny svoje veřejné vlastnosti a musí mít PSTypeConverter.</span><span class="sxs-lookup"><span data-stu-id="5916e-179">The type needs to have a default constructor, have all of its properties public, and have a PSTypeConverter.</span></span> <span data-ttu-id="5916e-180">V případě už definovaných typů, které autor modulu nevlastní, neexistuje žádný způsob, jak je „opravit“, proto doporučujeme, abyste se vyhnuli používání komplexních typů jako parametrů.</span><span class="sxs-lookup"><span data-stu-id="5916e-180">However, for already-defined types that the module author does not own, there is no way to “fix” them, hence the recommendation to avoid complex types for parameters all together.</span></span> <span data-ttu-id="5916e-181">Tip pro vytváření runbooků: Pokud z nějakého důvodu vaše rutiny potřebují přijmout parametr komplexního typu nebo pokud používáte cizí modul, který vyžaduje parametr komplexního typu, řešením pro runbooky pracovního postupu PowerShellu a pracovní postupy PowerShellu v místním PowerShellu je zabalit rutinu, která generuje komplexní typ, a rutinu, která využívá komplexní typ, do stejné aktivity InlineScript.</span><span class="sxs-lookup"><span data-stu-id="5916e-181">Runbook Authoring tip: If for some reason your cmdlets need to take a complex type parameter, or you are using someone else’s module that requires a complex type parameter, the workaround in PowerShell Workflow runbooks and PowerShell Workflows in local PowerShell, is to wrap the cmdlet that generates the complex type and the cmdlet that consumes the complex type in the same InlineScript activity.</span></span> <span data-ttu-id="5916e-182">Vzhledem k tomu, že InlineScript svůj obsah spouští jako PowerShell spíš než jako pracovní postup PowerShellu, rutina generující komplexní typ vytvoří tento správný typ a ne deserializovaný komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="5916e-182">Since InlineScript executes its contents as PowerShell rather than PowerShell Workflow, the cmdlet generating the complex type would produce that correct type, not the deserialized complex type.</span></span>
5. <span data-ttu-id="5916e-183">Nastavte všechny rutiny v modulu jako bezstavové.</span><span class="sxs-lookup"><span data-stu-id="5916e-183">Make all cmdlets in the module stateless.</span></span> <span data-ttu-id="5916e-184">Pracovní postup PowerShellu spouští každou rutinu volanou v pracovním postupu v jiné relaci.</span><span class="sxs-lookup"><span data-stu-id="5916e-184">PowerShell Workflow runs every cmdlet called in the workflow in a different session.</span></span> <span data-ttu-id="5916e-185">To znamená, že rutiny, které závisí na stavu relace, která je vytvořená nebo upravená jinými rutinami ve stejném modulu, nebudou v runboocích pracovního postupu PowerShellu fungovat.</span><span class="sxs-lookup"><span data-stu-id="5916e-185">This means any cmdlets that depend on session state created / modified by other cmdlets in the same module will not work in PowerShell Workflow runbooks.</span></span>  <span data-ttu-id="5916e-186">Následuje příklad špatného postupu.</span><span class="sxs-lookup"><span data-stu-id="5916e-186">Here is an example of what not to do.</span></span>
   
    ```
    $globalNum = 0
    function Set-GlobalNum {
       param(
           [int] $num
       )
   
       $globalNum = $num
    }
    function Get-GlobalNumTimesTwo {
       $output = $globalNum * 2
   
       $output
    }
    ```
   <br>
6. <span data-ttu-id="5916e-187">Modul musí být plně obsažený v balíčku, na který můžete použít příkaz Xcopy.</span><span class="sxs-lookup"><span data-stu-id="5916e-187">The module should be fully contained in an Xcopy-able package.</span></span> <span data-ttu-id="5916e-188">Moduly Azure Automation se distribuují do izolovaného prostoru (sandbox), a tak když se runbooky potřebují spustit, musí pracovat nezávisle na hostiteli, na kterém běží.</span><span class="sxs-lookup"><span data-stu-id="5916e-188">Because Azure Automation modules are distributed to the Automation sandboxes when runbooks need to execute, they need to work independently of the host they are running on.</span></span> <span data-ttu-id="5916e-189">To znamená, že byste měli být schopni zazipovat balíček modulu (do formátu Zip), přesunout ho na libovolného jiného hostitele se stejnou nebo novější verzí PowerShellu a modul bude po importu do prostředí PowerShell takového hostitele normálně fungovat.</span><span class="sxs-lookup"><span data-stu-id="5916e-189">What this means is that you should be able to Zip up the module package, move it to any other host with the same or newer PowerShell version, and have it function as normal when imported into that host’s PowerShell environment.</span></span> <span data-ttu-id="5916e-190">Aby to tak proběhlo, nesmí být modul závislý na žádném souboru mimo složku modulu (složka, kterou zazipujete při importu do Azure Automation) ani na žádném jedinečném nastavení registrů na hostiteli, například nastavení z instalace produktu.</span><span class="sxs-lookup"><span data-stu-id="5916e-190">In order for that to happen, the module should not depend on any files outside the module folder (the folder that gets zipped up when importing into Azure Automation), or on any unique registry settings on a host, such as those set by the install of a product.</span></span> <span data-ttu-id="5916e-191">Pokud tento osvědčený postup nedodržíte, modul nebude ve službě Azure Automation funkční.</span><span class="sxs-lookup"><span data-stu-id="5916e-191">If this best practice is not followed, the module will not be usable in Azure Automation.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="5916e-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5916e-192">Next steps</span></span>

* <span data-ttu-id="5916e-193">První kroky s runbooky pracovních postupů PowerShellu najdete v článku [Můj první runbook pracovního postupu PowerShellu](automation-first-runbook-textual.md).</span><span class="sxs-lookup"><span data-stu-id="5916e-193">To get started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
* <span data-ttu-id="5916e-194">Další informace o vytváření modulů PowerShellu najdete v článku [Psaní modulu Windows PowerShellu](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="5916e-194">To learn more about creating PowerShell Modules, see [Writing a Windows PowerShell Module](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)</span></span>

