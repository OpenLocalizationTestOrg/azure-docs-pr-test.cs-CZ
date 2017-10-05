---
title: "Povolit protokol HTTPS na vlastní domény služby Azure CDN | Microsoft Docs"
description: "Zjistěte, jak povolit protokol HTTPS na koncový bod Azure CDN s vlastní doménu."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: casoper
ms.openlocfilehash: b334ba6bbec1d0a7e23a514174bffae01c7fff05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-https-on-an-azure-cdn-custom-domain"></a>Povolit protokol HTTPS na vlastní domény služby Azure CDN

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Podpora protokolu HTTPS pro vlastní domény Azure CDN umožňuje doručovat bezpečné obsahu přes SSL pomocí vlastní název domény pro zlepšení zabezpečení dat v průběhu přenosu. V pracovním postupu začátku do konce povolit HTTPS pro vaše vlastní doména je zjednodušený prostřednictvím povolování jedním kliknutím, kompletní certifikát správy a všechny s bez dalších nákladů.

Je důležité k zajištění ochrany osobních údajů a integritu dat všech s citlivými daty webové aplikace v průběhu přenosu. Pomocí protokolu HTTPS zajišťuje, aby citlivá data šifrovala při posílání přes internet. Poskytuje vztah důvěryhodnosti, ověřování a chrání vaše webové aplikace před útoky. Azure CDN v současné době podporuje protokol HTTPS na koncový bod CDN. Například pokud vytvoříte koncový bod CDN z Azure CDN (např. https://contoso.azureedge.net), je ve výchozím nastavení povolené HTTPS. Nyní s vlastní doménu HTTPS, můžete povolit zabezpečené doručování pro vlastní doménu (např. https://www.contoso.com) také. 

Mezi klíčové atributy HTTPS funkce patří:

- Bez dalších nákladů: existují neplatí pro získání certifikátů nebo obnovení a bez dalších nákladů pro komunikaci přes protokol HTTPS. Právě platíte za GB odchozí od CDN.

- Jednoduché povolování: jeden zřizování klikněte na tlačítko je k dispozici z [portál Azure](https://portal.azure.com). K povolení této funkce můžete také použít rozhraní REST API nebo jiné nástroje pro vývojáře.

- Dokončení správy certifikátů: všechna certifikátů nákup a můžete je zajištěná Správa. Certifikáty jsou automaticky zřizovat a obnovit před vypršením platnosti. Tím se úplně odebere rizika přerušení poskytování služeb v důsledku vypršení platnosti certifikátu.

>[!NOTE] 
>Před povolením podpora protokolu HTTPS, musí jste již vytvořili [vlastní doménu Azure CDN](./cdn-map-content-to-custom-domain.md).

## <a name="step-1-enabling-the-feature"></a>Krok 1: Povolení funkce 

1. V [portál Azure](https://portal.azure.com), přejděte na svůj profil Verizon standard nebo premium CDN.

2. V seznamu koncových bodů klikněte na koncový bod, který obsahuje vaše vlastní doména.

3. Klikněte na tlačítko vlastní doménu, pro který chcete povolit protokol HTTPS.

    ![Okno koncový bod](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Klikněte na tlačítko **na** povolit protokol HTTPS a uložte změny.

    ![Dialogové okno vlastní protokol HTTPS](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="step-2-domain-validation"></a>Krok 2: Ověření domény

>[!IMPORTANT] 
>Předtím, než HTTPS active na vaši vlastní doménu, je třeba provést ověření domény. Máte 6 pracovních dnů ke schválení domény. Žádosti budou zrušeny pomocí žádná schválení do 6 pracovních dnů.  

Po povolení HTTPS na vaši vlastní doménu, bude naše HTTPS poskytovatel certifikátu DigiCert ověřit vlastnictví vaší domény kontaktováním osob žádajících o registraci pro vaši doménu, na základě informací o osob žádajících o registraci WHOIS, e-mailem (ve výchozím nastavení) nebo telefon. DigiCert taky odesílat v ověřovací e-mailu následující adresy. Pokud je privátní WHOIS osob žádajících o registraci informace, zkontrolujte, zda že můžete schválit přímo z těchto adres.

>Správce @ správce < vaše domain-name.com > @< vaše domain-name.com >  
>správce webového serveru @< vaše domain-name.com >  
>hostmaster @< vaše domain-name.com >  
>správce pošty @< vaše domain-name.com >


Po přijetí e-mailu, máte dvě možnosti ověřování:

1. Můžete schválit všechny budoucí objednávky zadané přes stejný účet pro stejnou kořenovou doménu, například consoto.com. Toto je doporučený postup, pokud plánujete přidat další vlastní domény v budoucnosti pro stejnou kořenovou doménu.
 
2. Právě můžete schválit na konkrétního hostitele použitý v této žádosti. Dodatečné schválení se bude vyžadovat pro následné požadavky.

    Příklad e-mailu:
    
    ![Dialogové okno vlastní protokol HTTPS](./media/cdn-custom-ssl/domain-validation-email-example.png)

Po schválení DigiCert přidá vlastní název domény na síti SAN certifikát. Tento certifikát bude platný jeden rok a bude automaticky obnoveno před jeho platnost vypršela.

## <a name="step-3-wait-for-the-propagation-then-start-using-your-feature"></a>Krok 3: Počkejte šíření pak spusťte pomocí vaší funkce

Po ověření názvu domény se bude trvat až 6 až 8 hodin pro funkci HTTPS vlastní domény tak, aby byl aktivní. Po dokončení procesu "vlastní HTTPS" stav na portálu Azure bude nastaven na "Povoleno". HTTPS s vaše vlastní doména je nyní připravena k použití.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

1. *Kdo je poskytovatel certifikátu a jaký typ certifikátu se použije?*

    Používáme alternativní názvy předmětu (SAN) certifikátu od DigiCert. Certifikát SAN můžete zabezpečit více plně kvalifikované názvy domény s jedním certifikátem.

2. *Můžete používat vyhrazené certifikát?*
    
    Aktuálně ale je plánovaná.

3. *Co když neobdrželi ověření domény e-mailu z DigiCert?*

    Pokud jste neobdrželi e-mailu do 24 hodin, obraťte se na společnost Microsoft.

4. *Používá certifikát SAN méně bezpečné než vyhrazené certifikát?*
    
    Certifikát SAN standardům stejné šifrování a zabezpečení jako vyhrazené certifikátu. Všechny vystavené certifikáty SSL používají algoritmus SHA-256 pro server rozšířené zabezpečení.

5. *Můžete použít vlastní doménu HTTPS s Azure CDN společnosti Akamai?*

    Tato funkce je v současné době pouze s Azure CDN společnosti Verizon k dispozici. Pracujeme na podporu této funkce s Azure CDN společnosti Akamai v nadcházejících měsících.


## <a name="next-steps"></a>Další kroky

- Zjistěte, jak nastavit [vlastní doménu na koncový bod Azure CDN](./cdn-map-content-to-custom-domain.md)


