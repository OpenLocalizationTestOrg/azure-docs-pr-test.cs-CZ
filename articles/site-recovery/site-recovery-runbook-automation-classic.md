---
title: "aaaAdd služby Azure automation runbook toorecovery plány portálu classic hello | Microsoft Docs"
description: "Tento článek popisuje, jak Azure Site Recovery teď můžete pomocí Azure Automation toocomplete složité úlohy během obnovení tooAzure plány obnovení tooextend"
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: f24eaa62-9dea-4fce-92e1-a72513ca0289
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 3bb7420911afbce289b656f28823b1923e8af0c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-azure-automation-runbooks-toorecovery-plans-in-hello-classic-portal"></a>Přidat plány toorecovery sady runbook služby Azure automation na portálu classic hello
Tento kurz popisuje, jak Azure Site Recovery integruje se službou Azure Automation tooprovide rozšiřitelnost toorecovery plány. Plány obnovení můžete orchestraci obnovení chránit pomocí Azure Site Recovery pro cloud toosecondary replikace a scénáře tooAzure replikace virtuálních počítačů. Také pomoci při obnovení hello **přesné**, **repeatable**, a **automatizované**. Pokud jsou selhání tooAzure vaše virtuální počítače, integraci s Azure Automation rozšiřuje plány obnovení a poskytuje schopnost tooexecute sady runbook, což umožňuje efektivní automatizace úloh.

Pokud jste ještě se dozvěděli o službě Azure Automation ještě, zaregistrujte si [sem](https://azure.microsoft.com/services/automation/) a stáhnout jejich ukázkové skripty [zde](https://azure.microsoft.com/documentation/scripts/). Další informace o [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) a způsob, jak tooAzure tooorchestrate obnovení pomocí obnovení plánuje [zde](https://azure.microsoft.com/blog/?p=166264).

V tomto kurzu krátké se podíváme na tom, jak můžete integrovat Azure Automation runbook do plánů obnovení. Nemůžeme se automatizovat jednoduché úkolů, které dříve vyžadují ruční zásah a najdete v části Jak tooconvert více krok obnovení do akce obnovení jedním kliknutím. Podíváme se také na řešení k jednoduchého skriptu, pokud se nepovede.

## <a name="protect-hello-application-tooazure"></a>Chránit tooAzure aplikace hello
Dejte nám začněte jednoduché aplikace, která obsahuje dva virtuální počítače. Zde máme aplikace HRweb ze společnosti Fabrikam. Společnost Fabrikam. HRweb front-end a back-Fabrikam-Hrweb end jsou hello dva virtuální počítače chráněné tooAzure pomocí Azure Site Recovery. tooprotect hello virtuálních počítačů pomocí Azure Site Recovery, postupujte podle následujících kroků hello.

1. Povolení ochrany pro virtuální počítače.
2. Zajistěte, aby virtuální počítače hello dokončit počáteční replikaci a jsou replikace.
3. Počkejte na dokončení počáteční replikace hello a hello stav replikace uvádí chráněné.

## ![](media/site-recovery-runbook-automation/01.png)
V tomto kurzu vytvoříme plán obnovení pro společnost Fabrikam HRweb aplikace toofailover hello aplikace tooAzure hello. Potom jsme se integrovat do sady runbook, která vytvoří koncový bod na hello při selhání virtuálního počítače Azure tooserve webových stránek na portu 80.

Nejdříve vytvoříme plán obnovení pro naši aplikaci.

## <a name="create-hello-recovery-plan"></a>Vytvoření plánu obnovení hello
tooAzure aplikace hello toorecover, musíte toocreate plán obnovení.
Pomocí plán obnovení, že které lze nastavit pořadí hello obnovení virtuálních počítačů. Hello virtuálního počítače umístěny do skupiny 1 se obnovit a spusťte první a postupujte hello virtuálního počítače ve skupině 2.

Vytvořte do plánu obnovení, který může vypadat níže.

![](media/site-recovery-runbook-automation/12.png)

Další informace o plány obnovení, přečtěte si dokumentaci tooread [sem](https://msdn.microsoft.com/library/azure/dn788799.aspx "zde").

V dalším kroku vytvoříme hello nezbytné artefakty ve službě Azure Automation.

## <a name="create-hello-automation-account-and-its-assets"></a>Vytvoření účtu automation hello a její prostředky
Budete potřebovat runbooky toocreate účet Azure Automation. Pokud již účet nemáte, přejděte karta automatizace tooAzure označený jako ![](media/site-recovery-runbook-automation/02.png)a vytvořit nový účet.

1. Dejte účtu hello tooidentify název s.
2. Zadejte geografické oblasti, kde chcete tooplace hello účtu.

Je doporučeno tooplace hello účet v hello stejné oblasti jako trezor hello automatické obnovení systému.

![](media/site-recovery-runbook-automation/03.png)

Dále vytvořte hello následující prostředky v hello účtu.

### <a name="add-a-subscription-name-as-asset"></a>Přidejte název odběru jako prostředek
1. Přidejte nové nastavení ![](media/site-recovery-runbook-automation/04.png) v hello prostředky Azure Automation a vyberte příliš![](media/site-recovery-runbook-automation/05.png)
2. Vyberte typ proměnné hello jako **řetězec**
3. Zadejte název proměnné jako **AzureSubscriptionName**

   ![](media/site-recovery-runbook-automation/06.png)
4. Jako hodnotu proměnné hello zadejte skutečný název předplatného Azure.

   ![](media/site-recovery-runbook-automation/07_1.png)

Můžete určit název hello předplatného ze stránky hello nastavení svého účtu na hello portálu Azure.

### <a name="add-an-azure-login-credential-as-asset"></a>Přidat jako asset přihlašovacích údajů přihlášení k Azure
Automatizace Azure používá prostředí Azure PowerShell tooconnect toothe předplatného a funguje na hello artefakty existuje. V takovém případě budete muset ověřit pomocí účtu Microsoft nebo pracovní nebo školní účet.
Přihlašovací údaje účtu hello můžete uložit ve toobe asset používá bezpečně hello runbook.

1. Přidejte nové nastavení ![](media/site-recovery-runbook-automation/04.png) v hello prostředky Azure Automation a vyberte![](media/site-recovery-runbook-automation/09.png)
2. Vyberte typ přihlašovacích údajů hello jako **přihlašovací údaje Windows PowerShell**
3. Zadejte název hello jako **AzureCredential**

   ![](media/site-recovery-runbook-automation/10.png)
4. Zadejte hello uživatelské jméno a heslo toosign v s.

Obě tato nastavení jsou nyní k dispozici v vaše prostředky.

![](media/site-recovery-runbook-automation/11.png)

Další informace o tom, jak je zadána tooconnect tooyour předplatné přes PowerShell [zde](/powershell/azure/overview).

V dalším kroku vytvoříte sady runbook ve službě Azure Automation, které můžete přidat koncový bod pro hello front-end virtuální počítač po převzetí služeb při selhání.

## <a name="azure-automation-context"></a>Kontext služby Azure automation
Automatické obnovení systému předá kontextu proměnné toohello runbook toohelp napíšete skripty deterministický. Jeden může uvádějí, že jsou předvídatelný hello názvy hello cloudové služby a hello virtuálního počítače, ale se stane, že není vždy hello případ díky toocertain scénáře, jako je hello jeden kde byl změněn název hello hello virtuálního počítače z důvodu toounsupported znaků v Azure. Proto tyto informace se předá plán obnovení toohello automatické obnovení systému jako součást hello *kontextu*.

Níže je příklad, jak vypadá hello kontextové proměnné.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Následující tabulka Hello obsahuje název a popis pro každou proměnnou v kontextu hello.

| **Název proměnné** | **Popis** |
| --- | --- |
| RecoveryPlanName |Název plánu spuštěn. Pomáhá provedení akce podle názvu pomocí hello stejný skriptu |
| FailoverType |Určuje, zda je převzetí služeb při selhání hello otestovat, plánovaná nebo neplánovaná. |
| FailoverDirection |Určete, zda je obnovení tooprimary nebo sekundární |
| GroupID |Když běží hello plán identifikovat hello číslo skupiny v rámci plánu obnovení hello |
| VmMap |Pole všech hello virtuálních počítačů ve skupině hello |
| Klíč VMMap |Jedinečný klíč (GUID) pro každý virtuální počítač. Má hello stejné jako hello ID VMM hello virtuálního počítače, kde je to možné. |
| RoleName |Název virtuálního počítače Azure, který obnovuje hello |
| CloudServiceName |Název Azure cloudové služby v rámci které hello při vytvoření virtuálního počítače. |

tooidentify hello VmMap klíč v kontextu hello můžete také přejít toohello stránku vlastností virtuálního počítače v nástroji automatické obnovení systému a podívejte se na hello vlastnost identifikátor GUID virtuálního počítače.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Autor runbook služby Automation.
Teď vytvořte hello runbook tooopen port 80 hello front-end virtuálního počítače.

1. Vytvořit novou sadu runbook v hello účet Azure Automation s názvem hello **OpenPort80**

   ![](media/site-recovery-runbook-automation/14.png)
2. Přejděte toohello zobrazení Autor sady runbook hello a zadejte hello režimu konceptu.
3. Nejprve zadejte proměnné toouse hello jako kontext plán obnovení hello

   ```
       param (
           [Object]$RecoveryPlanContext
       )

   ```
4. Další připojení odběru toohello pomocí přihlašovacích údajů a předplatné názvu hello

   ```
       $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

       # Connect tooAzure
       $AzureAccount = Add-AzureAccount -Credential $Cred
       $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
       Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
   ```

   Všimněte si, že používáte hello Azure prostředky – **AzureCredential** a **AzureSubscriptionName** sem.
5. Nyní zadejte hello koncový bod podrobnosti a hello GUID hello virtuálního počítače, pro které chcete tooexpose hello koncový bod. V tomto případu hello front-end virtuálním počítači.

   ```
       # Specify hello parameters toobe used by hello script
       $AEProtocol = "TCP"
       $AELocalPort = 80
       $AEPublicPort = 80
       $AEName = "Port 80 for HTTP"
       $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
   ```

   Určuje hello protokol koncového bodu Azure, místního portu na hello virtuálního počítače a její namapované veřejný port. Tyto proměnné jsou parametry vyžadované hello Azure příkazy, které přidat tooVMs koncové body. Hello VMGUID obsahuje hello hello virtuálního počítače, které potřebujete toooperate na identifikátor GUID.
6. skript Hello teď extrahovat hello kontext pro hello zadaný identifikátor GUID virtuálního počítače a na virtuálním počítači hello odkazuje ho vytvořit koncový bod.

   ```
       #Read hello VM GUID from hello context
       $VM = $RecoveryPlanContext.VmMap.$VMGUID

       if ($VM -ne $null)
       {
           # Invoke pipeline commands within an InlineScript

           $EndpointStatus = InlineScript {
               # Invoke hello necessary pipeline commands tooadd a Azure Endpoint tooa specified Virtual Machine
               # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

               $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                   Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                   Update-AzureVM
               Write-Output $Status
           }
       }
   ```
7. Po dokončení, stiskněte tlačítko Publikovat ![](media/site-recovery-runbook-automation/20.png) tooallow váš skript toobe k dispozici pro spuštění.

Dále je pro vaši informaci uveden Hello dokončení skriptu

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect tooAzure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify hello parameters toobe used by hello script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read hello VM GUID from hello context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke hello necessary pipeline commands tooadd an Azure Endpoint tooa specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-hello-script-toohello-recovery-plan"></a>Přidat plán obnovení toohello hello skriptu
Až hello skriptu je připraven, můžete ho přidat toohello plánu obnovení, který jste vytvořili dříve.

1. V plánu obnovení hello, kterou jste vytvořili zvolte tooadd skriptu po skupiny 2. ![](media/site-recovery-runbook-automation/15.png)
2. Zadejte název skriptu. Toto je právě popisný název pro tento skript pro zobrazení v rámci plánu obnovení hello.
3. Ve skriptu tooAzure převzetí služeb při selhání hello – vyberte název hello účet Azure Automation.
4. V hello Runbooky Azure vyberte sadu runbook hello, vámi vytvořený.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Primární straně skripty
Když jsou prováděny tooAzure převzetí služeb při selhání, můžete také tooexecute primární straně skripty. Tyto skripty se spustí na serveru VMM hello během převzetí služeb při selhání.
Primární straně skriptů jsou k dispozici pouze pro před vypnutím pouze a post vypnutí fázích. Toto je očekávané hello primární lokality toobe obvykle není k dispozici při havárii narazilo.
Pouze v případě, že můžete vyjádřit výslovný souhlas pro operace primární lokality, je během neplánované převzetí služeb při selhání, se pokusí toorun hello primární straně skripty. Pokud nejsou dosažitelné nebo vypršel časový limit, bude pokračovat hello převzetí služeb při selhání toorecover hello virtuálních počítačů.
Primární straně skriptů jsou zrušení k dispozici pro servery VMware nebo fyzických/Hyper-v bez VMM chráněné tooAzure - při převzetí služeb při selhání tooAzure.
Ale když můžete navrácení služeb po obnovení z Azure tooon místní, primární straně skripty (sady Runbook) lze pro všechny cíle kromě VMware.

## <a name="test-hello-recovery-plan"></a>Testovací plán obnovení hello
Po přidání hello runbook toohello plán můžete spustit testovací převzetí služeb a pozorování v akci. Vždycky je doporučeno toorun testovací převzetí služeb při selhání tootest vaší aplikace a hello tooensure plán obnovení, nejsou žádné chyby.

1. Vyberte plán obnovení hello a spustit testovací převzetí služeb.
2. Během provádění hello plán můžete zjistit, zda provedlo hello runbook nebo nikoli prostřednictvím jeho stav.

   ![](media/site-recovery-runbook-automation/17.png)
3. Můžete také zjistit hello podrobný stav spuštění sady runbook na stránce úloh Azure Automation hello hello sady runbook.

   ![](media/site-recovery-runbook-automation/18.png)
4. Po dokončení převzetí služeb při selhání hello kromě hello výsledku spuštění runbooku, uvidíte, zda zpracování hello se úspěšně nebo ne návštěvou stránku hello virtuální počítač Azure a prohlížení hello koncové body.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Ukázkové skripty
Když jsme projít automatizaci jeden běžně používat úkolu přidávání tooan koncový bod virtuálního počítače Azure v tomto kurzu, můžete to udělat několika další výkonné automatizované úlohy pomocí služby Azure automation. Společnost Microsoft a hello Azure Automation komunity poskytují ukázkové sady runbook, které vám pomůžou začít vytvářet vlastní řešení a sady runbook nástroj, který můžete použít jako stavební bloky pro větší úlohy automatizace. Začít používat je z Galerie hello a vytvářet plány obnovení výkonné jedním kliknutím pro vaše aplikace pomocí Azure Site Recovery.

## <a name="additional-resources"></a>Další zdroje
[Přehled služby Azure Automation](http://msdn.microsoft.com/library/azure/dn643629.aspx "Přehled služby Azure Automation")

[Ukázkové skripty pro automatizaci Azure](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "ukázkové skripty služby Azure Automation")
