---
title: "Jak používat sendgrid vám umožňuje v Azure Functions | Microsoft Docs"
description: "Ukazuje, jak používat sendgrid vám umožňuje v Azure Functions"
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
ms.openlocfilehash: 05c9f4e4a4351219da68af8b702c25f21d7d4d02
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-sendgrid-in-azure-functions"></a><span data-ttu-id="2d38f-103">Jak používat sendgrid vám umožňuje v Azure Functions</span><span class="sxs-lookup"><span data-stu-id="2d38f-103">How to use SendGrid in Azure Functions</span></span>

## <a name="sendgrid-overview"></a><span data-ttu-id="2d38f-104">Přehled sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="2d38f-104">SendGrid Overview</span></span>

<span data-ttu-id="2d38f-105">Azure Functions podporuje sendgrid vám umožňuje výstupní vazby funkcí k odesílání e-mailové zprávy s několika řádků kódu a sendgrid vám umožňuje účet povolit.</span><span class="sxs-lookup"><span data-stu-id="2d38f-105">Azure Functions supports SendGrid output bindings to enable your functions to send email messages with a few lines of code and a SendGrid account.</span></span>

<span data-ttu-id="2d38f-106">Chcete-li použít rozhraní API sendgrid vám umožňuje v funkci Azure, musíte mít [sendgrid vám umožňuje účet](http://SendGrid.com).</span><span class="sxs-lookup"><span data-stu-id="2d38f-106">To use the SendGrid API in an Azure Function, you must have a [SendGrid account](http://SendGrid.com).</span></span> <span data-ttu-id="2d38f-107">Kromě toho musí mít klíč rozhraní API sendgrid vám umožňuje.</span><span class="sxs-lookup"><span data-stu-id="2d38f-107">Additionally, you must have a SendGrid API Key.</span></span> <span data-ttu-id="2d38f-108">Přihlaste se ke svému účtu sendgrid vám umožňuje a klikněte na tlačítko **nastavení** pak **klíč rozhraní API** vygenerovat klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2d38f-108">Log in to your SendGrid account and click **Settings** then **API Key** to generate an API key.</span></span> <span data-ttu-id="2d38f-109">Uchovávejte tento klíč k dispozici při používání v nadcházející kroku.</span><span class="sxs-lookup"><span data-stu-id="2d38f-109">Keep this key available as you use it in an upcoming step.</span></span>

<span data-ttu-id="2d38f-110">Nyní jste připraveni vytvořit aplikaci funkce Azure.</span><span class="sxs-lookup"><span data-stu-id="2d38f-110">You are now ready to create an Azure Function app.</span></span>

## <a name="create-an-azure-function-app"></a><span data-ttu-id="2d38f-111">Vytvoření aplikace Azure Function App</span><span class="sxs-lookup"><span data-stu-id="2d38f-111">Create an Azure Function app</span></span> 

<span data-ttu-id="2d38f-112">Aplikace Azure funkce jsou kontejnery pro jeden nebo více Azure functions.</span><span class="sxs-lookup"><span data-stu-id="2d38f-112">Azure Function Apps are containers for one or more Azure functions.</span></span> <span data-ttu-id="2d38f-113">Řešení Azure functions se právě který – funkce.</span><span class="sxs-lookup"><span data-stu-id="2d38f-113">Azure functions are just that - a function.</span></span> <span data-ttu-id="2d38f-114">Jednotlivé funkce Azure je vázaný na jedna aktivační událost, která je událost, která způsobí, že funkce spustit.</span><span class="sxs-lookup"><span data-stu-id="2d38f-114">Each Azure function is tied to one trigger, which is an event that causes the function to run.</span></span>
<span data-ttu-id="2d38f-115">Jednotlivé funkce může obsahovat libovolný počet vstup nebo výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="2d38f-115">Each function can contain any number of input or output bindings.</span></span> <span data-ttu-id="2d38f-116">Vazby jsou služby, které můžete použít ve funkci.</span><span class="sxs-lookup"><span data-stu-id="2d38f-116">Bindings are services that you can use in a function.</span></span> <span data-ttu-id="2d38f-117">Sendgrid vám umožňuje je vazbu výstup, které můžete použít k odesílání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="2d38f-117">SendGrid is an output binding you can use to send email.</span></span> 

1. <span data-ttu-id="2d38f-118">Přihlaste se k portálu Azure a [vytvoření aplikace Azure funkce](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) nebo otevřete stávající funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="2d38f-118">Log in to the Azure portal and [create an Azure Function App](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) or open an existing Function app.</span></span> 
2. <span data-ttu-id="2d38f-119">Vytvoření Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="2d38f-119">Create an Azure function.</span></span> <span data-ttu-id="2d38f-120">Chcete-li jednoduchost, zvolte ruční aktivační události a C#.</span><span class="sxs-lookup"><span data-stu-id="2d38f-120">To keep it simple, choose a manual trigger and C#.</span></span> 

 ![Vytvoření Azure funkce](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a><span data-ttu-id="2d38f-122">Sendgrid vám umožňuje nakonfigurovat pro použití v aplikaci Azure – funkce</span><span class="sxs-lookup"><span data-stu-id="2d38f-122">Configure SendGrid for use in an Azure Function app</span></span>

<span data-ttu-id="2d38f-123">Musíte si klíč rozhraní API sendgrid vám umožňuje uložit jako nastavení aplikace pro použití ve funkci.</span><span class="sxs-lookup"><span data-stu-id="2d38f-123">You must store your SendGrid API Key as an app setting to use it in a function.</span></span> <span data-ttu-id="2d38f-124">Pole ApiKey není klíč skutečné sendgrid vám umožňuje rozhraní API, ale definovat nastavení aplikace, které představuje skutečný klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2d38f-124">The ApiKey field is not your actual SendGrid API key, but an app setting you define that represents your actual API key.</span></span> <span data-ttu-id="2d38f-125">Ukládání klíče tímto způsobem je pro zabezpečení, protože je oddělená od žádné kódu nebo soubory, které může zkontrolovat do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="2d38f-125">Storing your key this way is for security, since it is separate from any code or files that might be checked into source code control.</span></span>

- <span data-ttu-id="2d38f-126">Vytvoření **AppSettings** klíče v aplikaci funkce **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2d38f-126">Create an **AppSettings** key in your function app's **Application Settings**.</span></span>

 ![Vytvoření Azure funkce](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a><span data-ttu-id="2d38f-128">Konfigurovat sendgrid vám umožňuje výstup vazby</span><span class="sxs-lookup"><span data-stu-id="2d38f-128">Configure SendGrid output bindings</span></span>

<span data-ttu-id="2d38f-129">Je k dispozici jako Azure sendgrid vám umožňuje funkce výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="2d38f-129">SendGrid is available as an Azure function output binding.</span></span> <span data-ttu-id="2d38f-130">Chcete-li vytvořit sendgrid vám umožňuje výstup vazby:</span><span class="sxs-lookup"><span data-stu-id="2d38f-130">To create a SendGrid output binding:</span></span>

1. <span data-ttu-id="2d38f-131">Přejděte na **integrací** karta funkce na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2d38f-131">Go to the **Integrate** tab of the function in the Azure portal.</span></span>
2. <span data-ttu-id="2d38f-132">Klikněte na tlačítko **nový výstupní** sendgrid vám umožňuje vytvořit výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="2d38f-132">Click **New Output** to create a SendGrid output binding.</span></span>
3. <span data-ttu-id="2d38f-133">Vyplňte **klíč rozhraní API** a **název parametru zpráva** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2d38f-133">Fill in the **API Key** and **Message parameter name** properties.</span></span> <span data-ttu-id="2d38f-134">Pokud chcete, můžete zadat další vlastnosti nyní, nebo místo toho je kód.</span><span class="sxs-lookup"><span data-stu-id="2d38f-134">If you want, you can enter the other properties now, or code them instead.</span></span> <span data-ttu-id="2d38f-135">Tato nastavení mohou být použity jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="2d38f-135">These settings can be used as defaults.</span></span>

 ![Konfigurovat sendgrid vám umožňuje výstup vazby](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

<span data-ttu-id="2d38f-137">Přidání vazbu na funkci vytvoří soubor s názvem **function.json** ve složce vaší funkce.</span><span class="sxs-lookup"><span data-stu-id="2d38f-137">Adding a binding to a function creates a file called **function.json** in your function's folder.</span></span> <span data-ttu-id="2d38f-138">Tento soubor obsahuje všechny stejné informace, které se zobrazí v Azure funkce **integrací** kartě, ale ve formátu Json formátu.</span><span class="sxs-lookup"><span data-stu-id="2d38f-138">This file contains all the same information that you see in the Azure function's **Integrate** tab, but in Json format.</span></span> <span data-ttu-id="2d38f-139">Nastavení **ApiKey**, **zpráva**, a **z** pole vytvořit následující záznamy v **function.json** souboru:</span><span class="sxs-lookup"><span data-stu-id="2d38f-139">Setting the **ApiKey**, **message**, and **from** fields create the following entries in the **function.json** file:</span></span> 

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

<span data-ttu-id="2d38f-140">Pokud dáváte přednost, můžete to upravovat soubor sami přímo.</span><span class="sxs-lookup"><span data-stu-id="2d38f-140">If you prefer, you may modify this file yourself directly.</span></span>

<span data-ttu-id="2d38f-141">Teď, když jste vytvořili a nakonfigurovali aplikaci funkce a funkce, můžete napsat kód odeslat e-mail.</span><span class="sxs-lookup"><span data-stu-id="2d38f-141">Now that you have created and configured the Function App and function, you can write the code to send an email.</span></span>

## <a name="write-code-that-creates-and-sends-email"></a><span data-ttu-id="2d38f-142">Psaní kódu, které vytváří a odesílá e-mailu</span><span class="sxs-lookup"><span data-stu-id="2d38f-142">Write code that creates and sends email</span></span>

<span data-ttu-id="2d38f-143">Rozhraní API sendgrid vám umožňuje obsahuje všechny příkazy, které potřebujete k vytvoření a odeslání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="2d38f-143">The SendGrid API contains all the commands you need to create and send an email.</span></span>  

- <span data-ttu-id="2d38f-144">Nahraďte kód ve funkci s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2d38f-144">Replace the code in the function with the following code:</span></span>

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
    // change to email of recipient
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

<span data-ttu-id="2d38f-145">Všimněte si první řádek obsahuje ```#r``` direktiva, která odkazuje na sestavení sendgrid vám umožňuje.</span><span class="sxs-lookup"><span data-stu-id="2d38f-145">Notice the first line contains the ```#r``` directive that references the SendGrid assembly.</span></span> <span data-ttu-id="2d38f-146">Potom můžete použít ```using``` příkaz více snadný přístup k objektům v daném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2d38f-146">After that, you can use a ```using``` statement to more easily access the objects in that namespace.</span></span> <span data-ttu-id="2d38f-147">V kódu, vytváření instancí ```Mail```, ```Personalization```, a ```Content``` objekty z rozhraní API Sendgridu, které tvoří e-mailu.</span><span class="sxs-lookup"><span data-stu-id="2d38f-147">In the code, create instances of ```Mail```, ```Personalization```, and ```Content``` objects from the SendGrid API that compose the email.</span></span> <span data-ttu-id="2d38f-148">Při návratu zprávy, přináší sendgrid vám umožňuje ho.</span><span class="sxs-lookup"><span data-stu-id="2d38f-148">When you return the message, SendGrid delivers it.</span></span> 

<span data-ttu-id="2d38f-149">Podpis funkce je také obsahuje další parametr typu out ```Mail``` s názvem ```message```.</span><span class="sxs-lookup"><span data-stu-id="2d38f-149">The function's signature also contains an extra out parameter of type ```Mail``` named ```message```.</span></span> <span data-ttu-id="2d38f-150">Obě vstup a výstup vazby express sami jako parametry funkce v kódu.</span><span class="sxs-lookup"><span data-stu-id="2d38f-150">Both input and output bindings express themselves as function parameters in code.</span></span> 

2. <span data-ttu-id="2d38f-151">Otestujte svůj kód kliknutím **Test** a zadání zprávy do **text žádosti** pole, pak levým na **spustit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2d38f-151">Test your code by clicking **Test** and entering a message into the **Request body** field, then clicking the **Run** button.</span></span>

 ![Otestujte svůj kód](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. <span data-ttu-id="2d38f-153">Zkontrolujte e-mailu, chcete-li ověřit, že sendgrid vám umožňuje odeslat e-mailu.</span><span class="sxs-lookup"><span data-stu-id="2d38f-153">Check email to verify that SendGrid sent the email.</span></span> <span data-ttu-id="2d38f-154">Měl by přejděte na adresu v kódu z kroku 1 a obsahovat zprávu od **text žádosti**.</span><span class="sxs-lookup"><span data-stu-id="2d38f-154">It should go to the address in the code from step 1, and contain the message from the **Request body**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d38f-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2d38f-155">Next steps</span></span>
<span data-ttu-id="2d38f-156">Tento článek vám ukázal, jak pomocí služby sendgrid vám umožňuje vytvořit a odeslat e-mailu.</span><span class="sxs-lookup"><span data-stu-id="2d38f-156">This article has demonstrated how to use the SendGrid service to create and send email.</span></span> <span data-ttu-id="2d38f-157">Další informace o používání služby Azure Functions ve svých aplikacích najdete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="2d38f-157">To learn more about using Azure Functions in your apps, see the following topics:</span></span> 

- <span data-ttu-id="2d38f-158">[Osvědčené postupy pro Azure Functions](functions-best-practices.md) uvádí některé osvědčené postupy pro použití při vytváření Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="2d38f-158">[Best practices for Azure Functions](functions-best-practices.md) Lists some best practices to use when creating Azure Functions.</span></span>

- <span data-ttu-id="2d38f-159">[Referenční informace pro vývojáře Azure Functions](functions-reference.md) referenční informace pro programátory pro kódování funkcí a definování triggerů a vazeb.</span><span class="sxs-lookup"><span data-stu-id="2d38f-159">[Azure Functions developer reference](functions-reference.md) Programmer reference for coding functions and defining triggers and bindings.</span></span>

- <span data-ttu-id="2d38f-160">[Testování Azure Functions](functions-test-a-function.md) popisuje různé nástroje a techniky pro testování funkcí.</span><span class="sxs-lookup"><span data-stu-id="2d38f-160">[Testing Azure Functions](functions-test-a-function.md) Describes various tools and techniques for testing your functions.</span></span>