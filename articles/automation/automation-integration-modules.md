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
# <a name="azure-automation-integration-modules"></a>Moduly integrace pro Azure Automation
PowerShell je základní technologií hello za Azure Automation. Vzhledem k tomu, že Azure Automation je postavená na Powershellu, jsou moduly Powershellu klíče toohello rozšiřitelnosti služby Azure Automation. V tomto článku jsme vás provede hello specifiky používání Azure Automation moduly Powershellu, označuje tooas "Moduly integrace" a osvědčených postupů pro vytváření vlastních modulů prostředí PowerShell toomake se, že fungují jako moduly integrace v rámci Automatizace Azure. 

## <a name="what-is-a-powershell-module"></a>Co je modul PowerShellu?
Modul Powershellu je skupina rutin Powershellu, například **Get-Date** nebo **Copy-Item**, který lze použít z konzole PowerShell text hello, skripty, pracovní postupy, sady runbook a prostředcích DSC Powershellu, jako WindowsFeature nebo File, které lze použít v konfiguracích DSC Powershellu. Všechny funkce hello prostředí PowerShell je k dispozici prostřednictvím rutin a prostředků DSC a každý rutina nebo prostředek DSC je zálohovaný díky modul prostředí PowerShell řada z nich se dodává přímo s Powershellem. Například hello **Get-Date** rutiny je součástí hello modulu Powershellu Microsoft.PowerShell.Utility a **Copy-Item** rutiny je součástí modulu Powershellu Microsoft.PowerShell.Management hello a Hello prostředek DSC balíček je součástí modulu powershellu PSDesiredStateConfiguration hello. Oba tyto moduly se dodávají spolu s PowerShellem. Ale řada modulů Powershellu se nedodává jako součást Powershellu a místo toho se distribuují s produkty první nebo třetích stran, jako je System Center 2012 Configuration Manager nebo hello rozsáhlé komunity Powershellu na místech, jako jsou Galerie prostředí PowerShell.  Hello moduly jsou užitečné, protože složité úlohy zjednodušují prostřednictvím zapouzdřené funkcionality.  Další informace o [modulech PowerShellu najdete na webu MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx). 

## <a name="what-is-an-azure-automation-integration-module"></a>Co je modul integrace pro Azure Automation?
Modul integrace se příliš neliší od modulu PowerShellu. Jeho prostě modul Powershellu, který volitelně obsahuje jeden další soubor – soubor metadat určujících toobe typu připojení automatizace Azure používat s rutinami modulu hello v sadách runbook. Volitelný soubor nebo Ne, tyto PowerShell moduly může být importován do Azure Automation toomake jejich rutin, které jsou k dispozici pro použití v rámci sady runbook a jejich prostředků DSC k dispozici pro použití v rámci konfigurací DSC. Pozadí hello Azure Automation tyto moduly ukládá a v runbooku a dobu provádění úlohy kompilace DSC je načte do karantény hello Azure Automation, kde se runbooky spustí a konfigurace DSC se zkompilují.  Veškeré prostředky DSC v modulech se také automaticky umístí na serveru vyžádané replikace Automation DSC hello, tak, aby si je mohly vyžádat počítače pokus tooapply konfigurace DSC.  

Počet modulů prostředí Azure PowerShell dodávané předinstalované hello ve službě Azure Automation pro jste toouse, můžete začít používat hned automatizací správy Azure, ale můžete naimportovat moduly Powershellu pro libovolnou systému, služby nebo nástroje chcete toointegrate s. 

> [!NOTE]
> Některé moduly jsou dodávána jako "globální moduly" hello služby Automation. Tyto globální moduly jsou k dispozici tooyou při vytvoření účtu automation a budeme je aktualizovat někdy, který je automaticky přenáší tooyour účet automation. Pokud nechcete, aby je toobe automaticky aktualizované, můžete vždy importovat hello stejném modulu sami a které se mají přednost před verze globálního modulu hello tohoto modulu dodávané ve službě hello. 

Hello formát, ve kterém budete importovat balíček modulu integrace je komprimovaný soubor s hello stejný název jako hello modul a má příponu .zip. Obsahuje modul Windows PowerShell hello a všechny podpůrné soubory včetně souboru manifestu (.psd1), pokud modul hello jeden má.

Pokud hello modul obsahovat typ připojení Azure Automation, musí obsahovat také soubor s názvem hello `<ModuleName>-Automation.json` který určuje vlastnosti typu připojení hello. Toto je soubor json, který je umístěn v rámci hello složce modulu vašeho komprimovaného souboru .zip a obsahuje hello pole "připojení", který je požadovaný tooconnect toohello systému nebo služby hello modul představuje. Tím dojdete k vytvoření typu připojení ve službě Azure Automation. Použití tohoto souboru můžete nastavit hello názvy polí, typy, a zda mají být hello pole šifrovaná nebo volitelná, pro typ připojení hello hello modulu. Hello následuje šablonu ve formátu souboru json hello:

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

Pokud jste nasadili Service Management Automation a vytvářením balíčků modulů integrace pro svoje automatizační runbooky, to by měl vypadat dobře obeznámeni tooyou. 

## <a name="authoring-best-practices"></a>Osvědčené postupy pro vytváření obsahu
I když jsou moduly integrace v podstatě moduly Powershellu, je stále existuje řada věcí, doporučujeme zvážit při vytváření modulu Powershellu, toomake ho nejvíce použitelné ve službě Azure Automation. Některé z nich se týkají konkrétně Azure Automation a jiné jsou užitečné pouze toomake moduly dobře fungovaly v prostředí PowerShell Workflow, bez ohledu na to, jestli používáte Automation. 

1. Přidejte i stručný obsah, popis a pomocný identifikátor URI pro každé rutině v modulu hello. V prostředí PowerShell, můžete definovat určité informace nápovědy rutin tooallow hello uživatele tooreceive nápovědu k jejich používání s hello **Get-Help** rutiny. Dáme vám příklad, jak můžete definovat stručný obsah a pomocný identifikátor URI pro modul PowerShellu, který je zapsaný v souboru .psm1.<br>  
   
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
   <br> Zadání těchto informací nebude zobrazovat pouze této nápovědy pomocí hello **Get-Help** hello rutiny v konzole PowerShell, zveřejní také tuto funkci nápovědy v rámci Azure Automation.  Například při vkládání aktivit během vytváření obsahu runbooku. Kliknutím na "Zobrazit podrobnou nápovědu" bude hello otevřete pomocný identifikátor URI na jiné kartě hello webové prohlížeče používáte tooaccess Azure Automation.<br>![Nápověda k modulu integrace](media/automation-integration-modules/automation-integration-module-activitydesc.png)
2. Pokud hello modul spouští na vzdáleném systému,

    a. Měl by obsahovat soubor metadat modulu integrace, který definuje hello informace potřebné tooconnect toothat vzdáleného systému, což znamená typ připojení hello.  
    b. Každá rutina v modulu hello by měl být schopný tootake objekt připojení (instanci takového typu připojení) jako parametr.  

    Rutiny v modulu hello stát jednodušší toouse ve službě Azure Automation, pokud povolíte předávání objektů s hello pole typu připojení hello jako parametr rutiny toohello. Tímto způsobem uživatelé nemají toomap parametry hello připojení asset toohello na odpovídající parametry rutiny při každém volání rutiny. Založeno na výše uvedený příklad runbooku hello, používá asset připojení Twilio nazývá CorpTwilio tooaccess Twilio a vrátí všechny hello telefonní čísla v účtu hello.  Všimněte si, jak mapuje pole hello hello připojení toohello parametrů rutiny hello?<br>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    Tooapproach jednodušším a lepším způsobem to přímo předává hello připojení objekt toohello rutiny –
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   
    Takové chování pro vaše rutiny můžete povolit tím, že je tooaccept objekt připojení přímo jako parametr místo právě připojení pole parametrů. Obvykle je vhodné nastavit pro jednotlivé, parametr tak, aby uživatel Azure Automation nepoužívá mohl vaše rutiny volat bez vytváření zatřiďovací tabulky tooact jako objekt připojení hello. Sada parametrů **SpecifyConnectionFields** níže je použité toopass vlastností polí připojení hello po jednom. **UseConnectionObject** hello vám umožní předat připojení přímo pomocí. Jak můžete vidět, rutina Send-TwilioSMS v hello hello [modulu Powershellu Twilio](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) umožňuje předání oběma způsoby: 
   
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
3. Definujte výstupní typ pro všechny rutiny v modulu hello. Definování typu výstupu pro rutiny umožňuje technologii IntelliSense návrhu toohelp určit hello výstupní vlastnosti rutiny hello použijete při vytváření obsahu. To se hodí zejména při automatizace vytváření grafického obsahu runbooku, kde je znalost doby návrhu klíče tooan snadnou práci s modulem.<br><br> ![Typ výstupu grafického runbooku](media/automation-integration-modules/runbook-graphical-module-output-type.png)<br> Tato funkce je toohello funkce rutiny "našeptávání" výstupu v prostředí PowerShell ISE bez nutnosti toorun ho.<br><br> ![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)<br>
4. Rutiny v modulu hello by neměl trvat komplexní typy objektů jako parametry. Pracovní postup PowerShellu se liší od PowerShellu tím, že komplexní typy ukládá v deserializované podobě. Primitivní typy zůstanou jako primitiva, ale komplexní typy jsou převedené tootheir deserializovat verze, které jsou v podstatě kontejnery vlastností. Například, pokud jste použili hello **Get-Process** rutiny v runbooku (nebo v pracovním postupu Powershellu k tomuto účelu), by vrátit objekt typu [Deserialized.System.Diagnostic.Process], [[[očekávaný není hello Typ System.Diagnostic.Process]. Tento typ má všechny hello stejné vlastnosti jako nedeserializovaný typ, ale žádná z metod hello hello. A pokud se pokusíte tuto hodnotu pro toopass jako parametr rutiny tooa, kde hello rutina očekává hodnotu [System.Diagnostic.Process] pro tento parametr, dostanete hello následující chyba: *nelze zpracovat transformaci argumentu na parametru 'proces' . Chyba: "nelze převést hodnotu"System.Diagnostics.Process (CcmExec)"hello typu"Deserialized.System.Diagnostics.Process není"tootype"System.Diagnostics.Process".*   Je to proto, že je že Neshoda typu mezi hello očekávaný typ [System.Diagnostic.Process] a hello daného typu [Deserialized.System.Diagnostic.Process]. Hello způsobem řešení tohoto problému je, že tooensure hello rutiny modulu nepřebírají komplexní typy jako parametry. Zde je ukázka nesprávného způsobu řešení toodo hello ho.
   
    ```
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
    <br>
    A tady je hello správné, přijímá Primitivum, které je možné interně hello rutinou toograb hello komplexní objekt a jeho použití. Vzhledem k tomu, že rutiny se spouštějí v kontextu hello prostředí PowerShell, není pracovního postupu Powershellu, uvnitř rutiny hello $process stane správným typem [System.Diagnostic.Process] hello.  
   
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
   Assety připojení v runboocích představují zatřiďovací tabulky, které jsou pro komplexní typ, a ještě tyto zatřiďovací tabulky pravděpodobně možné toobe toobe předány do rutin pro jejich – parametr připojení perfektně, s výjimkou žádné přetypování. Některé typy Powershellu technicky jsou možné toocast správně z jejich formuláře tootheir deserializovat serializovaných formuláře a proto mohou být předány do rutin pro parametry, které přijímají nedeserializovaný typ hello. Zatřiďovací tabulka je jedním z nich. Je možné, Autor modulu definované typy toobe implementovat způsobem, který můžou správně deserializovat také, ale existují některé tooconsider kompromis. Hello typ potřebám toohave výchozí konstruktor, mají všechny jeho veřejné vlastnosti a a musí mít PSTypeConverter. Ale pro už definovaných typů, že Autor modulu hello není vlastníkem, je již příliš "Opravit", proto hello doporučení tooavoid komplexních typů jako parametrů. Tip pro vytváření Runbooků: Pokud pro nějakého důvodu vaše rutiny potřebují tootake komplexní typ parametru nebo používáte cizí modul, který vyžaduje parametr komplexního typu, hello řešením pro runbooky pracovních postupů Powershellu a pracovní postupy Powershellu v místním Prostředí PowerShell, je toowrap hello rutinu, která generuje komplexní typ hello a hello rutinu, která využívá komplexní typ hello hello stejné aktivity InlineScript. Vzhledem k tomu, že InlineScript svůj obsah spouští jako PowerShell spíš než pracovní postup prostředí PowerShell, hello rutina generující komplexní typ hello vytvoří tento správný typ, není hello deserializovat komplexního typu.
5. Nastavte všechny rutiny v modulu hello bezstavové. Pracovní postup Powershellu spouští každou rutinu volanou v pracovním postupu hello v jiné relaci. To znamená rutiny, které závisí na stavu relace vytvořená nebo upravená jinými rutinami ve stejném modulu nebude fungovat v runboocích pracovního postupu Powershellu hello.  Tady je příklad co není toodo.
   
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
6. modul Hello musí být plně obsažený v Xcopy možné balíčku. Protože moduly Azure Automation jsou distribuované toohello automatizace izolovaných prostorů, když se runbooky potřebují tooexecute, které potřebují toowork nezávisle na hostiteli hello, na kterém běží na. To znamená, že by měl být schopný tooZip balíček modulu hello, přesuňte jej tooany jiného hostitele se hello stejnou nebo novější verze prostředí PowerShell a fungovat normálně, při importu do prostředí PowerShell takového hostitele. Aby tento toohappen by neměl hello modulu závisí na žádném souboru mimo složku modulu hello (hello složka, kterou zazipujete při importu do Azure Automation) nebo na žádném jedinečném nastavení registrů na hostiteli, například nastavení hello instalace produktu. Pokud tento osvědčený postup nedodržíte, modul hello nebude použitelné ve službě Azure Automation.  

## <a name="next-steps"></a>Další kroky

* tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md)
* toolearn Další informace o vytváření modulů Powershellu najdete v části [zápis modulu prostředí Windows PowerShell](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)

