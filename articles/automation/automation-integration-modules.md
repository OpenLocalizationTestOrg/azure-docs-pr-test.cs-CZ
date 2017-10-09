---
title: "aaaCreate modul integrace služby Azure Automation | Microsoft Docs"
description: "Kurz vás provede hello vytváření, testování a ukázkovým použitím modulů integrace ve službě Azure Automation."
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
ms.openlocfilehash: d4064d8b106496f4dab442024131886c4affccab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-integration-modules"></a><span data-ttu-id="b78a6-103">Moduly integrace pro Azure Automation</span><span class="sxs-lookup"><span data-stu-id="b78a6-103">Azure Automation Integration Modules</span></span>
<span data-ttu-id="b78a6-104">PowerShell je základní technologií hello za Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="b78a6-104">PowerShell is hello fundamental technology behind Azure Automation.</span></span> <span data-ttu-id="b78a6-105">Vzhledem k tomu, že Azure Automation je postavená na Powershellu, jsou moduly Powershellu klíče toohello rozšiřitelnosti služby Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="b78a6-105">Since Azure Automation is built on PowerShell, PowerShell modules are key toohello extensibility of Azure Automation.</span></span> <span data-ttu-id="b78a6-106">V tomto článku jsme vás provede hello specifiky používání Azure Automation moduly Powershellu, označuje tooas "Moduly integrace" a osvědčených postupů pro vytváření vlastních modulů prostředí PowerShell toomake se, že fungují jako moduly integrace v rámci Automatizace Azure.</span><span class="sxs-lookup"><span data-stu-id="b78a6-106">In this article, we will guide you through hello specifics of Azure Automation’s use of PowerShell modules, referred tooas “Integration Modules”, and best practices for creating your own PowerShell modules toomake sure they work as Integration Modules within Azure Automation.</span></span> 

## <a name="what-is-a-powershell-module"></a><span data-ttu-id="b78a6-107">Co je modul PowerShellu?</span><span class="sxs-lookup"><span data-stu-id="b78a6-107">What is a PowerShell Module?</span></span>
<span data-ttu-id="b78a6-108">Modul Powershellu je skupina rutin Powershellu, například **Get-Date** nebo **Copy-Item**, který lze použít z konzole PowerShell text hello, skripty, pracovní postupy, sady runbook a prostředcích DSC Powershellu, jako WindowsFeature nebo File, které lze použít v konfiguracích DSC Powershellu.</span><span class="sxs-lookup"><span data-stu-id="b78a6-108">A PowerShell module is a group of PowerShell cmdlets like **Get-Date** or **Copy-Item**, that can be used from hello PowerShell console, scripts, workflows, runbooks, and PowerShell DSC resources like WindowsFeature or File, that can be used from PowerShell DSC configurations.</span></span> <span data-ttu-id="b78a6-109">Všechny funkce hello prostředí PowerShell je k dispozici prostřednictvím rutin a prostředků DSC a každý rutina nebo prostředek DSC je zálohovaný díky modul prostředí PowerShell řada z nich se dodává přímo s Powershellem.</span><span class="sxs-lookup"><span data-stu-id="b78a6-109">All of hello functionality of PowerShell is exposed through cmdlets and DSC resources, and every cmdlet/DSC resource is backed by a PowerShell module, many of which ship with PowerShell itself.</span></span> <span data-ttu-id="b78a6-110">Například hello **Get-Date** rutiny je součástí hello modulu Powershellu Microsoft.PowerShell.Utility a **Copy-Item** rutiny je součástí modulu Powershellu Microsoft.PowerShell.Management hello a Hello prostředek DSC balíček je součástí modulu powershellu PSDesiredStateConfiguration hello.</span><span class="sxs-lookup"><span data-stu-id="b78a6-110">For example, hello **Get-Date** cmdlet is part of hello Microsoft.PowerShell.Utility PowerShell module, and **Copy-Item** cmdlet is part of hello Microsoft.PowerShell.Management PowerShell module and hello Package DSC resource is part of hello PSDesiredStateConfiguration PowerShell module.</span></span> <span data-ttu-id="b78a6-111">Oba tyto moduly se dodávají spolu s PowerShellem.</span><span class="sxs-lookup"><span data-stu-id="b78a6-111">Both of these modules ship with PowerShell.</span></span> <span data-ttu-id="b78a6-112">Ale řada modulů Powershellu se nedodává jako součást Powershellu a místo toho se distribuují s produkty první nebo třetích stran, jako je System Center 2012 Configuration Manager nebo hello rozsáhlé komunity Powershellu na místech, jako jsou Galerie prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b78a6-112">But many PowerShell modules do not ship as part of PowerShell, and are instead distributed with first or third-party products like System Center 2012 Configuration Manager or by hello vast PowerShell community on places like PowerShell Gallery.</span></span>  <span data-ttu-id="b78a6-113">Hello moduly jsou užitečné, protože složité úlohy zjednodušují prostřednictvím zapouzdřené funkcionality.</span><span class="sxs-lookup"><span data-stu-id="b78a6-113">hello modules are useful because they make complex tasks simpler through encapsulated functionality.</span></span>  <span data-ttu-id="b78a6-114">Další informace o [modulech PowerShellu najdete na webu MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="b78a6-114">You can learn more about [PowerShell modules on MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx).</span></span> 

## <a name="what-is-an-azure-automation-integration-module"></a><span data-ttu-id="b78a6-115">Co je modul integrace pro Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="b78a6-115">What is an Azure Automation Integration Module?</span></span>
<span data-ttu-id="b78a6-116">Modul integrace se příliš neliší od modulu PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="b78a6-116">An Integration Module isn't very different from a PowerShell module.</span></span> <span data-ttu-id="b78a6-117">Jeho prostě modul Powershellu, který volitelně obsahuje jeden další soubor – soubor metadat určujících toobe typu připojení automatizace Azure používat s rutinami modulu hello v sadách runbook.</span><span class="sxs-lookup"><span data-stu-id="b78a6-117">Its simply a PowerShell module that optionally contains one additional file - a metadata file specifying an Azure Automation connection type toobe used with hello module's cmdlets in runbooks.</span></span> <span data-ttu-id="b78a6-118">Volitelný soubor nebo Ne, tyto PowerShell moduly může být importován do Azure Automation toomake jejich rutin, které jsou k dispozici pro použití v rámci sady runbook a jejich prostředků DSC k dispozici pro použití v rámci konfigurací DSC.</span><span class="sxs-lookup"><span data-stu-id="b78a6-118">Optional file or not, these PowerShell modules can be imported into Azure Automation toomake their cmdlets available for use within runbooks and their DSC resources available for use within DSC configurations.</span></span> <span data-ttu-id="b78a6-119">Pozadí hello Azure Automation tyto moduly ukládá a v runbooku a dobu provádění úlohy kompilace DSC je načte do karantény hello Azure Automation, kde se runbooky spustí a konfigurace DSC se zkompilují.</span><span class="sxs-lookup"><span data-stu-id="b78a6-119">Behind hello scenes, Azure Automation stores these modules, and at runbook job and DSC compilation job execution time, loads them into hello Azure Automation sandboxes where runbooks are executed and DSC configurations are compiled.</span></span>  <span data-ttu-id="b78a6-120">Veškeré prostředky DSC v modulech se také automaticky umístí na serveru vyžádané replikace Automation DSC hello, tak, aby si je mohly vyžádat počítače pokus tooapply konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="b78a6-120">Any DSC resources in modules are also automatically placed on hello Automation DSC pull server, so that they can be pulled by machines attempting tooapply DSC configurations.</span></span>  

<span data-ttu-id="b78a6-121">Počet modulů prostředí Azure PowerShell dodávané předinstalované hello ve službě Azure Automation pro jste toouse, můžete začít používat hned automatizací správy Azure, ale můžete naimportovat moduly Powershellu pro libovolnou systému, služby nebo nástroje chcete toointegrate s.</span><span class="sxs-lookup"><span data-stu-id="b78a6-121">We ship a number of Azure  PowerShell modules out of hello box in Azure Automation for you toouse so you can get started automating Azure management right away, but you can import PowerShell modules for whatever system, service, or tool you want toointegrate with.</span></span> 

> [!NOTE]
> <span data-ttu-id="b78a6-122">Některé moduly jsou dodávána jako "globální moduly" hello služby Automation.</span><span class="sxs-lookup"><span data-stu-id="b78a6-122">Certain modules are shipped as “global modules” in hello Automation service.</span></span> <span data-ttu-id="b78a6-123">Tyto globální moduly jsou k dispozici tooyou při vytvoření účtu automation a budeme je aktualizovat někdy, který je automaticky přenáší tooyour účet automation.</span><span class="sxs-lookup"><span data-stu-id="b78a6-123">These global modules are available tooyou when you create an automation account, and we update them sometimes which automatically pushes them out tooyour automation account.</span></span> <span data-ttu-id="b78a6-124">Pokud nechcete, aby je toobe automaticky aktualizované, můžete vždy importovat hello stejném modulu sami a které se mají přednost před verze globálního modulu hello tohoto modulu dodávané ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="b78a6-124">If you don’t want them toobe auto-updated, you can always import hello same module yourself, and that will take precedence over hello global module version of that module that we ship in hello service.</span></span> 

<span data-ttu-id="b78a6-125">Hello formát, ve kterém budete importovat balíček modulu integrace je komprimovaný soubor s hello stejný název jako hello modul a má příponu .zip.</span><span class="sxs-lookup"><span data-stu-id="b78a6-125">hello format in which you import an Integration Module package is a compressed file with hello same name as hello module and a .zip extension.</span></span> <span data-ttu-id="b78a6-126">Obsahuje modul Windows PowerShell hello a všechny podpůrné soubory včetně souboru manifestu (.psd1), pokud modul hello jeden má.</span><span class="sxs-lookup"><span data-stu-id="b78a6-126">It contains hello Windows PowerShell module and any supporting files, including a manifest file (.psd1) if hello module has one.</span></span>

<span data-ttu-id="b78a6-127">Pokud hello modul obsahovat typ připojení Azure Automation, musí obsahovat také soubor s názvem hello `<ModuleName>-Automation.json` který určuje vlastnosti typu připojení hello.</span><span class="sxs-lookup"><span data-stu-id="b78a6-127">If hello module should contain an Azure Automation connection type, it must also contain a file with hello name `<ModuleName>-Automation.json` that specifies hello connection type properties.</span></span> <span data-ttu-id="b78a6-128">Toto je soubor json, který je umístěn v rámci hello složce modulu vašeho komprimovaného souboru .zip a obsahuje hello pole "připojení", který je požadovaný tooconnect toohello systému nebo služby hello modul představuje.</span><span class="sxs-lookup"><span data-stu-id="b78a6-128">This is a json file placed within hello module folder of your compressed .zip file, and contains hello fields of a “connection” that is required tooconnect toohello system or service hello module represents.</span></span> <span data-ttu-id="b78a6-129">Tím dojdete k vytvoření typu připojení ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="b78a6-129">This will end up creating a connection type in Azure Automation.</span></span> <span data-ttu-id="b78a6-130">Použití tohoto souboru můžete nastavit hello názvy polí, typy, a zda mají být hello pole šifrovaná nebo volitelná, pro typ připojení hello hello modulu.</span><span class="sxs-lookup"><span data-stu-id="b78a6-130">Using this file you can set hello field names, types, and whether hello fields should be encrypted and / or optional, for hello connection type of hello module.</span></span> <span data-ttu-id="b78a6-131">Hello následuje šablonu ve formátu souboru json hello:</span><span class="sxs-lookup"><span data-stu-id="b78a6-131">hello following is a template in hello json file format:</span></span>

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

<span data-ttu-id="b78a6-132">Pokud jste nasadili Service Management Automation a vytvářením balíčků modulů integrace pro svoje automatizační runbooky, to by měl vypadat dobře obeznámeni tooyou.</span><span class="sxs-lookup"><span data-stu-id="b78a6-132">If you have deployed Service Management Automation and created Integration Modules packages for your automation runbooks, this should look very familiar tooyou.</span></span> 

## <a name="authoring-best-practices"></a><span data-ttu-id="b78a6-133">Osvědčené postupy pro vytváření obsahu</span><span class="sxs-lookup"><span data-stu-id="b78a6-133">Authoring Best Practices</span></span>
<span data-ttu-id="b78a6-134">I když jsou moduly integrace v podstatě moduly Powershellu, je stále existuje řada věcí, doporučujeme zvážit při vytváření modulu Powershellu, toomake ho nejvíce použitelné ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="b78a6-134">Even though Integration Modules are essentially PowerShell modules, there’s still a number of things we recommend you consider while authoring a PowerShell module, toomake it most usable in Azure Automation.</span></span> <span data-ttu-id="b78a6-135">Některé z nich se týkají konkrétně Azure Automation a jiné jsou užitečné pouze toomake moduly dobře fungovaly v prostředí PowerShell Workflow, bez ohledu na to, jestli používáte Automation.</span><span class="sxs-lookup"><span data-stu-id="b78a6-135">Some of these are Azure Automation specific, and some of them are useful just toomake your modules work well in PowerShell Workflow, regardless of whether or not you’re using Automation.</span></span> 

1. <span data-ttu-id="b78a6-136">Přidejte i stručný obsah, popis a pomocný identifikátor URI pro každé rutině v modulu hello.</span><span class="sxs-lookup"><span data-stu-id="b78a6-136">Include a synopsis, description, and help URI for every cmdlet in hello module.</span></span> <span data-ttu-id="b78a6-137">V prostředí PowerShell, můžete definovat určité informace nápovědy rutin tooallow hello uživatele tooreceive nápovědu k jejich používání s hello **Get-Help** rutiny.</span><span class="sxs-lookup"><span data-stu-id="b78a6-137">In PowerShell, you can define certain help information for cmdlets tooallow hello user tooreceive help on using them with hello **Get-Help** cmdlet.</span></span> <span data-ttu-id="b78a6-138">Dáme vám příklad, jak můžete definovat stručný obsah a pomocný identifikátor URI pro modul PowerShellu, který je zapsaný v souboru .psm1.</span><span class="sxs-lookup"><span data-stu-id="b78a6-138">For example, here’s how you can define a synopsis and help URI for a PowerShell module written in a .psm1 file.</span></span><br>  
   
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
   <br> <span data-ttu-id="b78a6-139">Zadání těchto informací nebude zobrazovat pouze této nápovědy pomocí hello **Get-Help** hello rutiny v konzole PowerShell, zveřejní také tuto funkci nápovědy v rámci Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="b78a6-139">Providing this info will not only show this help using hello **Get-Help** cmdlet in hello PowerShell console, it will also expose this help functionality within Azure Automation.</span></span>  <span data-ttu-id="b78a6-140">Například při vkládání aktivit během vytváření obsahu runbooku.</span><span class="sxs-lookup"><span data-stu-id="b78a6-140">For example, when inserting activities during runbook authoring.</span></span> <span data-ttu-id="b78a6-141">Kliknutím na "Zobrazit podrobnou nápovědu" bude hello otevřete pomocný identifikátor URI na jiné kartě hello webové prohlížeče používáte tooaccess Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="b78a6-141">Clicking “View detailed help” will open hello help URI in another tab of hello web browser you’re using tooaccess Azure Automation.</span></span><br><span data-ttu-id="b78a6-142">![Nápověda k modulu integrace](media/automation-integration-modules/automation-integration-module-activitydesc.png)</span><span class="sxs-lookup"><span data-stu-id="b78a6-142">![Integration Module Help](media/automation-integration-modules/automation-integration-module-activitydesc.png)</span></span>
2. <span data-ttu-id="b78a6-143">Pokud hello modul spouští na vzdáleném systému,</span><span class="sxs-lookup"><span data-stu-id="b78a6-143">If hello module runs against a remote system,</span></span>

    <span data-ttu-id="b78a6-144">a.</span><span class="sxs-lookup"><span data-stu-id="b78a6-144">a.</span></span> <span data-ttu-id="b78a6-145">Měl by obsahovat soubor metadat modulu integrace, který definuje hello informace potřebné tooconnect toothat vzdáleného systému, což znamená typ připojení hello.</span><span class="sxs-lookup"><span data-stu-id="b78a6-145">It should contain an Integration Module metadata file that defines hello information needed tooconnect toothat remote system, meaning hello connection type.</span></span>  
    <span data-ttu-id="b78a6-146">b.</span><span class="sxs-lookup"><span data-stu-id="b78a6-146">b.</span></span> <span data-ttu-id="b78a6-147">Každá rutina v modulu hello by měl být schopný tootake objekt připojení (instanci takového typu připojení) jako parametr.</span><span class="sxs-lookup"><span data-stu-id="b78a6-147">Each cmdlet in hello module should be able tootake in a connection object (an instance of that connection type) as a parameter.</span></span>  

    <span data-ttu-id="b78a6-148">Rutiny v modulu hello stát jednodušší toouse ve službě Azure Automation, pokud povolíte předávání objektů s hello pole typu připojení hello jako parametr rutiny toohello.</span><span class="sxs-lookup"><span data-stu-id="b78a6-148">Cmdlets in hello module become easier toouse in Azure Automation if you allow passing an object with hello fields of hello connection type as a parameter toohello cmdlet.</span></span> <span data-ttu-id="b78a6-149">Tímto způsobem uživatelé nemají toomap parametry hello připojení asset toohello na odpovídající parametry rutiny při každém volání rutiny.</span><span class="sxs-lookup"><span data-stu-id="b78a6-149">This way users don’t have toomap parameters of hello connection asset toohello cmdlet's corresponding parameters each time they call a cmdlet.</span></span> <span data-ttu-id="b78a6-150">Založeno na výše uvedený příklad runbooku hello, používá asset připojení Twilio nazývá CorpTwilio tooaccess Twilio a vrátí všechny hello telefonní čísla v účtu hello.</span><span class="sxs-lookup"><span data-stu-id="b78a6-150">Based on hello runbook example above, it uses a Twilio connection asset called CorpTwilio tooaccess Twilio and return all hello phone numbers in hello account.</span></span>  <span data-ttu-id="b78a6-151">Všimněte si, jak mapuje pole hello hello připojení toohello parametrů rutiny hello?</span><span class="sxs-lookup"><span data-stu-id="b78a6-151">Notice how it is mapping hello fields of hello connection toohello parameters of hello cmdlet?</span></span><br>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    <span data-ttu-id="b78a6-152">Tooapproach jednodušším a lepším způsobem to přímo předává hello připojení objekt toohello rutiny –</span><span class="sxs-lookup"><span data-stu-id="b78a6-152">An easier and better way tooapproach this is directly passing hello connection object toohello cmdlet -</span></span>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   
    <span data-ttu-id="b78a6-153">Takové chování pro vaše rutiny můžete povolit tím, že je tooaccept objekt připojení přímo jako parametr místo právě připojení pole parametrů.</span><span class="sxs-lookup"><span data-stu-id="b78a6-153">You can enable behavior like this for your cmdlets by allowing them tooaccept a connection object directly as a parameter, instead of just connection fields for parameters.</span></span> <span data-ttu-id="b78a6-154">Obvykle je vhodné nastavit pro jednotlivé, parametr tak, aby uživatel Azure Automation nepoužívá mohl vaše rutiny volat bez vytváření zatřiďovací tabulky tooact jako objekt připojení hello.</span><span class="sxs-lookup"><span data-stu-id="b78a6-154">Usually you’ll want a parameter set for each, so that a user not using Azure Automation can call your cmdlets without constructing a hashtable tooact as hello connection object.</span></span> <span data-ttu-id="b78a6-155">Sada parametrů **SpecifyConnectionFields** níže je použité toopass vlastností polí připojení hello po jednom.</span><span class="sxs-lookup"><span data-stu-id="b78a6-155">Parameter set **SpecifyConnectionFields** below is used toopass hello connection field properties one by one.</span></span> <span data-ttu-id="b78a6-156">**UseConnectionObject** hello vám umožní předat připojení přímo pomocí.</span><span class="sxs-lookup"><span data-stu-id="b78a6-156">**UseConnectionObject** lets you pass hello connection straight through.</span></span> <span data-ttu-id="b78a6-157">Jak můžete vidět, rutina Send-TwilioSMS v hello hello [modulu Powershellu Twilio](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) umožňuje předání oběma způsoby:</span><span class="sxs-lookup"><span data-stu-id="b78a6-157">As you can see, hello Send-TwilioSMS cmdlet in hello [Twilio PowerShell module](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) allows passing either way:</span></span> 
   
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
3. <span data-ttu-id="b78a6-158">Definujte výstupní typ pro všechny rutiny v modulu hello.</span><span class="sxs-lookup"><span data-stu-id="b78a6-158">Define output type for all cmdlets in hello module.</span></span> <span data-ttu-id="b78a6-159">Definování typu výstupu pro rutiny umožňuje technologii IntelliSense návrhu toohelp určit hello výstupní vlastnosti rutiny hello použijete při vytváření obsahu.</span><span class="sxs-lookup"><span data-stu-id="b78a6-159">Defining an output type for a cmdlet allows design-time IntelliSense toohelp you determine hello output properties of hello cmdlet, for use during authoring.</span></span> <span data-ttu-id="b78a6-160">To se hodí zejména při automatizace vytváření grafického obsahu runbooku, kde je znalost doby návrhu klíče tooan snadnou práci s modulem.</span><span class="sxs-lookup"><span data-stu-id="b78a6-160">It is especially helpful during Automation runbook graphical authoring, where design time knowledge is key tooan easy user experience with your module.</span></span><br><br> <span data-ttu-id="b78a6-161">![Typ výstupu grafického runbooku](media/automation-integration-modules/runbook-graphical-module-output-type.png)</span><span class="sxs-lookup"><span data-stu-id="b78a6-161">![Graphical Runbook Output Type](media/automation-integration-modules/runbook-graphical-module-output-type.png)</span></span><br> <span data-ttu-id="b78a6-162">Tato funkce je toohello funkce rutiny "našeptávání" výstupu v prostředí PowerShell ISE bez nutnosti toorun ho.</span><span class="sxs-lookup"><span data-stu-id="b78a6-162">This is similar toohello "type ahead" functionality of a cmdlet's output in PowerShell ISE without having toorun it.</span></span><br><br> <span data-ttu-id="b78a6-163">![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)</span><span class="sxs-lookup"><span data-stu-id="b78a6-163">![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)</span></span><br>
4. <span data-ttu-id="b78a6-164">Rutiny v modulu hello by neměl trvat komplexní typy objektů jako parametry.</span><span class="sxs-lookup"><span data-stu-id="b78a6-164">Cmdlets in hello module should not take complex object types for parameters.</span></span> <span data-ttu-id="b78a6-165">Pracovní postup PowerShellu se liší od PowerShellu tím, že komplexní typy ukládá v deserializované podobě.</span><span class="sxs-lookup"><span data-stu-id="b78a6-165">PowerShell Workflow is different from PowerShell in that it stores complex types in deserialized form.</span></span> <span data-ttu-id="b78a6-166">Primitivní typy zůstanou jako primitiva, ale komplexní typy jsou převedené tootheir deserializovat verze, které jsou v podstatě kontejnery vlastností.</span><span class="sxs-lookup"><span data-stu-id="b78a6-166">Primitive types will stay as primitives, but complex types are converted tootheir deserialized versions, which are essentially property bags.</span></span> <span data-ttu-id="b78a6-167">Například, pokud jste použili hello **Get-Process** rutiny v runbooku (nebo v pracovním postupu Powershellu k tomuto účelu), by vrátit objekt typu [Deserialized.System.Diagnostic.Process], [[[očekávaný není hello Typ System.Diagnostic.Process].</span><span class="sxs-lookup"><span data-stu-id="b78a6-167">For example, if you used hello **Get-Process** cmdlet in a runbook (or a PowerShell Workflow for that matter), it would return an object of type [Deserialized.System.Diagnostic.Process], not hello expected [System.Diagnostic.Process] type.</span></span> <span data-ttu-id="b78a6-168">Tento typ má všechny hello stejné vlastnosti jako nedeserializovaný typ, ale žádná z metod hello hello.</span><span class="sxs-lookup"><span data-stu-id="b78a6-168">This type has all hello same properties as hello non-deserialized type, but none of hello methods.</span></span> <span data-ttu-id="b78a6-169">A pokud se pokusíte tuto hodnotu pro toopass jako parametr rutiny tooa, kde hello rutina očekává hodnotu [System.Diagnostic.Process] pro tento parametr, dostanete hello následující chyba: *nelze zpracovat transformaci argumentu na parametru 'proces' . Chyba: "nelze převést hodnotu"System.Diagnostics.Process (CcmExec)"hello typu"Deserialized.System.Diagnostics.Process není"tootype"System.Diagnostics.Process".*</span><span class="sxs-lookup"><span data-stu-id="b78a6-169">And if you try toopass this value as a parameter tooa cmdlet, where hello cmdlet expects a [System.Diagnostic.Process] value for this parameter, you’ll receive hello following error: *Cannot process argument transformation on parameter 'process'. Error: "Cannot convert hello "System.Diagnostics.Process (CcmExec)" value of type  "Deserialized.System.Diagnostics.Process" tootype "System.Diagnostics.Process".*</span></span>   <span data-ttu-id="b78a6-170">Je to proto, že je že Neshoda typu mezi hello očekávaný typ [System.Diagnostic.Process] a hello daného typu [Deserialized.System.Diagnostic.Process].</span><span class="sxs-lookup"><span data-stu-id="b78a6-170">This is because there is a type mismatch between hello expected [System.Diagnostic.Process] type and hello given [Deserialized.System.Diagnostic.Process] type.</span></span> <span data-ttu-id="b78a6-171">Hello způsobem řešení tohoto problému je, že tooensure hello rutiny modulu nepřebírají komplexní typy jako parametry.</span><span class="sxs-lookup"><span data-stu-id="b78a6-171">hello way around this issue is tooensure hello cmdlets of your module do not take complex types for parameters.</span></span> <span data-ttu-id="b78a6-172">Zde je ukázka nesprávného způsobu řešení toodo hello ho.</span><span class="sxs-lookup"><span data-stu-id="b78a6-172">Here is hello wrong way toodo it.</span></span>
   
    ```
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
    <br>
    <span data-ttu-id="b78a6-173">A tady je hello správné, přijímá Primitivum, které je možné interně hello rutinou toograb hello komplexní objekt a jeho použití.</span><span class="sxs-lookup"><span data-stu-id="b78a6-173">And here is hello right way, taking in a primitive that can be used internally by hello cmdlet toograb hello complex object and use it.</span></span> <span data-ttu-id="b78a6-174">Vzhledem k tomu, že rutiny se spouštějí v kontextu hello prostředí PowerShell, není pracovního postupu Powershellu, uvnitř rutiny hello $process stane správným typem [System.Diagnostic.Process] hello.</span><span class="sxs-lookup"><span data-stu-id="b78a6-174">Since cmdlets execute in hello context of PowerShell, not PowerShell Workflow, inside hello cmdlet $process becomes hello correct [System.Diagnostic.Process] type.</span></span>  
   
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
   <span data-ttu-id="b78a6-175">Assety připojení v runboocích představují zatřiďovací tabulky, které jsou pro komplexní typ, a ještě tyto zatřiďovací tabulky pravděpodobně možné toobe toobe předány do rutin pro jejich – parametr připojení perfektně, s výjimkou žádné přetypování.</span><span class="sxs-lookup"><span data-stu-id="b78a6-175">Connection assets in runbooks are hashtables, which are a complex type, and yet these hashtables seem toobe able toobe passed into cmdlets for their –Connection parameter perfectly, with no cast exception.</span></span> <span data-ttu-id="b78a6-176">Některé typy Powershellu technicky jsou možné toocast správně z jejich formuláře tootheir deserializovat serializovaných formuláře a proto mohou být předány do rutin pro parametry, které přijímají nedeserializovaný typ hello.</span><span class="sxs-lookup"><span data-stu-id="b78a6-176">Technically, some PowerShell types are able toocast properly from their serialized form tootheir deserialized form, and hence can be passed into cmdlets for parameters accepting hello non-deserialized type.</span></span> <span data-ttu-id="b78a6-177">Zatřiďovací tabulka je jedním z nich.</span><span class="sxs-lookup"><span data-stu-id="b78a6-177">Hashtable is one of these.</span></span> <span data-ttu-id="b78a6-178">Je možné, Autor modulu definované typy toobe implementovat způsobem, který můžou správně deserializovat také, ale existují některé tooconsider kompromis.</span><span class="sxs-lookup"><span data-stu-id="b78a6-178">It’s possible for a module author’s defined types toobe implemented in a way that they can correctly deserialize as well, but there are some trade-offs tooconsider.</span></span> <span data-ttu-id="b78a6-179">Hello typ potřebám toohave výchozí konstruktor, mají všechny jeho veřejné vlastnosti a a musí mít PSTypeConverter.</span><span class="sxs-lookup"><span data-stu-id="b78a6-179">hello type needs toohave a default constructor, have all of its properties public, and have a PSTypeConverter.</span></span> <span data-ttu-id="b78a6-180">Ale pro už definovaných typů, že Autor modulu hello není vlastníkem, je již příliš "Opravit", proto hello doporučení tooavoid komplexních typů jako parametrů.</span><span class="sxs-lookup"><span data-stu-id="b78a6-180">However, for already-defined types that hello module author does not own, there is no way too“fix” them, hence hello recommendation tooavoid complex types for parameters all together.</span></span> <span data-ttu-id="b78a6-181">Tip pro vytváření Runbooků: Pokud pro nějakého důvodu vaše rutiny potřebují tootake komplexní typ parametru nebo používáte cizí modul, který vyžaduje parametr komplexního typu, hello řešením pro runbooky pracovních postupů Powershellu a pracovní postupy Powershellu v místním Prostředí PowerShell, je toowrap hello rutinu, která generuje komplexní typ hello a hello rutinu, která využívá komplexní typ hello hello stejné aktivity InlineScript.</span><span class="sxs-lookup"><span data-stu-id="b78a6-181">Runbook Authoring tip: If for some reason your cmdlets need tootake a complex type parameter, or you are using someone else’s module that requires a complex type parameter, hello workaround in PowerShell Workflow runbooks and PowerShell Workflows in local PowerShell, is toowrap hello cmdlet that generates hello complex type and hello cmdlet that consumes hello complex type in hello same InlineScript activity.</span></span> <span data-ttu-id="b78a6-182">Vzhledem k tomu, že InlineScript svůj obsah spouští jako PowerShell spíš než pracovní postup prostředí PowerShell, hello rutina generující komplexní typ hello vytvoří tento správný typ, není hello deserializovat komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="b78a6-182">Since InlineScript executes its contents as PowerShell rather than PowerShell Workflow, hello cmdlet generating hello complex type would produce that correct type, not hello deserialized complex type.</span></span>
5. <span data-ttu-id="b78a6-183">Nastavte všechny rutiny v modulu hello bezstavové.</span><span class="sxs-lookup"><span data-stu-id="b78a6-183">Make all cmdlets in hello module stateless.</span></span> <span data-ttu-id="b78a6-184">Pracovní postup Powershellu spouští každou rutinu volanou v pracovním postupu hello v jiné relaci.</span><span class="sxs-lookup"><span data-stu-id="b78a6-184">PowerShell Workflow runs every cmdlet called in hello workflow in a different session.</span></span> <span data-ttu-id="b78a6-185">To znamená rutiny, které závisí na stavu relace vytvořená nebo upravená jinými rutinami ve stejném modulu nebude fungovat v runboocích pracovního postupu Powershellu hello.</span><span class="sxs-lookup"><span data-stu-id="b78a6-185">This means any cmdlets that depend on session state created / modified by other cmdlets in hello same module will not work in PowerShell Workflow runbooks.</span></span>  <span data-ttu-id="b78a6-186">Tady je příklad co není toodo.</span><span class="sxs-lookup"><span data-stu-id="b78a6-186">Here is an example of what not toodo.</span></span>
   
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
6. <span data-ttu-id="b78a6-187">modul Hello musí být plně obsažený v Xcopy možné balíčku.</span><span class="sxs-lookup"><span data-stu-id="b78a6-187">hello module should be fully contained in an Xcopy-able package.</span></span> <span data-ttu-id="b78a6-188">Protože moduly Azure Automation jsou distribuované toohello automatizace izolovaných prostorů, když se runbooky potřebují tooexecute, které potřebují toowork nezávisle na hostiteli hello, na kterém běží na.</span><span class="sxs-lookup"><span data-stu-id="b78a6-188">Because Azure Automation modules are distributed toohello Automation sandboxes when runbooks need tooexecute, they need toowork independently of hello host they are running on.</span></span> <span data-ttu-id="b78a6-189">To znamená, že by měl být schopný tooZip balíček modulu hello, přesuňte jej tooany jiného hostitele se hello stejnou nebo novější verze prostředí PowerShell a fungovat normálně, při importu do prostředí PowerShell takového hostitele.</span><span class="sxs-lookup"><span data-stu-id="b78a6-189">What this means is that you should be able tooZip up hello module package, move it tooany other host with hello same or newer PowerShell version, and have it function as normal when imported into that host’s PowerShell environment.</span></span> <span data-ttu-id="b78a6-190">Aby tento toohappen by neměl hello modulu závisí na žádném souboru mimo složku modulu hello (hello složka, kterou zazipujete při importu do Azure Automation) nebo na žádném jedinečném nastavení registrů na hostiteli, například nastavení hello instalace produktu.</span><span class="sxs-lookup"><span data-stu-id="b78a6-190">In order for that toohappen, hello module should not depend on any files outside hello module folder (hello folder that gets zipped up when importing into Azure Automation), or on any unique registry settings on a host, such as those set by hello install of a product.</span></span> <span data-ttu-id="b78a6-191">Pokud tento osvědčený postup nedodržíte, modul hello nebude použitelné ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="b78a6-191">If this best practice is not followed, hello module will not be usable in Azure Automation.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="b78a6-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b78a6-192">Next steps</span></span>

* <span data-ttu-id="b78a6-193">tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="b78a6-193">tooget started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
* <span data-ttu-id="b78a6-194">toolearn Další informace o vytváření modulů Powershellu najdete v části [zápis modulu prostředí Windows PowerShell](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="b78a6-194">toolearn more about creating PowerShell Modules, see [Writing a Windows PowerShell Module](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)</span></span>

