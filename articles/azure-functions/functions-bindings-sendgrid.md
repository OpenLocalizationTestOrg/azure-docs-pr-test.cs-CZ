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
# <a name="azure-functions-sendgrid-bindings"></a><span data-ttu-id="1041c-103">Azure sendgrid vám umožňuje funkce vazby</span><span class="sxs-lookup"><span data-stu-id="1041c-103">Azure Functions SendGrid bindings</span></span>

<span data-ttu-id="1041c-104">Tento článek vysvětluje postup konfigurace a práce s vazeb sendgrid vám umožňuje v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="1041c-104">This article explains how to configure and work with SendGrid bindings in Azure Functions.</span></span> <span data-ttu-id="1041c-105">Sendgridu můžete Azure Functions k odeslání přizpůsobených e-mailu prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="1041c-105">With SendGrid, you can use Azure Functions to send customized email programmatically.</span></span>

<span data-ttu-id="1041c-106">Tento článek je referenční informace pro vývojáře Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="1041c-106">This article is reference information for Azure Functions developers.</span></span> <span data-ttu-id="1041c-107">Pokud začínáte na Azure Functions, začněte s následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="1041c-107">If you're new to Azure Functions, start with the following resources:</span></span>

<span data-ttu-id="1041c-108">[Vytvoření první funkce Azure](functions-create-first-azure-function.md).</span><span class="sxs-lookup"><span data-stu-id="1041c-108">[Create your first Azure Function](functions-create-first-azure-function.md).</span></span> 
<span data-ttu-id="1041c-109">[C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md), nebo [uzlu](functions-reference-node.md) vývojáře odkazy.</span><span class="sxs-lookup"><span data-stu-id="1041c-109">[C#](functions-reference-csharp.md), [F#](functions-reference-fsharp.md), or [Node](functions-reference-node.md) developer references.</span></span>

## <a name="functionjson-for-sendgrid-bindings"></a><span data-ttu-id="1041c-110">Function.JSON pro sendgrid vám umožňuje vazby</span><span class="sxs-lookup"><span data-stu-id="1041c-110">function.json for SendGrid bindings</span></span>

<span data-ttu-id="1041c-111">Azure Functions nabízí vazbu výstup pro sendgrid vám umožňuje.</span><span class="sxs-lookup"><span data-stu-id="1041c-111">Azure Functions provides an output binding for SendGrid.</span></span> <span data-ttu-id="1041c-112">Výstup sendgrid vám umožňuje vytvoření vazby umožňuje vytvořit a odeslat e-mailu prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="1041c-112">The SendGrid output binding enables you to create and send email programmatically.</span></span> 

<span data-ttu-id="1041c-113">Vazba sendgrid vám umožňuje podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="1041c-113">The SendGrid binding supports the following properties:</span></span>

- <span data-ttu-id="1041c-114">`name`: Požadovaná proměnná používá v kódu funkce pro požadavek nebo textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="1041c-114">`name` : Required - the variable name used in function code for the request or request body.</span></span> <span data-ttu-id="1041c-115">Tato hodnota je ```$return``` po pouze jeden návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1041c-115">This value is ```$return``` when there is only one return value.</span></span> 
- <span data-ttu-id="1041c-116">`type`: Požadované – musí být nastavena na "Sendgridu."</span><span class="sxs-lookup"><span data-stu-id="1041c-116">`type` : Required - must be set to "sendGrid."</span></span>
- <span data-ttu-id="1041c-117">`direction`: Požadované – musí být nastavena na "out."</span><span class="sxs-lookup"><span data-stu-id="1041c-117">`direction` : Required - must be set to "out."</span></span>
- <span data-ttu-id="1041c-118">`apiKey`: Požadované – musí být nastavena na název vašeho rozhraní API klíče uložené v aplikaci funkce nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1041c-118">`apiKey` : Required - must be set to the name of your API key stored in the Function App's app settings.</span></span>
- <span data-ttu-id="1041c-119">`to`: příjemce e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="1041c-119">`to` : the recipient's email address.</span></span>
- <span data-ttu-id="1041c-120">`from`: e-mailovou adresu odesílatele.</span><span class="sxs-lookup"><span data-stu-id="1041c-120">`from` : the sender's email address.</span></span>
- <span data-ttu-id="1041c-121">`subject`: Předmět e-mailu.</span><span class="sxs-lookup"><span data-stu-id="1041c-121">`subject` : the subject of the email.</span></span>
- <span data-ttu-id="1041c-122">`text`: obsah e-mailu.</span><span class="sxs-lookup"><span data-stu-id="1041c-122">`text` : the email content.</span></span>

<span data-ttu-id="1041c-123">Příklad **function.json**:</span><span class="sxs-lookup"><span data-stu-id="1041c-123">Example of **function.json**:</span></span>

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
> <span data-ttu-id="1041c-124">Azure Functions ukládá informace o připojení a klíče rozhraní API jako nastavení aplikace, tak, aby se změnami do vašeho úložiště správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="1041c-124">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="1041c-125">Tato akce chrání vaše citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="1041c-125">This action safeguards your sensitive information.</span></span>
>
>

## <a name="c-example-of-the-sendgrid-output-binding"></a><span data-ttu-id="1041c-126">C# příklad sendgrid vám umožňuje výstup vazby</span><span class="sxs-lookup"><span data-stu-id="1041c-126">C# example of the SendGrid output binding</span></span>

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

## <a name="node-example-of-the-sendgrid-output-binding"></a><span data-ttu-id="1041c-127">Příklad uzlu sendgrid vám umožňuje výstup vazby</span><span class="sxs-lookup"><span data-stu-id="1041c-127">Node example of the SendGrid output binding</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="1041c-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1041c-128">Next steps</span></span>
<span data-ttu-id="1041c-129">Informace o jiných vazby a aktivačních událostí pro Azure Functions najdete v tématu</span><span class="sxs-lookup"><span data-stu-id="1041c-129">For information about other bindings and triggers for Azure Functions, see</span></span> 
- [<span data-ttu-id="1041c-130">Azure odkaz funkce pro vývojáře triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="1041c-130">Azure Functions triggers and bindings developer reference</span></span>](functions-triggers-bindings.md)

- <span data-ttu-id="1041c-131">[Osvědčené postupy pro Azure Functions](functions-best-practices.md) uvádí některé osvědčené postupy pro použití při vytváření Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="1041c-131">[Best practices for Azure Functions](functions-best-practices.md) Lists some best practices to use when creating Azure Functions.</span></span>

- <span data-ttu-id="1041c-132">[Referenční informace pro vývojáře Azure Functions](functions-reference.md) referenční informace pro programátory pro kódování funkcí a definování triggerů a vazeb.</span><span class="sxs-lookup"><span data-stu-id="1041c-132">[Azure Functions developer reference](functions-reference.md) Programmer reference for coding functions and defining triggers and bindings.</span></span>
