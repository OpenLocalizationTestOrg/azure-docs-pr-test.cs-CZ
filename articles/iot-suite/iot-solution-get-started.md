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
# <a name="mydriving-iot-system-quick-start"></a>Systém MyDriving IoT: rychlý start
MyDriving je systém, který ukazuje hello návrhu a implementace typické [Internet věcí](iot-suite-overview.md) řešení (IoT), který shromažďuje telemetrická data ze zařízení, zpracuje tato data v cloudu hello a platí machine learning tooprovide adaptivní odpovědi. Ukázka Hello zaznamená data o vaší služebních cest car pomocí dat z mobilního telefonu a adaptér, který shromažďuje informace z vašeho automobilu řízení systému. Tato zpětná vazba tooprovide dat používá řízení jakým způsobem v porovnání tooother uživatelé.

Hello skutečnému účelu MyDriving je tooget, kterou jste zahájili při vytváření vlastní řešení IoT. Ale před Pojďme vám tedy budete s hello MyDriving aplikace – jako člen týmu pro naše testovací uživatele. To vám dává prostředí aplikace hello a hello systém za ho jako příjemce, než se pustíte do architektury hello. Je také vás seznámí tooHockeyApp nástrojů způsob správy distribuce alpha a beta hello tootest uživatelů vaší aplikace.

## <a name="use-hello-mobile-experience"></a>Použít mobilní prostředí hello
Pokud máte zařízení se systémem Android, iOS nebo Windows 10 můžete používat aplikaci MyDriving hello.

### <a name="android-and-windows-10-mobile-installation"></a>Instalace Windows 10 Mobile a Android
Na zařízení:

1. Povolit vývoj aplikací:
   
   * Android: V **nastavení** > **zabezpečení**, povolit aplikacím z **neznámé zdroje**.
   * Windows 10: V **nastavení** > **aktualizace** > **pro vývojáře**, nastavte **režim vývojáře**.
2. Připojit náš tým testování tak, že se přihlásíte, nebo přihlášení k, [HockeyApp](https://rink.hockeyapp.net). HockeyApp umožňuje snadno toodistribute raných verzích tootest uživatelů vaší aplikace.
   
   Pokud používáte Windows 10, použijte prohlížeč Edge hello.
   
   Pokud byste byli Build 2016 účastníka, přihlaste se pomocí hello stejné e-mail účet Microsoft, který jste zaregistrováni hello konference, pomocí jedné z hello tlačítka společností Microsoft. Jste již přihlášeni s HockeyApp.
   
   ![HockeyApp přihlašovací obrazovky](./media/iot-solution-get-started/image1.png)
3. Stažení a instalace aplikace hello z tohoto umístění:
   
   * [Android](http://rink.io/spMyDrivingAndroid)
   * [Windows 10](http://rink.io/spMyDrivingUWP)
   
   Existují dvě položky. Nainstalujte certifikát hello v **důvěryhodné osoby**. Nainstalujte aplikaci hello.

*Všechny problémy počáteční hello aplikace na Windows 10 Mobile?* Aktualizace nebo dvě za, může být váš telefon. Ujistěte se, že máte k dispozici nejnovější aktualizace hello nebo nainstalujte:

* [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
* [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
* [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)

### <a name="ios-installation"></a>instalace iOS
Pokud jste chodili Build 2016, stáhněte aplikaci hello jako člen týmu našich testů na HockeyApp:

1. Zařízení s iOS, přihlaste se příliš[HockeyApp](https://rink.hockeyapp.net).
   Použijte jeden z hello Microsoft přihlášení tlačítka a přihlaste se hello stejné Microsoft účtu e-mailu zaregistrovali hello konference. (Nepoužívejte hello pole e-mailu a heslo.)
   
   ![HockeyApp přihlašovací obrazovky](./media/iot-solution-get-started/image1.png)
2. Na řídicím panelu HockeyApp hello vyberte MyDriving a stáhněte ho.
3. Autorizovat hello betaverze z HockeyApp:
   
   a. Přejděte příliš**nastavení** > **Obecné** > **profily a správu zařízení.**
   
   b. Vztah důvěryhodnosti hello **Bit Stadium GmbH** certifikátu.

Pokud jste nechodili na ni Build 2016, můžete sestavit a nasazení aplikace hello sami:

1. Stáhněte si kód hello [z Githubu].
2. Vytvářet a nasazovat s [pomocí Xamarin].

Najít další podrobnosti v hello [MyDriving referenční příručka](http://aka.ms/mydrivingdocs).

## <a name="get-an-obd-adapter-optional"></a>Získat adaptér Diagnostického (volitelné)
Toto je část hello, který je tato možnost skutečné systému Internet věcí! Můžete používat aplikaci hello bez, ale je další fun s Opravdová věc hello a nejsou nákladné.

Integrovaného diagnostiky (Diagnostického) je funkce hello vaše Auto, která hello Garážový používá tootune až automobil a diagnostikovat liché zvuky a žárovky upozornění. Pokud vaše Auto je skvělým antiquity, najdete v hello kabiny, obvykle za klopa v části řídicího panelu hello někde soketu. S hello správné konektor můžete získat metriky výkonu hello modul a provést určité úpravy. Konektor Diagnostického lze zakoupit levné z míst, obvykle hello. Připojí pomocí Bluetooth nebo Wi-Fi tooan aplikace na váš telefon.

V tomto případě vytvoříme tooconnect car toohello cloudu. Hello přímé připojení z hello Diagnostického je tooyour telefon, ale naše aplikace funguje jako předávací hostitel. Vaše Auto telemetrie odeslala přímých toohello MyDriving IoT hub, kde je zpracovat toolog vaše silniční služebních cest a vyhodnocení vašeho řízení stylu.

zařízení s Diagnostického tooconnect:

1. Zkontrolujte, zda vaše Auto Diagnostického soketu.
2. Získejte Diagnostického adaptér:
   
   * Pokud používáte Android nebo Windows phone, budete potřebovat adaptér II Diagnostického zařízeními podporujícími technologii Bluetooth. Použili jsme [BAFX produkty 34t5 Bluetooth OBDII kontrolovat nástroj].
   * Pokud používáte phone iOS, budete potřebovat Wi-Fi povolit Diagnostického adaptér. Použili jsme [ScanTool OBDLink MX Wi-Fi: Diagnostického adaptér nebo diagnostiky skener].
3. Postupujte podle hello pokynů dodaných s vaší tooconnect adaptér Diagnostického ho tooyour phone. Následující hello mějte na paměti:
   
   * Adaptér Bluetooth musí být spárována s phone hello na hello **nastavení** stránky.
   * Wi-Fi adaptér musí mít adresu v rozsahu 192.168.xxx.xxx hello.
4. Pokud máte několik aut, můžete využít samostatný adaptér získat pro každý (maximálně tři).

Pokud nemáte Diagnostického adaptér, budou hello aplikace i dál posílat umístění a rychlost data z hello phone GPS příjemce toohello zpět končit a požádá, pokud chcete, aby toosimulate Diagnostického.

Můžete zjistit informace o tom, jak aplikace hello používá data z adaptéru Diagnostického hello a o možnostech pro vytvoření vlastní Diagnostického zařízení v části 2.1, "Zařízení IoT," v hello [MyDriving referenční příručka](http://aka.ms/mydrivingdocs).

## <a name="use-hello-app"></a>Použití aplikace hello
Spustí aplikaci hello. Dojde počáteční toowalk rychlý start vás jak to funguje.

### <a name="track-your-trips"></a>Sledovat vaše služebních cest
Klepněte na hello záznamů tlačítko (big červené kolečko v hello dolní části obrazovky hello) toostart služební cestě a klepněte na znovu tooend.

![Obrázek tlačítka hello záznam pro cestu sledování](./media/iot-solution-get-started/image2.png)

Při každém spuštění na cestu, pokud neexistuje žádné zařízení Diagnostického, budete vyzváni Pokud chcete simulátor toouse hello.

Na konci hello cesty klepněte na tlačítko Zastavit hello a získat shrnutí.

![Příklad cesty souhrn](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Zkontrolujte vaše služebních cest
![Příklad posledních cesty](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Zkontrolujte váš profil
![Příklad profilu řídí stylu](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Pošlete nám svůj názor testu
Vzhledem k tomu, že jsme vytvořili vlastní systémy IoT MyDriving toohelp základní informace, chceme jistě toohear od vás o tom, jak dobře funguje. Dejte nám vědět, pokud:

* Narazíte na problémy nebo výzvami.
* Není k rozšíření bodu, který by bylo vhodnější tooyour scénář.
* Najít efektivnější způsob tooaccomplish určité požadavky.
* Máte další návrhy na zlepšení MyDriving nebo této dokumentace.

V rámci hello MyDriving aplikace, můžete použít hello předdefinované HockeyApp zpětnou vazbu mechanismus: na iOS a Android, právě poskytnout telefonu zatřesením, nebo použijte hello **zpětné vazby** příkazu nabídky. Snímek, to bude připojit automaticky, takže jsme budete vědět, co jste o rozhovoru. A pokud jsou všechny velice nepříjemná havárií, HockeyApp shromažďuje hello havárií protokoly tootell nám o nich. Můžete také poskytnout zpětnou vazbu prostřednictvím hello [HockeyApp portál].

Můžete také soubor [problém na Githubu], nebo komentář níže (en-us edition).

Těšíme toohearing od vás!

## <a name="next-steps"></a>Další kroky
* Prozkoumejte hello [MyDriving referenční příručka](http://aka.ms/mydrivingdocs) toounderstand jak jsme navržen a sestaven hello celý systém MyDriving.
* [Vytvořit a nasadit vlastní systému](iot-solution-build-system.md) pomocí našich skriptů Azure Resource Manager. Hello [MyDriving referenční příručka](http://aka.ms/mydrivingdocs) také vás provede oblasti, kde budete provádět hello v většina úpravy.

[z Githubu]: https://github.com/Azure-Samples/MyDriving
[pomocí Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/
[BAFX produkty 34t5 Bluetooth OBDII kontrolovat nástroj]: http://www.amazon.com/gp/product/B005NLQAHS
[ScanTool OBDLink MX Wi-Fi: Diagnostického adaptér nebo diagnostiky skener]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
[HockeyApp portál]: https://rink.hockeyapp.org
[problém na Githubu]: https://github.com/Azure-Samples/MyDriving/issues
