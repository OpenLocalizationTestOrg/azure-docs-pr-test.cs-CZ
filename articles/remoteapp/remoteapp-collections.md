---
title: "druh aaaWhat kolekce potřebujete pro Azure RemoteApp? | Dokumentace Microsoftu"
description: "Informace o typech hello kolekcí, které jsou k dispozici s Azure Remoteappem."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: c13ec78d-07e9-4646-8194-cf3efafc1760
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: f00b5fe41af597cf75e26300bf7842c3a8ff94fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Jaký typ kolekce potřebujete pro Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Azure RemoteApp umožňuje sdílet aplikace a prostředky s uživateli na všech zařízeních. Můžeme provést vytvořením kolekce toohold hello aplikacím a prostředkům, a potom sdílet těchto kolekcí s uživateli. Existují 2 možnosti jinou kolekci, s jinou síť a možnosti ověřování – které je pro vás nejvhodnější?

Projděme různé aspekty hello a možnosti toomake tooget hello potřebujete nejvíce mimo kolekcí vzdálené aplikace Azure RemoteApp. 

## <a name="quick-differences-between-hello-collection-types"></a>Rychlé rozdíly mezi typy kolekcí hello
|  | Cloud | Hybridní |
| --- | --- | --- |
| Použít existující virtuální síť |Ano |Ano |
| Vyžaduje účty připojení AD (DirSync) |Ne |Ano |
| Vyžaduje připojení k doméně |Ne |Ano |
| Vyžaduje tooVNET dostupný řadič domény |Ne |Ano |

## <a name="cloud-collections"></a>Cloudové kolekce
* Rychlé toocreate - hello kolekce se rychle zřizuje, což znamená, že vaše aplikace získat toousers rychlejší.
* Zahrnout vlastní aplikace nebo náš sdílenou složkou. Můžete vytvořit vlastní image (vytvořen z virtuálního počítače Azure) nebo jedna z bitové kopie hello součástí vašeho předplatného.
* Nepotřebujete tooconfigure připojení mezi vaší kolekce a místní doméně.
* Ale můžete volitelně použít vlastní virtuální síť Azure tooprovide přístup do místního prostředí pro data sdílení nebo toouse ověřování jiný systém než Windows do prostředkům, jako SQL Server (s použitím ověřování databáze).

OK jak jednu vytvořit?

* Jenom cloud? Vytvořit s hello **rychle vytvořit** možnost hello portálu.
* Cloud + virtuální síť? Vytvořte pomocí hello **vytvořit s virtuální síť** možnost ale nevybírejte toojoin domény.

## <a name="hybrid-collections"></a>Hybridní kolekce
* Poskytnout úplný přístup tooon místní sítě a virtuální sítí Azure.
* Zahrnuje přístup připojení k doméně pro aplikace a data. Vzdálené aplikace můžete ověřování pro vaše místní službu Active Directory – můžete pak přístupu k prostředkům ve vaší doméně.
* Povolit rozšířené monitorování a správu s existující řešení System Center a zásady skupiny systému Windows (prostřednictvím vlastní image založený na Windows Server 2012 R2)
* Podpora [ExpressRoute](https://azure.microsoft.com/services/expressroute/) tooconnect vaší virtuální sítě Azure tooyour virtuální místní sítě.

Vytvořte pomocí hello **vytvořit s virtuální síť** možnost a zvolte toojoin doménu.

## <a name="authentication-options"></a>Možnosti ověřování
Azure RemoteApp podporuje účty Microsoft a účtů služby Azure Active Directory, ale ne všechny kolekce podporují všechny metody. 

| Typ účtu |  | Cloud | Cloud + virtuální sítě | Hybridní |
| --- | --- | --- | --- | --- |
| Účet Microsoft | |Ano |Ano |Ne |
| Azure Active Directory (Azure AD) | | | | |
| Jenom Azure AD |Ano |Ano |Ne | |
| AD Connect se synchronizací hesla |Ano |Ano |Ano | |
| Připojení AD bez synchronizace hesla |Ano |Ano |Ne | |
| AD Connect se službou AD FS |Ano |Ano |Ano | |
| poskytovatelů identit Azure podporován 3. stran (jako je Ping) |Ano |Ano |Ano | |
| Multi-factor Authentication | |Ano |Ano |Ano |

### <a name="cloud-and-cloud--vnet"></a>Cloud a cloudu + virtuální sítě
Cloudové kolekce můžete účty Microsoft, účty Azure AD nebo jejich kombinace hello dva. Použijte hello účty, které fungují nejvhodnější pro vaše uživatele.

Nejsou žádné zvláštní požadavky pro použití účtů Microsoft. 

Pokud chcete, aby toouse účty Azure AD, je třeba toomake ujistěte, zda váš klient Azure AD odpovídají hello jeden spojené s vaším předplatným. Když jste vytvořili vaše předplatné Azure Remoteappu, se automaticky spojeny s předplatným hello klienta Azure AD, které jste používali. Žádné uživatele Azure AD můžete udělit oprávnění tooneeds toobe téhož klienta. V případě potřeby můžete [změna klienta Azure AD hello](remoteapp-changetenant.md) spojené s vaším předplatným.

### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hybridní (nebo cloudu + Azure AD + AD)
Pomocí služby Azure AD a místní služby Active Directory je předpokladem pro hybridní kolekce. Je nutné toouse AD Connect toointegrate hello dva adresáře. Ale nutné některé volbou při přechodu do toohow konfigurace AD Connect. 

Existují 2 AD Connect scénáře - pomocí synchronizace hesel nebo pomocí AD federace. Podívejte se na hello [AD Connect informace](../active-directory/active-directory-aadconnect.md) toofigure, které z nich nejlépe vyhovuje.

Můžete také použít Azure AD + AD s cloudovou kolekcí. Ujistěte se, že budete postupovat podle hello stejné nastavení kroků.

Podívejte se na [Azure AD + požadavky služby Active Directory pro Azure RemoteApp](remoteapp-ad.md) hello kroky požadované tooconfigure Azure AD a služby Active Directory.

## <a name="go-create-your-collection"></a>Vytvoření kolekce můžete vše vyzkoušet.
OK podezření, že už víme jste, co ji nyní, takže není právě jednu věc levém toodo – vytvoření vaší první kolekce Azure Remoteappu.

[Vytvoření cloudové kolekce](remoteapp-create-cloud-deployment.md) nebo [vytvoření hybridní kolekce](remoteapp-create-hybrid-deployment.md) -právě získat vytváření.

