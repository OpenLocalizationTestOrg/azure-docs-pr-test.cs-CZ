---
title: "Synchronizace Azure AD Connect: technické koncepty | Microsoft Docs"
description: "Vysvětluje hello technické koncepty synchronizace Azure AD Connect."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 731cfeb3-beaf-4d02-aef4-b02a8f99fd11
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi;andkjell
ms.openlocfilehash: c6309bb9be462fb3d49c5b6ab302d4327ce4b7be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-technical-concepts"></a>Synchronizace služby Azure AD Connect: Technické koncepty
Tento článek je uveden seznam hello tématu [Principy architektura](active-directory-aadconnectsync-technical-concepts.md).

Synchronizace Azure AD Connect je založen na plnou metaadresáře synchronizace platformy.
Následující části Hello zavést hello koncepty metaadresáře synchronizace.
Sestavování na serveru MIIS, ILM a FIM, hello Azure Active Directory Sync Services poskytuje hello další platformu pro připojování toodata zdrojů, synchronizaci dat mezi zdroje dat, jakož i hello zajišťování a rušení zajištění identit.

![Technické koncepce](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

Hello následující části obsahují další podrobnosti o hello následující aspekty hello synchronizační služba FIM:

* konektor
* Toku atributů
* Prostoru konektoru
* Úložiště Metaverse
* Zřizování

## <a name="connector"></a>konektor
Hello kódové moduly, které jsou používané toocommunicate s připojený adresář se nazývají konektory (dříve označované jako agenti pro správu (MAs)).

Byly nainstalovány na počítači hello spuštěna synchronizace Azure AD Connect. Hello konektorů poskytuje hello bez agentů možnost tooconverse pomocí protokolů vzdáleného systému namísto spoléhání na nasazení hello specializované agentů. To znamená ke snížení rizika a dobu nasazení, zejména v případě, že zabývající se kritické aplikace a systémy.

Hello obrázku výše konektor hello je totožná s hello prostoru konektoru, ale zahrnuje veškerou komunikaci s hello externího systému.

Hello konektor zodpovídá za všechny import a export funkcí toohello systému a uvolní vývojáři z nutnosti toounderstand jak tooconnect tooeach systém nativně, při použití deklarativní zřizování transformace dat toocustomize.

Import a export pouze v případě naplánované, aby vám umožnil další izolaci od změny, ke kterým dochází v rámci systému hello, protože změny nejsou automaticky rozšířena toohello připojeného zdroje dat. Kromě toho mohou vývojáři vytvořit svoje vlastní konektory pro připojení toovirtually libovolný zdroj dat.

## <a name="attribute-flow"></a>Toku atributů
úložiště metaverse Hello je hello konsolidované zobrazení všechny připojené k identit od sousedního prostor konektoru. V hello předchozím obrázku je znázorněn toku atributů linkami se šipkami pro příchozí a odchozí tok. Tok atributů je hello proces kopírování nebo transformace dat z jednoho systému tooanother a všech atributů toků (příchozí nebo odchozí).

Tok atributů probíhá mezi hello prostoru konektoru a úložiště metaverse hello obousměrně když operací synchronizace (úplná nebo rozdílová) jsou naplánované toorun.

Tok atributů dochází pouze při spuštění těchto synchronizace. Toky atributů jsou definovány v synchronizační pravidla. To může být příchozí (ISR hello obrázku výše) nebo odchozí (OSR hello obrázku výše).

## <a name="connected-system"></a>Připojený systém
Připojený systém (neboli připojený adresář) odkazuje toohello vzdáleného systému Azure AD Connect sync připojil tooand čtení a zápis dat tooand identity z.

## <a name="connector-space"></a>Prostoru konektoru
Každý připojeného zdroje dat je reprezentován jako podmnožinu filtrované hello objektů a atributů v prostoru konektoru hello.
To umožňuje hello synchronizační služby toooperate místně bez hello nutné toocontact hello vzdáleného systému, při synchronizaci hello objekty a omezuje interakce tooimports a exportuje pouze.

Pokud hello zdroj dat a konektor hello tooprovide hello možnost vytvoření seznamu změn (Rozdílový import), pak zvyšuje provozní efektivitu hello výrazně jako cyklus pouze změny provedené od posledního cyklického dotazování hello se vyměňují. prostoru konektoru Hello insulates hello připojeného zdroje dat z šíření automaticky vyžadující, že tento plán hello konektor importuje a exportuje změny. Tato přidané pojištění uděluje jistotu při testování, zobrazení náhledu nebo potvrzení hello příští aktualizaci.

## <a name="metaverse"></a>Úložiště Metaverse
úložiště metaverse Hello je hello konsolidované zobrazení všechny připojené k identit od sousedního prostor konektoru.

Jako identity jsou spojeny dohromady a je mu přiřazená autority pro různé atributy prostřednictvím mapování toku importu, začne objektu úložiště metaverse centrální hello tooaggregate informace z více systémů. Mapování tohoto datového toku atributů objektu provádění informace toooutbound systémy.

Když je autoritativní systému projekty do úložiště metaverse hello jsou vytvořeny objekty. Jakmile se odeberou všechna připojení, je odstraněn objekt úložiště metaverse hello.

Objektů v hello metaverse nelze upravovat přímo. Všechna data v objektu hello musí podílí prostřednictvím toku atributů. úložiště metaverse Hello udržuje trvalé konektory s každou prostoru konektoru. Tyto konektory nevyžadují přehodnocení pro každou synchronizaci spustit. To znamená, že synchronizace Azure AD Connect nemá toolocate hello odpovídající vzdálený objekt pokaždé, když. Tím je zabráněno hello potřebu tooattributes nákladná agenty tooprevent změny, která by za normálních okolností zodpovědná za korelace hello objekty.

Při zjišťování nových zdrojů dat, které mohou mít dříve existující objekty, které potřebují spravovat, toobe používá synchronizaci Azure AD Connect proces volá připojení k pravidlo tooevaluate kandidáty potenciální s které tooestablish odkaz.
Po vytvoření odkazu hello výskytu toto testování a toku normální atributů může proběhnout mezi hello vzdálené připojeného zdroje dat a úložiště metaverse hello.

## <a name="provisioning"></a>Zřizování
Když autoritativní zdroj projekty nový objekt do úložiště metaverse hello nového objektu prostoru konektoru lze vytvořit v představující podřízené připojeného zdroje dat jiný konektor.

To vytváří ze své podstaty odkaz a obousměrně pokračovat toku atributů.

Vždy, když pravidlo zjistí, že nový objekt místa konektoru je toobe vytvořen, nazývá se zřizování. Ale protože tato operace je provedeno pouze v rámci hello prostoru konektoru, je neprovádí do hello připojeného zdroje dat do provedení exportu.

## <a name="additional-resources"></a>Další zdroje
* [Azure AD Connect Sync: Možnosti přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
