---
title: "aaaHow toouse sendgrid vám umožňuje v Azure Functions | Microsoft Docs"
description: "Ukazuje, jak toouse sendgrid vám umožňuje v Azure Functions"
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/31/2017
ms.author: rachelap
ms.openlocfilehash: a0ffdae04e5924c773d2d26427626fc1f570f7ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-sendgrid-in-azure-functions"></a>Jak toouse sendgrid vám umožňuje v Azure Functions

## <a name="sendgrid-overview"></a>Přehled sendgrid vám umožňuje

Azure Functions podporuje sendgrid vám umožňuje výstupní vazby tooenable funkce e-mailové zprávy toosend zadání několika řádků kódu a účet sendgrid vám umožňuje.

toouse rozhraní API hello sendgrid vám umožňuje v funkci Azure, musíte mít [sendgrid vám umožňuje účet](http://SendGrid.com). Kromě toho musí mít klíč rozhraní API sendgrid vám umožňuje. Přihlaste se tooyour sendgrid vám umožňuje účet a klikněte na tlačítko **nastavení** pak **klíč rozhraní API** klíč toogenerate rozhraní API. Uchovávejte tento klíč k dispozici při používání v nadcházející kroku.

Nyní je připraven toocreate funkce Azure aplikace.

## <a name="create-an-azure-function-app"></a>Vytvoření aplikace Azure Function App 

Aplikace Azure funkce jsou kontejnery pro jeden nebo více Azure functions. Řešení Azure functions se právě který – funkce. Jednotlivé funkce Azure je vázané tooone aktivační události, které je událost, která způsobí, že funkce toorun hello.
Jednotlivé funkce může obsahovat libovolný počet vstup nebo výstup vazby. Vazby jsou služby, které můžete použít ve funkci. Sendgrid vám umožňuje je výstup vazby, které můžete použít toosend e-mailu. 

1. Přihlaste se toohello portál Azure a [vytvoření aplikace Azure funkce](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) nebo otevřete stávající funkce aplikace. 
2. Vytvoření Azure funkce. tookeep to jednoduché, zvolte ruční aktivační události a C#. 

 ![Vytvoření Azure funkce](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a>Sendgrid vám umožňuje nakonfigurovat pro použití v aplikaci Azure – funkce

Musíte si klíč rozhraní API sendgrid vám umožňuje uložit jako toouse nastavení aplikace je ve funkci. pole ApiKey Hello není klíč skutečné sendgrid vám umožňuje rozhraní API, ale definovat nastavení aplikace, které představuje skutečný klíč rozhraní API. Ukládání klíče tímto způsobem je pro zabezpečení, protože je oddělená od žádné kódu nebo soubory, které může zkontrolovat do správy zdrojového kódu.

- Vytvoření **AppSettings** klíče v aplikaci funkce **nastavení aplikace**.

 ![Vytvoření Azure funkce](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a>Konfigurovat sendgrid vám umožňuje výstup vazby

Je k dispozici jako Azure sendgrid vám umožňuje funkce výstup vazby. toocreate sendgrid vám umožňuje výstup vazby:

1. Přejděte toohello **integrací** kartě hello funkce v hello portálu Azure.
2. Klikněte na tlačítko **nový výstupní** toocreate sendgrid vám umožňuje výstup vazby.
3. Vyplňte hello **klíč rozhraní API** a **název parametru zpráva** vlastnosti. Pokud chcete, můžete zadat nyní hello další vlastnosti, nebo místo toho je code. Tato nastavení mohou být použity jako výchozí.

 ![Konfigurovat sendgrid vám umožňuje výstup vazby](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

Přidání funkce tooa vazby vytvoří soubor s názvem **function.json** ve složce vaší funkce. Tento soubor obsahuje všechny hello stejné informace, který se zobrazí v hello Azure funkce **integrací** kartě, ale ve formátu Json formátu. Nastavení hello **ApiKey**, **zpráva**, a **z** pole vytvořit následující záznamy v hello hello **function.json** souboru: 

```json
{
  "bindings": [    
    {
      "type": "sendGrid",
      "name": "message",
      "apiKey": "SendGridKey",
      "direction": "out",
      "from": "azure@contoso.com"
    }
  ],
  "disabled": false
}
```

Pokud dáváte přednost, můžete to upravovat soubor sami přímo.

Teď, když je vytvořen a nakonfigurován hello aplikaci funkce a funkce, můžete napsat kód toosend hello e-mailu.

## <a name="write-code-that-creates-and-sends-email"></a>Psaní kódu, které vytváří a odesílá e-mailu

Hello sendgrid vám umožňuje rozhraní API obsahuje všechny hello příkazy potřebujete toocreate a e-mailovou zprávu.  

- Nahraďte kód hello ve funkci hello hello následující kód:

```cs
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static void Run(TraceWriter log, string input, out Mail message)
{
    message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    // change tooemail of recipient
    personalization.AddTo(new Email("MoreEmailPlease@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

Všimněte si hello první řádek obsahuje hello ```#r``` direktiva, která odkazuje na sestavení sendgrid vám umožňuje hello. Potom můžete použít ```using``` toomore příkaz snadný přístup k hello objekty v daném oboru názvů. V kódu hello vytvoření instancí ```Mail```, ```Personalization```, a ```Content``` objekty z hello sendgrid vám umožňuje rozhraní API, které tvoří hello e-mailu. Při návratu uvítací zprávu, přináší sendgrid vám umožňuje ho. 

Hello funkce podpis také obsahuje další parametr typu out ```Mail``` s názvem ```message```. Obě vstup a výstup vazby express sami jako parametry funkce v kódu. 

2. Otestujte svůj kód kliknutím **Test** a zadání zprávy do hello **text žádosti** pole a pak kliknete na hello **spustit** tlačítko.

 ![Otestujte svůj kód](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. Zkontrolujte, že sendgrid vám umožňuje odeslat e-mailu hello tooverify e-mailu. Měl by přejděte toohello adresu v kódu hello z kroku 1 a obsahovat uvítací zprávu z hello **text žádosti**.

## <a name="next-steps"></a>Další kroky
Tento článek vám ukázal, jak toouse hello toocreate služby sendgrid vám umožňuje a odeslání e-mailu. toolearn Další informace o použití Azure Functions v aplikacích, najdete v části hello následující témata: 

- [Osvědčené postupy pro Azure Functions](functions-best-practices.md) uvádí některé osvědčené postupy toouse při vytváření Azure Functions.

- [Referenční informace pro vývojáře Azure Functions](functions-reference.md) referenční informace pro programátory pro kódování funkcí a definování triggerů a vazeb.

- [Testování Azure Functions](functions-test-a-function.md) popisuje různé nástroje a techniky pro testování funkcí.