---
title: "Jak používat služby sendgrid vám umožňuje e-mailů (.NET) | Microsoft Docs"
description: "Zjistěte, jak odeslat e-mailu pomocí e-mailovou službu sendgrid vám umožňuje v Azure. Ukázky kódu jsou vytvořené v C# a používají .NET API."
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
ms.openlocfilehash: b3a48b3c838763b022a18e55817ec7455fe94c85
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-send-email-using-sendgrid-with-azure"></a><span data-ttu-id="0f27b-104">Postup odesílání e-mailu pomocí sendgrid vám umožňuje pomocí Azure</span><span class="sxs-lookup"><span data-stu-id="0f27b-104">How to Send Email Using SendGrid with Azure</span></span>
## <a name="overview"></a><span data-ttu-id="0f27b-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="0f27b-105">Overview</span></span>
<span data-ttu-id="0f27b-106">Tato příručka ukazuje, jak provádět běžné úkoly programování s e-mailovou službu sendgrid vám umožňuje v Azure.</span><span class="sxs-lookup"><span data-stu-id="0f27b-106">This guide demonstrates how to perform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="0f27b-107">Ukázky jsou napsané v jazyce C\# a podporuje standardní 1.3 .NET.</span><span class="sxs-lookup"><span data-stu-id="0f27b-107">The samples are written in C\# and supports .NET Standard 1.3.</span></span> <span data-ttu-id="0f27b-108">Pokryté scénáře zahrnují vytváření e-mailu, odesílání e-mailu, přidání příloh a povolení různé e-mailu a sledování nastavení.</span><span class="sxs-lookup"><span data-stu-id="0f27b-108">The scenarios covered include constructing email, sending email, adding attachments, and enabling various mail and tracking settings.</span></span> <span data-ttu-id="0f27b-109">Další informace o sendgrid vám umožňuje a odesílání e-mailu, najdete v článku [další kroky] [ Next steps] části.</span><span class="sxs-lookup"><span data-stu-id="0f27b-109">For more information on SendGrid and sending email, see the [Next steps][Next steps] section.</span></span>

## <a name="what-is-the-sendgrid-email-service"></a><span data-ttu-id="0f27b-110">Co je služba sendgrid vám umožňuje e-mailu?</span><span class="sxs-lookup"><span data-stu-id="0f27b-110">What is the SendGrid Email Service?</span></span>
<span data-ttu-id="0f27b-111">Je sendgrid vám umožňuje [cloudový e-mailovou službu] poskytuje spolehlivé [doručování e-mailem transakční], škálovatelnost a analýzu v reálném čase společně s flexibilní rozhraní API, které umožňují snadnou vlastní integrace.</span><span class="sxs-lookup"><span data-stu-id="0f27b-111">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="0f27b-112">Běžné případy použití sendgrid vám umožňuje patří:</span><span class="sxs-lookup"><span data-stu-id="0f27b-112">Common SendGrid use cases include:</span></span>

* <span data-ttu-id="0f27b-113">Automatické odesílání potvrzení nebo potvrzení nákupní zákazníkům.</span><span class="sxs-lookup"><span data-stu-id="0f27b-113">Automatically sending receipts or purchase confirmations to customers.</span></span>
* <span data-ttu-id="0f27b-114">Správa distribuce uvádí pro odesílání zákazníkům měsíční letáků a reklamními nabídkami.</span><span class="sxs-lookup"><span data-stu-id="0f27b-114">Administering distribution lists for sending customers monthly fliers and promotions.</span></span>
* <span data-ttu-id="0f27b-115">Shromažďování metriky v reálném čase pro takové věci, jako e-mailu blokovaný a zákaznické engagement.</span><span class="sxs-lookup"><span data-stu-id="0f27b-115">Collecting real-time metrics for things like blocked email and customer engagement.</span></span>
* <span data-ttu-id="0f27b-116">Předávání dotazy zákazníků.</span><span class="sxs-lookup"><span data-stu-id="0f27b-116">Forwarding customer inquiries.</span></span>
* <span data-ttu-id="0f27b-117">Zpracování příchozích e-mailů.</span><span class="sxs-lookup"><span data-stu-id="0f27b-117">Processing incoming emails.</span></span>

<span data-ttu-id="0f27b-118">Další informace najdete v článku [https://sendgrid.com](https://sendgrid.com) nebo sendgrid vám umožňuje na [knihovna jazyka C#] [ sendgrid-csharp] úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="0f27b-118">For more information, visit [https://sendgrid.com](https://sendgrid.com) or SendGrid's [C# library][sendgrid-csharp] GitHub repo.</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="0f27b-119">Vytvoření účtu sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="0f27b-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a><span data-ttu-id="0f27b-120">Odkaz na knihovně tříd rozhraní .NET sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="0f27b-120">Reference the SendGrid .NET Class Library</span></span>
<span data-ttu-id="0f27b-121">[Balíček NuGet sendgrid vám umožňuje](https://www.nuget.org/packages/Sendgrid) Nejsnadnějším způsobem, chcete-li získat sendgrid vám umožňuje rozhraní API a ke konfiguraci vaší aplikace s všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="0f27b-121">The [SendGrid NuGet package](https://www.nuget.org/packages/Sendgrid) is the easiest way to get the SendGrid API and to configure your application with all dependencies.</span></span> <span data-ttu-id="0f27b-122">NuGet je součástí sady Microsoft Visual Studio 2015 rozšíření sady Visual Studio a vyšší usnadňuje instalace a aktualizace knihovny a nástroje.</span><span class="sxs-lookup"><span data-stu-id="0f27b-122">NuGet is a Visual Studio extension included with Microsoft Visual Studio 2015 and above that makes it easy to install and update libraries and tools.</span></span>

> [!NOTE]
> <span data-ttu-id="0f27b-123">Nainstalovat NuGet, pokud používáte verzi sady Visual Studio starší než Visual Studio 2015, najdete [http://www.nuget.org](http://www.nuget.org)a klikněte na **nainstalovat NuGet** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0f27b-123">To install NuGet if you are running a version of Visual Studio earlier than Visual Studio 2015, visit [http://www.nuget.org](http://www.nuget.org), and click the **Install NuGet** button.</span></span>
>
>

<span data-ttu-id="0f27b-124">Chcete-li nainstalovat balíček NuGet sendgrid vám umožňuje v aplikaci, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="0f27b-124">To install the SendGrid NuGet package in your application, do the following:</span></span>

1. <span data-ttu-id="0f27b-125">Klikněte na **nový projekt** a vyberte **šablony**.</span><span class="sxs-lookup"><span data-stu-id="0f27b-125">Click on **New Project** and select a **Template**.</span></span>

   ![Vytvoření nového projektu][create-new-project]
2. <span data-ttu-id="0f27b-127">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **odkazy**, pak klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="0f27b-127">In **Solution Explorer**, right-click **References**, then click **Manage NuGet Packages**.</span></span>

   ![Balíček NuGet sendgrid vám umožňuje][SendGrid-NuGet-package]
3. <span data-ttu-id="0f27b-129">Vyhledejte **sendgrid vám umožňuje** a vyberte **sendgrid vám umožňuje** položky v seznamu výsledků.</span><span class="sxs-lookup"><span data-stu-id="0f27b-129">Search for **SendGrid** and select the **SendGrid** item in the results list.</span></span>
4. <span data-ttu-id="0f27b-130">Vyberte z rozevíracího seznamu verze mohli pracovat v objektovém modelu nejnovější stabilní verze balíčku Nuget a rozhraní API ukázáno v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="0f27b-130">Select the latest stable version of the Nuget package from the version dropdown to be able to work with the object model and APIs demonstrated in this article.</span></span>

   ![Sendgrid vám umožňuje balíčku][sendgrid-package]
5. <span data-ttu-id="0f27b-132">Klikněte na tlačítko **nainstalovat** k dokončení instalace a zavřete toto dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0f27b-132">Click **Install** to complete the installation, and then close this dialog.</span></span>

<span data-ttu-id="0f27b-133">Knihovna tříd rozhraní .NET sendgrid vám umožňuje na nazývá **sendgrid vám umožňuje**.</span><span class="sxs-lookup"><span data-stu-id="0f27b-133">SendGrid's .NET class library is called **SendGrid**.</span></span> <span data-ttu-id="0f27b-134">Obsahuje následující obory názvů:</span><span class="sxs-lookup"><span data-stu-id="0f27b-134">It contains the following namespaces:</span></span>

* <span data-ttu-id="0f27b-135">**Sendgrid vám umožňuje** pro komunikaci s rozhraním API sendgrid vám umožňuje společnosti.</span><span class="sxs-lookup"><span data-stu-id="0f27b-135">**SendGrid** for communicating with SendGrid’s API.</span></span>
* <span data-ttu-id="0f27b-136">**SendGrid.Helpers.Mail** pro pomocné metody snadno vytvářet SendGridMessage objekty, které určují, jak odeslat e-mailů.</span><span class="sxs-lookup"><span data-stu-id="0f27b-136">**SendGrid.Helpers.Mail** for helper methods to easily create SendGridMessage objects that specify how to send emails.</span></span>

<span data-ttu-id="0f27b-137">Přidejte následující deklarace oborů názvů kódu do horní části souboru žádné C# ve kterém chcete programový přístupu k e-mailovou službu sendgrid vám umožňuje.</span><span class="sxs-lookup"><span data-stu-id="0f27b-137">Add the following code namespace declarations to the top of any C# file in which you want to programmatically access the SendGrid email service.</span></span>

    using SendGrid;
    using SendGrid.Helpers.Mail;

## <a name="how-to-create-an-email"></a><span data-ttu-id="0f27b-138">Postupy: vytvoření e-mailu</span><span class="sxs-lookup"><span data-stu-id="0f27b-138">How to: Create an Email</span></span>
<span data-ttu-id="0f27b-139">Použití **SendGridMessage** objekt k vytvoření e-mailovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="0f27b-139">Use the **SendGridMessage** object to create an email message.</span></span> <span data-ttu-id="0f27b-140">Po vytvoření objektu zprávu můžete nastavit vlastnosti a metody, včetně e-mailu odesílatel, příjemce e-mailu a předmět a text e-mailu.</span><span class="sxs-lookup"><span data-stu-id="0f27b-140">Once the message object is created, you can set properties and methods, including the email sender, the email recipient, and the subject and body of the email.</span></span>

<span data-ttu-id="0f27b-141">Následující příklad ukazuje, jak vytvořit objekt plně vyplněná e-mailu:</span><span class="sxs-lookup"><span data-stu-id="0f27b-141">The following example demonstrates how to create a fully populated email object:</span></span>

    var msg = new SendGridMessage();

    msg.SetFrom(new EmailAddress("dx@example.com", "SendGrid DX Team"));

    var recipients = new List<EmailAddress>
    {
        new EmailAddress("jeff@example.com", "Jeff Smith"),
        new EmailAddress("anna@example.com", "Anna Lidman"),
        new EmailAddress("peter@example.com", "Peter Saddow")
    };
    msg.AddTos(recipients);

    msg.SetSubject("Testing the SendGrid C# Library");

    msg.AddContent(MimeType.Text, "Hello World plain text!");
    msg.AddContent(MimeType.Html, "<p>Hello World!</p>");

<span data-ttu-id="0f27b-142">Další informace o všech vlastností a metod podporovaných **sendgrid vám umožňuje** zadejte najdete v tématu [sendgrid vám umožňuje csharp] [ sendgrid-csharp] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="0f27b-142">For more information on all properties and methods supported by the **SendGrid** type, see [sendgrid-csharp][sendgrid-csharp] on GitHub.</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="0f27b-143">Postupy: odeslání e-mailu</span><span class="sxs-lookup"><span data-stu-id="0f27b-143">How to: Send an Email</span></span>
<span data-ttu-id="0f27b-144">Po vytvoření e-mailovou zprávu, můžete ho pomocí rozhraní API sendgrid vám umožňuje společnosti odeslat.</span><span class="sxs-lookup"><span data-stu-id="0f27b-144">After creating an email message, you can send it using SendGrid's API.</span></span> <span data-ttu-id="0f27b-145">Alternativně můžete použít [. NET je součástí knihovny][NET-library].</span><span class="sxs-lookup"><span data-stu-id="0f27b-145">Alternatively, you may use [.NET's built in library][NET-library].</span></span>

<span data-ttu-id="0f27b-146">Odesílání e-mailu vyžaduje, že je klíč rozhraní API sendgrid vám umožňuje zadat.</span><span class="sxs-lookup"><span data-stu-id="0f27b-146">Sending email requires that you supply your SendGrid API Key.</span></span> <span data-ttu-id="0f27b-147">Pokud potřebujete podrobnosti o tom, jak nakonfigurovat klíče rozhraní API, navštivte klíče rozhraní API sendgrid vám umožňuje na [dokumentace][documentation].</span><span class="sxs-lookup"><span data-stu-id="0f27b-147">If you need details about how to configure API Keys, please visit SendGrid's API Keys [documentation][documentation].</span></span>

<span data-ttu-id="0f27b-148">Tyto přihlašovací údaje prostřednictvím portálu Azure může uložit kliknutím na tlačítko Nastavení aplikace a přidáním páry klíč/hodnota v části Nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="0f27b-148">You may store these credentials via your Azure Portal by clicking Application settings and adding the key/value pairs under App settings.</span></span>

 ![Nastavení aplikace Azure][azure_app_settings]

 <span data-ttu-id="0f27b-150">Potom můžete získat z nich následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0f27b-150">Then, you may access them as follows:</span></span>

    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
    var client = new SendGridClient(apiKey);

<span data-ttu-id="0f27b-151">Následující příklady ukazují, jak odeslat zprávu pomocí webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0f27b-151">The following examples show how to send a message using the Web API.</span></span>

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
                    Subject = "Hello World from the SendGrid CSharp SDK!",
                    PlainTextContent = "Hello, Email!",
                    HtmlContent = "<strong>Hello, Email!</strong>"
                };
                msg.AddTo(new EmailAddress("test@example.com", "Test User"));
                var response = await client.SendEmailAsync(msg);
            }
        }
    }

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="0f27b-152">Postupy: Přidání přílohy</span><span class="sxs-lookup"><span data-stu-id="0f27b-152">How to: Add an attachment</span></span>
<span data-ttu-id="0f27b-153">Přílohy mohou být přidány do zprávy voláním **AddAttachment** metoda a minimálně zadáte název souboru a kódováním Base64 obsahu, které chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="0f27b-153">Attachments can be added to a message by calling the **AddAttachment** method and minimally specifying the file name and Base64 encoded content you want to attach.</span></span> <span data-ttu-id="0f27b-154">Může obsahovat více přiložených, po pro každý soubor chcete připojit, a to voláním této metody nebo pomocí **AddAttachments** metoda.</span><span class="sxs-lookup"><span data-stu-id="0f27b-154">You can include multiple attachments by calling this method once for each file you wish to attach or by using the **AddAttachments** method.</span></span> <span data-ttu-id="0f27b-155">Následující příklad ukazuje, přidání přílohu zprávu:</span><span class="sxs-lookup"><span data-stu-id="0f27b-155">The following example demonstrates adding an attachment to a message:</span></span>

    var banner2 = new Attachment()
    {
        Content = Convert.ToBase64String(raw_content),
        Type = "image/png",
        Filename = "banner2.png",
        Disposition = "inline",
        ContentId = "Banner 2"
    };
    msg.AddAttachment(banner2);

## <a name="how-to-use-mail-settings-to-enable-footers-tracking-and-analytics"></a><span data-ttu-id="0f27b-156">Postupy: nastavení e-mailu slouží k povolení zápatí stránky, sledování a analýzy</span><span class="sxs-lookup"><span data-stu-id="0f27b-156">How to: Use mail settings to enable footers, tracking, and analytics</span></span>
<span data-ttu-id="0f27b-157">Sendgrid vám umožňuje poskytuje funkce další e-mailu pomocí e-mailu nastavení a nastavení sledování.</span><span class="sxs-lookup"><span data-stu-id="0f27b-157">SendGrid provides additional email functionality through the use of mail settings and tracking settings.</span></span> <span data-ttu-id="0f27b-158">Tato nastavení lze přidat do e-mailovou zprávu povolit konkrétní funkce, jako je sledování klikněte na tlačítko, Google analytics, předplatné, sledování a tak dále.</span><span class="sxs-lookup"><span data-stu-id="0f27b-158">These settings can be added to an email message to enable specific functionality such as click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="0f27b-159">Úplný seznam aplikací, najdete v článku [nastavení dokumentaci][settings-documentation].</span><span class="sxs-lookup"><span data-stu-id="0f27b-159">For a full list of apps, see the [Settings Documentation][settings-documentation].</span></span>

<span data-ttu-id="0f27b-160">Aplikace lze použít pro **sendgrid vám umožňuje** e-mailové zprávy pomocí metody implementované jako součást **SendGridMessage** třídy.</span><span class="sxs-lookup"><span data-stu-id="0f27b-160">Apps can be applied to **SendGrid** email messages using methods implemented as part of the **SendGridMessage** class.</span></span> <span data-ttu-id="0f27b-161">Následující příklady ukazují zápatí a klikněte na tlačítko Sledování filtry:</span><span class="sxs-lookup"><span data-stu-id="0f27b-161">The following examples demonstrate the footer and click tracking filters:</span></span>

<span data-ttu-id="0f27b-162">Následující příklady ukazují zápatí a klikněte na tlačítko Sledování filtry:</span><span class="sxs-lookup"><span data-stu-id="0f27b-162">The following examples demonstrate the footer and click tracking filters:</span></span>

### <a name="footer-settings"></a><span data-ttu-id="0f27b-163">Nastavení zápatí</span><span class="sxs-lookup"><span data-stu-id="0f27b-163">Footer settings</span></span>
    msg.SetFooterSetting(
                         true,
                         "Some Footer HTML",
                         "<strong>Some Footer Text</strong>");

### <a name="click-tracking"></a><span data-ttu-id="0f27b-164">Klikněte na tlačítko Sledování</span><span class="sxs-lookup"><span data-stu-id="0f27b-164">Click tracking</span></span>
    msg.SetClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="0f27b-165">Postupy: použití služeb další sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="0f27b-165">How to: Use Additional SendGrid Services</span></span>
<span data-ttu-id="0f27b-166">Sendgrid vám umožňuje nabízí několik rozhraní API a pomocí webhooků, které můžete využít další funkcionalitu v rámci aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="0f27b-166">SendGrid offers several APIs and webhooks that you can use to leverage additional functionality within your Azure application.</span></span> <span data-ttu-id="0f27b-167">Další podrobnosti najdete v tématu [referenční dokumentace rozhraní API sendgrid vám umožňuje][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="0f27b-167">For more details, see the [SendGrid API Reference][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f27b-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f27b-168">Next steps</span></span>
<span data-ttu-id="0f27b-169">Teď, když jste se naučili základy služby sendgrid vám umožňuje e-mailu, postupujte podle následujících odkazech na další informace.</span><span class="sxs-lookup"><span data-stu-id="0f27b-169">Now that you've learned the basics of the SendGrid Email service, follow these links to learn more.</span></span>

* <span data-ttu-id="0f27b-170">C sendgrid vám umožňuje\# knihovny úložišti: [sendgrid vám umožňuje csharp][sendgrid-csharp]</span><span class="sxs-lookup"><span data-stu-id="0f27b-170">SendGrid C\# library repo: [sendgrid-csharp][sendgrid-csharp]</span></span>
* <span data-ttu-id="0f27b-171">Dokumentaci k rozhraní API sendgrid vám umožňuje: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="0f27b-171">SendGrid API documentation: <https://sendgrid.com/docs></span></span>

[Next steps]: #next-steps
[What is the SendGrid Email Service?]: #whatis
[Create a SendGrid Account]: #createaccount
[Reference the SendGrid .NET Class Library]: #reference
[How to: Create an Email]: #createemail
[How to: Send an Email]: #sendemail
[How to: Add an Attachment]: #addattachment
[How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
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

<span data-ttu-id="0f27b-172">[cloudový e-mailovou službu]: https://sendgrid.com/solutions</span><span class="sxs-lookup"><span data-stu-id="0f27b-172">[cloud-based email service]: https://sendgrid.com/solutions</span></span>
<span data-ttu-id="0f27b-173">[doručování e-mailem transakční]: https://sendgrid.com/use-cases/transactional-email</span><span class="sxs-lookup"><span data-stu-id="0f27b-173">[transactional email delivery]: https://sendgrid.com/use-cases/transactional-email</span></span>

