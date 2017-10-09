---
title: "aaaCreate Azure Automation účet Spustit jako pomocí prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje, jak tooupgrade vašeho účtu Automation pomocí prostředí PowerShell toocreate hello spustit jako účty Pokud neprovedete tento krok během počátečního vytváření z portálu hello."
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
ms.date: 04/14/2017
ms.author: magoedte
ms.openlocfilehash: 1049601321d2bc1e5f9d982f622788f8e4e4d797
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-automation-run-as-account-using-powershell"></a><span data-ttu-id="fa79a-103">Aktualizace účtu Automation Spustit jako pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="fa79a-103">Update Automation Run As account using PowerShell</span></span>
<span data-ttu-id="fa79a-104">Tooupdate prostředí PowerShell můžete použít svůj existující účet Automation, pokud:</span><span class="sxs-lookup"><span data-stu-id="fa79a-104">You can use PowerShell tooupdate your existing Automation account if:</span></span>

* <span data-ttu-id="fa79a-105">Vytvoření účtu Automation, ale odmítnout toocreate hello účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="fa79a-105">You create an Automation account but decline toocreate hello Run As account.</span></span>
* <span data-ttu-id="fa79a-106">Už používáte prostředky Resource Manager toomanage účet Automation a chcete tooupdate hello tooinclude hello spustit jako účet pro ověřování sady runbook.</span><span class="sxs-lookup"><span data-stu-id="fa79a-106">You already use an Automation account toomanage Resource Manager resources and you want tooupdate hello account tooinclude hello Run As account for runbook authentication.</span></span>
* <span data-ttu-id="fa79a-107">Už používáte toomanage účet Automation pro klasické prostředky a chcete, aby tooupdate ho toouse hello Classic spustit jako účet, místo vytvoření nového účtu a migrace tooit vaše runbooky a prostředky.</span><span class="sxs-lookup"><span data-stu-id="fa79a-107">You already use an Automation account toomanage classic resources and you want tooupdate it toouse hello Classic Run As account instead of creating a new account and migrating your runbooks and assets tooit.</span></span>   
* <span data-ttu-id="fa79a-108">Chcete toocreate spustit jako a Classic spustit jako účet pomocí certifikát vydaný certifikační autoritou (CA) rozlehlé sítě.</span><span class="sxs-lookup"><span data-stu-id="fa79a-108">You want toocreate a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa79a-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fa79a-109">Prerequisites</span></span>

* <span data-ttu-id="fa79a-110">skript Hello lze spustit pouze na systémy Windows 10 a Windows Server 2016 s moduly Azure Resource Manager 2.01 a novější.</span><span class="sxs-lookup"><span data-stu-id="fa79a-110">hello script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 2.01 and later.</span></span> <span data-ttu-id="fa79a-111">V předchozích verzích Windows není podporován.</span><span class="sxs-lookup"><span data-stu-id="fa79a-111">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="fa79a-112">Azure PowerShell 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="fa79a-112">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="fa79a-113">Informace o verzi hello PowerShell 1.0 najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fa79a-113">For information about hello PowerShell 1.0 release, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="fa79a-114">Účet služby Automation, který je odkazováno jako hodnota hello hello *-AutomationAccountName* a *- ApplicationDisplayName* parametry v hello následující skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fa79a-114">An Automation account, which is referenced as hello value for hello *–AutomationAccountName* and *-ApplicationDisplayName* parameters in hello following PowerShell script.</span></span>

<span data-ttu-id="fa79a-115">tooget hello hodnoty pro *SubscriptionID*, *ResourceGroup*, a *AutomationAccountName*, které jsou požadované parametry pro hello skripty, hello následující:</span><span class="sxs-lookup"><span data-stu-id="fa79a-115">tooget hello values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for hello scripts, do hello following:</span></span>
1. <span data-ttu-id="fa79a-116">V hello portálu Azure, vyberte svůj účet Automation na hello **účet Automation** a pak vyberte **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="fa79a-116">In hello Azure portal, select your Automation account on hello **Automation account** blade, and then select **All settings**.</span></span> 
2. <span data-ttu-id="fa79a-117">Na hello **všechna nastavení** okno, v části **nastavení účtu**, vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="fa79a-117">On hello **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="fa79a-118">Všimněte si hodnot hello hello **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="fa79a-118">Note hello values on hello **Properties** blade.</span></span>

![okno "Vlastnosti" účet Automation Hello](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

## <a name="create-run-as-account-powershell-script"></a><span data-ttu-id="fa79a-120">Vytvoření powershellového skriptu pro účet Spustit jako</span><span class="sxs-lookup"><span data-stu-id="fa79a-120">Create Run As Account PowerShell script</span></span>
<span data-ttu-id="fa79a-121">Tento skript prostředí PowerShell zahrnuje podporu pro hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="fa79a-121">This PowerShell script includes support for hello following configurations:</span></span>

* <span data-ttu-id="fa79a-122">Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="fa79a-122">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="fa79a-123">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="fa79a-123">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="fa79a-124">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu</span><span class="sxs-lookup"><span data-stu-id="fa79a-124">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="fa79a-125">Vytvořte účet Spustit jako a Classic spustit jako účet pomocí certifikát podepsaný svým držitelem v hello cloudu Azure Government.</span><span class="sxs-lookup"><span data-stu-id="fa79a-125">Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud.</span></span>

<span data-ttu-id="fa79a-126">V závislosti na hello možnost konfigurace, kterou vyberete vytvoří skript hello hello následující položky.</span><span class="sxs-lookup"><span data-stu-id="fa79a-126">Depending on hello configuration option you select, hello script creates hello following items.</span></span>

<span data-ttu-id="fa79a-127">**Pro účty Spustit jako:**</span><span class="sxs-lookup"><span data-stu-id="fa79a-127">**For Run As accounts:**</span></span>

* <span data-ttu-id="fa79a-128">Vytvoří Azure AD application toobe exportovaný s buď hello podepsaného svým držitelem nebo enterprise veřejný klíč certifikátu, vytvoří hlavní účet služby pro aplikaci hello ve službě Azure AD a hello přiřadí role Přispěvatel pro účet hello ve vaší stávající předplatné.</span><span class="sxs-lookup"><span data-stu-id="fa79a-128">Creates an Azure AD application toobe exported with either hello self-signed or enterprise certificate public key, creates a service principal account for hello application in Azure AD, and assigns hello Contributor role for hello account in your current subscription.</span></span> <span data-ttu-id="fa79a-129">Můžete změnit tato nastavení tooOwner nebo jakákoli jiná role.</span><span class="sxs-lookup"><span data-stu-id="fa79a-129">You can change this setting tooOwner or any other role.</span></span> <span data-ttu-id="fa79a-130">Další informace najdete v tématu [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="fa79a-130">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="fa79a-131">Vytvoří prostředek certifikátu automatizace s názvem *AzureRunAsCertificate* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="fa79a-131">Creates an Automation certificate asset named *AzureRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="fa79a-132">asset certifikátu Hello obsahuje hello privátní klíč certifikátu používaného aplikací hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fa79a-132">hello certificate asset holds hello certificate private key that's used by hello Azure AD application.</span></span>
* <span data-ttu-id="fa79a-133">Vytvoří prostředek připojení automatizace s názvem *AzureRunAsConnection* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="fa79a-133">Creates an Automation connection asset named *AzureRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="fa79a-134">asset připojení Hello obsahuje hello applicationId, tenantId, subscriptionId a kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="fa79a-134">hello connection asset holds hello applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="fa79a-135">**Pro účet Spustit jako pro Classic:**</span><span class="sxs-lookup"><span data-stu-id="fa79a-135">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="fa79a-136">Vytvoří prostředek certifikátu automatizace s názvem *AzureClassicRunAsCertificate* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="fa79a-136">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="fa79a-137">asset Hello certifikát obsahuje privátní klíč pro hello certifikátu používá certifikát pro správu hello.</span><span class="sxs-lookup"><span data-stu-id="fa79a-137">hello certificate asset holds hello certificate private key used by hello management certificate.</span></span>
* <span data-ttu-id="fa79a-138">Vytvoří prostředek připojení automatizace s názvem *AzureClassicRunAsConnection* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="fa79a-138">Creates an Automation connection asset named *AzureClassicRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="fa79a-139">asset připojení Hello obsahuje název odběru hello, subscriptionId a název certifikátu asset.</span><span class="sxs-lookup"><span data-stu-id="fa79a-139">hello connection asset holds hello subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="fa79a-140">Pokud vyberete jednu z možností pro vytvoření účtu Classic spustit jako, po hello skript se spustí, nahrávání hello veřejné správy toohello (přípona názvu souboru .cer) úložiště certifikátů pro předplatné hello této hello účet Automation byla vytvořena v.</span><span class="sxs-lookup"><span data-stu-id="fa79a-140">If you select either option for creating a Classic Run As account, after hello script is executed, upload hello public certificate (.cer file name extension) toohello management store for hello subscription that hello Automation account was created in.</span></span>
> 

1. <span data-ttu-id="fa79a-141">Uložte následující skript v počítači hello.</span><span class="sxs-lookup"><span data-stu-id="fa79a-141">Save hello following script on your computer.</span></span> <span data-ttu-id="fa79a-142">V tomto příkladu ho uložte pod názvem hello *New-RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="fa79a-142">In this example, save it with hello filename *New-RunAsAccount.ps1*.</span></span>

        #Requires -RunAsAdministrator
         Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,
                                      [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
           -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
           -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired)

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
        $KeyCredential.EndDate = $KeyCredential.EndDate.AddDays(-1)
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use key credentials and create an Azure AD application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds tooallow hello service principal application toobecome active (ordinarily takes a few seconds)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           Sleep -s 10
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
           return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
           Write-Error -Message "Please install hello latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
           return
        }

        Login-AzureRmAccount -EnvironmentName $EnvironmentName
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create a Run As account by using a service principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"

        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
          $CertificateName = $AutomationAccountName+$CertifcateAssetName
          $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
          $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
          $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
          CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create a service principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create hello Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate hello ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload hello .cer format of #CERT# toohello Management store by following hello steps below." + [Environment]::NewLine +
                    "Log in toohello Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload hello .cer format of #CERT#"

             if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
             $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
             $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
             $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        } else {
             $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
             $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
             $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
             $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
             $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
             CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create hello Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate hello ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. <span data-ttu-id="fa79a-143">V počítači, spusťte **prostředí Windows PowerShell** z hello **spustit** obrazovky se zvýšenými uživatelskými právy.</span><span class="sxs-lookup"><span data-stu-id="fa79a-143">On your computer, start **Windows PowerShell** from hello **Start** screen with elevated user rights.</span></span>
3. <span data-ttu-id="fa79a-144">Z hello se zvýšenými oprávněními prostředí příkazového řádku, přejděte toohello složku, která obsahuje hello skript, který jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="fa79a-144">From hello elevated command-line shell, go toohello folder that contains hello script you created in step 1.</span></span>  
4. <span data-ttu-id="fa79a-145">Spusťte skript hello pomocí hodnoty parametrů hello hello konfigurace, kterou požadujete.</span><span class="sxs-lookup"><span data-stu-id="fa79a-145">Execute hello script by using hello parameter values for hello configuration you require.</span></span>

    <span data-ttu-id="fa79a-146">**Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem**</span><span class="sxs-lookup"><span data-stu-id="fa79a-146">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="fa79a-147">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem**</span><span class="sxs-lookup"><span data-stu-id="fa79a-147">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="fa79a-148">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu**</span><span class="sxs-lookup"><span data-stu-id="fa79a-148">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="fa79a-149">**Vytvořit účet Spustit jako a Classic spustit jako účet pomocí certifikát podepsaný svým držitelem v hello cloudu Azure Government.**</span><span class="sxs-lookup"><span data-stu-id="fa79a-149">**Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="fa79a-150">Po provedení hello skript bude výzvami tooauthenticate s Azure.</span><span class="sxs-lookup"><span data-stu-id="fa79a-150">After hello script has executed, you will be prompted tooauthenticate with Azure.</span></span> <span data-ttu-id="fa79a-151">Přihlaste se pomocí účtu, který je členem role Správci předplatného hello a spolusprávce předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="fa79a-151">Sign in with an account that is a member of hello subscription administrators role and co-administrator of hello subscription.</span></span>
    >
    >

<span data-ttu-id="fa79a-152">Po hello skript byl úspěšně proveden, vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="fa79a-152">After hello script has executed successfully, note hello following:</span></span>
* <span data-ttu-id="fa79a-153">Pokud jste vytvořili účet Classic spustit jako s certifikát podepsaný svým držitelem veřejné (soubor .cer), skript hello vytvoří a uloží jej toohello dočasné soubory složky v počítači v rámci profilu uživatele hello *%USERPROFILE%\AppData\Local\Temp*, který používá relaci prostředí PowerShell tooexecute hello.</span><span class="sxs-lookup"><span data-stu-id="fa79a-153">If you created a Classic Run As account with a self-signed public certificate (.cer file), hello script creates and saves it toohello temporary files folder on your computer under hello user profile *%USERPROFILE%\AppData\Local\Temp*, which you used tooexecute hello PowerShell session.</span></span>
* <span data-ttu-id="fa79a-154">Pokud jste vytvořili účet Spustit jako pro Classic s využitím podnikového veřejného certifikátu (soubor .cer), použijte tento certifikát.</span><span class="sxs-lookup"><span data-stu-id="fa79a-154">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="fa79a-155">Postupujte podle pokynů hello pro [odesílání toohello certifikátu rozhraní API pro správu portálu Azure classic](../azure-api-management-certs.md)a potom ověřit konfiguraci hello přihlašovacích údajů s prostředky nasazení classic pomocí hello [ukázkový kód tooauthenticate s prostředky Azure Classic nasazení](automation-verify-runas-authentication.md#classic-run-as-authentication).</span><span class="sxs-lookup"><span data-stu-id="fa79a-155">Follow hello instructions for [uploading a management API certificate toohello Azure classic portal](../azure-api-management-certs.md), and then validate hello credential configuration with classic deployment resources by using hello [sample code tooauthenticate with Azure Classic Deployment Resources](automation-verify-runas-authentication.md#classic-run-as-authentication).</span></span> 
* <span data-ttu-id="fa79a-156">Pokud jste to udělali *není* vytvořit účet Classic spustit jako, ověření pomocí prostředků Resource Manageru a ověřit konfiguraci hello přihlašovacích údajů pomocí hello [ukázkový kód pro ověřování pomocí služby správy prostředky](automation-verify-runas-authentication.md#automation-run-as-authentication).</span><span class="sxs-lookup"><span data-stu-id="fa79a-156">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate hello credential configuration by using hello [sample code for authenticating with Service Management resources](automation-verify-runas-authentication.md#automation-run-as-authentication).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa79a-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fa79a-157">Next steps</span></span>
* <span data-ttu-id="fa79a-158">Další informace o objektech služby najdete v části příliš[objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="fa79a-158">For more information about Service Principals, refer too[Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span>
* <span data-ttu-id="fa79a-159">Další informace o certifikátech a službám Azure, najdete v části příliš[Přehled certifikátů pro Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="fa79a-159">For more information about certificates and Azure services, refer too[Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span></span>
