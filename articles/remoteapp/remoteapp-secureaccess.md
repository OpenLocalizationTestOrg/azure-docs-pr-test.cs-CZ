---
title: "aaaSecuring přístup tooAzure vzdálené aplikace RemoteApp, a dále | Microsoft Docs"
description: "Zjistěte, jak zabezpečeného přístupu tooAzure vzdálené aplikace RemoteApp pomocí podmíněného přístupu v Azure Active Directory"
services: remoteapp
documentationcenter: 
author: piotrci
manager: mbaldwin
ms.assetid: a19b0b09-ab26-4beb-80bb-33a46da39887
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 98dfe69e2f5fa5014b6eb3157e01f131c287134d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securing-access-tooazure-remoteapp-and-beyond"></a>Zabezpečení přístupu tooAzure vzdálené aplikace RemoteApp, a dále
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

V tomto článku jsme vám poskytne přehled o tom, jak může správce nastavit zabezpečený přístup kanál od prostřednictvím Azure RemoteApp hello koncovým uživatelům a konče zabezpečené prostředků, jako je například SQL database nebo jiné aplikace back-end. cílem Hello je toomake jistotu, že jenom oprávnění uživatelé, které schůzku hello požadovaných podmínek přístup k vzdálené aplikace a který hello zabezpečené back-end lze přistupovat pouze z prostředí Azure RemoteApp hello kontrolovat a nikoli z jiných umístění.

Existují 3 hlavní oblasti, které Dobrý den, správce potřebuje toolook na:

![Aspekty podmíněného přístupu Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Přečtěte si informace a odpovědi toothese dotazy.

## <a name="who-can-access-hello-collection"></a>Kdo má přístup k hello kolekce?
Hello správce zvolí hello uživatelé, kteří mohou přistupovat ke vzdálené aplikace v kolekci hello. Můžete použít Azure Active Directory (Azure AD) pracovní nebo školní účty (dříve nazývané, "účty organizace") nebo účty Microsoft (například @outlook.com). Většina podnikové scénáře použití účty Azure AD; Tyto vám umožňují používat funkce podmíněného přístupu, které jsou popsané později a jsou také hello pouze volbou pro kolekce připojený k doméně. Hello zbytek hello článku předpokládá, že používáte účty Azure AD s Azure Remoteappem.

**Co jsme mají udělat:**

Pomocí Azure AD účty toocontrol přístup tooAzure vzdálené aplikace RemoteApp udává nám dvě věci:

1. Vždy vědět, kdo má přístup k hello aplikace, které jsme publikovali a přístup k back EndY tyto aplikace se připojují k.
2. Jsme řídit hello základní Azure AD, můžeme vytvořit a odstranit uživatelské účty, zásady pro hesla sady, použijte službu Multi-Factor authentication, atd. 

## <a name="how-is-hello-collection-accessed-from-where"></a>Způsob přístupu k hello kolekce? Odkud?
Správci často chtít toodefine zásad pro přístup k veřejné internetové prostředí, například Azure RemoteApp. Například chtějí tooensure, aby uživatele, kteří používají hello prostředí z mimo hello podnikové sítě měly přístup toogain vícefaktorové ověřování (MFA) toouse; nebo možná budou zablokovány úplně.

Azure RemoteApp mohou správci hello funkce je k dispozici prostřednictvím zásad podmíněného přístupu tooset Azure AD Premium pro svoje prostředí Azure RemoteApp. Může také používat rozšířené funkce vytváření sestav a výstrah funkce toomonitor jak hello prostředí přistupuje.

### <a name="how-tooset-up-conditional-access-for-azure-remoteapp"></a>Jak tooset až podmíněného přístupu pro Azure RemoteApp
Snažíme se probíhající toowalk prostřednictvím ukázkový scénář – hello Azure RemoteApp správce chce prostředí toohello tooblock přístupu, pokud jsou uživatelé mimo podnikovou síť hello.

> [!NOTE]
> Předpokladu, že jste provedli upgrade toohello Azure AD Premium vrstvy a že jste vytvořili aspoň jednu kolekci Azure RemoteApp.
> 
> 

1. Na portálu Azure klikněte na tlačítko hello **služby Active Directory** kartě. Klikněte na tlačítko chcete tooconfigure directory hello.
   
   Mějte na paměti: Podmíněného přístupu je vlastnost adresáře a nikoli instanci Azure Remoteappu, aby všechny konfigurace se provádí na úrovni služby directory hello. Také to znamená, že potřebujete toobe hello directory správce toomake tyto změny.
2. Klikněte na tlačítko **aplikace**a potom klikněte na **Microsoft Azure RemoteApp** tooset až podmíněného přístupu. Všimněte si, že můžete nastavit podmíněný přístup pro každou aplikaci "software jako služba" v adresáři samostatně.
   ![Nastavení podmíněného přístupu pro Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
3. Na hello **konfigurace** nastavte **povolit pravidla přístupu k** tooON.
   ![Povolení pravidel přístupu pro Azure RemoteApp](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
4. Teď můžete nakonfigurovat různá pravidla a zvolte, kteří tooapply je:
   
   1. Zvolte **zablokovat přístup, když není v práci** toocompletely zabránit uživatelům v přístupu k Azure RemoteApp mimo prostředí sítě hello zadáte.
   2. Klikněte na možnost hello níže toodefine rozsahů adres IP v hello, které tvoří "důvěryhodné síti." Všechno, co mimo těch, které budou odmítnuty.
5. Spuštěním klientovi Azure Remoteappu hello z IP adresy mimo zadaný rozsah hello otestujte vaši konfiguraci. Po registraci se pomocí přihlašovacích údajů Azure AD měla zobrazit zpráva takto:

![Odepření přístupu tooAzure vzdálené aplikace RemoteApp](./media/remoteapp-secureaccess/ra-accessdenied.png)

### <a name="future-conditional-access-features"></a>Funkce budoucí podmíněného přístupu
tým služby Azure Active Directory Hello pracuje na nové funkce podmíněného přístupu. Správci budou mít možnost toocreate nové typy pravidel nad rámec pravidla na základě umístění sítě. Veřejné náhled hello nové funkce by měla být brzy dostupná.

### <a name="how-toomonitor-access-tooazure-remoteapp"></a>Jak toomonitor přistupovat k tooAzure vzdálené aplikace RemoteApp
Skvělé funkce toouse spolu s podmíněného přístupu je funkce vytváření sestav Azure Active Directory Premium hello. Můžete použít toomonitor sestavy, který přistupuje k prostředí a detekovat podezřelé aktivity.

Například můžete zobrazit názvy hello hello uživatelů, kteří získat přístup k Azure Remoteappu, kolikrát tomu bylo ho a kdy.

1. Na portálu Azure, klikněte na tlačítko **služby Active Directory**a pak klikněte na tlačítko adresáře.
2. Přejděte toohello **sestavy** kartě.
3. Hello seznamu sestav vyberte **využití aplikace** pod **integrovaných aplikací**.
   
   Zobrazí se některé souhrnných statistik pro Azure RemoteApp. 
   ![Souhrnné statistiky přístup k Azure RemoteApp](./media/remoteapp-secureaccess/ra-accessstats.png)
4. Klikněte na tlačítko hello aplikace tooreveal informace o uživatele, kteří používají Azure RemoteApp.
   ![Statistiky přístup uživatelů pro Azure RemoteApp](./media/remoteapp-secureaccess/ra-userstats.png)

### <a name="summary"></a>Souhrn
S Azure Active Directory Premium můžete nastavit přístup pravidla tooAzure vzdálené aplikace RemoteApp (a další software jako služba aplikace, který je k dispozici prostřednictvím služby Azure AD). Pravidla jsou aktuálně omezená toonetwork umístění na základě zásad, ale bude v budoucí hello rozšířeno tooother aspekty enterprise Management.

Azure AD Premium nabízí také vytváření sestav a monitorování funkce, které dále rozšířit hello řízení Dobrý den, správce má v jejich prostředí Azure RemoteApp.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Jak se ujistěte se, že moje zabezpečený prostředek je dostupný jenom z mé prostředí Azure RemoteApp?
V předchozích částech tohoto článku jsme zaměřené na zabezpečení prostředí Azure RemoteApp toohello přístupu. Jsme mají udělat, výběr hello uživatelů, kteří mají povolen přístup a nastavení řízení toofurther pravidla přístupu, jak mohou používat službu hello.

Běžný scénář pro nasazení Azure RemoteApp je, že vzdálené aplikace hello nutné toocommunicate s back-end prostředku, třeba databázi SQL. Tento prostředek je hostována místně (např. v podnikové síti) nebo v cloudu hello (např. v Azure IaaS). Správci často chtít toomake jistotu, že hello back-end prostředků lze přistupovat pouze aplikace nasazené prostřednictvím Azure Remoteappu a není třeba aplikace spuštěna přímo na počítači uživatele a přístupu k prostřednictvím veřejného Internetu. Azure RemoteApp dochází často jako hello centrálně spravovat a zabezpečené prostředí a proto hello pouze cestu, přes který by uživatelé komunikovat s hello prostředků back-end.

řešení Hello je tooplace obě hello prostředí Azure RemoteApp a hello zabezpečení prostředků v hello virtuální sítě stejné Azure (VNET). Pokud je prostředkem hello v jiné lokalitě, můžete vytvořit připojení VPN typu site-to-site, například toocreate Azure datového centra STA hello virtuální sítě a hello zákazníka v místním prostředí.

Azure RemoteApp podporuje dva typy nasazení kolekce, kde můžete zadat své vlastní virtuální sítě:

* Non-připojené k doméně: hello aplikace bude mít "směrem pohledu" Dobrý den další prostředky ve hello virtuální sítě. Například to může být použité tooconnect aplikace tooa SQL database, která používá ověřování systému SQL (aplikace ověření uživatele hello přímo na databázi hello)
* Připojený k doméně: hello virtuální počítače používané Azure RemoteApp jsou připojené k tooa řadiče domény v hello virtuální sítě. To je užitečné, když aplikace hello potřebují tooauthenticate vůči řadiči domény systému Windows v pořadí tooget přístup tooa back-end prostředku.
  ![Kolekce připojený k doméně v Azure Remoteappu](./media/remoteapp-secureaccess/ra-domainjoined.png)

### <a name="how-toocreate-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Jak toocreate zabezpečené připojení mezi Azure a Moje místního prostředí
Existuje několik možností konfigurace pro připojení vašich Azure a místních prostředích. Zde jsou k dispozici pěkný přehled možností hello.

S Azure Remoteappem potřebovat tooconfigure virtuální síť nejprve a pak použít během procesu vytváření hello vaší kolekce. 

## <a name="hello-complete-solution"></a>úplné řešení Hello
Hello obrázek ukazuje hello kompletního řešení, kde jsme jste vytvořili kanál zabezpečeného přístupu z hello koncového uživatele prostřednictvím Azure Remoteappu (ARA), do prostředků hello back-end.
![Zabezpečení Azure RemoteApp](./media/remoteapp-secureaccess/ra-secureoverview.png) ve fázi 1 jsme vybraných uživatelů hello a vytvořit pravidla přístupu, které řídí, jak ARA přístupná. V následujícím příkladu hello jsme pouze povolit přístup pro uživatelů, kteří pracují z podnikové sítě hello. Nekompatibilní uživatelé nebudou moct tooaccess hello ARA prostředí vůbec.
Na fázi 2 jsme mají přístup hello back-end prostředků pouze prostřednictvím hello konfigurace virtuální sítě a sítě VPN, který jsme řízení. Azure RemoteApp byla uložena do umístění hello stejné virtuální síti. Hello konečným výsledkem je, že hello prostředků lze přistupovat pouze prostřednictvím prostředí ARA hello.

