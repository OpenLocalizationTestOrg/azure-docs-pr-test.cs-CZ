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
# <a name="test-azure-automation-run-as-account-authentication"></a><span data-ttu-id="81009-103">Test ověření účtu Azure Automation Spustit jako</span><span class="sxs-lookup"><span data-stu-id="81009-103">Test Azure Automation Run As account authentication</span></span>
<span data-ttu-id="81009-104">Po úspěšném vytvoření účtu Automation se můžete provádět jednoduchá testovací tooconfirm, můžete ověřit toosuccessfully v Azure Resource Manager nebo nasazení Azure classic pomocí nově vytvořené nebo aktualizované účtu Automation spustit jako.</span><span class="sxs-lookup"><span data-stu-id="81009-104">After an Automation account is successfully created, you can perform a simple test tooconfirm you are able toosuccessfully authenticate in Azure Resource Manager or Azure classic deployment using your newly created or updated Automation Run As account.</span></span>    

## <a name="automation-run-as-authentication"></a><span data-ttu-id="81009-105">Ověření pomocí účtu Automation Spustit jako</span><span class="sxs-lookup"><span data-stu-id="81009-105">Automation Run As authentication</span></span>
<span data-ttu-id="81009-106">Použít hello ukázkový kód níže příliš[vytvoření sady runbook PowerShell](automation-creating-importing-runbook.md) tooverify ověřování pomocí hello spuštěn pod účtem a také ve vaší vlastní runbooky tooauthenticate a spravovat prostředky Resource Manager pomocí účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="81009-106">Use hello sample code below too[create a PowerShell runbook](automation-creating-importing-runbook.md) tooverify authentication using hello Run As account and also in your custom runbooks tooauthenticate and manage Resource Manager resources with your Automation account.</span></span>   

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

<span data-ttu-id="81009-107">Všimněte si hello rutina, kterou používá pro ověřování v sadě runbook hello - **Add-AzureRmAccount**, používá hello *ServicePrincipalCertificate* sadu parametrů.</span><span class="sxs-lookup"><span data-stu-id="81009-107">Notice hello cmdlet used for authenticating in hello runbook - **Add-AzureRmAccount**, uses hello *ServicePrincipalCertificate* parameter set.</span></span>  <span data-ttu-id="81009-108">Ověřování provádí pomocí certifikátu objektu služby a ne pomocí přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="81009-108">It authenticates by using service principal certificate, not credentials.</span></span>  

<span data-ttu-id="81009-109">Když můžete [spuštění sady runbook hello](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate účtu spustit jako [úlohy runbooku](automation-runbook-execution.md) je vytvořen, zobrazí se okno hello úlohy a stav úlohy hello v hello zobrazí **Souhrn úlohy**dlaždici.</span><span class="sxs-lookup"><span data-stu-id="81009-109">When you [run hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate your Run As account, a [runbook job](automation-runbook-execution.md) is created, hello Job blade is displayed, and hello job status displayed in hello **Job Summary** tile.</span></span> <span data-ttu-id="81009-110">Počáteční stav úlohy Hello bude *zařazeno ve frontě* označující, že se čeká na pracovního procesu runbooku v toobecome cloudu hello k dispozici.</span><span class="sxs-lookup"><span data-stu-id="81009-110">hello job status will start as *Queued* indicating that it is waiting for a runbook worker in hello cloud toobecome available.</span></span> <span data-ttu-id="81009-111">Pak ji přesune příliš*počáteční* když pracovní proces hello úlohy a potom *systémem* při hello runbook skutečně spustí.</span><span class="sxs-lookup"><span data-stu-id="81009-111">It will then move too*Starting* when a worker claims hello job, and then *Running* when hello runbook actually starts running.</span></span>  <span data-ttu-id="81009-112">Po dokončení úlohy runbooku hello bychom měli vidět stav **dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="81009-112">When hello runbook job completes, we should see a status of **Completed**.</span></span>

<span data-ttu-id="81009-113">toosee hello podrobné výsledky hello sady runbook, klikněte na hello **výstup** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="81009-113">toosee hello detailed results of hello runbook, click on hello **Output** tile.</span></span>  <span data-ttu-id="81009-114">Na hello **výstup** okno, měli byste vidět ho byl úspěšně ověřen a vrátí seznam všech prostředků ve všech skupinách prostředků ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="81009-114">On hello **Output** blade, you should see it has successfully authenticated and returns a list of all resources in all resource groups in your subscription.</span></span>  

<span data-ttu-id="81009-115">Jenom nezapomeňte tooremove hello blok kódu, počínaje hello komentář `#Get all ARM resources from all resource groups` při opakovaně hello kód pro své sady runbook.</span><span class="sxs-lookup"><span data-stu-id="81009-115">Just remember tooremove hello block of code starting with hello comment `#Get all ARM resources from all resource groups` when you reuse hello code for your runbooks.</span></span>

## <a name="classic-run-as-authentication"></a><span data-ttu-id="81009-116">Ověření pomocí účtu Spustit jako pro Azure Classic</span><span class="sxs-lookup"><span data-stu-id="81009-116">Classic Run As authentication</span></span>
<span data-ttu-id="81009-117">Použít hello ukázkový kód níže příliš[vytvoření sady runbook PowerShell](automation-creating-importing-runbook.md) tooverify ověřování pomocí hello Classic spuštěn pod účtem a také ve vaší vlastní runbooky tooauthenticate a spravovat prostředky v modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="81009-117">Use hello sample code below too[create a PowerShell runbook](automation-creating-importing-runbook.md) tooverify authentication using hello Classic Run As account and also in your custom runbooks tooauthenticate and manage resources in hello classic deployment model.</span></span>  

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

<span data-ttu-id="81009-118">Když můžete [spuštění sady runbook hello](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate účtu spustit jako [úlohy runbooku](automation-runbook-execution.md) je vytvořen, zobrazí se okno hello úlohy a stav úlohy hello v hello zobrazí **Souhrn úlohy**dlaždici.</span><span class="sxs-lookup"><span data-stu-id="81009-118">When you [run hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate your Run As account, a [runbook job](automation-runbook-execution.md) is created, hello Job blade is displayed, and hello job status displayed in hello **Job Summary** tile.</span></span> <span data-ttu-id="81009-119">Počáteční stav úlohy Hello bude *zařazeno ve frontě* označující, že se čeká na pracovního procesu runbooku v toobecome cloudu hello k dispozici.</span><span class="sxs-lookup"><span data-stu-id="81009-119">hello job status will start as *Queued* indicating that it is waiting for a runbook worker in hello cloud toobecome available.</span></span> <span data-ttu-id="81009-120">Pak ji přesune příliš*počáteční* když pracovní proces hello úlohy a potom *systémem* při hello runbook skutečně spustí.</span><span class="sxs-lookup"><span data-stu-id="81009-120">It will then move too*Starting* when a worker claims hello job, and then *Running* when hello runbook actually starts running.</span></span>  <span data-ttu-id="81009-121">Po dokončení úlohy runbooku hello bychom měli vidět stav **dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="81009-121">When hello runbook job completes, we should see a status of **Completed**.</span></span>

<span data-ttu-id="81009-122">toosee hello podrobné výsledky hello sady runbook, klikněte na hello **výstup** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="81009-122">toosee hello detailed results of hello runbook, click on hello **Output** tile.</span></span>  <span data-ttu-id="81009-123">Na hello **výstup** okno, měli byste vidět ho byl úspěšně ověřen a vrátí seznam všech virtuálních počítačů Azure pomocí VMName, která jsou nasazena v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="81009-123">On hello **Output** blade, you should see it has successfully authenticated and returns a list of all Azure VMs by VMName that are deployed in your subscription.</span></span>  

<span data-ttu-id="81009-124">Jenom nezapomeňte rutiny hello tooremove **Get-AzureVM** při opakovaně hello kód pro své sady runbook.</span><span class="sxs-lookup"><span data-stu-id="81009-124">Just remember tooremove hello cmdlet **Get-AzureVM** when you reuse hello code for your runbooks.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81009-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81009-125">Next steps</span></span>
* <span data-ttu-id="81009-126">tooget kroky s runbooky prostředí PowerShell najdete v části [Můj první Powershellový runbook](automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="81009-126">tooget started with PowerShell runbooks, see [My first PowerShell runbook](automation-first-runbook-textual-powershell.md).</span></span>
* <span data-ttu-id="81009-127">toolearn Další informace o vytváření grafického obsahu, najdete v části [vytváření grafického obsahu ve službě Azure Automation](automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="81009-127">toolearn more about Graphical Authoring, see [Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md).</span></span>
