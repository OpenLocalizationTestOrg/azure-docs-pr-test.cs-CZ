---
title: "Jaký typ kolekce potřebujete pro Azure RemoteApp? | Dokumentace Microsoftu"
description: "Další informace o typech kolekcí, které jsou k dispozici s Azure Remoteappem."
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
ms.openlocfilehash: 10f6c0533027767b6635ebff1e6a9872bde06a68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Jaký typ kolekce potřebujete pro Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Azure RemoteApp umožňuje sdílet aplikace a prostředky s uživateli na všech zařízeních. Můžeme provést vytvořením kolekce k uchování aplikacím a prostředkům, a potom sdílet těchto kolekcí s uživateli. Existují 2 možnosti jinou kolekci, s jinou síť a možnosti ověřování – které je pro vás nejvhodnější?

Projděme různé aspekty a možnosti, které je potřeba udělat k plnému využití z vaší kolekce Azure Remoteappu. 

## <a name="quick-differences-between-the-collection-types"></a>Rychlé rozdíly mezi typy kolekcí
|  | Cloud | Hybridní |
| --- | --- | --- |
| Použít existující virtuální síť |Ano |Ano |
| Vyžaduje účty připojení AD (DirSync) |Ne |Ano |
| Vyžaduje připojení k doméně |Ne |Ano |
| Vyžaduje řadič domény dostupný pro virtuální síť |Ne |Ano |

## <a name="cloud-collections"></a>Cloudové kolekce
* Rychlé vytvoření - je kolekce rychle zřízený, což znamená, vaše aplikace získat uživatelům rychlejší.
* Zahrnout vlastní aplikace nebo náš sdílenou složkou. Můžete vytvořit vlastní image (vytvořen z virtuálního počítače Azure) nebo jednu z imagí, které jsou součástí vašeho předplatného.
* Nemusíte nakonfigurovat připojení mezi vaší kolekce a místní doméně.
* Ale můžete volitelně použít vlastní virtuální sítě Azure k poskytování přístupu do místního prostředí pro sdílení dat, nebo chcete použít jiný systém než Windows ověřování do prostředkům, jako SQL Server (s použitím ověřování databáze).

OK jak jednu vytvořit?

* Jenom cloud? Vytvoření pomocí **rychle vytvořit** možnost na portálu.
* Cloud + virtuální síť? Vytvořte pomocí **vytvořit s virtuální síť** možnost, ale chcete připojit k doméně.

## <a name="hybrid-collections"></a>Hybridní kolekce
* Poskytnutí plného přístupu k místní síti + virtuální sítí Azure.
* Zahrnuje přístup připojení k doméně pro aplikace a data. Vzdálené aplikace můžete ověřování pro vaše místní službu Active Directory – můžete pak přístupu k prostředkům ve vaší doméně.
* Povolit rozšířené monitorování a správu s existující řešení System Center a zásady skupiny systému Windows (prostřednictvím vlastní image založený na Windows Server 2012 R2)
* Podpora [ExpressRoute](https://azure.microsoft.com/services/expressroute/) připojení virtuální sítě Azure k místní virtuální síť.

Vytvořte pomocí **vytvořit s virtuální síť** možnost a vyberte pro připojení k doméně.

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
Cloudové kolekce můžete účty Microsoft, účty Azure AD nebo kombinaci těchto dvou. Použijte účty, které fungují nejvhodnější pro vaše uživatele.

Nejsou žádné zvláštní požadavky pro použití účtů Microsoft. 

Pokud chcete použít účty Azure AD, budete muset Ujistěte se, že klientovi Azure AD odpovídá názvu, který je spojen s vaším předplatným. Když jste vytvořili vaše předplatné Azure Remoteappu, klient Azure AD, které jste používali se automaticky spojeny s předplatným. Žádné uživatele Azure AD, které můžete udělit oprávnění k musí být téhož klienta. V případě potřeby můžete [změna klienta Azure AD](remoteapp-changetenant.md) spojené s vaším předplatným.

### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hybridní (nebo cloudu + Azure AD + AD)
Pomocí služby Azure AD a místní služby Active Directory je předpokladem pro hybridní kolekce. Budete muset použít AD Connect k integraci dva adresáře. Ale nutné některé volbou při rozhodování o tom, jak nakonfigurujete AD Connect. 

Existují 2 AD Connect scénáře - pomocí synchronizace hesel nebo pomocí AD federace. Podívejte se [AD Connect informace](../active-directory/active-directory-aadconnect.md) zjistěte, které z těchto funguje nejlépe za vás.

Můžete také použít Azure AD + AD s cloudovou kolekcí. Ujistěte se, že budete postupovat podle stejné nastavení kroků.

Podívejte se na [Azure AD + požadavky služby Active Directory pro Azure RemoteApp](remoteapp-ad.md) kroky potřebné ke konfiguraci Azure AD a služby Active Directory.

## <a name="go-create-your-collection"></a>Vytvoření kolekce můžete vše vyzkoušet.
OK je pravděpodobné, že už víme jste, co ji nyní, takže není právě jednou z věcí zleva provést – vytvoření vaší první kolekce Azure Remoteappu.

[Vytvoření cloudové kolekce](remoteapp-create-cloud-deployment.md) nebo [vytvoření hybridní kolekce](remoteapp-create-hybrid-deployment.md) -právě získat vytváření.

