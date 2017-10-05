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
# <a name="mydriving-iot-system-quick-start"></a>Systém MyDriving IoT: rychlý start
MyDriving je systém, který ukazuje návrhu a implementace typické [Internet věcí](iot-suite-overview.md) řešení (IoT), který shromažďuje telemetrická data ze zařízení, zpracuje tato data v cloudu a použije strojového učení k poskytování adaptivní odpověď. Ukázky zaznamená data o vaší služebních cest car pomocí dat z mobilního telefonu a adaptér, který shromažďuje informace z vašeho automobilu řízení systému. Tato data se používá k poskytnutí zpětné vazby na rozdíl od jiných uživatelů vaší řízení stylu.

Skutečné účelem MyDriving je vám pomůžou začít s vytvářením řešení IoT. Ale před Pojďme vám tedy budete s aplikací MyDriving – jako člen týmu pro naše testovací uživatele. To vám dává prostředí aplikace a systém za ho jako příjemce, než se pustíte do architekturu. Je také vás seznámí s HockeyApp nástrojů způsob správy distribuce alpha a beta aplikací, jak testovat uživatele.

## <a name="use-the-mobile-experience"></a>Použít mobilní prostředí
Pokud máte zařízení se systémem Android, iOS nebo Windows 10 můžete používat aplikaci MyDriving.

### <a name="android-and-windows-10-mobile-installation"></a>Instalace Windows 10 Mobile a Android
Na zařízení:

1. Povolit vývoj aplikací:
   
   * Android: V **nastavení** > **zabezpečení**, povolit aplikacím z **neznámé zdroje**.
   * Windows 10: V **nastavení** > **aktualizace** > **pro vývojáře**, nastavte **režim vývojáře**.
2. Připojit náš tým testování tak, že se přihlásíte, nebo přihlášení k, [HockeyApp](https://rink.hockeyapp.net). HockeyApp usnadňuje distribuci raných verzích aplikaci, kterou chcete testovat uživatele.
   
   Pokud používáte Windows 10, použijte prohlížeč Microsoft Edge.
   
   Pokud byste byli Build 2016 účastníka, přihlaste se pomocí stejné Microsoft účtu e-mailu zaregistrovaný pro konference, pomocí jedné z tlačítka společností Microsoft. Jste již přihlášeni s HockeyApp.
   
   ![HockeyApp přihlašovací obrazovky](./media/iot-solution-get-started/image1.png)
3. Stáhněte a nainstalujte aplikaci z tohoto umístění:
   
   * [Android](http://rink.io/spMyDrivingAndroid)
   * [Windows 10](http://rink.io/spMyDrivingUWP)
   
   Existují dvě položky. Instalaci certifikátu v **důvěryhodné osoby**. Nainstalujte aplikaci.

*Problémy s spouštění aplikace na Windows 10 Mobile?* Aktualizace nebo dvě za, může být váš telefon. Ujistěte se, že máte k dispozici nejnovější aktualizace, nebo nainstalujte:

* [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
* [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
* [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)

### <a name="ios-installation"></a>instalace iOS
Pokud jste chodili Build 2016, stáhněte si aplikaci jako člen týmu našich testů na HockeyApp:

1. Na zařízení s iOS, přihlaste se k [HockeyApp](https://rink.hockeyapp.net).
   Použijte jeden z Microsoft přihlášení tlačítka a přihlaste se pomocí stejné e-mail účet Microsoft, který zaregistrovali konference. (Nepoužívejte pole e-mailu a heslo.)
   
   ![HockeyApp přihlašovací obrazovky](./media/iot-solution-get-started/image1.png)
2. Na řídicím panelu HockeyApp MyDriving vyberte a stáhněte ho.
3. Autorizovat betaverze z HockeyApp:
   
   a. Přejděte na **nastavení** > **Obecné** > **profily a správu zařízení.**
   
   b. Vztah důvěryhodnosti **Bit Stadium GmbH** certifikátu.

Pokud jste nechodili na ni Build 2016, můžete sestavit a nasadit aplikaci:

1. Stáhněte si kód [z Githubu].
2. Vytvářet a nasazovat s [pomocí Xamarin].

Najít další podrobnosti najdete [MyDriving referenční příručka](http://aka.ms/mydrivingdocs).

## <a name="get-an-obd-adapter-optional"></a>Získat adaptér Diagnostického (volitelné)
Toto je část, která je tato možnost skutečné systému Internet věcí! Můžete používat aplikaci bez, ale je další fun s Opravdová věc a nejsou nákladné.

Integrovaného diagnostiky (Diagnostického) je funkce vaše Auto, která garáž používá nastavení automobil výkonu a Diagnostika liché zvuky a žárovky upozornění. Pokud vaše Auto je skvělým antiquity, zjistíte soket někde v kabině, obvykle za klopa v řídicím panelu. Pravé konektor můžete získat metriky výkonu stroje a provést určité úpravy. Konektor Diagnostického lze zakoupit levné z obvyklé místech. Připojí pomocí Bluetooth nebo Wi-Fi do aplikace na váš telefon.

V tomto případě vytvoříme automobil připojit ke cloudu. Přímé připojení z Diagnostického je na váš telefon, ale naše aplikace funguje jako předávací hostitel. Vaše Auto telemetrie je odeslána přímo k centru MyDriving IoT, kde je zpracována protokolu vaší silniční služebních cest a vyhodnocení vašeho řízení stylu.

Připojení zařízení s Diagnostického:

1. Zkontrolujte, zda vaše Auto Diagnostického soketu.
2. Získejte Diagnostického adaptér:
   
   * Pokud používáte Android nebo Windows phone, budete potřebovat adaptér II Diagnostického zařízeními podporujícími technologii Bluetooth. Použili jsme [BAFX produkty 34t5 Bluetooth OBDII kontrolovat nástroj].
   * Pokud používáte phone iOS, budete potřebovat Wi-Fi povolit Diagnostického adaptér. Použili jsme [ScanTool OBDLink MX Wi-Fi: Diagnostického adaptér nebo diagnostiky skener].
3. Postupujte podle pokynů dodaných s adaptérem Diagnostického pro připojení k telefonu. Následující mějte na paměti:
   
   * Adaptér Bluetooth musí být spárována s telefonu, na **nastavení** stránky.
   * Wi-Fi adaptér musí mít adresu v rozsahu 192.168.xxx.xxx.
4. Pokud máte několik aut, můžete využít samostatný adaptér získat pro každý (maximálně tři).

Pokud nemáte adaptér Diagnostického, aplikace bude i dál posílat data o umístění a rychlost z telefonu GPS příjemce k back-endu a požádá, pokud chcete simulovat Diagnostického.

Můžete zjistit informace o tom, jak aplikace používá data z adaptéru Diagnostického a o možnostech pro vytvoření vlastní Diagnostického zařízení v části 2.1, "Zařízení IoT," v [MyDriving referenční příručka](http://aka.ms/mydrivingdocs).

## <a name="use-the-app"></a>Použití aplikace
Spusťte aplikaci. Je počáteční rychlý úvodní kurz pro vás provede procesem jak to funguje.

### <a name="track-your-trips"></a>Sledovat vaše služebních cest
Klepněte na tlačítko záznam (big červeném kroužku v dolní části obrazovky) spustit služební cestě a klepněte na znovu na konec.

![Obrázek na tlačítko záznam pro cestu sledování](./media/iot-solution-get-started/image2.png)

Při každém spuštění na cestu, pokud neexistuje žádné zařízení Diagnostického, budete vyzváni Pokud budete chtít použít simulátoru.

Na konci cesty klepněte na tlačítko Zastavit a získat shrnutí.

![Příklad cesty souhrn](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Zkontrolujte vaše služebních cest
![Příklad posledních cesty](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Zkontrolujte váš profil
![Příklad profilu řídí stylu](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Pošlete nám svůj názor testu
Vzhledem k tomu, že jsme vytvořili vlastní systémy IoT MyDriving pomohou základní informace, jistě chceme slyšet od vás o tom, jak dobře funguje. Dejte nám vědět, pokud:

* Narazíte na problémy nebo výzvami.
* Není k rozšíření bodu, který by bylo vhodnější pro váš scénář.
* Můžete najít efektivnější způsob, jak provést určité požadavky.
* Máte další návrhy na zlepšení MyDriving nebo této dokumentace.

V rámci MyDriving aplikace, můžete použít předdefinované zpětnou vazbu mechanismus HockeyApp: na iOS a Android, právě poskytnout telefonu zatřesením, nebo použijte **zpětné vazby** příkazu nabídky. Snímek, to bude připojit automaticky, takže jsme budete vědět, co jste o rozhovoru. A pokud jsou všechny velice nepříjemná havárií, HockeyApp shromažďuje protokoly havárií na Řekněte nám o nich. Můžete také poskytnout zpětnou vazbu prostřednictvím [HockeyApp portál].

Můžete také soubor [problém na Githubu], nebo komentář níže (en-us edition).

Těšíme sluchu od vás!

## <a name="next-steps"></a>Další kroky
* Prozkoumejte [MyDriving referenční příručka](http://aka.ms/mydrivingdocs) pochopit, jak jsme jste navržen a sestaven celý systém MyDriving.
* [Vytvořit a nasadit vlastní systému](iot-solution-build-system.md) pomocí našich skriptů Azure Resource Manager. [MyDriving referenční příručka](http://aka.ms/mydrivingdocs) také vás provede oblasti, kde budete provádět úpravy nejvíc.

[z webu GitHub]: https://github.com/Azure-Samples/MyDriving
[pomocí Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/
[Nástroj pro BAFX produkty 34t5 Bluetooth OBDII kontrolu]: http://www.amazon.com/gp/product/B005NLQAHS
[Skener adaptér nebo diagnostiky Diagnostického ScanTool OBDLink MX Wi-Fi:]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
[HockeyApp portálu]: https://rink.hockeyapp.org
[vydávat na Githubu]: https://github.com/Azure-Samples/MyDriving/issues
