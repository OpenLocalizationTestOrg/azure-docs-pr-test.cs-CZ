---
title: "aaaHow toouse hello sendgrid vám umožňuje e-mailovou službu (Node.js) | Microsoft Docs"
description: "Zjistěte, jak odeslat e-mail s hello sendgrid vám umožňuje e-mailovou službu v Azure. Ukázky kódu jsou vytvořené pomocí rozhraní API Node.js hello."
services: 
documentationcenter: nodejs
author: erikre
manager: wpickett
editor: 
ms.assetid: cac444b4-26b0-45ea-9c3d-eca28d57dacb
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 01/05/2016
ms.author: erikre
ms.openlocfilehash: fd617b6aaa656e7b5dd51c51ebb0db1e848450f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-from-nodejs"></a>Jak tooSend Sendgridu pomocí e-mailu z Node.js
Tato příručka ukazuje, jak tooperform běžné úlohy programování sendgridu e-mailové služby v Azure. Ukázky Hello jsou zapsány pomocí rozhraní API Node.js hello. Hello pokryté scénáře zahrnují **vytváření e-mailu**, **odesílání e-mailu**, **přidávání příloh**, **pomocí filtrů**a **aktualizace vlastností**. Další informace o sendgrid vám umožňuje a odesílání e-mailu, najdete v části hello [další kroky](#next-steps) části.

## <a name="what-is-hello-sendgrid-email-service"></a>Co je hello sendgrid vám umožňuje e-mailovou službu?
Je sendgrid vám umožňuje [cloudový e-mailovou službu] poskytuje spolehlivé [doručování e-mailem transakční], škálovatelnost a analýzu v reálném čase společně s flexibilní rozhraní API, které umožňují snadnou vlastní integrace. Obvyklé scénáře použití sendgrid vám umožňuje patří:

* Automatické odesílání toocustomers potvrzení
* Správa distribučních seznamů pro odesílání zákazníkům měsíční e letáků a speciálních nabídek
* Shromažďování metriky v reálném čase pro takové věci, jako e-mailu blokovaný a odezvy zákazníka
* Generování sestav toohelp identifikovat trendy
* Předávání dotazy zákazníků
* E-mailových oznámení z vaší aplikace

Další informace najdete v tématu [https://sendgrid.com](https://sendgrid.com).

## <a name="create-a-sendgrid-account"></a>Vytvoření účtu sendgrid vám umožňuje
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-hello-sendgrid-nodejs-module"></a>Referenční dokumentace hello sendgrid vám umožňuje modulu Node.js
Hello sendgrid vám umožňuje modul pro Node.js dají nainstalovat přes hello uzel Správce balíčků (npm) pomocí hello následující příkaz:

    npm install sendgrid

Po instalaci můžete požadovat hello modulu ve vaší aplikaci pomocí hello následující kód:

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

modul sendgrid vám umožňuje Hello exportuje hello **sendgrid vám umožňuje** a **e-mailu** funkce.
**Sendgrid vám umožňuje** je odpovědná za zasílání e-mailu pomocí webového rozhraní API, zatímco **e-mailu** zapouzdří e-mailovou zprávu.

## <a name="how-to-create-an-email"></a>Postupy: vytvoření e-mailu
Vytvoření e-mailovou zprávu pomocí modulu sendgrid vám umožňuje hello zahrnuje nejdříve vytvořením e-mailovou zprávu pomocí funkce hello e-mailu a pak jej pomocí funkce hello sendgrid vám umožňuje odeslat. Hello následuje příklad vytvoření nové zprávy pomocí funkce e-mailu hello:

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

Můžete také zprávy ve formátu HTML pro klienty, kteří ho podporují nastavením vlastnosti hello html. Například:

    html: This is a sample <b>HTML<b> email message.

Nastavení vlastností obou hello text nebo html poskytuje bezproblémové nouzového řešení ověření pomocí textového obsahu pro klienty, které nemohou podporovat zprávy ve formátu HTML.

Další informace o všech vlastnostech podporovaných zprostředkovatelem hello funkce e-mailu, najdete v části [sendgrid vám umožňuje nodejs][sendgrid-nodejs].

## <a name="how-to-send-an-email"></a>Postupy: odeslání e-mailu
Po vytvoření e-mailovou zprávu pomocí hello funkce e-mailu, můžete ho pomocí hello webového rozhraní API poskytované sendgrid vám umožňuje odeslat. 

### <a name="web-api"></a>Web API
    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [!NOTE]
> Při hello výše příklady zobrazit v e-mailu objekt a zpětného volání funkci předávání, můžete také přímo vyvolat funkce pro odeslání hello přímo zadáním vlastnosti e-mailu. Například:  
> 
> `````
> sendgrid.send({
> to: 'john@contoso.com',
> from: 'anna@contoso.com',
> subject: 'test mail',
> text: 'This is a sample email message.'
> });
> `````
> 
> 

## <a name="how-to-add-an-attachment"></a>Postupy: Přidání přílohy
Přílohy lze přidat zprávu tooa zadáním hello názvy souboru a cesty ke v hello **soubory** vlastnost. Hello následující příklad ukazuje, odesílání přílohy:

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used toospecify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [!NOTE]
> Při použití hello **soubory** vlastnost hello soubor musí být přístupné prostřednictvím [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile). Pokud chcete tooattach souboru hello hostovaný ve službě Azure Storage, například v kontejneru objektů Blob, je nutné nejprve zkopírovat hello soubor toolocal úložiště nebo tooan Azure jednotky předtím, než může být odeslán jako příloha pomocí hello **soubory** vlastnost.
> 
> 

## <a name="how-to-use-filters-tooenable-footers-and-tracking"></a>Postupy: použití filtrů tooEnable zápatí a sledování
Sendgrid vám umožňuje poskytuje další e-mailové funkce prostřednictvím hello použití filtrů. Jsou to nastavení, které mohou být přidány tooan e-mailové zprávě povolit specifické funkce, například povolení sledování klikněte na tlačítko, Google analytics, sledování, předplatné a tak dále. Úplný seznam filtrů, najdete v části [nastavení filtru][Filter Settings].

Filtry lze použité tooa zpráv pomocí hello **filtry** vlastnost.
Každý filtr je zadána hodnota hash obsahující nastavení pro konkrétní filtru.
Hello následující příklady ukazují hello zápatí a klikněte na tlačítko Sledování filtry:

### <a name="footer"></a>Zápatí stránky
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'footer': {
            'settings': {
                'enable': 1,
                'text/plain': 'This is a text footer.'
            }
        }
    });

    sendgrid.send(email);

### <a name="click-tracking"></a>Klikněte na tlačítko Sledování
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'clicktrack': {
            'settings': {
                'enable': 1
            }
        }
    });

    sendgrid.send(email);

## <a name="how-to-update-email-properties"></a>Postupy: aktualizovat vlastnosti e-mailu
Některé vlastnosti e-mailu můžete přepsat pomocí  **nastavit*vlastnost*** nebo připojených pomocí  **přidat*vlastnost***. Například můžete přidat další příjemce pomocí

    email.addTo('jeff@contoso.com');

pomocí nebo nastavit filtr

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

Další informace najdete v tématu [sendgrid vám umožňuje nodejs][sendgrid-nodejs].

## <a name="how-to-use-additional-sendgrid-services"></a>Postupy: použití služeb další sendgrid vám umožňuje
Sendgrid vám umožňuje nabízí webové rozhraní API, které můžete použít další funkce sendgrid vám umožňuje tooleverage z vaše aplikace Azure. Úplné podrobnosti najdete v tématu hello [dokumentaci k rozhraní API sendgrid vám umožňuje][SendGrid API documentation].

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello hello služby sendgrid vám umožňuje e-mailů, použijte tyto odkazy toolearn Další.

* Úložiště modul Node.js sendgrid vám umožňuje: [sendgrid vám umožňuje nodejs][sendgrid-nodejs]
* Dokumentaci k rozhraní API sendgrid vám umožňuje: <https://sendgrid.com/docs>
* Speciální nabídka sendgrid vám umožňuje Azure zákazníků: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
[cloudový e-mailovou službu]: https://sendgrid.com/email-solutions
[doručování e-mailem transakční]: https://sendgrid.com/transactional-email
