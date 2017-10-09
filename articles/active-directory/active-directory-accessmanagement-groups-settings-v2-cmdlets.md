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
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a><span data-ttu-id="0c0a4-104">Verze 2 rutiny služby Azure Active Directory pro správu skupin</span><span class="sxs-lookup"><span data-stu-id="0c0a4-104">Azure Active Directory version 2 cmdlets for group management</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0c0a4-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0c0a4-105">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="0c0a4-106">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="0c0a4-106">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="0c0a4-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0c0a4-107">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="0c0a4-108">Tento článek obsahuje příklady, jak toouse prostředí PowerShell toomanage skupin v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0c0a4-108">This article contains examples of how toouse PowerShell toomanage your groups in Azure Active Directory (Azure AD).</span></span>  <span data-ttu-id="0c0a4-109">Zde také zjistíte, jak tooget nastavit pomocí modulu Azure AD PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-109">It also tells you how tooget set up with hello Azure AD PowerShell module.</span></span> <span data-ttu-id="0c0a4-110">Nejdřív musíte [stáhnout modul Azure AD PowerShell hello](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="0c0a4-110">First, you must [download hello Azure AD PowerShell module](https://www.powershellgallery.com/packages/AzureAD/).</span></span>

## <a name="installing-hello-azure-ad-powershell-module"></a><span data-ttu-id="0c0a4-111">Instalace modulu Azure AD PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="0c0a4-111">Installing hello Azure AD PowerShell module</span></span>
<span data-ttu-id="0c0a4-112">tooinstall hello modul Azure AD PowerShell, použijte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-112">tooinstall hello Azure AD PowerShell module, use hello following commands:</span></span>

    PS C:\Windows\system32> install-module azuread

<span data-ttu-id="0c0a4-113">byl nainstalován tooverify, který hello modulu, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-113">tooverify that hello module was installed, use hello following command:</span></span>

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

<span data-ttu-id="0c0a4-114">Teď můžete začít používat hello rutiny v modulu hello.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-114">Now you can start using hello cmdlets in hello module.</span></span> <span data-ttu-id="0c0a4-115">Úplný popis hello rutiny v modulu hello Azure AD, naleznete toohello online referenční dokumentaci k nástroji pro [Azure Active Directory PowerShell verze 2](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="0c0a4-115">For a full description of hello cmdlets in hello Azure AD module, please refer toohello online reference documentation for [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

## <a name="connecting-toohello-directory"></a><span data-ttu-id="0c0a4-116">Připojení adresáře toohello</span><span class="sxs-lookup"><span data-stu-id="0c0a4-116">Connecting toohello directory</span></span>
<span data-ttu-id="0c0a4-117">Předtím, než můžete začít spravovat skupiny pomocí rutin prostředí Azure AD PowerShell, je nutné připojit adresáře toohello relace prostředí PowerShell chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-117">Before you can start managing groups using Azure AD PowerShell cmdlets, you must connect your PowerShell session toohello directory you want toomanage.</span></span> <span data-ttu-id="0c0a4-118">Hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-118">Use hello following command:</span></span>

    PS C:\Windows\system32> Connect-AzureAD

<span data-ttu-id="0c0a4-119">Hello vyzve vás rutina pro hello přihlašovací údaje, které chcete toouse tooaccess adresáře.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-119">hello cmdlet prompts you for hello credentials you want toouse tooaccess your directory.</span></span> <span data-ttu-id="0c0a4-120">V tomto příkladu používáme karen@drumkit.onmicrosoft.com tooaccess hello ukázkový adresáře.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-120">In this example, we are using karen@drumkit.onmicrosoft.com tooaccess hello demonstration directory.</span></span> <span data-ttu-id="0c0a4-121">Hello rutina vrátí relaci potvrzení tooshow hello byl úspěšně připojen tooyour directory:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-121">hello cmdlet returns a confirmation tooshow hello session was connected successfully tooyour directory:</span></span>

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

<span data-ttu-id="0c0a4-122">Teď můžete začít používat hello AzureAD rutiny toomanage skupin ve vašem adresáři.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-122">Now you can start using hello AzureAD cmdlets toomanage groups in your directory.</span></span>

## <a name="retrieving-groups"></a><span data-ttu-id="0c0a4-123">Načítání skupin</span><span class="sxs-lookup"><span data-stu-id="0c0a4-123">Retrieving groups</span></span>
<span data-ttu-id="0c0a4-124">hello tooretrieve stávajících skupin z adresáře, které můžete použít rutinu Get-AzureADGroups.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-124">tooretrieve existing groups from your directory you can use hello Get-AzureADGroups cmdlet.</span></span> <span data-ttu-id="0c0a4-125">tooretrieve všechny skupiny v adresáři hello, použijte rutinu hello bez parametrů:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-125">tooretrieve all groups in hello directory, use hello cmdlet without parameters:</span></span>

    PS C:\Windows\system32> get-azureadgroup

<span data-ttu-id="0c0a4-126">Hello rutina vrátí všechny skupiny v hello připojený adresář.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-126">hello cmdlet returns all groups in hello connected directory.</span></span>

<span data-ttu-id="0c0a4-127">Můžete použít tooretrieve parametr - objectID hello konkrétní skupinu, pro který určíte objectID hello skupiny:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-127">You can use hello -objectID parameter tooretrieve a specific group for which you specify hello group’s objectID:</span></span>

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

<span data-ttu-id="0c0a4-128">rutina Hello nyní vrátí hello skupiny, jejichž objectID odpovídá hodnotě hello hello parametru, kterou jste zadali:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-128">hello cmdlet now returns hello group whose objectID matches hello value of hello parameter you entered:</span></span>

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

<span data-ttu-id="0c0a4-129">Můžete vyhledat konkrétní skupinu pomocí hello - parametr filtru.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-129">You can search for a specific group using hello -filter parameter.</span></span> <span data-ttu-id="0c0a4-130">Tento parametr přebere klauzuli filtru ODATA a vrátí všechny skupiny, které odpovídají filtru hello jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-130">This parameter takes an ODATA filter clause and returns all groups that match hello filter, as in hello following example:</span></span>

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
> <span data-ttu-id="0c0a4-131">Hello rutiny prostředí AzureAD PowerShell implementovat hello standardní dotazu OData.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-131">hello AzureAD PowerShell cmdlets implement hello OData query standard.</span></span> <span data-ttu-id="0c0a4-132">Další informace najdete v tématu **$filter** v [možností dotazu systému OData pomocí koncový bod OData hello](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).</span><span class="sxs-lookup"><span data-stu-id="0c0a4-132">For more information, see **$filter** in [OData system query options using hello OData endpoint](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).</span></span>

## <a name="creating-groups"></a><span data-ttu-id="0c0a4-133">Vytváření skupin</span><span class="sxs-lookup"><span data-stu-id="0c0a4-133">Creating groups</span></span>
<span data-ttu-id="0c0a4-134">toocreate novou skupinu v adresáři, použijte rutinu New-AzureADGroup hello.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-134">toocreate a new group in your directory, use hello New-AzureADGroup cmdlet.</span></span> <span data-ttu-id="0c0a4-135">Tato rutina vytvoří novou skupinu zabezpečení s názvem "Marketing":</span><span class="sxs-lookup"><span data-stu-id="0c0a4-135">This cmdlet creates a new security group called “Marketing":</span></span>

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a><span data-ttu-id="0c0a4-136">Aktualizace skupiny</span><span class="sxs-lookup"><span data-stu-id="0c0a4-136">Updating groups</span></span>
<span data-ttu-id="0c0a4-137">tooupdate existující skupiny, použijte rutinu Set-AzureADGroup hello.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-137">tooupdate an existing group, use hello Set-AzureADGroup cmdlet.</span></span> <span data-ttu-id="0c0a4-138">V tomto příkladu jsme změnit hello vlastnost DisplayName hello skupiny "Správci Intune."</span><span class="sxs-lookup"><span data-stu-id="0c0a4-138">In this example, we’re changing hello DisplayName property of hello group “Intune Administrators.”</span></span> <span data-ttu-id="0c0a4-139">Nejdřív jsme se hledání hello skupiny pomocí rutiny Get-AzureADGroup hello a filtrovat pomocí atributu DisplayName hello:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-139">First, we’re finding hello group using hello Get-AzureADGroup cmdlet and filter using hello DisplayName attribute:</span></span>

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

<span data-ttu-id="0c0a4-140">V dalším kroku jsme změnit hello popis vlastnost toohello nová hodnota "Správci Intune zařízení":</span><span class="sxs-lookup"><span data-stu-id="0c0a4-140">Next, we’re changing hello Description property toohello new value “Intune Device Administrators”:</span></span>

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

<span data-ttu-id="0c0a4-141">Teď Pokud nám najít skupinu hello vidíme vlastnost Popis hello je aktualizovaný tooreflect hello nová hodnota:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-141">Now if we find hello group again, we see hello Description property is updated tooreflect hello new value:</span></span>

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

## <a name="deleting-groups"></a><span data-ttu-id="0c0a4-142">Odstranění skupiny</span><span class="sxs-lookup"><span data-stu-id="0c0a4-142">Deleting groups</span></span>
<span data-ttu-id="0c0a4-143">toodelete skupiny z adresáře, použijte rutinu Remove-AzureADGroup hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-143">toodelete groups from your directory, use hello Remove-AzureADGroup cmdlet as follows:</span></span>

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a><span data-ttu-id="0c0a4-144">Správa členů skupin</span><span class="sxs-lookup"><span data-stu-id="0c0a4-144">Managing members of groups</span></span>
<span data-ttu-id="0c0a4-145">Pokud potřebujete tooadd nové členy tooa skupiny, použijte rutinu hello AzureADGroupMember přidat.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-145">If you need tooadd new members tooa group, use hello Add-AzureADGroupMember cmdlet.</span></span> <span data-ttu-id="0c0a4-146">Tento příkaz přidá skupinu Správci Intune toohello člen, které jsme použili v předchozím příkladu hello:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-146">This command adds a member toohello Intune Administrators group we used in hello previous example:</span></span>

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="0c0a4-147">Hello - ObjectId parametr je hello ObjectID z hello skupiny toowhich chceme tooadd členem, a hello - RefObjectId je hello ObjectID hello uživatele chceme tooadd jako skupina toohello člen.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-147">hello -ObjectId parameter is hello ObjectID of hello group toowhich we want tooadd a member, and hello -RefObjectId is hello ObjectID of hello user we want tooadd as a member toohello group.</span></span>

<span data-ttu-id="0c0a4-148">tooget hello stávající členy skupiny, použijte rutinu Get-AzureADGroupMember hello, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-148">tooget hello existing members of a group, use hello Get-AzureADGroupMember cmdlet, as in this example:</span></span>

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

<span data-ttu-id="0c0a4-149">tooremove hello člen jsme dříve přidán toohello skupiny, použijte rutinu Remove-AzureADGroupMember hello, jak je znázorněno zde:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-149">tooremove hello member we previously added toohello group, use hello Remove-AzureADGroupMember cmdlet, as is shown here:</span></span>

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="0c0a4-150">tooverify hello skupiny membership(s) uživatele, použijte rutinu vyberte AzureADGroupIdsUserIsMemberOf hello.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-150">tooverify hello group membership(s) of a user, use hello Select-AzureADGroupIdsUserIsMemberOf cmdlet.</span></span> <span data-ttu-id="0c0a4-151">Tato rutina provede jako jeho parametry hello ObjectId hello uživatele, pro které toocheck hello členství ve skupinách a seznam skupin, pro která toocheck hello členství.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-151">This cmdlet takes as its parameters hello ObjectId of hello user for which toocheck hello group memberships, and a list of groups for which toocheck hello memberships.</span></span> <span data-ttu-id="0c0a4-152">Hello seznam skupin musí být zadaný v podobě hello komplexní proměnné typu "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck", proto jsme nejprve musí vytvořit proměnnou typu:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-152">hello list of groups must be provided in hello form of a complex variable of type “Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck”, so we first must create a variable with that type:</span></span>

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

<span data-ttu-id="0c0a4-153">V dalším kroku jsme zadejte hodnoty pro identifikátory skupiny toocheck hello v atributu hello "Identifikátory skupiny" Tato proměnná komplexní:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-153">Next, we provide values for hello groupIds toocheck in hello attribute “GroupIds” of this complex variable:</span></span>

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

<span data-ttu-id="0c0a4-154">Nyní pokud chceme toocheck hello členství ve skupinách uživatele s 72cd4bbd-2594-40a2-935c-016f3cfeeeea ObjectID pro skupiny hello v $g, bychom měli použít:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-154">Now, if we want toocheck hello group memberships of a user with ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea against hello groups in $g, we should use:</span></span>

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


<span data-ttu-id="0c0a4-155">Vrácená hodnota Hello je seznam skupin, které je tento uživatel členem.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-155">hello value returned is a list of groups of which this user is a member.</span></span> <span data-ttu-id="0c0a4-156">Můžete taky použít tuto metodu toocheck kontakty, skupiny nebo objekty služby členství pro daný seznam skupin, pomocí vyberte-AzureADGroupIdsContactIsMemberOf, vyberte AzureADGroupIdsGroupIsMemberOf nebo Vyberte AzureADGroupIdsServicePrincipalIsMemberOf</span><span class="sxs-lookup"><span data-stu-id="0c0a4-156">You can also apply this method toocheck Contacts, Groups or Service Principals membership for a given list of groups, using Select-AzureADGroupIdsContactIsMemberOf, Select-AzureADGroupIdsGroupIsMemberOf or Select-AzureADGroupIdsServicePrincipalIsMemberOf</span></span>

## <a name="managing-owners-of-groups"></a><span data-ttu-id="0c0a4-157">Správa vlastníků skupiny</span><span class="sxs-lookup"><span data-stu-id="0c0a4-157">Managing owners of groups</span></span>
<span data-ttu-id="0c0a4-158">tooadd vlastníky tooa skupiny, použijte rutinu hello AzureADGroupOwner přidat:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-158">tooadd owners tooa group, use hello Add-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="0c0a4-159">Hello - ObjectId parametr je hello ObjectID z hello skupiny toowhich chceme tooadd vlastníka a hello - RefObjectId je hello ObjectID hello uživatele chceme tooadd jako vlastníka skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-159">hello -ObjectId parameter is hello ObjectID of hello group toowhich we want tooadd an owner, and hello -RefObjectId is hello ObjectID of hello user we want tooadd as an owner of hello group.</span></span>

<span data-ttu-id="0c0a4-160">tooretrieve hello vlastníků skupiny, použijte rutinu Get-AzureADGroupOwner hello:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-160">tooretrieve hello owners of a group, use hello Get-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

<span data-ttu-id="0c0a4-161">Hello rutina vrátí hello seznamu vlastníků pro zadanou skupinu hello:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-161">hello cmdlet returns hello list of owners for hello specified group:</span></span>

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

<span data-ttu-id="0c0a4-162">Pokud chcete tooremove vlastníka ze skupiny, použijte rutinu Remove-AzureADGroupOwner hello:</span><span class="sxs-lookup"><span data-stu-id="0c0a4-162">If you want tooremove an owner from a group, use hello Remove-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a><span data-ttu-id="0c0a4-163">Vyhrazené aliasy</span><span class="sxs-lookup"><span data-stu-id="0c0a4-163">Reserved aliases</span></span> 
<span data-ttu-id="0c0a4-164">Při vytváření skupiny určité koncové body umožňují hello koncový uživatel toospecify mailNickname nebo alias toobe používá jako součást hello e-mailová adresa skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-164">When a group is created, certain endpoints allow hello end user toospecify a mailNickname or alias toobe used as part of hello email address of hello group.</span></span> <span data-ttu-id="0c0a4-165">Skupiny s hello následující aliasy vysoce privilegované e-mailu lze vytvořit pouze globální správce Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0c0a4-165">Groups with hello following highly privileged email aliases can only be created by an Azure AD global administrator.</span></span> 
  
* <span data-ttu-id="0c0a4-166">zneužití</span><span class="sxs-lookup"><span data-stu-id="0c0a4-166">abuse</span></span> 
* <span data-ttu-id="0c0a4-167">Správce</span><span class="sxs-lookup"><span data-stu-id="0c0a4-167">admin</span></span> 
* <span data-ttu-id="0c0a4-168">Správce</span><span class="sxs-lookup"><span data-stu-id="0c0a4-168">administrator</span></span> 
* <span data-ttu-id="0c0a4-169">hostmaster</span><span class="sxs-lookup"><span data-stu-id="0c0a4-169">hostmaster</span></span> 
* <span data-ttu-id="0c0a4-170">majordomo</span><span class="sxs-lookup"><span data-stu-id="0c0a4-170">majordomo</span></span> 
* <span data-ttu-id="0c0a4-171">správce pošty</span><span class="sxs-lookup"><span data-stu-id="0c0a4-171">postmaster</span></span> 
* <span data-ttu-id="0c0a4-172">kořenové</span><span class="sxs-lookup"><span data-stu-id="0c0a4-172">root</span></span> 
* <span data-ttu-id="0c0a4-173">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="0c0a4-173">secure</span></span> 
* <span data-ttu-id="0c0a4-174">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="0c0a4-174">security</span></span> 
* <span data-ttu-id="0c0a4-175">Správce protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="0c0a4-175">ssl-admin</span></span> 
* <span data-ttu-id="0c0a4-176">správce webového serveru</span><span class="sxs-lookup"><span data-stu-id="0c0a4-176">webmaster</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0c0a4-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0c0a4-177">Next steps</span></span>
<span data-ttu-id="0c0a4-178">Můžete najít další dokumentaci k Azure Active Directory PowerShell na [rutiny služby Azure Active Directory](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="0c0a4-178">You can find more Azure Active Directory PowerShell documentation at [Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

* [<span data-ttu-id="0c0a4-179">Správa přístupu tooresources pomocí skupin Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0c0a4-179">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="0c0a4-180">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0c0a4-180">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
