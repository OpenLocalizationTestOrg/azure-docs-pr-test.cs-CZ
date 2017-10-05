---
title: "Konfigurace účtu Spustit jako pro Azure | Dokumentace Microsoftu"
description: "Tento kurz vás provede vytvořením, otestováním a ukázkovým použitím ověření objektu zabezpečení ve službě Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
keywords: "název objektu služby, název instančního objektu, setspn, ověřování azure"
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/06/2017
ms.author: magoedte
ROBOTS: NOINDEX
redirect_url: /azure/automation/
redirect_document_id: TRUE
ms.openlocfilehash: f88c775eba19bb227d0e206d6420c08b57305b92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-runbooks-with-an-azure-run-as-account"></a><span data-ttu-id="ed042-104">Ověření runbooků pomocí účtu Spustit jako pro Azure</span><span class="sxs-lookup"><span data-stu-id="ed042-104">Authenticate runbooks with an Azure Run As account</span></span>
<span data-ttu-id="ed042-105">V tomto tématu se seznámíte se způsobem konfigurace účtu Azure Automation na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ed042-105">This article shows you how to configure an Azure Automation account in the Azure portal.</span></span> <span data-ttu-id="ed042-106">Pomocí funkce účtu Spustit jako za účelem ověříte prostředky, které spravují runbooky, a to buď v Azure Resource Manageru, nebo ve službě Azure Service Management.</span><span class="sxs-lookup"><span data-stu-id="ed042-106">To do so, you use the Run As account feature to authenticate runbooks managing resources in either Azure Resource Manager or Azure Service Management.</span></span>

<span data-ttu-id="ed042-107">Když na webu Azure Portal vytvoříte účet Automation, automaticky se vytvoří dva účty:</span><span class="sxs-lookup"><span data-stu-id="ed042-107">When you create an Automation account in the Azure portal, you automatically create two accounts:</span></span>

* <span data-ttu-id="ed042-108">Účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="ed042-108">A Run As account.</span></span> <span data-ttu-id="ed042-109">Tento účet vytvoří instanční objekt ve službě Azure Active Directory (Azure AD) a certifikát.</span><span class="sxs-lookup"><span data-stu-id="ed042-109">This account creates a service principal in Azure Active Directory (Azure AD) and a certificate.</span></span> <span data-ttu-id="ed042-110">Přiřadí také řízení přístupu na základě role Přispěvatel (RBAC), které spravuje prostředky Resource Manageru pomocí runbooků.</span><span class="sxs-lookup"><span data-stu-id="ed042-110">It also assigns the Contributor role-based access control (RBAC), which manages Resource Manager resources by using runbooks.</span></span>
* <span data-ttu-id="ed042-111">Účet Spustit jako pro Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="ed042-111">A Classic Run As account.</span></span> <span data-ttu-id="ed042-112">Tento účet nahraje certifikát pro správu, který se používá ke správě prostředků správy služeb nebo klasických prostředků pomocí runbooků.</span><span class="sxs-lookup"><span data-stu-id="ed042-112">This account uploads a management certificate, which is used to manage Service Management or classic resources by using runbooks.</span></span>

<span data-ttu-id="ed042-113">Vytvoření účtu Automation zjednodušuje tento proces a pomůže vám rychle začít vytvářet a nasazovat runbooky na podporu vašich automatizačních potřeb.</span><span class="sxs-lookup"><span data-stu-id="ed042-113">Creating an Automation account simplifies the process for you and helps you quickly start building and deploying runbooks to support your automation needs.</span></span>

<span data-ttu-id="ed042-114">Pomocí účtu Spustit jako a účtu Spustit jako pro Azure Classic můžete:</span><span class="sxs-lookup"><span data-stu-id="ed042-114">With Run As and Classic Run As accounts, you can:</span></span>

* <span data-ttu-id="ed042-115">Zajistit standardizovaný způsob ověřování pomocí Azure při správě prostředků Azure Resource Manageru nebo prostředků správy služeb pomocí runbooků na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ed042-115">Provide a standardized way to authenticate with Azure when you manage Resource Manager or Service Management resources from runbooks in the Azure portal.</span></span>
* <span data-ttu-id="ed042-116">Automatizovat používání globálních runbooků, které se dají nakonfigurovat ve výstrahách Azure.</span><span class="sxs-lookup"><span data-stu-id="ed042-116">Automate the use of global runbooks, which you can configure in Azure Alerts.</span></span>

> [!NOTE]
> <span data-ttu-id="ed042-117">[Funkce integrace výstrah Azure](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) pomocí globálních runbooků Automation vyžaduje účet Automation, který je nakonfigurovaný s využitím účtů Spustit jako a Spustit jako pro Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="ed042-117">The [Azure Alert integration feature](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) with Automation global runbooks requires an Automation account that's configured with a Run As account and a Classic Run As account.</span></span> <span data-ttu-id="ed042-118">Můžete vybrat účet Automation, který už má definované účty Spustit jako a Spustit jako pro Azure Classic, nebo můžete vytvořit nový účet Automation.</span><span class="sxs-lookup"><span data-stu-id="ed042-118">You can select an Automation account that already has defined Run As and Classic Run As accounts, or you can choose to create a new Automation account.</span></span>
>  

<span data-ttu-id="ed042-119">V tomto článku si ukážeme, jak na webu Azure Portal vytvořit účet Automation, jak tento účet aktualizovat pomocí Azure PowerShellu, jak spravovat jeho konfiguraci účtu a jak ověřovat v runboocích.</span><span class="sxs-lookup"><span data-stu-id="ed042-119">This article shows how to create an Automation account from the Azure portal, update an Automation account by using Azure PowerShell, manage the account configuration, and authenticate in your runbooks.</span></span>

<span data-ttu-id="ed042-120">Ještě než začnete vytvářet účet Automation, je vhodné seznámit se s následujícími aspekty a vzít je v úvahu:</span><span class="sxs-lookup"><span data-stu-id="ed042-120">Before you begin creating an Automation account, it's a good idea to understand and consider the following:</span></span>

* <span data-ttu-id="ed042-121">Vytvoření účtu Automation neovlivní existující účty Automation, které už jste případně vytvořili v modelu nasazení Classic nebo v modelu nasazení Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="ed042-121">Creating an Automation account does not affect Automation accounts you might have already created in either the classic or Resource Manager deployment model.</span></span>
* <span data-ttu-id="ed042-122">Proces funguje jenom pro účty Automation, které vytvoříte na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ed042-122">The process works only for Automation accounts that you create in the Azure portal.</span></span> <span data-ttu-id="ed042-123">Pokus o vytvoření účtu na portálu Azure Classic nezreplikuje konfiguraci účtu Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="ed042-123">Attempting to create an account from the Azure classic portal does not replicate the Run As account configuration.</span></span>
* <span data-ttu-id="ed042-124">Pokud už máte runbooky a prostředky (třeba plány nebo proměnné) pro správu klasických prostředků a chcete, aby runbooky k ověřování používaly nový účet Spustit jako pro Classic, proveďte jednu z těchto akcí:</span><span class="sxs-lookup"><span data-stu-id="ed042-124">If you already have runbooks and assets (such as schedules or variables) in place to manage classic resources, and you want runbooks to authenticate with the new Classic Run As account, do either of the following:</span></span>

  * <span data-ttu-id="ed042-125">Pokud chcete vytvořit účet Spustit jako pro Classic, postupujte podle pokynů v části Správa účtu Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="ed042-125">To create a Classic Run As account, follow the instructions in the "Managing your Run As account" section.</span></span> 
  * <span data-ttu-id="ed042-126">Pokud chcete aktualizovat existující účet, použijte skript PowerShellu v části Aktualizace účtu Automation pomocí PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="ed042-126">To update your existing account, use the PowerShell script in the "Update your Automation account by using PowerShell" section.</span></span>
* <span data-ttu-id="ed042-127">Abyste mohli ověřovat pomocí nového účtu Automation Spustit jako a účtu Spustit jako pro Classic, musíte stávající runbooky upravit pomocí ukázkového kódu uvedeného v části [Příklady kódu pro ověřování](#authentication-code-examples).</span><span class="sxs-lookup"><span data-stu-id="ed042-127">To authenticate by using the new Run As account and Classic Run As Automation account, you  need to modify your existing runbooks with the example code provided in the section [Authentication code examples](#authentication-code-examples).</span></span>

    >[!NOTE]
    ><span data-ttu-id="ed042-128">Účet Spustit jako slouží k ověřování pomocí prostředků Resource Manageru s využitím instančního objektu založeného na certifikátech.</span><span class="sxs-lookup"><span data-stu-id="ed042-128">The Run As account is for authentication against Resource Manager resources using the certificate-based service principal.</span></span> <span data-ttu-id="ed042-129">Účet Spustit jako pro Classic slouží k ověřování pomocí prostředků správy služeb s využitím certifikátu pro správu.</span><span class="sxs-lookup"><span data-stu-id="ed042-129">The Classic Run As account is for authenticating against Service Management resources with a management certificate.</span></span>

## <a name="create-an-automation-account-from-the-azure-portal"></a><span data-ttu-id="ed042-130">Vytvoření účtu Automation na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ed042-130">Create an Automation account from the Azure portal</span></span>
<span data-ttu-id="ed042-131">V této části vytvoříte na webu Azure Portal účet služby Azure Automation, který potom vytvoří jak účet Spustit jako, tak účet Spustit jako pro Classic.</span><span class="sxs-lookup"><span data-stu-id="ed042-131">In this section, you create an Azure Automation account from the Azure portal, which in turn creates both a Run As account and a Classic Run As account.</span></span>

>[!NOTE]
><span data-ttu-id="ed042-132">Abyste mohli vytvořit účet Automation, musíte být členem role Správci služeb nebo spolusprávcem předplatného, který k tomuto předplatnému uděluje přístup.</span><span class="sxs-lookup"><span data-stu-id="ed042-132">To create an Automation account, you must be a member of the Service Admins role or co-administrator of the subscription that is granting access to the subscription.</span></span> <span data-ttu-id="ed042-133">K výchozí instanci Active Directory tohoto předplatného musíte být přihlášení jako uživatel.</span><span class="sxs-lookup"><span data-stu-id="ed042-133">You must also be added as a user to that subscription's default Active Directory instance.</span></span> <span data-ttu-id="ed042-134">Účtu nemusí mít přiřazenou privilegovanou roli.</span><span class="sxs-lookup"><span data-stu-id="ed042-134">The account does not need to be assigned a privileged role.</span></span>
>
><span data-ttu-id="ed042-135">Pokud před přidáním k roli spolusprávce nejste členem instance Active Directory příslušného předplatného, budete do služby Active Directory přidaní jako host.</span><span class="sxs-lookup"><span data-stu-id="ed042-135">If you are not a member of the subscription’s Active Directory instance before you are added to the co-administrator role of the subscription, you will be added to Active Directory as a guest.</span></span> <span data-ttu-id="ed042-136">V tomto případě se zobrazí varování, že nemáte oprávnění k vytvoření,</span><span class="sxs-lookup"><span data-stu-id="ed042-136">In this instance, you will receive a “You do not have permissions to create…”</span></span> <span data-ttu-id="ed042-137">v okně **Přidání účtu Automation**.</span><span class="sxs-lookup"><span data-stu-id="ed042-137">warning on the **Add Automation Account** blade.</span></span>
>
><span data-ttu-id="ed042-138">Uživatele, kteří byli nejdřív přidaní do role spolusprávce, je možné z instance Active Directory předplatného odebrat a potom je znovu přidat – tak se z nich ve službě Active Directory stanou úplní uživatelé.</span><span class="sxs-lookup"><span data-stu-id="ed042-138">Users who were added to the co-administrator role first can be removed from the subscription's Active Directory instance and re-added to make them a full User in Active Directory.</span></span> <span data-ttu-id="ed042-139">Takovou situaci můžete ověřit v podokně **Azure Active Directory** na webu Azure Portal. Vyberte **Uživatelé a skupiny**, potom **Všichni uživatelé** a po výběru konkrétního uživatele vyberte **Profil**.</span><span class="sxs-lookup"><span data-stu-id="ed042-139">To verify this situation from the **Azure Active Directory** pane in the Azure portal by selecting **Users and groups**, selecting **All users** and, after you select the specific user, selecting **Profile**.</span></span> <span data-ttu-id="ed042-140">Hodnota atributu **Typ uživatele** v profilu uživatele by neměla být **Host**.</span><span class="sxs-lookup"><span data-stu-id="ed042-140">The value of the **User type** attribute under the users profile should not equal **Guest**.</span></span>
>

1. <span data-ttu-id="ed042-141">Přihlaste se k webu Azure Portal pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.</span><span class="sxs-lookup"><span data-stu-id="ed042-141">Sign in to the Azure portal with an account that is a member of the subscription administrators role and co-administrator of the subscription.</span></span>

2. <span data-ttu-id="ed042-142">Vyberte **Účty Automation**.</span><span class="sxs-lookup"><span data-stu-id="ed042-142">Select **Automation Accounts**.</span></span>

3. <span data-ttu-id="ed042-143">V okně **Účty Automation** klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="ed042-143">On the **Automation Accounts** blade, click **Add**.</span></span>
<span data-ttu-id="ed042-144">Otevře se okno **Přidat účet Automation**.</span><span class="sxs-lookup"><span data-stu-id="ed042-144">The **Add Automation Account** blade opens.</span></span>

 ![Okno Přidat účet Automation](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)

   > [!NOTE]
   > <span data-ttu-id="ed042-146">Pokud váš účet není členem role správců předplatného a spolusprávcem předplatného, v okně **Přidat účet Automation** se zobrazí následující upozornění:</span><span class="sxs-lookup"><span data-stu-id="ed042-146">If your account is not a member of the subscription administrators role and co-administrator of the subscription, the following warning is displayed on the **Add Automation Account** blade:</span></span>
   >
   >![Přidání upozornění k účtu Automation](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   >
   >

4. <span data-ttu-id="ed042-148">V okně **Přidat účet Automation** zadejte název nového účtu Automation do pole **Název**.</span><span class="sxs-lookup"><span data-stu-id="ed042-148">On the **Add Automation Account** blade, in the **Name** box, type a name for your new Automation account.</span></span>

5. <span data-ttu-id="ed042-149">Pokud máte víc předplatných, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ed042-149">If you have more than one subscription, do the following:</span></span>

    <span data-ttu-id="ed042-150">a.</span><span class="sxs-lookup"><span data-stu-id="ed042-150">a.</span></span> <span data-ttu-id="ed042-151">V části **Předplatné** zadejte předplatné pro nový účet.</span><span class="sxs-lookup"><span data-stu-id="ed042-151">Under **Subscription**, specify one for the new account.</span></span>

    <span data-ttu-id="ed042-152">b.</span><span class="sxs-lookup"><span data-stu-id="ed042-152">b.</span></span> <span data-ttu-id="ed042-153">V části **Skupina prostředků** klikněte na **Vytvořit novou** nebo **Použít stávající**.</span><span class="sxs-lookup"><span data-stu-id="ed042-153">Under **Resource Group**, click **Create new** or **Use existing**.</span></span>

    <span data-ttu-id="ed042-154">c.</span><span class="sxs-lookup"><span data-stu-id="ed042-154">c.</span></span> <span data-ttu-id="ed042-155">V části **Umístění** zadejte datové centrum Azure.</span><span class="sxs-lookup"><span data-stu-id="ed042-155">Under **Location**, specify an Azure datacenter.</span></span>

6. <span data-ttu-id="ed042-156">V části **Vytvořit účet Spustit v Azure jako** vyberte **Ano** a potom klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ed042-156">Under **Create Azure Run As account**, select **Yes**, and then click **Create**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ed042-157">Pokud se rozhodnete nevytvářet účet Spustit jako a vyberete **Ne**, zobrazí se v okně **Přidat účet Automation** zpráva s upozorněním.</span><span class="sxs-lookup"><span data-stu-id="ed042-157">If you choose not to create the Run As account by selecting **No**, a warning message is displayed the **Add Automation Account** blade.</span></span> <span data-ttu-id="ed042-158">Přestože je účet vytvořený na webu Azure Portal, nemá odpovídající identitu ověřování v rámci adresářové služby klasického předplatného nebo předplatného Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="ed042-158">Although the account is created in the Azure portal, it does not have a corresponding authentication identity within your classic or Resource Manager subscription directory service.</span></span> <span data-ttu-id="ed042-159">Následkem toho účet nemá přístup k prostředkům ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="ed042-159">Consequently, the account has no access to resources in your subscription.</span></span> <span data-ttu-id="ed042-160">Všechny runbooky odkazující na tento účet kvůli tomu nebudou moct ověřit a provádět úlohy s prostředky v těchto modelech nasazení.</span><span class="sxs-lookup"><span data-stu-id="ed042-160">This scenario prevents any runbooks that reference this account from authenticating and performing tasks against resources in those deployment models.</span></span>
   >
   > ![Zpráva s upozorněním v okně Přidání účtu Automation](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)
   >
   > <span data-ttu-id="ed042-162">Navíc protože není vytvořený instanční objekt, není přiřazená role přispěvatele.</span><span class="sxs-lookup"><span data-stu-id="ed042-162">Additionally, because the service principal is not created, the Contributor role is not assigned.</span></span>
   >

7. <span data-ttu-id="ed042-163">Zatímco Azure vytváří účet Automation, můžete průběh sledovat v nabídce v části **Oznámení**.</span><span class="sxs-lookup"><span data-stu-id="ed042-163">While Azure creates the Automation account, you can track the progress under **Notifications** from the menu.</span></span>

### <a name="resources"></a><span data-ttu-id="ed042-164">Zdroje a prostředky</span><span class="sxs-lookup"><span data-stu-id="ed042-164">Resources</span></span>
<span data-ttu-id="ed042-165">Po úspěšném vytvoření účtu Automation se pro vaší potřebu automaticky vytvoří několik prostředků.</span><span class="sxs-lookup"><span data-stu-id="ed042-165">When the Automation account is successfully created, several resources are automatically created for you.</span></span> <span data-ttu-id="ed042-166">Prostředky jsou shrnuté v následujících dvou tabulkách:</span><span class="sxs-lookup"><span data-stu-id="ed042-166">The resources are summarized in the following two tables:</span></span>

#### <a name="run-as-account-resources"></a><span data-ttu-id="ed042-167">Prostředky účtu Spustit jako</span><span class="sxs-lookup"><span data-stu-id="ed042-167">Run As account resources</span></span>

| <span data-ttu-id="ed042-168">Prostředek</span><span class="sxs-lookup"><span data-stu-id="ed042-168">Resource</span></span> | <span data-ttu-id="ed042-169">Popis</span><span class="sxs-lookup"><span data-stu-id="ed042-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ed042-170">Runbook AzureAutomationTutorial</span><span class="sxs-lookup"><span data-stu-id="ed042-170">AzureAutomationTutorial Runbook</span></span> | <span data-ttu-id="ed042-171">Ukázkový grafický runbook, který předvádí ověření pomocí účtu Spustit jako a získává všechny prostředku Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="ed042-171">An example graphical runbook that demonstrates how to authenticate by using the Run As account and gets all the Resource Manager resources.</span></span> |
| <span data-ttu-id="ed042-172">Runbook AzureAutomationTutorialScript</span><span class="sxs-lookup"><span data-stu-id="ed042-172">AzureAutomationTutorialScript Runbook</span></span> | <span data-ttu-id="ed042-173">Ukázkový runbook PowerShellu, který předvádí ověření pomocí účtu Spustit jako a získává všechny prostředku Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="ed042-173">An example PowerShell runbook that demonstrates how to authenticate by using the Run As account and gets all the Resource Manager resources.</span></span> |
| <span data-ttu-id="ed042-174">AzureRunAsCertificate</span><span class="sxs-lookup"><span data-stu-id="ed042-174">AzureRunAsCertificate</span></span> | <span data-ttu-id="ed042-175">Prostředek certifikátu vytvořený automaticky během vytváření účtu Automation. Pro stávající účet použijte následující skript PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="ed042-175">The certificate asset that's automatically created when you create an Automation account or use the following PowerShell script for an existing account.</span></span> <span data-ttu-id="ed042-176">Certifikát umožňuje ověření pomocí Azure, abyste mohli spravovat prostředky Azure Resource Manageru pomocí runbooků.</span><span class="sxs-lookup"><span data-stu-id="ed042-176">The certificate allows you to authenticate with Azure so that you can manage Azure Resource Manager resources from runbooks.</span></span> <span data-ttu-id="ed042-177">Tento certifikát má životnost jeden rok.</span><span class="sxs-lookup"><span data-stu-id="ed042-177">The certificate has a one-year lifespan.</span></span> |
| <span data-ttu-id="ed042-178">AzureRunAsConnection</span><span class="sxs-lookup"><span data-stu-id="ed042-178">AzureRunAsConnection</span></span> | <span data-ttu-id="ed042-179">Prostředek připojení vytvořený automaticky během vytváření účtu Automation. Pro stávající účet použijte skript PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="ed042-179">The connection asset that's automatically created when you create an Automation account or use the PowerShell script for an existing account.</span></span> |

#### <a name="classic-run-as-account-resources"></a><span data-ttu-id="ed042-180">Prostředky účtu Spustit jako pro Classic</span><span class="sxs-lookup"><span data-stu-id="ed042-180">Classic Run As account resources</span></span>

| <span data-ttu-id="ed042-181">Prostředek</span><span class="sxs-lookup"><span data-stu-id="ed042-181">Resource</span></span> | <span data-ttu-id="ed042-182">Popis</span><span class="sxs-lookup"><span data-stu-id="ed042-182">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ed042-183">Runbook AzureClassicAutomationTutorial</span><span class="sxs-lookup"><span data-stu-id="ed042-183">AzureClassicAutomationTutorial Runbook</span></span> | <span data-ttu-id="ed042-184">Ukázkový grafický runbook, který získá všechny virtuální počítače vytvořené v rámci předplatného pomocí modelu nasazení Classic s využitím účtu Spustit jako pro Classic (certifikát) a potom zapíše název a stav virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ed042-184">An example graphical runbook that gets all the VMs that are created using the classic deployment model in a subscription by using the Classic Run As account (certificate), and then writes the VM name and status.</span></span> |
| <span data-ttu-id="ed042-185">Runbook se skriptem AzureClassicAutomationTutorial</span><span class="sxs-lookup"><span data-stu-id="ed042-185">AzureClassicAutomationTutorial Script Runbook</span></span> | <span data-ttu-id="ed042-186">Ukázkový runbook PowerShellu, který získá všechny klasické virtuální počítače v rámci předplatného pomocí účtu Spustit jako pro Azure Classic (certifikát) a potom vypíše název a stav virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ed042-186">An example PowerShell runbook that gets all the classic VMs in a subscription by using the Classic Run As account (certificate), and then writes the VM name and status.</span></span> |
| <span data-ttu-id="ed042-187">AzureClassicRunAsCertificate</span><span class="sxs-lookup"><span data-stu-id="ed042-187">AzureClassicRunAsCertificate</span></span> | <span data-ttu-id="ed042-188">Automaticky vytvořený prostředek certifikátu, který použijete k ověřování pomocí Azure, abyste mohli spravovat klasické prostředky Azure pomocí runbooků.</span><span class="sxs-lookup"><span data-stu-id="ed042-188">The automatically created certificate asset that you use to authenticate with Azure so that you can manage Azure classic resources from runbooks.</span></span> <span data-ttu-id="ed042-189">Tento certifikát má životnost jeden rok.</span><span class="sxs-lookup"><span data-stu-id="ed042-189">The certificate has a one-year lifespan.</span></span> |
| <span data-ttu-id="ed042-190">AzureClassicRunAsConnection</span><span class="sxs-lookup"><span data-stu-id="ed042-190">AzureClassicRunAsConnection</span></span> | <span data-ttu-id="ed042-191">Automaticky vytvořený prostředek připojení, který použijete k ověřování pomocí Azure, abyste mohli spravovat klasické prostředky Azure pomocí runbooků.</span><span class="sxs-lookup"><span data-stu-id="ed042-191">The automatically created connection asset that you use to authenticate with Azure so that you can manage Azure classic resources from runbooks.</span></span> |

## <a name="verify-run-as-authentication"></a><span data-ttu-id="ed042-192">Kontrola ověřování pomocí účtu Spustit jako</span><span class="sxs-lookup"><span data-stu-id="ed042-192">Verify Run As authentication</span></span>
<span data-ttu-id="ed042-193">Proveďte malý test, abyste potvrdili, že pomocí nového účtu Spustit jako můžete provádět ověření.</span><span class="sxs-lookup"><span data-stu-id="ed042-193">Perform a small test to confirm that you can successfully authenticate by using the new Run As account.</span></span>

1. <span data-ttu-id="ed042-194">Na webu Azure Portal otevřete účet Automation, který jste vytvořili dřív.</span><span class="sxs-lookup"><span data-stu-id="ed042-194">In the Azure portal, open the Automation account that you created earlier.</span></span>

2. <span data-ttu-id="ed042-195">Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.</span><span class="sxs-lookup"><span data-stu-id="ed042-195">Click the **Runbooks** tile to open the list of runbooks.</span></span>

3. <span data-ttu-id="ed042-196">Vyberte runbook **AzureAutomationTutorialScript** a kliknutím na **Spustit** spusťte tento runbook.</span><span class="sxs-lookup"><span data-stu-id="ed042-196">Select the **AzureAutomationTutorialScript** runbook, and then click **Start** to start the runbook.</span></span> <span data-ttu-id="ed042-197">Dojde k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="ed042-197">The following events occur:</span></span>
 * <span data-ttu-id="ed042-198">Vytvoří se [úloha runbooku](automation-runbook-execution.md), zobrazí se okno **Úloha** a na dlaždici **Souhrn úlohy** se zobrazí stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="ed042-198">A [runbook job](automation-runbook-execution.md) is created, the **Job** blade is displayed, and the job status is displayed in the **Job Summary** tile.</span></span>
 * <span data-ttu-id="ed042-199">Počáteční stav úlohy bude **e frontě**. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu.</span><span class="sxs-lookup"><span data-stu-id="ed042-199">The job status begins as **Queued**, indicating that it is waiting for a runbook worker in the cloud to become available.</span></span>
 * <span data-ttu-id="ed042-200">Když pracovní proces úlohu přijme, změní se stav na **Spouštění**.</span><span class="sxs-lookup"><span data-stu-id="ed042-200">The status becomes **Starting** when a worker claims the job.</span></span>
 * <span data-ttu-id="ed042-201">Když se runbook skutečně spustí, změní se stav na **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="ed042-201">The status becomes **Running** when the runbook starts running.</span></span>
 * <span data-ttu-id="ed042-202">Po dokončení úlohy runbooku by se měl zobrazit stav **Dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="ed042-202">When the runbook job has finished running, you should see a status of **Completed**.</span></span>

       ![Security Principal Runbook Test](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)
4. <span data-ttu-id="ed042-203">Pokud chcete zobrazit podrobné výsledky runbooku, klikněte na dlaždici **Výstup**.</span><span class="sxs-lookup"><span data-stu-id="ed042-203">To see the detailed results of the runbook, click the **Output** tile.</span></span>  
<span data-ttu-id="ed042-204">Zobrazí se okno **Výstup**, ve kterém se ukazuje, že se runbook úspěšně ověřil a vrátil seznam všech prostředků, které jsou ve skupině prostředků dostupné.</span><span class="sxs-lookup"><span data-stu-id="ed042-204">The **Output** blade is displayed, showing that the runbook has successfully authenticated and returned a list of all resources available in the resource group.</span></span>

5. <span data-ttu-id="ed042-205">Zavřením okna **Výstup** se vrátíte do okna **Souhrn úlohy**.</span><span class="sxs-lookup"><span data-stu-id="ed042-205">Close the **Output** blade to return to the **Job Summary** blade.</span></span>

6. <span data-ttu-id="ed042-206">Zavřete okno **Souhrn úlohy** a odpovídající okno runbooku **AzureAutomationTutorialScript**.</span><span class="sxs-lookup"><span data-stu-id="ed042-206">Close the **Job Summary** blade and the corresponding **AzureAutomationTutorialScript** runbook blade.</span></span>

## <a name="verify-classic-run-as-authentication"></a><span data-ttu-id="ed042-207">Kontrola ověřování pomocí účtu Spustit jako pro Azure Classic</span><span class="sxs-lookup"><span data-stu-id="ed042-207">Verify Classic Run As authentication</span></span>
<span data-ttu-id="ed042-208">Proveďte podobný malý test, abyste potvrdili, že pomocí nového účtu Spustit jako pro Classic můžete provádět ověření.</span><span class="sxs-lookup"><span data-stu-id="ed042-208">Perform a similar small test to confirm that you can successfully authenticate by using the new Classic Run As account.</span></span>

1. <span data-ttu-id="ed042-209">Na webu Azure Portal otevřete účet Automation, který jste vytvořili dřív.</span><span class="sxs-lookup"><span data-stu-id="ed042-209">In the Azure portal, open the Automation account that you created earlier.</span></span>

2. <span data-ttu-id="ed042-210">Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.</span><span class="sxs-lookup"><span data-stu-id="ed042-210">Click the **Runbooks** tile to open the list of runbooks.</span></span>

3. <span data-ttu-id="ed042-211">Vyberte runbook **AzureClassicAutomationTutorialScript** a kliknutím na tlačítko **Spustit** tento runbook spusťte.</span><span class="sxs-lookup"><span data-stu-id="ed042-211">Select the **AzureClassicAutomationTutorialScript** runbook, and then click **Start** to  start the runbook.</span></span> <span data-ttu-id="ed042-212">Dojde k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="ed042-212">The following events occur:</span></span>

 * <span data-ttu-id="ed042-213">Vytvoří se [úloha runbooku](automation-runbook-execution.md), zobrazí se okno **Úloha** a na dlaždici **Souhrn úlohy** se zobrazí stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="ed042-213">A [runbook job](automation-runbook-execution.md) is created, the **Job** blade is displayed, and the job status is displayed in the **Job Summary** tile.</span></span>
 * <span data-ttu-id="ed042-214">Počáteční stav úlohy bude **e frontě**. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu.</span><span class="sxs-lookup"><span data-stu-id="ed042-214">The job status begins as **Queued**, indicating that it is waiting for a runbook worker in the cloud to become available.</span></span>
 * <span data-ttu-id="ed042-215">Když pracovní proces úlohu přijme, změní se stav na **Spouštění**.</span><span class="sxs-lookup"><span data-stu-id="ed042-215">The status becomes **Starting** when a worker claims the job.</span></span>
 * <span data-ttu-id="ed042-216">Když se runbook skutečně spustí, změní se stav na **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="ed042-216">The status becomes **Running** when the runbook starts running.</span></span>
 * <span data-ttu-id="ed042-217">Po dokončení úlohy runbooku by se měl zobrazit stav **Dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="ed042-217">When the runbook job has finished running, you should see a status of **Completed**.</span></span>

    ![Test runbooku objektu zabezpečení](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
4. <span data-ttu-id="ed042-219">Pokud chcete zobrazit podrobné výsledky runbooku, klikněte na dlaždici **Výstup**.</span><span class="sxs-lookup"><span data-stu-id="ed042-219">To see the detailed results of the runbook, click the **Output** tile.</span></span>  
<span data-ttu-id="ed042-220">Zobrazí se okno **Výstup**, ve kterém se ukazuje, že se runbook úspěšně ověřil a vrátil seznam všech klasických virtuálních prostředků v předplatném.</span><span class="sxs-lookup"><span data-stu-id="ed042-220">The **Output** blade is displayed, showing that the runbook has successfully authenticated and returned a list of all classic VMs in the subscription.</span></span>

5. <span data-ttu-id="ed042-221">Zavřením okna **Výstup** se vrátíte do okna **Souhrn úlohy**.</span><span class="sxs-lookup"><span data-stu-id="ed042-221">Close the **Output** blade to return to the **Job Summary** blade.</span></span>

6. <span data-ttu-id="ed042-222">Zavřete okno **Souhrn úlohy** a odpovídající okno runbooku **AzureAutomationTutorialScript**.</span><span class="sxs-lookup"><span data-stu-id="ed042-222">Close the **Job Summary** blade and the corresponding **AzureAutomationTutorialScript** runbook blade.</span></span>

## <a name="managing-your-run-as-account"></a><span data-ttu-id="ed042-223">Správa účtu Spustit jako</span><span class="sxs-lookup"><span data-stu-id="ed042-223">Managing your Run As account</span></span>
<span data-ttu-id="ed042-224">V určitém okamžiku před vypršením platnosti účtu Automation musíte obnovit certifikát.</span><span class="sxs-lookup"><span data-stu-id="ed042-224">At some point before your Automation account expires, you will need to renew the certificate.</span></span> <span data-ttu-id="ed042-225">Pokud se domníváte, že zabezpečení účtu Spustit jako je ohrožené, můžete ho odstranit a vytvořit znovu.</span><span class="sxs-lookup"><span data-stu-id="ed042-225">If you believe that the Run As account has been compromised, you can delete and re-create it.</span></span> <span data-ttu-id="ed042-226">Tato část popisuje, jak tyto operace provést.</span><span class="sxs-lookup"><span data-stu-id="ed042-226">This section discusses how to perform these operations.</span></span>

### <a name="self-signed-certificate-renewal"></a><span data-ttu-id="ed042-227">Obnovení certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="ed042-227">Self-signed certificate renewal</span></span>
<span data-ttu-id="ed042-228">Platnost certifikátu podepsaného svým držitelem, který jste vytvořili pro účet Spustit jako, vyprší jeden rok od data jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="ed042-228">The self-signed certificate that you created for the Run As account expires one year from the date of creation.</span></span> <span data-ttu-id="ed042-229">Před vypršením platnosti ho můžete kdykoli obnovit.</span><span class="sxs-lookup"><span data-stu-id="ed042-229">You can renew it at any time before it expires.</span></span> <span data-ttu-id="ed042-230">Když ho obnovíte, aktuální platný certifikát se uchová, aby se zajistilo, že to negativně neovlivní runbooky ověřované účtem Spustit jako, které jsou právě ve frontě nebo aktivně spuštěné.</span><span class="sxs-lookup"><span data-stu-id="ed042-230">When you renew it, the current valid certificate is retained to ensure that any runbooks that are queued up or actively running, and that authenticate with the Run As account, are not negatively affected.</span></span> <span data-ttu-id="ed042-231">Certifikát zůstane platný až do data vypršení jeho platnosti.</span><span class="sxs-lookup"><span data-stu-id="ed042-231">The certificate remains valid until its expiration date.</span></span>

> [!NOTE]
> <span data-ttu-id="ed042-232">Pokud jste svůj účet Automation Spustit jako nakonfigurovali k používání certifikátu vydaného podnikovou certifikační autoritou a použijete tuto možnost, tento podnikový certifikát se nahradí certifikátem podepsaným svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="ed042-232">If you have configured your Automation Run As account to use a certificate issued by your enterprise certificate authority and you use this option, the enterprise certificate will be replaced by a self-signed certificate.</span></span>

<span data-ttu-id="ed042-233">Pokud chcete certifikát obnovit, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ed042-233">To renew the certificate, do the following:</span></span>

1. <span data-ttu-id="ed042-234">Na webu Azure Portal otevřete účet Automation.</span><span class="sxs-lookup"><span data-stu-id="ed042-234">In the Azure portal, open the Automation account.</span></span>

2. <span data-ttu-id="ed042-235">V okně **Účet Automation** v podokně **Vlastnosti účtu** v části **Nastavení účtů** vyberte **Účty Spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="ed042-235">On the **Automation Account** blade, in the **Account properties** pane, under **Account Settings**, select **Run As Accounts**.</span></span>

    ![Podokno vlastností účtu Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)
3. <span data-ttu-id="ed042-237">V okně vlastností **Účty Spustit jako** vyberte účet Spustit jako nebo účet Spustit jako pro Classic, pro který chcete obnovit certifikát.</span><span class="sxs-lookup"><span data-stu-id="ed042-237">On the **Run As Accounts** properties blade, select either the Run As account or the Classic Run As account that you want to renew the certificate for.</span></span>

4. <span data-ttu-id="ed042-238">V okně **Vlastnosti** vybraného účtu klikněte na **Obnovit certifikát**.</span><span class="sxs-lookup"><span data-stu-id="ed042-238">On the **Properties** blade for the selected account, click **Renew certificate**.</span></span>

    ![Obnovení certifikátu pro účet Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)

5. <span data-ttu-id="ed042-240">Zatímco se certifikát obnovuje, můžete průběh sledovat v nabídce v části **Oznámení**.</span><span class="sxs-lookup"><span data-stu-id="ed042-240">While the certificate is being renewed, you can track the progress under **Notifications** from the menu.</span></span>

### <a name="delete-a-run-as-or-classic-run-as-account"></a><span data-ttu-id="ed042-241">Odstranění účet Spustit jako nebo Spustit jako pro Classic</span><span class="sxs-lookup"><span data-stu-id="ed042-241">Delete a Run As or Classic Run As account</span></span>
<span data-ttu-id="ed042-242">Tato část popisuje, jak odstranit a znovu vytvořit účet Spustit jako nebo účet Spustit jako pro Classic.</span><span class="sxs-lookup"><span data-stu-id="ed042-242">This section describes how to delete and re-create a Run As or Classic Run As account.</span></span> <span data-ttu-id="ed042-243">Při provedení této akce se účet Automation uchová.</span><span class="sxs-lookup"><span data-stu-id="ed042-243">When you perform this action, the Automation account is retained.</span></span> <span data-ttu-id="ed042-244">Odstraněný účet Spustit jako nebo účet Spustit jako pro Classic můžete znovu vytvořit na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ed042-244">After you delete a Run As or Classic Run As account, you can re-create it in the Azure portal.</span></span>

1. <span data-ttu-id="ed042-245">Na webu Azure Portal otevřete účet Automation.</span><span class="sxs-lookup"><span data-stu-id="ed042-245">In the Azure portal, open the Automation account.</span></span>

2. <span data-ttu-id="ed042-246">V okně **Účet Automation** v podokně vlastností účtu vyberte **Účty Spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="ed042-246">On the **Automation account** blade, in the account properties pane, select **Run As Accounts**.</span></span>

3. <span data-ttu-id="ed042-247">V okně vlastností **Účty Spustit jako** vyberte účet Spustit jako nebo účet Spustit jako pro Classic, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="ed042-247">On the **Run As Accounts** properties blade, select either the Run As account or Classic Run As account that you want to delete.</span></span> <span data-ttu-id="ed042-248">Potom v okně **Vlastnosti** vybraného účtu klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="ed042-248">Then, on the **Properties** blade for the selected account, click **Delete**.</span></span>

 ![Odstranění účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)

4. <span data-ttu-id="ed042-250">Zatímco se účet odstraňuje, můžete průběh sledovat v nabídce v části **Oznámení**.</span><span class="sxs-lookup"><span data-stu-id="ed042-250">While the account is being deleted, you can track the progress under **Notifications** from the menu.</span></span>

5. <span data-ttu-id="ed042-251">Účet po odstranění můžete znovu vytvořit v okně vlastností **Účty Spustit jako** výběrem možnosti Vytvořit v části **Účet Spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="ed042-251">After the account has been deleted, you can re-create it on the **Run As Accounts** properties blade by selecting the create option **Azure Run As Account**.</span></span>

 ![Znovuvytvoření účtu Automation Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)

### <a name="misconfiguration"></a><span data-ttu-id="ed042-253">Chybná konfigurace</span><span class="sxs-lookup"><span data-stu-id="ed042-253">Misconfiguration</span></span>
<span data-ttu-id="ed042-254">Může se stát, že se během prvotního nastavení nesprávně vytvoří nebo později odstraní některá z položek konfigurace nezbytných pro správné fungování účtu Spustit jako nebo Spustit jako pro Classic.</span><span class="sxs-lookup"><span data-stu-id="ed042-254">Some configuration items necessary for the Run As or Classic Run As account to function properly might have been deleted or created improperly during initial setup.</span></span> <span data-ttu-id="ed042-255">Mezi tyto položky patří:</span><span class="sxs-lookup"><span data-stu-id="ed042-255">The items include:</span></span>

* <span data-ttu-id="ed042-256">Asset certifikátu</span><span class="sxs-lookup"><span data-stu-id="ed042-256">Certificate asset</span></span>
* <span data-ttu-id="ed042-257">Asset připojení</span><span class="sxs-lookup"><span data-stu-id="ed042-257">Connection asset</span></span>
* <span data-ttu-id="ed042-258">Účet Spustit jako byl odebrán z role přispěvatele</span><span class="sxs-lookup"><span data-stu-id="ed042-258">Run As account has been removed from the contributor role</span></span>
* <span data-ttu-id="ed042-259">Instanční objekt nebo aplikace v Azure AD</span><span class="sxs-lookup"><span data-stu-id="ed042-259">Service principal or application in Azure AD</span></span>

<span data-ttu-id="ed042-260">V předchozí a dalších instancích chybné konfigurace účet Automation zjistí změny a v okně vlastností *Účty Spustit jako* příslušného účtu zobrazí stav **Nedokončeno**.</span><span class="sxs-lookup"><span data-stu-id="ed042-260">In the preceding and other instances of misconfiguration, the Automation account detects the changes and displays a status of *Incomplete* on the **Run As Accounts** properties blade for the account.</span></span>

![Nedokončená konfigurace účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)

<span data-ttu-id="ed042-262">Pokud vyberete tento účet Spustit jako, v podokně **Vlastnosti** účtu se zobrazí následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="ed042-262">When you select the Run As account, the account **Properties** pane displays the following error message:</span></span>

![Zpráva upozornění o nedokončené konfiguraci účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png)<span data-ttu-id="ed042-264">.</span><span class="sxs-lookup"><span data-stu-id="ed042-264">.</span></span>

<span data-ttu-id="ed042-265">Tyto potíže s účtem Spustit jako můžete rychle vyřešit jeho odstraněním a znovuvytvořením.</span><span class="sxs-lookup"><span data-stu-id="ed042-265">You can quickly resolve these Run As account issues by deleting and re-creating the account.</span></span>

## <a name="update-your-automation-account-by-using-powershell"></a><span data-ttu-id="ed042-266">Aktualizace účtu Automation pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="ed042-266">Update your Automation account by using PowerShell</span></span>
<span data-ttu-id="ed042-267">Účet Automation můžete aktualizovat pomocí PowerShellu, pokud jste postupovali takto:</span><span class="sxs-lookup"><span data-stu-id="ed042-267">You can use PowerShell to update your existing Automation account if:</span></span>

* <span data-ttu-id="ed042-268">Vytvořili jste účet Automation, ale odmítli jste vytvoření účtu Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="ed042-268">You create an Automation account but decline to create the Run As account.</span></span>
* <span data-ttu-id="ed042-269">Už máte účet Automation pro správu prostředků Resource Manageru a chcete ho aktualizovat, aby zahrnoval účet Spustit jako pro ověřování runbooků.</span><span class="sxs-lookup"><span data-stu-id="ed042-269">You already use an Automation account to manage Resource Manager resources and you want to update the account to include the Run As account for runbook authentication.</span></span>
* <span data-ttu-id="ed042-270">Už máte účet Automation pro správu klasických prostředků a chcete ho aktualizovat, abyste mohli použít účet Spustit jako pro Classic a nemuseli vytvářet nový účet a migrovat na něj runbooky a prostředky.</span><span class="sxs-lookup"><span data-stu-id="ed042-270">You already use an Automation account to manage classic resources and you want to update it to use the Classic Run As account instead of creating a new account and migrating your runbooks and assets to it.</span></span>   
* <span data-ttu-id="ed042-271">Rozhodli jste se vytvořit účet Spustit jako a účet Spustit jako pro Classic pomocí certifikátu, který vydala vaše podniková certifikační autorita.</span><span class="sxs-lookup"><span data-stu-id="ed042-271">You want to create a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

<span data-ttu-id="ed042-272">Tento skript má následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="ed042-272">The script has the following prerequisites:</span></span>

* <span data-ttu-id="ed042-273">Tento skript je možné spustit jenom v systémech Windows 10 a Windows Server 2016 s nainstalovanými moduly Azure Resource Manageru verze 2.01 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ed042-273">The script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 2.01 and later.</span></span> <span data-ttu-id="ed042-274">V předchozích verzích Windows není podporován.</span><span class="sxs-lookup"><span data-stu-id="ed042-274">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="ed042-275">Azure PowerShell 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ed042-275">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="ed042-276">Informace o vydání PowerShellu 1.0 najdete v článku [Postup instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ed042-276">For information about the PowerShell 1.0 release, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="ed042-277">Účet Automation, na který se odkazuje jako na hodnotu parametrů *-AutomationAccountName* a *-ApplicationDisplayName* v následujícím skriptu PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="ed042-277">An Automation account, which is referenced as the value for the *–AutomationAccountName* and *-ApplicationDisplayName* parameters in the following PowerShell script.</span></span>

<span data-ttu-id="ed042-278">Abyste získali hodnoty pro parametry *SubscriptionID*, *ResourceGroup* a *AutomationAccountName*, které jsou pro skripty povinné, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ed042-278">To get the values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for the scripts, do the following:</span></span>
1. <span data-ttu-id="ed042-279">Na webu Azure Portal v okně **Účet Automation** vyberte příslušný účet Automation a potom vyberte **Všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ed042-279">In the Azure portal, select your Automation account on the **Automation account** blade, and then select **All settings**.</span></span> 
2. <span data-ttu-id="ed042-280">V okně **Všechna nastavení** v části **Nastavení účtu** vyberte **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="ed042-280">On the **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="ed042-281">Hodnoty v okně **Vlastnosti** si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="ed042-281">Note the values on the **Properties** blade.</span></span>

![Podokno vlastností účtu Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-a-run-as-account-powershell-script"></a><span data-ttu-id="ed042-283">Vytvoření powershellového skriptu pro účet Spustit jako</span><span class="sxs-lookup"><span data-stu-id="ed042-283">Create a Run As account PowerShell script</span></span>
<span data-ttu-id="ed042-284">Tento skript PowerShellu zahrnuje podporu následujících konfigurací:</span><span class="sxs-lookup"><span data-stu-id="ed042-284">This PowerShell script includes support for the following configurations:</span></span>

* <span data-ttu-id="ed042-285">Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="ed042-285">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="ed042-286">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="ed042-286">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="ed042-287">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu</span><span class="sxs-lookup"><span data-stu-id="ed042-287">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="ed042-288">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem v cloudu Azure Government</span><span class="sxs-lookup"><span data-stu-id="ed042-288">Create a Run As account and a Classic Run As account by using a self-signed certificate in the Azure Government cloud.</span></span>

<span data-ttu-id="ed042-289">V závislosti na možnosti konfigurace, kterou vyberete, skript vytvoří následující položky.</span><span class="sxs-lookup"><span data-stu-id="ed042-289">Depending on the configuration option you select, the script creates the following items.</span></span>

<span data-ttu-id="ed042-290">**Pro účty Spustit jako:**</span><span class="sxs-lookup"><span data-stu-id="ed042-290">**For Run As accounts:**</span></span>

* <span data-ttu-id="ed042-291">Vytvoří aplikaci Azure AD, která se exportuje s veřejným klíčem certifikátu podepsaného svým držitelem nebo podnikového certifikátu, vytvoří účet instančního objektu pro tuto aplikaci v Azure AD a přiřadí roli přispěvatele pro tento účet v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="ed042-291">Creates an Azure AD application to be exported with either the self-signed or enterprise certificate public key, creates a service principal account for the application in Azure AD, and assigns the Contributor role for the account in your current subscription.</span></span> <span data-ttu-id="ed042-292">Toto nastavení můžete změnit na roli Vlastník nebo libovolnou jinou roli.</span><span class="sxs-lookup"><span data-stu-id="ed042-292">You can change this setting to Owner or any other role.</span></span> <span data-ttu-id="ed042-293">Další informace najdete v tématu [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="ed042-293">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="ed042-294">V příslušném účtu Automation vytvoří prostředek certifikátu Automation s názvem *AzureRunAsCertificate*.</span><span class="sxs-lookup"><span data-stu-id="ed042-294">Creates an Automation certificate asset named *AzureRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="ed042-295">Prostředek certifikátu obsahuje privátní klíč certifikátu, který používá aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ed042-295">The certificate asset holds the certificate private key that's used by the Azure AD application.</span></span>
* <span data-ttu-id="ed042-296">V příslušném účtu Automation vytvoří prostředek připojení Automation s názvem *AzureRunAsConnection*.</span><span class="sxs-lookup"><span data-stu-id="ed042-296">Creates an Automation connection asset named *AzureRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="ed042-297">Prostředek připojení obsahuje parametry applicationId, tenantId, subscriptionId a certificate thumbprint.</span><span class="sxs-lookup"><span data-stu-id="ed042-297">The connection asset holds the applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="ed042-298">**Pro účet Spustit jako pro Classic:**</span><span class="sxs-lookup"><span data-stu-id="ed042-298">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="ed042-299">V příslušném účtu Automation vytvoří prostředek certifikátu Automation s názvem *AzureClassicRunAsCertificate*.</span><span class="sxs-lookup"><span data-stu-id="ed042-299">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="ed042-300">Prostředek certifikátu obsahuje privátní klíč certifikátu, který používá certifikát pro správu.</span><span class="sxs-lookup"><span data-stu-id="ed042-300">The certificate asset holds the certificate private key used by the management certificate.</span></span>
* <span data-ttu-id="ed042-301">V příslušném účtu Automation vytvoří prostředek připojení Automation s názvem *AzureClassicRunAsConnection*.</span><span class="sxs-lookup"><span data-stu-id="ed042-301">Creates an Automation connection asset named *AzureClassicRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="ed042-302">Prostředek propojení obsahuje název a ID předplatného a název prostředku certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ed042-302">The connection asset holds the subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="ed042-303">Pokud při vytváření účtu Spustit jako pro Classic vyberete libovolnou z těchto možností, po spuštění skriptu nahrajte veřejný certifikát (soubor s příponou .cer) do úložiště správy předplatného, ve kterém byl účet Automation vytvořený.</span><span class="sxs-lookup"><span data-stu-id="ed042-303">If you select either option for creating a Classic Run As account, after the script is executed, upload the public certificate (.cer file name extension) to the management store for the subscription that the Automation account was created in.</span></span>
> 

<span data-ttu-id="ed042-304">Pokud chcete spustit skript a nahrát certifikát, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ed042-304">To execute the script and upload the certificate, do the following:</span></span>

1. <span data-ttu-id="ed042-305">Uložte následující skript do počítače.</span><span class="sxs-lookup"><span data-stu-id="ed042-305">Save the following script on your computer.</span></span> <span data-ttu-id="ed042-306">V tomto příkladu ho uložte s názvem *New-RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="ed042-306">In this example, save it with the filename *New-RunAsAccount.ps1*.</span></span>

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
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
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
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
           Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
           return
        }

        Login-AzureRmAccount -EnvironmentName $EnvironmentName
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
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. <span data-ttu-id="ed042-307">Na svém počítači klikněte na **Start** a potom spusťte **Windows PowerShell** se zvýšenými uživatelskými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="ed042-307">On your computer, click **Start**, and then start **Windows PowerShell** with elevated user rights.</span></span>

3. <span data-ttu-id="ed042-308">V prostředí příkazového řádku PowerShellu se zvýšenými oprávněními přejděte do složky, která obsahuje skript vytvořený v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="ed042-308">From the elevated PowerShell command-line shell, go to the folder that contains the script you created in step 1.</span></span>

4. <span data-ttu-id="ed042-309">Spusťte tento skript s využitím hodnot parametrů pro požadovanou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ed042-309">Execute the script by using the parameter values for the configuration you require.</span></span>

    <span data-ttu-id="ed042-310">**Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem**</span><span class="sxs-lookup"><span data-stu-id="ed042-310">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="ed042-311">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem**</span><span class="sxs-lookup"><span data-stu-id="ed042-311">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="ed042-312">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu**</span><span class="sxs-lookup"><span data-stu-id="ed042-312">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="ed042-313">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem v cloudu Azure Government**</span><span class="sxs-lookup"><span data-stu-id="ed042-313">**Create a Run As account and a Classic Run As account by using a self-signed certificate in the Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="ed042-314">Po spuštění skriptu se zobrazí výzva k ověření pomocí Azure.</span><span class="sxs-lookup"><span data-stu-id="ed042-314">After the script has executed, you will be prompted to authenticate with Azure.</span></span> <span data-ttu-id="ed042-315">Přihlaste se pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.</span><span class="sxs-lookup"><span data-stu-id="ed042-315">Sign in with an account that is a member of the subscription administrators role and co-administrator of the subscription.</span></span>
    >
    >

<span data-ttu-id="ed042-316">Po úspěšném spuštění skriptu je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="ed042-316">After the script has executed successfully, note the following:</span></span>
* <span data-ttu-id="ed042-317">Pokud jste vytvořili účet Spustit jako pro Classic s využitím veřejného certifikátu podepsaného svým držitelem (soubor .cer), skript ho vytvoří a uloží ve složce dočasných souborů ve vašem počítači pod profilem uživatele *%USERPROFILE%\AppData\Local\Temp*, který používáte ke spuštění relace PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="ed042-317">If you created a Classic Run As account with a self-signed public certificate (.cer file), the script creates and saves it to the temporary files folder on your computer under the user profile *%USERPROFILE%\AppData\Local\Temp*, which you used to execute the PowerShell session.</span></span>
* <span data-ttu-id="ed042-318">Pokud jste vytvořili účet Spustit jako pro Classic s využitím podnikového veřejného certifikátu (soubor .cer), použijte tento certifikát.</span><span class="sxs-lookup"><span data-stu-id="ed042-318">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="ed042-319">Postupujte podle kroků pro [odeslání certifikátu rozhraní API pro správu na portál Azure Classic](../azure-api-management-certs.md) a potom použijte [ukázkový kód pro ověření pomocí prostředků správy služeb](#sample-code-to-authenticate-with-service-management-resources) k ověření konfigurace přihlašovacích údajů pomocí prostředků správy služeb.</span><span class="sxs-lookup"><span data-stu-id="ed042-319">Follow the instructions for [uploading a management API certificate to the Azure classic portal](../azure-api-management-certs.md), and then validate the credential configuration with Service Management resources by using the [sample code to authenticate with Service Management Resources](#sample-code-to-authenticate-with-service-management-resources).</span></span> 
* <span data-ttu-id="ed042-320">Pokud jste *nevytvořili* účet Spustit jako pro Classic, použijte k ověření pomocí prostředků Resource Manageru a ke kontrole konfigurace přihlašovacích údajů [ukázkový kód pro ověření s využitím prostředků správy služeb](#sample-code-to-authenticate-with-resource-manager-resources).</span><span class="sxs-lookup"><span data-stu-id="ed042-320">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate the credential configuration by using the [sample code for authenticating with Service Management resources](#sample-code-to-authenticate-with-resource-manager-resources).</span></span>

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a><span data-ttu-id="ed042-321">Ukázkový kód pro ověření pomocí prostředků Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="ed042-321">Sample code to authenticate with Resource Manager resources</span></span>
<span data-ttu-id="ed042-322">Můžete použít následující aktualizovaný ukázkový kód z ukázkového runbooku *AzureAutomationTutorialScript* a provést ověření pomocí účtu Spustit jako, abyste mohli prostředky Resource Manageru spravovat pomocí svých runbooků.</span><span class="sxs-lookup"><span data-stu-id="ed042-322">You can use the following updated sample code, taken from the *AzureAutomationTutorialScript* example runbook, to authenticate by using the Run As account to manage Resource Manager resources with your runbooks.</span></span>

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get the connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Signing in to Azure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context to a specific subscription"     
       Set-AzureRmContext -SubscriptionId $SubId              
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    }

<span data-ttu-id="ed042-323">Skript obsahuje dva další řádky kódu, které podporují odkazování na kontext předplatného, abyste mohli lépe pracovat v prostředí s několika předplatnými.</span><span class="sxs-lookup"><span data-stu-id="ed042-323">To help you to easily work between multiple subscriptions, the script includes two additional lines of code that support referencing a subscription context.</span></span> <span data-ttu-id="ed042-324">Prostředek proměnné s názvem *SubscriptionId* obsahuje ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="ed042-324">A variable asset named *SubscriptionId* contains the ID of the subscription.</span></span> <span data-ttu-id="ed042-325">Po příkazu rutiny `Add-AzureRmAccount` bude uvedená rutina [`Set-AzureRmContext`](/powershell/module/azurerm.profile/set-azurermcontext) s nastaveným parametrem *-SubscriptionId*.</span><span class="sxs-lookup"><span data-stu-id="ed042-325">After the `Add-AzureRmAccount` cmdlet statement, the [`Set-AzureRmContext`](/powershell/module/azurerm.profile/set-azurermcontext) cmdlet is stated with the parameter set *-SubscriptionId*.</span></span> <span data-ttu-id="ed042-326">Pokud je název proměnné příliš obecný, můžete ho upravit tak, že přidáte předponu nebo použijete jinou zásadu vytváření názvů, abyste tuto proměnnou dokázali lépe identifikovat.</span><span class="sxs-lookup"><span data-stu-id="ed042-326">If the variable name is too generic, you can revise it to include a prefix or use another naming convention to make it easier to identify.</span></span> <span data-ttu-id="ed042-327">Alternativně můžete místo *-SubscriptionId* použít sadu parametrů *-SubscriptionName* s odpovídajícím prostředkem proměnné.</span><span class="sxs-lookup"><span data-stu-id="ed042-327">Alternatively, you can use the parameter set *-SubscriptionName* instead of *-SubscriptionId* with a corresponding variable asset.</span></span>

<span data-ttu-id="ed042-328">Rutina, kterou používáte pro ověřování v runbooku, `Add-AzureRmAccount`, používá sadu parametrů *ServicePrincipalCertificate*.</span><span class="sxs-lookup"><span data-stu-id="ed042-328">The cmdlet that you use for authenticating in the runbook, `Add-AzureRmAccount`, uses the *ServicePrincipalCertificate* parameter set.</span></span> <span data-ttu-id="ed042-329">Ověřování provádí pomocí certifikátu instančního objektu, a ne pomocí přihlašovacích údajů uživatele.</span><span class="sxs-lookup"><span data-stu-id="ed042-329">It authenticates by using the service principal certificate, not the user credentials.</span></span>

## <a name="sample-code-to-authenticate-with-service-management-resources"></a><span data-ttu-id="ed042-330">Ukázkový kód pro ověření pomocí prostředků správy služby</span><span class="sxs-lookup"><span data-stu-id="ed042-330">Sample code to authenticate with Service Management resources</span></span>
<span data-ttu-id="ed042-331">Můžete použít následující aktualizovaný ukázkový kód z ukázkového runbooku *AzureClassicAutomationTutorialScript* a provést ověření pomocí účtu Spustit jako pro Classic, abyste mohli klasické prostředky spravovat pomocí svých runbooků.</span><span class="sxs-lookup"><span data-stu-id="ed042-331">You can use the following updated sample code, which is taken from the *AzureClassicAutomationTutorialScript* example runbook, to authenticate by using the Classic Run As account to manage classic resources with your runbooks.</span></span>

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID

## <a name="next-steps"></a><span data-ttu-id="ed042-332">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed042-332">Next steps</span></span>
* [<span data-ttu-id="ed042-333">Aplikační a instanční objekty v Azure AD</span><span class="sxs-lookup"><span data-stu-id="ed042-333">Application and service principal objects in Azure AD</span></span>](../active-directory/active-directory-application-objects.md)
* [<span data-ttu-id="ed042-334">Řízení přístupu na základě role ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="ed042-334">Role-based access control in Azure Automation</span></span>](automation-role-based-access-control.md)
* [<span data-ttu-id="ed042-335">Přehled certifikátů pro Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="ed042-335">Certificates overview for Azure Cloud Services</span></span>](../cloud-services/cloud-services-certs-create.md)
