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
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Rutiny Azure Active Directory pro konfiguraci nastavení skupiny

> [!IMPORTANT]
> Tento obsah platí pouze tooOffice 365 skupiny. Další informace o tom, nastavit skupiny zabezpečení uživatelů toocreate tooallow, `Set-MSOLCompanySettings -UsersPermissionToCreateGroupsEnabled $True` jak je popsáno v [Set-MSOLCompanySettings](https://docs.microsoft.com/en-us/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0). 

Nastavení skupiny Office 365 se konfigurují pomocí nastavení objektu a objekt SettingsTemplate. Na začátku nevidíte žádné objekty nastavení v adresáři, protože adresáře je nakonfigurovaný s hello výchozí nastavení. toochange hello výchozí nastavení, musíte vytvořit nový objekt nastavení pomocí nastavení šablony. Nastavení šablony jsou definovány společností Microsoft. Existuje několik různých nastavení šablon. nastavení skupiny tooconfigure Office 365 pro váš adresář, použijete šablonu hello s názvem "Group.Unified". nastavení skupiny tooconfigure Office 365 na jednu skupinu, použijte šablonu hello s názvem "Group.Unified.Guest". Tato šablona je použité toomanage hosta přístup tooan Office 365 skupina. 

Hello rutiny jsou součástí modulu hello Azure Active Directory PowerShell V2. Pokyny jak toodownload a nainstalujte modul hello ve vašem počítači, najdete v článku hello [Azure Active Directory PowerShell verze 2](https://docs.microsoft.com/powershell/azuread/). Můžete nainstalovat hello verze 2 verzi modulu hello z [Galerie prostředí PowerShell hello](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="retrieve-a-specific-settings-value"></a>Načíst hodnotu konkrétní nastavení
Pokud znáte název hello hello nastavení tooretrieve, můžete použít hello pod rutiny tooretrieve hello aktuální nastavení hodnotu. V tomto příkladu jsme se načítání hello hodnoty pro nastavení s názvem "UsageGuidelinesUrl." Další informace, že informace o nastavení adresářových služeb a jejich názvy další dolů v tomto článku.

```powershell
(Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
```

## <a name="create-settings-at-hello-directory-level"></a>Vytvořit nastavení na úrovni služby directory hello
Pomocí těchto kroků vytvoříte nastavení na úrovni adresáře, které se vztahují tooall skupiny Office 365 (Unified skupiny) v adresáři hello.

1. V hello DirectorySettings rutiny je nutné zadat ID hello hello SettingsTemplate chcete toouse. Pokud toto ID si nejste jisti, tato rutina vrací hello seznam všech nastavení šablon:
  
  ```
  PS C:> Get-AzureADDirectorySettingTemplate
  ```
  Toto volání rutiny vrátí všechny šablony, které jsou k dispozici:
  
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
2. tooadd adresu URL obecných zásad použití, je třeba nejprve tooget hello SettingsTemplate objekt, který definuje hodnota adresy URL platí hello využití; To znamená hello Group.Unified šablony:
  
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
  ```
3. Dále vytvořte nový objekt nastavení na základě této šablony:
  
  ```
  $Setting = $template.CreateDirectorySetting()
  ```  
4. Potom aktualizujte hodnotu platí využití hello:
  
  ```
  $setting["UsageGuidelinesUrl"] = "<https://guideline.com>"
  ```  
5. Nakonec použijte hello nastavení:
  
  ```
  New-AzureADDirectorySetting -DirectorySetting $setting
  ```

Po úspěšném dokončení hello rutina vrátí ID hello hello nové nastavení objektu:
  ```
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```
Tady jsou definované v hello Group.Unified SettingsTemplate nastavení hello.

| **Nastavení** | **Popis** |
| --- | --- |
|  <ul><li>EnableGroupCreation<li>Typ: logická hodnota<li>Výchozí: True |Hello příznak označující, zda je povoleno vytvoření Unified skupiny v adresáři hello. |
|  <ul><li>GroupCreationAllowedGroupId<li>Typ: Řetězec<li>Výchozí hodnota: "" |Identifikátor GUID skupiny hello zabezpečení, pro které hello mohou členové skupiny Unified toocreate i v případě EnableGroupCreation hodnotu false. |
|  <ul><li>UsageGuidelinesUrl<li>Typ: Řetězec<li>Výchozí hodnota: "" |Odkaz toohello pokyny týkající se používání skupiny. |
|  <ul><li>ClassificationDescriptions<li>Typ: Řetězec<li>Výchozí hodnota: "" | Čárkami oddělený seznam popisů klasifikace. |
|  <ul><li>DefaultClassification<li>Typ: Řetězec<li>Výchozí hodnota: "" | Hello klasifikace, který je použit jako hello výchozí klasifikace pro skupinu, pokud byl zadán žádný toobe.|
|  <ul><li>PrefixSuffixNamingRequirement<li>Typ: Řetězec<li>Výchozí hodnota: "" |Není dosud implementována.
|  <ul><li>AllowGuestsToBeGroupOwner<li>Typ: logická hodnota<li>Výchozí: False | Logická hodnota, která určuje, zda uživatel typu Host může být vlastníkem skupiny. |
|  <ul><li>AllowGuestsToAccessGroups<li>Typ: logická hodnota<li>Výchozí: True | Logická hodnota, která určuje, zda uživatel typu Host může mít obsah tooUnified skupiny přístupu. |
|  <ul><li>GuestUsageGuidelinesUrl<li>Typ: Řetězec<li>Výchozí hodnota: "" | Adresa url Hello odkaz toohello hosta použití obecných pokynů. |
|  <ul><li>AllowToAddGuests<li>Typ: logická hodnota<li>Výchozí: True | Logická hodnota, která určuje, jestli je povolené tooadd hosté toothis adresář.|
|  <ul><li>ClassificationList<li>Typ: Řetězec<li>Výchozí hodnota: "" |Čárkami oddělený seznam hodnot platný klasifikace, které můžou být použité tooUnified skupiny. |
|  <ul><li>EnableGroupCreation<li>Typ: logická hodnota<li>Výchozí: True | Logická hodnota, která určuje, zda uživatelé bez oprávnění správce, můžete vytvořit nové skupiny jednotné. |


## <a name="read-settings-at-hello-directory-level"></a>Číst nastavení na úrovni služby directory hello
Tyto kroky nastavení na úrovni adresáře, které se vztahují skupiny Office tooall v adresáři hello přečíst.

1. Přečtěte si všechna existující nastavení adresáře:
  ```
  Get-AzureADDirectorySetting -All $True
  ```
  Tato rutina vrátí seznam všech nastavení adresáře:
  ```
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```

2. Přečtěte si všechna nastavení pro konkrétní skupinu:
  ```
  Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
  ```

3. Načíst všechny hodnoty nastavení directory nastavení objektu konkrétního adresáře, pomocí Id GUID nastavení:
  ```
  (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
  ```
  Tato rutina vrací hello názvy a hodnoty v tomto objektu nastavení pro konkrétní skupiny:
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

## <a name="update-settings-for-a-specific-group"></a>Aktualizovat nastavení pro konkrétní skupinu

1. Vyhledejte hello nastavení šablony s názvem "Groups.Unified.Guest"
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
2. Načtení objektu šablony hello hello Groups.Unified.Guest šablony:
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
  ```
3. Vytvořte nový objekt nastavení z šablony hello:
  ```
  $Setting = $Template.CreateDirectorySetting()
  ```

4. Nastavte hodnotu toohello požadované nastavení hello:
  ```
  $Setting["AllowToAddGuests"]=$False
  ```
5. Vytvoření nového nastavení hello hello požadovanou skupinu v adresáři hello:
  ```
  New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting
  
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...
  ```

## <a name="update-settings-at-hello-directory-level"></a>Aktualizujte nastavení na úrovni služby directory hello

Tyto kroky aktualizujte nastavení na úrovni služby directory, které se vztahují tooall Unified skupiny v adresáři hello. Těchto příkladech se předpokládá, že již existuje objekt nastavení ve vašem adresáři.

1. Najít hello stávající objekt nastavení:
  ```
  Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ
  
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  
  $setting = Get-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323
  ```
2. Aktualizujte hodnotu hello:
  
  ```
  $Setting["AllowToAddGuests"] = "false"
  ```
3. Aktualizace nastavení hello:
  
  ```
  Set-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323 -DirectorySetting $Setting
  ```

## <a name="remove-settings-at-hello-directory-level"></a>Odeberte nastavení na úrovni služby directory hello
Tento krok odstraní nastavení na úrovni adresáře, které použít skupiny Office tooall v adresáři hello.
  ```
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="cmdlet-syntax-reference"></a>Referenční informace o rutinách syntaxe
Můžete najít další dokumentaci k Azure Active Directory PowerShell na [rutiny služby Azure Active Directory](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="additional-reading"></a>Další čtení

* [Správa přístupu tooresources pomocí skupin Azure Active Directory](active-directory-manage-groups.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
