---
title: "aaaHow toouse hello sendgrid vám umožňuje e-mailovou službu (.NET) | Microsoft Docs"
description: "Zjistěte, jak odeslat e-mail s hello sendgrid vám umožňuje e-mailovou službu v Azure. Ukázky kódu jsou vytvořené v C# a použití hello .NET API."
services: app-service-web
documentationcenter: .net
author: thinkingserious
manager: erikre
editor: 
ms.assetid: 21bf4028-9046-476b-9799-3d3082a0f84c
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/15/2017
ms.author: dx@sendgrid.com
ms.openlocfilehash: b3d77bb67898b991c7293e6b9086b263f6bcb755
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-with-azure"></a><span data-ttu-id="c06fe-104">Jak tooSend e-mailu pomocí sendgrid vám umožňuje pomocí Azure</span><span class="sxs-lookup"><span data-stu-id="c06fe-104">How tooSend Email Using SendGrid with Azure</span></span>
## <a name="overview"></a><span data-ttu-id="c06fe-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="c06fe-105">Overview</span></span>
<span data-ttu-id="c06fe-106">Tato příručka ukazuje, jak tooperform běžné úlohy programování sendgridu e-mailové služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="c06fe-106">This guide demonstrates how tooperform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="c06fe-107">Hello ukázky jsou napsané v jazyce C\# a podporuje standardní 1.3 .NET.</span><span class="sxs-lookup"><span data-stu-id="c06fe-107">hello samples are written in C\# and supports .NET Standard 1.3.</span></span> <span data-ttu-id="c06fe-108">Hello pokryté scénáře zahrnují vytváření e-mailu, odesílání e-mailu, přidání příloh a povolení různé e-mailu a sledování nastavení.</span><span class="sxs-lookup"><span data-stu-id="c06fe-108">hello scenarios covered include constructing email, sending email, adding attachments, and enabling various mail and tracking settings.</span></span> <span data-ttu-id="c06fe-109">Další informace o sendgrid vám umožňuje a odesílání e-mailu, najdete v části hello [další kroky] [ Next steps] části.</span><span class="sxs-lookup"><span data-stu-id="c06fe-109">For more information on SendGrid and sending email, see hello [Next steps][Next steps] section.</span></span>

## <a name="what-is-hello-sendgrid-email-service"></a><span data-ttu-id="c06fe-110">Co je hello sendgrid vám umožňuje e-mailovou službu?</span><span class="sxs-lookup"><span data-stu-id="c06fe-110">What is hello SendGrid Email Service?</span></span>
<span data-ttu-id="c06fe-111">Je sendgrid vám umožňuje [cloudový e-mailovou službu] poskytuje spolehlivé [doručování e-mailem transakční], škálovatelnost a analýzu v reálném čase společně s flexibilní rozhraní API, které umožňují snadnou vlastní integrace.</span><span class="sxs-lookup"><span data-stu-id="c06fe-111">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="c06fe-112">Běžné případy použití sendgrid vám umožňuje patří:</span><span class="sxs-lookup"><span data-stu-id="c06fe-112">Common SendGrid use cases include:</span></span>

* <span data-ttu-id="c06fe-113">Odesílání automaticky potvrzení nebo toocustomers potvrzení nákupu.</span><span class="sxs-lookup"><span data-stu-id="c06fe-113">Automatically sending receipts or purchase confirmations toocustomers.</span></span>
* <span data-ttu-id="c06fe-114">Správa distribuce uvádí pro odesílání zákazníkům měsíční letáků a reklamními nabídkami.</span><span class="sxs-lookup"><span data-stu-id="c06fe-114">Administering distribution lists for sending customers monthly fliers and promotions.</span></span>
* <span data-ttu-id="c06fe-115">Shromažďování metriky v reálném čase pro takové věci, jako e-mailu blokovaný a zákaznické engagement.</span><span class="sxs-lookup"><span data-stu-id="c06fe-115">Collecting real-time metrics for things like blocked email and customer engagement.</span></span>
* <span data-ttu-id="c06fe-116">Předávání dotazy zákazníků.</span><span class="sxs-lookup"><span data-stu-id="c06fe-116">Forwarding customer inquiries.</span></span>
* <span data-ttu-id="c06fe-117">Zpracování příchozích e-mailů.</span><span class="sxs-lookup"><span data-stu-id="c06fe-117">Processing incoming emails.</span></span>

<span data-ttu-id="c06fe-118">Další informace najdete v článku [https://sendgrid.com](https://sendgrid.com) nebo sendgrid vám umožňuje na [knihovna jazyka C#] [ sendgrid-csharp] úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="c06fe-118">For more information, visit [https://sendgrid.com](https://sendgrid.com) or SendGrid's [C# library][sendgrid-csharp] GitHub repo.</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="c06fe-119">Vytvoření účtu sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="c06fe-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-hello-sendgrid-net-class-library"></a><span data-ttu-id="c06fe-120">Referenční dokumentace hello sendgrid vám umožňuje knihovna tříd rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="c06fe-120">Reference hello SendGrid .NET Class Library</span></span>
<span data-ttu-id="c06fe-121">Hello [balíček NuGet sendgrid vám umožňuje](https://www.nuget.org/packages/Sendgrid) je hello nejjednodušší způsob, jak tooget hello sendgrid vám umožňuje rozhraní API a tooconfigure vaší aplikace pomocí všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="c06fe-121">hello [SendGrid NuGet package](https://www.nuget.org/packages/Sendgrid) is hello easiest way tooget hello SendGrid API and tooconfigure your application with all dependencies.</span></span> <span data-ttu-id="c06fe-122">NuGet je Visual Studio rozšíření obsahovala s Microsoft Visual Studio 2015 a vyšší umožňuje snadno tooinstall a aktualizace knihovny a nástroje.</span><span class="sxs-lookup"><span data-stu-id="c06fe-122">NuGet is a Visual Studio extension included with Microsoft Visual Studio 2015 and above that makes it easy tooinstall and update libraries and tools.</span></span>

> [!NOTE]
> <span data-ttu-id="c06fe-123">tooinstall NuGet, pokud používáte verzi sady Visual Studio starší než Visual Studio 2015, navštivte [http://www.nuget.org](http://www.nuget.org)a klikněte na tlačítko hello **nainstalovat NuGet** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c06fe-123">tooinstall NuGet if you are running a version of Visual Studio earlier than Visual Studio 2015, visit [http://www.nuget.org](http://www.nuget.org), and click hello **Install NuGet** button.</span></span>
>
>

<span data-ttu-id="c06fe-124">hello tooinstall balíček NuGet sendgrid vám umožňuje v aplikaci hello následující:</span><span class="sxs-lookup"><span data-stu-id="c06fe-124">tooinstall hello SendGrid NuGet package in your application, do hello following:</span></span>

1. <span data-ttu-id="c06fe-125">Klikněte na **nový projekt** a vyberte **šablony**.</span><span class="sxs-lookup"><span data-stu-id="c06fe-125">Click on **New Project** and select a **Template**.</span></span>

   ![Vytvoření nového projektu][create-new-project]
2. <span data-ttu-id="c06fe-127">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **odkazy**, pak klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c06fe-127">In **Solution Explorer**, right-click **References**, then click **Manage NuGet Packages**.</span></span>

   ![Balíček NuGet sendgrid vám umožňuje][SendGrid-NuGet-package]
3. <span data-ttu-id="c06fe-129">Vyhledejte **sendgrid vám umožňuje** a vyberte hello **sendgrid vám umožňuje** položky v seznamu výsledků.</span><span class="sxs-lookup"><span data-stu-id="c06fe-129">Search for **SendGrid** and select hello **SendGrid** item in the results list.</span></span>
4. <span data-ttu-id="c06fe-130">Vyberte hello nejnovější stabilní verze balíčku Nuget hello z hello verze rozevírací toobe možné toowork s hello objektový model a rozhraní API ukázáno v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="c06fe-130">Select hello latest stable version of hello Nuget package from hello version dropdown toobe able toowork with hello object model and APIs demonstrated in this article.</span></span>

   ![Sendgrid vám umožňuje balíčku][sendgrid-package]
5. <span data-ttu-id="c06fe-132">Klikněte na tlačítko **nainstalovat** toocomplete hello instalace a zavřete toto dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c06fe-132">Click **Install** toocomplete hello installation, and then close this dialog.</span></span>

<span data-ttu-id="c06fe-133">Knihovna tříd rozhraní .NET sendgrid vám umožňuje na nazývá **sendgrid vám umožňuje**.</span><span class="sxs-lookup"><span data-stu-id="c06fe-133">SendGrid's .NET class library is called **SendGrid**.</span></span> <span data-ttu-id="c06fe-134">Obsahuje hello následující obory názvů:</span><span class="sxs-lookup"><span data-stu-id="c06fe-134">It contains hello following namespaces:</span></span>

* <span data-ttu-id="c06fe-135">**Sendgrid vám umožňuje** pro komunikaci s rozhraním API sendgrid vám umožňuje společnosti.</span><span class="sxs-lookup"><span data-stu-id="c06fe-135">**SendGrid** for communicating with SendGrid’s API.</span></span>
* <span data-ttu-id="c06fe-136">**SendGrid.Helpers.Mail** pomocníka tooeasily metody vytvoření SendGridMessage objekty, které určují, jak toosend e-maily.</span><span class="sxs-lookup"><span data-stu-id="c06fe-136">**SendGrid.Helpers.Mail** for helper methods tooeasily create SendGridMessage objects that specify how toosend emails.</span></span>

<span data-ttu-id="c06fe-137">Přidejte následující kód obor názvů deklarace toohello horní jakéhokoliv C# souboru, ve kterém chcete tooprogrammatically přístup hello sendgrid vám umožňuje e-mailovou službu hello.</span><span class="sxs-lookup"><span data-stu-id="c06fe-137">Add hello following code namespace declarations toohello top of any C# file in which you want tooprogrammatically access hello SendGrid email service.</span></span>

    using SendGrid;
    using SendGrid.Helpers.Mail;

## <a name="how-to-create-an-email"></a><span data-ttu-id="c06fe-138">Postupy: vytvoření e-mailu</span><span class="sxs-lookup"><span data-stu-id="c06fe-138">How to: Create an Email</span></span>
<span data-ttu-id="c06fe-139">Použití hello **SendGridMessage** objektu toocreate e-mailovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="c06fe-139">Use hello **SendGridMessage** object toocreate an email message.</span></span> <span data-ttu-id="c06fe-140">Po vytvoření objektu zpráva hello můžete nastavit vlastnosti a metody, včetně hello odesílatelem e-mailu, hello příjemce e-mailu a hello předmět a text e-mailů hello.</span><span class="sxs-lookup"><span data-stu-id="c06fe-140">Once hello message object is created, you can set properties and methods, including hello email sender, hello email recipient, and hello subject and body of hello email.</span></span>

<span data-ttu-id="c06fe-141">Hello následující příklad ukazuje, jak toocreate objekt plně vyplněná e-mailu:</span><span class="sxs-lookup"><span data-stu-id="c06fe-141">hello following example demonstrates how toocreate a fully populated email object:</span></span>

    var msg = new SendGridMessage();

    msg.SetFrom(new EmailAddress("dx@example.com", "SendGrid DX Team"));

    var recipients = new List<EmailAddress>
    {
        new EmailAddress("jeff@example.com", "Jeff Smith"),
        new EmailAddress("anna@example.com", "Anna Lidman"),
        new EmailAddress("peter@example.com", "Peter Saddow")
    };
    msg.AddTos(recipients);

    msg.SetSubject("Testing hello SendGrid C# Library");

    msg.AddContent(MimeType.Text, "Hello World plain text!");
    msg.AddContent(MimeType.Html, "<p>Hello World!</p>");

<span data-ttu-id="c06fe-142">Další informace o všech vlastností a metod podporovaných **sendgrid vám umožňuje** zadejte najdete v tématu [sendgrid vám umožňuje csharp] [ sendgrid-csharp] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="c06fe-142">For more information on all properties and methods supported by the **SendGrid** type, see [sendgrid-csharp][sendgrid-csharp] on GitHub.</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="c06fe-143">Postupy: odeslání e-mailu</span><span class="sxs-lookup"><span data-stu-id="c06fe-143">How to: Send an Email</span></span>
<span data-ttu-id="c06fe-144">Po vytvoření e-mailovou zprávu, můžete ho pomocí rozhraní API sendgrid vám umožňuje společnosti odeslat.</span><span class="sxs-lookup"><span data-stu-id="c06fe-144">After creating an email message, you can send it using SendGrid's API.</span></span> <span data-ttu-id="c06fe-145">Alternativně můžete použít [. NET je součástí knihovny][NET-library].</span><span class="sxs-lookup"><span data-stu-id="c06fe-145">Alternatively, you may use [.NET's built in library][NET-library].</span></span>

<span data-ttu-id="c06fe-146">Odesílání e-mailu vyžaduje, že je klíč rozhraní API sendgrid vám umožňuje zadat.</span><span class="sxs-lookup"><span data-stu-id="c06fe-146">Sending email requires that you supply your SendGrid API Key.</span></span> <span data-ttu-id="c06fe-147">Pokud potřebujete informace o tooconfigure klíče rozhraní API, navštivte klíče rozhraní API sendgrid vám umožňuje na [dokumentace][documentation].</span><span class="sxs-lookup"><span data-stu-id="c06fe-147">If you need details about how tooconfigure API Keys, please visit SendGrid's API Keys [documentation][documentation].</span></span>

<span data-ttu-id="c06fe-148">Tyto přihlašovací údaje mohou ukládat prostřednictvím portálu Azure tak, že kliknete na příkaz Nastavení aplikace a přidat páry klíč – hodnota hello v části Nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c06fe-148">You may store these credentials via your Azure Portal by clicking Application settings and adding hello key/value pairs under App settings.</span></span>

 ![Nastavení aplikace Azure][azure_app_settings]

 <span data-ttu-id="c06fe-150">Potom můžete získat z nich následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c06fe-150">Then, you may access them as follows:</span></span>

    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
    var client = new SendGridClient(apiKey);

<span data-ttu-id="c06fe-151">Hello následující příklady ukazují, jak hello toosend zprávu pomocí webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c06fe-151">hello following examples show how toosend a message using hello Web API.</span></span>

    using System;
    using System.Threading.Tasks;
    using SendGrid;
    using SendGrid.Helpers.Mail;

    namespace Example
    {
        internal class Example
        {
            private static void Main()
            {
                Execute().Wait();
            }

            static async Task Execute()
            {
                var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
                var client = new SendGridClient(apiKey);
                var msg = new SendGridMessage()
                {
                    From = new EmailAddress("test@example.com", "DX Team"),
                    Subject = "Hello World from hello SendGrid CSharp SDK!",
                    PlainTextContent = "Hello, Email!",
                    HtmlContent = "<strong>Hello, Email!</strong>"
                };
                msg.AddTo(new EmailAddress("test@example.com", "Test User"));
                var response = await client.SendEmailAsync(msg);
            }
        }
    }

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="c06fe-152">Postupy: Přidání přílohy</span><span class="sxs-lookup"><span data-stu-id="c06fe-152">How to: Add an attachment</span></span>
<span data-ttu-id="c06fe-153">Přílohy lze přidat zprávu tooa pomocí volání hello **AddAttachment** metoda a minimálně zadáte název souboru hello a kódováním Base64 obsahu, které chcete tooattach.</span><span class="sxs-lookup"><span data-stu-id="c06fe-153">Attachments can be added tooa message by calling hello **AddAttachment** method and minimally specifying hello file name and Base64 encoded content you want tooattach.</span></span> <span data-ttu-id="c06fe-154">Může obsahovat více přiložených, voláním této metody po pro každý soubor chcete tooattach nebo pomocí hello **AddAttachments** metoda.</span><span class="sxs-lookup"><span data-stu-id="c06fe-154">You can include multiple attachments by calling this method once for each file you wish tooattach or by using hello **AddAttachments** method.</span></span> <span data-ttu-id="c06fe-155">Hello následující příklad ukazuje, přidání zprávu tooa přílohy:</span><span class="sxs-lookup"><span data-stu-id="c06fe-155">hello following example demonstrates adding an attachment tooa message:</span></span>

    var banner2 = new Attachment()
    {
        Content = Convert.ToBase64String(raw_content),
        Type = "image/png",
        Filename = "banner2.png",
        Disposition = "inline",
        ContentId = "Banner 2"
    };
    msg.AddAttachment(banner2);

## <a name="how-to-use-mail-settings-tooenable-footers-tracking-and-analytics"></a><span data-ttu-id="c06fe-156">Postupy: použití zápatí tooenable nastavení e-mailu, sledování a analýzy</span><span class="sxs-lookup"><span data-stu-id="c06fe-156">How to: Use mail settings tooenable footers, tracking, and analytics</span></span>
<span data-ttu-id="c06fe-157">Sendgrid vám umožňuje poskytuje další e-mailové funkce prostřednictvím hello použití nastavení pošty a sledování nastavení.</span><span class="sxs-lookup"><span data-stu-id="c06fe-157">SendGrid provides additional email functionality through hello use of mail settings and tracking settings.</span></span> <span data-ttu-id="c06fe-158">Tato nastavení mohou být přidány tooan e-mailové zprávy tooenable konkrétní funkce například klikněte na tlačítko sledování, Google analytics, předplatné, sledování a tak dále.</span><span class="sxs-lookup"><span data-stu-id="c06fe-158">These settings can be added tooan email message tooenable specific functionality such as click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="c06fe-159">Úplný seznam aplikací, najdete v části hello [nastavení dokumentaci][settings-documentation].</span><span class="sxs-lookup"><span data-stu-id="c06fe-159">For a full list of apps, see hello [Settings Documentation][settings-documentation].</span></span>

<span data-ttu-id="c06fe-160">Aplikace lze použít příliš**sendgrid vám umožňuje** e-mailové zprávy pomocí metody implementované jako součást hello **SendGridMessage** třídy.</span><span class="sxs-lookup"><span data-stu-id="c06fe-160">Apps can be applied too**SendGrid** email messages using methods implemented as part of hello **SendGridMessage** class.</span></span> <span data-ttu-id="c06fe-161">Hello následující příklady ukazují hello zápatí a klikněte na tlačítko Sledování filtry:</span><span class="sxs-lookup"><span data-stu-id="c06fe-161">hello following examples demonstrate hello footer and click tracking filters:</span></span>

<span data-ttu-id="c06fe-162">Hello následující příklady ukazují hello zápatí a klikněte na tlačítko Sledování filtry:</span><span class="sxs-lookup"><span data-stu-id="c06fe-162">hello following examples demonstrate hello footer and click tracking filters:</span></span>

### <a name="footer-settings"></a><span data-ttu-id="c06fe-163">Nastavení zápatí</span><span class="sxs-lookup"><span data-stu-id="c06fe-163">Footer settings</span></span>
    msg.SetFooterSetting(
                         true,
                         "Some Footer HTML",
                         "<strong>Some Footer Text</strong>");

### <a name="click-tracking"></a><span data-ttu-id="c06fe-164">Klikněte na tlačítko Sledování</span><span class="sxs-lookup"><span data-stu-id="c06fe-164">Click tracking</span></span>
    msg.SetClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="c06fe-165">Postupy: použití služeb další sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="c06fe-165">How to: Use Additional SendGrid Services</span></span>
<span data-ttu-id="c06fe-166">Sendgrid vám umožňuje nabízí několik rozhraní API a pomocí webhooků, které můžete použít další funkce tooleverage v aplikaci Azure.</span><span class="sxs-lookup"><span data-stu-id="c06fe-166">SendGrid offers several APIs and webhooks that you can use tooleverage additional functionality within your Azure application.</span></span> <span data-ttu-id="c06fe-167">Další podrobnosti najdete v tématu hello [referenční dokumentace rozhraní API sendgrid vám umožňuje][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="c06fe-167">For more details, see hello [SendGrid API Reference][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="c06fe-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c06fe-168">Next steps</span></span>
<span data-ttu-id="c06fe-169">Teď, když jste se naučili základy hello hello služby sendgrid vám umožňuje e-mailů, použijte tyto odkazy toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="c06fe-169">Now that you've learned hello basics of hello SendGrid Email service, follow these links toolearn more.</span></span>

* <span data-ttu-id="c06fe-170">C sendgrid vám umožňuje\# knihovny úložišti: [sendgrid vám umožňuje csharp][sendgrid-csharp]</span><span class="sxs-lookup"><span data-stu-id="c06fe-170">SendGrid C\# library repo: [sendgrid-csharp][sendgrid-csharp]</span></span>
* <span data-ttu-id="c06fe-171">Dokumentaci k rozhraní API sendgrid vám umožňuje: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="c06fe-171">SendGrid API documentation: <https://sendgrid.com/docs></span></span>

[Next steps]: #next-steps
[What is hello SendGrid Email Service?]: #whatis
[Create a SendGrid Account]: #createaccount
[Reference hello SendGrid .NET Class Library]: #reference
[How to: Create an Email]: #createemail
[How to: Send an Email]: #sendemail
[How to: Add an Attachment]: #addattachment
[How to: Use Filters tooEnable Footers, Tracking, and Analytics]: #usefilters
[How to: Use Additional SendGrid Services]: #useservices

[create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/new-project.png
[SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/reference.png
[sendgrid-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid-package.png
[azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/azure-app-settings.png
[sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
[SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
[App Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/api_v3.html
[NET-library]: https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html#-Using-NETs-Builtin-SMTP-Library
[documentation]: https://sendgrid.com/docs/Classroom/Send/api_keys.html
[settings-documentation]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html

[cloudový e-mailovou službu]: https://sendgrid.com/solutions
[doručování e-mailem transakční]: https://sendgrid.com/use-cases/transactional-email

