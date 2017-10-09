---
title: "aaaEnable připojení ke vzdálené ploše pro roli ve službě Azure Cloud Services | Microsoft Docs"
description: "Jak tooconfigure vaše azure cloudové služby aplikace tooallow připojení ke vzdálené ploše"
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
ms.openlocfilehash: 55d7043df571c2e88b04aa9ef01dc8ae1d6784f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a><span data-ttu-id="605e4-103">Povolit připojení ke vzdálené ploše pro roli v cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="605e4-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="605e4-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="605e4-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="605e4-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="605e4-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="605e4-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="605e4-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="605e4-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="605e4-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)
>
>

<span data-ttu-id="605e4-108">Vzdálená plocha umožňuje tooaccess hello ploše role, která běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="605e4-108">Remote Desktop enables you tooaccess hello desktop of a role running in Azure.</span></span> <span data-ttu-id="605e4-109">Můžete použít tootroubleshoot připojení vzdálené plochy a diagnostikovat problémy s vaší aplikací, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="605e4-109">You can use a Remote Desktop connection tootroubleshoot and diagnose problems with your application while it is running.</span></span>

<span data-ttu-id="605e4-110">Můžete povolit připojení ke vzdálené ploše v příslušné roli během vývoje začleněním modulů hello vzdálené plochy v definice služby nebo můžete zvolit tooenable vzdálené plochy prostřednictvím hello rozšíření vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="605e4-110">You can enable a Remote Desktop connection in your role during development by including hello Remote Desktop modules in your service definition or you can choose tooenable Remote Desktop through hello Remote Desktop Extension.</span></span> <span data-ttu-id="605e4-111">Hello žádoucí je toouse hello vzdálené plochy rozšíření jako Vzdálená plocha můžete povolit i po nasazení aplikace hello bez nutnosti tooredeploy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="605e4-111">hello preferred approach is toouse hello Remote Desktop extension as you can enable Remote Desktop even after hello application is deployed without having tooredeploy your application.</span></span>

## <a name="configure-remote-desktop-from-hello-azure-portal"></a><span data-ttu-id="605e4-112">Konfigurace vzdálené plochy z hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="605e4-112">Configure Remote Desktop from hello Azure portal</span></span>
<span data-ttu-id="605e4-113">Hello portál Azure používá přístup vzdálené plochy rozšíření hello, takže Vzdálená plocha můžete povolit i po nasazení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="605e4-113">hello Azure portal uses hello Remote Desktop Extension approach so you can enable Remote Desktop even after hello application is deployed.</span></span> <span data-ttu-id="605e4-114">Hello **vzdálené plochy** okno pro cloudové služby vám umožní tooenable vzdálené plochy, změna hello místní účet správce používá tooconnect toohello virtuální počítače, hello certifikátu používané v ověřování a nastavte hello Datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="605e4-114">hello **Remote Desktop** blade for your cloud service allows you tooenable Remote Desktop, change hello local Administrator account used tooconnect toohello virtual machines, hello certificate used in authentication and set hello expiration date.</span></span>

1. <span data-ttu-id="605e4-115">Klikněte na tlačítko **cloudové služby**, klikněte na název hello hello cloudové služby a pak klikněte na tlačítko **vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="605e4-115">Click **Cloud Services**, click hello name of hello cloud service, and then click **Remote Desktop**.</span></span>

    ![Cloudové služby Vzdálená plocha](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. <span data-ttu-id="605e4-117">Zvolte, zda jste tooenable vzdálené plochy pro jednotlivé role nebo pro všechny role, a potom změnit hodnotu hello hello přepínači příliš**povoleno**.</span><span class="sxs-lookup"><span data-stu-id="605e4-117">Choose whether you want tooenable Remote Desktop for an individual role or for all roles, then change hello value of hello switcher too**Enabled**.</span></span>

3. <span data-ttu-id="605e4-118">Vyplňte pole hello požadované pro uživatelské jméno, heslo, vypršení platnosti a certifikát.</span><span class="sxs-lookup"><span data-stu-id="605e4-118">Fill in hello required fields for user name, password, expiry, and certificate.</span></span>

    ![Cloudové služby Vzdálená plocha](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > <span data-ttu-id="605e4-120">Všechny instance role se restartuje, při prvním povolení služby Vzdálená plocha a klikněte na tlačítko OK (zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="605e4-120">All role instances will be restarted when you first enable Remote Desktop and click OK (checkmark).</span></span> <span data-ttu-id="605e4-121">tooprevent restartu hello certifikátů používaných tooencrypt hello heslo musí být nainstalován na hello role.</span><span class="sxs-lookup"><span data-stu-id="605e4-121">tooprevent a reboot, hello certificate used tooencrypt hello password must be installed on hello role.</span></span> <span data-ttu-id="605e4-122">tooprevent restartování, [nahrát certifikát pro cloudové služby hello](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) a pak se vraťte toothis dialogu.</span><span class="sxs-lookup"><span data-stu-id="605e4-122">tooprevent a restart, [upload a certificate for hello cloud service](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) and then return toothis dialog.</span></span>
   >
   >
3. <span data-ttu-id="605e4-123">V **role**, vyberte roli hello tooupdate nebo vyberte **všechny** u všech rolí.</span><span class="sxs-lookup"><span data-stu-id="605e4-123">In **Roles**, select hello role you want tooupdate or select **All** for all roles.</span></span>

4. <span data-ttu-id="605e4-124">Po dokončení aktualizace vaše konfigurace, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="605e4-124">When you finish your configuration updates, click **Save**.</span></span> <span data-ttu-id="605e4-125">To bude trvat chvíli instance role jsou připravené tooreceive připojení.</span><span class="sxs-lookup"><span data-stu-id="605e4-125">It will take a few moments before your role instances are ready tooreceive connections.</span></span>

## <a name="remote-into-role-instances"></a><span data-ttu-id="605e4-126">Vzdálené do instance rolí</span><span class="sxs-lookup"><span data-stu-id="605e4-126">Remote into role instances</span></span>
<span data-ttu-id="605e4-127">Po povolení vzdálené plochy na hello role můžete zahájit připojení přímo z portálu Azure hello:</span><span class="sxs-lookup"><span data-stu-id="605e4-127">Once Remote Desktop is enabled on hello roles, you can initiate a connection directly from hello Azure Portal:</span></span>

1. <span data-ttu-id="605e4-128">Klikněte na tlačítko **instance** tooopen hello **instance** okno.</span><span class="sxs-lookup"><span data-stu-id="605e4-128">Click **Instances** tooopen hello **Instances** blade.</span></span>
2. <span data-ttu-id="605e4-129">Vyberte instanci role, která má nakonfigurované připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="605e4-129">Select a role instance that has Remote Desktop configured.</span></span>
3. <span data-ttu-id="605e4-130">Klikněte na tlačítko **Connect** souboru toodownload protokolu RDP pro instanci role hello.</span><span class="sxs-lookup"><span data-stu-id="605e4-130">Click **Connect** toodownload an RDP file for hello role instance.</span></span>

    ![Cloudové služby Vzdálená plocha](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. <span data-ttu-id="605e4-132">Klikněte na tlačítko **otevřete** a potom **připojit** toostart hello připojení vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="605e4-132">Click **Open** and then **Connect** toostart hello Remote Desktop connection.</span></span>

>[!NOTE]
> <span data-ttu-id="605e4-133">Pokud cloudové služby nachází za skupinu NSG, může být nutné toocreate pravidla, která umožňují přenosy na portech **3389** a **20000**.</span><span class="sxs-lookup"><span data-stu-id="605e4-133">If your cloud service is sitting behind an NSG, you may need toocreate rules that allow traffic on ports **3389** and **20000**.</span></span>  <span data-ttu-id="605e4-134">Vzdálená plocha používá port **3389**.</span><span class="sxs-lookup"><span data-stu-id="605e4-134">Remote Desktop uses port **3389**.</span></span>  <span data-ttu-id="605e4-135">Cloudové služby instance jsou vyrovnáváním, zatížení, takže nemůže přímo řídit které tooconnect instance k.</span><span class="sxs-lookup"><span data-stu-id="605e4-135">Cloud Service instances are load balanced, so you can't directly control which instance tooconnect to.</span></span>  <span data-ttu-id="605e4-136">Hello *RemoteForwarder* a *RemoteAccess* agenty spravovat provoz protokolu RDP a povolit hello klienta toosend soubor cookie s RDP a zadejte jednotlivé instance tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="605e4-136">hello *RemoteForwarder* and *RemoteAccess* agents manage RDP traffic and allow hello client toosend an RDP cookie and specify an individual instance tooconnect to.</span></span>  <span data-ttu-id="605e4-137">Hello *RemoteForwarder* a *RemoteAccess* agentů vyžadují tento port **20000*** otevřít, což může být zablokován, pokud máte skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="605e4-137">hello *RemoteForwarder* and *RemoteAccess* agents require that port **20000*** be opened, which may be blocked if you have an NSG.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="605e4-138">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="605e4-138">Additional resources</span></span>

<span data-ttu-id="605e4-139">[Jak tooConfigure cloudové služby](cloud-services-how-to-configure.md)
[cloudových služeb časté otázky – vzdálené plochy](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="605e4-139">[How tooConfigure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
