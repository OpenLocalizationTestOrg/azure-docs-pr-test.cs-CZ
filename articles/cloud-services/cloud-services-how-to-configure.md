---
title: "Postup konfigurace cloudové služby (portálu classic) | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat cloudové služby v Azure. Naučte se aktualizovat konfiguraci této cloudové služby a konfigurace vzdáleného přístupu k instancí rolí."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4902f79d-ea91-41ca-89a4-7c818180ee5f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 39bb294c96ce0c12d91cf8b3488ac3e1a7b2f7b2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-cloud-services"></a><span data-ttu-id="271d0-104">Postup konfigurace cloudové služby</span><span class="sxs-lookup"><span data-stu-id="271d0-104">How to Configure Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="271d0-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="271d0-105">Azure portal</span></span>](cloud-services-how-to-configure-portal.md)
> * [<span data-ttu-id="271d0-106">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="271d0-106">Azure classic portal</span></span>](cloud-services-how-to-configure.md)
> 
> 

<span data-ttu-id="271d0-107">Můžete nakonfigurovat nastavení nejčastěji používaných pro cloudové služby na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="271d0-107">You can configure the most commonly used settings for a cloud service in the Azure classic portal.</span></span> <span data-ttu-id="271d0-108">Pokud chcete místo toho aktualizovat konfigurační soubory přímo, stáhněte si příslušný konfigurační soubor služby a pak ho aktualizujte a nahrajte. Tím aktualizujete konfiguraci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="271d0-108">Or, if you like to update your configuration files directly, download a service configuration file to update, and then upload the updated file and update the cloud service with the configuration changes.</span></span> <span data-ttu-id="271d0-109">V obou případech se aktualizovaná konfigurace projeví ve všech instancích rolí.</span><span class="sxs-lookup"><span data-stu-id="271d0-109">Either way, the configuration updates are pushed out to all role instances.</span></span>

<span data-ttu-id="271d0-110">Portál Azure classic můžete taky [povolit připojení ke vzdálené ploše pro roli ve službě Azure Cloud Services](cloud-services-role-enable-remote-desktop.md)</span><span class="sxs-lookup"><span data-stu-id="271d0-110">The Azure classic portal also allows you to [enable Remote Desktop Connection for a Role in Azure Cloud Services](cloud-services-role-enable-remote-desktop.md)</span></span>

<span data-ttu-id="271d0-111">Azure můžete pouze zajistit dostupnost služby 99,95 % během aktualizace konfigurace Pokud máte aspoň dvě instance role pro každou roli.</span><span class="sxs-lookup"><span data-stu-id="271d0-111">Azure can only ensure 99.95 percent service availability during the configuration updates if you have at least two role instances for every role.</span></span> <span data-ttu-id="271d0-112">Umožňující jeden virtuální počítač ke zpracování požadavků klientů dalších během aktualizace.</span><span class="sxs-lookup"><span data-stu-id="271d0-112">That enables one virtual machine to process client requests while the other is being updated.</span></span> <span data-ttu-id="271d0-113">Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="271d0-113">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="change-a-cloud-service"></a><span data-ttu-id="271d0-114">Změnit cloudové služby</span><span class="sxs-lookup"><span data-stu-id="271d0-114">Change a cloud service</span></span>
1. <span data-ttu-id="271d0-115">V [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**, klikněte na název cloudové služby a pak klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="271d0-115">In the [Azure classic portal](http://manage.windowsazure.com/), click **Cloud Services**, click the name of the cloud service, and then click **Configure**.</span></span>
   
    ![Stránka Konfigurace](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
   
    <span data-ttu-id="271d0-117">Na **konfigurace** stránky, můžete nakonfigurovat nastavení aktualizace role, sledování a zvolte hostovaného operačního systému a třídu u instancí role.</span><span class="sxs-lookup"><span data-stu-id="271d0-117">On the **Configure** page, you can configure monitoring, update role settings, and choose the guest operating system and family for role instances.</span></span> 
2. <span data-ttu-id="271d0-118">V **monitorování**, nastavit Verbose nebo minimální úroveň monitorování a konfigurace diagnostiky připojovacích řetězců, které jsou požadovány pro podrobné monitorování.</span><span class="sxs-lookup"><span data-stu-id="271d0-118">In **monitoring**, set the monitoring level to Verbose or Minimal, and configure the diagnostics connection strings that are required for verbose monitoring.</span></span>
3. <span data-ttu-id="271d0-119">Pro služby role (seskupené podle role) můžete aktualizovat následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="271d0-119">For service roles (grouped by role), you can update the following settings:</span></span>
   
    * <span data-ttu-id="271d0-120">**Nastavení** změnit hodnoty nastavení různé konfigurace, které jsou určené v *ConfigurationSettings* prvky souboru služby (.csdef).</span><span class="sxs-lookup"><span data-stu-id="271d0-120">**Settings** Modify the values of miscellaneous configuration settings that are specified in the *ConfigurationSettings* elements of the service configuration (.cscfg) file.</span></span>

    * <span data-ttu-id="271d0-121">**Certifikáty**</span><span class="sxs-lookup"><span data-stu-id="271d0-121">**Certificates**</span></span>  
        <span data-ttu-id="271d0-122">Změňte kryptografický otisk certifikátu, který se používá v šifrování SSL pro roli.</span><span class="sxs-lookup"><span data-stu-id="271d0-122">Change the certificate thumbprint that's being used in SSL encryption for a role.</span></span> <span data-ttu-id="271d0-123">Chcete-li změnit certifikát, musíte nejprve nahrát nový certifikát (na **certifikáty** stránky).</span><span class="sxs-lookup"><span data-stu-id="271d0-123">To change a certificate, you must first upload the new certificate (on the **Certificates** page).</span></span> <span data-ttu-id="271d0-124">Potom aktualizujte kryptografický otisk certifikátu řetězec zobrazí v nastavení role.</span><span class="sxs-lookup"><span data-stu-id="271d0-124">Then update the thumbprint in the certificate string displayed in the role settings.</span></span>
4. <span data-ttu-id="271d0-125">V **operačního systému**, můžete změnit operační systém řady nebo verze pro instance rolí, nebo zvolte **automatické** povolit automatické aktualizace aktuální verzi operačního systému.</span><span class="sxs-lookup"><span data-stu-id="271d0-125">In **operating system**, you can change the operating system family or version for role instances, or choose **Automatic** to enable automatic updates of the current operating system version.</span></span> <span data-ttu-id="271d0-126">Nastavení operačního systému na webových rolí a rolí pracovního procesu vztahovat, ale neovlivní virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="271d0-126">The operating system settings apply to web roles and worker roles, but do not affect Virtual Machines.</span></span>
   
    <span data-ttu-id="271d0-127">Během nasazení na všech instancích role je nainstalovaná nejnovější verze operačního systému a ve výchozím nastavení se automaticky aktualizují operační systémy.</span><span class="sxs-lookup"><span data-stu-id="271d0-127">During deployment, the most recent operating system version is installed on all role instances, and the operating systems are updated automatically by default.</span></span> 
   
    <span data-ttu-id="271d0-128">Pokud budete potřebovat pro cloudové služby ke spuštění na jinou verzi operačního systému z důvodu požadavky na kompatibilitu ve vašem kódu, můžete operačního systému rodiny a verze.</span><span class="sxs-lookup"><span data-stu-id="271d0-128">If you need for your cloud service to run on a different operating system version because of compatibility requirements in your code, you can choose an operating system family and version.</span></span> <span data-ttu-id="271d0-129">Když zvolíte konkrétní verze operačního systému, aktualizací automatické operačního systému pro cloudové služby jsou pozastavené.</span><span class="sxs-lookup"><span data-stu-id="271d0-129">When you choose a specific operating system version, automatic operating system updates for the cloud service are suspended.</span></span> <span data-ttu-id="271d0-130">Musíte zajistit, že operační systémy přijímat aktualizace.</span><span class="sxs-lookup"><span data-stu-id="271d0-130">You will need to ensure the operating systems receive updates.</span></span>
   
    <span data-ttu-id="271d0-131">-Li vyřešit všechny problémy s kompatibilitou, které aplikace mají s nejnovější verzí operačního systému, můžete povolit aktualizace automatické operačního systému tak verzi operačního systému na **automatické**.</span><span class="sxs-lookup"><span data-stu-id="271d0-131">If you resolve all compatibility issues that your apps have with the most recent operating system version, you can enable automatic operating system updates by setting the operating system version to **Automatic**.</span></span> 
   
    ![Nastavení operačního systému](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)
5. <span data-ttu-id="271d0-133">Pokud chcete uložit nastavení konfigurace a vložit je instance rolí, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="271d0-133">To save your configuration settings, and push them to the role instances, click **Save**.</span></span> <span data-ttu-id="271d0-134">(Klikněte na tlačítko **zahodit** zrušit změny.) **Uložit** a **zahodit** jsou přidány do panelu příkazů po provedení změny nastavení.</span><span class="sxs-lookup"><span data-stu-id="271d0-134">(Click **Discard** to cancel the changes.) **Save** and **Discard** are added to the command bar after you change a setting.</span></span>

## <a name="update-a-cloud-service-configuration-file"></a><span data-ttu-id="271d0-135">Aktualizace konfiguračního souboru cloudové služby</span><span class="sxs-lookup"><span data-stu-id="271d0-135">Update a cloud service configuration file</span></span>
1. <span data-ttu-id="271d0-136">Stáhněte si cloudové služby konfiguračního souboru (.cscfg) s aktuální konfigurací.</span><span class="sxs-lookup"><span data-stu-id="271d0-136">Download a cloud service configuration file (.cscfg) with the current configuration.</span></span> <span data-ttu-id="271d0-137">Na **konfigurace** stránky pro cloudovou službu, klikněte na tlačítko **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="271d0-137">On the **Configure** page for the cloud service, click **Download**.</span></span> <span data-ttu-id="271d0-138">Pak klikněte na tlačítko **Uložit**, nebo klikněte na tlačítko **uložit jako** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="271d0-138">Then click **Save**, or click **Save As** to save the file.</span></span>
2. <span data-ttu-id="271d0-139">Po aktualizaci konfigurační soubor služby, odesílání a aktualizace konfigurace:</span><span class="sxs-lookup"><span data-stu-id="271d0-139">After you update the service configuration file, upload and apply the configuration updates:</span></span>
   
   1. <span data-ttu-id="271d0-140">Na **konfigurace** klikněte na tlačítko **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="271d0-140">On the **Configure** page, click **Upload**.</span></span>
      
       ![Nahrát konfigurace](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
   2. <span data-ttu-id="271d0-142">V **konfigurační soubor**, použijte **Procházet** a vyberte soubor .cscfg aktualizované.</span><span class="sxs-lookup"><span data-stu-id="271d0-142">In **Configuration file**, use **Browse** to select the updated .cscfg file.</span></span>
   3. <span data-ttu-id="271d0-143">Pokud cloudové služby obsahuje všechny role, které mají jenom jednu instanci, vyberte **použít konfiguraci i v případě, že jeden nebo více rolí obsahuje jednu instanci** zaškrtávacího políčka umožníte aktualizace konfigurace pro role, aby bylo možné pokračovat.</span><span class="sxs-lookup"><span data-stu-id="271d0-143">If your cloud service contains any roles that have only one instance, select the **Apply configuration even if one or more roles contain a single instance** check box to enable the configuration updates for the roles to proceed.</span></span>
      
       <span data-ttu-id="271d0-144">Pokud definujete aspoň dvě instance každé role, Azure nemůže zaručit alespoň 99,95 % dostupnosti cloudové služby během aktualizace služby konfigurace.</span><span class="sxs-lookup"><span data-stu-id="271d0-144">Unless you define at least two instances of every role, Azure cannot guarantee at least 99.95 percent availability of your cloud service during service configuration updates.</span></span> <span data-ttu-id="271d0-145">Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="271d0-145">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>
   4. <span data-ttu-id="271d0-146">Klikněte na tlačítko **OK** (zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="271d0-146">Click **OK** (checkmark).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="271d0-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="271d0-147">Next steps</span></span>
* <span data-ttu-id="271d0-148">Zjistěte, jak [nasazení cloudové služby](cloud-services-how-to-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="271d0-148">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy.md).</span></span>
* <span data-ttu-id="271d0-149">Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="271d0-149">Configure a [custom domain name](cloud-services-custom-domain-name.md).</span></span>
* <span data-ttu-id="271d0-150">[Správa služby cloud](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="271d0-150">[Manage your cloud service](cloud-services-how-to-manage.md).</span></span>
* [<span data-ttu-id="271d0-151">Povolit připojení ke vzdálené ploše pro roli v cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="271d0-151">Enable Remote Desktop Connection for a Role in Azure Cloud Services</span></span>](cloud-services-role-enable-remote-desktop.md)
* <span data-ttu-id="271d0-152">Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).</span><span class="sxs-lookup"><span data-stu-id="271d0-152">Configure [ssl certificates](cloud-services-configure-ssl-certificate.md).</span></span>

