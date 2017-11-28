---
title: "aaaHow toouse hello sendgrid vám umožňuje e-mailovou službu (Java) | Microsoft Docs"
description: "Zjistěte, jak odeslat e-mail s hello sendgrid vám umožňuje e-mailovou službu v Azure. Ukázky kódu napsanou v jazyce Java."
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
ms.openlocfilehash: 542ce0003e94fc8f5551487d5a3cd6f75d27e8cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-from-java"></a><span data-ttu-id="0d4b5-104">Jak tooSend Sendgridu pomocí e-mailu z Javy</span><span class="sxs-lookup"><span data-stu-id="0d4b5-104">How tooSend Email Using SendGrid from Java</span></span>
<span data-ttu-id="0d4b5-105">Tato příručka ukazuje, jak tooperform běžné úlohy programování sendgridu e-mailové služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-105">This guide demonstrates how tooperform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="0d4b5-106">Hello ukázky jsou napsané v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-106">hello samples are written in Java.</span></span> <span data-ttu-id="0d4b5-107">Hello pokryté scénáře zahrnují **vytváření e-mailu**, **odesílání e-mailu**, **přidávání příloh**, **pomocí filtrů**a **aktualizace vlastností**.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-107">hello scenarios covered include **constructing email**, **sending email**, **adding attachments**, **using filters**, and **updating properties**.</span></span> <span data-ttu-id="0d4b5-108">Další informace o sendgrid vám umožňuje a odesílání e-mailu, najdete v části hello [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-108">For more information on SendGrid and sending email, see hello [Next steps](#next-steps) section.</span></span>

## <a name="what-is-hello-sendgrid-email-service"></a><span data-ttu-id="0d4b5-109">Co je hello sendgrid vám umožňuje e-mailovou službu?</span><span class="sxs-lookup"><span data-stu-id="0d4b5-109">What is hello SendGrid Email Service?</span></span>
<span data-ttu-id="0d4b5-110">Je sendgrid vám umožňuje [cloudový e-mailovou službu] poskytuje spolehlivé [doručování e-mailem transakční], škálovatelnost a analýzu v reálném čase společně s flexibilní rozhraní API, které umožňují snadnou vlastní integrace.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="0d4b5-111">Obvyklé scénáře použití sendgrid vám umožňuje patří:</span><span class="sxs-lookup"><span data-stu-id="0d4b5-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="0d4b5-112">Automatické odesílání toocustomers potvrzení</span><span class="sxs-lookup"><span data-stu-id="0d4b5-112">Automatically sending receipts toocustomers</span></span>
* <span data-ttu-id="0d4b5-113">Správa distribučních seznamů pro odesílání zákazníkům měsíční e letáků a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="0d4b5-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="0d4b5-114">Shromažďování metriky v reálném čase pro takové věci, jako e-mailu blokovaný a odezvy zákazníka</span><span class="sxs-lookup"><span data-stu-id="0d4b5-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="0d4b5-115">Generování sestav toohelp identifikovat trendy</span><span class="sxs-lookup"><span data-stu-id="0d4b5-115">Generating reports toohelp identify trends</span></span>
* <span data-ttu-id="0d4b5-116">Předávání dotazy zákazníků</span><span class="sxs-lookup"><span data-stu-id="0d4b5-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="0d4b5-117">E-mailových oznámení z vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="0d4b5-117">Email notifications from your application</span></span>

<span data-ttu-id="0d4b5-118">Další informace najdete v tématu <http://sendgrid.com>.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-118">For more information, see <http://sendgrid.com>.</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="0d4b5-119">Vytvoření účtu sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="0d4b5-119">Create a SendGrid account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-hello-javaxmail-libraries"></a><span data-ttu-id="0d4b5-120">Postupy: použití knihoven javax.mail hello</span><span class="sxs-lookup"><span data-stu-id="0d4b5-120">How to: Use hello javax.mail libraries</span></span>
<span data-ttu-id="0d4b5-121">Získat hello javax.mail knihovny, například z <http://www.oracle.com/technetwork/java/javamail> a importovat je do vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-121">Obtain hello javax.mail libraries, for example from <http://www.oracle.com/technetwork/java/javamail> and import them into your code.</span></span> <span data-ttu-id="0d4b5-122">Na hlavní hello proces pomocí hello javax.mail knihovny toosend e-mailu pomocí protokolu SMTP je toodo hello následující:</span><span class="sxs-lookup"><span data-stu-id="0d4b5-122">At a high-level, hello process for using hello javax.mail library toosend email using SMTP is toodo hello following:</span></span>

1. <span data-ttu-id="0d4b5-123">Zadejte hodnoty SMTP hello, včetně hello SMTP serveru, která je smtp.sendgrid.net z sendgrid vám umožňuje.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-123">Specify hello SMTP values, including hello SMTP server, which for SendGrid is smtp.sendgrid.net.</span></span>

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

1. <span data-ttu-id="0d4b5-124">Rozšíření hello *javax.mail.Authenticator* třída a ve vaší implementace nástroje *getPasswordAuthentication* metody vracet sendgrid vám umožňuje uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-124">Extend hello *javax.mail.Authenticator* class, and in your implementation of the *getPasswordAuthentication* method, return your SendGrid user name and password.</span></span>  

       private class SMTPAuthenticator extends javax.mail.Authenticator {
       public PasswordAuthentication getPasswordAuthentication() {
          String username = SMTP_AUTH_USER;
          String password = SMTP_AUTH_PWD;
          return new PasswordAuthentication(username, password);
       }
2. <span data-ttu-id="0d4b5-125">Vytvoření relace ověřené e-mailu prostřednictvím *javax.mail.Session* objektu.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-125">Create an authenticated email session through a *javax.mail.Session* object.</span></span>  

       Authenticator auth = new SMTPAuthenticator();
       Session mailSession = Session.getDefaultInstance(properties, auth);
3. <span data-ttu-id="0d4b5-126">Vytvořte zprávu a přiřadit **k**, **z**, **subjektu** a obsahu hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-126">Create your message and assign **To**, **From**, **Subject** and content values.</span></span> <span data-ttu-id="0d4b5-127">To je ukázáno v hello [postupy: vytvoření e-mailu](#how-to-create-an-email) části.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-127">This is shown in hello [How To: Create an Email](#how-to-create-an-email) section.</span></span>
4. <span data-ttu-id="0d4b5-128">Odeslat zprávu hello prostřednictvím *javax.mail.Transport* objektu.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-128">Send hello message through a *javax.mail.Transport* object.</span></span> <span data-ttu-id="0d4b5-129">To je ukázáno v hello [postupy: odeslání e-mailu] [postup: e-mailovou zprávu] části.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-129">This is shown in hello [How To: Send an Email][How to: Send an Email] section.</span></span>

## <a name="how-to-create-an-email"></a><span data-ttu-id="0d4b5-130">Postupy: vytvoření e-mailu</span><span class="sxs-lookup"><span data-stu-id="0d4b5-130">How to: Create an email</span></span>
<span data-ttu-id="0d4b5-131">Následující Hello ukazuje, jak toospecify hodnoty pro e-mailu.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-131">hello following shows how toospecify values for an email.</span></span>

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

## <a name="how-to-send-an-email"></a><span data-ttu-id="0d4b5-132">Postupy: odeslání e-mailu</span><span class="sxs-lookup"><span data-stu-id="0d4b5-132">How to: Send an email</span></span>
<span data-ttu-id="0d4b5-133">Hello následující ukazuje, jak toosend e-mailu.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-133">hello following shows how toosend an email.</span></span>

    Transport transport = mailSession.getTransport();
    // Connect hello transport object.
    transport.connect();
    // Send hello message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close hello connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="0d4b5-134">Postupy: Přidání přílohy</span><span class="sxs-lookup"><span data-stu-id="0d4b5-134">How to: Add an attachment</span></span>
<span data-ttu-id="0d4b5-135">Hello následující kód ukazuje, jak tooadd přílohu.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-135">hello following code shows you how tooadd an attachment.</span></span>

    // Local file name and path.
    String attachmentName = "myfile.zip";
    String attachmentPath = "c:\\myfiles\\";
    MimeBodyPart attachmentPart = new MimeBodyPart();
    // Specify hello local file tooattach.
    DataSource source = new FileDataSource(attachmentPath + attachmentName);
    attachmentPart.setDataHandler(new DataHandler(source));
    // This example uses hello local file name as hello attachment name.
    // They could be different if you prefer.
    attachmentPart.setFileName(attachmentName);
    multipart.addBodyPart(attachmentPart);

## <a name="how-to-use-filters-tooenable-footers-tracking-and-analytics"></a><span data-ttu-id="0d4b5-136">Postupy: použití filtrů tooenable zápatí, sledování a analýza</span><span class="sxs-lookup"><span data-stu-id="0d4b5-136">How to: Use filters tooenable footers, tracking, and analytics</span></span>
<span data-ttu-id="0d4b5-137">Sendgrid vám umožňuje poskytuje další e-mailové funkce prostřednictvím použití hello *filtry*.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-137">SendGrid provides additional email functionality through hello use of *filters*.</span></span> <span data-ttu-id="0d4b5-138">Jsou to nastavení, které mohou být přidány tooan e-mailové zprávě povolit specifické funkce, například povolení sledování klikněte na tlačítko, Google analytics, sledování, předplatné a tak dále.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-138">These are settings that can be added tooan email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="0d4b5-139">Úplný seznam filtrů, najdete v části [nastavení filtru][Filter Settings].</span><span class="sxs-lookup"><span data-stu-id="0d4b5-139">For a full list of filters, see [Filter Settings][Filter Settings].</span></span>

* <span data-ttu-id="0d4b5-140">Následující Hello ukazuje, jak filtrovat tooinsert zápatí této výsledky v textu HTML zobrazovaných v dolní části hello odesílány e-mailů hello.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-140">hello following shows how tooinsert a footer filter that results in HTML text appearing at hello bottom of hello email being sent.</span></span>

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"footer\":
          {\"settings\":
          {\"enable\":1,\"text/html\":
          \"<html><b>Thank you</b> for your business.</html>\"}}}}");
* <span data-ttu-id="0d4b5-141">Další příklad filtr je kliknutím na tlačítko sledování.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-141">Another example of a filter is click tracking.</span></span> <span data-ttu-id="0d4b5-142">Řekněme, že text e-mailu obsahuje hypertextový odkaz, například hello následující a chcete, aby tootrack hello, klikněte na tlačítko míry:</span><span class="sxs-lookup"><span data-stu-id="0d4b5-142">Let’s say that your email text contains a hyperlink, such as hello following, and you want tootrack hello click rate:</span></span>

      messagePart.setContent(
          "Hello,
          <p>This is hello body of hello message. Visit
          <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
          Thank you.",
          "text/html");
* <span data-ttu-id="0d4b5-143">Klikněte na tooenable hello, sledování, použijte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="0d4b5-143">tooenable hello click tracking, use hello following code:</span></span>

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"clicktrack\":
          {\"settings\":
          {\"enable\":1}}}}");

## <a name="how-to-update-email-properties"></a><span data-ttu-id="0d4b5-144">Postupy: aktualizovat vlastnosti e-mailu</span><span class="sxs-lookup"><span data-stu-id="0d4b5-144">How to: Update email properties</span></span>
<span data-ttu-id="0d4b5-145">Některé vlastnosti e-mailu můžete přepsat pomocí  **nastavit*vlastnost*** nebo připojených pomocí  **přidat*vlastnost***.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-145">Some email properties can be overwritten using **set*Property*** or appended using **add*Property***.</span></span>

<span data-ttu-id="0d4b5-146">Například toospecify **ReplyTo** adresy, použijte hello:</span><span class="sxs-lookup"><span data-stu-id="0d4b5-146">For example, toospecify **ReplyTo** addresses, use hello following:</span></span>

    InternetAddress addresses[] =
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };

    message.setReplyTo(addresses);

<span data-ttu-id="0d4b5-147">tooadd **kopie** příjemce, použijte hello následující:</span><span class="sxs-lookup"><span data-stu-id="0d4b5-147">tooadd a **Cc** recipient, use hello following:</span></span>

    message.addRecipient(Message.RecipientType.CC, new
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="0d4b5-148">Postupy: použití další služby sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="0d4b5-148">How to: Use additional SendGrid services</span></span>
<span data-ttu-id="0d4b5-149">Sendgrid vám umožňuje nabízí webové rozhraní API, které můžete použít další funkce sendgrid vám umožňuje tooleverage z vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-149">SendGrid offers web-based APIs that you can use tooleverage additional SendGrid functionality from your Azure application.</span></span> <span data-ttu-id="0d4b5-150">Úplné podrobnosti najdete v tématu hello [dokumentaci k rozhraní API sendgrid vám umožňuje][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="0d4b5-150">For full details, see hello [SendGrid API documentation][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d4b5-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d4b5-151">Next steps</span></span>
<span data-ttu-id="0d4b5-152">Teď, když jste se naučili základy hello hello služby sendgrid vám umožňuje e-mailů, použijte tyto odkazy toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="0d4b5-152">Now that you’ve learned hello basics of hello SendGrid Email service, follow these links toolearn more.</span></span>

* <span data-ttu-id="0d4b5-153">Ukázka, která demonstruje použití sendgrid vám umožňuje v nasazení služby Azure: [jak toosend e-mailu pomocí sendgrid vám umožňuje z prostředí Java v Azure nasazení](store-sendgrid-java-how-to-send-email-example.md)</span><span class="sxs-lookup"><span data-stu-id="0d4b5-153">Sample that demonstrates using SendGrid in an Azure deployment: [How toosend email using SendGrid from Java in an Azure deployment](store-sendgrid-java-how-to-send-email-example.md)</span></span>
* <span data-ttu-id="0d4b5-154">Sendgrid vám umožňuje Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html></span><span class="sxs-lookup"><span data-stu-id="0d4b5-154">SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html></span></span>
* <span data-ttu-id="0d4b5-155">Dokumentaci k rozhraní API sendgrid vám umožňuje: <https://sendgrid.com/docs/API_Reference/index.html></span><span class="sxs-lookup"><span data-stu-id="0d4b5-155">SendGrid API documentation: <https://sendgrid.com/docs/API_Reference/index.html></span></span>
* <span data-ttu-id="0d4b5-156">Speciální nabídka sendgrid vám umožňuje Azure zákazníků: <https://sendgrid.com/windowsazure.html></span><span class="sxs-lookup"><span data-stu-id="0d4b5-156">SendGrid special offer for Azure customers: <https://sendgrid.com/windowsazure.html></span></span>

[http://sendgrid.com]: https://sendgrid.com
[http://sendgrid.com/pricing.html]: http://sendgrid.com/pricing.html
[http://www.sendgrid.com/azure.html]: https://www.sendgrid.com/windowsazure.html
[http://sendgrid.com/features]: https://sendgrid.com/features
[http://www.oracle.com/technetwork/java/javamail]: http://www.oracle.com/technetwork/java/javamail/index.html
[Filter Settings]: https://sendgrid.com/docs/API_Reference/Web_API/filter_settings.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/index.html
[http://sendgrid.com/azure.html]: https://sendgrid.com/windowsazure.html
[cloudový e-mailovou službu]: https://sendgrid.com/email-solutions
[doručování e-mailem transakční]: https://sendgrid.com/transactional-email
