---
title: "Vytvoření účtu Azure Automation Spustit jako pomocí PowerShellu | Dokumentace Microsoftu"
description: "Tento článek popisuje, jak upgradovat váš účet Automation pomocí PowerShellu, abyste vytvořili účty Spustit jako, pokud jste tento krok neprovedli během počátečního vytváření z portálu."
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
ms.openlocfilehash: a41a57ee3815a815c23e9bbe2dd0309e6c61c8f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="update-automation-run-as-account-using-powershell"></a><span data-ttu-id="7f617-103">Aktualizace účtu Automation Spustit jako pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="7f617-103">Update Automation Run As account using PowerShell</span></span>
<span data-ttu-id="7f617-104">Účet Automation můžete aktualizovat pomocí PowerShellu, pokud jste postupovali takto:</span><span class="sxs-lookup"><span data-stu-id="7f617-104">You can use PowerShell to update your existing Automation account if:</span></span>

* <span data-ttu-id="7f617-105">Vytvořili jste účet Automation, ale odmítli jste vytvoření účtu Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="7f617-105">You create an Automation account but decline to create the Run As account.</span></span>
* <span data-ttu-id="7f617-106">Už máte účet Automation pro správu prostředků Resource Manageru a chcete ho aktualizovat, aby zahrnoval účet Spustit jako pro ověřování runbooků.</span><span class="sxs-lookup"><span data-stu-id="7f617-106">You already use an Automation account to manage Resource Manager resources and you want to update the account to include the Run As account for runbook authentication.</span></span>
* <span data-ttu-id="7f617-107">Už máte účet Automation pro správu klasických prostředků a chcete ho aktualizovat, abyste mohli použít účet Spustit jako pro Classic a nemuseli vytvářet nový účet a migrovat na něj runbooky a prostředky.</span><span class="sxs-lookup"><span data-stu-id="7f617-107">You already use an Automation account to manage classic resources and you want to update it to use the Classic Run As account instead of creating a new account and migrating your runbooks and assets to it.</span></span>   
* <span data-ttu-id="7f617-108">Rozhodli jste se vytvořit účet Spustit jako a účet Spustit jako pro Classic pomocí certifikátu, který vydala vaše podniková certifikační autorita.</span><span class="sxs-lookup"><span data-stu-id="7f617-108">You want to create a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f617-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7f617-109">Prerequisites</span></span>

* <span data-ttu-id="7f617-110">Tento skript je možné spustit jenom v systémech Windows 10 a Windows Server 2016 s nainstalovanými moduly Azure Resource Manageru verze 2.01 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7f617-110">The script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 2.01 and later.</span></span> <span data-ttu-id="7f617-111">V předchozích verzích Windows není podporován.</span><span class="sxs-lookup"><span data-stu-id="7f617-111">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="7f617-112">Azure PowerShell 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7f617-112">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="7f617-113">Informace o vydání PowerShellu 1.0 najdete v článku [Postup instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7f617-113">For information about the PowerShell 1.0 release, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="7f617-114">Účet Automation, na který se odkazuje jako na hodnotu parametrů *-AutomationAccountName* a *-ApplicationDisplayName* v následujícím skriptu PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="7f617-114">An Automation account, which is referenced as the value for the *–AutomationAccountName* and *-ApplicationDisplayName* parameters in the following PowerShell script.</span></span>

<span data-ttu-id="7f617-115">Abyste získali hodnoty pro parametry *SubscriptionID*, *ResourceGroup* a *AutomationAccountName*, které jsou pro skripty povinné, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="7f617-115">To get the values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for the scripts, do the following:</span></span>
1. <span data-ttu-id="7f617-116">Na webu Azure Portal v okně **Účet Automation** vyberte příslušný účet Automation a potom vyberte **Všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="7f617-116">In the Azure portal, select your Automation account on the **Automation account** blade, and then select **All settings**.</span></span> 
2. <span data-ttu-id="7f617-117">V okně **Všechna nastavení** v části **Nastavení účtu** vyberte **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="7f617-117">On the **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="7f617-118">Hodnoty v okně **Vlastnosti** si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="7f617-118">Note the values on the **Properties** blade.</span></span>

![Podokno vlastností účtu Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

## <a name="create-run-as-account-powershell-script"></a><span data-ttu-id="7f617-120">Vytvoření powershellového skriptu pro účet Spustit jako</span><span class="sxs-lookup"><span data-stu-id="7f617-120">Create Run As Account PowerShell script</span></span>
<span data-ttu-id="7f617-121">Tento skript PowerShellu zahrnuje podporu následujících konfigurací:</span><span class="sxs-lookup"><span data-stu-id="7f617-121">This PowerShell script includes support for the following configurations:</span></span>

* <span data-ttu-id="7f617-122">Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="7f617-122">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="7f617-123">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="7f617-123">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="7f617-124">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu</span><span class="sxs-lookup"><span data-stu-id="7f617-124">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="7f617-125">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem v cloudu Azure Government</span><span class="sxs-lookup"><span data-stu-id="7f617-125">Create a Run As account and a Classic Run As account by using a self-signed certificate in the Azure Government cloud.</span></span>

<span data-ttu-id="7f617-126">V závislosti na možnosti konfigurace, kterou vyberete, skript vytvoří následující položky.</span><span class="sxs-lookup"><span data-stu-id="7f617-126">Depending on the configuration option you select, the script creates the following items.</span></span>

<span data-ttu-id="7f617-127">**Pro účty Spustit jako:**</span><span class="sxs-lookup"><span data-stu-id="7f617-127">**For Run As accounts:**</span></span>

* <span data-ttu-id="7f617-128">Vytvoří aplikaci Azure AD, která se exportuje s veřejným klíčem certifikátu podepsaného svým držitelem nebo podnikového certifikátu, vytvoří účet instančního objektu pro tuto aplikaci v Azure AD a přiřadí roli přispěvatele pro tento účet v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="7f617-128">Creates an Azure AD application to be exported with either the self-signed or enterprise certificate public key, creates a service principal account for the application in Azure AD, and assigns the Contributor role for the account in your current subscription.</span></span> <span data-ttu-id="7f617-129">Toto nastavení můžete změnit na roli Vlastník nebo libovolnou jinou roli.</span><span class="sxs-lookup"><span data-stu-id="7f617-129">You can change this setting to Owner or any other role.</span></span> <span data-ttu-id="7f617-130">Další informace najdete v tématu [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="7f617-130">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="7f617-131">V příslušném účtu Automation vytvoří prostředek certifikátu Automation s názvem *AzureRunAsCertificate*.</span><span class="sxs-lookup"><span data-stu-id="7f617-131">Creates an Automation certificate asset named *AzureRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="7f617-132">Prostředek certifikátu obsahuje privátní klíč certifikátu, který používá aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7f617-132">The certificate asset holds the certificate private key that's used by the Azure AD application.</span></span>
* <span data-ttu-id="7f617-133">V příslušném účtu Automation vytvoří prostředek připojení Automation s názvem *AzureRunAsConnection*.</span><span class="sxs-lookup"><span data-stu-id="7f617-133">Creates an Automation connection asset named *AzureRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="7f617-134">Prostředek připojení obsahuje parametry applicationId, tenantId, subscriptionId a certificate thumbprint.</span><span class="sxs-lookup"><span data-stu-id="7f617-134">The connection asset holds the applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="7f617-135">**Pro účet Spustit jako pro Classic:**</span><span class="sxs-lookup"><span data-stu-id="7f617-135">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="7f617-136">V příslušném účtu Automation vytvoří prostředek certifikátu Automation s názvem *AzureClassicRunAsCertificate*.</span><span class="sxs-lookup"><span data-stu-id="7f617-136">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="7f617-137">Prostředek certifikátu obsahuje privátní klíč certifikátu, který používá certifikát pro správu.</span><span class="sxs-lookup"><span data-stu-id="7f617-137">The certificate asset holds the certificate private key used by the management certificate.</span></span>
* <span data-ttu-id="7f617-138">V příslušném účtu Automation vytvoří prostředek připojení Automation s názvem *AzureClassicRunAsConnection*.</span><span class="sxs-lookup"><span data-stu-id="7f617-138">Creates an Automation connection asset named *AzureClassicRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="7f617-139">Prostředek propojení obsahuje název a ID předplatného a název prostředku certifikátu.</span><span class="sxs-lookup"><span data-stu-id="7f617-139">The connection asset holds the subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="7f617-140">Pokud při vytváření účtu Spustit jako pro Classic vyberete libovolnou z těchto možností, po spuštění skriptu nahrajte veřejný certifikát (soubor s příponou .cer) do úložiště správy předplatného, ve kterém byl účet Automation vytvořený.</span><span class="sxs-lookup"><span data-stu-id="7f617-140">If you select either option for creating a Classic Run As account, after the script is executed, upload the public certificate (.cer file name extension) to the management store for the subscription that the Automation account was created in.</span></span>
> 

1. <span data-ttu-id="7f617-141">Uložte následující skript do počítače.</span><span class="sxs-lookup"><span data-stu-id="7f617-141">Save the following script on your computer.</span></span> <span data-ttu-id="7f617-142">V tomto příkladu ho uložte s názvem *New-RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="7f617-142">In this example, save it with the filename *New-RunAsAccount.ps1*.</span></span>

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

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
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
           Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
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

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate the ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
                    "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload the .cer format of #CERT#"

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

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. <span data-ttu-id="7f617-143">Na svém počítači na obrazovce **Start** spusťte **Windows PowerShell** se zvýšenými uživatelskými právy.</span><span class="sxs-lookup"><span data-stu-id="7f617-143">On your computer, start **Windows PowerShell** from the **Start** screen with elevated user rights.</span></span>
3. <span data-ttu-id="7f617-144">V prostředí příkazového řádku se zvýšenými oprávněními přejděte do složky, která obsahuje skript vytvořený v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="7f617-144">From the elevated command-line shell, go to the folder that contains the script you created in step 1.</span></span>  
4. <span data-ttu-id="7f617-145">Spusťte tento skript s využitím hodnot parametrů pro požadovanou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="7f617-145">Execute the script by using the parameter values for the configuration you require.</span></span>

    <span data-ttu-id="7f617-146">**Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem**</span><span class="sxs-lookup"><span data-stu-id="7f617-146">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="7f617-147">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem**</span><span class="sxs-lookup"><span data-stu-id="7f617-147">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="7f617-148">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu**</span><span class="sxs-lookup"><span data-stu-id="7f617-148">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="7f617-149">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem v cloudu Azure Government**</span><span class="sxs-lookup"><span data-stu-id="7f617-149">**Create a Run As account and a Classic Run As account by using a self-signed certificate in the Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="7f617-150">Po spuštění skriptu se zobrazí výzva k ověření pomocí Azure.</span><span class="sxs-lookup"><span data-stu-id="7f617-150">After the script has executed, you will be prompted to authenticate with Azure.</span></span> <span data-ttu-id="7f617-151">Přihlaste se pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.</span><span class="sxs-lookup"><span data-stu-id="7f617-151">Sign in with an account that is a member of the subscription administrators role and co-administrator of the subscription.</span></span>
    >
    >

<span data-ttu-id="7f617-152">Po úspěšném spuštění skriptu je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="7f617-152">After the script has executed successfully, note the following:</span></span>
* <span data-ttu-id="7f617-153">Pokud jste vytvořili účet Spustit jako pro Classic s využitím veřejného certifikátu podepsaného svým držitelem (soubor .cer), skript ho vytvoří a uloží ve složce dočasných souborů ve vašem počítači pod profilem uživatele *%USERPROFILE%\AppData\Local\Temp*, který používáte ke spuštění relace PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="7f617-153">If you created a Classic Run As account with a self-signed public certificate (.cer file), the script creates and saves it to the temporary files folder on your computer under the user profile *%USERPROFILE%\AppData\Local\Temp*, which you used to execute the PowerShell session.</span></span>
* <span data-ttu-id="7f617-154">Pokud jste vytvořili účet Spustit jako pro Classic s využitím podnikového veřejného certifikátu (soubor .cer), použijte tento certifikát.</span><span class="sxs-lookup"><span data-stu-id="7f617-154">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="7f617-155">Postupujte podle kroků pro [odeslání certifikátu rozhraní API pro správu na portál Azure Classic](../azure-api-management-certs.md) a potom použijte [ukázkový kód pro ověření pomocí prostředků nasazení Azure Classic](automation-verify-runas-authentication.md#classic-run-as-authentication) k ověření konfigurace přihlašovacích údajů pomocí prostředků nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="7f617-155">Follow the instructions for [uploading a management API certificate to the Azure classic portal](../azure-api-management-certs.md), and then validate the credential configuration with classic deployment resources by using the [sample code to authenticate with Azure Classic Deployment Resources](automation-verify-runas-authentication.md#classic-run-as-authentication).</span></span> 
* <span data-ttu-id="7f617-156">Pokud jste *nevytvořili* účet Spustit jako pro Classic, použijte k ověření pomocí prostředků Resource Manageru a ke kontrole konfigurace přihlašovacích údajů [ukázkový kód pro ověření s využitím prostředků správy služeb](automation-verify-runas-authentication.md#automation-run-as-authentication).</span><span class="sxs-lookup"><span data-stu-id="7f617-156">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate the credential configuration by using the [sample code for authenticating with Service Management resources](automation-verify-runas-authentication.md#automation-run-as-authentication).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7f617-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7f617-157">Next steps</span></span>
* <span data-ttu-id="7f617-158">Další informace o objektech služby najdete v článku [Objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="7f617-158">For more information about Service Principals, refer to [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span>
* <span data-ttu-id="7f617-159">Další informace o certifikátech a službách Azure najdete v článku [Přehled certifikátů pro Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="7f617-159">For more information about certificates and Azure services, refer to [Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span></span>
