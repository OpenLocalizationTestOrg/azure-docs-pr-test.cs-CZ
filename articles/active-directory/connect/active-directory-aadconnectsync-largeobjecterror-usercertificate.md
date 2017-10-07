---
title: "Synchronizace Azure AD Connect: LargeObject zpracování chyb vzniklých při userCertificate atribut | Microsoft Docs"
description: "Toto téma obsahuje postup nápravy hello LargeObject chyby způsobené userCertificate atribut."
services: active-directory
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 146ad5b3-74d9-4a83-b9e8-0973a19828d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: e547fb5f601b92f6f5154de9997651b1f28c51b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-handling-largeobject-errors-caused-by-usercertificate-attribute"></a>Synchronizace Azure AD Connect: LargeObject zpracování chyb vzniklých při userCertificate atribut

Azure AD vynucuje maximální limit **15** certifikátu hodnot hello **userCertificate** atribut. Pokud Azure AD Connect exportuje objekt s více než 15 hodnoty tooAzure AD, Azure AD vrátí **LargeObject** chybová zpráva:

>*"hello zřízený objekt je příliš velký. Trim hello počet hodnot atributu na tento objekt. Hello operace bude opakována v hello příštím synchronizačním cyklu..."*

ostatní atributy AD může být způsobeno Hello LargeObject chyby. tooconfirm je skutečně způsobená hello userCertificate atributu, je nutné tooverify proti hello objekt buď v místní službě AD, nebo v hello [hledání úložiště Metaverse Synchronization Service Manager](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-mvsearch).

tooobtain hello seznam objektů ve vašem klientovi s chybami LargeObject, použijte jednu z následujících metod hello:

 * Pokud je váš klient Azure AD Connect Health pro synchronizaci, můžete se podívat toohello [synchronizace chybách](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-sync#object-level-synchronization-error-report-preview) zadat.
 
 * Hello e-mailové oznámení pro chyby synchronizace adresáře odeslaný na konci hello jednotlivých cyklů synchronizace má hello seznam objektů s chybami LargeObject. 
 * Hello [kartu Synchronization Service Manager operace](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-operations) hello seznam objektů s chybami LargeObject zobrazí, pokud kliknete na tlačítko hello nejnovější exportu tooAzure AD.
 
## <a name="mitigation-options"></a>Zmírnění dopadů možnosti
Až do vyřešení hello LargeObject chyba exportovat jiné toohello změny atributů, nemůže být stejný objekt tooAzure AD. Chyba hello tooresolve, můžete zvážit hello následující možnosti:

 * Upgrade Azure AD Connect toobuild 1.1.524.0 nebo po. Ve službě Azure AD Connect sestavení 1.1.524.0, hello out-of-box synchronizační pravidla byly aktualizované toonot export atributy userCertificate a userSMIMECertificate Pokud hello atributy mají více než 15 hodnoty. Podrobnosti o tom tooupgrade Azure AD Connect, naleznete v tooarticle [Azure AD Connect: Upgrade z předchozí verze toohello nejnovější](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

 * Implementace **pravidlo odchozí synchronizace** v Azure AD Connect, který exportuje **hodnota místo hello skutečnými hodnotami pro objekty s více než 15 certifikát hodnoty null**. Tato možnost je vhodná, pokud nechcete, aby žádný z hello certifikát hodnoty toobe exportovat tooAzure AD pro objekty s více než 15 hodnotami. Podrobnosti o tom tooimplement toto synchronizační pravidlo, najdete v části toonext [implementace synchronizační pravidlo toolimit export userCertificate atributu](#implementing-sync-rule-to-limit-export-of-usercertificate-attribute).

 * Snižte počet hello hodnot certifikát na hello místní objektu AD (maximálně 15) odebráním hodnoty, které jsou již používá vaše organizace. To je vhodné, pokud tomu atribut hello je způsobena certifikáty s vypršenou platností nebo se nepoužívá. Můžete použít hello [zde k dispozici skript prostředí PowerShell](https://gallery.technet.microsoft.com/Remove-Expired-Certificates-0517e34f) toohelp najít, zálohování a odstranit certifikáty s vypršenou platností v místní AD. Před odstraněním hello certifikáty, se doporučuje, abyste ověřili s hello infrastruktura veřejných klíčů správci ve vaší organizaci.

 * Konfigurovat Azure AD Connect tooexclude hello userCertificate atribut nebudou exportovány tooAzure AD. Obecně nedoporučujeme tuto možnost vzhledem k tomu může být použit atribut hello podle konkrétních scénářů tooenable služeb Microsoft Online Services. Konkrétně:
    * atribut userCertificate Hello na objekt uživatele hello slouží Exchange Online a klienty Outlook pro zprávu podepisování a šifrování. toolearn Další informace o této funkci naleznete tooarticle [S/MIME pro zprávu podepisování a šifrování](https://technet.microsoft.com/library/dn626158(v=exchg.150).aspx).

    * atribut userCertificate Hello u objektu počítače hello používá Azure AD tooallow Windows 10 místní zařízení připojená k doméně tooconnect tooAzure AD. toolearn Další informace o této funkci naleznete tooarticle [připojení zařízení připojených k doméně tooAzure AD pro Windows 10 vyskytne](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy).

## <a name="implementing-sync-rule-toolimit-export-of-usercertificate-attribute"></a>Implementace synchronizační pravidlo toolimit export userCertificate atributu
tooresolve hello LargeObject chyby způsobené hello userCertificate atribut, můžete implementovat pravidlo odchozí synchronizace v Azure AD Connect, který exportuje **hodnota místo hello skutečnými hodnotami pro objekty s více než 15 certifikát hodnotynull**. Tato část popisuje hello kroky požadované tooimplement hello synchronizační pravidlo pro **uživatele** objekty. Hello kroky lze upravit pro **kontaktujte** a **počítače** objekty.

> [!IMPORTANT]
> Export hodnotu null odebere certifikátu, hodnoty předtím exportovali úspěšně tooAzure AD.

Postup Hello je možné shrnout jako:

1. Zakažte plánovače synchronizace a ověřte, že neexistuje žádná synchronizace v průběhu.
3. Pro atribut userCertificate najít existující pravidlo odchozí synchronizace hello.
4. Vytvořte pravidlo odchozí synchronizace hello vyžaduje.
5. Ověřte hello nové pravidlo synchronizace na existující objekt s chybou LargeObject.
6. Použijte hello nové synchronizační pravidlo tooremaining objekty s chybou LargeObject.
7. Ověřte neexistují žádné neočekávané změny čekání toobe exportovat tooAzure AD.
8. Exportujte hello změny tooAzure AD.
9. Plánování pro synchronizační nástroj znovu povolte.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Krok 1. Zakažte plánovače synchronizace a ověřte, že neexistuje žádná synchronizace v průběhu
Ujistěte se, žádná synchronizace bude probíhat, když jste uprostřed hello implementace novou synchronizaci pravidlo tooavoid nezamýšleným změní vrácení exportovat tooAzure AD. plánovače předdefinované synchronizace toodisable hello:
1. Spusťte relaci prostředí PowerShell na server Azure AD Connect hello.

2. Zakážete plánované synchronizace spuštěním rutiny:`Set-ADSyncScheduler -SyncCycleEnabled $false`

> [!Note]
> Hello předchozí kroky jsou pouze použít toonewer verze (1.1.xxx.x) služby Azure AD Connect s plánovačem předdefinované hello. Pokud používáte starší verze (1.0.xxx.x) služby Azure AD Connect, který používá plánovače úloh systému Windows nebo používáte vlastní vlastní plánovače (ne běžné) tootrigger periodickou synchronizaci, je třeba toodisable je odpovídajícím způsobem.

3. Spustit hello **Synchronization Service Manager** podle přejdete → tooSTART synchronizační služba.

4. Přejděte toohello **operace** kartě a potvrďte neprobíhá žádná operace, jejichž stav je *"v průběhu."*

### <a name="step-2-find-hello-existing-outbound-sync-rule-for-usercertificate-attribute"></a>Krok 2. Pro atribut userCertificate najít hello existující pravidlo odchozí synchronizace
Měla by existovat existující synchronizační pravidlo, které je povolené a nakonfigurované tooexport userCertificate atribut pro uživatelské objekty tooAzure AD. Vyhledejte toto synchronizační pravidlo toofind se jeho **přednost** a **oboru filtru** konfigurace:

1. Spustit hello **editoru pravidel synchronizace** podle přejdete → tooSTART pravidla synchronizace Editor.

2. Konfigurovat filtry hledání hello hello následující hodnoty:

    | Atribut | Hodnota |
    | --- | --- |
    | Směr |**Odchozí** |
    | Typ objektu MV |**Osoba** |
    | konektor |*název vašeho konektoru služby Azure AD* |
    | Typ objektu konektoru |**uživatel** |
    | Atribut MV |**userCertificate** |

3. Pokud používáte OOB (out-of-box) synchronizační pravidla tooAzure AD connector tooexport userCertficiate atribut pro uživatelské objekty, měli byste obdržet hello *"se tooAAD – ExchangeOnline uživatele"* pravidlo.
4. Zapište hello **přednost** hodnotu toto synchronizační pravidlo.
5. Vyberte hello synchronizační pravidlo a klikněte na tlačítko **upravit**.
6. V hello *"Upravit vyhrazené pravidlo potvrzení"* automaticky otevíraný dialog, klikněte na tlačítko **ne**. (Nemusíte si dělat starosti, jsme nebudete toomake žádné pravidlo synchronizace toothis změn).
7. V úvodní obrazovka upravit, vyberte hello **Scoping filtru** kartě.
8. Poznamenejte si hello obor filtru konfigurace. Pokud používáte hello OOB synchronizační pravidlo, musí přesně být **jednoho oboru filtru skupina obsahující dvě klauzule**, včetně:

    | Atribut | Operátor | Hodnota |
    | --- | --- | --- |
    | sourceObjectType | ROVNÁ | Uživatel |
    | cloudMastered | NOTEQUAL | True |

### <a name="step-3-create-hello-outbound-sync-rule-required"></a>Krok 3. Vytvořit pravidlo odchozí synchronizace hello požadované
Hello nové pravidlo synchronizace musí mít hello stejné **oboru filtru** a **vyšší prioritu** než hello existující synchronizační pravidlo. Tím se zajistí, že tento hello nové synchronizační pravidlo vztahuje toohello stejné sady objektů jako hello existující synchronizační pravidlo a přepsání hello existující synchronizační pravidlo pro atribut userCertificate hello. Pravidlo synchronizace toocreate hello:
1. V editoru pravidel synchronizace hello, klikněte na tlačítko hello **přidat nové pravidlo** tlačítko.
2. V části hello **kartě Popis**, poskytovat hello následující konfigurace:

    | Atribut | Hodnota | Podrobnosti |
    | --- | --- | --- |
    | Name (Název) | *Zadejte název* | Například *"Se tooAAD – vlastní přepsání pro userCertificate"* |
    | Popis | *Zadejte popis* | Například *"Pokud userCertificate atribut má více než 15 hodnot, export hodnotu NULL."* |
    | Připojený systém | *Vyberte hello Azure AD Connector.* |
    | Typ objektu systému připojené | **uživatel** | |
    | Typ objektu úložiště Metaverse | **osoba** | |
    | Typ propojení | **Spojení** | |
    | Priorita | *Zvolili číslo mezi 1 a 99* | Hello číslo zvolený nesmí používat žádné existující pravidlo synchronizace a má nižší hodnotu (a tedy vyšší prioritu) než hello existující synchronizační pravidlo. |

3. Přejděte toohello **Scoping filtru** kartě a implementovat hello používá stejné oboru filtru hello existující synchronizační pravidlo.
4. Přeskočit hello **připojení pravidla** kartě.
5. Přejděte toohello **transformace** kartě tooadd nového transformaci pomocí následující konfigurace:

    | Atribut | Hodnota |
    | --- | --- |
    | Typ toku |**Výraz** |
    | Atribut target |**userCertificate** |
    | Zdrojový atribut |*Následující výraz hello použití*:`IIF(IsNullOrEmpty([userCertificate]), NULL, IIF((Count([userCertificate])> 15),AuthoritativeNull,[userCertificate]))` |
    
6. Klikněte na tlačítko hello **přidat** tlačítko toocreate hello synchronizační pravidlo.

### <a name="step-4-verify-hello-new-sync-rule-on-an-existing-object-with-largeobject-error"></a>Krok 4. Ověřte hello nové pravidlo synchronizace na existující objekt s chybou LargeObject
Toto je tooverify, který hello synchronizační pravidlo vytvořené pracuje správně na existující objekt AD s chybou LargeObject před jeho instalací tooother objekty:
1. Přejděte toohello **Operations** ve hello Synchronization Service Manager.
2. Vyberte hello nejnovější exportu tooAzure AD a klikněte na některý z objektů hello s chybami LargeObject.
3.  Na obrazovce automaticky otevírané okno hello vlastnosti objektu prostoru konektoru, klikněte na hello **Preview** tlačítko.
4. Na obrazovce automaticky otevírané okno hello Preview, vyberte **úplné synchronizace** a klikněte na tlačítko **potvrdit Preview**.
5. Zavřete úvodní obrazovka Preview a úvodní obrazovka vlastnosti objektu prostoru konektoru.
6. Přejděte toohello **konektory** ve hello Synchronization Service Manager.
7. Klikněte pravým tlačítkem na hello **Azure AD** konektoru a vyberte **spustit...**
8. V místní hello spustit konektor, vyberte **exportovat** krok a klikněte na tlačítko **OK**.
9. Počkejte Export tooAzure AD toocomplete a potvrďte, že se nezobrazí žádná chyba další LargeObject na tento konkrétní objekt.

### <a name="step-5-apply-hello-new-sync-rule-tooremaining-objects-with-largeobject-error"></a>Krok 5. Použít hello nové synchronizační pravidlo tooremaining objekty s chybou LargeObject
Po přidání hello synchronizační pravidlo, je třeba toorun kroku úplná synchronizace do hello konektoru AD:
1. Přejděte toohello **konektory** ve hello Synchronization Service Manager.
2. Klikněte pravým tlačítkem na hello **AD** konektoru a vyberte **spustit...**
3. V místní hello spustit konektor, vyberte **úplná synchronizace** krok a klikněte na tlačítko **OK**.
4. Počkejte toocomplete krok hello úplnou synchronizaci.
5. Hello výše kroky opakujte pro hello zbývající AD konektory, pokud máte více než jednu AD konektory. Obvykle se vyžadují více konektorů, pokud máte více místních adresářů.

### <a name="step-6-verify-there-are-no-unexpected-changes-waiting-toobe-exported-tooazure-ad"></a>Krok 6. Ověřte neexistují žádné neočekávané změny čekání toobe exportovat tooAzure AD
1. Přejděte toohello **konektory** ve hello Synchronization Service Manager.
2. Klikněte pravým tlačítkem na hello **Azure AD** konektoru a vyberte **hledání prostoru konektoru**.
3. V místní nabídce hledání prostoru konektoru hello:
    1. Nastavit obor příliš**čekající exportovat**.
    2. Zkontrolujte všechny 3 zaškrtávací políčka, včetně **přidat**, **upravit** a **odstranit**.
    3. Klikněte na tlačítko **vyhledávání** tlačítko tooreturn všechny objekty s změny čekání toobe exportovat tooAzure AD.
    4. Ověřte, že neexistují žádné neočekávané změny. změny hello tooexamine pro daný objekt, dvakrát klikněte na objekt hello.

### <a name="step-7-export-hello-changes-tooazure-ad"></a>Krok 7. Export hello změny tooAzure AD
tooexport hello změny tooAzure AD:
1. Přejděte toohello **konektory** ve hello Synchronization Service Manager.
2. Klikněte pravým tlačítkem na hello **Azure AD** konektoru a vyberte **spustit...**
4. V místní hello spustit konektor, vyberte **exportovat** krok a klikněte na tlačítko **OK**.
5. Počkejte Export tooAzure AD toocomplete a potvrďte, že nejsou žádné další chyby LargeObject.

### <a name="step-8-re-enable-sync-scheduler"></a>Krok 8. Opětovné povolení plánovače synchronizace
Teď, když je hello problém vyřešen, znovu povolte plánovače předdefinované synchronizace hello:
1. Spusťte relaci prostředí PowerShell.
2. Plánované synchronizace znovu povolte spuštěním rutiny:`Set-ADSyncScheduler -SyncCycleEnabled $true`

> [!Note]
> Hello předchozí kroky jsou pouze použít toonewer verze (1.1.xxx.x) služby Azure AD Connect s plánovačem předdefinované hello. Pokud používáte starší verze (1.0.xxx.x) služby Azure AD Connect, který používá plánovače úloh systému Windows nebo používáte vlastní vlastní plánovače (ne běžné) tootrigger periodickou synchronizaci, je třeba toodisable je odpovídajícím způsobem.

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

