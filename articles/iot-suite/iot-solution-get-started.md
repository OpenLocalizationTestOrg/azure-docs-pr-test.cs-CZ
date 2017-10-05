---
title: "Příklad MyDriving Azure IoT: rychlý start | Microsoft Docs"
description: "Začínáme s aplikaci, která je komplexní ukázka postup architektury systému IoT pomocí služby Microsoft Azure, včetně služby Stream Analytics, Machine Learning a Event Hubs."
services: 
documentationcenter: .net
suite: 
author: harikmenon
manager: douge
ms.assetid: f40ea71b-5721-4a6b-a886-53c2e9dffe8f
ms.service: multiple
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: dotnet
ms.topic: article
ms.date: 03/25/2016
ms.author: harikm
ms.openlocfilehash: 031b492df1f186087e7b91102cbb44f552999293
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="mydriving-iot-system-quick-start"></a><span data-ttu-id="31fca-103">Systém MyDriving IoT: rychlý start</span><span class="sxs-lookup"><span data-stu-id="31fca-103">MyDriving IoT system: Quick start</span></span>
<span data-ttu-id="31fca-104">MyDriving je systém, který ukazuje návrhu a implementace typické [Internet věcí](iot-suite-overview.md) řešení (IoT), který shromažďuje telemetrická data ze zařízení, zpracuje tato data v cloudu a použije strojového učení k poskytování adaptivní odpověď.</span><span class="sxs-lookup"><span data-stu-id="31fca-104">MyDriving is a system that demonstrates the design and implementation of a typical [Internet of Things](iot-suite-overview.md) (IoT) solution that gathers telemetry from devices, processes that data in the cloud, and applies machine learning to provide an adaptive response.</span></span> <span data-ttu-id="31fca-105">Ukázky zaznamená data o vaší služebních cest car pomocí dat z mobilního telefonu a adaptér, který shromažďuje informace z vašeho automobilu řízení systému.</span><span class="sxs-lookup"><span data-stu-id="31fca-105">The demonstration logs data about your car trips, by using data from both your mobile phone and an adapter that collects information from your car’s control system.</span></span> <span data-ttu-id="31fca-106">Tato data se používá k poskytnutí zpětné vazby na rozdíl od jiných uživatelů vaší řízení stylu.</span><span class="sxs-lookup"><span data-stu-id="31fca-106">It uses this data to provide feedback on your driving style in comparison to other users.</span></span>

<span data-ttu-id="31fca-107">Skutečné účelem MyDriving je vám pomůžou začít s vytvářením řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="31fca-107">The real purpose of MyDriving is to get you started in creating your own IoT solution.</span></span> <span data-ttu-id="31fca-108">Ale před Pojďme vám tedy budete s aplikací MyDriving – jako člen týmu pro naše testovací uživatele.</span><span class="sxs-lookup"><span data-stu-id="31fca-108">But before that, let’s get you going with the MyDriving app itself--as a member of our test user team.</span></span> <span data-ttu-id="31fca-109">To vám dává prostředí aplikace a systém za ho jako příjemce, než se pustíte do architekturu.</span><span class="sxs-lookup"><span data-stu-id="31fca-109">This gives you an experience of the app and the system behind it as a consumer, before you delve into the architecture.</span></span> <span data-ttu-id="31fca-110">Je také vás seznámí s HockeyApp nástrojů způsob správy distribuce alpha a beta aplikací, jak testovat uživatele.</span><span class="sxs-lookup"><span data-stu-id="31fca-110">It also introduces you to HockeyApp, a cool way of managing the alpha and beta distributions of your apps to test users.</span></span>

## <a name="use-the-mobile-experience"></a><span data-ttu-id="31fca-111">Použít mobilní prostředí</span><span class="sxs-lookup"><span data-stu-id="31fca-111">Use the mobile experience</span></span>
<span data-ttu-id="31fca-112">Pokud máte zařízení se systémem Android, iOS nebo Windows 10 můžete používat aplikaci MyDriving.</span><span class="sxs-lookup"><span data-stu-id="31fca-112">You can use the MyDriving app if you have an Android, iOS, or Windows 10 device.</span></span>

### <a name="android-and-windows-10-mobile-installation"></a><span data-ttu-id="31fca-113">Instalace Windows 10 Mobile a Android</span><span class="sxs-lookup"><span data-stu-id="31fca-113">Android and Windows 10 Mobile installation</span></span>
<span data-ttu-id="31fca-114">Na zařízení:</span><span class="sxs-lookup"><span data-stu-id="31fca-114">On your device:</span></span>

1. <span data-ttu-id="31fca-115">Povolit vývoj aplikací:</span><span class="sxs-lookup"><span data-stu-id="31fca-115">Allow development apps:</span></span>
   
   * <span data-ttu-id="31fca-116">Android: V **nastavení** > **zabezpečení**, povolit aplikacím z **neznámé zdroje**.</span><span class="sxs-lookup"><span data-stu-id="31fca-116">Android: In **Settings** > **Security**, allow apps from **Unknown sources**.</span></span>
   * <span data-ttu-id="31fca-117">Windows 10: V **nastavení** > **aktualizace** > **pro vývojáře**, nastavte **režim vývojáře**.</span><span class="sxs-lookup"><span data-stu-id="31fca-117">Windows 10: In **Settings** > **Updates** > **For Developers**, set **Developer mode**.</span></span>
2. <span data-ttu-id="31fca-118">Připojit náš tým testování tak, že se přihlásíte, nebo přihlášení k, [HockeyApp](https://rink.hockeyapp.net).</span><span class="sxs-lookup"><span data-stu-id="31fca-118">Join our beta test team by signing up with, or signing in to, [HockeyApp](https://rink.hockeyapp.net).</span></span> <span data-ttu-id="31fca-119">HockeyApp usnadňuje distribuci raných verzích aplikaci, kterou chcete testovat uživatele.</span><span class="sxs-lookup"><span data-stu-id="31fca-119">HockeyApp makes it easy to distribute early releases of your app to test users.</span></span>
   
   <span data-ttu-id="31fca-120">Pokud používáte Windows 10, použijte prohlížeč Microsoft Edge.</span><span class="sxs-lookup"><span data-stu-id="31fca-120">If you’re using Windows 10, use the Edge browser.</span></span>
   
   <span data-ttu-id="31fca-121">Pokud byste byli Build 2016 účastníka, přihlaste se pomocí stejné Microsoft účtu e-mailu zaregistrovaný pro konference, pomocí jedné z tlačítka společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="31fca-121">If you were a Build 2016 attendee, sign in with the same Microsoft account email that you registered for the conference, by using one of the Microsoft buttons.</span></span> <span data-ttu-id="31fca-122">Jste již přihlášeni s HockeyApp.</span><span class="sxs-lookup"><span data-stu-id="31fca-122">You’re already signed up with HockeyApp.</span></span>
   
   ![HockeyApp přihlašovací obrazovky](./media/iot-solution-get-started/image1.png)
3. <span data-ttu-id="31fca-124">Stáhněte a nainstalujte aplikaci z tohoto umístění:</span><span class="sxs-lookup"><span data-stu-id="31fca-124">Download and install the app from here:</span></span>
   
   * [<span data-ttu-id="31fca-125">Android</span><span class="sxs-lookup"><span data-stu-id="31fca-125">Android</span></span>](http://rink.io/spMyDrivingAndroid)
   * [<span data-ttu-id="31fca-126">Windows 10</span><span class="sxs-lookup"><span data-stu-id="31fca-126">Windows 10</span></span>](http://rink.io/spMyDrivingUWP)
   
   <span data-ttu-id="31fca-127">Existují dvě položky.</span><span class="sxs-lookup"><span data-stu-id="31fca-127">There are two items.</span></span> <span data-ttu-id="31fca-128">Instalaci certifikátu v **důvěryhodné osoby**.</span><span class="sxs-lookup"><span data-stu-id="31fca-128">Install the certificate in **Trusted People**.</span></span> <span data-ttu-id="31fca-129">Nainstalujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="31fca-129">Then install the app.</span></span>

<span data-ttu-id="31fca-130">*Problémy s spouštění aplikace na Windows 10 Mobile?*</span><span class="sxs-lookup"><span data-stu-id="31fca-130">*Any issues starting the app on Windows 10 Mobile?*</span></span> <span data-ttu-id="31fca-131">Aktualizace nebo dvě za, může být váš telefon.</span><span class="sxs-lookup"><span data-stu-id="31fca-131">Your phone might be an update or two behind.</span></span> <span data-ttu-id="31fca-132">Ujistěte se, že máte k dispozici nejnovější aktualizace, nebo nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="31fca-132">Make sure you've got the latest updates, or install:</span></span>

* [<span data-ttu-id="31fca-133">Microsoft.NET.Native.Framework.1.2.appx</span><span class="sxs-lookup"><span data-stu-id="31fca-133">Microsoft.NET.Native.Framework.1.2.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
* [<span data-ttu-id="31fca-134">Microsoft.NET.Native.Runtime.1.1.appx</span><span class="sxs-lookup"><span data-stu-id="31fca-134">Microsoft.NET.Native.Runtime.1.1.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
* [<span data-ttu-id="31fca-135">Microsoft.VCLibs.ARM.14.00.appx</span><span class="sxs-lookup"><span data-stu-id="31fca-135">Microsoft.VCLibs.ARM.14.00.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)

### <a name="ios-installation"></a><span data-ttu-id="31fca-136">instalace iOS</span><span class="sxs-lookup"><span data-stu-id="31fca-136">iOS installation</span></span>
<span data-ttu-id="31fca-137">Pokud jste chodili Build 2016, stáhněte si aplikaci jako člen týmu našich testů na HockeyApp:</span><span class="sxs-lookup"><span data-stu-id="31fca-137">If you attended Build 2016, download the app as a member of our test team on HockeyApp:</span></span>

1. <span data-ttu-id="31fca-138">Na zařízení s iOS, přihlaste se k [HockeyApp](https://rink.hockeyapp.net).</span><span class="sxs-lookup"><span data-stu-id="31fca-138">On your iOS device, sign in to [HockeyApp](https://rink.hockeyapp.net).</span></span>
   <span data-ttu-id="31fca-139">Použijte jeden z Microsoft přihlášení tlačítka a přihlaste se pomocí stejné e-mail účet Microsoft, který zaregistrovali konference.</span><span class="sxs-lookup"><span data-stu-id="31fca-139">Use one of the Microsoft sign-in buttons, and sign in with the same Microsoft account email that you registered with the conference.</span></span> <span data-ttu-id="31fca-140">(Nepoužívejte pole e-mailu a heslo.)</span><span class="sxs-lookup"><span data-stu-id="31fca-140">(Don’t use the email and password fields.)</span></span>
   
   ![HockeyApp přihlašovací obrazovky](./media/iot-solution-get-started/image1.png)
2. <span data-ttu-id="31fca-142">Na řídicím panelu HockeyApp MyDriving vyberte a stáhněte ho.</span><span class="sxs-lookup"><span data-stu-id="31fca-142">In the HockeyApp dashboard, select MyDriving and download it.</span></span>
3. <span data-ttu-id="31fca-143">Autorizovat betaverze z HockeyApp:</span><span class="sxs-lookup"><span data-stu-id="31fca-143">Authorize the beta release from HockeyApp:</span></span>
   
   <span data-ttu-id="31fca-144">a.</span><span class="sxs-lookup"><span data-stu-id="31fca-144">a.</span></span> <span data-ttu-id="31fca-145">Přejděte na **nastavení** > **Obecné** > **profily a správu zařízení.**</span><span class="sxs-lookup"><span data-stu-id="31fca-145">Go to **Settings** > **General** > **Profiles and Device Management.**</span></span>
   
   <span data-ttu-id="31fca-146">b.</span><span class="sxs-lookup"><span data-stu-id="31fca-146">b.</span></span> <span data-ttu-id="31fca-147">Vztah důvěryhodnosti **Bit Stadium GmbH** certifikátu.</span><span class="sxs-lookup"><span data-stu-id="31fca-147">Trust the **Bit Stadium GmbH** certificate.</span></span>

<span data-ttu-id="31fca-148">Pokud jste nechodili na ni Build 2016, můžete sestavit a nasadit aplikaci:</span><span class="sxs-lookup"><span data-stu-id="31fca-148">If you didn’t attend Build 2016, you can build and deploy the app yourself:</span></span>

1. <span data-ttu-id="31fca-149">Stáhněte si kód [z Githubu].</span><span class="sxs-lookup"><span data-stu-id="31fca-149">Download the code [from GitHub].</span></span>
2. <span data-ttu-id="31fca-150">Vytvářet a nasazovat s [pomocí Xamarin].</span><span class="sxs-lookup"><span data-stu-id="31fca-150">Build and deploy by [using Xamarin].</span></span>

<span data-ttu-id="31fca-151">Najít další podrobnosti najdete [MyDriving referenční příručka](http://aka.ms/mydrivingdocs).</span><span class="sxs-lookup"><span data-stu-id="31fca-151">Find more details in the [MyDriving Reference Guide](http://aka.ms/mydrivingdocs).</span></span>

## <a name="get-an-obd-adapter-optional"></a><span data-ttu-id="31fca-152">Získat adaptér Diagnostického (volitelné)</span><span class="sxs-lookup"><span data-stu-id="31fca-152">Get an OBD adapter (optional)</span></span>
<span data-ttu-id="31fca-153">Toto je část, která je tato možnost skutečné systému Internet věcí!</span><span class="sxs-lookup"><span data-stu-id="31fca-153">This is the part that makes this a real Internet of Things system!</span></span> <span data-ttu-id="31fca-154">Můžete používat aplikaci bez, ale je další fun s Opravdová věc a nejsou nákladné.</span><span class="sxs-lookup"><span data-stu-id="31fca-154">You can use the app without one, but it’s more fun with the real thing, and they aren’t expensive.</span></span>

<span data-ttu-id="31fca-155">Integrovaného diagnostiky (Diagnostického) je funkce vaše Auto, která garáž používá nastavení automobil výkonu a Diagnostika liché zvuky a žárovky upozornění.</span><span class="sxs-lookup"><span data-stu-id="31fca-155">On-board diagnostics (OBD) is the feature of your car that the garage uses to tune up your car and diagnose odd noises and warning lamps.</span></span> <span data-ttu-id="31fca-156">Pokud vaše Auto je skvělým antiquity, zjistíte soket někde v kabině, obvykle za klopa v řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="31fca-156">Unless your car is of great antiquity, you’ll find a socket somewhere in the cabin, typically behind a flap under the dashboard.</span></span> <span data-ttu-id="31fca-157">Pravé konektor můžete získat metriky výkonu stroje a provést určité úpravy.</span><span class="sxs-lookup"><span data-stu-id="31fca-157">With the right connector, you can get metrics of the engine’s performance and make certain adjustments.</span></span> <span data-ttu-id="31fca-158">Konektor Diagnostického lze zakoupit levné z obvyklé místech.</span><span class="sxs-lookup"><span data-stu-id="31fca-158">An OBD connector can be purchased cheaply from the usual places.</span></span> <span data-ttu-id="31fca-159">Připojí pomocí Bluetooth nebo Wi-Fi do aplikace na váš telefon.</span><span class="sxs-lookup"><span data-stu-id="31fca-159">It connects by using Bluetooth or Wi-Fi to an app on your phone.</span></span>

<span data-ttu-id="31fca-160">V tomto případě vytvoříme automobil připojit ke cloudu.</span><span class="sxs-lookup"><span data-stu-id="31fca-160">In this case, we’re going to connect your car to the cloud.</span></span> <span data-ttu-id="31fca-161">Přímé připojení z Diagnostického je na váš telefon, ale naše aplikace funguje jako předávací hostitel.</span><span class="sxs-lookup"><span data-stu-id="31fca-161">The direct connection from the OBD is to your phone, but our app works as a relay.</span></span> <span data-ttu-id="31fca-162">Vaše Auto telemetrie je odeslána přímo k centru MyDriving IoT, kde je zpracována protokolu vaší silniční služebních cest a vyhodnocení vašeho řízení stylu.</span><span class="sxs-lookup"><span data-stu-id="31fca-162">Your car's telemetry is sent straight to the MyDriving IoT hub, where it's processed to log your road trips and assess your driving style.</span></span>

<span data-ttu-id="31fca-163">Připojení zařízení s Diagnostického:</span><span class="sxs-lookup"><span data-stu-id="31fca-163">To connect an OBD device:</span></span>

1. <span data-ttu-id="31fca-164">Zkontrolujte, zda vaše Auto Diagnostického soketu.</span><span class="sxs-lookup"><span data-stu-id="31fca-164">Check that your car has an OBD socket.</span></span>
2. <span data-ttu-id="31fca-165">Získejte Diagnostického adaptér:</span><span class="sxs-lookup"><span data-stu-id="31fca-165">Obtain an OBD adapter:</span></span>
   
   * <span data-ttu-id="31fca-166">Pokud používáte Android nebo Windows phone, budete potřebovat adaptér II Diagnostického zařízeními podporujícími technologii Bluetooth.</span><span class="sxs-lookup"><span data-stu-id="31fca-166">If you're using an Android or Windows phone, you need a Bluetooth-enabled OBD II adapter.</span></span> <span data-ttu-id="31fca-167">Použili jsme [BAFX produkty 34t5 Bluetooth OBDII kontrolovat nástroj].</span><span class="sxs-lookup"><span data-stu-id="31fca-167">We used [BAFX Products 34t5 Bluetooth OBDII Scan Tool].</span></span>
   * <span data-ttu-id="31fca-168">Pokud používáte phone iOS, budete potřebovat Wi-Fi povolit Diagnostického adaptér.</span><span class="sxs-lookup"><span data-stu-id="31fca-168">If you're using an iOS phone, you need a Wi-Fi-enabled OBD adapter.</span></span> <span data-ttu-id="31fca-169">Použili jsme [ScanTool OBDLink MX Wi-Fi: Diagnostického adaptér nebo diagnostiky skener].</span><span class="sxs-lookup"><span data-stu-id="31fca-169">We used [ScanTool OBDLink MX Wi-Fi: OBD Adapter/Diagnostic Scanner].</span></span>
3. <span data-ttu-id="31fca-170">Postupujte podle pokynů dodaných s adaptérem Diagnostického pro připojení k telefonu.</span><span class="sxs-lookup"><span data-stu-id="31fca-170">Follow the instructions that come with your OBD adapter to connect it to your phone.</span></span> <span data-ttu-id="31fca-171">Následující mějte na paměti:</span><span class="sxs-lookup"><span data-stu-id="31fca-171">Keep the following in mind:</span></span>
   
   * <span data-ttu-id="31fca-172">Adaptér Bluetooth musí být spárována s telefonu, na **nastavení** stránky.</span><span class="sxs-lookup"><span data-stu-id="31fca-172">A Bluetooth adapter must be paired with the phone, on the **Settings** page.</span></span>
   * <span data-ttu-id="31fca-173">Wi-Fi adaptér musí mít adresu v rozsahu 192.168.xxx.xxx.</span><span class="sxs-lookup"><span data-stu-id="31fca-173">A Wi-Fi adapter must have an address in the range 192.168.xxx.xxx.</span></span>
4. <span data-ttu-id="31fca-174">Pokud máte několik aut, můžete využít samostatný adaptér získat pro každý (maximálně tři).</span><span class="sxs-lookup"><span data-stu-id="31fca-174">If you have several cars, you can get a separate adapter for each (maximum of three).</span></span>

<span data-ttu-id="31fca-175">Pokud nemáte adaptér Diagnostického, aplikace bude i dál posílat data o umístění a rychlost z telefonu GPS příjemce k back-endu a požádá, pokud chcete simulovat Diagnostického.</span><span class="sxs-lookup"><span data-stu-id="31fca-175">If you don’t have an OBD adapter, the app will still send location and speed data from the phone's GPS receiver to the back end and will ask if you want to simulate an OBD.</span></span>

<span data-ttu-id="31fca-176">Můžete zjistit informace o tom, jak aplikace používá data z adaptéru Diagnostického a o možnostech pro vytvoření vlastní Diagnostického zařízení v části 2.1, "Zařízení IoT," v [MyDriving referenční příručka](http://aka.ms/mydrivingdocs).</span><span class="sxs-lookup"><span data-stu-id="31fca-176">You can find out more about how the app uses data from the OBD adapter and about options for creating your own OBD device in section 2.1, "IoT Devices," in the [MyDriving Reference Guide](http://aka.ms/mydrivingdocs).</span></span>

## <a name="use-the-app"></a><span data-ttu-id="31fca-177">Použití aplikace</span><span class="sxs-lookup"><span data-stu-id="31fca-177">Use the app</span></span>
<span data-ttu-id="31fca-178">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="31fca-178">Start the app.</span></span> <span data-ttu-id="31fca-179">Je počáteční rychlý úvodní kurz pro vás provede procesem jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="31fca-179">There’s an initial Quickstart to walk you through how it works.</span></span>

### <a name="track-your-trips"></a><span data-ttu-id="31fca-180">Sledovat vaše služebních cest</span><span class="sxs-lookup"><span data-stu-id="31fca-180">Track your trips</span></span>
<span data-ttu-id="31fca-181">Klepněte na tlačítko záznam (big červeném kroužku v dolní části obrazovky) spustit služební cestě a klepněte na znovu na konec.</span><span class="sxs-lookup"><span data-stu-id="31fca-181">Tap the record button (big red circle at the bottom of the screen) to start a trip, and tap again to end.</span></span>

![Obrázek na tlačítko záznam pro cestu sledování](./media/iot-solution-get-started/image2.png)

<span data-ttu-id="31fca-183">Při každém spuštění na cestu, pokud neexistuje žádné zařízení Diagnostického, budete vyzváni Pokud budete chtít použít simulátoru.</span><span class="sxs-lookup"><span data-stu-id="31fca-183">Each time you start a trip, if there’s no OBD device, you’ll be asked if you want to use the simulator.</span></span>

<span data-ttu-id="31fca-184">Na konci cesty klepněte na tlačítko Zastavit a získat shrnutí.</span><span class="sxs-lookup"><span data-stu-id="31fca-184">At the end of a trip, tap the stop button, and you get a summary.</span></span>

![Příklad cesty souhrn](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a><span data-ttu-id="31fca-186">Zkontrolujte vaše služebních cest</span><span class="sxs-lookup"><span data-stu-id="31fca-186">Review your trips</span></span>
![Příklad posledních cesty](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a><span data-ttu-id="31fca-188">Zkontrolujte váš profil</span><span class="sxs-lookup"><span data-stu-id="31fca-188">Review your profile</span></span>
![Příklad profilu řídí stylu](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a><span data-ttu-id="31fca-190">Pošlete nám svůj názor testu</span><span class="sxs-lookup"><span data-stu-id="31fca-190">Send us your test feedback</span></span>
<span data-ttu-id="31fca-191">Vzhledem k tomu, že jsme vytvořili vlastní systémy IoT MyDriving pomohou základní informace, jistě chceme slyšet od vás o tom, jak dobře funguje.</span><span class="sxs-lookup"><span data-stu-id="31fca-191">Because we created MyDriving to help jumpstart your own IoT systems, we certainly want to hear from you about how well it works.</span></span> <span data-ttu-id="31fca-192">Dejte nám vědět, pokud:</span><span class="sxs-lookup"><span data-stu-id="31fca-192">Let us know if:</span></span>

* <span data-ttu-id="31fca-193">Narazíte na problémy nebo výzvami.</span><span class="sxs-lookup"><span data-stu-id="31fca-193">You run into difficulties or challenges.</span></span>
* <span data-ttu-id="31fca-194">Není k rozšíření bodu, který by bylo vhodnější pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="31fca-194">There is an extension point that would make it more suitable to your scenario.</span></span>
* <span data-ttu-id="31fca-195">Můžete najít efektivnější způsob, jak provést určité požadavky.</span><span class="sxs-lookup"><span data-stu-id="31fca-195">You find a more efficient way to accomplish certain needs.</span></span>
* <span data-ttu-id="31fca-196">Máte další návrhy na zlepšení MyDriving nebo této dokumentace.</span><span class="sxs-lookup"><span data-stu-id="31fca-196">You have any other suggestions for improving MyDriving or this documentation.</span></span>

<span data-ttu-id="31fca-197">V rámci MyDriving aplikace, můžete použít předdefinované zpětnou vazbu mechanismus HockeyApp: na iOS a Android, právě poskytnout telefonu zatřesením, nebo použijte **zpětné vazby** příkazu nabídky.</span><span class="sxs-lookup"><span data-stu-id="31fca-197">Within the MyDriving app itself, you can use the built-in HockeyApp feedback mechanism: on iOS and Android, just give your phone a shake, or use the **Feedback** menu command.</span></span> <span data-ttu-id="31fca-198">Snímek, to bude připojit automaticky, takže jsme budete vědět, co jste o rozhovoru.</span><span class="sxs-lookup"><span data-stu-id="31fca-198">This will automatically attach a screenshot, so that we’ll know what you’re talking about.</span></span> <span data-ttu-id="31fca-199">A pokud jsou všechny velice nepříjemná havárií, HockeyApp shromažďuje protokoly havárií na Řekněte nám o nich.</span><span class="sxs-lookup"><span data-stu-id="31fca-199">And if there are any unfortunate crashes, HockeyApp collects the crash logs to tell us about them.</span></span> <span data-ttu-id="31fca-200">Můžete také poskytnout zpětnou vazbu prostřednictvím [HockeyApp portál].</span><span class="sxs-lookup"><span data-stu-id="31fca-200">You can also give feedback through the [HockeyApp portal].</span></span>

<span data-ttu-id="31fca-201">Můžete také soubor [problém na Githubu], nebo komentář níže (en-us edition).</span><span class="sxs-lookup"><span data-stu-id="31fca-201">You can also file an [issue on GitHub], or leave a comment below (en-us edition).</span></span>

<span data-ttu-id="31fca-202">Těšíme sluchu od vás!</span><span class="sxs-lookup"><span data-stu-id="31fca-202">We look forward to hearing from you!</span></span>

## <a name="next-steps"></a><span data-ttu-id="31fca-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31fca-203">Next steps</span></span>
* <span data-ttu-id="31fca-204">Prozkoumejte [MyDriving referenční příručka](http://aka.ms/mydrivingdocs) pochopit, jak jsme jste navržen a sestaven celý systém MyDriving.</span><span class="sxs-lookup"><span data-stu-id="31fca-204">Explore the [MyDriving Reference Guide](http://aka.ms/mydrivingdocs) to understand how we’ve designed and built the entire MyDriving system.</span></span>
* <span data-ttu-id="31fca-205">[Vytvořit a nasadit vlastní systému](iot-solution-build-system.md) pomocí našich skriptů Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="31fca-205">[Create and deploy a system of your own](iot-solution-build-system.md) by using our Azure Resource Manager scripts.</span></span> <span data-ttu-id="31fca-206">[MyDriving referenční příručka](http://aka.ms/mydrivingdocs) také vás provede oblasti, kde budete provádět úpravy nejvíc.</span><span class="sxs-lookup"><span data-stu-id="31fca-206">The [MyDriving Reference Guide](http://aka.ms/mydrivingdocs) also guides you through areas where you’ll make the most customizations.</span></span>

<span data-ttu-id="31fca-207">[z webu GitHub]: https://github.com/Azure-Samples/MyDriving</span><span class="sxs-lookup"><span data-stu-id="31fca-207">[from GitHub]: https://github.com/Azure-Samples/MyDriving</span></span>
<span data-ttu-id="31fca-208">[pomocí Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/</span><span class="sxs-lookup"><span data-stu-id="31fca-208">[using Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/</span></span>
<span data-ttu-id="31fca-209">[Nástroj pro BAFX produkty 34t5 Bluetooth OBDII kontrolu]: http://www.amazon.com/gp/product/B005NLQAHS</span><span class="sxs-lookup"><span data-stu-id="31fca-209">[BAFX Products 34t5 Bluetooth OBDII Scan Tool]: http://www.amazon.com/gp/product/B005NLQAHS</span></span>
<span data-ttu-id="31fca-210">[Skener adaptér nebo diagnostiky Diagnostického ScanTool OBDLink MX Wi-Fi:]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP</span><span class="sxs-lookup"><span data-stu-id="31fca-210">[ScanTool OBDLink MX Wi-Fi: OBD Adapter/Diagnostic Scanner]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP</span></span>
<span data-ttu-id="31fca-211">[HockeyApp portálu]: https://rink.hockeyapp.org</span><span class="sxs-lookup"><span data-stu-id="31fca-211">[HockeyApp portal]: https://rink.hockeyapp.org</span></span>
<span data-ttu-id="31fca-212">[vydávat na Githubu]: https://github.com/Azure-Samples/MyDriving/issues</span><span class="sxs-lookup"><span data-stu-id="31fca-212">[issue on GitHub]: https://github.com/Azure-Samples/MyDriving/issues</span></span>
