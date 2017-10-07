---
title: "aaaCreate identitu pro aplikace Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Popisuje, jak řídit toouse prostředí Azure PowerShell toocreate aplikaci Azure Active Directory a objektu služby a udělte ho tooresources přístup prostřednictvím přístupu na základě rolí. Ukazuje, jak tooauthenticate aplikace pomocí hesla nebo certifikátu."
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
ms.openlocfilehash: c534360799b590054a051e4426e5e27dccb559b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-powershell-toocreate-a-service-principal-tooaccess-resources"></a><span data-ttu-id="03747-104">Použití Azure PowerShell toocreate hlavní tooaccess prostředky služby</span><span class="sxs-lookup"><span data-stu-id="03747-104">Use Azure PowerShell toocreate a service principal tooaccess resources</span></span>

<span data-ttu-id="03747-105">Pokud máte aplikace nebo skript, který potřebuje tooaccess prostředků, můžete nastavit identity aplikace hello a ověřit hello aplikaci s svoje vlastní přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="03747-105">When you have an app or script that needs tooaccess resources, you can set up an identity for hello app and authenticate hello app with its own credentials.</span></span> <span data-ttu-id="03747-106">Tato identita se označuje jako hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="03747-106">This identity is known as a service principal.</span></span> <span data-ttu-id="03747-107">Tento přístup umožňuje:</span><span class="sxs-lookup"><span data-stu-id="03747-107">This approach enables you to:</span></span>

* <span data-ttu-id="03747-108">Přiřazení oprávnění toohello identita aplikace, která se liší od vlastní oprávnění.</span><span class="sxs-lookup"><span data-stu-id="03747-108">Assign permissions toohello app identity that are different than your own permissions.</span></span> <span data-ttu-id="03747-109">Obvykle se tato oprávnění se s omezeným přístupem tooexactly jaké aplikace hello musí toodo.</span><span class="sxs-lookup"><span data-stu-id="03747-109">Typically, these permissions are restricted tooexactly what hello app needs toodo.</span></span>
* <span data-ttu-id="03747-110">Při provádění bezobslužného skriptu, použijte certifikát pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="03747-110">Use a certificate for authentication when executing an unattended script.</span></span>

<span data-ttu-id="03747-111">Toto téma ukazuje, jak toouse [prostředí Azure PowerShell](/powershell/azure/overview) tooset až všechno, co potřebujete pro toorun aplikaci v části vlastní pověření a identity.</span><span class="sxs-lookup"><span data-stu-id="03747-111">This topic shows you how toouse [Azure PowerShell](/powershell/azure/overview) tooset up everything you need for an application toorun under its own credentials and identity.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="03747-112">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="03747-112">Required permissions</span></span>
<span data-ttu-id="03747-113">toocomplete tohoto tématu, musíte mít dostatečná oprávnění v Azure Active Directory a vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="03747-113">toocomplete this topic, you must have sufficient permissions in both your Azure Active Directory and your Azure subscription.</span></span> <span data-ttu-id="03747-114">Konkrétně musí být schopný toocreate aplikace v hello Azure Active Directory a přiřadit role hlavní tooa služby hello.</span><span class="sxs-lookup"><span data-stu-id="03747-114">Specifically, you must be able toocreate an app in hello Azure Active Directory, and assign hello service principal tooa role.</span></span> 

<span data-ttu-id="03747-115">Hello bylo jestli váš účet má odpovídající oprávnění je prostřednictvím portálu hello nejjednodušší toocheck způsobem.</span><span class="sxs-lookup"><span data-stu-id="03747-115">hello easiest way toocheck whether your account has adequate permissions is through hello portal.</span></span> <span data-ttu-id="03747-116">V tématu [zkontrolujte požadované oprávnění](resource-group-create-service-principal-portal.md#required-permissions).</span><span class="sxs-lookup"><span data-stu-id="03747-116">See [Check required permission](resource-group-create-service-principal-portal.md#required-permissions).</span></span>

<span data-ttu-id="03747-117">Nyní pokračujte tooa části k ověřování:</span><span class="sxs-lookup"><span data-stu-id="03747-117">Now, proceed tooa section for authenticating with:</span></span>

* [<span data-ttu-id="03747-118">heslo</span><span class="sxs-lookup"><span data-stu-id="03747-118">password</span></span>](#create-service-principal-with-password)
* [<span data-ttu-id="03747-119">certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="03747-119">self-signed certificate</span></span>](#create-service-principal-with-self-signed-certificate)
* [<span data-ttu-id="03747-120">certifikát od certifikační autority</span><span class="sxs-lookup"><span data-stu-id="03747-120">certificate from Certificate Authority</span></span>](#create-service-principal-with-certificate-from-certificate-authority)

## <a name="powershell-commands"></a><span data-ttu-id="03747-121">Příkazy prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="03747-121">PowerShell commands</span></span>

<span data-ttu-id="03747-122">tooset až objekt služby, použijte:</span><span class="sxs-lookup"><span data-stu-id="03747-122">tooset up a service principal, you use:</span></span>

| <span data-ttu-id="03747-123">Příkaz</span><span class="sxs-lookup"><span data-stu-id="03747-123">Command</span></span> | <span data-ttu-id="03747-124">Popis</span><span class="sxs-lookup"><span data-stu-id="03747-124">Description</span></span> |
| ------- | ----------- | 
| [<span data-ttu-id="03747-125">Nové AzureRmADServicePrincipal</span><span class="sxs-lookup"><span data-stu-id="03747-125">New-AzureRmADServicePrincipal</span></span>](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | <span data-ttu-id="03747-126">Vytvoří objekt služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="03747-126">Creates an Azure Active Directory service principal</span></span> |
| [<span data-ttu-id="03747-127">New-AzureRmRoleAssignment</span><span class="sxs-lookup"><span data-stu-id="03747-127">New-AzureRmRoleAssignment</span></span>](/powershell/module/azurerm.resources/new-azurermroleassignment) | <span data-ttu-id="03747-128">Přiřadí hello zadaný RBAC role toohello zadaný objekt, na hello zadaný obor.</span><span class="sxs-lookup"><span data-stu-id="03747-128">Assigns hello specified RBAC role toohello specified principal, at hello specified scope.</span></span> |


## <a name="create-service-principal-with-password"></a><span data-ttu-id="03747-129">Vytvoření instančního objektu s heslem</span><span class="sxs-lookup"><span data-stu-id="03747-129">Create service principal with password</span></span>

<span data-ttu-id="03747-130">toocreate objekt služby s hello role Přispěvatel pro vaše předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="03747-130">toocreate a service principal with hello Contributor role for your subscription, use:</span></span> 

```powershell
Login-AzureRmAccount
$sp = New-AzureRmADServicePrincipal -DisplayName exampleapp -Password "{provide-password}"
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

<span data-ttu-id="03747-131">Příklad Hello v režimu spánku 20 sekund tooallow nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="03747-131">hello example sleeps for 20 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="03747-132">Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři hello."</span><span class="sxs-lookup"><span data-stu-id="03747-132">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>

<span data-ttu-id="03747-133">Hello následující skript vám umožní toospecify obor než hello výchozí předplatné a opakování hello přiřazení role, pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="03747-133">hello following script enables you toospecify a scope other than hello default subscription, and retries hello role assignment if an error occurs:</span></span>

```powershell
Param (

 # Use tooset scope tooresource group. If no value is provided, scope is set toosubscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use tooset subscription. If no value is provided, default subscription is used. 
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

 
 # Create Service Principal for hello AD app
 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

<span data-ttu-id="03747-134">Několik položek toonote o hello skriptu:</span><span class="sxs-lookup"><span data-stu-id="03747-134">A few items toonote about hello script:</span></span>

* <span data-ttu-id="03747-135">toogrant hello identity přístup toohello výchozí předplatné, není nutné tooprovide parametry ResourceGroup nebo ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="03747-135">toogrant hello identity access toohello default subscription, you do not need tooprovide either ResourceGroup or SubscriptionId parameters.</span></span>
* <span data-ttu-id="03747-136">Zadejte parametr ResourceGroup hello pouze v případě, že chcete toolimit hello obor skupiny prostředků tooa přiřazení role hello.</span><span class="sxs-lookup"><span data-stu-id="03747-136">Specify hello ResourceGroup parameter only when you want toolimit hello scope of hello role assignment tooa resource group.</span></span>
*  <span data-ttu-id="03747-137">V tomto příkladu přidáte roli Přispěvatel hlavní toohello služby hello.</span><span class="sxs-lookup"><span data-stu-id="03747-137">In this example, you add hello service principal toohello Contributor role.</span></span> <span data-ttu-id="03747-138">Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="03747-138">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="03747-139">Hello skriptu v režimu spánku 15 sekund tooallow nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="03747-139">hello script sleeps for 15 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="03747-140">Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři hello."</span><span class="sxs-lookup"><span data-stu-id="03747-140">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>
* <span data-ttu-id="03747-141">Pokud potřebujete toogrant hello služby hlavní přístup toomore odběry nebo skupiny prostředků, spusťte hello `New-AzureRMRoleAssignment` rutinu znovu s různými obory.</span><span class="sxs-lookup"><span data-stu-id="03747-141">If you need toogrant hello service principal access toomore subscriptions or resource groups, run hello `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>


### <a name="provide-credentials-through-powershell"></a><span data-ttu-id="03747-142">Zadejte přihlašovací údaje pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="03747-142">Provide credentials through PowerShell</span></span>
<span data-ttu-id="03747-143">Nyní musíte toolog v jako operace tooperform aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="03747-143">Now, you need toolog in as hello application tooperform operations.</span></span> <span data-ttu-id="03747-144">Pro hello uživatelské jméno, použijte hello `ApplicationId` kterou jste vytvořili pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="03747-144">For hello user name, use hello `ApplicationId` that you created for hello application.</span></span> <span data-ttu-id="03747-145">Hello heslo použijte hello jeden, který jste zadali při vytváření účtu hello.</span><span class="sxs-lookup"><span data-stu-id="03747-145">For hello password, use hello one you specified when creating hello account.</span></span> 

```powershell   
$creds = Get-Credential
Login-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId {tenant-id}
```

<span data-ttu-id="03747-146">ID klienta Hello nerozlišuje malá písmena, takže jej můžete vložit přímo ve vašem skriptu.</span><span class="sxs-lookup"><span data-stu-id="03747-146">hello tenant ID is not sensitive, so you can embed it directly in your script.</span></span> <span data-ttu-id="03747-147">Pokud potřebujete ID klienta hello tooretrieve, použijte:</span><span class="sxs-lookup"><span data-stu-id="03747-147">If you need tooretrieve hello tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

## <a name="create-service-principal-with-self-signed-certificate"></a><span data-ttu-id="03747-148">Vytvoření instančního objektu se certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="03747-148">Create service principal with self-signed certificate</span></span>

<span data-ttu-id="03747-149">toocreate hlavní název služby pomocí certifikátu podepsaného svým držitelem a hello role Přispěvatel pro vaše předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="03747-149">toocreate a service principal with a self-signed certificate and hello Contributor role for your subscription, use:</span></span> 

```powershell
Login-AzureRmAccount
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

<span data-ttu-id="03747-150">Příklad Hello v režimu spánku 20 sekund tooallow nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="03747-150">hello example sleeps for 20 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="03747-151">Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři hello."</span><span class="sxs-lookup"><span data-stu-id="03747-151">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>

<span data-ttu-id="03747-152">Hello následující skript vám umožní toospecify obor než hello výchozí předplatné a opakování hello přiřazení role, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="03747-152">hello following script enables you toospecify a scope other than hello default subscription, and retries hello role assignment if an error occurs.</span></span> <span data-ttu-id="03747-153">Pokud nemáte Azure PowerShell 2.0 na Windows 10 nebo Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="03747-153">You must have Azure PowerShell 2.0 on Windows 10 or Windows Server 2016.</span></span>

```powershell
Param (

 # Use tooset scope tooresource group. If no value is provided, scope is set toosubscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use tooset subscription. If no value is provided, default subscription is used. 
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
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

<span data-ttu-id="03747-154">Několik položek toonote o hello skriptu:</span><span class="sxs-lookup"><span data-stu-id="03747-154">A few items toonote about hello script:</span></span>

* <span data-ttu-id="03747-155">toogrant hello identity přístup toohello výchozí předplatné, není nutné tooprovide parametry ResourceGroup nebo ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="03747-155">toogrant hello identity access toohello default subscription, you do not need tooprovide either ResourceGroup or SubscriptionId parameters.</span></span>
* <span data-ttu-id="03747-156">Zadejte parametr ResourceGroup hello pouze v případě, že chcete toolimit hello obor skupiny prostředků tooa přiřazení role hello.</span><span class="sxs-lookup"><span data-stu-id="03747-156">Specify hello ResourceGroup parameter only when you want toolimit hello scope of hello role assignment tooa resource group.</span></span>
* <span data-ttu-id="03747-157">V tomto příkladu přidáte roli Přispěvatel hlavní toohello služby hello.</span><span class="sxs-lookup"><span data-stu-id="03747-157">In this example, you add hello service principal toohello Contributor role.</span></span> <span data-ttu-id="03747-158">Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="03747-158">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="03747-159">Hello skriptu v režimu spánku 15 sekund tooallow nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="03747-159">hello script sleeps for 15 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="03747-160">Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři hello."</span><span class="sxs-lookup"><span data-stu-id="03747-160">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>
* <span data-ttu-id="03747-161">Pokud potřebujete toogrant hello služby hlavní přístup toomore odběry nebo skupiny prostředků, spusťte hello `New-AzureRMRoleAssignment` rutinu znovu s různými obory.</span><span class="sxs-lookup"><span data-stu-id="03747-161">If you need toogrant hello service principal access toomore subscriptions or resource groups, run hello `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>

<span data-ttu-id="03747-162">Pokud jste **nemají Windows 10 nebo Windows Server 2016 Technical Preview**, budete potřebovat toodownload hello [certifikát podepsaný svým držitelem generátor](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) z webu Microsoft Script Center.</span><span class="sxs-lookup"><span data-stu-id="03747-162">If you **do not have Windows 10 or Windows Server 2016 Technical Preview**, you need toodownload hello [Self-signed certificate generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) from Microsoft Script Center.</span></span> <span data-ttu-id="03747-163">Rozbalte obsah a importovat hello rutiny, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="03747-163">Extract its contents and import hello cmdlet you need.</span></span>

```powershell  
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```
  
<span data-ttu-id="03747-164">Ve skriptu hello nahraďte hello následující dva řádky toogenerate hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="03747-164">In hello script, substitute hello following two lines toogenerate hello certificate.</span></span>
  
```powershell
New-SelfSignedCertificateEx  -StoreLocation CurrentUser -StoreName My -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a><span data-ttu-id="03747-165">Zadejte certifikát pomocí automatizované skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="03747-165">Provide certificate through automated PowerShell script</span></span>
<span data-ttu-id="03747-166">Vždy, když se přihlásíte jako hlavní název služby, musíte pro vaši aplikaci AD id klienta hello tooprovide hello adresáře.</span><span class="sxs-lookup"><span data-stu-id="03747-166">Whenever you sign in as a service principal, you need tooprovide hello tenant id of hello directory for your AD app.</span></span> <span data-ttu-id="03747-167">Klient je instance služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="03747-167">A tenant is an instance of Azure Active Directory.</span></span> <span data-ttu-id="03747-168">Pokud máte pouze jedno předplatné, můžete použít:</span><span class="sxs-lookup"><span data-stu-id="03747-168">If you only have one subscription, you can use:</span></span>

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

<span data-ttu-id="03747-169">aplikace Hello ID a ID klienta nejsou písmena, takže je můžete vložit přímo ve vašem skriptu.</span><span class="sxs-lookup"><span data-stu-id="03747-169">hello application ID and tenant ID are not sensitive, so you can embed them directly in your script.</span></span> <span data-ttu-id="03747-170">Pokud potřebujete ID klienta hello tooretrieve, použijte:</span><span class="sxs-lookup"><span data-stu-id="03747-170">If you need tooretrieve hello tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

<span data-ttu-id="03747-171">Pokud potřebujete ID aplikace hello tooretrieve, použijte:</span><span class="sxs-lookup"><span data-stu-id="03747-171">If you need tooretrieve hello application ID, use:</span></span>

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a><span data-ttu-id="03747-172">Vytvoření instančního objektu pomocí certifikátu od certifikační autority</span><span class="sxs-lookup"><span data-stu-id="03747-172">Create service principal with certificate from Certificate Authority</span></span>
<span data-ttu-id="03747-173">toouse certifikátu vydaného certifikační autority toocreate hlavní název služby, použijte hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="03747-173">toouse a certificate issued from a Certificate Authority toocreate service principal, use hello following script:</span></span>

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
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

<span data-ttu-id="03747-174">Několik položek toonote o hello skriptu:</span><span class="sxs-lookup"><span data-stu-id="03747-174">A few items toonote about hello script:</span></span>

* <span data-ttu-id="03747-175">Přístup je vymezená toohello předplatné.</span><span class="sxs-lookup"><span data-stu-id="03747-175">Access is scoped toohello subscription.</span></span>
* <span data-ttu-id="03747-176">V tomto příkladu přidáte roli Přispěvatel hlavní toohello služby hello.</span><span class="sxs-lookup"><span data-stu-id="03747-176">In this example, you add hello service principal toohello Contributor role.</span></span> <span data-ttu-id="03747-177">Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="03747-177">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="03747-178">Hello skriptu v režimu spánku 15 sekund tooallow nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="03747-178">hello script sleeps for 15 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="03747-179">Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři hello."</span><span class="sxs-lookup"><span data-stu-id="03747-179">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>
* <span data-ttu-id="03747-180">Pokud potřebujete toogrant hello služby hlavní přístup toomore odběry nebo skupiny prostředků, spusťte hello `New-AzureRMRoleAssignment` rutinu znovu s různými obory.</span><span class="sxs-lookup"><span data-stu-id="03747-180">If you need toogrant hello service principal access toomore subscriptions or resource groups, run hello `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>

### <a name="provide-certificate-through-automated-powershell-script"></a><span data-ttu-id="03747-181">Zadejte certifikát pomocí automatizované skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="03747-181">Provide certificate through automated PowerShell script</span></span>
<span data-ttu-id="03747-182">Vždy, když se přihlásíte jako hlavní název služby, musíte pro vaši aplikaci AD id klienta hello tooprovide hello adresáře.</span><span class="sxs-lookup"><span data-stu-id="03747-182">Whenever you sign in as a service principal, you need tooprovide hello tenant id of hello directory for your AD app.</span></span> <span data-ttu-id="03747-183">Klient je instance služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="03747-183">A tenant is an instance of Azure Active Directory.</span></span>

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

<span data-ttu-id="03747-184">aplikace Hello ID a ID klienta nejsou písmena, takže je můžete vložit přímo ve vašem skriptu.</span><span class="sxs-lookup"><span data-stu-id="03747-184">hello application ID and tenant ID are not sensitive, so you can embed them directly in your script.</span></span> <span data-ttu-id="03747-185">Pokud potřebujete ID klienta hello tooretrieve, použijte:</span><span class="sxs-lookup"><span data-stu-id="03747-185">If you need tooretrieve hello tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

<span data-ttu-id="03747-186">Pokud potřebujete ID aplikace hello tooretrieve, použijte:</span><span class="sxs-lookup"><span data-stu-id="03747-186">If you need tooretrieve hello application ID, use:</span></span>

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a><span data-ttu-id="03747-187">Změnit pověření</span><span class="sxs-lookup"><span data-stu-id="03747-187">Change credentials</span></span>

<span data-ttu-id="03747-188">přihlašovací údaje hello toochange pro aplikaci AD, buď kvůli ohrožení zabezpečení nebo vypršení platnosti pověření použít hello [odebrat AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) a [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) rutiny.</span><span class="sxs-lookup"><span data-stu-id="03747-188">toochange hello credentials for an AD app, either because of a security compromise or a credential expiration, use hello [Remove-AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) and [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) cmdlets.</span></span>

<span data-ttu-id="03747-189">tooremove všechny hello přihlašovací údaje pro aplikaci, použijte:</span><span class="sxs-lookup"><span data-stu-id="03747-189">tooremove all hello credentials for an application, use:</span></span>

```powershell
Remove-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -All
```

<span data-ttu-id="03747-190">tooadd heslo, použijte:</span><span class="sxs-lookup"><span data-stu-id="03747-190">tooadd a password, use:</span></span>

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -Password p@ssword!
```

<span data-ttu-id="03747-191">tooadd hodnotu certifikát vytvořit certifikát podepsaný svým držitelem, jak je znázorněno v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="03747-191">tooadd a certificate value, create a self-signed certificate as shown in this topic.</span></span> <span data-ttu-id="03747-192">Pak použijte:</span><span class="sxs-lookup"><span data-stu-id="03747-192">Then, use:</span></span>

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
```

## <a name="save-access-token-toosimplify-log-in"></a><span data-ttu-id="03747-193">Uložit přístup tokenu toosimplify přihlášení</span><span class="sxs-lookup"><span data-stu-id="03747-193">Save access token toosimplify log in</span></span>
<span data-ttu-id="03747-194">tooavoid poskytnete hello služby hlavní přihlašovací údaje pokaždé, když je nutné toolog v, můžete uložit hello přístupový token.</span><span class="sxs-lookup"><span data-stu-id="03747-194">tooavoid providing hello service principal credentials every time it needs toolog in, you can save hello access token.</span></span>

<span data-ttu-id="03747-195">toouse hello aktuální přístupový token v jiné relaci, uložte profil hello.</span><span class="sxs-lookup"><span data-stu-id="03747-195">toouse hello current access token in a later session, save hello profile.</span></span>
   
```powershell
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```
   
<span data-ttu-id="03747-196">Otevřete profil hello a zkontrolujte jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="03747-196">Open hello profile and examine its contents.</span></span> <span data-ttu-id="03747-197">Všimněte si, že obsahuje přístupový token.</span><span class="sxs-lookup"><span data-stu-id="03747-197">Notice that it contains an access token.</span></span> <span data-ttu-id="03747-198">Místo ručně přihlášení znovu načíst jednoduše hello profilu.</span><span class="sxs-lookup"><span data-stu-id="03747-198">Instead of manually logging in again, simply load hello profile.</span></span>
   
```powershell
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

> [!NOTE]
> <span data-ttu-id="03747-199">Hello přístupovému tokenu vyprší platnost, takže pomocí uloženého profilu funguje výhradně u tak dlouho, dokud hello token je platný.</span><span class="sxs-lookup"><span data-stu-id="03747-199">hello access token expires, so using a saved profile only works for as long as hello token is valid.</span></span>
>  

<span data-ttu-id="03747-200">Alternativně můžete vyvolat operace REST z prostředí PowerShell toolog v.</span><span class="sxs-lookup"><span data-stu-id="03747-200">Alternatively, you can invoke REST operations from PowerShell toolog in.</span></span> <span data-ttu-id="03747-201">Z hello odpověď ověřování můžete načíst hello přístupový token pro použití s dalšími operacemi.</span><span class="sxs-lookup"><span data-stu-id="03747-201">From hello authentication response, you can retrieve hello access token for use with other operations.</span></span> <span data-ttu-id="03747-202">Příklad získání tokenu přístupu hello vyvoláním operace REST, naleznete v části [generování tokenu přístupu](resource-manager-rest-api.md#generating-an-access-token).</span><span class="sxs-lookup"><span data-stu-id="03747-202">For an example of retrieving hello access token by invoking REST operations, see [Generating an Access Token](resource-manager-rest-api.md#generating-an-access-token).</span></span>

## <a name="debug"></a><span data-ttu-id="03747-203">Ladění</span><span class="sxs-lookup"><span data-stu-id="03747-203">Debug</span></span>

<span data-ttu-id="03747-204">Můžete setkat s následujícím chybám při vytváření objektu služby hello:</span><span class="sxs-lookup"><span data-stu-id="03747-204">You may encounter hello following errors when creating a service principal:</span></span>

* <span data-ttu-id="03747-205">**"Authentication_Unauthorized"** nebo **"žádné předplatné nalezena v kontextu hello".**</span><span class="sxs-lookup"><span data-stu-id="03747-205">**"Authentication_Unauthorized"** or **"No subscription found in hello context."**</span></span> <span data-ttu-id="03747-206">-Se zobrazí tato chyba, pokud váš účet nemá hello [požadovaná oprávnění](#required-permissions) na Azure Active Directory tooregister hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="03747-206">- You see this error when your account does not have hello [required permissions](#required-permissions) on hello Azure Active Directory tooregister an app.</span></span> <span data-ttu-id="03747-207">Obvykle se zobrazí tato chyba při jenom správci ve vašem Azure Active Directory můžete zaregistrovat aplikace a váš účet není správce. Požádejte vašeho správce tooeither přiřadit můžete tooan roli správce, nebo tooenable uživatelé tooregister aplikace.</span><span class="sxs-lookup"><span data-stu-id="03747-207">Typically, you see this error when only admin users in your Azure Active Directory can register apps, and your account is not an admin. Ask your administrator tooeither assign you tooan administrator role, or tooenable users tooregister apps.</span></span>

* <span data-ttu-id="03747-208">Váš účet **"nemá autorizace tooperform akce 'Microsoft.Authorization/roleAssignments/write' u rozsahu: /subscriptions/ {guid}'."**  -Zobrazí tato chyba, pokud se váš účet nemá dostatečná oprávnění tooassign identitu tooan role.</span><span class="sxs-lookup"><span data-stu-id="03747-208">Your account **"does not have authorization tooperform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/{guid}'."** - You see this error when your account does not have sufficient permissions tooassign a role tooan identity.</span></span> <span data-ttu-id="03747-209">Požádejte správce tooadd vaše předplatné můžete tooUser přístupu k roli správce.</span><span class="sxs-lookup"><span data-stu-id="03747-209">Ask your subscription administrator tooadd you tooUser Access Administrator role.</span></span>

## <a name="sample-applications"></a><span data-ttu-id="03747-210">Ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="03747-210">Sample applications</span></span>
<span data-ttu-id="03747-211">Informace o protokolování jako aplikace hello prostřednictvím různých platforem najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="03747-211">For information about logging in as hello application through different platforms, see:</span></span>

* [<span data-ttu-id="03747-212">.NET</span><span class="sxs-lookup"><span data-stu-id="03747-212">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="03747-213">Java</span><span class="sxs-lookup"><span data-stu-id="03747-213">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="03747-214">Node.js</span><span class="sxs-lookup"><span data-stu-id="03747-214">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="03747-215">Python</span><span class="sxs-lookup"><span data-stu-id="03747-215">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="03747-216">Ruby</span><span class="sxs-lookup"><span data-stu-id="03747-216">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a><span data-ttu-id="03747-217">Další kroky</span><span class="sxs-lookup"><span data-stu-id="03747-217">Next steps</span></span>
* <span data-ttu-id="03747-218">Podrobné pokyny k integraci aplikace do Azure pro správu prostředků najdete v tématu [tooauthorization Příručka pro vývojáře s hello rozhraní API služby Azure Resource Manager](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="03747-218">For detailed steps on integrating an application into Azure for managing resources, see [Developer's guide tooauthorization with hello Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="03747-219">Podrobnější vysvětlení aplikací a objekty služby najdete v tématu [objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="03747-219">For a more detailed explanation of applications and service principals, see [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span> 
* <span data-ttu-id="03747-220">Další informace o ověřování Azure Active Directory najdete v tématu [scénáře ověřování pro Azure AD](../active-directory/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="03747-220">For more information about Azure Active Directory authentication, see [Authentication Scenarios for Azure AD](../active-directory/active-directory-authentication-scenarios.md).</span></span>
* <span data-ttu-id="03747-221">Seznam dostupných akcí, které může být povolen nebo odepřen toousers najdete v tématu [poskytovatel prostředků Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="03747-221">For a list of available actions that can be granted or denied toousers, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>

