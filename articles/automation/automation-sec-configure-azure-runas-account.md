---
title: "aaaConfigure spustit jako účet Azure | Microsoft Docs"
description: "Tento kurz vás provede procesem vytváření, testování a ukázkovým použitím hello objekt zabezpečení ověřování ve službě Azure Automation."
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
redirect_document_id: True
ms.openlocfilehash: 06675d2f6b9d8e7260ffaead4f2b2f61c2b98d13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-runbooks-with-an-azure-run-as-account"></a><span data-ttu-id="d59c2-104">Ověření runbooků pomocí účtu Spustit jako pro Azure</span><span class="sxs-lookup"><span data-stu-id="d59c2-104">Authenticate runbooks with an Azure Run As account</span></span>
<span data-ttu-id="d59c2-105">Tento článek ukazuje, jak tooconfigure Azure Automation účet v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d59c2-105">This article shows you how tooconfigure an Azure Automation account in hello Azure portal.</span></span> <span data-ttu-id="d59c2-106">toodo tedy můžete používat hello spustit jako účet funkce tooauthenticate runbooky správu prostředků v Azure Resource Manageru nebo Azure Service Management.</span><span class="sxs-lookup"><span data-stu-id="d59c2-106">toodo so, you use hello Run As account feature tooauthenticate runbooks managing resources in either Azure Resource Manager or Azure Service Management.</span></span>

<span data-ttu-id="d59c2-107">Když vytvoříte účet služby Automation v hello portálu Azure, můžete automaticky vytvořit dva účty:</span><span class="sxs-lookup"><span data-stu-id="d59c2-107">When you create an Automation account in hello Azure portal, you automatically create two accounts:</span></span>

* <span data-ttu-id="d59c2-108">Účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="d59c2-108">A Run As account.</span></span> <span data-ttu-id="d59c2-109">Tento účet vytvoří instanční objekt ve službě Azure Active Directory (Azure AD) a certifikát.</span><span class="sxs-lookup"><span data-stu-id="d59c2-109">This account creates a service principal in Azure Active Directory (Azure AD) and a certificate.</span></span> <span data-ttu-id="d59c2-110">Také přiřadí hello Přispěvatel přístupu na základě role řízení (RBAC), která spravuje prostředky Resource Manageru pomocí sad runbook.</span><span class="sxs-lookup"><span data-stu-id="d59c2-110">It also assigns hello Contributor role-based access control (RBAC), which manages Resource Manager resources by using runbooks.</span></span>
* <span data-ttu-id="d59c2-111">Účet Spustit jako pro Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="d59c2-111">A Classic Run As account.</span></span> <span data-ttu-id="d59c2-112">Tento účet odešle certifikát správy, který je použit toomanage Service Management nebo klasické prostředky pomocí sad runbook.</span><span class="sxs-lookup"><span data-stu-id="d59c2-112">This account uploads a management certificate, which is used toomanage Service Management or classic resources by using runbooks.</span></span>

<span data-ttu-id="d59c2-113">Vytváření Automation účet zjednodušuje proces hello a pomáhá rychle začít vytvářet a nasazovat runbooky toosupport vaše automation potřebuje.</span><span class="sxs-lookup"><span data-stu-id="d59c2-113">Creating an Automation account simplifies hello process for you and helps you quickly start building and deploying runbooks toosupport your automation needs.</span></span>

<span data-ttu-id="d59c2-114">Pomocí účtu Spustit jako a účtu Spustit jako pro Azure Classic můžete:</span><span class="sxs-lookup"><span data-stu-id="d59c2-114">With Run As and Classic Run As accounts, you can:</span></span>

* <span data-ttu-id="d59c2-115">Poskytovat standardizovaného způsobu tooauthenticate s Azure při správě správce prostředků nebo služby správy prostředků ze sady runbook v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d59c2-115">Provide a standardized way tooauthenticate with Azure when you manage Resource Manager or Service Management resources from runbooks in hello Azure portal.</span></span>
* <span data-ttu-id="d59c2-116">Automatizovat hello používání globálních runbooků, které můžete nakonfigurovat ve výstrahách Azure.</span><span class="sxs-lookup"><span data-stu-id="d59c2-116">Automate hello use of global runbooks, which you can configure in Azure Alerts.</span></span>

> [!NOTE]
> <span data-ttu-id="d59c2-117">Hello [Azure výstrah funkce integrace](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) s automatizace globálních runbooků vyžaduje účet Automation, který je nakonfigurovaný s účet Spustit jako a účet Classic spustit jako.</span><span class="sxs-lookup"><span data-stu-id="d59c2-117">hello [Azure Alert integration feature](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) with Automation global runbooks requires an Automation account that's configured with a Run As account and a Classic Run As account.</span></span> <span data-ttu-id="d59c2-118">Můžete vybrat účet Automation, který již má definovaný účty spustit jako a Classic spustit jako, nebo můžete toocreate nového účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="d59c2-118">You can select an Automation account that already has defined Run As and Classic Run As accounts, or you can choose toocreate a new Automation account.</span></span>
>  

<span data-ttu-id="d59c2-119">Tento článek ukazuje, jak toocreate účtu Automation na portálu Azure, hello aktualizace účtu Automation pomocí prostředí Azure PowerShell, spravovat konfiguraci účtu hello a ověřit svoje runbooky.</span><span class="sxs-lookup"><span data-stu-id="d59c2-119">This article shows how toocreate an Automation account from hello Azure portal, update an Automation account by using Azure PowerShell, manage hello account configuration, and authenticate in your runbooks.</span></span>

<span data-ttu-id="d59c2-120">Než začnete, vytvoření účtu Automation, je vhodné toounderstand a zvažte následující hello:</span><span class="sxs-lookup"><span data-stu-id="d59c2-120">Before you begin creating an Automation account, it's a good idea toounderstand and consider hello following:</span></span>

* <span data-ttu-id="d59c2-121">Vytvoření účtu Automation nemá vliv na účty služby Automation, které mohou již jste vytvořili v hello classic nebo modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d59c2-121">Creating an Automation account does not affect Automation accounts you might have already created in either hello classic or Resource Manager deployment model.</span></span>
* <span data-ttu-id="d59c2-122">proces Hello funguje pouze pro účty Automation, které vytvoříte v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d59c2-122">hello process works only for Automation accounts that you create in hello Azure portal.</span></span> <span data-ttu-id="d59c2-123">Probíhá pokus toocreate účtu hello portál Azure classic se nereplikuje konfigurace účtu spustit jako hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-123">Attempting toocreate an account from hello Azure classic portal does not replicate hello Run As account configuration.</span></span>
* <span data-ttu-id="d59c2-124">Pokud už máte runbooky a prostředky (například plány nebo proměnné) v místní toomanage klasické prostředky, a chcete tooauthenticate sady runbook s hello nový Classic účet Spustit jako, proveďte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="d59c2-124">If you already have runbooks and assets (such as schedules or variables) in place toomanage classic resources, and you want runbooks tooauthenticate with hello new Classic Run As account, do either of hello following:</span></span>

  * <span data-ttu-id="d59c2-125">toocreate účet Classic spustit jako, postupujte podle pokynů hello v části "Správa vašeho účet Spustit jako" hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-125">toocreate a Classic Run As account, follow hello instructions in hello "Managing your Run As account" section.</span></span> 
  * <span data-ttu-id="d59c2-126">tooupdate existující účet, použijte hello prostředí PowerShell skript v části "Aktualizace účtu Automation pomocí prostředí PowerShell" hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-126">tooupdate your existing account, use hello PowerShell script in hello "Update your Automation account by using PowerShell" section.</span></span>
* <span data-ttu-id="d59c2-127">tooauthenticate pomocí hello nový účet Spustit jako a Classic spustit jako automatizace účtu, je nutné toomodify vaší existující sady runbook s hello ukázkový kód, které jsou uvedeny v části hello [příklady ověřovacího kódu](#authentication-code-examples).</span><span class="sxs-lookup"><span data-stu-id="d59c2-127">tooauthenticate by using hello new Run As account and Classic Run As Automation account, you  need toomodify your existing runbooks with hello example code provided in hello section [Authentication code examples](#authentication-code-examples).</span></span>

    >[!NOTE]
    ><span data-ttu-id="d59c2-128">Hello účet Spustit jako je pro ověřování s prostředky Resource Manager pomocí hello založené na certifikátech instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="d59c2-128">hello Run As account is for authentication against Resource Manager resources using hello certificate-based service principal.</span></span> <span data-ttu-id="d59c2-129">Hello Classic spustit jako účtu je pro ověřování proti zdroje informací pro správu služby pomocí certifikátu správy.</span><span class="sxs-lookup"><span data-stu-id="d59c2-129">hello Classic Run As account is for authenticating against Service Management resources with a management certificate.</span></span>

## <a name="create-an-automation-account-from-hello-azure-portal"></a><span data-ttu-id="d59c2-130">Vytvoření účtu Automation z hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d59c2-130">Create an Automation account from hello Azure portal</span></span>
<span data-ttu-id="d59c2-131">V této části vytvoříte účet služby Azure Automation z hello portál Azure, které pak vytvoří účet Spustit jako a účet Classic spustit jako.</span><span class="sxs-lookup"><span data-stu-id="d59c2-131">In this section, you create an Azure Automation account from hello Azure portal, which in turn creates both a Run As account and a Classic Run As account.</span></span>

>[!NOTE]
><span data-ttu-id="d59c2-132">toocreate účet Automation, musíte být členem role Správci služby hello správce nebo spolusprávce předplatného hello je udělení přístupu toohello předplatné.</span><span class="sxs-lookup"><span data-stu-id="d59c2-132">toocreate an Automation account, you must be a member of hello Service Admins role or co-administrator of hello subscription that is granting access toohello subscription.</span></span> <span data-ttu-id="d59c2-133">Je třeba přidat také jako instanci Active Directory výchozí předplatné toothat uživatele.</span><span class="sxs-lookup"><span data-stu-id="d59c2-133">You must also be added as a user toothat subscription's default Active Directory instance.</span></span> <span data-ttu-id="d59c2-134">účet Hello nemusí toobe přiřadit privilegované role.</span><span class="sxs-lookup"><span data-stu-id="d59c2-134">hello account does not need toobe assigned a privileged role.</span></span>
>
><span data-ttu-id="d59c2-135">Pokud si nejste členem hello předplatné instanci Active Directory, než se přidají toohello role spolusprávcem předplatného hello, bude možné přidat tooActive Directory jako Host.</span><span class="sxs-lookup"><span data-stu-id="d59c2-135">If you are not a member of hello subscription’s Active Directory instance before you are added toohello co-administrator role of hello subscription, you will be added tooActive Directory as a guest.</span></span> <span data-ttu-id="d59c2-136">V této instanci zobrazí se "Nemáte oprávnění toocreate..."</span><span class="sxs-lookup"><span data-stu-id="d59c2-136">In this instance, you will receive a “You do not have permissions toocreate…”</span></span> <span data-ttu-id="d59c2-137">upozornění na hello **přidat účet Automation** okno.</span><span class="sxs-lookup"><span data-stu-id="d59c2-137">warning on hello **Add Automation Account** blade.</span></span>
>
><span data-ttu-id="d59c2-138">Uživatelé, kteří byly přidány toohello spolusprávcem role nejprve lze odebrat z instance služby Active Directory hello předplatného a znova se přidal toomake je úplné uživatelské ve službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d59c2-138">Users who were added toohello co-administrator role first can be removed from hello subscription's Active Directory instance and re-added toomake them a full User in Active Directory.</span></span> <span data-ttu-id="d59c2-139">tooverify této situaci z hello **Azure Active Directory** poddokně na portálu Azure tak, že vyberete hello **uživatelů a skupin**, vyberete **všichni uživatelé** a po výběru Hello konkrétního uživatele, vyberete **profil**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-139">tooverify this situation from hello **Azure Active Directory** pane in hello Azure portal by selecting **Users and groups**, selecting **All users** and, after you select hello specific user, selecting **Profile**.</span></span> <span data-ttu-id="d59c2-140">Hello hodnotu hello **typ uživatele** atribut v profilu uživatele hello nesmí rovnat **hosta**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-140">hello value of hello **User type** attribute under hello users profile should not equal **Guest**.</span></span>
>

1. <span data-ttu-id="d59c2-141">Přihlaste se toohello portálu Azure pomocí účtu, který je členem role Správci předplatného hello a spolusprávce předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-141">Sign in toohello Azure portal with an account that is a member of hello subscription administrators role and co-administrator of hello subscription.</span></span>

2. <span data-ttu-id="d59c2-142">Vyberte **Účty Automation**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-142">Select **Automation Accounts**.</span></span>

3. <span data-ttu-id="d59c2-143">Na hello **účty Automation** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-143">On hello **Automation Accounts** blade, click **Add**.</span></span>
<span data-ttu-id="d59c2-144">Hello **přidat účet Automation** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="d59c2-144">hello **Add Automation Account** blade opens.</span></span>

 ![okno "Přidat účet Automation" Hello](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)

   > [!NOTE]
   > <span data-ttu-id="d59c2-146">Pokud váš účet není členem role Správci předplatného hello a spolusprávce předplatného hello, hello následující upozornění se zobrazí na hello **přidat účet Automation** okno:</span><span class="sxs-lookup"><span data-stu-id="d59c2-146">If your account is not a member of hello subscription administrators role and co-administrator of hello subscription, hello following warning is displayed on hello **Add Automation Account** blade:</span></span>
   >
   >![Přidání upozornění k účtu Automation](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   >
   >

4. <span data-ttu-id="d59c2-148">Na hello **přidat účet Automation** okno, v hello **název** pole, zadejte název nového účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="d59c2-148">On hello **Add Automation Account** blade, in hello **Name** box, type a name for your new Automation account.</span></span>

5. <span data-ttu-id="d59c2-149">Pokud máte více než jedno předplatné, hello následující:</span><span class="sxs-lookup"><span data-stu-id="d59c2-149">If you have more than one subscription, do hello following:</span></span>

    <span data-ttu-id="d59c2-150">a.</span><span class="sxs-lookup"><span data-stu-id="d59c2-150">a.</span></span> <span data-ttu-id="d59c2-151">V části **předplatné**, zadejte jednu pro nový účet hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-151">Under **Subscription**, specify one for hello new account.</span></span>

    <span data-ttu-id="d59c2-152">b.</span><span class="sxs-lookup"><span data-stu-id="d59c2-152">b.</span></span> <span data-ttu-id="d59c2-153">V části **Skupina prostředků** klikněte na **Vytvořit novou** nebo **Použít stávající**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-153">Under **Resource Group**, click **Create new** or **Use existing**.</span></span>

    <span data-ttu-id="d59c2-154">c.</span><span class="sxs-lookup"><span data-stu-id="d59c2-154">c.</span></span> <span data-ttu-id="d59c2-155">V části **Umístění** zadejte datové centrum Azure.</span><span class="sxs-lookup"><span data-stu-id="d59c2-155">Under **Location**, specify an Azure datacenter.</span></span>

6. <span data-ttu-id="d59c2-156">V části **Vytvořit účet Spustit v Azure jako** vyberte **Ano** a potom klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-156">Under **Create Azure Run As account**, select **Yes**, and then click **Create**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d59c2-157">Pokud se rozhodnete není toocreate hello účet Spustit jako výběrem **ne**, se zobrazí zpráva s upozorněním hello **přidat účet Automation** okno.</span><span class="sxs-lookup"><span data-stu-id="d59c2-157">If you choose not toocreate hello Run As account by selecting **No**, a warning message is displayed hello **Add Automation Account** blade.</span></span> <span data-ttu-id="d59c2-158">I když hello účet je vytvořen v hello portálu Azure, nemá odpovídající identitu ověřování v rámci vaší classic nebo Resource Manager předplatné adresářové služby.</span><span class="sxs-lookup"><span data-stu-id="d59c2-158">Although hello account is created in hello Azure portal, it does not have a corresponding authentication identity within your classic or Resource Manager subscription directory service.</span></span> <span data-ttu-id="d59c2-159">V důsledku toho hello účet nemá žádná tooresources přístup v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="d59c2-159">Consequently, hello account has no access tooresources in your subscription.</span></span> <span data-ttu-id="d59c2-160">Všechny runbooky odkazující na tento účet kvůli tomu nebudou moct ověřit a provádět úlohy s prostředky v těchto modelech nasazení.</span><span class="sxs-lookup"><span data-stu-id="d59c2-160">This scenario prevents any runbooks that reference this account from authenticating and performing tasks against resources in those deployment models.</span></span>
   >
   > ![Upozornění v okně "Přidat účet Automation" hello](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)
   >
   > <span data-ttu-id="d59c2-162">Kromě toho protože instanční objekt hello není vytvořen, není přiřazena role Přispěvatel hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-162">Additionally, because hello service principal is not created, hello Contributor role is not assigned.</span></span>
   >

7. <span data-ttu-id="d59c2-163">Zatímco Azure vytváří účet Automation hello, můžete sledovat průběh hello pod **oznámení** nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-163">While Azure creates hello Automation account, you can track hello progress under **Notifications** from hello menu.</span></span>

### <a name="resources"></a><span data-ttu-id="d59c2-164">Zdroje</span><span class="sxs-lookup"><span data-stu-id="d59c2-164">Resources</span></span>
<span data-ttu-id="d59c2-165">Když hello účet Automation je úspěšně vytvořen, jsou automaticky vytvoří několik prostředků.</span><span class="sxs-lookup"><span data-stu-id="d59c2-165">When hello Automation account is successfully created, several resources are automatically created for you.</span></span> <span data-ttu-id="d59c2-166">Hello prostředky jsou shrnuty v následující dvě tabulky hello:</span><span class="sxs-lookup"><span data-stu-id="d59c2-166">hello resources are summarized in hello following two tables:</span></span>

#### <a name="run-as-account-resources"></a><span data-ttu-id="d59c2-167">Prostředky účtu Spustit jako</span><span class="sxs-lookup"><span data-stu-id="d59c2-167">Run As account resources</span></span>

| <span data-ttu-id="d59c2-168">Prostředek</span><span class="sxs-lookup"><span data-stu-id="d59c2-168">Resource</span></span> | <span data-ttu-id="d59c2-169">Popis</span><span class="sxs-lookup"><span data-stu-id="d59c2-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d59c2-170">Runbook AzureAutomationTutorial</span><span class="sxs-lookup"><span data-stu-id="d59c2-170">AzureAutomationTutorial Runbook</span></span> | <span data-ttu-id="d59c2-171">Ukázkový grafický runbook, který ukazuje, jak tooauthenticate pomocí hello účet Spustit jako a získá všechny prostředky Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-171">An example graphical runbook that demonstrates how tooauthenticate by using hello Run As account and gets all hello Resource Manager resources.</span></span> |
| <span data-ttu-id="d59c2-172">Runbook AzureAutomationTutorialScript</span><span class="sxs-lookup"><span data-stu-id="d59c2-172">AzureAutomationTutorialScript Runbook</span></span> | <span data-ttu-id="d59c2-173">Ukázkový runbook prostředí PowerShell, který ukazuje, jak tooauthenticate pomocí hello účet Spustit jako a získá všechny prostředky Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-173">An example PowerShell runbook that demonstrates how tooauthenticate by using hello Run As account and gets all hello Resource Manager resources.</span></span> |
| <span data-ttu-id="d59c2-174">AzureRunAsCertificate</span><span class="sxs-lookup"><span data-stu-id="d59c2-174">AzureRunAsCertificate</span></span> | <span data-ttu-id="d59c2-175">Hello asset certifikátu, který se automaticky vytvoří při vytvoření účtu Automation nebo použijte hello následující skript prostředí PowerShell pro existující účet.</span><span class="sxs-lookup"><span data-stu-id="d59c2-175">hello certificate asset that's automatically created when you create an Automation account or use hello following PowerShell script for an existing account.</span></span> <span data-ttu-id="d59c2-176">certifikát Hello umožňuje tooauthenticate s Azure, aby mohl spravovat prostředky Azure Resource Manager ze sady runbook.</span><span class="sxs-lookup"><span data-stu-id="d59c2-176">hello certificate allows you tooauthenticate with Azure so that you can manage Azure Resource Manager resources from runbooks.</span></span> <span data-ttu-id="d59c2-177">Hello certifikát má životnost jeden rok.</span><span class="sxs-lookup"><span data-stu-id="d59c2-177">hello certificate has a one-year lifespan.</span></span> |
| <span data-ttu-id="d59c2-178">AzureRunAsConnection</span><span class="sxs-lookup"><span data-stu-id="d59c2-178">AzureRunAsConnection</span></span> | <span data-ttu-id="d59c2-179">Hello asset připojení, která se automaticky vytvoří při vytvoření účtu Automation, nebo pomocí skriptu prostředí PowerShell hello pro existující účet.</span><span class="sxs-lookup"><span data-stu-id="d59c2-179">hello connection asset that's automatically created when you create an Automation account or use hello PowerShell script for an existing account.</span></span> |

#### <a name="classic-run-as-account-resources"></a><span data-ttu-id="d59c2-180">Prostředky účtu Spustit jako pro Classic</span><span class="sxs-lookup"><span data-stu-id="d59c2-180">Classic Run As account resources</span></span>

| <span data-ttu-id="d59c2-181">Prostředek</span><span class="sxs-lookup"><span data-stu-id="d59c2-181">Resource</span></span> | <span data-ttu-id="d59c2-182">Popis</span><span class="sxs-lookup"><span data-stu-id="d59c2-182">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d59c2-183">Runbook AzureClassicAutomationTutorial</span><span class="sxs-lookup"><span data-stu-id="d59c2-183">AzureClassicAutomationTutorial Runbook</span></span> | <span data-ttu-id="d59c2-184">Ukázkový grafický runbook, který získá všechny hello virtuálních počítačů, které jsou vytvořené pomocí modelu nasazení classic hello v předplatném pomocí hello Classic účet Spustit jako (certifikátu) a pak zapíše hello název virtuálního počítače a stav.</span><span class="sxs-lookup"><span data-stu-id="d59c2-184">An example graphical runbook that gets all hello VMs that are created using hello classic deployment model in a subscription by using hello Classic Run As account (certificate), and then writes hello VM name and status.</span></span> |
| <span data-ttu-id="d59c2-185">Runbook se skriptem AzureClassicAutomationTutorial</span><span class="sxs-lookup"><span data-stu-id="d59c2-185">AzureClassicAutomationTutorial Script Runbook</span></span> | <span data-ttu-id="d59c2-186">Ukázkový runbook prostředí PowerShell, který získá všechny hello klasické virtuální počítače v předplatném pomocí hello Classic účet Spustit jako (certifikátu), a pak zápisy hello název virtuálního počítače a stav.</span><span class="sxs-lookup"><span data-stu-id="d59c2-186">An example PowerShell runbook that gets all hello classic VMs in a subscription by using hello Classic Run As account (certificate), and then writes hello VM name and status.</span></span> |
| <span data-ttu-id="d59c2-187">AzureClassicRunAsCertificate</span><span class="sxs-lookup"><span data-stu-id="d59c2-187">AzureClassicRunAsCertificate</span></span> | <span data-ttu-id="d59c2-188">asset certifikátu Hello automaticky vytvoří pomocí tooauthenticate s Azure, abyste mohli spravovat prostředky Azure classic ze sady runbook.</span><span class="sxs-lookup"><span data-stu-id="d59c2-188">hello automatically created certificate asset that you use tooauthenticate with Azure so that you can manage Azure classic resources from runbooks.</span></span> <span data-ttu-id="d59c2-189">Hello certifikát má životnost jeden rok.</span><span class="sxs-lookup"><span data-stu-id="d59c2-189">hello certificate has a one-year lifespan.</span></span> |
| <span data-ttu-id="d59c2-190">AzureClassicRunAsConnection</span><span class="sxs-lookup"><span data-stu-id="d59c2-190">AzureClassicRunAsConnection</span></span> | <span data-ttu-id="d59c2-191">prostředek Hello automaticky vytvořené připojení používat tooauthenticate službou Azure, abyste mohli spravovat prostředky Azure classic ze sady runbook.</span><span class="sxs-lookup"><span data-stu-id="d59c2-191">hello automatically created connection asset that you use tooauthenticate with Azure so that you can manage Azure classic resources from runbooks.</span></span> |

## <a name="verify-run-as-authentication"></a><span data-ttu-id="d59c2-192">Kontrola ověřování pomocí účtu Spustit jako</span><span class="sxs-lookup"><span data-stu-id="d59c2-192">Verify Run As authentication</span></span>
<span data-ttu-id="d59c2-193">Provedení testu malých tooconfirm, který úspěšně, můžete ověřovat pomocí hello nový účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="d59c2-193">Perform a small test tooconfirm that you can successfully authenticate by using hello new Run As account.</span></span>

1. <span data-ttu-id="d59c2-194">V hello portálu Azure otevřete účet Automation hello, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="d59c2-194">In hello Azure portal, open hello Automation account that you created earlier.</span></span>

2. <span data-ttu-id="d59c2-195">Klikněte na tlačítko hello **Runbooky** dlaždice tooopen hello seznamu sad runbook.</span><span class="sxs-lookup"><span data-stu-id="d59c2-195">Click hello **Runbooks** tile tooopen hello list of runbooks.</span></span>

3. <span data-ttu-id="d59c2-196">Vyberte hello **AzureAutomationTutorialScript** runbook a potom klikněte na **spustit** toostart hello runbook.</span><span class="sxs-lookup"><span data-stu-id="d59c2-196">Select hello **AzureAutomationTutorialScript** runbook, and then click **Start** toostart hello runbook.</span></span> <span data-ttu-id="d59c2-197">dojde k Hello následující události:</span><span class="sxs-lookup"><span data-stu-id="d59c2-197">hello following events occur:</span></span>
 * <span data-ttu-id="d59c2-198">A [úlohy runbooku](automation-runbook-execution.md) vytvoření hello **úlohy** zobrazí se okno a v hello se zobrazí stav úlohy hello **Souhrn úlohy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="d59c2-198">A [runbook job](automation-runbook-execution.md) is created, hello **Job** blade is displayed, and hello job status is displayed in hello **Job Summary** tile.</span></span>
 * <span data-ttu-id="d59c2-199">Stav úlohy Hello začne jako **zařazeno ve frontě**, což indikuje, že se čeká na pracovního procesu runbooku v toobecome cloudu hello k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d59c2-199">hello job status begins as **Queued**, indicating that it is waiting for a runbook worker in hello cloud toobecome available.</span></span>
 * <span data-ttu-id="d59c2-200">Stav Hello stane **počáteční** když pracovní proces úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-200">hello status becomes **Starting** when a worker claims hello job.</span></span>
 * <span data-ttu-id="d59c2-201">Stav Hello stane **systémem** při spuštění sady runbook hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-201">hello status becomes **Running** when hello runbook starts running.</span></span>
 * <span data-ttu-id="d59c2-202">Po dokončení spuštění úlohy runbooku hello měli vidět stav **dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-202">When hello runbook job has finished running, you should see a status of **Completed**.</span></span>

       ![Security Principal Runbook Test](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)
4. <span data-ttu-id="d59c2-203">toosee hello podrobné výsledky hello sady runbook, klikněte na tlačítko hello **výstup** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="d59c2-203">toosee hello detailed results of hello runbook, click hello **Output** tile.</span></span>  
<span data-ttu-id="d59c2-204">Hello **výstup** okno se zobrazí, zobrazující dané sady runbook hello úspěšně ověřen a vrátí seznam všech prostředků, které jsou k dispozici ve skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-204">hello **Output** blade is displayed, showing that hello runbook has successfully authenticated and returned a list of all resources available in hello resource group.</span></span>

5. <span data-ttu-id="d59c2-205">Zavřít hello **výstup** okno tooreturn toohello **Souhrn úlohy** okno.</span><span class="sxs-lookup"><span data-stu-id="d59c2-205">Close hello **Output** blade tooreturn toohello **Job Summary** blade.</span></span>

6. <span data-ttu-id="d59c2-206">Zavřít hello **Souhrn úlohy** okno a odpovídající hello **AzureAutomationTutorialScript** okno sady runbook.</span><span class="sxs-lookup"><span data-stu-id="d59c2-206">Close hello **Job Summary** blade and hello corresponding **AzureAutomationTutorialScript** runbook blade.</span></span>

## <a name="verify-classic-run-as-authentication"></a><span data-ttu-id="d59c2-207">Kontrola ověřování pomocí účtu Spustit jako pro Azure Classic</span><span class="sxs-lookup"><span data-stu-id="d59c2-207">Verify Classic Run As authentication</span></span>
<span data-ttu-id="d59c2-208">Provedení podobné malá testování tooconfirm, který úspěšně, můžete ověřovat pomocí hello nový Classic účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="d59c2-208">Perform a similar small test tooconfirm that you can successfully authenticate by using hello new Classic Run As account.</span></span>

1. <span data-ttu-id="d59c2-209">V hello portálu Azure otevřete účet Automation hello, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="d59c2-209">In hello Azure portal, open hello Automation account that you created earlier.</span></span>

2. <span data-ttu-id="d59c2-210">Klikněte na tlačítko hello **Runbooky** dlaždice tooopen hello seznamu sad runbook.</span><span class="sxs-lookup"><span data-stu-id="d59c2-210">Click hello **Runbooks** tile tooopen hello list of runbooks.</span></span>

3. <span data-ttu-id="d59c2-211">Vyberte hello **AzureClassicAutomationTutorialScript** runbook a potom klikněte na **spustit** příliš spuštění sady runbook hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-211">Select hello **AzureClassicAutomationTutorialScript** runbook, and then click **Start** too start hello runbook.</span></span> <span data-ttu-id="d59c2-212">dojde k Hello následující události:</span><span class="sxs-lookup"><span data-stu-id="d59c2-212">hello following events occur:</span></span>

 * <span data-ttu-id="d59c2-213">A [úlohy runbooku](automation-runbook-execution.md) vytvoření hello **úlohy** zobrazí se okno a v hello se zobrazí stav úlohy hello **Souhrn úlohy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="d59c2-213">A [runbook job](automation-runbook-execution.md) is created, hello **Job** blade is displayed, and hello job status is displayed in hello **Job Summary** tile.</span></span>
 * <span data-ttu-id="d59c2-214">Stav úlohy Hello začne jako **zařazeno ve frontě**, což indikuje, že se čeká na pracovního procesu runbooku v toobecome cloudu hello k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d59c2-214">hello job status begins as **Queued**, indicating that it is waiting for a runbook worker in hello cloud toobecome available.</span></span>
 * <span data-ttu-id="d59c2-215">Stav Hello stane **počáteční** když pracovní proces úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-215">hello status becomes **Starting** when a worker claims hello job.</span></span>
 * <span data-ttu-id="d59c2-216">Stav Hello stane **systémem** při spuštění sady runbook hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-216">hello status becomes **Running** when hello runbook starts running.</span></span>
 * <span data-ttu-id="d59c2-217">Po dokončení spuštění úlohy runbooku hello měli vidět stav **dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-217">When hello runbook job has finished running, you should see a status of **Completed**.</span></span>

    ![Test runbooku objektu zabezpečení](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
4. <span data-ttu-id="d59c2-219">toosee hello podrobné výsledky hello sady runbook, klikněte na tlačítko hello **výstup** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="d59c2-219">toosee hello detailed results of hello runbook, click hello **Output** tile.</span></span>  
<span data-ttu-id="d59c2-220">Hello **výstup** okno se zobrazí, zobrazující dané sady runbook hello úspěšně ověřen a vrátí seznam všech klasické virtuální počítače v rámci předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-220">hello **Output** blade is displayed, showing that hello runbook has successfully authenticated and returned a list of all classic VMs in hello subscription.</span></span>

5. <span data-ttu-id="d59c2-221">Zavřít hello **výstup** okno tooreturn toohello **Souhrn úlohy** okno.</span><span class="sxs-lookup"><span data-stu-id="d59c2-221">Close hello **Output** blade tooreturn toohello **Job Summary** blade.</span></span>

6. <span data-ttu-id="d59c2-222">Zavřít hello **Souhrn úlohy** okno a odpovídající hello **AzureAutomationTutorialScript** okno sady runbook.</span><span class="sxs-lookup"><span data-stu-id="d59c2-222">Close hello **Job Summary** blade and hello corresponding **AzureAutomationTutorialScript** runbook blade.</span></span>

## <a name="managing-your-run-as-account"></a><span data-ttu-id="d59c2-223">Správa účtu Spustit jako</span><span class="sxs-lookup"><span data-stu-id="d59c2-223">Managing your Run As account</span></span>
<span data-ttu-id="d59c2-224">V určitém okamžiku před vypršením platnosti účtu Automation, musíte certifikát toorenew hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-224">At some point before your Automation account expires, you will need toorenew hello certificate.</span></span> <span data-ttu-id="d59c2-225">Pokud se domníváte, že došlo k ohrožení zabezpečení tohoto hello účet Spustit jako, můžete odstranit a znovu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="d59c2-225">If you believe that hello Run As account has been compromised, you can delete and re-create it.</span></span> <span data-ttu-id="d59c2-226">Tato část pojednává o tom, jak tooperform těchto operací.</span><span class="sxs-lookup"><span data-stu-id="d59c2-226">This section discusses how tooperform these operations.</span></span>

### <a name="self-signed-certificate-renewal"></a><span data-ttu-id="d59c2-227">Obnovení certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="d59c2-227">Self-signed certificate renewal</span></span>
<span data-ttu-id="d59c2-228">certifikát podepsaný svým držitelem Hello, kterou jste vytvořili pro hello účet Spustit jako vyprší platnost jeden rok z hello data vytvoření.</span><span class="sxs-lookup"><span data-stu-id="d59c2-228">hello self-signed certificate that you created for hello Run As account expires one year from hello date of creation.</span></span> <span data-ttu-id="d59c2-229">Před vypršením platnosti ho můžete kdykoli obnovit.</span><span class="sxs-lookup"><span data-stu-id="d59c2-229">You can renew it at any time before it expires.</span></span> <span data-ttu-id="d59c2-230">Při obnovování, je aktuální platný certifikát hello udržených tooensure, že nejsou žádné sady runbook, které jsou zařazeny do fronty až nebo aktivně spuštěná, a který ověření pomocí hello účet Spustit jako, negativní vliv.</span><span class="sxs-lookup"><span data-stu-id="d59c2-230">When you renew it, hello current valid certificate is retained tooensure that any runbooks that are queued up or actively running, and that authenticate with hello Run As account, are not negatively affected.</span></span> <span data-ttu-id="d59c2-231">certifikát Hello zůstává platná do data vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="d59c2-231">hello certificate remains valid until its expiration date.</span></span>

> [!NOTE]
> <span data-ttu-id="d59c2-232">Pokud jste nakonfigurovali vaší spustit v Automation jako účet toouse certifikát vydaný certifikační autoritou rozlehlé sítě a chcete použít tuto možnost, hello podnikový certifikát budou nahrazeny certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="d59c2-232">If you have configured your Automation Run As account toouse a certificate issued by your enterprise certificate authority and you use this option, hello enterprise certificate will be replaced by a self-signed certificate.</span></span>

<span data-ttu-id="d59c2-233">toorenew hello certifikátů, hello následující:</span><span class="sxs-lookup"><span data-stu-id="d59c2-233">toorenew hello certificate, do hello following:</span></span>

1. <span data-ttu-id="d59c2-234">V hello portálu Azure otevřete účet Automation hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-234">In hello Azure portal, open hello Automation account.</span></span>

2. <span data-ttu-id="d59c2-235">Na hello **účet Automation** okno, v hello **účet vlastnosti** podokně v části **nastavení účtu**, vyberte **účty spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-235">On hello **Automation Account** blade, in hello **Account properties** pane, under **Account Settings**, select **Run As Accounts**.</span></span>

    ![Podokno vlastností účtu Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)
3. <span data-ttu-id="d59c2-237">Na hello **účty spustit jako** vlastnosti okně vyberte buď hello účet Spustit jako nebo hello Classic účet Spustit jako, které chcete toorenew hello certifikát pro.</span><span class="sxs-lookup"><span data-stu-id="d59c2-237">On hello **Run As Accounts** properties blade, select either hello Run As account or hello Classic Run As account that you want toorenew hello certificate for.</span></span>

4. <span data-ttu-id="d59c2-238">Na hello **vlastnosti** okně hello vybraný účet, klikněte na tlačítko **obnovit certifikát**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-238">On hello **Properties** blade for hello selected account, click **Renew certificate**.</span></span>

    ![Obnovení certifikátu pro účet Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)

5. <span data-ttu-id="d59c2-240">Při obnovení certifikátu hello se můžete sledovat průběh hello pod **oznámení** nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-240">While hello certificate is being renewed, you can track hello progress under **Notifications** from hello menu.</span></span>

### <a name="delete-a-run-as-or-classic-run-as-account"></a><span data-ttu-id="d59c2-241">Odstranění účet Spustit jako nebo Spustit jako pro Classic</span><span class="sxs-lookup"><span data-stu-id="d59c2-241">Delete a Run As or Classic Run As account</span></span>
<span data-ttu-id="d59c2-242">Tato část popisuje, jak toodelete a znovu vytvořit účet Spustit jako nebo Classic spustit jako.</span><span class="sxs-lookup"><span data-stu-id="d59c2-242">This section describes how toodelete and re-create a Run As or Classic Run As account.</span></span> <span data-ttu-id="d59c2-243">Když provedete tuto akci, se uchovávají hello účet Automation.</span><span class="sxs-lookup"><span data-stu-id="d59c2-243">When you perform this action, hello Automation account is retained.</span></span> <span data-ttu-id="d59c2-244">Po odstranění účtu spustit jako nebo Classic spustit jako, znovu ji vytvořte hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d59c2-244">After you delete a Run As or Classic Run As account, you can re-create it in hello Azure portal.</span></span>

1. <span data-ttu-id="d59c2-245">V hello portálu Azure otevřete účet Automation hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-245">In hello Azure portal, open hello Automation account.</span></span>

2. <span data-ttu-id="d59c2-246">Na hello **účet Automation** okno, v hello účet vlastnosti podokně, vyberte **účty spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-246">On hello **Automation account** blade, in hello account properties pane, select **Run As Accounts**.</span></span>

3. <span data-ttu-id="d59c2-247">Na hello **účty spustit jako** okno Vlastnosti, vyberte buď hello účet Spustit jako nebo Classic spustit jako účet, který má toodelete.</span><span class="sxs-lookup"><span data-stu-id="d59c2-247">On hello **Run As Accounts** properties blade, select either hello Run As account or Classic Run As account that you want toodelete.</span></span> <span data-ttu-id="d59c2-248">Potom na hello **vlastnosti** okně hello vybraný účet, klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-248">Then, on hello **Properties** blade for hello selected account, click **Delete**.</span></span>

 ![Odstranění účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)

4. <span data-ttu-id="d59c2-250">Zatímco se odstraňuje hello účet, můžete sledovat průběh hello v části **oznámení** nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-250">While hello account is being deleted, you can track hello progress under **Notifications** from hello menu.</span></span>

5. <span data-ttu-id="d59c2-251">Po odstranění účtu hello můžete znovu vytvořit ji na hello **účty spustit jako** okno Vlastnosti výběrem hello vytvořit možnost **Azure účet Spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-251">After hello account has been deleted, you can re-create it on hello **Run As Accounts** properties blade by selecting hello create option **Azure Run As Account**.</span></span>

 ![Znovu vytvořit účet Automation spustit jako hello](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)

### <a name="misconfiguration"></a><span data-ttu-id="d59c2-253">Chybná konfigurace</span><span class="sxs-lookup"><span data-stu-id="d59c2-253">Misconfiguration</span></span>
<span data-ttu-id="d59c2-254">Některé položky konfigurace potřebné pro toofunction účet Spustit jako nebo Classic spustit jako hello správně může byla odstraněna nebo nesprávně vytvořený během počáteční instalace.</span><span class="sxs-lookup"><span data-stu-id="d59c2-254">Some configuration items necessary for hello Run As or Classic Run As account toofunction properly might have been deleted or created improperly during initial setup.</span></span> <span data-ttu-id="d59c2-255">položky Hello patří:</span><span class="sxs-lookup"><span data-stu-id="d59c2-255">hello items include:</span></span>

* <span data-ttu-id="d59c2-256">Asset certifikátu</span><span class="sxs-lookup"><span data-stu-id="d59c2-256">Certificate asset</span></span>
* <span data-ttu-id="d59c2-257">Asset připojení</span><span class="sxs-lookup"><span data-stu-id="d59c2-257">Connection asset</span></span>
* <span data-ttu-id="d59c2-258">Účet Spustit jako byl odebrán z role Přispěvatel hello</span><span class="sxs-lookup"><span data-stu-id="d59c2-258">Run As account has been removed from hello contributor role</span></span>
* <span data-ttu-id="d59c2-259">Instanční objekt nebo aplikace v Azure AD</span><span class="sxs-lookup"><span data-stu-id="d59c2-259">Service principal or application in Azure AD</span></span>

<span data-ttu-id="d59c2-260">V hello předchozí a další instance chybné konfigurace, zjistí hello účet Automation hello změní a zobrazuje stav *nekompletní* na hello **účty spustit jako** okně Vlastnosti pro hello účet.</span><span class="sxs-lookup"><span data-stu-id="d59c2-260">In hello preceding and other instances of misconfiguration, hello Automation account detects hello changes and displays a status of *Incomplete* on hello **Run As Accounts** properties blade for hello account.</span></span>

![Nedokončená konfigurace účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)

<span data-ttu-id="d59c2-262">Když vyberete účet Spustit jako hello, hello účet **vlastnosti** podokně zobrazí hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="d59c2-262">When you select hello Run As account, hello account **Properties** pane displays hello following error message:</span></span>

![Zpráva upozornění o nedokončené konfiguraci účtu Spustit jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png)<span data-ttu-id="d59c2-264">.</span><span class="sxs-lookup"><span data-stu-id="d59c2-264">.</span></span>

<span data-ttu-id="d59c2-265">Odstranit a znovu vytvořit účet hello můžete rychle vyřešit tyto problémy účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="d59c2-265">You can quickly resolve these Run As account issues by deleting and re-creating hello account.</span></span>

## <a name="update-your-automation-account-by-using-powershell"></a><span data-ttu-id="d59c2-266">Aktualizace účtu Automation pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="d59c2-266">Update your Automation account by using PowerShell</span></span>
<span data-ttu-id="d59c2-267">Tooupdate prostředí PowerShell můžete použít svůj existující účet Automation, pokud:</span><span class="sxs-lookup"><span data-stu-id="d59c2-267">You can use PowerShell tooupdate your existing Automation account if:</span></span>

* <span data-ttu-id="d59c2-268">Vytvoření účtu Automation, ale odmítnout toocreate hello účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="d59c2-268">You create an Automation account but decline toocreate hello Run As account.</span></span>
* <span data-ttu-id="d59c2-269">Už používáte prostředky Resource Manager toomanage účet Automation a chcete tooupdate hello tooinclude hello spustit jako účet pro ověřování sady runbook.</span><span class="sxs-lookup"><span data-stu-id="d59c2-269">You already use an Automation account toomanage Resource Manager resources and you want tooupdate hello account tooinclude hello Run As account for runbook authentication.</span></span>
* <span data-ttu-id="d59c2-270">Už používáte toomanage účet Automation pro klasické prostředky a chcete, aby tooupdate ho toouse hello Classic spustit jako účet, místo vytvoření nového účtu a migrace tooit vaše runbooky a prostředky.</span><span class="sxs-lookup"><span data-stu-id="d59c2-270">You already use an Automation account toomanage classic resources and you want tooupdate it toouse hello Classic Run As account instead of creating a new account and migrating your runbooks and assets tooit.</span></span>   
* <span data-ttu-id="d59c2-271">Chcete toocreate spustit jako a Classic spustit jako účet pomocí certifikát vydaný certifikační autoritou (CA) rozlehlé sítě.</span><span class="sxs-lookup"><span data-stu-id="d59c2-271">You want toocreate a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

<span data-ttu-id="d59c2-272">skript Hello má hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="d59c2-272">hello script has hello following prerequisites:</span></span>

* <span data-ttu-id="d59c2-273">skript Hello lze spustit pouze na systémy Windows 10 a Windows Server 2016 s moduly Azure Resource Manager 2.01 a novější.</span><span class="sxs-lookup"><span data-stu-id="d59c2-273">hello script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 2.01 and later.</span></span> <span data-ttu-id="d59c2-274">V předchozích verzích Windows není podporován.</span><span class="sxs-lookup"><span data-stu-id="d59c2-274">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="d59c2-275">Azure PowerShell 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d59c2-275">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="d59c2-276">Informace o verzi hello PowerShell 1.0 najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d59c2-276">For information about hello PowerShell 1.0 release, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="d59c2-277">Účet služby Automation, který je odkazováno jako hodnota hello hello *-AutomationAccountName* a *- ApplicationDisplayName* parametry v hello následující skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d59c2-277">An Automation account, which is referenced as hello value for hello *–AutomationAccountName* and *-ApplicationDisplayName* parameters in hello following PowerShell script.</span></span>

<span data-ttu-id="d59c2-278">tooget hello hodnoty pro *SubscriptionID*, *ResourceGroup*, a *AutomationAccountName*, které jsou požadované parametry pro hello skripty, hello následující:</span><span class="sxs-lookup"><span data-stu-id="d59c2-278">tooget hello values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for hello scripts, do hello following:</span></span>
1. <span data-ttu-id="d59c2-279">V hello portálu Azure, vyberte svůj účet Automation na hello **účet Automation** a pak vyberte **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-279">In hello Azure portal, select your Automation account on hello **Automation account** blade, and then select **All settings**.</span></span> 
2. <span data-ttu-id="d59c2-280">Na hello **všechna nastavení** okno, v části **nastavení účtu**, vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="d59c2-280">On hello **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="d59c2-281">Všimněte si hodnot hello hello **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="d59c2-281">Note hello values on hello **Properties** blade.</span></span>

![okno "Vlastnosti" účet Automation Hello](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-a-run-as-account-powershell-script"></a><span data-ttu-id="d59c2-283">Vytvoření powershellového skriptu pro účet Spustit jako</span><span class="sxs-lookup"><span data-stu-id="d59c2-283">Create a Run As account PowerShell script</span></span>
<span data-ttu-id="d59c2-284">Tento skript prostředí PowerShell zahrnuje podporu pro hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="d59c2-284">This PowerShell script includes support for hello following configurations:</span></span>

* <span data-ttu-id="d59c2-285">Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="d59c2-285">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="d59c2-286">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="d59c2-286">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="d59c2-287">Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu</span><span class="sxs-lookup"><span data-stu-id="d59c2-287">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="d59c2-288">Vytvořte účet Spustit jako a Classic spustit jako účet pomocí certifikát podepsaný svým držitelem v hello cloudu Azure Government.</span><span class="sxs-lookup"><span data-stu-id="d59c2-288">Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud.</span></span>

<span data-ttu-id="d59c2-289">V závislosti na hello možnost konfigurace, kterou vyberete vytvoří skript hello hello následující položky.</span><span class="sxs-lookup"><span data-stu-id="d59c2-289">Depending on hello configuration option you select, hello script creates hello following items.</span></span>

<span data-ttu-id="d59c2-290">**Pro účty Spustit jako:**</span><span class="sxs-lookup"><span data-stu-id="d59c2-290">**For Run As accounts:**</span></span>

* <span data-ttu-id="d59c2-291">Vytvoří Azure AD application toobe exportovaný s buď hello podepsaného svým držitelem nebo enterprise veřejný klíč certifikátu, vytvoří hlavní účet služby pro aplikaci hello ve službě Azure AD a hello přiřadí role Přispěvatel pro účet hello ve vaší stávající předplatné.</span><span class="sxs-lookup"><span data-stu-id="d59c2-291">Creates an Azure AD application toobe exported with either hello self-signed or enterprise certificate public key, creates a service principal account for hello application in Azure AD, and assigns hello Contributor role for hello account in your current subscription.</span></span> <span data-ttu-id="d59c2-292">Můžete změnit tato nastavení tooOwner nebo jakákoli jiná role.</span><span class="sxs-lookup"><span data-stu-id="d59c2-292">You can change this setting tooOwner or any other role.</span></span> <span data-ttu-id="d59c2-293">Další informace najdete v tématu [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="d59c2-293">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="d59c2-294">Vytvoří prostředek certifikátu automatizace s názvem *AzureRunAsCertificate* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="d59c2-294">Creates an Automation certificate asset named *AzureRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="d59c2-295">asset certifikátu Hello obsahuje hello privátní klíč certifikátu používaného aplikací hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d59c2-295">hello certificate asset holds hello certificate private key that's used by hello Azure AD application.</span></span>
* <span data-ttu-id="d59c2-296">Vytvoří prostředek připojení automatizace s názvem *AzureRunAsConnection* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="d59c2-296">Creates an Automation connection asset named *AzureRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="d59c2-297">asset připojení Hello obsahuje hello applicationId, tenantId, subscriptionId a kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d59c2-297">hello connection asset holds hello applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="d59c2-298">**Pro účet Spustit jako pro Classic:**</span><span class="sxs-lookup"><span data-stu-id="d59c2-298">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="d59c2-299">Vytvoří prostředek certifikátu automatizace s názvem *AzureClassicRunAsCertificate* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="d59c2-299">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="d59c2-300">asset Hello certifikát obsahuje privátní klíč pro hello certifikátu používá certifikát pro správu hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-300">hello certificate asset holds hello certificate private key used by hello management certificate.</span></span>
* <span data-ttu-id="d59c2-301">Vytvoří prostředek připojení automatizace s názvem *AzureClassicRunAsConnection* v hello určený účet Automation.</span><span class="sxs-lookup"><span data-stu-id="d59c2-301">Creates an Automation connection asset named *AzureClassicRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="d59c2-302">asset připojení Hello obsahuje název odběru hello, subscriptionId a název certifikátu asset.</span><span class="sxs-lookup"><span data-stu-id="d59c2-302">hello connection asset holds hello subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="d59c2-303">Pokud vyberete jednu z možností pro vytvoření účtu Classic spustit jako, po hello skript se spustí, nahrávání hello veřejné správy toohello (přípona názvu souboru .cer) úložiště certifikátů pro předplatné hello této hello účet Automation byla vytvořena v.</span><span class="sxs-lookup"><span data-stu-id="d59c2-303">If you select either option for creating a Classic Run As account, after hello script is executed, upload hello public certificate (.cer file name extension) toohello management store for hello subscription that hello Automation account was created in.</span></span>
> 

<span data-ttu-id="d59c2-304">tooexecute hello skriptu a nahrání certifikátu hello, hello následující:</span><span class="sxs-lookup"><span data-stu-id="d59c2-304">tooexecute hello script and upload hello certificate, do hello following:</span></span>

1. <span data-ttu-id="d59c2-305">Uložte následující skript v počítači hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-305">Save hello following script on your computer.</span></span> <span data-ttu-id="d59c2-306">V tomto příkladu ho uložte pod názvem hello *New-RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="d59c2-306">In this example, save it with hello filename *New-RunAsAccount.ps1*.</span></span>

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
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
           Write-Error -Message "Please install hello latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
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
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. <span data-ttu-id="d59c2-307">Na svém počítači klikněte na **Start** a potom spusťte **Windows PowerShell** se zvýšenými uživatelskými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="d59c2-307">On your computer, click **Start**, and then start **Windows PowerShell** with elevated user rights.</span></span>

3. <span data-ttu-id="d59c2-308">Z hello se zvýšenými oprávněními prostředí příkazového řádku prostředí PowerShell, přejděte toohello složku, která obsahuje hello skript, který jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="d59c2-308">From hello elevated PowerShell command-line shell, go toohello folder that contains hello script you created in step 1.</span></span>

4. <span data-ttu-id="d59c2-309">Spusťte skript hello pomocí hodnoty parametrů hello hello konfigurace, kterou požadujete.</span><span class="sxs-lookup"><span data-stu-id="d59c2-309">Execute hello script by using hello parameter values for hello configuration you require.</span></span>

    <span data-ttu-id="d59c2-310">**Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem**</span><span class="sxs-lookup"><span data-stu-id="d59c2-310">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="d59c2-311">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem**</span><span class="sxs-lookup"><span data-stu-id="d59c2-311">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="d59c2-312">**Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu**</span><span class="sxs-lookup"><span data-stu-id="d59c2-312">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="d59c2-313">**Vytvořit účet Spustit jako a Classic spustit jako účet pomocí certifikát podepsaný svým držitelem v hello cloudu Azure Government.**</span><span class="sxs-lookup"><span data-stu-id="d59c2-313">**Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="d59c2-314">Po provedení hello skript bude výzvami tooauthenticate s Azure.</span><span class="sxs-lookup"><span data-stu-id="d59c2-314">After hello script has executed, you will be prompted tooauthenticate with Azure.</span></span> <span data-ttu-id="d59c2-315">Přihlaste se pomocí účtu, který je členem role Správci předplatného hello a spolusprávce předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-315">Sign in with an account that is a member of hello subscription administrators role and co-administrator of hello subscription.</span></span>
    >
    >

<span data-ttu-id="d59c2-316">Po hello skript byl úspěšně proveden, vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="d59c2-316">After hello script has executed successfully, note hello following:</span></span>
* <span data-ttu-id="d59c2-317">Pokud jste vytvořili účet Classic spustit jako s certifikát podepsaný svým držitelem veřejné (soubor .cer), skript hello vytvoří a uloží jej toohello dočasné soubory složky v počítači v rámci profilu uživatele hello *%USERPROFILE%\AppData\Local\Temp*, který používá relaci prostředí PowerShell tooexecute hello.</span><span class="sxs-lookup"><span data-stu-id="d59c2-317">If you created a Classic Run As account with a self-signed public certificate (.cer file), hello script creates and saves it toohello temporary files folder on your computer under hello user profile *%USERPROFILE%\AppData\Local\Temp*, which you used tooexecute hello PowerShell session.</span></span>
* <span data-ttu-id="d59c2-318">Pokud jste vytvořili účet Spustit jako pro Classic s využitím podnikového veřejného certifikátu (soubor .cer), použijte tento certifikát.</span><span class="sxs-lookup"><span data-stu-id="d59c2-318">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="d59c2-319">Postupujte podle pokynů hello pro [odesílání toohello certifikátu rozhraní API pro správu portálu Azure classic](../azure-api-management-certs.md)a ověřit konfiguraci hello přihlašovacích údajů pomocí služby správy prostředků pomocí hello [ukázkový kód tooauthenticate s prostředky služby správy](#sample-code-to-authenticate-with-service-management-resources).</span><span class="sxs-lookup"><span data-stu-id="d59c2-319">Follow hello instructions for [uploading a management API certificate toohello Azure classic portal](../azure-api-management-certs.md), and then validate hello credential configuration with Service Management resources by using hello [sample code tooauthenticate with Service Management Resources](#sample-code-to-authenticate-with-service-management-resources).</span></span> 
* <span data-ttu-id="d59c2-320">Pokud jste to udělali *není* vytvořit účet Classic spustit jako, ověření pomocí prostředků Resource Manageru a ověřit konfiguraci hello přihlašovacích údajů pomocí hello [ukázkový kód pro ověřování pomocí služby správy prostředky](#sample-code-to-authenticate-with-resource-manager-resources).</span><span class="sxs-lookup"><span data-stu-id="d59c2-320">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate hello credential configuration by using hello [sample code for authenticating with Service Management resources](#sample-code-to-authenticate-with-resource-manager-resources).</span></span>

## <a name="sample-code-tooauthenticate-with-resource-manager-resources"></a><span data-ttu-id="d59c2-321">Ukázka kódu tooauthenticate pomocí prostředků Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="d59c2-321">Sample code tooauthenticate with Resource Manager resources</span></span>
<span data-ttu-id="d59c2-322">Můžete použít následující hello aktualizovat ukázkový kód, provedených od hello *AzureAutomationTutorialScript* ukázkový runbook, tooauthenticate pomocí hello spustit jako účet toomanage prostředky Resource Manageru pomocí svých runbooků.</span><span class="sxs-lookup"><span data-stu-id="d59c2-322">You can use hello following updated sample code, taken from hello *AzureAutomationTutorialScript* example runbook, tooauthenticate by using hello Run As account toomanage Resource Manager resources with your runbooks.</span></span>

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get hello connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Signing in tooAzure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context tooa specific subscription"     
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

<span data-ttu-id="d59c2-323">toohelp tooeasily můžete pracovat s několika předplatnými, hello skript obsahuje dva další řádky kódu, které podporují odkazování na kontext předplatného.</span><span class="sxs-lookup"><span data-stu-id="d59c2-323">toohelp you tooeasily work between multiple subscriptions, hello script includes two additional lines of code that support referencing a subscription context.</span></span> <span data-ttu-id="d59c2-324">Proměnný asset s názvem *SubscriptionId* obsahuje ID hello hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="d59c2-324">A variable asset named *SubscriptionId* contains hello ID of hello subscription.</span></span> <span data-ttu-id="d59c2-325">Po hello `Add-AzureRmAccount` příkazu rutiny hello [ `Set-AzureRmContext` ](/powershell/module/azurerm.profile/set-azurermcontext) rutiny je uvedená v sada parametrů hello *- SubscriptionId*.</span><span class="sxs-lookup"><span data-stu-id="d59c2-325">After hello `Add-AzureRmAccount` cmdlet statement, hello [`Set-AzureRmContext`](/powershell/module/azurerm.profile/set-azurermcontext) cmdlet is stated with hello parameter set *-SubscriptionId*.</span></span> <span data-ttu-id="d59c2-326">Pokud hello název proměnné příliš obecný, můžete zkontrolujte ji tooinclude předpony nebo pomocí jiné zásady vytváření názvů toomake je snazší tooidentify.</span><span class="sxs-lookup"><span data-stu-id="d59c2-326">If hello variable name is too generic, you can revise it tooinclude a prefix or use another naming convention toomake it easier tooidentify.</span></span> <span data-ttu-id="d59c2-327">Alternativně můžete použít sadu parametrů hello *- Název_předplatného* místo *- SubscriptionId* s odpovídajícím proměnným assetem.</span><span class="sxs-lookup"><span data-stu-id="d59c2-327">Alternatively, you can use hello parameter set *-SubscriptionName* instead of *-SubscriptionId* with a corresponding variable asset.</span></span>

<span data-ttu-id="d59c2-328">Hello rutiny, který používáte pro ověřování v sadě runbook hello, `Add-AzureRmAccount`, používá hello *ServicePrincipalCertificate* sadu parametrů.</span><span class="sxs-lookup"><span data-stu-id="d59c2-328">hello cmdlet that you use for authenticating in hello runbook, `Add-AzureRmAccount`, uses hello *ServicePrincipalCertificate* parameter set.</span></span> <span data-ttu-id="d59c2-329">Ověřuje pomocí hello certifikát hlavní služby, hello pověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="d59c2-329">It authenticates by using hello service principal certificate, not hello user credentials.</span></span>

## <a name="sample-code-tooauthenticate-with-service-management-resources"></a><span data-ttu-id="d59c2-330">Ukázka kódu tooauthenticate pomocí služby správy prostředků</span><span class="sxs-lookup"><span data-stu-id="d59c2-330">Sample code tooauthenticate with Service Management resources</span></span>
<span data-ttu-id="d59c2-331">Můžete použít následující aktualizovaný ukázkový kód, který je převzat ze hello hello *AzureClassicAutomationTutorialScript* ukázkový runbook, tooauthenticate pomocí hello Classic účet Spustit jako toomanage klasické prostředky s vaší sady runbook.</span><span class="sxs-lookup"><span data-stu-id="d59c2-331">You can use hello following updated sample code, which is taken from hello *AzureClassicAutomationTutorialScript* example runbook, tooauthenticate by using hello Classic Run As account toomanage classic resources with your runbooks.</span></span>

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get hello connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate tooAzure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in hello Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting hello certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in hello Automation account."
    }

    Write-Verbose "Authenticating tooAzure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID

## <a name="next-steps"></a><span data-ttu-id="d59c2-332">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d59c2-332">Next steps</span></span>
* [<span data-ttu-id="d59c2-333">Aplikační a instanční objekty v Azure AD</span><span class="sxs-lookup"><span data-stu-id="d59c2-333">Application and service principal objects in Azure AD</span></span>](../active-directory/active-directory-application-objects.md)
* [<span data-ttu-id="d59c2-334">Řízení přístupu na základě role ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="d59c2-334">Role-based access control in Azure Automation</span></span>](automation-role-based-access-control.md)
* [<span data-ttu-id="d59c2-335">Přehled certifikátů pro Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="d59c2-335">Certificates overview for Azure Cloud Services</span></span>](../cloud-services/cloud-services-certs-create.md)
