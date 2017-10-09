---
title: "Azure Active Directory Domain Services: Scénáře nasazení | Microsoft Docs"
description: "Scénáře nasazení služby Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c5216ec9-4c4f-4b7e-830b-9d70cf176b20
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: maheshu
ms.openlocfilehash: 8a64bd8f7c6eba8f6490e10fa62bef195b6b3d5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-scenarios-and-use-cases"></a>Scénáře nasazení a případy použití
V této části se podíváme na několik scénářů a případy použití využívající Azure Active Directory (AD) Domain Services.

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Zabezpečení a snadné správy virtuální počítače Azure
Virtuální počítače Azure můžete použít Azure Active Directory Domain Services toomanage zjednodušenou způsobem. Virtuální počítače Azure může být připojený k toohello spravované domény, a tak umožňuje toouse pověření toolog ve vaší podnikové AD. Tento přístup umožňuje vyhnout se přihlašovací údaje správy problémů, jako je například Správa účtů místního správce na všech virtuálních počítačích Azure.

Virtuální počítače serveru, které jsou připojené k toohello spravované domény můžete také spravovat a zabezpečené pomocí zásad skupiny. Můžete provést požadované bezpečnostní směrné plány tooyour Azure virtuálních počítačů a jejich Zamknout v souladu s pokyny pro podnikové zabezpečení. Například můžete vytvořit zásady správy možnosti toorestrict hello typy skupin aplikací, které může být spuštěn na těchto virtuálních počítačů.

![Zjednodušená správa virtuálních počítačů Azure](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Jako servery a další infrastrukturou dosáhne ukončenou životností, je mnoho aplikací, které jsou teď hostované na cloudové toohello místní přesunutí Contoso. Jejich aktuální standard IT vyžaduje, aby servery, které hostují firemní aplikace musí být připojený k doméně a spravovaná pomocí zásad skupiny. Společnosti Contoso správce IT upřednostní toodomain připojení virtuálních počítačů nasazených v Azure, správu toomake jednodušší. V důsledku toho správcům a uživatelům se přihlásit pomocí svých podnikových přihlašovacích údajů. V hello stejnou dobu, může být počítače nakonfigurované toocomply s směrné plány požadované zabezpečení pomocí zásad skupiny. Contoso přejete není toohave toodeploy, sledování a správě řadičů domény v Azure toosecure virtuální počítače Azure. Azure AD Domain Services je proto skvělé přizpůsobit pro tento případ použití.

**Poznámky k nasazení**

Mějte na paměti následující důležité body pro tento scénář nasazení hello:

* Ve výchozím nastavení spravovaných domén, které poskytuje Azure AD Domain Services poskytují struktura single ploché organizační jednotky (organizační jednotka). Všechny počítače připojené k doméně se nacházejí v jedné organizační jednotce ploché. Můžete však zvolit toocreate vlastní organizační jednotky.
* Azure AD Domain Services podporuje jednoduchý zásad skupiny v hello formu předdefinované objektu zásad skupiny každý hello uživatelů a počítačů kontejnery. Můžete vytvořit vlastní objekty zásad skupiny a směrovat je toocustom organizační jednotky.
* Azure AD Domain Services podporuje hello základní počítač objekt schéma služby AD. Objekt počítače hello schéma nelze rozšířit.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-tooazure-infrastructure-services"></a>Navýšení a shift místní aplikace, která používá tooAzure ověřování vazby LDAP služby infrastruktury
![Vazba LDAP](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso má místní aplikace, který byl zakoupen od nezávislý dodavatel softwaru mnoha lety. Hello aplikace je nyní v režimu údržby pomocí hello ISV a žádající aplikací toohello změny je výtažkovými pro společnost Contoso. Tato aplikace má front-end založené na webu, který shromažďuje přihlašovací údaje uživatele pomocí webového formuláře a potom ověřuje uživatele provedením toohello vazby LDAP podnikové služby Active Directory. Contoso byste chtěli toomigrate tooAzure tuto aplikaci služby infrastruktury. Je žádoucí, aby aplikace hello pracuje je, bez nutnosti změny. Uživatelé navíc musí být schopný tooauthenticate pomocí svých existujících podnikových přihlašovacích údajů a bez nutnosti tooretrain uživatelé toodo věci jinak. Jinými slovy koncoví uživatelé by měli být oblivious o kterém je spuštěna aplikace hello a hello migrace by měl být transparentní toothem.

**Poznámky k nasazení**

Mějte na paměti následující důležité body pro tento scénář nasazení hello:

* Ujistěte se, že aplikace hello není nutné directory toohello toomodify a zápis. Domény toomanaged přístup pro zápis LDAP poskytované Azure AD Domain Services není podporována.
* Nelze změnit hesla přímo na hello spravované domény. Koncoví uživatelé můžou změnit heslo, buď pomocí mechanismus pro změnu hesla pomocí samoobslužné služby Azure AD nebo proti hello místního adresáře. Tyto změny jsou automaticky synchronizované a k dispozici ve spravované doméně hello.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-tooaccess-hello-directory-tooazure-infrastructure-services"></a>Navýšení a shift místní aplikace, která používá LDAP číst tooaccess hello directory tooAzure služby infrastruktury
Contoso má místní obchodní (LOB) aplikace, která byla vyvinuta téměř deset před. Tato aplikace je adresář clustery a byla navrženou toowork se systémem Windows Server AD. aplikace Hello používá protokolu LDAP (Lightweight Directory Access Protocol) tooread informace nebo atributy o uživatelů ze služby Active Directory. aplikace Hello nemá upravit atributy nebo jinak zapsat toohello adresáře. Contoso byste chtěli toomigrate tooAzure tuto aplikaci služby infrastruktury a vyřazení z provozu hello stárnutí místní hardware aktuálně hostuje tuhle aplikaci. aplikace Hello nemůže být přepsaná toouse moderní directory rozhraní API například hello založené na REST Azure AD Graph API. Proto možnost navýšení a shift se požaduje, jímž hello aplikace může být migrované toorun v cloudu hello bez úpravy kódu nebo přepisování aplikace hello.

**Poznámky k nasazení**

Mějte na paměti následující důležité body pro tento scénář nasazení hello:

* Ujistěte se, že aplikace hello není nutné directory toohello toomodify a zápis. Domény toomanaged přístup pro zápis LDAP poskytované Azure AD Domain Services není podporována.
* Ujistěte se, že aplikace hello není nutné vlastní nebo rozšířené schéma služby Active Directory. Rozšíření schématu nejsou podporovány v Azure AD Domain Services.

## <a name="migrate-an-on-premises-service-or-daemon-application-tooazure-infrastructure-services"></a>Migrace místní služba nebo démon aplikace tooAzure služby infrastruktury
Některé aplikace obsahovat několik vrstev, kde jeden hello vrstev musí tooperform ověřených volání tooa back-end vrstvy například a databázové vrstvy. Účty služby Active Directory se běžně používají pro tyto případy použití. Můžete navýšení a shift takové aplikace tooAzure služby infrastruktury a používání Azure AD Domain Services pro potřeby identity hello z těchto aplikací. Můžete zvolit toouse hello stejný účet služby, který je synchronizován se z vašeho místního adresáře tooAzure AD. Alternativně můžete nejdřív vytvořit vlastní organizační jednotky a poté vytvořit samostatný účet služby v dané organizační jednotce, toodeploy takové aplikace.

![Účet služby pomocí WIA](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso má uživatelské softwarová aplikace úložiště, která zahrnuje front-end webové, SQL server a server back-end serveru FTP. Integrované v systému Windows ověřování účtů služeb je použité tooauthenticate hello front-end webovém toohello FTP serveru. front-end webový Hello je nastavený toorun jako účet služby. Hello back-end server je nakonfigurován tooauthorize přístup z účtu služby hello pro front-end webové hello. Contoso upřednostní není toohave toodeploy virtuálního počítače řadiče domény v cloudu toomove hello tooAzure tuto aplikaci služby infrastruktury. Společnosti Contoso správci IT mohou nasadit servery hello hostování hello webový server front-end, systému SQL server a hello FTP server tooAzure virtuálních počítačů. Tyto počítače jsou pak tooan připojený k Azure AD Domain Services spravované domény. Pak můžete použít stejný účet služby ve své místní adresář pro účely ověření hello aplikace hello. Tento účet služby je spravované domény synchronizované toohello Azure AD Domain Services a je k dispozici pro použití.

**Poznámky k nasazení**

Mějte na paměti následující důležité body pro tento scénář nasazení hello:

* Ujistěte se, že aplikace hello používá uživatelské jméno a heslo pro ověřování. Azure AD Domain Services nepodporuje ověřování na základě certifikátů nebo čipová karta.
* Nelze změnit hesla přímo na hello spravované domény. Koncoví uživatelé můžou změnit heslo, buď pomocí mechanismus pro změnu hesla pomocí samoobslužné služby Azure AD nebo proti hello místního adresáře. Tyto změny jsou automaticky synchronizované a k dispozici ve spravované doméně hello.

## <a name="windows-server-remote-desktop-services-deployments-in-azure"></a>Windows Server vzdálené plochy nasazení v Azure
Můžete použít Azure AD Domain Services tooprovide spravované AD domain services tooyour vzdálené plochy servery nasazené v Azure.

Další informace o tomto scénáři nasazení najdete v tématu Jak příliš[integrovat Azure AD Domain Services v rámci vašeho nasazení vzdálené plochy](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds).
