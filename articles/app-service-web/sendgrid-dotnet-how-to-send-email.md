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
# <a name="how-toosend-email-using-sendgrid-with-azure"></a>Jak tooSend e-mailu pomocí sendgrid vám umožňuje pomocí Azure
## <a name="overview"></a>Přehled
Tato příručka ukazuje, jak tooperform běžné úlohy programování sendgridu e-mailové služby v Azure. Hello ukázky jsou napsané v jazyce C\# a podporuje standardní 1.3 .NET. Hello pokryté scénáře zahrnují vytváření e-mailu, odesílání e-mailu, přidání příloh a povolení různé e-mailu a sledování nastavení. Další informace o sendgrid vám umožňuje a odesílání e-mailu, najdete v části hello [další kroky] [ Next steps] části.

## <a name="what-is-hello-sendgrid-email-service"></a>Co je hello sendgrid vám umožňuje e-mailovou službu?
Je sendgrid vám umožňuje [cloudový e-mailovou službu] poskytuje spolehlivé [doručování e-mailem transakční], škálovatelnost a analýzu v reálném čase společně s flexibilní rozhraní API, které umožňují snadnou vlastní integrace. Běžné případy použití sendgrid vám umožňuje patří:

* Odesílání automaticky potvrzení nebo toocustomers potvrzení nákupu.
* Správa distribuce uvádí pro odesílání zákazníkům měsíční letáků a reklamními nabídkami.
* Shromažďování metriky v reálném čase pro takové věci, jako e-mailu blokovaný a zákaznické engagement.
* Předávání dotazy zákazníků.
* Zpracování příchozích e-mailů.

Další informace najdete v článku [https://sendgrid.com](https://sendgrid.com) nebo sendgrid vám umožňuje na [knihovna jazyka C#] [ sendgrid-csharp] úložiště GitHub.

## <a name="create-a-sendgrid-account"></a>Vytvoření účtu sendgrid vám umožňuje
[!INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-hello-sendgrid-net-class-library"></a>Referenční dokumentace hello sendgrid vám umožňuje knihovna tříd rozhraní .NET
Hello [balíček NuGet sendgrid vám umožňuje](https://www.nuget.org/packages/Sendgrid) je hello nejjednodušší způsob, jak tooget hello sendgrid vám umožňuje rozhraní API a tooconfigure vaší aplikace pomocí všechny závislosti. NuGet je Visual Studio rozšíření obsahovala s Microsoft Visual Studio 2015 a vyšší umožňuje snadno tooinstall a aktualizace knihovny a nástroje.

> [!NOTE]
> tooinstall NuGet, pokud používáte verzi sady Visual Studio starší než Visual Studio 2015, navštivte [http://www.nuget.org](http://www.nuget.org)a klikněte na tlačítko hello **nainstalovat NuGet** tlačítko.
>
>

hello tooinstall balíček NuGet sendgrid vám umožňuje v aplikaci hello následující:

1. Klikněte na **nový projekt** a vyberte **šablony**.

   ![Vytvoření nového projektu][create-new-project]
2. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **odkazy**, pak klikněte na tlačítko **spravovat balíčky NuGet**.

   ![Balíček NuGet sendgrid vám umožňuje][SendGrid-NuGet-package]
3. Vyhledejte **sendgrid vám umožňuje** a vyberte hello **sendgrid vám umožňuje** položky v seznamu výsledků.
4. Vyberte hello nejnovější stabilní verze balíčku Nuget hello z hello verze rozevírací toobe možné toowork s hello objektový model a rozhraní API ukázáno v tomto článku.

   ![Sendgrid vám umožňuje balíčku][sendgrid-package]
5. Klikněte na tlačítko **nainstalovat** toocomplete hello instalace a zavřete toto dialogové okno.

Knihovna tříd rozhraní .NET sendgrid vám umožňuje na nazývá **sendgrid vám umožňuje**. Obsahuje hello následující obory názvů:

* **Sendgrid vám umožňuje** pro komunikaci s rozhraním API sendgrid vám umožňuje společnosti.
* **SendGrid.Helpers.Mail** pomocníka tooeasily metody vytvoření SendGridMessage objekty, které určují, jak toosend e-maily.

Přidejte následující kód obor názvů deklarace toohello horní jakéhokoliv C# souboru, ve kterém chcete tooprogrammatically přístup hello sendgrid vám umožňuje e-mailovou službu hello.

    using SendGrid;
    using SendGrid.Helpers.Mail;

## <a name="how-to-create-an-email"></a>Postupy: vytvoření e-mailu
Použití hello **SendGridMessage** objektu toocreate e-mailovou zprávu. Po vytvoření objektu zpráva hello můžete nastavit vlastnosti a metody, včetně hello odesílatelem e-mailu, hello příjemce e-mailu a hello předmět a text e-mailů hello.

Hello následující příklad ukazuje, jak toocreate objekt plně vyplněná e-mailu:

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

Další informace o všech vlastností a metod podporovaných **sendgrid vám umožňuje** zadejte najdete v tématu [sendgrid vám umožňuje csharp] [ sendgrid-csharp] na Githubu.

## <a name="how-to-send-an-email"></a>Postupy: odeslání e-mailu
Po vytvoření e-mailovou zprávu, můžete ho pomocí rozhraní API sendgrid vám umožňuje společnosti odeslat. Alternativně můžete použít [. NET je součástí knihovny][NET-library].

Odesílání e-mailu vyžaduje, že je klíč rozhraní API sendgrid vám umožňuje zadat. Pokud potřebujete informace o tooconfigure klíče rozhraní API, navštivte klíče rozhraní API sendgrid vám umožňuje na [dokumentace][documentation].

Tyto přihlašovací údaje mohou ukládat prostřednictvím portálu Azure tak, že kliknete na příkaz Nastavení aplikace a přidat páry klíč – hodnota hello v části Nastavení aplikace.

 ![Nastavení aplikace Azure][azure_app_settings]

 Potom můžete získat z nich následujícím způsobem:

    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
    var client = new SendGridClient(apiKey);

Hello následující příklady ukazují, jak hello toosend zprávu pomocí webového rozhraní API.

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

## <a name="how-to-add-an-attachment"></a>Postupy: Přidání přílohy
Přílohy lze přidat zprávu tooa pomocí volání hello **AddAttachment** metoda a minimálně zadáte název souboru hello a kódováním Base64 obsahu, které chcete tooattach. Může obsahovat více přiložených, voláním této metody po pro každý soubor chcete tooattach nebo pomocí hello **AddAttachments** metoda. Hello následující příklad ukazuje, přidání zprávu tooa přílohy:

    var banner2 = new Attachment()
    {
        Content = Convert.ToBase64String(raw_content),
        Type = "image/png",
        Filename = "banner2.png",
        Disposition = "inline",
        ContentId = "Banner 2"
    };
    msg.AddAttachment(banner2);

## <a name="how-to-use-mail-settings-tooenable-footers-tracking-and-analytics"></a>Postupy: použití zápatí tooenable nastavení e-mailu, sledování a analýzy
Sendgrid vám umožňuje poskytuje další e-mailové funkce prostřednictvím hello použití nastavení pošty a sledování nastavení. Tato nastavení mohou být přidány tooan e-mailové zprávy tooenable konkrétní funkce například klikněte na tlačítko sledování, Google analytics, předplatné, sledování a tak dále. Úplný seznam aplikací, najdete v části hello [nastavení dokumentaci][settings-documentation].

Aplikace lze použít příliš**sendgrid vám umožňuje** e-mailové zprávy pomocí metody implementované jako součást hello **SendGridMessage** třídy. Hello následující příklady ukazují hello zápatí a klikněte na tlačítko Sledování filtry:

Hello následující příklady ukazují hello zápatí a klikněte na tlačítko Sledování filtry:

### <a name="footer-settings"></a>Nastavení zápatí
    msg.SetFooterSetting(
                         true,
                         "Some Footer HTML",
                         "<strong>Some Footer Text</strong>");

### <a name="click-tracking"></a>Klikněte na tlačítko Sledování
    msg.SetClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Postupy: použití služeb další sendgrid vám umožňuje
Sendgrid vám umožňuje nabízí několik rozhraní API a pomocí webhooků, které můžete použít další funkce tooleverage v aplikaci Azure. Další podrobnosti najdete v tématu hello [referenční dokumentace rozhraní API sendgrid vám umožňuje][SendGrid API documentation].

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello hello služby sendgrid vám umožňuje e-mailů, použijte tyto odkazy toolearn Další.

* C sendgrid vám umožňuje\# knihovny úložišti: [sendgrid vám umožňuje csharp][sendgrid-csharp]
* Dokumentaci k rozhraní API sendgrid vám umožňuje: <https://sendgrid.com/docs>

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

