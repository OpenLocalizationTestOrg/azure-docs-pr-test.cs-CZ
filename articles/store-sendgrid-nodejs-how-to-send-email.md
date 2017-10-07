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
# <a name="how-toosend-email-using-sendgrid-from-nodejs"></a><span data-ttu-id="87ea6-104">Jak tooSend Sendgridu pomocí e-mailu z Node.js</span><span class="sxs-lookup"><span data-stu-id="87ea6-104">How tooSend Email Using SendGrid from Node.js</span></span>
<span data-ttu-id="87ea6-105">Tato příručka ukazuje, jak tooperform běžné úlohy programování sendgridu e-mailové služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="87ea6-105">This guide demonstrates how tooperform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="87ea6-106">Ukázky Hello jsou zapsány pomocí rozhraní API Node.js hello.</span><span class="sxs-lookup"><span data-stu-id="87ea6-106">hello samples are written using hello Node.js API.</span></span> <span data-ttu-id="87ea6-107">Hello pokryté scénáře zahrnují **vytváření e-mailu**, **odesílání e-mailu**, **přidávání příloh**, **pomocí filtrů**a **aktualizace vlastností**.</span><span class="sxs-lookup"><span data-stu-id="87ea6-107">hello scenarios covered include **constructing email**, **sending email**, **adding attachments**, **using filters**, and **updating properties**.</span></span> <span data-ttu-id="87ea6-108">Další informace o sendgrid vám umožňuje a odesílání e-mailu, najdete v části hello [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="87ea6-108">For more information on SendGrid and sending email, see hello [Next Steps](#next-steps) section.</span></span>

## <a name="what-is-hello-sendgrid-email-service"></a><span data-ttu-id="87ea6-109">Co je hello sendgrid vám umožňuje e-mailovou službu?</span><span class="sxs-lookup"><span data-stu-id="87ea6-109">What is hello SendGrid Email Service?</span></span>
<span data-ttu-id="87ea6-110">Je sendgrid vám umožňuje [cloudový e-mailovou službu] poskytuje spolehlivé [doručování e-mailem transakční], škálovatelnost a analýzu v reálném čase společně s flexibilní rozhraní API, které umožňují snadnou vlastní integrace.</span><span class="sxs-lookup"><span data-stu-id="87ea6-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="87ea6-111">Obvyklé scénáře použití sendgrid vám umožňuje patří:</span><span class="sxs-lookup"><span data-stu-id="87ea6-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="87ea6-112">Automatické odesílání toocustomers potvrzení</span><span class="sxs-lookup"><span data-stu-id="87ea6-112">Automatically sending receipts toocustomers</span></span>
* <span data-ttu-id="87ea6-113">Správa distribučních seznamů pro odesílání zákazníkům měsíční e letáků a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="87ea6-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="87ea6-114">Shromažďování metriky v reálném čase pro takové věci, jako e-mailu blokovaný a odezvy zákazníka</span><span class="sxs-lookup"><span data-stu-id="87ea6-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="87ea6-115">Generování sestav toohelp identifikovat trendy</span><span class="sxs-lookup"><span data-stu-id="87ea6-115">Generating reports toohelp identify trends</span></span>
* <span data-ttu-id="87ea6-116">Předávání dotazy zákazníků</span><span class="sxs-lookup"><span data-stu-id="87ea6-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="87ea6-117">E-mailových oznámení z vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="87ea6-117">Email notifications from your application</span></span>

<span data-ttu-id="87ea6-118">Další informace najdete v tématu [https://sendgrid.com](https://sendgrid.com).</span><span class="sxs-lookup"><span data-stu-id="87ea6-118">For more information, see [https://sendgrid.com](https://sendgrid.com).</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="87ea6-119">Vytvoření účtu sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="87ea6-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-hello-sendgrid-nodejs-module"></a><span data-ttu-id="87ea6-120">Referenční dokumentace hello sendgrid vám umožňuje modulu Node.js</span><span class="sxs-lookup"><span data-stu-id="87ea6-120">Reference hello SendGrid Node.js Module</span></span>
<span data-ttu-id="87ea6-121">Hello sendgrid vám umožňuje modul pro Node.js dají nainstalovat přes hello uzel Správce balíčků (npm) pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="87ea6-121">hello SendGrid module for Node.js can be installed through hello node package manager (npm) by using hello following command:</span></span>

    npm install sendgrid

<span data-ttu-id="87ea6-122">Po instalaci můžete požadovat hello modulu ve vaší aplikaci pomocí hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="87ea6-122">After installation, you can require hello module in your application by using hello following code:</span></span>

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

<span data-ttu-id="87ea6-123">modul sendgrid vám umožňuje Hello exportuje hello **sendgrid vám umožňuje** a **e-mailu** funkce.</span><span class="sxs-lookup"><span data-stu-id="87ea6-123">hello SendGrid module exports hello **SendGrid** and **Email** functions.</span></span>
<span data-ttu-id="87ea6-124">**Sendgrid vám umožňuje** je odpovědná za zasílání e-mailu pomocí webového rozhraní API, zatímco **e-mailu** zapouzdří e-mailovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="87ea6-124">**SendGrid** is responsible for sending email through Web API, while **Email** encapsulates an email message.</span></span>

## <a name="how-to-create-an-email"></a><span data-ttu-id="87ea6-125">Postupy: vytvoření e-mailu</span><span class="sxs-lookup"><span data-stu-id="87ea6-125">How to: Create an Email</span></span>
<span data-ttu-id="87ea6-126">Vytvoření e-mailovou zprávu pomocí modulu sendgrid vám umožňuje hello zahrnuje nejdříve vytvořením e-mailovou zprávu pomocí funkce hello e-mailu a pak jej pomocí funkce hello sendgrid vám umožňuje odeslat.</span><span class="sxs-lookup"><span data-stu-id="87ea6-126">Creating an email message using hello SendGrid module involves first creating an email message using hello Email function, and then sending it using hello SendGrid function.</span></span> <span data-ttu-id="87ea6-127">Hello následuje příklad vytvoření nové zprávy pomocí funkce e-mailu hello:</span><span class="sxs-lookup"><span data-stu-id="87ea6-127">hello following is an example of creating a new message using hello Email function:</span></span>

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

<span data-ttu-id="87ea6-128">Můžete také zprávy ve formátu HTML pro klienty, kteří ho podporují nastavením vlastnosti hello html.</span><span class="sxs-lookup"><span data-stu-id="87ea6-128">You can also specify an HTML message for clients that support it by setting hello html property.</span></span> <span data-ttu-id="87ea6-129">Například:</span><span class="sxs-lookup"><span data-stu-id="87ea6-129">For example:</span></span>

    html: This is a sample <b>HTML<b> email message.

<span data-ttu-id="87ea6-130">Nastavení vlastností obou hello text nebo html poskytuje bezproblémové nouzového řešení ověření pomocí textového obsahu pro klienty, které nemohou podporovat zprávy ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="87ea6-130">Setting both hello text and html properties provides graceful fallback to text content for clients that cannot support HTML messages.</span></span>

<span data-ttu-id="87ea6-131">Další informace o všech vlastnostech podporovaných zprostředkovatelem hello funkce e-mailu, najdete v části [sendgrid vám umožňuje nodejs][sendgrid-nodejs].</span><span class="sxs-lookup"><span data-stu-id="87ea6-131">For more information on all properties supported by hello Email function, see [sendgrid-nodejs][sendgrid-nodejs].</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="87ea6-132">Postupy: odeslání e-mailu</span><span class="sxs-lookup"><span data-stu-id="87ea6-132">How to: Send an Email</span></span>
<span data-ttu-id="87ea6-133">Po vytvoření e-mailovou zprávu pomocí hello funkce e-mailu, můžete ho pomocí hello webového rozhraní API poskytované sendgrid vám umožňuje odeslat.</span><span class="sxs-lookup"><span data-stu-id="87ea6-133">After creating an email message using hello Email function, you can send it using hello Web API provided by SendGrid.</span></span> 

### <a name="web-api"></a><span data-ttu-id="87ea6-134">Web API</span><span class="sxs-lookup"><span data-stu-id="87ea6-134">Web API</span></span>
    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [!NOTE]
> <span data-ttu-id="87ea6-135">Při hello výše příklady zobrazit v e-mailu objekt a zpětného volání funkci předávání, můžete také přímo vyvolat funkce pro odeslání hello přímo zadáním vlastnosti e-mailu.</span><span class="sxs-lookup"><span data-stu-id="87ea6-135">While hello above examples show passing in an email object and callback function, you can also directly invoke hello send function by directly specifying email properties.</span></span> <span data-ttu-id="87ea6-136">Například:</span><span class="sxs-lookup"><span data-stu-id="87ea6-136">For example:</span></span>  
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

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="87ea6-137">Postupy: Přidání přílohy</span><span class="sxs-lookup"><span data-stu-id="87ea6-137">How to: Add an Attachment</span></span>
<span data-ttu-id="87ea6-138">Přílohy lze přidat zprávu tooa zadáním hello názvy souboru a cesty ke v hello **soubory** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="87ea6-138">Attachments can be added tooa message by specifying hello file name(s) and path(s) in hello **files** property.</span></span> <span data-ttu-id="87ea6-139">Hello následující příklad ukazuje, odesílání přílohy:</span><span class="sxs-lookup"><span data-stu-id="87ea6-139">hello following example demonstrates sending an attachment:</span></span>

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
> <span data-ttu-id="87ea6-140">Při použití hello **soubory** vlastnost hello soubor musí být přístupné prostřednictvím [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile).</span><span class="sxs-lookup"><span data-stu-id="87ea6-140">When using hello **files** property, hello file must be accessible through [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile).</span></span> <span data-ttu-id="87ea6-141">Pokud chcete tooattach souboru hello hostovaný ve službě Azure Storage, například v kontejneru objektů Blob, je nutné nejprve zkopírovat hello soubor toolocal úložiště nebo tooan Azure jednotky předtím, než může být odeslán jako příloha pomocí hello **soubory** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="87ea6-141">If hello file you wish tooattach is hosted in Azure Storage, such as in a Blob container, you must first copy hello file toolocal storage or tooan Azure drive before it can be sent as an attachment using hello **files** property.</span></span>
> 
> 

## <a name="how-to-use-filters-tooenable-footers-and-tracking"></a><span data-ttu-id="87ea6-142">Postupy: použití filtrů tooEnable zápatí a sledování</span><span class="sxs-lookup"><span data-stu-id="87ea6-142">How to: Use Filters tooEnable Footers and Tracking</span></span>
<span data-ttu-id="87ea6-143">Sendgrid vám umožňuje poskytuje další e-mailové funkce prostřednictvím hello použití filtrů.</span><span class="sxs-lookup"><span data-stu-id="87ea6-143">SendGrid provides additional email functionality through hello use of filters.</span></span> <span data-ttu-id="87ea6-144">Jsou to nastavení, které mohou být přidány tooan e-mailové zprávě povolit specifické funkce, například povolení sledování klikněte na tlačítko, Google analytics, sledování, předplatné a tak dále.</span><span class="sxs-lookup"><span data-stu-id="87ea6-144">These are settings that can be added tooan email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="87ea6-145">Úplný seznam filtrů, najdete v části [nastavení filtru][Filter Settings].</span><span class="sxs-lookup"><span data-stu-id="87ea6-145">For a full list of filters, see [Filter Settings][Filter Settings].</span></span>

<span data-ttu-id="87ea6-146">Filtry lze použité tooa zpráv pomocí hello **filtry** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="87ea6-146">Filters can be applied tooa message by using hello **filters** property.</span></span>
<span data-ttu-id="87ea6-147">Každý filtr je zadána hodnota hash obsahující nastavení pro konkrétní filtru.</span><span class="sxs-lookup"><span data-stu-id="87ea6-147">Each filter is specified by a hash containing filter-specific settings.</span></span>
<span data-ttu-id="87ea6-148">Hello následující příklady ukazují hello zápatí a klikněte na tlačítko Sledování filtry:</span><span class="sxs-lookup"><span data-stu-id="87ea6-148">hello following examples demonstrate hello footer and click tracking filters:</span></span>

### <a name="footer"></a><span data-ttu-id="87ea6-149">Zápatí stránky</span><span class="sxs-lookup"><span data-stu-id="87ea6-149">Footer</span></span>
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

### <a name="click-tracking"></a><span data-ttu-id="87ea6-150">Klikněte na tlačítko Sledování</span><span class="sxs-lookup"><span data-stu-id="87ea6-150">Click Tracking</span></span>
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

## <a name="how-to-update-email-properties"></a><span data-ttu-id="87ea6-151">Postupy: aktualizovat vlastnosti e-mailu</span><span class="sxs-lookup"><span data-stu-id="87ea6-151">How to: Update Email Properties</span></span>
<span data-ttu-id="87ea6-152">Některé vlastnosti e-mailu můžete přepsat pomocí  **nastavit*vlastnost*** nebo připojených pomocí  **přidat*vlastnost***.</span><span class="sxs-lookup"><span data-stu-id="87ea6-152">Some email properties can be overwritten using **set*Property*** or appended using **add*Property***.</span></span> <span data-ttu-id="87ea6-153">Například můžete přidat další příjemce pomocí</span><span class="sxs-lookup"><span data-stu-id="87ea6-153">For example, you can add additional recipients by using</span></span>

    email.addTo('jeff@contoso.com');

<span data-ttu-id="87ea6-154">pomocí nebo nastavit filtr</span><span class="sxs-lookup"><span data-stu-id="87ea6-154">or set a filter by using</span></span>

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

<span data-ttu-id="87ea6-155">Další informace najdete v tématu [sendgrid vám umožňuje nodejs][sendgrid-nodejs].</span><span class="sxs-lookup"><span data-stu-id="87ea6-155">For more information, see [sendgrid-nodejs][sendgrid-nodejs].</span></span>

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="87ea6-156">Postupy: použití služeb další sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="87ea6-156">How to: Use Additional SendGrid Services</span></span>
<span data-ttu-id="87ea6-157">Sendgrid vám umožňuje nabízí webové rozhraní API, které můžete použít další funkce sendgrid vám umožňuje tooleverage z vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="87ea6-157">SendGrid offers web-based APIs that you can use tooleverage additional SendGrid functionality from your Azure application.</span></span> <span data-ttu-id="87ea6-158">Úplné podrobnosti najdete v tématu hello [dokumentaci k rozhraní API sendgrid vám umožňuje][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="87ea6-158">For full details, see hello [SendGrid API documentation][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="87ea6-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87ea6-159">Next Steps</span></span>
<span data-ttu-id="87ea6-160">Teď, když jste se naučili základy hello hello služby sendgrid vám umožňuje e-mailů, použijte tyto odkazy toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="87ea6-160">Now that you've learned hello basics of hello SendGrid Email service, follow these links toolearn more.</span></span>

* <span data-ttu-id="87ea6-161">Úložiště modul Node.js sendgrid vám umožňuje: [sendgrid vám umožňuje nodejs][sendgrid-nodejs]</span><span class="sxs-lookup"><span data-stu-id="87ea6-161">SendGrid Node.js module repository: [sendgrid-nodejs][sendgrid-nodejs]</span></span>
* <span data-ttu-id="87ea6-162">Dokumentaci k rozhraní API sendgrid vám umožňuje: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="87ea6-162">SendGrid API documentation: <https://sendgrid.com/docs></span></span>
* <span data-ttu-id="87ea6-163">Speciální nabídka sendgrid vám umožňuje Azure zákazníků: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)</span><span class="sxs-lookup"><span data-stu-id="87ea6-163">SendGrid special offer for Azure customers: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)</span></span>

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
[cloudový e-mailovou službu]: https://sendgrid.com/email-solutions
[doručování e-mailem transakční]: https://sendgrid.com/transactional-email
