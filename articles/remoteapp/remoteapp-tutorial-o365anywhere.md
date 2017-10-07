---
title: "aaaGet hello stejné prostředí pro Office 365 na jakémkoliv zařízení s Azure Remoteappem | Microsoft Docs"
description: "Zjistěte, jak tooshare všechny aplikace Office 365 s uživateli pomocí Azure Remoteappu."
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
ms.openlocfilehash: 140056c22c8c69b9ec605318e35a72b144da07eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-same-office-365-experience-on-any-device-with-azure-remoteapp"></a><span data-ttu-id="e29fe-103">Get hello stejné prostředí Office 365 na jakémkoliv zařízení s Azure Remoteappem</span><span class="sxs-lookup"><span data-stu-id="e29fe-103">Get hello same Office 365 experience on any device with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="e29fe-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="e29fe-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="e29fe-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e29fe-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="e29fe-106">Tento článek se zabývá jak toodeploy Office 365 na jakémkoliv zařízení ve vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="e29fe-106">This article will cover how toodeploy Office 365 on any device in your company.</span></span> <span data-ttu-id="e29fe-107">Uživatelé dostanou hello stejné funkce a zkušenosti uživatelského rozhraní na Android, Apple i Windows.</span><span class="sxs-lookup"><span data-stu-id="e29fe-107">Your users can get hello same capabilities and UI experience on Android, Apple and Windows.</span></span>

<span data-ttu-id="e29fe-108">Dosáhnete toho pomocí Azure RemoteAppu hostováním Office 365 na škálovatelných virtuálních počítačích v Azure, ke kterým se uživatelé mohou připojit.</span><span class="sxs-lookup"><span data-stu-id="e29fe-108">We will accomplish this using Azure RemoteApp by hosting Office 365 on scale-able virtual machines in Azure that users can connect to.</span></span> <span data-ttu-id="e29fe-109">Tato sada virtuálních počítačů se nazývá „kolekce v cloudu“.</span><span class="sxs-lookup"><span data-stu-id="e29fe-109">This set of virtual machines we call a "cloud collection".</span></span>

## <a name="create-a-cloud-collection"></a><span data-ttu-id="e29fe-110">Vytvoření cloudové kolekce</span><span class="sxs-lookup"><span data-stu-id="e29fe-110">Create a cloud collection</span></span>
<span data-ttu-id="e29fe-111">Nejprve po vytvoření účtu Azure, přejděte příliš**vzdálené aplikace RemoteApp** kliknutím na odkaz hello na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="e29fe-111">First after you have created an Azure account, navigate too**RemoteApp** by clicking on hello link on hello left side.</span></span>
<span data-ttu-id="e29fe-112">![Zobrazuje Azure RemoteApp na portálu Azure hello](./media/remoteapp-tutorial-o365anywhere/1-menu.png)</span><span class="sxs-lookup"><span data-stu-id="e29fe-112">![Showing Azure RemoteApp on hello Azure Portal](./media/remoteapp-tutorial-o365anywhere/1-menu.png)</span></span>

<span data-ttu-id="e29fe-113">Pokračujte kliknutím na **nové** na dolní hello a "rychlé vytvoření" kolekce.</span><span class="sxs-lookup"><span data-stu-id="e29fe-113">Then continue by clicking **new** on hello bottom and "quick creating" a collection.</span></span> <span data-ttu-id="e29fe-114">Zadejte název, hello oblast, předplatné hello, hello plán a image hello "Office Proffesional 2013, který nabízíme.</span><span class="sxs-lookup"><span data-stu-id="e29fe-114">Provide a name, hello region, hello subscription, hello plan and hello image "Office Proffesional 2013" that we provide.</span></span>
<span data-ttu-id="e29fe-115">![Dialogové okno Vytvořit](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)</span><span class="sxs-lookup"><span data-stu-id="e29fe-115">![Create Dialog](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)</span></span>

<span data-ttu-id="e29fe-116">Jakmile dokončíte proces vytvoření kolekce hello formuláře hello měli začít.</span><span class="sxs-lookup"><span data-stu-id="e29fe-116">Once you finish hello form hello collection creation process should start.</span></span> <span data-ttu-id="e29fe-117">To může trvat až hodinu tooan.</span><span class="sxs-lookup"><span data-stu-id="e29fe-117">This may take up tooan hour or so.</span></span>

![Čekání](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

<span data-ttu-id="e29fe-119">Po dokončení procesu hello bude vypadat podobně jako tento.</span><span class="sxs-lookup"><span data-stu-id="e29fe-119">Once hello process is done, it will look something like this.</span></span> <span data-ttu-id="e29fe-120">Když kliknete na **PUBLIKOVÁNÍ**, uvidíte, že většina aplikací Office již pro vás byla publikována.</span><span class="sxs-lookup"><span data-stu-id="e29fe-120">If we click **Publishing** we can see that most Office applications have been published for us already.</span></span>
<span data-ttu-id="e29fe-121">![Vytvořená kolekce](./media/remoteapp-tutorial-o365anywhere/4-done.png)</span><span class="sxs-lookup"><span data-stu-id="e29fe-121">![Collection created](./media/remoteapp-tutorial-o365anywhere/4-done.png)</span></span>

![Publikované aplikace](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

<span data-ttu-id="e29fe-123">V tomto okamžiku můžete také přidat další uživatele, kteří mají přístup toothis kolekce kliknutím **přístup uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="e29fe-123">At this point you can also add more users that have access toothis collection by clicking **User Access**.</span></span>
<span data-ttu-id="e29fe-124">![Konfigurace přístupu uživatelů](./media/remoteapp-tutorial-o365anywhere/6-user.png)</span><span class="sxs-lookup"><span data-stu-id="e29fe-124">![Configure user access](./media/remoteapp-tutorial-o365anywhere/6-user.png)</span></span>

<span data-ttu-id="e29fe-125">Nyní už si můžete vyzkoušet připojení tooOffice 365!</span><span class="sxs-lookup"><span data-stu-id="e29fe-125">Now let's try out connecting tooOffice 365!</span></span>

## <a name="connect-toooffice-365"></a><span data-ttu-id="e29fe-126">Připojit tooOffice 365</span><span class="sxs-lookup"><span data-stu-id="e29fe-126">Connect tooOffice 365</span></span>
<span data-ttu-id="e29fe-127">Jsme budete příliš zamiřte[https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), posuňte se dolů a klikněte na **klienty Stáhnout** tooinstall hello klientovi Azure Remoteappu jste na zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="e29fe-127">We'll head over too[https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), scroll down  and click **Download clients** tooinstall hello Azure RemoteApp client on hello device you're on.</span></span> <span data-ttu-id="e29fe-128">Hello snímky obrazovky níže jsou pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="e29fe-128">hello screenshots below are for Windows.</span></span>

<span data-ttu-id="e29fe-129">Jakmile se spustí aplikace hello budete dotázáni, toosign pomocí svého účtu Microsoft (dřív označovaných jako "Live ID"), použijte stejný jako účtu Azure hello teď.</span><span class="sxs-lookup"><span data-stu-id="e29fe-129">Once hello application starts you'll be asked toosign in with your Microsoft account (formerly called a "Live ID"), use hello same one as your Azure account for now.</span></span> <span data-ttu-id="e29fe-130">Pokud jste přihlášení, mělo by se vám zobrazit oznámení o nových pozvánkách. Klikněte na něj a zobrazí se seznam jako na obrázku dole.</span><span class="sxs-lookup"><span data-stu-id="e29fe-130">When you're signed in you should see a notification about new invitations, click there and you should see a list like one below.</span></span> <span data-ttu-id="e29fe-131">Přijměte pozvánku hello, která odpovídá e-mailu vlastníka účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="e29fe-131">Accept hello invitation that matches your Azure account owner email.</span></span>

![Nová pozvánka](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

<span data-ttu-id="e29fe-133">Takhle to vypadá, když máte nové pozvánky.</span><span class="sxs-lookup"><span data-stu-id="e29fe-133">What it looks like when there are new invitations.</span></span>

![Potvrzení aplikace](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

<span data-ttu-id="e29fe-135">Po přijetí pozvánky hello byste měli vidět všechny aplikace Office hello v klientovi Azure Remoteappu hello.</span><span class="sxs-lookup"><span data-stu-id="e29fe-135">Once you accept hello invitation you should see all hello Office apps in hello Azure RemoteApp client.</span></span>

![Seznam aplikací](./media/remoteapp-tutorial-o365anywhere/9-work.png)

<span data-ttu-id="e29fe-137">Po kliknutí na některé z těchto aplikací hello by měl spusťte na hello virtuální počítač Azure a musí být nastavené!</span><span class="sxs-lookup"><span data-stu-id="e29fe-137">When you click on any of these hello application should start on hello Azure virtual machine and you should be all set!</span></span> <span data-ttu-id="e29fe-138">Užijte si ji!</span><span class="sxs-lookup"><span data-stu-id="e29fe-138">Enjoy!</span></span>

![spouštění](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![powerpoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)

