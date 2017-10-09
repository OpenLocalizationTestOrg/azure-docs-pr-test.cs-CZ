---
title: aaastore-sendgrid-Java-How-to-send-email-example
description: "Jak toosend e-mailu pomocí sendgrid vám umožňuje z prostředí Java v Azure nasazení"
services: 
documentationcenter: java
author: thinkingserious
manager: sendgrid
editor: mollybos
ms.assetid: af096a73-6985-4350-92e4-ce1722c8d72f
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/30/2014
ms.author: vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com
ms.openlocfilehash: 51fde1fc71467f8252532b30d2f87856ec25067b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Jak tooSend Sendgridu pomocí e-mailu z Javy v nasazení Azure
Hello následující příklad ukazuje, je použití sendgrid vám umožňuje toosend e-mailů z webové stránky hostované v Azure. Výsledná aplikace Hello vyzve uživatele hello hodnoty e-mailu, jak ukazuje následující snímek obrazovky hello.

![E-mailu formuláře][emailform]

Hello výsledná e-mailu bude vypadat podobně jako toohello následující snímek obrazovky.

![E-mailové zprávy][emailsent]

Budete potřebovat následující hello toodo toouse hello kód v tomto tématu:

1. Získat hello javax.mail JAR, například z <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. Přidejte tooyour JAR hello cesta sestavení Java.
3. Pokud používáte Eclipse toocreate tuto aplikaci Java, můžete zahrnout hello sendgrid vám umožňuje knihovny ve vašem nasazení souboru aplikace (WAR) pomocí funkce sestavení Eclipse na nasazení. Pokud nepoužíváte Eclipse toocreate tuto aplikaci Java, ujistěte se, jsou součástí hello hello knihovny Azure stejný atribut role jako cestu – třída aplikace a přidané toohello vaší Java vaší aplikace.

Je také nutné mít vlastní sendgrid vám umožňuje uživatelské jméno a heslo, toobe možné toosend hello e-mailu. tooget spuštění služby sendgrid, najdete v části [jak toosend e-mailu pomocí sendgrid vám umožňuje z prostředí Java](store-sendgrid-java-how-to-send-email.md).

Kromě toho znalost hello informace [vytvoření aplikace Hello World služby Azure v prostředí Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944), nebo s jinými technikami, pro hostování aplikací Java v Azure, pokud nepoužíváte Eclipse, důrazně doporučujeme.

## <a name="create-a-web-form-for-sending-email"></a>Vytvoření webového formuláře pro odesílání e-mailu
Hello následující kód ukazuje, jak toocreate webové formuláři tooretrieve uživatelská data pro odesílání e-mailu. Pro účely tohoto obsahu, je soubor JSP hello s názvem **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-hello-code-toosend-hello-email"></a>Vytvoření hello kód toosend hello e-mailu
Hello následující kód, který je volána po vyplnění formuláře hello v emailform.jsp, vytvoří hello e-mailové zprávy a odešle ji. Pro účely tohoto obsahu, je soubor JSP hello s názvem **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%

     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");

     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;

            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {

         // hello SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";

         Properties properties;

         properties = new Properties();

         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");

         // Display hello email fields entered by hello user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");

         // Create hello authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();

         // Create hello mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);

         // Display debug information toostdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);

         // Create hello message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 

         message = new MimeMessage(mailSession);

         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            

         // Specify hello email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);

         // Uncomment hello following if you want tooadd a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");

         // Uncomment hello following if you want tooenable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");

         Transport transport;
         transport = mailSession.getTransport();
         // Connect hello transport object.
         transport.connect();
         // Send hello message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close hello connection.
         transport.close();

        out.println("<p>Email processing completed.</p>");

     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>

    </body>
    </html>

Kromě e-mailu hello toosending, emailform.jsp poskytuje výsledku pro uživatele hello; Příkladem je hello následující snímek obrazovky:

![Odesílání e-mailu výsledek][emailresult]

## <a name="next-steps"></a>Další kroky
Nasadit emulátoru služby výpočty toohello vaší aplikace a v prohlížeči spuštění emailform.jsp, zadejte hodnoty ve formuláři hello, klikněte na **odeslání této e-mailu**a pak zobrazte výsledky v sendemail.jsp.

Tento kód byl poskytnut tooshow můžete jak toouse sendgrid vám umožňuje v jazyce Java v Azure. Před nasazením tooAzure v produkčním prostředí, můžete chtít tooadd další zpracování chyb a další funkce. Například: 

* Můžete použít objekty BLOB úložiště Azure nebo databázi SQL toostore e-mailové adresy a e-mailové zprávy, místo použití webového formuláře. Informace o použití objektů BLOB služby Azure storage v jazyce Java najdete v tématu [jak tooUse hello služby úložiště objektů Blob z Javy](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). Informace o používání databáze SQL v jazyce Java najdete v tématu [pomocí SQL Database v jazyce Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).
* Můžete použít `RoleEnvironment.getConfigurationSettings` tooretrieve hello sendgrid vám umožňuje uživatelské jméno a heslo z nastavení konfigurace vašeho nasazení, místo použití hello webové formuláře tooretrieve tyto hodnoty. Informace o hello `RoleEnvironment` třídy najdete v tématu [hello pomocí knihovny spuštění služby Azure v JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) a dokumentaci balíček hello běh služby Azure na <http://dl.windowsazure.com/javadoc>.
* Další informace o používání sendgrid vám umožňuje v jazyce Java najdete v tématu [jak toosend e-mailu pomocí sendgrid vám umožňuje z prostředí Java](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
