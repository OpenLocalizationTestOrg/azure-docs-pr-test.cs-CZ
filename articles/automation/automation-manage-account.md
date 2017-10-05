---
title: "Správa účtu Azure Automation | Dokumentace Microsoftu"
description: "Tento článek popisuje, jak spravovat konfiguraci vašeho účtu Automation, jako je chybná konfigurace, odstranění nebo obnovení certifikátu."
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
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: 41efdbcacede74bac038342688362ff480cadc7e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-automation-account"></a><span data-ttu-id="13614-103">Správa účtu Azure Automation</span><span class="sxs-lookup"><span data-stu-id="13614-103">Manage Azure Automation account</span></span>
<span data-ttu-id="13614-104">V určitém okamžiku před vypršením platnosti účtu Automation musíte obnovit certifikát.</span><span class="sxs-lookup"><span data-stu-id="13614-104">At some point before your Automation account expires, you will need to renew the certificate.</span></span> <span data-ttu-id="13614-105">Pokud se domníváte, že zabezpečení účtu Spustit jako je ohrožené, můžete ho odstranit a vytvořit znovu.</span><span class="sxs-lookup"><span data-stu-id="13614-105">If you believe that the Run As account has been compromised, you can delete and re-create it.</span></span> <span data-ttu-id="13614-106">Tato část popisuje, jak tyto operace provést.</span><span class="sxs-lookup"><span data-stu-id="13614-106">This section discusses how to perform these operations.</span></span>

## <a name="self-signed-certificate-renewal"></a><span data-ttu-id="13614-107">Obnovení certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="13614-107">Self-signed certificate renewal</span></span>
<span data-ttu-id="13614-108">Platnost certifikátu podepsaného svým držitelem, který jste vytvořili pro účet Spustit jako, vyprší jeden rok od data jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="13614-108">The self-signed certificate that you created for the Run As account expires one year from the date of creation.</span></span> <span data-ttu-id="13614-109">Před vypršením platnosti ho můžete kdykoli obnovit.</span><span class="sxs-lookup"><span data-stu-id="13614-109">You can renew it at any time before it expires.</span></span> <span data-ttu-id="13614-110">Když ho obnovíte, aktuální platný certifikát se uchová, aby se zajistilo, že to negativně neovlivní runbooky ověřované účtem Spustit jako, které jsou právě ve frontě nebo aktivně spuštěné.</span><span class="sxs-lookup"><span data-stu-id="13614-110">When you renew it, the current valid certificate is retained to ensure that any runbooks that are queued up or actively running, and that authenticate with the Run As account, are not negatively affected.</span></span> <span data-ttu-id="13614-111">Certifikát zůstane platný až do data vypršení jeho platnosti.</span><span class="sxs-lookup"><span data-stu-id="13614-111">The certificate remains valid until its expiration date.</span></span>

> [!NOTE]
> <span data-ttu-id="13614-112">Pokud jste svůj účet Automation Spustit jako nakonfigurovali k používání certifikátu vydaného podnikovou certifikační autoritou a použijete tuto možnost, tento podnikový certifikát se nahradí certifikátem podepsaným svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="13614-112">If you have configured your Automation Run As account to use a certificate issued by your enterprise certificate authority and you use this option, the enterprise certificate will be replaced by a self-signed certificate.</span></span>

<span data-ttu-id="13614-113">Pokud chcete certifikát obnovit, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="13614-113">To renew the certificate, do the following:</span></span>

1. <span data-ttu-id="13614-114">Na webu Azure Portal otevřete účet Automation.</span><span class="sxs-lookup"><span data-stu-id="13614-114">In the Azure portal, open the Automation account.</span></span>

2. <span data-ttu-id="13614-115">V okně **Účet Automation** v podokně **Vlastnosti účtu** v části **Nastavení účtů** vyberte **Účty Spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="13614-115">On the **Automation Account** blade, in the **Account properties** pane, under **Account Settings**, select **Run As Accounts**.</span></span>

    ![Podokno vlastností účtu Automation](media/automation-manage-account/automation-account-properties-pane.png)
3. <span data-ttu-id="13614-117">V okně vlastností **Účty Spustit jako** vyberte účet Spustit jako nebo účet Spustit jako pro Classic, pro který chcete obnovit certifikát.</span><span class="sxs-lookup"><span data-stu-id="13614-117">On the **Run As Accounts** properties blade, select either the Run As account or the Classic Run As account that you want to renew the certificate for.</span></span>

4. <span data-ttu-id="13614-118">V okně **Vlastnosti** vybraného účtu klikněte na **Obnovit certifikát**.</span><span class="sxs-lookup"><span data-stu-id="13614-118">On the **Properties** blade for the selected account, click **Renew certificate**.</span></span>

    ![Obnovení certifikátu pro účet Spustit jako](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. <span data-ttu-id="13614-120">Zatímco se certifikát obnovuje, můžete průběh sledovat v nabídce v části **Oznámení**.</span><span class="sxs-lookup"><span data-stu-id="13614-120">While the certificate is being renewed, you can track the progress under **Notifications** from the menu.</span></span>

## <a name="delete-a-run-as-or-classic-run-as-account"></a><span data-ttu-id="13614-121">Odstranění účet Spustit jako nebo Spustit jako pro Classic</span><span class="sxs-lookup"><span data-stu-id="13614-121">Delete a Run As or Classic Run As account</span></span>
<span data-ttu-id="13614-122">Tato část popisuje, jak odstranit a znovu vytvořit účet Spustit jako nebo účet Spustit jako pro Classic.</span><span class="sxs-lookup"><span data-stu-id="13614-122">This section describes how to delete and re-create a Run As or Classic Run As account.</span></span> <span data-ttu-id="13614-123">Při provedení této akce se účet Automation uchová.</span><span class="sxs-lookup"><span data-stu-id="13614-123">When you perform this action, the Automation account is retained.</span></span> <span data-ttu-id="13614-124">Odstraněný účet Spustit jako nebo účet Spustit jako pro Classic můžete znovu vytvořit na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="13614-124">After you delete a Run As or Classic Run As account, you can re-create it in the Azure portal.</span></span>

1. <span data-ttu-id="13614-125">Na webu Azure Portal otevřete účet Automation.</span><span class="sxs-lookup"><span data-stu-id="13614-125">In the Azure portal, open the Automation account.</span></span>

2. <span data-ttu-id="13614-126">V okně **Účet Automation** v podokně vlastností účtu vyberte **Účty Spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="13614-126">On the **Automation account** blade, in the account properties pane, select **Run As Accounts**.</span></span>

3. <span data-ttu-id="13614-127">V okně vlastností **Účty Spustit jako** vyberte účet Spustit jako nebo účet Spustit jako pro Classic, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="13614-127">On the **Run As Accounts** properties blade, select either the Run As account or Classic Run As account that you want to delete.</span></span> <span data-ttu-id="13614-128">Potom v okně **Vlastnosti** vybraného účtu klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="13614-128">Then, on the **Properties** blade for the selected account, click **Delete**.</span></span>

 ![Odstranění účtu Spustit jako](media/automation-manage-account/automation-account-delete-runas.png)

4. <span data-ttu-id="13614-130">Zatímco se účet odstraňuje, můžete průběh sledovat v nabídce v části **Oznámení**.</span><span class="sxs-lookup"><span data-stu-id="13614-130">While the account is being deleted, you can track the progress under **Notifications** from the menu.</span></span>

5. <span data-ttu-id="13614-131">Účet po odstranění můžete znovu vytvořit v okně vlastností **Účty Spustit jako** výběrem možnosti Vytvořit v části **Účet Spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="13614-131">After the account has been deleted, you can re-create it on the **Run As Accounts** properties blade by selecting the create option **Azure Run As Account**.</span></span>

 ![Znovuvytvoření účtu Automation Spustit jako](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a><span data-ttu-id="13614-133">Chybná konfigurace</span><span class="sxs-lookup"><span data-stu-id="13614-133">Misconfiguration</span></span>
<span data-ttu-id="13614-134">Může se stát, že se během prvotního nastavení nesprávně vytvoří nebo později odstraní některá z položek konfigurace nezbytných pro správné fungování účtu Spustit jako nebo Spustit jako pro Classic.</span><span class="sxs-lookup"><span data-stu-id="13614-134">Some configuration items necessary for the Run As or Classic Run As account to function properly might have been deleted or created improperly during initial setup.</span></span> <span data-ttu-id="13614-135">Mezi tyto položky patří:</span><span class="sxs-lookup"><span data-stu-id="13614-135">The items include:</span></span>

* <span data-ttu-id="13614-136">Asset certifikátu</span><span class="sxs-lookup"><span data-stu-id="13614-136">Certificate asset</span></span>
* <span data-ttu-id="13614-137">Asset připojení</span><span class="sxs-lookup"><span data-stu-id="13614-137">Connection asset</span></span>
* <span data-ttu-id="13614-138">Účet Spustit jako byl odebrán z role přispěvatele</span><span class="sxs-lookup"><span data-stu-id="13614-138">Run As account has been removed from the contributor role</span></span>
* <span data-ttu-id="13614-139">Instanční objekt nebo aplikace v Azure AD</span><span class="sxs-lookup"><span data-stu-id="13614-139">Service principal or application in Azure AD</span></span>

<span data-ttu-id="13614-140">V předchozí a dalších instancích chybné konfigurace účet Automation zjistí změny a v okně vlastností *Účty Spustit jako* příslušného účtu zobrazí stav **Nedokončeno**.</span><span class="sxs-lookup"><span data-stu-id="13614-140">In the preceding and other instances of misconfiguration, the Automation account detects the changes and displays a status of *Incomplete* on the **Run As Accounts** properties blade for the account.</span></span>

![Nedokončená konfigurace účtu Spustit jako](media/automation-manage-account/automation-account-runas-incomplete-config.png)

<span data-ttu-id="13614-142">Pokud vyberete tento účet Spustit jako, v podokně **Vlastnosti** účtu se zobrazí následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="13614-142">When you select the Run As account, the account **Properties** pane displays the following error message:</span></span>

![Zpráva upozornění o nedokončené konfiguraci účtu Spustit jako](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png)<span data-ttu-id="13614-144">.</span><span class="sxs-lookup"><span data-stu-id="13614-144">.</span></span>

<span data-ttu-id="13614-145">Tyto potíže s účtem Spustit jako můžete rychle vyřešit jeho odstraněním a znovuvytvořením.</span><span class="sxs-lookup"><span data-stu-id="13614-145">You can quickly resolve these Run As account issues by deleting and re-creating the account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="13614-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="13614-146">Next steps</span></span>
* <span data-ttu-id="13614-147">Další informace o objektech služby najdete v článku [Objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="13614-147">For more information about Service Principals, refer to [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span>
* <span data-ttu-id="13614-148">Další informace o řízení přístupu na základě rolí ve službě Azure Automation najdete v článku [Řízení přístupu na základě rolí ve službě Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="13614-148">For more information about Role-based Access Control in Azure Automation, refer to [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="13614-149">Další informace o certifikátech a službách Azure najdete v článku [Přehled certifikátů pro Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="13614-149">For more information about certificates and Azure services, refer to [Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span></span>