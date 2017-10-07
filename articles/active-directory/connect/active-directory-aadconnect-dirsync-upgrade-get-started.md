---
title: "Azure AD Connect: Upgrade z nástroje DirSync | Dokumentace Microsoftu"
description: "Zjistěte, jak tooupgrade z nástroje DirSync tooAzure AD Connect. Tento článek popisuje hello kroky pro upgrade z nástroje DirSync tooAzure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: baf52da7-76a8-44c9-8e72-33245790001c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 05572af410698deaa1392c8837bfcb749efc69e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-upgrade-from-dirsync"></a>Azure AD Connect: Upgrade z nástroje DirSync
Azure AD Connect je tooDirSync následníka hello. Můžete najít hello způsoby, které můžete upgradovat z nástroje DirSync v tomto tématu. Pokud upgradujete z jiné verze služby Azure AD Connect nebo ze služby Azure AD Sync, tyto kroky nefungují.

Před zahájením instalace Azure AD Connect, ujistěte se, příliš[stáhnout Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) a dokončení hello nezbytné kroky [Azure AD Connect: Hardware a nezbytné předpoklady](active-directory-aadconnect-prerequisites.md). Konkrétně je vhodné tooread o následující hello, vzhledem k tomu, že se liší od DirSync těchto oblastech:

* Hello požadovaná verze rozhraní .net a prostředí PowerShell. Novější verze jsou požadované toobe na serveru hello než co potřeba nástroje DirSync.
* Hello konfiguraci proxy serveru. Pokud používáte proxy serveru tooreach hello internet, toto nastavení musí být nakonfigurované před upgradem. DirSync vždy použít hello proxy server nakonfigurovaný pro uživatele hello ho nainstalujete, ale nastavení počítače používá Azure AD Connect místo.
* toobe požadované adresy URL Hello otevřete v hello proxy serveru. Pro základní scénáře, tyto scénáře, podporuje taky DirSync, jsou požadavky hello hello stejné. Pokud chcete, toouse hello nové funkce zahrnuté do Azure AD Connect, musí být otevřeno některé nové adresy URL.

> [!NOTE]
> Jakmile povolíte vaší nové Azure AD Connect server toostart synchronizace změn tooAzure AD není nesmí vrátit zpět toousing DirSync nebo Azure AD Sync. Přechod na starší verzi z Azure AD Connect toolegacy klientů, včetně nástroje DirSync a Azure AD Sync se nepodporuje a může vést tooissues například ztráty dat ve službě Azure AD.

Pokud neupgradujete z nástroje DirSync, přečtěte si [související dokumentaci](#related-documentation) a použijte jiné scénáře.

## <a name="upgrade-from-dirsync"></a>Upgrade z nástroje DirSync
V závislosti na aktuálním nasazení nástroje DirSync existují různé možnosti pro hello upgrade. Pokud hello předpokládaná doba upgradu je menší než 3 hodiny, hello doporučení je toodo místní upgrade. Pokud hello předpokládaná doba upgradu je více než tří hodin, hello doporučení je toodo paralelní nasazení na jiném serveru. Odhaduje se, že pokud máte více než 50 000 objektů trvá více než tří hodin toodo hello upgradu.

| Scénář |
| --- | --- |
| [Místní upgrade](#in-place-upgrade) |
| [Paralelní nasazení](#parallel-deployment) |

> [!NOTE]
> Pokud plánujete tooupgrade z nástroje DirSync tooAzure AD Connect, nesnažte se DirSync odinstalovat sami před provedením upgradu hello. Azure AD Connect bude číst a migrovat hello konfigurace z nástroje DirSync a po kontrole hello serveru odinstalovat.

**Místní upgrade**  
Hello očekává, že čas toocomplete hello upgradu je zobrazena průvodcem hello. Tento odhad vychází hello předpokládá, že trvá tři hodiny toocomplete upgradu pro databázi s 50 000 objekty (uživatelé, kontakty a skupiny). Pokud hello počet objektů v databázi je menší než 50 000, Azure AD Connect doporučuje místní upgrade. Pokud se rozhodnete toocontinue, jsou při upgradu automaticky použita vaše aktuální nastavení a váš server automaticky obnoví aktivní synchronizace.

Pokud chcete toodo migraci konfigurace a provést paralelní nasazení, můžete přepsat hello místní upgrade doporučení. Může například trvat hello možnost toorefresh hello hardwaru a operačního systému. Další informace najdete v tématu hello [paralelní nasazení](#parallel-deployment) části.

**Paralelní nasazení**  
Pokud počet vašich objektů přesahuje 50 000, doporučuje se použít paralelní nasazení. Tímto nasazením se vyhnete různým provozním zdržením, ke kterým by mohlo u vašich uživatelů dojít. Hello instalace služby Azure AD Connect se pokusí tooestimate hello výpadek hello upgrade, ale pokud jste DirSync upgradovali v posledních hello, vlastní prostředí je pravděpodobně toobe hello nejlepší průvodce.

### <a name="supported-dirsync-configurations-toobe-upgraded"></a>Podporované konfigurace toobe DirSync upgradovat
Hello následující změny konfigurace jsou podporovány nástrojem DirSync upgradovaný:

* Filtrování domén a organizačních jednotek
* alternativní ID (UPN)
* synchronizace hesla a hybridní nastavení Exchange
* doménová struktura/doména a nastavení služby Azure AD
* filtrování podle atributů uživatele

Hello následující změnu nelze upgradovat. Pokud máte tuto konfiguraci, hello upgrade bude zablokován:

* nepodporované změny nástroje DirSync, např. odstraněné atributy nebo použití vlastní rozšířující knihovny DLL.

![Upgradování zablokováno](./media/active-directory-aadconnect-dirsync-upgrade-get-started/analysisblocked.png)

V takových případech je hello doporučení tooinstall nový server Azure AD Connect v [pracovním režimu](active-directory-aadconnectsync-operations.md#staging-mode) a ověřte hello starou konfiguraci nástroje DirSync a novou konfiguraci služby Azure AD Connect. Veškeré změny znovu použijte pomocí vlastní konfigurace, jak je popsáno v tématu [Vlastní konfigurace služby Azure AD Connect Sync](active-directory-aadconnectsync-whatis.md).

Hello hesla, která nástroj DirSync používá pro účty služby hello nelze načíst a se nemigrují. Tato hesla se obnoví během upgradu hello.

### <a name="high-level-steps-for-upgrading-from-dirsync-tooazure-ad-connect"></a>Hlavní kroky upgradu z nástroje DirSync tooAzure AD Connect
1. Vítejte tooAzure AD Connect
2. Analýza aktuální konfigurace nástroje DirSync
3. Získání hesla globálního správce služby Azure AD
4. Shromažďování přihlašovacích údajů pro účet správce podnikové sítě (použije se pouze během instalace hello služby Azure AD Connect)
5. Instalace služby Azure AD Connect
   * Odinstalace nástroje DirSync (nebo jeho dočasné vypnutí)
   * Instalace služby Azure AD Connect
   * Volitelné zahájení synchronizace

Další kroky jsou požadovány, pokud:

* Aktuálně používáte plnou instalaci systému SQL Server – místního nebo vzdáleného.
* V rozsahu synchronizace máte více než 50 tisíc objektů.

## <a name="in-place-upgrade"></a>Místní upgrade
1. Spusťte instalační program systému hello Azure AD Connect (MSI).
2. Zkontrolujte a odsouhlaste toolicense podmínky a Všimněte si, ochrany osobních údajů.  
   ![Vítejte tooAzure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Welcome.png)
3. Klikněte na tlačítko Další toobegin analýza stávající instalace nástroje DirSync.  
   ![Analýza stávající instalace nástroje Directory Sync](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Analyze.png)
4. Po dokončení analýzy hello o postupu naleznete doporučení hello tooproceed.  
   * Pokud používáte systém SQL Server Express a máte méně než 50 000 objektů, zobrazí se hello následující obrazovka:  
     ![Analýza dokončena, připraveno tooupgrade z nástroje DirSync](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReady.png)
   * Pokud pro nástroj DirSync používáte plnou instalaci SQL Serveru, zobrazí se místo toho tato stránka:  
     ![Analýza dokončena, připraveno tooupgrade z nástroje DirSync](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReadyFullSQL.png)  
     Zobrazí se Hello informace týkající se hello existující databázový server SQL že DirSync používá. V případě potřeby proveďte příslušné změny. Klikněte na tlačítko **Další** toocontinue hello instalace.
   * Pokud máte více než 50 000 objektů, zobrazí se místo toho tato obrazovka:  
     ![Analýza dokončena, připraveno tooupgrade z nástroje DirSync](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)  
     tooproceed v místním upgradu, klikněte na další zprávu toothis hello políčko: **pokračovat v upgradu nástroje DirSync na tomto počítači.**
     toodo [paralelní nasazení](#parallel-deployment) místo toho můžete exportovat nastavení konfigurace nástroje DirSync hello a přesunout hello konfigurace toohello nový server.
5. Zadejte heslo hello hello účet, který že aktuálně používáte tooconnect tooAzure AD. Toto musí být hello účet, který DirSync aktuálně používá.  
   ![Zadejte svoje přihlašovací údaje služby Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToAzureAD.png)  
   Pokud se zobrazí chyba a máte problémy s připojením, přečtěte si téma [Řešení problémů s připojením](active-directory-aadconnect-troubleshoot-connectivity.md).
6. Zadejte účet správce podnikové sítě pro Active Directory.  
   ![Zadejte svoje přihlašovací údaje ke službě AD DS.](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToADDS.png)
7. Nyní jste tooconfigure připraven. Pokud kliknete na tlačítko **Upgradovat**, odinstaluje se nástroj DirSync a nakonfiguruje se služba Azure AD Connect a zahájí synchronizaci.  
   ![Připraveno tooconfigure](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ReadyToConfigure.png)
8. Po dokončení instalace hello Odhlásit se a přihlaste znovu tooWindows. teprve pak použijte Synchronization Service Manager, Synchronization Rule Editor, případně toomake další změny v konfiguraci.

## <a name="parallel-deployment"></a>Paralelní nasazení
### <a name="export-hello-dirsync-configuration"></a>Export konfigurace nástroje DirSync hello
**Paralelní nasazení s více než 50 tisíci objekty**

Pokud máte více než 50 000 objektů a pak hello instalace služby Azure AD Connect doporučuje paralelní nasazení.

Zobrazí se na obrazovce podobné toohello následující:  
![Analýza dokončena](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)

Pokud chcete, aby tooproceed v paralelním nasazení, je třeba tooperform hello následující kroky:

* Klikněte na tlačítko hello **exportovat nastavení** tlačítko. Při instalaci Azure AD Connect na samostatný server, tato nastavení jsou migrovány z aktuální instalace nástroje DirSync tooyour nové Azure AD Connect.

Po dokončení exportu nastavení můžete ukončit průvodce Azure AD Connect hello na serveru nástroje DirSync hello. Pokračujte dalším krokem hello příliš[nainstalujte Azure AD Connect na samostatný server](#installation-of-azure-ad-connect-on-separate-server)

**Paralelní nasazení s méně než 50 tisíci objekty**

Pokud máte méně než 50 000 objektů, ale stále chcete toodo paralelní nasazení, pak hello následující:

1. Spusťte instalační program systému hello Azure AD Connect (MSI).
2. Až se zobrazí hello **úvodní tooAzure AD Connect** obrazovce ukončení Průvodce instalací hello kliknutím hello "X" v horním pravém rohu okna hello hello.
3. Otevřete příkazový řádek.
4. Hello k instalaci umístění služby Azure AD Connect (výchozí: C:\Program Files\Microsoft Azure Active Directory Connect) provést hello následující příkaz: `AzureADConnect.exe /ForceExport`.
5. Klikněte na tlačítko hello **exportovat nastavení** tlačítko. Při instalaci Azure AD Connect na samostatný server, tato nastavení jsou migrovány z aktuální instalace nástroje DirSync tooyour nové Azure AD Connect.

![Analýza dokončena](./media/active-directory-aadconnect-dirsync-upgrade-get-started/forceexport.png)

Po dokončení exportu nastavení můžete ukončit průvodce Azure AD Connect hello na serveru nástroje DirSync hello. Pokračujte dalším krokem hello příliš[nainstalujte Azure AD Connect na samostatný server](#installation-of-azure-ad-connect-on-separate-server).

### <a name="install-azure-ad-connect-on-separate-server"></a>Instalace služby Azure AD Connect na samostatný server
Při instalaci Azure AD Connect na nový server, hello předpokladem je, že chcete tooperform čistou instalaci Azure AD Connect. Vzhledem k tomu, že chcete konfiguraci nástroje DirSync hello toouse, existují některé tootake kroků navíc:

1. Spusťte instalační program systému hello Azure AD Connect (MSI).
2. Až se zobrazí hello **úvodní tooAzure AD Connect** obrazovce ukončení Průvodce instalací hello kliknutím hello "X" v horním pravém rohu okna hello hello.
3. Otevřete příkazový řádek.
4. Hello k instalaci umístění služby Azure AD Connect (výchozí: C:\Program Files\Microsoft Azure Active Directory Connect) provést hello následující příkaz: `AzureADConnect.exe /migrate`.
   Spustí se Průvodce instalace Hello Azure AD Connect a nabídne vám následující obrazovku hello:  
   ![Zadejte svoje přihlašovací údaje služby Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ImportSettings.png)
5. Vyberte hello soubor s nastavením exportovaný z instalace nástroje DirSync.
6. Nakonfigurujte rozšířené možnosti, například:
   * Vlastní umístění instalace pro Azure AD Connect.
   * Existující instance systému SQL Server (výchozí: Azure AD Connect nainstaluje systém SQL Server 2012 Express). Nepoužívejte hello stejnou instanci databáze jako server nástroje DirSync.
   * Účet služby použít tooconnect tooSQL serveru (Pokud je databáze systému SQL Server je vzdálený, pak tento účet musí být účtem doménové služby).
     Tyto možnosti se zobrazí na této obrazovce:  
     ![Zadejte svoje přihlašovací údaje služby Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/advancedsettings.png)
7. Klikněte na **Další**.
8. Na hello **připraven tooconfigure** ponechte hello **zahájit proces synchronizace hello ihned po dokončení konfigurace hello** zaškrtnutí. Hello server je nyní v [pracovním režimu](active-directory-aadconnectsync-operations.md#staging-mode) , změny nejsou exportovaný tooAzure AD.
9. Klikněte na **Nainstalovat**.
10. Po dokončení instalace hello Odhlásit se a přihlaste znovu tooWindows. teprve pak použijte Synchronization Service Manager, Synchronization Rule Editor, případně toomake další změny v konfiguraci.

> [!NOTE]
> Zahájí synchronizaci mezi Windows Server Active Directory a Azure Active Directory, ale žádné změny jsou exportovaný tooAzure AD. V jednu chvíli může změny aktivně exportovat pouze jeden synchronizační nástroj. Tento stav se nazývá [pracovní režim](active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-that-azure-ad-connect-is-ready-toobegin-synchronization"></a>Ověřte, že Azure AD Connect připravena toobegin synchronizace
tooverify, který je připraven tootake přes z nástroje DirSync Azure AD Connect, budete potřebovat tooopen **Synchronization Service Manager** ve skupině hello **Azure AD Connect** z nabídky start hello.

V aplikaci hello přejděte toohello **Operations** kartě. Na této kartě potvrďte, že hello následující operace dokončili:

* Importujte na hello AD Connector.
* Importujte na hello Azure AD Connector.
* Plná synchronizace v hello AD Connector.
* Plná synchronizace v hello Azure AD Connector.

![Import a synchronizace dokončeny](./media/active-directory-aadconnect-dirsync-upgrade-get-started/importsynccompleted.png)

Zkontrolujte výsledek hello z těchto operací a ujistěte se, že nejsou žádné chyby.

Pokud chcete toosee a zkontrolovat hello změny, které jsou o toobe exportovat tooAzure AD, přečtěte si jak tooverify hello konfigurace v [pracovním režimu](active-directory-aadconnectsync-operations.md#staging-mode). Proveďte požadované změny, pokud nenarazíte na nic neočekávaného.

Jste připravené tooswitch z nástroje DirSync tooAzure AD, když dokončíte tyto kroky a radostí s hello výsledek.

### <a name="uninstall-dirsync-old-server"></a>Odinstalace nástroje DirSync (starý server)
* V nabídce **Programy a funkce** vyhledejte **synchronizační nástroj služby Windows Azure Active Directory**.
* Odinstalujte **synchronizační nástroj služby Windows Azure Active Directory**.
* Hello odinstalace může trvat až toocomplete too15 minut.

Pokud dáváte přednost toouninstall DirSync později, můžete také dočasně vypnout hello server nebo zakázat službu hello. Pokud se něco pokazí, tato metoda vám umožní toore povolí ho. Však neočekává se, že tento další krok hello se nezdaří, proto by nemělo být potřeba.

S nástrojem DirSync odinstalovat nebo zakázat neexistuje žádný aktivní server exportující tooAzure AD. předtím, než změny v místní Active Directory bude pokračovat toobe synchronizované tooAzure AD se musí dokončit Hello další krok tooenable Azure AD Connect.

### <a name="enable-azure-ad-connect-new-server"></a>Povolení služby Azure AD Connect (nový server)
Po instalaci znovu Azure AD connect, budete umožňují toomake další změny konfigurace. Spustit **Azure AD Connect** z nabídky start hello nebo hello zástupce na ploše hello. Zajistěte, aby že nepokoušíte toorun instalaci hello MSI znovu.

Měli byste vidět hello následující:  
![Další úlohy](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AdditionalTasks.png)

* Vyberte **Konfigurovat pracovní režim**.
* Vypněte tak zaškrtnutí políčka hello pracovní **pracovní režim povoleno** zaškrtávací políčko.

![Zadejte svoje přihlašovací údaje služby Azure AD.](./media/active-directory-aadconnect-dirsync-upgrade-get-started/configurestaging.png)

* Klikněte na tlačítko hello **Další** tlačítko
* Na potvrzovací stránku hello, klikněte na tlačítko hello **nainstalovat** tlačítko.

Azure AD Connect je nyní váš aktivní server a nesmí přepnete zpět toousing existující server DirSync.

## <a name="next-steps"></a>Další kroky
Teď, když máte nainstalovanou službu Azure AD Connect můžete [ověřit hello instalaci a přiřadit licence](active-directory-aadconnect-whats-next.md).

Další informace o těchto nových funkcích, které byly povoleny v rámci instalace hello: [automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md), [prevence náhodného odstranění](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md), a [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Další informace o těchto běžných tématech: [Plánovač a jak synchronizaci tootrigger](active-directory-aadconnectsync-feature-scheduler.md).

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
