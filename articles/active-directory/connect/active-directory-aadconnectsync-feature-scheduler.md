---
title: 'Synchronizace Azure AD Connect: Scheduler | Microsoft Docs'
description: "Toto téma popisuje funkce integrované scheduler hello v synchronizaci Azure AD Connect."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b1a598f-89c0-4244-9b20-f4aaad5233cf
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: c587039cc68d305862a07beff364894b6f74cd2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-scheduler"></a>Synchronizace Azure AD Connect: plánovače
Toto téma popisuje hello předdefinované plánovače v synchronizaci Azure AD Connect (také známa jako synchronizační modul).

Tato funkce byla zavedená s sestavení 1.1.105.0 (vydané. února 2016).

## <a name="overview"></a>Přehled
Synchronizace Azure AD Connect synchronizovat změny, které se vašeho místního adresáře pomocí plánovače. Existují dva procesy scheduler, jednu pro synchronizaci hesel a druhou pro objekt nebo atribut úlohy synchronizace a údržby. Toto téma popisuje hello pozdější.

V dřívějších verzích se hello Plánovač pro objekty a atributy externí toohello synchronizační modul. Použije plánovače úloh systému Windows nebo samostatný hello synchronizace proces tootrigger služby systému Windows. Hello scheduler je s hello 1.1 verzích předdefinované toohello synchronizační modul a umožňují některé vlastní nastavení. Hello nový výchozí frekvence synchronizace je 30 minut.

Hello scheduler je zodpovědná za dvě úlohy:

* **Synchronizační cyklus**. Hello proces tooimport, synchronizaci a export změny.
* **Úlohy údržby**. Obnovení klíčů a certifikátů pro obnovení hesla a služby DRS (Device Registration). Vymazat staré položky v protokolu operations hello.

Hello scheduler samotné bude vždy spuštěn, ale může být nakonfigurované tooonly spustit jedna nebo žádná z těchto úloh. Například pokud potřebujete toohave vlastní proces synchronizace cyklus, můžete zakázat tato úloha v Plánovači hello, ale stále spuštění hello úlohy údržby.

## <a name="scheduler-configuration"></a>Konfiguraci plánovače
toosee aktuální nastavení konfigurace, přejděte tooPowerShell a spusťte `Get-ADSyncScheduler`. Zobrazuje něco jako tomto obrázku:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings2016.png)

Pokud se zobrazí **příkaz synchronizace hello nebo rutiny není k dispozici** při spuštění této rutiny, pak není načíst modul prostředí PowerShell hello. Tento problém se může stát, když spustíte Azure AD Connect na řadiči domény nebo na serveru s vyšší úrovně omezení prostředí PowerShell než hello výchozí nastavení. Pokud se zobrazí tato chyba, spusťte `Import-Module ADSync` toomake hello rutiny k dispozici.

* **AllowedSyncCycleInterval**. Hello nejkratší časový interval mezi cykly synchronizace povolený Azure AD. Nelze synchronizovat častěji, než toto nastavení a přesto být podporována.
* **CurrentlyEffectiveSyncCycleInterval**. Hello plán aktuálně v platnost. Má stejnou hodnotu jako CustomizedSyncInterval hello (Pokud nastavit) Pokud není častější, než AllowedSyncInterval. Pokud používáte sestavení před 1.1.281 a změníte CustomizedSyncCycleInterval, tato změna se projeví po příštím synchronizačním cyklu. Ze sestavení 1.1.281 hello změny se projeví okamžitě.
* **CustomizedSyncCycleInterval**. Pokud chcete hello Plánovač toorun frekvencí jakékoli jiné než výchozí hello 30 minut, nakonfigurujete toto nastavení. Na obrázku hello výše hello Plánovač byla nastavena toorun každou hodinu místo. Pokud jste nastavili tato nastavení tooa hodnota nižší než AllowedSyncInterval, se používá hello pozdější.
* **NextSyncCyclePolicyType**. Rozdílová nebo počáteční. Určuje, zda hello příštím spuštění by měl jenom rozdílové změny procesu, nebo pokud hello příštím spuštění musí používat úplnou import a synchronizaci. Hello pozdější by také znovu zpracovat všechna pravidla, nové nebo změněné.
* **NextSyncCycleStartTimeInUTC**. Při příštím spuštění hello scheduler hello příštím synchronizačním cyklu.
* **PurgeRunHistoryInterval**. Hello čas operace, které by měly být udržovány protokoly. Tyto protokoly mohou být zjišťovány hello synchronization service Manageru. Hello výchozí je tookeep tyto protokoly po dobu 7 dnů.
* **SyncCycleEnabled**. Označuje, pokud hello Plánovač běží export procesy hello import, synchronizaci a jako součást své činnosti.
* **MaintenanceEnabled**. Zobrazí, pokud je povoleno procesu údržby hello. Aktualizuje hello certifikáty nebo klíče a vyprazdňovat hello protokolu operací.
* **StagingModeEnabled**. Zobrazí-li [pracovním režimu](active-directory-aadconnectsync-operations.md#staging-mode) je povoleno. Pokud je toto nastavení povoleno, pak jej potlačí hello exportuje spuštění, ale pořád spustit import a synchronizaci.
* **SchedulerSuspended**. Connect nastavte během upgradu tootemporarily bloku hello scheduler spuštění.

Můžete změnit některé z těchto nastavení s `Set-ADSyncScheduler`. můžete upravit Hello následující parametry:

* CustomizedSyncCycleInterval
* NextSyncCyclePolicyType
* PurgeRunHistoryInterval
* SyncCycleEnabled
* MaintenanceEnabled

V dřívějších sestavení Azure AD Connect **isStagingModeEnabled** byl vystavený v ADSyncScheduler sady. Je **nepodporované** tooset tuto vlastnost. Hello vlastnost **SchedulerSuspended** by měl být upraven pouze Connect. Je **nepodporované** tooset to pomocí prostředí PowerShell přímo.

Konfigurace plánovače Hello je uložená ve službě Azure AD. Pokud máte na testovacím serveru, všechny změny na primárním serveru hello také ovlivní hello pracovní server (s výjimkou IsStagingModeEnabled).

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Syntaxe:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d – počet dnů, HH - hodiny, mm - minuty, ss - sekundy

Příklad:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Změny hello Plánovač toorun každé 3 hodiny.

Příklad:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Změny změnit hello Plánovač toorun denně.

### <a name="disable-hello-scheduler"></a>Zakázat hello plánovače  
Pokud potřebujete toomake změny konfigurace, pak budete chtít toodisable hello plánovače. Například, když jste [konfigurace filtrování](active-directory-aadconnectsync-configure-filtering.md) nebo [proveďte změny pravidel toosynchronization](active-directory-aadconnectsync-change-the-configuration.md).

scheduler hello toodisable, spusťte `Set-ADSyncScheduler -SyncCycleEnabled $false`.

![Zakázat hello plánovače](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)

Pokud jste udělali změny, nevynechali tooenable hello scheduler znovu s `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="start-hello-scheduler"></a>Spuštění hello plánovače
Hello scheduler je ve výchozím nastavení spouští každých 30 minut. V některých případech můžete chtít toorun cyklus synchronizace mezi hello plánované cyklů nebo potřebujete toorun jiného typu.

**Rozdílová synchronizace cyklu**  
Cyklus synchronizace delta zahrnuje hello následující kroky:

* Rozdílový import na všechny konektory
* Rozdílová synchronizace na všechny konektory
* Exportovat na všechny konektory

Může být, že máte naléhavé změny, která musí být synchronizovány okamžitě, proto musíte toomanually spustit cyklus. Pokud potřebujete toomanually spustit cyklus, pak z prostředí PowerShell spustit `Start-ADSyncSyncCycle -PolicyType Delta`.

**Cyklus úplné synchronizace**  
Pokud jste provedli jednu z hello následující změny konfigurace, je třeba toorun a cyklus úplné synchronizace (také známa jako Počáteční):

* Přidat další objekty nebo atributy toobe importovat ze zdrojového adresáře
* Provedené změny toohello synchronizační pravidla
* Změnit [filtrování](active-directory-aadconnectsync-configure-filtering.md) tak odlišný počet objektů, které by měly být zahrnuty

Pokud jste provedli jednu z těchto změn, musíte toorun úplnou synchronizaci cyklus, takže hello synchronizační modul má prostor konektoru hello možnost tooreconsolidate hello. Cyklus úplné synchronizace zahrnuje hello následující kroky:

* Úplný Import na všechny konektory
* Plná synchronizace v všechny konektory
* Exportovat na všechny konektory

Spustit tooinitiate a cyklus úplné synchronizace `Start-ADSyncSyncCycle -PolicyType Initial` z řádku prostředí PowerShell. Tento příkaz spustí cyklus úplné synchronizace.

## <a name="stop-hello-scheduler"></a>Zastavení plánovače hello
Pokud hello scheduler aktuálně běží synchronizační cyklus, bude pravděpodobně nutné toostop ho. Například pokud spustíte Průvodce instalací hello a se tato chyba:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

Když běží synchronizační cyklus, nemůžete změnit konfiguraci. Vám může Počkejte, dokud hello scheduler byl dokončen proces hello, ale můžete ho umožní vám provádět změny okamžitě také zastavit. Zastavení hello aktuální cyklus není škodlivé a změny čekající na zpracování se zpracují bez dalšího spuštění.

1. Spuštění pozastavením hello Plánovač toostop aktuálním cyklu pomocí rutiny prostředí PowerShell hello `Stop-ADSyncSyncCycle`.
2. Pokud používáte sestavení před 1.1.281, pak zastavování plánovače hello nezastaví hello aktuální konektor z jeho aktuální úlohy. tooforce hello toostop konektor, proveďte následující akce hello: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
   * Spustit **synchronizační služba** z nabídky start hello. Přejděte příliš**konektory**, zvýrazněte hello konektor stavem hello **systémem**a vyberte **Zastavit** z hello akce.

Hello scheduler je stále aktivní, začne znovu na nejbližší příležitosti.

## <a name="custom-scheduler"></a>Vlastní plánovače
Hello rutiny popsané v této části jsou k dispozici pouze v sestavení [1.1.130.0](active-directory-aadconnect-version-history.md#111300) a novější.

Pokud integrované scheduler hello nevyhovuje vašim požadavkům, můžete naplánovat hello konektorů pomocí prostředí PowerShell.

### <a name="invoke-adsyncrunprofile"></a>Vyvolání ADSyncRunProfile
Profil můžete spustit pro konektor tímto způsobem:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

Hello názvy toouse pro [názvy konektorů.](active-directory-aadconnectsync-service-manager-ui-connectors.md) a [spustit profil názvy](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) lze nalézt v hello [uživatelského rozhraní Správce služby synchronizace](active-directory-aadconnectsync-service-manager-ui.md).

![Vyvolání profil spuštění](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

Hello `Invoke-ADSyncRunProfile` rutina je synchronní, to znamená, nevrátí řízení až hello konektoru po dokončení operace hello úspěšně nebo se stala chyba.

Při plánování vaší konektory hello doporučení je tooschedule je v hello následující pořadí:

1. (Úplná nebo rozdílová) Importovat z místních adresářů, jako je Active Directory
2. (Úplná nebo rozdílová) Import ze služby Azure AD
3. (Úplná nebo rozdílová) Synchronizaci z místních adresářů, jako je Active Directory
4. (Úplná nebo rozdílová) Synchronizace z Azure AD
5. Export tooAzure AD
6. Export tooon místních adresářů, jako je Active Directory

Pořadí je, jak předdefinované scheduler hello spouští hello konektory.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
Můžete také sledovat hello synchronizační modul toosee, pokud je zaneprázdněn nebo nečinné. Tato rutina vrací prázdný výsledek, pokud hello synchronizační modul je nečinnosti a není spuštěn konektor. Pokud je spuštěn konektor, vrátí hello název hello konektor.

```
Get-ADSyncConnectorRunStatus
```

![Stav spuštění konektoru](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
Hello obrázku výše je první řádek hello z stavu, kdy je hello synchronizační modul nečinnosti. druhý řádek Hello z při spuštění hello konektoru služby Azure AD.

## <a name="scheduler-and-installation-wizard"></a>Průvodce Plánovač a instalace
Pokud spustíte Průvodce instalací hello, hello scheduler dočasně pozastaveno. Toto chování je vzhledem k tomu, že se předpokládá, provedete změny v konfiguraci a nelze ji použít tato nastavení, pokud aktivně hello synchronizační modul běží. Z tohoto důvodu nenechávejte hello Průvodce instalací otevřete vzhledem k tomu, že zastaví hello synchronizační modul provádět všechny akce synchronizace.

## <a name="next-steps"></a>Další kroky
Další informace o hello [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
