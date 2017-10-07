---
title: "Synchronizace Azure AD Connect: Změna účtu služby hello Azure AD Connect Sync | Microsoft Docs"
description: "Tento dokument téma popisuje hello šifrovací klíč a jak tooabandon po hello heslo je změnit."
services: active-directory
keywords: "Účtu synchronizační služby Azure AD, heslo"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 11948ac4662f722e4f684ef6c9b9ccdc6387e60f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="changing-hello-azure-ad-connect-sync-service-account-password"></a>Změna hesla účtu služby synchronizace Azure AD Connect hello
Pokud změníte heslo účtu služby synchronizace Azure AD Connect hello, hello synchronizační služba nebude možné spustit správně opuštění hello šifrovací klíč a heslo účtu služby synchronizace Azure AD Connect hello znovu inicializován. 

Azure AD Connect, jako součást hello synchronizační služby používá šifrování klíče toostore hello hesla hello služby AD DS a účty služby Azure AD.  Tyto účty jsou zašifrované, než jsou uloženy v databázi hello. 

Hello šifrovací klíč použít zabezpečené pomocí [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx). Rozhraní DPAPI chrání hello šifrovací klíč pomocí hello **heslo účtu služby sync hello Azure AD Connect**. 

Pokud potřebujete heslo účtu služby hello toochange můžete použít postupy hello v [Abandoning hello Azure AD Connect Sync šifrovací klíč](#abandoning-the-azure-ad-connect-sync-encryption-key) tooaccomplish to.  Tyto postupy by měla být používána Pokud z jakéhokoli důvodu potřebujete tooabandon hello šifrovací klíč.

##<a name="issues-that-arise-from-changing-hello-password"></a>Problémy, které vznikají z Změna hesla hello
Existují dvě věci, které je třeba provést, pokud změníte heslo účtu služby hello toobe.

Nejprve je třeba heslo hello toochange pod hello správce řízení služeb systému Windows.  Dokud nebude tento problém vyřešen. zobrazí se následující chyby:


- Pokud se pokusíte toostart hello synchronizační služba v modulu snap-in Správce řízení služeb systému Windows, zobrazí se chyba hello "**Windows nelze spustit službu Microsoft Azure AD Sync hello v místním počítači**". **Chyba. 1069: hello služby se nepodařilo spustit z důvodu tooa přihlášení se nezdařilo.** "
- V prohlížeči událostí systému Windows, protokolu událostí systému hello obsahuje chybu s **7038 ID události** a zpráva "**hello ADSync service byla nelze toolog na stejně jako u hello aktuálně nakonfigurované heslo z důvodu toohello následující chyba: Hello uživatelské jméno nebo heslo není správné.** "

Druhý pro určité podmínky, pokud se aktualizuje hello heslo, hello synchronizační služba už načíst hello šifrovací klíč pomocí rozhraní DPAPI. Bez hello šifrovací klíč hello synchronizační služby nelze dešifrovat hello hesla vyžaduje toosynchronize do a z místní AD a Azure AD.
Zobrazí se chyby, jako:

- V části správce řízení služeb systému Windows Pokud zkuste toostart hello synchronizační službu a nemůže získat hello šifrovací klíč, se nezdaří s chybou "** Windows se nepodařilo spustit hello Microsoft Azure AD Sync v místním počítači. Další informace najdete v tématu hello protokolu událostí systému. Pokud je služba jiného výrobce než Microsoftu, obraťte se na dodavatele služby hello a odkazovat kód chyby specifický tooservice **-21451857952 ***. "
- V prohlížeči událostí systému Windows, protokolu událostí aplikace hello obsahuje chybu s **6028 ID události** a chybovou zprávou *"**hello serveru šifrovací klíč je nepřístupný.* *"*

tooensure neobdrží tyto chyby, postupujte podle pokynů hello v [Abandoning hello Azure AD Connect Sync šifrovací klíč](#abandoning-the-azure-ad-connect-sync-encryption-key) při změně hesla hello.
 
## <a name="abandoning-hello-azure-ad-connect-sync-encryption-key"></a>Zrušení hello Azure AD Connect Sync šifrovací klíč
>[!IMPORTANT]
>Hello následující postupy platí pouze pro tooAzure AD Connect sestavení 1.1.443.0 nebo starší.

Použijte následující postupy tooabandon hello šifrovací klíč hello.

### <a name="what-toodo-if-you-need-tooabandon-hello-encryption-key"></a>Jaké toodo, pokud potřebujete tooabandon hello šifrovací klíč

Pokud potřebujete tooabandon hello šifrovací klíč, použijte následující postupy tooaccomplish hello.

1. [Zrušte hello existující šifrovací klíč](#abandon-the-existing-encryption-key)

2. [Zadejte heslo hello hello účet služby AD DS](#provide-the-password-of-the-ad-ds-account)

3. [Znovu inicializovat hello heslo hello účet synchronizační služby Azure AD](#reinitialize-the-password-of-the-azure-ad-sync-account)

4. [Spustit hello synchronizační služby](#start-the-synchronization-service)

#### <a name="abandon-hello-existing-encryption-key"></a>Zrušte hello existující šifrovací klíč
Zrušte hello existující šifrovací klíč, že nový šifrovací klíč lze vytvořit:

1. Přihlaste se tooyour připojení serveru služby Azure AD jako správce.

2. Spusťte novou relaci prostředí PowerShell.

3. Přejděte toofolder:`$env:Program Files\Microsoft Azure AD Sync\bin\`

4. Spusťte příkaz hello:`./miiskmu.exe /a`

![Azure AD Connect Sync šifrovací klíče nástroje](media/active-directory-aadconnectsync-encryption-key/key5.png)

#### <a name="provide-hello-password-of-hello-ad-ds-account"></a>Zadejte heslo hello hello účet služby AD DS
Jak hello stávající hesla uložená v databázi hello již nebude možné dešifrovat, musíte tooprovide hello synchronizační služba s hello heslo účtu hello služby AD DS. Hello synchronizační služba šifruje hello hesla pomocí hello nový šifrovací klíč:

1. Spusťte hello Synchronization Service Manager (START → synchronizace Service).
</br>![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)  
2. Přejděte toohello **konektory** kartě.
3. Vyberte hello **AD Connector.** odpovídající tooyour místní AD. Pokud máte více než jeden konektor AD, opakujte hello následující kroky pro každý z nich.
4. V části **akce**, vyberte **vlastnosti**.
5. V místním dialogovém okně hello, vyberte **připojit doménové struktury Directory tooActive**:
6. Zadejte heslo hello účtu hello služby AD DS v hello **heslo** textové pole. Pokud neznáte jeho heslo, je nutné ji nastavit tooa známé hodnoty před provedením tohoto kroku.
7. Klikněte na tlačítko **OK** toosave hello nové heslo a automaticky otevíraný dialog zavřít hello.
![Azure AD Connect Sync šifrovací klíče nástroje](media/active-directory-aadconnectsync-encryption-key/key6.png)

#### <a name="reinitialize-hello-password-of-hello-azure-ad-sync-account"></a>Znovu inicializovat hello heslo hello účet synchronizační služby Azure AD
Nemůžete přímo zadat heslo hello hello Azure AD služba účet toohello synchronizační služba. Místo toho musíte rutinu hello toouse **přidat ADSyncAADServiceAccount** tooreinitialize hello účet služby Azure AD. rutina Hello resetuje heslo účtu hello a je k dispozici toohello synchronizační služba:

1. Na server Azure AD Connect hello spusťte novou relaci prostředí PowerShell.
2. Spusťte rutinu `Add-ADSyncAADServiceAccount`.
3. V místním dialogovém okně hello zadejte přihlašovací údaje hello Azure AD globálního správce pro vašeho tenanta Azure AD.
![Azure AD Connect Sync šifrovací klíče nástroje](media/active-directory-aadconnectsync-encryption-key/key7.png)
4. Pokud neproběhne úspěšně, zobrazí se příkazový řádek Powershellu hello.

#### <a name="start-hello-synchronization-service"></a>Spustit hello synchronizační služby
Teď, když hello synchronizační služba má přístup toohello šifrovací klíč a všechny hello hesla je nutné, můžete restartovat službu hello v hello správce řízení služeb systému Windows:


1. Přejděte tooWindows správce řízení služeb (počáteční → služeb).
2. Vyberte **Microsoft Azure AD Sync** a klikněte na tlačítko Restartovat.

## <a name="next-steps"></a>Další kroky
**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)

* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
