---
title: "Řešení potíží Azure RemoteApp – selhání připojení a spuštění aplikace | Microsoft Docs"
description: "Informace o řešení problémů s spuštění a připojení k aplikacím v Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: e5cf7171-d1c2-4053-a38b-5af7821305e1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: fc9d538991adce7fc13e9654b9a7c6d113d03fde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a><span data-ttu-id="d17b1-103">Řešení potíží s Azure RemoteApp - selhání připojení a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="d17b1-103">Troubleshoot Azure RemoteApp - Application launch and connection failures</span></span>
> [!IMPORTANT]
> <span data-ttu-id="d17b1-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="d17b1-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="d17b1-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="d17b1-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="d17b1-106">Aplikace hostované v Azure Remoteappu může selhat spustit několik různých důvodů.</span><span class="sxs-lookup"><span data-stu-id="d17b1-106">Applications hosted in Azure RemoteApp can fail to launch for a few different reasons.</span></span> <span data-ttu-id="d17b1-107">Tento článek popisuje různé příčiny a chybové zprávy uživatelů může dojít při pokusu o spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="d17b1-107">This article describes various reasons and error messages users might receive when trying to launch applications.</span></span> <span data-ttu-id="d17b1-108">Také pojednává o selhání připojení.</span><span class="sxs-lookup"><span data-stu-id="d17b1-108">It also talks about connection failures.</span></span> <span data-ttu-id="d17b1-109">(Ale tento článek nepopisuje potíže při přihlašování do klientovi Azure Remoteappu.)</span><span class="sxs-lookup"><span data-stu-id="d17b1-109">(But this article does not describe issues when signing into the Azure RemoteApp client.)</span></span>  

<span data-ttu-id="d17b1-110">Přečtěte si informace o běžných chybové zprávy z důvodu chyby připojení a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="d17b1-110">Read on for information about common error messages due to app launch and connection failures.</span></span>

## <a name="were-getting-you-set-up-try-again-in-10-minutes"></a><span data-ttu-id="d17b1-111">Nemůžeme se získávání nastavení... Zkuste to znovu za 10 minut.</span><span class="sxs-lookup"><span data-stu-id="d17b1-111">We're getting you set up... Try again in 10 minutes.</span></span>
<span data-ttu-id="d17b1-112">Tato chyba znamená, že Azure RemoteApp je škálování splňují požadavky na kapacitu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d17b1-112">This error means Azure RemoteApp is scaling up to meet the capacity need of your users.</span></span> <span data-ttu-id="d17b1-113">Na pozadí se vytváří více instancí virtuálního počítače Azure RemoteApp pro zvládání potřeb kapacity uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d17b1-113">In the background more Azure RemoteApp VM instances are being created to handle the capacity needs of your users.</span></span> <span data-ttu-id="d17b1-114">Obvykle to trvá přibližně pět minut ale může trvat až 10 minut.</span><span class="sxs-lookup"><span data-stu-id="d17b1-114">Typically this takes around five minutes but can take up to 10 minutes.</span></span> <span data-ttu-id="d17b1-115">V některých případech to nedojde dostatečně rychle a prostředky jsou potřeba okamžitě.</span><span class="sxs-lookup"><span data-stu-id="d17b1-115">Sometimes, this doesn't happen fast enough and resources are needed immediately.</span></span> <span data-ttu-id="d17b1-116">Například 9: 00 situace, kdy je potřeba řada uživatelů používají vaši aplikaci v Azure RemoteAppn ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="d17b1-116">For example a 9 AM scenario where many users need to use your app in Azure RemoteAppn at the same time.</span></span> <span data-ttu-id="d17b1-117">V takovém případě vám můžeme provést aktivaci **kapacity režimu** na back-end.</span><span class="sxs-lookup"><span data-stu-id="d17b1-117">If this happens to you we can enable **capacity mode** on the back end.</span></span> <span data-ttu-id="d17b1-118">Uděláte to otevřete lístek podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="d17b1-118">To do this open an Azure Support ticket.</span></span> <span data-ttu-id="d17b1-119">Ujistěte se, zahrnout svoje ID předplatného v žádosti.</span><span class="sxs-lookup"><span data-stu-id="d17b1-119">Be certain to include your subscription ID in the request.</span></span>  

![Nemůžeme se zobrazuje při nastavování](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-to-your-applications-please-re-launch-your-application"></a><span data-ttu-id="d17b1-121">Nelze znovu připojit automaticky do aplikací, znovu spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="d17b1-121">Could not auto-reconnect to your applications, please re-launch your application</span></span>
<span data-ttu-id="d17b1-122">Tato chybová zpráva je často vidět Pokud jste používali Azure RemoteApp a pak přesuňte počítače do režimu spánku déle než 4 hodiny a pak probudila vašeho počítače, a klientovi Azure Remoteappu pokusí automaticky znovu připojit a byl překročen časový limit.</span><span class="sxs-lookup"><span data-stu-id="d17b1-122">This error message is often seen if you were using Azure RemoteApp and then put your PC to sleep longer than 4 hours and then woke your PC up and the Azure RemoteApp client attempt to auto reconnect and timeout was exceeded.</span></span>  <span data-ttu-id="d17b1-123">Vyzvat uživatele, aby přejděte zpět do aplikace a pokus o otevření v klientovi Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="d17b1-123">Instruct users to navigate back to the application and attempt to open it from the Azure RemoteApp client.</span></span>

![Nelze automaticky-znovu připojit k vaší aplikace](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-the-temp-profile"></a><span data-ttu-id="d17b1-125">Problémy s dočasnými profily</span><span class="sxs-lookup"><span data-stu-id="d17b1-125">Problems with the temp profile</span></span>
<span data-ttu-id="d17b1-126">K této chybě dojde, když profilu uživatele (Disk profilu uživatele) se nepodařilo připojit a uživatel obdrží dočasných profilů.</span><span class="sxs-lookup"><span data-stu-id="d17b1-126">This error occurs when your user profile (User Profile Disk) failed to mount and the user received a temporary profile.</span></span>  <span data-ttu-id="d17b1-127">Správci by měl přejděte do kolekce na portálu Azure a potom přejděte na **relací** kartě a pokusit se **Odhlásit** uživatele.</span><span class="sxs-lookup"><span data-stu-id="d17b1-127">Administrators should navigate to the collection in the Azure portal and then go to the **Sessions** tab and attempt to **Log Off** the user.</span></span> <span data-ttu-id="d17b1-128">To bude vynutit úplné protokolu z uživatelské relace - pak uživatel pokusí znovu spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d17b1-128">This will force a full log off of the user session - then have the user attempt to launch an app again.</span></span> <span data-ttu-id="d17b1-129">Pokud to nepomůže, obraťte se na podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="d17b1-129">If that fails contact Azure support.</span></span>

## <a name="azure-remoteapp-has-stopped-working"></a><span data-ttu-id="d17b1-130">Azure RemoteApp přestal pracovat.</span><span class="sxs-lookup"><span data-stu-id="d17b1-130">Azure RemoteApp has stopped working</span></span>
<span data-ttu-id="d17b1-131">Tato chybová zpráva znamená problém s klientovi Azure Remoteappu a je třeba restartovat.</span><span class="sxs-lookup"><span data-stu-id="d17b1-131">This error message means the Azure RemoteApp client is having an issue and needs to be restarted.</span></span> <span data-ttu-id="d17b1-132">Vyzvat uživatele, aby zavřete: vyberte **ukončení programu** a poté znovu spusťte klientovi Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="d17b1-132">Instruct users to close: select **Close program** and then launch the Azure RemoteApp client again.</span></span>  <span data-ttu-id="d17b1-133">Pokud problém nezmizí, otevřete a lístku podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="d17b1-133">If the issue continues open and Azure Support ticket.</span></span>

![Azure RemoteApp přestal pracovat.](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-the-connection-or-contact-your-system-administrator"></a><span data-ttu-id="d17b1-135">Připojení ke vzdálené ploše se přístupu k tomuto prostředku došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="d17b1-135">An error occurred while Remote Desktop Connection was accessing this resource.</span></span> <span data-ttu-id="d17b1-136">Pokus o připojení nebo se obraťte na správce systému</span><span class="sxs-lookup"><span data-stu-id="d17b1-136">Retry the connection or contact your system administrator</span></span>
<span data-ttu-id="d17b1-137">Toto je obecná chyba zpráva – požádejte podporu Azure, takže jsme prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="d17b1-137">This is a generic error message - contact Azure support so we can investigate.</span></span> 

![Obecná zpráva Azure RemoteApp](./media/remoteapp-apptrouble/ra-apptrouble4.png) 

