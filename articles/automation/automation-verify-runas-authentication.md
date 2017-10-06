---
title: "Konfigurace účtu Azure Automation aaaValidate | Microsoft Docs"
description: "Tento článek popisuje, jak tooconfirm hello účtu Automation je správně nastavená konfigurace."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: magoedte
ms.openlocfilehash: 3a990dcc6661cf67c4b62592ce03d55a3791053a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="test-azure-automation-run-as-account-authentication"></a>Test ověření účtu Azure Automation Spustit jako
Po úspěšném vytvoření účtu Automation se můžete provádět jednoduchá testovací tooconfirm, můžete ověřit toosuccessfully v Azure Resource Manager nebo nasazení Azure classic pomocí nově vytvořené nebo aktualizované účtu Automation spustit jako.    

## <a name="automation-run-as-authentication"></a>Ověření pomocí účtu Automation Spustit jako
Použít hello ukázkový kód níže příliš[vytvoření sady runbook PowerShell](automation-creating-importing-runbook.md) tooverify ověřování pomocí hello spuštěn pod účtem a také ve vaší vlastní runbooky tooauthenticate a spravovat prostředky Resource Manager pomocí účtu Automation.   

    $connectionName = "AzureRunAsConnection"
    try
    {
        # Get hello connection "AzureRunAsConnection "
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

        "Logging in tooAzure..."
        Add-AzureRmAccount `
           -ServicePrincipal `
           -TenantId $servicePrincipalConnection.TenantId `
           -ApplicationId $servicePrincipalConnection.ApplicationId `
           -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    }
    catch {
       if (!$servicePrincipalConnection)
       {
          $ErrorMessage = "Connection $connectionName not found."
          throw $ErrorMessage
      } else{
          Write-Error -Message $_.Exception
          throw $_.Exception
      }
    }

    #Get all ARM resources from all resource groups
    $ResourceGroups = Get-AzureRmResourceGroup 

    foreach ($ResourceGroup in $ResourceGroups)
    {    
       Write-Output ("Showing resources in resource group " + $ResourceGroup.ResourceGroupName)
       $Resources = Find-AzureRmResource -ResourceGroupNameContains $ResourceGroup.ResourceGroupName | Select ResourceName, ResourceType
       ForEach ($Resource in $Resources)
       {
          Write-Output ($Resource.ResourceName + " of type " +  $Resource.ResourceType)
       }
       Write-Output ("")
    } 

Všimněte si hello rutina, kterou používá pro ověřování v sadě runbook hello - **Add-AzureRmAccount**, používá hello *ServicePrincipalCertificate* sadu parametrů.  Ověřování provádí pomocí certifikátu objektu služby a ne pomocí přihlašovacích údajů.  

Když můžete [spuštění sady runbook hello](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate účtu spustit jako [úlohy runbooku](automation-runbook-execution.md) je vytvořen, zobrazí se okno hello úlohy a stav úlohy hello v hello zobrazí **Souhrn úlohy**dlaždici. Počáteční stav úlohy Hello bude *zařazeno ve frontě* označující, že se čeká na pracovního procesu runbooku v toobecome cloudu hello k dispozici. Pak ji přesune příliš*počáteční* když pracovní proces hello úlohy a potom *systémem* při hello runbook skutečně spustí.  Po dokončení úlohy runbooku hello bychom měli vidět stav **dokončeno**.

toosee hello podrobné výsledky hello sady runbook, klikněte na hello **výstup** dlaždici.  Na hello **výstup** okno, měli byste vidět ho byl úspěšně ověřen a vrátí seznam všech prostředků ve všech skupinách prostředků ve vašem předplatném.  

Jenom nezapomeňte tooremove hello blok kódu, počínaje hello komentář `#Get all ARM resources from all resource groups` při opakovaně hello kód pro své sady runbook.

## <a name="classic-run-as-authentication"></a>Ověření pomocí účtu Spustit jako pro Azure Classic
Použít hello ukázkový kód níže příliš[vytvoření sady runbook PowerShell](automation-creating-importing-runbook.md) tooverify ověřování pomocí hello Classic spuštěn pod účtem a také ve vaší vlastní runbooky tooauthenticate a spravovat prostředky v modelu nasazení classic hello.  

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get hello connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate tooAzure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in hello Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting hello certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in hello Automation account."
    }

    Write-Verbose "Authenticating tooAzure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID
    
    #Get all VMs in hello subscription and return list with name of each
    Get-AzureVM | ft Name

Když můžete [spuštění sady runbook hello](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate účtu spustit jako [úlohy runbooku](automation-runbook-execution.md) je vytvořen, zobrazí se okno hello úlohy a stav úlohy hello v hello zobrazí **Souhrn úlohy**dlaždici. Počáteční stav úlohy Hello bude *zařazeno ve frontě* označující, že se čeká na pracovního procesu runbooku v toobecome cloudu hello k dispozici. Pak ji přesune příliš*počáteční* když pracovní proces hello úlohy a potom *systémem* při hello runbook skutečně spustí.  Po dokončení úlohy runbooku hello bychom měli vidět stav **dokončeno**.

toosee hello podrobné výsledky hello sady runbook, klikněte na hello **výstup** dlaždici.  Na hello **výstup** okno, měli byste vidět ho byl úspěšně ověřen a vrátí seznam všech virtuálních počítačů Azure pomocí VMName, která jsou nasazena v rámci vašeho předplatného.  

Jenom nezapomeňte rutiny hello tooremove **Get-AzureVM** při opakovaně hello kód pro své sady runbook.

## <a name="next-steps"></a>Další kroky
* tooget kroky s runbooky prostředí PowerShell najdete v části [Můj první Powershellový runbook](automation-first-runbook-textual-powershell.md).
* toolearn Další informace o vytváření grafického obsahu, najdete v části [vytváření grafického obsahu ve službě Azure Automation](automation-graphical-authoring-intro.md).
