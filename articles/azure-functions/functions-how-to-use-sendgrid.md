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
# <a name="how-toouse-sendgrid-in-azure-functions"></a><span data-ttu-id="92b91-103">Jak toouse sendgrid vám umožňuje v Azure Functions</span><span class="sxs-lookup"><span data-stu-id="92b91-103">How toouse SendGrid in Azure Functions</span></span>

## <a name="sendgrid-overview"></a><span data-ttu-id="92b91-104">Přehled sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="92b91-104">SendGrid Overview</span></span>

<span data-ttu-id="92b91-105">Azure Functions podporuje sendgrid vám umožňuje výstupní vazby tooenable funkce e-mailové zprávy toosend zadání několika řádků kódu a účet sendgrid vám umožňuje.</span><span class="sxs-lookup"><span data-stu-id="92b91-105">Azure Functions supports SendGrid output bindings tooenable your functions toosend email messages with a few lines of code and a SendGrid account.</span></span>

<span data-ttu-id="92b91-106">toouse rozhraní API hello sendgrid vám umožňuje v funkci Azure, musíte mít [sendgrid vám umožňuje účet](http://SendGrid.com).</span><span class="sxs-lookup"><span data-stu-id="92b91-106">toouse hello SendGrid API in an Azure Function, you must have a [SendGrid account](http://SendGrid.com).</span></span> <span data-ttu-id="92b91-107">Kromě toho musí mít klíč rozhraní API sendgrid vám umožňuje.</span><span class="sxs-lookup"><span data-stu-id="92b91-107">Additionally, you must have a SendGrid API Key.</span></span> <span data-ttu-id="92b91-108">Přihlaste se tooyour sendgrid vám umožňuje účet a klikněte na tlačítko **nastavení** pak **klíč rozhraní API** klíč toogenerate rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="92b91-108">Log in tooyour SendGrid account and click **Settings** then **API Key** toogenerate an API key.</span></span> <span data-ttu-id="92b91-109">Uchovávejte tento klíč k dispozici při používání v nadcházející kroku.</span><span class="sxs-lookup"><span data-stu-id="92b91-109">Keep this key available as you use it in an upcoming step.</span></span>

<span data-ttu-id="92b91-110">Nyní je připraven toocreate funkce Azure aplikace.</span><span class="sxs-lookup"><span data-stu-id="92b91-110">You are now ready toocreate an Azure Function app.</span></span>

## <a name="create-an-azure-function-app"></a><span data-ttu-id="92b91-111">Vytvoření aplikace Azure Function App</span><span class="sxs-lookup"><span data-stu-id="92b91-111">Create an Azure Function app</span></span> 

<span data-ttu-id="92b91-112">Aplikace Azure funkce jsou kontejnery pro jeden nebo více Azure functions.</span><span class="sxs-lookup"><span data-stu-id="92b91-112">Azure Function Apps are containers for one or more Azure functions.</span></span> <span data-ttu-id="92b91-113">Řešení Azure functions se právě který – funkce.</span><span class="sxs-lookup"><span data-stu-id="92b91-113">Azure functions are just that - a function.</span></span> <span data-ttu-id="92b91-114">Jednotlivé funkce Azure je vázané tooone aktivační události, které je událost, která způsobí, že funkce toorun hello.</span><span class="sxs-lookup"><span data-stu-id="92b91-114">Each Azure function is tied tooone trigger, which is an event that causes hello function toorun.</span></span>
<span data-ttu-id="92b91-115">Jednotlivé funkce může obsahovat libovolný počet vstup nebo výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="92b91-115">Each function can contain any number of input or output bindings.</span></span> <span data-ttu-id="92b91-116">Vazby jsou služby, které můžete použít ve funkci.</span><span class="sxs-lookup"><span data-stu-id="92b91-116">Bindings are services that you can use in a function.</span></span> <span data-ttu-id="92b91-117">Sendgrid vám umožňuje je výstup vazby, které můžete použít toosend e-mailu.</span><span class="sxs-lookup"><span data-stu-id="92b91-117">SendGrid is an output binding you can use toosend email.</span></span> 

1. <span data-ttu-id="92b91-118">Přihlaste se toohello portál Azure a [vytvoření aplikace Azure funkce](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) nebo otevřete stávající funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="92b91-118">Log in toohello Azure portal and [create an Azure Function App](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) or open an existing Function app.</span></span> 
2. <span data-ttu-id="92b91-119">Vytvoření Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="92b91-119">Create an Azure function.</span></span> <span data-ttu-id="92b91-120">tookeep to jednoduché, zvolte ruční aktivační události a C#.</span><span class="sxs-lookup"><span data-stu-id="92b91-120">tookeep it simple, choose a manual trigger and C#.</span></span> 

 ![Vytvoření Azure funkce](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a><span data-ttu-id="92b91-122">Sendgrid vám umožňuje nakonfigurovat pro použití v aplikaci Azure – funkce</span><span class="sxs-lookup"><span data-stu-id="92b91-122">Configure SendGrid for use in an Azure Function app</span></span>

<span data-ttu-id="92b91-123">Musíte si klíč rozhraní API sendgrid vám umožňuje uložit jako toouse nastavení aplikace je ve funkci.</span><span class="sxs-lookup"><span data-stu-id="92b91-123">You must store your SendGrid API Key as an app setting toouse it in a function.</span></span> <span data-ttu-id="92b91-124">pole ApiKey Hello není klíč skutečné sendgrid vám umožňuje rozhraní API, ale definovat nastavení aplikace, které představuje skutečný klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="92b91-124">hello ApiKey field is not your actual SendGrid API key, but an app setting you define that represents your actual API key.</span></span> <span data-ttu-id="92b91-125">Ukládání klíče tímto způsobem je pro zabezpečení, protože je oddělená od žádné kódu nebo soubory, které může zkontrolovat do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="92b91-125">Storing your key this way is for security, since it is separate from any code or files that might be checked into source code control.</span></span>

- <span data-ttu-id="92b91-126">Vytvoření **AppSettings** klíče v aplikaci funkce **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="92b91-126">Create an **AppSettings** key in your function app's **Application Settings**.</span></span>

 ![Vytvoření Azure funkce](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a><span data-ttu-id="92b91-128">Konfigurovat sendgrid vám umožňuje výstup vazby</span><span class="sxs-lookup"><span data-stu-id="92b91-128">Configure SendGrid output bindings</span></span>

<span data-ttu-id="92b91-129">Je k dispozici jako Azure sendgrid vám umožňuje funkce výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="92b91-129">SendGrid is available as an Azure function output binding.</span></span> <span data-ttu-id="92b91-130">toocreate sendgrid vám umožňuje výstup vazby:</span><span class="sxs-lookup"><span data-stu-id="92b91-130">toocreate a SendGrid output binding:</span></span>

1. <span data-ttu-id="92b91-131">Přejděte toohello **integrací** kartě hello funkce v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="92b91-131">Go toohello **Integrate** tab of hello function in hello Azure portal.</span></span>
2. <span data-ttu-id="92b91-132">Klikněte na tlačítko **nový výstupní** toocreate sendgrid vám umožňuje výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="92b91-132">Click **New Output** toocreate a SendGrid output binding.</span></span>
3. <span data-ttu-id="92b91-133">Vyplňte hello **klíč rozhraní API** a **název parametru zpráva** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="92b91-133">Fill in hello **API Key** and **Message parameter name** properties.</span></span> <span data-ttu-id="92b91-134">Pokud chcete, můžete zadat nyní hello další vlastnosti, nebo místo toho je code.</span><span class="sxs-lookup"><span data-stu-id="92b91-134">If you want, you can enter hello other properties now, or code them instead.</span></span> <span data-ttu-id="92b91-135">Tato nastavení mohou být použity jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="92b91-135">These settings can be used as defaults.</span></span>

 ![Konfigurovat sendgrid vám umožňuje výstup vazby](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

<span data-ttu-id="92b91-137">Přidání funkce tooa vazby vytvoří soubor s názvem **function.json** ve složce vaší funkce.</span><span class="sxs-lookup"><span data-stu-id="92b91-137">Adding a binding tooa function creates a file called **function.json** in your function's folder.</span></span> <span data-ttu-id="92b91-138">Tento soubor obsahuje všechny hello stejné informace, který se zobrazí v hello Azure funkce **integrací** kartě, ale ve formátu Json formátu.</span><span class="sxs-lookup"><span data-stu-id="92b91-138">This file contains all hello same information that you see in hello Azure function's **Integrate** tab, but in Json format.</span></span> <span data-ttu-id="92b91-139">Nastavení hello **ApiKey**, **zpráva**, a **z** pole vytvořit následující záznamy v hello hello **function.json** souboru:</span><span class="sxs-lookup"><span data-stu-id="92b91-139">Setting hello **ApiKey**, **message**, and **from** fields create hello following entries in hello **function.json** file:</span></span> 

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

<span data-ttu-id="92b91-140">Pokud dáváte přednost, můžete to upravovat soubor sami přímo.</span><span class="sxs-lookup"><span data-stu-id="92b91-140">If you prefer, you may modify this file yourself directly.</span></span>

<span data-ttu-id="92b91-141">Teď, když je vytvořen a nakonfigurován hello aplikaci funkce a funkce, můžete napsat kód toosend hello e-mailu.</span><span class="sxs-lookup"><span data-stu-id="92b91-141">Now that you have created and configured hello Function App and function, you can write hello code toosend an email.</span></span>

## <a name="write-code-that-creates-and-sends-email"></a><span data-ttu-id="92b91-142">Psaní kódu, které vytváří a odesílá e-mailu</span><span class="sxs-lookup"><span data-stu-id="92b91-142">Write code that creates and sends email</span></span>

<span data-ttu-id="92b91-143">Hello sendgrid vám umožňuje rozhraní API obsahuje všechny hello příkazy potřebujete toocreate a e-mailovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="92b91-143">hello SendGrid API contains all hello commands you need toocreate and send an email.</span></span>  

- <span data-ttu-id="92b91-144">Nahraďte kód hello ve funkci hello hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="92b91-144">Replace hello code in hello function with hello following code:</span></span>

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

<span data-ttu-id="92b91-145">Všimněte si hello první řádek obsahuje hello ```#r``` direktiva, která odkazuje na sestavení sendgrid vám umožňuje hello.</span><span class="sxs-lookup"><span data-stu-id="92b91-145">Notice hello first line contains hello ```#r``` directive that references hello SendGrid assembly.</span></span> <span data-ttu-id="92b91-146">Potom můžete použít ```using``` toomore příkaz snadný přístup k hello objekty v daném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="92b91-146">After that, you can use a ```using``` statement toomore easily access hello objects in that namespace.</span></span> <span data-ttu-id="92b91-147">V kódu hello vytvoření instancí ```Mail```, ```Personalization```, a ```Content``` objekty z hello sendgrid vám umožňuje rozhraní API, které tvoří hello e-mailu.</span><span class="sxs-lookup"><span data-stu-id="92b91-147">In hello code, create instances of ```Mail```, ```Personalization```, and ```Content``` objects from hello SendGrid API that compose hello email.</span></span> <span data-ttu-id="92b91-148">Při návratu uvítací zprávu, přináší sendgrid vám umožňuje ho.</span><span class="sxs-lookup"><span data-stu-id="92b91-148">When you return hello message, SendGrid delivers it.</span></span> 

<span data-ttu-id="92b91-149">Hello funkce podpis také obsahuje další parametr typu out ```Mail``` s názvem ```message```.</span><span class="sxs-lookup"><span data-stu-id="92b91-149">hello function's signature also contains an extra out parameter of type ```Mail``` named ```message```.</span></span> <span data-ttu-id="92b91-150">Obě vstup a výstup vazby express sami jako parametry funkce v kódu.</span><span class="sxs-lookup"><span data-stu-id="92b91-150">Both input and output bindings express themselves as function parameters in code.</span></span> 

2. <span data-ttu-id="92b91-151">Otestujte svůj kód kliknutím **Test** a zadání zprávy do hello **text žádosti** pole a pak kliknete na hello **spustit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="92b91-151">Test your code by clicking **Test** and entering a message into hello **Request body** field, then clicking hello **Run** button.</span></span>

 ![Otestujte svůj kód](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. <span data-ttu-id="92b91-153">Zkontrolujte, že sendgrid vám umožňuje odeslat e-mailu hello tooverify e-mailu.</span><span class="sxs-lookup"><span data-stu-id="92b91-153">Check email tooverify that SendGrid sent hello email.</span></span> <span data-ttu-id="92b91-154">Měl by přejděte toohello adresu v kódu hello z kroku 1 a obsahovat uvítací zprávu z hello **text žádosti**.</span><span class="sxs-lookup"><span data-stu-id="92b91-154">It should go toohello address in hello code from step 1, and contain hello message from hello **Request body**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="92b91-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="92b91-155">Next steps</span></span>
<span data-ttu-id="92b91-156">Tento článek vám ukázal, jak toouse hello toocreate služby sendgrid vám umožňuje a odeslání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="92b91-156">This article has demonstrated how toouse hello SendGrid service toocreate and send email.</span></span> <span data-ttu-id="92b91-157">toolearn Další informace o použití Azure Functions v aplikacích, najdete v části hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="92b91-157">toolearn more about using Azure Functions in your apps, see hello following topics:</span></span> 

- <span data-ttu-id="92b91-158">[Osvědčené postupy pro Azure Functions](functions-best-practices.md) uvádí některé osvědčené postupy toouse při vytváření Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="92b91-158">[Best practices for Azure Functions](functions-best-practices.md) Lists some best practices toouse when creating Azure Functions.</span></span>

- <span data-ttu-id="92b91-159">[Referenční informace pro vývojáře Azure Functions](functions-reference.md) referenční informace pro programátory pro kódování funkcí a definování triggerů a vazeb.</span><span class="sxs-lookup"><span data-stu-id="92b91-159">[Azure Functions developer reference](functions-reference.md) Programmer reference for coding functions and defining triggers and bindings.</span></span>

- <span data-ttu-id="92b91-160">[Testování Azure Functions](functions-test-a-function.md) popisuje různé nástroje a techniky pro testování funkcí.</span><span class="sxs-lookup"><span data-stu-id="92b91-160">[Testing Azure Functions](functions-test-a-function.md) Describes various tools and techniques for testing your functions.</span></span>