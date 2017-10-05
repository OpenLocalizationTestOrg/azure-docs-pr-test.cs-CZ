---
title: "Přidání uživatele do kolekce Azure Remoteappu | Microsoft Docs"
description: "Postup přidání uživatelů do kolekcí vzdálené aplikace Azure RemoteApp"
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
ms.openlocfilehash: 281e74c7941c42d8a3e4351953391229e54ce0a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-add-a-user-to-your-azure-remoteapp-collection"></a>Postup přidání uživatele do kolekcí vzdálené aplikace Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Než uživatelé můžete zobrazit a používat aplikace v Azure Remoteappu, musíte jim udělit přístup k vaší kolekce. Toto je snadno část: na **přístup uživatelů** , zadejte informace o účtu pro uživatele a pak klikněte na symbol zaškrtnutí.

Jaké informace o účtu potřebujete? To závisí na typu kolekce jste vytvořili (Cloudová nebo hybridní) a zda používáte Office 365 ProPlus v dané kolekci.

## <a name="supported-user-identities"></a>Podporované uživatelských identit
Typy jinou kolekci (cloud a hybridní) podporují, pomocí identity jiného uživatele pro přístup k aplikacím.  

Pro hybridní kolekci vzdálené aplikace RemoteApp musíte nastavit infrastrukturu domény služby Active Directory místní a tenanta služby Azure Active Directory s integrace adresáře (a volitelně jednotné přihlášení). Kromě toho budete muset vytvořit některé objekty služby Active Directory v místním adresáři.  

Pro cloudové kolekce vzdálené aplikace RemoteApp každý uživatel, který má Azure Active Directory podporu identity lze udělit přístup uživatele vzdálené aplikace RemoteApp zahrnout Accounts Microsoft.  Viz následující tabulka.

Office 365 se uživatelů Azure Active Directory. Pokud budou mít hybridní Azure Active Directory, adresář synchronizoval účty, se můžete udělit přístup uživatelů v hybridním nasazení vzdálené aplikace RemoteApp.   

Tato tabulka slouží jako Stručná referenční příručka, pro který je podporován identity v kolekci a jaké jsou požadavky služby Active Directory.

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
> Uživatelé Azure Active Directory musí pocházet z klienta, který je spojen s vaším předplatným. (Předplatné můžete zobrazit a upravit na kartě **Nastavení** v portálu. Další informace najdete v části [Změna klienta Azure Active Directory používaného RemoteAppem](remoteapp-changetenant.md).)
> 
> 

## <a name="office-365-proplus-user-account-information"></a>Informace o účtu Office 365 ProPlus uživatele
Pokud používáte image šablony Office 365 ProPlus v kolekci *nebo* Pokud jste vytvořili vlastní obrázek, který používá Office 365, které jsou povoleny pouze přidat uživatele Azure Active Directory, které mají předplatná Office 365 pro výchozí doménu vašeho předplatného. V tématu [pomocí Office 365 s Azure Remoteappem](remoteapp-o365.md) Další informace.

