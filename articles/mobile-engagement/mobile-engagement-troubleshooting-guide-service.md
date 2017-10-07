---
title: "aaaAzure Mobile Engagement Průvodce odstraňováním potíží – služby"
description: "Řešení potíží s příručky pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8b4275da-c0b4-4690-824a-48e9d7a1fc6e
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: cf48db323a873ccef95946f7bb26e8d7473c002f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-service-issues"></a><span data-ttu-id="a8080-103">Průvodci odstraňováním potíží problémů služby</span><span class="sxs-lookup"><span data-stu-id="a8080-103">Troubleshooting guide for Service issues</span></span>
<span data-ttu-id="a8080-104">Hello následují možných problémů se můžete setkat s jak spouští Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="a8080-104">hello following are possible issues you may encounter with how Azure Mobile Engagement runs.</span></span>

## <a name="service-outages"></a><span data-ttu-id="a8080-105">Výpadky</span><span class="sxs-lookup"><span data-stu-id="a8080-105">Service Outages</span></span>
### <a name="issue"></a><span data-ttu-id="a8080-106">Problém</span><span class="sxs-lookup"><span data-stu-id="a8080-106">Issue</span></span>
* <span data-ttu-id="a8080-107">Problémy, které se zobrazují toobe způsobené Azure Mobile Engagement výpadky.</span><span class="sxs-lookup"><span data-stu-id="a8080-107">Issues that appear toobe caused by Azure Mobile Engagement Service Outages.</span></span>

### <a name="causes"></a><span data-ttu-id="a8080-108">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="a8080-108">Causes</span></span>
* <span data-ttu-id="a8080-109">Problémy, které se zobrazují toobe způsobené Azure Mobile Engagement výpadky může být způsobeno několika různých problémy:</span><span class="sxs-lookup"><span data-stu-id="a8080-109">Issues that appear toobe caused by Azure Mobile Engagement Service Outages can be caused by several different issues:</span></span>
  * <span data-ttu-id="a8080-110">Izolované problémy, které původně zobrazit systémové tooall Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="a8080-110">Isolated issues that originally appear systemic tooall of Azure Mobile Engagement</span></span>
  * <span data-ttu-id="a8080-111">Známé problémy, kvůli výpadku serveru (ne vždy zobrazuje stav serveru):</span><span class="sxs-lookup"><span data-stu-id="a8080-111">Known issues caused by server outages (not always shows in server status):</span></span>
  * <span data-ttu-id="a8080-112">Plánování zpoždění, cílení na chyby, oznámení "BADGE" update problémy, statistiky zastavit shromažďování nabízené přestane pracovat, rozhraní API zastavení práce, nové aplikace nebo uživatele nelze vytvořit, chyb DNS a vypršení časového limitu v hello uživatelského rozhraní, rozhraní API nebo aplikace na zařízení.</span><span class="sxs-lookup"><span data-stu-id="a8080-112">Scheduling delays, Targeting errors, Badge update issues, Statistics stop collecting, Push stops working, API's stop working, New apps or users can't be created, DNS errors, and Timeout errors in hello UI, API, or Apps on a device.</span></span>
  * <span data-ttu-id="a8080-113">Cloud závislostí výpadků [stav služby Azure](http://status.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="a8080-113">Cloud Dependency Outages [Azure Service Status](http://status.azure.com/)</span></span>
  * <span data-ttu-id="a8080-114">Nabízená oznámení služby (PNS) závislostí výpadků</span><span class="sxs-lookup"><span data-stu-id="a8080-114">Push Notification Services (PNS) Dependency Outages</span></span>
  * <span data-ttu-id="a8080-115">Výpadky obchodu s aplikacemi</span><span class="sxs-lookup"><span data-stu-id="a8080-115">App Store Outages</span></span>

1) <span data-ttu-id="a8080-116">tootest toosee, pokud je systémová hello problém můžete otestovat hello stejné funkce z jiné</span><span class="sxs-lookup"><span data-stu-id="a8080-116">tootest toosee if hello problem is systemic you can test hello same function from a different</span></span>

* <span data-ttu-id="a8080-117">Integrovaná aplikace Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="a8080-117">Azure Mobile Engagement integrated application</span></span>
* <span data-ttu-id="a8080-118">Testovací zařízení</span><span class="sxs-lookup"><span data-stu-id="a8080-118">Test device</span></span>
* <span data-ttu-id="a8080-119">Zkušební verze operačního systému zařízení</span><span class="sxs-lookup"><span data-stu-id="a8080-119">Test device OS version</span></span>
* <span data-ttu-id="a8080-120">Kampaň</span><span class="sxs-lookup"><span data-stu-id="a8080-120">Campaign</span></span>
* <span data-ttu-id="a8080-121">Uživatelský účet správce</span><span class="sxs-lookup"><span data-stu-id="a8080-121">Administrator user account</span></span>
* <span data-ttu-id="a8080-122">Prohlížeč (IE, Firefox, Chrome, atd.)</span><span class="sxs-lookup"><span data-stu-id="a8080-122">Browser (IE, Firefox, Chrome, etc.)</span></span>
* <span data-ttu-id="a8080-123">Počítač</span><span class="sxs-lookup"><span data-stu-id="a8080-123">Computer</span></span>

2) <span data-ttu-id="a8080-124">tootest Pokud hello problém se týká pouze hello uživatelského rozhraní nebo hello rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="a8080-124">tootest if hello problem only affects hello UI or hello API's:</span></span>

* <span data-ttu-id="a8080-125">Otestujte hello stejné funkce z obou hello Azure Mobile Engagement uživatelského rozhraní a hello Azure Mobile Engagement rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a8080-125">Test hello same function from both hello Azure Mobile Engagement UI and hello Azure Mobile Engagement API's.</span></span>

3) <span data-ttu-id="a8080-126">tootest Pokud hello problém s vaší sítí mobilního telefonu:</span><span class="sxs-lookup"><span data-stu-id="a8080-126">tootest if hello problem is with your Cellular Phone Network:</span></span>

* <span data-ttu-id="a8080-127">Test při připojené toohello Internetu přes Wi-Fi a při připojení prostřednictvím sítě 3G mobilního telefonu.</span><span class="sxs-lookup"><span data-stu-id="a8080-127">Test while connected toohello Internet via WIFI and while connected via your 3G cellular phone network.</span></span>
* <span data-ttu-id="a8080-128">Potvrďte, že brána firewall neblokuje hello Azure Mobile Engagement IP adresy nebo porty.</span><span class="sxs-lookup"><span data-stu-id="a8080-128">Confirm that your firewall is not blocking any of hello Azure Mobile Engagement IP Addresses or Ports.</span></span>

4) <span data-ttu-id="a8080-129">tootest Pokud hello problém s vaším zařízením:</span><span class="sxs-lookup"><span data-stu-id="a8080-129">tootest if hello problem is with your Device:</span></span>

* <span data-ttu-id="a8080-130">Otestujte, pokud vaše zařízení je možné tooconnect tooAzure Mobile Engagement s jinou integrované aplikace Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="a8080-130">Test if your Device is able tooconnect tooAzure Mobile Engagement with another Azure Mobile Engagement integrated app.</span></span>
* <span data-ttu-id="a8080-131">Test, který může generovat události, úlohy a dojde k chybě z váš telefon, který si můžete prohlédnout ve hello Azure Mobile Engagement uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a8080-131">Test that you can generate Events, Jobs, and Crashes from your phone that can be seen in hello Azure Mobile Engagement UI.</span></span> 
* <span data-ttu-id="a8080-132">Otestovat, pokud odesíláte nabízená oznámení z hello Azure Mobile Engagement uživatelského rozhraní tooyour zařízení podle jeho ID zařízení.</span><span class="sxs-lookup"><span data-stu-id="a8080-132">Test if you can send push notifications from hello Azure Mobile Engagement UI tooyour device based on its Device ID.</span></span> 

5) <span data-ttu-id="a8080-133">tootest Pokud hello problém s vaší aplikací:</span><span class="sxs-lookup"><span data-stu-id="a8080-133">tootest if hello problem is with your App:</span></span>

* <span data-ttu-id="a8080-134">Instalace a testování vaší aplikace z emulátoru místo z fyzického zařízení:</span><span class="sxs-lookup"><span data-stu-id="a8080-134">Install and test your application from an emulator instead of from a physical device:</span></span>

6) <span data-ttu-id="a8080-135">tootest Pokud hello problém s operačním systémem upgraduje tooend uživatele, zařízení, které vyžadují upgradu tooresolve SDK:</span><span class="sxs-lookup"><span data-stu-id="a8080-135">tootest if hello problem is with OS upgrades tooend user Devices, which require an SDK upgrade tooresolve:</span></span>

* <span data-ttu-id="a8080-136">Testování aplikace na různých zařízeních s různými verzemi nástroje hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="a8080-136">Test your application on different devices with different versions of hello OS.</span></span>
* <span data-ttu-id="a8080-137">Potvrďte, že používáte nejnovější verzi hello SDK hello.</span><span class="sxs-lookup"><span data-stu-id="a8080-137">Confirm that you are using hello most recent version of hello SDK.</span></span>

## <a name="connectivity-and-incorrect-information-issues"></a><span data-ttu-id="a8080-138">Připojení a nesprávné informace o problémech</span><span class="sxs-lookup"><span data-stu-id="a8080-138">Connectivity and Incorrect Information Issues</span></span>
### <a name="issue"></a><span data-ttu-id="a8080-139">Problém</span><span class="sxs-lookup"><span data-stu-id="a8080-139">Issue</span></span>
* <span data-ttu-id="a8080-140">Problémy protokolování do hello Azure Mobile Engagement uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a8080-140">Problems logging into hello Azure Mobile Engagement UI.</span></span>
* <span data-ttu-id="a8080-141">Chyby připojení s hello rozhraní API služby Azure Mobile Engagement je.</span><span class="sxs-lookup"><span data-stu-id="a8080-141">Connection errors with hello Azure Mobile Engagement API's.</span></span>
* <span data-ttu-id="a8080-142">Potíže s odesíláním značky informace o aplikaci prostřednictvím hello rozhraní API pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="a8080-142">Problems uploading App Info Tags via hello Device API.</span></span>
* <span data-ttu-id="a8080-143">Problémy stahování protokolů nebo exportovaná data z Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="a8080-143">Problems downloading logs or exported data from Azure Mobile Engagement.</span></span>
* <span data-ttu-id="a8080-144">Nesprávné informace zobrazené v hello Azure Mobile Engagement uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a8080-144">Incorrect information shown in hello Azure Mobile Engagement UI.</span></span>
* <span data-ttu-id="a8080-145">Nesprávné informace zobrazené v protokolech Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="a8080-145">Incorrect information shown in Azure Mobile Engagement logs.</span></span>

### <a name="causes"></a><span data-ttu-id="a8080-146">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="a8080-146">Causes</span></span>
* <span data-ttu-id="a8080-147">Potvrďte, že váš uživatelský účet má dostatečná oprávnění tooperform hello úloh.</span><span class="sxs-lookup"><span data-stu-id="a8080-147">Confirm your user account has sufficient permissions tooperform hello task.</span></span>
* <span data-ttu-id="a8080-148">Potvrďte, že tento problém hello není izolované tooone počítači nebo v místní síti.</span><span class="sxs-lookup"><span data-stu-id="a8080-148">Confirm that hello problem is not isolated tooone computer or your local network.</span></span>
* <span data-ttu-id="a8080-149">Potvrďte, že tento hello služby Azure Mobile Engagement žádné hlášené výpadky.</span><span class="sxs-lookup"><span data-stu-id="a8080-149">Confirm that that hello Azure Mobile Engagement service has no reported outages.</span></span>
* <span data-ttu-id="a8080-150">Přesvědčte se, vaše aplikace informace o označení souborů podívejte se na všech těchto pravidel:</span><span class="sxs-lookup"><span data-stu-id="a8080-150">Confirm that your App Info Tag files follow all of these rules:</span></span>
  * <span data-ttu-id="a8080-151">Použití pouze hello UTF8 znakovou sadu (znakovou sadu ANSI hello není podporována.).</span><span class="sxs-lookup"><span data-stu-id="a8080-151">Use only hello UTF8 character set (hello ANSI character set is not supported).</span></span>
  * <span data-ttu-id="a8080-152">Použít čárku "," jako hello oddělovací znak (služby požadavek toorequest toochange hello .csv oddělovací znak můžete otevřít z čárkou tooanother znak "," například středníkem ";").</span><span class="sxs-lookup"><span data-stu-id="a8080-152">Use a comma "," as hello separator character (you can open a service request toorequest toochange hello .csv separator character from a comma "," tooanother character such as a semi-colon ";").</span></span>
  * <span data-ttu-id="a8080-153">Používejte všechny malá písmena pro logické hodnoty "true" a "false".</span><span class="sxs-lookup"><span data-stu-id="a8080-153">Use all lower case for Boolean values "true" and "false".</span></span>
  * <span data-ttu-id="a8080-154">Použijte soubor, který je menší než hello maximální velikost souboru 35 MB.</span><span class="sxs-lookup"><span data-stu-id="a8080-154">Use a file that is smaller than hello maximum file size of 35MB.</span></span>

