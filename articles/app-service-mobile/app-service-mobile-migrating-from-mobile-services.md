---
title: "aaaMigrate z mobilní služby tooan aplikace služby mobilní aplikace"
description: "Zjistěte, jak migrovat tooeasily vaši mobilní služby aplikace tooan aplikace služby mobilní aplikace"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 07507ea2-690f-4f79-8776-3375e2adeb9e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: glenga
ms.openlocfilehash: cd2e8d98595703389300b79da9bf51cdcefe7b40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="article-top"></a>Migrovat existující Azure Mobile Service tooAzure služby App Service
S hello [obecné dostupnosti služby Azure App Service], Azure Mobile Services lokalitami mohou být snadno migrovat na místě tootake výhod všech funkcí nástroje hello Azure App Service.  Tento dokument popisuje co tooexpect při migraci váš web z tooAzure mobilní služby Azure App Service.

## <a name="what-does-migration-do"></a>K čemu migrace slouží tooyour lokality
Migrace služby Azure Mobile změní služby Mobile do [Azure App Service] aplikace, aniž by to ovlivnilo hello kódu.  Vaše centra oznámení, SQL datové připojení, nastavení ověřování, naplánované úlohy a název domény zůstanou beze změny.  Mobilních klientů pomocí služby Azure Mobile toooperate normálně pokračovat.  Migrace restartuje služby po přenášená tooAzure služby App Service.

[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Proč by měl migrovat webový server
Microsoft doporučuje, který migrujete vaší služby Azure mobilní tootake výhod hello funkcí služby Azure App Service, včetně:

* Nové hostitelské funkce včetně [WebJobs] a [vlastních názvů domén].
* Připojení tooyour místních prostředků pomocí [VNet] kromě příliš[hybridní připojení].
* Monitorování a řešení potíží s New Relic nebo [Application Insights].
* Předdefinované DevOps nástrojů, včetně [přípravné sloty], vrácení zpět a v produkční testování.
* [Automatické škálování], Vyrovnávání zatížení a [monitorování výkonu].

Další informace o hello výhody služby Azure App Service naleznete v tématu hello [vs Mobile Services. Služby App Service] tématu.

## <a name="before-you-begin"></a>Než začnete
Před zahájením všechna hlavní práce na svém webu, měli zálohovat skripty mobilní služby a databáze SQL.

## <a name="migrating-site"></a>Migrace vaší lokality
Proces migrace Hello migruje všechny lokality v jedné oblasti Azure.

toomigrate vaší lokality:

1. Přihlaste se toohello [portálu Azure Classic].
2. Vyberte mobilní službu v oblasti hello chcete toomigrate.
3. Klikněte na tlačítko hello **migrovat tooApp služby** tlačítko.

   ![Hello migrací tlačítko][0]
4. Přečtěte si hello dialogové okno služby tooApp migrací.
5. Zadejte v poli hello hello název vaší mobilní služby.  Například pokud je název vaší domény contoso.azure mobile.net, zadejte *contoso* v poli hello.
6. Klikněte na tlačítko hello značek.

Monitorování stavu hello hello migrace v hello aktivity monitorování. Váš web je uveden jako *migrace* v hello portálu Azure Classic.

  ![Monitorování aktivity migrace][1]

Každý migrace může trvat od 3 minut too15 za mobilní služby se migruje.  Během migrace hello zůstává k dispozici vaší lokality.
Na konci hello hello procesu migrace se restartuje vaší lokality.  Hello lokality není k dispozici během procesu restartování hello, který mohou trvat několik sekund.

## <a name="finalizing-migration"></a>Dokončení migrace hello
Plánování tootest serveru z mobilního klienta v závěru hello hello procesu migrace.  Zkontrolujte, zda že je možné provádět všechny běžné akce klienta bez změny toohello mobilního klienta.  

### <a name="update-app-service-tier"></a>Vyberte odpovídající aplikační službu cenové úrovně
Máte větší flexibilitu v po dokončení migrace tooAzure služby App Service – ceny.

1. Přihlaste se toohello [portál Azure].
2. Vyberte **všechny prostředky** nebo **App Services** klikněte hello název migrované služby Mobile.
3. Otevře se okno nastavení Hello ve výchozím nastavení.
4. Klikněte na tlačítko **plán služby App Service** v nabídce nastavení hello.
5. Klikněte na tlačítko hello **cenová úroveň** dlaždici.
6. Klikněte na tlačítko hello dlaždice odpovídající tooyour požadavky a pak klikněte na **vyberte**.  Může být nutné tooClick **zobrazit všechny** toosee hello dostupné cenové úrovně.

Jako počáteční bod doporučujeme následující úrovně hello:

| Cenová úroveň mobilní služby | Cenová úroveň služby App Service |
|:--- |:--- |
| Free |F1 Free |
| Basic |B1 Basic |
| Standard |S1 Standard |

Není značnou flexibilitu při volbě hello právo cenovou úroveň pro vaši aplikaci.  Odkazovat příliš[App Service – ceny] úplné podrobnosti o cenách hello vaší nové aplikace služby.

> [!TIP]
> Hello aplikace služby standardní vrstva obsahuje funkce toomany přístupu, které bude pravděpodobně třeba toouse, včetně [přípravné sloty], automatické zálohování a automatické škálování.  Podívejte se na nové možnosti hello, když jste existuje!
>
>

### <a name="review-migration-scheduler-jobs"></a>Zkontrolujte hello migrovat Plánovač úloh
Plánovač úloh se nezobrazí dokud přibližně 30 minut po migraci.  Naplánované úlohy pokračovat toorun hello pozadí.
tooview naplánované úlohy po jsou viditelné znovu:

1. Přihlaste se toohello [portál Azure].
2. Vyberte **procházet >**, zadejte **plán** v hello *filtru* pole a pak vyberte **kolekce plánovače**.

Existují omezení počtu volné plánovače úloh k dispozici po migraci.  Zkontrolujte využití a hello [Azure Scheduler plánuje].

### <a name="configure-cors"></a>Konfigurace CORS v případě potřeby
Sdílení prostředků různého původu je technika tooallow webu tooaccess webového rozhraní API v jiné doméně.  Pokud používáte Azure Mobile Services přidružené web, musíte tooconfigure CORS jako součást migrace hello.  Pokud přistupujete k Azure Mobile Services výhradně z mobilních zařízení, potom CORS nemusí toobe nakonfigurovaný s výjimkou ve výjimečných případech.

Nastavení migrovaných CORS jsou dostupné jako hello **MS_CrossDomainWhitelist** nastavení aplikace.  toomigrate vaší lokality toohello zařízení CORS služby App Service:

1. Přihlaste se toohello [portál Azure].
2. Vyberte **všechny prostředky** nebo **App Services** klikněte hello název migrované služby Mobile.
3. Otevře se okno nastavení Hello ve výchozím nastavení.
4. Klikněte na tlačítko **CORS** v nabídce hello rozhraní API.
5. Zadejte žádné povolené zdroje hello pole, které k dispozici, a po každém z nich stiskněte Enter.
6. Jakmile seznam Povolené zdroje je správná, kliknutím na tlačítko Uložit hello.

> [!TIP]
> Jeden z hello výhody používání Azure App Service je, že můžete spustit web a mobilní služby na hello stejné lokalitě.  Další informace najdete v tématu hello [další kroky](#next-steps) části.
>
>

### <a name="download-publish-profile"></a>Stáhněte si nový profil publikování
profil publikování Hello vaší lokality se změní při migraci tooAzure služby App Service.  Pokud máte v úmyslu toopublish váš web z Visual Studia, potřebujete nový profil publikování.  toodownload hello nový profil publikování:

1. Přihlaste se toohello [portál Azure].
2. Vyberte **všechny prostředky** nebo **App Services** klikněte hello název migrované služby Mobile.
3. Klikněte na tlačítko **profilu publikování Get**.

soubor PublishSettings Hello je stažené tooyour počítač.  Obvykle se označuje jako *sitename*. PublishSettings.  Import hello publikovat nastavení do existujícího projektu:

1. Otevřete Visual Studio a projekt mobilní služby Azure.
2. Klikněte pravým tlačítkem na projekt v hello **Průzkumníku řešení** a vyberte **publikování...**
3. Klikněte na tlačítko **importu**
4. Klikněte na tlačítko **Procházet** a vyberte vaše stažený soubor nastavení publikování.  Klikněte na tlačítko **OK**.
5. Klikněte na tlačítko **ověřit připojení** tooensure hello publikovat pracovní nastavení.
6. Klikněte na tlačítko **publikovat** toopublish vaší lokality.

## <a name="working-with-your-site"></a>Práce s vaší lokality po migraci
Zahájení práce s vaší nové služby App Service v hello [portál Azure] po migraci.  Hello Následují některé poznámky na konkrétní operace, kterou jste použili tooperform v hello [portálu Azure Classic], společně s jejich ekvivalent služby App Service.

### <a name="publishing-your-site"></a>Stahování a publikování migrovaná lokalita
Váš web je k dispozici prostřednictvím git a ftp a můžete publikovat různé různé mechanismy, včetně WebDeploy, sady TFS, Mercurial, Githubu a FTP.  přihlašovací údaje nasazení Hello se migrují pomocí hello zbytek webu.  Pokud jste nenastavili přihlašovací údaje nasazení, nebo si je nepamatujete, můžete je obnovit:

1. Přihlaste se toohello [portál Azure].
2. Vyberte **všechny prostředky** nebo **App Services** klikněte hello název migrované služby Mobile.
3. Otevře se okno nastavení Hello ve výchozím nastavení.
4. Klikněte na tlačítko **přihlašovací údaje nasazení** v hello publikování nabídky.
5. Zadejte přihlašovací údaje nasazení nové hello v příslušných polích hello a potom klikněte na tlačítko Uložit hello.

Můžete použít tyto přihlašovací údaje tooclone hello lokality s gitem nebo nastavit automatické nasazení z Githubu, sady TFS nebo Mercurial.  Další informace najdete v tématu hello [dokumentaci k nasazení služby Azure App Service].

### <a name="appsettings"></a>Nastavení aplikace
Většina nastavení migrovaných mobilní služby jsou dostupné prostřednictvím nastavení aplikace.  Seznam nastavení aplikace hello můžete získat z hello [portál Azure].
tooview nebo změnit nastavení aplikace:

1. Přihlaste se toohello [portál Azure].
2. Vyberte **všechny prostředky** nebo **App Services** klikněte hello název migrované služby Mobile.
3. Otevře se okno nastavení Hello ve výchozím nastavení.
4. Klikněte na tlačítko **nastavení aplikace** v nabídce Obecné hello.
5. Posuňte se v oddílu toohello nastavení aplikace a najít nastavení vaší aplikace.
6. Klikněte na tlačítko hello hodnota hello aplikace nastavení tooedit hello hodnoty.  Klikněte na tlačítko **Uložit** toosave hello hodnotu.

Můžete aktualizovat více nastavení aplikace v hello stejnou dobu.

> [!TIP]
> Existují dvě nastavení aplikace s hello stejnou hodnotu.  Například, mohou se zobrazit *ApplicationKey* a *MS\_ApplicationKey*.  Aktualizovat nastavení obě aplikace hello stejnou dobu.
>
>

### <a name="authentication"></a>Ověřování
Všechna nastavení ověřování jsou k dispozici jako nastavení aplikace v migrovaná lokalita.  tooupdate nastavení ověřování, je nutné změnit nastavení příslušné aplikace.  Hello následující tabulka uvádí nastavení hello příslušné aplikace pro zprostředkovatele ověřování:

| Poskytovatel | ID klienta | Tajný klíč klienta | Další nastavení |
|:--- |:--- |:--- |:--- |
| Účet Microsoft |**MS\_MicrosoftClientID** |**MS\_MicrosoftClientSecret** |**MS\_MicrosoftPackageSID** |
| Facebook |**MS\_FacebookAppID** |**MS\_FacebookAppSecret** | |
| Twitter |**MS\_TwitterConsumerKey** |**MS\_TwitterConsumerSecret** | |
| Google |**MS\_GoogleClientID** |**MS\_GoogleClientSecret** | |
| Azure AD |**MS\_AadClientID** | |**MS\_AadTenants** |

Poznámka: **MS\_AadTenants** se ukládají jako textový soubor s oddělovači seznam domén klienta (hello pole "Klientům povoleno" hello portálu Mobile Services).

> [!WARNING]
> **Nepoužívejte mechanismy ověřování hello v nabídce nastavení hello**
>
> Aplikační služba Azure poskytuje samostatném "bez použití kódu" ověřování a autorizace systému pod hello *ověřování / autorizace* nabídky nastavení a hello (nepoužívané) *ověřování mobilní* Možnosti v části nabídky nastavení hello.  Tyto možnosti jsou kompatibilní s migrované mobilní služby Azure.  Můžete [upgradu lokality](app-service-mobile-net-upgrading-from-mobile-services.md) tootake výhod hello ověřování služby Azure App Service.
>
>

### <a name="easytables"></a>Data
Hello *Data* kartě v Mobile Services nahradila *snadno tabulky* v rámci hello portálu Azure.  tooaccess snadno tabulky:

1. Přihlaste se toohello [portál Azure].
2. Vyberte **všechny prostředky** nebo **App Services** klikněte hello název migrované služby Mobile.
3. Otevře se okno nastavení Hello ve výchozím nastavení.
4. Klikněte na tlačítko **snadno tabulky** v nabídce mobilních hello.

Můžete přidat tabulku kliknutím hello **přidat** tlačítko nebo přístup k vaší stávající tabulky kliknutím na název tabulky.  Existují různé operace, které můžete provést z tohoto okna, včetně:

* Změna oprávnění tabulky
* Úpravy provozní skripty hello
* Správa schématu tabulky hello
* Odstraňování hello tabulky
* Vymazání obsahu tabulky hello
* Odstranění konkrétní řádky tabulky hello

### <a name="easyapis"></a>ROZHRANÍ API
Hello *rozhraní API* kartě v Mobile Services nahradila *rozhraní API pro snadný* v rámci hello portálu Azure.  tooaccess snadno rozhraní API:

1. Přihlaste se toohello [portál Azure].
2. Vyberte **všechny prostředky** nebo **App Services** klikněte hello název migrované služby Mobile.
3. Otevře se okno nastavení Hello ve výchozím nastavení.
4. Klikněte na tlačítko **rozhraní API pro snadný** v nabídce mobilních hello.

Vaše migrované rozhraní API jsou již uveden v okně hello.  V tomto okně můžete také přidat rozhraní API.  toomanage konkrétní rozhraní API, klikněte na tlačítko hello rozhraní API.
V nové okně hello můžete upravit hello oprávnění a upravit hello skripty pro hello rozhraní API.

### <a name="on-demand-jobs"></a>Plánovač úloh
Všechny plánovače úloh jsou k dispozici prostřednictvím hello části kolekce úloh plánovače.  tooaccess plánovače úloh:

1. Přihlaste se toohello [portál Azure].
2. Vyberte **procházet >**, zadejte **plán** v hello *filtru* pole a pak vyberte **kolekce plánovače**.
3. Vyberte hello kolekce úloh pro svůj web.  Je název *sitename*-úlohy.
4. Klikněte na tlačítko **nastavení**.
5. Klikněte na tlačítko **Plánovač úloh** v části Správa.

Naplánované úlohy jsou uvedeny s četností hello, které zadáte před migrací.  Úlohy na vyžádání jsou zakázány.  toorun úlohu na vyžádání:

1. Vyberte hello úlohu chcete toorun.
2. V případě potřeby klikněte na tlačítko **povolit** tooenable hello úlohy.
3. Klikněte na tlačítko **nastavení**, pak **plán**.
4. Vyberte opakování **jednou**, pak klikněte na tlačítko **uložit**

Vaše úlohy na vyžádání se nacházejí v `App_Data/config/scripts/scheduler post-migration`.  Doporučujeme převést všechny úlohy na vyžádání příliš[WebJobs] nebo [funkce].  Zápis nových úloh plánovače jako [WebJobs] nebo [funkce].

### <a name="notification-hubs"></a>Centra oznámení
Mobile Services používá centra oznámení pro nabízená oznámení.  Hello následující nastavení aplikace jsou použité toolink hello tooyour centra oznámení mobilní služby po migraci:

| Nastavení aplikace | Popis |
|:--- |:--- |
| **MS\_PushEntityNamespace** |Hello Namespace centra oznámení |
| **MS\_NotificationHubName** |Hello název centra oznámení. |
| **MS\_NotificationHubConnectionString** |Hello připojovací řetězec centra oznámení |
| **MS\_parametr NamespaceName** |Alias pro MS_PushEntityNamespace |

Vaše centrum oznámení je spravován prostřednictvím hello [portál Azure].  Poznamenejte si název centra oznámení hello (můžete najít to pomocí nastavení aplikace hello):

1. Přihlaste se toohello [portál Azure].
2. Vyberte **Procházet**>, pak vyberte **centra oznámení**
3. Klikněte na název centra oznámení hello spojený s hello mobilní služby.

> [!NOTE]
> Pokud vaše Centrum oznámení je typu "Mixed", není viditelná.  "Smíšený" typ oznámení, že hubs využívat Notification Hubs a starší verze funkce Service Bus.  [Převést smíšený obory] než budete pokračovat.  Po dokončení převodu hello vaše Centrum oznámení se zobrazí v hello [portál Azure].
>
>

Další informace najdete v tématu hello [Notification Hubs] dokumentaci.

> [!TIP]
> Funkce správy centra oznámení v hello [portál Azure] jsou stále ve verzi preview.  Hello [portálu Azure Classic] zůstává k dispozici pro správu všech Notification Hubs.
>
>

### <a name="legacy-push"></a>Starší verze nabízené nastavení
Pokud jste nakonfigurovali nabízené na mobilní službu před hello Úvod na centra oznámení, že používáte *starší verze nabízené*.  Pokud používáte nabízenou a nevidíte centra oznámení uvedené v konfiguraci, pak je pravděpodobné, že používáte *starší verze nabízené*.  Tato funkce je migrována se všechny ostatní funkce.  Doporučujeme však upgradu tooNotification centra brzy po dokončení migrace hello.

V dočasné hello všechna nastavení starší verze nabízené hello (s výjimkou významné hello certifikátu služby APN hello) jsou k dispozici v nastavení aplikace.  Aktualizujte certifikát služby APN hello nahrazením hello odpovídající souboru v systému souborů hello.

### <a name="app-settings"></a>Další nastavení aplikace
následující nastavení další aplikace Hello jsou migrované z mobilní služby a k dispozici v části *nastavení* > *nastavení aplikace*:

| Nastavení aplikace | Popis |
|:--- |:--- |
| **MS\_MobileServiceName** |Hello název vaší aplikace |
| **MS\_MobileServiceDomainSuffix** |Předpona domény Hello. jednofaktorovému Azure mobile.net |
| **MS\_ApplicationKey** |Klíč vaší aplikace |
| **MS\_hlavního klíče.** |Hlavní klíč vaší aplikace |

klíč aplikace Hello a hlavní klíč jsou identické toohello aplikace klíče z původní mobilní služby.  Konkrétně je hello klíč aplikace odesílá mobilních klientů toovalidate jejich použití hello mobilní rozhraní API.

### <a name="cliequivalents"></a>Ekvivalenty příkazového řádku
Už můžete hello *azure mobilní* příkaz toomanage webu Azure Mobile Services.  Místo toho mnoho funkcí nahradil hello *azure lokality* příkaz.  Použijte následující tabulku toofind ekvivalenty pro běžné příkazy hello:

| *Azure Mobile* příkaz | Ekvivalentní *Azure Site* příkaz |
|:--- |:--- |
| mobilní umístění |Seznam umístění lokality |
| mobilní seznamu |seznam webů |
| mobilní zobrazit *název* |Zobrazit lokality *název* |
| mobilní restartování *název* |lokality restartování *název* |
| mobilní znovu ho zaveďte *název* |lokality nasazení znovu ho zaveďte *commitId* *název* |
| mobilní sady klíčů *název* *typ* *hodnota* |Odstranit lokality appsetting *klíč* *název* <br/> Přidání webu appsetting *klíč*=*hodnotu* *název* |
| Seznam mobilních konfigurace *název* |seznam webů appsetting *název* |
| mobilní konfigurační získat *název* *klíč* |lokality zobrazit appsetting *klíč* *název* |
| mobilní konfigurační sady *název* *klíč* |Odstranit lokality appsetting *klíč* *název* <br/> Přidání webu appsetting *klíč*=*hodnotu* *název* |
| Seznam mobilních domén *název* |seznam domén lokality *název* |
| Přidat mobilní domény *název* *domény* |Přidání domény lokality *domény* *název* |
| Odstranění mobilních domény *název* |odstranění webu domény *domény* *název* |
| Zobrazit mobilních škálování *název* |Zobrazit lokality *název* |
| Změna mobilní škálování *název* |režim škálování webu *režimu* *název* <br /> lokality škálování instancí *instance* *název* |
| Seznam mobilních appsetting *název* |seznam webů appsetting *název* |
| Přidat mobilní appsetting *název* *klíč* *hodnota* |Přidání webu appsetting *klíč*=*hodnotu* *název* |
| Odstranit mobilní appsetting *název* *klíč* |Odstranit lokality appsetting *klíč* *název* |
| Zobrazit mobilních appsetting *název* *klíč* |Odstranit lokality appsetting *klíč* *název* |

Aktualizujte ověřování nebo nabízená oznámení, že nastavení aktualizací hello příslušné nastavení aplikace.
Úpravy souborů a publikování webu přes protokol ftp nebo git.

### <a name="diagnostics"></a>Protokolování a diagnostiky
Protokolování diagnostiky vypnutá normálně ve službě Azure App Service.  protokolování diagnostiky tooenable:

1. Přihlaste se toohello [portál Azure].
2. Vyberte **všechny prostředky** nebo **App Services** klikněte hello název migrované služby Mobile.
3. Otevře se okno nastavení Hello ve výchozím nastavení.
4. Vyberte **diagnostické protokoly** v nabídce Funkce hello.
5. Klikněte na tlačítko **ON** pro následující protokoly hello: **protokolování aplikace (systém souborů)**, **podrobné chybové zprávy**, a **trasování chybných požadavků**
6. Klikněte na tlačítko **systém souborů** pro protokolování webového serveru
7. Klikněte na tlačítko **uložit**

tooview hello protokoly:

1. Přihlaste se toohello [portál Azure].
2. Vyberte **všechny prostředky** nebo **App Services** klikněte hello název migrované služby Mobile.
3. Klikněte na tlačítko hello **nástroje** tlačítko
4. Vyberte **datový proud protokolu** v nabídce dodržovat hello.

Protokoly se zobrazí v okně hello, jako jsou generovány.  Můžete také stáhnout hello protokoly pro pozdější analýzu pomocí svých přihlašovacích údajů nasazení. Další informace najdete v tématu hello [protokolování] dokumentaci.

## <a name="known-issues"></a>Známé problémy
### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Odstraňování klon migrovat aplikace mobilních způsobí, že výpadek lokality
Pokud jste klonovat migrované mobilní službě pomocí prostředí Azure PowerShell a pak odstraňte hello klonování, odeberou se hello položku DNS pro vaši službu produkční.  Váš web je už nebude přístupný z Internetu hello.  

Řešení: Pokud chcete tooclone váš web, to provést prostřednictvím portálu hello.

### <a name="changing-webconfig-does-not-work"></a>Změna souboru Web.config nefunguje
Pokud máte stránku ASP.NET, změní toohello `Web.config` souboru získat nebyly použity.  Hello Azure App Service vytvoří vhodný `Web.config` souboru během spuštění toosupport hello Mobile Services runtime.  Určitá nastavení (například vlastní hlavičky) můžete přepsat pomocí transformace souboru XML.  Vytvořte soubor v názvem `applicationHost.xdt` – tento soubor musí skončit ve hello `D:\home\site` v hello služby Azure.  Nahrát `applicationHost.xdt` soubor pomocí vlastní nasazení skriptu nebo přímo pomocí modulu Kudu.  Následující Hello ukazuje dokument příklad:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Další informace najdete v tématu hello [XDT transformace ukázky] dokumentaci na Githubu.

### <a name="migrated-mobile-services-cannot-be-added-tootraffic-manager"></a>Migrované Mobile Services nelze přidat tooTraffic Manager
Při vytváření profilu Traffic Manageru nelze přímo zvolte profil toohello migrované mobilní služby.  Použití "externí koncový bod."  Externí koncový bod lze přidat pouze pomocí prostředí PowerShell.  Další informace najdete v tématu [Traffic Manager kurzu](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Další kroky
Teď, když je aplikace migrované tooApp služby, existují i další funkce, které můžete použít:

* Nasazení [přípravné sloty] umožňují toostage změny tooyour lokality a provádět A / B testování.
* [WebJobs] zadejte náhradní server pro úlohy naplánované na vyžádání.
* Můžete [nepřetržitě nasazení] vaší lokality pomocí propojení vaší lokality tooGitHub, Mercurial nebo TFS.
* Můžete použít [Application Insights] toomonitor vaší lokality.
* Používat a web a mobilní API z hello stejný kód.

### <a name="upgrading-your-site"></a>Upgrade lokality tooAzure vaši mobilní služby Mobile Apps SDK
* Pro projekty serveru na základě Node.js hello nové [Mobile Apps Node.js SDK] poskytuje několik nových funkcí. Například teď můžete provést místní vývoj a ladění, použít libovolná verze Node.js výše 0.10 a přizpůsobit pomocí veškerý middleware Express.js.
* Pro. Projekty serveru na základě NET hello nové [balíčky NuGet sady SDK pro Mobile Apps](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) mít větší flexibilitu v NuGet závislosti.  Tyto balíčky podporu hello nové služby App Service ověřování a vytvořit s žádným projektem technologie ASP.NET. Další informace o upgradu naleznete v tématu [upgradovat stávající služba Mobile .NET tooApp služby](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[App Service – ceny]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Application Insights]: ../application-insights/app-insights-overview.md
[Automatické škálování]: ../app-service-web/web-sites-scale.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[dokumentaci k nasazení služby Azure App Service]: ../app-service-web/web-sites-deploy.md
[portálu Azure Classic]: https://manage.windowsazure.com
[portál Azure]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure Scheduler plánuje]: ../scheduler/scheduler-plans-billing.md
[nepřetržitě nasazení]: ../app-service-web/app-service-continuous-deployment.md
[Převést smíšený obory]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[vlastních názvů domén]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[obecné dostupnosti služby Azure App Service]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[hybridní připojení]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[protokolování]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Mobile Apps Node.js SDK]: https://github.com/azure/azure-mobile-apps-node
[vs Mobile Services. Služby App Service]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Notification Hubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[monitorování výkonu]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[přípravné sloty]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[XDT transformace ukázky]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[funkce]: ../azure-functions/functions-overview.md
