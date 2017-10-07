---
title: "aaaAdd uživatele tooyour kolekci Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak uživatelé tooyour tooadd kolekci Azure Remoteappu"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 6b751fd2-2b11-499f-a2eb-12cb47f3129b
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 0ae88e04c8bfc2ed55dc963945ed7e9ff687b603
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-a-user-tooyour-azure-remoteapp-collection"></a>Jak tooadd uživatele tooyour kolekci Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Než uživatelé můžete zobrazit a používat aplikace v Azure Remoteappu, musíte je přístup ke kolekci tooyour toogrant. Toto je část snadno hello: na hello **přístup uživatelů** , zadejte informace o účtu hello hello uživatele a pak klikněte hello zaškrtnutí.

Jaké informace o účtu potřebujete? To závisí na typu hello kolekce jste vytvořili (Cloudová nebo hybridní) a zda používáte Office 365 ProPlus v dané kolekci.

## <a name="supported-user-identities"></a>Podporované uživatelských identit
typy jinou kolekci Hello (cloud a hybridní) podporují, pomocí identity jiného uživatele pro přístup k tooapplications.  

Pro hybridní kolekci vzdálené aplikace RemoteApp budete potřebovat tooset až infrastrukturu domény služby Active Directory místní a tenanta služby Azure Active Directory s integrace adresáře (a volitelně jednotné přihlašování). Kromě toho musíte toocreate některé objekty služby Active Directory v hello místního adresáře.  

Pro cloudové kolekce vzdálené aplikace RemoteApp každý uživatel, který má Azure Active Directory podporu identity lze udělit práva uživatele přístupu tooRemoteApp tooinclude Accounts Microsoft.  Viz následující tabulka hello.

Office 365 se uživatelů Azure Active Directory. Pokud budou mít hybridní Azure Active Directory, adresář synchronizoval účty, se můžete udělit přístup uživatelů v hybridním nasazení vzdálené aplikace RemoteApp.   

Tato tabulka slouží jako Stručná referenční příručka, pro který je podporován identity v kolekci a jaké jsou požadavky služby Active Directory hello.

| Uživatelské účty | Cloud | Hybridní |
| --- | --- | --- |
| Účet Microsoft |Ano |Ne |
| Azure Active Directory (Azure AD) | | |
| Jenom Azure AD cloud |Ano |Ne |
| ADsync se synchronizací hesla |Ano |Ano |
| ADsync bez synchronizace hesla |Ano |Ne |
| ADsync se službou AD FS |Ano |Ano |
| [3. stran Azure podporován zprostředkovatelů identity](https://msdn.microsoft.com/library/azure/jj679342.aspx) (třeba příkaz Ping) |Ano |Ano |
| Multi-factor Authentication |Ano |Ano |

Podívejte se na [informace](remoteapp-ad.md) týkající se konfigurace služby Active Directory pro vzdálenou aplikaci RemoteApp.

> [!NOTE]
> Uživatelé Azure Active Directory Hello musí pocházet z hello klienta, který je spojen s vaším předplatným. (Můžete zobrazit a změnit předplatné hello **nastavení** kartě hello portálu. V tématu [klienta Azure Active Directory hello změnu používaného Remoteappem](remoteapp-changetenant.md) Další informace.)
> 
> 

## <a name="office-365-proplus-user-account-information"></a>Informace o účtu Office 365 ProPlus uživatele
Pokud používáte image šablony Office 365 ProPlus hello v kolekci *nebo* Pokud jste vytvořili vlastní obrázek, který používá Office 365, jsou povoleny pouze tooadd Azure Active Directory uživatelů, kteří mají předplatná Office 365 pro hello výchozí doménu vašeho předplatného. V tématu [pomocí Office 365 s Azure Remoteappem](remoteapp-o365.md) Další informace.

