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
# <a name="how-toosend-email-using-sendgrid-from-java"></a>Jak tooSend Sendgridu pomocí e-mailu z Javy
Tato příručka ukazuje, jak tooperform běžné úlohy programování sendgridu e-mailové služby v Azure. Hello ukázky jsou napsané v jazyce Java. Hello pokryté scénáře zahrnují **vytváření e-mailu**, **odesílání e-mailu**, **přidávání příloh**, **pomocí filtrů**a **aktualizace vlastností**. Další informace o sendgrid vám umožňuje a odesílání e-mailu, najdete v části hello [další kroky](#next-steps) části.

## <a name="what-is-hello-sendgrid-email-service"></a>Co je hello sendgrid vám umožňuje e-mailovou službu?
Je sendgrid vám umožňuje [cloudový e-mailovou službu] poskytuje spolehlivé [doručování e-mailem transakční], škálovatelnost a analýzu v reálném čase společně s flexibilní rozhraní API, které umožňují snadnou vlastní integrace. Obvyklé scénáře použití sendgrid vám umožňuje patří:

* Automatické odesílání toocustomers potvrzení
* Správa distribučních seznamů pro odesílání zákazníkům měsíční e letáků a speciálních nabídek
* Shromažďování metriky v reálném čase pro takové věci, jako e-mailu blokovaný a odezvy zákazníka
* Generování sestav toohelp identifikovat trendy
* Předávání dotazy zákazníků
* E-mailových oznámení z vaší aplikace

Další informace najdete v tématu <http://sendgrid.com>.

## <a name="create-a-sendgrid-account"></a>Vytvoření účtu sendgrid vám umožňuje
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-hello-javaxmail-libraries"></a>Postupy: použití knihoven javax.mail hello
Získat hello javax.mail knihovny, například z <http://www.oracle.com/technetwork/java/javamail> a importovat je do vašeho kódu. Na hlavní hello proces pomocí hello javax.mail knihovny toosend e-mailu pomocí protokolu SMTP je toodo hello následující:

1. Zadejte hodnoty SMTP hello, včetně hello SMTP serveru, která je smtp.sendgrid.net z sendgrid vám umožňuje.

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

1. Rozšíření hello *javax.mail.Authenticator* třída a ve vaší implementace nástroje *getPasswordAuthentication* metody vracet sendgrid vám umožňuje uživatelské jméno a heslo.  

       private class SMTPAuthenticator extends javax.mail.Authenticator {
       public PasswordAuthentication getPasswordAuthentication() {
          String username = SMTP_AUTH_USER;
          String password = SMTP_AUTH_PWD;
          return new PasswordAuthentication(username, password);
       }
2. Vytvoření relace ověřené e-mailu prostřednictvím *javax.mail.Session* objektu.  

       Authenticator auth = new SMTPAuthenticator();
       Session mailSession = Session.getDefaultInstance(properties, auth);
3. Vytvořte zprávu a přiřadit **k**, **z**, **subjektu** a obsahu hodnoty. To je ukázáno v hello [postupy: vytvoření e-mailu](#how-to-create-an-email) části.
4. Odeslat zprávu hello prostřednictvím *javax.mail.Transport* objektu. To je ukázáno v hello [postupy: odeslání e-mailu] [postup: e-mailovou zprávu] části.

## <a name="how-to-create-an-email"></a>Postupy: vytvoření e-mailu
Následující Hello ukazuje, jak toospecify hodnoty pro e-mailu.

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

## <a name="how-to-send-an-email"></a>Postupy: odeslání e-mailu
Hello následující ukazuje, jak toosend e-mailu.

    Transport transport = mailSession.getTransport();
    // Connect hello transport object.
    transport.connect();
    // Send hello message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close hello connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a>Postupy: Přidání přílohy
Hello následující kód ukazuje, jak tooadd přílohu.

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

## <a name="how-to-use-filters-tooenable-footers-tracking-and-analytics"></a>Postupy: použití filtrů tooenable zápatí, sledování a analýza
Sendgrid vám umožňuje poskytuje další e-mailové funkce prostřednictvím použití hello *filtry*. Jsou to nastavení, které mohou být přidány tooan e-mailové zprávě povolit specifické funkce, například povolení sledování klikněte na tlačítko, Google analytics, sledování, předplatné a tak dále. Úplný seznam filtrů, najdete v části [nastavení filtru][Filter Settings].

* Následující Hello ukazuje, jak filtrovat tooinsert zápatí této výsledky v textu HTML zobrazovaných v dolní části hello odesílány e-mailů hello.

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"footer\":
          {\"settings\":
          {\"enable\":1,\"text/html\":
          \"<html><b>Thank you</b> for your business.</html>\"}}}}");
* Další příklad filtr je kliknutím na tlačítko sledování. Řekněme, že text e-mailu obsahuje hypertextový odkaz, například hello následující a chcete, aby tootrack hello, klikněte na tlačítko míry:

      messagePart.setContent(
          "Hello,
          <p>This is hello body of hello message. Visit
          <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
          Thank you.",
          "text/html");
* Klikněte na tooenable hello, sledování, použijte hello následující kód:

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"clicktrack\":
          {\"settings\":
          {\"enable\":1}}}}");

## <a name="how-to-update-email-properties"></a>Postupy: aktualizovat vlastnosti e-mailu
Některé vlastnosti e-mailu můžete přepsat pomocí  **nastavit*vlastnost*** nebo připojených pomocí  **přidat*vlastnost***.

Například toospecify **ReplyTo** adresy, použijte hello:

    InternetAddress addresses[] =
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };

    message.setReplyTo(addresses);

tooadd **kopie** příjemce, použijte hello následující:

    message.addRecipient(Message.RecipientType.CC, new
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a>Postupy: použití další služby sendgrid vám umožňuje
Sendgrid vám umožňuje nabízí webové rozhraní API, které můžete použít další funkce sendgrid vám umožňuje tooleverage z vaše aplikace Azure. Úplné podrobnosti najdete v tématu hello [dokumentaci k rozhraní API sendgrid vám umožňuje][SendGrid API documentation].

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello hello služby sendgrid vám umožňuje e-mailů, použijte tyto odkazy toolearn Další.

* Ukázka, která demonstruje použití sendgrid vám umožňuje v nasazení služby Azure: [jak toosend e-mailu pomocí sendgrid vám umožňuje z prostředí Java v Azure nasazení](store-sendgrid-java-how-to-send-email-example.md)
* Sendgrid vám umožňuje Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html>
* Dokumentaci k rozhraní API sendgrid vám umožňuje: <https://sendgrid.com/docs/API_Reference/index.html>
* Speciální nabídka sendgrid vám umožňuje Azure zákazníků: <https://sendgrid.com/windowsazure.html>

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
