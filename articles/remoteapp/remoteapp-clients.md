---
title: "Přístup k aplikacím z libovolného zařízení | Microsoft Docs"
description: "Zjistěte, klientů, kteří jsou podporovány pro Azure RemoteApp a jak získat přístup k vaší aplikace."
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
ms.openlocfilehash: 10a5be6251765b59fac92a33120cedcf8091a677
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="accessing-your-apps-in-azure-remoteapp"></a><span data-ttu-id="487ce-103">Přístup k aplikacím ve službě Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="487ce-103">Accessing your apps in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="487ce-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="487ce-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="487ce-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="487ce-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="487ce-106">Jedním z beauties Azure RemoteApp je, že vám přístup k aplikacím, z libovolného zařízení.</span><span class="sxs-lookup"><span data-stu-id="487ce-106">One of the beauties of Azure RemoteApp is that you can access apps from any of your devices.</span></span> <span data-ttu-id="487ce-107">I lépe můžete začít pracovat na jednom zařízení a potom bezproblémově přechod do druhé zařízení a vyzvednutí vpravo, kde jste přestali.</span><span class="sxs-lookup"><span data-stu-id="487ce-107">Even better, you can start working on one device and then seamlessly transition to a second device and pick up right where you left off.</span></span> <span data-ttu-id="487ce-108">Abyste mohli začít budete muset stáhnout příslušnou klienta pro vaše zařízení a přihlaste se ke službě.</span><span class="sxs-lookup"><span data-stu-id="487ce-108">To get started you need to download the appropriate client for your device and sign in to the service.</span></span>

<span data-ttu-id="487ce-109">V tomto tématu: přečtěte aktuálně podporovaných klientů a jak je stáhnout před I ukazují, jak se přihlásit k Remoteappu z každé z klientů.</span><span class="sxs-lookup"><span data-stu-id="487ce-109">In this topic, we'll review the clients currently supported and how to download them before I show you how to sign in to RemoteApp from each of the clients.</span></span>

## <a name="supported-clients"></a><span data-ttu-id="487ce-110">Podporovaní klienti</span><span class="sxs-lookup"><span data-stu-id="487ce-110">Supported clients</span></span>
<span data-ttu-id="487ce-111">Mají přístup ke vzdálené aplikace RemoteApp pomocí následujícího postupu, pokud zařízení s jedním z těchto operačních systémů:</span><span class="sxs-lookup"><span data-stu-id="487ce-111">You can access RemoteApp using the steps below if your device is running one of these operating systems:</span></span>

* <span data-ttu-id="487ce-112">Windows 10</span><span class="sxs-lookup"><span data-stu-id="487ce-112">Windows 10</span></span> 
* <span data-ttu-id="487ce-113">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="487ce-113">Windows 8.1</span></span>
* <span data-ttu-id="487ce-114">Windows 8</span><span class="sxs-lookup"><span data-stu-id="487ce-114">Windows 8</span></span>
* <span data-ttu-id="487ce-115">Windows 7 Service Pack 1</span><span class="sxs-lookup"><span data-stu-id="487ce-115">Windows 7 Service Pack 1</span></span>
* <span data-ttu-id="487ce-116">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="487ce-116">Windows Phone 8.1</span></span>
* <span data-ttu-id="487ce-117">iOS</span><span class="sxs-lookup"><span data-stu-id="487ce-117">iOS</span></span>
* <span data-ttu-id="487ce-118">Mac OS X</span><span class="sxs-lookup"><span data-stu-id="487ce-118">Mac OS X</span></span>
* <span data-ttu-id="487ce-119">Android</span><span class="sxs-lookup"><span data-stu-id="487ce-119">Android</span></span>

 <span data-ttu-id="487ce-120">Co tenké klienty?</span><span class="sxs-lookup"><span data-stu-id="487ce-120">What about thin clients?</span></span> <span data-ttu-id="487ce-121">Jsou podporovány následující tenké klienty systému Windows Embedded:</span><span class="sxs-lookup"><span data-stu-id="487ce-121">The following Windows Embedded thin clients are supported:</span></span>

* <span data-ttu-id="487ce-122">Windows Embedded Standard 7</span><span class="sxs-lookup"><span data-stu-id="487ce-122">Windows Embedded Standard 7</span></span>
* <span data-ttu-id="487ce-123">Windows Embedded 8 Standard</span><span class="sxs-lookup"><span data-stu-id="487ce-123">Windows Embedded 8 Standard</span></span>
* <span data-ttu-id="487ce-124">Windows Embedded 8.1 Industry Pro</span><span class="sxs-lookup"><span data-stu-id="487ce-124">Windows Embedded 8.1 Industry Pro</span></span>
* <span data-ttu-id="487ce-125">Windows 10 IoT Enterprise</span><span class="sxs-lookup"><span data-stu-id="487ce-125">Windows 10 IoT Enterprise</span></span>

## <a name="downloading-the-client"></a><span data-ttu-id="487ce-126">Stahování klienta</span><span class="sxs-lookup"><span data-stu-id="487ce-126">Downloading the client</span></span>
<span data-ttu-id="487ce-127">Bez ohledu na to, jakou platformu používáte klienta potřebujete přístup k vzdálené aplikace RemoteApp najdete na [stažení klienta vzdálené plochy](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="487ce-127">No matter what platform you are using, the client you need to access RemoteApp can be found on the [Remote Desktop client download](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) page.</span></span>

<span data-ttu-id="487ce-128">Kliknutím na jiné odkazy buď přímo spustí stahování klienta nebo budou odeslány že klientovi stažení stránky na webu app store pro platformu.</span><span class="sxs-lookup"><span data-stu-id="487ce-128">Clicking the different links will either directly start downloading the client or will send you to the client download page in the app store for that platform.</span></span> <span data-ttu-id="487ce-129">Nainstalujte klienta podle pokynů na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="487ce-129">Install the client by following the instructions on the screen.</span></span>

<span data-ttu-id="487ce-130">Po instalaci klienta na vašem zařízení a jeho spuštění, přejít na odpovídající části se dozvíte, jak se přihlásit k Remoteappu z tohoto klienta.</span><span class="sxs-lookup"><span data-stu-id="487ce-130">Once you have installed the client on your device and launched it, jump to the corresponding section below to learn how to sign in to RemoteApp from that client.</span></span>

## <a name="android"></a><span data-ttu-id="487ce-131">Android</span><span class="sxs-lookup"><span data-stu-id="487ce-131">Android</span></span>
<span data-ttu-id="487ce-132">Po instalaci aplikace Vzdálená plocha společnosti Microsoft z obchodu Google Play, můžete ji najít v seznamu aplikací v rámci **vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="487ce-132">Once you have installed the Microsoft Remote Desktop app from the Google Play store, you can find it in your app list under **Remote Desktop**.</span></span>

1. <span data-ttu-id="487ce-133">Spuštění aplikace přináší můžete pro prázdný Center připojení, pokud jste již dosud používali aplikaci.</span><span class="sxs-lookup"><span data-stu-id="487ce-133">Launching the app brings you to an empty Connection Center, unless you've already been using the app.</span></span> <span data-ttu-id="487ce-134">Chcete-li začít s Azure Remoteappem, klepněte na tlačítko Přidat **"" +""** a klepněte na **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="487ce-134">To get started with Azure RemoteApp, tap the add button **""+""** and tap **Azure RemoteApp**.</span></span>    
   
     ![Prázdný připojení Center](./media/remoteapp-clients/Android1.png)
2. <span data-ttu-id="487ce-136">Musíte se přihlásit pomocí e-mailovou adresu k přístupu ke službě.</span><span class="sxs-lookup"><span data-stu-id="487ce-136">You need to sign in with your email address to access the service.</span></span> <span data-ttu-id="487ce-137">Klepněte na **Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="487ce-137">Tap **Get started**.</span></span>
   
    ![Přihlaste se řádku](./media/remoteapp-clients/Android2.png)
3. <span data-ttu-id="487ce-139">Na další stránce zadejte vaše **e-mailová adresa** a klepněte na **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="487ce-139">On the next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="487ce-140">To zahájí proces přihlášení pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="487ce-140">This begins the sign-in process using Azure Active Directory.</span></span>
   
    ![První stránka Azure Active Directory](./media/remoteapp-clients/Android3.png)
4. <span data-ttu-id="487ce-142">Postupujte podle pokynů na obrazovce se přihlásit pomocí účtu Microsoft (dříve nazývané "LiveID") a ID organizace.</span><span class="sxs-lookup"><span data-stu-id="487ce-142">Follow the instructions on the screen to sign in with your Microsoft account (previously called "LiveID") or organization ID.</span></span> <span data-ttu-id="487ce-143">Po přihlášení může zobrazí se stránka výpis všech pozvánky, kterou jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="487ce-143">Once signed in, you may be presented with a page listing all the invitations you have received.</span></span> <span data-ttu-id="487ce-144">Pokud jste, vyberte pozvánky, kterým důvěřujete a klepněte na **provádí**.</span><span class="sxs-lookup"><span data-stu-id="487ce-144">If you are, select the invitations you trust and tap **Done**.</span></span>    
   
    ![Stránka pozvánek](./media/remoteapp-clients/Android4.png)
5. <span data-ttu-id="487ce-146">Po přijetí vaší pozvánky, seznam aplikací, které máte přístup k stáhne do zařízení a dostupné v centru připojení.</span><span class="sxs-lookup"><span data-stu-id="487ce-146">After accepting your invitations, the list of apps you have access to will be downloaded to your device and made available in the Connection Center.</span></span> <span data-ttu-id="487ce-147">Klepněte na jednu z aplikací k jej začít používat.</span><span class="sxs-lookup"><span data-stu-id="487ce-147">Tap one of the apps to start using it.</span></span>
   
    ![Centrum připojení s informačního kanálu](./media/remoteapp-clients/Android5.png)
6. <span data-ttu-id="487ce-149">Pokud jste pozvánku ještě stále můžete vyzkoušet na služby.</span><span class="sxs-lookup"><span data-stu-id="487ce-149">If you do not have an invitation yet, you can still try out the service.</span></span> <span data-ttu-id="487ce-150">Chcete-li tak učinit, klepněte na **přejít na bezplatné zkušební verze** po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="487ce-150">To do so, tap **Go to free trial** when prompted.</span></span>
   
    ![Ukázkový dotaz kanálu](./media/remoteapp-clients/Android6.png)
7. <span data-ttu-id="487ce-152">Tím získáte přístup k základní sadu aplikací, které vám pomůžou začít s Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="487ce-152">This will give you access to a basic set of apps to get you started with RemoteApp.</span></span>
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/Android7.png)

## <a name="ios"></a><span data-ttu-id="487ce-154">iOS</span><span class="sxs-lookup"><span data-stu-id="487ce-154">iOS</span></span>
<span data-ttu-id="487ce-155">Po instalaci aplikace Vzdálená plocha společnosti Microsoft z obchodu s aplikacemi, můžete ji najít v seznamu aplikací v rámci **klientovi služby Vzdálená plocha**.</span><span class="sxs-lookup"><span data-stu-id="487ce-155">Once you have installed the Microsoft Remote Desktop app from the App store, you can find it in your app list under **RD Client**.</span></span>

1. <span data-ttu-id="487ce-156">Spuštění aplikace přináší můžete pro prázdný Center připojení, pokud jste již dosud používali aplikaci.</span><span class="sxs-lookup"><span data-stu-id="487ce-156">Launching the app brings you to an empty Connection Center, unless you've already been using the app.</span></span> <span data-ttu-id="487ce-157">Chcete-li začít s Azure Remoteappem, klepněte na tlačítko Přidat **"" +""** a klepněte na **přidat Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="487ce-157">To get started with Azure RemoteApp, tap the add button **""+""** and tap **Add Azure RemoteApp**.</span></span>
   
    ![Prázdný připojení Center](./media/remoteapp-clients/IOS1.png)
2. <span data-ttu-id="487ce-159">Je nutné se přihlásit pomocí e-mailovou adresu k přístupu ke službě, ke spuštění tohoto procesu zadejte vaše **e-mailová adresa** a klepněte na **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="487ce-159">You need to sign in with your email address to access the service, to start that process, type in your **email address** and tap **Continue**.</span></span>
   
    ![Přihlaste se řádku](./media/remoteapp-clients/picture1.png)
3. <span data-ttu-id="487ce-161">Postupujte podle pokynů na obrazovce se přihlásit pomocí účtu Microsoft (LiveID) a ID organizace.</span><span class="sxs-lookup"><span data-stu-id="487ce-161">Follow the instructions on the screen to sign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="487ce-162">Po přihlášení může zobrazí se stránka výpis všech pozvánky, kterou jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="487ce-162">Once signed in, you may be presented with a page listing all the invitations you have received.</span></span> <span data-ttu-id="487ce-163">Pokud jste, vyberte pozvánky, kterým důvěřujete a klepněte na **provádí**.</span><span class="sxs-lookup"><span data-stu-id="487ce-163">If you are, select the invitations you trust and tap **Done**.</span></span>
   
    ![Stránka pozvánek](./media/remoteapp-clients/IOS3.png)
4. <span data-ttu-id="487ce-165">Po přijetí vaší pozvánky, seznam aplikací, které máte přístup k stáhne do zařízení a dostupné v centru připojení.</span><span class="sxs-lookup"><span data-stu-id="487ce-165">After accepting your invitations, the list of apps you have access to will be downloaded to your device and made available in the Connection Center.</span></span> <span data-ttu-id="487ce-166">Klepněte na jednu z aplikací a spustíte jej začít používat.</span><span class="sxs-lookup"><span data-stu-id="487ce-166">Tap one of the apps to launch it and start using it.</span></span>
   
    ![Centrum připojení s informačního kanálu](./media/remoteapp-clients/IOS4.png)
5. <span data-ttu-id="487ce-168">Pokud jste pozvánku ještě stále můžete vyzkoušet na služby.</span><span class="sxs-lookup"><span data-stu-id="487ce-168">If you do not have an invitation yet, you can still try out the service.</span></span> <span data-ttu-id="487ce-169">Chcete-li tak učinit, klepněte na **přejít na bezplatné zkušební verze** po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="487ce-169">To do so, tap **Go to free trial** when prompted.</span></span>
   
    ![Ukázkový dotaz kanálu](./media/remoteapp-clients/IOS5.png)
6. <span data-ttu-id="487ce-171">Tím získáte přístup k základní sadu aplikací, které vám pomůžou začít s Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="487ce-171">This will give you access to a basic set of apps to get you started with RemoteApp.</span></span>
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/IOS6.png)

## <a name="mac-os-x"></a><span data-ttu-id="487ce-173">Mac OS X</span><span class="sxs-lookup"><span data-stu-id="487ce-173">Mac OS X</span></span>
<span data-ttu-id="487ce-174">Po instalaci aplikace Vzdálená plocha společnosti Microsoft z obchodu s aplikacemi, můžete ji najít v seznamu aplikací v rámci **Vzdálená plocha od Microsoftu**.</span><span class="sxs-lookup"><span data-stu-id="487ce-174">Once you have installed the Microsoft Remote Desktop app from the App store, you can find it in your app list under **Microsoft Remote Desktop**.</span></span>

1. <span data-ttu-id="487ce-175">Spuštění aplikace přináší můžete pro prázdný Center připojení, pokud jste již dosud používali aplikaci.</span><span class="sxs-lookup"><span data-stu-id="487ce-175">Launching the app brings you to an empty Connection Center, unless you've already been using the app.</span></span> <span data-ttu-id="487ce-176">Chcete-li začít s Azure RemoteApp, klikněte na tlačítko **Azure RemoteApp** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="487ce-176">To get started with Azure RemoteApp, click the **Azure RemoteApp** button.</span></span>
   
    ![Prázdný připojení Center](./media/remoteapp-clients/Mac1.png)
2. <span data-ttu-id="487ce-178">Je nutné se přihlásit pomocí e-mailovou adresu k přístupu ke službě, ke spuštění procesu, klepněte na **Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="487ce-178">You need to sign in with your email address to access the service, to start that process, tap **Get Started**.</span></span>
   
    ![Přihlaste se řádku](./media/remoteapp-clients/Mac2.png)
3. <span data-ttu-id="487ce-180">Na další stránce zadejte vaše **e-mailová adresa** a klepněte na **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="487ce-180">On the next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="487ce-181">To začne přihlašovací proces pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="487ce-181">This begins the sign in process using Azure Active Directory.</span></span>
   
    ![První stránka Azure Active Directory](./media/remoteapp-clients/picture2.png)
4. <span data-ttu-id="487ce-183">Postupujte podle pokynů na obrazovce se přihlásit pomocí účtu Microsoft (LiveID) a ID organizace.</span><span class="sxs-lookup"><span data-stu-id="487ce-183">Follow the instructions on the screen to sign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="487ce-184">Po přihlášení může zobrazí se stránka výpis všech pozvánky, kterou jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="487ce-184">Once signed in, you may be presented with a page listing all the invitations you have received.</span></span> <span data-ttu-id="487ce-185">Pokud jste, vyberte pozvánky, kterým důvěřujete a zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="487ce-185">If you are, select the invitations you trust and close the dialog.</span></span>
   
    ![Stránka pozvánek](./media/remoteapp-clients/Mac4.png)
5. <span data-ttu-id="487ce-187">Po přijetí vaší pozvánky, seznam aplikací, které máte přístup k stáhne do zařízení a dostupné v centru připojení.</span><span class="sxs-lookup"><span data-stu-id="487ce-187">After accepting your invitations, the list of apps you have access to will be downloaded to your device and made available in the Connection Center.</span></span> <span data-ttu-id="487ce-188">Dvakrát klikněte na jednu z aplikací a spustíte jej začít používat.</span><span class="sxs-lookup"><span data-stu-id="487ce-188">Double-click one of the apps to launch it and start using it.</span></span>
   
    ![Centrum připojení s informačního kanálu](./media/remoteapp-clients/Mac5.png)
6. <span data-ttu-id="487ce-190">Pokud jste pozvánku ještě stále můžete vyzkoušet na služby.</span><span class="sxs-lookup"><span data-stu-id="487ce-190">If you do not have an invitation yet, you can still try out the service.</span></span> <span data-ttu-id="487ce-191">Chcete-li to provést, klikněte na tlačítko **přejít na bezplatné zkušební verze** po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="487ce-191">To do so, click **Go to free trial** when prompted.</span></span>
   
    ![Ukázkový dotaz kanálu](./media/remoteapp-clients/Mac6.png)
7. <span data-ttu-id="487ce-193">Tím získáte přístup k základní sadu aplikací, které vám pomůžou začít s Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="487ce-193">This will give you access to a basic set of apps to get you started with RemoteApp.</span></span>
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/Mac7.png)

## <a name="windows-all-supported-versions-except-windows-phone"></a><span data-ttu-id="487ce-195">Windows (s výjimkou Windows Phone všechny podporované verze)</span><span class="sxs-lookup"><span data-stu-id="487ce-195">Windows (All supported versions except Windows Phone)</span></span>
<span data-ttu-id="487ce-196">Klient spustí automaticky po dokončení instalace, ale pokud budete potřebovat pro přístup k ní později se nachází v seznamu aplikací pod názvem **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="487ce-196">The client launches automatically after it finishes installing, however when you need to access it again later it can be found in your app list under the name **Azure RemoteApp**.</span></span>

1. <span data-ttu-id="487ce-197">Spuštění klienta, první stránka, kterou vidíte hřívací zařízení vás vítá do Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="487ce-197">Ater launching the client, the first page you see welcomes you to Azure RemoteApp.</span></span> <span data-ttu-id="487ce-198">Chcete-li pokračovat, klikněte na **Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="487ce-198">To proceed, click on **Get Started**.</span></span>
   
    ![Úvodní stránka klientovi Azure Remoteappu](./media/remoteapp-clients/Windows1.png)
2. <span data-ttu-id="487ce-200">Na další stránku spustí přihlašovací v procesu pro Azure RemoteApp pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="487ce-200">The next page starts the sign in process for Azure RemoteApp using Azure Active Directory.</span></span> <span data-ttu-id="487ce-201">Tento proces by měla vypadat povědomě, pokud jste použili služby společnosti Microsoft v minulosti.</span><span class="sxs-lookup"><span data-stu-id="487ce-201">This process should look familiar if you have used Microsoft services in the past.</span></span> <span data-ttu-id="487ce-202">Začněte tím, že zadáte vaše **e-mailová adresa** a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="487ce-202">Start by typing your **email address** and click **Continue**.</span></span>
   
    ![První výzva služby Azure Active Directory](./media/remoteapp-clients/Windows2.png)
3. <span data-ttu-id="487ce-204">Postupujte podle pokynů na obrazovce se přihlásit pomocí účtu Microsoft (LiveID) a ID organizace.</span><span class="sxs-lookup"><span data-stu-id="487ce-204">Follow the instructions on the screen to sign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="487ce-205">Po přihlášení může zobrazí se stránka výpis všech pozvánky, kterou jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="487ce-205">Once signed in, you may be presented with a page listing all the invitations you have received.</span></span> <span data-ttu-id="487ce-206">Pokud jste, vyberte pozvánky, kterým důvěřujete a klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="487ce-206">If you are, select the invitations you trust and click **Done**.</span></span>
   
    ![Stránka pozvánek klientovi Azure Remoteappu](./media/remoteapp-clients/Windows3.png)
4. <span data-ttu-id="487ce-208">Po přijetí vaší pozvánky, seznam aplikací, které máte přístup k stáhne do zařízení a dostupné v centru připojení.</span><span class="sxs-lookup"><span data-stu-id="487ce-208">After accepting your invitations, the list of apps you have access to will be downloaded to your device and made available in the Connection Center.</span></span> <span data-ttu-id="487ce-209">Dvakrát klikněte na jednu z aplikací a spustíte jej začít používat.</span><span class="sxs-lookup"><span data-stu-id="487ce-209">Double-click one of the apps to launch it and start using it.</span></span>
   
    ![Připojení středu klientovi Azure Remoteappu](./media/remoteapp-clients/Windows4.png)
5. <span data-ttu-id="487ce-211">Pokud žádná vám poslal ještě pozvánku, nemusíte si dělat starosti My jsme vám zahrnutých!</span><span class="sxs-lookup"><span data-stu-id="487ce-211">If no one has sent you an invitation yet, don't worry we've got you covered!</span></span> <span data-ttu-id="487ce-212">Stále budete mít přístup ke kolekci ukázku, můžete otestovat na službu.</span><span class="sxs-lookup"><span data-stu-id="487ce-212">You'll still have access to a demo collection so you can test out the service.</span></span>
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/Windows5.png)

## <a name="windows-phone-81"></a><span data-ttu-id="487ce-214">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="487ce-214">Windows Phone 8.1</span></span>
<span data-ttu-id="487ce-215">Po instalaci aplikace Vzdálená plocha společnosti Microsoft z obchodu Windows Phone 8.1, můžete ji najít v seznamu aplikací v rámci **vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="487ce-215">Once you have installed the Microsoft Remote Desktop app from the Windows Phone 8.1 store, you can find it in your app list under **Remote Desktop**.</span></span>

1. <span data-ttu-id="487ce-216">Spuštění aplikace přináší můžete přímo pro prázdný Center připojení, pokud jste již dosud používali aplikaci.</span><span class="sxs-lookup"><span data-stu-id="487ce-216">Launching the app brings you directly to an empty Connection Center, unless you've already been using the app.</span></span> <span data-ttu-id="487ce-217">Chcete-li začít s Azure Remoteappem, klepněte na tlačítko Přidat **"" +""** v dolní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="487ce-217">To get started with Azure RemoteApp, tap the add button **""+""** at the bottom of the screen.</span></span>
   
    ![Prázdný připojení Center](./media/remoteapp-clients/WinPhone1.png)
2. <span data-ttu-id="487ce-219">Potom klepněte na **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="487ce-219">Next, tap on **Azure RemoteApp**.</span></span>
   
    ![Přidat stránku položek](./media/remoteapp-clients/WinPhone2.png)
3. <span data-ttu-id="487ce-221">Je nutné se přihlásit pomocí e-mailovou adresu k přístupu ke službě, ke spuštění procesu, klepněte na **připojit**.</span><span class="sxs-lookup"><span data-stu-id="487ce-221">You need to sign in with your email address to access the service, to start that process, tap **connect**.</span></span>
   
    ![Přihlaste se řádku](./media/remoteapp-clients/WinPhone3.png)
4. <span data-ttu-id="487ce-223">Na další stránce zadejte vaše **e-mailová adresa** a klepněte na **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="487ce-223">On the next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="487ce-224">To začne přihlašovací proces pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="487ce-224">This begins the sign in process using Azure Active Directory.</span></span>
   
    ![První stránka Azure Active Directory](./media/remoteapp-clients/WinPhone4.png)
5. <span data-ttu-id="487ce-226">Postupujte podle pokynů na obrazovce se přihlásit pomocí účtu Microsoft (LiveID) a ID organizace.</span><span class="sxs-lookup"><span data-stu-id="487ce-226">Follow the instructions on the screen to sign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="487ce-227">Po přihlášení může zobrazí se stránka výpis všech pozvánky, kterou jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="487ce-227">Once signed in, you may be presented with a page listing all the invitations you have received.</span></span> <span data-ttu-id="487ce-228">Pokud jste, vyberte pozvánky, kterým důvěřujete a klepněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="487ce-228">If you are, select the invitations you trust and tap **save**.</span></span>
   
    ![Stránka pozvánek](./media/remoteapp-clients/WinPhone5.png)
6. <span data-ttu-id="487ce-230">Po přijetí vaší pozvánky, seznam aplikací, které máte přístup k stáhne do zařízení a dostupné v centru připojení.</span><span class="sxs-lookup"><span data-stu-id="487ce-230">After accepting your invitations, the list of apps you have access to will be downloaded to your device and made available in the Connection Center.</span></span> <span data-ttu-id="487ce-231">Klepněte na jednu z aplikací a spustíte jej začít používat.</span><span class="sxs-lookup"><span data-stu-id="487ce-231">Tap one of the apps to launch it and start using it.</span></span>
   
    ![Centrum připojení s informačního kanálu](./media/remoteapp-clients/WinPhone6.png)
7. <span data-ttu-id="487ce-233">Pokud jste pozvánku ještě stále můžete vyzkoušet na služby.</span><span class="sxs-lookup"><span data-stu-id="487ce-233">If you do not have an invitation yet, you can still try out the service.</span></span> <span data-ttu-id="487ce-234">Chcete-li tak učinit, klepněte na **Ano** po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="487ce-234">To do so, tap **yes** when prompted.</span></span>
   
    ![Ukázkový dotaz kanálu](./media/remoteapp-clients/WinPhone7.png)
8. <span data-ttu-id="487ce-236">Tím získáte přístup k základní sadu aplikací, které vám pomůžou začít s Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="487ce-236">This will give you access to a basic set of apps to get you started with RemoteApp.</span></span>
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/WinPhone8.png)

