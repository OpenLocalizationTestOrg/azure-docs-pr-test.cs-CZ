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
# <a name="azure-functions-sendgrid-bindings"></a><span data-ttu-id="51c2d-103">Azure sendgrid vám umožňuje funkce vazby</span><span class="sxs-lookup"><span data-stu-id="51c2d-103">Azure Functions SendGrid bindings</span></span>

<span data-ttu-id="51c2d-104">Tento článek vysvětluje, jak tooconfigure a práce s vazeb sendgrid vám umožňuje v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="51c2d-104">This article explains how tooconfigure and work with SendGrid bindings in Azure Functions.</span></span> <span data-ttu-id="51c2d-105">Sendgridu můžete použít Azure Functions toosend přizpůsobené e-mailu prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="51c2d-105">With SendGrid, you can use Azure Functions toosend customized email programmatically.</span></span>

<span data-ttu-id="51c2d-106">Tento článek je referenční informace pro vývojáře Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="51c2d-106">This article is reference information for Azure Functions developers.</span></span> <span data-ttu-id="51c2d-107">Pokud jste nové funkce tooAzure, začněte hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="51c2d-107">If you're new tooAzure Functions, start with hello following resources:</span></span>

<span data-ttu-id="51c2d-108">[Vytvoření první funkce Azure](functions-create-first-azure-function.md).</span><span class="sxs-lookup"><span data-stu-id="51c2d-108">[Create your first Azure Function](functions-create-first-azure-function.md).</span></span> 
<span data-ttu-id="51c2d-109">[C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md), nebo [uzlu](functions-reference-node.md) vývojáře odkazy.</span><span class="sxs-lookup"><span data-stu-id="51c2d-109">[C#](functions-reference-csharp.md), [F#](functions-reference-fsharp.md), or [Node](functions-reference-node.md) developer references.</span></span>

## <a name="functionjson-for-sendgrid-bindings"></a><span data-ttu-id="51c2d-110">Function.JSON pro sendgrid vám umožňuje vazby</span><span class="sxs-lookup"><span data-stu-id="51c2d-110">function.json for SendGrid bindings</span></span>

<span data-ttu-id="51c2d-111">Azure Functions nabízí vazbu výstup pro sendgrid vám umožňuje.</span><span class="sxs-lookup"><span data-stu-id="51c2d-111">Azure Functions provides an output binding for SendGrid.</span></span> <span data-ttu-id="51c2d-112">Hello sendgrid vám umožňuje výstupu vazby vám umožní toocreate a odesílání e-mailu prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="51c2d-112">hello SendGrid output binding enables you toocreate and send email programmatically.</span></span> 

<span data-ttu-id="51c2d-113">Vazba sendgrid vám umožňuje Hello podporuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="51c2d-113">hello SendGrid binding supports hello following properties:</span></span>

- <span data-ttu-id="51c2d-114">`name`: Požadovaná proměnná hello používá v kódu funkce pro požadavek hello nebo textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="51c2d-114">`name` : Required - hello variable name used in function code for hello request or request body.</span></span> <span data-ttu-id="51c2d-115">Tato hodnota je ```$return``` po pouze jeden návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="51c2d-115">This value is ```$return``` when there is only one return value.</span></span> 
- <span data-ttu-id="51c2d-116">`type`: Požadované – musí být nastaven příliš "Sendgridu."</span><span class="sxs-lookup"><span data-stu-id="51c2d-116">`type` : Required - must be set too"sendGrid."</span></span>
- <span data-ttu-id="51c2d-117">`direction`: Požadované – musí být je definováno příliš "."</span><span class="sxs-lookup"><span data-stu-id="51c2d-117">`direction` : Required - must be set too"out."</span></span>
- <span data-ttu-id="51c2d-118">`apiKey`: Požadované – musí být název toohello sadu rozhraní API klíče uložené v nastavení aplikace hello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="51c2d-118">`apiKey` : Required - must be set toohello name of your API key stored in hello Function App's app settings.</span></span>
- <span data-ttu-id="51c2d-119">`to`: hello příjemce e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="51c2d-119">`to` : hello recipient's email address.</span></span>
- <span data-ttu-id="51c2d-120">`from`: hello e-mailovou adresu odesílatele.</span><span class="sxs-lookup"><span data-stu-id="51c2d-120">`from` : hello sender's email address.</span></span>
- <span data-ttu-id="51c2d-121">`subject`: hello předmět e-mailu hello.</span><span class="sxs-lookup"><span data-stu-id="51c2d-121">`subject` : hello subject of hello email.</span></span>
- <span data-ttu-id="51c2d-122">`text`: hello obsahu e-mailů.</span><span class="sxs-lookup"><span data-stu-id="51c2d-122">`text` : hello email content.</span></span>

<span data-ttu-id="51c2d-123">Příklad **function.json**:</span><span class="sxs-lookup"><span data-stu-id="51c2d-123">Example of **function.json**:</span></span>

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
> <span data-ttu-id="51c2d-124">Azure Functions ukládá informace o připojení a klíče rozhraní API jako nastavení aplikace, tak, aby se změnami do vašeho úložiště správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="51c2d-124">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="51c2d-125">Tato akce chrání vaše citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="51c2d-125">This action safeguards your sensitive information.</span></span>
>
>

## <a name="c-example-of-hello-sendgrid-output-binding"></a><span data-ttu-id="51c2d-126">C# příklad hello sendgrid vám umožňuje výstup vazby</span><span class="sxs-lookup"><span data-stu-id="51c2d-126">C# example of hello SendGrid output binding</span></span>

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

## <a name="node-example-of-hello-sendgrid-output-binding"></a><span data-ttu-id="51c2d-127">Příklad uzlu hello sendgrid vám umožňuje výstup vazby</span><span class="sxs-lookup"><span data-stu-id="51c2d-127">Node example of hello SendGrid output binding</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="51c2d-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51c2d-128">Next steps</span></span>
<span data-ttu-id="51c2d-129">Informace o jiných vazby a aktivačních událostí pro Azure Functions najdete v tématu</span><span class="sxs-lookup"><span data-stu-id="51c2d-129">For information about other bindings and triggers for Azure Functions, see</span></span> 
- [<span data-ttu-id="51c2d-130">Azure odkaz funkce pro vývojáře triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="51c2d-130">Azure Functions triggers and bindings developer reference</span></span>](functions-triggers-bindings.md)

- <span data-ttu-id="51c2d-131">[Osvědčené postupy pro Azure Functions](functions-best-practices.md) uvádí některé osvědčené postupy toouse při vytváření Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="51c2d-131">[Best practices for Azure Functions](functions-best-practices.md) Lists some best practices toouse when creating Azure Functions.</span></span>

- <span data-ttu-id="51c2d-132">[Referenční informace pro vývojáře Azure Functions](functions-reference.md) referenční informace pro programátory pro kódování funkcí a definování triggerů a vazeb.</span><span class="sxs-lookup"><span data-stu-id="51c2d-132">[Azure Functions developer reference](functions-reference.md) Programmer reference for coding functions and defining triggers and bindings.</span></span>
