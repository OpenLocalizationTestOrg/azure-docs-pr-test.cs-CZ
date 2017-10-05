---
title: "Povolit připojení ke vzdálené ploše pro roli v cloudových služeb Azure | Microsoft Docs"
description: "Postup konfigurace aplikace služby azure cloud umožňující připojení ke vzdálené ploše"
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: 73ea1d64-1529-4d72-b58e-f6c10499e6bb
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: mmccrory
ms.openlocfilehash: 0ff7fde5f3753aa6a24fb0af54d68d0dc0bd96a4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a><span data-ttu-id="f516a-103">Povolit připojení ke vzdálené ploše pro roli v cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="f516a-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f516a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f516a-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="f516a-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="f516a-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="f516a-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f516a-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="f516a-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f516a-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)
>
>

<span data-ttu-id="f516a-108">Vzdálená plocha umožňuje přístup k ploše role, která běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="f516a-108">Remote Desktop enables you to access the desktop of a role running in Azure.</span></span> <span data-ttu-id="f516a-109">Připojení ke vzdálené ploše můžete odstraňovat a diagnostikovat problémy s aplikací, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="f516a-109">You can use a Remote Desktop connection to troubleshoot and diagnose problems with your application while it is running.</span></span>

<span data-ttu-id="f516a-110">Můžete povolit připojení ke vzdálené ploše v příslušné roli během vývoje zahrnutím moduly vzdálené plochy v definice služby nebo můžete povolit vzdálené plochy prostřednictvím vzdálené plochy rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f516a-110">You can enable a Remote Desktop connection in your role during development by including the Remote Desktop modules in your service definition or you can choose to enable Remote Desktop through the Remote Desktop Extension.</span></span> <span data-ttu-id="f516a-111">Použití rozšíření vzdálené plochy, protože vzdálená plocha můžete povolit i poté, co je aplikace nasazena, aniž by museli znovu nasaďte aplikaci je žádoucí.</span><span class="sxs-lookup"><span data-stu-id="f516a-111">The preferred approach is to use the Remote Desktop extension as you can enable Remote Desktop even after the application is deployed without having to redeploy your application.</span></span>

## <a name="configure-remote-desktop-from-the-azure-portal"></a><span data-ttu-id="f516a-112">Konfigurace vzdálené plochy z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f516a-112">Configure Remote Desktop from the Azure portal</span></span>
<span data-ttu-id="f516a-113">Portál Azure používá vzdálené plochy rozšíření přístup, takže Vzdálená plocha můžete povolit i poté, co je aplikace nasazená.</span><span class="sxs-lookup"><span data-stu-id="f516a-113">The Azure portal uses the Remote Desktop Extension approach so you can enable Remote Desktop even after the application is deployed.</span></span> <span data-ttu-id="f516a-114">**Vzdálené plochy** okno pro cloudové služby umožňuje povolit vzdálené plochy, změňte místní účet správce používá k připojení k virtuálním počítačům, certifikát používané v ověřování a nastavit datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="f516a-114">The **Remote Desktop** blade for your cloud service allows you to enable Remote Desktop, change the local Administrator account used to connect to the virtual machines, the certificate used in authentication and set the expiration date.</span></span>

1. <span data-ttu-id="f516a-115">Klikněte na tlačítko **cloudové služby**, klikněte na název cloudové služby a pak klikněte na tlačítko **vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="f516a-115">Click **Cloud Services**, click the name of the cloud service, and then click **Remote Desktop**.</span></span>

    ![Cloudové služby Vzdálená plocha](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. <span data-ttu-id="f516a-117">Zvolte, zda chcete povolit vzdálená plocha pro jednotlivé role nebo pro všechny role, pak změňte hodnotu přepínači na **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="f516a-117">Choose whether you want to enable Remote Desktop for an individual role or for all roles, then change the value of the switcher to **Enabled**.</span></span>

3. <span data-ttu-id="f516a-118">Vyplňte požadovaná pole pro uživatelské jméno, heslo, vypršení platnosti a certifikát.</span><span class="sxs-lookup"><span data-stu-id="f516a-118">Fill in the required fields for user name, password, expiry, and certificate.</span></span>

    ![Cloudové služby Vzdálená plocha](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > <span data-ttu-id="f516a-120">Všechny instance role se restartuje, při prvním povolení služby Vzdálená plocha a klikněte na tlačítko OK (zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="f516a-120">All role instances will be restarted when you first enable Remote Desktop and click OK (checkmark).</span></span> <span data-ttu-id="f516a-121">Abyste zabránili restartování, musí být nainstalovaný certifikát použitý k šifrování hesla v roli.</span><span class="sxs-lookup"><span data-stu-id="f516a-121">To prevent a reboot, the certificate used to encrypt the password must be installed on the role.</span></span> <span data-ttu-id="f516a-122">Abyste zabránili restartování, [nahrát certifikát pro cloudové služby](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) a pak se vraťte do tohoto dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="f516a-122">To prevent a restart, [upload a certificate for the cloud service](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) and then return to this dialog.</span></span>
   >
   >
3. <span data-ttu-id="f516a-123">V **role**, vyberte roli, kterou chcete aktualizovat nebo vyberte **všechny** u všech rolí.</span><span class="sxs-lookup"><span data-stu-id="f516a-123">In **Roles**, select the role you want to update or select **All** for all roles.</span></span>

4. <span data-ttu-id="f516a-124">Po dokončení aktualizace vaše konfigurace, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f516a-124">When you finish your configuration updates, click **Save**.</span></span> <span data-ttu-id="f516a-125">Bude trvat několik minut, než je připraven přijmout připojení instance role.</span><span class="sxs-lookup"><span data-stu-id="f516a-125">It will take a few moments before your role instances are ready to receive connections.</span></span>

## <a name="remote-into-role-instances"></a><span data-ttu-id="f516a-126">Vzdálené do instance rolí</span><span class="sxs-lookup"><span data-stu-id="f516a-126">Remote into role instances</span></span>
<span data-ttu-id="f516a-127">Jakmile povolíte vzdálené plochy na rolích, můžete zahájit připojení přímo z portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="f516a-127">Once Remote Desktop is enabled on the roles, you can initiate a connection directly from the Azure Portal:</span></span>

1. <span data-ttu-id="f516a-128">Klikněte na tlačítko **instance** otevřete **instance** okno.</span><span class="sxs-lookup"><span data-stu-id="f516a-128">Click **Instances** to open the **Instances** blade.</span></span>
2. <span data-ttu-id="f516a-129">Vyberte instanci role, která má nakonfigurované připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="f516a-129">Select a role instance that has Remote Desktop configured.</span></span>
3. <span data-ttu-id="f516a-130">Klikněte na tlačítko **Connect** stáhnout soubor RDP pro instanci role.</span><span class="sxs-lookup"><span data-stu-id="f516a-130">Click **Connect** to download an RDP file for the role instance.</span></span>

    ![Cloudové služby Vzdálená plocha](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. <span data-ttu-id="f516a-132">Klikněte na tlačítko **otevřete** a potom **Connect** spustit připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="f516a-132">Click **Open** and then **Connect** to start the Remote Desktop connection.</span></span>

>[!NOTE]
> <span data-ttu-id="f516a-133">Pokud cloudové služby nachází za skupinu NSG, musíte vytvořit pravidla, která povolí komunikaci na portech **3389** a **20000**.</span><span class="sxs-lookup"><span data-stu-id="f516a-133">If your cloud service is sitting behind an NSG, you may need to create rules that allow traffic on ports **3389** and **20000**.</span></span>  <span data-ttu-id="f516a-134">Vzdálená plocha používá port **3389**.</span><span class="sxs-lookup"><span data-stu-id="f516a-134">Remote Desktop uses port **3389**.</span></span>  <span data-ttu-id="f516a-135">Instance cloudové služby jsou Vyrovnávané, takže nemůže přímo řídit kterou instanci pro připojení k.</span><span class="sxs-lookup"><span data-stu-id="f516a-135">Cloud Service instances are load balanced, so you can't directly control which instance to connect to.</span></span>  <span data-ttu-id="f516a-136">*RemoteForwarder* a *RemoteAccess* agenty spravovat provoz protokolu RDP a umožňují klientu odesílat soubor cookie s RDP a zadejte jednotlivé instance pro připojení k.</span><span class="sxs-lookup"><span data-stu-id="f516a-136">The *RemoteForwarder* and *RemoteAccess* agents manage RDP traffic and allow the client to send an RDP cookie and specify an individual instance to connect to.</span></span>  <span data-ttu-id="f516a-137">*RemoteForwarder* a *RemoteAccess* agentů vyžadují tento port **20000*** otevřít, což může být zablokován, pokud máte skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="f516a-137">The *RemoteForwarder* and *RemoteAccess* agents require that port **20000*** be opened, which may be blocked if you have an NSG.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f516a-138">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f516a-138">Additional resources</span></span>

<span data-ttu-id="f516a-139">[Postup konfigurace cloudové služby](cloud-services-how-to-configure.md)
[cloudových služeb časté otázky – vzdálené plochy](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="f516a-139">[How to Configure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
