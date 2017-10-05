---
title: "Vytvoření identity pro aplikaci Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Popisuje, jak používat prostředí Azure PowerShell k vytvoření aplikace Azure Active Directory a objektu zabezpečení a jí udělit přístup k prostředkům prostřednictvím řízení přístupu na základě rolí. Ukazuje, jak k ověření aplikace pomocí hesla nebo certifikátu."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 55e83b0742652abbb42100a11a468bc13a7a8aed
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a><span data-ttu-id="ac092-104">Vytvoření instančního objektu pro přístup k prostředkům pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac092-104">Use Azure PowerShell to create a service principal to access resources</span></span>

<span data-ttu-id="ac092-105">Pokud máte aplikace nebo skriptu, která potřebuje přístup k prostředkům, můžete nastavit identity aplikace a ověřit aplikaci s svoje vlastní přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="ac092-105">When you have an app or script that needs to access resources, you can set up an identity for the app and authenticate the app with its own credentials.</span></span> <span data-ttu-id="ac092-106">Tato identita se označuje jako hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="ac092-106">This identity is known as a service principal.</span></span> <span data-ttu-id="ac092-107">Tento přístup umožňuje:</span><span class="sxs-lookup"><span data-stu-id="ac092-107">This approach enables you to:</span></span>

* <span data-ttu-id="ac092-108">Přiřadíte oprávnění k identitě aplikace, která se liší od vlastní oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ac092-108">Assign permissions to the app identity that are different than your own permissions.</span></span> <span data-ttu-id="ac092-109">Tato oprávnění jsou obvykle omezené na přesně co aplikaci je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="ac092-109">Typically, these permissions are restricted to exactly what the app needs to do.</span></span>
* <span data-ttu-id="ac092-110">Při provádění bezobslužného skriptu, použijte certifikát pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="ac092-110">Use a certificate for authentication when executing an unattended script.</span></span>

<span data-ttu-id="ac092-111">Toto téma ukazuje, jak používat [prostředí Azure PowerShell](/powershell/azure/overview) nastavit všechno, co potřebujete pro spuštění pod svou vlastní pověření a identity aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac092-111">This topic shows you how to use [Azure PowerShell](/powershell/azure/overview) to set up everything you need for an application to run under its own credentials and identity.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="ac092-112">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="ac092-112">Required permissions</span></span>
<span data-ttu-id="ac092-113">K dokončení tohoto tématu, musíte mít dostatečná oprávnění v Azure Active Directory a vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="ac092-113">To complete this topic, you must have sufficient permissions in both your Azure Active Directory and your Azure subscription.</span></span> <span data-ttu-id="ac092-114">Konkrétně musí být schopná vytvořit aplikaci ve službě Azure Active Directory a přiřazení objektu služby roli.</span><span class="sxs-lookup"><span data-stu-id="ac092-114">Specifically, you must be able to create an app in the Azure Active Directory, and assign the service principal to a role.</span></span> 

<span data-ttu-id="ac092-115">Nejjednodušším způsobem, jak zkontrolovat, jestli má váš účet dostatečná oprávnění, je použít k tomu portál.</span><span class="sxs-lookup"><span data-stu-id="ac092-115">The easiest way to check whether your account has adequate permissions is through the portal.</span></span> <span data-ttu-id="ac092-116">V tématu [zkontrolujte požadované oprávnění](resource-group-create-service-principal-portal.md#required-permissions).</span><span class="sxs-lookup"><span data-stu-id="ac092-116">See [Check required permission](resource-group-create-service-principal-portal.md#required-permissions).</span></span>

<span data-ttu-id="ac092-117">Nyní přejděte k části k ověřování:</span><span class="sxs-lookup"><span data-stu-id="ac092-117">Now, proceed to a section for authenticating with:</span></span>

* [<span data-ttu-id="ac092-118">heslo</span><span class="sxs-lookup"><span data-stu-id="ac092-118">password</span></span>](#create-service-principal-with-password)
* [<span data-ttu-id="ac092-119">certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="ac092-119">self-signed certificate</span></span>](#create-service-principal-with-self-signed-certificate)
* [<span data-ttu-id="ac092-120">certifikát od certifikační autority</span><span class="sxs-lookup"><span data-stu-id="ac092-120">certificate from Certificate Authority</span></span>](#create-service-principal-with-certificate-from-certificate-authority)

## <a name="powershell-commands"></a><span data-ttu-id="ac092-121">Příkazy prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac092-121">PowerShell commands</span></span>

<span data-ttu-id="ac092-122">Chcete-li nastavit hlavní název služby, použijte:</span><span class="sxs-lookup"><span data-stu-id="ac092-122">To set up a service principal, you use:</span></span>

| <span data-ttu-id="ac092-123">Příkaz</span><span class="sxs-lookup"><span data-stu-id="ac092-123">Command</span></span> | <span data-ttu-id="ac092-124">Popis</span><span class="sxs-lookup"><span data-stu-id="ac092-124">Description</span></span> |
| ------- | ----------- | 
| [<span data-ttu-id="ac092-125">Nové AzureRmADServicePrincipal</span><span class="sxs-lookup"><span data-stu-id="ac092-125">New-AzureRmADServicePrincipal</span></span>](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | <span data-ttu-id="ac092-126">Vytvoří objekt služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ac092-126">Creates an Azure Active Directory service principal</span></span> |
| [<span data-ttu-id="ac092-127">New-AzureRmRoleAssignment</span><span class="sxs-lookup"><span data-stu-id="ac092-127">New-AzureRmRoleAssignment</span></span>](/powershell/module/azurerm.resources/new-azurermroleassignment) | <span data-ttu-id="ac092-128">Zadaná role RBAC přiřadí objekt zadaný v zadaném oboru.</span><span class="sxs-lookup"><span data-stu-id="ac092-128">Assigns the specified RBAC role to the specified principal, at the specified scope.</span></span> |


## <a name="create-service-principal-with-password"></a><span data-ttu-id="ac092-129">Vytvoření instančního objektu s heslem</span><span class="sxs-lookup"><span data-stu-id="ac092-129">Create service principal with password</span></span>

<span data-ttu-id="ac092-130">Chcete-li vytvořit objekt služby k roli Přispěvatel pro vaše předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="ac092-130">To create a service principal with the Contributor role for your subscription, use:</span></span> 

```powershell
Login-AzureRmAccount
$sp = New-AzureRmADServicePrincipal -DisplayName exampleapp -Password "{provide-password}"
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

<span data-ttu-id="ac092-131">V příkladu v režimu spánku 20 sekund umožňující chvíli pro novou službu hlavní rozšíří v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ac092-131">The example sleeps for 20 seconds to allow some time for the new service principal to propagate throughout Azure Active Directory.</span></span> <span data-ttu-id="ac092-132">Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři."</span><span class="sxs-lookup"><span data-stu-id="ac092-132">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in the directory."</span></span>

<span data-ttu-id="ac092-133">Následující skript umožňuje určit obor než výchozí předplatné a opakuje přiřazení role, pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="ac092-133">The following script enables you to specify a scope other than the default subscription, and retries the role assignment if an error occurs:</span></span>

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $Password
)

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 
 # Create Service Principal for the AD app
 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

<span data-ttu-id="ac092-134">Několik položek si uvědomit o skriptu:</span><span class="sxs-lookup"><span data-stu-id="ac092-134">A few items to note about the script:</span></span>

* <span data-ttu-id="ac092-135">K udělení přístupu identity k výchozí předplatné, není potřeba zadat parametry ResourceGroup nebo ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="ac092-135">To grant the identity access to the default subscription, you do not need to provide either ResourceGroup or SubscriptionId parameters.</span></span>
* <span data-ttu-id="ac092-136">Zadejte parametr ResourceGroup pouze v případě, že chcete omezit rozsah přiřazení role do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="ac092-136">Specify the ResourceGroup parameter only when you want to limit the scope of the role assignment to a resource group.</span></span>
*  <span data-ttu-id="ac092-137">V tomto příkladu přidáte objektu služby roli Přispěvatel.</span><span class="sxs-lookup"><span data-stu-id="ac092-137">In this example, you add the service principal to the Contributor role.</span></span> <span data-ttu-id="ac092-138">Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="ac092-138">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="ac092-139">Skript v režimu spánku 15 sekund umožňující chvíli pro novou službu hlavní rozšíří v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ac092-139">The script sleeps for 15 seconds to allow some time for the new service principal to propagate throughout Azure Active Directory.</span></span> <span data-ttu-id="ac092-140">Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři."</span><span class="sxs-lookup"><span data-stu-id="ac092-140">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in the directory."</span></span>
* <span data-ttu-id="ac092-141">Pokud potřebujete udělit přístup k hlavní službě pro další skupiny prostředků nebo předplatných, spusťte `New-AzureRMRoleAssignment` rutinu znovu s různými obory.</span><span class="sxs-lookup"><span data-stu-id="ac092-141">If you need to grant the service principal access to more subscriptions or resource groups, run the `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>


### <a name="provide-credentials-through-powershell"></a><span data-ttu-id="ac092-142">Zadejte přihlašovací údaje pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac092-142">Provide credentials through PowerShell</span></span>
<span data-ttu-id="ac092-143">Nyní budete muset přihlásit jako aplikace k provádění operací.</span><span class="sxs-lookup"><span data-stu-id="ac092-143">Now, you need to log in as the application to perform operations.</span></span> <span data-ttu-id="ac092-144">Uživatelské jméno, použijte `ApplicationId` kterou jste vytvořili pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ac092-144">For the user name, use the `ApplicationId` that you created for the application.</span></span> <span data-ttu-id="ac092-145">Heslo použijte ten, který jste zadali při vytváření účtu.</span><span class="sxs-lookup"><span data-stu-id="ac092-145">For the password, use the one you specified when creating the account.</span></span> 

```powershell   
$creds = Get-Credential
Login-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId {tenant-id}
```

<span data-ttu-id="ac092-146">ID klienta nerozlišuje malá písmena, takže jej můžete vložit přímo ve vašem skriptu.</span><span class="sxs-lookup"><span data-stu-id="ac092-146">The tenant ID is not sensitive, so you can embed it directly in your script.</span></span> <span data-ttu-id="ac092-147">Pokud potřebujete zjistit ID klienta, použijte:</span><span class="sxs-lookup"><span data-stu-id="ac092-147">If you need to retrieve the tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

## <a name="create-service-principal-with-self-signed-certificate"></a><span data-ttu-id="ac092-148">Vytvoření instančního objektu se certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="ac092-148">Create service principal with self-signed certificate</span></span>

<span data-ttu-id="ac092-149">Chcete-li vytvořit objekt služby se certifikát podepsaný svým držitelem a role Přispěvatel pro vaše předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="ac092-149">To create a service principal with a self-signed certificate and the Contributor role for your subscription, use:</span></span> 

```powershell
Login-AzureRmAccount
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

<span data-ttu-id="ac092-150">V příkladu v režimu spánku 20 sekund umožňující chvíli pro novou službu hlavní rozšíří v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ac092-150">The example sleeps for 20 seconds to allow some time for the new service principal to propagate throughout Azure Active Directory.</span></span> <span data-ttu-id="ac092-151">Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři."</span><span class="sxs-lookup"><span data-stu-id="ac092-151">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in the directory."</span></span>

<span data-ttu-id="ac092-152">Následující skript umožňuje určit obor než výchozí předplatné a opakuje přiřazení role, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="ac092-152">The following script enables you to specify a scope other than the default subscription, and retries the role assignment if an error occurs.</span></span> <span data-ttu-id="ac092-153">Pokud nemáte Azure PowerShell 2.0 na Windows 10 nebo Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="ac092-153">You must have Azure PowerShell 2.0 on Windows 10 or Windows Server 2016.</span></span>

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
 $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

<span data-ttu-id="ac092-154">Několik položek si uvědomit o skriptu:</span><span class="sxs-lookup"><span data-stu-id="ac092-154">A few items to note about the script:</span></span>

* <span data-ttu-id="ac092-155">K udělení přístupu identity k výchozí předplatné, není potřeba zadat parametry ResourceGroup nebo ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="ac092-155">To grant the identity access to the default subscription, you do not need to provide either ResourceGroup or SubscriptionId parameters.</span></span>
* <span data-ttu-id="ac092-156">Zadejte parametr ResourceGroup pouze v případě, že chcete omezit rozsah přiřazení role do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="ac092-156">Specify the ResourceGroup parameter only when you want to limit the scope of the role assignment to a resource group.</span></span>
* <span data-ttu-id="ac092-157">V tomto příkladu přidáte objektu služby roli Přispěvatel.</span><span class="sxs-lookup"><span data-stu-id="ac092-157">In this example, you add the service principal to the Contributor role.</span></span> <span data-ttu-id="ac092-158">Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="ac092-158">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="ac092-159">Skript v režimu spánku 15 sekund umožňující chvíli pro novou službu hlavní rozšíří v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ac092-159">The script sleeps for 15 seconds to allow some time for the new service principal to propagate throughout Azure Active Directory.</span></span> <span data-ttu-id="ac092-160">Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři."</span><span class="sxs-lookup"><span data-stu-id="ac092-160">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in the directory."</span></span>
* <span data-ttu-id="ac092-161">Pokud potřebujete udělit přístup k hlavní službě pro další skupiny prostředků nebo předplatných, spusťte `New-AzureRMRoleAssignment` rutinu znovu s různými obory.</span><span class="sxs-lookup"><span data-stu-id="ac092-161">If you need to grant the service principal access to more subscriptions or resource groups, run the `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>

<span data-ttu-id="ac092-162">Pokud jste **nemají Windows 10 nebo Windows Server 2016 Technical Preview**, budete muset stáhnout [certifikát podepsaný svým držitelem generátor](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) z webu Microsoft Script Center.</span><span class="sxs-lookup"><span data-stu-id="ac092-162">If you **do not have Windows 10 or Windows Server 2016 Technical Preview**, you need to download the [Self-signed certificate generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) from Microsoft Script Center.</span></span> <span data-ttu-id="ac092-163">Rozbalte obsah a importovat rutiny, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="ac092-163">Extract its contents and import the cmdlet you need.</span></span>

```powershell  
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```
  
<span data-ttu-id="ac092-164">Ve skriptu nahraďte následující dva řádky, které se vygeneruje certifikát.</span><span class="sxs-lookup"><span data-stu-id="ac092-164">In the script, substitute the following two lines to generate the certificate.</span></span>
  
```powershell
New-SelfSignedCertificateEx  -StoreLocation CurrentUser -StoreName My -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a><span data-ttu-id="ac092-165">Zadejte certifikát pomocí automatizované skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac092-165">Provide certificate through automated PowerShell script</span></span>
<span data-ttu-id="ac092-166">Vždy, když se přihlásíte jako hlavní název služby, je třeba zadat id klienta adresáře pro vaši aplikaci AD.</span><span class="sxs-lookup"><span data-stu-id="ac092-166">Whenever you sign in as a service principal, you need to provide the tenant id of the directory for your AD app.</span></span> <span data-ttu-id="ac092-167">Klient je instance služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ac092-167">A tenant is an instance of Azure Active Directory.</span></span> <span data-ttu-id="ac092-168">Pokud máte pouze jedno předplatné, můžete použít:</span><span class="sxs-lookup"><span data-stu-id="ac092-168">If you only have one subscription, you can use:</span></span>

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertSubject,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match $CertSubject }).Thumbprint
 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

<span data-ttu-id="ac092-169">ID aplikace a ID klienta nejsou písmena, takže je můžete vložit přímo ve vašem skriptu.</span><span class="sxs-lookup"><span data-stu-id="ac092-169">The application ID and tenant ID are not sensitive, so you can embed them directly in your script.</span></span> <span data-ttu-id="ac092-170">Pokud potřebujete zjistit ID klienta, použijte:</span><span class="sxs-lookup"><span data-stu-id="ac092-170">If you need to retrieve the tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

<span data-ttu-id="ac092-171">Pokud potřebujete zjistit ID aplikace, použijte:</span><span class="sxs-lookup"><span data-stu-id="ac092-171">If you need to retrieve the application ID, use:</span></span>

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a><span data-ttu-id="ac092-172">Vytvoření instančního objektu pomocí certifikátu od certifikační autority</span><span class="sxs-lookup"><span data-stu-id="ac092-172">Create service principal with certificate from Certificate Authority</span></span>
<span data-ttu-id="ac092-173">Pokud chcete použít certifikát vydaný certifikační autority vytvoření instančního objektu, použijte následující skript:</span><span class="sxs-lookup"><span data-stu-id="ac092-173">To use a certificate issued from a Certificate Authority to create service principal, use the following script:</span></span>

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -SubscriptionId $SubscriptionId

 $KeyId = (New-Guid).Guid
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
 $KeyCredential.StartDate = $PFXCert.NotBefore
 $KeyCredential.EndDate= $PFXCert.NotAfter
 $KeyCredential.KeyId = $KeyId
 $KeyCredential.CertValue = $KeyValue

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -KeyCredentials $keyCredential
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

<span data-ttu-id="ac092-174">Několik položek si uvědomit o skriptu:</span><span class="sxs-lookup"><span data-stu-id="ac092-174">A few items to note about the script:</span></span>

* <span data-ttu-id="ac092-175">Přístup je vymezen k předplatnému.</span><span class="sxs-lookup"><span data-stu-id="ac092-175">Access is scoped to the subscription.</span></span>
* <span data-ttu-id="ac092-176">V tomto příkladu přidáte objektu služby roli Přispěvatel.</span><span class="sxs-lookup"><span data-stu-id="ac092-176">In this example, you add the service principal to the Contributor role.</span></span> <span data-ttu-id="ac092-177">Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="ac092-177">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="ac092-178">Skript v režimu spánku 15 sekund umožňující chvíli pro novou službu hlavní rozšíří v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ac092-178">The script sleeps for 15 seconds to allow some time for the new service principal to propagate throughout Azure Active Directory.</span></span> <span data-ttu-id="ac092-179">Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři."</span><span class="sxs-lookup"><span data-stu-id="ac092-179">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in the directory."</span></span>
* <span data-ttu-id="ac092-180">Pokud potřebujete udělit přístup k hlavní službě pro další skupiny prostředků nebo předplatných, spusťte `New-AzureRMRoleAssignment` rutinu znovu s různými obory.</span><span class="sxs-lookup"><span data-stu-id="ac092-180">If you need to grant the service principal access to more subscriptions or resource groups, run the `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>

### <a name="provide-certificate-through-automated-powershell-script"></a><span data-ttu-id="ac092-181">Zadejte certifikát pomocí automatizované skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac092-181">Provide certificate through automated PowerShell script</span></span>
<span data-ttu-id="ac092-182">Vždy, když se přihlásíte jako hlavní název služby, je třeba zadat id klienta adresáře pro vaši aplikaci AD.</span><span class="sxs-lookup"><span data-stu-id="ac092-182">Whenever you sign in as a service principal, you need to provide the tenant id of the directory for your AD app.</span></span> <span data-ttu-id="ac092-183">Klient je instance služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ac092-183">A tenant is an instance of Azure Active Directory.</span></span>

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

<span data-ttu-id="ac092-184">ID aplikace a ID klienta nejsou písmena, takže je můžete vložit přímo ve vašem skriptu.</span><span class="sxs-lookup"><span data-stu-id="ac092-184">The application ID and tenant ID are not sensitive, so you can embed them directly in your script.</span></span> <span data-ttu-id="ac092-185">Pokud potřebujete zjistit ID klienta, použijte:</span><span class="sxs-lookup"><span data-stu-id="ac092-185">If you need to retrieve the tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

<span data-ttu-id="ac092-186">Pokud potřebujete zjistit ID aplikace, použijte:</span><span class="sxs-lookup"><span data-stu-id="ac092-186">If you need to retrieve the application ID, use:</span></span>

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a><span data-ttu-id="ac092-187">Změnit pověření</span><span class="sxs-lookup"><span data-stu-id="ac092-187">Change credentials</span></span>

<span data-ttu-id="ac092-188">Můžete změnit pověření pro aplikaci AD, buď kvůli ohrožení zabezpečení nebo vypršení platnosti pověření [odebrat AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) a [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ac092-188">To change the credentials for an AD app, either because of a security compromise or a credential expiration, use the [Remove-AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) and [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) cmdlets.</span></span>

<span data-ttu-id="ac092-189">Chcete-li odebrat všechny přihlašovací údaje pro aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="ac092-189">To remove all the credentials for an application, use:</span></span>

```powershell
Remove-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -All
```

<span data-ttu-id="ac092-190">Chcete-li přidat heslo, použijte:</span><span class="sxs-lookup"><span data-stu-id="ac092-190">To add a password, use:</span></span>

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -Password p@ssword!
```

<span data-ttu-id="ac092-191">Přidání certifikátu hodnoty, vytvořte certifikát podepsaný svým držitelem, jak je znázorněno v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="ac092-191">To add a certificate value, create a self-signed certificate as shown in this topic.</span></span> <span data-ttu-id="ac092-192">Pak použijte:</span><span class="sxs-lookup"><span data-stu-id="ac092-192">Then, use:</span></span>

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
```

## <a name="save-access-token-to-simplify-log-in"></a><span data-ttu-id="ac092-193">Uložit přístupový token pro zjednodušení přihlášení</span><span class="sxs-lookup"><span data-stu-id="ac092-193">Save access token to simplify log in</span></span>
<span data-ttu-id="ac092-194">Abyste se vyhnuli poskytování pověření hlavní služby pokaždé, když se musí se přihlásit, můžete uložit přístupový token.</span><span class="sxs-lookup"><span data-stu-id="ac092-194">To avoid providing the service principal credentials every time it needs to log in, you can save the access token.</span></span>

<span data-ttu-id="ac092-195">Pokud chcete používat aktuální přístupový token v jiné relaci, uložení profilu.</span><span class="sxs-lookup"><span data-stu-id="ac092-195">To use the current access token in a later session, save the profile.</span></span>
   
```powershell
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```
   
<span data-ttu-id="ac092-196">Otevřete profil a zkontrolujte jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="ac092-196">Open the profile and examine its contents.</span></span> <span data-ttu-id="ac092-197">Všimněte si, že obsahuje přístupový token.</span><span class="sxs-lookup"><span data-stu-id="ac092-197">Notice that it contains an access token.</span></span> <span data-ttu-id="ac092-198">Místo ručně přihlášení znovu jednoduše načíst profil.</span><span class="sxs-lookup"><span data-stu-id="ac092-198">Instead of manually logging in again, simply load the profile.</span></span>
   
```powershell
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

> [!NOTE]
> <span data-ttu-id="ac092-199">Vyprší platnost přístupového tokenu, takže pomocí uloženého profilu funguje výhradně u tak dlouho, dokud token je platný.</span><span class="sxs-lookup"><span data-stu-id="ac092-199">The access token expires, so using a saved profile only works for as long as the token is valid.</span></span>
>  

<span data-ttu-id="ac092-200">Alternativně můžete vyvolat operace REST z prostředí PowerShell k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ac092-200">Alternatively, you can invoke REST operations from PowerShell to log in.</span></span> <span data-ttu-id="ac092-201">Z odpovědi ověřování může získat přístupový token pro použití s dalšími operacemi.</span><span class="sxs-lookup"><span data-stu-id="ac092-201">From the authentication response, you can retrieve the access token for use with other operations.</span></span> <span data-ttu-id="ac092-202">Příklad získání tokenu přístupu vyvoláním operace REST, naleznete v části [generování tokenu přístupu](resource-manager-rest-api.md#generating-an-access-token).</span><span class="sxs-lookup"><span data-stu-id="ac092-202">For an example of retrieving the access token by invoking REST operations, see [Generating an Access Token](resource-manager-rest-api.md#generating-an-access-token).</span></span>

## <a name="debug"></a><span data-ttu-id="ac092-203">Ladění</span><span class="sxs-lookup"><span data-stu-id="ac092-203">Debug</span></span>

<span data-ttu-id="ac092-204">Při vytváření objektu služby se můžete setkat s následujícími chybami:</span><span class="sxs-lookup"><span data-stu-id="ac092-204">You may encounter the following errors when creating a service principal:</span></span>

* <span data-ttu-id="ac092-205">**"Authentication_Unauthorized"** nebo **"žádné předplatné nalezena v kontextu."**</span><span class="sxs-lookup"><span data-stu-id="ac092-205">**"Authentication_Unauthorized"** or **"No subscription found in the context."**</span></span> <span data-ttu-id="ac092-206">-Se zobrazí tato chyba, pokud váš účet nemá [požadovaná oprávnění](#required-permissions) v Azure Active Directory pro registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac092-206">- You see this error when your account does not have the [required permissions](#required-permissions) on the Azure Active Directory to register an app.</span></span> <span data-ttu-id="ac092-207">Obvykle se zobrazí tato chyba při jenom správci ve vašem Azure Active Directory můžete zaregistrovat aplikace a váš účet není správce.</span><span class="sxs-lookup"><span data-stu-id="ac092-207">Typically, you see this error when only admin users in your Azure Active Directory can register apps, and your account is not an admin.</span></span> <span data-ttu-id="ac092-208">Požádejte správce buď můžete přiřadit role správce nebo umožňuje uživatelům registrovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="ac092-208">Ask your administrator to either assign you to an administrator role, or to enable users to register apps.</span></span>

* <span data-ttu-id="ac092-209">Váš účet **"nemá oprávnění k provedení akce 'Microsoft.Authorization/roleAssignments/write' u rozsahu: /subscriptions/ {guid}'."**  -Zobrazí tato chyba, pokud se váš účet nemá dostatečná oprávnění k přiřazení role identity.</span><span class="sxs-lookup"><span data-stu-id="ac092-209">Your account **"does not have authorization to perform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/{guid}'."** - You see this error when your account does not have sufficient permissions to assign a role to an identity.</span></span> <span data-ttu-id="ac092-210">Požádejte správce předplatného můžete přidat do role správce přístupu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ac092-210">Ask your subscription administrator to add you to User Access Administrator role.</span></span>

## <a name="sample-applications"></a><span data-ttu-id="ac092-211">Ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="ac092-211">Sample applications</span></span>
<span data-ttu-id="ac092-212">Informace o protokolování jako aplikace prostřednictvím různých platforem najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="ac092-212">For information about logging in as the application through different platforms, see:</span></span>

* [<span data-ttu-id="ac092-213">.NET</span><span class="sxs-lookup"><span data-stu-id="ac092-213">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="ac092-214">Java</span><span class="sxs-lookup"><span data-stu-id="ac092-214">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="ac092-215">Node.js</span><span class="sxs-lookup"><span data-stu-id="ac092-215">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="ac092-216">Python</span><span class="sxs-lookup"><span data-stu-id="ac092-216">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="ac092-217">Ruby</span><span class="sxs-lookup"><span data-stu-id="ac092-217">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a><span data-ttu-id="ac092-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ac092-218">Next steps</span></span>
* <span data-ttu-id="ac092-219">Podrobné pokyny k integraci aplikace do Azure pro správu prostředků najdete v tématu [Příručka pro vývojáře k autorizaci s rozhraním API pro Azure Resource Manager](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="ac092-219">For detailed steps on integrating an application into Azure for managing resources, see [Developer's guide to authorization with the Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="ac092-220">Podrobnější vysvětlení aplikací a objekty služby najdete v tématu [objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="ac092-220">For a more detailed explanation of applications and service principals, see [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span> 
* <span data-ttu-id="ac092-221">Další informace o ověřování Azure Active Directory najdete v tématu [scénáře ověřování pro Azure AD](../active-directory/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="ac092-221">For more information about Azure Active Directory authentication, see [Authentication Scenarios for Azure AD](../active-directory/active-directory-authentication-scenarios.md).</span></span>
* <span data-ttu-id="ac092-222">Seznam dostupných akcí, které může být povolen nebo odepřen uživatelům najdete v tématu [poskytovatel prostředků Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="ac092-222">For a list of available actions that can be granted or denied to users, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>

