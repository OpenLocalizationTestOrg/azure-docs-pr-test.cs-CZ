---
title: "vazby funkce sendgrid vám umožňuje aaaAzure | Microsoft Docs"
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
ms.openlocfilehash: 10a3837875eb6ae18e6c789bcf64cc401cf5f26a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-sendgrid-bindings"></a>Azure sendgrid vám umožňuje funkce vazby

Tento článek vysvětluje, jak tooconfigure a práce s vazeb sendgrid vám umožňuje v Azure Functions. Sendgridu můžete použít Azure Functions toosend přizpůsobené e-mailu prostřednictvím kódu programu.

Tento článek je referenční informace pro vývojáře Azure Functions. Pokud jste nové funkce tooAzure, začněte hello následující prostředky:

[Vytvoření první funkce Azure](functions-create-first-azure-function.md). 
[C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md), nebo [uzlu](functions-reference-node.md) vývojáře odkazy.

## <a name="functionjson-for-sendgrid-bindings"></a>Function.JSON pro sendgrid vám umožňuje vazby

Azure Functions nabízí vazbu výstup pro sendgrid vám umožňuje. Hello sendgrid vám umožňuje výstupu vazby vám umožní toocreate a odesílání e-mailu prostřednictvím kódu programu. 

Vazba sendgrid vám umožňuje Hello podporuje hello následující vlastnosti:

- `name`: Požadovaná proměnná hello používá v kódu funkce pro požadavek hello nebo textu požadavku. Tato hodnota je ```$return``` po pouze jeden návratovou hodnotu. 
- `type`: Požadované – musí být nastaven příliš "Sendgridu."
- `direction`: Požadované – musí být je definováno příliš "."
- `apiKey`: Požadované – musí být název toohello sadu rozhraní API klíče uložené v nastavení aplikace hello funkce aplikace.
- `to`: hello příjemce e-mailovou adresu.
- `from`: hello e-mailovou adresu odesílatele.
- `subject`: hello předmět e-mailu hello.
- `text`: hello obsahu e-mailů.

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

## <a name="c-example-of-hello-sendgrid-output-binding"></a>C# příklad hello sendgrid vám umožňuje výstup vazby

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

## <a name="node-example-of-hello-sendgrid-output-binding"></a>Příklad uzlu hello sendgrid vám umožňuje výstup vazby

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

- [Osvědčené postupy pro Azure Functions](functions-best-practices.md) uvádí některé osvědčené postupy toouse při vytváření Azure Functions.

- [Referenční informace pro vývojáře Azure Functions](functions-reference.md) referenční informace pro programátory pro kódování funkcí a definování triggerů a vazeb.
