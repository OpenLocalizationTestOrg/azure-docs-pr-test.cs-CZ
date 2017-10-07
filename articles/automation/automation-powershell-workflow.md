---
title: "aaaLearning pracovní postup prostředí PowerShell pro Azure Automation | Microsoft Docs"
description: "Tento článek je určený jako rychlé lekce pro autory obeznámeni s prostředí PowerShell toounderstand hello určité rozdíly mezi prostředí PowerShell a pracovní postup prostředí PowerShell a použít tooAutomation koncepty sady runbook."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 84bf133e-5343-4e0e-8d6c-bb14304a70db
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 362c504eb96d31b99a826b128e6a591beecaa084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a>Učení klíčové koncepty pracovního postupu prostředí Windows PowerShell pro automatizaci sady runbook 
Runbooky ve službě Azure Automation se implementují jako pracovní postupy prostředí Windows PowerShell.  Pracovní postup prostředí Windows PowerShell je skript prostředí Windows PowerShell podobně jako tooa, ale existují některé významné rozdíly, které může být matoucí tooa nového uživatele.  Tento článek je určený toohelp zápisu sady runbook pomocí prostředí PowerShell. pracovní postup, doporučujeme, abyste že zápisu sady runbook pomocí prostředí PowerShell, pokud potřebujete kontrolní body.  Existuje několik rozdílů v syntaxi při autorizaci sad runbook PowerShell Workflow a tyto rozdíly vyžaduje trochu další pracovní toowrite efektivní pracovních postupů.  

Pracovní postup je pořadí naprogramovaných, propojených kroků, které provádějí dlouhodobé úlohy nebo vyžadují koordinaci více kroků hello v rámci více zařízení nebo spravovaných uzlů. Hello výhody pracovního postupu oproti běžnému skriptu patří schopnost hello toosimultaneously provádět akci vůči několika zařízením a tooautomatically hello možnost obnovení v případě selhání. Pracovní postup prostředí Windows PowerShell je skript prostředí Windows PowerShell, který používá Windows Workflow Foundation. Když pracovní postup hello napsané pomocí syntaxe prostředí Windows PowerShell a spuštění pomocí prostředí Windows PowerShell, je zpracován programovacím modelem Windows Workflow Foundation.

Kompletní informace o hello témata v tomto článku najdete v části [Začínáme s pracovním postupem prostředí Windows PowerShell](http://technet.microsoft.com/library/jj134242.aspx).

## <a name="basic-structure-of-a-workflow"></a>Základní struktura pracovního postupu
Hello první krok tooconverting pracovního postupu Powershellu tooa skriptu prostředí PowerShell je uveden s hello **pracovního postupu** – klíčové slovo.  Pracovní postup začíná hello **pracovního postupu** – klíčové slovo, za nímž následuje hello textu hello skriptu uzavřené do složených závorek. Následuje Hello název pracovního postupu hello hello **pracovního postupu** – klíčové slovo, jak je znázorněno v hello následující syntaxi:

    Workflow Test-Workflow
    {
       <Commands>
    }

Název Hello hello pracovního postupu musí odpovídat názvu hello hello automatizace sady runbook. Pokud hello runbook importována, pak hello filename musí odpovídat názvu pracovního postupu hello a musí končit *.ps1*.

pracovní postup toohello parametry tooadd, použijte hello **Param** – klíčové slovo stejně jako tooa skriptu.

## <a name="code-changes"></a>Změny kódu
Kód pracovního postupu Powershellu vypadá kód skriptu tooPowerShell téměř shodné s výjimkou několik významných změn.  Hello následující oddíly popisují změny, je nutné, skript prostředí PowerShell tooa toomake pro něj toorun v pracovním postupu.

### <a name="activities"></a>Aktivity
Aktivita je specifická úloha v pracovním postupu. Stejně jako se skript skládá z jednoho nebo více příkazů, pracovní postup se skládá z jedné nebo více aktivit, které se provádějí v pořadí. Pracovní postup prostředí Windows PowerShell automaticky převádí, mnoho hello tooactivities rutiny prostředí Windows PowerShell při spuštění pracovního postupu. Pokud určíte jednu z těchto rutin ve vašem runbooku, hello odpovídající aktivita spuštěna programovacím modelem Windows Workflow Foundation. Těchto rutin bez odpovídající aktivity pracovního postupu prostředí Windows PowerShell automaticky spustí rutinu hello v rámci [InlineScript](#inlinescript) aktivity. Existuje sada rutin, které jsou vyloučeny a nejde ho použít v pracovním postupu, pokud je výslovně nezahrnete v bloku InlineScript. Další informace o těchto pojmech najdete v části [pomocí aktivity ve skriptových pracovních postupech](http://technet.microsoft.com/library/jj574194.aspx).

Aktivity pracovních postupů sdílejí sadu společné parametry tooconfigure jejich provoz. Podrobnosti o společných parametrech pracovního postupu hello najdete v tématu [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Poziční parametry
Poziční parametry nelze použít s rutinami v pracovním postupu a aktivity.  To znamená je, že je nutné použít názvy parametrů.

Zvažte například následující kód, který získá všechny spuštěné služby hello.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Pokud se pokusíte toorun Tento stejný kód v pracovním postupu, obdržíte zprávu jako "Sadu nelze vyřešit pomocí hello zadaný parametr pojmenované parametry."  toocorrect, zadejte název parametru hello jako následující hello.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Deserializovaná objekty
Objekty v pracovních postupech jsou deserializovat.  To znamená, že jejich vlastnosti jsou stále k dispozici, ale není své metody.  Zvažte například následující kód prostředí PowerShell, který zastaví službu pomocí metody Stop hello objektu služby hello hello.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Pokud se pokusíte toorun to v pracovním postupu, se zobrazí chyba s oznámením "volání metody není podporováno v pracovním postupu Windows PowerShell".  

Jednou z možností je toowrap tyto dva řádky kódu [InlineScript](#inlinescript) blokovat v takovém případě $Service bude objekt služby v rámci bloku hello.

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    }

Další možností je toouse jiná rutina, která provádí hello stejné funkce jako metoda hello, pokud je k dispozici.  V našem příkladu rutiny hello Stop-Service poskytuje hello stejné funkce jako metoda Stop hello a můžete použít následující hello pro pracovní postup.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript
Hello **InlineScript** aktivity je užitečné, když potřebujete jeden nebo více příkazů pro toorun jako tradiční skript prostředí PowerShell namísto pracovního postupu Powershellu.  Zatímco příkazy v pracovním postupu jsou odesílány pro zpracování tooWindows Workflow Foundation, příkazy v bloku InlineScript jsou zpracovány prostředím Windows PowerShell.

Aktivita InlineScript používá hello následující syntaxe uvedená níže.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Výstup můžete vrátit InlineScript přiřazením proměnné tooa výstup hello. Hello následující příklad zastaví službu a poté uloží název služby hello.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Můžete předat hodnoty do bloku InlineScript, ale musíte použít **$Using** modifikátor oboru.  Hello následující příklad je identické toohello předchozí příklad s tím rozdílem, že hello služby poskytl název proměnné.

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"

        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Aktivity InlineScript může být důležité v určitých pracovních postupech, nepodporují konstrukce pracovního postupu a musí být použit pouze v případě potřeby pro hello následujících důvodů:

* Nemůžete použít [kontrolní body](#checkpoints) uvnitř bloku InlineScript. V případě selhání v rámci bloku hello musí pokračovat znovu od začátku hello hello bloku.
* Nemůžete použít [paralelní provádění](#parallel-processing) uvnitř InlineScriptBlock.
* InlineScript ovlivňuje škálovatelnost hello pracovního postupu, protože drží relaci prostředí Windows PowerShell hello pro celou délku bloku InlineScript hello hello.

Další informace o používání InlineScript najdete v tématu [spouštění příkazů prostředí Windows PowerShell v pracovním postupu](http://technet.microsoft.com/library/jj574197.aspx) a [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).

## <a name="parallel-processing"></a>Paralelní zpracování
Jednou z výhod pracovních postupů prostředí Windows PowerShell je hello možnost tooperform sadu příkazů paralelně namísto postupně jako v případě typického skriptu.

Můžete použít hello **paralelní** – klíčové slovo toocreate blok skriptu s více příkazy, které běží souběžně. Tato služba využívá hello následující syntaxe uvedená níže. V takovém případě se aktivity "activity1" a "activity2" spustí na hello stejnou dobu. Aktivita "activity3" spustí až po dokončení aktivity "activity1" a "activity2".

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Zvažte například následující příkazy prostředí PowerShell, které kopírování více cílové sítě tooa soubory hello.  Tyto příkazy se spouštějí postupně, takže než jednoho souboru se musí ukončit kopírování před příštím spuštění hello.     

    Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

Hello následující pracovní postup spouští tyto stejné příkazy paralelně, aby všechny spuštění kopírování v hello stejný čas.  Až poté, co jsou všechny zkopírovat je hello dokončení zobrazí zpráva.

    Workflow Copy-Files
    {
        Parallel
        {
            Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


Můžete použít hello **ForEach-Parallel** současně vytvořit tooprocess příkazy pro každou položku v kolekci. Hello položky v kolekci hello se zpracovávají paralelně, zatímco hello příkazy v bloku skriptu hello spouští sekvenčně. Tato služba využívá hello následující syntaxe uvedená níže. V takovém případě aktivity "activity1" spustí hello stejný čas pro všechny položky v kolekci hello. Pro každou položku "activity2" spustí po dokončení aktivity "activity1". Aktivita "activity3" spustí až po dokončení aktivity "activity1" a "activity2" pro všechny položky.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

Následující ukázka Hello je podobné předchozí příklad toohello kopírování souborů paralelně.  V takovém případě se zobrazí zpráva pro každý soubor, poté, co se zkopíruje.  Až poté, co jsou všechny úplně zkopírovat je hello konečné dokončení zobrazí zpráva.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files)
        {
            Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }

        Write-Output "All files copied."
    }

> [!NOTE]
> Nedoporučujeme systémem podřízené runbooky paralelně vzhledem k tomu, že to se ukázalo toogive nespolehlivé výsledky.  Hello výstup podřízeného runbooku hello někdy nezobrazuje a nastavení v jedné podřízené sady runbook může ovlivnit hello jiné paralelní podřízené runbooky
>

## <a name="checkpoints"></a>Kontrolní body
A *kontrolního bodu* je snímek aktuálního stavu hello hello pracovního postupu, který zahrnuje hello aktuální hodnotu proměnných a výstup generovaný toothat bodu. Pokud pracovní postup končí chybu nebo je pozastaveno, pak hello při příštím spuštění se začne od svého posledního kontrolního bodu místo hello začátek hello worfklow.  Můžete nastavit kontrolní bod v pracovním postupu s hello **Checkpoint-Workflow** aktivity.

V následující ukázkový kód hello dojde k výjimce po způsobuje "activity2" hello tooend pracovního postupu. Při dalším spuštění pracovního postupu hello, spustí se spuštěním aktivity Activity2, protože byla poslední po nastavení posledního kontrolního bodu hello.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

Po opakovaných aktivit, které mohou být náchylné k chybám tooexception a neměla by být pokud obnovení hello pracovního postupu byste měli nastavit kontrolní body v pracovním postupu. Pracovní postup může například vytvořit virtuální počítač. Můžete nastavit kontrolní bod před i po hello příkazy toocreate hello virtuálního počítače. Pokud hello vytváření nezdaří, by hello příkazy opakovat, pokud je hello pracovní postup spustit znovu. Pokud hello worfklow selže po vytvoření hello úspěšné, pak hello virtuální počítač nebude znovu vytvořen při obnovení pracovního postupu hello.

Následující ukázka Hello zkopíruje více souborů tooa síťové umístění a nastaví kontrolní bod po každý soubor.  Pokud dojde ke ztrátě hello umístění v síti, pracovní postup hello končí v chybě.  Když spustíte znovu, bude pokračovat v hello posledního kontrolního bodu, což znamená, že jsou přeskočeny hello pouze soubory, které již byly zkopírovány.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files)
        {
            Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }

        Write-Output "All files copied."
    }

Protože uživatelské jméno pověření nejsou trvalé po zavolání metody hello [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) aktivity nebo po hello posledního kontrolního bodu, potřebujete tooset hello přihlašovací údaje toonull a potom je znovu načíst z úložiště asset hello po  **Suspend-Workflow** nebo je vyvolán kontrolní bod.  V ostatních případech se může zobrazit hello následující chybová zpráva: *úlohy pracovního postupu hello nelze pokračovat, buď protože trvalosti dat nelze uložit zcela nebo uložit trvalosti dat je poškozeno. Je nutné restartovat hello pracovního postupu.*

Hello následující stejný kód ukazuje, jak toohandle to vaše runbooky pracovních postupů Powershellu.

    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first toocreate hello VM (code not shown)

          # Now add hello VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     }


Není požadováno v případě se ověřujete pomocí nakonfigurovaný s hlavní službou účet Spustit jako.  

Další informace o kontrolních bodech najdete v tématu [přidání kontrolních bodů tooa pracovní postup skriptu](http://technet.microsoft.com/library/jj574114.aspx).

## <a name="next-steps"></a>Další kroky
* tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md)
