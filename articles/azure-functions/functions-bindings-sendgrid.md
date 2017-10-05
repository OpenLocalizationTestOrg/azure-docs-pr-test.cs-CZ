---
title: "Azure vazby sendgrid vám umožňuje funkce | Microsoft Docs"
description: "Odkaz na Azure vazby sendgrid vám umožňuje funkce"
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/16/2017
ms.author: rachelap
ms.openlocfilehash: 445a40a884e648cdb2a57f8ef43bed4f8a3efcf2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-sendgrid-bindings"></a>Azure sendgrid vám umožňuje funkce vazby

Tento článek vysvětluje postup konfigurace a práce s vazeb sendgrid vám umožňuje v Azure Functions. Sendgridu můžete Azure Functions k odeslání přizpůsobených e-mailu prostřednictvím kódu programu.

Tento článek je referenční informace pro vývojáře Azure Functions. Pokud začínáte na Azure Functions, začněte s následující prostředky:

[Vytvoření první funkce Azure](functions-create-first-azure-function.md). 
[C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md), nebo [uzlu](functions-reference-node.md) vývojáře odkazy.

## <a name="functionjson-for-sendgrid-bindings"></a>Function.JSON pro sendgrid vám umožňuje vazby

Azure Functions nabízí vazbu výstup pro sendgrid vám umožňuje. Výstup sendgrid vám umožňuje vytvoření vazby umožňuje vytvořit a odeslat e-mailu prostřednictvím kódu programu. 

Vazba sendgrid vám umožňuje podporuje následující vlastnosti:

- `name`: Požadovaná proměnná používá v kódu funkce pro požadavek nebo textu požadavku. Tato hodnota je ```$return``` po pouze jeden návratovou hodnotu. 
- `type`: Požadované – musí být nastavena na "Sendgridu."
- `direction`: Požadované – musí být nastavena na "out."
- `apiKey`: Požadované – musí být nastavena na název vašeho rozhraní API klíče uložené v aplikaci funkce nastavení aplikace.
- `to`: příjemce e-mailovou adresu.
- `from`: e-mailovou adresu odesílatele.
- `subject`: Předmět e-mailu.
- `text`: obsah e-mailu.

Příklad **function.json**:

```json 
{
    "bindings": [
        {
            "name": "$return",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

> [!NOTE]
> Azure Functions ukládá informace o připojení a klíče rozhraní API jako nastavení aplikace, tak, aby se změnami do vašeho úložiště správy zdrojového kódu. Tato akce chrání vaše citlivé informace.
>
>

## <a name="c-example-of-the-sendgrid-output-binding"></a>C# příklad sendgrid vám umožňuje výstup vazby

```csharp
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static Mail Run(TraceWriter log, string input, out Mail message)
{
     message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    personalization.AddTo(new Email("recipient@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

## <a name="node-example-of-the-sendgrid-output-binding"></a>Příklad uzlu sendgrid vám umožňuje výstup vazby

```javascript
module.exports = function (context, input) {    
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: "sender@contoso.com",        
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};

```

## <a name="next-steps"></a>Další kroky
Informace o jiných vazby a aktivačních událostí pro Azure Functions najdete v tématu 
- [Azure odkaz funkce pro vývojáře triggerů a vazeb](functions-triggers-bindings.md)

- [Osvědčené postupy pro Azure Functions](functions-best-practices.md) uvádí některé osvědčené postupy pro použití při vytváření Azure Functions.

- [Referenční informace pro vývojáře Azure Functions](functions-reference.md) referenční informace pro programátory pro kódování funkcí a definování triggerů a vazeb.
