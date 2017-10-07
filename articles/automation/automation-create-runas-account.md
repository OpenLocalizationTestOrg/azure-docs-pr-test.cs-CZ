---
title: "aaaCreate Azure Automation účty spustit jako | Microsoft Docs"
description: "Tento článek popisuje, jak tooupdate vaše automatizace účtu a vytvoření účtů spustit jako pomocí prostředí PowerShell nebo z portálu hello."
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
ms.date: 07/27/2017
ms.author: magoedte
ms.openlocfilehash: 94eb54fa0b518056a726d17146c63411e248273b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-your-automation-account-authentication-with-run-as-accounts"></a><span data-ttu-id="5da0d-103">Aktualizace ověřování účtu Automation o účty Spustit jako</span><span class="sxs-lookup"><span data-stu-id="5da0d-103">Update your Automation account authentication with Run As accounts</span></span> 
<span data-ttu-id="5da0d-104">Můžete aktualizovat svůj existující účet Automation z hello portálu nebo pomocí prostředí PowerShell, pokud:</span><span class="sxs-lookup"><span data-stu-id="5da0d-104">You can update your existing Automation account from hello portal or use PowerShell if:</span></span>

* <span data-ttu-id="5da0d-105">Vytvoření účtu Automation, ale odmítnout toocreate hello účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="5da0d-105">You create an Automation account but decline toocreate hello Run As account.</span></span>
* <span data-ttu-id="5da0d-106">Už používáte prostředky Resource Manager toomanage účet Automation a chcete tooupdate hello tooinclude hello spustit jako účet pro ověřování sady runbook.</span><span class="sxs-lookup"><span data-stu-id="5da0d-106">You already use an Automation account toomanage Resource Manager resources and you want tooupdate hello account tooinclude hello Run As account for runbook authentication.</span></span>
* <span data-ttu-id="5da0d-107">Už používáte toomanage účet Automation pro klasické prostředky a chcete, aby tooupdate ho toouse hello Classic spustit jako účet, místo vytvoření nového účtu a migrace tooit vaše runbooky a prostředky.</span><span class="sxs-lookup"><span data-stu-id="5da0d-107">You already use an Automation account toomanage classic resources and you want tooupdate it toouse hello Classic Run As account instead of creating a new account and migrating your runbooks and assets tooit.</span></span>   
* <span data-ttu-id="5da0d-108">Chcete toocreate spustit jako a Classic spustit jako účet pomocí certifikát vydaný certifikační autoritou (CA) rozlehlé sítě.</span><span class="sxs-lookup"><span data-stu-id="5da0d-108">You want toocreate a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5da0d-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5da0d-109">Prerequisites</span></span>

* <span data-ttu-id="5da0d-110">skript Hello lze spustit pouze v systému Windows 10 a Windows Server 2016 s moduly Azure Resource Manager 3.0.0 a novější.</span><span class="sxs-lookup"><span data-stu-id="5da0d-110">hello script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 3.0.0 and later.</span></span> <span data-ttu-id="5da0d-111">V předchozích verzích Windows není podporován.</span><span class="sxs-lookup"><span data-stu-id="5da0d-111">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="5da0d-112">Azure PowerShell 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5da0d-112">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="5da0d-113">Informace o verzi hello PowerShell 1.0 najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="5da0d-113">For information about hello PowerShell 1.0 release, see [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span>
* <span data-ttu-id="5da0d-114">Účet služby Automation, který je odkazováno jako hodnota hello hello *-AutomationAccountName* a *- ApplicationDisplayName* parametry v hello následující skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5da0d-114">An Automation account, which is referenced as hello value for hello *–AutomationAccountName* and *-ApplicationDisplayName* parameters in hello following PowerShell script.</span></span>

<span data-ttu-id="5da0d-115">tooget hello hodnoty pro *SubscriptionID*, *ResourceGroup*, a *AutomationAccountName*, které jsou požadované parametry pro skript hello, hello následující:</span><span class="sxs-lookup"><span data-stu-id="5da0d-115">tooget hello values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for hello script, do hello following:</span></span>

1. <span data-ttu-id="5da0d-116">V hello portálu Azure, vyberte svůj účet Automation na hello **účet Automation** a pak vyberte **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="5da0d-116">In hello Azure portal, select your Automation account on hello **Automation account** blade, and then select **All settings**.</span></span>  
2. <span data-ttu-id="5da0d-117">Na hello **všechna nastavení** okno, v části **nastavení účtu**, vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="5da0d-117">On hello **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="5da0d-118">Všimněte si hodnot hello hello **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="5da0d-118">Note hello values on hello **Properties** blade.</span></span><br><br> <span data-ttu-id="5da0d-119">![okno "Vlastnosti" účet Automation Hello](media/automation-create-runas-account/automation-account-properties.png)</span><span class="sxs-lookup"><span data-stu-id="5da0d-119">![hello Automation account "Properties" blade](media/automation-create-runas-account/automation-account-properties.png)</span></span>  

### <a name="required-permissions-tooupdate-your-automation-account"></a><span data-ttu-id="5da0d-120">Požadované oprávnění tooupdate účtu Automation</span><span class="sxs-lookup"><span data-stu-id="5da0d-120">Required permissions tooupdate your Automation account</span></span>
<span data-ttu-id="5da0d-121">tooupdate účet Automation, musí mít hello následující konkrétní oprávnění a vyžadují oprávnění toocomplete v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="5da0d-121">tooupdate an Automation account, you must have hello following specific privileges and permissions required toocomplete this topic.</span></span>   
 
* <span data-ttu-id="5da0d-122">Účtu uživatele AD musí toobe přidané tooa role s role Přispěvatel toohello ekvivalentní oprávnění pro Microsoft.Automation prostředky, jak je uvedeno v článku [řízení přístupu na základě Role ve službě Azure Automation](automation-role-based-access-control.md#contributor-role-permissions).</span><span class="sxs-lookup"><span data-stu-id="5da0d-122">Your AD user account needs toobe added tooa role with permissions equivalent toohello Contributor role for Microsoft.Automation resources as outlined in article [Role-based access control in Azure Automation](automation-role-based-access-control.md#contributor-role-permissions).</span></span>  
* <span data-ttu-id="5da0d-123">Uživatelé bez oprávnění správce v klientovi služby Azure AD mohou [registraci aplikací AD](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions) pokud registrace aplikace hello nastavení je nastaven příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="5da0d-123">Non-admin users in your Azure AD tenant can [register AD applications](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions) if hello App registrations setting is set too**Yes**.</span></span>  <span data-ttu-id="5da0d-124">Pokud registrace aplikace hello nastavení je nastaven příliš**ne**, uživatel hello provedení této akce musí být globálním správcem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5da0d-124">If hello app registrations setting is set too**No**, hello user performing this action must be a global administrator in Azure AD.</span></span> 

<span data-ttu-id="5da0d-125">Pokud si nejste členem instanci Active Directory hello předplatné, předtím, než jsou přidány toohello globální správce nebo řidičská-administrator role hello předplatné, se přidají tooActive Directory jako Host.</span><span class="sxs-lookup"><span data-stu-id="5da0d-125">If you are not a member of hello subscription’s Active Directory instance before you are added toohello global administrator/co-administrator role of hello subscription, you are added tooActive Directory as a guest.</span></span> <span data-ttu-id="5da0d-126">V takovém případě se zobrazí "Nemáte oprávnění toocreate..."</span><span class="sxs-lookup"><span data-stu-id="5da0d-126">In this situation, you receive a “You do not have permissions toocreate…”</span></span> <span data-ttu-id="5da0d-127">upozornění na hello **přidat účet Automation** okno.</span><span class="sxs-lookup"><span data-stu-id="5da0d-127">warning on hello **Add Automation Account** blade.</span></span> <span data-ttu-id="5da0d-128">Uživatelé, kteří byly přidány toohello globální správce nebo řidičská-administrator role nejprve lze odebrat z instance služby Active Directory hello předplatného a znova se přidal toomake je úplné uživatelské ve službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5da0d-128">Users who were added toohello global administrator/co-administrator role first can be removed from hello subscription's Active Directory instance and re-added toomake them a full User in Active Directory.</span></span> <span data-ttu-id="5da0d-129">tooverify této situaci se z hello **Azure Active Directory** podokně hello portál Azure, vyberte **uživatelů a skupin**, vyberte **všichni uživatelé** a po výběru hello konkrétního uživatele, vyberte **profil**.</span><span class="sxs-lookup"><span data-stu-id="5da0d-129">tooverify this situation, from hello **Azure Active Directory** pane in hello Azure portal, select **Users and groups**, select **All users** and, after you select hello specific user, select **Profile**.</span></span> <span data-ttu-id="5da0d-130">Hello hodnotu hello **typ uživatele** atribut v profilu uživatele hello nesmí rovnat **hosta**.</span><span class="sxs-lookup"><span data-stu-id="5da0d-130">hello value of hello **User type** attribute under hello users profile should not equal **Guest**.</span></span>

## <a name="create-run-as-account-from-hello-portal"></a><span data-ttu-id="5da0d-131">Vytvořte účet Spustit jako z portálu hello</span><span class="sxs-lookup"><span data-stu-id="5da0d-131">Create Run As account from hello portal</span></span>
<span data-ttu-id="5da0d-132">V této části proveďte následující kroky tooupdate hello si účet Azure Automation ze hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5da0d-132">In this section, perform hello following steps tooupdate your Azure Automation account from  hello Azure portal.</span></span>  <span data-ttu-id="5da0d-133">Můžete vytvořit hello účty spustit jako a Classic spustit jako jednotlivě, a pokud nepotřebujete toomanage prostředky na portálu Azure classic hello, můžete vytvořit pouze hello spustit v Azure jako účet.</span><span class="sxs-lookup"><span data-stu-id="5da0d-133">You create hello Run As and Classic Run As accounts individually, and if you don't need toomanage resources in hello classic Azure portal, you can just create hello Azure Run As account.</span></span>  

<span data-ttu-id="5da0d-134">proces Hello vytvoří hello následující položky v účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="5da0d-134">hello process creates hello following items in your Automation account.</span></span>

<span data-ttu-id="5da0d-135">**Pro účty Spustit jako:**</span><span class="sxs-lookup"><span data-stu-id="5da0d-135">**For Run As accounts:**</span></span>

* <span data-ttu-id="5da0d-136">Vytvoří aplikaci Azure AD se certifikát podepsaný svým držitelem, vytvoří hlavní účet služby pro aplikaci hello ve službě Azure AD a přiřadí role Přispěvatel hello hello účtu v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="5da0d-136">Creates an Azure AD application with a self-signed certificate, creates a service principal account for hello application in Azure AD, and assigns hello Contributor role for hello account in your current subscription.</span></span> <span data-ttu-id="5da0d-137">Můžete změnit tato nastavení tooOwner nebo jakákoli jiná role.</span><span class="sxs-lookup"><span data-stu-id="5da0d-137">You can change this setting tooOwner or any other role.</span></span> <span data-ttu-id="5da0d-138">Další informace najdete v tématu [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="5da0d-138">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="5da0d-139">Vytvoří prostředek certifikátu automatizace s názvem *AzureRunAsCertificate* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="5da0d-139">Creates an Automation certificate asset named *AzureRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="5da0d-140">asset certifikátu Hello obsahuje hello privátní klíč certifikátu používaného aplikací hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5da0d-140">hello certificate asset holds hello certificate private key that's used by hello Azure AD application.</span></span>
* <span data-ttu-id="5da0d-141">Vytvoří prostředek připojení automatizace s názvem *AzureRunAsConnection* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="5da0d-141">Creates an Automation connection asset named *AzureRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="5da0d-142">asset připojení Hello obsahuje hello applicationId, tenantId, subscriptionId a kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="5da0d-142">hello connection asset holds hello applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="5da0d-143">**Pro účet Spustit jako pro Classic:**</span><span class="sxs-lookup"><span data-stu-id="5da0d-143">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="5da0d-144">Vytvoří prostředek certifikátu automatizace s názvem *AzureClassicRunAsCertificate* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="5da0d-144">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="5da0d-145">asset Hello certifikát obsahuje privátní klíč pro hello certifikátu používá certifikát pro správu hello.</span><span class="sxs-lookup"><span data-stu-id="5da0d-145">hello certificate asset holds hello certificate private key used by hello management certificate.</span></span>
* <span data-ttu-id="5da0d-146">Vytvoří prostředek připojení automatizace s názvem *AzureClassicRunAsConnection* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="5da0d-146">Creates an Automation connection asset named *AzureClassicRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="5da0d-147">asset připojení Hello obsahuje název odběru hello, subscriptionId a název certifikátu asset.</span><span class="sxs-lookup"><span data-stu-id="5da0d-147">hello connection asset holds hello subscription name, subscriptionId, and certificate asset name.</span></span>

1. <span data-ttu-id="5da0d-148">Přihlaste se toohello portálu Azure pomocí účtu, který je členem role Správci předplatného hello a spolusprávce předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="5da0d-148">Sign in toohello Azure portal with an account that is a member of hello Subscription Admins role and co-administrator of hello subscription.</span></span>
2. <span data-ttu-id="5da0d-149">V okně účtu Automation hello, vyberte **účty spustit jako** části hello **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="5da0d-149">From hello Automation account blade, select **Run As Accounts** under hello section **Account Settings**.</span></span>  
3. <span data-ttu-id="5da0d-150">Podle toho, který účet požadujete, vyberte buď **Účet Spustit jako pro Azure**, nebo **Účet Spustit jako pro Azure Classic**.</span><span class="sxs-lookup"><span data-stu-id="5da0d-150">Depending on which account you require, select either **Azure Run As Account** or **Azure Classic Run As Account**.</span></span>  <span data-ttu-id="5da0d-151">Po výběru buď hello **přidat spustit v Azure jako** nebo **přidat Azure Classic účet Spustit jako** okno se zobrazí a po zkontrolování hello souhrnné informace, klikněte na tlačítko **vytvořit** tooproceed s vytváření účtu spustit jako.</span><span class="sxs-lookup"><span data-stu-id="5da0d-151">After selecting either hello **Add Azure Run As** or **Add Azure Classic Run As Account** blade appears and after reviewing hello overview information, click **Create** tooproceed with Run As account creation.</span></span>  
4. <span data-ttu-id="5da0d-152">Zatímco Azure vytváří účet Spustit jako hello, můžete sledovat průběh hello pod **oznámení** z hello nabídky a banner se zobrazí s informacemi o tom Probíhá vytváření účtu hello.</span><span class="sxs-lookup"><span data-stu-id="5da0d-152">While Azure creates hello Run As account, you can track hello progress under **Notifications** from hello menu and a banner is displayed stating hello account is being created.</span></span>  <span data-ttu-id="5da0d-153">Tento proces může trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="5da0d-153">This process can take a few minutes toocomplete.</span></span>  

## <a name="create-run-as-account-using-powershell-script"></a><span data-ttu-id="5da0d-154">Vytvoření účtu Spustit jako pomocí skriptu PowerShellu</span><span class="sxs-lookup"><span data-stu-id="5da0d-154">Create Run As account using PowerShell script</span></span>
<span data-ttu-id="5da0d-155">Tento skript prostředí PowerShell zahrnuje podporu pro hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="5da0d-155">This PowerShell script includes support for hello following configurations:</span></span>

* <span data-ttu-id="5da0d-156">Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="5da0d-156">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="5da0d-157">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="5da0d-157">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="5da0d-158">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu</span><span class="sxs-lookup"><span data-stu-id="5da0d-158">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="5da0d-159">Vytvořte účet Spustit jako a Classic spustit jako účet pomocí certifikát podepsaný svým držitelem v hello cloudu Azure Government.</span><span class="sxs-lookup"><span data-stu-id="5da0d-159">Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud.</span></span>

<span data-ttu-id="5da0d-160">V závislosti na hello možnost konfigurace, kterou vyberete vytvoří skript hello hello následující položky.</span><span class="sxs-lookup"><span data-stu-id="5da0d-160">Depending on hello configuration option you select, hello script creates hello following items.</span></span>

<span data-ttu-id="5da0d-161">**Pro účty Spustit jako:**</span><span class="sxs-lookup"><span data-stu-id="5da0d-161">**For Run As accounts:**</span></span>

* <span data-ttu-id="5da0d-162">Vytvoří Azure AD application toobe exportovaný s buď hello podepsaného svým držitelem nebo enterprise veřejný klíč certifikátu, vytvoří hlavní účet služby pro aplikaci hello ve službě Azure AD a hello přiřadí role Přispěvatel pro účet hello ve vaší stávající předplatné.</span><span class="sxs-lookup"><span data-stu-id="5da0d-162">Creates an Azure AD application toobe exported with either hello self-signed or enterprise certificate public key, creates a service principal account for hello application in Azure AD, and assigns hello Contributor role for hello account in your current subscription.</span></span> <span data-ttu-id="5da0d-163">Můžete změnit tato nastavení tooOwner nebo jakákoli jiná role.</span><span class="sxs-lookup"><span data-stu-id="5da0d-163">You can change this setting tooOwner or any other role.</span></span> <span data-ttu-id="5da0d-164">Další informace najdete v tématu [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="5da0d-164">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="5da0d-165">Vytvoří prostředek certifikátu automatizace s názvem *AzureRunAsCertificate* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="5da0d-165">Creates an Automation certificate asset named *AzureRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="5da0d-166">asset certifikátu Hello obsahuje hello privátní klíč certifikátu používaného aplikací hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5da0d-166">hello certificate asset holds hello certificate private key that's used by hello Azure AD application.</span></span>
* <span data-ttu-id="5da0d-167">Vytvoří prostředek připojení automatizace s názvem *AzureRunAsConnection* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="5da0d-167">Creates an Automation connection asset named *AzureRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="5da0d-168">asset připojení Hello obsahuje hello applicationId, tenantId, subscriptionId a kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="5da0d-168">hello connection asset holds hello applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="5da0d-169">**Pro účet Spustit jako pro Classic:**</span><span class="sxs-lookup"><span data-stu-id="5da0d-169">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="5da0d-170">Vytvoří prostředek certifikátu automatizace s názvem *AzureClassicRunAsCertificate* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="5da0d-170">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="5da0d-171">asset Hello certifikát obsahuje privátní klíč pro hello certifikátu používá certifikát pro správu hello.</span><span class="sxs-lookup"><span data-stu-id="5da0d-171">hello certificate asset holds hello certificate private key used by hello management certificate.</span></span>
* <span data-ttu-id="5da0d-172">Vytvoří prostředek připojení automatizace s názvem *AzureClassicRunAsConnection* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="5da0d-172">Creates an Automation connection asset named *AzureClassicRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="5da0d-173">asset připojení Hello obsahuje název odběru hello, subscriptionId a název certifikátu asset.</span><span class="sxs-lookup"><span data-stu-id="5da0d-173">hello connection asset holds hello subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="5da0d-174">Pokud vyberete jednu z možností pro vytvoření účtu Classic spustit jako, po hello skript se spustí, nahrávání hello veřejné správy toohello (přípona názvu souboru .cer) úložiště certifikátů pro předplatné hello této hello účet Automation byla vytvořena v.</span><span class="sxs-lookup"><span data-stu-id="5da0d-174">If you select either option for creating a Classic Run As account, after hello script is executed, upload hello public certificate (.cer file name extension) toohello management store for hello subscription that hello Automation account was created in.</span></span>
> 

1. <span data-ttu-id="5da0d-175">Uložte následující skript v počítači hello.</span><span class="sxs-lookup"><span data-stu-id="5da0d-175">Save hello following script on your computer.</span></span> <span data-ttu-id="5da0d-176">V tomto příkladu ho uložte pod názvem hello *New-RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="5da0d-176">In this example, save it with hello filename *New-RunAsAccount.ps1*.</span></span>

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
        $KeyCredential.EndDate = Get-Date $PfxCert.GetExpirationDateString()
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
        if (!(($AzureRMProfileVersion.Major -ge 3 -and $AzureRMProfileVersion.Minor -ge 0) -or ($AzureRMProfileVersion.Major -gt 3)))
        {
            Write-Error -Message "Please install hello latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
            return
        }

        Login-AzureRmAccount -Environment $EnvironmentName 
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
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. <span data-ttu-id="5da0d-177">V počítači, spusťte **prostředí Windows PowerShell** z hello **spustit** obrazovky se zvýšenými uživatelskými právy.</span><span class="sxs-lookup"><span data-stu-id="5da0d-177">On your computer, start **Windows PowerShell** from hello **Start** screen with elevated user rights.</span></span>
3. <span data-ttu-id="5da0d-178">Z hello se zvýšenými oprávněními prostředí příkazového řádku, přejděte toohello složku, která obsahuje hello skript, který jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="5da0d-178">From hello elevated command-line shell, go toohello folder that contains hello script you created in step 1.</span></span>  
4. <span data-ttu-id="5da0d-179">Spusťte skript hello pomocí hodnoty parametrů hello hello konfigurace, kterou požadujete.</span><span class="sxs-lookup"><span data-stu-id="5da0d-179">Execute hello script by using hello parameter values for hello configuration you require.</span></span>

    <span data-ttu-id="5da0d-180">**Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem**</span><span class="sxs-lookup"><span data-stu-id="5da0d-180">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="5da0d-181">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem**</span><span class="sxs-lookup"><span data-stu-id="5da0d-181">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="5da0d-182">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu**</span><span class="sxs-lookup"><span data-stu-id="5da0d-182">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="5da0d-183">**Vytvořit účet Spustit jako a Classic spustit jako účet pomocí certifikát podepsaný svým držitelem v hello cloudu Azure Government.**</span><span class="sxs-lookup"><span data-stu-id="5da0d-183">**Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="5da0d-184">Po provedení hello skript bude výzvami tooauthenticate s Azure.</span><span class="sxs-lookup"><span data-stu-id="5da0d-184">After hello script has executed, you will be prompted tooauthenticate with Azure.</span></span> <span data-ttu-id="5da0d-185">Přihlaste se pomocí účtu, který je členem role Správci předplatného hello a spolusprávce předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="5da0d-185">Sign in with an account that is a member of hello subscription administrators role and co-administrator of hello subscription.</span></span>
    >
    >

<span data-ttu-id="5da0d-186">Po hello skript byl úspěšně proveden, vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="5da0d-186">After hello script has executed successfully, note hello following:</span></span>
* <span data-ttu-id="5da0d-187">Pokud jste vytvořili účet Classic spustit jako s certifikát podepsaný svým držitelem veřejné (soubor .cer), skript hello vytvoří a uloží jej toohello dočasné soubory složky v počítači v rámci profilu uživatele hello *%USERPROFILE%\AppData\Local\Temp*, který používá relaci prostředí PowerShell tooexecute hello.</span><span class="sxs-lookup"><span data-stu-id="5da0d-187">If you created a Classic Run As account with a self-signed public certificate (.cer file), hello script creates and saves it toohello temporary files folder on your computer under hello user profile *%USERPROFILE%\AppData\Local\Temp*, which you used tooexecute hello PowerShell session.</span></span>
* <span data-ttu-id="5da0d-188">Pokud jste vytvořili účet Spustit jako pro Classic s využitím podnikového veřejného certifikátu (soubor .cer), použijte tento certifikát.</span><span class="sxs-lookup"><span data-stu-id="5da0d-188">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="5da0d-189">Postupujte podle pokynů hello pro [odesílání toohello certifikátu rozhraní API pro správu portálu Azure classic](../azure-api-management-certs.md)a potom ověřit konfiguraci hello přihlašovacích údajů s prostředky nasazení classic pomocí hello [ukázkový kód tooauthenticate s prostředky Azure Classic nasazení](automation-verify-runas-authentication.md#classic-run-as-authentication).</span><span class="sxs-lookup"><span data-stu-id="5da0d-189">Follow hello instructions for [uploading a management API certificate toohello Azure classic portal](../azure-api-management-certs.md), and then validate hello credential configuration with classic deployment resources by using hello [sample code tooauthenticate with Azure Classic Deployment Resources](automation-verify-runas-authentication.md#classic-run-as-authentication).</span></span> 
* <span data-ttu-id="5da0d-190">Pokud jste to udělali *není* vytvořit účet Classic spustit jako, ověření pomocí prostředků Resource Manageru a ověřit konfiguraci hello přihlašovacích údajů pomocí hello [ukázkový kód pro ověřování pomocí služby správy prostředky](automation-verify-runas-authentication.md#automation-run-as-authentication).</span><span class="sxs-lookup"><span data-stu-id="5da0d-190">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate hello credential configuration by using hello [sample code for authenticating with Service Management resources](automation-verify-runas-authentication.md#automation-run-as-authentication).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5da0d-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5da0d-191">Next steps</span></span>
* <span data-ttu-id="5da0d-192">Další informace o objektech služby najdete v části příliš[objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="5da0d-192">For more information about Service Principals, refer too[Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span>
* <span data-ttu-id="5da0d-193">Další informace o certifikátech a službám Azure, najdete v části příliš[Přehled certifikátů pro Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="5da0d-193">For more information about certificates and Azure services, refer too[Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span></span>
