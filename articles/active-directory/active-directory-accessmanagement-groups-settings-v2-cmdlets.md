---
title: "Příklady prostředí PowerShell pro správu skupin v Azure Active Directory | Microsoft Docs"
description: "Tato stránka obsahuje příklady prostředí PowerShell, které vám pomohou spravovat vaše skupiny ve službě Azure Active Directory"
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
ms.openlocfilehash: a81820bc778c26f6e8051e2817ebd2b9c24b697a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a><span data-ttu-id="ae35b-104">Verze 2 rutiny služby Azure Active Directory pro správu skupin</span><span class="sxs-lookup"><span data-stu-id="ae35b-104">Azure Active Directory version 2 cmdlets for group management</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ae35b-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ae35b-105">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="ae35b-106">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="ae35b-106">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="ae35b-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ae35b-107">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="ae35b-108">Tento článek obsahuje příklady, jak pomocí prostředí PowerShell ke správě skupin v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ae35b-108">This article contains examples of how to use PowerShell to manage your groups in Azure Active Directory (Azure AD).</span></span>  <span data-ttu-id="ae35b-109">Je také vysvětluje, jak získat nastavit modulu Azure AD PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ae35b-109">It also tells you how to get set up with the Azure AD PowerShell module.</span></span> <span data-ttu-id="ae35b-110">Nejdřív musíte [stáhnout modulu Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="ae35b-110">First, you must [download the Azure AD PowerShell module](https://www.powershellgallery.com/packages/AzureAD/).</span></span>

## <a name="installing-the-azure-ad-powershell-module"></a><span data-ttu-id="ae35b-111">Instalace modulu Azure AD PowerShell</span><span class="sxs-lookup"><span data-stu-id="ae35b-111">Installing the Azure AD PowerShell module</span></span>
<span data-ttu-id="ae35b-112">K instalaci modulu Azure AD PowerShell, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ae35b-112">To install the Azure AD PowerShell module, use the following commands:</span></span>

    PS C:\Windows\system32> install-module azuread

<span data-ttu-id="ae35b-113">Pokud chcete ověřit, zda byl nainstalován modul, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ae35b-113">To verify that the module was installed, use the following command:</span></span>

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

<span data-ttu-id="ae35b-114">Teď můžete začít používat rutiny v modulu.</span><span class="sxs-lookup"><span data-stu-id="ae35b-114">Now you can start using the cmdlets in the module.</span></span> <span data-ttu-id="ae35b-115">Úplný popis rutin v modulu Azure AD, naleznete v dokumentaci online referencí pro [Azure Active Directory PowerShell verze 2](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="ae35b-115">For a full description of the cmdlets in the Azure AD module, please refer to the online reference documentation for [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

## <a name="connecting-to-the-directory"></a><span data-ttu-id="ae35b-116">Připojení k adresáři</span><span class="sxs-lookup"><span data-stu-id="ae35b-116">Connecting to the directory</span></span>
<span data-ttu-id="ae35b-117">Předtím, než můžete začít spravovat skupiny pomocí rutin prostředí Azure AD PowerShell, je nutné připojit relace prostředí PowerShell k adresáři, který chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="ae35b-117">Before you can start managing groups using Azure AD PowerShell cmdlets, you must connect your PowerShell session to the directory you want to manage.</span></span> <span data-ttu-id="ae35b-118">Použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ae35b-118">Use the following command:</span></span>

    PS C:\Windows\system32> Connect-AzureAD

<span data-ttu-id="ae35b-119">Rutina vás vyzve k zadání přihlašovacích údajů, které chcete použít pro přístup k adresáři.</span><span class="sxs-lookup"><span data-stu-id="ae35b-119">The cmdlet prompts you for the credentials you want to use to access your directory.</span></span> <span data-ttu-id="ae35b-120">V tomto příkladu používáme karen@drumkit.onmicrosoft.com pro přístup k adresáři demonstrační.</span><span class="sxs-lookup"><span data-stu-id="ae35b-120">In this example, we are using karen@drumkit.onmicrosoft.com to access the demonstration directory.</span></span> <span data-ttu-id="ae35b-121">Rutina vrátí potvrzení jestli že relace byl úspěšně připojen do vašeho adresáře:</span><span class="sxs-lookup"><span data-stu-id="ae35b-121">The cmdlet returns a confirmation to show the session was connected successfully to your directory:</span></span>

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

<span data-ttu-id="ae35b-122">Teď můžete začít používat rutiny AzureAD ke správě skupin ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="ae35b-122">Now you can start using the AzureAD cmdlets to manage groups in your directory.</span></span>

## <a name="retrieving-groups"></a><span data-ttu-id="ae35b-123">Načítání skupin</span><span class="sxs-lookup"><span data-stu-id="ae35b-123">Retrieving groups</span></span>
<span data-ttu-id="ae35b-124">Načíst existující skupiny z adresáře můžete použít rutinu Get-AzureADGroups.</span><span class="sxs-lookup"><span data-stu-id="ae35b-124">To retrieve existing groups from your directory you can use the Get-AzureADGroups cmdlet.</span></span> <span data-ttu-id="ae35b-125">Pokud chcete načíst všechny skupiny v adresáři, použijte rutinu bez parametrů:</span><span class="sxs-lookup"><span data-stu-id="ae35b-125">To retrieve all groups in the directory, use the cmdlet without parameters:</span></span>

    PS C:\Windows\system32> get-azureadgroup

<span data-ttu-id="ae35b-126">Rutina vrátí všechny skupiny v adresáři připojené.</span><span class="sxs-lookup"><span data-stu-id="ae35b-126">The cmdlet returns all groups in the connected directory.</span></span>

<span data-ttu-id="ae35b-127">Pomocí parametru - objectID načíst konkrétní skupinu, pro který určíte objectID skupiny:</span><span class="sxs-lookup"><span data-stu-id="ae35b-127">You can use the -objectID parameter to retrieve a specific group for which you specify the group’s objectID:</span></span>

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

<span data-ttu-id="ae35b-128">Rutina vrátí teď skupiny, jejichž objectID odpovídá hodnotě parametru, které jste zadali:</span><span class="sxs-lookup"><span data-stu-id="ae35b-128">The cmdlet now returns the group whose objectID matches the value of the parameter you entered:</span></span>

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

<span data-ttu-id="ae35b-129">Můžete vyhledat konkrétní skupinu pomocí parametru - filtru.</span><span class="sxs-lookup"><span data-stu-id="ae35b-129">You can search for a specific group using the -filter parameter.</span></span> <span data-ttu-id="ae35b-130">Tento parametr přebere klauzuli filtru ODATA a vrátí všechny skupiny, které odpovídají filtru, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="ae35b-130">This parameter takes an ODATA filter clause and returns all groups that match the filter, as in the following example:</span></span>

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
> <span data-ttu-id="ae35b-131">Rutiny prostředí AzureAD PowerShell implementovat standard dotazu OData.</span><span class="sxs-lookup"><span data-stu-id="ae35b-131">The AzureAD PowerShell cmdlets implement the OData query standard.</span></span> <span data-ttu-id="ae35b-132">Další informace najdete v tématu **$filter** v [možností dotazu systému OData pomocí koncový bod OData](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).</span><span class="sxs-lookup"><span data-stu-id="ae35b-132">For more information, see **$filter** in [OData system query options using the OData endpoint](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).</span></span>

## <a name="creating-groups"></a><span data-ttu-id="ae35b-133">Vytváření skupin</span><span class="sxs-lookup"><span data-stu-id="ae35b-133">Creating groups</span></span>
<span data-ttu-id="ae35b-134">Pokud chcete vytvořit novou skupinu v adresáři, použijte rutinu New-AzureADGroup.</span><span class="sxs-lookup"><span data-stu-id="ae35b-134">To create a new group in your directory, use the New-AzureADGroup cmdlet.</span></span> <span data-ttu-id="ae35b-135">Tato rutina vytvoří novou skupinu zabezpečení s názvem "Marketing":</span><span class="sxs-lookup"><span data-stu-id="ae35b-135">This cmdlet creates a new security group called “Marketing":</span></span>

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a><span data-ttu-id="ae35b-136">Aktualizace skupiny</span><span class="sxs-lookup"><span data-stu-id="ae35b-136">Updating groups</span></span>
<span data-ttu-id="ae35b-137">Pokud chcete aktualizovat existující skupiny, použijte rutinu Set-AzureADGroup.</span><span class="sxs-lookup"><span data-stu-id="ae35b-137">To update an existing group, use the Set-AzureADGroup cmdlet.</span></span> <span data-ttu-id="ae35b-138">V tomto příkladu jsme změnit vlastnost DisplayName skupiny "Správci Intune."</span><span class="sxs-lookup"><span data-stu-id="ae35b-138">In this example, we’re changing the DisplayName property of the group “Intune Administrators.”</span></span> <span data-ttu-id="ae35b-139">Nejprve se hledání skupiny pomocí rutiny Get-AzureADGroup a filtrovat pomocí atributu DisplayName:</span><span class="sxs-lookup"><span data-stu-id="ae35b-139">First, we’re finding the group using the Get-AzureADGroup cmdlet and filter using the DisplayName attribute:</span></span>

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

<span data-ttu-id="ae35b-140">V dalším kroku jsme změnit vlastnosti Popis na novou hodnotu "Správci Intune zařízení":</span><span class="sxs-lookup"><span data-stu-id="ae35b-140">Next, we’re changing the Description property to the new value “Intune Device Administrators”:</span></span>

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

<span data-ttu-id="ae35b-141">Nyní když jsme najít skupinu vidíme, že je vlastnost Popis aktualizovat tak, aby odrážely novou hodnotu:</span><span class="sxs-lookup"><span data-stu-id="ae35b-141">Now if we find the group again, we see the Description property is updated to reflect the new value:</span></span>

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

## <a name="deleting-groups"></a><span data-ttu-id="ae35b-142">Odstranění skupiny</span><span class="sxs-lookup"><span data-stu-id="ae35b-142">Deleting groups</span></span>
<span data-ttu-id="ae35b-143">Pokud chcete odstranit skupiny z adresáře, použijte rutinu Remove-AzureADGroup následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ae35b-143">To delete groups from your directory, use the Remove-AzureADGroup cmdlet as follows:</span></span>

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a><span data-ttu-id="ae35b-144">Správa členů skupin</span><span class="sxs-lookup"><span data-stu-id="ae35b-144">Managing members of groups</span></span>
<span data-ttu-id="ae35b-145">Pokud potřebujete přidat nové členy do skupiny, použijte rutinu Add-AzureADGroupMember.</span><span class="sxs-lookup"><span data-stu-id="ae35b-145">If you need to add new members to a group, use the Add-AzureADGroupMember cmdlet.</span></span> <span data-ttu-id="ae35b-146">Tento příkaz přidá členem skupiny Intune Administrators, které jsme použili v předchozím příkladu:</span><span class="sxs-lookup"><span data-stu-id="ae35b-146">This command adds a member to the Intune Administrators group we used in the previous example:</span></span>

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="ae35b-147">Parametr - ObjectId ObjectID skupinu, do které jsme chcete člena přidat, a - RefObjectId je ObjectID uživatele, kterého chcete přidat jako člena do skupiny.</span><span class="sxs-lookup"><span data-stu-id="ae35b-147">The -ObjectId parameter is the ObjectID of the group to which we want to add a member, and the -RefObjectId is the ObjectID of the user we want to add as a member to the group.</span></span>

<span data-ttu-id="ae35b-148">Získat stávající členy skupiny, použijte rutinu Get-AzureADGroupMember jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="ae35b-148">To get the existing members of a group, use the Get-AzureADGroupMember cmdlet, as in this example:</span></span>

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

<span data-ttu-id="ae35b-149">Odebrat člen, který jsme dříve přidány do skupiny, použijte rutinu Remove-AzureADGroupMember, jak je znázorněno zde:</span><span class="sxs-lookup"><span data-stu-id="ae35b-149">To remove the member we previously added to the group, use the Remove-AzureADGroupMember cmdlet, as is shown here:</span></span>

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="ae35b-150">Pokud chcete ověřit membership(s) skupiny uživatele, použijte rutinu AzureADGroupIdsUserIsMemberOf vyberte.</span><span class="sxs-lookup"><span data-stu-id="ae35b-150">To verify the group membership(s) of a user, use the Select-AzureADGroupIdsUserIsMemberOf cmdlet.</span></span> <span data-ttu-id="ae35b-151">Tato rutina se používá jako jeho parametry ObjectId uživatele, pro kterého chcete zkontrolovat členství ve skupinách a seznamu skupin, pro které chcete zkontrolovat členství.</span><span class="sxs-lookup"><span data-stu-id="ae35b-151">This cmdlet takes as its parameters the ObjectId of the user for which to check the group memberships, and a list of groups for which to check the memberships.</span></span> <span data-ttu-id="ae35b-152">Seznam skupin musí být zadaný ve formě proměnné komplexního typu "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck", proto jsme nejprve musíte vytvořit proměnné typu:</span><span class="sxs-lookup"><span data-stu-id="ae35b-152">The list of groups must be provided in the form of a complex variable of type “Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck”, so we first must create a variable with that type:</span></span>

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

<span data-ttu-id="ae35b-153">V dalším kroku poskytujeme hodnoty pro identifikátory skupiny do atribut "Identifikátory skupiny" Tato proměnná komplexní se změnami:</span><span class="sxs-lookup"><span data-stu-id="ae35b-153">Next, we provide values for the groupIds to check in the attribute “GroupIds” of this complex variable:</span></span>

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

<span data-ttu-id="ae35b-154">Nyní Pokud nám chcete zkontrolovat členství ve skupinách uživatele s ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea proti skupin v $g, bychom měli použít:</span><span class="sxs-lookup"><span data-stu-id="ae35b-154">Now, if we want to check the group memberships of a user with ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea against the groups in $g, we should use:</span></span>

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


<span data-ttu-id="ae35b-155">Hodnota vrácená je seznam skupin, které je tento uživatel členem.</span><span class="sxs-lookup"><span data-stu-id="ae35b-155">The value returned is a list of groups of which this user is a member.</span></span> <span data-ttu-id="ae35b-156">Můžete taky použít tuto metodu za účelem zkontrolujte kontakty, skupiny nebo objekty služby členství pro daný seznam skupin, pomocí vyberte-AzureADGroupIdsContactIsMemberOf, vyberte AzureADGroupIdsGroupIsMemberOf nebo Vyberte AzureADGroupIdsServicePrincipalIsMemberOf</span><span class="sxs-lookup"><span data-stu-id="ae35b-156">You can also apply this method to check Contacts, Groups or Service Principals membership for a given list of groups, using Select-AzureADGroupIdsContactIsMemberOf, Select-AzureADGroupIdsGroupIsMemberOf or Select-AzureADGroupIdsServicePrincipalIsMemberOf</span></span>

## <a name="managing-owners-of-groups"></a><span data-ttu-id="ae35b-157">Správa vlastníků skupiny</span><span class="sxs-lookup"><span data-stu-id="ae35b-157">Managing owners of groups</span></span>
<span data-ttu-id="ae35b-158">Vlastníci přidat do skupiny, použijte rutinu Add-AzureADGroupOwner:</span><span class="sxs-lookup"><span data-stu-id="ae35b-158">To add owners to a group, use the Add-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="ae35b-159">Parametr - ObjectId je ObjectID skupiny, do které chceme přidat vlastníka a - RefObjectId je ObjectID uživatele, kterého chcete přidat jako vlastníka skupiny.</span><span class="sxs-lookup"><span data-stu-id="ae35b-159">The -ObjectId parameter is the ObjectID of the group to which we want to add an owner, and the -RefObjectId is the ObjectID of the user we want to add as an owner of the group.</span></span>

<span data-ttu-id="ae35b-160">Pro načtení vlastníků skupiny, použijte rutinu Get-AzureADGroupOwner:</span><span class="sxs-lookup"><span data-stu-id="ae35b-160">To retrieve the owners of a group, use the Get-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

<span data-ttu-id="ae35b-161">Rutina vrátí seznam vlastníků pro zadanou skupinu:</span><span class="sxs-lookup"><span data-stu-id="ae35b-161">The cmdlet returns the list of owners for the specified group:</span></span>

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

<span data-ttu-id="ae35b-162">Pokud chcete odebrat vlastníka ze skupiny, použijte rutinu Remove-AzureADGroupOwner:</span><span class="sxs-lookup"><span data-stu-id="ae35b-162">If you want to remove an owner from a group, use the Remove-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a><span data-ttu-id="ae35b-163">Vyhrazené aliasy</span><span class="sxs-lookup"><span data-stu-id="ae35b-163">Reserved aliases</span></span> 
<span data-ttu-id="ae35b-164">Pokud skupinu je vytvořen, určité koncové body umožňují koncovému uživateli umožňují zadat mailNickname nebo alias, který se má použít jako součást e-mailovou adresu skupiny.</span><span class="sxs-lookup"><span data-stu-id="ae35b-164">When a group is created, certain endpoints allow the end user to specify a mailNickname or alias to be used as part of the email address of the group.</span></span> <span data-ttu-id="ae35b-165">Skupiny s následující aliasy vysoce privilegované e-mailu lze vytvořit pouze globální správce Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ae35b-165">Groups with the following highly privileged email aliases can only be created by an Azure AD global administrator.</span></span> 
  
* <span data-ttu-id="ae35b-166">zneužití</span><span class="sxs-lookup"><span data-stu-id="ae35b-166">abuse</span></span> 
* <span data-ttu-id="ae35b-167">Správce</span><span class="sxs-lookup"><span data-stu-id="ae35b-167">admin</span></span> 
* <span data-ttu-id="ae35b-168">Správce</span><span class="sxs-lookup"><span data-stu-id="ae35b-168">administrator</span></span> 
* <span data-ttu-id="ae35b-169">hostmaster</span><span class="sxs-lookup"><span data-stu-id="ae35b-169">hostmaster</span></span> 
* <span data-ttu-id="ae35b-170">majordomo</span><span class="sxs-lookup"><span data-stu-id="ae35b-170">majordomo</span></span> 
* <span data-ttu-id="ae35b-171">správce pošty</span><span class="sxs-lookup"><span data-stu-id="ae35b-171">postmaster</span></span> 
* <span data-ttu-id="ae35b-172">kořenové</span><span class="sxs-lookup"><span data-stu-id="ae35b-172">root</span></span> 
* <span data-ttu-id="ae35b-173">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="ae35b-173">secure</span></span> 
* <span data-ttu-id="ae35b-174">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="ae35b-174">security</span></span> 
* <span data-ttu-id="ae35b-175">Správce protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="ae35b-175">ssl-admin</span></span> 
* <span data-ttu-id="ae35b-176">správce webového serveru</span><span class="sxs-lookup"><span data-stu-id="ae35b-176">webmaster</span></span> 

## <a name="next-steps"></a><span data-ttu-id="ae35b-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ae35b-177">Next steps</span></span>
<span data-ttu-id="ae35b-178">Můžete najít další dokumentaci k Azure Active Directory PowerShell na [rutiny služby Azure Active Directory](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="ae35b-178">You can find more Azure Active Directory PowerShell documentation at [Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

* [<span data-ttu-id="ae35b-179">Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ae35b-179">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="ae35b-180">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ae35b-180">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
