---
title: "aaaAccessing aplikace z libovolného zařízení | Microsoft Docs"
description: "Další informace, klientů, kteří jsou podporovány pro Azure RemoteApp a jak tooaccess vaší aplikace."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: fb7bd17d-7aa8-43fd-9278-f96e0e9308e4
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 15985b40d870e3155d4132063bf5b9677ff9afed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-your-apps-in-azure-remoteapp"></a><span data-ttu-id="f59e3-103">Přístup k aplikacím ve službě Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="f59e3-103">Accessing your apps in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f59e3-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="f59e3-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="f59e3-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f59e3-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="f59e3-106">Jedním z beauties hello Azure RemoteApp je, že vám přístup k aplikacím, z libovolného zařízení.</span><span class="sxs-lookup"><span data-stu-id="f59e3-106">One of hello beauties of Azure RemoteApp is that you can access apps from any of your devices.</span></span> <span data-ttu-id="f59e3-107">I lépe můžete začít pracovat na jednom zařízení a bezproblémově přechod tooa druhé zařízení a vyzvednutí vpravo, kde jste přestali.</span><span class="sxs-lookup"><span data-stu-id="f59e3-107">Even better, you can start working on one device and then seamlessly transition tooa second device and pick up right where you left off.</span></span> <span data-ttu-id="f59e3-108">tooget spuštění, budete potřebovat toodownload hello příslušného klienta pro vaše zařízení a přihlaste se toohello služby.</span><span class="sxs-lookup"><span data-stu-id="f59e3-108">tooget started you need toodownload hello appropriate client for your device and sign in toohello service.</span></span>

<span data-ttu-id="f59e3-109">V tomto tématu přečtěte hello klientům aktuálně podporované a jak toodownload je před I ukazují, jak toosign v tooRemoteApp z každé hello klientům.</span><span class="sxs-lookup"><span data-stu-id="f59e3-109">In this topic, we'll review hello clients currently supported and how toodownload them before I show you how toosign in tooRemoteApp from each of hello clients.</span></span>

## <a name="supported-clients"></a><span data-ttu-id="f59e3-110">Podporovaní klienti</span><span class="sxs-lookup"><span data-stu-id="f59e3-110">Supported clients</span></span>
<span data-ttu-id="f59e3-111">Mají přístup ke vzdálené aplikace RemoteApp pomocí následujících kroků hello, pokud zařízení s jedním z těchto operačních systémů:</span><span class="sxs-lookup"><span data-stu-id="f59e3-111">You can access RemoteApp using hello steps below if your device is running one of these operating systems:</span></span>

* <span data-ttu-id="f59e3-112">Windows 10</span><span class="sxs-lookup"><span data-stu-id="f59e3-112">Windows 10</span></span> 
* <span data-ttu-id="f59e3-113">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="f59e3-113">Windows 8.1</span></span>
* <span data-ttu-id="f59e3-114">Windows 8</span><span class="sxs-lookup"><span data-stu-id="f59e3-114">Windows 8</span></span>
* <span data-ttu-id="f59e3-115">Windows 7 Service Pack 1</span><span class="sxs-lookup"><span data-stu-id="f59e3-115">Windows 7 Service Pack 1</span></span>
* <span data-ttu-id="f59e3-116">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="f59e3-116">Windows Phone 8.1</span></span>
* <span data-ttu-id="f59e3-117">iOS</span><span class="sxs-lookup"><span data-stu-id="f59e3-117">iOS</span></span>
* <span data-ttu-id="f59e3-118">Mac OS X</span><span class="sxs-lookup"><span data-stu-id="f59e3-118">Mac OS X</span></span>
* <span data-ttu-id="f59e3-119">Android</span><span class="sxs-lookup"><span data-stu-id="f59e3-119">Android</span></span>

 <span data-ttu-id="f59e3-120">Co tenké klienty? jsou podporovány následující tenké klienty systému Windows Embedded Hello:</span><span class="sxs-lookup"><span data-stu-id="f59e3-120">What about thin clients? hello following Windows Embedded thin clients are supported:</span></span>

* <span data-ttu-id="f59e3-121">Windows Embedded Standard 7</span><span class="sxs-lookup"><span data-stu-id="f59e3-121">Windows Embedded Standard 7</span></span>
* <span data-ttu-id="f59e3-122">Windows Embedded 8 Standard</span><span class="sxs-lookup"><span data-stu-id="f59e3-122">Windows Embedded 8 Standard</span></span>
* <span data-ttu-id="f59e3-123">Windows Embedded 8.1 Industry Pro</span><span class="sxs-lookup"><span data-stu-id="f59e3-123">Windows Embedded 8.1 Industry Pro</span></span>
* <span data-ttu-id="f59e3-124">Windows 10 IoT Enterprise</span><span class="sxs-lookup"><span data-stu-id="f59e3-124">Windows 10 IoT Enterprise</span></span>

## <a name="downloading-hello-client"></a><span data-ttu-id="f59e3-125">Stahování hello klienta</span><span class="sxs-lookup"><span data-stu-id="f59e3-125">Downloading hello client</span></span>
<span data-ttu-id="f59e3-126">Bez ohledu na to, jakou platformu, kterou používáte, klient hello potřebujete tooaccess vzdálené aplikace RemoteApp najdete na hello [stažení klienta vzdálené plochy](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="f59e3-126">No matter what platform you are using, hello client you need tooaccess RemoteApp can be found on hello [Remote Desktop client download](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) page.</span></span>

<span data-ttu-id="f59e3-127">Kliknutím na odkazy různých hello buď přímo spustí stahování hello klienta nebo vám pošle stránky pro stažení klienta toohello v obchodě s aplikacemi hello pro platformu.</span><span class="sxs-lookup"><span data-stu-id="f59e3-127">Clicking hello different links will either directly start downloading hello client or will send you toohello client download page in hello app store for that platform.</span></span> <span data-ttu-id="f59e3-128">Instalace klienta hello podle pokynů hello na úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="f59e3-128">Install hello client by following hello instructions on hello screen.</span></span>

<span data-ttu-id="f59e3-129">Po instalaci klienta hello na vašem zařízení a jeho spuštění, přejít toohello odpovídající části níže toolearn jak toosign v tooRemoteApp z tohoto klienta.</span><span class="sxs-lookup"><span data-stu-id="f59e3-129">Once you have installed hello client on your device and launched it, jump toohello corresponding section below toolearn how toosign in tooRemoteApp from that client.</span></span>

## <a name="android"></a><span data-ttu-id="f59e3-130">Android</span><span class="sxs-lookup"><span data-stu-id="f59e3-130">Android</span></span>
<span data-ttu-id="f59e3-131">Po instalaci aplikace Vzdálená plocha od Microsoftu hello z hello obchodu Google Play, můžete ji najít v seznamu aplikací v rámci **vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-131">Once you have installed hello Microsoft Remote Desktop app from hello Google Play store, you can find it in your app list under **Remote Desktop**.</span></span>

1. <span data-ttu-id="f59e3-132">Spuštění aplikace hello přináší tooan prázdný Center připojení, pokud jste již dosud používali aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f59e3-132">Launching hello app brings you tooan empty Connection Center, unless you've already been using hello app.</span></span> <span data-ttu-id="f59e3-133">tooget začít s Azure Remoteappem, hello klepněte na tlačítko Přidat **"" +""** a klepněte na **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-133">tooget started with Azure RemoteApp, tap hello add button **""+""** and tap **Azure RemoteApp**.</span></span>    
   
     ![Prázdný připojení Center](./media/remoteapp-clients/Android1.png)
2. <span data-ttu-id="f59e3-135">Je nutné toosign se pomocí vaší e-mailovou adresu tooaccess hello službu.</span><span class="sxs-lookup"><span data-stu-id="f59e3-135">You need toosign in with your email address tooaccess hello service.</span></span> <span data-ttu-id="f59e3-136">Klepněte na **Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-136">Tap **Get started**.</span></span>
   
    ![Přihlaste se řádku](./media/remoteapp-clients/Android2.png)
3. <span data-ttu-id="f59e3-138">Na další stránku hello, zadejte do vaší **e-mailová adresa** a klepněte na **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-138">On hello next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="f59e3-139">To začne hello přihlášení pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f59e3-139">This begins hello sign-in process using Azure Active Directory.</span></span>
   
    ![První stránka Azure Active Directory](./media/remoteapp-clients/Android3.png)
4. <span data-ttu-id="f59e3-141">Postupujte podle pokynů hello na obrazovce toosign hello se pomocí účtu Microsoft (dříve nazývané "LiveID") a ID organizace.</span><span class="sxs-lookup"><span data-stu-id="f59e3-141">Follow hello instructions on hello screen toosign in with your Microsoft account (previously called "LiveID") or organization ID.</span></span> <span data-ttu-id="f59e3-142">Po přihlášení může zobrazí se stránka výpis všech hello pozvánky, kterou jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="f59e3-142">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="f59e3-143">Pokud jste, vyberte hello pozvánky, kterým důvěřujete a klepněte na **provádí**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-143">If you are, select hello invitations you trust and tap **Done**.</span></span>    
   
    ![Stránka pozvánek](./media/remoteapp-clients/Android4.png)
5. <span data-ttu-id="f59e3-145">Po přijetí pozvánky, hello seznam aplikací mít přístup toowill být stažené tooyour zařízení a k dispozici v hello Center připojení.</span><span class="sxs-lookup"><span data-stu-id="f59e3-145">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="f59e3-146">Klepněte na jednu z toostart aplikace hello použití.</span><span class="sxs-lookup"><span data-stu-id="f59e3-146">Tap one of hello apps toostart using it.</span></span>
   
    ![Centrum připojení s informačního kanálu](./media/remoteapp-clients/Android5.png)
6. <span data-ttu-id="f59e3-148">Pokud jste pozvánku ještě stále můžete vyzkoušet hello služby.</span><span class="sxs-lookup"><span data-stu-id="f59e3-148">If you do not have an invitation yet, you can still try out hello service.</span></span> <span data-ttu-id="f59e3-149">Ano, klepněte na toodo **přejděte zkušební verze toofree** po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="f59e3-149">toodo so, tap **Go toofree trial** when prompted.</span></span>
   
    ![Ukázkový dotaz kanálu](./media/remoteapp-clients/Android6.png)
7. <span data-ttu-id="f59e3-151">Tím získáte přístup k tooa základní sadu aplikací tooget jste začali s vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="f59e3-151">This will give you access tooa basic set of apps tooget you started with RemoteApp.</span></span>
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/Android7.png)

## <a name="ios"></a><span data-ttu-id="f59e3-153">iOS</span><span class="sxs-lookup"><span data-stu-id="f59e3-153">iOS</span></span>
<span data-ttu-id="f59e3-154">Po instalaci aplikace Vzdálená plocha od Microsoftu hello z obchodu s aplikacemi text hello, můžete ji najít v seznamu aplikací v rámci **klientovi služby Vzdálená plocha**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-154">Once you have installed hello Microsoft Remote Desktop app from hello App store, you can find it in your app list under **RD Client**.</span></span>

1. <span data-ttu-id="f59e3-155">Spuštění aplikace hello přináší tooan prázdný Center připojení, pokud jste již dosud používali aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f59e3-155">Launching hello app brings you tooan empty Connection Center, unless you've already been using hello app.</span></span> <span data-ttu-id="f59e3-156">tooget začít s Azure Remoteappem, hello klepněte na tlačítko Přidat **"" +""** a klepněte na **přidat Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-156">tooget started with Azure RemoteApp, tap hello add button **""+""** and tap **Add Azure RemoteApp**.</span></span>
   
    ![Prázdný připojení Center](./media/remoteapp-clients/IOS1.png)
2. <span data-ttu-id="f59e3-158">Je třeba toosign se pomocí vaší e-mailovou adresu tooaccess hello služby, toostart tohoto procesu zadejte vaše **e-mailová adresa** a klepněte na **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-158">You need toosign in with your email address tooaccess hello service, toostart that process, type in your **email address** and tap **Continue**.</span></span>
   
    ![Přihlaste se řádku](./media/remoteapp-clients/picture1.png)
3. <span data-ttu-id="f59e3-160">Postupujte podle pokynů hello na obrazovce toosign hello se pomocí účtu Microsoft (LiveID) a ID organizace.</span><span class="sxs-lookup"><span data-stu-id="f59e3-160">Follow hello instructions on hello screen toosign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="f59e3-161">Po přihlášení může zobrazí se stránka výpis všech hello pozvánky, kterou jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="f59e3-161">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="f59e3-162">Pokud jste, vyberte hello pozvánky, kterým důvěřujete a klepněte na **provádí**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-162">If you are, select hello invitations you trust and tap **Done**.</span></span>
   
    ![Stránka pozvánek](./media/remoteapp-clients/IOS3.png)
4. <span data-ttu-id="f59e3-164">Po přijetí pozvánky, hello seznam aplikací mít přístup toowill být stažené tooyour zařízení a k dispozici v hello Center připojení.</span><span class="sxs-lookup"><span data-stu-id="f59e3-164">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="f59e3-165">Ji a spusťte ji pomocí, klepněte na jednu z toolaunch aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f59e3-165">Tap one of hello apps toolaunch it and start using it.</span></span>
   
    ![Centrum připojení s informačního kanálu](./media/remoteapp-clients/IOS4.png)
5. <span data-ttu-id="f59e3-167">Pokud jste pozvánku ještě stále můžete vyzkoušet hello služby.</span><span class="sxs-lookup"><span data-stu-id="f59e3-167">If you do not have an invitation yet, you can still try out hello service.</span></span> <span data-ttu-id="f59e3-168">Ano, klepněte na toodo **přejděte zkušební verze toofree** po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="f59e3-168">toodo so, tap **Go toofree trial** when prompted.</span></span>
   
    ![Ukázkový dotaz kanálu](./media/remoteapp-clients/IOS5.png)
6. <span data-ttu-id="f59e3-170">Tím získáte přístup k tooa základní sadu aplikací tooget jste začali s vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="f59e3-170">This will give you access tooa basic set of apps tooget you started with RemoteApp.</span></span>
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/IOS6.png)

## <a name="mac-os-x"></a><span data-ttu-id="f59e3-172">Mac OS X</span><span class="sxs-lookup"><span data-stu-id="f59e3-172">Mac OS X</span></span>
<span data-ttu-id="f59e3-173">Po instalaci aplikace Vzdálená plocha od Microsoftu hello z obchodu s aplikacemi text hello, můžete ji najít v seznamu aplikací v rámci **Vzdálená plocha od Microsoftu**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-173">Once you have installed hello Microsoft Remote Desktop app from hello App store, you can find it in your app list under **Microsoft Remote Desktop**.</span></span>

1. <span data-ttu-id="f59e3-174">Spuštění aplikace hello přináší tooan prázdný Center připojení, pokud jste již dosud používali aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f59e3-174">Launching hello app brings you tooan empty Connection Center, unless you've already been using hello app.</span></span> <span data-ttu-id="f59e3-175">tooget začít s Azure Remoteappem, klikněte na tlačítko hello **Azure RemoteApp** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f59e3-175">tooget started with Azure RemoteApp, click hello **Azure RemoteApp** button.</span></span>
   
    ![Prázdný připojení Center](./media/remoteapp-clients/Mac1.png)
2. <span data-ttu-id="f59e3-177">Je třeba toosign se pomocí vaší e-mailovou adresu tooaccess hello služby, toostart, že proces, klepněte na **Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-177">You need toosign in with your email address tooaccess hello service, toostart that process, tap **Get Started**.</span></span>
   
    ![Přihlaste se řádku](./media/remoteapp-clients/Mac2.png)
3. <span data-ttu-id="f59e3-179">Na další stránku hello, zadejte do vaší **e-mailová adresa** a klepněte na **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-179">On hello next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="f59e3-180">To začne hello přihlašovací proces pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f59e3-180">This begins hello sign in process using Azure Active Directory.</span></span>
   
    ![První stránka Azure Active Directory](./media/remoteapp-clients/picture2.png)
4. <span data-ttu-id="f59e3-182">Postupujte podle pokynů hello na obrazovce toosign hello se pomocí účtu Microsoft (LiveID) a ID organizace.</span><span class="sxs-lookup"><span data-stu-id="f59e3-182">Follow hello instructions on hello screen toosign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="f59e3-183">Po přihlášení může zobrazí se stránka výpis všech hello pozvánky, kterou jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="f59e3-183">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="f59e3-184">Pokud jste, vyberte hello pozvánky, kterým důvěřujete a zavřete dialogové okno hello.</span><span class="sxs-lookup"><span data-stu-id="f59e3-184">If you are, select hello invitations you trust and close hello dialog.</span></span>
   
    ![Stránka pozvánek](./media/remoteapp-clients/Mac4.png)
5. <span data-ttu-id="f59e3-186">Po přijetí pozvánky, hello seznam aplikací mít přístup toowill být stažené tooyour zařízení a k dispozici v hello Center připojení.</span><span class="sxs-lookup"><span data-stu-id="f59e3-186">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="f59e3-187">Dvakrát klikněte na toolaunch aplikace hello ji a spusťte ji pomocí.</span><span class="sxs-lookup"><span data-stu-id="f59e3-187">Double-click one of hello apps toolaunch it and start using it.</span></span>
   
    ![Centrum připojení s informačního kanálu](./media/remoteapp-clients/Mac5.png)
6. <span data-ttu-id="f59e3-189">Pokud jste pozvánku ještě stále můžete vyzkoušet hello služby.</span><span class="sxs-lookup"><span data-stu-id="f59e3-189">If you do not have an invitation yet, you can still try out hello service.</span></span> <span data-ttu-id="f59e3-190">Ano, klikněte na tlačítko toodo **přejděte zkušební verze toofree** po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="f59e3-190">toodo so, click **Go toofree trial** when prompted.</span></span>
   
    ![Ukázkový dotaz kanálu](./media/remoteapp-clients/Mac6.png)
7. <span data-ttu-id="f59e3-192">Tím získáte přístup k tooa základní sadu aplikací tooget jste začali s vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="f59e3-192">This will give you access tooa basic set of apps tooget you started with RemoteApp.</span></span>
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/Mac7.png)

## <a name="windows-all-supported-versions-except-windows-phone"></a><span data-ttu-id="f59e3-194">Windows (s výjimkou Windows Phone všechny podporované verze)</span><span class="sxs-lookup"><span data-stu-id="f59e3-194">Windows (All supported versions except Windows Phone)</span></span>
<span data-ttu-id="f59e3-195">Hello klienta spustí automaticky po dokončení instalace, ale pokud budete potřebovat tooaccess ho později se nachází v seznamu aplikací pod názvem hello **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-195">hello client launches automatically after it finishes installing, however when you need tooaccess it again later it can be found in your app list under hello name **Azure RemoteApp**.</span></span>

1. <span data-ttu-id="f59e3-196">Spuštění klienta hello hello první stránku, kterou vidíte hřívací zařízení vás vítá tooAzure vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="f59e3-196">Ater launching hello client, hello first page you see welcomes you tooAzure RemoteApp.</span></span> <span data-ttu-id="f59e3-197">tooproceed, kliknutím na tlačítko **Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-197">tooproceed, click on **Get Started**.</span></span>
   
    ![Úvodní stránka klientovi Azure Remoteappu hello](./media/remoteapp-clients/Windows1.png)
2. <span data-ttu-id="f59e3-199">Další stránku Hello spustí přihlašovací hello v procesu pro Azure RemoteApp pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f59e3-199">hello next page starts hello sign in process for Azure RemoteApp using Azure Active Directory.</span></span> <span data-ttu-id="f59e3-200">Tento proces by měla vypadat povědomě, pokud jste použili služby společnosti Microsoft v minulosti hello.</span><span class="sxs-lookup"><span data-stu-id="f59e3-200">This process should look familiar if you have used Microsoft services in hello past.</span></span> <span data-ttu-id="f59e3-201">Začněte tím, že zadáte vaše **e-mailová adresa** a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-201">Start by typing your **email address** and click **Continue**.</span></span>
   
    ![První výzva služby Azure Active Directory](./media/remoteapp-clients/Windows2.png)
3. <span data-ttu-id="f59e3-203">Postupujte podle pokynů hello na obrazovce toosign hello se pomocí účtu Microsoft (LiveID) a ID organizace.</span><span class="sxs-lookup"><span data-stu-id="f59e3-203">Follow hello instructions on hello screen toosign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="f59e3-204">Po přihlášení může zobrazí se stránka výpis všech hello pozvánky, kterou jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="f59e3-204">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="f59e3-205">Pokud jste, vyberte hello pozvánky, kterým důvěřujete a klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-205">If you are, select hello invitations you trust and click **Done**.</span></span>
   
    ![Pozvánek stránku hello klientovi Azure Remoteappu](./media/remoteapp-clients/Windows3.png)
4. <span data-ttu-id="f59e3-207">Po přijetí pozvánky, hello seznam aplikací mít přístup toowill být stažené tooyour zařízení a k dispozici v hello Center připojení.</span><span class="sxs-lookup"><span data-stu-id="f59e3-207">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="f59e3-208">Dvakrát klikněte na toolaunch aplikace hello ji a spusťte ji pomocí.</span><span class="sxs-lookup"><span data-stu-id="f59e3-208">Double-click one of hello apps toolaunch it and start using it.</span></span>
   
    ![Připojení středu klientovi Azure Remoteappu hello](./media/remoteapp-clients/Windows4.png)
5. <span data-ttu-id="f59e3-210">Pokud žádná vám poslal ještě pozvánku, nemusíte si dělat starosti My jsme vám zahrnutých!</span><span class="sxs-lookup"><span data-stu-id="f59e3-210">If no one has sent you an invitation yet, don't worry we've got you covered!</span></span> <span data-ttu-id="f59e3-211">Dál budete mít přístup tooa ukázku kolekce abyste je mohli otestovat out hello služby.</span><span class="sxs-lookup"><span data-stu-id="f59e3-211">You'll still have access tooa demo collection so you can test out hello service.</span></span>
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/Windows5.png)

## <a name="windows-phone-81"></a><span data-ttu-id="f59e3-213">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="f59e3-213">Windows Phone 8.1</span></span>
<span data-ttu-id="f59e3-214">Po instalaci aplikace Vzdálená plocha od Microsoftu hello z úložiště hello Windows Phone 8.1, můžete ji najít v seznamu aplikací v rámci **vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-214">Once you have installed hello Microsoft Remote Desktop app from hello Windows Phone 8.1 store, you can find it in your app list under **Remote Desktop**.</span></span>

1. <span data-ttu-id="f59e3-215">Spuštění aplikace hello přináší můžete přímo tooan prázdný Center připojení, pokud jste již dosud používali aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f59e3-215">Launching hello app brings you directly tooan empty Connection Center, unless you've already been using hello app.</span></span> <span data-ttu-id="f59e3-216">tooget začít s Azure Remoteappem, hello klepněte na tlačítko Přidat **"" +""** v hello dolní části obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="f59e3-216">tooget started with Azure RemoteApp, tap hello add button **""+""** at hello bottom of hello screen.</span></span>
   
    ![Prázdný připojení Center](./media/remoteapp-clients/WinPhone1.png)
2. <span data-ttu-id="f59e3-218">Potom klepněte na **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-218">Next, tap on **Azure RemoteApp**.</span></span>
   
    ![Přidat stránku položek](./media/remoteapp-clients/WinPhone2.png)
3. <span data-ttu-id="f59e3-220">Je třeba toosign se pomocí vaší e-mailovou adresu tooaccess hello služby, toostart, že proces, klepněte na **připojit**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-220">You need toosign in with your email address tooaccess hello service, toostart that process, tap **connect**.</span></span>
   
    ![Přihlaste se řádku](./media/remoteapp-clients/WinPhone3.png)
4. <span data-ttu-id="f59e3-222">Na další stránku hello, zadejte do vaší **e-mailová adresa** a klepněte na **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-222">On hello next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="f59e3-223">To začne hello přihlašovací proces pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f59e3-223">This begins hello sign in process using Azure Active Directory.</span></span>
   
    ![První stránka Azure Active Directory](./media/remoteapp-clients/WinPhone4.png)
5. <span data-ttu-id="f59e3-225">Postupujte podle pokynů hello na obrazovce toosign hello se pomocí účtu Microsoft (LiveID) a ID organizace.</span><span class="sxs-lookup"><span data-stu-id="f59e3-225">Follow hello instructions on hello screen toosign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="f59e3-226">Po přihlášení může zobrazí se stránka výpis všech hello pozvánky, kterou jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="f59e3-226">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="f59e3-227">Pokud jste, vyberte hello pozvánky, kterým důvěřujete a klepněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f59e3-227">If you are, select hello invitations you trust and tap **save**.</span></span>
   
    ![Stránka pozvánek](./media/remoteapp-clients/WinPhone5.png)
6. <span data-ttu-id="f59e3-229">Po přijetí pozvánky, hello seznam aplikací mít přístup toowill být stažené tooyour zařízení a k dispozici v hello Center připojení.</span><span class="sxs-lookup"><span data-stu-id="f59e3-229">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="f59e3-230">Ji a spusťte ji pomocí, klepněte na jednu z toolaunch aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f59e3-230">Tap one of hello apps toolaunch it and start using it.</span></span>
   
    ![Centrum připojení s informačního kanálu](./media/remoteapp-clients/WinPhone6.png)
7. <span data-ttu-id="f59e3-232">Pokud jste pozvánku ještě stále můžete vyzkoušet hello služby.</span><span class="sxs-lookup"><span data-stu-id="f59e3-232">If you do not have an invitation yet, you can still try out hello service.</span></span> <span data-ttu-id="f59e3-233">Ano, klepněte na toodo **Ano** po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="f59e3-233">toodo so, tap **yes** when prompted.</span></span>
   
    ![Ukázkový dotaz kanálu](./media/remoteapp-clients/WinPhone7.png)
8. <span data-ttu-id="f59e3-235">Tím získáte přístup k tooa základní sadu aplikací tooget jste začali s vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="f59e3-235">This will give you access tooa basic set of apps tooget you started with RemoteApp.</span></span>
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/WinPhone8.png)

