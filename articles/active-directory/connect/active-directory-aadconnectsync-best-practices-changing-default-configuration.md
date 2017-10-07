---
title: "Synchronizace Azure AD Connect: Změna výchozí konfigurace hello | Microsoft Docs"
description: "Obsahuje doporučené postupy pro změnu výchozí konfigurace hello synchronizace Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 7638a031-1635-4942-94c3-fce8f09eed5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: aa05e935edd02c49c3c3fdc198b854f50327847c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-best-practices-for-changing-hello-default-configuration"></a>Synchronizace Azure AD Connect: osvědčené postupy pro změnu výchozí konfigurace hello
účelem Hello tohoto tématu je toodescribe podporované a nepodporované změny tooAzure AD Connect sync.

Konfigurace Hello vytvořené Azure AD Connect funguje tak, jak je"pro většinu prostředí, které se synchronizují místní služby Active Directory s Azure AD. V některých případech je však nutné tooapply potřebovat tooa některé změny konfigurace toosatisfy konkrétní nebo požadavek.

## <a name="changes-toohello-service-account"></a>Účet služby toohello změny
Synchronizace Azure AD Connect je spuštěna pod účtem služby, který je vytvořený pomocí Průvodce instalací hello. Tento účet služby obsahuje hello šifrovací klíče toohello databáze používá synchronizaci. Bude vytvořen pomocí 127 znaků dlouhé heslo a hello heslo se nastavuje toonot vyprší.

* Je **nepodporované** toochange nebo resetování hesla hello hello účtu služby. Díky tomu zničí hello šifrovacích klíčů a hello služby není možné tooaccess hello databáze a není možné toostart.

## <a name="changes-toohello-scheduler"></a>Změny toohello plánovače
Počínaje verzí hello ze sestavení 1.1 (leden 2016) můžete nakonfigurovat hello [Plánovač](active-directory-aadconnectsync-feature-scheduler.md) toohave cyklus synchronizace jiný než výchozí hello 30 minut.

## <a name="changes-toosynchronization-rules"></a>Změny tooSynchronization pravidla
Průvodce instalací Hello nabízí konfiguraci, která by měla toowork pro hello nejběžnějších scénářů. V případě, že budete potřebovat toomake změny toohello konfigurace, potom postupujte podle těchto pravidel toostill mít na podporovanou konfiguraci.

* Můžete [změnit toky atributů](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) Pokud toky přímé atributů výchozí hello nejsou vhodné pro vaši organizaci.
* Pokud chcete příliš[není toku atribut](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) a odeberte všechny existující atribut hodnoty ve službě Azure AD, pak je nutné toocreate pravidlo pro tento scénář.
* [Zakázat pravidlo synchronizace nežádoucí](#disable-an-unwanted-sync-rule) místo odstranění. Během upgradu se znovu vytvoří pravidlo odstraněné.
* příliš[změnit pravidlo out-of-box](#change-an-out-of-box-rule), by měl vytvořit kopii hello původní pravidlo a zakázat pravidlo out-of-box hello. Hello Editor pravidla synchronizace vyzve a vám pomůže.
* Exportujte vaše vlastní synchronizační pravidla pomocí hello editoru pravidel synchronizace. Hello editor získáte pomocí skriptu prostředí PowerShell, můžete použít tooeasily je znovu vytvořit ve scénáři zotavení po havárii.

> [!WARNING]
> Hello out-of-box synchronizační pravidla mají kryptografický otisk. Pokud změníte toothese pravidel, je už odpovídající hello kryptografický otisk. V budoucích hello mohou mít problémy, když zkusíte tooapply novou verzi služby Azure AD Connect. Jenom zkontrolujte změny hello způsobem, který je popsaný v tomto článku.

### <a name="disable-an-unwanted-sync-rule"></a>Zakázat pravidlo nežádoucí synchronizace
Neodstraňujte pravidlo synchronizace out-of-box. Pak se znovu vytvoří při další upgradu.

V některých případech se Průvodce instalací hello vytvořeného konfigurace, která nepracuje v topologii. Například pokud máte doménovou strukturu prostředků účtu, ale máte v doménové struktuře účtu hello se schématem systému Exchange hello hello rozšířené schéma, pravidla pro Exchange vytvoří se pro hello účet doménové struktury a doménové struktury prostředku hello. V takovém případě musíte toodisable hello synchronizační pravidlo pro Exchange.

![Zakázané synchronizační pravidlo](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

Hello obrázku výše Průvodce instalací hello nalezl zadáno staré schéma Exchange 2003 v doménové struktuře účtu hello. Toto rozšíření schématu bylo přidáno před doménové struktury prostředku hello byla zavedená v prostředí společnosti Fabrikam. tooensure žádné atributy od původního implementace Exchange hello jsou synchronizovány, hello synchronizační pravidlo by mělo být zakázáno, jak je vidět.

### <a name="change-an-out-of-box-rule"></a>Změnit pravidlo out-of-box
Hello by se měl změnit pravidlo out-of-box je pouze pokud budete potřebovat toochange hello spojení pravidlo. Pokud potřebujete toochange tok atributů, měli vytvořit synchronizační pravidlo s vyšší prioritou než hello out-of-box pravidla. Hello jen pravidlo potřebujete prakticky tooclone je pravidlo hello **v ze služby Active Directory - připojení uživatele k**. Můžete přepsat všechna pravidla s pravidlem vyšší prioritu.

Pokud potřebujete toomake změny tooan out-of-box pravidlo, vytvořit kopii hello out-of-box pravidlo a zakázat pravidlo původní hello. Proveďte hello změny toohello klonovaného pravidle. Hello Editor pravidla synchronizace vám pomáhá s těchto kroků. Když otevřete pravidlo out-of-box, se zobrazí toto dialogové okno:  
![Upozornění z pravidla pole](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Vyberte **Ano** toocreate kopii hello pravidlo. Klonovaný pravidlo Hello je pak otevřít.  
![Klonovaný pravidlo](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Na tomto klonovaného pravidle proveďte všechny potřebné změny tooscope, spojení a transformace.

## <a name="next-steps"></a>Další kroky
**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
