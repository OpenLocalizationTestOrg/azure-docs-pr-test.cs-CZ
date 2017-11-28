---
title: "Příklad MyDriving Azure IoT: rychlý start | Microsoft Docs"
description: "Začínáme s aplikaci, která je komplexní ukázka jak tooarchitect s systémem IoT pomocí služby Microsoft Azure, včetně služby Stream Analytics, Machine Learning a Event Hubs."
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
ms.openlocfilehash: 411b9a992deb22b915f8291d8559e2917d976b2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="mydriving-iot-system-quick-start"></a><span data-ttu-id="655cb-103">Systém MyDriving IoT: rychlý start</span><span class="sxs-lookup"><span data-stu-id="655cb-103">MyDriving IoT system: Quick start</span></span>
<span data-ttu-id="655cb-104">MyDriving je systém, který ukazuje hello návrhu a implementace typické [Internet věcí](iot-suite-overview.md) řešení (IoT), který shromažďuje telemetrická data ze zařízení, zpracuje tato data v cloudu hello a platí machine learning tooprovide adaptivní odpovědi.</span><span class="sxs-lookup"><span data-stu-id="655cb-104">MyDriving is a system that demonstrates hello design and implementation of a typical [Internet of Things](iot-suite-overview.md) (IoT) solution that gathers telemetry from devices, processes that data in hello cloud, and applies machine learning tooprovide an adaptive response.</span></span> <span data-ttu-id="655cb-105">Ukázka Hello zaznamená data o vaší služebních cest car pomocí dat z mobilního telefonu a adaptér, který shromažďuje informace z vašeho automobilu řízení systému.</span><span class="sxs-lookup"><span data-stu-id="655cb-105">hello demonstration logs data about your car trips, by using data from both your mobile phone and an adapter that collects information from your car’s control system.</span></span> <span data-ttu-id="655cb-106">Tato zpětná vazba tooprovide dat používá řízení jakým způsobem v porovnání tooother uživatelé.</span><span class="sxs-lookup"><span data-stu-id="655cb-106">It uses this data tooprovide feedback on your driving style in comparison tooother users.</span></span>

<span data-ttu-id="655cb-107">Hello skutečnému účelu MyDriving je tooget, kterou jste zahájili při vytváření vlastní řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="655cb-107">hello real purpose of MyDriving is tooget you started in creating your own IoT solution.</span></span> <span data-ttu-id="655cb-108">Ale před Pojďme vám tedy budete s hello MyDriving aplikace – jako člen týmu pro naše testovací uživatele.</span><span class="sxs-lookup"><span data-stu-id="655cb-108">But before that, let’s get you going with hello MyDriving app itself--as a member of our test user team.</span></span> <span data-ttu-id="655cb-109">To vám dává prostředí aplikace hello a hello systém za ho jako příjemce, než se pustíte do architektury hello.</span><span class="sxs-lookup"><span data-stu-id="655cb-109">This gives you an experience of hello app and hello system behind it as a consumer, before you delve into hello architecture.</span></span> <span data-ttu-id="655cb-110">Je také vás seznámí tooHockeyApp nástrojů způsob správy distribuce alpha a beta hello tootest uživatelů vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="655cb-110">It also introduces you tooHockeyApp, a cool way of managing hello alpha and beta distributions of your apps tootest users.</span></span>

## <a name="use-hello-mobile-experience"></a><span data-ttu-id="655cb-111">Použít mobilní prostředí hello</span><span class="sxs-lookup"><span data-stu-id="655cb-111">Use hello mobile experience</span></span>
<span data-ttu-id="655cb-112">Pokud máte zařízení se systémem Android, iOS nebo Windows 10 můžete používat aplikaci MyDriving hello.</span><span class="sxs-lookup"><span data-stu-id="655cb-112">You can use hello MyDriving app if you have an Android, iOS, or Windows 10 device.</span></span>

### <a name="android-and-windows-10-mobile-installation"></a><span data-ttu-id="655cb-113">Instalace Windows 10 Mobile a Android</span><span class="sxs-lookup"><span data-stu-id="655cb-113">Android and Windows 10 Mobile installation</span></span>
<span data-ttu-id="655cb-114">Na zařízení:</span><span class="sxs-lookup"><span data-stu-id="655cb-114">On your device:</span></span>

1. <span data-ttu-id="655cb-115">Povolit vývoj aplikací:</span><span class="sxs-lookup"><span data-stu-id="655cb-115">Allow development apps:</span></span>
   
   * <span data-ttu-id="655cb-116">Android: V **nastavení** > **zabezpečení**, povolit aplikacím z **neznámé zdroje**.</span><span class="sxs-lookup"><span data-stu-id="655cb-116">Android: In **Settings** > **Security**, allow apps from **Unknown sources**.</span></span>
   * <span data-ttu-id="655cb-117">Windows 10: V **nastavení** > **aktualizace** > **pro vývojáře**, nastavte **režim vývojáře**.</span><span class="sxs-lookup"><span data-stu-id="655cb-117">Windows 10: In **Settings** > **Updates** > **For Developers**, set **Developer mode**.</span></span>
2. <span data-ttu-id="655cb-118">Připojit náš tým testování tak, že se přihlásíte, nebo přihlášení k, [HockeyApp](https://rink.hockeyapp.net).</span><span class="sxs-lookup"><span data-stu-id="655cb-118">Join our beta test team by signing up with, or signing in to, [HockeyApp](https://rink.hockeyapp.net).</span></span> <span data-ttu-id="655cb-119">HockeyApp umožňuje snadno toodistribute raných verzích tootest uživatelů vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="655cb-119">HockeyApp makes it easy toodistribute early releases of your app tootest users.</span></span>
   
   <span data-ttu-id="655cb-120">Pokud používáte Windows 10, použijte prohlížeč Edge hello.</span><span class="sxs-lookup"><span data-stu-id="655cb-120">If you’re using Windows 10, use hello Edge browser.</span></span>
   
   <span data-ttu-id="655cb-121">Pokud byste byli Build 2016 účastníka, přihlaste se pomocí hello stejné e-mail účet Microsoft, který jste zaregistrováni hello konference, pomocí jedné z hello tlačítka společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="655cb-121">If you were a Build 2016 attendee, sign in with hello same Microsoft account email that you registered for hello conference, by using one of hello Microsoft buttons.</span></span> <span data-ttu-id="655cb-122">Jste již přihlášeni s HockeyApp.</span><span class="sxs-lookup"><span data-stu-id="655cb-122">You’re already signed up with HockeyApp.</span></span>
   
   ![HockeyApp přihlašovací obrazovky](./media/iot-solution-get-started/image1.png)
3. <span data-ttu-id="655cb-124">Stažení a instalace aplikace hello z tohoto umístění:</span><span class="sxs-lookup"><span data-stu-id="655cb-124">Download and install hello app from here:</span></span>
   
   * [<span data-ttu-id="655cb-125">Android</span><span class="sxs-lookup"><span data-stu-id="655cb-125">Android</span></span>](http://rink.io/spMyDrivingAndroid)
   * [<span data-ttu-id="655cb-126">Windows 10</span><span class="sxs-lookup"><span data-stu-id="655cb-126">Windows 10</span></span>](http://rink.io/spMyDrivingUWP)
   
   <span data-ttu-id="655cb-127">Existují dvě položky.</span><span class="sxs-lookup"><span data-stu-id="655cb-127">There are two items.</span></span> <span data-ttu-id="655cb-128">Nainstalujte certifikát hello v **důvěryhodné osoby**.</span><span class="sxs-lookup"><span data-stu-id="655cb-128">Install hello certificate in **Trusted People**.</span></span> <span data-ttu-id="655cb-129">Nainstalujte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="655cb-129">Then install hello app.</span></span>

<span data-ttu-id="655cb-130">*Všechny problémy počáteční hello aplikace na Windows 10 Mobile?*</span><span class="sxs-lookup"><span data-stu-id="655cb-130">*Any issues starting hello app on Windows 10 Mobile?*</span></span> <span data-ttu-id="655cb-131">Aktualizace nebo dvě za, může být váš telefon.</span><span class="sxs-lookup"><span data-stu-id="655cb-131">Your phone might be an update or two behind.</span></span> <span data-ttu-id="655cb-132">Ujistěte se, že máte k dispozici nejnovější aktualizace hello nebo nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="655cb-132">Make sure you've got hello latest updates, or install:</span></span>

* [<span data-ttu-id="655cb-133">Microsoft.NET.Native.Framework.1.2.appx</span><span class="sxs-lookup"><span data-stu-id="655cb-133">Microsoft.NET.Native.Framework.1.2.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
* [<span data-ttu-id="655cb-134">Microsoft.NET.Native.Runtime.1.1.appx</span><span class="sxs-lookup"><span data-stu-id="655cb-134">Microsoft.NET.Native.Runtime.1.1.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
* [<span data-ttu-id="655cb-135">Microsoft.VCLibs.ARM.14.00.appx</span><span class="sxs-lookup"><span data-stu-id="655cb-135">Microsoft.VCLibs.ARM.14.00.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)

### <a name="ios-installation"></a><span data-ttu-id="655cb-136">instalace iOS</span><span class="sxs-lookup"><span data-stu-id="655cb-136">iOS installation</span></span>
<span data-ttu-id="655cb-137">Pokud jste chodili Build 2016, stáhněte aplikaci hello jako člen týmu našich testů na HockeyApp:</span><span class="sxs-lookup"><span data-stu-id="655cb-137">If you attended Build 2016, download hello app as a member of our test team on HockeyApp:</span></span>

1. <span data-ttu-id="655cb-138">Zařízení s iOS, přihlaste se příliš[HockeyApp](https://rink.hockeyapp.net).</span><span class="sxs-lookup"><span data-stu-id="655cb-138">On your iOS device, sign in too[HockeyApp](https://rink.hockeyapp.net).</span></span>
   <span data-ttu-id="655cb-139">Použijte jeden z hello Microsoft přihlášení tlačítka a přihlaste se hello stejné Microsoft účtu e-mailu zaregistrovali hello konference.</span><span class="sxs-lookup"><span data-stu-id="655cb-139">Use one of hello Microsoft sign-in buttons, and sign in with hello same Microsoft account email that you registered with hello conference.</span></span> <span data-ttu-id="655cb-140">(Nepoužívejte hello pole e-mailu a heslo.)</span><span class="sxs-lookup"><span data-stu-id="655cb-140">(Don’t use hello email and password fields.)</span></span>
   
   ![HockeyApp přihlašovací obrazovky](./media/iot-solution-get-started/image1.png)
2. <span data-ttu-id="655cb-142">Na řídicím panelu HockeyApp hello vyberte MyDriving a stáhněte ho.</span><span class="sxs-lookup"><span data-stu-id="655cb-142">In hello HockeyApp dashboard, select MyDriving and download it.</span></span>
3. <span data-ttu-id="655cb-143">Autorizovat hello betaverze z HockeyApp:</span><span class="sxs-lookup"><span data-stu-id="655cb-143">Authorize hello beta release from HockeyApp:</span></span>
   
   <span data-ttu-id="655cb-144">a.</span><span class="sxs-lookup"><span data-stu-id="655cb-144">a.</span></span> <span data-ttu-id="655cb-145">Přejděte příliš**nastavení** > **Obecné** > **profily a správu zařízení.**</span><span class="sxs-lookup"><span data-stu-id="655cb-145">Go too**Settings** > **General** > **Profiles and Device Management.**</span></span>
   
   <span data-ttu-id="655cb-146">b.</span><span class="sxs-lookup"><span data-stu-id="655cb-146">b.</span></span> <span data-ttu-id="655cb-147">Vztah důvěryhodnosti hello **Bit Stadium GmbH** certifikátu.</span><span class="sxs-lookup"><span data-stu-id="655cb-147">Trust hello **Bit Stadium GmbH** certificate.</span></span>

<span data-ttu-id="655cb-148">Pokud jste nechodili na ni Build 2016, můžete sestavit a nasazení aplikace hello sami:</span><span class="sxs-lookup"><span data-stu-id="655cb-148">If you didn’t attend Build 2016, you can build and deploy hello app yourself:</span></span>

1. <span data-ttu-id="655cb-149">Stáhněte si kód hello [z Githubu].</span><span class="sxs-lookup"><span data-stu-id="655cb-149">Download hello code [from GitHub].</span></span>
2. <span data-ttu-id="655cb-150">Vytvářet a nasazovat s [pomocí Xamarin].</span><span class="sxs-lookup"><span data-stu-id="655cb-150">Build and deploy by [using Xamarin].</span></span>

<span data-ttu-id="655cb-151">Najít další podrobnosti v hello [MyDriving referenční příručka](http://aka.ms/mydrivingdocs).</span><span class="sxs-lookup"><span data-stu-id="655cb-151">Find more details in hello [MyDriving Reference Guide](http://aka.ms/mydrivingdocs).</span></span>

## <a name="get-an-obd-adapter-optional"></a><span data-ttu-id="655cb-152">Získat adaptér Diagnostického (volitelné)</span><span class="sxs-lookup"><span data-stu-id="655cb-152">Get an OBD adapter (optional)</span></span>
<span data-ttu-id="655cb-153">Toto je část hello, který je tato možnost skutečné systému Internet věcí!</span><span class="sxs-lookup"><span data-stu-id="655cb-153">This is hello part that makes this a real Internet of Things system!</span></span> <span data-ttu-id="655cb-154">Můžete používat aplikaci hello bez, ale je další fun s Opravdová věc hello a nejsou nákladné.</span><span class="sxs-lookup"><span data-stu-id="655cb-154">You can use hello app without one, but it’s more fun with hello real thing, and they aren’t expensive.</span></span>

<span data-ttu-id="655cb-155">Integrovaného diagnostiky (Diagnostického) je funkce hello vaše Auto, která hello Garážový používá tootune až automobil a diagnostikovat liché zvuky a žárovky upozornění.</span><span class="sxs-lookup"><span data-stu-id="655cb-155">On-board diagnostics (OBD) is hello feature of your car that hello garage uses tootune up your car and diagnose odd noises and warning lamps.</span></span> <span data-ttu-id="655cb-156">Pokud vaše Auto je skvělým antiquity, najdete v hello kabiny, obvykle za klopa v části řídicího panelu hello někde soketu.</span><span class="sxs-lookup"><span data-stu-id="655cb-156">Unless your car is of great antiquity, you’ll find a socket somewhere in hello cabin, typically behind a flap under hello dashboard.</span></span> <span data-ttu-id="655cb-157">S hello správné konektor můžete získat metriky výkonu hello modul a provést určité úpravy.</span><span class="sxs-lookup"><span data-stu-id="655cb-157">With hello right connector, you can get metrics of hello engine’s performance and make certain adjustments.</span></span> <span data-ttu-id="655cb-158">Konektor Diagnostického lze zakoupit levné z míst, obvykle hello.</span><span class="sxs-lookup"><span data-stu-id="655cb-158">An OBD connector can be purchased cheaply from hello usual places.</span></span> <span data-ttu-id="655cb-159">Připojí pomocí Bluetooth nebo Wi-Fi tooan aplikace na váš telefon.</span><span class="sxs-lookup"><span data-stu-id="655cb-159">It connects by using Bluetooth or Wi-Fi tooan app on your phone.</span></span>

<span data-ttu-id="655cb-160">V tomto případě vytvoříme tooconnect car toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="655cb-160">In this case, we’re going tooconnect your car toohello cloud.</span></span> <span data-ttu-id="655cb-161">Hello přímé připojení z hello Diagnostického je tooyour telefon, ale naše aplikace funguje jako předávací hostitel.</span><span class="sxs-lookup"><span data-stu-id="655cb-161">hello direct connection from hello OBD is tooyour phone, but our app works as a relay.</span></span> <span data-ttu-id="655cb-162">Vaše Auto telemetrie odeslala přímých toohello MyDriving IoT hub, kde je zpracovat toolog vaše silniční služebních cest a vyhodnocení vašeho řízení stylu.</span><span class="sxs-lookup"><span data-stu-id="655cb-162">Your car's telemetry is sent straight toohello MyDriving IoT hub, where it's processed toolog your road trips and assess your driving style.</span></span>

<span data-ttu-id="655cb-163">zařízení s Diagnostického tooconnect:</span><span class="sxs-lookup"><span data-stu-id="655cb-163">tooconnect an OBD device:</span></span>

1. <span data-ttu-id="655cb-164">Zkontrolujte, zda vaše Auto Diagnostického soketu.</span><span class="sxs-lookup"><span data-stu-id="655cb-164">Check that your car has an OBD socket.</span></span>
2. <span data-ttu-id="655cb-165">Získejte Diagnostického adaptér:</span><span class="sxs-lookup"><span data-stu-id="655cb-165">Obtain an OBD adapter:</span></span>
   
   * <span data-ttu-id="655cb-166">Pokud používáte Android nebo Windows phone, budete potřebovat adaptér II Diagnostického zařízeními podporujícími technologii Bluetooth.</span><span class="sxs-lookup"><span data-stu-id="655cb-166">If you're using an Android or Windows phone, you need a Bluetooth-enabled OBD II adapter.</span></span> <span data-ttu-id="655cb-167">Použili jsme [BAFX produkty 34t5 Bluetooth OBDII kontrolovat nástroj].</span><span class="sxs-lookup"><span data-stu-id="655cb-167">We used [BAFX Products 34t5 Bluetooth OBDII Scan Tool].</span></span>
   * <span data-ttu-id="655cb-168">Pokud používáte phone iOS, budete potřebovat Wi-Fi povolit Diagnostického adaptér.</span><span class="sxs-lookup"><span data-stu-id="655cb-168">If you're using an iOS phone, you need a Wi-Fi-enabled OBD adapter.</span></span> <span data-ttu-id="655cb-169">Použili jsme [ScanTool OBDLink MX Wi-Fi: Diagnostického adaptér nebo diagnostiky skener].</span><span class="sxs-lookup"><span data-stu-id="655cb-169">We used [ScanTool OBDLink MX Wi-Fi: OBD Adapter/Diagnostic Scanner].</span></span>
3. <span data-ttu-id="655cb-170">Postupujte podle hello pokynů dodaných s vaší tooconnect adaptér Diagnostického ho tooyour phone.</span><span class="sxs-lookup"><span data-stu-id="655cb-170">Follow hello instructions that come with your OBD adapter tooconnect it tooyour phone.</span></span> <span data-ttu-id="655cb-171">Následující hello mějte na paměti:</span><span class="sxs-lookup"><span data-stu-id="655cb-171">Keep hello following in mind:</span></span>
   
   * <span data-ttu-id="655cb-172">Adaptér Bluetooth musí být spárována s phone hello na hello **nastavení** stránky.</span><span class="sxs-lookup"><span data-stu-id="655cb-172">A Bluetooth adapter must be paired with hello phone, on hello **Settings** page.</span></span>
   * <span data-ttu-id="655cb-173">Wi-Fi adaptér musí mít adresu v rozsahu 192.168.xxx.xxx hello.</span><span class="sxs-lookup"><span data-stu-id="655cb-173">A Wi-Fi adapter must have an address in hello range 192.168.xxx.xxx.</span></span>
4. <span data-ttu-id="655cb-174">Pokud máte několik aut, můžete využít samostatný adaptér získat pro každý (maximálně tři).</span><span class="sxs-lookup"><span data-stu-id="655cb-174">If you have several cars, you can get a separate adapter for each (maximum of three).</span></span>

<span data-ttu-id="655cb-175">Pokud nemáte Diagnostického adaptér, budou hello aplikace i dál posílat umístění a rychlost data z hello phone GPS příjemce toohello zpět končit a požádá, pokud chcete, aby toosimulate Diagnostického.</span><span class="sxs-lookup"><span data-stu-id="655cb-175">If you don’t have an OBD adapter, hello app will still send location and speed data from hello phone's GPS receiver toohello back end and will ask if you want toosimulate an OBD.</span></span>

<span data-ttu-id="655cb-176">Můžete zjistit informace o tom, jak aplikace hello používá data z adaptéru Diagnostického hello a o možnostech pro vytvoření vlastní Diagnostického zařízení v části 2.1, "Zařízení IoT," v hello [MyDriving referenční příručka](http://aka.ms/mydrivingdocs).</span><span class="sxs-lookup"><span data-stu-id="655cb-176">You can find out more about how hello app uses data from hello OBD adapter and about options for creating your own OBD device in section 2.1, "IoT Devices," in hello [MyDriving Reference Guide](http://aka.ms/mydrivingdocs).</span></span>

## <a name="use-hello-app"></a><span data-ttu-id="655cb-177">Použití aplikace hello</span><span class="sxs-lookup"><span data-stu-id="655cb-177">Use hello app</span></span>
<span data-ttu-id="655cb-178">Spustí aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="655cb-178">Start hello app.</span></span> <span data-ttu-id="655cb-179">Dojde počáteční toowalk rychlý start vás jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="655cb-179">There’s an initial Quickstart toowalk you through how it works.</span></span>

### <a name="track-your-trips"></a><span data-ttu-id="655cb-180">Sledovat vaše služebních cest</span><span class="sxs-lookup"><span data-stu-id="655cb-180">Track your trips</span></span>
<span data-ttu-id="655cb-181">Klepněte na hello záznamů tlačítko (big červené kolečko v hello dolní části obrazovky hello) toostart služební cestě a klepněte na znovu tooend.</span><span class="sxs-lookup"><span data-stu-id="655cb-181">Tap hello record button (big red circle at hello bottom of hello screen) toostart a trip, and tap again tooend.</span></span>

![Obrázek tlačítka hello záznam pro cestu sledování](./media/iot-solution-get-started/image2.png)

<span data-ttu-id="655cb-183">Při každém spuštění na cestu, pokud neexistuje žádné zařízení Diagnostického, budete vyzváni Pokud chcete simulátor toouse hello.</span><span class="sxs-lookup"><span data-stu-id="655cb-183">Each time you start a trip, if there’s no OBD device, you’ll be asked if you want toouse hello simulator.</span></span>

<span data-ttu-id="655cb-184">Na konci hello cesty klepněte na tlačítko Zastavit hello a získat shrnutí.</span><span class="sxs-lookup"><span data-stu-id="655cb-184">At hello end of a trip, tap hello stop button, and you get a summary.</span></span>

![Příklad cesty souhrn](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a><span data-ttu-id="655cb-186">Zkontrolujte vaše služebních cest</span><span class="sxs-lookup"><span data-stu-id="655cb-186">Review your trips</span></span>
![Příklad posledních cesty](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a><span data-ttu-id="655cb-188">Zkontrolujte váš profil</span><span class="sxs-lookup"><span data-stu-id="655cb-188">Review your profile</span></span>
![Příklad profilu řídí stylu](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a><span data-ttu-id="655cb-190">Pošlete nám svůj názor testu</span><span class="sxs-lookup"><span data-stu-id="655cb-190">Send us your test feedback</span></span>
<span data-ttu-id="655cb-191">Vzhledem k tomu, že jsme vytvořili vlastní systémy IoT MyDriving toohelp základní informace, chceme jistě toohear od vás o tom, jak dobře funguje.</span><span class="sxs-lookup"><span data-stu-id="655cb-191">Because we created MyDriving toohelp jumpstart your own IoT systems, we certainly want toohear from you about how well it works.</span></span> <span data-ttu-id="655cb-192">Dejte nám vědět, pokud:</span><span class="sxs-lookup"><span data-stu-id="655cb-192">Let us know if:</span></span>

* <span data-ttu-id="655cb-193">Narazíte na problémy nebo výzvami.</span><span class="sxs-lookup"><span data-stu-id="655cb-193">You run into difficulties or challenges.</span></span>
* <span data-ttu-id="655cb-194">Není k rozšíření bodu, který by bylo vhodnější tooyour scénář.</span><span class="sxs-lookup"><span data-stu-id="655cb-194">There is an extension point that would make it more suitable tooyour scenario.</span></span>
* <span data-ttu-id="655cb-195">Najít efektivnější způsob tooaccomplish určité požadavky.</span><span class="sxs-lookup"><span data-stu-id="655cb-195">You find a more efficient way tooaccomplish certain needs.</span></span>
* <span data-ttu-id="655cb-196">Máte další návrhy na zlepšení MyDriving nebo této dokumentace.</span><span class="sxs-lookup"><span data-stu-id="655cb-196">You have any other suggestions for improving MyDriving or this documentation.</span></span>

<span data-ttu-id="655cb-197">V rámci hello MyDriving aplikace, můžete použít hello předdefinované HockeyApp zpětnou vazbu mechanismus: na iOS a Android, právě poskytnout telefonu zatřesením, nebo použijte hello **zpětné vazby** příkazu nabídky.</span><span class="sxs-lookup"><span data-stu-id="655cb-197">Within hello MyDriving app itself, you can use hello built-in HockeyApp feedback mechanism: on iOS and Android, just give your phone a shake, or use hello **Feedback** menu command.</span></span> <span data-ttu-id="655cb-198">Snímek, to bude připojit automaticky, takže jsme budete vědět, co jste o rozhovoru.</span><span class="sxs-lookup"><span data-stu-id="655cb-198">This will automatically attach a screenshot, so that we’ll know what you’re talking about.</span></span> <span data-ttu-id="655cb-199">A pokud jsou všechny velice nepříjemná havárií, HockeyApp shromažďuje hello havárií protokoly tootell nám o nich.</span><span class="sxs-lookup"><span data-stu-id="655cb-199">And if there are any unfortunate crashes, HockeyApp collects hello crash logs tootell us about them.</span></span> <span data-ttu-id="655cb-200">Můžete také poskytnout zpětnou vazbu prostřednictvím hello [HockeyApp portál].</span><span class="sxs-lookup"><span data-stu-id="655cb-200">You can also give feedback through hello [HockeyApp portal].</span></span>

<span data-ttu-id="655cb-201">Můžete také soubor [problém na Githubu], nebo komentář níže (en-us edition).</span><span class="sxs-lookup"><span data-stu-id="655cb-201">You can also file an [issue on GitHub], or leave a comment below (en-us edition).</span></span>

<span data-ttu-id="655cb-202">Těšíme toohearing od vás!</span><span class="sxs-lookup"><span data-stu-id="655cb-202">We look forward toohearing from you!</span></span>

## <a name="next-steps"></a><span data-ttu-id="655cb-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="655cb-203">Next steps</span></span>
* <span data-ttu-id="655cb-204">Prozkoumejte hello [MyDriving referenční příručka](http://aka.ms/mydrivingdocs) toounderstand jak jsme navržen a sestaven hello celý systém MyDriving.</span><span class="sxs-lookup"><span data-stu-id="655cb-204">Explore hello [MyDriving Reference Guide](http://aka.ms/mydrivingdocs) toounderstand how we’ve designed and built hello entire MyDriving system.</span></span>
* <span data-ttu-id="655cb-205">[Vytvořit a nasadit vlastní systému](iot-solution-build-system.md) pomocí našich skriptů Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="655cb-205">[Create and deploy a system of your own](iot-solution-build-system.md) by using our Azure Resource Manager scripts.</span></span> <span data-ttu-id="655cb-206">Hello [MyDriving referenční příručka](http://aka.ms/mydrivingdocs) také vás provede oblasti, kde budete provádět hello v většina úpravy.</span><span class="sxs-lookup"><span data-stu-id="655cb-206">hello [MyDriving Reference Guide](http://aka.ms/mydrivingdocs) also guides you through areas where you’ll make hello most customizations.</span></span>

[z Githubu]: https://github.com/Azure-Samples/MyDriving
[pomocí Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/
[BAFX produkty 34t5 Bluetooth OBDII kontrolovat nástroj]: http://www.amazon.com/gp/product/B005NLQAHS
[ScanTool OBDLink MX Wi-Fi: Diagnostického adaptér nebo diagnostiky skener]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
[HockeyApp portál]: https://rink.hockeyapp.org
[problém na Githubu]: https://github.com/Azure-Samples/MyDriving/issues
