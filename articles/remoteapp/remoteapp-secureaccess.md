---
title: "Zabezpečení přístupu k Azure Remoteappu a mimo ni | Microsoft Docs"
description: "Zjistěte, jak zabezpečený přístup k Azure RemoteApp pomocí podmíněného přístupu v Azure Active Directory"
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
ms.openlocfilehash: 28ee4698515d11964e5371628d21dbc00687c861
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="securing-access-to-azure-remoteapp-and-beyond"></a>Zabezpečení přístupu k Azure Remoteappu a mimo ni
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

V tomto článku jsme vám poskytne přehled o tom, jak může správce nastavit zabezpečený přístup kanál od koncového uživatele pomocí Azure Remoteappu a konče zabezpečené prostředků, jako je například SQL database nebo jiné aplikace back-end. Cílem je zajistit, že jenom oprávnění uživatelé splňující požadované podmínky přístup k vzdálené aplikace a zabezpečení back-end lze přistupovat pouze v z řízené prostředí Azure Remoteappu a nikoli z jiných umístění.

Existují 3 hlavní oblasti, které správce potřebuje k podívejte se na:

![Aspekty podmíněného přístupu Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Pro čtení na informace a odpovědi na tyto otázky.

## <a name="who-can-access-the-collection"></a>Kdo může přistupovat ke kolekci?
Správce zvolí uživatelé, kteří mohou přistupovat ke vzdálené aplikace v kolekci. Můžete použít Azure Active Directory (Azure AD) pracovní nebo školní účty (dříve nazývané, "účty organizace") nebo účty Microsoft (například @outlook.com). Většina podnikové scénáře použití účty Azure AD; umožňují vám používat podmíněný přístup funkce popsané později a jsou také pouze volbou pro kolekce připojený k doméně. Zbývající části článku předpokládá, že používáte účty Azure AD s Azure Remoteappem.

**Co jsme mají udělat:**

Použití účtů Azure AD k řízení přístupu k Azure RemoteApp nám poskytuje dvě věci:

1. Vždy vědět kdo má přístup k aplikacím jsme publikovali a získat přístup k žádnému back EndY tyto aplikace se připojit k.
2. Jsme řídit základní Azure AD, můžeme vytvořit a odstranit uživatelské účty, zásady pro hesla sady, použijte službu Multi-Factor authentication, atd. 

## <a name="how-is-the-collection-accessed-from-where"></a>Způsob přístupu k kolekce? Odkud?
Běžně chtějí správci definovat zásady pro přístup k veřejné internetové prostředí, například Azure RemoteApp. Například chtějí zajistit uživatelům přístup k prostředí z mimo firemní síť k používání služby Multi-Factor authentication (MFA) pro získání přístupu; nebo možná budou zablokovány úplně.

Azure RemoteApp mohou správci funkce, která je k dispozici prostřednictvím Azure AD Premium nastavit zásady podmíněného přístupu pro jejich prostředí Azure RemoteApp. Vytváření sestav a výstrahy funkce lze také použít ke sledování, jak je přistupuje prostředí.

### <a name="how-to-set-up-conditional-access-for-azure-remoteapp"></a>Jak nastavit podmíněný přístup pro Azure RemoteApp
Přidáme provede ukázkový scénář – Azure RemoteApp správce chce blokovat přístup k prostředí, pokud jsou uživatelé mimo firemní síť.

> [!NOTE]
> Předpokladu, že jste provedli upgrade na vrstvě | Premium Azure AD a že jste vytvořili aspoň jednu kolekci Azure RemoteApp.
> 
> 

1. V portálu Azure klikněte **služby Active Directory** kartě. Klikněte na adresář, který chcete nakonfigurovat.
   
   Mějte na paměti: Podmíněného přístupu je vlastnost adresáře a nikoli instanci Azure Remoteappu, aby všechny konfigurace se provádí na úrovni adresáře. Také to znamená, že musíte být správce adresáře k provedení těchto změn.
2. Klikněte na tlačítko **aplikace**a potom klikněte na **Microsoft Azure RemoteApp** k nastavení podmíněného přístupu. Všimněte si, že můžete nastavit podmíněný přístup pro každou aplikaci "software jako služba" v adresáři samostatně.
   ![Nastavení podmíněného přístupu pro Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
3. Na **konfigurace** nastavte **povolit pravidla přístupu k** na hodnotu ON.
   ![Povolení pravidel přístupu pro Azure RemoteApp](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
4. Teď můžete nakonfigurovat různá pravidla a zvolte, kteří mají použít:
   
   1. Zvolte **zablokovat přístup, když není v práci** úplně zabránit uživatelům v přístupu k Azure RemoteApp mimo síťové prostředí, které zadáte.
   2. Klikněte na níže uvedenou možnost definovat rozsahy IP adres, které tvoří "důvěryhodné síti." Všechno, co mimo těch, které budou odmítnuty.
5. Otestujte vaši konfiguraci spuštěním klientovi Azure Remoteappu z IP adresy mimo zadaný rozsah. Po registraci se pomocí přihlašovacích údajů Azure AD měla zobrazit zpráva takto:

![Odepření přístupu k Azure RemoteApp](./media/remoteapp-secureaccess/ra-accessdenied.png)

### <a name="future-conditional-access-features"></a>Funkce budoucí podmíněného přístupu
Tým služby Azure Active Directory pracuje na nové funkce podmíněného přístupu. Správci můžou k vytvoření nových typů pravidel mimo síť umístění na základě pravidel. Veřejné preview nové funkce by měla být brzy dostupná.

### <a name="how-to-monitor-access-to-azure-remoteapp"></a>Postup sledování přístup k Azure RemoteApp
Skvělé funkce, která použít spolu s podmíněného přístupu je funkce vytváření sestav Azure Active Directory Premium. Můžete použít sestavy monitorování, který přistupuje k prostředí a detekovat podezřelé aktivity.

Například můžete zobrazit názvy uživatelů, kteří získat přístup k Azure Remoteappu, kolikrát se jako a kdy.

1. Na portálu Azure, klikněte na tlačítko **služby Active Directory**a pak klikněte na tlačítko adresáře.
2. Přejděte na **sestavy** kartě.
3. V seznamu sestav vyberte **využití aplikace** pod **integrovaných aplikací**.
   
   Zobrazí se některé souhrnných statistik pro Azure RemoteApp. 
   ![Souhrnné statistiky přístup k Azure RemoteApp](./media/remoteapp-secureaccess/ra-accessstats.png)
4. Klikněte na aplikaci na nich informace o uživatele, kteří používají Azure RemoteApp.
   ![Statistiky přístup uživatelů pro Azure RemoteApp](./media/remoteapp-secureaccess/ra-userstats.png)

### <a name="summary"></a>Souhrn
S Azure Active Directory Premium můžete nastavit pravidla přístupu k Azure RemoteApp (a další software jako služba aplikace, který je k dispozici prostřednictvím služby Azure AD). Pravidla jsou aktuálně omezená na umístění na základě zásad sítě, ale bude v budoucnu rozšířit na další aspekty enterprise Management.

Azure AD Premium nabízí také vytváření sestav a monitorování funkce, které rozšiřují další ovládací prvek správce má v jejich prostředí Azure RemoteApp.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Jak se ujistěte se, že moje zabezpečený prostředek je dostupný jenom z mé prostředí Azure RemoteApp?
V předchozích částech tohoto článku jsme zaměřené na zabezpečení přístupu k prostředí Azure Remoteappu. Budeme mít udělat, výběr uživatelů, kteří mají povolen přístup a nastavení pravidel přístupu pro další ovládání, jak mohou používat službu.

Běžný scénář pro nasazení Azure RemoteApp je, že vzdálené aplikace potřebují ke komunikaci s back-end prostředku, třeba databázi SQL. Tento prostředek je hostována místně (např. v podnikové síti) nebo v cloudu (např. v Azure IaaS). Správci často chtějí Ujistěte se, že prostředek back-end lze přistupovat pouze prostřednictvím aplikace nasazené prostřednictvím Azure Remoteappu a není třeba aplikace spuštěna přímo na počítači uživatele a přístupu k prostřednictvím veřejného Internetu. Azure RemoteApp dochází často jako centrálně spravované a zabezpečené prostředí a proto pouze cestu, přes který by uživatelé komunikovat s back-end prostředků.

Řešení je umístit prostředí Azure Remoteappu a zabezpečení prostředků v stejnou Azure virtuální sítě (virtuální síť). Pokud je v jiné lokalitě, můžete vytvořit připojení site-to-site VPN, třeba chcete vytvořit síť VNet rozložení datového centra Azure a místního prostředí zákazníků.

Azure RemoteApp podporuje dva typy nasazení kolekce, kde můžete zadat své vlastní virtuální sítě:

* Non-připojené k doméně: aplikace bude mít "směrem pohledu" Další prostředky ve virtuální síti. Například může sloužit k připojení aplikace k databázi SQL, která používá ověřování systému SQL (aplikace ověření uživatele přímo v databázi)
* Připojený k doméně: virtuální počítače používané Azure RemoteApp připojeni k řadiči domény ve virtuální síti. To je užitečné, když je aplikace, které jsou potřeba ověřují řadiči domény systému Windows, aby bylo možné získat přístup k prostředku back-end.
  ![Kolekce připojený k doméně v Azure Remoteappu](./media/remoteapp-secureaccess/ra-domainjoined.png)

### <a name="how-to-create-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Postup vytvoření bezpečného připojení mezi Azure a Moje místního prostředí
Existuje několik možností konfigurace pro připojení vašich Azure a místních prostředích. Zde jsou k dispozici pěkný přehled možností.

S Azure Remoteappem musíte nejprve nakonfigurovat virtuální síť a pak jeho pomocí během procesu vytváření vaší kolekce. 

## <a name="the-complete-solution"></a>Úplné řešení
Následující diagram ukazuje úplného řešení, kde jsme jste vytvořili kanál zabezpečeného přístupu z koncového uživatele, prostřednictvím Azure Remoteappu (ARA), do prostředku back-end.
![Zabezpečení Azure RemoteApp](./media/remoteapp-secureaccess/ra-secureoverview.png) ve fázi 1 jsme vybrali uživatele a vytvořit pravidla přístupu, které řídí, jak ARA přístupná. V níže uvedeném příkladu jsme pouze umožňují přístup uživatelů, kteří pracují z podnikové sítě. Nekompatibilní uživatelé nebudou mít přístup k prostředí ARA vůbec.
Na fáze 2 jsme mít přístup k back-end prostředku pouze prostřednictvím o konfiguraci virtuální sítě a sítě VPN, které jsme řízení. Azure RemoteApp byl umístěn ve stejné virtuální síti. Konečným výsledkem je, že prostředek lze přistupovat pouze prostřednictvím prostředí ARA.

