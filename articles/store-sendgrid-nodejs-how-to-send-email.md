---
title: "Jak používat služby sendgrid vám umožňuje e-mailů (Node.js) | Microsoft Docs"
description: "Zjistěte, jak odeslat e-mailu pomocí e-mailovou službu sendgrid vám umožňuje v Azure. Ukázky kódu jsou vytvořené pomocí rozhraní API Node.js."
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
ms.openlocfilehash: 327cea3a24cc47a9cc463b37cc2346ebc475ef7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-send-email-using-sendgrid-from-nodejs"></a><span data-ttu-id="8245a-104">Postup odesílání e-mailu pomocí sendgrid vám umožňuje z Node.js</span><span class="sxs-lookup"><span data-stu-id="8245a-104">How to Send Email Using SendGrid from Node.js</span></span>
<span data-ttu-id="8245a-105">Tato příručka ukazuje, jak provádět běžné úkoly programování s e-mailovou službu sendgrid vám umožňuje v Azure.</span><span class="sxs-lookup"><span data-stu-id="8245a-105">This guide demonstrates how to perform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="8245a-106">Ukázky jsou zapsány pomocí rozhraní API Node.js.</span><span class="sxs-lookup"><span data-stu-id="8245a-106">The samples are written using the Node.js API.</span></span> <span data-ttu-id="8245a-107">Pokryté scénáře zahrnují **vytváření e-mailu**, **odesílání e-mailu**, **přidávání příloh**, **pomocí filtrů**, a **aktualizace vlastností**.</span><span class="sxs-lookup"><span data-stu-id="8245a-107">The scenarios covered include **constructing email**, **sending email**, **adding attachments**, **using filters**, and **updating properties**.</span></span> <span data-ttu-id="8245a-108">Další informace o sendgrid vám umožňuje a odesílání e-mailu, najdete v článku [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="8245a-108">For more information on SendGrid and sending email, see the [Next Steps](#next-steps) section.</span></span>

## <a name="what-is-the-sendgrid-email-service"></a><span data-ttu-id="8245a-109">Co je služba sendgrid vám umožňuje e-mailu?</span><span class="sxs-lookup"><span data-stu-id="8245a-109">What is the SendGrid Email Service?</span></span>
<span data-ttu-id="8245a-110">Je sendgrid vám umožňuje [cloudový e-mailovou službu] poskytuje spolehlivé [doručování e-mailem transakční], škálovatelnost a analýzu v reálném čase společně s flexibilní rozhraní API, které umožňují snadnou vlastní integrace.</span><span class="sxs-lookup"><span data-stu-id="8245a-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="8245a-111">Obvyklé scénáře použití sendgrid vám umožňuje patří:</span><span class="sxs-lookup"><span data-stu-id="8245a-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="8245a-112">Automatické odesílání oznámení zákazníkům</span><span class="sxs-lookup"><span data-stu-id="8245a-112">Automatically sending receipts to customers</span></span>
* <span data-ttu-id="8245a-113">Správa distribučních seznamů pro odesílání zákazníkům měsíční e letáků a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="8245a-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="8245a-114">Shromažďování metriky v reálném čase pro takové věci, jako e-mailu blokovaný a odezvy zákazníka</span><span class="sxs-lookup"><span data-stu-id="8245a-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="8245a-115">Generování sestav k identifikaci trendů</span><span class="sxs-lookup"><span data-stu-id="8245a-115">Generating reports to help identify trends</span></span>
* <span data-ttu-id="8245a-116">Předávání dotazy zákazníků</span><span class="sxs-lookup"><span data-stu-id="8245a-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="8245a-117">E-mailových oznámení z vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="8245a-117">Email notifications from your application</span></span>

<span data-ttu-id="8245a-118">Další informace najdete v tématu [https://sendgrid.com](https://sendgrid.com).</span><span class="sxs-lookup"><span data-stu-id="8245a-118">For more information, see [https://sendgrid.com](https://sendgrid.com).</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="8245a-119">Vytvoření účtu sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="8245a-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-nodejs-module"></a><span data-ttu-id="8245a-120">Odkazovat na modul Node.js sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="8245a-120">Reference the SendGrid Node.js Module</span></span>
<span data-ttu-id="8245a-121">Modul sendgrid vám umožňuje pro Node.js dají nainstalovat přes uzel Správce balíčků (npm) pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="8245a-121">The SendGrid module for Node.js can be installed through the node package manager (npm) by using the following command:</span></span>

    npm install sendgrid

<span data-ttu-id="8245a-122">Po instalaci můžete požadovat modulu ve vaší aplikaci pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="8245a-122">After installation, you can require the module in your application by using the following code:</span></span>

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

<span data-ttu-id="8245a-123">Exportuje modul sendgrid vám umožňuje **sendgrid vám umožňuje** a **e-mailu** funkce.</span><span class="sxs-lookup"><span data-stu-id="8245a-123">The SendGrid module exports the **SendGrid** and **Email** functions.</span></span>
<span data-ttu-id="8245a-124">**Sendgrid vám umožňuje** je odpovědná za zasílání e-mailu pomocí webového rozhraní API, zatímco **e-mailu** zapouzdří e-mailovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="8245a-124">**SendGrid** is responsible for sending email through Web API, while **Email** encapsulates an email message.</span></span>

## <a name="how-to-create-an-email"></a><span data-ttu-id="8245a-125">Postupy: vytvoření e-mailu</span><span class="sxs-lookup"><span data-stu-id="8245a-125">How to: Create an Email</span></span>
<span data-ttu-id="8245a-126">Vytvoření e-mailovou zprávu pomocí modulu sendgrid vám umožňuje zahrnuje nejdříve vytvořením e-mailovou zprávu pomocí funkce e-mailu a pak jej pomocí funkce sendgrid vám umožňuje odeslat.</span><span class="sxs-lookup"><span data-stu-id="8245a-126">Creating an email message using the SendGrid module involves first creating an email message using the Email function, and then sending it using the SendGrid function.</span></span> <span data-ttu-id="8245a-127">Následuje příklad vytvoření nové zprávy pomocí funkce e-mailu:</span><span class="sxs-lookup"><span data-stu-id="8245a-127">The following is an example of creating a new message using the Email function:</span></span>

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

<span data-ttu-id="8245a-128">Můžete také určit zprávu HTML pro klienty, kteří ho podporují nastavením vlastnosti html.</span><span class="sxs-lookup"><span data-stu-id="8245a-128">You can also specify an HTML message for clients that support it by setting the html property.</span></span> <span data-ttu-id="8245a-129">Například:</span><span class="sxs-lookup"><span data-stu-id="8245a-129">For example:</span></span>

    html: This is a sample <b>HTML<b> email message.

<span data-ttu-id="8245a-130">Nastavení vlastností text a html poskytuje bezproblémové nouzového řešení ověření pomocí textového obsahu pro klienty, které nemohou podporovat zprávy ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="8245a-130">Setting both the text and html properties provides graceful fallback to text content for clients that cannot support HTML messages.</span></span>

<span data-ttu-id="8245a-131">Další informace o všech vlastnostech podporovaných zprostředkovatelem funkce e-mailu, najdete v části [sendgrid vám umožňuje nodejs][sendgrid-nodejs].</span><span class="sxs-lookup"><span data-stu-id="8245a-131">For more information on all properties supported by the Email function, see [sendgrid-nodejs][sendgrid-nodejs].</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="8245a-132">Postupy: odeslání e-mailu</span><span class="sxs-lookup"><span data-stu-id="8245a-132">How to: Send an Email</span></span>
<span data-ttu-id="8245a-133">Po vytvoření e-mailovou zprávu pomocí funkce e-mailu, můžete ho pomocí webového rozhraní API poskytované sendgrid vám umožňuje odeslat.</span><span class="sxs-lookup"><span data-stu-id="8245a-133">After creating an email message using the Email function, you can send it using the Web API provided by SendGrid.</span></span> 

### <a name="web-api"></a><span data-ttu-id="8245a-134">Web API</span><span class="sxs-lookup"><span data-stu-id="8245a-134">Web API</span></span>
    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [!NOTE]
> <span data-ttu-id="8245a-135">Při výše uvedené příklady ukazují předávání v e-mailu funkci objekt a zpětného volání, můžete také přímo vyvolat funkce pro odeslání přímo zadáním vlastnosti e-mailu.</span><span class="sxs-lookup"><span data-stu-id="8245a-135">While the above examples show passing in an email object and callback function, you can also directly invoke the send function by directly specifying email properties.</span></span> <span data-ttu-id="8245a-136">Například:</span><span class="sxs-lookup"><span data-stu-id="8245a-136">For example:</span></span>  
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

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="8245a-137">Postupy: Přidání přílohy</span><span class="sxs-lookup"><span data-stu-id="8245a-137">How to: Add an Attachment</span></span>
<span data-ttu-id="8245a-138">Přílohy lze přidat na zprávu zadáním názvy souboru a cesty ke v **soubory** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8245a-138">Attachments can be added to a message by specifying the file name(s) and path(s) in the **files** property.</span></span> <span data-ttu-id="8245a-139">Následující příklad ukazuje, odesílání přílohy:</span><span class="sxs-lookup"><span data-stu-id="8245a-139">The following example demonstrates sending an attachment:</span></span>

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used to specify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [!NOTE]
> <span data-ttu-id="8245a-140">Při použití **soubory** vlastnost soubor musí být přístupné prostřednictvím [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile).</span><span class="sxs-lookup"><span data-stu-id="8245a-140">When using the **files** property, the file must be accessible through [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile).</span></span> <span data-ttu-id="8245a-141">Pokud chcete připojit soubor hostovaný ve službě Azure Storage, například v kontejneru objektů Blob, je nutné nejprve zkopírovat soubor do místního úložiště nebo k Azure jednotce před odesláním jako k připojení pomocí **soubory** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8245a-141">If the file you wish to attach is hosted in Azure Storage, such as in a Blob container, you must first copy the file to local storage or to an Azure drive before it can be sent as an attachment using the **files** property.</span></span>
> 
> 

## <a name="how-to-use-filters-to-enable-footers-and-tracking"></a><span data-ttu-id="8245a-142">Postupy: použití filtrů k povolení zápatí a sledování</span><span class="sxs-lookup"><span data-stu-id="8245a-142">How to: Use Filters to Enable Footers and Tracking</span></span>
<span data-ttu-id="8245a-143">Sendgrid vám umožňuje poskytuje další e-mailové funkce prostřednictvím filtry.</span><span class="sxs-lookup"><span data-stu-id="8245a-143">SendGrid provides additional email functionality through the use of filters.</span></span> <span data-ttu-id="8245a-144">Jsou to nastavení, které mohou být přidány do e-mailovou zprávu povolit specifické funkce, například povolení sledování klikněte na tlačítko, Google analytics, předplatné, sledování a tak dále.</span><span class="sxs-lookup"><span data-stu-id="8245a-144">These are settings that can be added to an email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="8245a-145">Úplný seznam filtrů, najdete v části [nastavení filtru][Filter Settings].</span><span class="sxs-lookup"><span data-stu-id="8245a-145">For a full list of filters, see [Filter Settings][Filter Settings].</span></span>

<span data-ttu-id="8245a-146">Filtry můžete použít na zprávy pomocí **filtry** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8245a-146">Filters can be applied to a message by using the **filters** property.</span></span>
<span data-ttu-id="8245a-147">Každý filtr je zadána hodnota hash obsahující nastavení pro konkrétní filtru.</span><span class="sxs-lookup"><span data-stu-id="8245a-147">Each filter is specified by a hash containing filter-specific settings.</span></span>
<span data-ttu-id="8245a-148">Následující příklady ukazují zápatí a klikněte na tlačítko Sledování filtry:</span><span class="sxs-lookup"><span data-stu-id="8245a-148">The following examples demonstrate the footer and click tracking filters:</span></span>

### <a name="footer"></a><span data-ttu-id="8245a-149">Zápatí stránky</span><span class="sxs-lookup"><span data-stu-id="8245a-149">Footer</span></span>
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

### <a name="click-tracking"></a><span data-ttu-id="8245a-150">Klikněte na tlačítko Sledování</span><span class="sxs-lookup"><span data-stu-id="8245a-150">Click Tracking</span></span>
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

## <a name="how-to-update-email-properties"></a><span data-ttu-id="8245a-151">Postupy: aktualizovat vlastnosti e-mailu</span><span class="sxs-lookup"><span data-stu-id="8245a-151">How to: Update Email Properties</span></span>
<span data-ttu-id="8245a-152">Některé vlastnosti e-mailu můžete přepsat pomocí  **nastavit*vlastnost*** nebo připojených pomocí  **přidat*vlastnost***.</span><span class="sxs-lookup"><span data-stu-id="8245a-152">Some email properties can be overwritten using **set*Property*** or appended using **add*Property***.</span></span> <span data-ttu-id="8245a-153">Například můžete přidat další příjemce pomocí</span><span class="sxs-lookup"><span data-stu-id="8245a-153">For example, you can add additional recipients by using</span></span>

    email.addTo('jeff@contoso.com');

<span data-ttu-id="8245a-154">pomocí nebo nastavit filtr</span><span class="sxs-lookup"><span data-stu-id="8245a-154">or set a filter by using</span></span>

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

<span data-ttu-id="8245a-155">Další informace najdete v tématu [sendgrid vám umožňuje nodejs][sendgrid-nodejs].</span><span class="sxs-lookup"><span data-stu-id="8245a-155">For more information, see [sendgrid-nodejs][sendgrid-nodejs].</span></span>

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="8245a-156">Postupy: použití služeb další sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="8245a-156">How to: Use Additional SendGrid Services</span></span>
<span data-ttu-id="8245a-157">Sendgrid vám umožňuje nabízí rozhraní API založené na webu, který můžete využít další funkce sendgrid vám umožňuje aplikaci Azure.</span><span class="sxs-lookup"><span data-stu-id="8245a-157">SendGrid offers web-based APIs that you can use to leverage additional SendGrid functionality from your Azure application.</span></span> <span data-ttu-id="8245a-158">Úplné podrobnosti najdete v tématu [dokumentaci k rozhraní API sendgrid vám umožňuje][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="8245a-158">For full details, see the [SendGrid API documentation][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="8245a-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8245a-159">Next Steps</span></span>
<span data-ttu-id="8245a-160">Teď, když jste se naučili základy služby sendgrid vám umožňuje e-mailu, postupujte podle následujících odkazech na další informace.</span><span class="sxs-lookup"><span data-stu-id="8245a-160">Now that you've learned the basics of the SendGrid Email service, follow these links to learn more.</span></span>

* <span data-ttu-id="8245a-161">Úložiště modul Node.js sendgrid vám umožňuje: [sendgrid vám umožňuje nodejs][sendgrid-nodejs]</span><span class="sxs-lookup"><span data-stu-id="8245a-161">SendGrid Node.js module repository: [sendgrid-nodejs][sendgrid-nodejs]</span></span>
* <span data-ttu-id="8245a-162">Dokumentaci k rozhraní API sendgrid vám umožňuje: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="8245a-162">SendGrid API documentation: <https://sendgrid.com/docs></span></span>
* <span data-ttu-id="8245a-163">Speciální nabídka sendgrid vám umožňuje Azure zákazníků: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)</span><span class="sxs-lookup"><span data-stu-id="8245a-163">SendGrid special offer for Azure customers: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)</span></span>

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
<span data-ttu-id="8245a-164">[cloudový e-mailovou službu]: https://sendgrid.com/email-solutions</span><span class="sxs-lookup"><span data-stu-id="8245a-164">[cloud-based email service]: https://sendgrid.com/email-solutions</span></span>
<span data-ttu-id="8245a-165">[doručování e-mailem transakční]: https://sendgrid.com/transactional-email</span><span class="sxs-lookup"><span data-stu-id="8245a-165">[transactional email delivery]: https://sendgrid.com/transactional-email</span></span>
