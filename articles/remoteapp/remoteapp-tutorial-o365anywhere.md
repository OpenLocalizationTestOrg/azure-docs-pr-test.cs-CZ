---
title: "Získání stejných možností Office 365 na jakémkoliv zařízení s Azure RemoteAppem | Dokumentace Microsoftu"
description: "Naučte se sdílet libovolnou aplikaci Office 365 s uživateli pomocí Azure RemoteAppu."
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 0c971ce9-7d45-4cfb-9737-15b6706047e8
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: guscatal;elizapo
ms.openlocfilehash: 584c781c97097cda3c1455ade05cba8659f11073
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a><span data-ttu-id="23963-103">Získejte stejné funkce Office 365 na jakémkoliv zařízení s Azure RemoteAppem</span><span class="sxs-lookup"><span data-stu-id="23963-103">Get the same Office 365 experience on any device with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="23963-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="23963-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="23963-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="23963-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="23963-106">Tento článek se zabývá nasazením služeb Office 365 na jakémkoliv zařízení ve vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="23963-106">This article will cover how to deploy Office 365 on any device in your company.</span></span> <span data-ttu-id="23963-107">Svým uživatelům můžete nabídnout stejné funkce a uživatelské rozhraní v systémech Android, Apple i Windows.</span><span class="sxs-lookup"><span data-stu-id="23963-107">Your users can get the same capabilities and UI experience on Android, Apple and Windows.</span></span>

<span data-ttu-id="23963-108">Dosáhnete toho pomocí Azure RemoteAppu hostováním Office 365 na škálovatelných virtuálních počítačích v Azure, ke kterým se uživatelé mohou připojit.</span><span class="sxs-lookup"><span data-stu-id="23963-108">We will accomplish this using Azure RemoteApp by hosting Office 365 on scale-able virtual machines in Azure that users can connect to.</span></span> <span data-ttu-id="23963-109">Tato sada virtuálních počítačů se nazývá „kolekce v cloudu“.</span><span class="sxs-lookup"><span data-stu-id="23963-109">This set of virtual machines we call a "cloud collection".</span></span>

## <a name="create-a-cloud-collection"></a><span data-ttu-id="23963-110">Vytvoření cloudové kolekce</span><span class="sxs-lookup"><span data-stu-id="23963-110">Create a cloud collection</span></span>
<span data-ttu-id="23963-111">Po vytvoření účtu Azure nejprve kliknutím na odkaz na levém okraji stránky přejděte na stránku **remoteapp**.</span><span class="sxs-lookup"><span data-stu-id="23963-111">First after you have created an Azure account, navigate to **RemoteApp** by clicking on the link on the left side.</span></span>
<span data-ttu-id="23963-112">![Azure RemoteApp na webu Azure Portal](./media/remoteapp-tutorial-o365anywhere/1-menu.png)</span><span class="sxs-lookup"><span data-stu-id="23963-112">![Showing Azure RemoteApp on the Azure Portal](./media/remoteapp-tutorial-o365anywhere/1-menu.png)</span></span>

<span data-ttu-id="23963-113">Pokračujte kliknutím na **NOVÝ** na dolním okraji stránky a rychle vytvořte kolekci.</span><span class="sxs-lookup"><span data-stu-id="23963-113">Then continue by clicking **new** on the bottom and "quick creating" a collection.</span></span> <span data-ttu-id="23963-114">Zadejte název, oblast, předplatné, plán a image Office Proffesional 2013, který nabízíme.</span><span class="sxs-lookup"><span data-stu-id="23963-114">Provide a name, the region, the subscription, the plan and the image "Office Proffesional 2013" that we provide.</span></span>
<span data-ttu-id="23963-115">![Dialogové okno Vytvořit](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)</span><span class="sxs-lookup"><span data-stu-id="23963-115">![Create Dialog](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)</span></span>

<span data-ttu-id="23963-116">Po vyplnění formuláře by se měl spustit proces vytvoření kolekce.</span><span class="sxs-lookup"><span data-stu-id="23963-116">Once you finish the form the collection creation process should start.</span></span> <span data-ttu-id="23963-117">To může trvat až hodinu.</span><span class="sxs-lookup"><span data-stu-id="23963-117">This may take up to an hour or so.</span></span>

![Čekání](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

<span data-ttu-id="23963-119">Po dokončení procesu bude stránka vypadat nějak takhle.</span><span class="sxs-lookup"><span data-stu-id="23963-119">Once the process is done, it will look something like this.</span></span> <span data-ttu-id="23963-120">Když kliknete na **PUBLIKOVÁNÍ**, uvidíte, že většina aplikací Office již pro vás byla publikována.</span><span class="sxs-lookup"><span data-stu-id="23963-120">If we click **Publishing** we can see that most Office applications have been published for us already.</span></span>
<span data-ttu-id="23963-121">![Vytvořená kolekce](./media/remoteapp-tutorial-o365anywhere/4-done.png)</span><span class="sxs-lookup"><span data-stu-id="23963-121">![Collection created](./media/remoteapp-tutorial-o365anywhere/4-done.png)</span></span>

![Publikované aplikace](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

<span data-ttu-id="23963-123">V tomto okamžiku můžete také kliknutím na **PŘÍSTUP UŽIVATELŮ** přidat další uživatele, kteří budou mít k této kolekci přístup.</span><span class="sxs-lookup"><span data-stu-id="23963-123">At this point you can also add more users that have access to this collection by clicking **User Access**.</span></span>
<span data-ttu-id="23963-124">![Konfigurace přístupu uživatelů](./media/remoteapp-tutorial-o365anywhere/6-user.png)</span><span class="sxs-lookup"><span data-stu-id="23963-124">![Configure user access](./media/remoteapp-tutorial-o365anywhere/6-user.png)</span></span>

<span data-ttu-id="23963-125">Nyní už si můžete vyzkoušet připojení k Office 365!</span><span class="sxs-lookup"><span data-stu-id="23963-125">Now let's try out connecting to Office 365!</span></span>

## <a name="connect-to-office-365"></a><span data-ttu-id="23963-126">Připojení k Office 365</span><span class="sxs-lookup"><span data-stu-id="23963-126">Connect to Office 365</span></span>
<span data-ttu-id="23963-127">Otevřete stránku [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), přejděte dolů a kliknutím na **Stáhnout klienty** nainstalujte do svého zařízení klienta Azure RemoteAppu.</span><span class="sxs-lookup"><span data-stu-id="23963-127">We'll head over to [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), scroll down  and click **Download clients** to install the Azure RemoteApp client on the device you're on.</span></span> <span data-ttu-id="23963-128">Snímky obrazovky níže jsou pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="23963-128">The screenshots below are for Windows.</span></span>

<span data-ttu-id="23963-129">Po spuštění aplikace budete vyzváni k přihlášení pomocí účtu Microsoft (dříve „Live ID“) – pro tentokrát použijte stejný jako účet, který jste použili pro Azure.</span><span class="sxs-lookup"><span data-stu-id="23963-129">Once the application starts you'll be asked to sign in with your Microsoft account (formerly called a "Live ID"), use the same one as your Azure account for now.</span></span> <span data-ttu-id="23963-130">Pokud jste přihlášení, mělo by se vám zobrazit oznámení o nových pozvánkách. Klikněte na něj a zobrazí se seznam jako na obrázku dole.</span><span class="sxs-lookup"><span data-stu-id="23963-130">When you're signed in you should see a notification about new invitations, click there and you should see a list like one below.</span></span> <span data-ttu-id="23963-131">Přijměte pozvánku, která odpovídá e-mailu vlastníka účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="23963-131">Accept the invitation that matches your Azure account owner email.</span></span>

![Nová pozvánka](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

<span data-ttu-id="23963-133">Takhle to vypadá, když máte nové pozvánky.</span><span class="sxs-lookup"><span data-stu-id="23963-133">What it looks like when there are new invitations.</span></span>

![Potvrzení aplikace](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

<span data-ttu-id="23963-135">Když pozvánku přijmete, měli byste vidět všechny aplikace Office v klientovi Azure RemoteAppu.</span><span class="sxs-lookup"><span data-stu-id="23963-135">Once you accept the invitation you should see all the Office apps in the Azure RemoteApp client.</span></span>

![Seznam aplikací](./media/remoteapp-tutorial-o365anywhere/9-work.png)

<span data-ttu-id="23963-137">Když kliknete na kteroukoliv z těchto položek, aplikace by se měla spustit ve virtuálním počítači Azure a je to.</span><span class="sxs-lookup"><span data-stu-id="23963-137">When you click on any of these the application should start on the Azure virtual machine and you should be all set!</span></span> <span data-ttu-id="23963-138">Užijte si ji!</span><span class="sxs-lookup"><span data-stu-id="23963-138">Enjoy!</span></span>

![spouštění](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![powerpoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)

