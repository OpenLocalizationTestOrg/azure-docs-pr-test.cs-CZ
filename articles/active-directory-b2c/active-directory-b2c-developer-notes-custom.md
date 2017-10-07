---
title: "Azure Active Directory B2C: Poznámky pro vývojáře na pomocí vlastních zásad | Microsoft Docs"
description: "Poznámky pro vývojáře o konfiguraci a údržbu Azure AD B2C pomocí vlastních zásad"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/05/2017
ms.author: joroja
ms.openlocfilehash: 979b8a264eb819ee4a208b9171a53a5ffbf062c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Poznámky k verzi pro verzi public preview služby Azure Active Directory B2C vlastních zásad
Hello sada funkcí vlastních zásad je nyní k dispozici pro vyhodnocení v rámci verze public preview pro všechny Azure Active Directory B2C zákazníků (Azure AD B2C). Tato sada funkcí je cílena na vývojáře pokročilé identity vytváření hello nejvíce komplexní řešení identity.  

Tato sada funkcí v současné době vyžadují vývojáři tooconfigure hello Identity rozhraní Framework přímo prostřednictvím úpravy souborů XML. Tato metoda konfigurace je výkonná a komplexní. Pokročilé vývojáře identity, kteří používají hello Identity Framework prostředí měli naplánovat tooinvest chvíli dokončení návodů a čtení odkaz dokumenty. 

## <a name="features-included-in-this-public-preview"></a>Funkce obsažené v této verzi public preview
Hello nové funkce zavedená ve verzi public preview hello vývojáři mohou provádět hello následující úlohy:<br>

* Autor a nahrání vlastní ověřování uživatele cesty pomocí vlastních zásad. 
   * Popis uživatelské cesty podrobné jako výměny mezi zprostředkovatelem deklarací identity. 
   * Definujte podmíněného větvení v cesty uživatele. 
* Integrate povoleno rozhraní REST API služby ve vaší vlastní ověřování uživatele cesty.  
* Přidejte federaci se zprostředkovatelů identity, které jsou kompatibilní s hello standardní OpenIDConnect. <br>
* Přidejte federaci se zprostředkovatelů identity, které splňovat protokol toohello SAML 2.0. 

## <a name="terms-of-hello-public-preview"></a>Podmínky verze public Preview hello

* Doporučujeme vám toouse hello nové funkce jenom pro účely hodnocení.<br>
* Nové funkce nejsou určeny pro použití v provozním prostředí.<br>
* Smlouvy o úrovni služeb (SLA) se nevztahují toohello nové funkce. <br>
* Žádosti o podporu můžete podává prostřednictvím regulární podpora kanálů. <br>
* Neexistuje žádné přislíbeném datum pro obecnou dostupnost.<br>
* Naše uvážení a z jakéhokoli důvodu můžete Microsoft příznak a odmítnout nebo omezit scénáře a cesty uživatele, které překračují hello oboru tooserve listinou produktu hello Azure AD B2C jako platforma správy (CIAM) přístupu a identit zákazníka.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Odpovědnosti vlastních zásad pro sadu funkcí vývojářů
Konfigurace zásad ruční uděluje přístup na nižší úrovni toohello základní platformu Azure AD B2C a výsledkem hello vytvoření rozhraní jedinečný plně přizpůsobitelná vztah důvěryhodnosti. Počet možných kombinací zprostředkovatelů vlastní identity, vztahy důvěryhodnosti, integrace s externích služeb a podrobné pracovních umístit větší nároky na hello pokročilé vývojáře využívají je.

výhody toofully z hello veřejné verze preview, doporučujeme, aby vývojáři využívání sada funkcí vlastní zásady hello splňovat toohello následující pokyny:
* Seznámení s hello konfigurace jazyk hello modul prostředí Identity a správu tajné klíče nebo klíče.
* Převzít vlastnictví scénáře a vlastní integrace.
* Proveďte testování metodický scénář.
* Postupujte podle vývoj softwaru a pracovní osvědčené postupy s minimálně jeden vývoj a testování prostředí a jeden produkčního prostředí.
* Udržení informovanosti o novém vývoji z hello poskytovatelů identit a službách, které můžete integrovat. Například sledovat určité změny v tajných klíčů a plánovaných a neplánovaných změny toohello služby.
* Nastavte aktivní monitorování a monitorovat rychlost reakce hello produkční prostředí.
* Zachovat aktuální kontaktní e-mailové adresy a zůstat přizpůsobivý toohello Microsoft team Web live e-mailů.
* Akce včas při doporučené toodo tak, že hello týmu Microsoft live-site. 


>[!NOTE]
>Tyto funkce možná časem zahrneme v integrovaných zásad služby Azure AD, přitom přístupnější tooall vývojáři.

## <a name="next-steps"></a>Další kroky
[Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md).
