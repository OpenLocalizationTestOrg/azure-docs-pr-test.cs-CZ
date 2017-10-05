---
title: "Jak používat služby sendgrid vám umožňuje e-mailů (Java) | Microsoft Docs"
description: "Zjistěte, jak odeslat e-mailu pomocí e-mailovou službu sendgrid vám umožňuje v Azure. Ukázky kódu napsanou v jazyce Java."
services: 
documentationcenter: java
author: thinkingserious
manager: sendgrid
editor: mollybos
ms.assetid: cc75c43e-ede9-492b-98c2-9147fcb92c21
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/30/2014
ms.author: elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork
ms.openlocfilehash: 85a0e302626ca14ac039ee6f662f372ddbeb62c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-send-email-using-sendgrid-from-java"></a><span data-ttu-id="45edb-104">Postup odesílání e-mailu pomocí sendgrid vám umožňuje z Javy</span><span class="sxs-lookup"><span data-stu-id="45edb-104">How to Send Email Using SendGrid from Java</span></span>
<span data-ttu-id="45edb-105">Tato příručka ukazuje, jak provádět běžné úkoly programování s e-mailovou službu sendgrid vám umožňuje v Azure.</span><span class="sxs-lookup"><span data-stu-id="45edb-105">This guide demonstrates how to perform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="45edb-106">Ukázky jsou napsané v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="45edb-106">The samples are written in Java.</span></span> <span data-ttu-id="45edb-107">Pokryté scénáře zahrnují **vytváření e-mailu**, **odesílání e-mailu**, **přidávání příloh**, **pomocí filtrů**, a **aktualizace vlastností**.</span><span class="sxs-lookup"><span data-stu-id="45edb-107">The scenarios covered include **constructing email**, **sending email**, **adding attachments**, **using filters**, and **updating properties**.</span></span> <span data-ttu-id="45edb-108">Další informace o sendgrid vám umožňuje a odesílání e-mailu, najdete v článku [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="45edb-108">For more information on SendGrid and sending email, see the [Next steps](#next-steps) section.</span></span>

## <a name="what-is-the-sendgrid-email-service"></a><span data-ttu-id="45edb-109">Co je služba sendgrid vám umožňuje e-mailu?</span><span class="sxs-lookup"><span data-stu-id="45edb-109">What is the SendGrid Email Service?</span></span>
<span data-ttu-id="45edb-110">Je sendgrid vám umožňuje [cloudový e-mailovou službu] poskytuje spolehlivé [doručování e-mailem transakční], škálovatelnost a analýzu v reálném čase společně s flexibilní rozhraní API, které umožňují snadnou vlastní integrace.</span><span class="sxs-lookup"><span data-stu-id="45edb-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="45edb-111">Obvyklé scénáře použití sendgrid vám umožňuje patří:</span><span class="sxs-lookup"><span data-stu-id="45edb-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="45edb-112">Automatické odesílání oznámení zákazníkům</span><span class="sxs-lookup"><span data-stu-id="45edb-112">Automatically sending receipts to customers</span></span>
* <span data-ttu-id="45edb-113">Správa distribučních seznamů pro odesílání zákazníkům měsíční e letáků a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="45edb-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="45edb-114">Shromažďování metriky v reálném čase pro takové věci, jako e-mailu blokovaný a odezvy zákazníka</span><span class="sxs-lookup"><span data-stu-id="45edb-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="45edb-115">Generování sestav k identifikaci trendů</span><span class="sxs-lookup"><span data-stu-id="45edb-115">Generating reports to help identify trends</span></span>
* <span data-ttu-id="45edb-116">Předávání dotazy zákazníků</span><span class="sxs-lookup"><span data-stu-id="45edb-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="45edb-117">E-mailových oznámení z vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="45edb-117">Email notifications from your application</span></span>

<span data-ttu-id="45edb-118">Další informace najdete v tématu <http://sendgrid.com>.</span><span class="sxs-lookup"><span data-stu-id="45edb-118">For more information, see <http://sendgrid.com>.</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="45edb-119">Vytvoření účtu sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="45edb-119">Create a SendGrid account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-the-javaxmail-libraries"></a><span data-ttu-id="45edb-120">Postupy: použití knihoven javax.mail</span><span class="sxs-lookup"><span data-stu-id="45edb-120">How to: Use the javax.mail libraries</span></span>
<span data-ttu-id="45edb-121">Získat javax.mail knihovny, například z <http://www.oracle.com/technetwork/java/javamail> a importovat je do vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="45edb-121">Obtain the javax.mail libraries, for example from <http://www.oracle.com/technetwork/java/javamail> and import them into your code.</span></span> <span data-ttu-id="45edb-122">Na hlavní proces pro použití knihovny javax.mail k odesílání e-mailu pomocí protokolu SMTP je provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="45edb-122">At a high-level, the process for using the javax.mail library to send email using SMTP is to do the following:</span></span>

1. <span data-ttu-id="45edb-123">Zadejte hodnoty SMTP, včetně serveru SMTP, který je pro sendgrid vám umožňuje smtp.sendgrid.net.</span><span class="sxs-lookup"><span data-stu-id="45edb-123">Specify the SMTP values, including the SMTP server, which for SendGrid is smtp.sendgrid.net.</span></span>

```
        import java.util.Properties;
        import javax.activation.*;
        import javax.mail.*;
        import javax.mail.internet.*;

        public class MyEmailer {
           private static final String SMTP_HOST_NAME = "smtp.sendgrid.net";
           private static final String SMTP_AUTH_USER = "your_sendgrid_username";
           private static final String SMTP_AUTH_PWD = "your_sendgrid_password";

           public static void main(String[] args) throws Exception{
               new MyEmailer().SendMail();
           }

           public void SendMail() throws Exception
           {
              Properties properties = new Properties();
                 properties.put("mail.transport.protocol", "smtp");
                 properties.put("mail.smtp.host", SMTP_HOST_NAME);
                 properties.put("mail.smtp.port", 587);
                 properties.put("mail.smtp.auth", "true");
                 // …
```

1. <span data-ttu-id="45edb-124">Rozšíření *javax.mail.Authenticator* třída a ve vaší implementace nástroje *getPasswordAuthentication* metody vracet sendgrid vám umožňuje uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="45edb-124">Extend the *javax.mail.Authenticator* class, and in your implementation of the *getPasswordAuthentication* method, return your SendGrid user name and password.</span></span>  

       private class SMTPAuthenticator extends javax.mail.Authenticator {
       public PasswordAuthentication getPasswordAuthentication() {
          String username = SMTP_AUTH_USER;
          String password = SMTP_AUTH_PWD;
          return new PasswordAuthentication(username, password);
       }
2. <span data-ttu-id="45edb-125">Vytvoření relace ověřené e-mailu prostřednictvím *javax.mail.Session* objektu.</span><span class="sxs-lookup"><span data-stu-id="45edb-125">Create an authenticated email session through a *javax.mail.Session* object.</span></span>  

       Authenticator auth = new SMTPAuthenticator();
       Session mailSession = Session.getDefaultInstance(properties, auth);
3. <span data-ttu-id="45edb-126">Vytvořte zprávu a přiřadit **k**, **z**, **subjektu** a obsahu hodnoty.</span><span class="sxs-lookup"><span data-stu-id="45edb-126">Create your message and assign **To**, **From**, **Subject** and content values.</span></span> <span data-ttu-id="45edb-127">To je ukázáno v [postupy: vytvoření e-mailu](#how-to-create-an-email) části.</span><span class="sxs-lookup"><span data-stu-id="45edb-127">This is shown in the [How To: Create an Email](#how-to-create-an-email) section.</span></span>
4. <span data-ttu-id="45edb-128">Odeslat zprávu prostřednictvím *javax.mail.Transport* objektu.</span><span class="sxs-lookup"><span data-stu-id="45edb-128">Send the message through a *javax.mail.Transport* object.</span></span> <span data-ttu-id="45edb-129">To je ukázáno v [postupy: e-mailovou zprávu] [postup: e-mailovou zprávu] části.</span><span class="sxs-lookup"><span data-stu-id="45edb-129">This is shown in the [How To: Send an Email][How to: Send an Email] section.</span></span>

## <a name="how-to-create-an-email"></a><span data-ttu-id="45edb-130">Postupy: vytvoření e-mailu</span><span class="sxs-lookup"><span data-stu-id="45edb-130">How to: Create an email</span></span>
<span data-ttu-id="45edb-131">Následující ukazuje, jak zadat hodnoty pro e-mailu.</span><span class="sxs-lookup"><span data-stu-id="45edb-131">The following shows how to specify values for an email.</span></span>

    MimeMessage message = new MimeMessage(mailSession);
    Multipart multipart = new MimeMultipart("alternative");
    BodyPart part1 = new MimeBodyPart();
    part1.setText("Hello, Your Contoso order has shipped. Thank you, John");
    BodyPart part2 = new MimeBodyPart();
    part2.setContent(
        "<p>Hello,</p>
        <p>Your Contoso order has <b>shipped</b>.</p>
        <p>Thank you,<br>John</br></p>",
        "text/html");
    multipart.addBodyPart(part1);
    multipart.addBodyPart(part2);
    message.setFrom(new InternetAddress("john@contoso.com"));
    message.addRecipient(Message.RecipientType.TO,
       new InternetAddress("someone@example.com"));
    message.setSubject("Your recent order");
    message.setContent(multipart);

## <a name="how-to-send-an-email"></a><span data-ttu-id="45edb-132">Postupy: odeslání e-mailu</span><span class="sxs-lookup"><span data-stu-id="45edb-132">How to: Send an email</span></span>
<span data-ttu-id="45edb-133">Následující ukazuje, jak odeslat e-mail.</span><span class="sxs-lookup"><span data-stu-id="45edb-133">The following shows how to send an email.</span></span>

    Transport transport = mailSession.getTransport();
    // Connect the transport object.
    transport.connect();
    // Send the message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close the connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="45edb-134">Postupy: Přidání přílohy</span><span class="sxs-lookup"><span data-stu-id="45edb-134">How to: Add an attachment</span></span>
<span data-ttu-id="45edb-135">Následující kód ukazuje, jak přidat přílohu.</span><span class="sxs-lookup"><span data-stu-id="45edb-135">The following code shows you how to add an attachment.</span></span>

    // Local file name and path.
    String attachmentName = "myfile.zip";
    String attachmentPath = "c:\\myfiles\\";
    MimeBodyPart attachmentPart = new MimeBodyPart();
    // Specify the local file to attach.
    DataSource source = new FileDataSource(attachmentPath + attachmentName);
    attachmentPart.setDataHandler(new DataHandler(source));
    // This example uses the local file name as the attachment name.
    // They could be different if you prefer.
    attachmentPart.setFileName(attachmentName);
    multipart.addBodyPart(attachmentPart);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a><span data-ttu-id="45edb-136">Postupy: použití filtrů k povolení zápatí stránky, sledování a analýzy</span><span class="sxs-lookup"><span data-stu-id="45edb-136">How to: Use filters to enable footers, tracking, and analytics</span></span>
<span data-ttu-id="45edb-137">Sendgrid vám umožňuje poskytuje další e-mailové funkce prostřednictvím *filtry*.</span><span class="sxs-lookup"><span data-stu-id="45edb-137">SendGrid provides additional email functionality through the use of *filters*.</span></span> <span data-ttu-id="45edb-138">Jsou to nastavení, které mohou být přidány do e-mailovou zprávu povolit specifické funkce, například povolení sledování klikněte na tlačítko, Google analytics, předplatné, sledování a tak dále.</span><span class="sxs-lookup"><span data-stu-id="45edb-138">These are settings that can be added to an email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="45edb-139">Úplný seznam filtrů, najdete v části [nastavení filtru][Filter Settings].</span><span class="sxs-lookup"><span data-stu-id="45edb-139">For a full list of filters, see [Filter Settings][Filter Settings].</span></span>

* <span data-ttu-id="45edb-140">Následující ukazuje, jak vložit zápatí filtr, který je výsledkem text HTML zobrazovaných v dolní části odesílány e-mailu.</span><span class="sxs-lookup"><span data-stu-id="45edb-140">The following shows how to insert a footer filter that results in HTML text appearing at the bottom of the email being sent.</span></span>

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"footer\":
          {\"settings\":
          {\"enable\":1,\"text/html\":
          \"<html><b>Thank you</b> for your business.</html>\"}}}}");
* <span data-ttu-id="45edb-141">Další příklad filtr je kliknutím na tlačítko sledování.</span><span class="sxs-lookup"><span data-stu-id="45edb-141">Another example of a filter is click tracking.</span></span> <span data-ttu-id="45edb-142">Řekněme, že vaše text e-mailu obsahuje hypertextový odkaz, jako je například následující příkaz, a chcete sledovat rychlost klikněte na:</span><span class="sxs-lookup"><span data-stu-id="45edb-142">Let’s say that your email text contains a hyperlink, such as the following, and you want to track the click rate:</span></span>

      messagePart.setContent(
          "Hello,
          <p>This is the body of the message. Visit
          <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
          Thank you.",
          "text/html");
* <span data-ttu-id="45edb-143">Pokud chcete povolit, klikněte na tlačítko sledování, použijte následující kód:</span><span class="sxs-lookup"><span data-stu-id="45edb-143">To enable the click tracking, use the following code:</span></span>

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"clicktrack\":
          {\"settings\":
          {\"enable\":1}}}}");

## <a name="how-to-update-email-properties"></a><span data-ttu-id="45edb-144">Postupy: aktualizovat vlastnosti e-mailu</span><span class="sxs-lookup"><span data-stu-id="45edb-144">How to: Update email properties</span></span>
<span data-ttu-id="45edb-145">Některé vlastnosti e-mailu můžete přepsat pomocí  **nastavit*vlastnost*** nebo připojených pomocí  **přidat*vlastnost***.</span><span class="sxs-lookup"><span data-stu-id="45edb-145">Some email properties can be overwritten using **set*Property*** or appended using **add*Property***.</span></span>

<span data-ttu-id="45edb-146">Například zadejte **ReplyTo** adresy, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="45edb-146">For example, to specify **ReplyTo** addresses, use the following:</span></span>

    InternetAddress addresses[] =
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };

    message.setReplyTo(addresses);

<span data-ttu-id="45edb-147">Chcete-li přidat **kopie** příjemce, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="45edb-147">To add a **Cc** recipient, use the following:</span></span>

    message.addRecipient(Message.RecipientType.CC, new
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="45edb-148">Postupy: použití další služby sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="45edb-148">How to: Use additional SendGrid services</span></span>
<span data-ttu-id="45edb-149">Sendgrid vám umožňuje nabízí rozhraní API založené na webu, který můžete využít další funkce sendgrid vám umožňuje aplikaci Azure.</span><span class="sxs-lookup"><span data-stu-id="45edb-149">SendGrid offers web-based APIs that you can use to leverage additional SendGrid functionality from your Azure application.</span></span> <span data-ttu-id="45edb-150">Úplné podrobnosti najdete v tématu [dokumentaci k rozhraní API sendgrid vám umožňuje][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="45edb-150">For full details, see the [SendGrid API documentation][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="45edb-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="45edb-151">Next steps</span></span>
<span data-ttu-id="45edb-152">Teď, když jste se naučili základy služby sendgrid vám umožňuje e-mailu, postupujte podle následujících odkazech na další informace.</span><span class="sxs-lookup"><span data-stu-id="45edb-152">Now that you’ve learned the basics of the SendGrid Email service, follow these links to learn more.</span></span>

* <span data-ttu-id="45edb-153">Ukázka, která demonstruje použití sendgrid vám umožňuje v nasazení služby Azure: [postup odesílání e-mailu pomocí sendgrid vám umožňuje z prostředí Java v Azure nasazení](store-sendgrid-java-how-to-send-email-example.md)</span><span class="sxs-lookup"><span data-stu-id="45edb-153">Sample that demonstrates using SendGrid in an Azure deployment: [How to send email using SendGrid from Java in an Azure deployment](store-sendgrid-java-how-to-send-email-example.md)</span></span>
* <span data-ttu-id="45edb-154">Sendgrid vám umožňuje Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html></span><span class="sxs-lookup"><span data-stu-id="45edb-154">SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html></span></span>
* <span data-ttu-id="45edb-155">Dokumentaci k rozhraní API sendgrid vám umožňuje: <https://sendgrid.com/docs/API_Reference/index.html></span><span class="sxs-lookup"><span data-stu-id="45edb-155">SendGrid API documentation: <https://sendgrid.com/docs/API_Reference/index.html></span></span>
* <span data-ttu-id="45edb-156">Speciální nabídka sendgrid vám umožňuje Azure zákazníků: <https://sendgrid.com/windowsazure.html></span><span class="sxs-lookup"><span data-stu-id="45edb-156">SendGrid special offer for Azure customers: <https://sendgrid.com/windowsazure.html></span></span>

[http://sendgrid.com]: https://sendgrid.com
[http://sendgrid.com/pricing.html]: http://sendgrid.com/pricing.html
[http://www.sendgrid.com/azure.html]: https://www.sendgrid.com/windowsazure.html
[http://sendgrid.com/features]: https://sendgrid.com/features
[http://www.oracle.com/technetwork/java/javamail]: http://www.oracle.com/technetwork/java/javamail/index.html
[Filter Settings]: https://sendgrid.com/docs/API_Reference/Web_API/filter_settings.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/index.html
[http://sendgrid.com/azure.html]: https://sendgrid.com/windowsazure.html
<span data-ttu-id="45edb-157">[cloudový e-mailovou službu]: https://sendgrid.com/email-solutions</span><span class="sxs-lookup"><span data-stu-id="45edb-157">[cloud-based email service]: https://sendgrid.com/email-solutions</span></span>
<span data-ttu-id="45edb-158">[doručování e-mailem transakční]: https://sendgrid.com/transactional-email</span><span class="sxs-lookup"><span data-stu-id="45edb-158">[transactional email delivery]: https://sendgrid.com/transactional-email</span></span>
