---
title: "aaaAzure AD + požadavky služby Active Directory pro Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak tooset až toowork služby Active Directory s Azure Remoteappem."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 66366b25-6012-45fa-a4f6-da0ddfe0b486
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: c1c4a7ad6fb96ec4d479fdc231f03d81b58b2b71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + požadavky služby Active Directory pro Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Hybridní kolekce Azure Remoteappu nebo cloudové kolekce, které chcete toofederate pomocí služby AD Connect potřebujete následující toodo hello.

### <a name="connect-azure-ad-and-active-directory"></a>Azure AD Connect a služby Active Directory
Pokud chcete tooconnect klientovi Azure AD a prostředí vaší místní služby Active Directory, použijte AD Connect. Můžete pouze bude trvat [4 kliknutí](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) tooconnect hello dva adresáře.

Poznámka: synchronizace adresářů je vyžadován pro hybridní kolekce.

### <a name="make-sure-your-domaincom-match"></a>Zajistěte, aby vaše "@domain.com" odpovídat
Než začnete, ujistěte se, že hello UPN pro vaše místní doménové struktuře odpovídá hello příponu domény služby Azure AD. 

Po nastavení příponu UPN domény hello ve službě Azure AD, všechny uživatele přihlašující se do Azure RemoteApp se přihlaste se jako "uživatel @<hello suffix you set up>." Ujistěte se, že uživatelé také přihlásit pomocí hello stejné user@suffix do hello místní domény. V některých případech můžete nastavit až jeden název domény ve službě Azure AD při zadávání příponou jinou doménu pro hello uživatele v-místním prostředí V takovém případě vaši uživatelé nebudou mít tooconnect tooany připojený k doméně počítače nebo prostředků pomocí Azure Remoteappu.

Například pokud nastavíte vaší domény přípona UPN v AAD jako contoso.com, ale někteří uživatelé na místní nebo AD jsou nakonfigurované toolog s @contoso.uk, pak se tito uživatelé nebudou moct toocorrectly protokolu do hello ARA kolekce. Uživatele, které musí být hlavní název uživatele v AAD a AD hello stejné pro hello přihlášení toobe možné"

### <a name="create-objects-for-azure-remoteapp"></a>Vytvoření objektů pro Azure RemoteApp
Musíte taky toocreate hello následující objekty služby Active Directory v místě:

* Účtu tooprovide přístup toodomain prostředky služby u aplikací RemoteApp díky připojení ke službě RDSH koncové body toohello místní domény.
* Organizační jednotce (OU) toocontain vzdálené aplikace RemoteApp objekty počítačů. Použití hello organizační jednotky je doporučené (ale není požadováno) tooisolate hello účty a zásady, které budete používat s Remoteappem.

Obě tyto objekty musíte při vytváření kolekcí vzdálené aplikace RemoteApp, tak buďte zda toodo tyto kroky nejdřív.

