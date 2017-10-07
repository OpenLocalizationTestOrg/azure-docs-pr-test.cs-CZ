---
title: "aaaAdd Azure Automation. runbooky toorecovery plány v Azure Site Recovery | Microsoft Docs"
description: "Zjistěte, jak Azure Site Recovery můžete rozšířit plánů obnovení pomocí Azure Automation. Zjistěte, jak toocomplete komplexní úlohy během obnovení tooAzure."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: ecece14d-5f92-4596-bbaf-5204addb95c2
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/23/2017
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 90d517200cec5527e98a0d00da466bace587b70b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-azure-automation-runbooks-toorecovery-plans"></a>Přidat plány toorecovery sady runbook automatizace Azure.
V tomto článku jsme popisují, jak Azure Site Recovery integruje se službou Azure Automation toohelp rozšíříte plánu obnovení. Plány obnovení můžete orchestraci obnovení virtuálních počítačů, které jsou chráněné službou Site Recovery. Jak pro replikaci tooa sekundární cloud a pro replikace tooAzure pracovat plány obnovení. Plány obnovení také pomoci zajistit, aby hello obnovení **přesné**, **repeatable**, a **automatizované**. Pokud budete selhání tooAzure vaše virtuální počítače, integraci s Azure Automation rozšiřuje plánu obnovení. Můžete ho tooexecute sady runbook, které nabízí výkonné automatizace úloh.

Pokud jste nový tooAzure automatizace, můžete [zaregistrovat](https://azure.microsoft.com/services/automation/) a [stažení ukázkové skripty](https://azure.microsoft.com/documentation/scripts/). Další informace a toolearn jak tooorchestrate tooAzure obnovení pomocí [plány obnovení](https://azure.microsoft.com/blog/?p=166264), najdete v části [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).

V tomto článku jsme popisují, jak můžete integrovat Azure Automation runbook do plánu obnovení. Používáme tooautomate příklady základní se úlohy, které dříve vyžadovaly ruční zásah. Můžeme také popisují, jak tooconvert tooa několika kroky obnovení jedním kliknutím akce obnovení.

## <a name="customize-hello-recovery-plan"></a>Přizpůsobit plán obnovení hello
1. Přejděte toohello **Site Recovery** okna prostředků plánu obnovení. V tomto příkladu hello plán obnovení má dva virtuální počítače přidané tooit, pro obnovení. toobegin přidání sady runbook, klikněte na tlačítko hello **přizpůsobit** kartě.

    ![Klikněte na tlačítko Přizpůsobit hello](media/site-recovery-runbook-automation-new/essentials-rp.png)


2. Klikněte pravým tlačítkem na **1. skupina: spuštění**a potom vyberte **akce po přidání**.

    ![Klikněte pravým tlačítkem na skupinu 1: Spuštění a přidejte po akce](media/site-recovery-runbook-automation-new/customize-rp.png)

3. Klikněte na tlačítko **vyberte skript**.

4. Na hello **akce aktualizace** okno, název hello skriptu **Hello, World**.

    ![okno Akce aktualizace Hello](media/site-recovery-runbook-automation-new/update-rp.png)

5. Zadejte název účtu Automation.
    >[!NOTE]
    > Hello účet Automation může být v libovolné oblasti Azure. Hello účet Automation musí být v hello stejnému předplatnému jako hello trezoru Azure Site Recovery.

6. V účtu Automation vyberte sadu runbook. Tato sada runbook je hello skript, který běží během provádění hello hello plán obnovení po obnovení hello hello první skupiny.

7. toosave hello skriptu, klikněte na tlačítko **OK**. Hello skript se přidá příliš**1. skupina: po kroky**.

    ![1:Start skupiny následné akce](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="considerations-for-adding-a-script"></a>Důležité informace týkající se přidání skriptu

* Možnosti příliš**odstranit krok** nebo **aktualizovat hello skriptu**, klikněte pravým tlačítkem na skript hello.
* Skript můžete spustit v Azure při převzetí služeb při selhání z tooAzure místní počítač. Ho také můžete spustit v Azure jako skript primární lokalitu před vypnutí, během navrácení služeb po obnovení z Azure tooan na místním počítači.
* Při spuštění skriptu, vloží kontextu plánu obnovení. Hello následující příklad ukazuje kontextové proměnné:

    ```
            {"RecoveryPlanName":"hrweb-recovery",

            "FailoverType":"Test",

            "FailoverDirection":"PrimaryToSecondary",

            "GroupId":"1",

            "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                    { "SubscriptionId":"7a1111111-c1d6-49c5-8c5d-111ce8dd183",

                    "ResourceGroupName":"ContosoRG",

                    "CloudServiceName":"pod02hrweb-Chicago-test",

                    "RoleName":"Fabrikam-Hrweb-frontend-test",

                    "RecoveryPointId":"TimeStamp"}

                    }

            }
    ```

    Hello následující tabulka uvádí hello název a popis pro každou proměnnou v kontextu hello.

    | **Název proměnné** | **Popis** |
    | --- | --- |
    | RecoveryPlanName |Hello název plánu hello spuštěn. Tato proměnná umožňuje provádět různé akce na základě názvu plánu obnovení hello. Také můžete znovu použít skript hello. |
    | FailoverType |Určuje, zda text hello převzetí služeb při selhání testu, plánované, nebo neplánované. |
    | FailoverDirection |Určuje, zda obnovení tooa primární nebo sekundární lokality. |
    | GroupID |Určuje číslo skupiny hello v plánu obnovení hello po spuštění plánu hello. |
    | VmMap |Pole všechny virtuální počítače ve skupině hello. |
    | Klíč VMMap |Jedinečný klíč (GUID) pro každý virtuální počítač. Jeho hello stejný jako hello Azure Virtual Machine Manager (VMM) ID hello virtuální počítač, kde je to možné. |
    | SubscriptionId |ID předplatného Azure Hello, ve kterém hello virtuálních počítačů vytvořila. |
    | RoleName |Název Hello hello virtuálního počítače Azure, který se obnovuje. |
    | CloudServiceName |název služby Azure cloud Hello, pod kterým hello virtuálních počítačů vytvořila. |
    | Název skupiny prostředků|Název skupiny prostředků Azure Hello, pod kterým hello virtuálních počítačů vytvořila. |
    | RecoveryPointId|Hello časové razítko pro při obnovení hello virtuálních počítačů. |

* Zajistěte, aby byl tento účet Automation hello hello následující moduly:
    * AzureRM.profile
    * AzureRM.Resources
    * AzureRM.Automation
    * AzureRM.Network
    * AzureRM.Compute

Všechny moduly musí být kompatibilní verze. Tooensure snadný způsob, jestli jsou všechny moduly kompatibilní je toouse hello nejnovější verze všechny moduly hello.

### <a name="access-all-vms-of-hello-vmmap-in-a-loop"></a>Přístup k všechny virtuální počítače hello VMMap ve smyčce
Použijte následující kód tooloop napříč všechny virtuální počítače hello Microsoft VMMap hello:

```
$VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
$vmMap = $RecoveryPlanContext.VmMap
 foreach($VMID in $VMinfo)
 {
     $VM = $vmMap.$VMID                
             if( !(($VM -eq $Null) -Or ($VM.ResourceGroupName -eq $Null) -Or ($VM.RoleName -eq $Null))) {
         #this check is tooensure that we skip when some data is not available else it will fail
 Write-output "Resource group name ", $VM.ResourceGroupName
 Write-output "Rolename " = $VM.RoleName
     }
 }

```

> [!NOTE]
> Hello prostředků skupiny název a role název hodnoty jsou prázdné, pokud je skript hello spouštěcí skupině tooa před akcí. Hello hodnoty jsou naplněny pouze v případě, že v převzetí služeb při selhání se podaří hello virtuálních počítačů této skupiny. skript Hello je po akce hello spouštěcí skupině.

## <a name="use-hello-same-automation-runbook-in-multiple-recovery-plans"></a>Použití hello stejné sady Automation runbook v více plánů obnovení

Můžete pomocí jednoho skriptu v více plánů obnovení pomocí externích proměnných. Můžete použít [proměnné automatizace Azure](../automation/automation-variables.md) toostore parametry, které můžete předat pro obnovení, naplánujte spuštění. Přidáním název plánu obnovení hello jako předpona toohello proměnné, můžete vytvořit jednotlivé proměnných pro každý plán obnovení. Pak použijte proměnné hello jako parametry. Můžete změnit parametr beze změny hello skript, ale stále změnu hello způsobem hello skriptu funguje.

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a>Použití jednoduché řetězec proměnné ve skriptu runbook

V tomto příkladu skript přijímá hello vstup z skupina zabezpečení sítě (NSG) a použije ho toohello virtuální počítače v plánu obnovení.

Pro skript toodetect hello plánu obnovení je spuštěna použijte kontextu plán obnovení hello:

```
workflow AddPublicIPAndNSG {
    param (
          [parameter(Mandatory=$false)]
          [Object]$RecoveryPlanContext
    )

    $RPName = $RecoveryPlanContext.RecoveryPlanName
```

tooapply existující skupina NSG, musíte znát název hello NSG a název skupiny prostředků hello NSG. Tyto proměnné můžete použijte jako vstupy pro skripty plánu obnovení. toodo, vytvořte dvě proměnné v hello prostředky účet Automation. Přidejte název hello hello plán obnovení, který vytváříte hello parametry pro jako název proměnné toohello předponu.

1. Vytvořte název proměnné toostore hello NSG. Přidejte název proměnné toohello předponu pomocí názvu hello hello plánu obnovení.

    ![Vytvořte proměnnou Název skupiny NSG](media/site-recovery-runbook-automation-new/var1.png)

2. Vytvořte název skupiny prostředků NSG proměnné toostore hello. Přidejte název proměnné toohello předponu pomocí názvu hello hello plánu obnovení.

    ![Vytvoření názvu skupiny prostředků NSG](media/site-recovery-runbook-automation-new/var2.png)


3.  Ve skriptu hello použijte následující hodnoty proměnné tooget hello kódu pro odkaz hello:

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  Použití proměnné hello v hello runbook tooapply hello NSG toohello síťové rozhraní služeb hello při selhání virtuálního počítače:

    ```
    InlineScript {
    if (($Using:NSGname -ne $Null) -And ($Using:NSGRGname -ne $Null)) {
            $NSG = Get-AzureRmNetworkSecurityGroup -Name $Using:NSGname -ResourceGroupName $Using:NSGRGname
            Write-output $NSG.Id
            #Apply hello NSG tooa network interface
            #$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
            #Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            #  -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $NSG
        }
    }
    ```

Pro každý plán obnovení vytvořte nezávislých proměnných, takže můžete opakovaně použít skript hello. Přidání předpony pomocí hello název plánu obnovení. Pro dokončení, začátku do konce skript pro tento scénář, najdete v části [během testovacího převzetí služeb při selhání plánu obnovení Site Recovery přidat veřejné IP adresy a NSG tooVMs](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).


### <a name="use-a-complex-variable-toostore-more-information"></a>Použití proměnné komplexní toostore Další informace

Vezměte v úvahu scénář, ve kterém chcete tooturn jednoho skriptu na veřejnou IP adresu pro konkrétní virtuální počítače. V jiné scénáři můžete chtít tooapply odlišné skupiny Nsg na různé virtuální počítače (ne na všech virtuálních počítačích). Můžete použít skript, který je opakovaně použitelné pro každého plánu obnovení. Proměnný počet virtuálních počítačů může mít každý plán obnovení. Například obnovení služby SharePoint má dva elementy front end. Základní-obchodní (LOB) aplikace má pouze jeden front-endu. Nelze vytvořit samostatné proměnných pro každý plán obnovení. 

V následující ukázka hello, jsme nové způsobem a vytvoříte [komplexní proměnná](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) v prostředky účet Azure Automation hello. To uděláte tak, že zadáte více hodnot. Je nutné použít hello toocomplete prostředí Azure PowerShell následující kroky:

1. V prostředí PowerShell Přihlaste se tooyour předplatné Azure:

    ```
    login-azurermaccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. toostore hello parametry, jak vytvořit proměnnou komplexní hello pomocí názvu hello hello plánu obnovení:

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. V této proměnné komplexní **VMDetails** je hello ID virtuálního počítače pro hello chráněných virtuálních počítačů. tooget hello ID virtuálního počítače v hello portál Azure, zobrazit vlastnosti virtuálního počítače hello. Hello následující snímek obrazovky ukazuje proměnné, která ukládá hello podrobnosti o dva virtuální počítače:

    ![Použít hello ID virtuálního počítače jako hello GUID](media/site-recovery-runbook-automation-new/vmguid.png)

4. Pomocí této proměnné v runbooku. Pokud hello označil že identifikátor GUID virtuálního počítače je nalezeno v kontextu plán obnovení hello, použít hello NSG na hello virtuálních počítačů:

    ```
    $VMDetailsObj = Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName
    ```

4. V sadě runbook projděte virtuální počítače hello kontextu plán obnovení hello. Zkontrolujte, zda text hello virtuálních počítačů existuje v **$VMDetailsObj**. Pokud existuje, přístup k vlastnostem hello hello proměnné tooapply hello NSG:

    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            Write-output $VMDetailsObj.value.$VMID

            if ($VMDetailsObj.value.$VMID -ne $Null) { #If hello VM exists in hello context, this will not b Null
                $VM = $vmMap.$VMID
                # Access hello properties of hello variable
                $NSGname = $VMDetailsObj.value.$VMID.'NSGName'
                $NSGRGname = $VMDetailsObj.value.$VMID.'NSGResourceGroupName'

                # Add code tooapply hello NSG properties toohello VM
            }
        }
    ```

Hello stejný skriptu můžete použít pro jinou obnovení plány. Zadejte jiné parametry uložením hello hodnotu, která odpovídá plánu obnovení tooa v různých proměnných.

## <a name="sample-scripts"></a>Ukázkové skripty

toodeploy ukázkové skripty tooyour účet Automation, klikněte na tlačítko hello **nasazení tooAzure** tlačítko.

[![Nasazení tooAzure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

Jiný příklad najdete v tématu hello následující videa. Ukazuje, jak toorecover tooAzure aplikace WordPress dvouvrstvá:


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="additional-resources"></a>Další zdroje
* [Azure Automation spustit jako účet služby](../automation/automation-sec-configure-azure-runas-account.md)
* [Přehled Azure Automation](http://msdn.microsoft.com/library/azure/dn643629.aspx "přehled Azure Automation.")
* [Azure Automation ukázkové skripty](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "ukázkové skripty Azure Automation.")
