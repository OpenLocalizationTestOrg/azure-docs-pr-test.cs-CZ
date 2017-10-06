---
title: aaaOffline synchronizaci dat v Azure Mobile Apps | Microsoft Docs
description: "Reference konceptu a Přehled funkce synchronizace offline dat hello pro Azure Mobile Apps"
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 982fb683-8884-40da-96e6-77eeca2500e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 58673240ba433651faf1f619ca5da33dd6459d2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="offline-data-sync-in-azure-mobile-apps"></a>Offline synchronizace dat pro Azure Mobile Apps
## <a name="what-is-offline-data-sync"></a>Co je offline synchronizací dat?
Synchronizace offline dat je klientských a serverových funkcí sady SDK Azure Mobile Apps, který usnadňuje vývojářům toocreate aplikace, které jsou funkční bez připojení k síti.

Pokud vaše aplikace je v offline režimu, můžete přesto vytvořit a upravit data, která se uloží tooa místního úložiště. Když je aplikace hello zpět do režimu online, se synchronizovat místní změny se váš back-end mobilní aplikace Azure. Funkce Hello zahrnuje taky podporu pro zjišťování konfliktů, když hello stejné záznam se změní na obou hello klienta a hello back-end. Konflikty můžete ji zpracovat buď na hello server nebo hello klienta.

Offline synchronizace má několik výhod:

* Ukládání do mezipaměti serveru data místně na zařízení hello vylepšit rychlost reakce aplikace
* Vytvářet robustní aplikace, které zůstávají užitečné, když jsou problémy s síti
* Povolit koncovým uživatelům toocreate a upravovat data i v případě, že neexistuje žádný přístup k síti, podporuje scénáře s malým nebo žádným připojením
* Synchronizaci dat mezi různými zařízeními a rozpoznat je v konfliktu, kdy hello stejné záznam se mění dvěma zařízeními
* Omezit využití sítě v sítích s vysokou latencí nebo měření podle objemu

Hello následující kurzy ukazují, jak tooadd offline synchronizace tooyour mobilních klientů pomocí Azure Mobile Apps:

* [Android: Zapnutí offline synchronizace]
* [Apache Cordova: Zapnutí offline synchronizace](app-service-mobile-cordova-get-started-offline-data.md)
* [iOS: zapnutí offline synchronizace]
* [Xamarin iOS: zapnutí offline synchronizace]
* [Xamarin Android: Zapnutí offline synchronizace]
* [Xamarin.Forms: Offline synchronizace povolit](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [univerzální platformu Windows: zapnutí offline synchronizace]

## <a name="what-is-a-sync-table"></a>Co je synchronizace tabulky?
tooaccess hello "/ tabulky" koncový bod, klient Azure Mobile hello sady SDK poskytují rozhraní například `IMobileServiceTable` (klient .NET SDK) nebo `MSTable` (iOS klienta). Tato rozhraní API připojit přímo back-end mobilní aplikace Azure toohello a selhat, pokud zařízení klienta hello nemá připojení k síti.

toosupport offline, vaše aplikace by měla místo toho použít hello *synchronizace tabulky* rozhraní API, například `IMobileServiceSyncTable` (klient .NET SDK) nebo `MSSyncTable` (iOS klienta). Všechny hello stejné operace CRUD (vytvořit, číst, Update, Delete) pracovat s synchronizace tabulky rozhraní API, s výjimkou teď budou číst z nebo zápis tooa *místního úložiště*. Před provedením jakékoli operace s tabulkou synchronizace, je nutné inicializovat hello místního úložiště.

## <a name="what-is-a-local-store"></a>Co je místní úložiště?
Místní úložiště je vrstvu trvalosti dat hello hello klientského zařízení. Hello Azure Mobile Apps klientské sady SDK poskytují výchozí místní úložiště implementace. V systému Windows, Xamarin a Android je založena na SQLite. V systému iOS je založena na základní Data.

toouse hello SQLite implementace založené na Windows Phone nebo Windows Store 8.1, musíte tooinstall rozšíření SQLite. Další informace najdete v tématu [univerzální platformu Windows: zapnutí offline synchronizace]. Android a iOS dodávají spolu s verzí SQLite v zařízení hello operační systém, samostatně, takže není nutné tooreference svou vlastní verzi SQLite.

Vývojářům můžete taky implementovat vlastní místní úložiště. Například pokud chcete toostore data v šifrovaném formátu v hello mobilního klienta, můžete definovat místní úložiště, které používá SQLCipher pro šifrování.

## <a name="what-is-a-sync-context"></a>Co je kontext synchronizace?
A *kontext synchronizace* souvisí s objektem mobilního klienta (například `IMobileServiceClient` nebo `MSClient`) a sleduje změny provedené s tabulkami synchronizace. kontext synchronizace Hello udržuje *operace fronty*, což zajišťuje uspořádaný seznam vytvoření operace (Create, Update, Delete), která je novější odeslat toohello serveru.

Místní úložiště je spojena s kontext synchronizace hello pomocí metodu initialize, jako třeba `IMobileServicesSyncContext.InitializeAsync(localstore)` v hello [klient .NET SDK].

## <a name="how-sync-works"></a>Jak offline synchronizace funguje
Při použití synchronizace tabulky, váš klientský kód řídí, kdy místní změny jsou synchronizovány s back-end mobilní aplikace Azure. Nic se odesílají toohello back-end, dokud není volání příliš*nabízené* místní změny. Podobně místního úložiště hello naplněný nová data jenom v případě, že je příliš volání*vyžádání* data.

* **Nabízená**: nabízení je operace v kontextu synchronizace hello a odešle všechny změny vytvoření od poslední nabízené hello. Všimněte si, že IT oddělení není možné toosend pouze změny jednotlivé tabulky, protože jinak by mohly být operace zasílány mimo pořadí. Nabízená provede řadu REST volání tooyour mobilní aplikace Azure back-end, který naopak upraví databázovém serveru.
* **Pro vyžádání obsahu**: vyžádané provádí na jednotlivých tabulek a lze přizpůsobit pomocí dotazu tooretrieve pouze podmnožinu dat serveru hello. Hello Azure Mobile klientskou sadu SDK a vkládat hello Výsledná data do místního úložiště hello.
* **Implicitní nabízených oznámení**: Pokud je u tabulku, která má čekající místní aktualizace spustit vyžádání, nejprve provede hello vyžádání `push()` v kontextu synchronizace hello. Tato nabízené pomáhá minimalizovat konflikty mezi změny, které jsou již zařazeny do fronty a nová data ze serveru hello.
* **Přírůstkové synchronizace**: hello první parametr toohello vyžádanou operaci je *název dotazu* používané jenom na hello klienta. Pokud použijete jinou hodnotu než null dotazu název, provede hello Azure Mobile SDK *přírůstkové synchronizace*. Pokaždé, když vyžádanou operaci vrátí celé sady výsledků, hello nejnovější `updatedAt` časové razítko z této sady výsledek je uložen v hello SDK místní systémové tabulky. Operace následné stažení načíst záznamy po této časové razítko.

  toouse přírůstkové synchronizace, server musí vracet smysluplný `updatedAt` hodnoty a musí taky podporovat řazení podle tohoto pole. Ale vzhledem k tomu, že hello SDK přidá svůj vlastní řazení hello updatedAt pole, nelze použít dotaz vyžádání obsahu, který má svou vlastní `orderBy` klauzule.

  Název dotazu Hello může být libovolný řetězec, který zvolíte, ale musí být jedinečný pro každý logický dotaz ve vaší aplikaci.
  Operace stažení různé, jinak hodnota může přepsat hello stejné časové razítko přírůstkové synchronizace a vaše dotazy může vrátit nesprávné výsledky.

  Pokud dotaz hello má parametr, jedním ze způsobů toocreate název jedinečný dotazu je hodnota parametru tooincorporate hello.
  Například filtrování na ID uživatele, název dotazu může být takto (v jazyku C#):

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Pokud chcete tooopt mimo přírůstkové synchronizace, předat `null` jako hello ID dotazu. V takovém případě budou načteny všechny záznamy při každém volání příliš`PullAsync`, která je potenciálně neefektivní.
* **Vymazání**: můžete vymazat obsah hello hello místní úložiště pomocí `IMobileServiceSyncTable.PurgeAsync`.
  Pokud máte zastaralá data v databázi hello klienta, nebo pokud chcete toodiscard všechny čekající změny může být nutné vyprazdňování.

  Vyprázdnění vymaže tabulku z místního úložiště hello. Pokud jsou operace čeká na synchronizaci s databází serveru hello, hello vyprázdnění vyvolá výjimku, pokud hello *Vynutit vyprázdnění* parametr je nastaven.

  Jako příklad zastaralá data v klientovi hello Předpokládejme v příkladu "seznam úkolů" hello, Device1 vrátí pouze položky, které nebyly dokončeny. Úkolu "Koupit mléka" je označena dokončena na serveru hello jiným zařízením. Device1 však má stále hello "Koupit mléka" todoitem v místním úložišti, protože ji je pouze stahování položky, které nejsou označeny dokončení. Vyprázdnění vymaže této zastaralé položky.

## <a name="next-steps"></a>Další kroky
* [iOS: zapnutí offline synchronizace]
* [Xamarin iOS: zapnutí offline synchronizace]
* [Xamarin Android: Zapnutí offline synchronizace]
* [univerzální platformu Windows: zapnutí offline synchronizace]

<!-- Links -->
[klient .NET SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Zapnutí offline synchronizace]: app-service-mobile-android-get-started-offline-data.md
[iOS: zapnutí offline synchronizace]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: zapnutí offline synchronizace]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Zapnutí offline synchronizace]: app-service-mobile-xamarin-android-get-started-offline-data.md
[univerzální platformu Windows: zapnutí offline synchronizace]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
