---
title: "řešení potíží vzdálené aplikace RemoteApp – aaaAzure selhání připojení a spuštění aplikace | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot problémy se spuštěním a připojením tooapplications v Azure Remoteappu."
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
ms.openlocfilehash: e51d480c9d3fa1f2076f95b63c7a8cd2f6956a4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a><span data-ttu-id="bdb99-103">Řešení potíží s Azure RemoteApp - selhání připojení a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="bdb99-103">Troubleshoot Azure RemoteApp - Application launch and connection failures</span></span>
> [!IMPORTANT]
> <span data-ttu-id="bdb99-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="bdb99-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="bdb99-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bdb99-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="bdb99-106">Aplikace hostované v Azure Remoteappu může selhat toolaunch několik různých důvodů.</span><span class="sxs-lookup"><span data-stu-id="bdb99-106">Applications hosted in Azure RemoteApp can fail toolaunch for a few different reasons.</span></span> <span data-ttu-id="bdb99-107">Tento článek popisuje různé příčiny a chybové zprávy uživatelů může dojít při pokusu o toolaunch aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdb99-107">This article describes various reasons and error messages users might receive when trying toolaunch applications.</span></span> <span data-ttu-id="bdb99-108">Také pojednává o selhání připojení.</span><span class="sxs-lookup"><span data-stu-id="bdb99-108">It also talks about connection failures.</span></span> <span data-ttu-id="bdb99-109">(Ale tento článek nepopisuje problémy při přihlašování do hello klientovi Azure Remoteappu.)</span><span class="sxs-lookup"><span data-stu-id="bdb99-109">(But this article does not describe issues when signing into hello Azure RemoteApp client.)</span></span>  

<span data-ttu-id="bdb99-110">Přečtěte si informace o běžných chybové zprávy z důvodu selhání tooapp spuštění a připojení.</span><span class="sxs-lookup"><span data-stu-id="bdb99-110">Read on for information about common error messages due tooapp launch and connection failures.</span></span>

## <a name="were-getting-you-set-up-try-again-in-10-minutes"></a><span data-ttu-id="bdb99-111">Nemůžeme se získávání nastavení... Zkuste to znovu za 10 minut.</span><span class="sxs-lookup"><span data-stu-id="bdb99-111">We're getting you set up... Try again in 10 minutes.</span></span>
<span data-ttu-id="bdb99-112">Tato chyba znamená, že Azure RemoteApp je vertikálním navýšení kapacity potřebná kapacita hello toomeet uživatelů.</span><span class="sxs-lookup"><span data-stu-id="bdb99-112">This error means Azure RemoteApp is scaling up toomeet hello capacity need of your users.</span></span> <span data-ttu-id="bdb99-113">Pozadí hello více instancí virtuálního počítače Azure RemoteApp vytváření požadavků na kapacitu hello toohandle uživatelů.</span><span class="sxs-lookup"><span data-stu-id="bdb99-113">In hello background more Azure RemoteApp VM instances are being created toohandle hello capacity needs of your users.</span></span> <span data-ttu-id="bdb99-114">Obvykle to trvá přibližně pět minut ale může trvat až too10 minut.</span><span class="sxs-lookup"><span data-stu-id="bdb99-114">Typically this takes around five minutes but can take up too10 minutes.</span></span> <span data-ttu-id="bdb99-115">V některých případech to nedojde dostatečně rychle a prostředky jsou potřeba okamžitě.</span><span class="sxs-lookup"><span data-stu-id="bdb99-115">Sometimes, this doesn't happen fast enough and resources are needed immediately.</span></span> <span data-ttu-id="bdb99-116">Například scénář 9: 00 kde mnoho uživatelů musí toouse aplikaci v Azure RemoteAppn ve hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="bdb99-116">For example a 9 AM scenario where many users need toouse your app in Azure RemoteAppn at hello same time.</span></span> <span data-ttu-id="bdb99-117">V takovém případě tooyou můžeme provést aktivaci **kapacity režimu** na hello back-end.</span><span class="sxs-lookup"><span data-stu-id="bdb99-117">If this happens tooyou we can enable **capacity mode** on hello back end.</span></span> <span data-ttu-id="bdb99-118">toodo na tomto otevřete lístek podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="bdb99-118">toodo this open an Azure Support ticket.</span></span> <span data-ttu-id="bdb99-119">Být určité tooinclude svoje ID předplatného v žádosti o hello.</span><span class="sxs-lookup"><span data-stu-id="bdb99-119">Be certain tooinclude your subscription ID in hello request.</span></span>  

![Nemůžeme se zobrazuje při nastavování](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-tooyour-applications-please-re-launch-your-application"></a><span data-ttu-id="bdb99-121">Nelze automatické opětné připojení tooyour aplikace, znovu spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="bdb99-121">Could not auto-reconnect tooyour applications, please re-launch your application</span></span>
<span data-ttu-id="bdb99-122">Tato chybová zpráva často dochází, pokud byly pomocí Azure Remoteappu a potom se spojí váš počítač toosleep delší než 4 hodiny a pak probudila váš počítač a znovu připojit hello Azure RemoteApp klienta pokus o tooauto a byl překročen časový limit.</span><span class="sxs-lookup"><span data-stu-id="bdb99-122">This error message is often seen if you were using Azure RemoteApp and then put your PC toosleep longer than 4 hours and then woke your PC up and hello Azure RemoteApp client attempt tooauto reconnect and timeout was exceeded.</span></span>  <span data-ttu-id="bdb99-123">Vyzvat uživatele toonavigate back toohello aplikace a pokusit se tooopen z hello klientovi Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="bdb99-123">Instruct users toonavigate back toohello application and attempt tooopen it from hello Azure RemoteApp client.</span></span>

![Nelze automatické opětné připojení tooyour aplikace](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-hello-temp-profile"></a><span data-ttu-id="bdb99-125">Problémy s dočasnými profily hello</span><span class="sxs-lookup"><span data-stu-id="bdb99-125">Problems with hello temp profile</span></span>
<span data-ttu-id="bdb99-126">K této chybě dojde, když profilu uživatele (Disk profilu uživatele) se nezdařilo toomount a hello uživatel obdrží dočasných profilů.</span><span class="sxs-lookup"><span data-stu-id="bdb99-126">This error occurs when your user profile (User Profile Disk) failed toomount and hello user received a temporary profile.</span></span>  <span data-ttu-id="bdb99-127">Správci by měl přejděte toohello kolekce v hello portál Azure a potom přejděte toohello **relací** kartě a pokusit se příliš**Odhlásit** hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="bdb99-127">Administrators should navigate toohello collection in hello Azure portal and then go toohello **Sessions** tab and attempt too**Log Off** hello user.</span></span> <span data-ttu-id="bdb99-128">To bude vynutit úplné protokolu z relace uživatele hello - potom mít hello uživatelské pokus o toolaunch aplikaci znovu.</span><span class="sxs-lookup"><span data-stu-id="bdb99-128">This will force a full log off of hello user session - then have hello user attempt toolaunch an app again.</span></span> <span data-ttu-id="bdb99-129">Pokud to nepomůže, obraťte se na podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="bdb99-129">If that fails contact Azure support.</span></span>

## <a name="azure-remoteapp-has-stopped-working"></a><span data-ttu-id="bdb99-130">Azure RemoteApp přestal pracovat.</span><span class="sxs-lookup"><span data-stu-id="bdb99-130">Azure RemoteApp has stopped working</span></span>
<span data-ttu-id="bdb99-131">Tato chybová zpráva znamená klientovi Azure Remoteappu hello má problém a je třeba toobe restartovat.</span><span class="sxs-lookup"><span data-stu-id="bdb99-131">This error message means hello Azure RemoteApp client is having an issue and needs toobe restarted.</span></span> <span data-ttu-id="bdb99-132">Vyzvat uživatele tooclose: vyberte **ukončení programu** a poté znovu spusťte hello klientovi Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="bdb99-132">Instruct users tooclose: select **Close program** and then launch hello Azure RemoteApp client again.</span></span>  <span data-ttu-id="bdb99-133">Pokud hello problém potrvá, otevřete a lístku podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="bdb99-133">If hello issue continues open and Azure Support ticket.</span></span>

![Azure RemoteApp přestal pracovat.](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-hello-connection-or-contact-your-system-administrator"></a><span data-ttu-id="bdb99-135">Připojení ke vzdálené ploše se přístupu k tomuto prostředku došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="bdb99-135">An error occurred while Remote Desktop Connection was accessing this resource.</span></span> <span data-ttu-id="bdb99-136">Opakujte pokus o připojení hello nebo se obraťte na správce systému</span><span class="sxs-lookup"><span data-stu-id="bdb99-136">Retry hello connection or contact your system administrator</span></span>
<span data-ttu-id="bdb99-137">Toto je obecná chyba zpráva – požádejte podporu Azure, takže jsme prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="bdb99-137">This is a generic error message - contact Azure support so we can investigate.</span></span> 

![Obecná zpráva Azure RemoteApp](./media/remoteapp-apptrouble/ra-apptrouble4.png) 

