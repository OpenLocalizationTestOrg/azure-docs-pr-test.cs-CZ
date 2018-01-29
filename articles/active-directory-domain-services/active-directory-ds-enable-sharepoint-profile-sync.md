---
title: "Azure Active Directory Domain Services: Povolení podpory pro službu SharePoint uživatelský profil | Microsoft Docs"
description: "Konfigurace Azure Active Directory Domain Services spravovaných domén na podporu synchronizace profilu pro SharePoint Server"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: d98fbe85f35932161476a800a2fd438f0bf2b9ef
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a>Konfigurace spravované doméně kvůli podpoře synchronizace profilu pro SharePoint Server
SharePoint Server obsahuje služba profilu uživatele, který je používán k synchronizaci profilu uživatele. Pokud chcete nastavit službu profilu uživatele, nutné příslušná oprávnění udělit v doméně služby Active Directory. Další informace najdete v tématu [udělení oprávnění služby Active Directory Domain Services pro profil synchronizace SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).

Tento článek vysvětluje způsob konfigurace Azure AD Domain Services spravovaných domén na nasazení služby synchronizace profilů uživatelů serveru SharePoint.

## <a name="the-aad-dc-service-accounts-group"></a>Účty AAD řadič domény služby skupiny
Skupina zabezpečení s názvem "**účty řadiče domény služby AAD**' je k dispozici v rámci organizační jednotky, uživatele' na vaší spravované domény. Zobrazí se v této skupině **Active Directory Users and Computers** modul snap-in konzoly MMC na vaší spravované domény.

![Skupina zabezpečení účtů služby AAD řadiče domény](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

Členové této skupiny zabezpečení jsou delegovanými následující oprávnění:
- Oprávnění k replikaci změn adresáře v kořenovém adresáři DSE spravované domény.
- Oprávnění k replikaci adresáře změny v názvovém kontextu konfigurace (cn = kontejneru konfigurace) spravované domény.

Tato skupina zabezpečení je taky člen předdefinované skupiny **Pre-Windows 2000 Compatible Access**.

![Skupina zabezpečení účtů služby AAD řadiče domény](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a>Povolit vaší spravované domény pro podporu serveru SharePoint synchronizace profilů uživatelů
Přidáním účtu služby používaného synchronizace profilu uživatele služby SharePoint **účty řadiče domény služby AAD** skupiny. V důsledku toho účet synchronizace získá odpovídající oprávnění k replikaci změn do adresáře. Tento krok konfigurace umožňuje synchronizace profilů uživatelů serveru SharePoint správně fungovat.

![Účty služby AAD DC - přidat členy](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![Účty služby AAD DC - přidat členy](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>Související obsah
* [Technické Reference - oprávnění Grant Active Directory Domain Services pro profil synchronizace SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx)
