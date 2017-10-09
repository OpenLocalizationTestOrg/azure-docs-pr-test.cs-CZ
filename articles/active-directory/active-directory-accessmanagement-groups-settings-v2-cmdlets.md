---
title: "Příklady aaaPowerShell pro správu skupin v Azure Active Directory | Microsoft Docs"
description: "Tato stránka obsahuje prostředí PowerShell příklady toohelp Správa skupin v Azure Active Directory"
keywords: "Azure AD, Azure Active Directory, prostředí PowerShell, správu skupin, skupiny"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7a5023dc-2727-4c25-8254-b531fc3244ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: rodejo
ms.openlocfilehash: ba049babc436e99a290f20899b3a87bcfa811d9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a>Verze 2 rutiny služby Azure Active Directory pro správu skupin
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [Portál Azure Classic](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Tento článek obsahuje příklady, jak toouse prostředí PowerShell toomanage skupin v Azure Active Directory (Azure AD).  Zde také zjistíte, jak tooget nastavit pomocí modulu Azure AD PowerShell hello. Nejdřív musíte [stáhnout modul Azure AD PowerShell hello](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="installing-hello-azure-ad-powershell-module"></a>Instalace modulu Azure AD PowerShell hello
tooinstall hello modul Azure AD PowerShell, použijte hello následující příkazy:

    PS C:\Windows\system32> install-module azuread

byl nainstalován tooverify, který hello modulu, použijte následující příkaz hello:

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

Teď můžete začít používat hello rutiny v modulu hello. Úplný popis hello rutiny v modulu hello Azure AD, naleznete toohello online referenční dokumentaci k nástroji pro [Azure Active Directory PowerShell verze 2](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="connecting-toohello-directory"></a>Připojení adresáře toohello
Předtím, než můžete začít spravovat skupiny pomocí rutin prostředí Azure AD PowerShell, je nutné připojit adresáře toohello relace prostředí PowerShell chcete toomanage. Hello použijte následující příkaz:

    PS C:\Windows\system32> Connect-AzureAD

Hello vyzve vás rutina pro hello přihlašovací údaje, které chcete toouse tooaccess adresáře. V tomto příkladu používáme karen@drumkit.onmicrosoft.com tooaccess hello ukázkový adresáře. Hello rutina vrátí relaci potvrzení tooshow hello byl úspěšně připojen tooyour directory:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Teď můžete začít používat hello AzureAD rutiny toomanage skupin ve vašem adresáři.

## <a name="retrieving-groups"></a>Načítání skupin
hello tooretrieve stávajících skupin z adresáře, které můžete použít rutinu Get-AzureADGroups. tooretrieve všechny skupiny v adresáři hello, použijte rutinu hello bez parametrů:

    PS C:\Windows\system32> get-azureadgroup

Hello rutina vrátí všechny skupiny v hello připojený adresář.

Můžete použít tooretrieve parametr - objectID hello konkrétní skupinu, pro který určíte objectID hello skupiny:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

rutina Hello nyní vrátí hello skupiny, jejichž objectID odpovídá hodnotě hello hello parametru, kterou jste zadali:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Můžete vyhledat konkrétní skupinu pomocí hello - parametr filtru. Tento parametr přebere klauzuli filtru ODATA a vrátí všechny skupiny, které odpovídají filtru hello jako hello následující ukázka:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

> [!NOTE] 
> Hello rutiny prostředí AzureAD PowerShell implementovat hello standardní dotazu OData. Další informace najdete v tématu **$filter** v [možností dotazu systému OData pomocí koncový bod OData hello](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Vytváření skupin
toocreate novou skupinu v adresáři, použijte rutinu New-AzureADGroup hello. Tato rutina vytvoří novou skupinu zabezpečení s názvem "Marketing":

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Aktualizace skupiny
tooupdate existující skupiny, použijte rutinu Set-AzureADGroup hello. V tomto příkladu jsme změnit hello vlastnost DisplayName hello skupiny "Správci Intune." Nejdřív jsme se hledání hello skupiny pomocí rutiny Get-AzureADGroup hello a filtrovat pomocí atributu DisplayName hello:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

V dalším kroku jsme změnit hello popis vlastnost toohello nová hodnota "Správci Intune zařízení":

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Teď Pokud nám najít skupinu hello vidíme vlastnost Popis hello je aktualizovaný tooreflect hello nová hodnota:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Odstranění skupiny
toodelete skupiny z adresáře, použijte rutinu Remove-AzureADGroup hello následujícím způsobem:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Správa členů skupin
Pokud potřebujete tooadd nové členy tooa skupiny, použijte rutinu hello AzureADGroupMember přidat. Tento příkaz přidá skupinu Správci Intune toohello člen, které jsme použili v předchozím příkladu hello:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Hello - ObjectId parametr je hello ObjectID z hello skupiny toowhich chceme tooadd členem, a hello - RefObjectId je hello ObjectID hello uživatele chceme tooadd jako skupina toohello člen.

tooget hello stávající členy skupiny, použijte rutinu Get-AzureADGroupMember hello, jako v následujícím příkladu:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

tooremove hello člen jsme dříve přidán toohello skupiny, použijte rutinu Remove-AzureADGroupMember hello, jak je znázorněno zde:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

tooverify hello skupiny membership(s) uživatele, použijte rutinu vyberte AzureADGroupIdsUserIsMemberOf hello. Tato rutina provede jako jeho parametry hello ObjectId hello uživatele, pro které toocheck hello členství ve skupinách a seznam skupin, pro která toocheck hello členství. Hello seznam skupin musí být zadaný v podobě hello komplexní proměnné typu "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck", proto jsme nejprve musí vytvořit proměnnou typu:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

V dalším kroku jsme zadejte hodnoty pro identifikátory skupiny toocheck hello v atributu hello "Identifikátory skupiny" Tato proměnná komplexní:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Nyní pokud chceme toocheck hello členství ve skupinách uživatele s 72cd4bbd-2594-40a2-935c-016f3cfeeeea ObjectID pro skupiny hello v $g, bychom měli použít:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Vrácená hodnota Hello je seznam skupin, které je tento uživatel členem. Můžete taky použít tuto metodu toocheck kontakty, skupiny nebo objekty služby členství pro daný seznam skupin, pomocí vyberte-AzureADGroupIdsContactIsMemberOf, vyberte AzureADGroupIdsGroupIsMemberOf nebo Vyberte AzureADGroupIdsServicePrincipalIsMemberOf

## <a name="managing-owners-of-groups"></a>Správa vlastníků skupiny
tooadd vlastníky tooa skupiny, použijte rutinu hello AzureADGroupOwner přidat:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Hello - ObjectId parametr je hello ObjectID z hello skupiny toowhich chceme tooadd vlastníka a hello - RefObjectId je hello ObjectID hello uživatele chceme tooadd jako vlastníka skupiny hello.

tooretrieve hello vlastníků skupiny, použijte rutinu Get-AzureADGroupOwner hello:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Hello rutina vrátí hello seznamu vlastníků pro zadanou skupinu hello:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Pokud chcete tooremove vlastníka ze skupiny, použijte rutinu Remove-AzureADGroupOwner hello:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a>Vyhrazené aliasy 
Při vytváření skupiny určité koncové body umožňují hello koncový uživatel toospecify mailNickname nebo alias toobe používá jako součást hello e-mailová adresa skupiny hello. Skupiny s hello následující aliasy vysoce privilegované e-mailu lze vytvořit pouze globální správce Azure AD. 
  
* zneužití 
* Správce 
* Správce 
* hostmaster 
* majordomo 
* správce pošty 
* kořenové 
* Zabezpečení 
* Zabezpečení 
* Správce protokolu SSL 
* správce webového serveru 

## <a name="next-steps"></a>Další kroky
Můžete najít další dokumentaci k Azure Active Directory PowerShell na [rutiny služby Azure Active Directory](/powershell/azure/install-adv2?view=azureadps-2.0).

* [Správa přístupu tooresources pomocí skupin Azure Active Directory](active-directory-manage-groups.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
