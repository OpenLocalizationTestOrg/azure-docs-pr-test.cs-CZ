---
title: "verze MFA aaaAzure a spotřeba plány | Microsoft Docs"
description: "Informace o hello vícefaktorového ověřování klienta a různé metody hello a verze, které jsou k dispozici. Podrobnosti o jednotlivých plánu spotřeby"
keywords: 
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: kgremban
ms.openlocfilehash: 4914747e435531b9f950356d23aa386f3d9585d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-azure-multi-factor-authentication"></a>Jak tooget Azure Multi-Factor Authentication

Pokud jde tooprotecting vaše účty, musí být dvoustupňové ověření standardní celé organizaci. Tato funkce je obzvláště důležité pro účty pro správu, které mají privilegovaný přístup tooresources. Z tohoto důvodu společnost Microsoft nabízí základní dvoustupňové ověření funkce tooOffice 365 a Azure správci. Chcete-li tooupgrade hello funkce pro vaši správci, nebo rozšířit rest toohello dvoustupňové ověření uživatelů, si můžete zakoupit Azure Multi-Factor Authentication. 

Tento článek se zabývá vysvětluje hello rozdíl mezi verzemi hello nabízí tooadministrators a hello plnou verzi Azure MFA a určuje funkce, které jsou v nich dostupné. Pokud jste připraveni toodeploy hello dokončení nabídky Azure MFA, hello novější části zahrnuje implementace možnosti a jak Microsoft vypočítá spotřeby.

>[!IMPORTANT]
>Tento článek je určen toobe Průvodce toohelp, rozumíte hello toobuy různé způsoby ověřování Azure Multi-Factor Authentication. Konkrétní podrobnosti o cenách a fakturace, by měla vždycky najdete toohello [Multi-Factor Authentication stránce s cenami](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="available-versions-of-azure-multi-factor-authentication"></a>K dispozici verze služby Azure Multi-Factor Authentication

Hello následující tabulka popisuje hello rozdíly mezi tři verze služby Multi-Factor authentication:

| Verze | Popis |
| --- | --- |
| Služba Multi-Factor Authentication (vícefaktorové ověřování) pro Office 365 |Tato verze funguje výhradně u aplikací Office 365 a je spravovat z portálu hello Office 365. Správci můžou [zabezpečení prostředků Office 365 s dvoustupnovym overovanim](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6). Tato verze je součástí předplatné služeb Office 365. |
| Víceúrovňové ověřování pro Azure správci | Globální správci klientů, které Azure můžete povolit dvoustupňové ověřování pro jejich účty globálních správců bez dalších poplatků.|
| Azure Multi-Factor Authentication | Často označují tooas verze "úplná" hello, Azure Multi-Factor Authentication nabízí hello nejkomplexnější sadu funkcí. Poskytuje další možnosti konfigurace prostřednictvím hello [portál Azure classic](https://manage.windowsazure.com), pokročilé vytváření sestav a podpora pro řadu místní i cloudové aplikace. Azure Multi-Factor Authentication je součástí Azure Active Directory Premium (plány P1 a P2) a Enterprise Mobility + Security (plány E3 a E5) a může být buď nasazené [v hello cloudu nebo místně](multi-factor-authentication-get-started.md). |

## <a name="feature-comparison-of-versions"></a>Porovnání funkcí verze
Hello následující tabulka obsahuje seznam hello funkcí, které jsou k dispozici v hello různým verzím systému ověřování Azure Multi-Factor Authentication.

> [!NOTE]
> Tato tabulka porovnání popisuje hello funkce, které jsou součástí jednotlivých verzí služby Multi-Factor Authentication. Pokud máte hello úplné službou Azure Multi-Factor Authentication, některé funkce nemusí být k dispozici v závislosti na tom, zda používáte [MFA v cloudu hello nebo MFA místní](multi-factor-authentication-get-started.md).


| Funkce | Služba Multi-Factor Authentication (vícefaktorové ověřování) pro Office 365 | Víceúrovňové ověřování pro Azure správci | Azure Multi-Factor Authentication |
| --- |:---:|:---:|:---:|
| Chránit účty správců pomocí vícefaktorového ověřování |● |● (pouze účty globální správce) |● |
| Mobilní aplikace jako druhý faktor |● |● |● |
| Telefonní hovor jako druhý faktor |● |● |● |
| Služby SMS jako druhý faktor |● |● |● |
| Hesla aplikací pro klienty, kteří nepodporují MFA |● |● |● |
| Správce kontrolu nad metody ověření |● |● |● |
| Režim kódu PIN | | |● |
| Výstraha podvodů | | |● |
| Sestavy MFA | | |● |
| Jednorázové potlačení | | |● |
| Vlastní přivítání pro telefonní hovory | | |● |
| ID vlastní volajícího pro telefonní hovory | | |● |
| Důvěryhodné IP adresy | | |● |
| Zapamatovat MFA pro důvěryhodná zařízení |● |● |● |
| MFA SDK | | |● (poskytovatele vyžaduje vícefaktorového ověřování a úplné předplatné) |
| MFA pro místní aplikace | | |● |

## <a name="how-tooget-azure-multi-factor-authentication"></a>Jak tooget Azure Multi-Factor Authentication
Pokud chcete hello úplné funkce nabízené službou Azure Multi-Factor Authentication, máte několik možností:

### <a name="option-1---mfa-licenses"></a>Možnost 1 - licence MFA

Nákupu Azure Multi-Factor Authentication licencí a přiřaďte je tooyour uživatele v Azure Active Directory. 

Pokud použijete tuto možnost, měli byste vytvořit poskytovatele Azure Multi-Factor Authentication, pouze pokud je také potřeba tooprovide dvoustupňové ověřování pro některé uživatele, kteří nemají licence. Jinak může být fakturuje dvakrát.

### <a name="option-2---bundled-licenses-that-include-mfa"></a>Možnost 2 - dodávat licencí, které zahrnují vícefaktorového ověřování

Nákupu licencí, které zahrnují Azure Multi-Factor Authentication, jako je Azure Active Directory Premium (P1 a P2) nebo Enterprise Mobility + Security (E3 nebo E5) a přiřadit jim tooyour uživatele v Azure Active Directory. 

Pokud použijete tuto možnost, měli byste vytvořit poskytovatele Azure Multi-Factor Authentication, pouze pokud je také potřeba tooprovide dvoustupňové ověřování pro některé uživatele, kteří nemají licence. Jinak může být fakturuje dvakrát. 

### <a name="option-3---mfa-consumption-based-model"></a>Možnost 3 - vícefaktorového ověřování na základě spotřeby modelu

Vytvořte poskytovatele Azure Multi-Factor Authentication v rámci předplatného Azure. Zprostředkovatele Azure MFA jsou prostředky Azure, které se účtují podle vaší smlouvy Enterprise, Azure peněžních závazků nebo platební karty jako všechny ostatní prostředky služby Azure. Tyto zprostředkovatele lze vytvořit pouze v úplné Azure předplatných, mimo jiné předplatná Azure, které mají 0 $ útrat. Omezené předplatných se vytvoří při aktivaci licencí, jako je v možnosti 1 a 2. 

Pokud používáte poskytovatele Azure Multi-Factor Authentication, existují dva modely využití k dispozici, který se účtují prostřednictvím vašeho předplatného Azure:  

1. **Na uživatele** – podnikům, které mají tooenable dvoustupňové ověřování pro pevný počet uživatelů, kteří potřebují pravidelně ověřování. Fakturace za uživatele je založena na hello počet uživatelů v klientovi Azure AD a Azure MFA serveru povolené pro MFA. Pokud uživatelé jsou povolené pro MFA v obou Azure AD a Azure MFA serveru a je povolená synchronizace domény (Azure AD Connect) a potom jsme počet hello větší sadu uživatelů. Pokud není povolená synchronizace domény, pak jsme počet hello součet všechny uživatele s povoleným vícefaktorového ověřování ve službě Azure AD a Azure MFA serveru. Fakturace je systém Commerce poměrné a oznámená toohello denně. 

  > [!NOTE]
  > Fakturace Příklad 1: máte 5 000 uživatele s povoleným MFA ještě dnes. Hello MFA systému vydělí toto číslo 31 a 161.29 uživatelé sestavy pro daný den. Zítra je potřeba povolit 15 více uživatelů, aby hello MFA systému sestav 161.77 uživatelů pro daný den. Hello konec hello fakturační cyklus přidá hello celkový počet uživatelů účtují u vašeho předplatného Azure si tooaround 5 000. 
  >
  > Fakturace příklad 2: máte směs uživatelé s licencí a uživatele bez, takže budete mít jednotlivé uživatele zprostředkovatele Azure MFA toomake až hello rozdíl. Existují 4500 Enterprise Mobility + zabezpečení licencí na klientovi, ale 5 000 uživatele s povoleným pro MFA. Vaše předplatné Azure je účtovány poplatky za 500 uživatelů, účtovány poměrnou částí a každý den jako 16.13 uživatele. 

2. **Za ověření** – podnikům, které mají tooenable dvoustupňové ověřování pro velkou skupinu uživatelů, kteří potřebují zřídka ověřování. Fakturace je založena na hello počet dvoustupňové ověření požadavků přijatých hello cloudové služby Azure MFA, bez ohledu na to, jestli tyto ověření úspěšné nebo byl odepřen. Tato fakturace se zobrazí na Azure použití příkazu v balíčky 10 ověřování a je hlášen toohello obchodování systém denně. 

  > [!NOTE]
  > Fakturace příklad 3: v současné době hello služby Azure MFA obdržela 3,105 dvoustupňové ověření požadavků. Vaše předplatné Azure se fakturuje 310.5 sad ověřování. 

Je důležité toonote může mít licence Azure MFA, ale stále získat účtuje konfiguraci na základě spotřeby. Pokud nastavíte zprostředkovatele Azure MFA na ověřování, fakturuje se pro každý požadavek dvoustupňové ověření, včetně těch, které se provádí uživatelé, kteří mají licence. Pokud nastavíte zprostředkovatele Azure MFA na uživatele v doméně, která není propojená tooyour klienta Azure AD, vám fakturují za povoleného uživatele i v případě, že uživatelé mají licence na Azure AD. 

## <a name="next-steps"></a>Další kroky

- Podrobnosti o cenách najdete v části [ceník služby Azure MFA](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

- Vyberte, zda toodeploy Azure MFA [v hello cloudu nebo místně](multi-factor-authentication-get-started.md)
