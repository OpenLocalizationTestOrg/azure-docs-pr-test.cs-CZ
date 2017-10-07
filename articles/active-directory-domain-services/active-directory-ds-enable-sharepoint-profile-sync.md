---
title: "Azure Active Directory Domain Services: Povolení podpory pro službu SharePoint uživatelský profil | Microsoft Docs"
description: "Konfigurace Azure Active Directory Domain Services spravovaných domén toosupport profil synchronizace pro SharePoint Server"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 9de4f810380309e8f6436fc24412701645978f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-managed-domain-toosupport-profile-synchronization-for-sharepoint-server"></a>Konfigurovat synchronizaci profil toosupport spravované domény pro SharePoint Server
SharePoint Server obsahuje služba profilu uživatele, který je používán k synchronizaci profilu uživatele. tooset až hello služba profilu uživatele, příslušná oprávnění potřebovat toobe udělena v doméně služby Active Directory. Další informace najdete v tématu [udělení oprávnění služby Active Directory Domain Services pro profil synchronizace SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).

Tento článek vysvětluje způsob konfigurace Azure AD Domain Services spravovaných domén toodeploy hello synchronizace profilu uživatele Server služby SharePoint služby.

## <a name="hello-aad-dc-service-accounts-group"></a>Hello účty AAD řadič domény služby skupiny
Skupina zabezpečení s názvem "**účty řadiče domény služby AAD**' je k dispozici v rámci organizační jednotky, uživatele' hello na vaší spravované domény. Zobrazí se tato skupina v hello **Active Directory Users and Computers** modul snap-in konzoly MMC na vaší spravované domény.

![Skupina zabezpečení účtů služby AAD řadiče domény](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

Členové této skupiny zabezpečení jsou delegovaný hello následující oprávnění:
- oprávnění Hello replikovat změny adresáře v kořenovém adresáři hello DSE hello spravované domény.
- Hello oprávnění replikovat změny adresáře v názvovém kontextu konfigurace hello (cn = kontejneru konfigurace) hello ke správě domény.

Tato skupina zabezpečení je taky jako člen předdefinované skupiny hello **Pre-Windows 2000 Compatible Access**.

![Skupina zabezpečení účtů služby AAD řadiče domény](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-toosupport-sharepoint-server-user-profile-sync"></a>Povolit vaší spravované domény toosupport synchronizace profilů uživatelů serveru SharePoint
Můžete přidat hello účtu služby používaného pro SharePoint uživatele profil synchronizace toohello **účty řadiče domény služby AAD** skupiny. V důsledku toho hello synchronizace účet získá odpovídající oprávnění tooreplicate změny toohello adresáře. Tento krok konfigurace umožňuje toowork synchronizace profilu uživatele serveru SharePoint správně.

![Účty služby AAD DC - přidat členy](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![Účty služby AAD DC - přidat členy](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>Související obsah
* [Technické Reference - oprávnění Grant Active Directory Domain Services pro profil synchronizace SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx)
