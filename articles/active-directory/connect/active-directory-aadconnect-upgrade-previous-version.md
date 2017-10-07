---
title: "Azure AD Connect: Upgrade z předchozí verze | Microsoft Docs"
description: "Vysvětluje hello různé metody tooupgrade toohello nejnovější verzi Azure Active Directory Connect, včetně místní upgrade a migrace dráha."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 31f084d8-2b89-478c-9079-76cf92e6618f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 57bd5b094654e4983cafa303b6f3daecadafb01c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-upgrade-from-a-previous-version-toohello-latest"></a>Azure AD Connect: Upgrade z předchozí verze toohello nejnovější
Toto téma popisuje hello různé metody, které můžete použít tooupgrade nejnovější verze vaší služby Azure Active Directory (Azure AD) připojit instalace toohello. Doporučujeme ponechat si sami aktuální s verzemi hello služby Azure AD Connect. Můžete také použít hello kroky v hello [kyvu migrace](#swing-migration) části při provedení podstatných změn v konfiguraci.

Pokud chcete, aby tooupgrade z nástroje DirSync, přečtěte si téma [Upgrade ze synchronizačního nástroje služby Azure AD (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md) místo.

Existuje několik různých strategií, které můžete použít tooupgrade Azure AD Connect.

| Metoda | Popis |
| --- | --- |
| [Automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) |Toto je hello nejsnazší pro zákazníky s rychlé instalace. |
| [Místní upgrade](#in-place-upgrade) |Pokud máte jeden server, můžete upgradovat hello instalace na místě na stejný server hello. |
| [Dráha migrace](#swing-migration) |Dva servery můžete připravit jeden ze serverů hello s novou verzí hello nebo konfigurace a změnit hello active server, až budete připraveni. |

Oprávnění informace najdete v tématu hello [oprávnění vyžadovaná pro upgrade](active-directory-aadconnect-accounts-permissions.md#upgrade).

> [!NOTE]
> Poté, co jste povolili vaší nové Azure AD Connect server toostart synchronizace změn tooAzure AD, můžete nesmí vrátit zpět toousing DirSync nebo Azure AD Sync. Přechod na starší verzi z toolegacy klienty Azure AD Connect, včetně nástroje DirSync a Azure AD Sync, není podporována a může vést tooissues například ztráty dat ve službě Azure AD.

## <a name="in-place-upgrade"></a>Místní upgrade
Místní upgrade funguje pro přechod z Azure AD Sync nebo Azure AD Connect. Nefunguje pro přesun z nástroje DirSync nebo řešení s Forefront Identity Manager (FIM) + konektoru služby Azure AD.

Tato metoda je upřednostňovaný, pokud máte jeden server a o méně než 100 000 objektů. Pokud se nějak změní toohello out-of-box synchronizační pravidla, úplný import a úplnou synchronizaci vzniknout po upgradu hello. Tato metoda zajišťuje, že tuto novou konfiguraci hello je použité tooall stávající objekty v systému hello. To běžet může trvat několik hodin v závislosti na hello počet objektů, které jsou v oboru hello synchronizační modul. je pozastavená Hello normální rozdílová synchronizace scheduler (který ve výchozím nastavení synchronizuje každých 30 minut), ale pokračuje v synchronizaci hesel. Můžete uvažovat o provádění hello místní upgrade během víkendu. Pokud neexistují žádné změny toohello out-of-box konfigurace verze hello nové Azure AD Connect, pak normální Rozdílový import synchronizační spustí místo.  
![Místní upgrade](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Pokud jste udělali změny toohello out-of-box synchronizační pravidla, pak tato pravidla jsou vráceny zpět toohello výchozí konfigurace při upgradu. toomake se, že bude konfiguraci nacházet mezi upgrady, ujistěte se, provést změny, jak se popisuje v [osvědčené postupy pro změnu výchozí konfigurace hello](active-directory-aadconnectsync-best-practices-changing-default-configuration.md).

Během upgradu na místě, může být zaváděné změny, které vyžadují toobe aktivity (včetně kroky úplný Import a úplnou synchronizaci) konkrétní synchronizace provést po dokončení upgradu. toodefer takových aktivit, najdete v toosection [jak toodefer úplné synchronizace po upgradu](#how-to-defer-full-synchronization-after-upgrade).

## <a name="swing-migration"></a>Postupná migrace
Pokud máte komplexní nasazení nebo mnoho objektů, může to být nepraktické toodo místní upgrade na hello systému za provozu. Pro některé zákazníky tento proces může trvat několik dní – a během této doby se zpracují žádné rozdílové změny. Tuto metodu můžete také použít, když plánujete konfiguraci tooyour toomake významné změny a chcete, aby tootry je si před jejich jste nabídnutých toohello cloudu.

Hello doporučené metody pro tyto scénáře je toouse dráha migrace. Je nutné (aspoň) dva servery – jednu aktivní a jeden pracovní server. aktivní server Hello (ukázka s plnou blue řádků v hello následující obrázek) zodpovídá za hello active produkční zatížení. Hello pracovní server (ukázka s přerušované čáry fialové) je připravená s novou verzí hello nebo konfigurace. Pokud je plně připraven, se provádí tento server aktivní. Hello předchozí active server, který teď má hello stará verze nebo konfigurace nainstalovaný, se provádí na testovacím serveru hello a upgradu.

dva servery Hello můžete použít různé verze. Například aktivní server hello plánování toodecommission můžete použít Azure AD Sync, a hello nový pracovní server můžete použít Azure AD Connect. Pokud používáte dráha migrace toodevelop hello novou konfiguraci, jeho toohave vhodné hello shodovat s verzemi na dva servery.  
![Pracovní server](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

> [!NOTE]
> Někteří zákazníci dávají přednost toohave tři nebo čtyři servery pro tento scénář. Pracovní server hello upgradován, nemáte záložní server pro [zotavení po havárii](active-directory-aadconnectsync-operations.md#disaster-recovery). Se tři nebo čtyři servery můžete připravit jednu sadu serverů primární nebo úsporného režimu s hello novou verzi, která zajišťuje, že existuje vždy pracovní server, který je připraven tootake přes.

Tyto kroky také fungovat toomove z Azure AD Sync nebo řešení s FIM + konektoru služby Azure AD. Tyto kroky nefungují pro nástroj DirSync, ale je hello stejnou dráha migrace metodu (také nazývané paralelní nasazení) s kroky pro nástroj DirSync v [Upgrade Azure Active Directory sync (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="use-a-swing-migration-tooupgrade"></a>Použít migraci tooupgrade dráha
1. Pokud používáte Azure AD Connect na serverech a plán tooonly zkontrolujte změnu konfigurace, ujistěte se, že aktivního serveru a testovacím serveru jsou obě pomocí hello stejnou verzi. Tím je snazší toocompare rozdíly později. Pokud provádíte upgrade ze služby Azure AD Sync, tyto servery mají různé verze. Pokud provádíte upgrade ze starší verze služby Azure AD Connect, je vhodné toostart s hello dva servery, které jsou pomocí hello stejnou verzi, ale není to nutné.
2. Pokud jste provedli vlastní konfiguraci a pracovní server nemá, postupujte podle kroků hello pod [přesunout vlastní konfiguraci z toohello aktivní server hello pracovní server](#move-custom-configuration-from-active-to-staging-server).
3. Pokud provádíte upgrade ze starší verze služby Azure AD Connect, upgradujte hello pracovní server toohello nejnovější verzi. Při přesouvání z Azure AD Sync, nainstalujte Azure AD Connect na testovacím serveru.
4. Umožní hello synchronizační modul spustit úplný import a úplnou synchronizaci na testovacím serveru.
5. Ověřte, že novou konfiguraci hello nebyla způsobit neočekávané změny pomocí hello kroků v části "Ověřit" v [ověřte, zda text hello konfigurace serveru](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server). Pokud něco není jako očekávané, správný, spusťte hello import a synchronizace a ověřte hello dat, dokud ho spokojeni, pomocí následujících kroků hello.
6. Přepnout hello pracovní server toobe hello aktivní server. Toto je poslední krok hello "Přepínač active server" [ověřte, zda text hello konfigurace serveru](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Pokud upgradujete Azure AD Connect, upgrade hello serveru, který je nyní v pracovní režimu toohello nejnovější verzi. Postupujte podle hello stejné kroky jako před tooget hello dat a konfigurace upgradovat. Pokud jste upgradovali z Azure AD Sync, teď můžete vypnout a vyřadit z provozu starý server.

### <a name="move-a-custom-configuration-from-hello-active-server-toohello-staging-server"></a>Přesunutí vlastní konfiguraci z pracovního serveru toohello hello aktivní server
Pokud jste provedli konfiguraci změny toohello aktivní server, je třeba toomake opravdu které hello stejné změny jsou použité toohello pracovní server. Přesunout toohelp s tím, můžete použít hello [nástroje Azure AD Connect konfigurace dokumentace](https://github.com/Microsoft/AADConnectConfigDocumenter).

Můžete přesunout hello vlastní synchronizační pravidla, že jste vytvořili pomocí prostředí PowerShell. Musíte použít jiné změny hello stejným způsobem jako na systémy a které nelze migrovat hello změny. Hello [konfigurace dokumentace](https://github.com/Microsoft/AADConnectConfigDocumenter) vám může pomoct porovnávání hello dvěma systémy toomake se, že jsou identické. Nástroj Hello může také pomoct při automatizaci hello se s postupem uvedeným v této části.

Budete potřebovat následující hello tooconfigure věcí hello stejný způsob na obou serverech:

* Připojení toohello stejné doménové struktury
* Všechny domény a filtrování organizační jednotky
* Hello stejné volitelné funkce, jako je například synchronizace hesel a zpětný zápis hesla

**Přesunout vlastní synchronizační pravidla**  
toomove vlastní synchronizační pravidla, hello následující:

1. Otevřete **editoru pravidel synchronizace** na váš aktivní server.
2. Vyberte vlastní pravidlo. Klikněte na tlačítko **exportovat**. Otevře okno Poznámkový blok. Uložte hello dočasný soubor s příponou PS1. Díky tomu skript prostředí PowerShell. Zkopírujte hello PS1 souboru toohello pracovní server.  
   ![Export pravidla synchronizace](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. Hello GUID konektoru se liší na hello pracovní server a musíte ji změnit. Spustit tooget hello identifikátor GUID, **editoru pravidel synchronizace**, vyberte jednu z pravidla out-of-box hello této představují hello stejné připojení systému a klikněte na tlačítko **exportovat**. Nahraďte hello GUID z hello pracovní server hello identifikátor GUID v souboru PS1.
4. V příkazovém řádku prostředí PowerShell spusťte soubor hello PS1. Tím se vytvoří pravidlo vlastní synchronizace hello na pracovní server hello.
5. Tento postup opakujte pro všechny vaše vlastní pravidla.

## <a name="how-toodefer-full-synchronization-after-upgrade"></a>Jak toodefer úplné synchronizace po upgradu
Během upgradu na místě, může být změny zavedl, které vyžadují toobe aktivity (včetně kroky úplný Import a úplnou synchronizaci) konkrétní synchronizace provést. Například vyžadovat změny schématu konektor **úplný import** krok a out-of-box změny pravidlo synchronizace vyžadují **úplné synchronizace** krok toobe proveden v ovlivněných konektory. Během upgradu, Azure AD Connect určí, jaké aktivity synchronizace se vyžadují a zaznamenává je jako *přepsání*. V hello následující synchronizační cyklus plánovače synchronizace hello převezme tato přepsání a spustí je. Jakmile přepsání úspěšně proveden, je odebrána.

Mohou nastat situace, kdy nechcete, aby tyto místní tootake přepsání okamžitě po provedení upgradu. Například máte mnoho synchronizované objekty a chcete tyto kroky toooccur synchronizace mimo pracovní dobu. tooremove tyto přepsání:

1. Během upgradu **zrušte zaškrtnutí políčka** hello možnost **po dokončení konfigurace spustit proces synchronizace hello**. To zakáže plánovače synchronizace hello a zabrání synchronizační cyklus probíhají automaticky předtím, než se odeberou hello přepsání.

   ![DisableFullSyncAfterUpgrade](./media/active-directory-aadconnect-upgrade-previous-version/disablefullsync01.png)

2. Po dokončení upgradu, spusťte následující rutinu toofind se přidaly které přepsání hello:`Get-ADSyncSchedulerConnectorOverride | fl`

   >[!NOTE]
   > přepsání Hello jsou specifické pro konektor. V hello následující příklad, kroky úplný Import a úplnou synchronizaci byly přidány tooboth hello místní konektor AD a Azure AD Connector.

   ![DisableFullSyncAfterUpgrade](./media/active-directory-aadconnect-upgrade-previous-version/disablefullsync02.png)

3. Poznamenejte si hello stávající přepsání, které byly přidány.
   
4. tooremove hello přepsání pro úplný import a úplnou synchronizaci v libovolné konektoru, spusťte následující rutinu hello:`Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid-of-ConnectorIdentifier> -FullImportRequired $false -FullSyncRequired $false`

   přepsání hello tooremove na všechny konektory, spusťte následující skript prostředí PowerShell hello:

   ```
   foreach ($connectorOverride in Get-ADSyncSchedulerConnectorOverride)
   {
       Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier $connectorOverride.ConnectorIdentifier.Guid -FullSyncRequired $false -FullImportRequired $false
   }
   ```

5. tooresume hello Plánovač a spouštění hello následující rutiny:`Set-ADSyncScheduler -SyncCycleEnabled $true`

   >[!IMPORTANT]
   > Mějte na paměti, postup synchronizace hello požadované tooexecute v nejdřívější usnadnění práce. Můžete buď ručně provést tyto kroky, pomocí hello Synchronization Service Manager nebo přidat zpět pomocí rutiny Set-ADSyncSchedulerConnectorOverride hello přepsání hello.

tooadd hello přepsání pro úplný import a úplnou synchronizaci v libovolné konektoru, spusťte následující rutinu hello:`Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid> -FullImportRequired $true -FullSyncRequired $true`

## <a name="next-steps"></a>Další kroky
Další informace o [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
