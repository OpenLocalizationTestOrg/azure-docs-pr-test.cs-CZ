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
# <a name="how-toosend-email-using-sendgrid-from-java-in-an-azure-deployment"></a><span data-ttu-id="f4742-103">Jak tooSend Sendgridu pomocí e-mailu z Javy v nasazení Azure</span><span class="sxs-lookup"><span data-stu-id="f4742-103">How tooSend Email Using SendGrid from Java in an Azure Deployment</span></span>
<span data-ttu-id="f4742-104">Hello následující příklad ukazuje, je použití sendgrid vám umožňuje toosend e-mailů z webové stránky hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="f4742-104">hello following example shows you how you can use SendGrid toosend emails from a web page hosted in Azure.</span></span> <span data-ttu-id="f4742-105">Výsledná aplikace Hello vyzve uživatele hello hodnoty e-mailu, jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="f4742-105">hello resulting application will prompt hello user for email values, as shown in hello following screen shot.</span></span>

![E-mailu formuláře][emailform]

<span data-ttu-id="f4742-107">Hello výsledná e-mailu bude vypadat podobně jako toohello následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="f4742-107">hello resulting email will look similar toohello following screen shot.</span></span>

![E-mailové zprávy][emailsent]

<span data-ttu-id="f4742-109">Budete potřebovat následující hello toodo toouse hello kód v tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="f4742-109">You'll need toodo hello following toouse hello code in this topic:</span></span>

1. <span data-ttu-id="f4742-110">Získat hello javax.mail JAR, například z <http://www.oracle.com/technetwork/java/javamail/index.html>.</span><span class="sxs-lookup"><span data-stu-id="f4742-110">Obtain hello javax.mail JARs, for example from <http://www.oracle.com/technetwork/java/javamail/index.html>.</span></span>
2. <span data-ttu-id="f4742-111">Přidejte tooyour JAR hello cesta sestavení Java.</span><span class="sxs-lookup"><span data-stu-id="f4742-111">Add hello JARs tooyour Java build path.</span></span>
3. <span data-ttu-id="f4742-112">Pokud používáte Eclipse toocreate tuto aplikaci Java, můžete zahrnout hello sendgrid vám umožňuje knihovny ve vašem nasazení souboru aplikace (WAR) pomocí funkce sestavení Eclipse na nasazení.</span><span class="sxs-lookup"><span data-stu-id="f4742-112">If you are using Eclipse toocreate this Java application, you can include hello SendGrid libraries in your application deployment file (WAR) using Eclipse's deployment assembly feature.</span></span> <span data-ttu-id="f4742-113">Pokud nepoužíváte Eclipse toocreate tuto aplikaci Java, ujistěte se, jsou součástí hello hello knihovny Azure stejný atribut role jako cestu – třída aplikace a přidané toohello vaší Java vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4742-113">If you are not using Eclipse toocreate this Java application, ensure hello libraries are included within hello same Azure role as your Java application, and added toohello class path of your application.</span></span>

<span data-ttu-id="f4742-114">Je také nutné mít vlastní sendgrid vám umožňuje uživatelské jméno a heslo, toobe možné toosend hello e-mailu.</span><span class="sxs-lookup"><span data-stu-id="f4742-114">You must also have your own SendGrid username and password, toobe able toosend hello email.</span></span> <span data-ttu-id="f4742-115">tooget spuštění služby sendgrid, najdete v části [jak toosend e-mailu pomocí sendgrid vám umožňuje z prostředí Java](store-sendgrid-java-how-to-send-email.md).</span><span class="sxs-lookup"><span data-stu-id="f4742-115">tooget started with SendGrid, see [How toosend email using SendGrid from Java](store-sendgrid-java-how-to-send-email.md).</span></span>

<span data-ttu-id="f4742-116">Kromě toho znalost hello informace [vytvoření aplikace Hello World služby Azure v prostředí Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944), nebo s jinými technikami, pro hostování aplikací Java v Azure, pokud nepoužíváte Eclipse, důrazně doporučujeme.</span><span class="sxs-lookup"><span data-stu-id="f4742-116">Additionally, familiarity with hello information at [Creating a Hello World Application for Azure in Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944), or with other techniques for hosting Java applications in Azure if you are not using Eclipse, is highly recommended.</span></span>

## <a name="create-a-web-form-for-sending-email"></a><span data-ttu-id="f4742-117">Vytvoření webového formuláře pro odesílání e-mailu</span><span class="sxs-lookup"><span data-stu-id="f4742-117">Create a web form for sending email</span></span>
<span data-ttu-id="f4742-118">Hello následující kód ukazuje, jak toocreate webové formuláři tooretrieve uživatelská data pro odesílání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="f4742-118">hello following code shows how toocreate a web form tooretrieve user data for sending email.</span></span> <span data-ttu-id="f4742-119">Pro účely tohoto obsahu, je soubor JSP hello s názvem **emailform.jsp**.</span><span class="sxs-lookup"><span data-stu-id="f4742-119">For purposes of this content, hello JSP file is named **emailform.jsp**.</span></span>

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

## <a name="create-hello-code-toosend-hello-email"></a><span data-ttu-id="f4742-120">Vytvoření hello kód toosend hello e-mailu</span><span class="sxs-lookup"><span data-stu-id="f4742-120">Create hello code toosend hello email</span></span>
<span data-ttu-id="f4742-121">Hello následující kód, který je volána po vyplnění formuláře hello v emailform.jsp, vytvoří hello e-mailové zprávy a odešle ji.</span><span class="sxs-lookup"><span data-stu-id="f4742-121">hello following code, which is called when you complete hello form in emailform.jsp, creates hello email message and sends it.</span></span> <span data-ttu-id="f4742-122">Pro účely tohoto obsahu, je soubor JSP hello s názvem **sendemail.jsp**.</span><span class="sxs-lookup"><span data-stu-id="f4742-122">For purposes of this content, hello JSP file is named **sendemail.jsp**.</span></span>

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

<span data-ttu-id="f4742-123">Kromě e-mailu hello toosending, emailform.jsp poskytuje výsledku pro uživatele hello; Příkladem je hello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="f4742-123">In addition toosending hello email, emailform.jsp provides a result for hello user; an example is hello following screen shot:</span></span>

![Odesílání e-mailu výsledek][emailresult]

## <a name="next-steps"></a><span data-ttu-id="f4742-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4742-125">Next steps</span></span>
<span data-ttu-id="f4742-126">Nasadit emulátoru služby výpočty toohello vaší aplikace a v prohlížeči spuštění emailform.jsp, zadejte hodnoty ve formuláři hello, klikněte na **odeslání této e-mailu**a pak zobrazte výsledky v sendemail.jsp.</span><span class="sxs-lookup"><span data-stu-id="f4742-126">Deploy your application toohello compute emulator and within a browser run emailform.jsp, enter values in hello form, click **Send this email**, and then see results in sendemail.jsp.</span></span>

<span data-ttu-id="f4742-127">Tento kód byl poskytnut tooshow můžete jak toouse sendgrid vám umožňuje v jazyce Java v Azure.</span><span class="sxs-lookup"><span data-stu-id="f4742-127">This code was provided tooshow you how toouse SendGrid in Java on Azure.</span></span> <span data-ttu-id="f4742-128">Před nasazením tooAzure v produkčním prostředí, můžete chtít tooadd další zpracování chyb a další funkce.</span><span class="sxs-lookup"><span data-stu-id="f4742-128">Before deploying tooAzure in production, you may want tooadd more error handling or other features.</span></span> <span data-ttu-id="f4742-129">Například:</span><span class="sxs-lookup"><span data-stu-id="f4742-129">For example:</span></span> 

* <span data-ttu-id="f4742-130">Můžete použít objekty BLOB úložiště Azure nebo databázi SQL toostore e-mailové adresy a e-mailové zprávy, místo použití webového formuláře.</span><span class="sxs-lookup"><span data-stu-id="f4742-130">You could use Azure storage blobs or SQL Database toostore email addresses and email messages, instead of using a web form.</span></span> <span data-ttu-id="f4742-131">Informace o použití objektů BLOB služby Azure storage v jazyce Java najdete v tématu [jak tooUse hello služby úložiště objektů Blob z Javy](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/).</span><span class="sxs-lookup"><span data-stu-id="f4742-131">For information about using Azure storage blobs in Java, see [How tooUse hello Blob Storage Service from Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/).</span></span> <span data-ttu-id="f4742-132">Informace o používání databáze SQL v jazyce Java najdete v tématu [pomocí SQL Database v jazyce Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).</span><span class="sxs-lookup"><span data-stu-id="f4742-132">For information about using SQL Database in Java, see [Using SQL Database in Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).</span></span>
* <span data-ttu-id="f4742-133">Můžete použít `RoleEnvironment.getConfigurationSettings` tooretrieve hello sendgrid vám umožňuje uživatelské jméno a heslo z nastavení konfigurace vašeho nasazení, místo použití hello webové formuláře tooretrieve tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f4742-133">You could use `RoleEnvironment.getConfigurationSettings` tooretrieve hello SendGrid username and password from your deployment's configuration settings, instead of using hello web form tooretrieve those values.</span></span> <span data-ttu-id="f4742-134">Informace o hello `RoleEnvironment` třídy najdete v tématu [hello pomocí knihovny spuštění služby Azure v JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) a dokumentaci balíček hello běh služby Azure na <http://dl.windowsazure.com/javadoc>.</span><span class="sxs-lookup"><span data-stu-id="f4742-134">For information about hello `RoleEnvironment` class, see [Using hello Azure Service Runtime Library in JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) and hello Azure Service Runtime package documentation at <http://dl.windowsazure.com/javadoc>.</span></span>
* <span data-ttu-id="f4742-135">Další informace o používání sendgrid vám umožňuje v jazyce Java najdete v tématu [jak toosend e-mailu pomocí sendgrid vám umožňuje z prostředí Java](store-sendgrid-java-how-to-send-email.md).</span><span class="sxs-lookup"><span data-stu-id="f4742-135">For more information about using SendGrid in Java, see [How toosend email using SendGrid from Java](store-sendgrid-java-how-to-send-email.md).</span></span>

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
