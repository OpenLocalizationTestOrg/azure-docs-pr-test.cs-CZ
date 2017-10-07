---
title: "nastavení skupiny aaaConfigure pomocí rutiny služby Azure Active Directory | Microsoft Docs"
description: "Jak spravovat hello nastavení pro skupiny pomocí rutiny služby Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 9f2090e6-3af4-4f07-bbb2-1d18dae89b73
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro;
ms.openlocfilehash: 46db49d9dec3eaa41c97ca74c57854189eddc16d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a><span data-ttu-id="ef2d4-103">Rutiny Azure Active Directory pro konfiguraci nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="ef2d4-103">Azure Active Directory cmdlets for configuring group settings</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ef2d4-104">Tento obsah platí pouze tooOffice 365 skupiny.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-104">This content applies only tooOffice 365 groups.</span></span> <span data-ttu-id="ef2d4-105">Další informace o tom, nastavit skupiny zabezpečení uživatelů toocreate tooallow, `Set-MSOLCompanySettings -UsersPermissionToCreateGroupsEnabled $True` jak je popsáno v [Set-MSOLCompanySettings](https://docs.microsoft.com/en-us/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).</span><span class="sxs-lookup"><span data-stu-id="ef2d4-105">For more information on how tooallow users toocreate Security groups, set `Set-MSOLCompanySettings -UsersPermissionToCreateGroupsEnabled $True` as described in [Set-MSOLCompanySettings](https://docs.microsoft.com/en-us/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).</span></span> 

<span data-ttu-id="ef2d4-106">Nastavení skupiny Office 365 se konfigurují pomocí nastavení objektu a objekt SettingsTemplate.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-106">Office 365 Groups settings are configured using a Settings object and a SettingsTemplate object.</span></span> <span data-ttu-id="ef2d4-107">Na začátku nevidíte žádné objekty nastavení v adresáři, protože adresáře je nakonfigurovaný s hello výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-107">Initially, you don't see any Settings objects in your directory, because your directory is configured with hello default settings.</span></span> <span data-ttu-id="ef2d4-108">toochange hello výchozí nastavení, musíte vytvořit nový objekt nastavení pomocí nastavení šablony.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-108">toochange hello default settings, you must create a new settings object using a settings template.</span></span> <span data-ttu-id="ef2d4-109">Nastavení šablony jsou definovány společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-109">Settings templates are defined by Microsoft.</span></span> <span data-ttu-id="ef2d4-110">Existuje několik různých nastavení šablon.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-110">There are several different settings templates.</span></span> <span data-ttu-id="ef2d4-111">nastavení skupiny tooconfigure Office 365 pro váš adresář, použijete šablonu hello s názvem "Group.Unified".</span><span class="sxs-lookup"><span data-stu-id="ef2d4-111">tooconfigure Office 365 group settings for your directory, you use hello template named "Group.Unified".</span></span> <span data-ttu-id="ef2d4-112">nastavení skupiny tooconfigure Office 365 na jednu skupinu, použijte šablonu hello s názvem "Group.Unified.Guest".</span><span class="sxs-lookup"><span data-stu-id="ef2d4-112">tooconfigure Office 365 group settings on a single group, use hello template named "Group.Unified.Guest".</span></span> <span data-ttu-id="ef2d4-113">Tato šablona je použité toomanage hosta přístup tooan Office 365 skupina.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-113">This template is used toomanage guest access tooan Office 365 group.</span></span> 

<span data-ttu-id="ef2d4-114">Hello rutiny jsou součástí modulu hello Azure Active Directory PowerShell V2.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-114">hello cmdlets are part of hello Azure Active Directory PowerShell V2 module.</span></span> <span data-ttu-id="ef2d4-115">Pokyny jak toodownload a nainstalujte modul hello ve vašem počítači, najdete v článku hello [Azure Active Directory PowerShell verze 2](https://docs.microsoft.com/powershell/azuread/).</span><span class="sxs-lookup"><span data-stu-id="ef2d4-115">For instructions how toodownload and install hello module on your computer, see hello article [Azure Active Directory PowerShell Version 2](https://docs.microsoft.com/powershell/azuread/).</span></span> <span data-ttu-id="ef2d4-116">Můžete nainstalovat hello verze 2 verzi modulu hello z [Galerie prostředí PowerShell hello](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="ef2d4-116">You can install hello version 2 release of hello module from [hello PowerShell gallery](https://www.powershellgallery.com/packages/AzureAD/).</span></span>

## <a name="retrieve-a-specific-settings-value"></a><span data-ttu-id="ef2d4-117">Načíst hodnotu konkrétní nastavení</span><span class="sxs-lookup"><span data-stu-id="ef2d4-117">Retrieve a specific settings value</span></span>
<span data-ttu-id="ef2d4-118">Pokud znáte název hello hello nastavení tooretrieve, můžete použít hello pod rutiny tooretrieve hello aktuální nastavení hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-118">If you know hello name of hello setting you want tooretrieve, you can use hello below cmdlet tooretrieve hello current settings value.</span></span> <span data-ttu-id="ef2d4-119">V tomto příkladu jsme se načítání hello hodnoty pro nastavení s názvem "UsageGuidelinesUrl."</span><span class="sxs-lookup"><span data-stu-id="ef2d4-119">In this example, we're retrieving hello value for a setting named "UsageGuidelinesUrl."</span></span> <span data-ttu-id="ef2d4-120">Další informace, že informace o nastavení adresářových služeb a jejich názvy další dolů v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-120">You can read more about directory settings and their names further down in this article.</span></span>

```powershell
(Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
```

## <a name="create-settings-at-hello-directory-level"></a><span data-ttu-id="ef2d4-121">Vytvořit nastavení na úrovni služby directory hello</span><span class="sxs-lookup"><span data-stu-id="ef2d4-121">Create settings at hello directory level</span></span>
<span data-ttu-id="ef2d4-122">Pomocí těchto kroků vytvoříte nastavení na úrovni adresáře, které se vztahují tooall skupiny Office 365 (Unified skupiny) v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-122">These steps create settings at directory level, which apply tooall Office 365 groups (Unified groups) in hello directory.</span></span>

1. <span data-ttu-id="ef2d4-123">V hello DirectorySettings rutiny je nutné zadat ID hello hello SettingsTemplate chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-123">In hello DirectorySettings cmdlets, you must specify hello ID of hello SettingsTemplate you want toouse.</span></span> <span data-ttu-id="ef2d4-124">Pokud toto ID si nejste jisti, tato rutina vrací hello seznam všech nastavení šablon:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-124">If you do not know this ID, this cmdlet returns hello list of all settings templates:</span></span>
  
  ```
  PS C:> Get-AzureADDirectorySettingTemplate
  ```
  <span data-ttu-id="ef2d4-125">Toto volání rutiny vrátí všechny šablony, které jsou k dispozici:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-125">This cmdlet call returns all templates that are available:</span></span>
  
  ```
  Id                                   DisplayName         Description
  --                                   -----------         -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Unified Group
  16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define hello different settings that can be used for hello associ...
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
  ```
2. <span data-ttu-id="ef2d4-126">tooadd adresu URL obecných zásad použití, je třeba nejprve tooget hello SettingsTemplate objekt, který definuje hodnota adresy URL platí hello využití; To znamená hello Group.Unified šablony:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-126">tooadd a usage guideline URL, first you need tooget hello SettingsTemplate object that defines hello usage guideline URL value; that is, hello Group.Unified template:</span></span>
  
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
  ```
3. <span data-ttu-id="ef2d4-127">Dále vytvořte nový objekt nastavení na základě této šablony:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-127">Next, create a new settings object based on that template:</span></span>
  
  ```
  $Setting = $template.CreateDirectorySetting()
  ```  
4. <span data-ttu-id="ef2d4-128">Potom aktualizujte hodnotu platí využití hello:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-128">Then update hello usage guideline value:</span></span>
  
  ```
  $setting["UsageGuidelinesUrl"] = "<https://guideline.com>"
  ```  
5. <span data-ttu-id="ef2d4-129">Nakonec použijte hello nastavení:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-129">Finally, apply hello settings:</span></span>
  
  ```
  New-AzureADDirectorySetting -DirectorySetting $setting
  ```

<span data-ttu-id="ef2d4-130">Po úspěšném dokončení hello rutina vrátí ID hello hello nové nastavení objektu:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-130">Upon successful completion, hello cmdlet returns hello ID of hello new settings object:</span></span>
  ```
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```
<span data-ttu-id="ef2d4-131">Tady jsou definované v hello Group.Unified SettingsTemplate nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-131">Here are hello settings defined in hello Group.Unified SettingsTemplate.</span></span>

| <span data-ttu-id="ef2d4-132">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="ef2d4-132">**Setting**</span></span> | <span data-ttu-id="ef2d4-133">**Popis**</span><span class="sxs-lookup"><span data-stu-id="ef2d4-133">**Description**</span></span> |
| --- | --- |
|  <ul><li><span data-ttu-id="ef2d4-134">EnableGroupCreation</span><span class="sxs-lookup"><span data-stu-id="ef2d4-134">EnableGroupCreation</span></span><li><span data-ttu-id="ef2d4-135">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ef2d4-135">Type: Boolean</span></span><li><span data-ttu-id="ef2d4-136">Výchozí: True</span><span class="sxs-lookup"><span data-stu-id="ef2d4-136">Default: True</span></span> |<span data-ttu-id="ef2d4-137">Hello příznak označující, zda je povoleno vytvoření Unified skupiny v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-137">hello flag indicating whether Unified Group creation is allowed in hello directory.</span></span> |
|  <ul><li><span data-ttu-id="ef2d4-138">GroupCreationAllowedGroupId</span><span class="sxs-lookup"><span data-stu-id="ef2d4-138">GroupCreationAllowedGroupId</span></span><li><span data-ttu-id="ef2d4-139">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="ef2d4-139">Type: String</span></span><li><span data-ttu-id="ef2d4-140">Výchozí hodnota: ""</span><span class="sxs-lookup"><span data-stu-id="ef2d4-140">Default: “”</span></span> |<span data-ttu-id="ef2d4-141">Identifikátor GUID skupiny hello zabezpečení, pro které hello mohou členové skupiny Unified toocreate i v případě EnableGroupCreation hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-141">GUID of hello security group for which hello members are allowed toocreate Unified Groups even when EnableGroupCreation == false.</span></span> |
|  <ul><li><span data-ttu-id="ef2d4-142">UsageGuidelinesUrl</span><span class="sxs-lookup"><span data-stu-id="ef2d4-142">UsageGuidelinesUrl</span></span><li><span data-ttu-id="ef2d4-143">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="ef2d4-143">Type: String</span></span><li><span data-ttu-id="ef2d4-144">Výchozí hodnota: ""</span><span class="sxs-lookup"><span data-stu-id="ef2d4-144">Default: “”</span></span> |<span data-ttu-id="ef2d4-145">Odkaz toohello pokyny týkající se používání skupiny.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-145">A link toohello Group Usage Guidelines.</span></span> |
|  <ul><li><span data-ttu-id="ef2d4-146">ClassificationDescriptions</span><span class="sxs-lookup"><span data-stu-id="ef2d4-146">ClassificationDescriptions</span></span><li><span data-ttu-id="ef2d4-147">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="ef2d4-147">Type: String</span></span><li><span data-ttu-id="ef2d4-148">Výchozí hodnota: ""</span><span class="sxs-lookup"><span data-stu-id="ef2d4-148">Default: “”</span></span> | <span data-ttu-id="ef2d4-149">Čárkami oddělený seznam popisů klasifikace.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-149">A comma-delimited list of classification descriptions.</span></span> |
|  <ul><li><span data-ttu-id="ef2d4-150">DefaultClassification</span><span class="sxs-lookup"><span data-stu-id="ef2d4-150">DefaultClassification</span></span><li><span data-ttu-id="ef2d4-151">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="ef2d4-151">Type: String</span></span><li><span data-ttu-id="ef2d4-152">Výchozí hodnota: ""</span><span class="sxs-lookup"><span data-stu-id="ef2d4-152">Default: “”</span></span> | <span data-ttu-id="ef2d4-153">Hello klasifikace, který je použit jako hello výchozí klasifikace pro skupinu, pokud byl zadán žádný toobe.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-153">hello classification that is toobe used as hello default classification for a group if none was specified.</span></span>|
|  <ul><li><span data-ttu-id="ef2d4-154">PrefixSuffixNamingRequirement</span><span class="sxs-lookup"><span data-stu-id="ef2d4-154">PrefixSuffixNamingRequirement</span></span><li><span data-ttu-id="ef2d4-155">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="ef2d4-155">Type: String</span></span><li><span data-ttu-id="ef2d4-156">Výchozí hodnota: ""</span><span class="sxs-lookup"><span data-stu-id="ef2d4-156">Default: “”</span></span> |<span data-ttu-id="ef2d4-157">Není dosud implementována.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-157">Not implemented yet</span></span>
|  <ul><li><span data-ttu-id="ef2d4-158">AllowGuestsToBeGroupOwner</span><span class="sxs-lookup"><span data-stu-id="ef2d4-158">AllowGuestsToBeGroupOwner</span></span><li><span data-ttu-id="ef2d4-159">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ef2d4-159">Type: Boolean</span></span><li><span data-ttu-id="ef2d4-160">Výchozí: False</span><span class="sxs-lookup"><span data-stu-id="ef2d4-160">Default: False</span></span> | <span data-ttu-id="ef2d4-161">Logická hodnota, která určuje, zda uživatel typu Host může být vlastníkem skupiny.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-161">Boolean indicating whether or not a guest user can be an owner of groups.</span></span> |
|  <ul><li><span data-ttu-id="ef2d4-162">AllowGuestsToAccessGroups</span><span class="sxs-lookup"><span data-stu-id="ef2d4-162">AllowGuestsToAccessGroups</span></span><li><span data-ttu-id="ef2d4-163">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ef2d4-163">Type: Boolean</span></span><li><span data-ttu-id="ef2d4-164">Výchozí: True</span><span class="sxs-lookup"><span data-stu-id="ef2d4-164">Default: True</span></span> | <span data-ttu-id="ef2d4-165">Logická hodnota, která určuje, zda uživatel typu Host může mít obsah tooUnified skupiny přístupu.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-165">Boolean indicating whether or not a guest user can have access tooUnified groups' content.</span></span> |
|  <ul><li><span data-ttu-id="ef2d4-166">GuestUsageGuidelinesUrl</span><span class="sxs-lookup"><span data-stu-id="ef2d4-166">GuestUsageGuidelinesUrl</span></span><li><span data-ttu-id="ef2d4-167">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="ef2d4-167">Type: String</span></span><li><span data-ttu-id="ef2d4-168">Výchozí hodnota: ""</span><span class="sxs-lookup"><span data-stu-id="ef2d4-168">Default: “”</span></span> | <span data-ttu-id="ef2d4-169">Adresa url Hello odkaz toohello hosta použití obecných pokynů.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-169">hello url of a link toohello guest usage guidelines.</span></span> |
|  <ul><li><span data-ttu-id="ef2d4-170">AllowToAddGuests</span><span class="sxs-lookup"><span data-stu-id="ef2d4-170">AllowToAddGuests</span></span><li><span data-ttu-id="ef2d4-171">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ef2d4-171">Type: Boolean</span></span><li><span data-ttu-id="ef2d4-172">Výchozí: True</span><span class="sxs-lookup"><span data-stu-id="ef2d4-172">Default: True</span></span> | <span data-ttu-id="ef2d4-173">Logická hodnota, která určuje, jestli je povolené tooadd hosté toothis adresář.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-173">A boolean indicating whether or not is allowed tooadd guests toothis directory.</span></span>|
|  <ul><li><span data-ttu-id="ef2d4-174">ClassificationList</span><span class="sxs-lookup"><span data-stu-id="ef2d4-174">ClassificationList</span></span><li><span data-ttu-id="ef2d4-175">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="ef2d4-175">Type: String</span></span><li><span data-ttu-id="ef2d4-176">Výchozí hodnota: ""</span><span class="sxs-lookup"><span data-stu-id="ef2d4-176">Default: “”</span></span> |<span data-ttu-id="ef2d4-177">Čárkami oddělený seznam hodnot platný klasifikace, které můžou být použité tooUnified skupiny.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-177">A comma-delimited list of valid classification values that can be applied tooUnified Groups.</span></span> |
|  <ul><li><span data-ttu-id="ef2d4-178">EnableGroupCreation</span><span class="sxs-lookup"><span data-stu-id="ef2d4-178">EnableGroupCreation</span></span><li><span data-ttu-id="ef2d4-179">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ef2d4-179">Type: Boolean</span></span><li><span data-ttu-id="ef2d4-180">Výchozí: True</span><span class="sxs-lookup"><span data-stu-id="ef2d4-180">Default: True</span></span> | <span data-ttu-id="ef2d4-181">Logická hodnota, která určuje, zda uživatelé bez oprávnění správce, můžete vytvořit nové skupiny jednotné.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-181">A boolean indicating whether or not non-admin users can create new Unified groups.</span></span> |


## <a name="read-settings-at-hello-directory-level"></a><span data-ttu-id="ef2d4-182">Číst nastavení na úrovni služby directory hello</span><span class="sxs-lookup"><span data-stu-id="ef2d4-182">Read settings at hello directory level</span></span>
<span data-ttu-id="ef2d4-183">Tyto kroky nastavení na úrovni adresáře, které se vztahují skupiny Office tooall v adresáři hello přečíst.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-183">These steps read settings at directory level, which apply tooall Office groups in hello directory.</span></span>

1. <span data-ttu-id="ef2d4-184">Přečtěte si všechna existující nastavení adresáře:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-184">Read all existing directory settings:</span></span>
  ```
  Get-AzureADDirectorySetting -All $True
  ```
  <span data-ttu-id="ef2d4-185">Tato rutina vrátí seznam všech nastavení adresáře:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-185">This cmdlet returns a list of all directory settings:</span></span>
  ```
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```

2. <span data-ttu-id="ef2d4-186">Přečtěte si všechna nastavení pro konkrétní skupinu:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-186">Read all settings for a specific group:</span></span>
  ```
  Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
  ```

3. <span data-ttu-id="ef2d4-187">Načíst všechny hodnoty nastavení directory nastavení objektu konkrétního adresáře, pomocí Id GUID nastavení:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-187">Read all directory settings values of a specific directory settings object, using Settings Id GUID:</span></span>
  ```
  (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
  ```
  <span data-ttu-id="ef2d4-188">Tato rutina vrací hello názvy a hodnoty v tomto objektu nastavení pro konkrétní skupiny:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-188">This cmdlet returns hello names and values in this settings object for this specific group:</span></span>
  ```
  Name                          Value
  ----                          -----
  ClassificationDescriptions
  DefaultClassification
  PrefixSuffixNamingRequirement
  AllowGuestsToBeGroupOwner     False 
  AllowGuestsToAccessGroups     True
  GuestUsageGuidelinesUrl
  GroupCreationAllowedGroupId
  AllowToAddGuests              True
  UsageGuidelinesUrl            <https://guideline.com>
  ClassificationList
  EnableGroupCreation           True
  ```

## <a name="update-settings-for-a-specific-group"></a><span data-ttu-id="ef2d4-189">Aktualizovat nastavení pro konkrétní skupinu</span><span class="sxs-lookup"><span data-stu-id="ef2d4-189">Update settings for a specific group</span></span>

1. <span data-ttu-id="ef2d4-190">Vyhledejte hello nastavení šablony s názvem "Groups.Unified.Guest"</span><span class="sxs-lookup"><span data-stu-id="ef2d4-190">Search for hello settings template named "Groups.Unified.Guest"</span></span>
  ```
  Get-AzureADDirectorySettingTemplate
  
  Id                                   DisplayName            Description
  --                                   -----------            -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Unified Group
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...
  ```
2. <span data-ttu-id="ef2d4-191">Načtení objektu šablony hello hello Groups.Unified.Guest šablony:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-191">Retrieve hello template object for hello Groups.Unified.Guest template:</span></span>
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
  ```
3. <span data-ttu-id="ef2d4-192">Vytvořte nový objekt nastavení z šablony hello:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-192">Create a new settings object from hello template:</span></span>
  ```
  $Setting = $Template.CreateDirectorySetting()
  ```

4. <span data-ttu-id="ef2d4-193">Nastavte hodnotu toohello požadované nastavení hello:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-193">Set hello setting toohello required value:</span></span>
  ```
  $Setting["AllowToAddGuests"]=$False
  ```
5. <span data-ttu-id="ef2d4-194">Vytvoření nového nastavení hello hello požadovanou skupinu v adresáři hello:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-194">Create hello new setting for hello required group in hello directory:</span></span>
  ```
  New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting
  
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...
  ```

## <a name="update-settings-at-hello-directory-level"></a><span data-ttu-id="ef2d4-195">Aktualizujte nastavení na úrovni služby directory hello</span><span class="sxs-lookup"><span data-stu-id="ef2d4-195">Update settings at hello directory level</span></span>

<span data-ttu-id="ef2d4-196">Tyto kroky aktualizujte nastavení na úrovni služby directory, které se vztahují tooall Unified skupiny v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-196">These steps update settings at directory level, which apply tooall Unified groups in hello directory.</span></span> <span data-ttu-id="ef2d4-197">Těchto příkladech se předpokládá, že již existuje objekt nastavení ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-197">These examples assume there is already a Settings object in your directory.</span></span>

1. <span data-ttu-id="ef2d4-198">Najít hello stávající objekt nastavení:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-198">Find hello existing Settings object:</span></span>
  ```
  Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ
  
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  
  $setting = Get-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323
  ```
2. <span data-ttu-id="ef2d4-199">Aktualizujte hodnotu hello:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-199">Update hello value:</span></span>
  
  ```
  $Setting["AllowToAddGuests"] = "false"
  ```
3. <span data-ttu-id="ef2d4-200">Aktualizace nastavení hello:</span><span class="sxs-lookup"><span data-stu-id="ef2d4-200">Update hello setting:</span></span>
  
  ```
  Set-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323 -DirectorySetting $Setting
  ```

## <a name="remove-settings-at-hello-directory-level"></a><span data-ttu-id="ef2d4-201">Odeberte nastavení na úrovni služby directory hello</span><span class="sxs-lookup"><span data-stu-id="ef2d4-201">Remove settings at hello directory level</span></span>
<span data-ttu-id="ef2d4-202">Tento krok odstraní nastavení na úrovni adresáře, které použít skupiny Office tooall v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="ef2d4-202">This step removes settings at directory level, which apply tooall Office groups in hello directory.</span></span>
  ```
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="cmdlet-syntax-reference"></a><span data-ttu-id="ef2d4-203">Referenční informace o rutinách syntaxe</span><span class="sxs-lookup"><span data-stu-id="ef2d4-203">Cmdlet syntax reference</span></span>
<span data-ttu-id="ef2d4-204">Můžete najít další dokumentaci k Azure Active Directory PowerShell na [rutiny služby Azure Active Directory](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="ef2d4-204">You can find more Azure Active Directory PowerShell documentation at [Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

## <a name="additional-reading"></a><span data-ttu-id="ef2d4-205">Další čtení</span><span class="sxs-lookup"><span data-stu-id="ef2d4-205">Additional reading</span></span>

* [<span data-ttu-id="ef2d4-206">Správa přístupu tooresources pomocí skupin Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ef2d4-206">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="ef2d4-207">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ef2d4-207">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
