---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity – přehled | Microsoft Docs"
description: "Přehled a obsahu mapy Průvodce aspekty návrhu hybridní Identity"
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 100509c4-0b83-4207-90c8-549ba8372cf7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 10aacb04c90abd100eb56d7c44d590946b052f18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Důležité informace k návrhu hybridní identity Azure Active Directory
Spotřebitelské zařízení jsou Proliferující hello, world podnikové a jsou snadno tooadopt cloudové aplikace software jako služba (SaaS). V důsledku toho je náročné zachování kontroly nad přístup k aplikaci uživatelů napříč platformami interní datová centra a cloudu.  

Řešení společnosti Microsoft identity span místní a cloudové schopnosti, vytváření identitu jednoho uživatele pro ověřování a autorizaci tooall prostředků, bez ohledu na umístění. Říkáme toto hybridní identita. Existují různých možností návrhu a konfigurací pro hybridní identita pomocí řešení Microsoft a v některých případech může být obtížné toodetermine, která kombinace bude nejlépe vyhovovat potřebám vaší organizace hello. 

Tento průvodce aspekty návrhu hybridní Identity vám pomůže toounderstand jak toodesign hybridní řešení identit, které bude nejlépe vyhovovat hello obchodním a technologickým požadavkům vaší organizace.  Tento průvodce Detailně popisuje sérii kroků a úloh, můžete postupovat podle toohelp návrhu hybridní řešení identit, který splňuje požadavky vaší organizace jedinečný. V rámci hello kroků a úloh průvodce hello nabídne hello relevantní technologie a funkce Možnosti k dispozici tooorganizations toomeet funkční a kvalitu služeb (například dostupnost, škálovatelnost, výkon, možnosti správy a zabezpečení) požadavky na úroveň. 

Konkrétně hello hybridní identity návrhu aspekty Průvodce cíle jsou hello tooanswer následující otázky: 

* Co dělat otázky I potřebovat tooask a odpovědět toodrive navrhnout hybridní identity specifické technologii nebo doméně problému, nejlépe splňuje mé požadavky?
* Jakou posloupnost aktivit měli dokončení toodesign hybridní řešení identit pro hello technologii nebo doménu problému? 
* Jaké hybridní identity technologické a konfigurační možnosti jsou k dispozici toohelp mi splnění mých požadavků? Jaké jsou hello kompromis mezi těmito možnostmi, aby bylo možné vybrat nejlepší možnost hello pro firmy?

## <a name="who-is-this-guide-intended-for"></a>Komu je tato příručka určená?
 Ředitel IT, společností CITO, hlavní Identity architekty Enterprise architekty a architekti informačních technologií zodpovědní za návrhy řešení hybridní identity pro střední a velké organizace.

## <a name="how-can-this-guide-help-you"></a>Jak může tento průvodce pomoct?
Můžete použít tento průvodce toounderstand jak toodesign hybridní řešení identit, které je možné toointegrate cloudu na základě systém správy identit s vaší stávající místní řešení identit. 

Hello následující obrázek ukazuje příklad hybridní řešení identit, která umožňuje správci IT toomanage toointegrate jejich aktuální Windows Server Active Directory řešením místně s Microsoft Azure Active Directory tooenable uživatelé toouse jedním Přihlašování (SSO) ve všech aplikacích, které jsou umístěné v cloudu hello a místně.

![](./media/hybrid-id-design-considerations/hybridID-example.png)

Hello výše obrázek je příkladem řešení hybridní identity, které se využívají cloudové služby toointegrate s místních funkcí v pořadí tooprovide procesu ověřování koncový uživatel toohello jeden prostředí a toofacilitate IT Správa Tyto prostředky. I když může se jednat o velmi běžný scénář, každá organizace hybridní identity návrhu je pravděpodobné toobe liší od hello příkladu znázorněno na obrázku 1 z důvodu toodifferent požadavky. 

Tento průvodce popisuje řady kroků a úloh, můžete postupovat podle toodesign hybridní řešení identit, který splňuje požadavky vaší organizace jedinečný. V rámci hello následující kroky a úlohy, hello Průvodce uvede hello relevantní technologie a funkce Možnosti k dispozici tooyou toomeet kvality funkčnosti a služeb požadavky na úroveň pro vaši organizaci.

**Předpoklady**: máte nějaké zkušenosti s Windows serverem, Active Directory Domain Services a Azure Active Directory. V tomto dokumentu předpokládáme, že hledáte jak řešení můžete splňují vaše obchodní potřeby samostatně nebo integrované řešení.

## <a name="design-considerations-overview"></a>Přehled aspektů návrhu
Tento dokument obsahuje sadu kroků a úloh, můžete postupovat podle toodesign hybridní řešení identit, které nejlépe vyhovuje vašim požadavkům. Hello kroky uvádíme v seřazené posloupnosti. Aspekty návrhu, s nimiž se seznámíte v dalších krocích může vyžadovat toochange rozhodnutí, která jste udělali v dřívějších krocích, však kvůli tooconflicting rozhodnutích při návrhu. Každý pokus se uskuteční tooalert jste konflikty návrhů toopotential v dokumentu hello. 

Bude přicházejí na hello návrhu, která nejlépe splňuje vaše požadavky dopracujete projdete kroky hello podle potřeby tooincorporate několikrát všechny aspekty uvedené hello v dokumentu hello. 

| Fáze hybridní Identity | Seznam témat |
| --- | --- |
| Stanovení požadavků na identity |[Určení obchodních potřeb](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Určení požadavků na synchronizaci adresáře](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Stanovení požadavků na službu Multi-Factor authentication](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Definování strategie přijetí hybridní identity](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md) |
| Plán pro zlepšení zabezpečení dat prostřednictvím řešení silné identity |[Určení požadavků na ochranu dat](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Stanovení požadavků na správu obsahu](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Určete požadavky řízení přístupu](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Stanovení požadavků na reakce na incidenty](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Definování strategie ochrany dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) |
| Plánování životního cyklu hybridní identity |[Určení úlohy správy hybridní identity](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Synchronizace správy](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Určení strategie přijetí správy hybridní identity](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |

## <a name="download-this-guide"></a>Stáhněte si tento průvodce
Pdf verze Průvodce aspekty návrhu hybridní Identity hello můžete stáhnout z hello [galerii Technet](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 

