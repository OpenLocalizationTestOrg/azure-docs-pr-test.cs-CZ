---
title: "aaaManage účet Azure Automation | Microsoft Docs"
description: "Tento článek popisuje, jak toomanage hello konfigurace účtu Automation, například chybné konfigurace, odstranění a obnovení certifikátu."
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
ms.openlocfilehash: 75e41f601a138d4e8853aa79dcbab6696a5e9fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-automation-account"></a><span data-ttu-id="c8b97-103">Správa účtu Azure Automation</span><span class="sxs-lookup"><span data-stu-id="c8b97-103">Manage Azure Automation account</span></span>
<span data-ttu-id="c8b97-104">V určitém okamžiku před vypršením platnosti účtu Automation, musíte certifikát toorenew hello.</span><span class="sxs-lookup"><span data-stu-id="c8b97-104">At some point before your Automation account expires, you will need toorenew hello certificate.</span></span> <span data-ttu-id="c8b97-105">Pokud se domníváte, že došlo k ohrožení zabezpečení tohoto hello účet Spustit jako, můžete odstranit a znovu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="c8b97-105">If you believe that hello Run As account has been compromised, you can delete and re-create it.</span></span> <span data-ttu-id="c8b97-106">Tato část pojednává o tom, jak tooperform těchto operací.</span><span class="sxs-lookup"><span data-stu-id="c8b97-106">This section discusses how tooperform these operations.</span></span>

## <a name="self-signed-certificate-renewal"></a><span data-ttu-id="c8b97-107">Obnovení certifikátu podepsaného svým držitelem</span><span class="sxs-lookup"><span data-stu-id="c8b97-107">Self-signed certificate renewal</span></span>
<span data-ttu-id="c8b97-108">certifikát podepsaný svým držitelem Hello, kterou jste vytvořili pro hello účet Spustit jako vyprší platnost jeden rok z hello data vytvoření.</span><span class="sxs-lookup"><span data-stu-id="c8b97-108">hello self-signed certificate that you created for hello Run As account expires one year from hello date of creation.</span></span> <span data-ttu-id="c8b97-109">Před vypršením platnosti ho můžete kdykoli obnovit.</span><span class="sxs-lookup"><span data-stu-id="c8b97-109">You can renew it at any time before it expires.</span></span> <span data-ttu-id="c8b97-110">Při obnovování, je aktuální platný certifikát hello udržených tooensure, že nejsou žádné sady runbook, které jsou zařazeny do fronty až nebo aktivně spuštěná, a který ověření pomocí hello účet Spustit jako, negativní vliv.</span><span class="sxs-lookup"><span data-stu-id="c8b97-110">When you renew it, hello current valid certificate is retained tooensure that any runbooks that are queued up or actively running, and that authenticate with hello Run As account, are not negatively affected.</span></span> <span data-ttu-id="c8b97-111">certifikát Hello zůstává platná do data vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="c8b97-111">hello certificate remains valid until its expiration date.</span></span>

> [!NOTE]
> <span data-ttu-id="c8b97-112">Pokud jste nakonfigurovali vaší spustit v Automation jako účet toouse certifikát vydaný certifikační autoritou rozlehlé sítě a chcete použít tuto možnost, hello podnikový certifikát budou nahrazeny certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="c8b97-112">If you have configured your Automation Run As account toouse a certificate issued by your enterprise certificate authority and you use this option, hello enterprise certificate will be replaced by a self-signed certificate.</span></span>

<span data-ttu-id="c8b97-113">toorenew hello certifikátů, hello následující:</span><span class="sxs-lookup"><span data-stu-id="c8b97-113">toorenew hello certificate, do hello following:</span></span>

1. <span data-ttu-id="c8b97-114">V hello portálu Azure otevřete účet Automation hello.</span><span class="sxs-lookup"><span data-stu-id="c8b97-114">In hello Azure portal, open hello Automation account.</span></span>

2. <span data-ttu-id="c8b97-115">Na hello **účet Automation** okno, v hello **účet vlastnosti** podokně v části **nastavení účtu**, vyberte **účty spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="c8b97-115">On hello **Automation Account** blade, in hello **Account properties** pane, under **Account Settings**, select **Run As Accounts**.</span></span>

    ![Podokno vlastností účtu Automation](media/automation-manage-account/automation-account-properties-pane.png)
3. <span data-ttu-id="c8b97-117">Na hello **účty spustit jako** vlastnosti okně vyberte buď hello účet Spustit jako nebo hello Classic účet Spustit jako, které chcete toorenew hello certifikát pro.</span><span class="sxs-lookup"><span data-stu-id="c8b97-117">On hello **Run As Accounts** properties blade, select either hello Run As account or hello Classic Run As account that you want toorenew hello certificate for.</span></span>

4. <span data-ttu-id="c8b97-118">Na hello **vlastnosti** okně hello vybraný účet, klikněte na tlačítko **obnovit certifikát**.</span><span class="sxs-lookup"><span data-stu-id="c8b97-118">On hello **Properties** blade for hello selected account, click **Renew certificate**.</span></span>

    ![Obnovení certifikátu pro účet Spustit jako](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. <span data-ttu-id="c8b97-120">Při obnovení certifikátu hello se můžete sledovat průběh hello pod **oznámení** nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="c8b97-120">While hello certificate is being renewed, you can track hello progress under **Notifications** from hello menu.</span></span>

## <a name="delete-a-run-as-or-classic-run-as-account"></a><span data-ttu-id="c8b97-121">Odstranění účet Spustit jako nebo Spustit jako pro Classic</span><span class="sxs-lookup"><span data-stu-id="c8b97-121">Delete a Run As or Classic Run As account</span></span>
<span data-ttu-id="c8b97-122">Tato část popisuje, jak toodelete a znovu vytvořit účet Spustit jako nebo Classic spustit jako.</span><span class="sxs-lookup"><span data-stu-id="c8b97-122">This section describes how toodelete and re-create a Run As or Classic Run As account.</span></span> <span data-ttu-id="c8b97-123">Když provedete tuto akci, se uchovávají hello účet Automation.</span><span class="sxs-lookup"><span data-stu-id="c8b97-123">When you perform this action, hello Automation account is retained.</span></span> <span data-ttu-id="c8b97-124">Po odstranění účtu spustit jako nebo Classic spustit jako, znovu ji vytvořte hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c8b97-124">After you delete a Run As or Classic Run As account, you can re-create it in hello Azure portal.</span></span>

1. <span data-ttu-id="c8b97-125">V hello portálu Azure otevřete účet Automation hello.</span><span class="sxs-lookup"><span data-stu-id="c8b97-125">In hello Azure portal, open hello Automation account.</span></span>

2. <span data-ttu-id="c8b97-126">Na hello **účet Automation** okno, v hello účet vlastnosti podokně, vyberte **účty spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="c8b97-126">On hello **Automation account** blade, in hello account properties pane, select **Run As Accounts**.</span></span>

3. <span data-ttu-id="c8b97-127">Na hello **účty spustit jako** okno Vlastnosti, vyberte buď hello účet Spustit jako nebo Classic spustit jako účet, který má toodelete.</span><span class="sxs-lookup"><span data-stu-id="c8b97-127">On hello **Run As Accounts** properties blade, select either hello Run As account or Classic Run As account that you want toodelete.</span></span> <span data-ttu-id="c8b97-128">Potom na hello **vlastnosti** okně hello vybraný účet, klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c8b97-128">Then, on hello **Properties** blade for hello selected account, click **Delete**.</span></span>

 ![Odstranění účtu Spustit jako](media/automation-manage-account/automation-account-delete-runas.png)

4. <span data-ttu-id="c8b97-130">Zatímco se odstraňuje hello účet, můžete sledovat průběh hello v části **oznámení** nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="c8b97-130">While hello account is being deleted, you can track hello progress under **Notifications** from hello menu.</span></span>

5. <span data-ttu-id="c8b97-131">Po odstranění účtu hello můžete znovu vytvořit ji na hello **účty spustit jako** okno Vlastnosti výběrem hello vytvořit možnost **Azure účet Spustit jako**.</span><span class="sxs-lookup"><span data-stu-id="c8b97-131">After hello account has been deleted, you can re-create it on hello **Run As Accounts** properties blade by selecting hello create option **Azure Run As Account**.</span></span>

 ![Znovu vytvořit účet Automation spustit jako hello](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a><span data-ttu-id="c8b97-133">Chybná konfigurace</span><span class="sxs-lookup"><span data-stu-id="c8b97-133">Misconfiguration</span></span>
<span data-ttu-id="c8b97-134">Některé položky konfigurace potřebné pro toofunction účet Spustit jako nebo Classic spustit jako hello správně může byla odstraněna nebo nesprávně vytvořený během počáteční instalace.</span><span class="sxs-lookup"><span data-stu-id="c8b97-134">Some configuration items necessary for hello Run As or Classic Run As account toofunction properly might have been deleted or created improperly during initial setup.</span></span> <span data-ttu-id="c8b97-135">položky Hello patří:</span><span class="sxs-lookup"><span data-stu-id="c8b97-135">hello items include:</span></span>

* <span data-ttu-id="c8b97-136">Asset certifikátu</span><span class="sxs-lookup"><span data-stu-id="c8b97-136">Certificate asset</span></span>
* <span data-ttu-id="c8b97-137">Asset připojení</span><span class="sxs-lookup"><span data-stu-id="c8b97-137">Connection asset</span></span>
* <span data-ttu-id="c8b97-138">Účet Spustit jako byl odebrán z role Přispěvatel hello</span><span class="sxs-lookup"><span data-stu-id="c8b97-138">Run As account has been removed from hello contributor role</span></span>
* <span data-ttu-id="c8b97-139">Instanční objekt nebo aplikace v Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8b97-139">Service principal or application in Azure AD</span></span>

<span data-ttu-id="c8b97-140">V hello předchozí a další instance chybné konfigurace, zjistí hello účet Automation hello změní a zobrazuje stav *nekompletní* na hello **účty spustit jako** okně Vlastnosti pro hello účet.</span><span class="sxs-lookup"><span data-stu-id="c8b97-140">In hello preceding and other instances of misconfiguration, hello Automation account detects hello changes and displays a status of *Incomplete* on hello **Run As Accounts** properties blade for hello account.</span></span>

![Nedokončená konfigurace účtu Spustit jako](media/automation-manage-account/automation-account-runas-incomplete-config.png)

<span data-ttu-id="c8b97-142">Když vyberete účet Spustit jako hello, hello účet **vlastnosti** podokně zobrazí hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="c8b97-142">When you select hello Run As account, hello account **Properties** pane displays hello following error message:</span></span>

![Zpráva upozornění o nedokončené konfiguraci účtu Spustit jako](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png)<span data-ttu-id="c8b97-144">.</span><span class="sxs-lookup"><span data-stu-id="c8b97-144">.</span></span>

<span data-ttu-id="c8b97-145">Odstranit a znovu vytvořit účet hello můžete rychle vyřešit tyto problémy účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="c8b97-145">You can quickly resolve these Run As account issues by deleting and re-creating hello account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8b97-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c8b97-146">Next steps</span></span>
* <span data-ttu-id="c8b97-147">Další informace o objektech služby najdete v části příliš[objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="c8b97-147">For more information about Service Principals, refer too[Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span>
* <span data-ttu-id="c8b97-148">Další informace o řízení přístupu na základě rolí ve službě Azure Automation najdete v části příliš[řízení přístupu na základě Role ve službě Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="c8b97-148">For more information about Role-based Access Control in Azure Automation, refer too[Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="c8b97-149">Další informace o certifikátech a službám Azure, najdete v části příliš[Přehled certifikátů pro Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="c8b97-149">For more information about certificates and Azure services, refer too[Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span></span>
