---
title: "Řešení potíží s průvodce – služba Azure Mobile Engagement."
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
ms.openlocfilehash: f13fd0540b783120014b3a8d4e41f78808c7fade
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-for-service-issues"></a><span data-ttu-id="e67ab-103">Průvodci odstraňováním potíží problémů služby</span><span class="sxs-lookup"><span data-stu-id="e67ab-103">Troubleshooting guide for Service issues</span></span>
<span data-ttu-id="e67ab-104">Následují možných problémů se můžete setkat s jak spouští Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="e67ab-104">The following are possible issues you may encounter with how Azure Mobile Engagement runs.</span></span>

## <a name="service-outages"></a><span data-ttu-id="e67ab-105">Výpadky</span><span class="sxs-lookup"><span data-stu-id="e67ab-105">Service Outages</span></span>
### <a name="issue"></a><span data-ttu-id="e67ab-106">Problém</span><span class="sxs-lookup"><span data-stu-id="e67ab-106">Issue</span></span>
* <span data-ttu-id="e67ab-107">Problémy, které by mohly být způsobeno Azure Mobile Engagement výpadky.</span><span class="sxs-lookup"><span data-stu-id="e67ab-107">Issues that appear to be caused by Azure Mobile Engagement Service Outages.</span></span>

### <a name="causes"></a><span data-ttu-id="e67ab-108">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="e67ab-108">Causes</span></span>
* <span data-ttu-id="e67ab-109">Problémy, které by mohly být způsobeno Azure Mobile Engagement výpadky může být způsobeno několika různých problémy:</span><span class="sxs-lookup"><span data-stu-id="e67ab-109">Issues that appear to be caused by Azure Mobile Engagement Service Outages can be caused by several different issues:</span></span>
  * <span data-ttu-id="e67ab-110">Izolované problémy, které původně zobrazit systémové ke všem Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="e67ab-110">Isolated issues that originally appear systemic to all of Azure Mobile Engagement</span></span>
  * <span data-ttu-id="e67ab-111">Známé problémy, kvůli výpadku serveru (ne vždy zobrazuje stav serveru):</span><span class="sxs-lookup"><span data-stu-id="e67ab-111">Known issues caused by server outages (not always shows in server status):</span></span>
  * <span data-ttu-id="e67ab-112">Plánování zpoždění, cílení na chyby, oznámení "BADGE" update problémy, statistiky zastavit shromažďování nabízené přestane pracovat, rozhraní API zastavení práce, nové aplikace nebo uživatele nelze vytvořit, chyb DNS a vypršení časového limitu v uživatelském rozhraní, rozhraní API nebo aplikace na zařízení.</span><span class="sxs-lookup"><span data-stu-id="e67ab-112">Scheduling delays, Targeting errors, Badge update issues, Statistics stop collecting, Push stops working, API's stop working, New apps or users can't be created, DNS errors, and Timeout errors in the UI, API, or Apps on a device.</span></span>
  * <span data-ttu-id="e67ab-113">Cloud závislostí výpadků [stav služby Azure](http://status.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="e67ab-113">Cloud Dependency Outages [Azure Service Status](http://status.azure.com/)</span></span>
  * <span data-ttu-id="e67ab-114">Nabízená oznámení služby (PNS) závislostí výpadků</span><span class="sxs-lookup"><span data-stu-id="e67ab-114">Push Notification Services (PNS) Dependency Outages</span></span>
  * <span data-ttu-id="e67ab-115">Výpadky obchodu s aplikacemi</span><span class="sxs-lookup"><span data-stu-id="e67ab-115">App Store Outages</span></span>

1) <span data-ttu-id="e67ab-116">Chcete-li otestujte, pokud je problém systémové můžete otestovat té samé funkce z jiné</span><span class="sxs-lookup"><span data-stu-id="e67ab-116">To test to see if the problem is systemic you can test the same function from a different</span></span>

* <span data-ttu-id="e67ab-117">Integrovaná aplikace Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="e67ab-117">Azure Mobile Engagement integrated application</span></span>
* <span data-ttu-id="e67ab-118">Testovací zařízení</span><span class="sxs-lookup"><span data-stu-id="e67ab-118">Test device</span></span>
* <span data-ttu-id="e67ab-119">Zkušební verze operačního systému zařízení</span><span class="sxs-lookup"><span data-stu-id="e67ab-119">Test device OS version</span></span>
* <span data-ttu-id="e67ab-120">Kampaň</span><span class="sxs-lookup"><span data-stu-id="e67ab-120">Campaign</span></span>
* <span data-ttu-id="e67ab-121">Uživatelský účet správce</span><span class="sxs-lookup"><span data-stu-id="e67ab-121">Administrator user account</span></span>
* <span data-ttu-id="e67ab-122">Prohlížeč (IE, Firefox, Chrome, atd.)</span><span class="sxs-lookup"><span data-stu-id="e67ab-122">Browser (IE, Firefox, Chrome, etc.)</span></span>
* <span data-ttu-id="e67ab-123">Počítač</span><span class="sxs-lookup"><span data-stu-id="e67ab-123">Computer</span></span>

2) <span data-ttu-id="e67ab-124">K testování, pokud se problém se týká pouze uživatelského rozhraní nebo rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="e67ab-124">To test if the problem only affects the UI or the API's:</span></span>

* <span data-ttu-id="e67ab-125">Stejnou funkci z rozhraní Azure Mobile Engagement i v Azure Mobile Engagement rozhraní API otestujte.</span><span class="sxs-lookup"><span data-stu-id="e67ab-125">Test the same function from both the Azure Mobile Engagement UI and the Azure Mobile Engagement API's.</span></span>

3) <span data-ttu-id="e67ab-126">K testování, pokud se problém s vaší sítí mobilního telefonu:</span><span class="sxs-lookup"><span data-stu-id="e67ab-126">To test if the problem is with your Cellular Phone Network:</span></span>

* <span data-ttu-id="e67ab-127">Test při připojení k Internetu prostřednictvím sítě Wi-Fi a při připojení prostřednictvím sítě 3G mobilního telefonu.</span><span class="sxs-lookup"><span data-stu-id="e67ab-127">Test while connected to the Internet via WIFI and while connected via your 3G cellular phone network.</span></span>
* <span data-ttu-id="e67ab-128">Potvrďte, že brána firewall neblokuje Azure Mobile Engagement IP adresy nebo porty.</span><span class="sxs-lookup"><span data-stu-id="e67ab-128">Confirm that your firewall is not blocking any of the Azure Mobile Engagement IP Addresses or Ports.</span></span>

4) <span data-ttu-id="e67ab-129">K testování, pokud se problém s vaším zařízením:</span><span class="sxs-lookup"><span data-stu-id="e67ab-129">To test if the problem is with your Device:</span></span>

* <span data-ttu-id="e67ab-130">Otestujte, pokud vaše zařízení je možné se připojit k Azure Mobile Engagement s jinou integrované aplikace Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="e67ab-130">Test if your Device is able to connect to Azure Mobile Engagement with another Azure Mobile Engagement integrated app.</span></span>
* <span data-ttu-id="e67ab-131">Test, který může generovat události, úlohy a dojde k chybě z váš telefon, který je možné zobrazit v uživatelském rozhraní Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="e67ab-131">Test that you can generate Events, Jobs, and Crashes from your phone that can be seen in the Azure Mobile Engagement UI.</span></span> 
* <span data-ttu-id="e67ab-132">Test při odeslání nabízených oznámení z uživatelského rozhraní Azure Mobile Engagement zařízení podle jeho ID zařízení.</span><span class="sxs-lookup"><span data-stu-id="e67ab-132">Test if you can send push notifications from the Azure Mobile Engagement UI to your device based on its Device ID.</span></span> 

5) <span data-ttu-id="e67ab-133">K testování, pokud se problém s vaší aplikací:</span><span class="sxs-lookup"><span data-stu-id="e67ab-133">To test if the problem is with your App:</span></span>

* <span data-ttu-id="e67ab-134">Instalace a testování vaší aplikace z emulátoru místo z fyzického zařízení:</span><span class="sxs-lookup"><span data-stu-id="e67ab-134">Install and test your application from an emulator instead of from a physical device:</span></span>

6) <span data-ttu-id="e67ab-135">K testování, pokud se problém s upgrady operačního systému pro koncového uživatele zařízení, které vyžadují upgrade SDK řešení:</span><span class="sxs-lookup"><span data-stu-id="e67ab-135">To test if the problem is with OS upgrades to end user Devices, which require an SDK upgrade to resolve:</span></span>

* <span data-ttu-id="e67ab-136">Testování aplikace na různých zařízeních s různými verzemi operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e67ab-136">Test your application on different devices with different versions of the OS.</span></span>
* <span data-ttu-id="e67ab-137">Potvrďte, že používáte nejnovější verzi sady SDK.</span><span class="sxs-lookup"><span data-stu-id="e67ab-137">Confirm that you are using the most recent version of the SDK.</span></span>

## <a name="connectivity-and-incorrect-information-issues"></a><span data-ttu-id="e67ab-138">Připojení a nesprávné informace o problémech</span><span class="sxs-lookup"><span data-stu-id="e67ab-138">Connectivity and Incorrect Information Issues</span></span>
### <a name="issue"></a><span data-ttu-id="e67ab-139">Problém</span><span class="sxs-lookup"><span data-stu-id="e67ab-139">Issue</span></span>
* <span data-ttu-id="e67ab-140">Problémy s přihlášením se do rozhraní Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="e67ab-140">Problems logging into the Azure Mobile Engagement UI.</span></span>
* <span data-ttu-id="e67ab-141">Chyby připojení se v Azure Mobile Engagement rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e67ab-141">Connection errors with the Azure Mobile Engagement API's.</span></span>
* <span data-ttu-id="e67ab-142">Potíže s odesíláním značky informace o aplikaci prostřednictvím rozhraní API pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="e67ab-142">Problems uploading App Info Tags via the Device API.</span></span>
* <span data-ttu-id="e67ab-143">Problémy stahování protokolů nebo exportovaná data z Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="e67ab-143">Problems downloading logs or exported data from Azure Mobile Engagement.</span></span>
* <span data-ttu-id="e67ab-144">Nesprávné informace zobrazené v uživatelském rozhraní Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="e67ab-144">Incorrect information shown in the Azure Mobile Engagement UI.</span></span>
* <span data-ttu-id="e67ab-145">Nesprávné informace zobrazené v protokolech Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="e67ab-145">Incorrect information shown in Azure Mobile Engagement logs.</span></span>

### <a name="causes"></a><span data-ttu-id="e67ab-146">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="e67ab-146">Causes</span></span>
* <span data-ttu-id="e67ab-147">Potvrďte, že váš uživatelský účet má dostatečná oprávnění k provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="e67ab-147">Confirm your user account has sufficient permissions to perform the task.</span></span>
* <span data-ttu-id="e67ab-148">Potvrďte, že problém není izolovaná do jednoho počítače nebo v místní síti.</span><span class="sxs-lookup"><span data-stu-id="e67ab-148">Confirm that the problem is not isolated to one computer or your local network.</span></span>
* <span data-ttu-id="e67ab-149">Potvrďte, že služba Azure Mobile Engagement nemá hlášené výpadky.</span><span class="sxs-lookup"><span data-stu-id="e67ab-149">Confirm that that the Azure Mobile Engagement service has no reported outages.</span></span>
* <span data-ttu-id="e67ab-150">Přesvědčte se, vaše aplikace informace o označení souborů podívejte se na všech těchto pravidel:</span><span class="sxs-lookup"><span data-stu-id="e67ab-150">Confirm that your App Info Tag files follow all of these rules:</span></span>
  * <span data-ttu-id="e67ab-151">Použití pouze UTF8 znakovou sadu (znakovou sadu ANSI není podporována.).</span><span class="sxs-lookup"><span data-stu-id="e67ab-151">Use only the UTF8 character set (the ANSI character set is not supported).</span></span>
  * <span data-ttu-id="e67ab-152">Použít čárku "," jako oddělovací znak (můžete otevřít službu požadavku k požadavku na změnu .csv oddělovací znak z čárkou "," na jiný znak, jako je například středníkem ";").</span><span class="sxs-lookup"><span data-stu-id="e67ab-152">Use a comma "," as the separator character (you can open a service request to request to change the .csv separator character from a comma "," to another character such as a semi-colon ";").</span></span>
  * <span data-ttu-id="e67ab-153">Používejte všechny malá písmena pro logické hodnoty "true" a "false".</span><span class="sxs-lookup"><span data-stu-id="e67ab-153">Use all lower case for Boolean values "true" and "false".</span></span>
  * <span data-ttu-id="e67ab-154">Použijte soubor, který je menší než maximální velikost souboru 35 MB.</span><span class="sxs-lookup"><span data-stu-id="e67ab-154">Use a file that is smaller than the maximum file size of 35MB.</span></span>

