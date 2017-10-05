---
title: "Azure Active Directory Domain Services: Průvodce odstraňováním potíží | Microsoft Docs"
description: "Průvodce řešením potíží pro Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: d6695b0c40f56093e8701dfe6394143268114453
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-domain-services---troubleshooting-guide"></a><span data-ttu-id="423c7-103">Azure AD Domain Services – Průvodce odstraňováním potíží s</span><span class="sxs-lookup"><span data-stu-id="423c7-103">Azure AD Domain Services - Troubleshooting guide</span></span>
<span data-ttu-id="423c7-104">Tento článek obsahuje pokyny k odstranění potíží pro problémy, se kterými se můžete setkat při nastavení nebo jejich správě Azure Active Directory (AD) Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-104">This article provides troubleshooting hints for issues you may encounter when setting up or administering Azure Active Directory (AD) Domain Services.</span></span>

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a><span data-ttu-id="423c7-105">Nejde povolit Azure AD Domain Services pro svůj adresář Azure AD</span><span class="sxs-lookup"><span data-stu-id="423c7-105">You cannot enable Azure AD Domain Services for your Azure AD directory</span></span>
<span data-ttu-id="423c7-106">Tato část vám pomůže vyřešit chyby při pokusu o povolení služby Azure AD Domain Services pro svůj adresář a selže nebo získá přepínat stav zpět na "Zakázáno".</span><span class="sxs-lookup"><span data-stu-id="423c7-106">This section helps you troubleshoot errors when you try to enable Azure AD Domain Services for your directory and it fails or gets toggled back to 'Disabled'.</span></span>

<span data-ttu-id="423c7-107">Vyberte kroky řešení potíží, které odpovídají v chybové zprávě, na které narazíte.</span><span class="sxs-lookup"><span data-stu-id="423c7-107">Pick the troubleshooting steps that correspond to the error message you encounter.</span></span>

| <span data-ttu-id="423c7-108">**Chybová zpráva**</span><span class="sxs-lookup"><span data-stu-id="423c7-108">**Error Message**</span></span> | <span data-ttu-id="423c7-109">**Řešení**</span><span class="sxs-lookup"><span data-stu-id="423c7-109">**Resolution**</span></span> |
| --- |:--- |
| <span data-ttu-id="423c7-110">*Název contoso100.com se už v síti používá. Zadejte název, který se nepoužívá.*</span><span class="sxs-lookup"><span data-stu-id="423c7-110">*The name contoso100.com is already in use on this network. Specify a name that is not in use.*</span></span> |[<span data-ttu-id="423c7-111">Konflikt názvů domény ve virtuální síti</span><span class="sxs-lookup"><span data-stu-id="423c7-111">Domain name conflict in the virtual network</span></span>](active-directory-ds-troubleshooting.md#domain-name-conflict) |
| <span data-ttu-id="423c7-112">*Domain Services nelze povolit v tomto tenantovi Azure AD. Služba nemá dostatečná oprávnění pro aplikaci s názvem Azure AD Domain Services Sync. Odstraňte aplikace s názvem Azure AD Domain Services Sync a potom se pokuste pro vašeho tenanta Azure AD povolit Domain Services.*</span><span class="sxs-lookup"><span data-stu-id="423c7-112">*Domain Services could not be enabled in this Azure AD tenant. The service does not have adequate permissions to the application called 'Azure AD Domain Services Sync'. Delete the application called 'Azure AD Domain Services Sync' and then try to enable Domain Services for your Azure AD tenant.*</span></span> |[<span data-ttu-id="423c7-113">Domain Services nemá dostatečná oprávnění k aplikaci synchronizace Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="423c7-113">Domain Services does not have adequate permissions to the Azure AD Domain Services Sync application</span></span>](active-directory-ds-troubleshooting.md#inadequate-permissions) |
| <span data-ttu-id="423c7-114">*Domain Services nelze povolit v tomto tenantovi Azure AD. Aplikace Domain Services ve vašem tenantovi Azure AD nemá požadovaná oprávnění k povolení Domain Services. Odstraňte aplikaci s identifikátorem aplikace d87dcbc6-a371-462e-88e3-28ad15ec4e64 a potom se pokuste pro vašeho tenanta Azure AD povolit Domain Services.*</span><span class="sxs-lookup"><span data-stu-id="423c7-114">*Domain Services could not be enabled in this Azure AD tenant. The Domain Services application in your Azure AD tenant does not have the required permissions to enable Domain Services. Delete the application with the application identifier d87dcbc6-a371-462e-88e3-28ad15ec4e64 and then try to enable Domain Services for your Azure AD tenant.*</span></span> |[<span data-ttu-id="423c7-115">Aplikace Domain Services není správně nakonfigurována ve vašem klientovi</span><span class="sxs-lookup"><span data-stu-id="423c7-115">The Domain Services application is not configured properly in your tenant</span></span>](active-directory-ds-troubleshooting.md#invalid-configuration) |
| <span data-ttu-id="423c7-116">*Domain Services nelze povolit v tomto tenantovi Azure AD. Aplikace Microsoft Azure AD je ve vašem tenantovi Azure AD zakázaná. Povolte aplikaci s identifikátorem aplikace 00000002-0000-0000-c000-000000000000 a potom se pokuste pro vašeho tenanta Azure AD povolit Domain Services.*</span><span class="sxs-lookup"><span data-stu-id="423c7-116">*Domain Services could not be enabled in this Azure AD tenant. The Microsoft Azure AD application is disabled in your Azure AD tenant. Enable the application with the application identifier 00000002-0000-0000-c000-000000000000 and then try to enable Domain Services for your Azure AD tenant.*</span></span> |[<span data-ttu-id="423c7-117">Aplikace Microsoft Graph je zakázána v klientovi služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="423c7-117">The Microsoft Graph application is disabled in your Azure AD tenant</span></span>](active-directory-ds-troubleshooting.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a><span data-ttu-id="423c7-118">Konflikt názvů domény</span><span class="sxs-lookup"><span data-stu-id="423c7-118">Domain Name conflict</span></span>
<span data-ttu-id="423c7-119">**Chybová zpráva:**</span><span class="sxs-lookup"><span data-stu-id="423c7-119">**Error message:**</span></span>

<span data-ttu-id="423c7-120">*Název contoso100.com se už v síti používá. Zadejte název, který se nepoužívá.*</span><span class="sxs-lookup"><span data-stu-id="423c7-120">*The name contoso100.com is already in use on this network. Specify a name that is not in use.*</span></span>

<span data-ttu-id="423c7-121">**Náprava:**</span><span class="sxs-lookup"><span data-stu-id="423c7-121">**Remediation:**</span></span>

<span data-ttu-id="423c7-122">Ujistěte se, že nemáte existující doménu se stejným názvem domény k dispozici ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="423c7-122">Ensure that you do not have an existing domain with the same domain name available on that virtual network.</span></span> <span data-ttu-id="423c7-123">Například předpokládejme, že již máte doménu „contoso.com“ k dispozici na vybrané virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="423c7-123">For instance, assume you have a domain called 'contoso.com' already available on the selected virtual network.</span></span> <span data-ttu-id="423c7-124">Později Zkuste povolit spravované doméně služby Azure AD Domain Services se stejným názvem domény (tedy "contoso.com") ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="423c7-124">Later, you try to enable an Azure AD Domain Services managed domain with the same domain name (that is, 'contoso.com') on that virtual network.</span></span> <span data-ttu-id="423c7-125">Dojde k chybě při pokusu o povolení služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-125">You encounter a failure when trying to enable Azure AD Domain Services.</span></span>

<span data-ttu-id="423c7-126">Toto selhání je z důvodu konfliktu názvů pro název domény ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="423c7-126">This failure is due to name conflicts for the domain name on that virtual network.</span></span> <span data-ttu-id="423c7-127">V této situaci musíte použít jiný název pro nastavení domény spravované službou Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-127">In this situation, you must use a different name to set up your Azure AD Domain Services managed domain.</span></span> <span data-ttu-id="423c7-128">Případně můžete zrušit existující doménu a poté pokračovat v povolení služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-128">Alternately, you can de-provision the existing domain and then proceed to enable Azure AD Domain Services.</span></span>

### <a name="inadequate-permissions"></a><span data-ttu-id="423c7-129">Nedostatečná oprávnění</span><span class="sxs-lookup"><span data-stu-id="423c7-129">Inadequate permissions</span></span>
<span data-ttu-id="423c7-130">**Chybová zpráva:**</span><span class="sxs-lookup"><span data-stu-id="423c7-130">**Error message:**</span></span>

<span data-ttu-id="423c7-131">*Domain Services nelze povolit v tomto tenantovi Azure AD. Služba nemá dostatečná oprávnění pro aplikaci s názvem Azure AD Domain Services Sync. Odstraňte aplikace s názvem Azure AD Domain Services Sync a potom se pokuste pro vašeho tenanta Azure AD povolit Domain Services.*</span><span class="sxs-lookup"><span data-stu-id="423c7-131">*Domain Services could not be enabled in this Azure AD tenant. The service does not have adequate permissions to the application called 'Azure AD Domain Services Sync'. Delete the application called 'Azure AD Domain Services Sync' and then try to enable Domain Services for your Azure AD tenant.*</span></span>

<span data-ttu-id="423c7-132">**Náprava:**</span><span class="sxs-lookup"><span data-stu-id="423c7-132">**Remediation:**</span></span>

<span data-ttu-id="423c7-133">Zkontrolujte, zda je aplikace s názvem "Azure AD Domain Services synchronizace, v adresáři služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="423c7-133">Check to see if there is an application with the name 'Azure AD Domain Services Sync' in your Azure AD directory.</span></span> <span data-ttu-id="423c7-134">Pokud tuto aplikaci existuje, odstraňte ji a pak opětovného povolení služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-134">If this application exists, delete it and then re-enable Azure AD Domain Services.</span></span>

<span data-ttu-id="423c7-135">Proveďte následující kroky k kontrolovat přítomnost aplikace a k odstranění, pokud existuje aplikace:</span><span class="sxs-lookup"><span data-stu-id="423c7-135">Perform the following steps to check for the presence of the application and to delete it, if the application exists:</span></span>

1. <span data-ttu-id="423c7-136">Přejděte do **portálu Azure Classic** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).</span><span class="sxs-lookup"><span data-stu-id="423c7-136">Navigate to the **Azure classic portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).</span></span>
2. <span data-ttu-id="423c7-137">V levém podokně vyberte uzel **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="423c7-137">Select the **Active Directory** node on the left pane.</span></span>
3. <span data-ttu-id="423c7-138">Vyberte klienta Azure AD (adresář), pro kterého chcete povolit službu Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-138">Select the Azure AD tenant (directory) for which you would like to enable Azure AD Domain Services.</span></span>
4. <span data-ttu-id="423c7-139">Přejděte na **aplikace** kartě.</span><span class="sxs-lookup"><span data-stu-id="423c7-139">Navigate to the **Applications** tab.</span></span>
5. <span data-ttu-id="423c7-140">Vyberte **aplikace Moje společnost vlastní** možnost v rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="423c7-140">Select the **Applications my company owns** option in the dropdown.</span></span>
6. <span data-ttu-id="423c7-141">Zkontrolujte aplikaci s názvem **synchronizace Azure AD Domain Services**.</span><span class="sxs-lookup"><span data-stu-id="423c7-141">Check for an application called **Azure AD Domain Services Sync**.</span></span> <span data-ttu-id="423c7-142">Pokud existuje aplikace, pokračujte ho odstranit.</span><span class="sxs-lookup"><span data-stu-id="423c7-142">If the application exists, proceed to delete it.</span></span>
7. <span data-ttu-id="423c7-143">Jakmile jste odstranili aplikace, zkuste znovu povolit Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-143">Once you have deleted the application, try to enable Azure AD Domain Services once again.</span></span>

### <a name="invalid-configuration"></a><span data-ttu-id="423c7-144">Neplatná konfigurace</span><span class="sxs-lookup"><span data-stu-id="423c7-144">Invalid configuration</span></span>
<span data-ttu-id="423c7-145">**Chybová zpráva:**</span><span class="sxs-lookup"><span data-stu-id="423c7-145">**Error message:**</span></span>

<span data-ttu-id="423c7-146">*Domain Services nelze povolit v tomto tenantovi Azure AD. Aplikace Domain Services ve vašem tenantovi Azure AD nemá požadovaná oprávnění k povolení Domain Services. Odstraňte aplikaci s identifikátorem aplikace d87dcbc6-a371-462e-88e3-28ad15ec4e64 a potom se pokuste pro vašeho tenanta Azure AD povolit Domain Services.*</span><span class="sxs-lookup"><span data-stu-id="423c7-146">*Domain Services could not be enabled in this Azure AD tenant. The Domain Services application in your Azure AD tenant does not have the required permissions to enable Domain Services. Delete the application with the application identifier d87dcbc6-a371-462e-88e3-28ad15ec4e64 and then try to enable Domain Services for your Azure AD tenant.*</span></span>

<span data-ttu-id="423c7-147">**Náprava:**</span><span class="sxs-lookup"><span data-stu-id="423c7-147">**Remediation:**</span></span>

<span data-ttu-id="423c7-148">Zkontrolujte, zda jsou aplikace s názvem 'AzureActiveDirectoryDomainControllerServices' (s identifikátorem aplikace d87dcbc6-a371-462e-88e3-28ad15ec4e64) v adresáři služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="423c7-148">Check to see if you have an application with the name 'AzureActiveDirectoryDomainControllerServices' (with an application identifier of d87dcbc6-a371-462e-88e3-28ad15ec4e64) in your Azure AD directory.</span></span> <span data-ttu-id="423c7-149">Pokud tuto aplikaci existuje, musíte ji odstranit a pak opětovného povolení služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-149">If this application exists, you need to delete it and then re-enable Azure AD Domain Services.</span></span>

<span data-ttu-id="423c7-150">Pomocí následujícího skriptu prostředí PowerShell můžete najít aplikaci a odstranit.</span><span class="sxs-lookup"><span data-stu-id="423c7-150">Use the following PowerShell script to find the application and delete it.</span></span>

> [!NOTE]
> <span data-ttu-id="423c7-151">Tento skript používá **Azure AD PowerShell verze 2** rutiny.</span><span class="sxs-lookup"><span data-stu-id="423c7-151">This script uses **Azure AD PowerShell version 2** cmdlets.</span></span> <span data-ttu-id="423c7-152">Úplný seznam všech dostupných rutin a stáhnout modul, přečtěte si [AzureAD PowerShell referenční dokumentaci k nástroji](https://msdn.microsoft.com/library/azure/mt757189.aspx).</span><span class="sxs-lookup"><span data-stu-id="423c7-152">For a full list of all available cmdlets and to download the module, read the [AzureAD PowerShell reference documentation](https://msdn.microsoft.com/library/azure/mt757189.aspx).</span></span>
>
>

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a><span data-ttu-id="423c7-153">Microsoft Graph zakázáno</span><span class="sxs-lookup"><span data-stu-id="423c7-153">Microsoft Graph disabled</span></span>
<span data-ttu-id="423c7-154">**Chybová zpráva:**</span><span class="sxs-lookup"><span data-stu-id="423c7-154">**Error message:**</span></span>

<span data-ttu-id="423c7-155">Domain Services nelze povolit v klientovi Azure AD.</span><span class="sxs-lookup"><span data-stu-id="423c7-155">Domain Services could not be enabled in this Azure AD tenant.</span></span> <span data-ttu-id="423c7-156">Aplikace Microsoft Azure AD je ve vašem tenantovi Azure AD zakázaná.</span><span class="sxs-lookup"><span data-stu-id="423c7-156">The Microsoft Azure AD application is disabled in your Azure AD tenant.</span></span> <span data-ttu-id="423c7-157">Povolit aplikaci s 00000002-0000-0000-c000-000000000000 identifikátor aplikace a zkuste povolit pro vašeho tenanta Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-157">Enable the application with the application identifier 00000002-0000-0000-c000-000000000000 and then try to enable Domain Services for your Azure AD tenant.</span></span>

<span data-ttu-id="423c7-158">**Náprava:**</span><span class="sxs-lookup"><span data-stu-id="423c7-158">**Remediation:**</span></span>

<span data-ttu-id="423c7-159">Zkontrolujte, zda jste zakázali aplikaci s identifikátorem 00000002-0000-0000-c000-000000000000.</span><span class="sxs-lookup"><span data-stu-id="423c7-159">Check to see if you have disabled an application with the identifier 00000002-0000-0000-c000-000000000000.</span></span> <span data-ttu-id="423c7-160">Tato aplikace je aplikace Microsoft Azure AD a poskytuje rozhraní Graph API přístup ke klientovi Azure AD.</span><span class="sxs-lookup"><span data-stu-id="423c7-160">This application is the Microsoft Azure AD application and provides Graph API access to your Azure AD tenant.</span></span> <span data-ttu-id="423c7-161">Služba Azure AD Domain Services musí být povolena synchronizace klientovi Azure AD k vaší spravované domény v této aplikaci.</span><span class="sxs-lookup"><span data-stu-id="423c7-161">Azure AD Domain Services needs this application to be enabled to synchronize your Azure AD tenant to your managed domain.</span></span>

<span data-ttu-id="423c7-162">Tuto chybu vyřešit, povolte tuto aplikaci a zkuste povolit pro vašeho tenanta Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-162">To resolve this error, enable this application and then try to enable Domain Services for your Azure AD tenant.</span></span>

## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="423c7-163">Uživatelé se nemůžou přihlásit ke spravované doméně Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="423c7-163">Users are unable to sign in to the Azure AD Domain Services managed domain</span></span>
<span data-ttu-id="423c7-164">Pokud jeden nebo více uživatelů v klientovi služby Azure AD nejde se přihlásit k nově vytvořený spravované doméně, proveďte následující kroky řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="423c7-164">If one or more users in your Azure AD tenant are unable to sign in to the newly created managed domain, perform the following troubleshooting steps:</span></span>

* <span data-ttu-id="423c7-165">**Přihlášení pomocí formátu UPN:** zkuste se přihlásit pomocí formátu UPN (například "joeuser@contoso.com') namísto SAMAccountName formátu (CONTOSO\joeuser).</span><span class="sxs-lookup"><span data-stu-id="423c7-165">**Sign-in using UPN format:** Try to sign in using the UPN format (for example, 'joeuser@contoso.com') instead of the SAMAccountName format ('CONTOSO\joeuser').</span></span> <span data-ttu-id="423c7-166">SAMAccountName mohou být automaticky generovány pro uživatele, jehož předpona názvu UPN je příliš dlouhý, nebo je stejný jako jiný uživatel na spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="423c7-166">The SAMAccountName may be automatically generated for users whose UPN prefix is overly long or is the same as another user on the managed domain.</span></span> <span data-ttu-id="423c7-167">Formát UPN je musí být jedinečný v rámci klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="423c7-167">The UPN format is guaranteed to be unique within an Azure AD tenant.</span></span>

> [!NOTE]
> <span data-ttu-id="423c7-168">Doporučujeme použít formát UPN pro přihlášení k spravované doméně služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-168">We recommend using the UPN format to sign in to the Azure AD Domain Services managed domain.</span></span>
>
>

* <span data-ttu-id="423c7-169">Zkontrolujte, že jste [povolili synchronizaci hesel](active-directory-ds-getting-started-password-sync.md) podle kroků uvedených v příručce Začínáme.</span><span class="sxs-lookup"><span data-stu-id="423c7-169">Ensure that you have [enabled password synchronization](active-directory-ds-getting-started-password-sync.md) in accordance with the steps outlined in the Getting Started guide.</span></span>
* <span data-ttu-id="423c7-170">**Externí účty:** Ujistěte se, že ovlivněných uživatelský účet není externího účtu v klientovi Azure AD.</span><span class="sxs-lookup"><span data-stu-id="423c7-170">**External accounts:** Ensure that the affected user account is not an external account in the Azure AD tenant.</span></span> <span data-ttu-id="423c7-171">Příklady externí účty jsou účty Microsoft (například "joe@live.com') nebo uživatelské účty z externího adresář Azure AD.</span><span class="sxs-lookup"><span data-stu-id="423c7-171">Examples of external accounts include Microsoft accounts (for example, 'joe@live.com') or user accounts from an external Azure AD directory.</span></span> <span data-ttu-id="423c7-172">Vzhledem k tomu, že Azure AD Domain Services nemá oprávnění pro tyto uživatelské účty, tito uživatelé nemůže přihlásit k spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="423c7-172">Since Azure AD Domain Services does not have credentials for such user accounts, these users cannot sign in to the managed domain.</span></span>
* <span data-ttu-id="423c7-173">**Synchronizovat účty:** Pokud účty ovlivněného uživatele jsou synchronizované z místního adresáře, ověřte, že:</span><span class="sxs-lookup"><span data-stu-id="423c7-173">**Synced accounts:** If the affected user accounts are synchronized from an on-premises directory, verify that:</span></span>

  * <span data-ttu-id="423c7-174">Nasazení nebo aktualizovat, aby [doporučujeme nejnovější verzi služby Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594).</span><span class="sxs-lookup"><span data-stu-id="423c7-174">You have deployed or updated to the [latest recommended release of Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594).</span></span>
  * <span data-ttu-id="423c7-175">Nakonfigurování Azure AD Connect na [provést úplnou synchronizaci](active-directory-ds-getting-started-password-sync.md).</span><span class="sxs-lookup"><span data-stu-id="423c7-175">You have configured Azure AD Connect to [perform a full synchronization](active-directory-ds-getting-started-password-sync.md).</span></span>
  * <span data-ttu-id="423c7-176">V závislosti na velikosti adresáře může chvíli trvat, než pro uživatelské účty a přihlašovací údaje, hodnoty hash být k dispozici v Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-176">Depending on the size of your directory, it may take a while for user accounts and credential hashes to be available in Azure AD Domain Services.</span></span> <span data-ttu-id="423c7-177">Zajistěte, že abyste počkali dost dlouho, než se budete pokoušet ověřování (v závislosti na velikosti adresáře - pár hodin za den nebo dvě pro velké adresáře).</span><span class="sxs-lookup"><span data-stu-id="423c7-177">Ensure you wait long enough before retrying authentication (depending on the size of your directory - a few hours to a day or two for large directories).</span></span>
  * <span data-ttu-id="423c7-178">Pokud potíže potrvají po ověření předchozích kroků, zkuste restartovat službu Microsoft Azure AD Sync.</span><span class="sxs-lookup"><span data-stu-id="423c7-178">If the issue persists after verifying the preceding steps, try restarting the Microsoft Azure AD Sync Service.</span></span> <span data-ttu-id="423c7-179">Z počítače synchronizace spustí příkazový řádek a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="423c7-179">From your sync machine, launch a command prompt and execute the following commands:</span></span>

    1. <span data-ttu-id="423c7-180">net stop "Microsoft Azure AD Sync.</span><span class="sxs-lookup"><span data-stu-id="423c7-180">net stop 'Microsoft Azure AD Sync'</span></span>
    2. <span data-ttu-id="423c7-181">net start "Microsoft Azure AD Sync.</span><span class="sxs-lookup"><span data-stu-id="423c7-181">net start 'Microsoft Azure AD Sync'</span></span>
* <span data-ttu-id="423c7-182">**Jen cloudové účty**: Pokud ovlivněných uživatelský účet účet uživatele jenom pro cloud, zajistěte, že uživatel došlo ke změně hesla po povolení služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-182">**Cloud-only accounts**: If the affected user account is a cloud-only user account, ensure that the user has changed their password after you enabled Azure AD Domain Services.</span></span> <span data-ttu-id="423c7-183">Tento krok způsobí, že se vygenerují hodnoty hash přihlašovacích údajů potřebné pro Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="423c7-183">This step causes the credential hashes required for Azure AD Domain Services to be generated.</span></span>

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a><span data-ttu-id="423c7-184">Uživatelé odebrána z vašeho klienta Azure AD nejsou odebrány z vaší spravované domény</span><span class="sxs-lookup"><span data-stu-id="423c7-184">Users removed from your Azure AD tenant are not removed from your managed domain</span></span>
<span data-ttu-id="423c7-185">Azure AD vás chrání před náhodným odstraněním objektů uživatelů.</span><span class="sxs-lookup"><span data-stu-id="423c7-185">Azure AD protects you from accidental deletion of user objects.</span></span> <span data-ttu-id="423c7-186">Když odstraníte uživatelský účet z vašeho tenanta Azure AD, odpovídající objekt uživatele se přesune do složky Koš.</span><span class="sxs-lookup"><span data-stu-id="423c7-186">When you delete a user account from your Azure AD tenant, the corresponding user object is moved to the Recycle Bin.</span></span> <span data-ttu-id="423c7-187">Pokud tuto operace odstranění se synchronizuje s vaší spravované domény, budou odpovídající uživatelskému účtu označeno jako zakázané.</span><span class="sxs-lookup"><span data-stu-id="423c7-187">When this delete operation is synchronized to your managed domain, it causes the corresponding user account to be marked disabled.</span></span> <span data-ttu-id="423c7-188">Tato funkce vám pomůže obnovit nebo zrušení odstranění uživatelský účet později.</span><span class="sxs-lookup"><span data-stu-id="423c7-188">This feature helps you recover or undelete the user account later.</span></span>

<span data-ttu-id="423c7-189">Uživatelský účet zůstane v zakázaném stavu ve vaší spravované domény, i když je znovu vytvořit uživatelský účet s stejný hlavní název uživatele v adresáři služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="423c7-189">The user account remains in the disabled state in your managed domain, even if you re-create a user account with the same UPN in your Azure AD directory.</span></span> <span data-ttu-id="423c7-190">Odebrat uživatelský účet z vaší spravované domény, budete muset vynutit, odstraňte jej z vašeho klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="423c7-190">To remove the user account from your managed domain, you need to force delete it from your Azure AD tenant.</span></span>

<span data-ttu-id="423c7-191">Chcete-li odebrat uživatelský účet plně z vaší spravované domény, trvale odstraňte uživatele z vašeho klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="423c7-191">To remove the user account fully from your managed domain, delete the user permanently from your Azure AD tenant.</span></span> <span data-ttu-id="423c7-192">Použijte rutinu PowerShellu Remove-MsolUser s možností -RemoveFromRecycleBin, jak to popisuje tento [článek MSDN](https://msdn.microsoft.com/library/azure/dn194132.aspx).</span><span class="sxs-lookup"><span data-stu-id="423c7-192">Use the Remove-MsolUser PowerShell cmdlet with the '-RemoveFromRecycleBin' option, as described in this [MSDN article](https://msdn.microsoft.com/library/azure/dn194132.aspx).</span></span>

## <a name="contact-us"></a><span data-ttu-id="423c7-193">Kontaktujte nás</span><span class="sxs-lookup"><span data-stu-id="423c7-193">Contact Us</span></span>
<span data-ttu-id="423c7-194">Obraťte se na produktový tým Azure Active Directory Domain Services na [sdílet zpětnou vazbu nebo pro podporu](active-directory-ds-contact-us.md).</span><span class="sxs-lookup"><span data-stu-id="423c7-194">Contact the Azure Active Directory Domain Services product team to [share feedback or for support](active-directory-ds-contact-us.md).</span></span>
