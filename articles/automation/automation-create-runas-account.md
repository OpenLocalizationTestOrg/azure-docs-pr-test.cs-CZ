---
title: "Vytváření účtů Azure Automation Spustit jako | Dokumentace Microsoftu"
description: "Tento článek popisuje postup aktualizace účtu Automation a vytváření účtů Spustit jako pomocí PowerShellu nebo z portálu."
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
ms.openlocfilehash: eaf6eb49bbfe4572827fcc101d1f552b48ab91e6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="update-your-automation-account-authentication-with-run-as-accounts"></a><span data-ttu-id="c6cb4-103">Aktualizace ověřování účtu Automation o účty Spustit jako</span><span class="sxs-lookup"><span data-stu-id="c6cb4-103">Update your Automation account authentication with Run As accounts</span></span> 
<span data-ttu-id="c6cb4-104">Existující účet Automation můžete aktualizovat z portálu nebo pomocí PowerShellu, pokud jste postupovali takto:</span><span class="sxs-lookup"><span data-stu-id="c6cb4-104">You can update your existing Automation account from the portal or use PowerShell if:</span></span>

* <span data-ttu-id="c6cb4-105">Vytvořili jste účet Automation, ale odmítli jste vytvoření účtu Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-105">You create an Automation account but decline to create the Run As account.</span></span>
* <span data-ttu-id="c6cb4-106">Už máte účet Automation pro správu prostředků Resource Manageru a chcete ho aktualizovat, aby zahrnoval účet Spustit jako pro ověřování runbooků.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-106">You already use an Automation account to manage Resource Manager resources and you want to update the account to include the Run As account for runbook authentication.</span></span>
* <span data-ttu-id="c6cb4-107">Už máte účet Automation pro správu klasických prostředků a chcete ho aktualizovat, abyste mohli použít účet Spustit jako pro Classic a nemuseli vytvářet nový účet a migrovat na něj runbooky a prostředky.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-107">You already use an Automation account to manage classic resources and you want to update it to use the Classic Run As account instead of creating a new account and migrating your runbooks and assets to it.</span></span>   
* <span data-ttu-id="c6cb4-108">Rozhodli jste se vytvořit účet Spustit jako a účet Spustit jako pro Classic pomocí certifikátu, který vydala vaše podniková certifikační autorita.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-108">You want to create a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6cb4-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c6cb4-109">Prerequisites</span></span>

* <span data-ttu-id="c6cb4-110">Tento skript je možné spustit jenom v systémech Windows 10 a Windows Server 2016 s nainstalovanými moduly Azure Resource Manageru verze 3.0.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-110">The script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 3.0.0 and later.</span></span> <span data-ttu-id="c6cb4-111">V předchozích verzích Windows není podporován.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-111">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="c6cb4-112">Azure PowerShell 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-112">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="c6cb4-113">Informace o vydání PowerShellu 1.0 najdete v článku [Postup instalace a konfigurace Azure PowerShellu](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="c6cb4-113">For information about the PowerShell 1.0 release, see [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span>
* <span data-ttu-id="c6cb4-114">Účet Automation, na který se odkazuje jako na hodnotu parametrů *-AutomationAccountName* a *-ApplicationDisplayName* v následujícím skriptu PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-114">An Automation account, which is referenced as the value for the *–AutomationAccountName* and *-ApplicationDisplayName* parameters in the following PowerShell script.</span></span>

<span data-ttu-id="c6cb4-115">Abyste získali hodnoty pro parametry *SubscriptionID*, *ResourceGroup* a *AutomationAccountName*, které jsou pro skript povinné, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="c6cb4-115">To get the values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for the script, do the following:</span></span>

1. <span data-ttu-id="c6cb4-116">Na webu Azure Portal v okně **Účet Automation** vyberte příslušný účet Automation a potom vyberte **Všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-116">In the Azure portal, select your Automation account on the **Automation account** blade, and then select **All settings**.</span></span>  
2. <span data-ttu-id="c6cb4-117">V okně **Všechna nastavení** v části **Nastavení účtu** vyberte **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-117">On the **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="c6cb4-118">Hodnoty v okně **Vlastnosti** si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-118">Note the values on the **Properties** blade.</span></span><br><br> <span data-ttu-id="c6cb4-119">![Podokno vlastností účtu Automation](media/automation-create-runas-account/automation-account-properties.png)</span><span class="sxs-lookup"><span data-stu-id="c6cb4-119">![The Automation account "Properties" blade](media/automation-create-runas-account/automation-account-properties.png)</span></span>  

### <a name="required-permissions-to-update-your-automation-account"></a><span data-ttu-id="c6cb4-120">Požadovaná oprávnění k aktualizaci účtu Automation</span><span class="sxs-lookup"><span data-stu-id="c6cb4-120">Required permissions to update your Automation account</span></span>
<span data-ttu-id="c6cb4-121">Pokud chcete aktualizovat účet Automation, musíte mít následující specifická oprávnění vyžadovaná k dokončení tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-121">To update an Automation account, you must have the following specific privileges and permissions required to complete this topic.</span></span>   
 
* <span data-ttu-id="c6cb4-122">Váš uživatelský účet AD musí být přidán do role se stejnými oprávněními jako role přispěvatele pro prostředky Microsoft.Automation, jak je uvedeno v článku [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md#contributor-role-permissions).</span><span class="sxs-lookup"><span data-stu-id="c6cb4-122">Your AD user account needs to be added to a role with permissions equivalent to the Contributor role for Microsoft.Automation resources as outlined in article [Role-based access control in Azure Automation](automation-role-based-access-control.md#contributor-role-permissions).</span></span>  
* <span data-ttu-id="c6cb4-123">Uživatelé ve vašem tenantovi Azure AD, kteří nejsou správci, můžou [registrovat aplikace služby AD](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions), pokud je nastavení Registrace aplikací nastaveno na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-123">Non-admin users in your Azure AD tenant can [register AD applications](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions) if the App registrations setting is set to **Yes**.</span></span>  <span data-ttu-id="c6cb4-124">Pokud je nastavení Registrace aplikací nastaveno na **Ne**, uživatel provádějící tuto akci musí být globálním správcem služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-124">If the app registrations setting is set to **No**, the user performing this action must be a global administrator in Azure AD.</span></span> 

<span data-ttu-id="c6cb4-125">Pokud před přidáním do role globálního správce nebo spolusprávce nejste členem instance Active Directory příslušného předplatného, budete do služby Active Directory přidaní jako host.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-125">If you are not a member of the subscription’s Active Directory instance before you are added to the global administrator/co-administrator role of the subscription, you are added to Active Directory as a guest.</span></span> <span data-ttu-id="c6cb4-126">V takové situaci se zobrazí upozornění Nemáte oprávnění k vytvoření...</span><span class="sxs-lookup"><span data-stu-id="c6cb4-126">In this situation, you receive a “You do not have permissions to create…”</span></span> <span data-ttu-id="c6cb4-127">v okně **Přidání účtu Automation**.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-127">warning on the **Add Automation Account** blade.</span></span> <span data-ttu-id="c6cb4-128">Uživatele, kteří byli nejdřív přidaní do role globálního správce nebo spolusprávce, je možné z instance Active Directory předplatného odebrat a potom je znovu přidat – tak se z nich ve službě Active Directory stanou úplní uživatelé.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-128">Users who were added to the global administrator/co-administrator role first can be removed from the subscription's Active Directory instance and re-added to make them a full User in Active Directory.</span></span> <span data-ttu-id="c6cb4-129">Takovou situaci můžete ověřit v podokně **Azure Active Directory** na webu Azure Portal. Vyberte **Uživatelé a skupiny**, potom **Všichni uživatelé** a po výběru konkrétního uživatele vyberte **Profil**.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-129">To verify this situation, from the **Azure Active Directory** pane in the Azure portal, select **Users and groups**, select **All users** and, after you select the specific user, select **Profile**.</span></span> <span data-ttu-id="c6cb4-130">Hodnota atributu **Typ uživatele** v profilu uživatele by neměla být **Host**.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-130">The value of the **User type** attribute under the users profile should not equal **Guest**.</span></span>

## <a name="create-run-as-account-from-the-portal"></a><span data-ttu-id="c6cb4-131">Vytvoření účtu Spustit jako z portálu</span><span class="sxs-lookup"><span data-stu-id="c6cb4-131">Create Run As account from the portal</span></span>
<span data-ttu-id="c6cb4-132">V této části provedete následující kroky a aktualizujete účet Azure Automation z webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-132">In this section, perform the following steps to update your Azure Automation account from  the Azure portal.</span></span>  <span data-ttu-id="c6cb4-133">Účty Spustit jako a Spustit jako pro Azure Classic vytváříte samostatně. Pokud nepotřebujete spravovat prostředky na portálu Azure Classic, stačí vytvořit jenom účet Spustit jako pro Azure.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-133">You create the Run As and Classic Run As accounts individually, and if you don't need to manage resources in the classic Azure portal, you can just create the Azure Run As account.</span></span>  

<span data-ttu-id="c6cb4-134">Tento proces vytvoří ve vašem účtu Automation následující položky.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-134">The process creates the following items in your Automation account.</span></span>

<span data-ttu-id="c6cb4-135">**Pro účty Spustit jako:**</span><span class="sxs-lookup"><span data-stu-id="c6cb4-135">**For Run As accounts:**</span></span>

* <span data-ttu-id="c6cb4-136">Vytvoří aplikaci Azure AD s certifikátem podepsaným svým držitelem, vytvoří účet instančního objektu pro tuto aplikaci v Azure AD a přiřadí roli přispěvatele pro tento účet v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-136">Creates an Azure AD application with a self-signed certificate, creates a service principal account for the application in Azure AD, and assigns the Contributor role for the account in your current subscription.</span></span> <span data-ttu-id="c6cb4-137">Toto nastavení můžete změnit na roli Vlastník nebo libovolnou jinou roli.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-137">You can change this setting to Owner or any other role.</span></span> <span data-ttu-id="c6cb4-138">Další informace najdete v tématu [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="c6cb4-138">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="c6cb4-139">V příslušném účtu Automation vytvoří prostředek certifikátu Automation s názvem *AzureRunAsCertificate*.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-139">Creates an Automation certificate asset named *AzureRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="c6cb4-140">Prostředek certifikátu obsahuje privátní klíč certifikátu, který používá aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-140">The certificate asset holds the certificate private key that's used by the Azure AD application.</span></span>
* <span data-ttu-id="c6cb4-141">V příslušném účtu Automation vytvoří prostředek připojení Automation s názvem *AzureRunAsConnection*.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-141">Creates an Automation connection asset named *AzureRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="c6cb4-142">Prostředek připojení obsahuje parametry applicationId, tenantId, subscriptionId a certificate thumbprint.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-142">The connection asset holds the applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="c6cb4-143">**Pro účet Spustit jako pro Classic:**</span><span class="sxs-lookup"><span data-stu-id="c6cb4-143">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="c6cb4-144">V příslušném účtu Automation vytvoří prostředek certifikátu Automation s názvem *AzureClassicRunAsCertificate*.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-144">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="c6cb4-145">Prostředek certifikátu obsahuje privátní klíč certifikátu, který používá certifikát pro správu.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-145">The certificate asset holds the certificate private key used by the management certificate.</span></span>
* <span data-ttu-id="c6cb4-146">V příslušném účtu Automation vytvoří prostředek připojení Automation s názvem *AzureClassicRunAsConnection*.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-146">Creates an Automation connection asset named *AzureClassicRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="c6cb4-147">Prostředek propojení obsahuje název a ID předplatného a název prostředku certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-147">The connection asset holds the subscription name, subscriptionId, and certificate asset name.</span></span>

1. <span data-ttu-id="c6cb4-148">Přihlaste se k webu Azure Portal pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-148">Sign in to the Azure portal with an account that is a member of the Subscription Admins role and co-administrator of the subscription.</span></span>
2. <span data-ttu-id="c6cb4-149">Z okna účtu Automation vyberte **Účty Spustit jako** v části **Nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-149">From the Automation account blade, select **Run As Accounts** under the section **Account Settings**.</span></span>  
3. <span data-ttu-id="c6cb4-150">Podle toho, který účet požadujete, vyberte buď **Účet Spustit jako pro Azure**, nebo **Účet Spustit jako pro Azure Classic**.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-150">Depending on which account you require, select either **Azure Run As Account** or **Azure Classic Run As Account**.</span></span>  <span data-ttu-id="c6cb4-151">Po výběru se zobrazí okno **Přidat účet Spustit jako pro Azure** nebo **Přidat účet Spustit jako pro Azure Classic**. Jakmile zkontrolujete souhrnné informace, klikněte na **Vytvořit** a pokračujte ve vytváření účtu Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-151">After selecting either the **Add Azure Run As** or **Add Azure Classic Run As Account** blade appears and after reviewing the overview information, click **Create** to proceed with Run As account creation.</span></span>  
4. <span data-ttu-id="c6cb4-152">Zatímco Azure vytváří účet Spustit jako, zobrazuje se banner s informací o vytváření účtu a průběh vytváření můžete sledovat prostřednictvím možnosti nabídky **Oznámení**.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-152">While Azure creates the Run As account, you can track the progress under **Notifications** from the menu and a banner is displayed stating the account is being created.</span></span>  <span data-ttu-id="c6cb4-153">Dokončení tohoto procesu může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-153">This process can take a few minutes to complete.</span></span>  

## <a name="create-run-as-account-using-powershell-script"></a><span data-ttu-id="c6cb4-154">Vytvoření účtu Spustit jako pomocí skriptu PowerShellu</span><span class="sxs-lookup"><span data-stu-id="c6cb4-154">Create Run As account using PowerShell script</span></span>
<span data-ttu-id="c6cb4-155">Tento skript PowerShellu zahrnuje podporu následujících konfigurací:</span><span class="sxs-lookup"><span data-stu-id="c6cb4-155">This PowerShell script includes support for the following configurations:</span></span>

* <span data-ttu-id="c6cb4-156">Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="c6cb4-156">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="c6cb4-157">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="c6cb4-157">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="c6cb4-158">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu</span><span class="sxs-lookup"><span data-stu-id="c6cb4-158">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="c6cb4-159">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem v cloudu Azure Government</span><span class="sxs-lookup"><span data-stu-id="c6cb4-159">Create a Run As account and a Classic Run As account by using a self-signed certificate in the Azure Government cloud.</span></span>

<span data-ttu-id="c6cb4-160">V závislosti na možnosti konfigurace, kterou vyberete, skript vytvoří následující položky.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-160">Depending on the configuration option you select, the script creates the following items.</span></span>

<span data-ttu-id="c6cb4-161">**Pro účty Spustit jako:**</span><span class="sxs-lookup"><span data-stu-id="c6cb4-161">**For Run As accounts:**</span></span>

* <span data-ttu-id="c6cb4-162">Vytvoří aplikaci Azure AD, která se exportuje s veřejným klíčem certifikátu podepsaného svým držitelem nebo podnikového certifikátu, vytvoří účet instančního objektu pro tuto aplikaci v Azure AD a přiřadí roli přispěvatele pro tento účet v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-162">Creates an Azure AD application to be exported with either the self-signed or enterprise certificate public key, creates a service principal account for the application in Azure AD, and assigns the Contributor role for the account in your current subscription.</span></span> <span data-ttu-id="c6cb4-163">Toto nastavení můžete změnit na roli Vlastník nebo libovolnou jinou roli.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-163">You can change this setting to Owner or any other role.</span></span> <span data-ttu-id="c6cb4-164">Další informace najdete v tématu [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="c6cb4-164">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="c6cb4-165">V příslušném účtu Automation vytvoří prostředek certifikátu Automation s názvem *AzureRunAsCertificate*.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-165">Creates an Automation certificate asset named *AzureRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="c6cb4-166">Prostředek certifikátu obsahuje privátní klíč certifikátu, který používá aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-166">The certificate asset holds the certificate private key that's used by the Azure AD application.</span></span>
* <span data-ttu-id="c6cb4-167">V příslušném účtu Automation vytvoří prostředek připojení Automation s názvem *AzureRunAsConnection*.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-167">Creates an Automation connection asset named *AzureRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="c6cb4-168">Prostředek připojení obsahuje parametry applicationId, tenantId, subscriptionId a certificate thumbprint.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-168">The connection asset holds the applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="c6cb4-169">**Pro účet Spustit jako pro Classic:**</span><span class="sxs-lookup"><span data-stu-id="c6cb4-169">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="c6cb4-170">V příslušném účtu Automation vytvoří prostředek certifikátu Automation s názvem *AzureClassicRunAsCertificate*.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-170">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="c6cb4-171">Prostředek certifikátu obsahuje privátní klíč certifikátu, který používá certifikát pro správu.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-171">The certificate asset holds the certificate private key used by the management certificate.</span></span>
* <span data-ttu-id="c6cb4-172">V příslušném účtu Automation vytvoří prostředek připojení Automation s názvem *AzureClassicRunAsConnection*.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-172">Creates an Automation connection asset named *AzureClassicRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="c6cb4-173">Prostředek propojení obsahuje název a ID předplatného a název prostředku certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-173">The connection asset holds the subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="c6cb4-174">Pokud při vytváření účtu Spustit jako pro Classic vyberete libovolnou z těchto možností, po spuštění skriptu nahrajte veřejný certifikát (soubor s příponou .cer) do úložiště správy předplatného, ve kterém byl účet Automation vytvořený.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-174">If you select either option for creating a Classic Run As account, after the script is executed, upload the public certificate (.cer file name extension) to the management store for the subscription that the Automation account was created in.</span></span>
> 

1. <span data-ttu-id="c6cb4-175">Uložte následující skript do počítače.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-175">Save the following script on your computer.</span></span> <span data-ttu-id="c6cb4-176">V tomto příkladu ho uložte s názvem *New-RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-176">In this example, save it with the filename *New-RunAsAccount.ps1*.</span></span>

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
        if (!(($AzureRMProfileVersion.Major -ge 3 -and $AzureRMProfileVersion.Minor -ge 0) -or ($AzureRMProfileVersion.Major -gt 3)))
        {
            Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
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
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. <span data-ttu-id="c6cb4-177">Na svém počítači na obrazovce **Start** spusťte **Windows PowerShell** se zvýšenými uživatelskými právy.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-177">On your computer, start **Windows PowerShell** from the **Start** screen with elevated user rights.</span></span>
3. <span data-ttu-id="c6cb4-178">V prostředí příkazového řádku se zvýšenými oprávněními přejděte do složky, která obsahuje skript vytvořený v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-178">From the elevated command-line shell, go to the folder that contains the script you created in step 1.</span></span>  
4. <span data-ttu-id="c6cb4-179">Spusťte tento skript s využitím hodnot parametrů pro požadovanou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-179">Execute the script by using the parameter values for the configuration you require.</span></span>

    <span data-ttu-id="c6cb4-180">**Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem**</span><span class="sxs-lookup"><span data-stu-id="c6cb4-180">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="c6cb4-181">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem**</span><span class="sxs-lookup"><span data-stu-id="c6cb4-181">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="c6cb4-182">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu**</span><span class="sxs-lookup"><span data-stu-id="c6cb4-182">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="c6cb4-183">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem v cloudu Azure Government**</span><span class="sxs-lookup"><span data-stu-id="c6cb4-183">**Create a Run As account and a Classic Run As account by using a self-signed certificate in the Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="c6cb4-184">Po spuštění skriptu se zobrazí výzva k ověření pomocí Azure.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-184">After the script has executed, you will be prompted to authenticate with Azure.</span></span> <span data-ttu-id="c6cb4-185">Přihlaste se pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-185">Sign in with an account that is a member of the subscription administrators role and co-administrator of the subscription.</span></span>
    >
    >

<span data-ttu-id="c6cb4-186">Po úspěšném spuštění skriptu je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="c6cb4-186">After the script has executed successfully, note the following:</span></span>
* <span data-ttu-id="c6cb4-187">Pokud jste vytvořili účet Spustit jako pro Classic s využitím veřejného certifikátu podepsaného svým držitelem (soubor .cer), skript ho vytvoří a uloží ve složce dočasných souborů ve vašem počítači pod profilem uživatele *%USERPROFILE%\AppData\Local\Temp*, který používáte ke spuštění relace PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-187">If you created a Classic Run As account with a self-signed public certificate (.cer file), the script creates and saves it to the temporary files folder on your computer under the user profile *%USERPROFILE%\AppData\Local\Temp*, which you used to execute the PowerShell session.</span></span>
* <span data-ttu-id="c6cb4-188">Pokud jste vytvořili účet Spustit jako pro Classic s využitím podnikového veřejného certifikátu (soubor .cer), použijte tento certifikát.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-188">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="c6cb4-189">Postupujte podle kroků pro [odeslání certifikátu rozhraní API pro správu na portál Azure Classic](../azure-api-management-certs.md) a potom použijte [ukázkový kód pro ověření pomocí prostředků nasazení Azure Classic](automation-verify-runas-authentication.md#classic-run-as-authentication) k ověření konfigurace přihlašovacích údajů pomocí prostředků nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="c6cb4-189">Follow the instructions for [uploading a management API certificate to the Azure classic portal](../azure-api-management-certs.md), and then validate the credential configuration with classic deployment resources by using the [sample code to authenticate with Azure Classic Deployment Resources](automation-verify-runas-authentication.md#classic-run-as-authentication).</span></span> 
* <span data-ttu-id="c6cb4-190">Pokud jste *nevytvořili* účet Spustit jako pro Classic, použijte k ověření pomocí prostředků Resource Manageru a ke kontrole konfigurace přihlašovacích údajů [ukázkový kód pro ověření s využitím prostředků správy služeb](automation-verify-runas-authentication.md#automation-run-as-authentication).</span><span class="sxs-lookup"><span data-stu-id="c6cb4-190">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate the credential configuration by using the [sample code for authenticating with Service Management resources](automation-verify-runas-authentication.md#automation-run-as-authentication).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6cb4-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6cb4-191">Next steps</span></span>
* <span data-ttu-id="c6cb4-192">Další informace o objektech služby najdete v článku [Objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="c6cb4-192">For more information about Service Principals, refer to [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span>
* <span data-ttu-id="c6cb4-193">Další informace o certifikátech a službách Azure najdete v článku [Přehled certifikátů pro Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="c6cb4-193">For more information about certificates and Azure services, refer to [Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span></span>