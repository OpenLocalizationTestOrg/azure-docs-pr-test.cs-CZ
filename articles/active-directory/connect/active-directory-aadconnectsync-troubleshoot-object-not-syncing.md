---
title: "objekt, který není synchronizace tooAzure AD aaaTroubleshoot | Microsoft Docs"
description: "Poradce při potížích se objekt nesynchronizuje tooAzure AD."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 81e0a0793a1d5ec76cfcaec6e974726d7854f58e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-an-object-that-is-not-synchronizing-tooazure-ad"></a>Řešení potíží s objekt, který není synchronizace tooAzure AD

Pokud není objekt synchronizace jako očekávané tooAzure AD, může být z několika důvodů. Pokud obdržíte e-mailu chyba z Azure AD nebo se zobrazí chyba hello v Azure AD Connect Health, přečtěte si [řešení chyb export](active-directory-aadconnect-troubleshoot-sync-errors.md) místo. Ale Pokud řešíte problém, kde hello objekt není ve službě Azure AD, pak toto téma je pro vás. Popisuje, jak synchronizovat toofind chyby v komponentě místní hello Azure AD Connect.

toofind hello chyby, kterou budete toolook v několika různých místech hello následující pořadí:

1. Hello [protokoly operací](#operations) pro vyhledání chyby identifikovaný hello synchronizační modul během importu a synchronizaci.
2. Hello [prostoru konektoru](#connector-space-object-properties) pro hledání chybějící objekty a chyby synchronizace.
3. Hello [úložiště metaverse](#metaverse-object-properties) pro vyhledání problémů souvisejících s daty.

Spustit [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md) předtím, než začnete provádět tyto kroky.

## <a name="operations"></a>Operace
Karta operace Hello v hello Synchronization Service Manager je, kde byste měli začít, vaše řešení potíží. Hello operations kartě se zobrazují hello výsledky z nejnovější operace hello.  
![Správce synchronizace služby](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/operations.png)  

horní polovině Hello zobrazuje všechny spustí chronické pořadí. Ve výchozím nastavení, protokol operations hello udržuje informace o hello posledních sedmi dnů, ale toto nastavení lze změnit pomocí hello [Plánovač](active-directory-aadconnectsync-feature-scheduler.md). Chcete toolook pro všechny spuštění, které nejsou zobrazeny stav úspěšného dokončení. Můžete změnit hello řazení kliknutím na záhlaví hello.

Hello **stav** sloupec je nejdůležitější informace hello a ukazuje hello nejzávažnějšího problém pro spuštění. Zde je stručný hello nejběžnější stavů v pořadí podle priority tooinvestigate (kde * znamenat několik řetězců možná chyba).

| Status | Komentář |
| --- | --- |
| ukončeno-* |Hello spuštění se nepodařilo dokončit. Například pokud hello vzdálený systém je vypnutý a nelze kontaktovat. |
| Zastavit limit chyb |Existuje více než 5 000 chyby. Hello spustit automaticky zastavil z důvodu toohello velký počet chyb. |
| dokončené -\*– chyby |Hello spustit dokončené, ale nejsou chyby (méně než 5 000), které by se měly prozkoumat. |
| dokončené -\*– upozornění |Hello spustit dokončilo, ale některá data se nenachází ve stavu hello očekává. Pokud máte chyby, pak tato zpráva je obvykle jenom příznakem. Dokud nebude mít řešit chyby, by neměl prozkoumat upozornění. |
| úspěch |Žádné problémy. |

Když vyberete řádek, aktualizuje hello dolní tooshow hello podrobnosti této spustit. toohello daleko pravé dolní hello, můžete mít tom, že seznam **krok #**. Tento seznam se zobrazí, jenom Pokud máte více domén v doménové struktuře každou doménu, kde je reprezentována krok. název domény Hello naleznete pod nadpisem hello **oddílu**. V části **Statistika synchronizace**, můžete najít další informace o hello počet změn, které byly zpracovány. Můžete kliknout na hello odkazy tooget seznam objektů hello změnit. Pokud máte objektů s chybami, tyto chyby, zobrazí na **chyby synchronizace**.

### <a name="troubleshoot-errors-in-operations-tab"></a>Řešení chyb v kartu operace
![Správce synchronizace služby](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/errorsync.png)  
Až budete mít chyby, jsou obě hello objekt v chyba a vlastní chyba hello odkazy, které poskytují další informace.

Spuštění kliknutím hello chybový řetězec (**synchronizační pravidlo Chyba – funkce aktivované** hello obrázku). Nejprve se zobrazí přehled hello objektu. toosee hello skutečné chybové, klikněte na tlačítko hello **trasování zásobníku**. Trasování poskytuje informace o úrovni ladění hello došlo k chybě.

Kliknete pravým tlačítkem na v hello **informace v zásobníku volání** vyberte **Vybrat vše**, a **kopie**. Můžete pak zkopírujte hello zásobníku a podívejte se na chyby hello ve svém oblíbeném editoru, například Poznámkový blok.

* Pokud chyba hello je ze **SyncRulesEngine**, pak informace zásobníku volání hello první má seznam všech atributů objektu hello. Posuňte se dolů, dokud neuvidíte hello záhlaví **ve vlastnosti InnerException = >**.  
  ![Správce synchronizace služby](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/errorinnerexception.png)  
  Hello řádek po ukazuje hello chyby. Hello obrázku výše chyba hello je z vlastní Fabrikam synchronizační pravidlo vytvořit.

Pokud vlastní chyba hello neposkytuje dostatek informací, je čas toolook v hello samotná data. Můžete kliknutím na odkaz hello s hello identifikátor objektu a pokračovat v řešení potíží s hello [konektoru místo importovaný objekt](#cs-import).

## <a name="connector-space-object-properties"></a>Vlastnosti objektu prostoru konektoru
Pokud nemáte žádné chyby nalezené v hello [operations](#operations) kartě, je dalším krokem hello toofollow hello konektoru místo objektu ze služby Active Directory, toohello úložiště metaverse a tooAzure AD. V této cestě by měl zjistit, kde je problém hello.

### <a name="search-for-an-object-in-hello-cs"></a>Vyhledejte objekt v hello CS

V **Synchronization Service Manager**, klikněte na tlačítko **konektory**, vyberte hello konektor služby Active Directory, a **hledání prostoru konektoru**.

V **oboru**, vyberte **relativního Rozlišujícího** (když chcete toosearch na atribut CN hello) nebo **rozlišující název nebo ukotvení** (když chcete toosearch na atribut distinguishedName hello). Zadejte hodnotu a klikněte na tlačítko **vyhledávání**.  
![Hledání prostoru konektoru](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssearch.png)  

Pokud nenajdete hello objekt hledáte, pak se může mít filtrované s [filtrování podle domén](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) nebo [založené na organizační jednotku filtrování](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering). Čtení hello [konfigurace filtrování](active-directory-aadconnectsync-configure-filtering.md) tooverify téma, které hello filtrování je nakonfigurovaný podle očekávání.

Další užitečné hledání tooselect hello konektoru služby Azure AD, je v **oboru** vyberte **čekajícího importu**a vyberte hello **přidat** zaškrtávací políčko. Toto hledání získáte všechny synchronizované objekty ve službě Azure AD, která nemůže být přidružena objektu místní.  
![Oddělena hledání prostoru konektoru](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssearchorphan.png)  
Tyto objekty byla vytvořena jinou synchronizační modul nebo synchronizační modul s jinou konfiguraci filtrování. Toto zobrazení je seznam **oddělena** už není spravované objekty. Zkontrolujte tento seznam a zvažte odebrání těchto objektů s použitím hello [Azure AD PowerShell](http://aka.ms/aadposh) rutiny.

### <a name="cs-import"></a>CS Import
Při otevření objekt cs existuje několik karet v horní části hello. Hello **importovat** kartě se zobrazují data hello, které je vynášené po importu.  
![CS objektu](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/csobject.png)    
Hello **stará hodnota** znázorňuje, co je aktuálně uloženy ve Connect a hello **nová hodnota** co byla přijata z hello zdrojovém systému a nebyla dosud nainstalována. Pokud dojde k chybě v objektu hello, nejsou zpracovány změny.

**Chyba**  
![CS objektu](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssyncerror.png)  
Hello **Chyba synchronizace** karta je zobrazena, pokud došlo k potížím s objektem hello pouze. Další informace najdete v tématu [řešení chyb synchronizace](#troubleshoot-errors-in-operations-tab).

### <a name="cs-lineage"></a>CS rodokmenu
Karta rodokmenu Hello ukazuje, jak objektu prostoru konektoru hello je související toohello objektu úložiště metaverse. Pokud hello konektor poslední importované liší od můžete zobrazit hello připojený systém a datových toopopulate pravidla používaná v hello úložiště metaverse.  
![CS rodokmenu](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cslineage.png)  
V hello **akce** sloupce, uvidíte je jedna **příchozí** synchronizační pravidlo s hello akce **zřídit**. Který označuje, že tak dlouho, dokud tento objekt místa konektoru je k dispozici, objektu úložiště metaverse hello zůstane. Pokud hello seznam pravidel synchronizace místo toho zobrazí synchronizační pravidlo s směr **odchozí** a **zřídit**, znamená to, že tento objekt se odstraní při odstranění objektu úložiště metaverse hello.  
![Správce synchronizace služby](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cslineageout.png)  
Můžete také zjistit v hello **PasswordSync** sloupec, který hello prostoru příchozí konektoru můžete přispívat změny toohello heslo, vzhledem k tomu, že jedno pravidlo synchronizace má hodnotu hello **True**. Toto heslo se pak odešlou tooAzure AD prostřednictvím hello odchozí pravidlo.

Na kartě rodokmenu hello, můžete získat toohello metaverse kliknutím [vlastnosti objektu úložiště Metaverse](#mv-attributes).

Na konci hello všechny karty jsou dvě tlačítka: **Preview** a **protokolu**.

### <a name="preview"></a>Preview
stránku s náhledem Hello je použité toosynchronize jeden jednoho objektu. Je vhodné, pokud jsou některá vlastní synchronizační pravidla pro řešení potíží a chcete toosee hello účinek změnu u jednoho objektu. Můžete vybrat mezi **úplné synchronizace** a **rozdílová synchronizace**. Můžete také vybrat mezi **generovat Preview**, který pouze udržuje hello změny v paměti, a **potvrdit Preview**, který aktualizovat hello úložiště metaverse a fáze všechny změny tootarget prostor konektoru.  
![Správce synchronizace služby](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/preview.png)  
Si můžete prohlédnout hello objektu a pravidlo, které se použijí pro tok konkrétní atribut.  
![Správce synchronizace služby](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/previewresult.png)

### <a name="log"></a>Protokol
Stránka protokolu Hello je použité toosee hello heslo synchronizace stav a historie. Další informace najdete v tématu [řešení synchronizace hesel](active-directory-aadconnectsync-troubleshoot-password-synchronization.md).

## <a name="metaverse-object-properties"></a>Vlastnosti objektu úložiště Metaverse
Je obvykle lepší toostart vyhledávání z hello zdroje služby Active Directory [prostoru konektoru](#connector-space). Ale můžete také spustit vyhledávání z úložiště metaverse hello.

### <a name="search-for-an-object-in-hello-mv"></a>Vyhledejte objekt v hello MV
V **Synchronization Service Manager**, klikněte na tlačítko **hledání úložiště Metaverse**. Vytvořte dotaz, že znáte najde hello uživatele. Můžete vyhledat běžné atributy, jako například název účtu (sAMAccountName) a userPrincipalName. Další informace najdete v tématu [hledání úložiště Metaverse](active-directory-aadconnectsync-service-manager-ui-mvsearch.md).
![Správce synchronizace služby](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvsearch.png)  

V hello **výsledky hledání** okně klikněte na objekt hello.

Pokud nebyla nalezena hello objektu, bylo nebyl ještě dosaženo hello metaverse. Pokračovat v hello služby Active Directory toosearch pro objekt hello [prostoru konektoru](#connector-space-object-properties). Může dojít k chybě synchronizace, která blokuje hello objekt z příchozí metaverse toohello nebo může být použit filtr.

### <a name="mv-attributes"></a>Atributy MV
Na kartě atributy hello uvidíte hello hodnoty a které konektor podílí ho.  
![Správce synchronizace služby](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvobject.png)  

Pokud není objekt synchronizace, podívejte se na následující atributy v úložišti metaverse hello hello:
- Atribut hello **cloudFiltered** k dispozici a nastavte příliš**true**? Pokud je, pak byly filtrovány, podle kroků toohello v [atributů na základě filtrování](active-directory-aadconnectsync-configure-filtering.md#attribute-based-filtering).
- Atribut hello **sourceAnchor** přítomen? Pokud ne, máte topologie doménové struktury prostředků účtu? Pokud se objekt identifikuje jako propojená poštovní schránka (hello atribut **msExchRecipientTypeDetails** má hodnotu hello 2), pak hello sourceAnchor je přispěli hello doménová struktura s aktivovaný účet služby Active Directory. Zajistěte, aby hello hlavní účet byl naimportovány a synchronizovány správně. Hello hlavní účet musí být uvedený v hello [konektory](#mv-connectors) hello objektu.

### <a name="mv-connectors"></a>Konektory MV
Hello konektorů kartě se zobrazují všechny prostor konektoru, které mají reprezentace objektu hello.  
![Správce synchronizace služby](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvconnectors.png)  
Měli byste konektor pro:

- Každý uživatel hello doménové struktury služby Active Directory je znázorněná. Tento zápis mohou zahrnovat foreignSecurityPrincipals a obraťte se na objekty.
- Konektor ve službě Azure AD.

Pokud chybí hello konektor tooAzure AD, přečtěte si [MV atributy](#MV-attributes) tooverify hello kritéria se zřídit tooAzure AD.

Na této kartě vám umožní toonavigate toohello [objektu prostoru konektoru](#connector-space-object-properties). Vyberte řádek a klikněte na tlačítko **vlastnosti**.

## <a name="next-steps"></a>Další kroky
Další informace o hello [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
