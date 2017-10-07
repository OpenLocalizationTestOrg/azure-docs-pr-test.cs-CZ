---
title: "Azure Active Directory B2C: Řešení potíží se zásadami vlastní | Microsoft Docs"
description: "Další informace o přístupy toosolving chyby při práci s vlastní zásady v Azure Active Directory."
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: joroja
ms.openlocfilehash: b9e1b46df1ddb29cdb90953e4a0d6f1dd085441f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-ad-b2c-custom-policies-and-identity-experience-framework"></a>Řešení potíží s Azure AD B2C vlastní zásady a Identity rozhraní Framework

Pokud používáte Azure Active Directory B2C (Azure AD B2C) vlastní zásady, můžete zaznamenat problémy nastavení hello Identity Framework prostředí v jeho formátu jazyka XML zásad.  Learning toowrite vlastní zásady je možné jako učení o nový jazyk. V tomto článku jsme popisují nástrojů a tipů, které vám pomohou rychle zjistit a řešit problémy. 

> [!NOTE]
> Tento článek se zaměřuje na řešení potíží s vlastních zásad pro konfiguraci Azure AD B2C. Ho nebude adres hello předávající strany aplikace nebo jeho identity knihovny.

## <a name="xml-editing"></a>Úpravy XML

Hello nejběžnější Chyba v nastavení vlastních zásad je nesprávně formátovaný XML. Dobrý editoru XML je téměř nezbytné. Dobrý editoru XML zobrazí XML nativně, barevně označí obsah, předvyplníte běžné podmínky, udržuje elementů XML indexované a můžete ověřit se schématem. Zde jsou uvedeny dvě naše Oblíbené editory XML:

* [Visual Studio Code](https://code.visualstudio.com/)
* [Poznámkový blok ++](https://notepad-plus-plus.org/)

Ověření schématu XML označuje chyby před nahráním souboru XML. V kořenové složce hello sady hello starter získáte definice schématu XML hello TrustFrameworkPolicy_0.3.0.0.xsd. Další informace najdete v dokumentaci hello XML editor, vyhledejte *nástroje XML* a *ověření XML*.

Mohou být užitečné kontrolu pravidla XML. Azure AD B2C odmítne všechny XML formátování chyb, které zjistí. V některých případech nesprávný formát XML může způsobit chybové zprávy, které jsou zavádějící.

## <a name="upload-policies-and-policy-validation"></a>Nahrát zásady a ověření zásad

 Ověření nahrávání souboru XML je automaticky. Nejčastější chyby způsobit toofail nahrávání hello. Ověření zahrnuje hello soubor zásad, který odesíláte. Zahrnuje také hello řetězci souborů souboru k odeslání hello odkazuje příliš (hello soubor zásad předávající strany, hello rozšíření soubor a soubor základní hello). 
 
 Běžné chyby ověření zahrnují následující hello.

Fragment kódu chyby:`... makes a reference tooClaimType with id "displaName" but neither hello policy nor any of its base policies contain such an element`
* Hello typ ClaimType hodnota může být zadáno chybně nebo neexistuje ve schématu hello.
* Typ ClaimType hodnoty musí být definovány alespoň v jedné hello souborů v zásadách hello. 
    Příklad: ` <ClaimType Id="socialIdpUserId">`
* Pokud typ ClaimType je definována v souboru hello rozšíření, ale používá se také v hodnotě TechnicalProfile ve hello základního souboru, odesílání základního souboru hello výsledkem chyba.

Fragment kódu chyby:`...makes a reference tooa ClaimsTransformation with id...`
* Hello příčiny pro hello chyba může být hello stejné jako u hello typ ClaimType chyby.

Fragment kódu chyby:`Reason: User is currently logged as a user of 'yourtenant.onmicrosoft.com' tenant. In order toomanage 'yourtenant.onmicrosoft.com', please login as a user of 'yourtenant.onmicrosoft.com' tenant`
* Zkontrolujte, že hello TenantId hodnota v hello  **\<TrustFrameworkPolicy\>**  a  **\<BasePolicy\>**  elementy odpovídat cílových Azure AD B2C klient.  

## <a name="troubleshoot-hello-runtime"></a>Řešení potíží s hello runtime

* Použití `Run Now` a `https://jwt.io` tootest zásad nezávisle na váš web nebo mobilní aplikaci. Tento web se chová jako aplikace předávající strany. Zobrazuje obsah hello hello JSON Web Token (JWT), je generován zásad služby Azure AD B2C. toocreate testovací aplikace v rámci prostředí Identity, hello použijte následující hodnoty:
    * Název: TestApp
    * Webovou aplikaci nebo webové rozhraní API: Ne
    * Nativní klient: Ne

* tootrace hello výměny zpráv mezi prohlížeče klienta a Azure AD B2C, použijte [Fiddler](http://www.telerik.com/fiddler). Může pomoct vám to znamenat kde nedaří vám dobře slouží uživatele ve vaší kroků Orchestrace.

* V **režimu pro vývoj**, použijte **Application Insights** tootrace hello aktivitu vám dobře slouží Identity Framework činnost koncového uživatele. V **režimu pro vývoj**, můžete sledovat hello výměny deklarací identity mezi hello Identity Framework prostředí a hello různé deklarace poskytovatelů, které jsou definovány technické profilů, jako je například poskytovatelů identit, služeb založených na rozhraní API adresář uživatelského Hello Azure AD B2C a dalším službám, jako například více-Factor-ověřování Azure.  

## <a name="recommended-practices"></a>Doporučené postupy

**Zachovat více verzí vaše scénáře. Seskupte je do projektu s vaší aplikací.** Základní text Hello, rozšíření a předávající strany soubory jsou přímo na sobě navzájem závislé. Uložte jako skupinu. Nové funkce při přidávání tooyour zásady, zachovat samostatné pracovní verze. Fáze pracovní verze ve vašem vlastním systému, které komunikují s kódem aplikace hello souborů.  Vaše aplikace může vyvolat mnoho různých předávající strany zásad v klientovi. Že jsou závislé na hello deklarace identity, které očekávají na základě zásad Azure AD B2C.

**Vývoj a testování technické profily pomocí cesty známé uživatele.** Použití testována starter pack zásady tooset vaše technické profilů. Test je samostatně předtím, než můžete začlenit do vlastní uživatelské cesty.

**Vývoj a testování cesty uživatele pomocí otestované technické profilů.** Změňte kroků Orchestrace hello cesty uživatele postupně. Progresivně sestavení určený scénářů.

## <a name="next-steps"></a>Další kroky

* V Githubu stáhněte si soubor .zip [active-directory-b2c-custom-policy-starterpack] (https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) hello.
