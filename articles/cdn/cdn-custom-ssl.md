---
title: "AAA \"Povolit protokol HTTPS na vlastní domény služby Azure CDN | Microsoft Docs\""
description: "Zjistěte, jak tooenable HTTPS na koncový bod Azure CDN s vlastní doménu."
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
ms.openlocfilehash: 93746222616c9ed7977ec3b22c38ac1d43b118f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-https-on-an-azure-cdn-custom-domain"></a>Povolit protokol HTTPS na vlastní domény služby Azure CDN

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Podpora protokolu HTTPS pro vlastní domény Azure CDN vám umožní bezpečné obsahu toodeliver přes SSL používá vlastní domény název tooimprove hello zabezpečení dat v průběhu přenosu. Hello-komplexní pracovní postup tooenable HTTPS pro vaše vlastní doména je zjednodušený prostřednictvím povolování jedním kliknutím, kompletní certifikát správy a všechny s bez dalších nákladů.

Je důležité tooensure hello ochrany osobních údajů a integritu dat všech s citlivými daty webové aplikace v průběhu přenosu. Pomocí protokolu HTTPS zajišťuje, aby citlivá data šifrovala při posílání přes hello hello Internetu. Poskytuje vztah důvěryhodnosti, ověřování a chrání vaše webové aplikace před útoky. Azure CDN v současné době podporuje protokol HTTPS na koncový bod CDN. Například pokud vytvoříte koncový bod CDN z Azure CDN (např. https://contoso.azureedge.net), je ve výchozím nastavení povolené HTTPS. Nyní s vlastní doménu HTTPS, můžete povolit zabezpečené doručování pro vlastní doménu (např. https://www.contoso.com) také. 

Mezi klíčové atributy hello HTTPS funkce patří:

- Bez dalších nákladů: existují neplatí pro získání certifikátů nebo obnovení a bez dalších nákladů pro komunikaci přes protokol HTTPS. Platíte jenom odchozí GB z hello CDN.

- Jednoduché povolování: jeden zřizování klikněte na tlačítko je k dispozici z hello [portál Azure](https://portal.azure.com). Můžete také použít rozhraní REST API nebo jiné funkce hello tooenable nástroje pro vývojáře.

- Dokončení správy certifikátů: všechna certifikátů nákup a můžete je zajištěná Správa. Certifikáty jsou automaticky zřízený a obnovit předchozí tooexpiration. Tím se úplně odebere hello riziky přerušení poskytování služeb v důsledku vypršení platnosti certifikátu.

>[!NOTE] 
>Podpora HTTPS předchozí tooenabling, musí jste již vytvořili [vlastní doménu Azure CDN](./cdn-map-content-to-custom-domain.md).

## <a name="step-1-enabling-hello-feature"></a>Krok 1: Povolení funkce hello 

1. V hello [portál Azure](https://portal.azure.com), procházet tooyour Verizon standard nebo premium profil CDN.

2. Hello seznamu koncových bodů klikněte na koncový bod hello obsahující vaši vlastní doménu.

3. Klikněte na tlačítko hello vlastní doménu, pro které chcete tooenable HTTPS.

    ![Okno koncový bod](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Klikněte na tlačítko **na** tooenable HTTPS a uložte změnu hello.

    ![Dialogové okno vlastní protokol HTTPS](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="step-2-domain-validation"></a>Krok 2: Ověření domény

>[!IMPORTANT] 
>Předtím, než HTTPS active na vaši vlastní doménu, je třeba provést ověření domény. Máte 6 obchodní dnů tooapprove hello domény. Žádosti budou zrušeny pomocí žádná schválení do 6 pracovních dnů.  

Po povolení HTTPS na vaši vlastní doménu, bude naše HTTPS poskytovatel certifikátu DigiCert ověřit vlastnictví vaší domény kontaktováním hello osob žádajících o registraci pro vaši doménu, na základě informací o osob žádajících o registraci WHOIS, e-mailem (ve výchozím nastavení) nebo telefon. DigiCert odešle také hello ověření e-mailu toohello níže adresy. Pokud je privátní WHOIS osob žádajících o registraci informace, zkontrolujte, zda že můžete schválit přímo z těchto adres.

>Správce @ správce < vaše domain-name.com > @< vaše domain-name.com >  
>správce webového serveru @< vaše domain-name.com >  
>hostmaster @< vaše domain-name.com >  
>správce pošty @< vaše domain-name.com >


Po přijetí e-mailu hello, máte dvě možnosti ověřování:

1. Můžete schválit všechny budoucí objednávky zadané prostřednictvím hello stejný účet pro hello stejné kořenové domény, například consoto.com. Toto je doporučený postup, pokud plánujete tooadd další vlastní domény v budoucnu pro hello hello stejné kořenové domény.
 
2. Právě můžete schválit hello konkrétní název hostitele použít v této žádosti. Dodatečné schválení se bude vyžadovat pro následné požadavky.

    Příklad e-mailu:
    
    ![Dialogové okno vlastní protokol HTTPS](./media/cdn-custom-ssl/domain-validation-email-example.png)

Po schválení DigiCert přidá certifikát SAN toohello název vaši vlastní doménu. certifikát Hello bude platný jeden rok a bude automaticky obnoveno před jeho platnost vypršela.

## <a name="step-3-wait-for-hello-propagation-then-start-using-your-feature"></a>Krok 3: Počkejte hello šíření pak spusťte pomocí vaší funkce

Po ověření názvu domény hello to bude trvat až too6-8 hodin pro hello vlastní domény HTTPS funkce toobe aktivní. Po dokončení procesu hello stav "vlastní HTTPS" hello v hello portál Azure bude nastaven na příliš "povoleno". HTTPS s vaše vlastní doména je nyní připravena k použití.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

1. *Kdo je poskytovatel certifikátu hello a jaký typ certifikátu se použije?*

    Používáme alternativní názvy předmětu (SAN) certifikátu od DigiCert. Certifikát SAN můžete zabezpečit více plně kvalifikované názvy domény s jedním certifikátem.

2. *Můžete používat vyhrazené certifikát?*
    
    Aktuálně, ale jeho na hello plán.

3. *Co když hello domény ověřovací e-mail neobdrželi z DigiCert?*

    Pokud jste neobdrželi e-mailu do 24 hodin, obraťte se na společnost Microsoft.

4. *Používá certifikát SAN méně bezpečné než vyhrazené certifikát?*
    
    Certifikát sítě SAN, že způsobem hello stejné standardy zabezpečení a šifrování jako vyhrazené certifikátu. Všechny vystavené certifikáty SSL používají algoritmus SHA-256 pro server rozšířené zabezpečení.

5. *Můžete použít vlastní doménu HTTPS s Azure CDN společnosti Akamai?*

    Tato funkce je v současné době pouze s Azure CDN společnosti Verizon k dispozici. Pracujeme na podporu této funkce s Azure CDN společnosti Akamai v nadcházejících měsících hello.


## <a name="next-steps"></a>Další kroky

- Zjistěte, jak tooset nahoru [vlastní doménu na koncový bod Azure CDN](./cdn-map-content-to-custom-domain.md)


